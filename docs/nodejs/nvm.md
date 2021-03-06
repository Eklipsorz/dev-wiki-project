---
sidebar_position: 2
---


# NVM

## 簡介
全名為 Node Version Manager (NVM)，是第三方提供的Node.js的版本管理器套件，專門切換不同版本的Node.js以及對應版本NPM來安裝，以應付不同專案對於Node.js的版本需求，比如説專案A只支援版本10的Node.js，而專案B只支援版本11的Node.js，透過NVM可以隨意從任意版本切換至版本10或者11的Node.js來分別開發這兩個專案，不用移除和重新安裝Node.js。

## Node.js 版本號
版本號格式通常會是兩種的其中一種，其中x會是正整數。
```
// formal version formation
vx.x.x 

// informal version formation
x.x.x
```


## 指令
1. nvm ls-remote: 查看node.js的可安裝版本有哪些
2. nvm ls: 查看nvm已安裝的套件有哪些（可看到不同版本且都已經安裝的node.js)
3. nvm install &lt;version&gt;: 指定要安裝的版本(version)，當指令要指定版本安裝時，會先檢查nvm cache上是否有對應版本的安裝檔，有的話就直接安裝，若沒有的話，便會從平台上下載該版本的安裝檔(以壓縮檔來表示)並存在nvm的cache裡，然後解壓縮安裝其版本內容。
4. nvm use &lt;version&gt;: 切換目前要使用的版本。
5. nvm uninstall &lt;version&gt;：移除指定版本在本機所安裝的版本內容，不包含存放在cache上的安裝檔。
6. nvm cache clear：清掉 nvm 的 cache 所有目前所存的內容，包括每個版本儲存在cache上的安裝檔(以壓縮檔來表示)。



