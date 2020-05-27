## <a name="share-interop-code-in-a-class-library"></a>共用類別庫中的 interop 程式碼

JS interop 程式碼可以包含在類別庫中，這可讓您在 NuGet 套件中共用程式碼。

類別庫會處理在建立的元件中內嵌 JavaScript 資源。 JavaScript 檔案會放在*wwwroot*資料夾中。 工具會在建立程式庫時，負責內嵌資源。

在應用程式的專案檔中參考的組建 NuGet 套件，與參考任何 NuGet 套件的方式相同。 還原套件之後，應用程式程式碼可以呼叫 JavaScript，就好像它是 c # 一樣。

如需詳細資訊，請參閱<xref:blazor/class-libraries>。
