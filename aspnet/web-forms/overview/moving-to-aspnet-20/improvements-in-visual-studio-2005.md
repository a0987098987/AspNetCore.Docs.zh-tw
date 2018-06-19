---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: 在 Visual Studio 2005 中的增強功能 |Microsoft 文件
author: microsoft
description: Visual Studio 2005 提供 Web 應用程式開發人員一長串的增強功能和增強功能，Web 專案。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: aafc59980e807677d6023110d324365ce92bb5fc
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/05/2018
ms.locfileid: "28987986"
---
<a name="improvements-in-visual-studio-2005"></a><span data-ttu-id="763ed-103">在 Visual Studio 2005 中的增強功能</span><span class="sxs-lookup"><span data-stu-id="763ed-103">Improvements in Visual Studio 2005</span></span>
====================
<span data-ttu-id="763ed-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="763ed-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="763ed-105">Visual Studio 2005 提供 Web 應用程式開發人員一長串的增強功能和增強功能，Web 專案。</span><span class="sxs-lookup"><span data-stu-id="763ed-105">Visual Studio 2005 provides Web application developers with a long list of improvements and enhancements to Web projects.</span></span>


<span data-ttu-id="763ed-106">Visual Studio 2005 提供 Web 應用程式開發人員一長串的增強功能和增強功能，Web 專案。</span><span class="sxs-lookup"><span data-stu-id="763ed-106">Visual Studio 2005 provides Web application developers with a long list of improvements and enhancements to Web projects.</span></span> <span data-ttu-id="763ed-107">強大的 Visual Studio.NET 2002年和 2003年是，許多抱怨中沒有已處理的 Web 專案的方式。</span><span class="sxs-lookup"><span data-stu-id="763ed-107">As powerful as Visual Studio .NET 2002 and 2003 are, there were many complaints in the way that Web projects were handled.</span></span> <span data-ttu-id="763ed-108">Visual Studio 2005 定址這些抱怨將大量的新功能。</span><span class="sxs-lookup"><span data-stu-id="763ed-108">Visual Studio 2005 adds a significant number of new features in order to address these complaints.</span></span> <span data-ttu-id="763ed-109">對於喜歡 Visual Studio.NET 2003 中處理的 Web 應用程式編譯方式，請參閱[Web 應用程式專案](https://go.microsoft.com/fwlink/?LinkId=57870)。</span><span class="sxs-lookup"><span data-stu-id="763ed-109">For those who prefer the way that Visual Studio .NET 2003 handled compilation of Web applications, see [Web Application Projects](https://go.microsoft.com/fwlink/?LinkId=57870).</span></span>

<span data-ttu-id="763ed-110">這個模組中也涵蓋在 Web 專案建立、 管理和開發的增強功能。</span><span class="sxs-lookup"><span data-stu-id="763ed-110">In this module, well cover improvements in Web project creation, management, and development.</span></span> <span data-ttu-id="763ed-111">在更新版本的模組中，也涵蓋建置 Web 專案，以及將其部署中的增強功能。</span><span class="sxs-lookup"><span data-stu-id="763ed-111">In a later module, well cover improvements in building Web projects and deploying them.</span></span>

## <a name="frontpage-server-extensions"></a><span data-ttu-id="763ed-112">FrontPage Server Extensions</span><span class="sxs-lookup"><span data-stu-id="763ed-112">FrontPage Server Extensions</span></span>

<span data-ttu-id="763ed-113">Visual Studio.NET 2002年和 2003年所需的方塊上才能建立或建立 Web 專案的 FrontPage Server Extensions。</span><span class="sxs-lookup"><span data-stu-id="763ed-113">Visual Studio .NET 2002 and 2003 required FrontPage Server Extensions on the box in order to create or build Web projects.</span></span> <span data-ttu-id="763ed-114">開發人員有兩種不同的存取模式 （FrontPage Server Extensions 或檔案存取模式） 之間的選擇，同時用來執行工作，例如 IIS 和其他內容中設定應用程式根目錄的 FrontPage Server Extensions。</span><span class="sxs-lookup"><span data-stu-id="763ed-114">Developers did have a choice between two different access modes (FrontPage Server Extensions or File access mode), both used FrontPage Server Extensions to perform tasks such as setting the application root in IIS, etc.</span></span>

<span data-ttu-id="763ed-115">Visual Studio 2005 中移除對本機專案的 FrontPage Server Extensions 依賴。</span><span class="sxs-lookup"><span data-stu-id="763ed-115">Visual Studio 2005 removes the reliance on FrontPage Server Extensions for local projects.</span></span> <span data-ttu-id="763ed-116">Visual Studio 2005 現在會存取 IIS metabase 直接而不是使用 FrontPage Server Extensions。</span><span class="sxs-lookup"><span data-stu-id="763ed-116">Visual Studio 2005 now accesses the IIS metabase directly instead of using the FrontPage Server Extensions.</span></span> <span data-ttu-id="763ed-117">Visual Studio 2005 也會加入 FTP 會允許遠端專案存取而不需要 FrontPage Server Extensions 的支援。</span><span class="sxs-lookup"><span data-stu-id="763ed-117">Visual Studio 2005 also adds support for FTP which allows for remote project access without requiring FrontPage Server Extensions.</span></span>

<span data-ttu-id="763ed-118">這些的開發人員想要在其專案中使用 FrontPage Server Extensions，此選項仍可使用。</span><span class="sxs-lookup"><span data-stu-id="763ed-118">For those developers who want to use FrontPage Server Extensions in their projects, the option is still available.</span></span> <span data-ttu-id="763ed-119">不過，根據從 ASP.NET 開發人員社群的強式回饋，它不需要。</span><span class="sxs-lookup"><span data-stu-id="763ed-119">However, based upon strong feedback from the ASP.NET developer community, it is not a requirement.</span></span>

> [!NOTE]
> <span data-ttu-id="763ed-120">仍然需要遠端專案建立、 開啟等 FrontPage Server Extensions。</span><span class="sxs-lookup"><span data-stu-id="763ed-120">FrontPage Server Extensions are still required for remote project creation, opening, etc.</span></span>


## <a name="aspnet-development-server"></a><span data-ttu-id="763ed-121">ASP.NET 程式開發伺服器</span><span class="sxs-lookup"><span data-stu-id="763ed-121">ASP.NET Development Server</span></span>

<span data-ttu-id="763ed-122">Visual Studio 2005 隨附新的 Web 伺服器呼叫 ASP.NET 程式開發伺服器。</span><span class="sxs-lookup"><span data-stu-id="763ed-122">Visual Studio 2005 ships with a new Web server called ASP.NET Development Server.</span></span> <span data-ttu-id="763ed-123">（此 Web 伺服器先前稱為 Cassini）。</span><span class="sxs-lookup"><span data-stu-id="763ed-123">(This Web server was previously known as Cassini.)</span></span>

<span data-ttu-id="763ed-124">有數個優點的 ASP.NET 程式開發伺服器。</span><span class="sxs-lookup"><span data-stu-id="763ed-124">There are several benefits of the ASP.NET Development Server.</span></span>

- <span data-ttu-id="763ed-125">它現在可能會非系統管理員來開發及偵錯 Web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="763ed-125">It is now possible for non-Administrators to develop and debug against a Web server.</span></span>
- <span data-ttu-id="763ed-126">ASP.NET 程式開發伺服器動態會對應至任何位置虛擬目錄以彈性的專案位置的檔案系統中。</span><span class="sxs-lookup"><span data-stu-id="763ed-126">The ASP.NET Development Server dynamically maps virtual directories to any location in the file system allowing for flexible project locations.</span></span>
- <span data-ttu-id="763ed-127">在 Windows XP Professional 上已經在使用 IIS 的使用者現在可以建立新的 Web 應用程式將不會影響的其預設的網站在 IIS 中的檔案或資料夾結構。</span><span class="sxs-lookup"><span data-stu-id="763ed-127">Users on Windows XP Professional who are already using IIS will now be able to create new Web applications that will not affect the file or folder structure of their Default Web Site in IIS.</span></span>

<span data-ttu-id="763ed-128">沒有特殊的設定，才能利用 ASP.NET 程式開發伺服器。</span><span class="sxs-lookup"><span data-stu-id="763ed-128">No special configuration is required to take advantage of the ASP.NET Development Server.</span></span> <span data-ttu-id="763ed-129">當偵錯或瀏覽檔案系統裝載的 Web 專案時，Visual Studio 2005 將自動服務此要求的隨機連接埠上啟動 ASP.NET 程式開發伺服器執行個體。</span><span class="sxs-lookup"><span data-stu-id="763ed-129">When a Web project that is hosted on the file system is debugged or browsed, Visual Studio 2005 will automatically start an instance of the ASP.NET Development Server on a random port to service the request.</span></span>

<span data-ttu-id="763ed-130">稍後在此模組中的 ASP.NET 程式開發伺服器上，將涵蓋的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="763ed-130">More information will be covered on the ASP.NET Development Server later in this module.</span></span>

## <a name="improved-file-management"></a><span data-ttu-id="763ed-131">改善的檔案管理</span><span class="sxs-lookup"><span data-stu-id="763ed-131">Improved File Management</span></span>

<span data-ttu-id="763ed-132">在 Visual Studio 2002 和 2003年中，專案檔 (如 VB.NET.vbproj) 和 C#.csproj 儲存 Web 應用程式中的所有檔案的資訊。</span><span class="sxs-lookup"><span data-stu-id="763ed-132">In Visual Studio 2002 and 2003, a project file (.vbproj for VB.NET and .csproj for C#) stored information on all files in the Web application.</span></span> <span data-ttu-id="763ed-133">方案總管 中顯示為基礎的專案檔中的檔案資訊。</span><span class="sxs-lookup"><span data-stu-id="763ed-133">The Solution Explorer display is based upon the file information in the project file.</span></span> <span data-ttu-id="763ed-134">因為這個緣故，[方案總管] 會經常外部編輯器所使用的情況下顯示不正確的資訊。</span><span class="sxs-lookup"><span data-stu-id="763ed-134">Because of this, the Solution Explorer would often display inaccurate information in cases where external editors were used.</span></span> <span data-ttu-id="763ed-135">Visual Studio 2002 和 2003年會通常覆寫檔案的變更，或顯示檔案的最新版本。</span><span class="sxs-lookup"><span data-stu-id="763ed-135">Visual Studio 2002 and 2003 would often overwrite file changes or not display the most recent version of files.</span></span>

<span data-ttu-id="763ed-136">Visual Studio 2005 會立即執行專案檔。</span><span class="sxs-lookup"><span data-stu-id="763ed-136">Visual Studio 2005 does away with the project file.</span></span> <span data-ttu-id="763ed-137">相反地，它會直接從磁碟，導致您的專案中的檔案正確顯示讀取檔案和資料夾資訊。</span><span class="sxs-lookup"><span data-stu-id="763ed-137">Instead, it reads the file and folder information directly from the disk, resulting in an accurate display of the files in your project.</span></span> <span data-ttu-id="763ed-138">因為 Visual Studio 2002 和 2003年中的 [參考] 資料夾不代表 Web 應用程式中的實際資料夾，Visual Studio 2005 也會移除 [參考] 資料夾從 [方案總管]。</span><span class="sxs-lookup"><span data-stu-id="763ed-138">Because the References folder in Visual Studio 2002 and 2003 does not represent an actual folder in your Web application, Visual Studio 2005 also removes the References folder from Solution Explorer.</span></span> <span data-ttu-id="763ed-139">若要存取 Visual Studio 2005 中的專案參考，您應該使用屬性頁專案。</span><span class="sxs-lookup"><span data-stu-id="763ed-139">To access the references for your project in Visual Studio 2005, you should use the Property pages for the project.</span></span>

## <a name="creating-web-projects"></a><span data-ttu-id="763ed-140">建立 Web 專案</span><span class="sxs-lookup"><span data-stu-id="763ed-140">Creating Web Projects</span></span>

<span data-ttu-id="763ed-141">Web 開發人員有許多的新選項可用於建立專案在 Visual Studio 2005。</span><span class="sxs-lookup"><span data-stu-id="763ed-141">Web developers have many new options available for project creation in Visual Studio 2005.</span></span> <span data-ttu-id="763ed-142">Web sites 現在可以建立任意位置在檔案系統，然後進行偵錯或瀏覽 使用新的 ASP.NET 程式開發伺服器。</span><span class="sxs-lookup"><span data-stu-id="763ed-142">Web sites can now be created anywhere in the file system and can then be debugged or browsed using the new ASP.NET Development Server.</span></span> <span data-ttu-id="763ed-143">開發人員也可以建立新的網站使用 FTP。</span><span class="sxs-lookup"><span data-stu-id="763ed-143">Developers can also create new Web sites using FTP.</span></span>

<span data-ttu-id="763ed-144">若要檢視 Visual Studio 2005 中建立 Web 專案的視訊逐步解說，請按一下這裡。</span><span class="sxs-lookup"><span data-stu-id="763ed-144">Click here to view a video walkthrough of creating Web projects in Visual Studio 2005.</span></span>


![](improvements-in-visual-studio-2005/_static/image1.png)


[<span data-ttu-id="763ed-145">開啟全螢幕視訊</span><span class="sxs-lookup"><span data-stu-id="763ed-145">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a><span data-ttu-id="763ed-146">檔案系統的專案</span><span class="sxs-lookup"><span data-stu-id="763ed-146">File System Projects</span></span>

<span data-ttu-id="763ed-147">如您所見視訊逐步解說中，您可以選擇在本機電腦上或遠端位置透過檔案共用上，在檔案系統上建立網站。</span><span class="sxs-lookup"><span data-stu-id="763ed-147">As you saw in the video walkthrough, you can choose to create Web sites on the file system either on the local machine or on a remote location via a file share.</span></span> <span data-ttu-id="763ed-148">檔案系統所建立的網站的瀏覽和偵錯使用 ASP.NET 程式開發伺服器。</span><span class="sxs-lookup"><span data-stu-id="763ed-148">Web sites that are created on the file system are browsed and debugged using the ASP.NET Development Server.</span></span>

> [!NOTE]
> <span data-ttu-id="763ed-149">ASP.NET 程式開發伺服器可能會導致一些混淆的客戶。</span><span class="sxs-lookup"><span data-stu-id="763ed-149">The ASP.NET Development Server may cause some confusion for customers.</span></span> <span data-ttu-id="763ed-150">如果 IISs 目錄結構 (亦即 c: / inetpub/wwwroot) 中的檔案系統上建立 Web 專案，將仍瀏覽的網站透過從 Visual Studio 2005 內啟動時，才會進行 ASP.NET 程式開發伺服器。</span><span class="sxs-lookup"><span data-stu-id="763ed-150">If a Web project is created on the file system in IISs directory structure (i.e. c:/inetpub/wwwroot), the Web site will still be browsed via the ASP.NET Development Server when launched from within Visual Studio 2005.</span></span> <span data-ttu-id="763ed-151">因此，任何 IIS 設定 （也就是驗證方法） 不適用。</span><span class="sxs-lookup"><span data-stu-id="763ed-151">Therefore, any IIS configuration (i.e. authentication methods) is not applicable.</span></span>


<span data-ttu-id="763ed-152">預設的 web 專案也會移除許多由的額外負荷僅包含 Default.aspx 頁面、 default.cs 檔案和應用程式/（_d） 資料夾。</span><span class="sxs-lookup"><span data-stu-id="763ed-152">The default web project also removes a lot of the overhead by only includes a Default.aspx page, default.cs file, and an App/_Data folder.</span></span> <span data-ttu-id="763ed-153">有需要，會加入 web.config 和特殊資料夾 （也就是應用程式/（_c））。</span><span class="sxs-lookup"><span data-stu-id="763ed-153">The web.config and special folders (i.e. app/_code) are added as they are needed.</span></span> <span data-ttu-id="763ed-154">您的 web 專案只會包含的檔案和您需要的資料夾。</span><span class="sxs-lookup"><span data-stu-id="763ed-154">Your web project only includes the files and folders that you need.</span></span>

### <a name="http-projects"></a><span data-ttu-id="763ed-155">HTTP 專案</span><span class="sxs-lookup"><span data-stu-id="763ed-155">HTTP Projects</span></span>

<span data-ttu-id="763ed-156">HTTP 專案可以是本機 IIS 網站上或遠端網站上所建立的專案。</span><span class="sxs-lookup"><span data-stu-id="763ed-156">HTTP projects can either be projects that are created on a local IIS Web site or on a remote Web site.</span></span> <span data-ttu-id="763ed-157">預設專案位置`http://localhost`。</span><span class="sxs-lookup"><span data-stu-id="763ed-157">The default project location is `http://localhost`.</span></span> <span data-ttu-id="763ed-158">如果您按一下瀏覽按鈕時，有兩個 HTTP options： 本機 IIS 和遠端站台。</span><span class="sxs-lookup"><span data-stu-id="763ed-158">If you click the Browse button, there are two HTTP options: Local IIS and Remote Site.</span></span> <span data-ttu-id="763ed-159">這兩個選項中的主要差異是和中選擇位置對話方塊如何將檔案複製到 Web 伺服器，會顯示網站資訊的方法。</span><span class="sxs-lookup"><span data-stu-id="763ed-159">The main difference in these two options is the method in which the web site information is displayed in the Choose Location dialog and in how the files are copied to the Web server.</span></span>

<span data-ttu-id="763ed-160">本機 IIS 選項讀取站台資訊從本機電腦上 metabase，並使用檔案系統複製檔案。</span><span class="sxs-lookup"><span data-stu-id="763ed-160">The Local IIS option reads the site information from the metabase on the local machine and files are copied using the file system.</span></span> <span data-ttu-id="763ed-161">遠端站台選項使用 FrontPage Server Extensions 和站台資訊檔案會複製使用 HTTP 和 FrontPage Server 延伸 RPC 呼叫。</span><span class="sxs-lookup"><span data-stu-id="763ed-161">The Remote Site option uses the FrontPage Server Extensions and the site information and files are copied using HTTP and FrontPage Server Extensions RPC calls.</span></span>

> [!NOTE]
> <span data-ttu-id="763ed-162">無法再使用 vs###/_tmp.htm 檔和 get/_aspx/_ver.aspx 來判斷版本資訊。</span><span class="sxs-lookup"><span data-stu-id="763ed-162">The vs###/_tmp.htm file and get/_aspx/_ver.aspx are no longer used to determine version information.</span></span>


<span data-ttu-id="763ed-163">預設 HTTP 選項是本機 IIS。</span><span class="sxs-lookup"><span data-stu-id="763ed-163">The default HTTP option is Local IIS.</span></span> <span data-ttu-id="763ed-164">此選項會讀取 IIS Metabase，判斷哪些站台可用並建立內容所在的位置。</span><span class="sxs-lookup"><span data-stu-id="763ed-164">This option reads the IIS Metabase to determine which sites are available and the location in which to create content.</span></span> <span data-ttu-id="763ed-165">您可以選取樹狀檢視中選取不同的資料夾或虛擬目錄。</span><span class="sxs-lookup"><span data-stu-id="763ed-165">You can select a different folder or virtual directory by selecting it in the tree view.</span></span> <span data-ttu-id="763ed-166">您可以也建立新的虛擬目錄、 標記資料夾做為應用程式，以及從這個對話方塊中刪除現有的虛擬目錄。</span><span class="sxs-lookup"><span data-stu-id="763ed-166">You can also create a new virtual directory, mark folders as applications, as well as delete existing virtual directories from this dialog box.</span></span>


![選擇位置對話方塊](improvements-in-visual-studio-2005/_static/image1.gif)

<span data-ttu-id="763ed-168">**圖 1**: 選擇位置對話方塊</span><span class="sxs-lookup"><span data-stu-id="763ed-168">**Figure 1**: The Choose Location Dialog</span></span>


<span data-ttu-id="763ed-169">不同於在舊版的 Visual Studio 中，如果您核取**使用安全通訊端層**核取方塊，SSL 憑證不符合您在瀏覽的 URL，您會看到安全性警告對話方塊，以詢問是否會要繼續進行。</span><span class="sxs-lookup"><span data-stu-id="763ed-169">Unlike in earlier versions of Visual Studio, if you check the **Use Secure Sockets Layer** checkbox and the SSL certificate does not match the URL you are browsing, you will be presented with a Security Alert dialog asking you if you would like to proceed.</span></span> <span data-ttu-id="763ed-170">請使用 Visual Studio.NET 2003 中，如果沒有任何憑證比對一個，建立專案會失敗。</span><span class="sxs-lookup"><span data-stu-id="763ed-170">Using Visual Studio .NET 2003, if the certificate was not a matching one, creating the project would fail.</span></span>


![安全性警示有關 SSL 憑證](improvements-in-visual-studio-2005/_static/image2.gif)

<span data-ttu-id="763ed-172">**圖 2**： 有關 SSL 憑證的安全性警示</span><span class="sxs-lookup"><span data-stu-id="763ed-172">**Figure 2**: Security Alert Regarding SSL Certificate</span></span>


### <a name="note-on-host-headers"></a><span data-ttu-id="763ed-173">請注意，在 主機標頭</span><span class="sxs-lookup"><span data-stu-id="763ed-173">Note on Host Headers</span></span>

<span data-ttu-id="763ed-174">如果您要在繫結至特定 IP 的網站上建立 Web 應用程式，您必須確定已設定的主機標頭。</span><span class="sxs-lookup"><span data-stu-id="763ed-174">If you are creating a Web application on a site bound to a specific IP, you will need to ensure that a host header is configured.</span></span> <span data-ttu-id="763ed-175">否則，Visual Studio 會建立站台`http://localhost`，但 IP 位址將不會解析正確地瀏覽或在 IDE 中偵錯從站台時。</span><span class="sxs-lookup"><span data-stu-id="763ed-175">Otherwise, Visual Studio will create the site at `http://localhost`, but the IP address will not resolve correctly when the site is browsed or debugged from within the IDE.</span></span>

<span data-ttu-id="763ed-176">如果您選取的遠端站台選項時，對話方塊就會變成可讓您輸入新網站的目的地 URL。</span><span class="sxs-lookup"><span data-stu-id="763ed-176">If you select the Remote Site option, the dialog changes to allow you to enter the destination URL for the new Web site.</span></span> <span data-ttu-id="763ed-177">此 URL 必須是已啟用 FrontPage Server Extensions 的伺服器上。</span><span class="sxs-lookup"><span data-stu-id="763ed-177">This URL must be on a server that has the FrontPage Server Extensions enabled.</span></span> <span data-ttu-id="763ed-178">如果您想要搭配使用 FrontPage Server Extensions 您本機 Web 伺服器，您可以使用遠端站台選項，並指定本機 URL。</span><span class="sxs-lookup"><span data-stu-id="763ed-178">If you want to work with your local Web server using the FrontPage Server Extensions, you can use the Remote Site option and specify a local URL.</span></span>


![遠端伺服器上建立網站](improvements-in-visual-studio-2005/_static/image1.jpg)

<span data-ttu-id="763ed-180">**圖 3**： 遠端伺服器上建立網站</span><span class="sxs-lookup"><span data-stu-id="763ed-180">**Figure 3**: Creating a Web Site on a Remote Server</span></span>


<span data-ttu-id="763ed-181">如果不符合 SSL 憑證，請建立應用程式透過 SSL，遠端站台上，確認對話方塊時稍有不同於使用本機 IIS 選項時，顯示對話方塊。</span><span class="sxs-lookup"><span data-stu-id="763ed-181">When creating an application on a remote site via SSL, if the SSL certificate does not match, the confirmation dialog is slightly different than the dialog displayed when using the Local IIS option.</span></span>


![遠端站台安全性警示](improvements-in-visual-studio-2005/_static/image3.gif)

<span data-ttu-id="763ed-183">**圖 4**： 遠端站台安全性警示</span><span class="sxs-lookup"><span data-stu-id="763ed-183">**Figure 4**: The Remote Site Security Alert</span></span>


<a id="_Toc116100243"></a>

#### <a name="ftp"></a><span data-ttu-id="763ed-184">FTP</span><span class="sxs-lookup"><span data-stu-id="763ed-184">FTP</span></span>

<span data-ttu-id="763ed-185">Visual Studio 2005 導入了建立透過 FTP 網站的選項。</span><span class="sxs-lookup"><span data-stu-id="763ed-185">Visual Studio 2005 introduces the option to create Web sites via FTP.</span></span> <span data-ttu-id="763ed-186">當您使用此選項時，IDE 會在本機使用者的暫存資料夾中建立的檔案，並再使用 FTP 將檔案移到 FTP 位置。</span><span class="sxs-lookup"><span data-stu-id="763ed-186">When you use this option, the IDE creates the files locally in the users temp folder and then uses FTP to move the files to the FTP location.</span></span>

> [!NOTE]
> <span data-ttu-id="763ed-187">暫存資料夾位置是 c: / Documents and Settings /&lt;使用者&gt;本機/設定/Temp/VWDWebCache/&lt;伺服器&gt;/_&lt;應用程式名稱&gt;</span><span class="sxs-lookup"><span data-stu-id="763ed-187">The temp folder location is c:/Documents and Settings/&lt;User&gt;/Local Settings/Temp/VWDWebCache/&lt;Server&gt;/_&lt;application name&gt;</span></span>


<span data-ttu-id="763ed-188">使用 FTP 選項時，您會看到選擇位置對話方塊。</span><span class="sxs-lookup"><span data-stu-id="763ed-188">When using the FTP option, you will be presented with a Choose Location dialog.</span></span> <span data-ttu-id="763ed-189">您輸入此對話方塊，如下所示的所需的 FTP 連線資訊。</span><span class="sxs-lookup"><span data-stu-id="763ed-189">You enter the required FTP connection information into this dialog as shown below.</span></span>


![選擇 ftp 位置對話方塊](improvements-in-visual-studio-2005/_static/image2.jpg)

<span data-ttu-id="763ed-191">**圖 5**: 選擇 ftp 位置對話方塊</span><span class="sxs-lookup"><span data-stu-id="763ed-191">**Figure 5**: The Choose Location Dialog for FTP</span></span>


## <a name="lab-setup-ftp-site-and-create-a-project"></a><span data-ttu-id="763ed-192">實驗室： 設定 FTP 站台，並建立專案</span><span class="sxs-lookup"><span data-stu-id="763ed-192">Lab: Setup FTP site and create a project</span></span>

<span data-ttu-id="763ed-193">下列步驟設定 FTP 站台，讓使用者有它們只可以透過 FTP 上傳至的位置。</span><span class="sxs-lookup"><span data-stu-id="763ed-193">The following steps configure the FTP site so that a user has a location that only they can upload to via FTP.</span></span>

### <a name="install-the-ftp-service"></a><span data-ttu-id="763ed-194">安裝 FTP 服務</span><span class="sxs-lookup"><span data-stu-id="763ed-194">Install the FTP Service</span></span>

1. <span data-ttu-id="763ed-195">開啟 新增或移除程式，選取 新增/移除 Windows 元件</span><span class="sxs-lookup"><span data-stu-id="763ed-195">Open Add Remove Programs, select Add/Remove Windows Components</span></span>
2. <span data-ttu-id="763ed-196">選取 Internet Information Services （Windows 2003 上的應用程式伺服器），然後按一下**詳細資料**。</span><span class="sxs-lookup"><span data-stu-id="763ed-196">Select Internet Information Services (Application Server on Windows 2003) and click **Details**.</span></span>
3. <span data-ttu-id="763ed-197">請檢查**檔案傳輸通訊協定 (FTP) 服務**按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="763ed-197">Check **File Transfer Protocol (FTP) Service** and click **OK**.</span></span>
4. <span data-ttu-id="763ed-198">按一下**下一步**安裝 FTP 服務。</span><span class="sxs-lookup"><span data-stu-id="763ed-198">Click **Next** to install the FTP service.</span></span>

### <a name="create-a-new-folder-for-content"></a><span data-ttu-id="763ed-199">建立新的資料夾內容</span><span class="sxs-lookup"><span data-stu-id="763ed-199">Create a New Folder for Content</span></span>

1. <span data-ttu-id="763ed-200">在 Windows 檔案總管，建立新資料夾，稱為**User1** c: / inetpub/wwwroot 內。</span><span class="sxs-lookup"><span data-stu-id="763ed-200">In Windows Explorer, create a new folder called **User1** inside of c:/inetpub/wwwroot.</span></span>

#### <a name="configure-folders-and-permissions-on-folders"></a><span data-ttu-id="763ed-201">在資料夾上設定資料夾和權限。</span><span class="sxs-lookup"><span data-stu-id="763ed-201">Configure folders and permissions on folders.</span></span>

1. <span data-ttu-id="763ed-202">從系統管理工具，以開啟 [網際網路資訊服務] 嵌入式管理單元。</span><span class="sxs-lookup"><span data-stu-id="763ed-202">Open the Internet Information Services snap-in from Administrative Tools.</span></span> <span data-ttu-id="763ed-203">您現在就必須在 [電腦名稱] 節點下的 FTP 站台資料夾。</span><span class="sxs-lookup"><span data-stu-id="763ed-203">You will now have an FTP Sites folder under the computer name node.</span></span>
2. <span data-ttu-id="763ed-204">展開**FTP 站台**。</span><span class="sxs-lookup"><span data-stu-id="763ed-204">Expand **FTP Sites**.</span></span>
3. <span data-ttu-id="763ed-205">以滑鼠右鍵按一下**預設 FTP 站台**，選取**新增**，然後**虛擬目錄**，然後按一下 **下一步**。</span><span class="sxs-lookup"><span data-stu-id="763ed-205">Right-click the **Default FTP Site**, select **New**, then **Virtual Directory**, then click **Next**.</span></span>
4. <span data-ttu-id="763ed-206">輸入**User1**虛擬目錄名稱，然後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="763ed-206">Enter **User1** for the virtual directory name and click **Next**.</span></span>
5. <span data-ttu-id="763ed-207">輸入**c: / inetpub/wwwroot/User1**路徑並按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="763ed-207">Enter **c:/inetpub/wwwroot/User1** for the path and click **Next**.</span></span>
6. <span data-ttu-id="763ed-208">按一下**下一步**然後**完成**以完成精靈。</span><span class="sxs-lookup"><span data-stu-id="763ed-208">Click **Next** and then **Finish** to complete the wizard.</span></span>
7. <span data-ttu-id="763ed-209">以滑鼠右鍵按一下**User1**下預設 FTP 網站，並選取的虛擬目錄**屬性**。</span><span class="sxs-lookup"><span data-stu-id="763ed-209">Right-click the **User1** virtual directory under Default FTP Site and select **Properties**.</span></span>
8. <span data-ttu-id="763ed-210">檢查**寫入**核取方塊，按一下 **確定**以關閉對話方塊。</span><span class="sxs-lookup"><span data-stu-id="763ed-210">Check the **Write** checkbox and click **OK** to close the dialog.</span></span>
9. <span data-ttu-id="763ed-211">以滑鼠右鍵按一下**預設 FTP 站台**選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="763ed-211">Right-click **Default FTP Site** and select **Properties**.</span></span>
10. <span data-ttu-id="763ed-212">在**安全性帳戶**索引標籤上，取消核取**允許匿名連線**。</span><span class="sxs-lookup"><span data-stu-id="763ed-212">On the **Security Accounts** tab, uncheck **Allow Anonymous Connections**.</span></span>
11. <span data-ttu-id="763ed-213">按一下**是**在詢問您是否要繼續 對話方塊中。</span><span class="sxs-lookup"><span data-stu-id="763ed-213">Click **Yes** in the dialog asking if you want to continue.</span></span>
12. <span data-ttu-id="763ed-214">按一下**確定**以關閉對話方塊。</span><span class="sxs-lookup"><span data-stu-id="763ed-214">Click **OK** to close the dialog.</span></span>
13. <span data-ttu-id="763ed-215">展開**Default Web Site**下**網站**節點。</span><span class="sxs-lookup"><span data-stu-id="763ed-215">Expand the **Default Web Site** under the **Web Sites** node.</span></span>
14. <span data-ttu-id="763ed-216">以滑鼠右鍵按一下**User1**目錄中，而且選取**屬性**</span><span class="sxs-lookup"><span data-stu-id="763ed-216">Right-click the **User1** directory and select **Properties**</span></span>
15. <span data-ttu-id="763ed-217">在**應用程式設定**區段中，按一下**建立**將標示為應用程式的資料夾。</span><span class="sxs-lookup"><span data-stu-id="763ed-217">In the **Application Settings** section, click **Create** to mark the folder as an application.</span></span>
16. <span data-ttu-id="763ed-218">按一下**確定**以關閉對話方塊。</span><span class="sxs-lookup"><span data-stu-id="763ed-218">Click **OK** to close the dialog.</span></span>
17. <span data-ttu-id="763ed-219">關閉 [網際網路資訊服務] 嵌入式管理單元。</span><span class="sxs-lookup"><span data-stu-id="763ed-219">Close the Internet Information Services snap-in.</span></span>

### <a name="create-web-project"></a><span data-ttu-id="763ed-220">建立 web 專案</span><span class="sxs-lookup"><span data-stu-id="763ed-220">Create web project</span></span>

1. <span data-ttu-id="763ed-221">開啟 Visual Studio 2005。</span><span class="sxs-lookup"><span data-stu-id="763ed-221">Open Visual Studio 2005.</span></span>
2. <span data-ttu-id="763ed-222">從**檔案**功能表上，選取**新網站**。</span><span class="sxs-lookup"><span data-stu-id="763ed-222">From the **File** menu, select **New Web Site**.</span></span>
3. <span data-ttu-id="763ed-223">在**位置**下拉式清單中，選取**FTP**。</span><span class="sxs-lookup"><span data-stu-id="763ed-223">In the **Location** dropdown, select **FTP**.</span></span>
4. <span data-ttu-id="763ed-224">按一下 [瀏覽] 。</span><span class="sxs-lookup"><span data-stu-id="763ed-224">Click **Browse**.</span></span>
5. <span data-ttu-id="763ed-225">輸入**localhost**中**伺服器**文字方塊。</span><span class="sxs-lookup"><span data-stu-id="763ed-225">Enter **localhost** in the **Server** textbox.</span></span>
6. <span data-ttu-id="763ed-226">輸入**User1**目錄 文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="763ed-226">Enter **User1** in the Directory textbox.</span></span>
7. <span data-ttu-id="763ed-227">按一下**開啟**。</span><span class="sxs-lookup"><span data-stu-id="763ed-227">Click **Open**.</span></span> <span data-ttu-id="763ed-228">FTP 位置將會輸入到新的網站 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="763ed-228">The FTP location will be entered into the New Web Site dialog.</span></span>
8. <span data-ttu-id="763ed-229">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="763ed-229">Click **OK**.</span></span>
9. <span data-ttu-id="763ed-230">取消核取**匿名登入**在 FTP 登入] 對話方塊中，輸入您的認證，然後按一下 [**確定**。</span><span class="sxs-lookup"><span data-stu-id="763ed-230">Uncheck **Anonymous log on** in the FTP Log On dialog, enter your credentials, and click **OK**.</span></span>
10. <span data-ttu-id="763ed-231">什麼是專案的 URL？</span><span class="sxs-lookup"><span data-stu-id="763ed-231">What is the URL for the project?</span></span> <span data-ttu-id="763ed-232">（專案的 URL 將會顯示在 [方案總管]）。</span><span class="sxs-lookup"><span data-stu-id="763ed-232">(The URL for the project will be displayed in Solution Explorer.)</span></span>
11. <span data-ttu-id="763ed-233">從**建置**功能表上，選取**建置網站**或**建置方案**。</span><span class="sxs-lookup"><span data-stu-id="763ed-233">From the **Build** menu, select **Build Web Site** or **Build Solution**.</span></span>
12. <span data-ttu-id="763ed-234">以滑鼠右鍵按一下方案總管 中的 Default.aspx，然後選取**瀏覽器中的檢視**。</span><span class="sxs-lookup"><span data-stu-id="763ed-234">Right-click on Default.aspx in Solution Explorer and select **View in Browser**.</span></span>
13. <span data-ttu-id="763ed-235">在必須有網站 URL 對話方塊中，輸入`http://localhost/user1`URL 並按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="763ed-235">In the Web Site URL Required dialog, enter `http://localhost/user1` for the URL and click **OK**.</span></span>

> [!NOTE]
> <span data-ttu-id="763ed-236">如果您收到錯誤指出無法載入型別 /_Default，請確定您的網站和不較早的版本上執行 ASP.NET 2.0。</span><span class="sxs-lookup"><span data-stu-id="763ed-236">If you get a error indicating an inability to load the type /_Default, make sure that you are running ASP.NET 2.0 on your Web site and not an earlier version.</span></span> <span data-ttu-id="763ed-237">您可以從 Internet Information Services 中的 [ASP.NET] 索引標籤執行。</span><span class="sxs-lookup"><span data-stu-id="763ed-237">You can do that from the ASP.NET tab in Internet Information Services.</span></span>


## <a name="opening-web-projects"></a><span data-ttu-id="763ed-238">開啟 Web 專案</span><span class="sxs-lookup"><span data-stu-id="763ed-238">Opening Web Projects</span></span>

<span data-ttu-id="763ed-239">開啟 Web 專案是類似於建立專案。</span><span class="sxs-lookup"><span data-stu-id="763ed-239">Opening Web projects is similar to creating projects.</span></span> <span data-ttu-id="763ed-240">下列各節說明留意出針對在 IDE 中的工作時的區域。</span><span class="sxs-lookup"><span data-stu-id="763ed-240">The following sections call out areas to keep an eye out for while working within the IDE.</span></span> <span data-ttu-id="763ed-241">其中也涵蓋搭配使用 HTTP 和 FTP 的 Web 專案的工作。</span><span class="sxs-lookup"><span data-stu-id="763ed-241">It also covers working with Web projects using HTTP and FTP.</span></span>

<span data-ttu-id="763ed-242">若要開啟 Web 專案，請從 [檔案] 功能表中選取開啟網站。</span><span class="sxs-lookup"><span data-stu-id="763ed-242">To open a Web project, select Open Web Site from the File menu.</span></span> <span data-ttu-id="763ed-243">與先前涵蓋相同選擇位置對話方塊會提示您，並且您有相同的四個選項可供您： 檔案系統、 本機 IIS、 FTP 和遠端站台。</span><span class="sxs-lookup"><span data-stu-id="763ed-243">You will be prompted with the same Choose Location dialog covered previously and you have the same four options available to you: File System, Local IIS, FTP, and Remote Site.</span></span>

<a id="_Toc116100245"></a>

## <a name="file-system"></a><span data-ttu-id="763ed-244">檔案系統</span><span class="sxs-lookup"><span data-stu-id="763ed-244">File System</span></span>

<span data-ttu-id="763ed-245">如同先前在此模組中，Visual Studio 無法再使用專案檔。</span><span class="sxs-lookup"><span data-stu-id="763ed-245">As indicated previously in this module, Visual Studio no longer uses a project file.</span></span> <span data-ttu-id="763ed-246">因此，如果您選擇從檔案系統開啟網站，您實際上具有選擇您想，任何資料夾，即使您選擇的資料夾不建立為 Web 專案一開始在 Visual Studio 中的選項。</span><span class="sxs-lookup"><span data-stu-id="763ed-246">Therefore, if you choose to open a Web site from the file system, you actually have the option of choosing any folder that you wish, even if the folder you choose was not created as a Web project initially in Visual Studio.</span></span> <span data-ttu-id="763ed-247">例如，您可以選擇開啟為網站的 [我的文件] 資料夾，Visual Studio 將愉快地保存他們開啟它並顯示您的檔案，如下所示。</span><span class="sxs-lookup"><span data-stu-id="763ed-247">For example, you can choose to open the My Documents folder as a Web site and Visual Studio will happily open it and display your files as shown below.</span></span>


![我的網站以開啟的文件](improvements-in-visual-studio-2005/_static/image3.jpg)

<span data-ttu-id="763ed-249">**圖 6**:*我的文件*開啟為網站</span><span class="sxs-lookup"><span data-stu-id="763ed-249">**Figure 6**: *My Documents* Opened As a Web Site</span></span>


<span data-ttu-id="763ed-250">因為 Visual Studio 只會建立其他檔案和資料夾在必要時，任何其他檔案或資料夾會加入至您所開啟的位置。</span><span class="sxs-lookup"><span data-stu-id="763ed-250">Because Visual Studio only creates additional files and folders when necessary, no additional files or folders are added to the location you open.</span></span> <span data-ttu-id="763ed-251">此架構的副作用是，它會防止您從巢狀方式置於檔案系統上的網站。</span><span class="sxs-lookup"><span data-stu-id="763ed-251">A side-effect of this architecture is that it prevents you from nesting Web sites on the file system.</span></span> <span data-ttu-id="763ed-252">例如，請考慮下列目錄結構。</span><span class="sxs-lookup"><span data-stu-id="763ed-252">For example, consider the following directory structure.</span></span>

<span data-ttu-id="763ed-253">在 c: / MyWebSite web 專案</span><span class="sxs-lookup"><span data-stu-id="763ed-253">Web project at C:/MyWebSite</span></span>

<span data-ttu-id="763ed-254">在 c: / MyWebSite/巢狀的另一個 web 專案</span><span class="sxs-lookup"><span data-stu-id="763ed-254">Another web project at C:/MyWebSite/Nested</span></span>

<span data-ttu-id="763ed-255">當您開啟位於 c: / MyWebSite 網站時，巢狀資料夾會顯示為該應用程式的子資料夾。</span><span class="sxs-lookup"><span data-stu-id="763ed-255">When you open the Web site at c:/MyWebSite, the Nested folder will appear as a sub-folder of that application.</span></span>

<a id="_Toc116100246"></a>

## <a name="http"></a><span data-ttu-id="763ed-256">HTTP</span><span class="sxs-lookup"><span data-stu-id="763ed-256">HTTP</span></span>

<span data-ttu-id="763ed-257">開啟時透過 HTTP 的網站，從 IIS metabase (本機 IIS) 或使用 FrontPage Server Extensions （遠端站台。） 讀取設定如果沒有巢狀的 web 應用程式，這些會以其識別為應用程式的圖示也顯示。</span><span class="sxs-lookup"><span data-stu-id="763ed-257">When opening Web sites via HTTP, settings are read either from the IIS metabase (Local IIS) or using FrontPage Server Extensions (Remote Site.) If there are nested web applications, these are displayed as well with an icon identifying them as an application.</span></span> <span data-ttu-id="763ed-258">如果您很熟悉使用 frontpage web 應用程式，Visual Studio 2005 中的行為是類似。</span><span class="sxs-lookup"><span data-stu-id="763ed-258">If you are familiar with working with web applications in FrontPage, the behavior in Visual Studio 2005 is similar.</span></span>

<span data-ttu-id="763ed-259">即使 Visual Studio 會顯示一個圖示巢狀下方時，目前會在 IDE 中開啟應用程式的應用程式，它將不允許您展開以檢視其內容。</span><span class="sxs-lookup"><span data-stu-id="763ed-259">Even though Visual Studio will display an icon for applications that are nested beneath the application that is currently opened within the IDE, it will not allow you to expand them to see their content.</span></span> <span data-ttu-id="763ed-260">您可以不過，連按兩下加以開啟。</span><span class="sxs-lookup"><span data-stu-id="763ed-260">You can, however, double-click on them to open them.</span></span> <span data-ttu-id="763ed-261">當您這樣做時，您會看到一個對話方塊，提示您開啟 web 應用程式 （並取代目前開啟的方案），或新增至目前方案的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="763ed-261">When you do, you will be presented with a dialog prompting you to either open the web application (and replace the currently open solution) or add the Web application to your current solution.</span></span>


![巢狀的應用程式圖示上按兩下顯示的這個對話方塊](improvements-in-visual-studio-2005/_static/image4.jpg)

<span data-ttu-id="763ed-263">**圖 7**： 巢狀的應用程式圖示上按兩下顯示的這個對話方塊</span><span class="sxs-lookup"><span data-stu-id="763ed-263">**Figure 7**: Double-clicking a nested application icon presents you with this dialog</span></span>


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a><span data-ttu-id="763ed-264">FTP 站台</span><span class="sxs-lookup"><span data-stu-id="763ed-264">FTP Site</span></span>

<span data-ttu-id="763ed-265">當您開啟透過 FTP 站台時，檔案會所有在本機複製到暫存資料夾。</span><span class="sxs-lookup"><span data-stu-id="763ed-265">When you open a site via FTP, the files are all copied locally to your temp folder.</span></span> <span data-ttu-id="763ed-266">本機儲存體位置的完整路徑會顯示專案的 [屬性] 窗格中，而且會使用下列格式建立。</span><span class="sxs-lookup"><span data-stu-id="763ed-266">The full path for the local storage location is displayed in the Properties pane for the project and is created using the following format.</span></span>

<span data-ttu-id="763ed-267">C: / Documents and Settings /&lt;使用者&gt;本機/設定/Temp/VWDWebCache/&lt;伺服器&gt;/_&lt;應用程式名稱&gt;</span><span class="sxs-lookup"><span data-stu-id="763ed-267">C:/Documents and Settings/&lt;User&gt;/Local Settings/Temp/VWDWebCache/&lt;Server&gt;/_&lt;application name&gt;</span></span>

<span data-ttu-id="763ed-268">當使用 FTP，Visual Studio 必須指定專案的基底 URL，使您可以瀏覽它如下所示。</span><span class="sxs-lookup"><span data-stu-id="763ed-268">When using FTP, Visual Studio will need to specify the base URL for your project so that you can browse it as shown below.</span></span> <span data-ttu-id="763ed-269">如果您沒有指定基底 URL，Visual Studio 會要求您為其第一次您嘗試瀏覽的網站中的頁面。</span><span class="sxs-lookup"><span data-stu-id="763ed-269">If you do not specify a base URL, Visual Studio will ask you for it the first time you attempt to browse a page in the Web site.</span></span>


![指定 FTP 站台的基底 URL](improvements-in-visual-studio-2005/_static/image5.jpg)

<span data-ttu-id="763ed-271">**圖 8**: FTP 站台的指定基底 URL</span><span class="sxs-lookup"><span data-stu-id="763ed-271">**Figure 8**: Specifying a Base URL for FTP Sites</span></span>


## <a name="improvements-in-compilation"></a><span data-ttu-id="763ed-272">在編譯中的增強功能</span><span class="sxs-lookup"><span data-stu-id="763ed-272">Improvements in Compilation</span></span>

<span data-ttu-id="763ed-273">使用 Visual Studio 2005 中的 Web 應用程式的速度明顯比之前的版本。</span><span class="sxs-lookup"><span data-stu-id="763ed-273">Working with Web applications in Visual Studio 2005 is noticeably faster than previous versions.</span></span> <span data-ttu-id="763ed-274">這是因為沒有小部分編譯架構中的變更。</span><span class="sxs-lookup"><span data-stu-id="763ed-274">This is due in no small part to the changes in compilation architecture.</span></span>

<span data-ttu-id="763ed-275">在 Visual Studio 2002 和 2003年，Web 應用程式編譯成一個主要組件位於 /bin 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="763ed-275">In Visual Studio 2002 and 2003, Web applications were compiled into one primary assembly residing in the /bin folder.</span></span> <span data-ttu-id="763ed-276">在 Visual Studio 2005 中已加入應用程式/（_c） 資料夾。</span><span class="sxs-lookup"><span data-stu-id="763ed-276">In Visual Studio 2005, an App/_Code folder was added.</span></span> <span data-ttu-id="763ed-277">類別和其他非 UI 程式碼會加入至應用程式/（_c） 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="763ed-277">Classes and other non-UI code are added to the App/_Code folder.</span></span> <span data-ttu-id="763ed-278">當 Visual Studio 建置專案時，應用程式/（_c） 資料夾中的所有檔案會都編譯成單一的 App/_Code.dll 檔案。</span><span class="sxs-lookup"><span data-stu-id="763ed-278">When Visual Studio builds the project, all files in the App/_Code folder are compiled into a single App/_Code.dll file.</span></span> <span data-ttu-id="763ed-279">這項變更的結果是後續建置會比舊版更快。</span><span class="sxs-lookup"><span data-stu-id="763ed-279">The result of this change is that subsequent builds are much faster than in previous versions.</span></span>

> [!NOTE]
> <span data-ttu-id="763ed-280">MSBuild 命令列公用程式也可用來建置 ASP.NET Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="763ed-280">The MSBuild command line utility can also be used to build ASP.NET Web applications.</span></span> <span data-ttu-id="763ed-281">該工具將涵蓋單元 9。</span><span class="sxs-lookup"><span data-stu-id="763ed-281">That tool will be covered in module 9.</span></span>


<span data-ttu-id="763ed-282">另一編譯增強功能是在 建置 功能表上新的組建 頁面選項。</span><span class="sxs-lookup"><span data-stu-id="763ed-282">Another compilation enhancement is the new Build Page option on the Build menu.</span></span> <span data-ttu-id="763ed-283">這項功能可讓開發人員重建只有目前頁面 （連同，過程，和相依性），讓變更可以更快速地編譯。</span><span class="sxs-lookup"><span data-stu-id="763ed-283">This feature allows a developer to rebuild only the current page (along with, of course, and dependencies) so that changes can be compiled more quickly.</span></span> <span data-ttu-id="763ed-284">C# 不提供背景編譯為了更新 IntelliSense 等，因為它們會受益於喜歡這項功能因為這樣可讓 intellisense 快速更新只重建單一頁面。</span><span class="sxs-lookup"><span data-stu-id="763ed-284">Because C# does not offer background compilation for purposes of updating IntelliSense, etc., they will benefit immensely from this feature because it will allow for IntelliSense to be updated quickly by simply rebuilding a single page.</span></span>

<span data-ttu-id="763ed-285">專案建置屬性可讓您設定的執行啟動頁面之前發生的組建類型。</span><span class="sxs-lookup"><span data-stu-id="763ed-285">The Build properties for a project allow you to configure the type of build that occurs before the startup page is executed.</span></span> <span data-ttu-id="763ed-286">開發人員可以選擇只建置目前的頁面，讓 Visual Studio 可以更快速地偵錯應用程式，程式碼變更之後開始。</span><span class="sxs-lookup"><span data-stu-id="763ed-286">Developers can choose to only build the current page so that Visual Studio can start debugging applications more quickly after code changes.</span></span>


![組建頁面啟動動作](improvements-in-visual-studio-2005/_static/image6.jpg)

<span data-ttu-id="763ed-288">**圖 9**： 組建頁面啟動動作</span><span class="sxs-lookup"><span data-stu-id="763ed-288">**Figure 9**: The Build Page Start Action</span></span>


<span data-ttu-id="763ed-289">到 Visual Studio 和 ASP.NET 架構的另一個絕佳增強功能是在編輯的區域，並繼續。</span><span class="sxs-lookup"><span data-stu-id="763ed-289">Another great enhancement to Visual Studio and the ASP.NET architecture is in the area of edit and continue.</span></span> <span data-ttu-id="763ed-290">在 Visual Studio 2005 中，開發人員就可以開始偵錯專案和專案進行程式碼變更，而不偵錯工具中斷連結。</span><span class="sxs-lookup"><span data-stu-id="763ed-290">In Visual Studio 2005, developers can start debugging a project and make code changes on the project without detaching the debugger.</span></span> <span data-ttu-id="763ed-291">事實上，您可以依其字面開始偵錯專案時，加入新的類別、 將程式碼加入至該類別、 將程式碼加入至您建立該類別的新執行個體的頁面和執行的類別，而不需要卸離偵錯工具的方法。</span><span class="sxs-lookup"><span data-stu-id="763ed-291">In fact, you can literally start debugging a project, add a new class, add code to that class, add code to your page that creates a new instance of that class and execute a method of the class, all without detaching the debugger.</span></span> <span data-ttu-id="763ed-292">執行新的程式碼是依其字面，只要重新整理瀏覽器 ！</span><span class="sxs-lookup"><span data-stu-id="763ed-292">Executing the new code is literally as easy as refreshing the browser!</span></span>

<span data-ttu-id="763ed-293">按一下這裡查看視訊逐步解說的編輯，並繼續在 Visual Studio 2005 的功能。</span><span class="sxs-lookup"><span data-stu-id="763ed-293">Click here to see a video walkthrough of the edit and continue feature in Visual Studio 2005.</span></span>


![](improvements-in-visual-studio-2005/_static/image2.png)


[<span data-ttu-id="763ed-294">開啟全螢幕視訊</span><span class="sxs-lookup"><span data-stu-id="763ed-294">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


<span data-ttu-id="763ed-295">健全的編輯後繼續在 ASP.NET 2.0 的功能，Visual Studio 2005 是由於 ASP.NET 應用程式發生架構變更。</span><span class="sxs-lookup"><span data-stu-id="763ed-295">The robust edit and continue functionality in ASP.NET 2.0 and Visual Studio 2005 is due to an architectural change for ASP.NET applications.</span></span> <span data-ttu-id="763ed-296">在 ASP.NET 中 1.x，在 Visual Studio 2002/2003 中建立的應用程式編譯成 /bin 資料夾中儲存的主要組件。</span><span class="sxs-lookup"><span data-stu-id="763ed-296">In ASP.NET 1.x, applications created in Visual Studio 2002/2003 were compiled into a primary assembly that was stored in the /bin folder.</span></span> <span data-ttu-id="763ed-297">所有的類別，頁面等應用程式編譯成該 DLL。</span><span class="sxs-lookup"><span data-stu-id="763ed-297">All classes, pages, etc. for the application were compiled into that one DLL.</span></span> <span data-ttu-id="763ed-298">然後在執行階段，ASP.NET 會編譯的所有控制項、 標記和 ASP.NET 頁面內的程式碼，並將這些 Dll 複製到 ASP.NET 暫存資料夾。</span><span class="sxs-lookup"><span data-stu-id="763ed-298">Then at runtime, ASP.NET would compile all of the controls, markup, and ASP.NET code within pages and copy those DLLs into the ASP.NET temporary folder.</span></span>

<span data-ttu-id="763ed-299">使用 ASP.NET 2.0 中，兩個編譯模型概述上方 （一個 Visual Studio），另一個用於 ASP.NET 在執行階段的 Visual Studio 2005 中合併成一個一般編譯模型。</span><span class="sxs-lookup"><span data-stu-id="763ed-299">In Visual Studio 2005 using ASP.NET 2.0, the two compilation models outline above (one for Visual Studio and one for ASP.NET at runtime) have been merged into one common compilation model.</span></span> <span data-ttu-id="763ed-300">這表示在執行階段現在而不是在開發階段攔截所有的編譯問題。</span><span class="sxs-lookup"><span data-stu-id="763ed-300">That means that all compilation issues are now caught during the development stage instead of at runtime.</span></span> <span data-ttu-id="763ed-301">它也可讓設計工具和功能，例如使用者控制項和主版頁面的 IntelliSense 支援。</span><span class="sxs-lookup"><span data-stu-id="763ed-301">It also allows for designer and IntelliSense support for features such as user controls and master pages.</span></span>

<span data-ttu-id="763ed-302">若要查看使用者控制項的設計工具支援的視訊逐步解說，請按一下這裡。</span><span class="sxs-lookup"><span data-stu-id="763ed-302">Click here to see a video walkthrough of designer support for user controls.</span></span>


![](improvements-in-visual-studio-2005/_static/image3.png)


[<span data-ttu-id="763ed-303">開啟全螢幕視訊</span><span class="sxs-lookup"><span data-stu-id="763ed-303">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> <span data-ttu-id="763ed-304">從頁面中，移除使用者控制項時@Register指示詞會保留在標記，而且應該手動移除，以避免發生剖析器錯誤，如果網站已刪除的使用者控制項。</span><span class="sxs-lookup"><span data-stu-id="763ed-304">When a user control is removed from a page, the @Register directive remains in the markup and should be removed manually in order to avoid parser errors if the user control is deleted from the Web site.</span></span>


<span data-ttu-id="763ed-305">在 Visual Studio 編譯模型中的另一個改善的幅度發行網站功能。</span><span class="sxs-lookup"><span data-stu-id="763ed-305">Another improvement in the Visual Studio compilation model is the Publish Web Site feature.</span></span> <span data-ttu-id="763ed-306">發行功能會先行編譯網站，因為開發人員可以享有不必編譯隨選的任何項目新增的效能。</span><span class="sxs-lookup"><span data-stu-id="763ed-306">Because the Publish feature precompiles the Web site, developers can enjoy the added performance of not having to compile anything on demand.</span></span> <span data-ttu-id="763ed-307">它也會先行編譯的應用程式/（_c） 資料夾中所有原始程式碼成 DLL，讓沒有原始程式碼已部署。</span><span class="sxs-lookup"><span data-stu-id="763ed-307">It also precompiles all source code in the App/_Code folder into a DLL so that no source code has to be deployed.</span></span>


![[發行網站] 對話方塊](improvements-in-visual-studio-2005/_static/image7.jpg)

<span data-ttu-id="763ed-309">**圖 10**： 發行網站 對話方塊</span><span class="sxs-lookup"><span data-stu-id="763ed-309">**Figure 10**: The Publish Web Site Dialog</span></span>


> [!NOTE]
> <span data-ttu-id="763ed-310">Aspnet/_compile.exe 公用程式也可用來預先編譯的 ASP.NET Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="763ed-310">The aspnet/_compile.exe utility can also be used to pre-compile an ASP.NET Web application.</span></span> <span data-ttu-id="763ed-311">該工具將涵蓋單元 9。</span><span class="sxs-lookup"><span data-stu-id="763ed-311">That tool will be covered in module 9.</span></span>


<span data-ttu-id="763ed-312">當您發行網站，先行編譯的檔案儲存在暫存 ASP.NET 檔案資料夾如下所示。</span><span class="sxs-lookup"><span data-stu-id="763ed-312">When you Publish a Web site, the precompiled files are stored in the Temporary ASP.NET Files folder as shown below.</span></span> <span data-ttu-id="763ed-313">具有檔案 *.compiled*檔案延伸模組會定義特定 dll 的相依性的 XML 檔案。</span><span class="sxs-lookup"><span data-stu-id="763ed-313">Files with a *.compiled* file extension are XML files that define dependencies for particular DLLs.</span></span> <span data-ttu-id="763ed-314">任何 Webform 或使用者控制項編譯成開頭的隨機 Dll*應用程式 /_Web /_*。</span><span class="sxs-lookup"><span data-stu-id="763ed-314">Any Webform or user controls are compiled into random DLLs that begin with *App/_Web/_*.</span></span>

<span data-ttu-id="763ed-315">如果您離開*讓這個先行編譯的網站成為可更新*勾選核取方塊，內部 Webforms 和使用者控制項的標記不會先行編譯成 DLL，可讓您在部署後進行變更。</span><span class="sxs-lookup"><span data-stu-id="763ed-315">If you leave the *Allow this precompiled site to be updatable* checkbox checked, markup inside of your Webforms and user controls will not be pre-compiled into a DLL allowing you to make changes after deployment.</span></span> <span data-ttu-id="763ed-316">如果您想要鎖定的標記，以便對已部署的內容不允許，取消核取此方塊。</span><span class="sxs-lookup"><span data-stu-id="763ed-316">If you would prefer to lock down the markup so that changes to the deployed content are not allowed, uncheck this box.</span></span>

<span data-ttu-id="763ed-317">*使用固定命名和單一網頁組件*核取方塊可讓您停用批次編譯，讓每一頁會編譯固定命名的組件。</span><span class="sxs-lookup"><span data-stu-id="763ed-317">The *Use fixed naming and single page assemblies* checkbox allows you to disable batch compilation so that each page is compiled into a fixed-named assembly.</span></span> <span data-ttu-id="763ed-318">保持未核取此方塊可讓您充分利用批次編譯。</span><span class="sxs-lookup"><span data-stu-id="763ed-318">Leaving this box unchecked allows you to take advantage of batch compilation.</span></span>

<span data-ttu-id="763ed-319">*啟用強式命名的先行編譯的組件*核取方塊可讓您將強式名稱您先行編譯的組件。</span><span class="sxs-lookup"><span data-stu-id="763ed-319">The *Enable strong naming on precompiled assemblies* checkbox allows you to strong-name your precompiled assemblies.</span></span>

> [!NOTE]
> <span data-ttu-id="763ed-320">在 ASP.NET 中 1.x，強式名稱組件必須安裝到全域組件快取 (GAC) 中。</span><span class="sxs-lookup"><span data-stu-id="763ed-320">In ASP.NET 1.x, strong-named assemblies had to be installed into the Global Assembly Cache (GAC).</span></span> <span data-ttu-id="763ed-321">在 ASP.NET 2.0 中，您不需要強式名稱組件安裝到 GAC。</span><span class="sxs-lookup"><span data-stu-id="763ed-321">In ASP.NET 2.0, you are not required to install strong-named assemblies into the GAC.</span></span>


![ASP.NET 應用程式的先行編譯的檔案](improvements-in-visual-studio-2005/_static/image8.jpg)

<span data-ttu-id="763ed-323">**圖 11**: ASP.NET 應用程式的先行編譯的檔案</span><span class="sxs-lookup"><span data-stu-id="763ed-323">**Figure 11**: An ASP.NET Applications Pre-Compiled Files</span></span>


> [!NOTE]
> <span data-ttu-id="763ed-324">在上述的應用程式，沒有 web.config 檔。</span><span class="sxs-lookup"><span data-stu-id="763ed-324">In the application above, there was no web.config file.</span></span> <span data-ttu-id="763ed-325">如果有，它會呼叫*PrecompiledApp.config*之後發行的 Web 網站處理程序。</span><span class="sxs-lookup"><span data-stu-id="763ed-325">If there had been, it would have been called *PrecompiledApp.config* after the Publish Web site process.</span></span>


## <a name="improvements-in-deployment"></a><span data-ttu-id="763ed-326">在部署中的增強功能</span><span class="sxs-lookup"><span data-stu-id="763ed-326">Improvements in Deployment</span></span>

<span data-ttu-id="763ed-327">做為 Visual Studio 2002 和 2003 中，Visual Studio 2005 提供複製專案功能。</span><span class="sxs-lookup"><span data-stu-id="763ed-327">As with Visual Studio 2002 and 2003, Visual Studio 2005 offers a Copy Project feature.</span></span> <span data-ttu-id="763ed-328">不過，此功能已經過增強 Visual Studio 2005 中，且現在稱為複製網站。</span><span class="sxs-lookup"><span data-stu-id="763ed-328">However, the feature has been beefed up in Visual Studio 2005 and is now called Copy Web Site.</span></span>

<span data-ttu-id="763ed-329">複製網站對話方塊分割成左框架內，右框架。</span><span class="sxs-lookup"><span data-stu-id="763ed-329">The Copy Web Site dialog is split into a left frame and a right frame.</span></span> <span data-ttu-id="763ed-330">左框架內呼叫來源網站和右框架稱為遠端網站。</span><span class="sxs-lookup"><span data-stu-id="763ed-330">The left frame is called the Source Web Site and the right frame is called the Remote Web Site.</span></span> <span data-ttu-id="763ed-331">可能會混淆有些開發人員的一件事是，右框架中顯示的網站不一定與遠端站台。</span><span class="sxs-lookup"><span data-stu-id="763ed-331">One thing that may confuse some developers is that the site displayed in the right frame is not necessarily a remote site.</span></span> <span data-ttu-id="763ed-332">這可能是在本機檔案系統或 IIS 的本機執行個體上的站台。</span><span class="sxs-lookup"><span data-stu-id="763ed-332">It could be a site on the local file system or on the local instance of IIS.</span></span> <span data-ttu-id="763ed-333">此外，在左框架所顯示的網站不一定與來源網站對話方塊可讓您從遠端網站上發行*至*來源網站。</span><span class="sxs-lookup"><span data-stu-id="763ed-333">Additionally, the site displayed in the left frame is not necessarily the source Web site because the dialog allows you to publish from the remote Web site *to* the source Web site.</span></span>

<span data-ttu-id="763ed-334">如果您將專案複製到遠端網站，該網站必須在其上安裝的 FrontPage Server Extensions。</span><span class="sxs-lookup"><span data-stu-id="763ed-334">If you are copying a project to a remote Web site, that site must have the FrontPage Server Extensions installed on it.</span></span> <span data-ttu-id="763ed-335">如果不存在，您必須使用 FTP 連接。</span><span class="sxs-lookup"><span data-stu-id="763ed-335">If it does not, you will need to connect using FTP.</span></span> <span data-ttu-id="763ed-336">相反地，如果您將專案複製到本機 IIS 執行個體，FrontPage Server Extensions 並不需要。</span><span class="sxs-lookup"><span data-stu-id="763ed-336">On the other hand, if you are copying a project to the local IIS instance, FrontPage Server Extensions are not required.</span></span>

> [!NOTE]
> <span data-ttu-id="763ed-337">如果您嘗試在本機 IIS 執行個體上建立新的網站，且 FrontPage 2002 Server Extensions 已安裝，您會取得錯誤訊息，告知您，建立網站不支援在 SharePoint 伺服器上。</span><span class="sxs-lookup"><span data-stu-id="763ed-337">If you try to create a new Web site on the local IIS instance and the FrontPage 2002 Server Extensions are installed, you will get an error message telling you that creating Web sites is not supported on a SharePoint server.</span></span> <span data-ttu-id="763ed-338">在此情況下，您可以選擇安裝 FrontPage 2000 伺服器擴充功能或移除 FrontPage Server Extensions。</span><span class="sxs-lookup"><span data-stu-id="763ed-338">In that case, you have the option of installing the FrontPage 2000 Server Extensions or removing the FrontPage Server Extensions.</span></span>


<span data-ttu-id="763ed-339">複製網站功能的視訊逐步解說，請按一下這裡。</span><span class="sxs-lookup"><span data-stu-id="763ed-339">Click here for a video walkthrough of the Copy Web Site feature.</span></span>


![](improvements-in-visual-studio-2005/_static/image4.png)


[<span data-ttu-id="763ed-340">開啟全螢幕視訊</span><span class="sxs-lookup"><span data-stu-id="763ed-340">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a><span data-ttu-id="763ed-341">在偵錯的增強功能</span><span class="sxs-lookup"><span data-stu-id="763ed-341">Improvements in Debugging</span></span>

<span data-ttu-id="763ed-342">沒有偵錯在 Visual Studio 2005 中的四個重要改良。</span><span class="sxs-lookup"><span data-stu-id="763ed-342">There are four key improvements in debugging in Visual Studio 2005.</span></span>

- <span data-ttu-id="763ed-343">根據預設可能會以非系統管理員在本機偵錯。</span><span class="sxs-lookup"><span data-stu-id="763ed-343">Debugging locally as a non-administrator is possible out of the box.</span></span>
- <span data-ttu-id="763ed-344">Compilation 項目的偵錯屬性現在預設為 false。</span><span class="sxs-lookup"><span data-stu-id="763ed-344">The Debug attribute for the Compilation element is now false by default.</span></span>
- <span data-ttu-id="763ed-345">遠端偵錯設定和組態是比以前更容易。</span><span class="sxs-lookup"><span data-stu-id="763ed-345">Remote debugging setup and configuration is easier than before.</span></span>
- <span data-ttu-id="763ed-346">您現在可以偵錯透過 FTP 位置開啟網站。</span><span class="sxs-lookup"><span data-stu-id="763ed-346">You can now debug a Web site opened via an FTP location.</span></span>

## <a name="debugging-as-a-non-administrator"></a><span data-ttu-id="763ed-347">非系統管理員身分偵錯</span><span class="sxs-lookup"><span data-stu-id="763ed-347">Debugging as a Non-Administrator</span></span>

<span data-ttu-id="763ed-348">新增 ASP.NET 程式開發伺服器可讓非系統管理員輕鬆地偵錯現成的 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="763ed-348">The addition of the ASP.NET Development Server allows non-administrators to easily debug ASP.NET applications right out of the box.</span></span> <span data-ttu-id="763ed-349">當本機檔案系統上執行的 ASP.NET 應用程式進行偵錯時，Visual Studio 會啟動 ASP.NET 程式開發伺服器，登入使用者的身分。</span><span class="sxs-lookup"><span data-stu-id="763ed-349">When an ASP.NET application running on the local file system is debugged, Visual Studio launches the ASP.NET Development Server under the context of the logged-on user.</span></span> <span data-ttu-id="763ed-350">該使用者可以再偵錯該應用程式，而不需要任何額外的設定。</span><span class="sxs-lookup"><span data-stu-id="763ed-350">That user can then debug that application without any additional configuration.</span></span>

## <a name="debug-is-false-by-default"></a><span data-ttu-id="763ed-351">偵錯的 False 預設</span><span class="sxs-lookup"><span data-stu-id="763ed-351">Debug is False by Default</span></span>

<span data-ttu-id="763ed-352">在 ASP.NET 中 1.x*偵錯*屬性*編譯*web.config 檔案的項目已設定為*true*預設。</span><span class="sxs-lookup"><span data-stu-id="763ed-352">In ASP.NET 1.x, the *debug* attribute in the *compilation* element of the web.config file was set to *true* by default.</span></span> <span data-ttu-id="763ed-353">它一律已建議開發人員，將此屬性設*false*之前應用程式部署到生產環境中，但因為大部分的開發人員不完全了解的偵錯屬性設定為結果為 true，而只是中斷它當做-是。</span><span class="sxs-lookup"><span data-stu-id="763ed-353">It has always been recommended that developers set this attribute to *false* before deploying an application to production, but because most developers don't fully understand the consequences of leaving the debug attribute set to true, they simply left it as-is.</span></span>

<span data-ttu-id="763ed-354">最嚴重的問題與具有偵錯屬性設為 true 時，它會停用 ASP.NETs 批次編譯模型。</span><span class="sxs-lookup"><span data-stu-id="763ed-354">The most severe problem with having the debug attribute set to true is that it disables ASP.NETs batch compilation model.</span></span> <span data-ttu-id="763ed-355">因此，每一頁會編譯到個別的 DLL。</span><span class="sxs-lookup"><span data-stu-id="763ed-355">Therefore, each page is compiled into a separate DLL.</span></span> <span data-ttu-id="763ed-356">如果 Web 應用程式包含的千分位 （未當然的透過任何方式） 的頁面，這表示可幾千個小的數個 Dll 將會建立由該應用程式。</span><span class="sxs-lookup"><span data-stu-id="763ed-356">If a Web application consists of thousands of pages (not unheard of by any means), that means several thousand small DLLs will be created by that application.</span></span> <span data-ttu-id="763ed-357">雖然這些 Dll 較小的大小，則不載入記憶體中的任何特定位置。</span><span class="sxs-lookup"><span data-stu-id="763ed-357">While these DLLs are small in size, they are not loaded into any particular location in memory.</span></span> <span data-ttu-id="763ed-358">因此，它們會造成系統記憶體中的片段，以及提供意見給 OutOfMemoryException 項目。</span><span class="sxs-lookup"><span data-stu-id="763ed-358">Therefore, they cause fragmentation in system memory and can contribute to OutOfMemoryException occurrences.</span></span>

<span data-ttu-id="763ed-359">在 ASP.NET 2.0 中，偵錯屬性依預設將設定為 false。</span><span class="sxs-lookup"><span data-stu-id="763ed-359">In ASP.NET 2.0, the debug attribute is set to false by default.</span></span> <span data-ttu-id="763ed-360">當您已經看，開發人員時進行偵錯 ASP.NET 應用程式在 Visual Studio 2005 中，系統會提示他們加入已啟用偵錯的 web.config 檔案。</span><span class="sxs-lookup"><span data-stu-id="763ed-360">As you have already seen, when a developer debugs an ASP.NET application in Visual Studio 2005, they are prompted to add a web.config file with debugging enabled.</span></span> <span data-ttu-id="763ed-361">這樣做會出現在 ASP.NET 中的相同缺點 1.x，但現在開發人員清楚警告屬性應該重設為 false，再移至生產環境應用程式。</span><span class="sxs-lookup"><span data-stu-id="763ed-361">Doing so incurs the same drawbacks that were present in ASP.NET 1.x, but now the developer is clearly warned that the attribute should be reset to false before moving the application to production.</span></span>

## <a name="remote-debugging-setup-and-configuration"></a><span data-ttu-id="763ed-362">遠端偵錯設定和組態</span><span class="sxs-lookup"><span data-stu-id="763ed-362">Remote Debugging Setup and Configuration</span></span>

<span data-ttu-id="763ed-363">在 Visual Studio 2002/2003 中，遠端偵錯依賴機器偵錯 Manager (mdm.exe) 及 vs7jit.exe 程序。</span><span class="sxs-lookup"><span data-stu-id="763ed-363">In Visual Studio 2002/2003, remote debugging relied on the Machine Debug Manager (mdm.exe) and the vs7jit.exe process.</span></span> <span data-ttu-id="763ed-364">因此，疑難排解遠端偵錯問題通常是客戶黑色方塊，它通常不是比較好的產品支援服務。</span><span class="sxs-lookup"><span data-stu-id="763ed-364">Because of that, troubleshooting remote debugging problems was often a black box for customers and it was often not much better for PSS.</span></span>

<span data-ttu-id="763ed-365">Visual Studio 2005 中移除的依賴 mdm.exe 和 vs7jit.exe 程序。</span><span class="sxs-lookup"><span data-stu-id="763ed-365">Visual Studio 2005 removes the reliance on the mdm.exe and vs7jit.exe processes.</span></span> <span data-ttu-id="763ed-366">相反地，它現在會使用遠端偵錯監視服務 (msvsmon.exe)。</span><span class="sxs-lookup"><span data-stu-id="763ed-366">Instead, it now uses the Remote Debug Monitor service (msvsmon.exe.)</span></span>

<span data-ttu-id="763ed-367">遠端偵錯在 Visual Studio 2005 中的需求是相當簡單。</span><span class="sxs-lookup"><span data-stu-id="763ed-367">The requirement for debugging in Visual Studio 2005 remotely is quite simple.</span></span> <span data-ttu-id="763ed-368">您需要在之前偵錯在遠端伺服器上執行 msvsmon.exe。</span><span class="sxs-lookup"><span data-stu-id="763ed-368">You need to run msvsmon.exe on the remote server prior to debugging.</span></span> <span data-ttu-id="763ed-369">您可以從 Visual Studio 光碟片安裝遠端偵錯監視，或只要執行 msvsmon.exe 從共用而不需要完全在 Web 伺服器上安裝任何項目。</span><span class="sxs-lookup"><span data-stu-id="763ed-369">You can install the Remote Debug Monitor from the Visual Studio CD or you can simply run msvsmon.exe from a share without installing anything at all on the Web server.</span></span>

<span data-ttu-id="763ed-370">當您執行 msvsmon.exe 時，很可能會投訴封鎖遠端偵錯的連接埠。</span><span class="sxs-lookup"><span data-stu-id="763ed-370">When you run msvsmon.exe, it is likely that it will complain about ports being blocked for remote debugging.</span></span> <span data-ttu-id="763ed-371">幸運的是，您可以輕鬆地解除封鎖從 [警告] 對話方塊中的右連接埠如下所示。</span><span class="sxs-lookup"><span data-stu-id="763ed-371">Fortunately, you can easily unblock the ports from right within the warning dialog as shown below.</span></span>


![Windows 防火牆會封鎖遠端偵錯的通知](improvements-in-visual-studio-2005/_static/image9.jpg)

<span data-ttu-id="763ed-373">**圖 12**： 通知 Windows 防火牆會封鎖遠端偵錯</span><span class="sxs-lookup"><span data-stu-id="763ed-373">**Figure 12**: Notification that Windows Firewall is Blocking Remote Debugging</span></span>


<span data-ttu-id="763ed-374">一旦您已解除封鎖偵錯所需的連接埠，您會看到遠端的偵錯監視，如下所示。</span><span class="sxs-lookup"><span data-stu-id="763ed-374">Once you have unblocked the ports necessary for debugging, you will see the Remote Debugging Monitor as shown below.</span></span> <span data-ttu-id="763ed-375">從這個介面，您可以監視連線，並變更輕鬆地偵錯權限。</span><span class="sxs-lookup"><span data-stu-id="763ed-375">From this interface, you can monitor connections and change debugging permissions easily.</span></span>


![遠端偵錯監視](improvements-in-visual-studio-2005/_static/image10.jpg)

<span data-ttu-id="763ed-377">**圖 13**： 遠端偵錯監視</span><span class="sxs-lookup"><span data-stu-id="763ed-377">**Figure 13**: The Remote Debugging Monitor</span></span>


<span data-ttu-id="763ed-378">它也可進行遠端偵錯 Web 應用程式開啟透過 FTP。</span><span class="sxs-lookup"><span data-stu-id="763ed-378">It is also possible to remotely debug a Web application opened via FTP.</span></span> <span data-ttu-id="763ed-379">會以先前未包含相同的步驟。</span><span class="sxs-lookup"><span data-stu-id="763ed-379">The steps are the same as those previously covered.</span></span> <span data-ttu-id="763ed-380">不過，您必須指定基底 URL 的瀏覽 FTP 專案稍早在此模組中所述。</span><span class="sxs-lookup"><span data-stu-id="763ed-380">However, you will need to specify a base URL for browsing the FTP project as outlined earlier in this module.</span></span>

## <a name="lab-2"></a><span data-ttu-id="763ed-381">Lab 2</span><span class="sxs-lookup"><span data-stu-id="763ed-381">Lab 2</span></span>

## <a name="remote-debugging-with-visual-studio-2005"></a><span data-ttu-id="763ed-382">Visual Studio 2005 的遠端偵錯</span><span class="sxs-lookup"><span data-stu-id="763ed-382">Remote Debugging with Visual Studio 2005</span></span>

<span data-ttu-id="763ed-383">這個實驗室將引導您使用 Visual Studio 2005 的遠端偵錯。</span><span class="sxs-lookup"><span data-stu-id="763ed-383">This lab will walk you through remote debugging with Visual Studio 2005.</span></span>

<span data-ttu-id="763ed-384">如本實驗室的視訊逐步解說，請按一下這裡。</span><span class="sxs-lookup"><span data-stu-id="763ed-384">Click here for a video walkthrough of this lab.</span></span>


![](improvements-in-visual-studio-2005/_static/image5.png)


[<span data-ttu-id="763ed-385">開啟全螢幕視訊</span><span class="sxs-lookup"><span data-stu-id="763ed-385">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


<span data-ttu-id="763ed-386">這個實驗室中必須要有一個執行的 Visual Studio 2005 和其他執行 IIS 5 或更高的兩部電腦。</span><span class="sxs-lookup"><span data-stu-id="763ed-386">This lab requires you to have two machines, one running Visual Studio 2005 and the other running IIS 5 or greater.</span></span>

1. <span data-ttu-id="763ed-387">開啟 Visual Studio 2005，並在遠端伺服器上建立新的網站。</span><span class="sxs-lookup"><span data-stu-id="763ed-387">Open Visual Studio 2005 and create a new Web site on the remote server.</span></span>

> [!NOTE]
> <span data-ttu-id="763ed-388">遠端的 IIS 執行個體上或透過 FTP，您可以建立網站。</span><span class="sxs-lookup"><span data-stu-id="763ed-388">You can create the Web site on a remote IIS instance or via FTP.</span></span>


1. <span data-ttu-id="763ed-389">從遠端的 Web 伺服器，將 msvsmon.exe 在開發機器上使用 UNC 路徑，並加以執行。</span><span class="sxs-lookup"><span data-stu-id="763ed-389">From the remote Web server, locate msvsmon.exe on the development machine using a UNC path and execute it.</span></span>  
 <span data-ttu-id="763ed-390">Msvsmon.exe 的預設位置是 //server/c$/Program 檔案/Microsoft Visual Studio 8/Common7/IDE/遠端偵錯工具/x86。</span><span class="sxs-lookup"><span data-stu-id="763ed-390">The default location for msvsmon.exe is //server/c$/Program Files/Microsoft Visual Studio 8/Common7/IDE/Remote Debugger/x86.</span></span>
2. <span data-ttu-id="763ed-391">如果系統提示您解除封鎖通訊埠進行遠端偵錯時，這樣做。</span><span class="sxs-lookup"><span data-stu-id="763ed-391">If prompted to unblock ports for remote debugging, do so.</span></span>
3. <span data-ttu-id="763ed-392">從開發電腦中，開啟程式碼後置 Default.aspx，然後頁面/（_l） 方法中設定中斷點。</span><span class="sxs-lookup"><span data-stu-id="763ed-392">From the development machine, open the code-behind for Default.aspx and set a breakpoint in the Page/_Load method.</span></span>
4. <span data-ttu-id="763ed-393">開始偵錯從開發電腦。</span><span class="sxs-lookup"><span data-stu-id="763ed-393">Start debugging from the development machine.</span></span>

<span data-ttu-id="763ed-394">您應該如預期般，叫用中斷點。</span><span class="sxs-lookup"><span data-stu-id="763ed-394">You should hit the breakpoint as expected.</span></span>

## <a name="aspnet-development-server"></a><span data-ttu-id="763ed-395">ASP.NET 程式開發伺服器</span><span class="sxs-lookup"><span data-stu-id="763ed-395">ASP.NET Development Server</span></span>

<span data-ttu-id="763ed-396">為 weve 已經討論過，Visual Studio 2005 隨附呼叫 ASP.NET 程式開發伺服器的 Web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="763ed-396">As weve already discussed, Visual Studio 2005 ships with a Web server called the ASP.NET Development Server.</span></span> <span data-ttu-id="763ed-397">（ASP.NET 程式開發伺服器有時稱為 Cassini。）此 Web 伺服器是方便瀏覽和偵錯 Web 應用程式在檔案系統上執行。</span><span class="sxs-lookup"><span data-stu-id="763ed-397">(The ASP.NET Development Server is sometimes referred to as Cassini.) This Web server is a convenient means to browse and debug Web applications running on the file system.</span></span>

<span data-ttu-id="763ed-398">ASP.NET 程式開發伺服器是受限制的 Web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="763ed-398">The ASP.NET Development Server is a restricted Web server.</span></span> <span data-ttu-id="763ed-399">不允許遠端連接，所以不允許啟動 Web 伺服器的使用者以外的任何使用者從任何要求。</span><span class="sxs-lookup"><span data-stu-id="763ed-399">It does not allow remote connections, it does not allow any requests from any user other than the user who started the Web server.</span></span> <span data-ttu-id="763ed-400">它也沒有為 ASP 網頁的功能。</span><span class="sxs-lookup"><span data-stu-id="763ed-400">It also does not have the capability of serving ASP pages.</span></span> <span data-ttu-id="763ed-401">只有 ASP.NET 資源和 HTML 資源 （包括影像、 CSS 檔案等） 都有受到處理。</span><span class="sxs-lookup"><span data-stu-id="763ed-401">Only ASP.NET resources and HTML resources (including images, CSS files, etc.) are served.</span></span>

<span data-ttu-id="763ed-402">可以透過命令列啟動 ASP.NET 程式開發伺服器，執行 WebDev.WebServer.exe 檔案位於 c:/Windows/Microsoft.NET/Framework/v2.0./*/* /  */*/\*.</span><span class="sxs-lookup"><span data-stu-id="763ed-402">The ASP.NET Development Server can be launched via the command line by running the WebDev.WebServer.exe file located at c:/Windows/Microsoft.NET/Framework/v2.0./*/*/*/*/\*.</span></span> <span data-ttu-id="763ed-403">下列對話方塊會顯示可用的參數。</span><span class="sxs-lookup"><span data-stu-id="763ed-403">The following dialog displays the parameters that are available.</span></span>


![](improvements-in-visual-studio-2005/_static/image11.jpg)

<span data-ttu-id="763ed-404">**圖 14**</span><span class="sxs-lookup"><span data-stu-id="763ed-404">**Figure 14**</span></span>


> [!NOTE]
> <span data-ttu-id="763ed-405">ASP.NET 程式開發伺服器不支援透過命令列明確啟動時。</span><span class="sxs-lookup"><span data-stu-id="763ed-405">The ASP.NET Development Server is not supported when launched explicitly via the command line.</span></span>
