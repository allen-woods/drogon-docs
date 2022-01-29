**Drogon** is a C++14/17-based HTTP application framework. Drogon can be used to easily build various types of web application server programs using C++.

**Drogon** is the name of a dragon in the American TV series "Game of Thrones" that I really like.

Drogon's main application platform is Linux. It also supports Mac OS, FreeBSD and Windows.

Its main features are as follows:

- Use a non-blocking I/O network lib based on epoll (kqueue under macOS/FreeBSD) to provide high-concurrency, high-performance network IO &mdash; please visit the [TFB Tests Results](https://www.techempower.com/benchmarks/#section=data-r19&hw=ph&test=composite) for more details;
- Provide a completely asynchronous programming mode;
- Provide a lightweight command-line tool (drogon_ctl) that automatically generates C++ code files for compilation, simplifies the creation of various classes in Drogon, and easily generates view code;
- Support HTTP1.0/1.1 (server side and client side);
- Based on template, a simple reflection mechanism is implemented to completely decouple the main program framework, controllers, and views.
- Support cookies and built-in sessions;
- Support server-side rendering (SSR), the controller generates data that is interpolated by a view to generate an HTML page.
- Support templating, views are described by CSP template files and C++ code is embedded into HTML pages through CSP tags.
- Support view page dynamic loading (dynamic compilation and loading at runtime);
- Provide a convenient and flexible routing solution from the path to the controller handler;
- Support filter chains to facilitate the execution of unified logic (such as login verification, HTTP Method constraint verification, etc.) before handling HTTP requests;
- Support HTTPS (based on OpenSSL);
- Support WebSocket (server side and client side);
- Support JSON format request and response, very friendly to the RESTful API application development;
- Support file download and upload;
- Support gzip, brotli compression transmission;
- Support pipelining;
- Support non-blocking, I/O based, asynchronous database reads and writes for PostgreSQL and MySQL (MariaDB);
- Support asynchronous database reads and writes for sqlite3 based on thread pool;
- Support ARM Architecture;
- Provide a convenient, lightweight ORM implementation that supports regular object-to-database bidirectional mapping;
- Support plugins which can be installed by the configuration file at load time;
- Support AOP with built-in joinpoints.

# 02 [Install drogon](ENG-02-Installation)
