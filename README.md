# Kaness CLI

Kaness is a dependency-free Go developer assistant with an ANSI terminal mascot and dual-column operation logs.

## Run

```sh
go run . help
go run . init
go run . dev          # detects Go or Node projects
go run . fix
go run . clean --dry-run
go run . clean --yes
go run . connect --url http://localhost:3000
```

Build an executable with `go build -o kaness .` and put it on your `PATH`.

`clean` only considers known local caches/artifacts (`node_modules/.cache`, `.next`, `dist`, `build`, and `.DS_Store`). It previews targets by default and requires confirmation unless `--yes` is supplied.
