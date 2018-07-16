# <a name="custom-webhost-service-sample"></a><span data-ttu-id="2dd82-101">自訂 WebHost 服務範例</span><span class="sxs-lookup"><span data-stu-id="2dd82-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="2dd82-102">這個範例會示範如何在不使用 IIS 的狀況下，將 ASP.NET Core 應用程式裝載為 Windows 服務。</span><span class="sxs-lookup"><span data-stu-id="2dd82-102">This sample shows how to host an ASP.NET Core app as a Windows Service without using IIS.</span></span> <span data-ttu-id="2dd82-103">這個範例會示範[在 Windows 服務中裝載 ASP.NET Core 應用程式](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service)中描述的案例。</span><span class="sxs-lookup"><span data-stu-id="2dd82-103">This sample demonstrates the scenario described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="2dd82-104">指示</span><span class="sxs-lookup"><span data-stu-id="2dd82-104">Instructions</span></span>

<span data-ttu-id="2dd82-105">這個範例應用程式是一個 Razor Pages Web 應用程式，其根據[在 Windows 服務中裝載 ASP.NET Core 應用程式](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service)中的指示而修改。</span><span class="sxs-lookup"><span data-stu-id="2dd82-105">The sample app is a Razor Pages web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="2dd82-106">若要在服務中執行該應用程式，請執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="2dd82-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="2dd82-107">在 *c:\svc* 建立一個資料夾。</span><span class="sxs-lookup"><span data-stu-id="2dd82-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="2dd82-108">使用 `dotnet publish --configuration Release --output c:\\svc`，將應用程式發行至資料夾。</span><span class="sxs-lookup"><span data-stu-id="2dd82-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="2dd82-109">這個命令會將應用程式的資產移至 *svc* 資料夾，包括所需的 `appsettings.json` 檔案和 `wwwroot` 資料夾。</span><span class="sxs-lookup"><span data-stu-id="2dd82-109">The command moves the app's assets to the *svc* folder, including the required `appsettings.json` file and the `wwwroot` folder.</span></span>

1. <span data-ttu-id="2dd82-110">開啟**系統管理員**命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="2dd82-110">Open an **administrator** command prompt.</span></span>

1. <span data-ttu-id="2dd82-111">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="2dd82-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  <span data-ttu-id="2dd82-112">「等號和路徑字串開頭間需要有間距。」</span><span class="sxs-lookup"><span data-stu-id="2dd82-112">*The space between the equal sign and the start of the path string is required.*</span></span>

1. <span data-ttu-id="2dd82-113">在瀏覽器中，巡覽至 `http://localhost:5000` 確認服務正在執行。</span><span class="sxs-lookup"><span data-stu-id="2dd82-113">In a browser, navigate to `http://localhost:5000` and verify that the service is running.</span></span> <span data-ttu-id="2dd82-114">應用程式重新導向至安全的端點 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="2dd82-114">The app redirects to the secure endpoint `https://localhost:5001`.</span></span>

1. <span data-ttu-id="2dd82-115">若要停止服務，使請用以下命令：</span><span class="sxs-lookup"><span data-stu-id="2dd82-115">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="2dd82-116">如果應用程式未如預期啟動，可快速存取錯誤訊息的方法是新增一個記錄提供者，例如 [Windows EventLog 提供者](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog)。</span><span class="sxs-lookup"><span data-stu-id="2dd82-116">If the app doesn't start as expected, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="2dd82-117">另一個選項是使用系統上的事件檢視器來檢查應用程式事件記錄檔。</span><span class="sxs-lookup"><span data-stu-id="2dd82-117">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="2dd82-118">例如，應用程式事件記錄檔中有一個與 FileNotFound 錯誤有關的未處理例外狀況：</span><span class="sxs-lookup"><span data-stu-id="2dd82-118">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

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
