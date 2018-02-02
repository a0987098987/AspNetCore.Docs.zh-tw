# <a name="custom-webhost-service-sample"></a><span data-ttu-id="bfce8-101">自訂 WebHost 服務範例</span><span class="sxs-lookup"><span data-stu-id="bfce8-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="bfce8-102">這個範例會示範裝載在 Windows 上的 ASP.NET Core 應用程式而不需要使用 IIS 當做 Windows 服務的建議的方式。</span><span class="sxs-lookup"><span data-stu-id="bfce8-102">This sample shows the recommended way to host an ASP.NET Core app on Windows without using IIS as a Windows Service.</span></span> <span data-ttu-id="bfce8-103">這個範例會示範中所述的功能[裝載在 Windows 服務的 ASP.NET Core 應用程式](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service)。</span><span class="sxs-lookup"><span data-stu-id="bfce8-103">This sample demonstrates the features described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="bfce8-104">指示</span><span class="sxs-lookup"><span data-stu-id="bfce8-104">Instructions</span></span>

<span data-ttu-id="bfce8-105">範例應用程式是簡易 MVC web 應用程式中的指示根據修改[裝載在 Windows 服務的 ASP.NET Core 應用程式](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service)。</span><span class="sxs-lookup"><span data-stu-id="bfce8-105">The sample app is a simple MVC web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="bfce8-106">若要執行應用程式服務中，執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="bfce8-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="bfce8-107">建立在資料夾*c:\svc*。</span><span class="sxs-lookup"><span data-stu-id="bfce8-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="bfce8-108">將應用程式發行至資料夾，以`dotnet publish --configuration Release --output c:\\svc`。</span><span class="sxs-lookup"><span data-stu-id="bfce8-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="bfce8-109">此命令會將應用程式的資產移至資料夾，包括所需`appsettings.json`檔案和`wwwroot`資料夾及其內容。</span><span class="sxs-lookup"><span data-stu-id="bfce8-109">The command will move the app's assets to the folder, including the required `appsettings.json` file and the `wwwroot` folder with its contents.</span></span>

1. <span data-ttu-id="bfce8-110">開啟**管理員**命令殼層。</span><span class="sxs-lookup"><span data-stu-id="bfce8-110">Open an **administrator** command shell.</span></span>

1. <span data-ttu-id="bfce8-111">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="bfce8-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

1. <span data-ttu-id="bfce8-112">在瀏覽器，移至`http://localhost:5000`來確認服務正在執行。</span><span class="sxs-lookup"><span data-stu-id="bfce8-112">In a browser, go to `http://localhost:5000` to verify that the service is running.</span></span>

1. <span data-ttu-id="bfce8-113">若要停止服務，使用此命令：</span><span class="sxs-lookup"><span data-stu-id="bfce8-113">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="bfce8-114">如果應用程式未如預期的服務執行時啟動啟動，讓錯誤訊息都能存取快速方法是將記錄提供者，例如[Windows 事件記錄檔提供者](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog)。</span><span class="sxs-lookup"><span data-stu-id="bfce8-114">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="bfce8-115">另一個選項是使用事件檢視器系統上的應用程式事件日誌。</span><span class="sxs-lookup"><span data-stu-id="bfce8-115">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="bfce8-116">例如，以下是針對 FileNotFound 錯誤應用程式事件日誌中未處理例外狀況：</span><span class="sxs-lookup"><span data-stu-id="bfce8-116">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

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
