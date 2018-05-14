# <a name="custom-webhost-service-sample"></a>自訂 WebHost 服務範例

這個範例會示範裝載在 Windows 上的 ASP.NET Core 應用程式而不需要使用 IIS 當做 Windows 服務的建議的方式。 這個範例會示範中所述的功能[裝載在 Windows 服務的 ASP.NET Core 應用程式](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service)。

## <a name="instructions"></a>指示

範例應用程式是簡易 MVC web 應用程式中的指示根據修改[裝載在 Windows 服務的 ASP.NET Core 應用程式](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service)。

若要執行應用程式服務中，執行下列步驟：

1. 建立在資料夾*c:\svc*。

1. 將應用程式發行至資料夾，以`dotnet publish --configuration Release --output c:\\svc`。 此命令會將應用程式的資產移至資料夾，包括所需`appsettings.json`檔案和`wwwroot`資料夾及其內容。

1. 開啟**管理員**命令殼層。

1. 執行下列命令：

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

1. 在瀏覽器，移至`http://localhost:5000`來確認服務正在執行。

1. 若要停止服務，使用此命令：

   ```console
   sc stop MyService
   ```

如果應用程式未如預期的服務執行時啟動啟動，讓錯誤訊息都能存取快速方法是將記錄提供者，例如[Windows 事件記錄檔提供者](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog)。 另一個選項是使用事件檢視器系統上的應用程式事件日誌。 例如，以下是針對 FileNotFound 錯誤應用程式事件日誌中未處理例外狀況：

```console
Application: AspNetCoreService.exe
Framework Version: v4.0.30319
Description: The process was terminated due to an unhandled exception.
Exception Info: System.IO.FileNotFoundException
   at Microsoft.Extensions.Configuration.FileConfigurationProvider.Load(Boolean)
   at Microsoft.Extensions.Configuration.ConfigurationRoot..ctor(System.Collections.Generic.IList`1<Microsoft.Extensions.Configuration.IConfigurationProvider>)
   at Microsoft.Extensions.Configuration.ConfigurationBuilder.Build()
   ...
```
