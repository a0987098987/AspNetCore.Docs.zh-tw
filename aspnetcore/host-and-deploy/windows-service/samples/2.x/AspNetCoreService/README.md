# <a name="custom-webhost-service-sample"></a>自訂 WebHost 服務範例

這個範例會示範如何在不使用 IIS 的狀況下，將 ASP.NET Core 應用程式裝載為 Windows 服務。 這個範例會示範[在 Windows 服務中裝載 ASP.NET Core 應用程式](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service)中描述的案例。

## <a name="instructions"></a>指示

這個範例應用程式是一個 Razor Pages Web 應用程式，其根據[在 Windows 服務中裝載 ASP.NET Core 應用程式](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service)中的指示而修改。

若要在服務中執行該應用程式，請執行下列步驟：

1. 在 *c:\svc* 建立一個資料夾。

1. 使用 `dotnet publish --configuration Release --output c:\\svc`，將應用程式發行至資料夾。 這個命令會將應用程式的資產移至 *svc* 資料夾，包括所需的 `appsettings.json` 檔案和 `wwwroot` 資料夾。

1. 開啟**系統管理員**命令提示字元。

1. 執行下列命令：

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  「等號和路徑字串開頭間需要有間距。」

1. 在瀏覽器中，巡覽至 `http://localhost:5000` 確認服務正在執行。 應用程式重新導向至安全的端點 `https://localhost:5001`。

1. 若要停止服務，使請用以下命令：

   ```console
   sc stop MyService
   ```

如果應用程式未如預期啟動，可快速存取錯誤訊息的方法是新增一個記錄提供者，例如 [Windows EventLog 提供者](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog)。 另一個選項是使用系統上的事件檢視器來檢查應用程式事件記錄檔。 例如，應用程式事件記錄檔中有一個與 FileNotFound 錯誤有關的未處理例外狀況：

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
