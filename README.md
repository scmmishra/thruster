# Thruster

Thruster is an HTTP/2 proxy for simple production-ready deployments of Rails
applications. It runs alongside the Puma webserver to provide a few additional
features to help your app run efficiently and safely on the open Internet:

- Automatic SSL certificate management with Let's Encrypt
- HTTP/2 support
- Basic HTTP caching
- X-Sendfile support for efficient file serving
- Automatic GZIP compression

Thruster tries to be as zero-config as possible, so most features are
automatically enabled with sensible defaults.

One exception to that is the `SSL_DOMAIN` environment variable, which is
required to enable SSL provisioning. If `SSL_DOMAIN` is not set, Thruster will
operate in HTTP-only mode.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'thruster'
```

Or install it globally:

```sh
$ gem install thruster
```

## Usage

To run your Puma application inside Thruster, prefix your usual command string
with `thrust`. For example:

```sh
$ thrust bin/rails server
```

Or with automatic SSL:

```sh
$ SSL_DOMAIN=myapp.example.com thrust bin/rails server
```

## Custom configuration

Thruster provides a number of environment variables that can be used to
customize its behavior:

| Variable Name         | Description                                                                     | Default Value |
|-----------------------|---------------------------------------------------------------------------------|---------------|
| `SSL_DOMAIN`          | The domain name to use for SSL provisioning. If not set, SSL will be disabled.  | None |
| `TARGET_PORT`         | The port that your Puma server should run on. Thruster will set `PORT` to this when starting your server. | 3000 |
| `CACHE_SIZE`          | The size of the HTTP cache in bytes.                                            | 64MB |
| `MAX_CACHE_ITEM_SIZE` | The maximum size of a single item in the HTTP cache in bytes.                   | 1MB |
| `X_SENDFILE_ENABLED`  | Whether to enable X-Sendfile support. Set to `0` or `false` to disable.         | Enabled |
| `MAX_REQUEST_BODY`    | The maximum size of a request body in bytes. Requests larger than this size will be refused; `0` means no maximum size. | `0` |
| `STORAGE_PATH`        | The path to store Thruster's internal state.                                    | `./storage/thruster` |
| `BAD_GATEWAY_PAGE`    | Path to an HTML file to serve when the backend server returns a 502 Bad Gateway error. If there is no file at the specific path, Thruster will serve an empty 502 response instead. | `./public/502.html` |
| `HTTP_PORT`           | The port to listen on for HTTP traffic.                                         | 80 |
| `HTTPS_PORT`          | The port to listen on for HTTPS traffic.                                        | 443 |
| `HTTP_IDLE_TIMEOUT`   | The maximum time in seconds that a client can be idle before the connection is closed. | 60 |
| `HTTP_READ_TIMEOUT`   | The maximum time in seconds that a client can take to send the request headers. | 30 |
| `HTTP_WRITE_TIMEOUT`  | The maximum time in seconds during which the client must read the response.     | 30 |
| `DEBUG`               | Set to `1` or `true` to enable debug logging.                                   | Disabled |
