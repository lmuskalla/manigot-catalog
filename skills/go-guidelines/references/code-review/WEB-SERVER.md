# Web Server: Topics Applied Together

This example shows how the Go topics integrate in a real HTTP server. Each section
references the relevant topic for detailed guidance.

## Structure

```go
package main

import (
    "context"
    "encoding/json"
    "errors"
    "log/slog"
    "net/http"
    "os"
    "os/signal"
    "time"
)

// --- Interfaces (interfaces.md) ---

// Store defines the data access boundary. Defined in the consumer
// package, not the implementation package.
type Store interface {
    GetUser(ctx context.Context, id string) (*User, error)
}

// --- Types and constructors (naming.md, declarations.md) ---

// Server handles HTTP requests for the user API.
type Server struct {
    store  Store
    router *http.ServeMux
}

// NewServer creates a Server with the given dependencies.
// The caller must call Shutdown to release resources.
func NewServer(store Store) *Server {
    s := &Server{store: store}
    s.router = http.NewServeMux()
    s.router.HandleFunc("GET /users/{id}", s.handleGetUser)
    return s
}

// --- Error handling (error-handling.md) ---

// Domain errors as sentinels — checked with errors.Is.
var ErrNotFound = errors.New("not found")

// --- HTTP handler (control-flow.md, context.md, error-handling.md) ---

func (s *Server) handleGetUser(w http.ResponseWriter, r *http.Request) {
    ctx := r.Context()  // context.md: derive from request
    id := r.PathValue("id")

    user, err := s.store.GetUser(ctx, id)
    if err != nil {
        if errors.Is(err, ErrNotFound) {  // error-handling.md: errors.Is
            http.Error(w, "user not found", http.StatusNotFound)
            return  // control-flow.md: early return
        }
        // HTTP handlers are an exception to "log OR return": log detail server-side, return sanitized error to client.
        slog.Error("GetUser failed", "id", id, "err", err)
        http.Error(w, "internal error", http.StatusInternalServerError)
        return
    }

    w.Header().Set("Content-Type", "application/json")
    json.NewEncoder(w).Encode(user)
}

// --- Graceful shutdown (concurrency.md, defensive.md) ---

func main() {
    store := NewDBStore(os.Getenv("DATABASE_URL"))
    srv := NewServer(store)

    httpSrv := &http.Server{
        Addr:         ":8080",
        Handler:      srv.router,
        ReadTimeout:  5 * time.Second,   // defensive.md: use time.Duration
        WriteTimeout: 10 * time.Second,
    }

    // concurrency.md: goroutine lifetime is clear
    go func() {
        sigCh := make(chan os.Signal, 1)  // concurrency.md: channel size 1
        signal.Notify(sigCh, os.Interrupt)
        <-sigCh

        ctx, cancel := context.WithTimeout(context.Background(), 30*time.Second)
        defer cancel()  // defensive.md: defer cleanup
        if err := httpSrv.Shutdown(ctx); err != nil {
            slog.Error("server shutdown failed", "err", err)
        }
    }()

    slog.Info("starting server", "addr", httpSrv.Addr)
    if err := httpSrv.ListenAndServe(); !errors.Is(err, http.ErrServerClosed) {
        slog.Error("server error", "err", err)
        os.Exit(1)  // packages.md: exit only from main
    }
}
```

## Skills Applied

| Area | Skill | What's demonstrated |
|------|-------|---------------------|
| Interface at consumer | [interfaces](../../interfaces.md) | `Store` defined where it's used |
| Naming | [naming](../../naming.md) | MixedCaps, receiver abbreviation, clear func names |
| Error handling | [error-handling](../../error-handling.md) | Sentinels, `errors.Is`, log-or-return |
| Context | [context](../../context.md) | Derived from request, passed through |
| Control flow | [control-flow](../../control-flow.md) | Early returns for error cases |
| Concurrency | [concurrency](../../concurrency.md) | Clear goroutine lifetime, channel sizing |
| Defensive | [defensive](../../defensive.md) | `defer cancel()`, `time.Duration`, graceful shutdown |
| Packages | [packages](../../packages.md) | Exit only in `main()` |
| Logging | [logging](../../logging.md) | Structured slog, handle error once |
