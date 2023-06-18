[![Build Status](https://github.com/mu-box/microbox-router/actions/workflows/ci.yaml/badge.svg)](https://github.com/mu-box/microbox-router/actions)
[![GoDoc](https://godoc.org/github.com/mu-box/microbox-router?status.svg)](https://godoc.org/github.com/mu-box/microbox-router)

# microbox-router
Simple client for creating and updating custom defined http[s] and ws[s] proxies/ssl-termination without restarting a server application.

## Status
Stable/Complete

## Quickstart
Implement a server in go (run it)
```go
// main.go
package main

import (
  "fmt"
  "time"
  "os"

  "github.com/mu-box/microbox-router"
)

func main() {
  // start http router (use a different port if you wish, just be sure you have permission)
  err := router.StartHTTP("0.0.0.0:8888")
  if err != nil {
    fmt.Printf("Failed to start - %v\n", err)
    os.Exit(1)
  }
  // configure a route
  routes := []router.Route{router.Route{Domain: "test.com", Page: "World, Hello!\n"}}
  // update the routes
  router.UpdateRoutes(routes)

  // do nothing for a minute (give yourself some time to check out your router)
  time.Sleep(time.Minute)
}
```
Test it
```sh
# make sure your ip/port matches that of your server
$ curl -H 'Host: test.com' 127.0.0.1:8888
World, Hello!
```
Congratulations proxymaster! Be sure to check out the [godocs](https://godoc.org/github.com/mu-box/microbox-router) to enhance your app's functionality.

## Data types:
#### Route:
Fields:
- **SubDomain**: Subdomain to match on
- **Domain**: Domain to match on
- **Path**: Path to match on
- **Targets**: URIs of servers
- **FwdPath**: Path to forward to targets (combined with target path)
- **Page**: Page to serve instead of routing to targets

go:
```go
Route{
  SubDomain: "admin",
  Domain: "test.com",
  Path: "/admin*",
  Targets: []string{"http://127.0.0.1:8080/app1","http://127.0.0.2"},
  FwdPath: "/",
  Page: "",
}
```
json (for end-user implementations):
```json
{
  "subdomain": "admin",
  "domain": "test.com",
  "path": "/admin*",
  "targets": ["http://127.0.0.1:8080/app1","http://127.0.0.2"],
  "fwdpath": "/",
  "page": ""
}
```

#### KeyPair:
Fields:
- **Key**: Pem style key
- **Cert**: Pem style cert

go:
```go
KeyPair{
  Key: "-----BEGIN PRIVATE KEY-----\nMII.../J8\n-----END PRIVATE KEY-----",
  Cert: "-----BEGIN CERTIFICATE-----\nMII...aI=\n-----END CERTIFICATE-----",
}
```

## Contributing

Contributions to the microbox-router project are welcome and encouraged. Contributions should follow the [Microbox Contribution Process & Guidelines](https://docs.microbox.cloud/contributing/).

## Todo

- Add configurable `ErrorLog` like `net/http/httputil`s ReverseProxy

## Licence

This project is released under [The MIT License](http://opensource.org/licenses/MIT).

[![open source](http://microbox.rocks/assets/open-src/microbox-open-src.png)](http://microbox.cloud/open-source)
