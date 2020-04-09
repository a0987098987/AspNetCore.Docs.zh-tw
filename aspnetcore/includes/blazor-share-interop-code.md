## <a name="share-interop-code-in-a-class-library"></a>在類別庫中共用互通程式碼

JS 互通代碼可以包含在類庫中,這允許您在 NuGet 包中共用代碼。

類庫處理在生成的程式集中嵌入 JavaScript 資源。 JavaScript 檔案放置在*wwwroot*資料夾中。 工具負責在構建庫時嵌入資源。

構建的 NuGet 套件在應用的專案檔中引用的方式與引用任何 NuGet 套件的方式相同。 還原包後,應用代碼可以調用到 JAVAScript 中,就像它是 C#一樣。

如需詳細資訊，請參閱 <xref:blazor/class-libraries>。
