# TypeScript 基本概念
## 什麼是 TypeScript？
TypeScript是由微軟開發的一種能用來編譯出 JavaScript 的程式語言，比JavaScript程式語言還多了一道編譯手續。
## 特性
1. 能夠在編譯階段找出程式碼語法上的問題
2. 添加了型別檢查機制，讓程式更容易閱讀與偵錯，使得TypeScript更適合用來開發大型專案。

## 設置環境
### 下載 TypeScript
```
npm i -g typescript ts-node
```
`ts-node` 為一個小型的 command line tool，讓我們可以在終端機做**編譯及執行 TypeScript**
### 指令提示
```
tsc --help
```
## 編譯 TypeScript
`XXX.ts` 為 TypeScript 檔案
```
tsc XXX.ts
```
上述動作只有進行編譯，會看到相同路徑下有編譯好的同名`.js`檔，要再執行`node XXX.js`。
## 編譯且執行 TypeScript
需透過 `ts-node` 這個小型的 command line tool
```
ts-node tsc XXX.ts
```

