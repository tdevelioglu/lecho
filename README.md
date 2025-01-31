# lecho :tomato:

[Zerolog](https://github.com/rs/zerolog) wrapper for [Echo](https://echo.labstack.com/) web framework.

## Installation

For Echo v4:

```
go get github.com/ziflex/lecho/v3
```

For Echo v3:

```
go get github.com/ziflex/lecho
```

## Quick start

```go
package main 

import (
	"os"
	"github.com/labstack/echo/v4"
	"github.com/labstack/echo/v4/middleware"
	"github.com/ziflex/lecho/v3"
)

func main() {
    e := echo.New()
    e.Logger = lecho.New(os.Stdout)
}
```

### Using existing zerolog instance

```go
package main 

import (
	"os"
	"github.com/labstack/echo/v4"
	"github.com/labstack/echo/v4/middleware"
	"github.com/ziflex/lecho/v3"
        "github.com/rs/zerolog"
)

func main() {
    log := zerolog.New(os.Stdout)
    e := echo.New()
    e.Logger = lecho.From(log)
}

```

## Options

```go

import (
	"os",
	"github.com/labstack/echo"
	"github.com/labstack/echo/middleware"
	"github.com/ziflex/lecho/v3"
)

func main() {
    e := echo.New()
    e.Logger = lecho.New(
       os.Stdout,
       lecho.WithLevel(log.DEBUG),
       lecho.WithFields(map[string]interface{}{ "name": "lecho factory"}),
       lecho.WithTimestamp(),
       lecho.WithCaller(),
       lecho.WithPrefix("we ❤️ lecho"),
       lecho.WithHook(...),
       lecho.WithHookFunc(...),
    )
}
```

## Middleware

### Logging requests and attaching request id to a context logger 

```go

import (
	"os",
	"github.com/labstack/echo"
	"github.com/labstack/echo/middleware"
	"github.com/ziflex/lecho/v3"
	"github.com/rs/zerolog"
)

func main() {
    e := echo.New()
    logger := lecho.New(
            os.Stdout,
            lecho.WithLevel(log.DEBUG),
            lecho.WithTimestamp(),
            lecho.WithCaller(),
         )
    e.Logger = logger
    
    e.Use(middleware.RequestID())
    e.Use(lecho.Middleware(lecho.Config{
    	Logger: logger
    }))	
    e.GET("/", func(c echo.Context) error {
        c.Logger().Print("Echo interface")
        zerolog.Ctx(c.Request().Context()).Print("Zerolog interface")
	
	return c.String(http.StatusOK, "Hello, World!")
    })
}
```

## Helpers

### Level converters

```go

import (
    "fmt",
    "github.com/labstack/echo"
    "github.com/labstack/echo/middleware"
    "github.com/labstack/gommon/log"
    "github.com/ziflex/lecho/v3"
)

func main() {
	var z zerolog.Level
	var e log.Lvl
	
    z, e = lecho.MatchEchoLevel(log.WARN)
    
    fmt.Println(z, e)
    
    e, z = lecho.MatchZeroLevel(zerolog.INFO)

    fmt.Println(z, e)
}

```
