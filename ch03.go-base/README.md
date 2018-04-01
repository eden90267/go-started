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