# 學習筆記

---

## 一、專案資料夾核心：`src` 與 `test`

當你開啟一個標準的軟體專案資料夾時，最常看到的兩個核心資料夾就是 `src` 和 `test`。這是一種**關注點分離（Separation of Concerns）**的業界標準做法。

### 1. `src` 資料夾（Source Code / 原始碼）

- **它是什麼：** 專案的「心臟」。這裡存放所有真正要運行、打包、部署到生產環境（Production）的應用程式程式碼。
- **裡面放什麼：** 核心邏輯、API 路由、資料庫模型、前端元件、演算法等。
- **舉個例子：** 如果你寫了一個會飛的程式 `fly.py`，它就應該放在 `src/` 裡面。

### 2. `test` 資料夾（Tests / 測試碼）

- **它是什麼：** 專案的「防護網」。這裡存放所有用來**驗證 `src` 程式碼是否正確運行**的測試腳本。它不會被部署到實際的線上產品中。
- **裡面放什麼：** 單元測試（Unit Tests）、整合測試（Integration Tests）、測試假資料（Fixtures）等。
- **舉個例子：** 你會在這裡寫一個 `test_fly.py`，用來檢查 `fly.py` 是不是真的能飛，或者會不會不小心摔下來。

---

## 二、Python 專案開發環境建置指南

### 建立專案資料夾

1. 點擊編輯器選單：**File → Open Folder**（開啟資料夾）。
2. 在電腦中建立一個新資料夾，命名為：`Project`。
3. 選擇該資料夾並點擊確認開啟。

### 建立專案目錄結構（`src` / `test`）

1. 開啟終端機：點擊選單 **Terminal → New Terminal**。
2. 在終端機中依序輸入指令：

```bash
mkdir src
mkdir test
```

完成後，專案目錄結構如下：

```
Project/
├─ src/
└─ test/
```

### 確認 Python 版本

```bash
python --version
```

預期顯示（版本號依實際安裝為準）：

```
Python 3.12.x
```

### 建立虛擬環境

```bash
python -m venv .venv
```

完成後，左側專案目錄中會出現一個名為 `.venv/` 的隱藏資料夾。

### PowerShell 權限設定（僅 Windows 系統首次設定需要）

> **說明：** 如果在下一步執行啟用指令時出現「因為這個系統上已停用指令碼執行」的權限錯誤，請執行此步驟。

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

當系統詢問是否確認變更時，輸入 `Y`。

### 啟動虛擬環境

```bash
.venv\Scripts\activate
```

啟動成功後，終端機提示字元的最左側會出現虛擬環境標記：

```
(.venv) PS C:\...
```

### 安裝常用開發套件

在已啟動的虛擬環境下（確保左側有 `(.venv)`），安裝測試與代碼格式化工具：

```bash
pip install pytest black
```

### 建立 Flask 基本程式碼檔案

**`src/__init__.py`**（Flask 應用程式主體）：

```python
from flask import Flask

def create_app():
    app = Flask(__name__)

    @app.route("/")
    def hello():
        return "Hello, Flask on port 1919!"

    return app
```

**`app.py`**（專案根目錄的啟動檔，注意加入 `port=1919` 設定）：

```python
from src import create_app

app = create_app()

if __name__ == "__main__":
    app.run(debug=True, port=1919)
```

### 啟動 Flask 網頁伺服器

```bash
python app.py
```

網頁成功啟動後，在瀏覽器網址列輸入：

```
http://127.0.0.1:1919
```

---

## 三、URI、URL、URN 核心概念

> **重點結論：URL 和 URN 都是 URI 的一種！**

### 🌐 核心概念大解密

#### 1. URI（Uniform Resource Identifier，統一資源識別碼）

它是所有資源識別的**總稱**。不論是用「名字」還是用「地點」來識別一個網路上的資源，它們都屬於 URI。

#### 2. URL（Uniform Resource Locator，統一資源定位符）

- **定義：** 除了識別資源，它還提供了**定位該資源的路徑**（也就是怎麼找到它）。
- **特點：** 包含協定（如 `http` 或 `https`）和伺服器位置。如果網站伺服器關機或檔案被刪除，這個 URL 就會失效（也就是俗稱的 404 錯誤）。
- **生活實例：** 桃園市中壢區大安街 X 號。
- **網路實例：** `https://www.google.com/index.html`

#### 3. URN（Uniform Resource Name，統一資源名稱）

- **定義：** 它是給資源一個**持久、獨一無二的名字**，只負責命名，不告訴你它在哪裡、也不管怎麼拿到它。
- **特點：** 就算原本存放資源的網站倒閉了，這個名字依然有效。
- **生活實例：** 你的姓名或身分證字號。
- **網路實例：** `urn:isbn:9789863479116`（出版界最常用的書籍身分證，不論這本書在誠品、博客來或圖書館，ISBN 永遠不會改變）。

### 📊 三者的包含關係

```
URI（總稱）
├─ URL（提供定位路徑）
└─ URN（提供持久名稱）
```

### 📝 經典總結對比表

| 項目 | URI | URL | URN |
|------|-----|-----|-----|
| **全名** | 統一資源識別碼 | 統一資源定位符 | 統一資源名稱 |
| **核心目的** | 識別某個資源 | 識別資源並提供**尋找路徑** | 給予資源**獨一無二的名字** |
| **是否包含協定** | 看情況 | 是（例如 `https://`） | 否（以 `urn:` 開頭） |
| **會因搬移失效嗎** | 看情況 | 會（搬家網址就變了） | 不會（名字永遠不變） |
| **範例** | 所有 URL 和 URN 都是 URI | `https://example.com/logo.png` | `urn:isbn:0451450523` |

---

## 四、把專案傳到 GitHub（Antigravity IDE）

### 在 GitHub 網頁上建立新的儲存庫

1. 打開瀏覽器，登入你的 GitHub 網站。
2. 點擊右上角的「**+**」號，選擇「**New repository**」（新增儲存庫）。
3. 在「Repository name」輸入你的專案名字（例如：`Flask_Project`）。
4. 其他設定都不要勾選（不要勾選 Add a README、.gitignore 等），直接點擊最下方的綠色按鈕「**Create repository**」。

> **Antigravity IDE：** 直接給 Antigravity IDE 建好的 GitHub 網址：
> `https://github.com/moma1899/flask-project.git`

### Antigravity IDE 自動 GitHub 部署流程

在 Antigravity IDE 點擊 GitHub Repository URL 後（`https://github.com/moma1899/flask-project`），IDE 自動完成：

- Git 初始化
- Remote 綁定
- Commit
- Push 到 GitHub

不需要手動輸入任何 git 指令。

#### 實際上 IDE 幫忙做的事情，等同於執行：

```bash
git init
git add .
git commit -m "first commit"
git remote add origin https://github.com/moma1899/flask-project.git
git branch -M main
git push -u origin main
```

---

## 五、AWS 帳號註冊與安全預算設定指南

### 註冊 AWS 帳號步驟

1. 打開瀏覽器，前往 AWS 官方網站，點擊「建立 AWS 帳號」按鈕。
2. 輸入電子郵件地址與想要設定的 AWS 帳號名稱，點擊驗證電子郵件，並輸入收到的驗證碼。
3. 設定 Root 帳號（最高權限管理員）密碼，請務必使用高強度的密碼組合。
4. 填寫聯絡人資訊，依據需求選擇「個人」或「商業」，並填寫正確的英文姓名、電話與地址。
5. 輸入信用卡或簽帳卡資訊。AWS 會扣取約 1 美元（或等值本地貨幣）來驗證卡片有效性，驗證後會退還。
6. 進行身分驗證，輸入電話號碼並選擇簡訊（SMS）或語音電話，輸入手機收到的四位數驗證碼。
7. 選擇支援方案：請勾選最左邊的「**基本支援 - 免費**」（Basic Support - Free）。
8. 點擊完成註冊，等待幾分鐘系統開通帳號後，即可進入 AWS 管理主控台（Management Console）。

### 新增 MFA（多因素驗證）安全防護步驟

> **說明：** Root 帳號權限極大，強烈建議綁定手機 APP 驗證碼，防止帳號被盜刷。

1. 請先在手機上下載驗證 APP，例如：**Google Authenticator** 或 **Microsoft Authenticator**。
2. 登入 AWS 管理主控台，點擊右上角你的帳號名稱，在下拉選單中選擇「**Security Credentials**」（安全性憑證）。
3. 在頁面中找到「**Multi-factor authentication (MFA)**」區塊，點擊「**Assign MFA device**」（指派 MFA 裝置）按鈕。
4. 輸入裝置名稱（例如：`MyPhone`），並選擇「**Authenticator app**」（驗證器應用程式），接著點擊下一步。
5. 網頁上會顯示一個 QR Code。打開手機上的驗證 APP，選擇掃描條碼並對準網頁上的 QR Code。
6. 掃描成功後，手機 APP 每 30 秒會產生一組六位數密碼。
7. 在網頁上的「**MFA code 1**」輸入當前的六位數密碼。
8. 稍等 30 秒，等手機 APP 產生下一組不同的密碼後，將其輸入到「**MFA code 2**」。
9. 點擊「**Add MFA**」按鈕，看見成功提示即代表未來登入都需要輸入手機驗證碼，安全性大幅提升。

### 建立預算（AWS Budgets）監控花費步驟

> **說明：** 設定預算上限，當超過免費額度或指定金額時，AWS 會發簡訊或 Email 通知你，避免產生驚人帳單。

1. 在 AWS 管理主控台最上方的搜尋欄輸入「**Budgets**」，點擊進入「AWS Budgets」服務。
2. 點擊右上角的「**Create budget**」（建立預算）按鈕。
3. 選擇預算類型：保持預設勾選的「**Cost budget - Recommended**」（成本預算 - 推薦），點擊下一步。
4. 設定預算詳細資訊：
   - **期間（Period）：** 選擇「Monthly」（每月）
   - **預算生效日：** 選擇「Recurring budget」（定期預算，每月重複）
   - **預算金額：** 選擇「Fixed」（固定金額）
   - **目標金額：** 輸入你想監控的金額（例如輸入 `5`，代表每月花費接近或超過 5 美元就會觸發警報）
   - **預算名稱：** 例如命名為 `My_Monthly_Budget`
5. 設定警報條件（Configure alerts）：
   - 點擊「**Add an alert threshold**」（新增警報閾值）
   - 在閾值欄位輸入 `80`（代表當實際或預測花費達到預算的 80% 時發出警告）
   - 觸發基礎（Trigger）：可選擇「Actual」（實際花費達到）或「Forecasted」（預測花費即將達到）
   - 在「**Email recipients**」欄位輸入你的 Email 地址
6. 點擊下一步，確認所有設定無誤後，點擊「**Create budget**」（建立預算）。

---

## 六、AWS IAM 帳號權限管理完整建置指南

### 專案目標架構

| 帳號 / 群組 | 角色定位 | 使用者 |
|------------|---------|-------|
| **Root Account** | 僅用於帳單、MFA 綁定與緊急救援，日常絕不登入 | — |
| **ADMIN 群組** | 綁定 AdministratorAccess 權限 | `moma1899` |
| **DEVELOP 群組** | 保持未定義管理權限，後續依需求給予特定資源權限 | `DEV_moma1899`、`DEV_xxx` |

### 第一階段：進入 IAM 主控台

1. 開啟瀏覽器進入 AWS 官方網站，登入 AWS 管理主控台。
2. 首次設定請先使用 Root 帳號（Email 與密碼）登入。
3. 在主控台最上方的搜尋列輸入 **IAM**，點擊進入「Identity and Access Management」服務。

### 第二階段：建立 IAM 使用者群組（Groups）

**建立 ADMIN 群組：**
1. 點擊左側選單的「使用者群組」，接著點擊「建立人員群組」。
2. 在群組名稱輸入：`ADMIN`。
3. 在指派權限原則清單中，搜尋並勾選：**AdministratorAccess**（最高管理員權限）。
4. 點擊最下方的「建立人員群組」按鈕完成建立。

**建立 DEVELOP 群組：**
1. 再次點擊「建立人員群組」。
2. 在群組名稱輸入：`DEVELOP`。
3. 權限原則部分**不要勾選**任何項目，保持「未定義」狀態，直接點擊「建立人員群組」。

### 第三階段：建立 ADMIN 使用者（User）

1. 點擊左側選單的「使用者」，接著點擊「建立使用者」。
2. 在使用者名稱輸入：`moma1899`。
3. 務必勾選：**提供人員對 AWS 管理主控台的存取權**。
4. 控制台密碼選擇「自動產生密碼」或「自訂密碼」，並建議勾選「下次登入必須修改密碼」。
5. 點擊下一步，在設定權限畫面中，勾選加入 **ADMIN** 群組。
6. 點擊下一步，確認資料無誤後點擊「建立使用者」。
7. ⚠️ **重要安全步驟：** 建立完成後，務必點擊「**下載 .csv 檔案**」，妥善保存裡面的登入 URL、使用者名稱與初始密碼。

### 第四階段：建立 DEVELOP 使用者（User）

1. 再次點擊「建立使用者」按鈕。
2. 在使用者名稱輸入：`DEV_moma1899`（或依團隊成員命名為 `DEV_xxx`）。
3. 同樣勾選：**提供人員對 AWS 管理主控台的存取權**，並設定好密碼原則。
4. 點擊下一步，在設定權限畫面中，勾選加入 **DEVELOP** 群組。
5. 點擊下一步，確認無誤後點擊「建立使用者」，並同樣下載保存其 `.csv` 登入憑證。

### 第五階段：帳號日常維運與登入規則

**專屬登入網址：**
```
https://您的AWS帳號ID.signin.aws.amazon.com/console
```

| 帳號類型 | 使用規則 |
|---------|---------|
| **Root Account** | 日常開發絕不使用，僅用於處理帳單、調整 MFA 及重大災難救援 |
| **ADMIN User（moma1899）** | 專門用於建置基礎建設、管理 IAM 權限、建立 EC2 / S3 / RDS 等大框架資源 |
| **DEVELOP User（DEV_moma1899）** | 日常程式碼開發、執行測試、一般雲端操作，不給予多餘的管理權限 |

### 最終架構檢查對照表

請至 IAM 使用者群組頁面確認是否符合以下配置：

| 群組名稱 | 使用者人數 | 關聯權限 |
|---------|----------|---------|
| ADMIN | 1 人 | AdministratorAccess |
| DEVELOP | 1 人以上 | 未定義 / 自訂 |

---

## 七、AWS Windows Server 建置與遠端登入完整指南

### 專案初始狀態

| 項目 | 設定值 |
|------|-------|
| 部署區域 | 亞太區域（東京）`ap-northeast-1` |
| 安全性設定 | 開通 TCP 連接埠 3389（RDP 遠端桌面協定） |
| 憑證金鑰 | RSA 格式金鑰對（`ck101.pem`） |
| 遠端帳號 | Administrator |

### 第一階段：切換東京區域與進入 EC2 主控台

1. 登入 AWS 管理主控台後，點擊右上角的區域選單。
2. 明確切換至：**Asia Pacific (Tokyo) ap-northeast-1**（亞太區域-東京）。
3. 在畫面上方的搜尋列輸入 **EC2**，點擊進入 EC2 虛擬伺服器主控台。

### 第二階段：設定並啟動 Windows 執行個體

1. 點擊左側選單的「執行個體」（Instances），接著點擊「**啟動執行個體**」（Launch instances）。
2. **設定名稱：** 在名稱欄位輸入 `windows-server-tokyo`。
3. **選擇作業系統（AMI）：** 點擊「Windows」圖標，選擇 **Microsoft Windows Server 2022 Base**。
4. **選擇規格：** 執行個體類型選擇 **t3.micro**（符合免費方案資格）。
5. **金鑰對設定（Key pair）：**
   - 若下拉選單已有 `ck101`，請直接選取。
   - 若沒有，點擊「建立新的金鑰對」，名稱輸入 `ck101`、類型選 RSA、格式選 `.pem`，建立後務必妥善存檔下載的 `ck101.pem`。
6. **網路設定與防火牆（開通 3389）：**
   - 在網路設定區塊點擊「編輯」（Edit）。
   - 新增安全群組規則：類型選擇 **RDP**（系統會自動帶入 TCP 協定與 **3389** 連接埠）。
   - 來源（Source）選擇 **Anywhere-IPv4**（會自動填入 `0.0.0.0/0`）。
7. 拉到頁面最下方，點擊右下角的「**啟動執行個體**」按鈕。

### 第三階段：獲取公用 IP 與解密密碼

1. 回到執行個體列表，等待伺服器狀態顯示為綠色的 **Running**（執行中）。
2. 點擊選取 `windows-server-tokyo`，在下方詳細資訊中找到 **Public IPv4 address**（公用 IPv4 位址），並將 IP 複製起來（例如 `54.xxx.xxx.xxx`）。
3. 勾選該台伺服器，點擊右上角的「**連線**」（Connect）按鈕。
4. 在連線頁面中，切換到 **RDP 客戶端**（RDP Client）分頁。
5. 點擊「**取得密碼**」（Get password）。
6. 點擊上傳檔案，選擇電腦中保存的 `ck101.pem` 金鑰檔案。
7. 上傳成功後，點擊「**解密密碼**」（Decrypt Password）按鈕。
8. 記錄下解密後的資訊：
   - **使用者名稱：** `Administrator`
   - **密碼：** 顯示出來的一長串專屬複雜密碼

### 第四階段：執行 Windows 遠端桌面連線（RDP）

1. 在你自己的 Windows 電腦上，按下鍵盤組合鍵 `Win + R` 喚起「執行」視窗。
2. 輸入 `mstsc` 並按下 Enter 鍵，開啟「遠端桌面連線」程式。
3. 在電腦（Computer）欄位中，貼上 EC2 公用 IPv4 位址（例如 `54.xxx.xxx.xxx`），然後點擊連線。
4. 當系統跳出認證視窗時：
   - 使用者名稱請輸入：`Administrator`
   - 密碼請貼上在 AWS 網頁上**解密出來的那串複雜密碼**
5. 出現憑證警告視窗時，點擊「是」或「確定」，即可成功進到東京區域的 Windows Server 桌面環境。

---

## 八、AWS Windows Server 佈署 Flask 專案與網路開通終極指南

### 專案初始狀態與核心設定

| 項目 | 設定值 |
|------|-------|
| 執行環境 | AWS 東京區域 Windows Server 執行個體 |
| 專案網頁連接埠 | 自訂 TCP 連接埠 **1919** |
| 安全性防護目標 | 放行 RDP（3389）與網頁流量（1919），其餘一律阻擋 |

> ⚠️ **核心程式碼檢查（`src/app.py`）：** 請確保 Flask 啟動程式碼明確指定監聽所有外部位址與 1919 連接埠：
> - `host="0.0.0.0"`
> - `port=1919`
>
> 只有 `host` 設為 `0.0.0.0`，Flask 才會把服務對外公開，否則預設只允許伺服器本機（`127.0.0.1`）連線，外部瀏覽器會被擋下。

### 第一階段：伺服器遠端登入與環境安裝

1. 使用遠端桌面連線（RDP），以 **Administrator** 身分登入 AWS Windows 伺服器。
2. **Python 環境確認與安裝：**
   - 點擊 Windows 開始功能表，開啟**命令提示字元**（cmd）。
   - 輸入 `python` 檢查：若彈出微軟商店（Microsoft Store），代表尚未安裝 Python。
   - 前往 Python 官方網站下載最新 Windows 安裝檔。
   - 執行安裝時，**務必勾選最下方的「Add python.exe to PATH」**，接著一路點擊下一步完成安裝。
   - 安裝完成後，關閉並重新開啟 cmd 視窗，讓系統重新讀取環境變數。
3. **Git 環境安裝：**
   - 在伺服器內開啟瀏覽器，前往 Git 官方網站下載 Windows 版本安裝檔（`https://git-scm.com/download/win`）。
   - 執行安裝檔，一路點擊 **Next** 直到 **Install** 完成安裝。
   - 在重新開啟的命令提示字元中輸入確認指令：

```bash
git --version
```

看見版本號回傳即代表 Git 安裝成功。

### 第二階段：複製 GitHub 專案原始碼

```bash
cd C:\
git clone https://github.com/moma1899/flask-project.git
cd C:\flask-project
dir
```

確認專案結構內是否完整包含 `src` 資料夾、`test` 資料夾與 `requirements.txt` 檔案。

### 第三階段：建置 Python 虛擬環境與套件安裝

```bash
python -m venv .venv
.\.venv\Scripts\activate
```

啟動成功後，終端機最左側會出現 `(.venv)` 提示字樣。

**安裝 Flask 與套件：**

```bash
# 優先執行批次安裝
pip install -r requirements.txt

# 若上面失敗，手動安裝 Flask
pip install flask
```

### 第四階段：啟動 Flask 網頁服務

```bash
python src\app.py
```

啟動成功後，畫面會回傳 **Running on all addresses (0.0.0.0)** 與 **Port 1919** 訊息。

> ⚠️ 此時先不要關閉此命令提示字元視窗，讓 Flask 服務在 Windows Server 背景持續運行。

### 第五階段：AWS 安全群組防火牆加開 1919 規則

> **說明：** 若未執行此階段，外部使用者將無法穿透 AWS 防火牆連入網頁。

1. 回到本機電腦的瀏覽器，登入 AWS 管理主控台，進入 **EC2** 服務。
2. 查看左側導覽選單，在「網路與安全性」區塊點擊**安全群組**（Security Groups）。
3. 找到並點擊 Windows Server 所綁定的群組（例如：`launch-wizard-1`）。
4. 點擊**傳入規則**（Inbound rules）分頁，再點擊**編輯傳入規則**（Edit inbound rules）按鈕。
5. ⚠️ **注意：** 原本已存在的 RDP (TCP 3389) 規則**請勿修改或刪除**。
6. 點擊**新增規則**（Add rule），依序填入：
   - 類型（Type）：**自訂 TCP**（Custom TCP）
   - 連接埠範圍（Port range）：`1919`
   - 來源類型（Source type）：**Anywhere-IPv4**（系統自動填入 `0.0.0.0/0`）
   - 描述（Description）：`Flask 1919`
7. 點擊右下角的**儲存規則**（Save rules）按鈕。

### 第六階段：最終安全規則檢視

儲存成功後，安全群組畫面必須同時存在以下兩條規則：

| 規則 | 協定 | 連接埠 | 來源 | 用途 |
|------|------|-------|------|------|
| RDP | TCP | 3389 | 0.0.0.0/0 | 管理端遠端桌面通道 |
| 自訂 TCP | TCP | 1919 | 0.0.0.0/0 | 使用者端網頁流量通道 |

### 第七階段：最終外部連線測試

在本機電腦的瀏覽器網址列輸入（以 `35.74.26.86` 為例）：

```
http://35.74.26.86:1919
```

頁面若能成功顯示網頁內容，即代表 Windows Server 的環境佈署、程式碼監聽與 AWS 雲端網路設定完成！

### 第八階段：功能測試路由驗證

| 功能 | 測試網址 | 預期回傳文字 |
|------|---------|------------|
| Feature1 | `http://你的伺服器公用IP:1919/feature1` | 早上要看股票 |
| Feature2 | `http://你的伺服器公用IP:1919/feature2` | 下午上班公司 |

兩個路由節點皆能精準響應對應的訊息，代表系統邏輯與整體路由架構完美開通。

---

## 九、AWS EC2 Windows Server 終止與清理完整指南

### 刪除前的重要觀念確認

| 操作 | 說明 |
|------|------|
| **終止（Terminate）** | 徹底刪除虛擬伺服器，資料、設定、原始碼全部消失，且**無法復原** |
| **停止（Stop）** | 暫時不使用，之後還想開啟時選用。停止期間 CPU 不計費，但硬碟（EBS 儲存體）依然產生微量費用 |

> 若要確保**完全不扣費**，必須選擇「終止」。

### 第一階段：在 EC2 主控台執行終止

1. 登入 AWS 管理主控台，進入 **EC2** 服務。
2. 點擊左側導覽選單的**執行個體**（Instances）。
3. 在列表中勾選你想要刪除的那台 Windows 伺服器（例如：`windows-server-tokyo`）。
4. 點擊**執行個體狀態**（Instance state）下拉選單。
5. 點擊**終止執行個體**（Terminate instance）。
6. 系統跳出警告視窗，確認無誤後，點擊視窗右下角的**終止**（Terminate）紅字按鈕。

### 第二階段：確認刪除狀態變更

狀態轉換流程：

```
Running（執行中）→ Shutting-down（正在關機）→ Terminated（已終止）
```

> **注意：** 變成 Terminated 之後，這台機器還會在畫面上殘留顯示幾個小時（AWS 的確認紀錄），這段期間它**已經完全停止計費**，可以放心。過一段時間後，它就會自動從列表中徹底消失。

### 第三階段：後續連帶資源清理（選填安全檢查）

**檢查安全群組（Security Groups）：**

之前為了專案加開了 `launch-wizard-1`（開通了 3389 與 1919 Port）。如果後續沒有要建立其他伺服器，可以到左側選單的「安全群組」中，選取該群組並點擊「刪除安全群組」，確保帳號環境的乾淨。

**金鑰對（Key Pairs）保留：**

之前建立的 `ck101` 金鑰對放在 AWS 雲端上是**完全免費**的，可以保留備用。
