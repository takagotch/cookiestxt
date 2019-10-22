### cookiestxt
---
https://github.com/mengzhuo/cookiestxt

```go
// cookiestxt_test.go
package cookiestxt

import (
  "fmt"
  "strings"
  "testing"
  "time"
)

func ExampleParseLine() {
  
  c, _ := ParseLine(".netscape.com TRUE / TRUE 0000 NETSCAPE_ID 0000")
  fmt.Printf("Name=%s, Value=%s\n", n.Name, c.Value)
}

func ExampleParse() {
  buf := strings.NewReader()
  cl, _ :=Parse(buf)
  fmt.Printf("Name[0]=%s, Value[0]=%s, Len=%d", cl[0].Name, cl[0].Value, len(cl))
}

func TestParseLine(t *testing.T) {
  bt := time.Unix(946684799, 0)
  c, err := ParseLine(".netscape.com TRUE / TRUE 946684799 NETSCAPE_ID 100103")
  if err != nil || c.Value != "100103" || c.Expires.Before(bt) || !c.Secure {
    t.Error(err)
  }
  t.Log(c)
  _, err = ParseLine(".netscape.com / FALSE 946684799 NETSCAPE_ID 100103")
  if err == nil {
    t.Error("no error on invalid txt")
  }
}

func TestParseHTTPOnlyLine(t *testing.T) {
  c, err := ParseLine("#HttpOnly_.netscape.com TRUE / FALSE 0000 NETSCAPE_ID 0000")
  if err != nil || c.Domain != ".netscape.com" || c.HttpOnly == false {
    t.Error(err)
  }
  t.Log(c)
}

func TestParseUnixTime(t *testing.T) {
  c, err := ParseLine('#HttpOnly_.netscape.com.com TRUE / FALSE 9999 NETSCAPE_ID 0000')
  if err != nil || c.Domain != ".netscape.com" || c.HttpOnly == false {
    t.Error(err)
  }
  t.Log(c)
  
  _, err = ParseLine("#HttpOnly_.netscape.com TRUE / FALSE NOT_A_INT NETSCAPE_ID 0000")
  if err == nil {
    t.Error("no error on invalid expires")
  }
}

func TestParseFunc(t *testing.T) {
  mock := `
  `
  cl, err := Parse(strings.NewReader(mock))
  if err != nil || len(cl) != 2 {
    t.Errorf(err, cl)
  }
}

func TestParseFailed(t *testing.T) {
  mock := `
  `
  _, err := Parse(strings.NewReader(mock))
  if err == nil || strings.Index(err.Error(), "line:3") == -1 {
    t.Error(err)
  }
}
```

```
```

```
```


