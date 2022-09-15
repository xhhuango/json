# JSON with NaN, +Inf, and -Inf Supported for Golang 
[![Go Report Card](https://goreportcard.com/badge/github.com/d-tsuji/awesome-go-orms)](https://goreportcard.com/report/github.com/xhhuango/json)
[![Actions Status](https://github.com/d-tsuji/awesome-go-orms/workflows/CI/badge.svg)](https://github.com/xhhuango/json/actions)
[![BSD-style license](https://img.shields.io/badge/license-BSD--style-brightgreen.svg)](https://opensource.org/licenses/MIT)
[![Go.Dev reference](https://img.shields.io/badge/go.dev-reference-blue?logo=go&logoColor=white)](https://pkg.go.dev/github.com/xhhuango/json)

A Fork from Go SDK encoding/json with NaN, +Inf, and -Inf Supported.

The release version of this package matches Go SDK version it forks from.

# Features

- A complete fork from Go SDK encoding/json
- NaN, +Inf, and -Inf are supported

## Installation

```shell
go get github.com/xhhuango/json
```

## Usage
```go
import (
	"math"
	"testing"
	
	"github.com/xhhuango/json"
)

type T struct {
	N  float64
	IP float64
	IN float64
}

func TestMarshalNaNAndInf(t *testing.T) {
    s := T{
        N:  math.NaN(),
        IP: math.Inf(1),
        IN: math.Inf(-1),
    }
    got, err := Marshal(s)
    if err != nil {
        t.Errorf("Marshal() error: %v", err)
    }
    want := `{"N":NaN,"IP":+Inf,"IN":-Inf}`
    if string(got) != want {
        t.Errorf("Marshal() = %s, want %s", got, want)
    }
}

func TestUnmarshalNaNAndInf(t *testing.T) {
    data := []byte(`{"N":NaN,"IP":+Inf,"IN":-Inf}`)
    var s T
    err := Unmarshal(data, &s)
    if err != nil {
        t.Fatalf("Unmarshal: %v", err)
    }
    if !math.IsNaN(s.N) || !math.IsInf(s.IP, 1) || !math.IsInf(s.IN, -1) {
        t.Fatalf("after Unmarshal, s.N=%f, s.IP=%f, s.IN=%f, want NaN, +Inf, -Inf", s.N, s.IP, s.IN)
    }
}
```

# See also
- [Golang encoding/json](https://pkg.go.dev/encoding/json)
