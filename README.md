# Airbrake Golang Notifier 

<img src="http://f.cl.ly/items/3J3h1L05222X3o1w2l2L/golang.jpg" width=800px>

# Example

```go
package main

import (
	"errors"

	"github.com/sendgrid/gobrake"
)

var airbrake = gobrake.NewNotifier(1234567, "FIXME")

func init() {
	airbrake.AddFilter(func(notice *gobrake.Notice) *gobrake.Notice {
		notice.Context["environment"] = "production"
		return notice
	})
}

func main() {
	defer func() {
		if v := recover(); v != nil {
			airbrake.Notify(v, nil)
			panic(v)
		}
	}()
	defer airbrake.Flush()

	airbrake.Notify(errors.New("operation failed"), nil)
}
```

## Ignoring notices

```go
airbrake.AddFilter(func(notice *gobrake.Notice) *gobrake.Notice {
	if notice.Context["environment"] == "development" {
		// Ignore notices in development environment.
		return nil
	}
	return notice
})
```
