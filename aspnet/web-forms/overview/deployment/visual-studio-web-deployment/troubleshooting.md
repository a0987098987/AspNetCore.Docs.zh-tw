---
uid: web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
title: 使用 Visual Studio 的 ASP.NET Web 部署： 疑難排解 |Microsoft 文件
author: tdykstra
description: 此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/01/2015
ms.topic: article
ms.assetid: c0090595-ab3b-4b9b-9e16-7a1891e8cb2f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 15bda09c59afaf9e5449c68c5206bb28de245541
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-troubleshooting"></a><span data-ttu-id="8e154-103">使用 Visual Studio 的 ASP.NET Web 部署： 疑難排解</span><span class="sxs-lookup"><span data-stu-id="8e154-103">ASP.NET Web Deployment using Visual Studio: Troubleshooting</span></span>
====================
<span data-ttu-id="8e154-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="8e154-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="8e154-105">下載入門專案</span><span class="sxs-lookup"><span data-stu-id="8e154-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="8e154-106">此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="8e154-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="8e154-107">數列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="8e154-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


<span data-ttu-id="8e154-108">此頁面描述，當您使用 Visual Studio 部署 ASP.NET web 應用程式時可能發生的一些常見問題。</span><span class="sxs-lookup"><span data-stu-id="8e154-108">This page describes some common problems that may arise when you deploy an ASP.NET web application by using Visual Studio.</span></span> <span data-ttu-id="8e154-109">每個都提供一個或多個可能原因和解決方案。</span><span class="sxs-lookup"><span data-stu-id="8e154-109">For each one, one or more possible causes and corresponding solutions are provided.</span></span>

<span data-ttu-id="8e154-110">顯示案例適用於 Azure 和協力廠商主機服務提供者。</span><span class="sxs-lookup"><span data-stu-id="8e154-110">The scenarios shown apply to both Azure and third-party hosting providers.</span></span> <span data-ttu-id="8e154-111">如需有關如何疑難排解在 Azure App Service web 應用程式的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="8e154-111">For more information about troubleshooting web apps in Azure App Service, see the following resources:</span></span>

- [<span data-ttu-id="8e154-112">疑難排解 Azure App Service 使用 Visual Studio 中的 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="8e154-112">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)
- [<span data-ttu-id="8e154-113">監視 Azure App Service 中的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="8e154-113">Monitor Web Apps in Azure App Service</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-monitor//)
- <span data-ttu-id="8e154-114">[發表新版的 Windows Azure SDK 2.0 for.NET](http://https://weblogs.asp.net/scottgu/announcing-the-release-of-windows-azure-sdk-2-0-for-net) （ScottGu 的部落格，會示範如何取得 Visual Studio 中的診斷記錄檔）</span><span class="sxs-lookup"><span data-stu-id="8e154-114">[Announcing the release of Windows Azure SDK 2.0 for .NET](http://https://weblogs.asp.net/scottgu/announcing-the-release-of-windows-azure-sdk-2-0-for-net) (ScottGu's blog, shows how to get diagnostic logs in Visual Studio)</span></span>

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a><span data-ttu-id="8e154-115">伺服器錯誤目前自訂錯誤設定 '/' 應用程式層中避免從遠端檢視錯誤詳細資料</span><span class="sxs-lookup"><span data-stu-id="8e154-115">Server Error in '/' Application - Current Custom Error Settings Prevent Details of the Error from Being Viewed Remotely</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-116">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-116">Scenario</span></span>

<span data-ttu-id="8e154-117">之後將網站部署至遠端主機，您會收到錯誤訊息所提及的 Web.config 檔案中的 customErrors 設定，而不會指出實際錯誤的原因是什麼：</span><span class="sxs-lookup"><span data-stu-id="8e154-117">After deploying a site to a remote host, you get an error message that mentions the customErrors setting in the Web.config file but doesn't indicate what the actual cause of the error was:</span></span>

[!code-xml[Main](troubleshooting/samples/sample1.xml)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-118">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-118">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-119">根據預設，ASP.NET 會顯示詳細的錯誤資訊，只有在本機電腦上執行您 web 應用程式時。</span><span class="sxs-lookup"><span data-stu-id="8e154-119">By default, ASP.NET shows detailed error information only when your web application is running on the local computer.</span></span> <span data-ttu-id="8e154-120">通常您不想 web 應用程式時網際網路上公開使用，因為駭客可能可以使用此資訊來尋找應用程式中的弱點可能會顯示詳細的錯誤資訊。</span><span class="sxs-lookup"><span data-stu-id="8e154-120">Generally you don't want to display detailed error information when your web application is publicly available over the Internet, because hackers may be able to use this information to find vulnerabilities in the application.</span></span> <span data-ttu-id="8e154-121">不過，當您部署至網站或更新到站台，有時東西會發生錯誤，而且您需要取得實際的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="8e154-121">However, when you are deploying a site or updates to a site, sometimes something will go wrong and you need to get the actual error message.</span></span>

<span data-ttu-id="8e154-122">若要啟用應用程式的遠端主機上執行時顯示詳細的錯誤訊息，請編輯 Web.config 檔案，以設定 customErrors 模式、 重新部署應用程式，並再次執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="8e154-122">To enable the application to display detailed error messages when it runs on the remote host, edit the Web.config file to set customErrors mode off, redeploy the application, and run the application again:</span></span>

1. <span data-ttu-id="8e154-123">如果應用程式 Web.config 檔案中 thesystem.web 項目具有 acustomErrors 項目，變更為 「 關閉 」 themode 屬性。</span><span class="sxs-lookup"><span data-stu-id="8e154-123">If the application Web.config file has acustomErrors element in thesystem.web element, change themode attribute to "off".</span></span> <span data-ttu-id="8e154-124">否則 acustomErrors 項目中新增 thesystem.web 元素 themode 屬性設定為 「 關閉 」，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="8e154-124">Otherwise add acustomErrors element in thesystem.web element with themode attribute set to "off", as shown in the following example:</span></span> 

    [!code-xml[Main](troubleshooting/samples/sample2.xml)]
2. <span data-ttu-id="8e154-125">部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="8e154-125">Deploy the application.</span></span>
3. <span data-ttu-id="8e154-126">執行應用程式，並重複任何先前般導致發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="8e154-126">Run the application and repeat whatever you did earlier that caused the error to occur.</span></span> <span data-ttu-id="8e154-127">現在您可以看到實際的錯誤訊息是什麼。</span><span class="sxs-lookup"><span data-stu-id="8e154-127">Now you can see what the actual error message is.</span></span>
4. <span data-ttu-id="8e154-128">當您解決錯誤時，還原原始的 customErrors 設定，然後重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="8e154-128">When you have resolved the error, restore the original customErrors setting and redeploy the application.</span></span>

## <a name="cannot-createshadow-copy-contosouniversity-when-that-file-already-exists"></a><span data-ttu-id="8e154-129">無法建立或陰影複製 'ContosoUniversity' 時已存在的檔案。</span><span class="sxs-lookup"><span data-stu-id="8e154-129">Cannot create/shadow copy 'ContosoUniversity' when that file already exists.</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-130">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-130">Scenario</span></span>

<span data-ttu-id="8e154-131">當您嘗試在 Visual Studio 中執行專案時您會得到類似以下範例的訊息的錯誤網頁：</span><span class="sxs-lookup"><span data-stu-id="8e154-131">When you try to run a project in Visual Studio you get an error page with a message like the following example:</span></span>

<span data-ttu-id="8e154-132">'/' 應用程式中的伺服器錯誤。</span><span class="sxs-lookup"><span data-stu-id="8e154-132">Server Error in '/' Application.</span></span> <span data-ttu-id="8e154-133">無法建立或陰影複製 'ContosoUniversity' 時已存在的檔案。</span><span class="sxs-lookup"><span data-stu-id="8e154-133">Cannot create/shadow copy 'ContosoUniversity' when that file already exists.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-134">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-134">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-135">稍候幾分鐘並重新整理瀏覽器中，或重新編譯站台並嘗試再次執行它。</span><span class="sxs-lookup"><span data-stu-id="8e154-135">Wait a minute and refresh the browser, or recompile the site and try running it again.</span></span>

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a><span data-ttu-id="8e154-136">拒絕存取網頁中使用 SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="8e154-136">Access is Denied in a Web Page that Uses SQL Server Compact</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-137">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-137">Scenario</span></span>

<span data-ttu-id="8e154-138">當您部署使用 SQL Server Compact 的網站，而且您執行已部署的站台存取資料庫中的頁面時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="8e154-138">When you deploy a site that uses SQL Server Compact and you run a page in the deployed site that accesses the database, you see the following error message:</span></span>

<span data-ttu-id="8e154-139">存取被拒絕。</span><span class="sxs-lookup"><span data-stu-id="8e154-139">Access is denied.</span></span> <span data-ttu-id="8e154-140">(來自 HRESULT 的例外狀況： 0x80070005 (E\_ACCESSDENIED))</span><span class="sxs-lookup"><span data-stu-id="8e154-140">(Exception from HRESULT: 0x80070005 (E\_ACCESSDENIED))</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-141">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-141">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-142">在伺服器上的網路服務帳戶必須能夠讀取 SQL Compact 服務中的原生二進位檔*bin\amd64*或*bin\x86*資料夾，但它沒有讀取這些資料夾的權限。</span><span class="sxs-lookup"><span data-stu-id="8e154-142">The NETWORK SERVICE account on the server needs to be able to read SQL Service Compact native binaries that are in the *bin\amd64* or *bin\x86* folder, but it does not have read permissions for those folders.</span></span> <span data-ttu-id="8e154-143">讀取網路服務的權限集*bin*資料夾中，務必要擴充到子資料夾的權限。</span><span class="sxs-lookup"><span data-stu-id="8e154-143">Set read permission for NETWORK SERVICE on the *bin* folder, making sure to extend the permissions to subfolders.</span></span>

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a><span data-ttu-id="8e154-144">無法讀取設定檔案，因為權限不足</span><span class="sxs-lookup"><span data-stu-id="8e154-144">Cannot Read Configuration File Due to Insufficient Permissions</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-145">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-145">Scenario</span></span>

<span data-ttu-id="8e154-146">當您按一下 [Visual Studio 發行應用程式部署至 IIS 在本機電腦上的按鈕發佈失敗而**輸出**] 視窗會顯示錯誤訊息如下所示：</span><span class="sxs-lookup"><span data-stu-id="8e154-146">When you click the Visual Studio publish button to deploy an application to IIS on your local machine, publishing fails and the **Output** window shows an error message similar to this:</span></span>

<span data-ttu-id="8e154-147">讀取 IIS 設定檔 'MACHINE/重新導向' 時，就會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="8e154-147">An error occurred when reading the IIS Configuration File 'MACHINE/REDIRECTION'.</span></span> <span data-ttu-id="8e154-148">執行此作業的識別是...錯誤： 無法讀取設定檔案，因為沒有足夠的權限。</span><span class="sxs-lookup"><span data-stu-id="8e154-148">The identity performing this operation was ... Error: Cannot read configuration file due to insufficient permissions.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-149">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-149">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-150">若要使用單鍵發行至 IIS 在本機電腦上，您必須以管理員權限執行 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="8e154-150">To use one-click publish to IIS on your local machine, you must be running Visual Studio with administrator permissions.</span></span> <span data-ttu-id="8e154-151">關閉 Visual Studio 並重新啟動它以管理員權限。</span><span class="sxs-lookup"><span data-stu-id="8e154-151">Close Visual Studio and restart it with administrator permissions.</span></span>

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a><span data-ttu-id="8e154-152">無法連接到目的電腦...使用指定的處理序</span><span class="sxs-lookup"><span data-stu-id="8e154-152">Could Not Connect to the Destination Computer ... Using the Specified Process</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-153">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-153">Scenario</span></span>

<span data-ttu-id="8e154-154">當您按一下 [Visual Studio 發行應用程式部署的按鈕發佈失敗而**輸出**] 視窗會顯示錯誤訊息如下所示：</span><span class="sxs-lookup"><span data-stu-id="8e154-154">When you click the Visual Studio publish button to deploy an application, publishing fails and the **Output** window shows an error message similar to this:</span></span>

[!code-console[Main](troubleshooting/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-155">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-155">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-156">Proxy 伺服器會中斷與目的地伺服器的通訊。</span><span class="sxs-lookup"><span data-stu-id="8e154-156">A proxy server is interrupting communication with the destination server.</span></span> <span data-ttu-id="8e154-157">從 Windows [控制台] 或在 Internet Explorer 中，選取**網際網路選項**選取**連線**] 索引標籤。在**網際網路內容**對話方塊中，按一下 [ **LAN 設定**。</span><span class="sxs-lookup"><span data-stu-id="8e154-157">From the Windows Control Panel or in Internet Explorer, select **Internet Options** and select the **Connections** tab. In the **Internet Properties** dialog box, click **LAN Settings**.</span></span> <span data-ttu-id="8e154-158">在**區域網路 (LAN) 設定**對話方塊中，清除**自動偵測設定**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="8e154-158">In the **Local Area Network (LAN) Settings** dialog box, clear the **Automatically detect settings** checkbox.</span></span> <span data-ttu-id="8e154-159">然後再按一下 [發行] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="8e154-159">Then click the publish button again.</span></span>

<span data-ttu-id="8e154-160">如果此問題持續發生，請連絡系統管理員，以判斷哪些可以透過 proxy 或防火牆設定。</span><span class="sxs-lookup"><span data-stu-id="8e154-160">If the problem persists, contact your system administrator to determine what can be done with proxy or firewall settings.</span></span> <span data-ttu-id="8e154-161">問題發生，因為 Web Deploy Web 管理服務部署 (8172); 使用非標準連接埠為其他連線，Web Deploy，將會使用連接埠 80。</span><span class="sxs-lookup"><span data-stu-id="8e154-161">The problem happens because Web Deploy uses a non-standard port for Web Management Service deployment (8172); for other connections, Web Deploy uses port 80.</span></span> <span data-ttu-id="8e154-162">當您部署到協力廠商主機服務提供者時，通常使用 Web 管理服務。</span><span class="sxs-lookup"><span data-stu-id="8e154-162">When you are deploying to a third-party hosting provider, you are typically using the Web Management Service.</span></span>

## <a name="default-net-40-application-pool-does-not-exist"></a><span data-ttu-id="8e154-163">預設.NET 4.0 的應用程式集區不存在</span><span class="sxs-lookup"><span data-stu-id="8e154-163">Default .NET 4.0 Application Pool Does Not Exist</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-164">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-164">Scenario</span></span>

<span data-ttu-id="8e154-165">當您部署的應用程式需要.NET Framework 4 時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="8e154-165">When you deploy an application that requires the .NET Framework 4, you see the following error message:</span></span>

<span data-ttu-id="8e154-166">預設的.NET 4.0 應用程式集區不存在或無法加入應用程式。</span><span class="sxs-lookup"><span data-stu-id="8e154-166">The default .NET 4.0 application pool does not exist or the application could not be added.</span></span> <span data-ttu-id="8e154-167">請確認 ASP.NET 4.0 安裝在這部電腦上。</span><span class="sxs-lookup"><span data-stu-id="8e154-167">Please verify that ASP.NET 4.0 is installed on this machine.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-168">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-168">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-169">在 IIS 中未安裝 ASP.NET 4。</span><span class="sxs-lookup"><span data-stu-id="8e154-169">ASP.NET 4 is not installed in IIS.</span></span> <span data-ttu-id="8e154-170">如果您要部署到伺服器，您的開發電腦且已安裝 Visual Studio 2010，ASP.NET 4 已安裝在電腦上，但可能未安裝在 IIS 中。</span><span class="sxs-lookup"><span data-stu-id="8e154-170">If the server you are deploying to is your development computer and has Visual Studio 2010 installed on it, ASP.NET 4 is installed on the computer but might not be installed in IIS.</span></span> <span data-ttu-id="8e154-171">在您要部署至伺服器上，開啟提升權限的命令提示字元並安裝 ASP.NET 4 在 IIS 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="8e154-171">On the server that you are deploying to, open an elevated command prompt and install ASP.NET 4 in IIS by running the following commands:</span></span>

[!code-console[Main](troubleshooting/samples/sample4.cmd)]

<span data-ttu-id="8e154-172">您也可能需要手動設定的預設應用程式集區的.NET Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="8e154-172">You might also need to manually set the .NET Framework version of the default application pool.</span></span> <span data-ttu-id="8e154-173">如需詳細資訊，請參閱 < 為本系列的測試環境教學課程的 iis 的部署。</span><span class="sxs-lookup"><span data-stu-id="8e154-173">For more information, see the Deploying to IIS as a Test Environment tutorial in this series.</span></span>

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a><span data-ttu-id="8e154-174">初始化字串的格式不符合索引 0 處開始的規格。</span><span class="sxs-lookup"><span data-stu-id="8e154-174">Format of the initialization string does not conform to specification starting at index 0.</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-175">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-175">Scenario</span></span>

<span data-ttu-id="8e154-176">在您部署應用程式使用一種單鍵之後發行，當您執行存取資料庫的頁面您收到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="8e154-176">After you deploy an application using one-click publish, when you run a page that accesses the database you get the following error message:</span></span>

<span data-ttu-id="8e154-177">初始化字串的格式不符合索引 0 處開始的規格。</span><span class="sxs-lookup"><span data-stu-id="8e154-177">Format of the initialization string does not conform to specification starting at index 0.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-178">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-178">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-179">開啟*Web.config*檔案中的已部署的站台和檢查連接字串值是否會以 $ 開頭 (ReplacableToken\_，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="8e154-179">Open the *Web.config* file in the deployed site and check to see whether the connection string values begin with $(ReplacableToken\_, as in the following example:</span></span>

[!code-xml[Main](troubleshooting/samples/sample5.xml)]

<span data-ttu-id="8e154-180">如果連接字串類似下列範例中，編輯專案檔，並將下列屬性加入至所有組建組態的 PropertyGroup 項目：</span><span class="sxs-lookup"><span data-stu-id="8e154-180">If the connection strings look like this example, edit the project file and add the following property to the PropertyGroup element that is for all build configurations:</span></span>

[!code-xml[Main](troubleshooting/samples/sample6.xml)]

<span data-ttu-id="8e154-181">然後重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="8e154-181">Then redeploy the application.</span></span>

## <a name="http-500-internal-server-error"></a><span data-ttu-id="8e154-182">HTTP 500 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="8e154-182">HTTP 500 Internal Server Error</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-183">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-183">Scenario</span></span>

<span data-ttu-id="8e154-184">當您執行已部署的站台時，您會看到下列錯誤訊息，不含特定資訊指出錯誤的原因：</span><span class="sxs-lookup"><span data-stu-id="8e154-184">When you run the deployed site, you see the following error message without specific information indicating the cause of the error:</span></span>

<span data-ttu-id="8e154-185">HTTP 錯誤 500-內部伺服器錯誤。</span><span class="sxs-lookup"><span data-stu-id="8e154-185">HTTP Error 500 - Internal Server Error.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-186">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-186">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-187">有許多原因會造成 500 錯誤，但如果您要遵照這些教學課程的其中一個可能原因是您將 XML 項目放在錯誤中的其中一個 Web.config 轉換檔案的位置。</span><span class="sxs-lookup"><span data-stu-id="8e154-187">There are many causes of 500 errors, but one possible cause if you are following these tutorials is that you put an XML element in the wrong place in one of the Web.config transformation files.</span></span> <span data-ttu-id="8e154-188">例如，您會收到這個錯誤如果您將插入的轉換&lt;位置&gt;項目底下&lt;system.web&gt;而不是直接在&lt;組態&gt;。</span><span class="sxs-lookup"><span data-stu-id="8e154-188">For example, you would get this error if you put the transformation that inserts a &lt;location&gt; element under &lt;system.web&gt; instead of directly under &lt;configuration&gt;.</span></span> <span data-ttu-id="8e154-189">若要確認轉換如預期般運作，您可以使用 Web.config 轉換預覽功能。</span><span class="sxs-lookup"><span data-stu-id="8e154-189">You can use the Web.config transform preview feature to verify that transformations are working as intended.</span></span> <span data-ttu-id="8e154-190">如果您發現不正確的原始轉換的方案是更正轉換檔案，然後重新部署。</span><span class="sxs-lookup"><span data-stu-id="8e154-190">The solution if you find a transform that was coded incorrectly is to correct the transformation file and redeploy.</span></span> <span data-ttu-id="8e154-191">如果錯誤不明顯，請嘗試 標記為註解轉換，然後重新部署，以查看哪一個造成 500 錯誤。</span><span class="sxs-lookup"><span data-stu-id="8e154-191">If an error isn't obvious, try commenting out transforms and redeploying to see which one is causing the 500 error.</span></span>

## <a name="http-50021-internal-server-error"></a><span data-ttu-id="8e154-192">HTTP 500.21 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="8e154-192">HTTP 500.21 Internal Server Error</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-193">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-193">Scenario</span></span>

<span data-ttu-id="8e154-194">當您執行已部署的站台時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="8e154-194">When you run the deployed site, you see the following error message:</span></span>

<span data-ttu-id="8e154-195">HTTP 錯誤 500.21-內部伺服器錯誤。</span><span class="sxs-lookup"><span data-stu-id="8e154-195">HTTP Error 500.21 - Internal Server Error.</span></span> <span data-ttu-id="8e154-196">"PageHandlerFactory 整合 」 的處理常式具有錯誤的模組"ManagedPipelineHandler 」 模組清單中。</span><span class="sxs-lookup"><span data-stu-id="8e154-196">Handler "PageHandlerFactory-Integrated" has a bad module "ManagedPipelineHandler" in its module list.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-197">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-197">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-198">網站已部署 ASP.NET 4 中，但 ASP.NET 4 未登錄於 IIS 伺服器的目標。</span><span class="sxs-lookup"><span data-stu-id="8e154-198">The site you have deployed targets ASP.NET 4, but ASP.NET 4 is not registered in IIS on the server.</span></span> <span data-ttu-id="8e154-199">在伺服器上開啟提升權限的命令提示字元並執行下列命令註冊 ASP.NET 4:</span><span class="sxs-lookup"><span data-stu-id="8e154-199">On the server open an elevated command prompt and register ASP.NET 4 by running the following commands:</span></span>

[!code-console[Main](troubleshooting/samples/sample7.cmd)]

<span data-ttu-id="8e154-200">您也可能需要手動設定的預設應用程式集區的.NET Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="8e154-200">You might also need to manually set the .NET Framework version of the default application pool.</span></span> <span data-ttu-id="8e154-201">如需詳細資訊，請參閱 < 為本系列的測試環境教學課程的 iis 的部署。</span><span class="sxs-lookup"><span data-stu-id="8e154-201">For more information, see the Deploying to IIS as a Test Environment tutorial in this series.</span></span>

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a><span data-ttu-id="8e154-202">登入失敗的應用程式中的開啟 SQL Server Express 資料庫\_資料</span><span class="sxs-lookup"><span data-stu-id="8e154-202">Login Failed Opening SQL Server Express Database in App\_Data</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-203">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-203">Scenario</span></span>

<span data-ttu-id="8e154-204">您更新*Web.config*檔案連接字串以指向做為 SQL Server Express 資料庫 *.mdf*檔案中您*應用程式\_資料*資料夾中，而且是第一個執行應用程式，您會看到下列錯誤訊息的時間：</span><span class="sxs-lookup"><span data-stu-id="8e154-204">You updated the *Web.config* file connection string to point to a SQL Server Express database as an *.mdf* file in your *App\_Data* folder, and the first time you run the application you see the following error message:</span></span>

<span data-ttu-id="8e154-205">「 System.Data.SqlClient.SqlException： 無法開啟資料庫"DatabaseName"登入時要求。</span><span class="sxs-lookup"><span data-stu-id="8e154-205">System.Data.SqlClient.SqlException: Cannot open database "DatabaseName" requested by the login.</span></span> <span data-ttu-id="8e154-206">登入失敗。</span><span class="sxs-lookup"><span data-stu-id="8e154-206">The login failed.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-207">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-207">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-208">名稱 *.mdf*檔案無法符合曾經已存在您的電腦的任何 SQL Server Express 資料庫的名稱，即使您刪除 *.mdf*先前已存在的資料庫檔案。</span><span class="sxs-lookup"><span data-stu-id="8e154-208">The name of the *.mdf* file cannot match the name of any SQL Server Express database that has ever existed on your computer, even if you deleted the *.mdf* file of the previously existing database.</span></span> <span data-ttu-id="8e154-209">變更名稱 *.mdf*從未使用過為資料庫名稱以及變更名稱的檔案*Web.config*檔案，以使用新的名稱。</span><span class="sxs-lookup"><span data-stu-id="8e154-209">Change the name of the *.mdf* file to a name that has never been used as a database name and change the *Web.config* file to use the new name.</span></span> <span data-ttu-id="8e154-210">或者，您可以使用[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)刪除先前已存在 SQL Server Express 資料庫。</span><span class="sxs-lookup"><span data-stu-id="8e154-210">As an alternative, you can use [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) to delete previously existing SQL Server Express databases.</span></span>

## <a name="model-compatibility-cannot-be-checked"></a><span data-ttu-id="8e154-211">模型的相容性無法檢查</span><span class="sxs-lookup"><span data-stu-id="8e154-211">Model Compatibility Cannot be Checked</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-212">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-212">Scenario</span></span>

<span data-ttu-id="8e154-213">您更新*Web.config*檔案連接字串以指向新的 SQL Server Express 資料庫，並執行應用程式第一次您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="8e154-213">You updated the *Web.config* file connection string to point to a new SQL Server Express database, and the first time you run the application you see the following error message:</span></span>

<span data-ttu-id="8e154-214">無法檢查模型相容，因為資料庫不包含模型中繼資料。</span><span class="sxs-lookup"><span data-stu-id="8e154-214">Model compatibility cannot be checked because the database does not contain model metadata.</span></span> <span data-ttu-id="8e154-215">請確認已新增 IncludeMetadataConvention DbModelBuilder 慣例。</span><span class="sxs-lookup"><span data-stu-id="8e154-215">Ensure that IncludeMetadataConvention has been added to the DbModelBuilder conventions.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-216">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-216">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-217">如果您將 Web.config 檔案中的資料庫名稱已用過您的電腦上的資料庫可能已存在的某些資料表之前。</span><span class="sxs-lookup"><span data-stu-id="8e154-217">If the database name you put in the Web.config file was ever used before on your computer, a database might already exist with some tables in it.</span></span> <span data-ttu-id="8e154-218">選取新的名稱，未在電腦上之前變更使用*Web.config*指向要使用這個新的資料庫名稱的檔案。</span><span class="sxs-lookup"><span data-stu-id="8e154-218">Select a new name that has not been used on your computer before and change the *Web.config* file to point to use this new database name.</span></span> <span data-ttu-id="8e154-219">或者，您可以使用[SQL Server Express 公用程式](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990)或[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)刪除現有的資料庫。</span><span class="sxs-lookup"><span data-stu-id="8e154-219">As an alternative, you can use [SQL Server Express Utility](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) or [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) to delete the existing database.</span></span>

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a><span data-ttu-id="8e154-220">當指令碼會嘗試建立使用者或角色的 SQL 錯誤</span><span class="sxs-lookup"><span data-stu-id="8e154-220">SQL Error When a Script Attempts to Create Users or Roles</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-221">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-221">Scenario</span></span>

<span data-ttu-id="8e154-222">您使用上設定的資料庫部署**封裝/發行 SQL**索引標籤上，在部署期間執行的 SQL 指令碼包括 Create User 或 Create Role 命令和指令碼執行失敗時執行這些命令。</span><span class="sxs-lookup"><span data-stu-id="8e154-222">You are using database deployment configured on the **Package/Publish SQL** tab, SQL scripts that run during deployment include Create User or Create Role commands, and script execution fails when those commands are executed.</span></span> <span data-ttu-id="8e154-223">您可能會看到更多詳細的訊息，如下所示：</span><span class="sxs-lookup"><span data-stu-id="8e154-223">You might see more detailed messages, such as the following:</span></span>

[!code-console[Main](troubleshooting/samples/sample8.cmd)]

<span data-ttu-id="8e154-224">當您完成設定資料庫部署中的，如果會發生這個錯誤**發行 Web**精靈而不是**封裝/發行 SQL**索引標籤上，建立的執行緒[組態和部署](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)論壇，而使用的方案將會加入至本疑難排解頁面。</span><span class="sxs-lookup"><span data-stu-id="8e154-224">If this error occurs when you have configured database deployment in the **Publish Web** wizard rather than the **Package/Publish SQL** tab, create a thread in the [Configuration and Deployment](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) forum, and the solution will be added to this troubleshooting page.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-225">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-225">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-226">您用來執行部署的使用者帳戶沒有建立使用者或角色的權限。</span><span class="sxs-lookup"><span data-stu-id="8e154-226">The user account you are using to perform deployment does not have permission to create users or roles.</span></span> <span data-ttu-id="8e154-227">例如，控管公司可能會指派 db\_datareader，db\_datawriter，和 db\_ddladmin 角色，它會設定您的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="8e154-227">For example, the hosting company might assign the db\_datareader, db\_datawriter, and db\_ddladmin roles to the user account that it sets up for you.</span></span> <span data-ttu-id="8e154-228">這些是足夠建立大多數的資料庫物件，但無法建立使用者或角色。</span><span class="sxs-lookup"><span data-stu-id="8e154-228">These are sufficient for creating most database objects, but not for creating users or roles.</span></span> <span data-ttu-id="8e154-229">若要避免此錯誤的方法之一是從資料庫部署中排除使用者和角色。</span><span class="sxs-lookup"><span data-stu-id="8e154-229">One way to avoid the error is by excluding users and roles from database deployment.</span></span> <span data-ttu-id="8e154-230">您可以藉由編輯資料庫的自動產生的指令碼的 PreSource 項目，使其包含下列屬性：</span><span class="sxs-lookup"><span data-stu-id="8e154-230">You can do this by editing the PreSource element for the database's automatically generated script so that it includes the following attributes:</span></span>

[!code-console[Main](troubleshooting/samples/sample9.cmd)]

<span data-ttu-id="8e154-231">如需如何編輯專案檔中的 PreSource 項目資訊，請參閱[如何： 編輯專案檔中的部署設定](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx)。</span><span class="sxs-lookup"><span data-stu-id="8e154-231">For information about how to edit the PreSource element in the project file, see [How to: Edit Deployment Settings in the Project File](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx).</span></span> <span data-ttu-id="8e154-232">如果需要會在目的地資料庫中的使用者或開發資料庫中的角色，請連絡您的主機服務提供者尋求協助。</span><span class="sxs-lookup"><span data-stu-id="8e154-232">If the users or roles in your development database need to be in the destination database, contact your hosting provider for assistance.</span></span>

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a><span data-ttu-id="8e154-233">在部署期間執行自訂指令碼時發生 SQL Server 逾時錯誤</span><span class="sxs-lookup"><span data-stu-id="8e154-233">SQL Server Timeout Error When Running Custom Scripts During Deployment</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-234">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-234">Scenario</span></span>

<span data-ttu-id="8e154-235">您已指定自訂 SQL 指令碼，在部署期間，執行，而且當 Web Deploy 執行它們，階段逾時。</span><span class="sxs-lookup"><span data-stu-id="8e154-235">You have specified custom SQL scripts to run during deployment, and when Web Deploy runs them, they time out.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-236">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-236">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-237">執行多個有不同的交易模式的指令碼可能會導致逾時錯誤。</span><span class="sxs-lookup"><span data-stu-id="8e154-237">Running multiple scripts that have different transaction modes can cause time-out errors.</span></span> <span data-ttu-id="8e154-238">根據預設，自動產生的指令碼執行在交易中，但自訂指令碼不這麼做。</span><span class="sxs-lookup"><span data-stu-id="8e154-238">By default, automatically generated scripts run in a transaction, but custom scripts do not.</span></span> <span data-ttu-id="8e154-239">如果您選取**提取資料和/或從現有的資料庫結構描述**選項**封裝/發行 SQL**索引標籤上，而且如果您新增自訂 SQL 指令碼，您必須變更一些指令碼的交易設定，讓所有指令碼會使用相同的交易設定。</span><span class="sxs-lookup"><span data-stu-id="8e154-239">If you select the **Pull data and/or schema from an existing database** option on the **Package/Publish SQL** tab, and if you add a custom SQL script, you must change transaction settings on some scripts so that all scripts use the same transaction settings.</span></span> <span data-ttu-id="8e154-240">如需詳細資訊，請參閱[How to： 部署資料庫與 Web 應用程式專案](https://msdn.microsoft.com/library/dd465343.aspx)。</span><span class="sxs-lookup"><span data-stu-id="8e154-240">For more information, see [How to: Deploy a Database With a Web Application Project](https://msdn.microsoft.com/library/dd465343.aspx).</span></span>

<span data-ttu-id="8e154-241">如果您已設定的交易，讓所有都相同，但是仍然收到這個錯誤，可能的解決方法是分別執行指令碼。</span><span class="sxs-lookup"><span data-stu-id="8e154-241">If you have configured transaction settings so that all are the same but still get this error, a possible workaround is to run the scripts separately.</span></span> <span data-ttu-id="8e154-242">在**資料庫指令碼**方格中的**封裝/發行**SQL 索引標籤上，清除**Include**造成逾時錯誤，指令碼 核取方塊，然後發行專案。</span><span class="sxs-lookup"><span data-stu-id="8e154-242">In the **Database Scripts** grid in the **Package/Publish** SQL tab, clear the **Include** check box for the script that causes the timeout error, then publish the project.</span></span> <span data-ttu-id="8e154-243">然後移回至**資料庫指令碼**方格中，選取該指令碼**Include**核取方塊，然後清除**Include**其他指令碼的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="8e154-243">Then go back into the **Database Scripts** grid, select that script's **Include** check box, and clear the **Include** check boxes for the other scripts.</span></span> <span data-ttu-id="8e154-244">然後將專案發行一次。</span><span class="sxs-lookup"><span data-stu-id="8e154-244">Then publish the project again.</span></span> <span data-ttu-id="8e154-245">此時，當您發行時，只有在選取的自訂指令碼執行。</span><span class="sxs-lookup"><span data-stu-id="8e154-245">This time when you publish, only the selected custom script runs.</span></span>

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a><span data-ttu-id="8e154-246">資料流資料的站台資訊清單尚無法使用</span><span class="sxs-lookup"><span data-stu-id="8e154-246">Stream Data of Site Manifest Is Not Yet Available</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-247">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-247">Scenario</span></span>

<span data-ttu-id="8e154-248">當您要安裝封裝，使用*deploy.cmd*檔案 t （測試） 選項時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="8e154-248">When you are installing a package using the *deploy.cmd* file with the t (test) option, you see the following error message:</span></span>

<span data-ttu-id="8e154-249">錯誤： 資料流資料的 ' sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' 尚無法使用。</span><span class="sxs-lookup"><span data-stu-id="8e154-249">Error: The stream data of 'sitemanifest/dbFullSql[@path='C:\TEMP\AdventureWorksGrant.sql']/sqlScript' is not yet available.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-250">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-250">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-251">錯誤訊息表示命令無法產生測試報表。</span><span class="sxs-lookup"><span data-stu-id="8e154-251">The error message means that the command cannot produce a test report.</span></span> <span data-ttu-id="8e154-252">不過，如果您使用的 y （實際的安裝） 選項，可能會執行命令。</span><span class="sxs-lookup"><span data-stu-id="8e154-252">However, the command might run if you use the y (actual installation) option.</span></span> <span data-ttu-id="8e154-253">訊息只會指出沒有測試模式中執行命令的問題。</span><span class="sxs-lookup"><span data-stu-id="8e154-253">The message indicates only that there is a problem with running the command in test mode.</span></span>

## <a name="this-application-requires-managedruntimeversion-v40"></a><span data-ttu-id="8e154-254">此應用程式需要 ManagedRuntimeVersion v4.0</span><span class="sxs-lookup"><span data-stu-id="8e154-254">This Application Requires ManagedRuntimeVersion v4.0</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-255">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-255">Scenario</span></span>

<span data-ttu-id="8e154-256">當您嘗試部署時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="8e154-256">When you attempt to deploy, you see the following error message:</span></span>

<span data-ttu-id="8e154-257">您嘗試使用應用程式集區已設定為 'v2.0' 的 'managedRuntimeVersion' 屬性。</span><span class="sxs-lookup"><span data-stu-id="8e154-257">The application pool that you are trying to use has the 'managedRuntimeVersion' property set to 'v2.0'.</span></span> <span data-ttu-id="8e154-258">此應用程式需要 'v4.0'。</span><span class="sxs-lookup"><span data-stu-id="8e154-258">This application requires 'v4.0'.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-259">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-259">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-260">在 IIS 中未安裝 ASP.NET 4。</span><span class="sxs-lookup"><span data-stu-id="8e154-260">ASP.NET 4 is not installed in IIS.</span></span> <span data-ttu-id="8e154-261">如果您要部署到伺服器，您的開發電腦且已安裝 Visual Studio 2010，ASP.NET 4 已安裝在電腦上，但可能未安裝在 IIS 中。</span><span class="sxs-lookup"><span data-stu-id="8e154-261">If the server you are deploying to is your development computer and has Visual Studio 2010 installed on it, ASP.NET 4 is installed on the computer but might not be installed in IIS.</span></span> <span data-ttu-id="8e154-262">在您要部署至伺服器上，開啟提升權限的命令提示字元並安裝 ASP.NET 4 在 IIS 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="8e154-262">On the server that you are deploying to, open an elevated command prompt and install ASP.NET 4 in IIS by running the following commands:</span></span>

[!code-console[Main](troubleshooting/samples/sample10.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a><span data-ttu-id="8e154-263">無法轉換 Microsoft.Web.Deployment.DeploymentProviderOptions</span><span class="sxs-lookup"><span data-stu-id="8e154-263">Unable to cast Microsoft.Web.Deployment.DeploymentProviderOptions</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-264">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-264">Scenario</span></span>

<span data-ttu-id="8e154-265">當您部署封裝時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="8e154-265">When you are deploying a package, you see the following error message:</span></span>

<span data-ttu-id="8e154-266">無法轉換型別 'Microsoft.Web.Deployment.DeploymentProviderOptions' 至 'Microsoft.Web.Deployment.DeploymentProviderOptions' 的物件。</span><span class="sxs-lookup"><span data-stu-id="8e154-266">Unable to cast object of type 'Microsoft.Web.Deployment.DeploymentProviderOptions' to 'Microsoft.Web.Deployment.DeploymentProviderOptions'.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-267">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-267">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-268">您嘗試部署從 IIS 管理員 」 中使用 Web 部署 1.1 UI，以已安裝的 Web Deploy 2.0 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="8e154-268">You are trying to deploy from IIS Manager using the Web Deploy 1.1 UI to a server that has Web Deploy 2.0 installed.</span></span> <span data-ttu-id="8e154-269">如果您使用 IIS 的遠端系統管理工具來部署的封裝，檢查匯入**新提供的功能**對話方塊中，當您建立連接。</span><span class="sxs-lookup"><span data-stu-id="8e154-269">If you are using the IIS Remote Administration Tool to deploy by importing a package, check the **New Features Available** dialog box when you establish the connection.</span></span> <span data-ttu-id="8e154-270">（此對話方塊可能只會顯示一次第一次建立連線時。</span><span class="sxs-lookup"><span data-stu-id="8e154-270">(This dialog box might only be shown once when the connection is first established.</span></span> <span data-ttu-id="8e154-271">若要清除的連線，然後再重新啟動，關閉 IIS 管理員和它再次啟動輸入 inetmgr/重設在命令提示字元。）列出的其中一項功能會**Web 部署 UI**，而且它具有低於 8 的版本號碼，您要部署到伺服器可能會有 1.1 和 2.0 版本的 Web Deploy 安裝。</span><span class="sxs-lookup"><span data-stu-id="8e154-271">To clear the connection and start over, close IIS Manager and start it up again by entering inetmgr /reset at the command prompt.) If one of the features listed is **Web Deploy UI**, and it has a version number lower than 8, the server you are deploying to might have both 1.1 and 2.0 versions of Web Deploy installed.</span></span> <span data-ttu-id="8e154-272">若要從已安裝 2.0 用戶端部署，伺服器必須只有 Web Deploy 2.0 安裝。</span><span class="sxs-lookup"><span data-stu-id="8e154-272">To deploy from a client that has 2.0 installed, the server must have only Web Deploy 2.0 installed.</span></span> <span data-ttu-id="8e154-273">您必須連絡裝載提供者來解決這個問題。</span><span class="sxs-lookup"><span data-stu-id="8e154-273">You will have to contact your hosting provider to resolve this problem.</span></span>

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a><span data-ttu-id="8e154-274">無法載入 SQL Server Compact 的原生元件</span><span class="sxs-lookup"><span data-stu-id="8e154-274">Unable to load the native components of SQL Server Compact</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-275">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-275">Scenario</span></span>

<span data-ttu-id="8e154-276">當您執行已部署的站台時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="8e154-276">When you run the deployed site, you see the following error message:</span></span>

<span data-ttu-id="8e154-277">無法載入 SQL Server Compact 版本 8482 的 ADO.NET 提供者相對應的原生元件。</span><span class="sxs-lookup"><span data-stu-id="8e154-277">Unable to load the native components of SQL Server Compact corresponding to the ADO.NET provider of version 8482.</span></span> <span data-ttu-id="8e154-278">安裝 SQL Server Compact 的正確版本。</span><span class="sxs-lookup"><span data-stu-id="8e154-278">Install the correct version of SQL Server Compact.</span></span> <span data-ttu-id="8e154-279">請參閱 KB 文章 974247 如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="8e154-279">Refer to KB article 974247 for more details.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-280">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-280">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-281">已部署的站台沒有*amd64*和*x86*及其子資料夾中這些應用程式 下的原生組件*bin*資料夾。</span><span class="sxs-lookup"><span data-stu-id="8e154-281">The deployed site does not have *amd64* and *x86* subfolders with the native assemblies in them under the application's *bin* folder.</span></span> <span data-ttu-id="8e154-282">SQL Server Compact 安裝的電腦，在原生組件位於*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*。</span><span class="sxs-lookup"><span data-stu-id="8e154-282">On a computer that has SQL Server Compact installed, the native assemblies are located in *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*.</span></span> <span data-ttu-id="8e154-283">若要取得正確的檔案到正確的資料夾，Visual Studio 專案中的最佳方式是安裝 NuGet SqlServerCompact 封裝。</span><span class="sxs-lookup"><span data-stu-id="8e154-283">The best way to get the correct files into the correct folders in a Visual Studio project is to install the NuGet SqlServerCompact package.</span></span> <span data-ttu-id="8e154-284">封裝安裝可新增建置後指令碼複製到原生組件*amd64*和*x86*。</span><span class="sxs-lookup"><span data-stu-id="8e154-284">Package installation adds a post-build script to copy the native assemblies into *amd64* and *x86*.</span></span> <span data-ttu-id="8e154-285">為了讓這些部署，不過，您必須手動將它們包含在專案。</span><span class="sxs-lookup"><span data-stu-id="8e154-285">In order for these to be deployed, however, you have to manually include them in the project.</span></span> <span data-ttu-id="8e154-286">如需詳細資訊，請參閱[部署 SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="8e154-286">For more information, see the [Deploying SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) tutorial.</span></span>

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a><span data-ttu-id="8e154-287">部署 Entity Framework Code First 應用程式後的 「 路徑不正確 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="8e154-287">"Path is not valid" error after deploying an Entity Framework Code First application</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-288">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-288">Scenario</span></span>

<span data-ttu-id="8e154-289">您部署的應用程式，例如 SQL Server Compact 的檔案中的應用程式儲存其資料庫會使用 Entity Framework Code First 移轉和 DBMS\_Data 資料夾。</span><span class="sxs-lookup"><span data-stu-id="8e154-289">You deploy an application that uses Entity Framework Code First Migrations and a DBMS such as SQL Server Compact which stores its database in a file in the App\_Data folder.</span></span> <span data-ttu-id="8e154-290">您必須設定為在您第一次部署之後建立資料庫的 Code First 移轉。</span><span class="sxs-lookup"><span data-stu-id="8e154-290">You have Code First Migrations configured to create the database after your first deployment.</span></span> <span data-ttu-id="8e154-291">當您執行應用程式會取得錯誤訊息，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="8e154-291">When you run the application you get an error message like the following example:</span></span>

<span data-ttu-id="8e154-292">路徑不是有效的。</span><span class="sxs-lookup"><span data-stu-id="8e154-292">The path is not valid.</span></span> <span data-ttu-id="8e154-293">請檢查資料庫目錄。</span><span class="sxs-lookup"><span data-stu-id="8e154-293">Check the directory for the database.</span></span> <span data-ttu-id="8e154-294">[Path = c:\inetpub\wwwroot\App\_Data\DatabaseName.sdf ]</span><span class="sxs-lookup"><span data-stu-id="8e154-294">[Path = c:\inetpub\wwwroot\App\_Data\DatabaseName.sdf ]</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-295">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-295">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-296">程式碼第一次嘗試建立資料庫，但應用程式\_資料資料夾不存在。</span><span class="sxs-lookup"><span data-stu-id="8e154-296">Code First is attempting to create the database but the App\_Data folder does not exist.</span></span> <span data-ttu-id="8e154-297">您未有任何檔案*應用程式\_資料*資料夾，當您部署，或您選取**排除應用程式\_資料**上**封裝/發行 Web**  索引標籤的 專案屬性 視窗。</span><span class="sxs-lookup"><span data-stu-id="8e154-297">Either you didn't have any files in the *App\_Data* folder when you deployed, or you selected **Exclude App\_Data** on the **Package/Publish Web** tab of the Project Properties window.</span></span> <span data-ttu-id="8e154-298">部署程序將不會在伺服器上建立資料夾，如果要複製到伺服器的資料夾中不有任何檔案。</span><span class="sxs-lookup"><span data-stu-id="8e154-298">The deployment process won't create a folder on the server if there are no files in the folder to be copied to the server.</span></span> <span data-ttu-id="8e154-299">如果您已經在資料庫中的站台設定，部署程序會刪除的檔案和*應用程式\_資料*資料夾本身如果您選取**移除目的端的其他檔案**中發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="8e154-299">If you already had the database set up in the site, the deployment process will delete the files and the *App\_Data* folder itself if you selected **Remove additional files at destination** in the publish profile.</span></span> <span data-ttu-id="8e154-300">若要解決此問題，將預留位置檔案放在.txt 檔案等*應用程式\_資料*資料夾，請確定您沒有**排除應用程式\_資料**選取，並重新部署。</span><span class="sxs-lookup"><span data-stu-id="8e154-300">To solve the problem, put a placeholder file such as a .txt file in the *App\_Data* folder, make sure you do not have **Exclude App\_Data** selected, and redeploy.</span></span>

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a><span data-ttu-id="8e154-301">「 無法使用具有已經從其基礎 RCW 分離的 COM 物件。 」</span><span class="sxs-lookup"><span data-stu-id="8e154-301">"COM object that has been separated from its underlying RCW cannot be used."</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-302">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-302">Scenario</span></span>

<span data-ttu-id="8e154-303">您已順利使用單鍵發行來部署您的應用程式啟動，而您收到這個錯誤：</span><span class="sxs-lookup"><span data-stu-id="8e154-303">You have been successfully using one-click publish to deploy your application and then you start getting this error:</span></span>

<span data-ttu-id="8e154-304">Web deployment 工作失敗。</span><span class="sxs-lookup"><span data-stu-id="8e154-304">Web deployment task failed.</span></span> <span data-ttu-id="8e154-305">(無法完成遠端代理程式的 url 要求 '<https://serverurl.com/msdeploy.axd?site=sitename>'。)</span><span class="sxs-lookup"><span data-stu-id="8e154-305">(Could not complete the request to remote agent URL '<https://serverurl.com/msdeploy.axd?site=sitename>'.)</span></span>  
 <span data-ttu-id="8e154-306">無法完成遠端代理程式的 url 要求 '<https://url/msdeploy.axd?site=sitename>'。</span><span class="sxs-lookup"><span data-stu-id="8e154-306">Could not complete the request to remote agent URL '<https://url/msdeploy.axd?site=sitename>'.</span></span>  
<span data-ttu-id="8e154-307">此要求已中止： 已取消要求。</span><span class="sxs-lookup"><span data-stu-id="8e154-307">The request was aborted: The request was canceled.</span></span>  
<span data-ttu-id="8e154-308">不能有已經從其基礎 RCW 分離的 COM 物件。</span><span class="sxs-lookup"><span data-stu-id="8e154-308">COM object that has been separated from its underlying RCW cannot be used.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-309">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-309">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-310">關閉並重新啟動 Visual Studio 通常是所有需要來解決這個錯誤。</span><span class="sxs-lookup"><span data-stu-id="8e154-310">Closing and restarting Visual Studio is usually all that is required to resolve this error.</span></span>

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a><span data-ttu-id="8e154-311">部署失敗因為使用者認證用於發行沒有 setACL 授權單位</span><span class="sxs-lookup"><span data-stu-id="8e154-311">Deployment Fails Because User Credentials Used for Publishing Don't Have setACL Authority</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-312">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-312">Scenario</span></span>

<span data-ttu-id="8e154-313">發行失敗，發生錯誤，指出您沒有授權單位設定資料夾的權限 （您要使用的使用者帳戶沒有 setACL 授權單位）。</span><span class="sxs-lookup"><span data-stu-id="8e154-313">Publishing fails with an error that indicates you don't have authority to set folder permissions (the user account you are using doesn't have setACL authority).</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-314">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-314">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-315">根據預設，Visual Studio 設定站台的根資料夾的權限上讀取和寫入權限應用程式\_Data 資料夾。</span><span class="sxs-lookup"><span data-stu-id="8e154-315">By default, Visual Studio sets read permissions on the root folder of the site and write permissions on the App\_Data folder.</span></span> <span data-ttu-id="8e154-316">如果您知道網站資料夾的預設權限是否正確，而且不需要設定，您加入停用此行為**&lt;IncludeSetACLProviderOn 目的地&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** 至發行設定檔 （以會影響單一設定檔） 或 wpp.targets 檔案 （以會影響所有設定檔）。</span><span class="sxs-lookup"><span data-stu-id="8e154-316">If you know that the default permissions on site folders are correct and do not need to be set, you disable this behavior by adding **&lt;IncludeSetACLProviderOn Destination&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** to the publish profile file (to affect a single profile) or to the wpp.targets file (to affect all profiles).</span></span> <span data-ttu-id="8e154-317">如需如何編輯這些檔案的資訊，請參閱[如何： 編輯設定檔 (.pubxml) 檔中的部署設定](https://msdn.microsoft.com/library/ff398069.aspx)。</span><span class="sxs-lookup"><span data-stu-id="8e154-317">For information about how to edit these files, see [How to: Edit Deployment Settings in Profile (.pubxml) Files](https://msdn.microsoft.com/library/ff398069.aspx).</span></span>

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a><span data-ttu-id="8e154-318">當應用程式嘗試寫入應用程式的資料夾時，存取被拒錯誤</span><span class="sxs-lookup"><span data-stu-id="8e154-318">Access Denied Errors when the Application Tries to Write to an Application Folder</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-319">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-319">Scenario</span></span>

<span data-ttu-id="8e154-320">您的應用程式錯誤時嘗試建立或編輯的檔案中的其中一個應用程式資料夾中，因為它並沒有該資料夾的寫入授權單位。</span><span class="sxs-lookup"><span data-stu-id="8e154-320">Your application errors when it tries to create or edit a file in one of the application folders, because it does not have write authority for that folder.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-321">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-321">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-322">根據預設，Visual Studio 設定站台的根資料夾的權限上讀取和寫入權限應用程式\_Data 資料夾。</span><span class="sxs-lookup"><span data-stu-id="8e154-322">By default, Visual Studio sets read permissions on the root folder of the site and write permissions on the App\_Data folder.</span></span> <span data-ttu-id="8e154-323">如果您的應用程式需要的子資料夾的寫入權限，您可以設定資料夾權限和部署到生產環境中的教學課程此數列所示設定該資料夾的權限。</span><span class="sxs-lookup"><span data-stu-id="8e154-323">If your application needs write access to a sub-folder, you can set permissions for that folder as shown in the Setting Folder Permissions and Deploying to the Production Environment tutorials in this series.</span></span> <span data-ttu-id="8e154-324">如果您的應用程式需要站台的根資料夾的寫入權限，您必須防止從根資料夾上設定唯讀存取權，藉由新增**&lt;IncludeSetACLProviderOn 目的地&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** 至發行設定檔 （以會影響單一設定檔） 或 wpp.targets 檔案 （以會影響所有設定檔）。</span><span class="sxs-lookup"><span data-stu-id="8e154-324">If your application needs write access to the root folder of the site, you have to prevent it from setting read-only access on the root folder by adding **&lt;IncludeSetACLProviderOn Destination&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** to the publish profile file (to affect a single profile) or to the wpp.targets file (to affect all profiles).</span></span> <span data-ttu-id="8e154-325">如需如何編輯這些檔案的資訊，請參閱[如何： 編輯設定檔 (.pubxml) 檔中的部署設定](https://msdn.microsoft.com/library/ff398069.aspx)。</span><span class="sxs-lookup"><span data-stu-id="8e154-325">For information about how to edit these files, see [How to: Edit Deployment Settings in Profile (.pubxml) Files](https://msdn.microsoft.com/library/ff398069.aspx).</span></span>

<a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a><span data-ttu-id="8e154-326">組態錯誤-targetFramework 屬性參考不晚於已安裝.NET Framework 版本的版本</span><span class="sxs-lookup"><span data-stu-id="8e154-326">Configuration Error - targetFramework attribute references a version that is later than the installed version of the .NET Framework</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-327">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-327">Scenario</span></span>

<span data-ttu-id="8e154-328">您已成功發佈之 web 專案的目標 ASP.NET 4.5，但您 （使用設定為 「 關閉 」 的 Web.config 檔案中的 customErrors 模式） 執行應用程式時收到下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="8e154-328">You successfully published a web project that targets ASP.NET 4.5, but when you run the application (with the customErrors mode set to "off" in the Web.config file) you get the following error:</span></span>

<span data-ttu-id="8e154-329">'targetFramework' 屬性中&lt;編譯&gt;只至目標版本 4.0 和更新版本的.NET Framework 使用 Web.config 檔案的項目 (例如，'&lt;編譯 targetFramework ="4.0"&gt;').</span><span class="sxs-lookup"><span data-stu-id="8e154-329">The 'targetFramework' attribute in the &lt;compilation&gt; element of the Web.config file is used only to target version 4.0 and later of the .NET Framework (for example, '&lt;compilation targetFramework="4.0"&gt;').</span></span> <span data-ttu-id="8e154-330">'TargetFramework' 屬性目前參考了不晚於已安裝.NET Framework 版本的版本。</span><span class="sxs-lookup"><span data-stu-id="8e154-330">The 'targetFramework' attribute currently references a version that is later than the installed version of the .NET Framework.</span></span> <span data-ttu-id="8e154-331">指定有效的目標版本的.NET Framework 中，或安裝必要的.NET Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="8e154-331">Specify a valid target version of the .NET Framework, or install the required version of the .NET Framework.</span></span>

<span data-ttu-id="8e154-332">錯誤頁面的 [來源錯誤] 方塊中，反白顯示從 Web.config 下行錯誤的原因為：</span><span class="sxs-lookup"><span data-stu-id="8e154-332">The Source Error box of the error page highlights the following line from Web.config as the cause of the error:</span></span>

<span data-ttu-id="8e154-333">&lt;compilation targetFramework="4.5" /&gt;</span><span class="sxs-lookup"><span data-stu-id="8e154-333">&lt;compilation targetFramework="4.5" /&gt;</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-334">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-334">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-335">伺服器不支援 ASP.NET 4.5。</span><span class="sxs-lookup"><span data-stu-id="8e154-335">The server does not support ASP.NET 4.5.</span></span> <span data-ttu-id="8e154-336">請連絡來判斷何時及是否可以加入 ASP.NET 4.5 支援主機服務提供者。</span><span class="sxs-lookup"><span data-stu-id="8e154-336">Contact the hosting provider to determine when and if support for ASP.NET 4.5 can be added.</span></span> <span data-ttu-id="8e154-337">如果升級伺服器不是一個選項，您必須部署 ASP.NET 4 或更早版本為目標的 web 專案改用。</span><span class="sxs-lookup"><span data-stu-id="8e154-337">If upgrading the server is not an option, you have to deploy a web project that targets ASP.NET 4 or earlier instead.</span></span>

<span data-ttu-id="8e154-338">如果您將 ASP.NET 4 或較早的 web 專案部署到相同的目的地時，選取**移除目的端的其他檔案** 核取方塊**設定** 索引標籤**發行 Web**精靈。</span><span class="sxs-lookup"><span data-stu-id="8e154-338">If you deploy an ASP.NET 4 or earlier web project to the same destination, select the **Remove additional files at destination** check box on the **Settings** tab of the **Publish Web** wizard.</span></span> <span data-ttu-id="8e154-339">如果您沒有選取**移除目的端的其他檔案**，您仍會繼續組態錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="8e154-339">If you don't select **Remove additional files at destination**, you will continue to get the Configuration Error page.</span></span>

<span data-ttu-id="8e154-340">專案**屬性**windows 包含目標 framework 下拉式清單中，但您無法解決這個問題只變更與 **.NET Framework 4.5**至 **.NET Framework 4**.</span><span class="sxs-lookup"><span data-stu-id="8e154-340">The project **Properties** windows includes a Target framework drop-down list, but you can't resolve this problem by just changing that from **.NET Framework 4.5** to **.NET Framework 4**.</span></span> <span data-ttu-id="8e154-341">如果您的目標 framework 變更為舊版 framework 時，專案還是會有較新的 framework 版本的組件的參考，並不會執行。</span><span class="sxs-lookup"><span data-stu-id="8e154-341">If you change the target framework to an earlier framework version, the project will still have references to the later framework version's assemblies and will not run.</span></span> <span data-ttu-id="8e154-342">您必須手動變更這些參考，或建立新的專案以.NET Framework 4 或更早版本為目標。</span><span class="sxs-lookup"><span data-stu-id="8e154-342">You have to manually change those references or create a new project that targets .NET Framework 4 or earlier.</span></span> <span data-ttu-id="8e154-343">如需詳細資訊，請參閱[Web sites 的.NET Framework 目標](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx)。</span><span class="sxs-lookup"><span data-stu-id="8e154-343">For more information, see [.NET Framework Targeting for Web Sites](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).</span></span>

## <a name="medium-trust-errors"></a><span data-ttu-id="8e154-344">中度信任錯誤</span><span class="sxs-lookup"><span data-stu-id="8e154-344">Medium Trust Errors</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-345">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-345">Scenario</span></span>

<span data-ttu-id="8e154-346">當您在生產環境中執行您的應用程式時，它會取得與中度信任相關的錯誤。</span><span class="sxs-lookup"><span data-stu-id="8e154-346">When you run your application in production, it gets an error related to medium trust.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-347">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-347">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-348">許多協力廠商裝載的提供者會在中度信任，這表示它不允許執行某些作業中執行您的網站。</span><span class="sxs-lookup"><span data-stu-id="8e154-348">Many third-party hosting providers run your web site in medium trust, which means that there are some things it isn't allowed to do.</span></span> <span data-ttu-id="8e154-349">例如，應用程式程式碼無法存取 Windows 登錄，無法讀取或寫入應用程式的資料夾階層之外的檔案。</span><span class="sxs-lookup"><span data-stu-id="8e154-349">For example, application code can't access the Windows registry and can't read or write files that are outside of your application's folder hierarchy.</span></span> <span data-ttu-id="8e154-350">根據預設應用程式執行的*完全信任*您本機電腦上，這表示應用程式可能可以執行一些作業，當您部署到生產環境時將會失敗。</span><span class="sxs-lookup"><span data-stu-id="8e154-350">By default your application runs in *full trust* on your local computer, which means that the application might be able to do things that would fail when you deploy it to production.</span></span>

<span data-ttu-id="8e154-351">您可以設定要在本機 IIS 環境中的中度信任執行，以進行疑難排解的應用程式。</span><span class="sxs-lookup"><span data-stu-id="8e154-351">You can configure the application to run in medium trust in the local IIS environment in order to troubleshoot.</span></span> <span data-ttu-id="8e154-352">若要這樣做，請開啟 應用程式*Web.config*檔並加入**信任**中的項目**system.web**項目，如這個範例所示。</span><span class="sxs-lookup"><span data-stu-id="8e154-352">To do that, open the application *Web.config* file, and add a **trust** element in the **system.web** element, as shown in this example.</span></span>

[!code-xml[Main](troubleshooting/samples/sample11.xml)]

<span data-ttu-id="8e154-353">在 IIS 中的中度信任現在將會執行應用程式即使在本機電腦上。</span><span class="sxs-lookup"><span data-stu-id="8e154-353">The application will now run in medium trust in IIS even on your local computer.</span></span>

<span data-ttu-id="8e154-354">沒有進行此資料庫，如果您要部署 Azure 應用程式服務，因為 Azure 不需要的中度信任。</span><span class="sxs-lookup"><span data-stu-id="8e154-354">Don't do this if you are deploying to Azure App Service, because Azure does not require medium trust.</span></span> <span data-ttu-id="8e154-355">在本教學課程是在的撰寫在 2 月版，2012 中，若要在中度信任中執行您的應用程式使用這個方法會造成錯誤在 Azure 中。</span><span class="sxs-lookup"><span data-stu-id="8e154-355">At the time this tutorial is being written in February, 2012, using this method to make your application run in medium trust will cause an error in Azure.</span></span>

<span data-ttu-id="8e154-356">如果您使用 Entity Framework Code First 移轉，而且您要部署到執行您的應用程式中的中度信任的主控提供者，請確定您有 5.0 版或更新的版本。</span><span class="sxs-lookup"><span data-stu-id="8e154-356">If you are using Entity Framework Code First Migrations and you are deploying to a hosting provider that runs your application in medium trust, make sure that you have version 5.0 or later installed.</span></span> <span data-ttu-id="8e154-357">在 Entity Framework 4.3 版中，移轉需要完全信任，才能更新資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="8e154-357">In Entity Framework version 4.3, Migrations requires full trust in order to update the database schema.</span></span>

## <a name="http-40417-not-found-error"></a><span data-ttu-id="8e154-358">找不到 HTTP 404.17 錯誤</span><span class="sxs-lookup"><span data-stu-id="8e154-358">HTTP 404.17 Not Found Error</span></span>

### <a name="scenario"></a><span data-ttu-id="8e154-359">情節</span><span class="sxs-lookup"><span data-stu-id="8e154-359">Scenario</span></span>

<span data-ttu-id="8e154-360">當您在 IIS 中的開發電腦上執行已部署的站台時，您會看到下列報表伺服器無法處理 Default.aspx 的錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="8e154-360">When you run the deployed site on your development computer in IIS, you see the following error message reporting that the server can't process Default.aspx:</span></span>

<span data-ttu-id="8e154-361">HTTP 錯誤 404.17-找不到</span><span class="sxs-lookup"><span data-stu-id="8e154-361">HTTP Error 404.17 - Not Found</span></span>

<span data-ttu-id="8e154-362">要求的內容似乎是指令碼，並不會處理由靜態檔案處理常式。</span><span class="sxs-lookup"><span data-stu-id="8e154-362">The requested content appears to be script and will not be served by the static file handler.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="8e154-363">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="8e154-363">Possible Cause and Solution</span></span>

<span data-ttu-id="8e154-364">在您的電腦上可能未安裝 ASP.NET 4.5。</span><span class="sxs-lookup"><span data-stu-id="8e154-364">ASP.NET 4.5 might not be installed on your computer.</span></span> <span data-ttu-id="8e154-365">為說明如何安裝 ASP.NET 4.5 這系列中的測試環境教學課程，請參閱 IIS 中部署的步驟。</span><span class="sxs-lookup"><span data-stu-id="8e154-365">See the steps in the Deploying to IIS as a Test Environment tutorial in this series that explain how to install ASP.NET 4.5.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8e154-366">上一步</span><span class="sxs-lookup"><span data-stu-id="8e154-366">Previous</span></span>](deploying-extra-files.md)
