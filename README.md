# web_server
.
- A small, POSIX-oriented HTTP web server written in C++98 (epoll-based event loop). This project implements a minimal but featureful HTTP server with support for:

- Level-triggered epoll-based event loop and file descriptor management
- HTTP parsing and response generation
- CGI script handling (for dynamic content)
- Multipart/form-data parsing (file uploads)
- Custom configuration and error pages
- Simple test pages and CGI test scripts in `test/`

This README explains how to build, run and extend the server. More design details are available in the `docs/` folder.

## Quick start

Requirements

- C++98 toolchain (g++/clang++)
- make (a `Makefile` is included)
- Linux (the server uses epoll)

Build

1. From the project root run:

   make

   This will build the project using the provided `Makefile`. If your environment differs, you can build the binary manually by inspecting the `Makefile` or compiling `src/` files directly.

Run

1. After building, run the produced binary. The Makefile typically generates an executable in the project root (check the output of `make`). For example:

   ./webserv             # or the executable name produced by your build

2. The server reads its configuration from `configs/default.conf` by default. Update that file to change the listening port, document root and other settings.

3. Open a browser and visit http://localhost:<port>/ where `<port>` is the port set in your `configs/default.conf`.

## Configuration

Configuration files live in the `configs/` directory. The default server configuration is `configs/default.conf`.

Error pages are stored in `configs/error_pages/` (for example `404.html`). You can customize the error pages there.

## Project layout

- `src/` — C++ source files. Main server code and feature implementations.
  - `server/` — event loop, epoll integration, socket and server bootstrap code.
  - `http/` — HTTP parser, request/response handling and multipart parsing.
  - `cgi/` — CGI handling implementation.
  - `Config/`, `error_pages/`, `Routing/`, `utils/` — supporting components.
- `include/` — public headers for the project (mirrors `src/` structure).
- `configs/` — configuration files and default error pages.
- `docs/` — design and implementation notes (read these to understand architecture and event loop choices).
- `test/` — test HTML pages and CGI test scripts used for manual testing.
- `www/` — example static site files used as a document root in tests.
- `sessions/` — Python helper files for session/cookie experiments (not required for the server binary itself).

## Tests and manual checks

There are simple manual test pages under `test/` and `test/cgi_scripts/`:

- `test/test.html` and `test/upload.html` — quick pages to verify static serving and multipart uploads.
- `test/cgi_scripts/` — small CGI scripts (Python and C) used to test CGI handling.

To test:

1. Start the server.
2. Point your browser at the test pages or use `curl` to exercise endpoints, uploads, and CGI scripts.

Example:

```
curl -v http://localhost:8080/test/test.html
```

## Development notes

- The event loop is implemented in `src/server/EventLoop.cpp` and uses `Epoll` and `FdManager` helpers.
- HTTP parsing and request routing is implemented in `src/http/` and `src/Routing/`.
- CGI handling forks and attaches standard streams to handle dynamic scripts; check `src/cgi/CGIHandler.cpp` and corresponding headers in `include/cgi/`.
- See `docs/` for architecture rationale (level-triggered vs edge-triggered epoll, event loop design, etc.).

## Contributing

Contributions are welcome. Suggested workflow:

1. Fork the repository.
2. Create a feature branch.
3. Add tests or manual instructions demonstrating the behavior.
4. Open a pull request describing the change and reasoning.

Please keep changes small and focused. If you change public headers in `include/`, update callers and add brief tests or usage examples.

## TODOs & improvements

- Add an automated unit / integration test suite.
- Add CI build configuration (GitHub Actions) to build and run quick smoke tests.

## License

MIT license is included in this repository.

----

For a deep dive into the design and implementation, see the files in `docs/` (particularly `IMPLEMENTATION_SUMMARY.md` and `EVENT_LOOP_ARCHITECTURE.md`).
