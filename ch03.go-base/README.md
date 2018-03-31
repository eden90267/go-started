# Go 基礎介紹

## 第一個 Hello World 程式

VSCODE setting 增加自動 import code 設定：

```
"go.formatTool": "goimports"
```

1. git clone
2. 建立 main.go

```go
// main.go
package main

import "fmt"

func main()  {
  fmt.Println("Hello World!!")
}
```

```shell
$ go run main.go
```

P.S. "gofmt" 的 formatTool 比較強大好用，存檔後即 format

```go
package main

import "fmt"

func main() {
  a := 1
  fmt.Println("Hello World!!")

  if a > 1 {
	fmt.Println(" a > 1")
  }
}
```

## 如何使用 Go Package