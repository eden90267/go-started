# Go 語言實戰案例

## MongoDB Transaction 交易問題

Perform Two Phase Commits 較複雜，可用 goroutine、go channel 可解決問題

### 究竟是什麼問題？

沒有 transaction
機制，會導致太多請求要求更新同一筆資料的等待造成讀取到髒資料作更新

我們可以用 go routine or go channel 方式來解決 or 其他 lock 方式來處理

## 使用 sync.Mutex 解決交易問題

可用 mongoDB 提供的 Two Phase Commits 來解決，但非常複雜

### 第 1 種 DB Transaction 解決方式：內建 sync package 的 lock 與 unlock 機制

```go
var mu = $sync.Mutex{}

// ...

mu.lock();
defer mu.Unlock() // 我在這個 func 結束之前先執行 mu.Unlock()，不用的話就放到 func 最尾巴

// 進行需同一時間執行要等上一個執行完的操作
```

常用在 key/value cache or db 上，不過效能比較差

### 第 2 種 DB Transaction 解決方式：使用 goroutine 與 go channel 寫一個簡單的 simple queue

```go

```