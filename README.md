# httprouter-chain-middleware
Chain middleware for httprouter

```go
package main

import (
  "fmt"
  "net/http"
  "log"
  "time"

	"github.com/julienschmidt/httprouter"
	"github.com/aldidana/httprouter-chain-middleware"
	"github.com/fatih/color"
)

func Logger(w http.ResponseWriter, r *http.Request, p httprouter.Params) error {
	start := time.Now()

	yellow := color.New(color.FgYellow).SprintFunc()
	green := color.New(color.FgGreen).SprintFunc()

	color.Set(color.FgCyan)

	log.Printf("%s\t%s\t%s", yellow(r.Method), yellow(r.RequestURI), green(time.Since(start)))

	color.Unset()

	return nil
}

func Index(w http.ResponseWriter, r *http.Request, _ httprouter.Params) error {
    fmt.Fprint(w, "Welcome!\n")
    return nil
}

func main() {
  router := httprouter.New()
  router.GET("/", middleware.Chain(Logger, Index))

  log.Fatal(http.ListenAndServe(":8080", router))
}
```
