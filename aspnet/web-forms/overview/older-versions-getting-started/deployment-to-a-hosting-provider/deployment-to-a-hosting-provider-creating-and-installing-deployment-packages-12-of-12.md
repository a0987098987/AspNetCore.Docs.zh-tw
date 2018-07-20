---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: 使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 疑難排解 (12 / 12) |Microsoft Docs
author: tdykstra
description: 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫，使用 Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: d6175d46c6df3c9a4d9bc98492a4458bec45221c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811593"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a><span data-ttu-id="90764-103">使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 疑難排解 (12 / 12)</span><span class="sxs-lookup"><span data-stu-id="90764-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Troubleshooting (12 of 12)</span></span>
====================
<span data-ttu-id="90764-104">藉由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="90764-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="90764-105">下載入門專案</span><span class="sxs-lookup"><span data-stu-id="90764-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="90764-106">這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 包含 SQL Server Compact 資料庫。</span><span class="sxs-lookup"><span data-stu-id="90764-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="90764-107">如果您安裝 Web Publish Update，您也可以使用 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="90764-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="90764-108">在數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。</span><span class="sxs-lookup"><span data-stu-id="90764-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="90764-109">顯示 Visual Studio 2012 RC 版本之後引入的部署功能，示範如何部署 SQL Server Compact，以外的 SQL Server 版本，並示範如何部署至 Windows Azure 網站的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="90764-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Windows Azure Web Sites, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


<span data-ttu-id="90764-110">此頁面描述一些常見的問題，當您使用 Visual Studio 部署 ASP.NET web 應用程式時可能發生。</span><span class="sxs-lookup"><span data-stu-id="90764-110">This page describes some common problems that may arise when you deploy an ASP.NET web application by using Visual Studio.</span></span> <span data-ttu-id="90764-111">每個會提供一或多個可能原因和對應的解決方案。</span><span class="sxs-lookup"><span data-stu-id="90764-111">For each one, one or more possible causes and corresponding solutions are provided.</span></span>

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a><span data-ttu-id="90764-112">伺服器錯誤 '/' 應用程式-在目前的自訂錯誤設定防止錯誤的詳細資料從遠端檢視</span><span class="sxs-lookup"><span data-stu-id="90764-112">Server Error in '/' Application - Current Custom Error Settings Prevent Details of the Error from Being Viewed Remotely</span></span>

### <a name="scenario"></a><span data-ttu-id="90764-113">情節</span><span class="sxs-lookup"><span data-stu-id="90764-113">Scenario</span></span>

<span data-ttu-id="90764-114">部署至遠端主機的站台之後，您會收到錯誤訊息，提及 customErrors 設定 Web.config 檔案中的，而不會指出實際錯誤的原因是什麼：</span><span class="sxs-lookup"><span data-stu-id="90764-114">After deploying a site to a remote host, you get an error message that mentions the customErrors setting in the Web.config file but doesn't indicate what the actual cause of the error was:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="90764-115">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="90764-115">Possible Cause and Solution</span></span>

<span data-ttu-id="90764-116">根據預設，ASP.NET 會顯示詳細的錯誤資訊，只有在本機電腦上執行您的 web 應用程式時。</span><span class="sxs-lookup"><span data-stu-id="90764-116">By default, ASP.NET shows detailed error information only when your web application is running on the local computer.</span></span> <span data-ttu-id="90764-117">通常您不想要顯示詳細的錯誤資訊，您的 web 應用程式時，透過網際網路公開可用，因為駭客可能無法使用此資訊來尋找應用程式中的弱點。</span><span class="sxs-lookup"><span data-stu-id="90764-117">Generally you don't want to display detailed error information when your web application is publicly available over the Internet, because hackers may be able to use this information to find vulnerabilities in the application.</span></span> <span data-ttu-id="90764-118">不過，當您部署或更新的站台至站台，有時會發生錯誤了，而且您需要取得實際的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="90764-118">However, when you are deploying a site or updates to a site, sometimes something will go wrong and you need to get the actual error message.</span></span>

<span data-ttu-id="90764-119">若要啟用的應用程式，以顯示詳細的錯誤訊息，遠端主機上執行時，編輯 Web.config 檔案，以設定`customErrors`模式會關閉，重新部署應用程式，並再次執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="90764-119">To enable the application to display detailed error messages when it runs on the remote host, edit the Web.config file to set `customErrors` mode off, redeploy the application, and run the application again:</span></span>

1. <span data-ttu-id="90764-120">如果應用程式的 Web.config 檔是否`customErrors`中的項目`system.web`項目，變更`mode`為 「 關閉 」 的屬性。</span><span class="sxs-lookup"><span data-stu-id="90764-120">If the application Web.config file has a `customErrors` element in the `system.web` element, change the `mode` attribute to "off".</span></span> <span data-ttu-id="90764-121">否則，請新增`customErrors`中的項目`system.web`項目`mode`屬性設為 「 關閉 」，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="90764-121">Otherwise add a `customErrors` element in the `system.web` element with the `mode` attribute set to "off", as shown in the following example:</span></span>

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. <span data-ttu-id="90764-122">部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="90764-122">Deploy the application.</span></span>
3. <span data-ttu-id="90764-123">執行應用程式並重複任何您先前導致發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="90764-123">Run the application and repeat whatever you did earlier that caused the error to occur.</span></span> <span data-ttu-id="90764-124">現在您可以看到實際的錯誤訊息的功能。</span><span class="sxs-lookup"><span data-stu-id="90764-124">Now you can see what the actual error message is.</span></span>
4. <span data-ttu-id="90764-125">當您解決錯誤時，還原原始`customErrors`設定並重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="90764-125">When you have resolved the error, restore the original `customErrors` setting and redeploy the application.</span></span>

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a><span data-ttu-id="90764-126">存取遭到拒絕在網頁中使用 SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="90764-126">Access is Denied in a Web Page that Uses SQL Server Compact</span></span>

### <a name="scenario"></a><span data-ttu-id="90764-127">情節</span><span class="sxs-lookup"><span data-stu-id="90764-127">Scenario</span></span>

<span data-ttu-id="90764-128">當您部署使用 SQL Server Compact 的網站和您在存取資料庫的已部署站台執行頁面時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="90764-128">When you deploy a site that uses SQL Server Compact and you run a page in the deployed site that accesses the database, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="90764-129">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="90764-129">Possible Cause and Solution</span></span>

<span data-ttu-id="90764-130">在伺服器上的網路服務帳戶必須能夠讀取 SQL Compact 服務中的原生二進位檔*bin\amd64*或是*bin\x86*資料夾中，但沒有讀取這些資料夾的權限。</span><span class="sxs-lookup"><span data-stu-id="90764-130">The NETWORK SERVICE account on the server needs to be able to read SQL Service Compact native binaries that are in the *bin\amd64* or *bin\x86* folder, but it does not have read permissions for those folders.</span></span> <span data-ttu-id="90764-131">讀取網路服務的權限集*bin*資料夾，並確認其擴充至子資料夾的權限。</span><span class="sxs-lookup"><span data-stu-id="90764-131">Set read permission for NETWORK SERVICE on the *bin* folder, making sure to extend the permissions to subfolders.</span></span>

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a><span data-ttu-id="90764-132">無法讀取組態檔，因為沒有足夠的權限</span><span class="sxs-lookup"><span data-stu-id="90764-132">Cannot Read Configuration File Due to Insufficient Permissions</span></span>

### <a name="scenario"></a><span data-ttu-id="90764-133">情節</span><span class="sxs-lookup"><span data-stu-id="90764-133">Scenario</span></span>

<span data-ttu-id="90764-134">當您按一下 Visual Studio 發行應用程式部署至 IIS 在您的本機電腦上的按鈕發行會失敗並**輸出**視窗會顯示一則錯誤訊息如下所示：</span><span class="sxs-lookup"><span data-stu-id="90764-134">When you click the Visual Studio publish button to deploy an application to IIS on your local machine, publishing fails and the **Output** window shows an error message similar to this:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="90764-135">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="90764-135">Possible Cause and Solution</span></span>

<span data-ttu-id="90764-136">若要使用單鍵發行至 IIS 在本機電腦上，您必須以系統管理員權限執行 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="90764-136">To use one-click publish to IIS on your local machine, you must be running Visual Studio with administrator permissions.</span></span> <span data-ttu-id="90764-137">關閉 Visual Studio 並重新啟動系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="90764-137">Close Visual Studio and restart it with administrator permissions.</span></span>

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a><span data-ttu-id="90764-138">無法連接到目的地電腦...使用指定的處理序</span><span class="sxs-lookup"><span data-stu-id="90764-138">Could Not Connect to the Destination Computer ... Using the Specified Process</span></span>

### <a name="scenario"></a><span data-ttu-id="90764-139">情節</span><span class="sxs-lookup"><span data-stu-id="90764-139">Scenario</span></span>

<span data-ttu-id="90764-140">當您按一下 Visual Studio 發佈按鈕來部署應用程式時，發行會失敗並**輸出**視窗會顯示一則錯誤訊息如下所示：</span><span class="sxs-lookup"><span data-stu-id="90764-140">When you click the Visual Studio publish button to deploy an application, publishing fails and the **Output** window shows an error message similar to this:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="90764-141">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="90764-141">Possible Cause and Solution</span></span>

<span data-ttu-id="90764-142">Proxy 伺服器會中斷與目的地伺服器的通訊。</span><span class="sxs-lookup"><span data-stu-id="90764-142">A proxy server is interrupting communication with the destination server.</span></span> <span data-ttu-id="90764-143">從 Windows 控制台或 Internet Explorer 中，選取**網際網路選項**，然後選取**連線** 索引標籤。在 **網際網路內容** 對話方塊中，按一下**LAN 設定**。</span><span class="sxs-lookup"><span data-stu-id="90764-143">From the Windows Control Panel or in Internet Explorer, select **Internet Options** and select the **Connections** tab. In the **Internet Properties** dialog box, click **LAN Settings**.</span></span> <span data-ttu-id="90764-144">在 **區域網路 (LAN) 設定**對話方塊中，清除**自動偵測設定**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="90764-144">In the **Local Area Network (LAN) Settings** dialog box, clear the **Automatically detect settings** checkbox.</span></span> <span data-ttu-id="90764-145">然後再按一下 [發行] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="90764-145">Then click the publish button again.</span></span>

<span data-ttu-id="90764-146">如果問題持續發生，請連絡您的系統管理員，以判斷哪些可以使用 proxy 或防火牆設定。</span><span class="sxs-lookup"><span data-stu-id="90764-146">If the problem persists, contact your system administrator to determine what can be done with proxy or firewall settings.</span></span> <span data-ttu-id="90764-147">問題是因為 Web Deploy Web 管理服務部署 (8172); 使用非標準連接埠對於其他連線，Web Deploy，將會使用連接埠 80。</span><span class="sxs-lookup"><span data-stu-id="90764-147">The problem happens because Web Deploy uses a non-standard port for Web Management Service deployment (8172); for other connections, Web Deploy uses port 80.</span></span> <span data-ttu-id="90764-148">當您要部署到協力廠商主機服務提供者時，通常使用 Web 管理服務。</span><span class="sxs-lookup"><span data-stu-id="90764-148">When you are deploying to a third-party hosting provider, you are typically using the Web Management Service.</span></span>

## <a name="default-net-40-application-pool-does-not-exist"></a><span data-ttu-id="90764-149">預設.NET 4.0 的應用程式集區不存在</span><span class="sxs-lookup"><span data-stu-id="90764-149">Default .NET 4.0 Application Pool Does Not Exist</span></span>

### <a name="scenario"></a><span data-ttu-id="90764-150">情節</span><span class="sxs-lookup"><span data-stu-id="90764-150">Scenario</span></span>

<span data-ttu-id="90764-151">當您部署的應用程式需要.NET Framework 4 時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="90764-151">When you deploy an application that requires the .NET Framework 4, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="90764-152">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="90764-152">Possible Cause and Solution</span></span>

<span data-ttu-id="90764-153">在 IIS 中未安裝 ASP.NET 4。</span><span class="sxs-lookup"><span data-stu-id="90764-153">ASP.NET 4 is not installed in IIS.</span></span> <span data-ttu-id="90764-154">如果您要部署到伺服器，您的開發電腦且已安裝 Visual Studio 2010，ASP.NET 4 的電腦上已安裝，但可能未安裝在 IIS 中。</span><span class="sxs-lookup"><span data-stu-id="90764-154">If the server you are deploying to is your development computer and has Visual Studio 2010 installed on it, ASP.NET 4 is installed on the computer but might not be installed in IIS.</span></span> <span data-ttu-id="90764-155">在您要部署伺服器上，開啟提升權限的命令提示字元並安裝 ASP.NET 4 在 IIS 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="90764-155">On the server that you are deploying to, open an elevated command prompt and install ASP.NET 4 in IIS by running the following commands:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

<span data-ttu-id="90764-156">您也可能需要手動設定預設應用程式集區的.NET Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="90764-156">You might also need to manually set the .NET Framework version of the default application pool.</span></span> <span data-ttu-id="90764-157">如需詳細資訊，請參閱 <<c0> [ 部署到 IIS 作為測試環境](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="90764-157">For more information, see the [Deploying to IIS as a Test Environment](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial.</span></span>

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a><span data-ttu-id="90764-158">初始化字串的格式不符合從索引 0 處開始的規格。</span><span class="sxs-lookup"><span data-stu-id="90764-158">Format of the initialization string does not conform to specification starting at index 0.</span></span>

### <a name="scenario"></a><span data-ttu-id="90764-159">情節</span><span class="sxs-lookup"><span data-stu-id="90764-159">Scenario</span></span>

<span data-ttu-id="90764-160">部署應用程式使用一種單鍵之後發行，當您執行的頁面存取資料庫，得到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="90764-160">After you deploy an application using one-click publish, when you run a page that accesses the database you get the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="90764-161">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="90764-161">Possible Cause and Solution</span></span>

<span data-ttu-id="90764-162">開啟*Web.config*檔案中的已部署的站台和檢查連接字串值是否已開頭`$(ReplacableToken_`，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="90764-162">Open the *Web.config* file in the deployed site and check to see whether the connection string values begin with `$(ReplacableToken_`, as in the following example:</span></span>

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

<span data-ttu-id="90764-163">如果連接字串看起來像此範例中，編輯專案檔，並新增下列屬性，以`PropertyGroup`是針對所有組建組態的項目：</span><span class="sxs-lookup"><span data-stu-id="90764-163">If the connection strings look like this example, edit the project file and add the following property to the `PropertyGroup` element that is for all build configurations:</span></span>

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

<span data-ttu-id="90764-164">然後重新部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="90764-164">Then redeploy the application.</span></span>

## <a name="http-500-internal-server-error"></a><span data-ttu-id="90764-165">HTTP 500 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="90764-165">HTTP 500 Internal Server Error</span></span>

### <a name="scenario"></a><span data-ttu-id="90764-166">情節</span><span class="sxs-lookup"><span data-stu-id="90764-166">Scenario</span></span>

<span data-ttu-id="90764-167">當您執行已部署的站台時，您會看到下列錯誤訊息沒有指出錯誤的原因的特定資訊：</span><span class="sxs-lookup"><span data-stu-id="90764-167">When you run the deployed site, you see the following error message without specific information indicating the cause of the error:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="90764-168">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="90764-168">Possible Cause and Solution</span></span>

<span data-ttu-id="90764-169">原因有很多的 500 錯誤，但如果您要遵循這些教學課程中的其中一個可能原因是您將 XML 項目放在錯誤的位置，其中一種 XML 轉換檔案。</span><span class="sxs-lookup"><span data-stu-id="90764-169">There are many causes of 500 errors, but one possible cause if you are following these tutorials is that you put an XML element in the wrong place in one of the XML transformation files.</span></span> <span data-ttu-id="90764-170">例如，您會收到這個錯誤如果您將插入的轉換`<location>`項目底下`<system.web>`而不是直接在`<configuration>`。</span><span class="sxs-lookup"><span data-stu-id="90764-170">For example, you would get this error if you put the transformation that inserts a `<location>` element under `<system.web>` instead of directly under `<configuration>`.</span></span> <span data-ttu-id="90764-171">解決方案在此情況下是更正 XML 轉換檔案，然後重新部署。</span><span class="sxs-lookup"><span data-stu-id="90764-171">The solution in that case is to correct the XML transformation file and redeploy.</span></span>

## <a name="http-50021-internal-server-error"></a><span data-ttu-id="90764-172">HTTP 500.21 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="90764-172">HTTP 500.21 Internal Server Error</span></span>

### <a name="scenario"></a><span data-ttu-id="90764-173">情節</span><span class="sxs-lookup"><span data-stu-id="90764-173">Scenario</span></span>

<span data-ttu-id="90764-174">當您執行已部署的站台時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="90764-174">When you run the deployed site, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="90764-175">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="90764-175">Possible Cause and Solution</span></span>

<span data-ttu-id="90764-176">站台，您已部署 ASP.NET 4 中，但 ASP.NET 4 未註冊在 IIS 伺服器的目標。</span><span class="sxs-lookup"><span data-stu-id="90764-176">The site you have deployed targets ASP.NET 4, but ASP.NET 4 is not registered in IIS on the server.</span></span> <span data-ttu-id="90764-177">在伺服器上開啟提升權限的命令提示字元，並執行下列命令註冊 ASP.NET 4:</span><span class="sxs-lookup"><span data-stu-id="90764-177">On the server open an elevated command prompt and register ASP.NET 4 by running the following commands:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

<span data-ttu-id="90764-178">您也可能需要手動設定預設應用程式集區的.NET Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="90764-178">You might also need to manually set the .NET Framework version of the default application pool.</span></span> <span data-ttu-id="90764-179">如需詳細資訊，請參閱 <<c0> [ 部署到 IIS 作為測試環境](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="90764-179">For more information, see the [Deploying to IIS as a Test Environment](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial.</span></span>

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a><span data-ttu-id="90764-180">登入失敗的應用程式中的開啟 SQL Server Express Database\_資料</span><span class="sxs-lookup"><span data-stu-id="90764-180">Login Failed Opening SQL Server Express Database in App\_Data</span></span>

### <a name="scenario"></a><span data-ttu-id="90764-181">情節</span><span class="sxs-lookup"><span data-stu-id="90764-181">Scenario</span></span>

<span data-ttu-id="90764-182">您已更新*Web.config*檔案以指向做為 SQL Server Express 資料庫的連接字串 *.mdf*檔案中您*應用程式\_資料*資料夾，然後第一個您執行應用程式，您會看到下列錯誤訊息的時間：</span><span class="sxs-lookup"><span data-stu-id="90764-182">You updated the *Web.config* file connection string to point to a SQL Server Express database as an *.mdf* file in your *App\_Data* folder, and the first time you run the application you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="90764-183">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="90764-183">Possible Cause and Solution</span></span>

<span data-ttu-id="90764-184">名稱 *.mdf*檔案無法比對任何具有曾經存在過您的電腦的 SQL Server Express 資料庫的名稱，即使您刪除 *.mdf*先前已存在的資料庫檔案。</span><span class="sxs-lookup"><span data-stu-id="90764-184">The name of the *.mdf* file cannot match the name of any SQL Server Express database that has ever existed on your computer, even if you deleted the *.mdf* file of the previously existing database.</span></span> <span data-ttu-id="90764-185">變更的名稱 *.mdf*從未使用做為資料庫名稱變更為名稱的檔案*Web.config*檔案，以使用新的名稱。</span><span class="sxs-lookup"><span data-stu-id="90764-185">Change the name of the *.mdf* file to a name that has never been used as a database name and change the *Web.config* file to use the new name.</span></span> <span data-ttu-id="90764-186">或者，您可以使用[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)刪除先前已經有 SQL Server Express 資料庫。</span><span class="sxs-lookup"><span data-stu-id="90764-186">As an alternative, you can use [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) to delete previously existing SQL Server Express databases.</span></span>

## <a name="model-compatibility-cannot-be-checked"></a><span data-ttu-id="90764-187">模型的相容性無法檢查</span><span class="sxs-lookup"><span data-stu-id="90764-187">Model Compatibility Cannot be Checked</span></span>

### <a name="scenario"></a><span data-ttu-id="90764-188">情節</span><span class="sxs-lookup"><span data-stu-id="90764-188">Scenario</span></span>

<span data-ttu-id="90764-189">您已更新*Web.config*檔案連接字串以指向新的 SQL Server Express 資料庫，並執行應用程式的第一次您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="90764-189">You updated the *Web.config* file connection string to point to a new SQL Server Express database, and the first time you run the application you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="90764-190">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="90764-190">Possible Cause and Solution</span></span>

<span data-ttu-id="90764-191">如果之前您在電腦上，資料庫可能已經存在的某些資料表中，曾經使用您放在 Web.config 檔案中的資料庫名稱。</span><span class="sxs-lookup"><span data-stu-id="90764-191">If the database name you put in the Web.config file was ever used before on your computer, a database might already exist with some tables in it.</span></span> <span data-ttu-id="90764-192">選取您電腦之前變更尚未使用的新名稱*Web.config*檔以指向使用這個新的資料庫名稱。</span><span class="sxs-lookup"><span data-stu-id="90764-192">Select a new name that has not been used on your computer before and change the *Web.config* file to point to use this new database name.</span></span> <span data-ttu-id="90764-193">或者，您可以使用[SQL Server Express 公用程式](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990)或是[SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593)刪除現有的資料庫。</span><span class="sxs-lookup"><span data-stu-id="90764-193">As an alternative, you can use [SQL Server Express Utility](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) or [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) to delete the existing database.</span></span>

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a><span data-ttu-id="90764-194">SQL 錯誤時指令碼會嘗試建立使用者或角色</span><span class="sxs-lookup"><span data-stu-id="90764-194">SQL Error When a Script Attempts to Create Users or Roles</span></span>

### <a name="scenario"></a><span data-ttu-id="90764-195">情節</span><span class="sxs-lookup"><span data-stu-id="90764-195">Scenario</span></span>

<span data-ttu-id="90764-196">您使用上設定的資料庫部署**封裝/發行 SQL**  索引標籤，在部署期間執行的 SQL 指令碼包含 Create User 或 Create Role 命令和指令碼執行失敗時執行這些命令。</span><span class="sxs-lookup"><span data-stu-id="90764-196">You are using database deployment configured on the **Package/Publish SQL** tab, SQL scripts that run during deployment include Create User or Create Role commands, and script execution fails when those commands are executed.</span></span> <span data-ttu-id="90764-197">您可能會看到更多詳細的訊息，如下所示：</span><span class="sxs-lookup"><span data-stu-id="90764-197">You might see more detailed messages, such as the following:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

<span data-ttu-id="90764-198">此錯誤發生時，您已設定中的資料庫部署**發佈 Web**精靈而非**封裝/發行 SQL**索引標籤上，建立中的執行緒[組態和部署](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment)論壇和解決方案將會新增至本疑難排解頁面。</span><span class="sxs-lookup"><span data-stu-id="90764-198">If this error occurs when you have configured database deployment in the **Publish Web** wizard rather than the **Package/Publish SQL** tab, create a thread in the [Configuration and Deployment](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) forum, and the solution will be added to this troubleshooting page.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="90764-199">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="90764-199">Possible Cause and Solution</span></span>

<span data-ttu-id="90764-200">您用來執行部署的使用者帳戶沒有建立使用者或角色的權限。</span><span class="sxs-lookup"><span data-stu-id="90764-200">The user account you are using to perform deployment does not have permission to create users or roles.</span></span> <span data-ttu-id="90764-201">比方說，控管公司可能會指派`db_datareader`， `db_datawriter`，和`db_ddladmin`它為您設定的使用者帳戶的角色。</span><span class="sxs-lookup"><span data-stu-id="90764-201">For example, the hosting company might assign the `db_datareader`, `db_datawriter`, and `db_ddladmin` roles to the user account that it sets up for you.</span></span> <span data-ttu-id="90764-202">這些是足夠用於建立大多數的資料庫物件，但不適用於建立使用者或角色。</span><span class="sxs-lookup"><span data-stu-id="90764-202">These are sufficient for creating most database objects, but not for creating users or roles.</span></span> <span data-ttu-id="90764-203">若要避免錯誤的方法之一是藉由從資料庫部署中排除使用者和角色。</span><span class="sxs-lookup"><span data-stu-id="90764-203">One way to avoid the error is by excluding users and roles from database deployment.</span></span> <span data-ttu-id="90764-204">您可以藉由編輯`PreSource`資料庫的項目會自動產生的指令碼使其包含下列屬性：</span><span class="sxs-lookup"><span data-stu-id="90764-204">You can do this by editing the `PreSource` element for the database's automatically generated script so that it includes the following attributes:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

<span data-ttu-id="90764-205">如需如何編輯`PreSource`項目，在專案檔中，請參閱[如何： 編輯專案檔中的部署設定](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx)。</span><span class="sxs-lookup"><span data-stu-id="90764-205">For information about how to edit the `PreSource` element in the project file, see [How to: Edit Deployment Settings in the Project File](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx).</span></span> <span data-ttu-id="90764-206">如果需要在目的地資料庫中的使用者或開發資料庫中的角色，請連絡您的主機服務提供者尋求協助。</span><span class="sxs-lookup"><span data-stu-id="90764-206">If the users or roles in your development database need to be in the destination database, contact your hosting provider for assistance.</span></span>

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a><span data-ttu-id="90764-207">SQL Server 逾時錯誤時在部署期間執行自訂指令碼</span><span class="sxs-lookup"><span data-stu-id="90764-207">SQL Server Timeout Error When Running Custom Scripts During Deployment</span></span>

### <a name="scenario"></a><span data-ttu-id="90764-208">情節</span><span class="sxs-lookup"><span data-stu-id="90764-208">Scenario</span></span>

<span data-ttu-id="90764-209">您已指定在部署期間，執行自訂 SQL 指令碼和 Web Deploy 執行時它們，它們的逾時間。</span><span class="sxs-lookup"><span data-stu-id="90764-209">You have specified custom SQL scripts to run during deployment, and when Web Deploy runs them, they time out.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="90764-210">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="90764-210">Possible Cause and Solution</span></span>

<span data-ttu-id="90764-211">執行具有不同的交易模式的多個指令碼，可能會導致逾時錯誤。</span><span class="sxs-lookup"><span data-stu-id="90764-211">Running multiple scripts that have different transaction modes can cause time-out errors.</span></span> <span data-ttu-id="90764-212">根據預設，自動產生的指令碼執行在交易中，但不是這麼做的自訂指令碼。</span><span class="sxs-lookup"><span data-stu-id="90764-212">By default, automatically generated scripts run in a transaction, but custom scripts do not.</span></span> <span data-ttu-id="90764-213">如果您選取**提取資料及/或從現有的資料庫結構描述**選項**封裝/發行 SQL**索引標籤上，而且如果您加入自訂的 SQL 指令碼，您必須變更一些指令碼的交易設定以便所有的指令碼會使用相同的交易設定。</span><span class="sxs-lookup"><span data-stu-id="90764-213">If you select the **Pull data and/or schema from an existing database** option on the **Package/Publish SQL** tab, and if you add a custom SQL script, you must change transaction settings on some scripts so that all scripts use the same transaction settings.</span></span> <span data-ttu-id="90764-214">如需詳細資訊，請參閱 <<c0> [ 如何： 資料庫使用 Web 應用程式專案部署](https://msdn.microsoft.com/library/dd465343.aspx)。</span><span class="sxs-lookup"><span data-stu-id="90764-214">For more information, see [How to: Deploy a Database With a Web Application Project](https://msdn.microsoft.com/library/dd465343.aspx).</span></span>

<span data-ttu-id="90764-215">如果您已設定交易設定，使所有都相同，但仍然收到這個錯誤，可能的解決方法是分別執行指令碼。</span><span class="sxs-lookup"><span data-stu-id="90764-215">If you have configured transaction settings so that all are the same but still get this error, a possible workaround is to run the scripts separately.</span></span> <span data-ttu-id="90764-216">在 **資料庫指令碼**方格中的**封裝/發行**SQL 索引標籤上，清除**Include**造成逾時錯誤，指令碼的核取方塊，然後發行專案。</span><span class="sxs-lookup"><span data-stu-id="90764-216">In the **Database Scripts** grid in the **Package/Publish** SQL tab, clear the **Include** check box for the script that causes the timeout error, then publish the project.</span></span> <span data-ttu-id="90764-217">然後移回**資料庫指令碼**方格中，選取該指令碼**Include**核取方塊，然後清除**Include**其他指令碼的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="90764-217">Then go back into the **Database Scripts** grid, select that script's **Include** check box, and clear the **Include** check boxes for the other scripts.</span></span> <span data-ttu-id="90764-218">然後重新發佈專案。</span><span class="sxs-lookup"><span data-stu-id="90764-218">Then publish the project again.</span></span> <span data-ttu-id="90764-219">此時，當您發行時，只選取自訂的指令碼執行。</span><span class="sxs-lookup"><span data-stu-id="90764-219">This time when you publish, only the selected custom script runs.</span></span>

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a><span data-ttu-id="90764-220">站台資訊清單的 Stream 資料尚無法使用</span><span class="sxs-lookup"><span data-stu-id="90764-220">Stream Data of Site Manifest Is Not Yet Available</span></span>

### <a name="scenario"></a><span data-ttu-id="90764-221">情節</span><span class="sxs-lookup"><span data-stu-id="90764-221">Scenario</span></span>

<span data-ttu-id="90764-222">當您要安裝封裝，使用*deploy.cmd*檔案中使用`t`（測試） 選項中，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="90764-222">When you are installing a package using the *deploy.cmd* file with the `t` (test) option, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="90764-223">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="90764-223">Possible Cause and Solution</span></span>

<span data-ttu-id="90764-224">錯誤訊息表示命令無法產生測試報告。</span><span class="sxs-lookup"><span data-stu-id="90764-224">The error message means that the command cannot produce a test report.</span></span> <span data-ttu-id="90764-225">不過，命令可能執行如果您使用`y`（實際安裝） 選項。</span><span class="sxs-lookup"><span data-stu-id="90764-225">However, the command might run if you use the `y` (actual installation) option.</span></span> <span data-ttu-id="90764-226">訊息僅會指示是在測試模式中執行命令的問題。</span><span class="sxs-lookup"><span data-stu-id="90764-226">The message indicates only that there is a problem with running the command in test mode.</span></span>

## <a name="this-application-requires-managedruntimeversion-v40"></a><span data-ttu-id="90764-227">此應用程式需要 ManagedRuntimeVersion v4.0</span><span class="sxs-lookup"><span data-stu-id="90764-227">This Application Requires ManagedRuntimeVersion v4.0</span></span>

### <a name="scenario"></a><span data-ttu-id="90764-228">情節</span><span class="sxs-lookup"><span data-stu-id="90764-228">Scenario</span></span>

<span data-ttu-id="90764-229">當您嘗試部署時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="90764-229">When you attempt to deploy, you see the following error message:</span></span>

 <span data-ttu-id="90764-230">錯誤： 資料流資料的 ' sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' 尚無法使用。</span><span class="sxs-lookup"><span data-stu-id="90764-230">Error: The stream data of 'sitemanifest/dbFullSql[@path='C:\TEMP\AdventureWorksGrant.sql']/sqlScript' is not yet available.</span></span> <span data-ttu-id="90764-231">您嘗試使用應用程式集區已設定為 'v2.0' 的 'managedRuntimeVersion' 屬性。</span><span class="sxs-lookup"><span data-stu-id="90764-231">The application pool that you are trying to use has the 'managedRuntimeVersion' property set to 'v2.0'.</span></span> <span data-ttu-id="90764-232">此應用程式需要 'v4.0'。</span><span class="sxs-lookup"><span data-stu-id="90764-232">This application requires 'v4.0'.</span></span> 

### <a name="possible-cause-and-solution"></a><span data-ttu-id="90764-233">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="90764-233">Possible Cause and Solution</span></span>

<span data-ttu-id="90764-234">在 IIS 中未安裝 ASP.NET 4。</span><span class="sxs-lookup"><span data-stu-id="90764-234">ASP.NET 4 is not installed in IIS.</span></span> <span data-ttu-id="90764-235">如果您要部署到伺服器，您的開發電腦且已安裝 Visual Studio 2010，ASP.NET 4 的電腦上已安裝，但可能未安裝在 IIS 中。</span><span class="sxs-lookup"><span data-stu-id="90764-235">If the server you are deploying to is your development computer and has Visual Studio 2010 installed on it, ASP.NET 4 is installed on the computer but might not be installed in IIS.</span></span> <span data-ttu-id="90764-236">在您要部署伺服器上，開啟提升權限的命令提示字元並安裝 ASP.NET 4 在 IIS 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="90764-236">On the server that you are deploying to, open an elevated command prompt and install ASP.NET 4 in IIS by running the following commands:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a><span data-ttu-id="90764-237">無法轉換 Microsoft.Web.Deployment.DeploymentProviderOptions</span><span class="sxs-lookup"><span data-stu-id="90764-237">Unable to cast Microsoft.Web.Deployment.DeploymentProviderOptions</span></span>

### <a name="scenario"></a><span data-ttu-id="90764-238">情節</span><span class="sxs-lookup"><span data-stu-id="90764-238">Scenario</span></span>

<span data-ttu-id="90764-239">當您部署封裝時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="90764-239">When you are deploying a package, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="90764-240">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="90764-240">Possible Cause and Solution</span></span>

<span data-ttu-id="90764-241">您嘗試部署從 IIS 管理員 」 中使用 Web 部署 1.1 UI 已安裝的 Web Deploy 2.0 的伺服器。</span><span class="sxs-lookup"><span data-stu-id="90764-241">You are trying to deploy from IIS Manager using the Web Deploy 1.1 UI to a server that has Web Deploy 2.0 installed.</span></span> <span data-ttu-id="90764-242">如果您使用 IIS 的遠端系統管理工具來部署匯入封裝時，檢查**新提供的功能**對話方塊中，當您建立連線。</span><span class="sxs-lookup"><span data-stu-id="90764-242">If you are using the IIS Remote Administration Tool to deploy by importing a package, check the **New Features Available** dialog box when you establish the connection.</span></span> <span data-ttu-id="90764-243">（此對話方塊可能只會顯示一次第一次建立連接時。</span><span class="sxs-lookup"><span data-stu-id="90764-243">(This dialog box might only be shown once when the connection is first established.</span></span> <span data-ttu-id="90764-244">清除連接，然後再重新啟動，以關閉 IIS 管理員，並啟動它一次輸入`inetmgr /reset`在命令提示字元。)列出的其中一項功能會**Web 部署 UI**，其版本號碼低於 8，您要部署到伺服器可能會有 1.1 和 2.0 的 Web Deploy 安裝的版本。</span><span class="sxs-lookup"><span data-stu-id="90764-244">To clear the connection and start over, close IIS Manager and start it up again by entering `inetmgr /reset` at the command prompt.) If one of the features listed is **Web Deploy UI**, and it has a version number lower than 8, the server you are deploying to might have both 1.1 and 2.0 versions of Web Deploy installed.</span></span> <span data-ttu-id="90764-245">若要從已安裝的 2.0 用戶端部署，伺服器必須只有 Web Deploy 2.0 安裝。</span><span class="sxs-lookup"><span data-stu-id="90764-245">To deploy from a client that has 2.0 installed, the server must have only Web Deploy 2.0 installed.</span></span> <span data-ttu-id="90764-246">您必須連絡主控提供者，若要解決此問題。</span><span class="sxs-lookup"><span data-stu-id="90764-246">You will have to contact your hosting provider to resolve this problem.</span></span>

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a><span data-ttu-id="90764-247">無法載入 SQL Server Compact 的原生元件</span><span class="sxs-lookup"><span data-stu-id="90764-247">Unable to load the native components of SQL Server Compact</span></span>

### <a name="scenario"></a><span data-ttu-id="90764-248">情節</span><span class="sxs-lookup"><span data-stu-id="90764-248">Scenario</span></span>

<span data-ttu-id="90764-249">當您執行已部署的站台時，您會看到下列錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="90764-249">When you run the deployed site, you see the following error message:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="90764-250">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="90764-250">Possible Cause and Solution</span></span>

<span data-ttu-id="90764-251">已部署的站台沒有*amd64*並*x86*及其子資料夾中它們的應用程式的原生組件*bin*資料夾。</span><span class="sxs-lookup"><span data-stu-id="90764-251">The deployed site does not have *amd64* and *x86* subfolders with the native assemblies in them under the application's *bin* folder.</span></span> <span data-ttu-id="90764-252">SQL Server Compact 安裝的電腦，在原生組件位於*C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*。</span><span class="sxs-lookup"><span data-stu-id="90764-252">On a computer that has SQL Server Compact installed, the native assemblies are located in *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*.</span></span> <span data-ttu-id="90764-253">若要取得正確的檔案到正確的資料夾，Visual Studio 專案中的最佳方式是安裝 NuGet SqlServerCompact 封裝。</span><span class="sxs-lookup"><span data-stu-id="90764-253">The best way to get the correct files into the correct folders in a Visual Studio project is to install the NuGet SqlServerCompact package.</span></span> <span data-ttu-id="90764-254">套件安裝新增建置後指令碼來複製原生組件，載入*amd64*並*x86*。</span><span class="sxs-lookup"><span data-stu-id="90764-254">Package installation adds a post-build script to copy the native assemblies into *amd64* and *x86*.</span></span> <span data-ttu-id="90764-255">為了讓這些部署，不過，您必須手動將其包含在專案中。</span><span class="sxs-lookup"><span data-stu-id="90764-255">In order for these to be deployed, however, you have to manually include them in the project.</span></span> <span data-ttu-id="90764-256">如需詳細資訊，請參閱 <<c0> [ 部署 SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="90764-256">For more information, see the [Deploying SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) tutorial.</span></span>

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a><span data-ttu-id="90764-257">部署 Entity Framework Code First 應用程式之後的 「 路徑不正確 」 錯誤</span><span class="sxs-lookup"><span data-stu-id="90764-257">"Path is not valid" error after deploying an Entity Framework Code First application</span></span>

### <a name="scenario"></a><span data-ttu-id="90764-258">情節</span><span class="sxs-lookup"><span data-stu-id="90764-258">Scenario</span></span>

<span data-ttu-id="90764-259">您部署的應用程式，例如 SQL Server Compact 將其資料庫儲存於應用程式中的檔案就會使用 Entity Framework Code First 移轉和 DBMS\_Data 資料夾。</span><span class="sxs-lookup"><span data-stu-id="90764-259">You deploy an application that uses Entity Framework Code First Migrations and a DBMS such as SQL Server Compact which stores its database in a file in the App\_Data folder.</span></span> <span data-ttu-id="90764-260">您必須設定為在您第一次部署之後建立資料庫的 Code First 移轉。</span><span class="sxs-lookup"><span data-stu-id="90764-260">You have Code First Migrations configured to create the database after your first deployment.</span></span> <span data-ttu-id="90764-261">當您執行應用程式時您會收到錯誤訊息如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="90764-261">When you run the application you get an error message like the following example:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="90764-262">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="90764-262">Possible Cause and Solution</span></span>

<span data-ttu-id="90764-263">程式碼第一次嘗試建立資料庫，但應用程式\_資料資料夾不存在。</span><span class="sxs-lookup"><span data-stu-id="90764-263">Code First is attempting to create the database but the App\_Data folder does not exist.</span></span> <span data-ttu-id="90764-264">您未有任何檔案*應用程式\_資料*當您部署，或您所選取的資料夾**排除應用程式\_資料**上**封裝/發行 Web**索引標籤**專案屬性**視窗。</span><span class="sxs-lookup"><span data-stu-id="90764-264">Either you didn't have any files in the *App\_Data* folder when you deployed, or you selected **Exclude App\_Data** on the **Package/Publish Web** tab of the **Project Properties** window.</span></span> <span data-ttu-id="90764-265">部署程序不會在伺服器上建立資料夾，如果要複製到伺服器的資料夾中不有任何檔案。</span><span class="sxs-lookup"><span data-stu-id="90764-265">The deployment process won't create a folder on the server if there are no files in the folder to be copied to the server.</span></span> <span data-ttu-id="90764-266">如果您已經在網站中設定的資料庫，在部署程序會刪除的檔案和*應用程式\_資料*本身如果您選取的資料夾**移除目的地上的其他檔案**中發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="90764-266">If you already had the database set up in the site, the deployment process will delete the files and the *App\_Data* folder itself if you selected **Remove additional files at destination** in the publish profile.</span></span> <span data-ttu-id="90764-267">若要解決此問題，將預留位置檔案放在.txt 檔案*應用程式\_資料*資料夾，請確定您沒有**排除應用程式\_資料**選取，並重新部署。</span><span class="sxs-lookup"><span data-stu-id="90764-267">To solve the problem, put a placeholder file such as a .txt file in the *App\_Data* folder, make sure you do not have **Exclude App\_Data** selected, and redeploy.</span></span> 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a><span data-ttu-id="90764-268">「 無法使用分開基礎 RCW 的 COM 物件。 」</span><span class="sxs-lookup"><span data-stu-id="90764-268">"COM object that has been separated from its underlying RCW cannot be used."</span></span>

### <a name="scenario"></a><span data-ttu-id="90764-269">情節</span><span class="sxs-lookup"><span data-stu-id="90764-269">Scenario</span></span>

<span data-ttu-id="90764-270">您已順利使用單鍵發行來部署您的應用程式啟動，而您收到此錯誤：</span><span class="sxs-lookup"><span data-stu-id="90764-270">You have been successfully using one-click publish to deploy your application and then you start getting this error:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="90764-271">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="90764-271">Possible Cause and Solution</span></span>

<span data-ttu-id="90764-272">關閉並重新啟動 Visual Studio 通常是所有所需解決此錯誤。</span><span class="sxs-lookup"><span data-stu-id="90764-272">Closing and restarting Visual Studio is usually all that is required to resolve this error.</span></span>

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a><span data-ttu-id="90764-273">部署失敗因為使用者認證用於發佈的並沒有 setACL 授權單位</span><span class="sxs-lookup"><span data-stu-id="90764-273">Deployment Fails Because User Credentials Used for Publishing Don't Have setACL Authority</span></span>

### <a name="scenario"></a><span data-ttu-id="90764-274">情節</span><span class="sxs-lookup"><span data-stu-id="90764-274">Scenario</span></span>

<span data-ttu-id="90764-275">發行失敗，發生錯誤，指出您沒有權限設定資料夾權限 （您使用的使用者帳戶沒有 setACL 授權單位）。</span><span class="sxs-lookup"><span data-stu-id="90764-275">Publishing fails with an error that indicates you don't have authority to set folder permissions (the user account you are using doesn't have setACL authority).</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="90764-276">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="90764-276">Possible Cause and Solution</span></span>

<span data-ttu-id="90764-277">根據預設，Visual Studio 設定站台的根資料夾的權限上讀取和寫入權限的應用程式\_Data 資料夾。</span><span class="sxs-lookup"><span data-stu-id="90764-277">By default, Visual Studio sets read permissions on the root folder of the site and write permissions on the App\_Data folder.</span></span> <span data-ttu-id="90764-278">若您知道的站台資料夾的預設權限是否正確，而且不需要設定，您加入停用此行為**&lt;IncludeSetACLProviderOn 目的地&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** 至發行設定檔 （以會影響單一設定檔） 或.wpp.targets 檔案 （以會影響所有設定檔）。</span><span class="sxs-lookup"><span data-stu-id="90764-278">If you know that the default permissions on site folders are correct and do not need to be set, you disable this behavior by adding **&lt;IncludeSetACLProviderOn Destination&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** to the publish profile file (to affect a single profile) or to the wpp.targets file (to affect all profiles).</span></span> <span data-ttu-id="90764-279">如需有關如何編輯這些檔案的資訊，請參閱[如何： 編輯設定檔 (.pubxml) 檔中的部署設定](https://msdn.microsoft.com/library/ff398069.aspx)。</span><span class="sxs-lookup"><span data-stu-id="90764-279">For information about how to edit these files, see [How to: Edit Deployment Settings in Profile (.pubxml) Files](https://msdn.microsoft.com/library/ff398069.aspx).</span></span> 

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a><span data-ttu-id="90764-280">當應用程式嘗試寫入應用程式資料夾時，存取被拒錯誤</span><span class="sxs-lookup"><span data-stu-id="90764-280">Access Denied Errors when the Application Tries to Write to an Application Folder</span></span>

### <a name="scenario"></a><span data-ttu-id="90764-281">情節</span><span class="sxs-lookup"><span data-stu-id="90764-281">Scenario</span></span>

<span data-ttu-id="90764-282">您的應用程式錯誤時嘗試建立或編輯的檔案中的其中一個應用程式資料夾中，因為它並沒有該資料夾的寫入授權單位。</span><span class="sxs-lookup"><span data-stu-id="90764-282">Your application errors when it tries to create or edit a file in one of the application folders, because it does not have write authority for that folder.</span></span>

### <a name="possible-cause-and-solution"></a><span data-ttu-id="90764-283">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="90764-283">Possible Cause and Solution</span></span>

<span data-ttu-id="90764-284">根據預設，Visual Studio 設定站台的根資料夾的權限上讀取和寫入權限的應用程式\_Data 資料夾。</span><span class="sxs-lookup"><span data-stu-id="90764-284">By default, Visual Studio sets read permissions on the root folder of the site and write permissions on the App\_Data folder.</span></span> <span data-ttu-id="90764-285">如果您的應用程式需要的子資料夾的寫入權限，您就可以設定為該資料夾的權限，如中所示[設定資料夾權限](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)並[部署到生產環境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="90764-285">If your application needs write access to a sub-folder, you can set permissions for that folder as shown in the [Setting Folder Permissions](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md) and [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorials.</span></span> <span data-ttu-id="90764-286">如果您的應用程式需要的站台的根資料夾的寫入權限，您必須防止根資料夾上設定唯讀存取權，加上**&lt;IncludeSetACLProviderOn 目的地&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** 至發行設定檔 （以會影響單一設定檔） 或.wpp.targets 檔案 （以會影響所有設定檔）。</span><span class="sxs-lookup"><span data-stu-id="90764-286">If your application needs write access to the root folder of the site, you have to prevent it from setting read-only access on the root folder by adding **&lt;IncludeSetACLProviderOn Destination&gt;False&lt;/IncludeSetACLProviderOnDestination&gt;** to the publish profile file (to affect a single profile) or to the wpp.targets file (to affect all profiles).</span></span> <span data-ttu-id="90764-287">如需有關如何編輯這些檔案的資訊，請參閱[如何： 編輯設定檔 (.pubxml) 檔中的部署設定](https://msdn.microsoft.com/library/ff398069.aspx)。</span><span class="sxs-lookup"><span data-stu-id="90764-287">For information about how to edit these files, see [How to: Edit Deployment Settings in Profile (.pubxml) Files](https://msdn.microsoft.com/library/ff398069.aspx).</span></span> <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a><span data-ttu-id="90764-288">組態錯誤-targetFramework 屬性參考的版本晚於安裝的.NET Framework 版本</span><span class="sxs-lookup"><span data-stu-id="90764-288">Configuration Error - targetFramework attribute references a version that is later than the installed version of the .NET Framework</span></span>

### <a name="scenario"></a><span data-ttu-id="90764-289">情節</span><span class="sxs-lookup"><span data-stu-id="90764-289">Scenario</span></span>

<span data-ttu-id="90764-290">您已成功發佈之 web 專案的目標 ASP.NET 4.5 中，但當您執行應用程式 (與`customErrors`模式設定為 「 關閉 」 的 Web.config 檔案中) 您會收到下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="90764-290">You successfully published a web project that targets ASP.NET 4.5, but when you run the application (with the `customErrors` mode set to "off" in the Web.config file) you get the following error:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

<span data-ttu-id="90764-291">來源錯誤中的 [錯誤] 頁面中，反白顯示下行從 Web.config 做為錯誤的原因：</span><span class="sxs-lookup"><span data-stu-id="90764-291">The Source Error box of the error page highlights the following line from Web.config as the cause of the error:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a><span data-ttu-id="90764-292">可能的原因和解決方案</span><span class="sxs-lookup"><span data-stu-id="90764-292">Possible Cause and Solution</span></span>

<span data-ttu-id="90764-293">伺服器不支援 ASP.NET 4.5。</span><span class="sxs-lookup"><span data-stu-id="90764-293">The server does not support ASP.NET 4.5.</span></span> <span data-ttu-id="90764-294">請連絡來判斷何時及是否可以加入支援 ASP.NET 4.5 的主機服務提供者。</span><span class="sxs-lookup"><span data-stu-id="90764-294">Contact the hosting provider to determine when and if support for ASP.NET 4.5 can be added.</span></span> <span data-ttu-id="90764-295">如果升級伺服器不是一個選項，您必須部署之 web 專案的目標 ASP.NET 4 或更早版本改。如果您將 ASP.NET 4 或較早的 web 專案部署到相同的目的地時，選取**移除其他檔案目的地** 核取方塊**設定**索引標籤**發佈 Web**精靈。</span><span class="sxs-lookup"><span data-stu-id="90764-295">If upgrading the server is not an option, you have to deploy a web project that targets ASP.NET 4 or earlier instead.If you deploy an ASP.NET 4 or earlier web project to the same destination, select the **Remove additional files at destination** check box on the **Settings** tab of the **Publish Web** wizard.</span></span> <span data-ttu-id="90764-296">如果您未選取**移除目的地上的其他檔案**，您仍會繼續設定錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="90764-296">If you don't select **Remove additional files at destination**, you will continue to get the Configuration Error page.</span></span>

<span data-ttu-id="90764-297">專案**屬性**windows 包含目標 framework 下拉式清單中，但您不能解決這個問題，只要變更，從 **.NET Framework 4.5**到 **.NET Framework 4**.</span><span class="sxs-lookup"><span data-stu-id="90764-297">The project **Properties** windows includes a Target framework drop-down list, but you can't resolve this problem by just changing that from **.NET Framework 4.5** to **.NET Framework 4**.</span></span> <span data-ttu-id="90764-298">如果您將目標 framework 變更為較早的 framework 版本時，專案還是會有較新的 framework 版本的組件的參考，並不會執行。</span><span class="sxs-lookup"><span data-stu-id="90764-298">If you change the target framework to an earlier framework version, the project will still have references to the later framework version's assemblies and will not run.</span></span> <span data-ttu-id="90764-299">您必須手動變更這些參考，或建立新的專案以.NET Framework 4 或更早版本為目標。</span><span class="sxs-lookup"><span data-stu-id="90764-299">You have to manually change those references or create a new project that targets .NET Framework 4 or earlier.</span></span> <span data-ttu-id="90764-300">如需詳細資訊，請參閱 <<c0> [ 網站的.NET Framework 目標](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx)。</span><span class="sxs-lookup"><span data-stu-id="90764-300">For more information, see [.NET Framework Targeting for Web Sites](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="90764-301">上一步</span><span class="sxs-lookup"><span data-stu-id="90764-301">Previous</span></span>](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
