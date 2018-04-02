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

```go
// helloworld.go
package main

func helloworld() string {
  return "Hello World"
}
```

```go
// main.go
package main

import "fmt"

func main() {
  fmt.Print(helloworld())
}
```

```shell
$ go run main.go helloworld.go
```

package 必須都是同一名稱：main

再來是多一層目錄的做法：

```go
// helloworld/helloworld.go
package hello

func HelloWorld() string {
  return "Hello World"
}
```

```go
// main.go
package main

import (
  "fmt" // GOROOT 的 src 目錄底下

  "github.com/eden90267/go-first/helloworld" // 外部引用，但也很少用 subfolder，大多在同層目錄，看習慣 
  //"./helloworld" // 也可以，但不是最好寫法，大多數都是上面的寫法
)

func main() {
  fmt.Print(hello.HelloWorld())
}
```

### 三種 import package 方法：

1. path 方式 import

  ```go
  // foo/helloworld.go
  package foo
  
  func HelloWorld() string {
    return "Hello World"
  }
  ```

  ```go
  // main.go
  package main
  
  import (
    "fmt"
  
    "github.com/eden90267/go-first/foo"
  )
  
  func main() {
    fmt.Print(foo.HelloWorld())
  }
  ```

2. 相同 package name 採用別名方式：

  ```go
  package main
  
  import (
    "fmt"
  
    bar "github.com/eden90267/go-first/foo"
  )
  
  func main() {
    fmt.Print(bar.HelloWorld())
  }
  ```

3. 直接用 “.” 的方式 (不建議，不好 debug，也會造成團隊混亂)

  ```go
  package main
  
  import (
    "fmt"
  
    . "github.com/eden90267/go-first/foo"
  )
  
  func main() {
    fmt.Print(HelloWorld())
  }
  ```

4. 使用 “_” 方式

  ```go
  // foo/helloworld.go
  package foo
  
  import "fmt"
  
  func init()  { // 所有 package 預設都會執行
    fmt.Println("foo init")
  }
  
  func HelloWorld() string {
    return "Hello foo"
  }
  ```

  ```go
  // bar/helloworld.go
  package bar
  
  import "fmt"
  
  func init()  {
    fmt.Println("bar init")
  }
  
  func HelloWorld() string {
    return "Hello bar"
  }
  ```

  ```go
  // main.go
  package main
  
  import (
    "fmt"
    _ "github.com/eden90267/go-first/bar" // 只要用 init 方式 (global manager 用)
    _ "github.com/eden90267/go-first/foo"
  )
  
  func main() {
    //fmt.Print(HelloWorld())
    fmt.Println("Hello main")
  }
  ```

## 如何宣告 go 變數

有三種方式

### 第一種：var

```go
// main.go
package main

import (
  "fmt"
)

var foo string
var bar int

func main() {
  foo = "Hello"
  bar = 100
  fmt.Println(foo)
  fmt.Println(bar)
  fmt.Println("Done!!")
}
```

### 第二種：var 宣告多個變數名稱

```go
// main.go
package main

import (
  "fmt"
)

var (
  foo string
  bar int
)

func main() {
  foo = "Hello"
  bar = 100
  fmt.Println(foo)
  fmt.Println(bar)
  fmt.Println("Done!!")
}
```

or var 宣告多個變數直接指定值

```go
package main

import (
  "fmt"
)

var (
  foo string = "Hello"
  bar int = 100
)

func main() {
  fmt.Println(foo)
  fmt.Println(bar)
  fmt.Println("Done!!")
}
```

### 第三種：`:=`

- 代表從沒宣告過，直接指定值，type 會視給的值決定
- 有區域性

```go
// main.go
package main

import (
  "fmt"
)

func main() {
  foo := "Hello"
  bar := 100
  fmt.Println(foo)
  fmt.Println(bar)
  fmt.Println("Done!!")
}
```

大多數使用 `:=` 的方式，比較簡潔，同時具有區域性，盡量少用 global variable

### 第四種：`const`

```go
package main

import (
  "fmt"
)

const (
  Monday = iota + 1 // iota 預設是 0，拿掉 + 1 Monday ~ Wednesday 就是 0, 1, 2
  Tuesday
  Wednesday
)

func main() {
  foo := "Hello"
  bar := 100
  fmt.Println(foo)
  fmt.Println(bar)
  fmt.Println(Monday)
  fmt.Println(Tuesday)
  fmt.Println(Wednesday)
  fmt.Println("Done!!")
}
```

## 如何使用 go func

- 單一回傳值 func
- 多重回傳值 func
- func 回傳 func
- Anonymous Func (常使用在 go routine)
- func 參數設計

### 單一回傳值 func

```go
// main.go
package main

import (
  "fmt"
)

func add(i, j int) int {
  return i + j
}

func main() {
  fmt.Println(add(1, 2))
}
```

### 多重回傳值 func

```go
package main

import (
  "fmt"
)

func swap(i, j int) (int, int) {
  return j, i
}

func main() {
  a := 1
  b := 2
  a, b = swap(a, b)
  fmt.Println("a:", a)
  fmt.Println("b:", b)

  a, b = b, a // 其實這樣就可以 swap 了
  fmt.Println("a:", a)
  fmt.Println("b:", b)
}
```

### func 回傳 func

```go
package main

import "fmt"

func foo() func() int {
  return func() int {
    return 100
  }
}

func main() {
  bar := foo()
  fmt.Printf("%T\n", bar)
  fmt.Println(bar())


  bar2 := func(i, j float32) float32 {
    return i + j
  }
  fmt.Printf("%T\n", bar2)
  fmt.Println(bar2(1, 45+2.3))
}
```

## Anonymous Func

- go routine
- func 只用一次

```go
package main

import "fmt"

func main() {
  foo := func() string {
    return "Hello world"
  }
  fmt.Println(foo())

  bar := func() {
    fmt.Println("Hello World 2")
  }
  bar()

  func() {
    fmt.Println("Hello World 3")
  }()

  go func(i, j int) { // 沒印出來 3 是因為還沒到 go routine 這 main function 就已經結束了
    fmt.Println(i+j)
  }(1,2)
}
```

### func 參數設計

```go
package main

import (
  "fmt"
  "strings"
)

func getUserListSQL(username, email string, sexy int) string {
  sql := "select * from user"
  where := []string{}

  if username != "" {
    where = append(where, fmt.Sprintf("username = '%s'", username))
  }
  if email != "" {
    where = append(where, fmt.Sprintf("email = '%s'", email))
  }
  if sexy != 0 {
    where = append(where, fmt.Sprintf("sexy = '%d'", sexy))
  }

  return sql + " where " + strings.Join(where, " or ")
}

type searchOpts struct {
  username string
  email string
  sexy int
}

func getUserListOptsSQL(opts searchOpts) string {
  sql := "select * from user"
  where := []string{}

  if opts.username != "" {
    where = append(where, fmt.Sprintf("username = '%s'", opts.username))
  }
  if opts.email != "" {
    where = append(where, fmt.Sprintf("email = '%s'", opts.email))
  }
  if opts.sexy != 0 {
    where = append(where, fmt.Sprintf("sexy = '%d'", opts.sexy))
  }

  return sql + " where " + strings.Join(where, " or ")
}

func main() {
  fmt.Println(getUserListSQL("eden90267", "", 0))
  fmt.Println(getUserListSQL("eden90267", "eden90267@gmail.com", 1))

  fmt.Println(getUserListOptsSQL(searchOpts{
    username: "eden90267",
    email: "eden90267@gmail.com",
  }))

  fmt.Println(getUserListOptsSQL(searchOpts{
    sexy: 2,
  }))
}
```

## 錯誤處理 (Error Handler)

- fmt, errors 的方式
- error func

### fmt, errors 的方式

```go
// main.go
package main

import (
  "errors"
  "fmt"
)

func checkUseNameExist(username string) (bool, error) {
  if username == "foo" {
    return true, fmt.Errorf("username %s already exist", username)
  }
  if username == "bar" {
    return true, errors.New("username bar already exist") // 較多人在用
  }

  return false, nil
}

func main() {
  if _, err := checkUseNameExist("foo"); err != nil {
    fmt.Println(err)
  }

  if _, err := checkUseNameExist("bar"); err != nil {
    fmt.Println(err)
  }
}
```

### error func

```go
// main.go
package main

import (
  "errors"
  "fmt"
)

type errUserNameExist struct {
  UserName string
}

func (e errUserNameExist) Error() string {
  return fmt.Sprintf("username %s already exist", e.UserName)
}

func isErrUserNameExist(err error) bool { // error 為 interfaces, 裡面有定義一個 Error() string func 的成員
  _, ok := err.(errUserNameExist) // 轉換成你指定的型態
  return ok
}

func checkUseNameExist(username string) (bool, error) {
  if username == "bar" {
    return true, errors.New("bar exists")
  }
  if username == "foo" {
    return true, errUserNameExist{UserName: username}
  }

  return false, nil
}

func main() {
  if _, err := checkUseNameExist("foo"); err != nil {
    if isErrUserNameExist(err) {
      fmt.Println(err)
    }
  }

  if _, err := checkUseNameExist("bar"); err != nil {
    if isErrUserNameExist(err) {
      fmt.Println(err)
    } else {
      fmt.Println("isErrUserNameExist is false");
    }
  }
}
```