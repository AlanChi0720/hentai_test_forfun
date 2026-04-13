# LNG 変態型人格測試量表

> 科學認定・絕對「不」準確

一個受 LNG 實況啟發的趣味人格測試網頁。回答 5 道奇葩心理測驗題，無論你選什麼，結果永遠只有一個：

**変態です / HENTAIDESU**

## 使用方式

直接用瀏覽器開啟 `index.html`，無需安裝任何依賴、無需伺服器。

```
hentai_test_forfun/
├── index.html       # 整個 app（HTML + CSS + JS 全部內嵌）
├── pic_data/
│   ├── hentaidesu.png    # 結果頁圖片
│   └── mabushi_hentai.png
└── questions.json    # 題目資料來源
```

## 新增或修改題目

1. 編輯 `questions.json`
2. 將更新後的 `questions` 陣列複製到 `index.html` 的 `const QUESTIONS = [...]`

每道題目需包含：`id`、`category`、`question`、`options`（4 個選項的陣列）。

## 靈感來源

[LNG 直播片段](https://youtu.be/Lr9mSyMI_J8?si=k0iD675DTivlCKax&t=4560)

## 製作

紐約怪人製作，致敬 LNG ｜ LNG Hentai Research Institute © 2025
