## <a name="troubleshoot"></a>疑難排解

### <a name="cookies-and-site-data"></a>Cookie 和網站資料

Cookie 和網站資料可以跨應用程式更新保存，並干擾測試和疑難排解。 進行應用程式程式碼變更、使用者帳戶與提供者的變更，或提供者應用程式設定變更時，請清除下列各項：

* 使用者登入 cookie
* 應用程式 cookie
* 快取和儲存的網站資料

防止延遲 cookie 和網站資料干擾測試和疑難排解的一種方法是：

* 設定瀏覽器
  * 使用瀏覽器進行測試，您可以設定在每次瀏覽器關閉時刪除所有 cookie 和網站資料。
  * 請確定瀏覽器已手動關閉，或由 IDE 在應用程式、測試使用者或提供者設定之間進行任何變更。
* 使用自訂命令，以 Visual Studio 中的 incognito 或私用模式開啟瀏覽器：
  * 從 Visual Studio 的 [**執行**] 按鈕開啟 **[流覽方式**] 對話方塊。
  * 選取 [新增] 按鈕。
  * 在 [**程式**] 欄位中提供瀏覽器的路徑。 下列可執行檔路徑是 Windows 10 的一般安裝位置。 如果您的瀏覽器安裝在不同的位置，或您未使用 Windows 10，請提供瀏覽器可執行檔的路徑。
    * Microsoft Edge：`C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe`
    * Google Chrome：`C:\Program Files (x86)\Google\Chrome\Application\chrome.exe`
    * Mozilla Firefox：`C:\Program Files\Mozilla Firefox\firefox.exe`
  * 在 [**引數**] 欄位中，提供瀏覽器用來以 incognito 或私用模式開啟的命令列選項。 有些瀏覽器需要應用程式的 URL。
    * Microsoft Edge：`-inprivate`
    * Google Chrome：`--incognito --new-window https://localhost:5001`
    * Mozilla Firefox：`-private -url https://localhost:5001`
  * 在 [**易記名稱**] 欄位中提供名稱。 例如： `Firefox Auth Testing` 。
  * 選取 [**確定]** 按鈕。
  * 若要避免針對使用應用程式測試的每個反復專案選取瀏覽器設定檔，請將設定檔設定為預設值，並將**設定為預設**按鈕。
  * 請確定在應用程式、測試使用者或提供者設定的任何變更之間，IDE 已關閉瀏覽器。

### <a name="run-the-server-app"></a>執行伺服器應用程式

測試和疑難排解託管的 Blazor 應用程式時，請確定您是從**伺服器**專案執行應用程式。 例如，在 Visual Studio 中，在您使用下列任何方法啟動應用程式之前，請確認已在**方案總管**中將伺服器專案反白顯示：

* 選取 [執行] 按鈕。
* 使用**Debug**  >  功能表中的 [**開始調試**]。
* 按 <kbd>F5</kbd>。

### <a name="inspect-the-content-of-a-json-web-token-jwt"></a>檢查 JSON Web 權杖（JWT）的內容

若要解碼 JSON Web Token （JWT），請使用 Microsoft 的[jwt.ms](https://jwt.ms/)工具。 UI 中的值永遠不會離開您的瀏覽器。
