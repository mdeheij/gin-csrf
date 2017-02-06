# gin-csrf

CSRF protection middleware for [Gin]. This middleware has to be used with [gin/contrib/sessions](https://github.com/gin-gonic/contrib/tree/master/sessions).

Credits to [tommy351](https://github.com/tommy351/gin-csrf) and [utrack](https://github.com/utrack/gin-csrf) making it work with gin-gonic/contrib/sessions.

## Installation

``` bash
$ go get github.com/mdeheij/gin-csrf
```

## Usage

``` go
import (
    "errors"
    
    "github.com/gin-gonic/gin"
    "github.com/gin-gonic/contrib/sessions"
    "github.com/mdeheij/gin-csrf"
)

func main(){
    r := gin.Default()
    store := sessions.NewCookieStore([]byte("secret"))
    r.Use(sessions.Sessions("mysession", store))
    r.Use(csrf.Middleware(csrf.Options{
        Secret: "secret123",
        ErrorFunc: func(c *gin.Context){
            c.String(400, "CSRF token mismatch")
			c.Abort()
        },
    }))
    
    r.GET("/getCSRF", func(c *gin.Context){
        c.String(200, csrf.GetToken(c))
    })
    
    r.GET("/protected", func(c *gin.Context){
        c.String(200, "CSRF token is valid")
    })
}
```

[Gin]: http://gin-gonic.github.io/gin/
[gin-sessions]: https://github.com/utrack/gin-sessions
