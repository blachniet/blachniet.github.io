---
title: "Go-YAML Field Names"
date: "2015-02-14"
categories:
- "site-updates"
tags:
- "go"
- "yaml"
aliases:
- "/blog/go-yaml-field-names/"
blachnietWordPressExport: true
---

I wasted away a good thirty minutes trying to figure out why I couldn't parse my YAML config file this morning using [go-yaml](https://github.com/go-yaml/yaml).

```go
package main

import (
    "fmt"
    "log"
    "gopkg.in/yaml.v2"
)

var configText = `
  Message: Welcome
  UpdateInterval: 5
  EmailAddresses:
    - john.doe@example.com
    - jane.doe@example.com`

type Config struct {
    Message        string
    UpdateInterval int
    EmailAddresses []string
}

func main() {
    var c Config
    if err := yaml.Unmarshal([]byte(configText), &c); err != nil {
        log.Fatalf("Failed to parse yaml: %v", err)
    }

    fmt.Printf("Config: %+v\n", c)
}
```

**Output:**

```plain
Config: {Message: UpdateInterval:0 EmailAddresses:[]}
```

As you can see in the output, no errors occur but the struct fields are not initialized properly. `Message` should be `Welcome`, `UpdateInterval` should be `5`, and there should be two email addresses.

It turns out that [go-yaml](https://github.com/go-yaml/yaml) expects the YAML field corresponding to a struct field to be lowercase. So if your struct field is `UpdateInterval`, the corresponding field in YAML is `updateinterval`. When I changed my config, I got the output I was expecting:

```go
var configText = `
  message: Welcome
  updateinterval: 5
  emailaddresses:
    - john.doe@example.com
    - jane.doe@example.com`
```

**Output:**

```plain
Config: {Message:Welcome UpdateInterval:5 EmailAddresses:[john.doe@example.com jane.doe@example.com]}
```

I didn't really like the all-lowercase field names in my config, so I used struct field tags to tell [go-yaml](https://github.com/go-yaml/yaml) what the corresponding YAML field names will look like and reverted to my original config:

```go
var configText = `
  Message: Welcome
  UpdateInterval: 5
  EmailAddresses:
    - john.doe@example.com
    - jane.doe@example.com`

type Config struct {
    Message        string   `yaml:"Message"`
    UpdateInterval int      `yaml:"UpdateInterval"`
    EmailAddresses []string `yaml:"EmailAddresses"`
}
```

**Output:**

```plain
Config: {Message:Welcome UpdateInterval:5 EmailAddresses:[john.doe@example.com jane.doe@example.com]}
```

Hopefully this will save someone else some time, because this drove me crazy this morning.
