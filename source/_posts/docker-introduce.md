---
title: docker-introduce
date: 2022-05-10 12:35:05
categories: docker
tags: docker container image
---

# what is Docker
Docker 是一種軟體平台，可讓你快速地建立、測試和部署應用程式。Docker 將軟體封裝到名為容器的標準化單位，其中包含程式庫、系統工具、程式碼和執行時間等執行軟體所需的所有項目。使用 Docker，可以將應用程式快速地部署到各種環境並加以擴展，而且知道程式碼可以執行。
在很多公有雲上執行 Docker 可讓開發人員和管理員以高度可靠且低成本的方式建立、發佈和執行各種規模的分散式應用程式。
![label](docker-introduce/docker.png) 
---
# why is Docker
使用 Docker 可快速交付程式碼、標準化應用程式操作、無縫移動程式碼，以及透過提高資源使用率節省成本。可以使用 Docker 獲得能夠隨處可靠執行的單一物件。Docker 簡單易懂的語法還能提供完整的控制權。廣泛採用代表有穩固的工具和立即可用應用程式生態系統隨時可供 Docker 使用。
1. 更快地發佈更多軟體
    - Docker 使用者發佈軟體的頻率比非 Docker 使用者平均高出 7 倍。Docker 可視需要頻繁地交付單獨的服務。
2. 標準化操作
    - 小型的容器化應用程式可讓使用者輕鬆部署、發現問題，並透過復原來補救。
3. 無縫移動
    - 以 Docker 為基礎的應用程式可以從本機開發機器無縫地遷移到公有雲上的生產部署。
4. 節省資金
    - Docker 容器可在每部伺服器上輕鬆執行更多程式碼，以提升使用率和節省資金。

---
# How to Use
1. 如何安裝
    - 從官網下載桌面版: [install on mac](https://docs.docker.com/desktop/mac/install/)
    - 有brew，就直接： ``` brew cask install docker```

2. 簡單驗證
   - ```docker --version```
   - ```docker image ps```

# 參考連結

[docker 官網](https://www.docker.com/)
