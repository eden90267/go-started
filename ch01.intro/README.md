# Go 語言介紹

## Go 課程介紹

- Web development
- System programming*
- DevOps*

## Go 語言歷史

2007 年開始是 Google side project (20%)，到 2009 年公開釋出

### 四件事情讓 Go 發展得更好

- Ian Lance Taylor 的加入
- Russ Cox 在 2008 年加入
  - 實現了 http.HandlerFunc 及 io 接口
- 安全專家 Adam Langley 加入
  - 協助底層 lib
- Docker / Kubernetes 使用 Go (2013 年及 2014 年)

Go 用量最大：中國

為跑更大 services，從 Java、C++ 轉向 Go

七牛雲儲存空間從 200 台伺服器變成 3 台

### Go 發佈週期

2016/08 1.5，Google 規定以後每半年發佈一版

- 2018/02 1.10 (最新版)

可查看這篇：Go 歷史沿革 (漫畫版)

## Go 語言優勢

### 為什麼設計 Go 語言

根據 Rob Pike 大神描述，Google 遇到的問題：

- 大量的 C++ 代碼，同時引入 Java 和 Python
- 成千上萬的工程師 (每個人風格不同)
- 數百萬的程式碼 (如何減少代碼產量)
- 分散式編譯系統 (交叉編譯速度...)
- 數百萬的伺服器 (部署時間...)

### Go 語言特性

- 沒有物件導向 (無繼承特性)
  - 所以才這麼快
- 強制類型
- Function 和 Method
- 沒有錯誤處理
- 用字首來區別可否存取
  - 大小寫來區別
- 不用 Import 或變數會引起編譯錯誤
- 完整的標準函式
- 支援 UTF-8 格式

### Go 優勢

- 學習曲線
- 開發及執行效率
- 由 Google 維護
- 部署方便 (binary 檔到處丟就可執行)
- 跨平台編譯
- 內建 Coding Style, Testing 等工具
- 多核心處理

## Go 語言大型專案

- docker
- kubernetes
- etcd
- influxdata
- Gitea
- HUGO
- Prometheus
- Grafana
- Caddy
- Gin-Gonic
- Drone

## Go 語言導入團隊

- [https://github.com/golang/go/wiki/WhyGo](https://github.com/golang/go/wiki/WhyGo)
- [https://github.com/golang/go/wiki/FromXToGo](https://github.com/golang/go/wiki/FromXToGo)

### 如何將 Go 語言導入團隊

- 學習曲線
  - 程式碼簡潔
  - 沒有物件導向
- 團隊開發工具整合
  - Coding Style
  - Testing Tool
  - Benchmark Tool
- 部署環境 (Go 1.5 Cross Compiler)
  - 降低部署時間
  - 降低測試時間
  - 重啟時間非常快，Load-Balancer 不需要 Pre-warning
- 系統效能 (記憶體用量，CPU 使用率 ...)
  - EC2 使用量降低 (降低 80 ~ 85%)
  - Response time 100ms -> 10ms

### 從商業利益看 Go 程式語言

[https://blog.wu-boy.com/2017/01/business-benefits-of-go/](https://blog.wu-boy.com/2017/01/business-benefits-of-go/)