# 學習筆記

---

## 一、Windows + Antigravity + GitHub 完整流程

### 📌 第一階段：初始專案建置與設定

#### 1. GitHub 建立 Repository（遠端倉庫）

- **操作步驟：** 到 GitHub 網站點選建立一個新的 repo，命名為：`aws_0528`。
- ⚠️ **重要注意：** **不要勾選**「Add a README file」，建立一個完全空白的專案。
- **取得網址：** 複製 HTTPS 按鈕，取得網址：
  ```
  https://github.com/moma1899/aws_0528.git
  ```

#### 2. Antigravity Clone 專案到本地端

**操作步驟：**

1. 打開 Antigravity 點選：**Clone Repository**
2. 貼上網址：`https://github.com/moma1899/aws_0528.git`
3. 選擇儲存的資料夾位置：**Desktop**
4. 點擊：**Select as Repository Destination**

**結果：** 桌面會成功出現資料夾：`Desktop/aws_0528`

> 💡 **提示：** 因為是完全空白的專案，此時軟體或終端機可能會出現 `warning: You appear to have cloned an empty repository` 的警告，這是完全正常的現象，不用擔心。

#### 3. 開啟 Antigravity Terminal

- **操作步驟：** 點選上方選單：**Terminal ➔ New Terminal**
- **路徑確認：** 請確認畫面左側或上方的路徑位置在：`C:\Users\TMP-214\Desktop\aws_0528`
- **測試輸入指令：**

```bash
git status
```

> 💡 **提示：** 此時畫面上會顯示 `No commits yet`（尚未有任何提交紀錄），這代表目前本地端是一個全新的起點。

#### 4. 建立 dev 分支

```bash
git checkout -b dev
```

**結果：** 執行後會成功建立並切換到本地的 `dev` 分支。

> ⚠️ **特別注意：** 因為目前專案內沒有任何檔案，也沒有任何 commit 紀錄，如果你這時候直接打 `git push --set-upstream origin dev`，Git 會因為抓不到內容而報錯。因此，必須先執行「步驟 5」與「步驟 6」，等有了第一個 commit 之後，再來執行推送到 GitHub 的動作。

#### 5. 修改與建立檔案

- **操作步驟：** 在本地端資料夾內，新增或修改檔案，例如建立一個名為 `README.md` 的檔案。
- **存檔檢查：** 存檔後，在 Terminal 輸入指令檢查：

```bash
git status
```

**結果：** 此時會看到 `README.md` 顯示為紅字（Untracked files），代表 Git 順利偵測到新檔案了。

#### 6. Commit 修改（提交記錄）

把檔案加入暫存區並提交：

```bash
git add .
git commit -m "change readme"
```

**結果：** 完成後，你的本地 `dev` 分支就擁有了整個專案歷史上的「第一個 commit 節點」。

#### 7. Push 推送到 GitHub

因為這是此空白專案第一次要把東西推上雲端，所以要輸入完整的綁定指令：

```bash
git push --set-upstream origin dev
```

**結果：** 這行指令會把你的第一個 commit 連同 `dev` 分支一起推送到 GitHub 上。

> 💡 **提示：** 之後如果再修改檔案，只要輸入簡單的指令即可：
> ```bash
> git push
> ```

#### 8. GitHub 發起 PR（Pull Request）與 Merge

1. **觸發 PR：** 打開 GitHub 網頁，因為系統偵測到剛推上去的 `dev` 分支，畫面上會出現黃色提示條與綠色按鈕：**Compare & pull request**，請點擊它。

2. **確認分支方向：** 請務必檢查方向是否為：

   ```
   base: main  ←  compare: dev
   ```

   > 💡 **特別說明：** 因為一開始沒建 README，GitHub 雲端本來是沒有 `main` 分支的。此時將 base 選為 `main`，GitHub 會在你通過 PR 時自動幫你建立 `main`。

3. **確認後依序按下：**
   - **Create pull request**
   - **Merge pull request**
   - **Confirm merge**

**結果：** `dev` 的內容就成功合併進 `main`，雲端此時也正式擁有了穩定的 `main` 分支。

---

### 🔄 第二階段：之後每天固定維護流程

當專案初始化完成後，往後的日常開發請固定走這套精簡流程：

```bash
# 1. 確保切換到 dev 分支
git checkout dev

# 2. 開工前先拉取雲端最新進度
git pull

# 3. 開始修改或新增檔案
# （進行程式碼編寫、修改完畢後記得存檔）

# 4. 檢查狀態、暫存、提交、推送
git status
git add .
git commit -m "你的修改內容"
git push
```

需要合併到主分支時，隨時去 GitHub 網頁開 PR 合併到 `main`。

> 💡 **記憶口訣：**
>
> **Antigravity clone ➔ dev 修改 ➔ commit ➔ push ➔ GitHub PR ➔ merge main**

---

## 二、Docker 配置運行指南

### 第一階段：建立 Dockerfile 核心設定檔

- **建立路徑：** 在 `flask-project` 專案最頂層根目錄下（與 `src` 資料夾、`requirements.txt` 完全同層級）建立一個新檔案。
- **檔案命名：** 必須精準命名為 `Dockerfile`（注意首字母大寫，且絕對不能帶有任何副檔名，不可寫成 `Dockerfile.txt`）。
- **填入內容：** 將以下 7 行設定完整複製並貼入，完成後儲存（`Ctrl + S`）：

```dockerfile
FROM python:3.11
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
EXPOSE 1919
CMD ["python", "src/app.py"]
```

### 第二階段：建立忽略檔案（`.dockerignore`）

> **說明：** 為了避免把本地端的虛擬環境資料夾或 Git 紀錄一起打包進映像檔，導致檔案過大，必須手動排除它們。

- **建立路徑：** 在同一個專案根目錄下（與 `Dockerfile` 同一層級），新增第二個檔案。
- **檔案命名：** 精準命名為 `.dockerignore`
- **填入內容：**

```
.venv
__pycache__
.git
```

### 第三階段：檢查並修正原始碼對外監聽設定（`app.py`）

- **檢查方式：** 開啟專案中的 `src/app.py` 檔案，向下拉到最底部的啟動程式碼。
- **核心規範：** 請確保啟動參數有明確指定為所有外部網路位址以及自訂的 1919 連接埠。
- **正確的啟動程式碼形式：**

```python
app.run(host="0.0.0.0", port=1919)
```

### 第四階段：本機安裝與啟動 Docker Desktop

如果 Windows 電腦上還沒有 Docker 環境，請開啟瀏覽器前往 Docker 官方網站下載安裝檔：

```
https://www.docker.com/products/docker-desktop/
```

### 最終終端機指令執行

打開終端機（cmd 或 PowerShell），確保工作路徑位於專案根目錄下，依序執行以下兩行指令：

```bash
docker build -t flask-app .
docker run -d -p 1919:1919 --name my-flask-container flask-app
```

**功能驗證：** 執行完畢後打開瀏覽器，輸入 `http://127.0.0.1:1919/feature1` 即可看到網頁產出！

---

## 三、Docker 指令執行與 Docker Desktop

### 步驟一：執行「打包映像檔」指令

```bash
docker build -t flask-app .
```

### 步驟二：執行「運行容器」指令

當上面的打包進度完全停止、沒有出現紅字，且重新跳出路徑提示後，接著執行：

```bash
docker run -d -p 1919:1919 --name my-flask-container flask-app
```

> **成功指標：** 按下 Enter 後，畫面如果跑出一長串由英文與數字組成的黑色雜湊值代碼，且沒有任何錯誤訊息，就代表 Docker 容器已經在背景啟動了！

### 步驟三：透過 Docker Desktop 介面直接開啟網址驗證

1. 將 **Docker Desktop** 軟體視窗拉到前台。
2. 點擊左側選單的 **Containers**，並在列表中點進 **my-flask-container** 詳情畫面。
3. 看向畫面中上方、在容器名稱 `my-flask-container` 的正下方，會看到一串藍色、帶有外部連結小圖示（↗）的網址連結：**1919:1919**
4. 用滑鼠點擊以下連結即可開啟：
   - `http://127.0.0.1:1919`
   - `http://172.17.0.2:1919`

---

## 四、Git 分支建立與 GitHub 推送

### 第一階段：本地端 Git 分支建立與切換

**建立並切換至新分支：**

```bash
git checkout -b docker
```

> **成功指標：** 畫面上會看見系統回傳 `Switched to a new branch 'docker'`。

**檢查目前所在分支：**

```bash
git branch
```

> **成功指標：** 畫面上會列出所有分支，你會看見 `main` 以及 `* docker`。其中帶有星號（`*`）的分支就代表你目前正在操作的環境。

### 第二階段：暫存與提交程式碼變更（Commit）

```bash
git add .
git commit -m "Add Docker deployment for Flask app"
```

### 第三階段：將 Docker 分支推送至 GitHub（Push）

```bash
git push -u origin docker
```

> **成功指標：** 上傳進度跑完後，畫面上會明確出現 `[new branch] docker -> docker` 以及提示你可以建立合併請求的 `Create a pull request for 'docker'` 字樣，這就代表雲端對接成功！

### 第四階段：GitHub 網頁端驗證與檢視

1. **分支切換檢查：**
   - 打開瀏覽器，進入你的 GitHub 專案首頁，並**重新整理頁面**。
   - 查看頁面左上角的分支（Branch）下拉選單，點擊後列表裡會全新出現 **docker** 的分支選項。

2. **檔案完整性確認：**
   - 點擊切換到 **docker** 分支。
   - 確認 `Dockerfile`、`docker-compose.yml`、`.dockerignore` 以及 `requirements.txt` 都有安穩地存放在雲端倉庫中。

3. **合併請求提示彈出：**
   - 切換完成後，GitHub 網頁上方會自動彈出一個醒目的提示橫條，右側帶有綠色按鈕 **Compare & pull request**，代表 GitHub 已偵測到你的進度，方便你隨時發起程式碼合併審查。

---

## 五、Local Docker Image 推送至 Docker Hub

### 專案初始狀態與目標

| 項目 | 設定值 |
|------|-------|
| 本地映像檔名稱 | `flask-project` |
| Docker Hub 使用者帳號 | `moma1899` |
| 專案網頁連接埠 | `1919` |
| 終極目標 | 將本地環境封裝，推送到雲端 Docker Hub 倉庫，達成任何機器都能一鍵下載並運行的目標 |

### 第一階段：本地端映像檔建置與檢視

**執行映像檔打包指令：**

```bash
docker build -t flask-project .
```

> ⚠️ **注意：** 指令最尾端的空白鍵與小點（`.`）千萬不能漏掉。

**查看本地端映像檔列表：**

```bash
docker images
```

> **預期回傳：** 列表中必須明確看見 REPOSITORY 名稱為 `flask-project`、TAG 為 `latest` 的項目。

### 第二階段：登入 Docker Hub 雲端倉庫

如果還沒有帳號，請先前往 Docker Hub 官方網站（`https://hub.docker.com/`）完成帳號註冊（此處以帳號 `moma1899` 為範例）。

**終端機連線登入：**

```bash
docker login
```

依據畫面提示，依序手動輸入你的 Docker Hub **Username**（使用者名稱）與 **Password**（密碼/存取權杖）。

> **成功指標：** 當終端機最後回傳 `Login Succeeded` 字樣，代表本機與雲端倉庫已順利完成開通認證。

### 第三階段：重新標記映像檔標籤（Tag）

> **說明：** Docker Hub 規定，要上傳的映像檔名稱前方必須帶有「你的 Docker Hub 帳號名稱」，否則伺服器會不知道要把這個檔案歸類給誰。

**執行重新標記指令：**

```bash
docker tag flask-project moma1899/flask-project:latest
```

**再次確認標籤狀態：**

```bash
docker images
```

> **預期回傳：** 此時列表中會同時出現 `flask-project` 以及 `moma1899/flask-project` 兩個項目，兩者的 Image ID 會完全相同，這代表標籤設定成功。

### 第四階段：正式推送映像檔到雲端（Push）

```bash
docker push moma1899/flask-project:latest
```

> **上傳進度觀察：** 畫面上會開始跑動 `digest: sha256` 等多行黑色雜湊值進度條。當進度條全部跑完並重新跳出路徑提示時，即代表上傳成功。

**網頁端成果驗證：**

開啟瀏覽器前往你的專案專屬雲端網址：

```
https://hub.docker.com/r/moma1899/flask-project
```

### 第五階段：任何機器皆可直接下載與異地運行（Pull & Run）

**遠端機器拉取映像檔：**

不論是在自己的電腦、朋友的電腦，還是全新開闢的 AWS 雲端伺服器上，只要該機器有安裝 Docker，直接輸入此指令即可將環境下載下來：

```bash
docker pull moma1899/flask-project:latest
```

**一鍵啟動網頁容器服務：**

```bash
docker run -p 1919:1919 moma1899/flask-project
```

> **成果：** 不需要裝 Python、不需要裝 Git、更不需要裝任何套件，專案直接完美運行！

### 最終章：完整 Docker 生命週期與架構對照

```
Flask App 業務邏輯（核心原始碼）
        │
        │  透過設定檔規範環境
        ▼
    Dockerfile（環境設定檔）
        │
        │  在本地端執行建置
        ▼
  docker build（命令執行）
        │
        │  產出本機靜態檔案
        ▼
Docker Image 映像檔（本地產物）
        │
        │  執行雲端推送
        ▼
  docker push（命令執行）
        │
        │  安全存儲於中央倉庫
        ▼
Docker Hub 公用倉庫（雲端中心）
        │
        │  任何乾淨的伺服器皆能下載
        ▼
  docker pull + docker run（異地部署）
```

---

## 六、AWS Linux 部署、MobaXterm 連線與 Docker Pull

### 第一階段：建立 Ubuntu Linux 執行個體

1. **登入 AWS 管理主控台：** 開啟瀏覽器前往 `https://console.aws.amazon.com/` 完成登入。
2. **啟動執行個體：** 進入 EC2 服務主頁，點擊**啟動執行個體**（Launch Instance）按鈕。
3. **設定主機名稱（Name）：** 手動輸入識別名稱：`ubuntu-docker`
4. **選取作業系統（AMI）：** 選擇 **Ubuntu Server 24.04 LTS**（長期支援穩定版）。
5. **選取規格（Instance Type）：** 選取免費額度適用的型號：**t3.micro**
6. **建立與下載遠端驗證金鑰對（Key Pair）：**
   - 點擊右側的**建立新的金鑰對**連結。
   - 金鑰對名稱輸入：`ck101-linux`
   - 金鑰檔案格式務必選取：**`.pem`**（適用於 OpenSSH 與 MobaXterm）。
   - 點擊建立後，瀏覽器會自動下載 `ck101-linux.pem` 檔案，請妥善保存，絕對不可外流或刪除。
7. **網路防火牆與安全群組配置（Security Groups）：** 找到網路設定區塊，勾選**建立安全群組**。
   - **第一條規則（管理通道）：** 系統自動帶出 **SSH (TCP 22)** 規則，來源類型保持為 `0.0.0.0/0`。
   - **第二條規則（網頁流量通道）：** 點擊**新增安全群組規則**，手動新增：
     - 類型：**自訂 TCP**（Custom TCP）
     - 連接埠範圍：`1919`
     - 來源類型：**Anywhere-IPv4**（系統自動填入 `0.0.0.0/0`）
8. 確認所有參數無誤後，點擊右下角的**啟動執行個體**按鈕。

### 第二階段：取得公用網路 IP 位址

1. **進入執行個體列表：** 點擊 EC2 左側選單的「執行個體」。
2. **複製網路位址：** 點選剛剛建立好的 `ubuntu-docker` 主機，在下方詳細資訊欄位中，找到並複製其 **公用 IPv4 位址**（例如：`43.212.22.87`）。

### 第三階段：使用 MobaXterm 進行遠端 SSH 連線

1. **開啟工具：** 在你的本地電腦上啟動 **MobaXterm** 軟體。
2. **開啟新連線會話：** 點擊軟體左上角的 **Session** 按鈕，在彈出的視窗中點選第一個 **SSH** 圖標。
3. **設定遠端伺服器與帳號：**
   - 在 **Remote host** 欄位中，貼上剛才複製的 AWS 公用 IPv4 位址（如：`43.212.22.87`）。
   - 勾選 **Specify username** 複選框，並在右側欄位輸入 Ubuntu 系統的預設帳號：`ubuntu`
4. **載入安全金鑰檔案：**
   - 點擊下方的 **Advanced SSH settings** 分頁標籤。
   - 勾選 **Use private key** 複選框。
   - 點擊右側的資料夾圖標，瀏覽並選取第一階段下載的 `ck101-linux.pem` 檔案。
5. **確認連線：** 所有設定填妥後，點擊視窗最下方的 **OK** 按鈕。

> **成功登入指標：** 連線成功後，黑色的終端機畫面會跳出綠色與白色的文字提示，最下方會顯示主機路徑：`ubuntu@ip-172-31-xx-xx:~$`。如果畫面停留在 `/home/mobaxterm`，代表連線失敗，請重新確認 IP 與金鑰路徑。

### 第四階段：在 Linux 環境中安裝並啟動 Docker

在 MobaXterm 成功連線後的黑色終端機中，依序執行以下指令：

**更新系統套件清單：**

```bash
sudo apt update
```

**一鍵安裝 Docker 引擎（`-y` 參數代表自動同意所有確認提示）：**

```bash
sudo apt install docker.io -y
```

**啟動服務並設定開機自動喚醒：**

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

**驗證安裝結果：**

```bash
docker --version
```

### 第五階段：從 Docker Hub 拉取映像檔（Pull Image）

**拉取雲端倉庫映像檔：**

```bash
sudo docker pull moma1899/flask-project:latest
```

> **進度觀察：** 終端機會開始跑動多行 `Extracting` 與 `Pull complete` 的進度條，請耐心等待它跑完並回到路徑提示符。

**檢查本地映像檔列表：**

```bash
sudo docker images
```

---

## 七、主管審查與 Git 分支安全收尾指令操作

### 第一階段：提交本地變更並推送到 GitHub

如果在本地對 Dockerfile 還有做最後的微調，請先在 `feat-docker` 分支下將它們存檔並提交紀錄：

```bash
git add .
git commit -m "完成 Docker 容器化配置與環境測試"
```

**將最新進度推送到 GitHub 雲端：**

```bash
git push -u origin feat-docker
```

### 第二階段：建立 Pull Request（PR）並回報主管審查

1. **在 GitHub 網頁端發起合併請求：**
   - 打開瀏覽器進入你的 GitHub 專案倉庫（`my-flask-dashboard`）。
   - 點擊網頁畫面上方跳出的黃色提示方塊中的 **Compare & pull request** 按鈕。
   - 確認比對方向為 `base: main ◄─ compare: feat-docker`，確認無誤後點擊 **Create pull request**。

2. **向主管發送審查通知：** 複製此時瀏覽器網址列的 PR 連結，連同你的測試成果（例如 AWS 公用 IP 網址與 Docker Desktop 順利運行的日誌紀錄）一起傳給主管。

   > **訊息公版：**
   > 「報告主管，Flask 專案的 Docker 配置已在遠端環境測試完畢。已發起 Pull Request，再請主管抽空進行 Code Review 並執行 Merge，謝謝！」

### 第三階段：主管審查通過，網頁端執行合併

主管點進你提供的 PR 連結，確認程式碼改動無誤後，拉到網頁最下方點擊綠色的 **Merge pull request** 按鈕，接著點擊 **Confirm merge**。

### 第四階段：本地與遠端分支安全清理指令

> **說明：** 當雲端合併完成後，請立刻依序在你的電腦終端機（PowerShell）輸入以下指令，徹底刪除舊分支，避免未來團隊成員不小心拉取到過期環境。

**步驟一：安全切換回主分支**

```bash
git checkout main
```

> **系統回傳指標：** 畫面上會看見 `Switched to branch 'main'`。

**步驟二：同步下載雲端合併後的最新主程式**

```bash
git pull origin main
```

> **系統回傳指標：** 畫面上會出現 Fast-forward 進度條，並明確顯示 `Dockerfile` 與 `.dockerignore` 檔案已被成功同步下載至本地電腦。

**步驟三：安全刪除本地端的舊功能分支**

```bash
git branch -d feat-docker
```

> **系統回傳指標：** 畫面上會回傳 `Deleted branch feat-docker`，代表本地端的殘留環境已清理完畢。

**步驟四：徹底抹除 GitHub 雲端上的舊功能分支（關鍵防護）**

```bash
git push origin --delete feat-docker
```

> **系統回傳指標：** 終端機會向雲端伺服器發送刪除訊號，最後回傳 `- [deleted] feat-docker`，這能確保未來任何人執行 pull 都絕對不會誤載這個舊功能分支。

**步驟五：最終狀態雙重檢查**

```bash
git branch -a
```

> **系統回傳指標：** 畫面上只會剩下紅色的遠端主頻道 `remotes/origin/main` 以及帶有綠色星號的本地 `* main`，完全找不到任何 `feat-docker` 的殘留痕跡，整個專案發布與收尾宣告完美成功！

---

## 八、總整理

本次 Day 2 涵蓋三大核心主題：

1. **環境容器化** — 使用 Docker 將 Flask 專案打包成可攜式映像檔，並推送至 Docker Hub 雲端倉庫。
2. **AWS Linux 雲端部署** — 在 AWS EC2 Ubuntu 執行個體上，透過 MobaXterm 遠端連線，安裝 Docker 並從 Docker Hub 一鍵拉取部署。
3. **Git 分支管理** — 建立功能分支（`feat-docker`）、發起 PR 審查、合併後安全清理本地與遠端舊分支的完整工作流程。
