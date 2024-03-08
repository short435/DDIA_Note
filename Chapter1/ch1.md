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

## 

