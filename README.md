# Gentls: self-signed TLS

[![Workflow](https://github.com/AlexanderMatveev/gentls/actions/workflows/go.yml/badge.svg)](https://github.com/AlexanderMatveev/gentls/actions)
[![Go Report Card](https://goreportcard.com/badge/github.com/AlexanderMatveev/gentls)](https://goreportcard.com/report/github.com/AlexanderMatveev/gentls)


## Overview

`gentls` is a small package to generate self-signed TLS for development purposes.

## Example

```go
package main

import (
	"crypto/x509/pkix"
	"fmt"
	"github.com/AlexanderMatveev/gentls"
	"log"
	"net/http"
	"time"
)

func main() {
	server := http.Server{
		Addr:    fmt.Sprintf(":%d", 3443),
	}
	start := time.Now()
	log.Print("Generating TLS certs")
	if server.TLSConfig, err = owntls.Generate(1024, pkix.Name{
		Organization: []string{"SelfSigned"},
		Country:      []string{"RU"},
		Locality:     []string{"Moscow"},
	}, time.Now().AddDate(10, 0, 0)); err != nil {
		log.Fatal(err)
	}
	log.Printf("TLS certs generated in %s", time.Since(start))
	log.Fatal(server.ListenAndServeTLS("", ""))
}

```