
# Docker＆Django系列-Docker 是什麼？
## Docker
### 基本介紹
Docker 是一個開源專案，以 Linux 容器(Container)等技術為基礎。每個容器都是相互隔離的。
### 目標
* 實作輕量級的作業系統虛擬化
* 在實際應用上，多人開發可以確保開發環境相同
### 概念
* 映像檔(Image)：用來建立 Docker Container
* 容器(Container)：執行應用
* 倉庫(Repository)：存放映像檔的地方，每個倉庫有多個映像檔，每個映像檔有多個標籤(tag)
### 和 Virtual Machines(VMs) 比較
* Container: 把 code 和 dependencies 打包起來的應用層，多個 Container 可以在同個機器上執行，共用 OS kernal，每個 Container 仍在各自空間執行。
* VMs: 將實體硬體抽象化，一個主機中含多個虛擬主機，每個虛擬主機都還有作業系統、應用程序，及必要的文件檔，佔用數十 GB，因此啟動時間較慢。
![](https://i.imgur.com/vqhZEcI.jpg)


|  | Container | VM |
| -------- | -------- | -------- |
| 啟動時間    |快    |    慢|
| 使用空間    |數十MB|數十 GB|

#### 參考資料：
* https://ithelp.ithome.com.tw/articles/10199339
* https://www.docker.com/resources/what-container/