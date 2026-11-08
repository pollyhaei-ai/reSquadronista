# Squadronista（台服 / Dalamud API 13）

這是 [VeraNala/Squadronista](https://github.com/VeraNala/Squadronista) 的分支，把組建目標改成**台灣版用戶端**用的 [yanmucorp/Dalamud](https://github.com/yanmucorp/Dalamud) **API 13 / .NET 9**，給 [cycleapple/XIVTCLauncher](https://github.com/cycleapple/XIVTCLauncher) 手動安裝。

它會在部隊遠征視窗旁試著排出成員與訓練。求解與介面讀取仍是上游寫法，**沒有為台服重調 UI 位址**。

## 下載

- 發行頁：<https://github.com/pollyhaei-ai/reSquadronista/releases/tag/api13-tw-1.0.0.4>
- 壓縮檔：<https://github.com/pollyhaei-ai/reSquadronista/releases/download/api13-tw-1.0.0.4/Squadronista-api13-tw-1.0.0.4.zip>

這是預覽版（prerelease），組建自 yanmucorp Dalamud 13.0.0.16。壓縮檔裡是 `Squadronista\` 資料夾，含 `Squadronista.dll`、`Squadronista.json`，以及一起載入的 `ECommons.dll`、`LLib.dll`、`HexaGen.Runtime.dll`。

## 安裝（XIVTCLauncher）

1. 在 XIVTCLauncher 啟用 Dalamud，進過一次遊戲，讓設定資料夾建立起來，然後**關閉遊戲**。
2. 把 zip 解壓到你登入時使用的那個 dev plugins 資料夾：
   - 共用設定：`%AppData%\FFXIVSimpleLauncher\Dalamud\Config\devPlugins\`
   - 帳號自己的插件設定檔：`%AppData%\FFXIVSimpleLauncher\Profiles\<設定檔 ID>\Dalamud\devPlugins\`
3. 確認檔案在 `devPlugins\Squadronista\` 裡，`Squadronista.dll` 和 `Squadronista.json` 同一層。
4. 進遊戲後啟用 Squadronista。聊天輸入 `/xldev` 可以打開同一個開發插件資料夾，也可以在那裡重新載入。
5. `/squad` 或 `/squadronista` 開啟視窗。

## 使用前請先看

- **不要裝到官方 Dalamud API 14**（國際服 XIVLauncher）。這個組建只給台服 yanmucorp API 13。
- 部隊任務畫面上的文字與數值**還沒在台服核對**。請先對一下任務名稱和能力值，再決定要不要照建議編隊。
- 開發插件檢查若出現 **OpenMainUi** 相關警告，可以忽略。這個版本是把主視窗掛在 `OpenConfigUi` 上，沒有另外實作 `OpenMainUi`，不代表載入失敗。用上面的斜線指令開啟即可。
- 附帶的 `LLib.dll`（6.4.0.0）是針對較舊的 API 13 組建的（Dalamud 13.0.0.2、`FFXIVClientStructs` 0.0.6402.0）。yanmucorp 13.0.0.16 內建的 ClientStructs 是 0.0.6966.0。能編譯、能載入，不表示 LLib 讀到的介面結構一定和目前台服一致。

## 來源

基於 [VeraNala/Squadronista](https://github.com/VeraNala/Squadronista)，作者 Liza。這個分支只改 Dalamud API / .NET 目標與說明，沒有改任務求解或介面位址。

上游沒有 `LICENSE` 檔。本倉庫**不另外添加或宣稱任何授權**。使用與再散佈請自行對照上游作者的權利。

## 自行編譯

`Dalamud.NET.Sdk` 13.1.0 不會附帶 Dalamud，而是對本機的 `Dalamud.dll` 編譯。沒有設定 `DALAMUD_HOME` 時，SDK 會去官方啟動器的 `%AppData%\XIVLauncher\addon\Hooks\dev\`。XIVTCLauncher 的檔案在 `%AppData%\FFXIVSimpleLauncher\Dalamud\`，含有 `Dalamud.Injector.exe` 與 `Dalamud.dll` 的是 `Injector`。

1. 安裝 .NET 9 SDK。`global.json` 指定 `9.0.100`，並在 9.0 功能帶內向前相容（包含對應 Dalamud .NET 9.0.3 的 9.0.3xx SDK）。
2. 把 `DALAMUD_HOME` 指到 yanmucorp Dalamud `13.0.0.16` 裡含有 `Dalamud.dll` 的目錄（本機編譯、[發行壓縮檔](https://github.com/yanmucorp/Dalamud/releases)，或啟動器已下載的 `Injector`）。
3. 在這個目錄執行 `dotnet build -c Release`。

在 Linux 或 macOS 上另外加上 `-p:EnableWindowsTargeting=true`。專案目標是 `net9.0-windows7.0`。

`lib/LLib.dll` 是倉庫裡既有的 .NET 9 二進位檔，版本如上一節所述。這個分支沒有重編它。若以後編譯抱怨 LLib 的型別對不上，請換成同一份 yanmucorp Dalamud 所建的 LLib，不要只靠這次能編過就當成台服 UI 已修好。

## English

Fork of [VeraNala/Squadronista](https://github.com/VeraNala/Squadronista) by Liza, retargeted to yanmucorp Dalamud API 13 / .NET 9 for the Traditional Chinese client and XIVTCLauncher. Download the prerelease zip from the [1.0.0.4 release](https://github.com/pollyhaei-ai/reSquadronista/releases/tag/api13-tw-1.0.0.4) and extract `Squadronista\` into `%AppData%\FFXIVSimpleLauncher\Dalamud\Config\devPlugins\` (or `Profiles\<id>\Dalamud\devPlugins\` when the account uses its own profile). Enable it in `/xldev`, then open it with `/squad`. Do not install this on official Dalamud API 14. UI offsets are still the upstream ones. Upstream ships no LICENSE, and this fork does not add one.
