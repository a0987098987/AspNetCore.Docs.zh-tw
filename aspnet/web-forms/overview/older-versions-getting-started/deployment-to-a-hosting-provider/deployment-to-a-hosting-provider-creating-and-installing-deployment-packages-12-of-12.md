---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: "使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 疑難排解 (12 / 12) |Microsoft 文件"
author: tdykstra
description: "這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫使用視覺化 Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: d8c4931a1d26af49ee61c896897fa6ddf12fccea
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a><span data-ttu-id="f2ee0-103">使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 疑難排解 (12 / 12)</span><span class="sxs-lookup"><span data-stu-id="f2ee0-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Troubleshooting (12 of 12)</span></span>
====================
<span data-ttu-id="f2ee0-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f2ee0-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="f2ee0-105">下載入門專案</span><span class="sxs-lookup"><span data-stu-id="f2ee0-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="f2ee0-106">這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 的 SQL Server Compact 資料庫。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="f2ee0-107">如果您安裝 Web 發行更新，您也可以使用 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="f2ee0-108">數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="f2ee0-109">顯示部署 Visual Studio 2012 RC 發行之後，引進的功能，示範如何將 SQL Server Compact 以外的 SQL Server 版本的部署和示範如何將部署至 Windows Azure 網站的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Windows Azure Web Sites, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


<span data-ttu-id="f2ee0-110">此頁面描述，當您使用 Visual Studio 部署 ASP.NET web 應用程式時可能發生的一些常見問題。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-110">This page describes some common problems that may arise when you deploy an ASP.NET web application by using Visual Studio.</span></span> <span data-ttu-id="f2ee0-111">每個都提供一個或多個可能原因和解決方案。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-111">For each one, one or more possible causes and corresponding solutions are provided.</span></span>

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a><span data-ttu-id="f2ee0-112">伺服器錯誤目前自訂錯誤設定 '/' 應用程式層中避免從遠端檢視錯誤詳細資料</span><span class="sxs-lookup"><span data-stu-id="f2ee0-112">Server Error in '/' Application - Current Custom Error Settings Prevent Details of the Error from Being Viewed Remotely</span></span>

### <a name="scenario"></a><span data-ttu-id="f2ee0-113">情節</span><span class="sxs-lookup"><span data-stu-id="f2ee0-113">Scenario</span></span>

<span data-ttu-id="f2ee0-114">之後將網站部署至遠端主機，您會收到錯誤訊息所提及的 Web.config 檔案中的 customErrors 設定，而不會指出實際錯誤的原因是什麼：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-114">After deploying a site to a remote host, you get an error message that mentions the customErrors setting in the Web.config file but doesn't indicate what the actual cause of the error was:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="f2ee0-115">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="f2ee0-115">Possible Cause and Solution</span></span>

<span data-ttu-id="f2ee0-116">根據預設，ASP.NET 會顯示詳細的錯誤資訊，只有在本機電腦上執行您 web 應用程式時。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-116">By default, ASP.NET shows detailed error information only when your web application is running on the local computer.</span></span> <span data-ttu-id="f2ee0-117">通常您不想 web 應用程式時網際網路上公開使用，因為駭客可能可以使用此資訊來尋找應用程式中的弱點可能會顯示詳細的錯誤資訊。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-117">Generally you don't want to display detailed error information when your web application is publicly available over the Internet, because hackers may be able to use this information to find vulnerabilities in the application.</span></span> <span data-ttu-id="f2ee0-118">不過，當您部署至網站或更新到站台，有時東西會發生錯誤，而且您需要取得實際的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-118">However, when you are deploying a site or updates to a site, sometimes something will go wrong and you need to get the actual error message.</span></span>

<span data-ttu-id="f2ee0-119">若要啟用應用程式的遠端主機上執行時顯示詳細的錯誤訊息，請編輯 Web.config 檔案，以設定`customErrors`模式關閉、 重新部署應用程式，並再次執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-119">To enable the application to display detailed error messages when it runs on the remote host, edit the Web.config file to set `customErrors` mode off, redeploy the application, and run the application again:</span></span>

1. <span data-ttu-id="f2ee0-120">如果應用程式 Web.config 檔案具有`customErrors`中的項目`system.web`項目，變更`mode`為 「 關閉 」 的屬性。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-120">If the application Web.config file has a `customErrors` element in the `system.web` element, change the `mode` attribute to "off".</span></span> <span data-ttu-id="f2ee0-121">否則，請加入`customErrors`中的項目`system.web`具有項目`mode`屬性設為 「 關閉 」，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-121">Otherwise add a `customErrors` element in the `system.web` element with the `mode` attribute set to "off", as shown in the following example:</span></span>

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. <span data-ttu-id="f2ee0-122">部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-122">Deploy the application.</span></span>
3. <span data-ttu-id="f2ee0-123">執行應用程式，並重複任何先前般導致發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-123">Run the application and repeat whatever you did earlier that caused the error to occur.</span></span> <span data-ttu-id="f2ee0-124">現在您可以看到實際的錯誤訊息是什麼。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-124">Now you can see what the actual error message is.</span></span>
4. <span data-ttu-id="f2ee0-125">當您解決錯誤時，還原原始`customErrors`設定並重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-125">When you have resolved the error, restore the original `customErrors` setting and redeploy the application.</span></span>

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a><span data-ttu-id="f2ee0-126">拒絕存取網頁中使用 SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="f2ee0-126">Access is Denied in a Web Page that Uses SQL Server Compact</span></span>

### <a name="scenario"></a><span data-ttu-id="f2ee0-127">情節</span><span class="sxs-lookup"><span data-stu-id="f2ee0-127">Scenario</span></span>

<span data-ttu-id="f2ee0-128">當您部署使用 SQL Server Compact 的網站，而且您執行已部署的站台存取資料庫中的頁面時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-128">When you deploy a site that uses SQL Server Compact and you run a page in the deployed site that accesses the database, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="f2ee0-129">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="f2ee0-129">Possible Cause and Solution</span></span>

<span data-ttu-id="f2ee0-130">在伺服器上的網路服務帳戶必須能夠讀取 SQL Compact 服務中的原生二進位檔*bin\amd64*或*bin\x86*資料夾，但它沒有讀取這些資料夾的權限。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-130">The NETWORK SERVICE account on the server needs to be able to read SQL Service Compact native binaries that are in the *bin\amd64* or *bin\x86* folder, but it does not have read permissions for those folders.</span></span> <span data-ttu-id="f2ee0-131">讀取網路服務的權限集*bin*資料夾中，務必要擴充到子資料夾的權限。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-131">Set read permission for NETWORK SERVICE on the *bin* folder, making sure to extend the permissions to subfolders.</span></span>

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a><span data-ttu-id="f2ee0-132">無法讀取設定檔案，因為權限不足</span><span class="sxs-lookup"><span data-stu-id="f2ee0-132">Cannot Read Configuration File Due to Insufficient Permissions</span></span>

### <a name="scenario"></a><span data-ttu-id="f2ee0-133">情節</span><span class="sxs-lookup"><span data-stu-id="f2ee0-133">Scenario</span></span>

<span data-ttu-id="f2ee0-134">當您按一下 [Visual Studio 發行應用程式部署至 IIS 在本機電腦上的按鈕發佈失敗而**輸出**] 視窗會顯示錯誤訊息如下所示：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-134">When you click the Visual Studio publish button to deploy an application to IIS on your local machine, publishing fails and the **Output** window shows an error message similar to this:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="f2ee0-135">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="f2ee0-135">Possible Cause and Solution</span></span>

<span data-ttu-id="f2ee0-136">若要使用單鍵發行至 IIS 在本機電腦上，您必須以管理員權限執行 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-136">To use one-click publish to IIS on your local machine, you must be running Visual Studio with administrator permissions.</span></span> <span data-ttu-id="f2ee0-137">關閉 Visual Studio 並重新啟動它以管理員權限。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-137">Close Visual Studio and restart it with administrator permissions.</span></span>

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a><span data-ttu-id="f2ee0-138">無法連接到目的電腦...使用指定的處理序</span><span class="sxs-lookup"><span data-stu-id="f2ee0-138">Could Not Connect to the Destination Computer ... Using the Specified Process</span></span>

### <a name="scenario"></a><span data-ttu-id="f2ee0-139">情節</span><span class="sxs-lookup"><span data-stu-id="f2ee0-139">Scenario</span></span>

<span data-ttu-id="f2ee0-140">當您按一下 [Visual Studio 發行應用程式部署的按鈕發佈失敗而**輸出**] 視窗會顯示錯誤訊息如下所示：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-140">When you click the Visual Studio publish button to deploy an application, publishing fails and the **Output** window shows an error message similar to this:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="f2ee0-141">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="f2ee0-141">Possible Cause and Solution</span></span>

<span data-ttu-id="f2ee0-142">Proxy 伺服器會中斷與目的地伺服器的通訊。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-142">A proxy server is interrupting communication with the destination server.</span></span> <span data-ttu-id="f2ee0-143">從 Windows [控制台] 或在 Internet Explorer 中，選取**網際網路選項**選取**連線**] 索引標籤。在**網際網路內容**對話方塊中，按一下 [ **LAN 設定**。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-143">From the Windows Control Panel or in Internet Explorer, select **Internet Options** and select the **Connections** tab. In the **Internet Properties** dialog box, click **LAN Settings**.</span></span> <span data-ttu-id="f2ee0-144">在**區域網路 (LAN) 設定**對話方塊中，清除**自動偵測設定**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-144">In the **Local Area Network (LAN) Settings** dialog box, clear the **Automatically detect settings** checkbox.</span></span> <span data-ttu-id="f2ee0-145">然後再按一下 [發行] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-145">Then click the publish button again.</span></span>

<span data-ttu-id="f2ee0-146">如果此問題持續發生，請連絡系統管理員，以判斷哪些可以透過 proxy 或防火牆設定。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-146">If the problem persists, contact your system administrator to determine what can be done with proxy or firewall settings.</span></span> <span data-ttu-id="f2ee0-147">問題發生，因為 Web Deploy Web 管理服務部署 (8172); 使用非標準連接埠為其他連線，Web Deploy，將會使用連接埠 80。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-147">The problem happens because Web Deploy uses a non-standard port for Web Management Service deployment (8172); for other connections, Web Deploy uses port 80.</span></span> <span data-ttu-id="f2ee0-148">當您部署到協力廠商主機服務提供者時，通常使用 Web 管理服務。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-148">When you are deploying to a third-party hosting provider, you are typically using the Web Management Service.</span></span>

## <a name="default-net-40-application-pool-does-not-exist"></a><span data-ttu-id="f2ee0-149">預設.NET 4.0 的應用程式集區不存在</span><span class="sxs-lookup"><span data-stu-id="f2ee0-149">Default .NET 4.0 Application Pool Does Not Exist</span></span>

### <a name="scenario"></a><span data-ttu-id="f2ee0-150">情節</span><span class="sxs-lookup"><span data-stu-id="f2ee0-150">Scenario</span></span>

<span data-ttu-id="f2ee0-151">當您部署的應用程式需要.NET Framework 4 時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-151">When you deploy an application that requires the .NET Framework 4, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="f2ee0-152">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="f2ee0-152">Possible Cause and Solution</span></span>

<span data-ttu-id="f2ee0-153">在 IIS 中未安裝 ASP.NET 4。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-153">ASP.NET 4 is not installed in IIS.</span></span> <span data-ttu-id="f2ee0-154">如果您要部署到伺服器，您的開發電腦且已安裝 Visual Studio 2010，ASP.NET 4 已安裝在電腦上，但可能未安裝在 IIS 中。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-154">If the server you are deploying to is your development computer and has Visual Studio 2010 installed on it, ASP.NET 4 is installed on the computer but might not be installed in IIS.</span></span> <span data-ttu-id="f2ee0-155">在您要部署至伺服器上，開啟提升權限的命令提示字元並安裝 ASP.NET 4 在 IIS 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-155">On the server that you are deploying to, open an elevated command prompt and install ASP.NET 4 in IIS by running the following commands:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

<span data-ttu-id="f2ee0-156">您也可能需要手動設定的預設應用程式集區的.NET Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-156">You might also need to manually set the .NET Framework version of the default application pool.</span></span> <span data-ttu-id="f2ee0-157">如需詳細資訊，請參閱[部署至 IIS 做為測試環境](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-157">For more information, see the [Deploying to IIS as a Test Environment](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial.</span></span>

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a><span data-ttu-id="f2ee0-158">初始化字串的格式不符合索引 0 處開始的規格。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-158">Format of the initialization string does not conform to specification starting at index 0.</span></span>

### <a name="scenario"></a><span data-ttu-id="f2ee0-159">情節</span><span class="sxs-lookup"><span data-stu-id="f2ee0-159">Scenario</span></span>

<span data-ttu-id="f2ee0-160">在您部署應用程式使用一種單鍵之後發行，當您執行存取資料庫的頁面您收到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-160">After you deploy an application using one-click publish, when you run a page that accesses the database you get the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="f2ee0-161">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="f2ee0-161">Possible Cause and Solution</span></span>

<span data-ttu-id="f2ee0-162">開啟*Web.config*檔案中的已部署的站台和檢查連接字串值是否以開頭`$(ReplacableToken_`，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-162">Open the *Web.config* file in the deployed site and check to see whether the connection string values begin with `$(ReplacableToken_`, as in the following example:</span></span>

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

<span data-ttu-id="f2ee0-163">如果連接字串類似下列範例中，編輯專案檔，並加入下列屬性加入`PropertyGroup`的所有組建組態的項目：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-163">If the connection strings look like this example, edit the project file and add the following property to the `PropertyGroup` element that is for all build configurations:</span></span>

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

<span data-ttu-id="f2ee0-164">然後重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-164">Then redeploy the application.</span></span>

## <a name="http-500-internal-server-error"></a><span data-ttu-id="f2ee0-165">HTTP 500 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="f2ee0-165">HTTP 500 Internal Server Error</span></span>

### <a name="scenario"></a><span data-ttu-id="f2ee0-166">情節</span><span class="sxs-lookup"><span data-stu-id="f2ee0-166">Scenario</span></span>

<span data-ttu-id="f2ee0-167">當您執行已部署的站台時，您會看到下列錯誤訊息，不含特定資訊指出錯誤的原因：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-167">When you run the deployed site, you see the following error message without specific information indicating the cause of the error:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="f2ee0-168">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="f2ee0-168">Possible Cause and Solution</span></span>

<span data-ttu-id="f2ee0-169">有許多原因會造成 500 錯誤，但如果您要遵照這些教學課程的其中一個可能原因是您將 XML 項目放在錯誤中的其中一個 XML 轉換檔案的位置。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-169">There are many causes of 500 errors, but one possible cause if you are following these tutorials is that you put an XML element in the wrong place in one of the XML transformation files.</span></span> <span data-ttu-id="f2ee0-170">例如，您會收到這個錯誤如果您將插入的轉換`<location>`項目底下`<system.web>`而不是直接在`<configuration>`。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-170">For example, you would get this error if you put the transformation that inserts a `<location>` element under `<system.web>` instead of directly under `<configuration>`.</span></span> <span data-ttu-id="f2ee0-171">方案在此情況下是更正 XML 轉換檔案，然後重新部署。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-171">The solution in that case is to correct the XML transformation file and redeploy.</span></span>

## <a name="http-50021-internal-server-error"></a><span data-ttu-id="f2ee0-172">HTTP 500.21 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="f2ee0-172">HTTP 500.21 Internal Server Error</span></span>

### <a name="scenario"></a><span data-ttu-id="f2ee0-173">情節</span><span class="sxs-lookup"><span data-stu-id="f2ee0-173">Scenario</span></span>

<span data-ttu-id="f2ee0-174">當您執行已部署的站台時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-174">When you run the deployed site, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="f2ee0-175">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="f2ee0-175">Possible Cause and Solution</span></span>

<span data-ttu-id="f2ee0-176">網站已部署 ASP.NET 4 中，但 ASP.NET 4 未登錄於 IIS 伺服器的目標。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-176">The site you have deployed targets ASP.NET 4, but ASP.NET 4 is not registered in IIS on the server.</span></span> <span data-ttu-id="f2ee0-177">在伺服器上開啟提升權限的命令提示字元並執行下列命令註冊 ASP.NET 4:</span><span class="sxs-lookup"><span data-stu-id="f2ee0-177">On the server open an elevated command prompt and register ASP.NET 4 by running the following commands:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

<span data-ttu-id="f2ee0-178">您也可能需要手動設定的預設應用程式集區的.NET Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-178">You might also need to manually set the .NET Framework version of the default application pool.</span></span> <span data-ttu-id="f2ee0-179">如需詳細資訊，請參閱[部署至 IIS 做為測試環境](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-179">For more information, see the [Deploying to IIS as a Test Environment](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial.</span></span>

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a><span data-ttu-id="f2ee0-180">登入失敗的應用程式中的開啟 SQL Server Express 資料庫\_資料</span><span class="sxs-lookup"><span data-stu-id="f2ee0-180">Login Failed Opening SQL Server Express Database in App\_Data</span></span>

### <a name="scenario"></a><span data-ttu-id="f2ee0-181">情節</span><span class="sxs-lookup"><span data-stu-id="f2ee0-181">Scenario</span></span>

<span data-ttu-id="f2ee0-182">您更新*Web.config*檔案連接字串以指向做為 SQL Server Express 資料庫*.mdf*檔案中您*應用程式\_資料*資料夾中，而且是第一個執行應用程式，您會看到下列錯誤訊息的時間：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-182">You updated the *Web.config* file connection string to point to a SQL Server Express database as an *.mdf* file in your *App\_Data* folder, and the first time you run the application you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="f2ee0-183">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="f2ee0-183">Possible Cause and Solution</span></span>

<span data-ttu-id="f2ee0-184">名稱*.mdf*檔案無法符合曾經已存在您的電腦的任何 SQL Server Express 資料庫的名稱，即使您刪除*.mdf*先前已存在的資料庫檔案。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-184">The name of the *.mdf* file cannot match the name of any SQL Server Express database that has ever existed on your computer, even if you deleted the *.mdf* file of the previously existing database.</span></span> <span data-ttu-id="f2ee0-185">變更名稱*.mdf*從未使用過為資料庫名稱以及變更名稱的檔案*Web.config*檔案，以使用新的名稱。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-185">Change the name of the *.mdf* file to a name that has never been used as a database name and change the *Web.config* file to use the new name.</span></span> <span data-ttu-id="f2ee0-186">或者，您可以使用[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)刪除先前已存在 SQL Server Express 資料庫。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-186">As an alternative, you can use [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) to delete previously existing SQL Server Express databases.</span></span>

## <a name="model-compatibility-cannot-be-checked"></a><span data-ttu-id="f2ee0-187">模型的相容性無法檢查</span><span class="sxs-lookup"><span data-stu-id="f2ee0-187">Model Compatibility Cannot be Checked</span></span>

### <a name="scenario"></a><span data-ttu-id="f2ee0-188">情節</span><span class="sxs-lookup"><span data-stu-id="f2ee0-188">Scenario</span></span>

<span data-ttu-id="f2ee0-189">您更新*Web.config*檔案連接字串以指向新的 SQL Server Express 資料庫，並執行應用程式第一次您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-189">You updated the *Web.config* file connection string to point to a new SQL Server Express database, and the first time you run the application you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="f2ee0-190">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="f2ee0-190">Possible Cause and Solution</span></span>

<span data-ttu-id="f2ee0-191">如果您將 Web.config 檔案中的資料庫名稱已用過您的電腦上的資料庫可能已存在的某些資料表之前。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-191">If the database name you put in the Web.config file was ever used before on your computer, a database might already exist with some tables in it.</span></span> <span data-ttu-id="f2ee0-192">選取新的名稱，未在電腦上之前變更使用*Web.config*指向要使用這個新的資料庫名稱的檔案。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-192">Select a new name that has not been used on your computer before and change the *Web.config* file to point to use this new database name.</span></span> <span data-ttu-id="f2ee0-193">或者，您可以使用[SQL Server Express 公用程式](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990)或[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)刪除現有的資料庫。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-193">As an alternative, you can use [SQL Server Express Utility](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) or [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) to delete the existing database.</span></span>

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a><span data-ttu-id="f2ee0-194">當指令碼會嘗試建立使用者或角色的 SQL 錯誤</span><span class="sxs-lookup"><span data-stu-id="f2ee0-194">SQL Error When a Script Attempts to Create Users or Roles</span></span>

### <a name="scenario"></a><span data-ttu-id="f2ee0-195">情節</span><span class="sxs-lookup"><span data-stu-id="f2ee0-195">Scenario</span></span>

<span data-ttu-id="f2ee0-196">您使用上設定的資料庫部署**封裝/發行 SQL**索引標籤上，在部署期間執行的 SQL 指令碼包括 Create User 或 Create Role 命令和指令碼執行失敗時執行這些命令。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-196">You are using database deployment configured on the **Package/Publish SQL** tab, SQL scripts that run during deployment include Create User or Create Role commands, and script execution fails when those commands are executed.</span></span> <span data-ttu-id="f2ee0-197">您可能會看到更多詳細的訊息，如下所示：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-197">You might see more detailed messages, such as the following:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

<span data-ttu-id="f2ee0-198">當您完成設定資料庫部署中的，如果會發生這個錯誤**發行 Web**精靈而不是**封裝/發行 SQL**索引標籤上，建立的執行緒[組態和部署](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)論壇，而使用的方案將會加入至本疑難排解頁面。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-198">If this error occurs when you have configured database deployment in the **Publish Web** wizard rather than the **Package/Publish SQL** tab, create a thread in the [Configuration and Deployment](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) forum, and the solution will be added to this troubleshooting page.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="f2ee0-199">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="f2ee0-199">Possible Cause and Solution</span></span>

<span data-ttu-id="f2ee0-200">您用來執行部署的使用者帳戶沒有建立使用者或角色的權限。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-200">The user account you are using to perform deployment does not have permission to create users or roles.</span></span> <span data-ttu-id="f2ee0-201">比方說，控管公司可能會指派`db_datareader`， `db_datawriter`，和`db_ddladmin`它設定為您的使用者帳戶的角色。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-201">For example, the hosting company might assign the `db_datareader`, `db_datawriter`, and `db_ddladmin` roles to the user account that it sets up for you.</span></span> <span data-ttu-id="f2ee0-202">這些是足夠建立大多數的資料庫物件，但無法建立使用者或角色。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-202">These are sufficient for creating most database objects, but not for creating users or roles.</span></span> <span data-ttu-id="f2ee0-203">若要避免此錯誤的方法之一是從資料庫部署中排除使用者和角色。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-203">One way to avoid the error is by excluding users and roles from database deployment.</span></span> <span data-ttu-id="f2ee0-204">您可以藉由編輯`PreSource`項目為資料庫自動產生的指令碼，使其包含下列屬性：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-204">You can do this by editing the `PreSource` element for the database's automatically generated script so that it includes the following attributes:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

<span data-ttu-id="f2ee0-205">如需有關如何編輯資訊`PreSource`項目在專案檔中，請參閱[如何： 編輯專案檔中的部署設定](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx)。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-205">For information about how to edit the `PreSource` element in the project file, see [How to: Edit Deployment Settings in the Project File](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx).</span></span> <span data-ttu-id="f2ee0-206">如果需要會在目的地資料庫中的使用者或開發資料庫中的角色，請連絡您的主機服務提供者尋求協助。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-206">If the users or roles in your development database need to be in the destination database, contact your hosting provider for assistance.</span></span>

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a><span data-ttu-id="f2ee0-207">在部署期間執行自訂指令碼時發生 SQL Server 逾時錯誤</span><span class="sxs-lookup"><span data-stu-id="f2ee0-207">SQL Server Timeout Error When Running Custom Scripts During Deployment</span></span>

### <a name="scenario"></a><span data-ttu-id="f2ee0-208">情節</span><span class="sxs-lookup"><span data-stu-id="f2ee0-208">Scenario</span></span>

<span data-ttu-id="f2ee0-209">您已指定自訂 SQL 指令碼，在部署期間，執行，而且當 Web Deploy 執行它們，階段逾時。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-209">You have specified custom SQL scripts to run during deployment, and when Web Deploy runs them, they time out.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="f2ee0-210">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="f2ee0-210">Possible Cause and Solution</span></span>

<span data-ttu-id="f2ee0-211">執行多個有不同的交易模式的指令碼可能會導致逾時錯誤。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-211">Running multiple scripts that have different transaction modes can cause time-out errors.</span></span> <span data-ttu-id="f2ee0-212">根據預設，自動產生的指令碼執行在交易中，但自訂指令碼不這麼做。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-212">By default, automatically generated scripts run in a transaction, but custom scripts do not.</span></span> <span data-ttu-id="f2ee0-213">如果您選取**提取資料和/或從現有的資料庫結構描述**選項**封裝/發行 SQL**索引標籤上，而且如果您新增自訂 SQL 指令碼，您必須變更一些指令碼的交易設定，讓所有指令碼會使用相同的交易設定。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-213">If you select the **Pull data and/or schema from an existing database** option on the **Package/Publish SQL** tab, and if you add a custom SQL script, you must change transaction settings on some scripts so that all scripts use the same transaction settings.</span></span> <span data-ttu-id="f2ee0-214">如需詳細資訊，請參閱[How to： 部署資料庫與 Web 應用程式專案](https://msdn.microsoft.com/library/dd465343.aspx)。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-214">For more information, see [How to: Deploy a Database With a Web Application Project](https://msdn.microsoft.com/library/dd465343.aspx).</span></span>

<span data-ttu-id="f2ee0-215">如果您已設定的交易，讓所有都相同，但是仍然收到這個錯誤，可能的解決方法是分別執行指令碼。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-215">If you have configured transaction settings so that all are the same but still get this error, a possible workaround is to run the scripts separately.</span></span> <span data-ttu-id="f2ee0-216">在**資料庫指令碼**方格中的**封裝/發行**SQL 索引標籤上，清除**Include**造成逾時錯誤，指令碼 核取方塊，然後發行專案。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-216">In the **Database Scripts** grid in the **Package/Publish** SQL tab, clear the **Include** check box for the script that causes the timeout error, then publish the project.</span></span> <span data-ttu-id="f2ee0-217">然後移回至**資料庫指令碼**方格中，選取該指令碼**Include**核取方塊，然後清除**Include**其他指令碼的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-217">Then go back into the **Database Scripts** grid, select that script's **Include** check box, and clear the **Include** check boxes for the other scripts.</span></span> <span data-ttu-id="f2ee0-218">然後將專案發行一次。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-218">Then publish the project again.</span></span> <span data-ttu-id="f2ee0-219">此時，當您發行時，只有在選取的自訂指令碼執行。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-219">This time when you publish, only the selected custom script runs.</span></span>

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a><span data-ttu-id="f2ee0-220">資料流資料的站台資訊清單尚無法使用</span><span class="sxs-lookup"><span data-stu-id="f2ee0-220">Stream Data of Site Manifest Is Not Yet Available</span></span>

### <a name="scenario"></a><span data-ttu-id="f2ee0-221">情節</span><span class="sxs-lookup"><span data-stu-id="f2ee0-221">Scenario</span></span>

<span data-ttu-id="f2ee0-222">當您要安裝封裝，使用*deploy.cmd*檔案搭配`t`（測試） 選項，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-222">When you are installing a package using the *deploy.cmd* file with the `t` (test) option, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="f2ee0-223">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="f2ee0-223">Possible Cause and Solution</span></span>

<span data-ttu-id="f2ee0-224">錯誤訊息表示命令無法產生測試報表。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-224">The error message means that the command cannot produce a test report.</span></span> <span data-ttu-id="f2ee0-225">不過，命令可能會執行如果您使用`y`（實際的安裝） 選項。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-225">However, the command might run if you use the `y` (actual installation) option.</span></span> <span data-ttu-id="f2ee0-226">訊息只會指出沒有測試模式中執行命令的問題。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-226">The message indicates only that there is a problem with running the command in test mode.</span></span>

## <a name="this-application-requires-managedruntimeversion-v40"></a><span data-ttu-id="f2ee0-227">此應用程式需要 ManagedRuntimeVersion v4.0</span><span class="sxs-lookup"><span data-stu-id="f2ee0-227">This Application Requires ManagedRuntimeVersion v4.0</span></span>

### <a name="scenario"></a><span data-ttu-id="f2ee0-228">情節</span><span class="sxs-lookup"><span data-stu-id="f2ee0-228">Scenario</span></span>

<span data-ttu-id="f2ee0-229">當您嘗試部署時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-229">When you attempt to deploy, you see the following error message:</span></span>

 <span data-ttu-id="f2ee0-230">錯誤： 資料流資料的 ' sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' 尚無法使用。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-230">Error: The stream data of 'sitemanifest/dbFullSql[@path='C:\TEMP\AdventureWorksGrant.sql']/sqlScript' is not yet available.</span></span> <span data-ttu-id="f2ee0-231">您嘗試使用應用程式集區已設定為 'v2.0' 的 'managedRuntimeVersion' 屬性。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-231">The application pool that you are trying to use has the 'managedRuntimeVersion' property set to 'v2.0'.</span></span> <span data-ttu-id="f2ee0-232">此應用程式需要 'v4.0'。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-232">This application requires 'v4.0'.</span></span> 

### <a name="possible-cause-and-solution"></a><span data-ttu-id="f2ee0-233">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="f2ee0-233">Possible Cause and Solution</span></span>

<span data-ttu-id="f2ee0-234">在 IIS 中未安裝 ASP.NET 4。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-234">ASP.NET 4 is not installed in IIS.</span></span> <span data-ttu-id="f2ee0-235">如果您要部署到伺服器，您的開發電腦且已安裝 Visual Studio 2010，ASP.NET 4 已安裝在電腦上，但可能未安裝在 IIS 中。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-235">If the server you are deploying to is your development computer and has Visual Studio 2010 installed on it, ASP.NET 4 is installed on the computer but might not be installed in IIS.</span></span> <span data-ttu-id="f2ee0-236">在您要部署至伺服器上，開啟提升權限的命令提示字元並安裝 ASP.NET 4 在 IIS 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-236">On the server that you are deploying to, open an elevated command prompt and install ASP.NET 4 in IIS by running the following commands:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a><span data-ttu-id="f2ee0-237">無法轉換 Microsoft.Web.Deployment.DeploymentProviderOptions</span><span class="sxs-lookup"><span data-stu-id="f2ee0-237">Unable to cast Microsoft.Web.Deployment.DeploymentProviderOptions</span></span>

### <a name="scenario"></a><span data-ttu-id="f2ee0-238">情節</span><span class="sxs-lookup"><span data-stu-id="f2ee0-238">Scenario</span></span>

<span data-ttu-id="f2ee0-239">當您部署封裝時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-239">When you are deploying a package, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="f2ee0-240">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="f2ee0-240">Possible Cause and Solution</span></span>

<span data-ttu-id="f2ee0-241">您嘗試部署從 IIS 管理員 」 中使用 Web 部署 1.1 UI，以已安裝的 Web Deploy 2.0 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-241">You are trying to deploy from IIS Manager using the Web Deploy 1.1 UI to a server that has Web Deploy 2.0 installed.</span></span> <span data-ttu-id="f2ee0-242">如果您使用 IIS 的遠端系統管理工具來部署的封裝，檢查匯入**新提供的功能**對話方塊中，當您建立連接。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-242">If you are using the IIS Remote Administration Tool to deploy by importing a package, check the **New Features Available** dialog box when you establish the connection.</span></span> <span data-ttu-id="f2ee0-243">（此對話方塊可能只會顯示一次第一次建立連線時。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-243">(This dialog box might only be shown once when the connection is first established.</span></span> <span data-ttu-id="f2ee0-244">若要清除連接並重新開始，關閉 IIS 管理員和它再次啟動輸入`inetmgr /reset`在命令提示字元。)列出的其中一項功能會**Web 部署 UI**，而且它具有低於 8 的版本號碼，您要部署到伺服器可能會有 1.1 和 2.0 版本的 Web Deploy 安裝。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-244">To clear the connection and start over, close IIS Manager and start it up again by entering `inetmgr /reset` at the command prompt.) If one of the features listed is **Web Deploy UI**, and it has a version number lower than 8, the server you are deploying to might have both 1.1 and 2.0 versions of Web Deploy installed.</span></span> <span data-ttu-id="f2ee0-245">若要從已安裝 2.0 用戶端部署，伺服器必須只有 Web Deploy 2.0 安裝。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-245">To deploy from a client that has 2.0 installed, the server must have only Web Deploy 2.0 installed.</span></span> <span data-ttu-id="f2ee0-246">您必須連絡裝載提供者來解決這個問題。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-246">You will have to contact your hosting provider to resolve this problem.</span></span>

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a><span data-ttu-id="f2ee0-247">無法載入 SQL Server Compact 的原生元件</span><span class="sxs-lookup"><span data-stu-id="f2ee0-247">Unable to load the native components of SQL Server Compact</span></span>

### <a name="scenario"></a><span data-ttu-id="f2ee0-248">情節</span><span class="sxs-lookup"><span data-stu-id="f2ee0-248">Scenario</span></span>

<span data-ttu-id="f2ee0-249">當您執行已部署的站台時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-249">When you run the deployed site, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="f2ee0-250">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="f2ee0-250">Possible Cause and Solution</span></span>

<span data-ttu-id="f2ee0-251">已部署的站台沒有*amd64*和*x86*及其子資料夾中這些應用程式 下的原生組件*bin*資料夾。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-251">The deployed site does not have *amd64* and *x86* subfolders with the native assemblies in them under the application's *bin* folder.</span></span> <span data-ttu-id="f2ee0-252">SQL Server Compact 安裝的電腦，在原生組件位於*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-252">On a computer that has SQL Server Compact installed, the native assemblies are located in *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*.</span></span> <span data-ttu-id="f2ee0-253">若要取得正確的檔案到正確的資料夾，Visual Studio 專案中的最佳方式是安裝 NuGet SqlServerCompact 封裝。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-253">The best way to get the correct files into the correct folders in a Visual Studio project is to install the NuGet SqlServerCompact package.</span></span> <span data-ttu-id="f2ee0-254">封裝安裝可新增建置後指令碼複製到原生組件*amd64*和*x86*。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-254">Package installation adds a post-build script to copy the native assemblies into *amd64* and *x86*.</span></span> <span data-ttu-id="f2ee0-255">為了讓這些部署，不過，您必須手動將它們包含在專案。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-255">In order for these to be deployed, however, you have to manually include them in the project.</span></span> <span data-ttu-id="f2ee0-256">如需詳細資訊，請參閱[部署 SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-256">For more information, see the [Deploying SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) tutorial.</span></span>

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a><span data-ttu-id="f2ee0-257">部署 Entity Framework Code First 應用程式後的 「 路徑不正確 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="f2ee0-257">"Path is not valid" error after deploying an Entity Framework Code First application</span></span>

### <a name="scenario"></a><span data-ttu-id="f2ee0-258">情節</span><span class="sxs-lookup"><span data-stu-id="f2ee0-258">Scenario</span></span>

<span data-ttu-id="f2ee0-259">您部署的應用程式，例如 SQL Server Compact 的檔案中的應用程式儲存其資料庫會使用 Entity Framework Code First 移轉和 DBMS\_Data 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-259">You deploy an application that uses Entity Framework Code First Migrations and a DBMS such as SQL Server Compact which stores its database in a file in the App\_Data folder.</span></span> <span data-ttu-id="f2ee0-260">您必須設定為在您第一次部署之後建立資料庫的 Code First 移轉。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-260">You have Code First Migrations configured to create the database after your first deployment.</span></span> <span data-ttu-id="f2ee0-261">當您執行應用程式會取得錯誤訊息，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-261">When you run the application you get an error message like the following example:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="f2ee0-262">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="f2ee0-262">Possible Cause and Solution</span></span>

<span data-ttu-id="f2ee0-263">程式碼第一次嘗試建立資料庫，但應用程式\_資料資料夾不存在。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-263">Code First is attempting to create the database but the App\_Data folder does not exist.</span></span> <span data-ttu-id="f2ee0-264">您未有任何檔案*應用程式\_資料*資料夾，當您部署，或您選取**排除應用程式\_資料**上**封裝/發行 Web**  索引標籤**專案屬性**視窗。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-264">Either you didn't have any files in the *App\_Data* folder when you deployed, or you selected **Exclude App\_Data** on the **Package/Publish Web** tab of the **Project Properties** window.</span></span> <span data-ttu-id="f2ee0-265">部署程序將不會在伺服器上建立資料夾，如果要複製到伺服器的資料夾中不有任何檔案。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-265">The deployment process won't create a folder on the server if there are no files in the folder to be copied to the server.</span></span> <span data-ttu-id="f2ee0-266">如果您已經在資料庫中的站台設定，部署程序會刪除的檔案和*應用程式\_資料*資料夾本身如果您選取**移除目的端的其他檔案**中發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-266">If you already had the database set up in the site, the deployment process will delete the files and the *App\_Data* folder itself if you selected **Remove additional files at destination** in the publish profile.</span></span> <span data-ttu-id="f2ee0-267">若要解決此問題，將預留位置檔案放在.txt 檔案等*應用程式\_資料*資料夾，請確定您沒有**排除應用程式\_資料**選取，並重新部署。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-267">To solve the problem, put a placeholder file such as a .txt file in the *App\_Data* folder, make sure you do not have **Exclude App\_Data** selected, and redeploy.</span></span> 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a><span data-ttu-id="f2ee0-268">「 無法使用具有已經從其基礎 RCW 分離的 COM 物件。 」</span><span class="sxs-lookup"><span data-stu-id="f2ee0-268">"COM object that has been separated from its underlying RCW cannot be used."</span></span>

### <a name="scenario"></a><span data-ttu-id="f2ee0-269">情節</span><span class="sxs-lookup"><span data-stu-id="f2ee0-269">Scenario</span></span>

<span data-ttu-id="f2ee0-270">您已順利使用單鍵發行來部署您的應用程式啟動，而您收到這個錯誤：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-270">You have been successfully using one-click publish to deploy your application and then you start getting this error:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="f2ee0-271">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="f2ee0-271">Possible Cause and Solution</span></span>

<span data-ttu-id="f2ee0-272">關閉並重新啟動 Visual Studio 通常是所有需要來解決這個錯誤。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-272">Closing and restarting Visual Studio is usually all that is required to resolve this error.</span></span>

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a><span data-ttu-id="f2ee0-273">部署失敗因為使用者認證用於發行沒有 setACL 授權單位</span><span class="sxs-lookup"><span data-stu-id="f2ee0-273">Deployment Fails Because User Credentials Used for Publishing Don't Have setACL Authority</span></span>

### <a name="scenario"></a><span data-ttu-id="f2ee0-274">情節</span><span class="sxs-lookup"><span data-stu-id="f2ee0-274">Scenario</span></span>

<span data-ttu-id="f2ee0-275">發行失敗，發生錯誤，指出您沒有授權單位設定資料夾的權限 （您要使用的使用者帳戶沒有 setACL 授權單位）。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-275">Publishing fails with an error that indicates you don't have authority to set folder permissions (the user account you are using doesn't have setACL authority).</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="f2ee0-276">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="f2ee0-276">Possible Cause and Solution</span></span>

<span data-ttu-id="f2ee0-277">根據預設，Visual Studio 設定站台的根資料夾的權限上讀取和寫入權限應用程式\_Data 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-277">By default, Visual Studio sets read permissions on the root folder of the site and write permissions on the App\_Data folder.</span></span> <span data-ttu-id="f2ee0-278">如果您知道網站資料夾的預設權限是否正確，而且不需要設定，您加入停用此行為 **&lt;IncludeSetACLProviderOn 目的地&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** 至發行設定檔 （以會影響單一設定檔） 或 wpp.targets 檔案 （以會影響所有設定檔）。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-278">If you know that the default permissions on site folders are correct and do not need to be set, you disable this behavior by adding **&lt;IncludeSetACLProviderOn Destination&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** to the publish profile file (to affect a single profile) or to the wpp.targets file (to affect all profiles).</span></span> <span data-ttu-id="f2ee0-279">如需如何編輯這些檔案的資訊，請參閱[如何： 編輯設定檔 (.pubxml) 檔中的部署設定](https://msdn.microsoft.com/library/ff398069.aspx)。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-279">For information about how to edit these files, see [How to: Edit Deployment Settings in Profile (.pubxml) Files](https://msdn.microsoft.com/library/ff398069.aspx).</span></span> 

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a><span data-ttu-id="f2ee0-280">當應用程式嘗試寫入應用程式的資料夾時，存取被拒錯誤</span><span class="sxs-lookup"><span data-stu-id="f2ee0-280">Access Denied Errors when the Application Tries to Write to an Application Folder</span></span>

### <a name="scenario"></a><span data-ttu-id="f2ee0-281">情節</span><span class="sxs-lookup"><span data-stu-id="f2ee0-281">Scenario</span></span>

<span data-ttu-id="f2ee0-282">您的應用程式錯誤時嘗試建立或編輯的檔案中的其中一個應用程式資料夾中，因為它並沒有該資料夾的寫入授權單位。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-282">Your application errors when it tries to create or edit a file in one of the application folders, because it does not have write authority for that folder.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="f2ee0-283">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="f2ee0-283">Possible Cause and Solution</span></span>

<span data-ttu-id="f2ee0-284">根據預設，Visual Studio 設定站台的根資料夾的權限上讀取和寫入權限應用程式\_Data 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-284">By default, Visual Studio sets read permissions on the root folder of the site and write permissions on the App\_Data folder.</span></span> <span data-ttu-id="f2ee0-285">如果您的應用程式需要的子資料夾的寫入權限，您就可以設定為該資料夾的權限，如中所示[設定資料夾權限](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)和[部署到生產環境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-285">If your application needs write access to a sub-folder, you can set permissions for that folder as shown in the [Setting Folder Permissions](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md) and [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorials.</span></span> <span data-ttu-id="f2ee0-286">如果您的應用程式需要站台的根資料夾的寫入權限，您必須防止從根資料夾上設定唯讀存取權，藉由新增 **&lt;IncludeSetACLProviderOn 目的地&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** 至發行設定檔 （以會影響單一設定檔） 或 wpp.targets 檔案 （以會影響所有設定檔）。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-286">If your application needs write access to the root folder of the site, you have to prevent it from setting read-only access on the root folder by adding **&lt;IncludeSetACLProviderOn Destination&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** to the publish profile file (to affect a single profile) or to the wpp.targets file (to affect all profiles).</span></span> <span data-ttu-id="f2ee0-287">如需如何編輯這些檔案的資訊，請參閱[如何： 編輯設定檔 (.pubxml) 檔中的部署設定](https://msdn.microsoft.com/library/ff398069.aspx)。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-287">For information about how to edit these files, see [How to: Edit Deployment Settings in Profile (.pubxml) Files](https://msdn.microsoft.com/library/ff398069.aspx).</span></span> <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a><span data-ttu-id="f2ee0-288">組態錯誤-targetFramework 屬性參考不晚於已安裝.NET Framework 版本的版本</span><span class="sxs-lookup"><span data-stu-id="f2ee0-288">Configuration Error - targetFramework attribute references a version that is later than the installed version of the .NET Framework</span></span>

### <a name="scenario"></a><span data-ttu-id="f2ee0-289">情節</span><span class="sxs-lookup"><span data-stu-id="f2ee0-289">Scenario</span></span>

<span data-ttu-id="f2ee0-290">您已成功發佈之 web 專案的目標 ASP.NET 4.5，但當您執行應用程式 (具有`customErrors`模式設為 「 關閉 」 的 Web.config 檔案中) 您收到下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-290">You successfully published a web project that targets ASP.NET 4.5, but when you run the application (with the `customErrors` mode set to "off" in the Web.config file) you get the following error:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

<span data-ttu-id="f2ee0-291">錯誤頁面的 [來源錯誤] 方塊中，反白顯示從 Web.config 下行錯誤的原因為：</span><span class="sxs-lookup"><span data-stu-id="f2ee0-291">The Source Error box of the error page highlights the following line from Web.config as the cause of the error:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="f2ee0-292">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="f2ee0-292">Possible Cause and Solution</span></span>

<span data-ttu-id="f2ee0-293">伺服器不支援 ASP.NET 4.5。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-293">The server does not support ASP.NET 4.5.</span></span> <span data-ttu-id="f2ee0-294">請連絡來判斷何時及是否可以加入 ASP.NET 4.5 支援主機服務提供者。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-294">Contact the hosting provider to determine when and if support for ASP.NET 4.5 can be added.</span></span> <span data-ttu-id="f2ee0-295">如果升級伺服器不是一個選項，您必須部署 ASP.NET 4 或更早版本為目標的 web 專案改用。如果您將 ASP.NET 4 或較早的 web 專案部署到相同的目的地時，選取**移除目的端的其他檔案** 核取方塊**設定** 索引標籤**發行 Web**精靈。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-295">If upgrading the server is not an option, you have to deploy a web project that targets ASP.NET 4 or earlier instead.If you deploy an ASP.NET 4 or earlier web project to the same destination, select the **Remove additional files at destination** check box on the **Settings** tab of the **Publish Web** wizard.</span></span> <span data-ttu-id="f2ee0-296">如果您沒有選取**移除目的端的其他檔案**，您仍會繼續組態錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-296">If you don't select **Remove additional files at destination**, you will continue to get the Configuration Error page.</span></span>

<span data-ttu-id="f2ee0-297">專案**屬性**windows 包含目標 framework 下拉式清單中，但您無法解決這個問題只變更與**.NET Framework 4.5**至**.NET Framework 4**.</span><span class="sxs-lookup"><span data-stu-id="f2ee0-297">The project **Properties** windows includes a Target framework drop-down list, but you can't resolve this problem by just changing that from **.NET Framework 4.5** to **.NET Framework 4**.</span></span> <span data-ttu-id="f2ee0-298">如果您的目標 framework 變更為舊版 framework 時，專案還是會有較新的 framework 版本的組件的參考，並不會執行。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-298">If you change the target framework to an earlier framework version, the project will still have references to the later framework version's assemblies and will not run.</span></span> <span data-ttu-id="f2ee0-299">您必須手動變更這些參考，或建立新的專案以.NET Framework 4 或更早版本為目標。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-299">You have to manually change those references or create a new project that targets .NET Framework 4 or earlier.</span></span> <span data-ttu-id="f2ee0-300">如需詳細資訊，請參閱[Web sites 的.NET Framework 目標](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx)。</span><span class="sxs-lookup"><span data-stu-id="f2ee0-300">For more information, see [.NET Framework Targeting for Web Sites](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="f2ee0-301">上一步</span><span class="sxs-lookup"><span data-stu-id="f2ee0-301">Previous</span></span>](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
