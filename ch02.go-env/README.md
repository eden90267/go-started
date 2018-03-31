# Go 環境介紹

## 用 gvm 安裝 Go 語言

1. [https://golang.org/dl/](https://golang.org/dl/)
2. 套件管理多重版本：[Go Version Manager](https://github.com/moovweb/gvm)

### gvm

(以 Mac OS X 為環境安裝)

```shell
$ zsh < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)
```

```shell
xcode-select --install
brew update
brew install mercurial
```

```shell
$ gvm install go1.7 -B
$ gvm use go1.7
$ export GOROOT_BOOTSTRAP=$GOROOT
$ gvm install go1.10
```

```shell
$ gvm use go1.10 --default
$ go env
```

P.S. Mac OS Sierra 會噴 fatal error: MSpanList_Insert 的 error，網路上解決方式用 go1.7 去編譯

## VSCode 編譯器搭配 Go 環境

1. Install plugin

    search text: go，then install plugin

2. 喜好設定 > setting

    ```json
    "go.formatTool": "gofmt",
    "go.goroot": "/Users/eden.liu/.gvm/gos/go1.10",
    "go.gopath": "/Users/eden.liu/git/go"
    ```

    goroot 與 gopath 在 terminal 打 `go env` 就可以看到了

## 什麼是 GOPATH 及 GOROOT

- GOROOT

    source code 的路徑

- GOPATH

    - go 的安裝環境，需要設定
    - 在 GO 1.8 之後已經預設將 GOPATH 設定在 `${HOME}/go` 路徑，
    - 如果沒設定的話就都會放在 `${GOROOT}` 底下

### GOPATH 目錄局結構

- bin
    - 編譯後產生可執行檔案
- src
    - 存放程式碼 (工作目錄)
- pkg
    - 放置編譯後的 .a 檔案 (讓你下次編譯更快)

### 目錄結構格式

- src/網域名稱/帳戶名稱/專案名稱
- src/github.com/eden90267/helloworld

### 實例

```shell
$ cd ~/git/go
$ go get github.com/appleboy/com
$ cd src/github.com/appleboy
$ mkdir helloworld
$ code helloworld
```

```go
// hello.go
package main

import (
	"fmt"
	"github.com/appleboy/com/random"
)

func main()  {
	fmt.Println("Hello World!!")
	fmt.Println(random.String(10))
}
```

```shell
$ go build .
$ ./helloworld
$ go run hello.go
```

## Go 指令介紹

- go get

  取得 go 的 source code，將目錄抓下來，並會看是否有 main
  function，如果有表示是一個執行檔，就會把該專案變成一個執行檔並放到 bin 資料夾

  ```shell
  $ go get github.com/go-training/drone-golang-example
  $ ls -al ~/git/go/bin
  ```

  建議設定 bash 檔：

  ```
  export PATH=$GOPATH/bin:$PATH
  ```

  這樣 go get 好用的執行檔就可直接使用

- go run：直接 run 你的執行檔。他其實就是把 go build 的執行檔拿出來執行，簡寫的意思
- go test：幫你做 go testing

    ```shell
    $ cd ~/git/go/src/github.com/go-training/drone-golang-example
    $ go test .
    ```

- go install

  幫你把 source code 編譯成 binary，丟到 bin folder 底下

  ```shell
  $ go install .
  ```

- go build

    在你專案目錄底下直接 build 出 binary file

    ```shell
    $ go build .
    # 可指定 binary file name
    $ go build -o test .
    ```