# LNG 変態型人格測試量表

> 科學認定・絕對「不」準確

一個受 LNG 實況啟發的趣味人格測試網頁。回答 5 道奇葩心理測驗題，無論你選什麼，結果永遠只有一個：

**変態です / HENTAIDESU**

## 線上遊玩

**[https://alanchi0720.github.io/hentai_test_forfun/](https://alanchi0720.github.io/hentai_test_forfun/)**

## 使用方式

直接用瀏覽器開啟 `index.html`，無需安裝任何依賴、無需伺服器。

```
hentai/
├── index.html            # 整個 app（HTML + CSS + JS 全部內嵌）
├── questions.json        # 題目資料來源
├── comments.json         # 結果頁隨機評語來源
├── dimensions.json       # 假 MBTI 維度條來源
├── pic_data/
│   ├── hentaidesu.png        # 結果頁圖片
│   └── mabushi_hentai.png    # 備用圖片素材
└── test_app.py           # Playwright 自動化測試腳本
```

## 修改內容

JSON 檔案是唯一的編輯入口，修改後需手動同步到 `index.html` 對應的 `const`：

| JSON 檔案 | 對應常數 | 說明 |
|---|---|---|
| `questions.json` | `QUESTIONS` | 題目（需含 `id`、`category`、`question`、`options` 四欄） |
| `comments.json` | `COMMENTS` | 結果頁評語，每次隨機抽 2 條 |
| `dimensions.json` | `DIMENSIONS` | 假 MBTI 維度條，需含 `label` 與 `gradient` |

## 測試

```
"C:/Users/alan8/AppData/Local/Programs/Python/Python311/python.exe" test_app.py
```

截圖存至 `screenshots/`。

## 靈感來源

[LNG 直播片段](https://youtu.be/Lr9mSyMI_J8?si=k0iD675DTivlCKax&t=4560)

## 製作

紐約怪人製作，致敬 LNG ｜ LNG Hentai Research Institute © 2025
