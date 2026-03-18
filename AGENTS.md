# AGENTS.md

## Cursor Cloud specific instructions

### Project overview
GoFrame (`github.com/gogf/gf/v2`) is a Go multi-module monorepo framework. The root module is the core framework; `contrib/` contains driver and plugin modules; `cmd/gf/` is the CLI tool.

### Timezone requirement
Many tests in `os/gtime` and `util/gconv` expect `Asia/Shanghai` (CST, UTC+8). Always run tests with:
```
TZ=Asia/Shanghai go test ./... -race
```

### Key commands
- **Build**: `go build ./...` (from repo root for core module)
- **Lint**: `make lint` (requires `golangci-lint` on PATH; installed at `$(go env GOPATH)/bin`)
- **Test**: `TZ=Asia/Shanghai go test ./... -race` (core module)
- **Tidy all modules**: `make tidy` (runs `.make_tidy.sh` across all `go.mod` files)
- **Build gf CLI**: `cd cmd/gf && go build -o gf .`

### GOPATH/bin
`golangci-lint` is installed to `$(go env GOPATH)/bin`. Ensure this is on `PATH`:
```
export PATH=$PATH:$(go env GOPATH)/bin
```
This is already appended to `~/.bashrc` by the setup process.

### External services for contrib tests
The core framework (`/workspace/go.mod`) has no external service dependencies. Contrib module tests (under `contrib/`) require Docker services (MySQL, PostgreSQL, Redis, Etcd, etc.) — see `.github/workflows/ci-main.yml` for the full service matrix. These are only needed when working on contrib modules.

### golangci-lint config
The lint config at `.golangci.yml` uses `modules-download-mode: readonly`. Run `go mod tidy` before `make lint` if you've changed dependencies.
