Use these only when the project has none configured. Run them read-only to find candidates, and do not add them to the project unless asked.

| Language | Lint / style | Types | Dead code | Tests |
|---|---|---|---|---|
| Python | `ruff check` (`F401` unused imports, `BLE` broad except, `E722` bare except, `T20` print calls, `D` docstrings) | `mypy` or `pyright` | `vulture` | `pytest` or `python -m unittest` |
| JS / TS | `eslint` (`no-unused-vars`, `no-console`, `no-empty`) | `tsc --noEmit` | `knip` or `ts-prune` | the project's test runner |
| C# | `dotnet format --verify-no-changes`, analyzers | compiler warnings as errors | IDE0051 / IDE0052 unused members | `dotnet test` |
| Java / Kotlin | Checkstyle / detekt / ktlint | compiler | IDE unused-symbol inspection | Gradle / Maven test |
| Go | `go vet`, `staticcheck` | compiler | `staticcheck` U1000 | `go test ./...` |
