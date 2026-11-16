# My Perfect Hotel

Poki 離線版 Unity 2022 WebGL 成品。原始來源：<https://poki.com/zh/g/my-perfect-hotel>

| 項目 | 值 |
|---|---|
| 產品內部名稱 | My Hotel |
| 開發商 | SayGames |
| Unity 版本 | 2022.3.62f2 |
| 畫面尺寸 | 1080 × 1920（直向） |
| WebGL | WebGL 2.0，未開啟 threads |
| 目前版本 | 1.0.3 |

## 玩法概要

經營類休閒遊戲：扮演飯店服務生，在大廳與客房間接待顧客、打掃整理、賺取金幣並逐步升級飯店設施與新區域；關卡以星星數（HUD 0/30 類進度）評價完成度。

## 啟動方式

### Windows（本機離線包）

雙擊 `啟動 My Perfect Hotel.bat`，會在 `127.0.0.1:8784` 啟動本機伺服器並開啟遊戲。

### 手動啟動

```bash
python -m http.server 8784
# 瀏覽器開啟 http://127.0.0.1:8784/index.html
```

> 建議透過本機伺服器執行；直接 `file://` 開啟可能因 WebGL 資源讀取限制而無法載入。伺服器需提供正確 MIME（`.wasm` → `application/wasm`）。

### GitHub Pages

倉庫已啟用 GitHub Pages，來源為 `main` 分支根目錄，線上版本與根目錄 `index.html` 同步。

## 目錄結構

```
my-perfect-hotel/
├── index.html              # 入口：window.config + 離線 PokiSDK stub
├── unity-2020.js           # 載入器、載入畫面與 Poki bridge
├── 啟動 My Perfect Hotel.bat
├── Build/                  # Unity loader / framework / .data / .wasm
├── StreamingAssets/aa/     # Addressables：settings.json、catalog.json 與 116 個 bundle（WebGL/）
├── fonts/                  # Torus 字體
├── screenshots/            # 載入輪播截圖
└── README.md
```

## 離線改動說明

- **PokiSDK 本地樁**：`index.html` 內以 stub 取代官方 SDK，廣告插播直接回調、獎勵廣告直接發放成功、登入／使用者／分析／錯誤回報皆為空實作，不再請求 Poki 後端。
- **縮圖全部本地**：載入畫面預設縮圖與後備路徑皆指向 `screenshots/1-small.jpg`，不再引用 `img.poki-cdn.com`。
- **Addressables 全數本地化**：資產不在 `.data` 內嵌，而位於 `StreamingAssets/aa/`；已從 `catalog.json` 解析出全部 116 個 bundle 並下載本地（場景、客房、介面、字體、彈窗、音效資料等），`settings.json`／`catalog.json` 亦為本地副本。
- **原始載入器備份**：原始 Poki `master-loader.js`（會拉遠端 SDK）不進入倉庫，也未被頁面引用。

## 二進位檔策略

`Build/*.wasm`（約 23.6 MB）、`Build/*.data`（約 8.5 MB）與 `StreamingAssets/aa/` 下的 Addressables bundle 直接提交進 git，**未使用 Git LFS**：本專案為單機靜態離線成品，版本數有限，直接提交的成本可接受；若日後改版頻繁、倉庫體積明顯膨脹，再評估改用 LFS。

## 版本記錄

### 1.0.3（2026-11-16）

- 自 Poki 離線包匯入的原始 Unity WebGL 成品，Addressables 116 個 bundle 全數本地化。
