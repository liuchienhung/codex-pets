# Codex Pets：我可愛的家庭

這裡提供一款以「我可愛的家庭」為主題的 Codex 寵物：[Codex Pets](https://developers.openai.com/codex/app/settings#codex-pets)。

安裝之後，你寫 Code 時就不再是孤軍奮戰。

每當你趕著交件、憑直覺改碼、略過測試就準備上線的時候，牠就會輕輕跳上桌邊，用家人般溫柔又熟悉的眼神望著你，彷彿在輕聲說：
「慢慢來沒關係的，把基礎打穩一點，未來的自己一定會謝謝現在認真對待的你。」

副作用是：
你會開始習慣多留一份註解、補上測試案例、重新梳理程式邏輯；寫累了就起身喝杯熱茶，突然想起家人等你的晚餐，心裡滿是踏實與溫暖。

![image](spritesheet.webp)

## 內容(每一個寵物資料夾都會有以下兩個檔案)

* `pet.json`
* `spritesheet.webp`

## 安裝

最簡單的安裝方式是在 Codex app 直接輸入以下提示詞：

```text
安裝 liuchienhung/codex-pets repo 的 Codex Pet 給我的 Codex app 使用
```

請在終端機執行：

```powershell
$petsDir = "$env:USERPROFILE\.codex\pets"
if (!(Test-Path $petsDir)) { New-Item -ItemType Directory -Force -Path $petsDir }

$zipUrl = "https://github.com/liuchienhung/codex-pets/archive/refs/heads/main.zip"
$zipFile  = "$petsDir\codex-pets-main.zip"
Invoke-WebRequest -Uri $zipUrl -OutFile $zipFile

# 解壓縮至暫存資料夾
$tempDir = "$petsDir\_temp_extract"
Expand-Archive -Path $zipFile -DestinationPath $tempDir -Force

# 取得 GitHub 自動產生的根資料夾（例如 codex-pets-main）
$rootFolder = Get-ChildItem -Path $tempDir | Where-Object { $_.PSIsContainer } | Select-Object -First 1

if ($null -ne $rootFolder) {
    # 遍歷根資料夾內的所有子資料夾（即各個寵物資料夾）並移至目標路徑
    Get-ChildItem -Path "$tempDir\$($rootFolder.Name)" -Directory | ForEach-Object {
        Move-Item -Path $_.FullName -Destination $petsDir -Force
    }
    
    # 清理暫存 ZIP 與解壓資料夾
    Remove-Item $zipFile -Force
    Remove-Item $tempDir -Recurse -Force
} else {
    Write-Warning "解壓失敗，請手動將寵物資料夾複製至 $petsDir"
}
```

執行後，Codex 會在 `%USERPROFILE%\.codex\pets\<寵物名稱>` 讀取這個寵物。

## 使用

1. 下載並解壓縮寵物檔案。
2. 確認 `%USERPROFILE%\.codex\pets\<寵物名稱>` 內有 `pet.json` 與 `spritesheet.webp`。
3. 重新啟動 Codex 並到 **設定** > **外觀** > **寵物** 選取 **Penny**。
4. 到聊天視窗輸入 `/pet` 或輸入 `/` 選取「寵物」即可啟用。
5. 然後在聊天視窗輸入任何指令，這個 Codex Pet 就會動起來了！

## 寵物資料夾檔案說明

* `pet.json`：寵物的中繼資料。
* `spritesheet.webp`：寵物的圖像素材。
