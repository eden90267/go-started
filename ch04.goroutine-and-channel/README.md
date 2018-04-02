# Go 語言 goroutine 和 channel

## 什麼是 goroutine

使用時機

- 程序複雜
- 可背景執行

最基本 go routine 方式：

```go
// main.go
package main

import (
  "fmt"
  "time"
)

func do(i int) {
  fmt.Println(i)
}

func main() {
  go do(1)
  go do(2)
  go do(3)
  time.Sleep(1 * time.Second)
}
```

有如下問題：

```go
// main.go
package main

import (
  "fmt"
  "time"
)

func do(i int) {
  fmt.Println(i)
}

func main() {
  msg := "Let's Go"
  go func() {
    fmt.Println(msg) // 執行結果是 Let's GoGoGo
  }()
  msg = "Let's GoGoGo"
  time.Sleep(1 * time.Second)
}
```

如何解決？

```go
package main

import (
  "fmt"
  "time"
)

func do(i int) {
  fmt.Println(i)
}

func main() {
  msg := "Let's Go"
  go func(input string) {
    fmt.Println(input) // 執行結果就是 Let's Go
  }(msg)
  msg = "Let's GoGoGo"
  time.Sleep(1 * time.Second)
}
```

外面變數丟到 go routine 的 func 裡面，就可避免受到外面的影響

```shell
$ go run -race main.go # 幫你偵測有沒有 data race condition 的狀況
```

## 使用 sync WaitGroup 等待 goroutine 執行結束

```go
// main.go
package main

import (
  "fmt"
  "time"
)

func do(i int) {
  fmt.Printf("start job: %d\n", i)
  time.Sleep(1 * time.Second)
  fmt.Printf("end job: %d\n", i)
}

func main() {
  go do(1)
  go do(2)
  go do(3)
  
  time.Sleep(2 * time.Second)
}
```

實際情況不會等到那樣的秒數，這時就可以使用 go 內建的 sync 包

```go
package main

import (
  "fmt"
  "sync"
  "time"
)

func do(i int, wg *sync.WaitGroup) {
  fmt.Printf("start job: %d\n", i)
  time.Sleep(1 * time.Second)
  fmt.Printf("end job: %d\n", i)
  wg.Done()
}

func main() {
  wg := sync.WaitGroup{}
  wg.Add(3)
  go do(1, &wg)
  go do(2, &wg)
  go do(3, &wg)

  wg.Wait()
  fmt.Println("Done!!")
}
```

最簡單也最常用在大型專案的方式來等待所有 goroutine 做完總結才做接下來的事情