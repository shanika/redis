The reactor at the heart of the server: it multiplexes all socket I/O and timed work and parses the RESP protocol off the wire.

## Responsibilities
- Run the single-threaded event loop and fire file/time events
- Accept connections over TCP, Unix sockets, and TLS, and buffer client input/output
- Parse inbound RESP into command argument vectors and serialize replies (optionally via I/O threads)

## Tech Stack
- C; `src/ae.c` with `ae_epoll`/`ae_kqueue`/`ae_evport`/`ae_select` backends, `src/networking.c`, `src/connection.c`, `src/socket.c`, `src/unix.c`, `src/tls.c`
