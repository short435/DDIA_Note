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

### Hardware

當前雲端架構設計是由大量的機器所組成的，有很多概念是從之而出的Ex:Microservices，因此提高對於單台機器故障的容忍度變得相當的重要，在有某台機器故障時，如何應對負載，如何避免整個系統重啟。

### Software

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

### Human

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









