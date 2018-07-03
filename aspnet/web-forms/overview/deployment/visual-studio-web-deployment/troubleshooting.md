---
uid: web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
title: 使用 Visual Studio 的 ASP.NET Web 部署： 疑難排解 |Microsoft Docs
author: tdykstra
description: 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/01/2015
ms.topic: article
ms.assetid: c0090595-ab3b-4b9b-9e16-7a1891e8cb2f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 0b57a91c29ba15463e6c534693b951aee8286ad2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363998"
---
<a name="aspnet-web-deployment-using-visual-studio-troubleshooting"></a><span data-ttu-id="37d73-103">使用 Visual Studio 的 ASP.NET Web 部署： 疑難排解</span><span class="sxs-lookup"><span data-stu-id="37d73-103">ASP.NET Web Deployment using Visual Studio: Troubleshooting</span></span>
====================
<span data-ttu-id="37d73-104">藉由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="37d73-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="37d73-105">下載入門專案</span><span class="sxs-lookup"><span data-stu-id="37d73-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="37d73-106">本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="37d73-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="37d73-107">這個系列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="37d73-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


<span data-ttu-id="37d73-108">此頁面描述一些常見的問題，當您使用 Visual Studio 部署 ASP.NET web 應用程式時可能發生。</span><span class="sxs-lookup"><span data-stu-id="37d73-108">This page describes some common problems that may arise when you deploy an ASP.NET web application by using Visual Studio.</span></span> <span data-ttu-id="37d73-109">每個會提供一或多個可能原因和對應的解決方案。</span><span class="sxs-lookup"><span data-stu-id="37d73-109">For each one, one or more possible causes and corresponding solutions are provided.</span></span>

<span data-ttu-id="37d73-110">顯示案例適用於 Azure 和協力廠商主機服務提供者。</span><span class="sxs-lookup"><span data-stu-id="37d73-110">The scenarios shown apply to both Azure and third-party hosting providers.</span></span> <span data-ttu-id="37d73-111">如需有關如何疑難排解 Azure App Service 中的 web 應用程式的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="37d73-111">For more information about troubleshooting web apps in Azure App Service, see the following resources:</span></span>

- [<span data-ttu-id="37d73-112">使用 Visual Studio 疑難排解 Azure App Service 中的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="37d73-112">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)
- [<span data-ttu-id="37d73-113">監視 Azure App Service 中的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="37d73-113">Monitor Web Apps in Azure App Service</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-monitor//)
- <span data-ttu-id="37d73-114">[發表最新的 Windows Azure SDK 2.0 for.NET](http://https://weblogs.asp.net/scottgu/announcing-the-release-of-windows-azure-sdk-2-0-for-net) （ScottGu 的部落格，會示範如何取得 Visual Studio 中的診斷記錄檔）</span><span class="sxs-lookup"><span data-stu-id="37d73-114">[Announcing the release of Windows Azure SDK 2.0 for .NET](http://https://weblogs.asp.net/scottgu/announcing-the-release-of-windows-azure-sdk-2-0-for-net) (ScottGu's blog, shows how to get diagnostic logs in Visual Studio)</span></span>

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a><span data-ttu-id="37d73-115">伺服器錯誤 '/' 應用程式-在目前的自訂錯誤設定防止錯誤的詳細資料從遠端檢視</span><span class="sxs-lookup"><span data-stu-id="37d73-115">Server Error in '/' Application - Current Custom Error Settings Prevent Details of the Error from Being Viewed Remotely</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-116">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-116">Scenario</span></span>

<span data-ttu-id="37d73-117">部署至遠端主機的站台之後，您會收到錯誤訊息，提及 customErrors 設定 Web.config 檔案中的，而不會指出實際錯誤的原因是什麼：</span><span class="sxs-lookup"><span data-stu-id="37d73-117">After deploying a site to a remote host, you get an error message that mentions the customErrors setting in the Web.config file but doesn't indicate what the actual cause of the error was:</span></span>

[!code-xml[Main](troubleshooting/samples/sample1.xml)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-118">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-118">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-119">根據預設，ASP.NET 會顯示詳細的錯誤資訊，只有在本機電腦上執行您的 web 應用程式時。</span><span class="sxs-lookup"><span data-stu-id="37d73-119">By default, ASP.NET shows detailed error information only when your web application is running on the local computer.</span></span> <span data-ttu-id="37d73-120">通常您不想要顯示詳細的錯誤資訊，您的 web 應用程式時，透過網際網路公開可用，因為駭客可能無法使用此資訊來尋找應用程式中的弱點。</span><span class="sxs-lookup"><span data-stu-id="37d73-120">Generally you don't want to display detailed error information when your web application is publicly available over the Internet, because hackers may be able to use this information to find vulnerabilities in the application.</span></span> <span data-ttu-id="37d73-121">不過，當您部署或更新的站台至站台，有時會發生錯誤了，而且您需要取得實際的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="37d73-121">However, when you are deploying a site or updates to a site, sometimes something will go wrong and you need to get the actual error message.</span></span>

<span data-ttu-id="37d73-122">若要啟用的應用程式，以顯示詳細的錯誤訊息，遠端主機上執行時，編輯 Web.config 檔，以便將 customErrors 模式設定、 重新部署應用程式，並再次執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="37d73-122">To enable the application to display detailed error messages when it runs on the remote host, edit the Web.config file to set customErrors mode off, redeploy the application, and run the application again:</span></span>

1. <span data-ttu-id="37d73-123">如果應用程式 Web.config 檔案有 acustomErrors 元素 thesystem.web 項目中，變更為 「 關閉 」 themode 屬性。</span><span class="sxs-lookup"><span data-stu-id="37d73-123">If the application Web.config file has acustomErrors element in thesystem.web element, change themode attribute to "off".</span></span> <span data-ttu-id="37d73-124">否則 acustomErrors 項目中新增 thesystem.web 項目並 themode 屬性設定為 「 關閉 」，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="37d73-124">Otherwise add acustomErrors element in thesystem.web element with themode attribute set to "off", as shown in the following example:</span></span> 

    [!code-xml[Main](troubleshooting/samples/sample2.xml)]
2. <span data-ttu-id="37d73-125">部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="37d73-125">Deploy the application.</span></span>
3. <span data-ttu-id="37d73-126">執行應用程式並重複任何您先前導致發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="37d73-126">Run the application and repeat whatever you did earlier that caused the error to occur.</span></span> <span data-ttu-id="37d73-127">現在您可以看到實際的錯誤訊息的功能。</span><span class="sxs-lookup"><span data-stu-id="37d73-127">Now you can see what the actual error message is.</span></span>
4. <span data-ttu-id="37d73-128">當您解決錯誤時，請還原原始的 customErrors 設定，並重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="37d73-128">When you have resolved the error, restore the original customErrors setting and redeploy the application.</span></span>

## <a name="cannot-createshadow-copy-contosouniversity-when-that-file-already-exists"></a><span data-ttu-id="37d73-129">無法建立/陰影複製 'ContosoUniversity' 已經存在的檔案。</span><span class="sxs-lookup"><span data-stu-id="37d73-129">Cannot create/shadow copy 'ContosoUniversity' when that file already exists.</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-130">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-130">Scenario</span></span>

<span data-ttu-id="37d73-131">當您嘗試在 Visual Studio 中執行專案時您會得到錯誤頁面的訊息，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="37d73-131">When you try to run a project in Visual Studio you get an error page with a message like the following example:</span></span>

<span data-ttu-id="37d73-132">'/' 應用程式中的伺服器錯誤。</span><span class="sxs-lookup"><span data-stu-id="37d73-132">Server Error in '/' Application.</span></span> <span data-ttu-id="37d73-133">無法建立/陰影複製 'ContosoUniversity' 已經存在的檔案。</span><span class="sxs-lookup"><span data-stu-id="37d73-133">Cannot create/shadow copy 'ContosoUniversity' when that file already exists.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-134">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-134">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-135">等候一分鐘的時間和重新整理瀏覽器中，或重新編譯的站台並嘗試再次執行它。</span><span class="sxs-lookup"><span data-stu-id="37d73-135">Wait a minute and refresh the browser, or recompile the site and try running it again.</span></span>

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a><span data-ttu-id="37d73-136">存取遭到拒絕在網頁中使用 SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="37d73-136">Access is Denied in a Web Page that Uses SQL Server Compact</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-137">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-137">Scenario</span></span>

<span data-ttu-id="37d73-138">當您部署使用 SQL Server Compact 的網站和您在存取資料庫的已部署站台執行頁面時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="37d73-138">When you deploy a site that uses SQL Server Compact and you run a page in the deployed site that accesses the database, you see the following error message:</span></span>

<span data-ttu-id="37d73-139">存取被拒絕。</span><span class="sxs-lookup"><span data-stu-id="37d73-139">Access is denied.</span></span> <span data-ttu-id="37d73-140">(來自 HRESULT 的例外狀況： 0x80070005 (E\_ACCESSDENIED))</span><span class="sxs-lookup"><span data-stu-id="37d73-140">(Exception from HRESULT: 0x80070005 (E\_ACCESSDENIED))</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-141">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-141">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-142">在伺服器上的網路服務帳戶必須能夠讀取 SQL Compact 服務中的原生二進位檔*bin\amd64*或是*bin\x86*資料夾中，但沒有讀取這些資料夾的權限。</span><span class="sxs-lookup"><span data-stu-id="37d73-142">The NETWORK SERVICE account on the server needs to be able to read SQL Service Compact native binaries that are in the *bin\amd64* or *bin\x86* folder, but it does not have read permissions for those folders.</span></span> <span data-ttu-id="37d73-143">讀取網路服務的權限集*bin*資料夾，並確認其擴充至子資料夾的權限。</span><span class="sxs-lookup"><span data-stu-id="37d73-143">Set read permission for NETWORK SERVICE on the *bin* folder, making sure to extend the permissions to subfolders.</span></span>

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a><span data-ttu-id="37d73-144">無法讀取組態檔，因為沒有足夠的權限</span><span class="sxs-lookup"><span data-stu-id="37d73-144">Cannot Read Configuration File Due to Insufficient Permissions</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-145">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-145">Scenario</span></span>

<span data-ttu-id="37d73-146">當您按一下 Visual Studio 發行應用程式部署至 IIS 在您的本機電腦上的按鈕發行會失敗並**輸出**視窗會顯示一則錯誤訊息如下所示：</span><span class="sxs-lookup"><span data-stu-id="37d73-146">When you click the Visual Studio publish button to deploy an application to IIS on your local machine, publishing fails and the **Output** window shows an error message similar to this:</span></span>

<span data-ttu-id="37d73-147">讀取 IIS 設定檔 'MACHINE/重新導向' 時，就會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="37d73-147">An error occurred when reading the IIS Configuration File 'MACHINE/REDIRECTION'.</span></span> <span data-ttu-id="37d73-148">執行此作業的身分識別為...錯誤： 無法讀取組態檔，因為沒有足夠的權限。</span><span class="sxs-lookup"><span data-stu-id="37d73-148">The identity performing this operation was ... Error: Cannot read configuration file due to insufficient permissions.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-149">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-149">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-150">若要使用單鍵發行至 IIS 在本機電腦上，您必須以系統管理員權限執行 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="37d73-150">To use one-click publish to IIS on your local machine, you must be running Visual Studio with administrator permissions.</span></span> <span data-ttu-id="37d73-151">關閉 Visual Studio 並重新啟動系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="37d73-151">Close Visual Studio and restart it with administrator permissions.</span></span>

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a><span data-ttu-id="37d73-152">無法連接到目的地電腦...使用指定的處理序</span><span class="sxs-lookup"><span data-stu-id="37d73-152">Could Not Connect to the Destination Computer ... Using the Specified Process</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-153">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-153">Scenario</span></span>

<span data-ttu-id="37d73-154">當您按一下 Visual Studio 發佈按鈕來部署應用程式時，發行會失敗並**輸出**視窗會顯示一則錯誤訊息如下所示：</span><span class="sxs-lookup"><span data-stu-id="37d73-154">When you click the Visual Studio publish button to deploy an application, publishing fails and the **Output** window shows an error message similar to this:</span></span>

[!code-console[Main](troubleshooting/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-155">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-155">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-156">Proxy 伺服器會中斷與目的地伺服器的通訊。</span><span class="sxs-lookup"><span data-stu-id="37d73-156">A proxy server is interrupting communication with the destination server.</span></span> <span data-ttu-id="37d73-157">從 Windows 控制台或 Internet Explorer 中，選取**網際網路選項**，然後選取**連線** 索引標籤。在 **網際網路內容** 對話方塊中，按一下**LAN 設定**。</span><span class="sxs-lookup"><span data-stu-id="37d73-157">From the Windows Control Panel or in Internet Explorer, select **Internet Options** and select the **Connections** tab. In the **Internet Properties** dialog box, click **LAN Settings**.</span></span> <span data-ttu-id="37d73-158">在 **區域網路 (LAN) 設定**對話方塊中，清除**自動偵測設定**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="37d73-158">In the **Local Area Network (LAN) Settings** dialog box, clear the **Automatically detect settings** checkbox.</span></span> <span data-ttu-id="37d73-159">然後再按一下 [發行] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="37d73-159">Then click the publish button again.</span></span>

<span data-ttu-id="37d73-160">如果問題持續發生，請連絡您的系統管理員，以判斷哪些可以使用 proxy 或防火牆設定。</span><span class="sxs-lookup"><span data-stu-id="37d73-160">If the problem persists, contact your system administrator to determine what can be done with proxy or firewall settings.</span></span> <span data-ttu-id="37d73-161">問題是因為 Web Deploy Web 管理服務部署 (8172); 使用非標準連接埠對於其他連線，Web Deploy，將會使用連接埠 80。</span><span class="sxs-lookup"><span data-stu-id="37d73-161">The problem happens because Web Deploy uses a non-standard port for Web Management Service deployment (8172); for other connections, Web Deploy uses port 80.</span></span> <span data-ttu-id="37d73-162">當您要部署到協力廠商主機服務提供者時，通常使用 Web 管理服務。</span><span class="sxs-lookup"><span data-stu-id="37d73-162">When you are deploying to a third-party hosting provider, you are typically using the Web Management Service.</span></span>

## <a name="default-net-40-application-pool-does-not-exist"></a><span data-ttu-id="37d73-163">預設.NET 4.0 的應用程式集區不存在</span><span class="sxs-lookup"><span data-stu-id="37d73-163">Default .NET 4.0 Application Pool Does Not Exist</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-164">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-164">Scenario</span></span>

<span data-ttu-id="37d73-165">當您部署的應用程式需要.NET Framework 4 時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="37d73-165">When you deploy an application that requires the .NET Framework 4, you see the following error message:</span></span>

<span data-ttu-id="37d73-166">預設.NET 4.0 應用程式集區不存在或無法加入應用程式。</span><span class="sxs-lookup"><span data-stu-id="37d73-166">The default .NET 4.0 application pool does not exist or the application could not be added.</span></span> <span data-ttu-id="37d73-167">請確認 ASP.NET 4.0 安裝在這部電腦上。</span><span class="sxs-lookup"><span data-stu-id="37d73-167">Please verify that ASP.NET 4.0 is installed on this machine.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-168">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-168">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-169">在 IIS 中未安裝 ASP.NET 4。</span><span class="sxs-lookup"><span data-stu-id="37d73-169">ASP.NET 4 is not installed in IIS.</span></span> <span data-ttu-id="37d73-170">如果您要部署到伺服器，您的開發電腦且已安裝 Visual Studio 2010，ASP.NET 4 的電腦上已安裝，但可能未安裝在 IIS 中。</span><span class="sxs-lookup"><span data-stu-id="37d73-170">If the server you are deploying to is your development computer and has Visual Studio 2010 installed on it, ASP.NET 4 is installed on the computer but might not be installed in IIS.</span></span> <span data-ttu-id="37d73-171">在您要部署伺服器上，開啟提升權限的命令提示字元並安裝 ASP.NET 4 在 IIS 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="37d73-171">On the server that you are deploying to, open an elevated command prompt and install ASP.NET 4 in IIS by running the following commands:</span></span>

[!code-console[Main](troubleshooting/samples/sample4.cmd)]

<span data-ttu-id="37d73-172">您也可能需要手動設定預設應用程式集區的.NET Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="37d73-172">You might also need to manually set the .NET Framework version of the default application pool.</span></span> <span data-ttu-id="37d73-173">如需詳細資訊，請參閱作為測試環境教學課程，在這一系列的 iis 部署。</span><span class="sxs-lookup"><span data-stu-id="37d73-173">For more information, see the Deploying to IIS as a Test Environment tutorial in this series.</span></span>

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a><span data-ttu-id="37d73-174">初始化字串的格式不符合從索引 0 處開始的規格。</span><span class="sxs-lookup"><span data-stu-id="37d73-174">Format of the initialization string does not conform to specification starting at index 0.</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-175">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-175">Scenario</span></span>

<span data-ttu-id="37d73-176">部署應用程式使用一種單鍵之後發行，當您執行的頁面存取資料庫，得到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="37d73-176">After you deploy an application using one-click publish, when you run a page that accesses the database you get the following error message:</span></span>

<span data-ttu-id="37d73-177">初始化字串的格式不符合從索引 0 處開始的規格。</span><span class="sxs-lookup"><span data-stu-id="37d73-177">Format of the initialization string does not conform to specification starting at index 0.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-178">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-178">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-179">開啟*Web.config*檔案中的已部署的站台和檢查連接字串值是否會以 $ 開頭 (ReplacableToken\_，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="37d73-179">Open the *Web.config* file in the deployed site and check to see whether the connection string values begin with $(ReplacableToken\_, as in the following example:</span></span>

[!code-xml[Main](troubleshooting/samples/sample5.xml)]

<span data-ttu-id="37d73-180">如果連接字串看起來像此範例中，編輯專案檔，並針對所有組建組態的 PropertyGroup 項目中加入下列屬性：</span><span class="sxs-lookup"><span data-stu-id="37d73-180">If the connection strings look like this example, edit the project file and add the following property to the PropertyGroup element that is for all build configurations:</span></span>

[!code-xml[Main](troubleshooting/samples/sample6.xml)]

<span data-ttu-id="37d73-181">然後重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="37d73-181">Then redeploy the application.</span></span>

## <a name="http-500-internal-server-error"></a><span data-ttu-id="37d73-182">HTTP 500 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="37d73-182">HTTP 500 Internal Server Error</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-183">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-183">Scenario</span></span>

<span data-ttu-id="37d73-184">當您執行已部署的站台時，您會看到下列錯誤訊息沒有指出錯誤的原因的特定資訊：</span><span class="sxs-lookup"><span data-stu-id="37d73-184">When you run the deployed site, you see the following error message without specific information indicating the cause of the error:</span></span>

<span data-ttu-id="37d73-185">HTTP 錯誤 500-內部伺服器錯誤。</span><span class="sxs-lookup"><span data-stu-id="37d73-185">HTTP Error 500 - Internal Server Error.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-186">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-186">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-187">原因有很多的 500 錯誤，但如果您要遵循這些教學課程中的其中一個可能原因是您將 XML 項目放在其中一個 Web.config 轉換檔中錯誤的位置。</span><span class="sxs-lookup"><span data-stu-id="37d73-187">There are many causes of 500 errors, but one possible cause if you are following these tutorials is that you put an XML element in the wrong place in one of the Web.config transformation files.</span></span> <span data-ttu-id="37d73-188">例如，您會收到這個錯誤如果您將插入的轉換&lt;位置&gt;下方的項目&lt;system.web&gt;而不是正下方&lt;組態&gt;。</span><span class="sxs-lookup"><span data-stu-id="37d73-188">For example, you would get this error if you put the transformation that inserts a &lt;location&gt; element under &lt;system.web&gt; instead of directly under &lt;configuration&gt;.</span></span> <span data-ttu-id="37d73-189">若要確認轉換如預期般運作，您可以使用 Web.config 轉換預覽功能。</span><span class="sxs-lookup"><span data-stu-id="37d73-189">You can use the Web.config transform preview feature to verify that transformations are working as intended.</span></span> <span data-ttu-id="37d73-190">如果您發現不正確的原始轉換解決方案是更正轉換檔案，然後重新部署。</span><span class="sxs-lookup"><span data-stu-id="37d73-190">The solution if you find a transform that was coded incorrectly is to correct the transformation file and redeploy.</span></span> <span data-ttu-id="37d73-191">如果錯誤不明顯，請嘗試 標記為註解轉換，然後重新部署，以查看哪一個導致 500 錯誤。</span><span class="sxs-lookup"><span data-stu-id="37d73-191">If an error isn't obvious, try commenting out transforms and redeploying to see which one is causing the 500 error.</span></span>

## <a name="http-50021-internal-server-error"></a><span data-ttu-id="37d73-192">HTTP 500.21 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="37d73-192">HTTP 500.21 Internal Server Error</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-193">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-193">Scenario</span></span>

<span data-ttu-id="37d73-194">當您執行已部署的站台時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="37d73-194">When you run the deployed site, you see the following error message:</span></span>

<span data-ttu-id="37d73-195">HTTP 錯誤 500.21-內部伺服器錯誤。</span><span class="sxs-lookup"><span data-stu-id="37d73-195">HTTP Error 500.21 - Internal Server Error.</span></span> <span data-ttu-id="37d73-196">「 Pagehandlerfactory-isapi-整合式 」 的處理常式有不正確的模組"ManagedPipelineHandler 」 模組清單中。</span><span class="sxs-lookup"><span data-stu-id="37d73-196">Handler "PageHandlerFactory-Integrated" has a bad module "ManagedPipelineHandler" in its module list.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-197">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-197">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-198">站台，您已部署 ASP.NET 4 中，但 ASP.NET 4 未註冊在 IIS 伺服器的目標。</span><span class="sxs-lookup"><span data-stu-id="37d73-198">The site you have deployed targets ASP.NET 4, but ASP.NET 4 is not registered in IIS on the server.</span></span> <span data-ttu-id="37d73-199">在伺服器上開啟提升權限的命令提示字元，並執行下列命令註冊 ASP.NET 4:</span><span class="sxs-lookup"><span data-stu-id="37d73-199">On the server open an elevated command prompt and register ASP.NET 4 by running the following commands:</span></span>

[!code-console[Main](troubleshooting/samples/sample7.cmd)]

<span data-ttu-id="37d73-200">您也可能需要手動設定預設應用程式集區的.NET Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="37d73-200">You might also need to manually set the .NET Framework version of the default application pool.</span></span> <span data-ttu-id="37d73-201">如需詳細資訊，請參閱作為測試環境教學課程，在這一系列的 iis 部署。</span><span class="sxs-lookup"><span data-stu-id="37d73-201">For more information, see the Deploying to IIS as a Test Environment tutorial in this series.</span></span>

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a><span data-ttu-id="37d73-202">登入失敗的應用程式中的開啟 SQL Server Express Database\_資料</span><span class="sxs-lookup"><span data-stu-id="37d73-202">Login Failed Opening SQL Server Express Database in App\_Data</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-203">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-203">Scenario</span></span>

<span data-ttu-id="37d73-204">您已更新*Web.config*檔案以指向做為 SQL Server Express 資料庫的連接字串 *.mdf*檔案中您*應用程式\_資料*資料夾，然後第一個您執行應用程式，您會看到下列錯誤訊息的時間：</span><span class="sxs-lookup"><span data-stu-id="37d73-204">You updated the *Web.config* file connection string to point to a SQL Server Express database as an *.mdf* file in your *App\_Data* folder, and the first time you run the application you see the following error message:</span></span>

<span data-ttu-id="37d73-205">「 System.Data.SqlClient.SqlException： 無法開啟資料庫"DatabaseName 」 登入所要求。</span><span class="sxs-lookup"><span data-stu-id="37d73-205">System.Data.SqlClient.SqlException: Cannot open database "DatabaseName" requested by the login.</span></span> <span data-ttu-id="37d73-206">登入失敗。</span><span class="sxs-lookup"><span data-stu-id="37d73-206">The login failed.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-207">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-207">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-208">名稱 *.mdf*檔案無法比對任何具有曾經存在過您的電腦的 SQL Server Express 資料庫的名稱，即使您刪除 *.mdf*先前已存在的資料庫檔案。</span><span class="sxs-lookup"><span data-stu-id="37d73-208">The name of the *.mdf* file cannot match the name of any SQL Server Express database that has ever existed on your computer, even if you deleted the *.mdf* file of the previously existing database.</span></span> <span data-ttu-id="37d73-209">變更的名稱 *.mdf*從未使用做為資料庫名稱變更為名稱的檔案*Web.config*檔案，以使用新的名稱。</span><span class="sxs-lookup"><span data-stu-id="37d73-209">Change the name of the *.mdf* file to a name that has never been used as a database name and change the *Web.config* file to use the new name.</span></span> <span data-ttu-id="37d73-210">或者，您可以使用[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)刪除先前已經有 SQL Server Express 資料庫。</span><span class="sxs-lookup"><span data-stu-id="37d73-210">As an alternative, you can use [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) to delete previously existing SQL Server Express databases.</span></span>

## <a name="model-compatibility-cannot-be-checked"></a><span data-ttu-id="37d73-211">模型的相容性無法檢查</span><span class="sxs-lookup"><span data-stu-id="37d73-211">Model Compatibility Cannot be Checked</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-212">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-212">Scenario</span></span>

<span data-ttu-id="37d73-213">您已更新*Web.config*檔案連接字串以指向新的 SQL Server Express 資料庫，並執行應用程式的第一次您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="37d73-213">You updated the *Web.config* file connection string to point to a new SQL Server Express database, and the first time you run the application you see the following error message:</span></span>

<span data-ttu-id="37d73-214">無法檢查模型相容性，因為資料庫不包含模型中繼資料。</span><span class="sxs-lookup"><span data-stu-id="37d73-214">Model compatibility cannot be checked because the database does not contain model metadata.</span></span> <span data-ttu-id="37d73-215">請確定 IncludeMetadataConvention 確認已新增 DbModelBuilder 慣例。</span><span class="sxs-lookup"><span data-stu-id="37d73-215">Ensure that IncludeMetadataConvention has been added to the DbModelBuilder conventions.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-216">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-216">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-217">如果之前您在電腦上，資料庫可能已經存在的某些資料表中，曾經使用您放在 Web.config 檔案中的資料庫名稱。</span><span class="sxs-lookup"><span data-stu-id="37d73-217">If the database name you put in the Web.config file was ever used before on your computer, a database might already exist with some tables in it.</span></span> <span data-ttu-id="37d73-218">選取您電腦之前變更尚未使用的新名稱*Web.config*檔以指向使用這個新的資料庫名稱。</span><span class="sxs-lookup"><span data-stu-id="37d73-218">Select a new name that has not been used on your computer before and change the *Web.config* file to point to use this new database name.</span></span> <span data-ttu-id="37d73-219">或者，您可以使用[SQL Server Express 公用程式](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990)或是[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)刪除現有的資料庫。</span><span class="sxs-lookup"><span data-stu-id="37d73-219">As an alternative, you can use [SQL Server Express Utility](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) or [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) to delete the existing database.</span></span>

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a><span data-ttu-id="37d73-220">SQL 錯誤時指令碼會嘗試建立使用者或角色</span><span class="sxs-lookup"><span data-stu-id="37d73-220">SQL Error When a Script Attempts to Create Users or Roles</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-221">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-221">Scenario</span></span>

<span data-ttu-id="37d73-222">您使用上設定的資料庫部署**封裝/發行 SQL**  索引標籤，在部署期間執行的 SQL 指令碼包含 Create User 或 Create Role 命令和指令碼執行失敗時執行這些命令。</span><span class="sxs-lookup"><span data-stu-id="37d73-222">You are using database deployment configured on the **Package/Publish SQL** tab, SQL scripts that run during deployment include Create User or Create Role commands, and script execution fails when those commands are executed.</span></span> <span data-ttu-id="37d73-223">您可能會看到更多詳細的訊息，如下所示：</span><span class="sxs-lookup"><span data-stu-id="37d73-223">You might see more detailed messages, such as the following:</span></span>

[!code-console[Main](troubleshooting/samples/sample8.cmd)]

<span data-ttu-id="37d73-224">此錯誤發生時，您已設定中的資料庫部署**發佈 Web**精靈而非**封裝/發行 SQL**索引標籤上，建立中的執行緒[組態和部署](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)論壇和解決方案將會新增至本疑難排解頁面。</span><span class="sxs-lookup"><span data-stu-id="37d73-224">If this error occurs when you have configured database deployment in the **Publish Web** wizard rather than the **Package/Publish SQL** tab, create a thread in the [Configuration and Deployment](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) forum, and the solution will be added to this troubleshooting page.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-225">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-225">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-226">您用來執行部署的使用者帳戶沒有建立使用者或角色的權限。</span><span class="sxs-lookup"><span data-stu-id="37d73-226">The user account you are using to perform deployment does not have permission to create users or roles.</span></span> <span data-ttu-id="37d73-227">比方說，控管公司的資料庫可能會被指派\_datareader，db\_資料寫入元，與 db\_ddladmin 角色，它會設定為您的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="37d73-227">For example, the hosting company might assign the db\_datareader, db\_datawriter, and db\_ddladmin roles to the user account that it sets up for you.</span></span> <span data-ttu-id="37d73-228">這些是足夠用於建立大多數的資料庫物件，但不適用於建立使用者或角色。</span><span class="sxs-lookup"><span data-stu-id="37d73-228">These are sufficient for creating most database objects, but not for creating users or roles.</span></span> <span data-ttu-id="37d73-229">若要避免錯誤的方法之一是藉由從資料庫部署中排除使用者和角色。</span><span class="sxs-lookup"><span data-stu-id="37d73-229">One way to avoid the error is by excluding users and roles from database deployment.</span></span> <span data-ttu-id="37d73-230">您可以藉由編輯資料庫的自動產生的指令碼的 PreSource 項目，使其包含下列屬性：</span><span class="sxs-lookup"><span data-stu-id="37d73-230">You can do this by editing the PreSource element for the database's automatically generated script so that it includes the following attributes:</span></span>

[!code-console[Main](troubleshooting/samples/sample9.cmd)]

<span data-ttu-id="37d73-231">如需如何編輯專案檔中的 PreSource 項目資訊，請參閱[如何： 編輯專案檔中的部署設定](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx)。</span><span class="sxs-lookup"><span data-stu-id="37d73-231">For information about how to edit the PreSource element in the project file, see [How to: Edit Deployment Settings in the Project File](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx).</span></span> <span data-ttu-id="37d73-232">如果需要在目的地資料庫中的使用者或開發資料庫中的角色，請連絡您的主機服務提供者尋求協助。</span><span class="sxs-lookup"><span data-stu-id="37d73-232">If the users or roles in your development database need to be in the destination database, contact your hosting provider for assistance.</span></span>

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a><span data-ttu-id="37d73-233">SQL Server 逾時錯誤時在部署期間執行自訂指令碼</span><span class="sxs-lookup"><span data-stu-id="37d73-233">SQL Server Timeout Error When Running Custom Scripts During Deployment</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-234">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-234">Scenario</span></span>

<span data-ttu-id="37d73-235">您已指定在部署期間，執行自訂 SQL 指令碼和 Web Deploy 執行時它們，它們的逾時間。</span><span class="sxs-lookup"><span data-stu-id="37d73-235">You have specified custom SQL scripts to run during deployment, and when Web Deploy runs them, they time out.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-236">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-236">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-237">執行具有不同的交易模式的多個指令碼，可能會導致逾時錯誤。</span><span class="sxs-lookup"><span data-stu-id="37d73-237">Running multiple scripts that have different transaction modes can cause time-out errors.</span></span> <span data-ttu-id="37d73-238">根據預設，自動產生的指令碼執行在交易中，但不是這麼做的自訂指令碼。</span><span class="sxs-lookup"><span data-stu-id="37d73-238">By default, automatically generated scripts run in a transaction, but custom scripts do not.</span></span> <span data-ttu-id="37d73-239">如果您選取**提取資料及/或從現有的資料庫結構描述**選項**封裝/發行 SQL**索引標籤上，而且如果您加入自訂的 SQL 指令碼，您必須變更一些指令碼的交易設定以便所有的指令碼會使用相同的交易設定。</span><span class="sxs-lookup"><span data-stu-id="37d73-239">If you select the **Pull data and/or schema from an existing database** option on the **Package/Publish SQL** tab, and if you add a custom SQL script, you must change transaction settings on some scripts so that all scripts use the same transaction settings.</span></span> <span data-ttu-id="37d73-240">如需詳細資訊，請參閱 <<c0> [ 如何： 資料庫使用 Web 應用程式專案部署](https://msdn.microsoft.com/library/dd465343.aspx)。</span><span class="sxs-lookup"><span data-stu-id="37d73-240">For more information, see [How to: Deploy a Database With a Web Application Project](https://msdn.microsoft.com/library/dd465343.aspx).</span></span>

<span data-ttu-id="37d73-241">如果您已設定交易設定，使所有都相同，但仍然收到這個錯誤，可能的解決方法是分別執行指令碼。</span><span class="sxs-lookup"><span data-stu-id="37d73-241">If you have configured transaction settings so that all are the same but still get this error, a possible workaround is to run the scripts separately.</span></span> <span data-ttu-id="37d73-242">在 **資料庫指令碼**方格中的**封裝/發行**SQL 索引標籤上，清除**Include**造成逾時錯誤，指令碼的核取方塊，然後發行專案。</span><span class="sxs-lookup"><span data-stu-id="37d73-242">In the **Database Scripts** grid in the **Package/Publish** SQL tab, clear the **Include** check box for the script that causes the timeout error, then publish the project.</span></span> <span data-ttu-id="37d73-243">然後移回**資料庫指令碼**方格中，選取該指令碼**Include**核取方塊，然後清除**Include**其他指令碼的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="37d73-243">Then go back into the **Database Scripts** grid, select that script's **Include** check box, and clear the **Include** check boxes for the other scripts.</span></span> <span data-ttu-id="37d73-244">然後重新發佈專案。</span><span class="sxs-lookup"><span data-stu-id="37d73-244">Then publish the project again.</span></span> <span data-ttu-id="37d73-245">此時，當您發行時，只選取自訂的指令碼執行。</span><span class="sxs-lookup"><span data-stu-id="37d73-245">This time when you publish, only the selected custom script runs.</span></span>

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a><span data-ttu-id="37d73-246">站台資訊清單的 Stream 資料尚無法使用</span><span class="sxs-lookup"><span data-stu-id="37d73-246">Stream Data of Site Manifest Is Not Yet Available</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-247">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-247">Scenario</span></span>

<span data-ttu-id="37d73-248">當您要安裝封裝，使用*deploy.cmd*檔案與 t （測試） 選項中，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="37d73-248">When you are installing a package using the *deploy.cmd* file with the t (test) option, you see the following error message:</span></span>

<span data-ttu-id="37d73-249">錯誤： 資料流資料的 ' sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' 尚無法使用。</span><span class="sxs-lookup"><span data-stu-id="37d73-249">Error: The stream data of 'sitemanifest/dbFullSql[@path='C:\TEMP\AdventureWorksGrant.sql']/sqlScript' is not yet available.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-250">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-250">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-251">錯誤訊息表示命令無法產生測試報告。</span><span class="sxs-lookup"><span data-stu-id="37d73-251">The error message means that the command cannot produce a test report.</span></span> <span data-ttu-id="37d73-252">不過，如果您使用 y （實際安裝） 選項，可能會執行的命令。</span><span class="sxs-lookup"><span data-stu-id="37d73-252">However, the command might run if you use the y (actual installation) option.</span></span> <span data-ttu-id="37d73-253">訊息僅會指示是在測試模式中執行命令的問題。</span><span class="sxs-lookup"><span data-stu-id="37d73-253">The message indicates only that there is a problem with running the command in test mode.</span></span>

## <a name="this-application-requires-managedruntimeversion-v40"></a><span data-ttu-id="37d73-254">此應用程式需要 ManagedRuntimeVersion v4.0</span><span class="sxs-lookup"><span data-stu-id="37d73-254">This Application Requires ManagedRuntimeVersion v4.0</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-255">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-255">Scenario</span></span>

<span data-ttu-id="37d73-256">當您嘗試部署時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="37d73-256">When you attempt to deploy, you see the following error message:</span></span>

<span data-ttu-id="37d73-257">您嘗試使用應用程式集區已設定為 'v2.0' 的 'managedRuntimeVersion' 屬性。</span><span class="sxs-lookup"><span data-stu-id="37d73-257">The application pool that you are trying to use has the 'managedRuntimeVersion' property set to 'v2.0'.</span></span> <span data-ttu-id="37d73-258">此應用程式需要 'v4.0'。</span><span class="sxs-lookup"><span data-stu-id="37d73-258">This application requires 'v4.0'.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-259">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-259">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-260">在 IIS 中未安裝 ASP.NET 4。</span><span class="sxs-lookup"><span data-stu-id="37d73-260">ASP.NET 4 is not installed in IIS.</span></span> <span data-ttu-id="37d73-261">如果您要部署到伺服器，您的開發電腦且已安裝 Visual Studio 2010，ASP.NET 4 的電腦上已安裝，但可能未安裝在 IIS 中。</span><span class="sxs-lookup"><span data-stu-id="37d73-261">If the server you are deploying to is your development computer and has Visual Studio 2010 installed on it, ASP.NET 4 is installed on the computer but might not be installed in IIS.</span></span> <span data-ttu-id="37d73-262">在您要部署伺服器上，開啟提升權限的命令提示字元並安裝 ASP.NET 4 在 IIS 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="37d73-262">On the server that you are deploying to, open an elevated command prompt and install ASP.NET 4 in IIS by running the following commands:</span></span>

[!code-console[Main](troubleshooting/samples/sample10.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a><span data-ttu-id="37d73-263">無法轉換 Microsoft.Web.Deployment.DeploymentProviderOptions</span><span class="sxs-lookup"><span data-stu-id="37d73-263">Unable to cast Microsoft.Web.Deployment.DeploymentProviderOptions</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-264">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-264">Scenario</span></span>

<span data-ttu-id="37d73-265">當您部署封裝時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="37d73-265">When you are deploying a package, you see the following error message:</span></span>

<span data-ttu-id="37d73-266">無法將類型 'Microsoft.Web.Deployment.DeploymentProviderOptions' 到 'Microsoft.Web.Deployment.DeploymentProviderOptions' 的物件。</span><span class="sxs-lookup"><span data-stu-id="37d73-266">Unable to cast object of type 'Microsoft.Web.Deployment.DeploymentProviderOptions' to 'Microsoft.Web.Deployment.DeploymentProviderOptions'.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-267">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-267">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-268">您嘗試部署從 IIS 管理員 」 中使用 Web 部署 1.1 UI 已安裝的 Web Deploy 2.0 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="37d73-268">You are trying to deploy from IIS Manager using the Web Deploy 1.1 UI to a server that has Web Deploy 2.0 installed.</span></span> <span data-ttu-id="37d73-269">如果您使用 IIS 的遠端系統管理工具來部署匯入封裝時，檢查**新提供的功能**對話方塊中，當您建立連線。</span><span class="sxs-lookup"><span data-stu-id="37d73-269">If you are using the IIS Remote Administration Tool to deploy by importing a package, check the **New Features Available** dialog box when you establish the connection.</span></span> <span data-ttu-id="37d73-270">（此對話方塊可能只會顯示一次第一次建立連接時。</span><span class="sxs-lookup"><span data-stu-id="37d73-270">(This dialog box might only be shown once when the connection is first established.</span></span> <span data-ttu-id="37d73-271">若要清除連線，然後再重新啟動，關閉 IIS 管理員並啟動它一次輸入 inetmgr/重設在命令提示字元。）列出的其中一項功能會**Web 部署 UI**，其版本號碼低於 8，您要部署到伺服器可能會有 1.1 和 2.0 的 Web Deploy 安裝的版本。</span><span class="sxs-lookup"><span data-stu-id="37d73-271">To clear the connection and start over, close IIS Manager and start it up again by entering inetmgr /reset at the command prompt.) If one of the features listed is **Web Deploy UI**, and it has a version number lower than 8, the server you are deploying to might have both 1.1 and 2.0 versions of Web Deploy installed.</span></span> <span data-ttu-id="37d73-272">若要從已安裝的 2.0 用戶端部署，伺服器必須只有 Web Deploy 2.0 安裝。</span><span class="sxs-lookup"><span data-stu-id="37d73-272">To deploy from a client that has 2.0 installed, the server must have only Web Deploy 2.0 installed.</span></span> <span data-ttu-id="37d73-273">您必須連絡主控提供者，若要解決此問題。</span><span class="sxs-lookup"><span data-stu-id="37d73-273">You will have to contact your hosting provider to resolve this problem.</span></span>

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a><span data-ttu-id="37d73-274">無法載入 SQL Server Compact 的原生元件</span><span class="sxs-lookup"><span data-stu-id="37d73-274">Unable to load the native components of SQL Server Compact</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-275">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-275">Scenario</span></span>

<span data-ttu-id="37d73-276">當您執行已部署的站台時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="37d73-276">When you run the deployed site, you see the following error message:</span></span>

<span data-ttu-id="37d73-277">無法載入 SQL Server Compact 對應至版本 8482 的 ADO.NET 提供者的原生元件。</span><span class="sxs-lookup"><span data-stu-id="37d73-277">Unable to load the native components of SQL Server Compact corresponding to the ADO.NET provider of version 8482.</span></span> <span data-ttu-id="37d73-278">安裝 SQL Server Compact 的正確版本。</span><span class="sxs-lookup"><span data-stu-id="37d73-278">Install the correct version of SQL Server Compact.</span></span> <span data-ttu-id="37d73-279">請參閱 KB 文章 974247 如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="37d73-279">Refer to KB article 974247 for more details.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-280">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-280">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-281">已部署的站台沒有*amd64*並*x86*及其子資料夾中它們的應用程式的原生組件*bin*資料夾。</span><span class="sxs-lookup"><span data-stu-id="37d73-281">The deployed site does not have *amd64* and *x86* subfolders with the native assemblies in them under the application's *bin* folder.</span></span> <span data-ttu-id="37d73-282">SQL Server Compact 安裝的電腦，在原生組件位於*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*。</span><span class="sxs-lookup"><span data-stu-id="37d73-282">On a computer that has SQL Server Compact installed, the native assemblies are located in *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*.</span></span> <span data-ttu-id="37d73-283">若要取得正確的檔案到正確的資料夾，Visual Studio 專案中的最佳方式是安裝 NuGet SqlServerCompact 封裝。</span><span class="sxs-lookup"><span data-stu-id="37d73-283">The best way to get the correct files into the correct folders in a Visual Studio project is to install the NuGet SqlServerCompact package.</span></span> <span data-ttu-id="37d73-284">套件安裝新增建置後指令碼來複製原生組件，載入*amd64*並*x86*。</span><span class="sxs-lookup"><span data-stu-id="37d73-284">Package installation adds a post-build script to copy the native assemblies into *amd64* and *x86*.</span></span> <span data-ttu-id="37d73-285">為了讓這些部署，不過，您必須手動將其包含在專案中。</span><span class="sxs-lookup"><span data-stu-id="37d73-285">In order for these to be deployed, however, you have to manually include them in the project.</span></span> <span data-ttu-id="37d73-286">如需詳細資訊，請參閱 <<c0> [ 部署 SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="37d73-286">For more information, see the [Deploying SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) tutorial.</span></span>

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a><span data-ttu-id="37d73-287">部署 Entity Framework Code First 應用程式之後的 「 路徑不正確 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="37d73-287">"Path is not valid" error after deploying an Entity Framework Code First application</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-288">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-288">Scenario</span></span>

<span data-ttu-id="37d73-289">您部署的應用程式，例如 SQL Server Compact 將其資料庫儲存於應用程式中的檔案就會使用 Entity Framework Code First 移轉和 DBMS\_Data 資料夾。</span><span class="sxs-lookup"><span data-stu-id="37d73-289">You deploy an application that uses Entity Framework Code First Migrations and a DBMS such as SQL Server Compact which stores its database in a file in the App\_Data folder.</span></span> <span data-ttu-id="37d73-290">您必須設定為在您第一次部署之後建立資料庫的 Code First 移轉。</span><span class="sxs-lookup"><span data-stu-id="37d73-290">You have Code First Migrations configured to create the database after your first deployment.</span></span> <span data-ttu-id="37d73-291">當您執行應用程式時您會收到錯誤訊息如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="37d73-291">When you run the application you get an error message like the following example:</span></span>

<span data-ttu-id="37d73-292">路徑不是有效的。</span><span class="sxs-lookup"><span data-stu-id="37d73-292">The path is not valid.</span></span> <span data-ttu-id="37d73-293">請檢查資料庫目錄。</span><span class="sxs-lookup"><span data-stu-id="37d73-293">Check the directory for the database.</span></span> <span data-ttu-id="37d73-294">[路徑 = c:\inetpub\wwwroot\App\_Data\DatabaseName.sdf]</span><span class="sxs-lookup"><span data-stu-id="37d73-294">[Path = c:\inetpub\wwwroot\App\_Data\DatabaseName.sdf ]</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-295">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-295">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-296">程式碼第一次嘗試建立資料庫，但應用程式\_資料資料夾不存在。</span><span class="sxs-lookup"><span data-stu-id="37d73-296">Code First is attempting to create the database but the App\_Data folder does not exist.</span></span> <span data-ttu-id="37d73-297">您未有任何檔案*應用程式\_資料*當您部署，或您所選取的資料夾**排除應用程式\_資料**上**封裝/發行 Web**  索引標籤的 專案屬性 視窗。</span><span class="sxs-lookup"><span data-stu-id="37d73-297">Either you didn't have any files in the *App\_Data* folder when you deployed, or you selected **Exclude App\_Data** on the **Package/Publish Web** tab of the Project Properties window.</span></span> <span data-ttu-id="37d73-298">部署程序不會在伺服器上建立資料夾，如果要複製到伺服器的資料夾中不有任何檔案。</span><span class="sxs-lookup"><span data-stu-id="37d73-298">The deployment process won't create a folder on the server if there are no files in the folder to be copied to the server.</span></span> <span data-ttu-id="37d73-299">如果您已經在網站中設定的資料庫，在部署程序會刪除的檔案和*應用程式\_資料*本身如果您選取的資料夾**移除目的地上的其他檔案**中發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="37d73-299">If you already had the database set up in the site, the deployment process will delete the files and the *App\_Data* folder itself if you selected **Remove additional files at destination** in the publish profile.</span></span> <span data-ttu-id="37d73-300">若要解決此問題，將預留位置檔案放在.txt 檔案*應用程式\_資料*資料夾，請確定您沒有**排除應用程式\_資料**選取，並重新部署。</span><span class="sxs-lookup"><span data-stu-id="37d73-300">To solve the problem, put a placeholder file such as a .txt file in the *App\_Data* folder, make sure you do not have **Exclude App\_Data** selected, and redeploy.</span></span>

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a><span data-ttu-id="37d73-301">「 無法使用分開基礎 RCW 的 COM 物件。 」</span><span class="sxs-lookup"><span data-stu-id="37d73-301">"COM object that has been separated from its underlying RCW cannot be used."</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-302">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-302">Scenario</span></span>

<span data-ttu-id="37d73-303">您已順利使用單鍵發行來部署您的應用程式啟動，而您收到此錯誤：</span><span class="sxs-lookup"><span data-stu-id="37d73-303">You have been successfully using one-click publish to deploy your application and then you start getting this error:</span></span>

<span data-ttu-id="37d73-304">Web deployment 工作失敗。</span><span class="sxs-lookup"><span data-stu-id="37d73-304">Web deployment task failed.</span></span> <span data-ttu-id="37d73-305">(無法完成遠端代理程式的 url 要求 '<https://serverurl.com/msdeploy.axd?site=sitename>'。)</span><span class="sxs-lookup"><span data-stu-id="37d73-305">(Could not complete the request to remote agent URL '<https://serverurl.com/msdeploy.axd?site=sitename>'.)</span></span>  
 <span data-ttu-id="37d73-306">無法完成遠端代理程式的 url 要求 '<https://url/msdeploy.axd?site=sitename>'。</span><span class="sxs-lookup"><span data-stu-id="37d73-306">Could not complete the request to remote agent URL '<https://url/msdeploy.axd?site=sitename>'.</span></span>  
<span data-ttu-id="37d73-307">要求已中止： 已取消要求。</span><span class="sxs-lookup"><span data-stu-id="37d73-307">The request was aborted: The request was canceled.</span></span>  
<span data-ttu-id="37d73-308">不能分開基礎 RCW 的 COM 物件。</span><span class="sxs-lookup"><span data-stu-id="37d73-308">COM object that has been separated from its underlying RCW cannot be used.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-309">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-309">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-310">關閉並重新啟動 Visual Studio 通常是所有所需解決此錯誤。</span><span class="sxs-lookup"><span data-stu-id="37d73-310">Closing and restarting Visual Studio is usually all that is required to resolve this error.</span></span>

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a><span data-ttu-id="37d73-311">部署失敗因為使用者認證用於發佈的並沒有 setACL 授權單位</span><span class="sxs-lookup"><span data-stu-id="37d73-311">Deployment Fails Because User Credentials Used for Publishing Don't Have setACL Authority</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-312">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-312">Scenario</span></span>

<span data-ttu-id="37d73-313">發行失敗，發生錯誤，指出您沒有權限設定資料夾權限 （您使用的使用者帳戶沒有 setACL 授權單位）。</span><span class="sxs-lookup"><span data-stu-id="37d73-313">Publishing fails with an error that indicates you don't have authority to set folder permissions (the user account you are using doesn't have setACL authority).</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-314">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-314">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-315">根據預設，Visual Studio 設定站台的根資料夾的權限上讀取和寫入權限的應用程式\_Data 資料夾。</span><span class="sxs-lookup"><span data-stu-id="37d73-315">By default, Visual Studio sets read permissions on the root folder of the site and write permissions on the App\_Data folder.</span></span> <span data-ttu-id="37d73-316">若您知道的站台資料夾的預設權限是否正確，而且不需要設定，您加入停用此行為**&lt;IncludeSetACLProviderOn 目的地&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** 至發行設定檔 （以會影響單一設定檔） 或.wpp.targets 檔案 （以會影響所有設定檔）。</span><span class="sxs-lookup"><span data-stu-id="37d73-316">If you know that the default permissions on site folders are correct and do not need to be set, you disable this behavior by adding **&lt;IncludeSetACLProviderOn Destination&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** to the publish profile file (to affect a single profile) or to the wpp.targets file (to affect all profiles).</span></span> <span data-ttu-id="37d73-317">如需有關如何編輯這些檔案的資訊，請參閱[如何： 編輯設定檔 (.pubxml) 檔中的部署設定](https://msdn.microsoft.com/library/ff398069.aspx)。</span><span class="sxs-lookup"><span data-stu-id="37d73-317">For information about how to edit these files, see [How to: Edit Deployment Settings in Profile (.pubxml) Files](https://msdn.microsoft.com/library/ff398069.aspx).</span></span>

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a><span data-ttu-id="37d73-318">當應用程式嘗試寫入應用程式資料夾時，存取被拒錯誤</span><span class="sxs-lookup"><span data-stu-id="37d73-318">Access Denied Errors when the Application Tries to Write to an Application Folder</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-319">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-319">Scenario</span></span>

<span data-ttu-id="37d73-320">您的應用程式錯誤時嘗試建立或編輯的檔案中的其中一個應用程式資料夾中，因為它並沒有該資料夾的寫入授權單位。</span><span class="sxs-lookup"><span data-stu-id="37d73-320">Your application errors when it tries to create or edit a file in one of the application folders, because it does not have write authority for that folder.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-321">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-321">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-322">根據預設，Visual Studio 設定站台的根資料夾的權限上讀取和寫入權限的應用程式\_Data 資料夾。</span><span class="sxs-lookup"><span data-stu-id="37d73-322">By default, Visual Studio sets read permissions on the root folder of the site and write permissions on the App\_Data folder.</span></span> <span data-ttu-id="37d73-323">如果您的應用程式需要的子資料夾的寫入權限，您可以設定為該資料夾的權限中所示設定資料夾權限並部署至生產環境教學課程，在這一系列。</span><span class="sxs-lookup"><span data-stu-id="37d73-323">If your application needs write access to a sub-folder, you can set permissions for that folder as shown in the Setting Folder Permissions and Deploying to the Production Environment tutorials in this series.</span></span> <span data-ttu-id="37d73-324">如果您的應用程式需要的站台的根資料夾的寫入權限，您必須防止根資料夾上設定唯讀存取權，加上**&lt;IncludeSetACLProviderOn 目的地&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** 至發行設定檔 （以會影響單一設定檔） 或.wpp.targets 檔案 （以會影響所有設定檔）。</span><span class="sxs-lookup"><span data-stu-id="37d73-324">If your application needs write access to the root folder of the site, you have to prevent it from setting read-only access on the root folder by adding **&lt;IncludeSetACLProviderOn Destination&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** to the publish profile file (to affect a single profile) or to the wpp.targets file (to affect all profiles).</span></span> <span data-ttu-id="37d73-325">如需有關如何編輯這些檔案的資訊，請參閱[如何： 編輯設定檔 (.pubxml) 檔中的部署設定](https://msdn.microsoft.com/library/ff398069.aspx)。</span><span class="sxs-lookup"><span data-stu-id="37d73-325">For information about how to edit these files, see [How to: Edit Deployment Settings in Profile (.pubxml) Files](https://msdn.microsoft.com/library/ff398069.aspx).</span></span>

<a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a><span data-ttu-id="37d73-326">組態錯誤-targetFramework 屬性參考的版本晚於安裝的.NET Framework 版本</span><span class="sxs-lookup"><span data-stu-id="37d73-326">Configuration Error - targetFramework attribute references a version that is later than the installed version of the .NET Framework</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-327">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-327">Scenario</span></span>

<span data-ttu-id="37d73-328">您已成功發佈 web 專案以 ASP.NET 4.5 為目標，但是 （使用 customErrors 模式設定為 「 關閉 」 的 Web.config 檔案中） 執行應用程式時您會收到下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="37d73-328">You successfully published a web project that targets ASP.NET 4.5, but when you run the application (with the customErrors mode set to "off" in the Web.config file) you get the following error:</span></span>

<span data-ttu-id="37d73-329">'targetFramework' 屬性中&lt;編譯&gt;才能目標版本 4.0 和更新版本的.NET Framework 使用 Web.config 檔案的項目 (例如，'&lt;編譯 targetFramework ="4.0"&gt;').</span><span class="sxs-lookup"><span data-stu-id="37d73-329">The 'targetFramework' attribute in the &lt;compilation&gt; element of the Web.config file is used only to target version 4.0 and later of the .NET Framework (for example, '&lt;compilation targetFramework="4.0"&gt;').</span></span> <span data-ttu-id="37d73-330">'TargetFramework' 屬性目前參考的版本晚於安裝的.NET Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="37d73-330">The 'targetFramework' attribute currently references a version that is later than the installed version of the .NET Framework.</span></span> <span data-ttu-id="37d73-331">指定有效的目標版本的.NET Framework 中，或安裝必要的.NET Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="37d73-331">Specify a valid target version of the .NET Framework, or install the required version of the .NET Framework.</span></span>

<span data-ttu-id="37d73-332">來源錯誤中的 [錯誤] 頁面中，反白顯示下行從 Web.config 做為錯誤的原因：</span><span class="sxs-lookup"><span data-stu-id="37d73-332">The Source Error box of the error page highlights the following line from Web.config as the cause of the error:</span></span>

<span data-ttu-id="37d73-333">&lt;compilation targetFramework="4.5" /&gt;</span><span class="sxs-lookup"><span data-stu-id="37d73-333">&lt;compilation targetFramework="4.5" /&gt;</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-334">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-334">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-335">伺服器不支援 ASP.NET 4.5。</span><span class="sxs-lookup"><span data-stu-id="37d73-335">The server does not support ASP.NET 4.5.</span></span> <span data-ttu-id="37d73-336">請連絡來判斷何時及是否可以加入支援 ASP.NET 4.5 的主機服務提供者。</span><span class="sxs-lookup"><span data-stu-id="37d73-336">Contact the hosting provider to determine when and if support for ASP.NET 4.5 can be added.</span></span> <span data-ttu-id="37d73-337">如果升級伺服器不是一個選項，您必須部署之 web 專案的目標 ASP.NET 4 或更早版本改。</span><span class="sxs-lookup"><span data-stu-id="37d73-337">If upgrading the server is not an option, you have to deploy a web project that targets ASP.NET 4 or earlier instead.</span></span>

<span data-ttu-id="37d73-338">如果您將 ASP.NET 4 或較早的 web 專案部署到相同的目的地時，選取**移除其他檔案目的地** 核取方塊**設定**索引標籤**發佈 Web**精靈。</span><span class="sxs-lookup"><span data-stu-id="37d73-338">If you deploy an ASP.NET 4 or earlier web project to the same destination, select the **Remove additional files at destination** check box on the **Settings** tab of the **Publish Web** wizard.</span></span> <span data-ttu-id="37d73-339">如果您未選取**移除目的地上的其他檔案**，您仍會繼續設定錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="37d73-339">If you don't select **Remove additional files at destination**, you will continue to get the Configuration Error page.</span></span>

<span data-ttu-id="37d73-340">專案**屬性**windows 包含目標 framework 下拉式清單中，但您不能解決這個問題，只要變更，從 **.NET Framework 4.5**到 **.NET Framework 4**.</span><span class="sxs-lookup"><span data-stu-id="37d73-340">The project **Properties** windows includes a Target framework drop-down list, but you can't resolve this problem by just changing that from **.NET Framework 4.5** to **.NET Framework 4**.</span></span> <span data-ttu-id="37d73-341">如果您將目標 framework 變更為較早的 framework 版本時，專案還是會有較新的 framework 版本的組件的參考，並不會執行。</span><span class="sxs-lookup"><span data-stu-id="37d73-341">If you change the target framework to an earlier framework version, the project will still have references to the later framework version's assemblies and will not run.</span></span> <span data-ttu-id="37d73-342">您必須手動變更這些參考，或建立新的專案以.NET Framework 4 或更早版本為目標。</span><span class="sxs-lookup"><span data-stu-id="37d73-342">You have to manually change those references or create a new project that targets .NET Framework 4 or earlier.</span></span> <span data-ttu-id="37d73-343">如需詳細資訊，請參閱 <<c0> [ 網站的.NET Framework 目標](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx)。</span><span class="sxs-lookup"><span data-stu-id="37d73-343">For more information, see [.NET Framework Targeting for Web Sites](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).</span></span>

## <a name="medium-trust-errors"></a><span data-ttu-id="37d73-344">中度信任錯誤</span><span class="sxs-lookup"><span data-stu-id="37d73-344">Medium Trust Errors</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-345">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-345">Scenario</span></span>

<span data-ttu-id="37d73-346">當您在生產環境中執行您的應用程式時，它會取得與中度信任相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="37d73-346">When you run your application in production, it gets an error related to medium trust.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-347">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-347">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-348">許多協力廠商裝載提供者會將您的網站以中度信任，這表示它不允許執行的一些事項。</span><span class="sxs-lookup"><span data-stu-id="37d73-348">Many third-party hosting providers run your web site in medium trust, which means that there are some things it isn't allowed to do.</span></span> <span data-ttu-id="37d73-349">例如，應用程式程式碼無法存取 Windows 登錄和無法讀取或寫入您的應用程式資料夾階層之外的檔案。</span><span class="sxs-lookup"><span data-stu-id="37d73-349">For example, application code can't access the Windows registry and can't read or write files that are outside of your application's folder hierarchy.</span></span> <span data-ttu-id="37d73-350">應用程式執行的預設*完全信任*您本機電腦上，這表示應用程式可能無法執行一些作業，當您將它部署到生產環境時將會失敗。</span><span class="sxs-lookup"><span data-stu-id="37d73-350">By default your application runs in *full trust* on your local computer, which means that the application might be able to do things that would fail when you deploy it to production.</span></span>

<span data-ttu-id="37d73-351">您可以設定應用程式以在本機 IIS 環境中的中度信任執行，以進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="37d73-351">You can configure the application to run in medium trust in the local IIS environment in order to troubleshoot.</span></span> <span data-ttu-id="37d73-352">若要這樣做，請開啟 應用程式*Web.config*檔案，並新增**信任**中的項目**system.web**項目，在此範例中所示。</span><span class="sxs-lookup"><span data-stu-id="37d73-352">To do that, open the application *Web.config* file, and add a **trust** element in the **system.web** element, as shown in this example.</span></span>

[!code-xml[Main](troubleshooting/samples/sample11.xml)]

<span data-ttu-id="37d73-353">現在應用程式會在 IIS 中的中度信任上執行，即使在您的本機電腦上。</span><span class="sxs-lookup"><span data-stu-id="37d73-353">The application will now run in medium trust in IIS even on your local computer.</span></span>

<span data-ttu-id="37d73-354">沒有此資料庫，如果您要部署至 Azure App Service，因為 Azure 不需要中度信任。</span><span class="sxs-lookup"><span data-stu-id="37d73-354">Don't do this if you are deploying to Azure App Service, because Azure does not require medium trust.</span></span> <span data-ttu-id="37d73-355">在本教學課程以 2 月版，2012 年的時間，讓您的應用程式的中度信任執行使用這個方法會造成錯誤在 Azure 中。</span><span class="sxs-lookup"><span data-stu-id="37d73-355">At the time this tutorial is being written in February, 2012, using this method to make your application run in medium trust will cause an error in Azure.</span></span>

<span data-ttu-id="37d73-356">如果您使用 Entity Framework Code First 移轉，而且您要部署至主機服務提供者，在中度信任執行您的應用程式，請確定您有 5.0 版或更新的版本。</span><span class="sxs-lookup"><span data-stu-id="37d73-356">If you are using Entity Framework Code First Migrations and you are deploying to a hosting provider that runs your application in medium trust, make sure that you have version 5.0 or later installed.</span></span> <span data-ttu-id="37d73-357">在 Entity Framework 4.3 版中，移轉會需要完全信任，才能更新資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="37d73-357">In Entity Framework version 4.3, Migrations requires full trust in order to update the database schema.</span></span>

## <a name="http-40417-not-found-error"></a><span data-ttu-id="37d73-358">找不到 HTTP 404.17 錯誤</span><span class="sxs-lookup"><span data-stu-id="37d73-358">HTTP 404.17 Not Found Error</span></span>

### <a name="scenario"></a><span data-ttu-id="37d73-359">情節</span><span class="sxs-lookup"><span data-stu-id="37d73-359">Scenario</span></span>

<span data-ttu-id="37d73-360">當您在 IIS 中的開發電腦上執行已部署的站台時，您會看到下列報表伺服器無法處理 Default.aspx 的錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="37d73-360">When you run the deployed site on your development computer in IIS, you see the following error message reporting that the server can't process Default.aspx:</span></span>

<span data-ttu-id="37d73-361">找不到的 HTTP 錯誤 404.17-</span><span class="sxs-lookup"><span data-stu-id="37d73-361">HTTP Error 404.17 - Not Found</span></span>

<span data-ttu-id="37d73-362">要求的內容似乎是指令碼，並不會處理靜態檔案處理常式。</span><span class="sxs-lookup"><span data-stu-id="37d73-362">The requested content appears to be script and will not be served by the static file handler.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="37d73-363">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="37d73-363">Possible Cause and Solution</span></span>

<span data-ttu-id="37d73-364">在您的電腦上可能未安裝 ASP.NET 4.5。</span><span class="sxs-lookup"><span data-stu-id="37d73-364">ASP.NET 4.5 might not be installed on your computer.</span></span> <span data-ttu-id="37d73-365">為測試環境本系列教學課程說明如何安裝 ASP.NET 4.5，請參閱 IIS 中部署的步驟。</span><span class="sxs-lookup"><span data-stu-id="37d73-365">See the steps in the Deploying to IIS as a Test Environment tutorial in this series that explain how to install ASP.NET 4.5.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="37d73-366">上一步</span><span class="sxs-lookup"><span data-stu-id="37d73-366">Previous</span></span>](deploying-extra-files.md)
