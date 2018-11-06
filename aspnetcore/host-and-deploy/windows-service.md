---
title: 在 Windows 服務上裝載 ASP.NET Core
author: guardrex
description: 了解如何在 Windows 服務上裝載 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/30/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 11913019bfe5d06c259b806fce9cc580a8280ad5
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758189"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="56632-103">在 Windows 服務上裝載 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="56632-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="56632-104">作者：[Luke Latham](https://github.com/guardrex) 和 [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="56632-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="56632-105">ASP.NET Core 應用程式可以裝載在 Windows 上，不需要使用 IIS 作為 [Windows 服務](/dotnet/framework/windows-services/introduction-to-windows-service-applications)。</span><span class="sxs-lookup"><span data-stu-id="56632-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="56632-106">當裝載為 Windows 服務時，應用程式將會在重新開機後自動啟動。</span><span class="sxs-lookup"><span data-stu-id="56632-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="56632-107">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="56632-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="56632-108">將專案轉換成 Windows 服務</span><span class="sxs-lookup"><span data-stu-id="56632-108">Convert a project into a Windows Service</span></span>

<span data-ttu-id="56632-109">至少需要變更下列內容，才能設定現有的 ASP.NET Core 專案作為服務執行：</span><span class="sxs-lookup"><span data-stu-id="56632-109">The following minimum changes are required to set up an existing ASP.NET Core project to run as a service:</span></span>

1. <span data-ttu-id="56632-110">在專案檔中：</span><span class="sxs-lookup"><span data-stu-id="56632-110">In the project file:</span></span>

   * <span data-ttu-id="56632-111">確認有 Windows [執行階段識別碼 (RID)](/dotnet/core/rid-catalog)，或將其新增至包含目標 Framework 的 `<PropertyGroup>`：</span><span class="sxs-lookup"><span data-stu-id="56632-111">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add it to the `<PropertyGroup>` that contains the target framework:</span></span>

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.2</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      <span data-ttu-id="56632-112">發行多個 RID：</span><span class="sxs-lookup"><span data-stu-id="56632-112">To publish for multiple RIDs:</span></span>

      * <span data-ttu-id="56632-113">以分號分隔的清單提供 RID。</span><span class="sxs-lookup"><span data-stu-id="56632-113">Provide the RIDs in a semicolon-delimited list.</span></span>
      * <span data-ttu-id="56632-114">使用屬性名稱 `<RuntimeIdentifiers>` (複數)。</span><span class="sxs-lookup"><span data-stu-id="56632-114">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

      <span data-ttu-id="56632-115">如需詳細資訊，請參閱 [.NET Core RID 目錄](/dotnet/core/rid-catalog)。</span><span class="sxs-lookup"><span data-stu-id="56632-115">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

   * <span data-ttu-id="56632-116">新增 [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) 的套件參考。</span><span class="sxs-lookup"><span data-stu-id="56632-116">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

1. <span data-ttu-id="56632-117">在 `Program.Main` 中進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="56632-117">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="56632-118">呼叫 [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice)，而非 `host.Run`。</span><span class="sxs-lookup"><span data-stu-id="56632-118">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="56632-119">呼叫 [UseContentRoot](xref:fundamentals/host/web-host#content-root) 並使用應用程式發佈位置的路徑，不要使用 `Directory.GetCurrentDirectory()`。</span><span class="sxs-lookup"><span data-stu-id="56632-119">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

1. <span data-ttu-id="56632-120">使用 [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) (這是一個 [Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles)) 或 Visual Studio Code 來發行應用程式。</span><span class="sxs-lookup"><span data-stu-id="56632-120">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="56632-121">使用 Visual Studio 時，請選取 [FolderProfile] 並設定 [目標位置]，再選取 [發行] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="56632-121">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

   <span data-ttu-id="56632-122">若要使用命令列介面 (CLI) 工具發行範例應用程式，請從專案資料夾的命令提示字元執行 [dotnet publish](/dotnet/core/tools/dotnet-publish)命令。</span><span class="sxs-lookup"><span data-stu-id="56632-122">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder.</span></span> <span data-ttu-id="56632-123">必須在 `<RuntimeIdenfifier>` (或 `<RuntimeIdentifiers>`) 中指定 RID 專案檔的屬性。</span><span class="sxs-lookup"><span data-stu-id="56632-123">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="56632-124">在下列範例中，應用程式在 `win7-x64` 執行階段「發行設定」中發行到在 *c:\\svc* 建立的資料夾：</span><span class="sxs-lookup"><span data-stu-id="56632-124">In the following example, the app is published in Release configuration for the `win7-x64` runtime to a folder created at *c:\\svc*:</span></span>

   ```console
   dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
   ```

1. <span data-ttu-id="56632-125">使用 `net user` 命令為服務建立使用者帳戶：</span><span class="sxs-lookup"><span data-stu-id="56632-125">Create a user account for the service using the `net user` command:</span></span>

   ```console
   net user {USER ACCOUNT} {PASSWORD} /add
   ```

   <span data-ttu-id="56632-126">針對範例應用程式，建立名為 `ServiceUser` 的使用者帳戶與密碼。</span><span class="sxs-lookup"><span data-stu-id="56632-126">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="56632-127">在下列命令中，將 `{PASSWORD}` 取代為[強式密碼](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements)。</span><span class="sxs-lookup"><span data-stu-id="56632-127">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

   ```console
   net user ServiceUser {PASSWORD} /add
   ```

   <span data-ttu-id="56632-128">若需要將使用者新增到某個群組，請使用 `net localgroup` 命令，其中 `{GROUP}` 是群組的名稱：</span><span class="sxs-lookup"><span data-stu-id="56632-128">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

   ```console
   net localgroup {GROUP} {USER ACCOUNT} /add
   ```

   <span data-ttu-id="56632-129">如需詳細資訊，請參閱[服務使用者帳戶](/windows/desktop/services/service-user-accounts)。</span><span class="sxs-lookup"><span data-stu-id="56632-129">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

1. <span data-ttu-id="56632-130">使用 [icacls](/windows-server/administration/windows-commands/icacls) 命令授與對應用程式資料夾的寫入/讀取/執行存取權：</span><span class="sxs-lookup"><span data-stu-id="56632-130">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command:</span></span>

   ```console
   icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
   ```

   * <span data-ttu-id="56632-131">`{PATH}` &ndash; 應用程式資料夾的路徑。</span><span class="sxs-lookup"><span data-stu-id="56632-131">`{PATH}` &ndash; Path to the app's folder.</span></span>
   * <span data-ttu-id="56632-132">`{USER ACCOUNT}` &ndash; 使用者帳戶 (SID)。</span><span class="sxs-lookup"><span data-stu-id="56632-132">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
   * <span data-ttu-id="56632-133">`(OI)` &ndash;「物件繼承旗標會將權限傳播到次級檔案。</span><span class="sxs-lookup"><span data-stu-id="56632-133">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
   * <span data-ttu-id="56632-134">`(CI)` &ndash;「物件繼承旗標會將權限傳播到次級檔案。</span><span class="sxs-lookup"><span data-stu-id="56632-134">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
   * <span data-ttu-id="56632-135">`{PERMISSION FLAGS}` &ndash; 設定應用程式的存取權限。</span><span class="sxs-lookup"><span data-stu-id="56632-135">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
     * <span data-ttu-id="56632-136">寫入 (`W`)</span><span class="sxs-lookup"><span data-stu-id="56632-136">Write (`W`)</span></span>
     * <span data-ttu-id="56632-137">讀取 (`R`)</span><span class="sxs-lookup"><span data-stu-id="56632-137">Read (`R`)</span></span>
     * <span data-ttu-id="56632-138">執行 (`X`)</span><span class="sxs-lookup"><span data-stu-id="56632-138">Execute (`X`)</span></span>
     * <span data-ttu-id="56632-139">完整 (`F`)</span><span class="sxs-lookup"><span data-stu-id="56632-139">Full (`F`)</span></span>
     * <span data-ttu-id="56632-140">修改 (`M`)</span><span class="sxs-lookup"><span data-stu-id="56632-140">Modify (`M`)</span></span>
   * <span data-ttu-id="56632-141">`/t` &ndash; 套用遞迴到現有的次級資料夾與檔案。</span><span class="sxs-lookup"><span data-stu-id="56632-141">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

   <span data-ttu-id="56632-142">針對發行到 *c:\\svc* 資料夾的範例應用程式與具有寫入/讀取/執行權限的 `ServiceUser` 帳戶，請使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="56632-142">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

   ```console
   icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
   ```

   <span data-ttu-id="56632-143">如需詳細資訊，請參閱 [icacls](/windows-server/administration/windows-commands/icacls)。</span><span class="sxs-lookup"><span data-stu-id="56632-143">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

1. <span data-ttu-id="56632-144">使用 [sc.exe](https://technet.microsoft.com/library/bb490995) 命令列工具建立服務。</span><span class="sxs-lookup"><span data-stu-id="56632-144">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="56632-145">`binPath` 值是應用程式可執行檔的路徑，其中包括可執行檔的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="56632-145">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="56632-146">**等號與每個參數與值的引號字元之間必須有空格。**</span><span class="sxs-lookup"><span data-stu-id="56632-146">**The space between the equal sign and the quote character of each parameter and value is required.**</span></span>

   ```console
   sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
   ```

   * <span data-ttu-id="56632-147">`{SERVICE NAME}` &ndash; 要指派給[服務控制管理員](/windows/desktop/services/service-control-manager)中之服務的名稱。</span><span class="sxs-lookup"><span data-stu-id="56632-147">`{SERVICE NAME}` &ndash; The name to assign to the service in [Service Control Manager](/windows/desktop/services/service-control-manager).</span></span>
   * <span data-ttu-id="56632-148">`{PATH}` &ndash; 服務可執行檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="56632-148">`{PATH}` &ndash; The path to the service executable.</span></span>
   * <span data-ttu-id="56632-149">`{DOMAIN}` (或若電腦未加入網域，則為本機電腦名稱) 與 `{USER ACCOUNT}` &ndash; 服務執行所在的網域 (或本機電腦名稱) 與使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="56632-149">`{DOMAIN}` (or if the machine isn't domain joined, the local machine name) and `{USER ACCOUNT}` &ndash; The domain (or local machine name) and user account under which the service runs.</span></span> <span data-ttu-id="56632-150">請**勿**省略 `obj` 參數。</span><span class="sxs-lookup"><span data-stu-id="56632-150">Do **not** omit the `obj` parameter.</span></span> <span data-ttu-id="56632-151">`obj` 的預設值是 [LocalSystem 帳戶](/windows/desktop/services/localsystem-account)帳戶。</span><span class="sxs-lookup"><span data-stu-id="56632-151">The default value for `obj` is the [LocalSystem account](/windows/desktop/services/localsystem-account) account.</span></span> <span data-ttu-id="56632-152">以 `LocalSystem` 帳戶執行服務會帶來重大安全性風險。</span><span class="sxs-lookup"><span data-stu-id="56632-152">Running a service under the `LocalSystem` account presents a significant security risk.</span></span> <span data-ttu-id="56632-153">一律與具有伺服器上受限權限的使用者帳戶來執行服務。</span><span class="sxs-lookup"><span data-stu-id="56632-153">Always run a service under a user account with restricted privileges on the server.</span></span>
   * <span data-ttu-id="56632-154">`{PASSWORD}` &ndash; 使用者帳戶密碼。</span><span class="sxs-lookup"><span data-stu-id="56632-154">`{PASSWORD}` &ndash; The user account password.</span></span>

   <span data-ttu-id="56632-155">在下列範例中：</span><span class="sxs-lookup"><span data-stu-id="56632-155">In the following example:</span></span>

   * <span data-ttu-id="56632-156">服務的名稱是 **MyService**。</span><span class="sxs-lookup"><span data-stu-id="56632-156">The service is named **MyService**.</span></span>
   * <span data-ttu-id="56632-157">已發行的服務位於 *c:\\svc* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="56632-157">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="56632-158">應用程式可執行檔的名稱是 *AspNetCoreService.exe*。</span><span class="sxs-lookup"><span data-stu-id="56632-158">The app executable is named *AspNetCoreService.exe*.</span></span> <span data-ttu-id="56632-159">`binPath` 值以一般引號 (") 括住。</span><span class="sxs-lookup"><span data-stu-id="56632-159">The `binPath` value is enclosed in straight quotation marks (").</span></span>
   * <span data-ttu-id="56632-160">服務是以 `ServiceUser` 帳戶執行。</span><span class="sxs-lookup"><span data-stu-id="56632-160">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="56632-161">將 `{DOMAIN}` 取代為使用者帳戶的網域或本機電腦名稱。</span><span class="sxs-lookup"><span data-stu-id="56632-161">Replace `{DOMAIN}` with the user account's domain or local machine name.</span></span> <span data-ttu-id="56632-162">將 `obj` 值以一般引號 (") 括住。</span><span class="sxs-lookup"><span data-stu-id="56632-162">Enclose the `obj` value in straight quotation marks (").</span></span> <span data-ttu-id="56632-163">範例：若裝載系統是名為 `MairaPC` 的本機電腦，請將 `obj` 設定為 `"MairaPC\ServiceUser"`。</span><span class="sxs-lookup"><span data-stu-id="56632-163">Example: If the hosting system is a local machine named `MairaPC`, set `obj` to `"MairaPC\ServiceUser"`.</span></span>
   * <span data-ttu-id="56632-164">將 `{PASSWORD}` 取代為使用者帳戶的密碼。</span><span class="sxs-lookup"><span data-stu-id="56632-164">Replace `{PASSWORD}` with the user account's password.</span></span> <span data-ttu-id="56632-165">`password` 值以一般引號 (") 括住。</span><span class="sxs-lookup"><span data-stu-id="56632-165">The `password` value is enclosed in straight quotation marks (").</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
   ```

   > [!IMPORTANT]
   > <span data-ttu-id="56632-166">確定參數的等號與參數的值之間有空格。</span><span class="sxs-lookup"><span data-stu-id="56632-166">Make sure that the spaces between the parameters' equal signs and the parameters' values are present.</span></span>

1. <span data-ttu-id="56632-167">以 `sc start {SERVICE NAME}` 命令啟動服務。</span><span class="sxs-lookup"><span data-stu-id="56632-167">Start the service with the `sc start {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="56632-168">請使用下列命令啟動範例應用程式服務：</span><span class="sxs-lookup"><span data-stu-id="56632-168">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="56632-169">此命令需要幾秒鐘啓動服務。</span><span class="sxs-lookup"><span data-stu-id="56632-169">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="56632-170">若要檢查服務的狀態，請使用 `sc query {SERVICE NAME}` 命令。</span><span class="sxs-lookup"><span data-stu-id="56632-170">To check the status of the service, use the `sc query {SERVICE NAME}` command.</span></span> <span data-ttu-id="56632-171">狀態會回報為下列值之一：</span><span class="sxs-lookup"><span data-stu-id="56632-171">The status is reported as one of the following values:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="56632-172">使用下列命令來檢查範例應用程式服務的狀態：</span><span class="sxs-lookup"><span data-stu-id="56632-172">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="56632-173">當服務處於 `RUNNING` 狀態，且若服務是 Web 應用程式時，請在其路徑瀏覽應用程式 (預設為 `http://localhost:5000`，使用 [HTTPS 重新導向中介軟體](xref:security/enforcing-ssl)時會重新導向至 `https://localhost:5001`)。</span><span class="sxs-lookup"><span data-stu-id="56632-173">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="56632-174">範例應用程式服務請瀏覽位於 `http://localhost:5000` 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="56632-174">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="56632-175">以 `sc stop {SERVICE NAME}` 命令停止服務。</span><span class="sxs-lookup"><span data-stu-id="56632-175">Stop the service with the `sc stop {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="56632-176">下列命令會停止範例應用程式服務：</span><span class="sxs-lookup"><span data-stu-id="56632-176">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="56632-177">在停止服務的短暫延遲之後，請以 `sc delete {SERVICE NAME}` 命令解除安裝服務。</span><span class="sxs-lookup"><span data-stu-id="56632-177">After a short delay to stop a service, uninstall the service with the `sc delete {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="56632-178">檢查範例應用程式服務的狀態：</span><span class="sxs-lookup"><span data-stu-id="56632-178">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="56632-179">當範例應用程式服務處於 `STOPPED` 狀態時，請使用下列命令來解除安裝範例應用程式服務：</span><span class="sxs-lookup"><span data-stu-id="56632-179">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a><span data-ttu-id="56632-180">在服務外執行應用程式</span><span class="sxs-lookup"><span data-stu-id="56632-180">Run the app outside of a service</span></span>

<span data-ttu-id="56632-181">在服務外執行時較容易進行測試和偵錯，因此習慣上只有在特定情況下，才會新增呼叫 `RunAsService` 的程式碼。</span><span class="sxs-lookup"><span data-stu-id="56632-181">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="56632-182">例如，使用 `--console` 命令列引數或連結偵錯工具，即可讓應用程式以主控台應用程式的形式執行：</span><span class="sxs-lookup"><span data-stu-id="56632-182">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="56632-183">因為 ASP.NET Core 組態需要命令列引數成對的名稱和數值，所以會先移除 `--console` 參數，再將引數傳遞至 [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder)。</span><span class="sxs-lookup"><span data-stu-id="56632-183">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="56632-184">因為 `CreateWebHostBuilder` 的簽章必須為 `CreateWebHostBuilder(string[])`，[整合測試](xref:test/integration-tests)才可正常運作，所以不會將 `isService` 從 `Main` 傳遞至 `CreateWebHostBuilder`。</span><span class="sxs-lookup"><span data-stu-id="56632-184">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="56632-185">處理停止和啟動事件</span><span class="sxs-lookup"><span data-stu-id="56632-185">Handle stopping and starting events</span></span>

<span data-ttu-id="56632-186">請執行下列額外變更，以處理 [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting)、[OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) 和 [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) 事件：</span><span class="sxs-lookup"><span data-stu-id="56632-186">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="56632-187">建立衍生自 [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) 的類別：</span><span class="sxs-lookup"><span data-stu-id="56632-187">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="56632-188">為將自訂的 `WebHostService` 傳遞給 [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) 的 [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) 建立擴充方法：</span><span class="sxs-lookup"><span data-stu-id="56632-188">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="56632-189">在 `Program.Main` 中，呼叫新的擴充方法 `RunAsCustomService`，而不是呼叫 [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice)：</span><span class="sxs-lookup"><span data-stu-id="56632-189">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > <span data-ttu-id="56632-190">因為 `CreateWebHostBuilder` 的簽章必須為 `CreateWebHostBuilder(string[])`，[整合測試](xref:test/integration-tests)才可正常運作，所以不會將 `isService` 從 `Main` 傳遞至 `CreateWebHostBuilder`。</span><span class="sxs-lookup"><span data-stu-id="56632-190">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

<span data-ttu-id="56632-191">如果自訂的 `WebHostService` 程式碼需要一個來自相依性插入的服務 (例如記錄器)，請從 [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) 屬性取得該服務：</span><span class="sxs-lookup"><span data-stu-id="56632-191">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="56632-192">Proxy 伺服器和負載平衡器案例</span><span class="sxs-lookup"><span data-stu-id="56632-192">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="56632-193">服務如果會與來自網際網路或公司網路的要求進行互動，並且位於 Proxy 或負載平衡器後方，可能會需要額外的設定。</span><span class="sxs-lookup"><span data-stu-id="56632-193">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="56632-194">如需詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。</span><span class="sxs-lookup"><span data-stu-id="56632-194">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="56632-195">設定 HTTPS</span><span class="sxs-lookup"><span data-stu-id="56632-195">Configure HTTPS</span></span>

<span data-ttu-id="56632-196">使用安全端點來設定服務：</span><span class="sxs-lookup"><span data-stu-id="56632-196">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="56632-197">使用您平台的憑證取得與部署機制來建立將用於裝載系統的 X.509 憑證。</span><span class="sxs-lookup"><span data-stu-id="56632-197">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>
1. <span data-ttu-id="56632-198">指定 [Kestrel 伺服器 HTTPS 端點設定](xref:fundamentals/servers/kestrel#endpoint-configuration)以使用憑證。</span><span class="sxs-lookup"><span data-stu-id="56632-198">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="56632-199">不支援使用 ASP.NET Core HTTPS 開發憑證來保護服務端點。</span><span class="sxs-lookup"><span data-stu-id="56632-199">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="56632-200">目前目錄和內容根目錄</span><span class="sxs-lookup"><span data-stu-id="56632-200">Current directory and content root</span></span>

<span data-ttu-id="56632-201">針對 Windows 服務呼叫 `Directory.GetCurrentDirectory()` 所傳回的目前工作目錄為 *C:\\WINDOWS\\system32* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="56632-201">The current working directory returned by calling `Directory.GetCurrentDirectory()` for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="56632-202">*System32* 資料夾不是儲存服務檔案 (例如，設定檔) 的合適位置。</span><span class="sxs-lookup"><span data-stu-id="56632-202">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="56632-203">請使用下列其中一種方法，於使用 [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) 時，利用 [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) 來維護及存取服務的資產和設定檔：</span><span class="sxs-lookup"><span data-stu-id="56632-203">Use one of the following approaches to maintain and access a service's assets and settings files with [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) when using an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span></span>

* <span data-ttu-id="56632-204">使用內容根路徑。</span><span class="sxs-lookup"><span data-stu-id="56632-204">Use the content root path.</span></span> <span data-ttu-id="56632-205">`IHostingEnvironment.ContentRootPath` 與建立服務時提供給 `binPath` 引數的路徑相同。</span><span class="sxs-lookup"><span data-stu-id="56632-205">The `IHostingEnvironment.ContentRootPath` is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="56632-206">請使用內容根路徑並維護應用程式內容根目錄中的檔案，而不是使用 `Directory.GetCurrentDirectory()` 來建立設定檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="56632-206">Instead of using `Directory.GetCurrentDirectory()` to create paths to settings files, use the content root path and maintain the files in the app's content root.</span></span>
* <span data-ttu-id="56632-207">將檔案儲存在磁碟上的適當位置。</span><span class="sxs-lookup"><span data-stu-id="56632-207">Store the files in a suitable location on disk.</span></span> <span data-ttu-id="56632-208">請使用 `SetBasePath` 將絕對路徑指定為包含檔案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="56632-208">Specify an absolute path with `SetBasePath` to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="56632-209">其他資源</span><span class="sxs-lookup"><span data-stu-id="56632-209">Additional resources</span></span>

* <span data-ttu-id="56632-210">[Kestrel 端點組態](xref:fundamentals/servers/kestrel#endpoint-configuration) (包括 HTTPS 組態與 SNI 支援)</span><span class="sxs-lookup"><span data-stu-id="56632-210">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
