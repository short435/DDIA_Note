# DDIA 第一章
---
第一章先告訴為何選擇寫有關於Data-Intensive系統的書，與書中主要的名詞解釋。
[TOC]

## Data System

資料系統是主要在各個服務背後支撐的關鍵，就目前日常會使用到的系統為例：Youtube/Gmail/Line，每個系統看似都儲存了大量的資料，卻會因為使用情況不同需要選擇不同的系統來達成，甚至需要結合不同的系統來完成主要目的。

舉一個常見的狀況，大流量的伺服器常常會使用Cache來保護一些重複查詢SQL的狀況，進而避免Slow Query或者減少重複查詢的事情發生，也能帶給使用者更好的使用體驗。

### Requirement for Data-Instensive System

* Database
* Cache
* Search Index
* Stream Processing
* Batch Processing

## 可靠性 - Reliability

軟體工程對於可靠性的基本解釋：
* 表現出使用者期望的功能
* 儘管使用的方式錯誤也不影響
* 在設定的負荷基準下，能夠滿足各種功能
* 能夠避免未經授權的使用

簡而言之，可靠性就是，能使應用程式“正常”的運行，即使有任何錯誤。

==容忍代替阻止，預防勝於治療==

可以藉由將錯誤訊息寫入例如TestCase中，來確認系統足夠承受錯誤，但有些對於系統所造成的破壞，是無法有方式治療的，所以對於此種方式則採用預防，例如：預防駭客侵入竊取資料(EXample: SQL Injection)。

### Hardware Fault

當前雲端架構設計是由大量的機器所組成的，有很多概念是從之而出的Ex:Microservices，因此提高對於單台機器故障的容忍度變得相當的重要，在有某台機器故障時，如何應對負載，如何避免整個系統重啟。

### Software Fault

軟體造成的錯誤與硬體不同，硬體的錯誤具有隨機性並且不會產生連鎖效應，但是軟體卻完全相反，單一節點的錯誤可能串連造成系統性錯誤，以下有幾個常見例子。

- 輸入錯誤
  - Linux System leap second on June 30, 2012 [Link](https://www.somebits.com/weblog/tech/bad/leap-second-2012.html)
- 硬體資源耗盡
  - 為常見的Memory Leak
  - Internet Bandwidth
- 系統所依賴的其他系統
  - API no response
- 連鎖錯誤

避免軟體的錯誤需要從細節做起，統整幾個常見方式。

- Testing
  - Unit test
  - Integration test
- 隔離程式且模組化程式
- Restart or reboot stragty
- Monitoring

### Human Fault

人為錯誤往往是造成問題的最大原因，需要組合多樣的方式來防範與增進系統，底下列舉了幾個常見的方式。

- 最小化犯錯機會
  - 改善後台系統、API設計、等等...，最好的系統並不是最細節最多的，越是多餘複雜的系統反而可能造增人為的忽略偷工減料，找到最佳的平衡才是最重要的。
- Testing
  - Unit test
  - Integration test
- Rollback
- Monitoring
- Traning

仔細看可以發現，上述的所有方式，無一例外地出現在現代CICD的概念中，測試、回版、監控、等...，都是相當重要的步驟。

## 可伸縮性 - Scalability

負載增加往往是造成服務降級(degration)的主要成因之一，無法保證你的系統可以永遠穩定的運作下去，這時可伸縮性變成為了一個重要的條件。
可伸縮性並非單純對於某一個條件下去定義，書中列舉了Twitter的案例，來說明設計者必須理解，當你的系統朝著某個方向發展時，有哪些方式可以提供成長帶來的需求。

### Describle Loading

書中提到了Twitter的案例，如何去定義你的負載，則先從服務講起。

Twitter主要服務

1. 推文系統
2. 首頁更新系統

**Method1**
單純考慮1.的時候使用SQL去建立一個如下的表單，使用SQL去查詢每個使用者所追隨的帳號其底下所有的推文應該是最直觀的做法。

追蹤者Table
```
| follower_id | followee_id |
```
發文Table
```
| id | sender_id | text | timestamp |
```
User Table
```
| id | screen_name | profile_name |
```

**Method2**
但此做法無法滿足首頁更新系統快速且大量的Loading，因此Twitter使用Cache去儲存每個使用者當下的首頁推文，並且在有新推文產生時，依據追蹤列表來更新Cache。

Method2相較於Method1更能夠平衡且快速的滿足1.與2.的條件，前提是所有的使用者的粉絲數接很平均能夠滿足系統提供的Loading。

**例外**
當某個使用者擁有過於大量的粉絲數時，大量的Cache寫入必定使得系統使系統降階。

### Describing Performance

討論負載時的兩個角度

1. 資源不變下，提高負載造成的影響
2. 負載提高時，需要增加多少資源

常見系統性能

1. Throughtput
2. Response Time
3. Latency

定義系統反應時間的方式

- **平均值**
  - 無法準確分析
- **百分比(percentiles)**
  - 準確抓出Edge case
  - Example
    - 50p的響應為200ms，即代表有一半(50%)的情況小於200ms，其餘則反之
    - 更近一步的提高百分比可以得到，90p(90%)/99p(99%)/999(99.9%)p


常見問題

-  tail latencies
-  head-of-line blocking




