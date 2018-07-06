---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: 在 Visual Studio 2005 中的改良 |Microsoft Docs
author: microsoft
description: Visual Studio 2005 提供一長串改進和增強功能，Web 專案的 Web 應用程式開發人員。
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: 0a42699381fd326891898e01b4e98662e9ce22bf
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811366"
---
<a name="improvements-in-visual-studio-2005"></a><span data-ttu-id="5e5b6-103">在 Visual Studio 2005 的增強功能</span><span class="sxs-lookup"><span data-stu-id="5e5b6-103">Improvements in Visual Studio 2005</span></span>
====================
<span data-ttu-id="5e5b6-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="5e5b6-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="5e5b6-105">Visual Studio 2005 提供一長串改進和增強功能，Web 專案的 Web 應用程式開發人員。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-105">Visual Studio 2005 provides Web application developers with a long list of improvements and enhancements to Web projects.</span></span>


<span data-ttu-id="5e5b6-106">Visual Studio 2005 提供一長串改進和增強功能，Web 專案的 Web 應用程式開發人員。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-106">Visual Studio 2005 provides Web application developers with a long list of improvements and enhancements to Web projects.</span></span> <span data-ttu-id="5e5b6-107">強大的 Visual Studio.NET 2002年和 2003年是，沒有多抱怨 Web 專案已處理的方式。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-107">As powerful as Visual Studio .NET 2002 and 2003 are, there were many complaints in the way that Web projects were handled.</span></span> <span data-ttu-id="5e5b6-108">Visual Studio 2005 加入大量的新功能，若要解決這些抱怨。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-108">Visual Studio 2005 adds a significant number of new features in order to address these complaints.</span></span> <span data-ttu-id="5e5b6-109">對於喜歡 Visual Studio.NET 2003年處理的 Web 應用程式編譯的方式，請參閱[Web 應用程式專案](https://go.microsoft.com/fwlink/?LinkId=57870)。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-109">For those who prefer the way that Visual Studio .NET 2003 handled compilation of Web applications, see [Web Application Projects](https://go.microsoft.com/fwlink/?LinkId=57870).</span></span>

<span data-ttu-id="5e5b6-110">在這個模組中，我們將介紹中建立 Web 專案、 管理和開發的增強功能。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-110">In this module, well cover improvements in Web project creation, management, and development.</span></span> <span data-ttu-id="5e5b6-111">在更新版本的模組中，我們將介紹增強功能，建置 Web 專案，並將其部署。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-111">In a later module, well cover improvements in building Web projects and deploying them.</span></span>

## <a name="frontpage-server-extensions"></a><span data-ttu-id="5e5b6-112">FrontPage Server Extensions</span><span class="sxs-lookup"><span data-stu-id="5e5b6-112">FrontPage Server Extensions</span></span>

<span data-ttu-id="5e5b6-113">Visual Studio.NET 2002年和 2003年所需的方塊上才能建立或建立 Web 專案的 FrontPage Server Extensions。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-113">Visual Studio .NET 2002 and 2003 required FrontPage Server Extensions on the box in order to create or build Web projects.</span></span> <span data-ttu-id="5e5b6-114">開發人員有兩種不同的存取模式 （FrontPage Server Extensions 或檔案存取模式） 之間做選擇，同時用來執行工作，例如設定 IIS 等等的應用程式根目錄的 FrontPage Server Extensions。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-114">Developers did have a choice between two different access modes (FrontPage Server Extensions or File access mode), both used FrontPage Server Extensions to perform tasks such as setting the application root in IIS, etc.</span></span>

<span data-ttu-id="5e5b6-115">Visual Studio 2005 會依賴 FrontPage Server Extensions 的本機專案中移除。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-115">Visual Studio 2005 removes the reliance on FrontPage Server Extensions for local projects.</span></span> <span data-ttu-id="5e5b6-116">Visual Studio 2005 現在會存取 IIS metabase 直接而不是使用 FrontPage Server Extensions。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-116">Visual Studio 2005 now accesses the IIS metabase directly instead of using the FrontPage Server Extensions.</span></span> <span data-ttu-id="5e5b6-117">Visual Studio 2005 也會新增為 FTP 以達到遠端專案存取權，而不需要 FrontPage Server Extensions 的支援。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-117">Visual Studio 2005 also adds support for FTP which allows for remote project access without requiring FrontPage Server Extensions.</span></span>

<span data-ttu-id="5e5b6-118">開發人員想要在其專案中使用 FrontPage Server Extensions 而言，此選項為仍然可用。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-118">For those developers who want to use FrontPage Server Extensions in their projects, the option is still available.</span></span> <span data-ttu-id="5e5b6-119">不過，根據從 ASP.NET 開發人員社群的強式回饋，它不需要。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-119">However, based upon strong feedback from the ASP.NET developer community, it is not a requirement.</span></span>

> [!NOTE]
> <span data-ttu-id="5e5b6-120">仍然需要遠端專案建立、 開啟等 FrontPage Server Extensions。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-120">FrontPage Server Extensions are still required for remote project creation, opening, etc.</span></span>


## <a name="aspnet-development-server"></a><span data-ttu-id="5e5b6-121">ASP.NET 程式開發伺服器</span><span class="sxs-lookup"><span data-stu-id="5e5b6-121">ASP.NET Development Server</span></span>

<span data-ttu-id="5e5b6-122">Visual Studio 2005 隨附新的 Web 伺服器，稱為 ASP.NET 程式開發伺服器。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-122">Visual Studio 2005 ships with a new Web server called ASP.NET Development Server.</span></span> <span data-ttu-id="5e5b6-123">（此網頁伺服器先前稱為 Cassini）。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-123">(This Web server was previously known as Cassini.)</span></span>

<span data-ttu-id="5e5b6-124">有許多的 ASP.NET 程式開發伺服器的數個優點。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-124">There are several benefits of the ASP.NET Development Server.</span></span>

- <span data-ttu-id="5e5b6-125">現在，則為非系統管理員能夠開發和偵錯對 Web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-125">It is now possible for non-Administrators to develop and debug against a Web server.</span></span>
- <span data-ttu-id="5e5b6-126">ASP.NET Development Server 動態虛擬目錄中對應至任何位置以彈性的專案位置的檔案系統。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-126">The ASP.NET Development Server dynamically maps virtual directories to any location in the file system allowing for flexible project locations.</span></span>
- <span data-ttu-id="5e5b6-127">在 Windows XP Professional 上已經在使用 IIS 的使用者現在可以建立新的 Web 應用程式將不會影響其預設網站上的 IIS 中的檔案或資料夾結構。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-127">Users on Windows XP Professional who are already using IIS will now be able to create new Web applications that will not affect the file or folder structure of their Default Web Site in IIS.</span></span>

<span data-ttu-id="5e5b6-128">任何特殊設定，不才能充分善用 ASP.NET 程式開發伺服器。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-128">No special configuration is required to take advantage of the ASP.NET Development Server.</span></span> <span data-ttu-id="5e5b6-129">當偵錯或瀏覽檔案系統裝載的 Web 專案時，Visual Studio 2005 會自動啟動 ASP.NET 程式開發伺服器的執行個體隨機的連接埠，服務要求。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-129">When a Web project that is hosted on the file system is debugged or browsed, Visual Studio 2005 will automatically start an instance of the ASP.NET Development Server on a random port to service the request.</span></span>

<span data-ttu-id="5e5b6-130">在 ASP.NET Development Server，稍後在本單元中，將會說明的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-130">More information will be covered on the ASP.NET Development Server later in this module.</span></span>

## <a name="improved-file-management"></a><span data-ttu-id="5e5b6-131">改善的檔案管理</span><span class="sxs-lookup"><span data-stu-id="5e5b6-131">Improved File Management</span></span>

<span data-ttu-id="5e5b6-132">在 Visual Studio 2002 和 2003年中，(.vbproj vb.net) 和 C# 的.csproj 專案檔會儲存在 Web 應用程式中的所有檔案資訊。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-132">In Visual Studio 2002 and 2003, a project file (.vbproj for VB.NET and .csproj for C#) stored information on all files in the Web application.</span></span> <span data-ttu-id="5e5b6-133">方案總管 中顯示為基礎的專案檔中的檔案資訊。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-133">The Solution Explorer display is based upon the file information in the project file.</span></span> <span data-ttu-id="5e5b6-134">因為這個緣故，[方案總管] 會通常在其中外部編輯器所使用的情況下顯示不正確的資訊。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-134">Because of this, the Solution Explorer would often display inaccurate information in cases where external editors were used.</span></span> <span data-ttu-id="5e5b6-135">Visual Studio 2002 和 2003年會經常覆寫檔案的變更，或顯示檔案的最新版本。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-135">Visual Studio 2002 and 2003 would often overwrite file changes or not display the most recent version of files.</span></span>

<span data-ttu-id="5e5b6-136">Visual Studio 2005 就立即與專案檔。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-136">Visual Studio 2005 does away with the project file.</span></span> <span data-ttu-id="5e5b6-137">相反地，它會讀取檔案和資料夾資訊直接從磁碟，導致您的專案中的檔案正確顯示。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-137">Instead, it reads the file and folder information directly from the disk, resulting in an accurate display of the files in your project.</span></span> <span data-ttu-id="5e5b6-138">因為 Visual Studio 2002 和 2003年中的 [參考] 資料夾不代表 Web 應用程式中的實際資料夾，Visual Studio 2005 也會移除 [參考] 資料夾從 [方案總管]。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-138">Because the References folder in Visual Studio 2002 and 2003 does not represent an actual folder in your Web application, Visual Studio 2005 also removes the References folder from Solution Explorer.</span></span> <span data-ttu-id="5e5b6-139">若要存取 Visual Studio 2005 的專案中的參考，您應該使用專案的屬性頁。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-139">To access the references for your project in Visual Studio 2005, you should use the Property pages for the project.</span></span>

## <a name="creating-web-projects"></a><span data-ttu-id="5e5b6-140">建立 Web 專案</span><span class="sxs-lookup"><span data-stu-id="5e5b6-140">Creating Web Projects</span></span>

<span data-ttu-id="5e5b6-141">Web 開發人員在 Visual Studio 2005 中，有許多的新選項可用於建立專案。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-141">Web developers have many new options available for project creation in Visual Studio 2005.</span></span> <span data-ttu-id="5e5b6-142">Web sites 現在可以建立任何位置在檔案系統，然後進行偵錯或瀏覽 使用新的 ASP.NET 程式開發伺服器。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-142">Web sites can now be created anywhere in the file system and can then be debugged or browsed using the new ASP.NET Development Server.</span></span> <span data-ttu-id="5e5b6-143">開發人員也可以建立新的網站使用 FTP。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-143">Developers can also create new Web sites using FTP.</span></span>

<span data-ttu-id="5e5b6-144">按一下這裡可觀看視訊逐步解說在 Visual Studio 2005 中建立 Web 專案。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-144">Click here to view a video walkthrough of creating Web projects in Visual Studio 2005.</span></span>


![](improvements-in-visual-studio-2005/_static/image1.png)


[<span data-ttu-id="5e5b6-145">開啟全螢幕影片</span><span class="sxs-lookup"><span data-stu-id="5e5b6-145">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a><span data-ttu-id="5e5b6-146">檔案系統的專案</span><span class="sxs-lookup"><span data-stu-id="5e5b6-146">File System Projects</span></span>

<span data-ttu-id="5e5b6-147">如您所見的視訊逐步解說中，您可以選擇在本機電腦上或遠端位置透過檔案共用上的檔案系統上建立網站。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-147">As you saw in the video walkthrough, you can choose to create Web sites on the file system either on the local machine or on a remote location via a file share.</span></span> <span data-ttu-id="5e5b6-148">檔案系統所建立的網站的瀏覽和偵錯使用 ASP.NET 程式開發伺服器。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-148">Web sites that are created on the file system are browsed and debugged using the ASP.NET Development Server.</span></span>

> [!NOTE]
> <span data-ttu-id="5e5b6-149">ASP.NET 程式開發伺服器可能會造成一些混淆的客戶。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-149">The ASP.NET Development Server may cause some confusion for customers.</span></span> <span data-ttu-id="5e5b6-150">如果 IISs 目錄結構 (亦即 c: / inetpub/wwwroot) 中的檔案系統上建立 Web 專案，則將仍瀏覽的網站透過從 Visual Studio 2005 內啟動時，才會進行 ASP.NET 程式開發伺服器。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-150">If a Web project is created on the file system in IISs directory structure (i.e. c:/inetpub/wwwroot), the Web site will still be browsed via the ASP.NET Development Server when launched from within Visual Studio 2005.</span></span> <span data-ttu-id="5e5b6-151">因此，任何 IIS 設定 （也就是驗證方法） 不適用。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-151">Therefore, any IIS configuration (i.e. authentication methods) is not applicable.</span></span>


<span data-ttu-id="5e5b6-152">預設的 web 專案也會移除許多的額外負荷，藉由只包含 Default.aspx 頁面、 default.cs 檔案和應用程式/資料 （_d） 資料夾。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-152">The default web project also removes a lot of the overhead by only includes a Default.aspx page, default.cs file, and an App/_Data folder.</span></span> <span data-ttu-id="5e5b6-153">如有需要會加入 web.config 和特殊資料夾 （也就是應用程式/（_c））。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-153">The web.config and special folders (i.e. app/_code) are added as they are needed.</span></span> <span data-ttu-id="5e5b6-154">您的 web 專案只包含的檔案和您所需要的資料夾。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-154">Your web project only includes the files and folders that you need.</span></span>

### <a name="http-projects"></a><span data-ttu-id="5e5b6-155">HTTP 專案</span><span class="sxs-lookup"><span data-stu-id="5e5b6-155">HTTP Projects</span></span>

<span data-ttu-id="5e5b6-156">HTTP 專案可以是本機 IIS 網站上或遠端網站上所建立的專案。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-156">HTTP projects can either be projects that are created on a local IIS Web site or on a remote Web site.</span></span> <span data-ttu-id="5e5b6-157">預設專案位置是`http://localhost`。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-157">The default project location is `http://localhost`.</span></span> <span data-ttu-id="5e5b6-158">如果您按一下 [瀏覽] 按鈕時，有兩個 HTTP 選項： 本機 IIS 和遠端站台。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-158">If you click the Browse button, there are two HTTP options: Local IIS and Remote Site.</span></span> <span data-ttu-id="5e5b6-159">這兩個選項中的主要差異是在 選擇位置對話方塊和如何將檔案複製到 Web 伺服器，會顯示網站資訊的方法。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-159">The main difference in these two options is the method in which the web site information is displayed in the Choose Location dialog and in how the files are copied to the Web server.</span></span>

<span data-ttu-id="5e5b6-160">本機 IIS 選項會讀取在本機電腦上的 metabase 中的站台資訊，並使用檔案系統複製檔案。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-160">The Local IIS option reads the site information from the metabase on the local machine and files are copied using the file system.</span></span> <span data-ttu-id="5e5b6-161">遠端站台選項使用 FrontPage Server Extensions 和站台資訊會複製檔案，使用 HTTP 和 FrontPage Server Extensions RPC 呼叫。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-161">The Remote Site option uses the FrontPage Server Extensions and the site information and files are copied using HTTP and FrontPage Server Extensions RPC calls.</span></span>

> [!NOTE]
> <span data-ttu-id="5e5b6-162">Vs###/_tmp.htm 檔案，並在 get/_aspx/_ver.aspx 不再會用來判斷版本資訊中。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-162">The vs###/_tmp.htm file and get/_aspx/_ver.aspx are no longer used to determine version information.</span></span>


<span data-ttu-id="5e5b6-163">預設 HTTP 選項是 本機 IIS。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-163">The default HTTP option is Local IIS.</span></span> <span data-ttu-id="5e5b6-164">此選項會讀取 IIS Metabase 來判斷哪一個站台可用並在其中建立內容的位置。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-164">This option reads the IIS Metabase to determine which sites are available and the location in which to create content.</span></span> <span data-ttu-id="5e5b6-165">您可以選取樹狀檢視中選取不同的資料夾或虛擬目錄。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-165">You can select a different folder or virtual directory by selecting it in the tree view.</span></span> <span data-ttu-id="5e5b6-166">您可以也建立新的虛擬目錄、 標示為應用程式的資料夾，以及從這個對話方塊中刪除現有的虛擬目錄。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-166">You can also create a new virtual directory, mark folders as applications, as well as delete existing virtual directories from this dialog box.</span></span>


![選擇位置對話方塊](improvements-in-visual-studio-2005/_static/image1.gif)

<span data-ttu-id="5e5b6-168">**圖 1**: 選擇位置對話方塊</span><span class="sxs-lookup"><span data-stu-id="5e5b6-168">**Figure 1**: The Choose Location Dialog</span></span>


<span data-ttu-id="5e5b6-169">不同於在舊版的 Visual Studio 中，如果您勾選**使用安全通訊端層**核取方塊和 SSL 憑證不符合您瀏覽的 URL，您會看到安全性警示對話方塊，詢問您，是否您想要繼續進行。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-169">Unlike in earlier versions of Visual Studio, if you check the **Use Secure Sockets Layer** checkbox and the SSL certificate does not match the URL you are browsing, you will be presented with a Security Alert dialog asking you if you would like to proceed.</span></span> <span data-ttu-id="5e5b6-170">如果憑證不相符的一個，請使用 Visual Studio.NET 2003年，建立專案會失敗。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-170">Using Visual Studio .NET 2003, if the certificate was not a matching one, creating the project would fail.</span></span>


![安全性警示的相關 SSL 憑證](improvements-in-visual-studio-2005/_static/image2.gif)

<span data-ttu-id="5e5b6-172">**圖 2**: SSL 憑證有關的安全性警示</span><span class="sxs-lookup"><span data-stu-id="5e5b6-172">**Figure 2**: Security Alert Regarding SSL Certificate</span></span>


### <a name="note-on-host-headers"></a><span data-ttu-id="5e5b6-173">請注意，在 主機標頭</span><span class="sxs-lookup"><span data-stu-id="5e5b6-173">Note on Host Headers</span></span>

<span data-ttu-id="5e5b6-174">如果您要繫結至特定的 IP 站台上建立 Web 應用程式，您必須確定已設定的主機標頭。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-174">If you are creating a Web application on a site bound to a specific IP, you will need to ensure that a host header is configured.</span></span> <span data-ttu-id="5e5b6-175">否則，Visual Studio 會建立在站台`http://localhost`，但網站瀏覽或從 IDE 內進行偵錯時無法正確解析 IP 位址。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-175">Otherwise, Visual Studio will create the site at `http://localhost`, but the IP address will not resolve correctly when the site is browsed or debugged from within the IDE.</span></span>

<span data-ttu-id="5e5b6-176">如果您選取 [遠端站台] 選項時，對話方塊就會變成可讓您輸入新的網站的目的地 URL。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-176">If you select the Remote Site option, the dialog changes to allow you to enter the destination URL for the new Web site.</span></span> <span data-ttu-id="5e5b6-177">此 URL 必須是已啟用 FrontPage Server Extensions 的伺服器上。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-177">This URL must be on a server that has the FrontPage Server Extensions enabled.</span></span> <span data-ttu-id="5e5b6-178">如果您想要搭配您使用 FrontPage Server Extensions 的本機 Web 伺服器，您可以使用 [遠端站台] 選項，並指定本機 URL。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-178">If you want to work with your local Web server using the FrontPage Server Extensions, you can use the Remote Site option and specify a local URL.</span></span>


![在遠端伺服器上建立網站](improvements-in-visual-studio-2005/_static/image1.jpg)

<span data-ttu-id="5e5b6-180">**圖 3**： 遠端伺服器上建立網站</span><span class="sxs-lookup"><span data-stu-id="5e5b6-180">**Figure 3**: Creating a Web Site on a Remote Server</span></span>


<span data-ttu-id="5e5b6-181">如果不符合 SSL 憑證，請在透過 SSL，遠端站台上建立應用程式，確認對話方塊時稍有不同於使用本機 IIS 選項時，顯示對話方塊。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-181">When creating an application on a remote site via SSL, if the SSL certificate does not match, the confirmation dialog is slightly different than the dialog displayed when using the Local IIS option.</span></span>


![遠端站台的安全性警示](improvements-in-visual-studio-2005/_static/image3.gif)

<span data-ttu-id="5e5b6-183">**圖 4**： 遠端站台的安全性警示</span><span class="sxs-lookup"><span data-stu-id="5e5b6-183">**Figure 4**: The Remote Site Security Alert</span></span>


<a id="_Toc116100243"></a>

#### <a name="ftp"></a><span data-ttu-id="5e5b6-184">FTP</span><span class="sxs-lookup"><span data-stu-id="5e5b6-184">FTP</span></span>

<span data-ttu-id="5e5b6-185">Visual Studio 2005 將介紹建立透過 FTP 網站的選項。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-185">Visual Studio 2005 introduces the option to create Web sites via FTP.</span></span> <span data-ttu-id="5e5b6-186">當您使用此選項時，IDE 就會在本機建立的使用者暫存資料夾中的檔案，並接著使用 FTP 將檔案移到 FTP 位置。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-186">When you use this option, the IDE creates the files locally in the users temp folder and then uses FTP to move the files to the FTP location.</span></span>

> [!NOTE]
> <span data-ttu-id="5e5b6-187">暫存資料夾位置是 c: / Documents and Settings /&lt;使用者&gt;本機/設定/Temp/VWDWebCache/&lt;伺服器&gt;/_&lt;應用程式名稱&gt;</span><span class="sxs-lookup"><span data-stu-id="5e5b6-187">The temp folder location is c:/Documents and Settings/&lt;User&gt;/Local Settings/Temp/VWDWebCache/&lt;Server&gt;/_&lt;application name&gt;</span></span>


<span data-ttu-id="5e5b6-188">使用 FTP 選項時，系統會向您選擇位置對話方塊。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-188">When using the FTP option, you will be presented with a Choose Location dialog.</span></span> <span data-ttu-id="5e5b6-189">您輸入所需的 FTP 連線資訊，此對話方塊，如下所示。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-189">You enter the required FTP connection information into this dialog as shown below.</span></span>


![選擇 ftp 位置對話方塊](improvements-in-visual-studio-2005/_static/image2.jpg)

<span data-ttu-id="5e5b6-191">**圖 5**: 選擇 ftp 位置對話方塊</span><span class="sxs-lookup"><span data-stu-id="5e5b6-191">**Figure 5**: The Choose Location Dialog for FTP</span></span>


## <a name="lab-setup-ftp-site-and-create-a-project"></a><span data-ttu-id="5e5b6-192">實驗室： 設定 FTP 站台，並建立專案</span><span class="sxs-lookup"><span data-stu-id="5e5b6-192">Lab: Setup FTP site and create a project</span></span>

<span data-ttu-id="5e5b6-193">下列步驟可用來設定 FTP 站台，讓使用者具有只是它們可以透過 FTP 上傳至的位置。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-193">The following steps configure the FTP site so that a user has a location that only they can upload to via FTP.</span></span>

### <a name="install-the-ftp-service"></a><span data-ttu-id="5e5b6-194">安裝 FTP 服務</span><span class="sxs-lookup"><span data-stu-id="5e5b6-194">Install the FTP Service</span></span>

1. <span data-ttu-id="5e5b6-195">開啟 新增或移除程式 中，選取 新增/移除 Windows 元件</span><span class="sxs-lookup"><span data-stu-id="5e5b6-195">Open Add Remove Programs, select Add/Remove Windows Components</span></span>
2. <span data-ttu-id="5e5b6-196">選取 Internet Information Services （Windows 2003 上的應用程式伺服器），然後按一下**詳細資料**。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-196">Select Internet Information Services (Application Server on Windows 2003) and click **Details**.</span></span>
3. <span data-ttu-id="5e5b6-197">請檢查**檔案傳輸通訊協定 (FTP) 服務**然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-197">Check **File Transfer Protocol (FTP) Service** and click **OK**.</span></span>
4. <span data-ttu-id="5e5b6-198">按一下 [**下一步]** 安裝 FTP 服務。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-198">Click **Next** to install the FTP service.</span></span>

### <a name="create-a-new-folder-for-content"></a><span data-ttu-id="5e5b6-199">建立新的資料夾內容</span><span class="sxs-lookup"><span data-stu-id="5e5b6-199">Create a New Folder for Content</span></span>

1. <span data-ttu-id="5e5b6-200">在 Windows 檔案總管中，會建立新資料夾，稱為**User1**內 c: / inetpub wwwroot。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-200">In Windows Explorer, create a new folder called **User1** inside of c:/inetpub/wwwroot.</span></span>

#### <a name="configure-folders-and-permissions-on-folders"></a><span data-ttu-id="5e5b6-201">在資料夾上設定資料夾和權限。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-201">Configure folders and permissions on folders.</span></span>

1. <span data-ttu-id="5e5b6-202">從 系統管理工具，以開啟 Internet Information Services 嵌入式管理單元。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-202">Open the Internet Information Services snap-in from Administrative Tools.</span></span> <span data-ttu-id="5e5b6-203">您現在會有電腦名稱節點下的 FTP 站台資料夾。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-203">You will now have an FTP Sites folder under the computer name node.</span></span>
2. <span data-ttu-id="5e5b6-204">依序展開**FTP 站台**。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-204">Expand **FTP Sites**.</span></span>
3. <span data-ttu-id="5e5b6-205">以滑鼠右鍵按一下**預設的 FTP 站台**，選取**新增**，然後**虛擬目錄**，然後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-205">Right-click the **Default FTP Site**, select **New**, then **Virtual Directory**, then click **Next**.</span></span>
4. <span data-ttu-id="5e5b6-206">請輸入**User1**的虛擬目錄名稱，然後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-206">Enter **User1** for the virtual directory name and click **Next**.</span></span>
5. <span data-ttu-id="5e5b6-207">請輸入**c: / inetpub/wwwroot/User1**路徑，然後按一下 [**下一步]**。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-207">Enter **c:/inetpub/wwwroot/User1** for the path and click **Next**.</span></span>
6. <span data-ttu-id="5e5b6-208">按一下 **下一步**，然後**完成**以完成精靈。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-208">Click **Next** and then **Finish** to complete the wizard.</span></span>
7. <span data-ttu-id="5e5b6-209">以滑鼠右鍵按一下**User1**在預設的 FTP 站台，然後選取的虛擬目錄**屬性**。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-209">Right-click the **User1** virtual directory under Default FTP Site and select **Properties**.</span></span>
8. <span data-ttu-id="5e5b6-210">檢查**撰寫**核取方塊，按一下 **確定**以關閉對話方塊。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-210">Check the **Write** checkbox and click **OK** to close the dialog.</span></span>
9. <span data-ttu-id="5e5b6-211">以滑鼠右鍵按一下**預設的 FTP 站台**，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-211">Right-click **Default FTP Site** and select **Properties**.</span></span>
10. <span data-ttu-id="5e5b6-212">在 **安全性帳戶**索引標籤上，取消核取**允許匿名連線**。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-212">On the **Security Accounts** tab, uncheck **Allow Anonymous Connections**.</span></span>
11. <span data-ttu-id="5e5b6-213">按一下 **是**詢問您是否要繼續  對話方塊中。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-213">Click **Yes** in the dialog asking if you want to continue.</span></span>
12. <span data-ttu-id="5e5b6-214">按一下 **確定**以關閉對話方塊。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-214">Click **OK** to close the dialog.</span></span>
13. <span data-ttu-id="5e5b6-215">依序展開**Default Web Site**下方**網站**節點。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-215">Expand the **Default Web Site** under the **Web Sites** node.</span></span>
14. <span data-ttu-id="5e5b6-216">以滑鼠右鍵按一下**User1**目錄，然後選取**屬性**</span><span class="sxs-lookup"><span data-stu-id="5e5b6-216">Right-click the **User1** directory and select **Properties**</span></span>
15. <span data-ttu-id="5e5b6-217">在 **應用程式設定**區段中，按一下**建立**將標示為應用程式的資料夾。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-217">In the **Application Settings** section, click **Create** to mark the folder as an application.</span></span>
16. <span data-ttu-id="5e5b6-218">按一下 **確定**以關閉對話方塊。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-218">Click **OK** to close the dialog.</span></span>
17. <span data-ttu-id="5e5b6-219">關閉 Internet Information Services 嵌入式管理單元。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-219">Close the Internet Information Services snap-in.</span></span>

### <a name="create-web-project"></a><span data-ttu-id="5e5b6-220">建立 web 專案</span><span class="sxs-lookup"><span data-stu-id="5e5b6-220">Create web project</span></span>

1. <span data-ttu-id="5e5b6-221">開啟 Visual Studio 2005。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-221">Open Visual Studio 2005.</span></span>
2. <span data-ttu-id="5e5b6-222">從**檔案**功能表上，選取**新的網站**。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-222">From the **File** menu, select **New Web Site**.</span></span>
3. <span data-ttu-id="5e5b6-223">在 **位置**下拉式清單中，選取**FTP**。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-223">In the **Location** dropdown, select **FTP**.</span></span>
4. <span data-ttu-id="5e5b6-224">按一下 [瀏覽] 。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-224">Click **Browse**.</span></span>
5. <span data-ttu-id="5e5b6-225">請輸入**localhost**中**Server**文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-225">Enter **localhost** in the **Server** textbox.</span></span>
6. <span data-ttu-id="5e5b6-226">請輸入**User1** [目錄] 文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-226">Enter **User1** in the Directory textbox.</span></span>
7. <span data-ttu-id="5e5b6-227">按一下 **開啟**。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-227">Click **Open**.</span></span> <span data-ttu-id="5e5b6-228">FTP 位置將會進入新網站 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-228">The FTP location will be entered into the New Web Site dialog.</span></span>
8. <span data-ttu-id="5e5b6-229">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-229">Click **OK**.</span></span>
9. <span data-ttu-id="5e5b6-230">取消核取**匿名登入**在 [FTP 登入] 對話方塊中，輸入您的認證，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-230">Uncheck **Anonymous log on** in the FTP Log On dialog, enter your credentials, and click **OK**.</span></span>
10. <span data-ttu-id="5e5b6-231">什麼是專案的 URL？</span><span class="sxs-lookup"><span data-stu-id="5e5b6-231">What is the URL for the project?</span></span> <span data-ttu-id="5e5b6-232">（專案的 URL 會顯示在 [方案總管]）。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-232">(The URL for the project will be displayed in Solution Explorer.)</span></span>
11. <span data-ttu-id="5e5b6-233">從**建置**功能表上，選取**建置網站**或是**建置方案**。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-233">From the **Build** menu, select **Build Web Site** or **Build Solution**.</span></span>
12. <span data-ttu-id="5e5b6-234">以滑鼠右鍵按一下方案總管 中的 Default.aspx，然後選取**瀏覽器中的檢視**。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-234">Right-click on Default.aspx in Solution Explorer and select **View in Browser**.</span></span>
13. <span data-ttu-id="5e5b6-235">在必須有網站 URL] 對話方塊中，輸入`http://localhost/user1`URL，然後按一下 [ **[確定]**。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-235">In the Web Site URL Required dialog, enter `http://localhost/user1` for the URL and click **OK**.</span></span>

> [!NOTE]
> <span data-ttu-id="5e5b6-236">如果您收到錯誤，指出無法載入類型 /_Default，請確定您的網站並不是較早的版本上執行 ASP.NET 2.0。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-236">If you get a error indicating an inability to load the type /_Default, make sure that you are running ASP.NET 2.0 on your Web site and not an earlier version.</span></span> <span data-ttu-id="5e5b6-237">您可以從 Internet Information Services 中的 [ASP.NET] 索引標籤來這麼做。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-237">You can do that from the ASP.NET tab in Internet Information Services.</span></span>


## <a name="opening-web-projects"></a><span data-ttu-id="5e5b6-238">開啟 Web 專案</span><span class="sxs-lookup"><span data-stu-id="5e5b6-238">Opening Web Projects</span></span>

<span data-ttu-id="5e5b6-239">開啟 Web 專案也是類似於建立專案。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-239">Opening Web projects is similar to creating projects.</span></span> <span data-ttu-id="5e5b6-240">下列各節說明留意出針對在 IDE 中工作時的區域。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-240">The following sections call out areas to keep an eye out for while working within the IDE.</span></span> <span data-ttu-id="5e5b6-241">它也涵蓋了有關使用 HTTP 和 FTP Web 專案。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-241">It also covers working with Web projects using HTTP and FTP.</span></span>

<span data-ttu-id="5e5b6-242">若要開啟 Web 專案，請從 [檔案] 功能表中選取開啟網站。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-242">To open a Web project, select Open Web Site from the File menu.</span></span> <span data-ttu-id="5e5b6-243">先前討論的相同選擇位置對話方塊會提示您，並且您有相同的四個選項可供您： 檔案系統、 本機 IIS、 FTP 和遠端站台。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-243">You will be prompted with the same Choose Location dialog covered previously and you have the same four options available to you: File System, Local IIS, FTP, and Remote Site.</span></span>

<a id="_Toc116100245"></a>

## <a name="file-system"></a><span data-ttu-id="5e5b6-244">檔案系統</span><span class="sxs-lookup"><span data-stu-id="5e5b6-244">File System</span></span>

<span data-ttu-id="5e5b6-245">如同先前在此模組，Visual Studio 不會再使用專案檔。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-245">As indicated previously in this module, Visual Studio no longer uses a project file.</span></span> <span data-ttu-id="5e5b6-246">因此，如果您選擇從檔案系統開啟網站，您實際上需要選擇想要的話，任何資料夾，即使您選擇的資料夾不是作為一開始在 Visual Studio 中的 Web 專案。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-246">Therefore, if you choose to open a Web site from the file system, you actually have the option of choosing any folder that you wish, even if the folder you choose was not created as a Web project initially in Visual Studio.</span></span> <span data-ttu-id="5e5b6-247">比方說，您可以選擇開啟為網站的 [我的文件] 資料夾，Visual Studio 會很高興地操作加以開啟並顯示您的檔案，如下所示。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-247">For example, you can choose to open the My Documents folder as a Web site and Visual Studio will happily open it and display your files as shown below.</span></span>


![我的網站以開啟的文件](improvements-in-visual-studio-2005/_static/image3.jpg)

<span data-ttu-id="5e5b6-249">**圖 6**:*我的文件*網站形式開啟</span><span class="sxs-lookup"><span data-stu-id="5e5b6-249">**Figure 6**: *My Documents* Opened As a Web Site</span></span>


<span data-ttu-id="5e5b6-250">因為 Visual Studio 只會建立其他檔案和資料夾在必要時，沒有其他檔案或資料夾會新增至您所開啟的位置中。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-250">Because Visual Studio only creates additional files and folders when necessary, no additional files or folders are added to the location you open.</span></span> <span data-ttu-id="5e5b6-251">此架構的副作用是，它會防止您在檔案系統上的巢狀的網站。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-251">A side-effect of this architecture is that it prevents you from nesting Web sites on the file system.</span></span> <span data-ttu-id="5e5b6-252">例如，請考慮下列目錄結構。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-252">For example, consider the following directory structure.</span></span>

<span data-ttu-id="5e5b6-253">在 c: / MyWebSite web 專案</span><span class="sxs-lookup"><span data-stu-id="5e5b6-253">Web project at C:/MyWebSite</span></span>

<span data-ttu-id="5e5b6-254">在 c: / MyWebSite/巢狀的另一個 web 專案</span><span class="sxs-lookup"><span data-stu-id="5e5b6-254">Another web project at C:/MyWebSite/Nested</span></span>

<span data-ttu-id="5e5b6-255">當您開啟的網站，在 c: / MyWebSite 時，巢狀資料夾會顯示為該應用程式的子資料夾中。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-255">When you open the Web site at c:/MyWebSite, the Nested folder will appear as a sub-folder of that application.</span></span>

<a id="_Toc116100246"></a>

## <a name="http"></a><span data-ttu-id="5e5b6-256">HTTP</span><span class="sxs-lookup"><span data-stu-id="5e5b6-256">HTTP</span></span>

<span data-ttu-id="5e5b6-257">開啟時透過 HTTP 的網站，從 IIS metabase (IIS)，或使用 FrontPage Server Extensions （遠端站台。） 讀取設定如果有巢狀的 web 應用程式，這些會以其識別為應用程式的圖示也顯示。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-257">When opening Web sites via HTTP, settings are read either from the IIS metabase (Local IIS) or using FrontPage Server Extensions (Remote Site.) If there are nested web applications, these are displayed as well with an icon identifying them as an application.</span></span> <span data-ttu-id="5e5b6-258">如果您已熟悉使用 FrontPage 中的 web 應用程式，Visual Studio 2005 中的行為會類似。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-258">If you are familiar with working with web applications in FrontPage, the behavior in Visual Studio 2005 is similar.</span></span>

<span data-ttu-id="5e5b6-259">即使 Visual Studio 會顯示在 IDE 中目前開啟的應用程式底下巢狀的應用程式的圖示，它將不允許您展開以查看其內容。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-259">Even though Visual Studio will display an icon for applications that are nested beneath the application that is currently opened within the IDE, it will not allow you to expand them to see their content.</span></span> <span data-ttu-id="5e5b6-260">您可以不過，按兩下加以開啟。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-260">You can, however, double-click on them to open them.</span></span> <span data-ttu-id="5e5b6-261">當您這樣做時，您會看到對話方塊，提示您開啟的 web 應用程式 （並取代目前開啟的方案），或加入至目前方案的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-261">When you do, you will be presented with a dialog prompting you to either open the web application (and replace the currently open solution) or add the Web application to your current solution.</span></span>


![按兩下巢狀的應用程式圖示會出現此對話方塊](improvements-in-visual-studio-2005/_static/image4.jpg)

<span data-ttu-id="5e5b6-263">**圖 7**： 按兩下巢狀的應用程式圖示會出現此對話方塊</span><span class="sxs-lookup"><span data-stu-id="5e5b6-263">**Figure 7**: Double-clicking a nested application icon presents you with this dialog</span></span>


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a><span data-ttu-id="5e5b6-264">FTP 站台</span><span class="sxs-lookup"><span data-stu-id="5e5b6-264">FTP Site</span></span>

<span data-ttu-id="5e5b6-265">當您開啟透過 FTP 站台時，檔案會全部在本機複製到您的暫存資料夾。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-265">When you open a site via FTP, the files are all copied locally to your temp folder.</span></span> <span data-ttu-id="5e5b6-266">本機儲存體位置的完整路徑會顯示在專案 [屬性] 窗格，並使用下列格式建立。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-266">The full path for the local storage location is displayed in the Properties pane for the project and is created using the following format.</span></span>

<span data-ttu-id="5e5b6-267">C: / Documents and Settings /&lt;使用者&gt;本機/設定/Temp/VWDWebCache/&lt;伺服器&gt;/_&lt;應用程式名稱&gt;</span><span class="sxs-lookup"><span data-stu-id="5e5b6-267">C:/Documents and Settings/&lt;User&gt;/Local Settings/Temp/VWDWebCache/&lt;Server&gt;/_&lt;application name&gt;</span></span>

<span data-ttu-id="5e5b6-268">在使用 FTP 時，Visual Studio 必須指定專案的基底 URL，以便您可以瀏覽它，如下所示。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-268">When using FTP, Visual Studio will need to specify the base URL for your project so that you can browse it as shown below.</span></span> <span data-ttu-id="5e5b6-269">如果您未指定基底 URL，Visual Studio 會要求您提供此第一次您嘗試瀏覽的網站中的頁面。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-269">If you do not specify a base URL, Visual Studio will ask you for it the first time you attempt to browse a page in the Web site.</span></span>


![指定 FTP 站台的基底 URL](improvements-in-visual-studio-2005/_static/image5.jpg)

<span data-ttu-id="5e5b6-271">**圖 8**: FTP 站台的指定基底 URL</span><span class="sxs-lookup"><span data-stu-id="5e5b6-271">**Figure 8**: Specifying a Base URL for FTP Sites</span></span>


## <a name="improvements-in-compilation"></a><span data-ttu-id="5e5b6-272">在編譯中的增強功能</span><span class="sxs-lookup"><span data-stu-id="5e5b6-272">Improvements in Compilation</span></span>

<span data-ttu-id="5e5b6-273">使用 Visual Studio 2005 中的 Web 應用程式是比舊版顯然速度更快。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-273">Working with Web applications in Visual Studio 2005 is noticeably faster than previous versions.</span></span> <span data-ttu-id="5e5b6-274">這是因為任何小型的組件中編譯架構中的變更。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-274">This is due in no small part to the changes in compilation architecture.</span></span>

<span data-ttu-id="5e5b6-275">在 Visual Studio 2002 和 2003年，Web 應用程式已編譯成一個 /bin 資料夾中的主要組件。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-275">In Visual Studio 2002 and 2003, Web applications were compiled into one primary assembly residing in the /bin folder.</span></span> <span data-ttu-id="5e5b6-276">在 Visual Studio 2005 中，已新增的應用程式/（_c） 資料夾。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-276">In Visual Studio 2005, an App/_Code folder was added.</span></span> <span data-ttu-id="5e5b6-277">類別和其他非 UI 程式碼會新增至應用程式/（_c） 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-277">Classes and other non-UI code are added to the App/_Code folder.</span></span> <span data-ttu-id="5e5b6-278">當 Visual Studio 建置專案時，應用程式/（_c） 資料夾中的所有檔案會都編譯成單一的 App/_Code.dll 檔案。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-278">When Visual Studio builds the project, all files in the App/_Code folder are compiled into a single App/_Code.dll file.</span></span> <span data-ttu-id="5e5b6-279">這項變更的結果是速度也比在舊版中後續的組建。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-279">The result of this change is that subsequent builds are much faster than in previous versions.</span></span>

> [!NOTE]
> <span data-ttu-id="5e5b6-280">MSBuild 命令列公用程式也可用來建置 ASP.NET Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-280">The MSBuild command line utility can also be used to build ASP.NET Web applications.</span></span> <span data-ttu-id="5e5b6-281">模組 9 中，將會說明該工具。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-281">That tool will be covered in module 9.</span></span>


<span data-ttu-id="5e5b6-282">另一個編譯增強功能是新的組建 頁面選項，在 建置 功能表上。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-282">Another compilation enhancement is the new Build Page option on the Build menu.</span></span> <span data-ttu-id="5e5b6-283">這項功能可讓開發人員重建只有目前頁面 （連同，課程中，和相依性），因此可以更快速地編譯變更。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-283">This feature allows a developer to rebuild only the current page (along with, of course, and dependencies) so that changes can be compiled more quickly.</span></span> <span data-ttu-id="5e5b6-284">C# 未提供的更新 IntelliSense 等進行背景編譯，因為它們將受益於非常這項功能因為這樣可讓 intellisense 快速更新的只重建單一頁面。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-284">Because C# does not offer background compilation for purposes of updating IntelliSense, etc., they will benefit immensely from this feature because it will allow for IntelliSense to be updated quickly by simply rebuilding a single page.</span></span>

<span data-ttu-id="5e5b6-285">專案組建屬性可讓您設定組建的執行啟動頁面之前，就會發生型別。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-285">The Build properties for a project allow you to configure the type of build that occurs before the startup page is executed.</span></span> <span data-ttu-id="5e5b6-286">開發人員可以選擇只建立目前的頁面，讓 Visual Studio 可以更快速地偵錯應用程式，程式碼變更後啟動。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-286">Developers can choose to only build the current page so that Visual Studio can start debugging applications more quickly after code changes.</span></span>


![組建頁面啟動動作](improvements-in-visual-studio-2005/_static/image6.jpg)

<span data-ttu-id="5e5b6-288">**圖 9**： 組建頁面啟動動作</span><span class="sxs-lookup"><span data-stu-id="5e5b6-288">**Figure 9**: The Build Page Start Action</span></span>


<span data-ttu-id="5e5b6-289">Visual Studio 和 ASP.NET 架構另一個絕佳的增強功能是編輯的區域中，並繼續。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-289">Another great enhancement to Visual Studio and the ASP.NET architecture is in the area of edit and continue.</span></span> <span data-ttu-id="5e5b6-290">在 Visual Studio 2005 中，開發人員可以開始偵錯的專案，並在專案上進行程式碼變更，而不中斷連結偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-290">In Visual Studio 2005, developers can start debugging a project and make code changes on the project without detaching the debugger.</span></span> <span data-ttu-id="5e5b6-291">事實上，您實際上可以啟動偵錯專案時，加入新的類別、 將程式碼新增至該類別、 將程式碼新增至您建立該類別的新執行個體的頁面和執行的類別，而不中斷連結偵錯工具的所有方法。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-291">In fact, you can literally start debugging a project, add a new class, add code to that class, add code to your page that creates a new instance of that class and execute a method of the class, all without detaching the debugger.</span></span> <span data-ttu-id="5e5b6-292">執行新的程式碼會解譯為常值，只要重新整理瀏覽器就行了 ！</span><span class="sxs-lookup"><span data-stu-id="5e5b6-292">Executing the new code is literally as easy as refreshing the browser!</span></span>

<span data-ttu-id="5e5b6-293">按一下這裡以查看編輯的影片逐步解說，並繼續在 Visual Studio 2005 的功能。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-293">Click here to see a video walkthrough of the edit and continue feature in Visual Studio 2005.</span></span>


![](improvements-in-visual-studio-2005/_static/image2.png)


[<span data-ttu-id="5e5b6-294">開啟全螢幕影片</span><span class="sxs-lookup"><span data-stu-id="5e5b6-294">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


<span data-ttu-id="5e5b6-295">強大的編輯後繼續 [功能]，請在 ASP.NET 2.0 和 Visual Studio 2005 是因為 ASP.NET 應用程式的架構變更。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-295">The robust edit and continue functionality in ASP.NET 2.0 and Visual Studio 2005 is due to an architectural change for ASP.NET applications.</span></span> <span data-ttu-id="5e5b6-296">在 ASP.NET 1.x 中，建立在 Visual Studio 2002/2003 中的應用程式編譯成主要組件儲存在 /bin 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-296">In ASP.NET 1.x, applications created in Visual Studio 2002/2003 were compiled into a primary assembly that was stored in the /bin folder.</span></span> <span data-ttu-id="5e5b6-297">所有類別，頁面等的應用程式編譯成該 DLL。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-297">All classes, pages, etc. for the application were compiled into that one DLL.</span></span> <span data-ttu-id="5e5b6-298">然後在執行階段，ASP.NET 會編譯所有的控制項、 標記和頁面內的 ASP.NET 程式碼，並將這些 Dll 複製到 ASP.NET 暫存資料夾。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-298">Then at runtime, ASP.NET would compile all of the controls, markup, and ASP.NET code within pages and copy those DLLs into the ASP.NET temporary folder.</span></span>

<span data-ttu-id="5e5b6-299">使用 ASP.NET 2.0 中，在執行階段的兩個編譯模型說明上方 （適用於 Visual Studio 的其中一個），另一個用於 ASP.NET 的 Visual Studio 2005 中已合併至另一個常見的編譯模型。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-299">In Visual Studio 2005 using ASP.NET 2.0, the two compilation models outline above (one for Visual Studio and one for ASP.NET at runtime) have been merged into one common compilation model.</span></span> <span data-ttu-id="5e5b6-300">這表示在執行階段現在在開發階段，而不是攔截所有編譯問題。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-300">That means that all compilation issues are now caught during the development stage instead of at runtime.</span></span> <span data-ttu-id="5e5b6-301">它也可讓設計工具和功能，例如使用者控制項和主版頁面的 IntelliSense 支援。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-301">It also allows for designer and IntelliSense support for features such as user controls and master pages.</span></span>

<span data-ttu-id="5e5b6-302">若要查看使用者控制項的設計工具支援的影片逐步解說，請按一下這裡。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-302">Click here to see a video walkthrough of designer support for user controls.</span></span>


![](improvements-in-visual-studio-2005/_static/image3.png)


[<span data-ttu-id="5e5b6-303">開啟全螢幕影片</span><span class="sxs-lookup"><span data-stu-id="5e5b6-303">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> <span data-ttu-id="5e5b6-304">從頁面上，移除使用者控制項時@Register指示詞會保留在標記，而且應該手動移除，以避免發生剖析器錯誤，如果從網站中刪除使用者控制項。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-304">When a user control is removed from a page, the @Register directive remains in the markup and should be removed manually in order to avoid parser errors if the user control is deleted from the Web site.</span></span>


<span data-ttu-id="5e5b6-305">在 Visual Studio 編譯模型中的另一項改進是發行網站的功能。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-305">Another improvement in the Visual Studio compilation model is the Publish Web Site feature.</span></span> <span data-ttu-id="5e5b6-306">[發佈] 功能會先行編譯網站，因為開發人員可以享有不必編譯任何項目依需求新增的效能。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-306">Because the Publish feature precompiles the Web site, developers can enjoy the added performance of not having to compile anything on demand.</span></span> <span data-ttu-id="5e5b6-307">它也會預先編譯應用程式/（_c） 資料夾中的所有原始程式碼的 dll，讓沒有原始程式碼，就必須部署。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-307">It also precompiles all source code in the App/_Code folder into a DLL so that no source code has to be deployed.</span></span>


![[發行網站] 對話方塊](improvements-in-visual-studio-2005/_static/image7.jpg)

<span data-ttu-id="5e5b6-309">**[圖 10**: 發行網站] 對話方塊</span><span class="sxs-lookup"><span data-stu-id="5e5b6-309">**Figure 10**: The Publish Web Site Dialog</span></span>


> [!NOTE]
> <span data-ttu-id="5e5b6-310">Aspnet/_compile.exe 公用程式也可用來預先編譯的 ASP.NET Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-310">The aspnet/_compile.exe utility can also be used to pre-compile an ASP.NET Web application.</span></span> <span data-ttu-id="5e5b6-311">模組 9 中，將會說明該工具。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-311">That tool will be covered in module 9.</span></span>


<span data-ttu-id="5e5b6-312">當您發佈網站時，先行編譯的檔案會儲存在 Temporary ASP.NET Files 資料夾，如下所示。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-312">When you Publish a Web site, the precompiled files are stored in the Temporary ASP.NET Files folder as shown below.</span></span> <span data-ttu-id="5e5b6-313">檔案與 *.compiled*副檔名是定義特定 dll 的相依性的 XML 檔案。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-313">Files with a *.compiled* file extension are XML files that define dependencies for particular DLLs.</span></span> <span data-ttu-id="5e5b6-314">編譯任何 Webform 或使用者控制項為開頭的隨機 Dll*應用程式 /_Web /_*。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-314">Any Webform or user controls are compiled into random DLLs that begin with *App/_Web/_*.</span></span>

<span data-ttu-id="5e5b6-315">如果您離開*讓這個先行編譯的站台成為可更新*選取核取方塊，Webforms 和使用者控制項內的標記將不會預先編譯成 DLL，讓您可以在部署之後進行變更。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-315">If you leave the *Allow this precompiled site to be updatable* checkbox checked, markup inside of your Webforms and user controls will not be pre-compiled into a DLL allowing you to make changes after deployment.</span></span> <span data-ttu-id="5e5b6-316">如果您不希望鎖定的標記，以便不允許變更已部署的內容，請取消核取此方塊。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-316">If you would prefer to lock down the markup so that changes to the deployed content are not allowed, uncheck this box.</span></span>

<span data-ttu-id="5e5b6-317">*使用固定命名和單一頁面的組件*核取方塊可讓您停用批次編譯，讓每個頁面會編譯成固定式名稱的組件。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-317">The *Use fixed naming and single page assemblies* checkbox allows you to disable batch compilation so that each page is compiled into a fixed-named assembly.</span></span> <span data-ttu-id="5e5b6-318">保留未核取此方塊，可讓您利用批次編譯。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-318">Leaving this box unchecked allows you to take advantage of batch compilation.</span></span>

<span data-ttu-id="5e5b6-319">*啟用強式命名上先行編譯的組件*核取方塊可讓您將強式名稱您先行編譯的組件。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-319">The *Enable strong naming on precompiled assemblies* checkbox allows you to strong-name your precompiled assemblies.</span></span>

> [!NOTE]
> <span data-ttu-id="5e5b6-320">在 ASP.NET 1.x 中，強式名稱組件必須安裝到全域組件快取 (GAC) 中。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-320">In ASP.NET 1.x, strong-named assemblies had to be installed into the Global Assembly Cache (GAC).</span></span> <span data-ttu-id="5e5b6-321">在 ASP.NET 2.0 中，您不需要強式名稱組件安裝到 GAC。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-321">In ASP.NET 2.0, you are not required to install strong-named assemblies into the GAC.</span></span>


![ASP.NET 應用程式預先編譯的檔案](improvements-in-visual-studio-2005/_static/image8.jpg)

<span data-ttu-id="5e5b6-323">**圖 11**: ASP.NET 應用程式預先編譯的檔案</span><span class="sxs-lookup"><span data-stu-id="5e5b6-323">**Figure 11**: An ASP.NET Applications Pre-Compiled Files</span></span>


> [!NOTE]
> <span data-ttu-id="5e5b6-324">在上述的應用程式，發生任何 web.config 檔。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-324">In the application above, there was no web.config file.</span></span> <span data-ttu-id="5e5b6-325">如果有，它會呼叫*PrecompiledApp.config*之後發行的 Web 網站處理程序。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-325">If there had been, it would have been called *PrecompiledApp.config* after the Publish Web site process.</span></span>


## <a name="improvements-in-deployment"></a><span data-ttu-id="5e5b6-326">部署的改進功能</span><span class="sxs-lookup"><span data-stu-id="5e5b6-326">Improvements in Deployment</span></span>

<span data-ttu-id="5e5b6-327">為 Visual Studio 2002 與 2003 年 Visual Studio 2005 提供複製專案功能。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-327">As with Visual Studio 2002 and 2003, Visual Studio 2005 offers a Copy Project feature.</span></span> <span data-ttu-id="5e5b6-328">不過，此功能已經過增強 Visual Studio 2005 中，且現在稱為複製網站。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-328">However, the feature has been beefed up in Visual Studio 2005 and is now called Copy Web Site.</span></span>

<span data-ttu-id="5e5b6-329">複製網站 對話方塊會分割成左框架內，右框架。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-329">The Copy Web Site dialog is split into a left frame and a right frame.</span></span> <span data-ttu-id="5e5b6-330">左框架內稱為來源網站和右邊框架會呼叫遠端網站。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-330">The left frame is called the Source Web Site and the right frame is called the Remote Web Site.</span></span> <span data-ttu-id="5e5b6-331">有些開發人員可能會混淆的一點是，在右側框架中顯示的網站不一定是遠端站台。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-331">One thing that may confuse some developers is that the site displayed in the right frame is not necessarily a remote site.</span></span> <span data-ttu-id="5e5b6-332">這可能是本機檔案系統或 IIS 的本機執行個體上的網站。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-332">It could be a site on the local file system or on the local instance of IIS.</span></span> <span data-ttu-id="5e5b6-333">此外，在左框架所顯示的網站也不一定與來源網站 對話方塊可讓您從遠端網站上發行*至*來源網站。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-333">Additionally, the site displayed in the left frame is not necessarily the source Web site because the dialog allows you to publish from the remote Web site *to* the source Web site.</span></span>

<span data-ttu-id="5e5b6-334">如果您將專案複製到遠端網站，該網站必須在其上安裝的 FrontPage Server Extensions。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-334">If you are copying a project to a remote Web site, that site must have the FrontPage Server Extensions installed on it.</span></span> <span data-ttu-id="5e5b6-335">如果沒有，您必須使用 FTP 連接。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-335">If it does not, you will need to connect using FTP.</span></span> <span data-ttu-id="5e5b6-336">相反地，如果您將專案複製到本機 IIS 執行個體，FrontPage Server Extensions 並不需要。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-336">On the other hand, if you are copying a project to the local IIS instance, FrontPage Server Extensions are not required.</span></span>

> [!NOTE]
> <span data-ttu-id="5e5b6-337">如果您嘗試在本機 IIS 執行個體上建立新的網站和 FrontPage 2002 Server Extensions 安裝，您會取得錯誤訊息，告知您的網站不支援建立 SharePoint 伺服器上。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-337">If you try to create a new Web site on the local IIS instance and the FrontPage 2002 Server Extensions are installed, you will get an error message telling you that creating Web sites is not supported on a SharePoint server.</span></span> <span data-ttu-id="5e5b6-338">在此情況下，您可以選擇 FrontPage 2000 Server Extensions 安裝或移除 FrontPage Server Extensions。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-338">In that case, you have the option of installing the FrontPage 2000 Server Extensions or removing the FrontPage Server Extensions.</span></span>


<span data-ttu-id="5e5b6-339">如 「 複製網站 」 功能的影片逐步解說，請按一下這裡。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-339">Click here for a video walkthrough of the Copy Web Site feature.</span></span>


![](improvements-in-visual-studio-2005/_static/image4.png)


[<span data-ttu-id="5e5b6-340">開啟全螢幕影片</span><span class="sxs-lookup"><span data-stu-id="5e5b6-340">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a><span data-ttu-id="5e5b6-341">在偵錯改善</span><span class="sxs-lookup"><span data-stu-id="5e5b6-341">Improvements in Debugging</span></span>

<span data-ttu-id="5e5b6-342">有四個主要的改善項目在 Visual Studio 2005 中偵錯。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-342">There are four key improvements in debugging in Visual Studio 2005.</span></span>

- <span data-ttu-id="5e5b6-343">根據預設，可能會以非系統管理員在本機偵錯。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-343">Debugging locally as a non-administrator is possible out of the box.</span></span>
- <span data-ttu-id="5e5b6-344">Compilation 項目的偵錯屬性現在預設為 false。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-344">The Debug attribute for the Compilation element is now false by default.</span></span>
- <span data-ttu-id="5e5b6-345">遠端偵錯設定和組態是比以前更容易。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-345">Remote debugging setup and configuration is easier than before.</span></span>
- <span data-ttu-id="5e5b6-346">您現在可以偵錯透過 FTP 位置開啟網站。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-346">You can now debug a Web site opened via an FTP location.</span></span>

## <a name="debugging-as-a-non-administrator"></a><span data-ttu-id="5e5b6-347">非系統管理員身分偵錯</span><span class="sxs-lookup"><span data-stu-id="5e5b6-347">Debugging as a Non-Administrator</span></span>

<span data-ttu-id="5e5b6-348">ASP.NET Development Server 新增允許非系統管理員輕鬆地偵錯豎立劃時代的 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-348">The addition of the ASP.NET Development Server allows non-administrators to easily debug ASP.NET applications right out of the box.</span></span> <span data-ttu-id="5e5b6-349">當本機檔案系統上執行的 ASP.NET 應用程式進行偵錯時，Visual Studio 會啟動 ASP.NET 程式開發伺服器，登入的使用者內容下。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-349">When an ASP.NET application running on the local file system is debugged, Visual Studio launches the ASP.NET Development Server under the context of the logged-on user.</span></span> <span data-ttu-id="5e5b6-350">該使用者接著可以偵錯該應用程式，而不需要任何額外的設定。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-350">That user can then debug that application without any additional configuration.</span></span>

## <a name="debug-is-false-by-default"></a><span data-ttu-id="5e5b6-351">偵錯的 False 預設</span><span class="sxs-lookup"><span data-stu-id="5e5b6-351">Debug is False by Default</span></span>

<span data-ttu-id="5e5b6-352">在 ASP.NET 1.x*偵錯*屬性中*編譯*web.config 檔案的項目已設定為 *，則為 true*預設。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-352">In ASP.NET 1.x, the *debug* attribute in the *compilation* element of the web.config file was set to *true* by default.</span></span> <span data-ttu-id="5e5b6-353">已建議一律，開發人員會將此屬性設定為*false*之前應用程式部署到生產環境中，但是因為大多數的開發人員不完全瞭解的離開偵錯屬性設定為結果為 true，就只是保持為-是。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-353">It has always been recommended that developers set this attribute to *false* before deploying an application to production, but because most developers don't fully understand the consequences of leaving the debug attribute set to true, they simply left it as-is.</span></span>

<span data-ttu-id="5e5b6-354">最嚴重的問題有偵錯屬性設為 true 時，它會停用 ASP.NETs 批次編譯模型。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-354">The most severe problem with having the debug attribute set to true is that it disables ASP.NETs batch compilation model.</span></span> <span data-ttu-id="5e5b6-355">因此，每個頁面會編譯成不同的 DLL。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-355">Therefore, each page is compiled into a separate DLL.</span></span> <span data-ttu-id="5e5b6-356">如果 Web 應用程式包含數千個分頁 （不前所未聞的任何方法），這表示該應用程式會建立數個數千個的小型 Dll。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-356">If a Web application consists of thousands of pages (not unheard of by any means), that means several thousand small DLLs will be created by that application.</span></span> <span data-ttu-id="5e5b6-357">這些 Dll 大小較小的它們並不是載入記憶體中的任何特定位置。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-357">While these DLLs are small in size, they are not loaded into any particular location in memory.</span></span> <span data-ttu-id="5e5b6-358">因此，它們會造成系統記憶體的分散程度，而且 OutOfMemoryException 相符項目可以參與。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-358">Therefore, they cause fragmentation in system memory and can contribute to OutOfMemoryException occurrences.</span></span>

<span data-ttu-id="5e5b6-359">在 ASP.NET 2.0 中，偵錯屬性依預設將設定為 false。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-359">In ASP.NET 2.0, the debug attribute is set to false by default.</span></span> <span data-ttu-id="5e5b6-360">因為您已經看過，當開發人員偵錯 ASP.NET 應用程式在 Visual Studio 2005 中，系統會提示他們啟用偵錯中加入的 web.config 檔案。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-360">As you have already seen, when a developer debugs an ASP.NET application in Visual Studio 2005, they are prompted to add a web.config file with debugging enabled.</span></span> <span data-ttu-id="5e5b6-361">這樣會產生原本在 ASP.NET 中的相同缺點 1.x，但現在開發人員會清楚地警告屬性應該重設為 false，再移至生產環境應用程式。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-361">Doing so incurs the same drawbacks that were present in ASP.NET 1.x, but now the developer is clearly warned that the attribute should be reset to false before moving the application to production.</span></span>

## <a name="remote-debugging-setup-and-configuration"></a><span data-ttu-id="5e5b6-362">遠端偵錯設定和組態</span><span class="sxs-lookup"><span data-stu-id="5e5b6-362">Remote Debugging Setup and Configuration</span></span>

<span data-ttu-id="5e5b6-363">在 Visual Studio 2002/2003，遠端偵錯依賴機器偵錯管理員 (mdm.exe) 和 vs7jit.exe 程序。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-363">In Visual Studio 2002/2003, remote debugging relied on the Machine Debug Manager (mdm.exe) and the vs7jit.exe process.</span></span> <span data-ttu-id="5e5b6-364">因此，疑難排解遠端偵錯的問題通常是黑色方塊的客戶，和通常不是更美好的 PSS。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-364">Because of that, troubleshooting remote debugging problems was often a black box for customers and it was often not much better for PSS.</span></span>

<span data-ttu-id="5e5b6-365">Visual Studio 2005 中移除的依賴的 mdm.exe 和 vs7jit.exe 程序。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-365">Visual Studio 2005 removes the reliance on the mdm.exe and vs7jit.exe processes.</span></span> <span data-ttu-id="5e5b6-366">相反地，它現在會使用遠端偵錯監視服務 (msvsmon.exe)。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-366">Instead, it now uses the Remote Debug Monitor service (msvsmon.exe.)</span></span>

<span data-ttu-id="5e5b6-367">遠端偵錯 Visual Studio 2005 中的需求都非常簡單。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-367">The requirement for debugging in Visual Studio 2005 remotely is quite simple.</span></span> <span data-ttu-id="5e5b6-368">您需要之前先偵錯在遠端伺服器上執行 msvsmon.exe。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-368">You need to run msvsmon.exe on the remote server prior to debugging.</span></span> <span data-ttu-id="5e5b6-369">您可以從 Visual Studio 光碟片來安裝遠端偵錯監視，或您只要執行 msvsmon.exe 從共用而不需要完全在 Web 伺服器上安裝任何項目。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-369">You can install the Remote Debug Monitor from the Visual Studio CD or you can simply run msvsmon.exe from a share without installing anything at all on the Web server.</span></span>

<span data-ttu-id="5e5b6-370">當您執行 msvsmon.exe 時，很可能會抱怨連接埠遭到封鎖遠端偵錯。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-370">When you run msvsmon.exe, it is likely that it will complain about ports being blocked for remote debugging.</span></span> <span data-ttu-id="5e5b6-371">幸運的是，您可以輕鬆地解除封鎖的連接埠從右側 [警告] 對話方塊中，如下所示。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-371">Fortunately, you can easily unblock the ports from right within the warning dialog as shown below.</span></span>


![Windows 防火牆會封鎖遠端偵錯的通知](improvements-in-visual-studio-2005/_static/image9.jpg)

<span data-ttu-id="5e5b6-373">**圖 12**： 通知 Windows 防火牆會封鎖遠端偵錯</span><span class="sxs-lookup"><span data-stu-id="5e5b6-373">**Figure 12**: Notification that Windows Firewall is Blocking Remote Debugging</span></span>


<span data-ttu-id="5e5b6-374">一旦您已解除封鎖的偵錯所需的連接埠，您會看到 遠端偵錯監視，如下所示。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-374">Once you have unblocked the ports necessary for debugging, you will see the Remote Debugging Monitor as shown below.</span></span> <span data-ttu-id="5e5b6-375">從這個介面中，您可以監視連線，並變更輕鬆地偵錯權限。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-375">From this interface, you can monitor connections and change debugging permissions easily.</span></span>


![遠端偵錯監視](improvements-in-visual-studio-2005/_static/image10.jpg)

<span data-ttu-id="5e5b6-377">**圖 13**： 遠端偵錯監視</span><span class="sxs-lookup"><span data-stu-id="5e5b6-377">**Figure 13**: The Remote Debugging Monitor</span></span>


<span data-ttu-id="5e5b6-378">它也可進行遠端偵錯 Web 應用程式開啟透過 FTP。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-378">It is also possible to remotely debug a Web application opened via FTP.</span></span> <span data-ttu-id="5e5b6-379">步驟都與先前涵蓋相同。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-379">The steps are the same as those previously covered.</span></span> <span data-ttu-id="5e5b6-380">不過，您必須指定基底 URL 來瀏覽 FTP 專案稍早在本單元中所述。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-380">However, you will need to specify a base URL for browsing the FTP project as outlined earlier in this module.</span></span>

## <a name="lab-2"></a><span data-ttu-id="5e5b6-381">實驗室 2</span><span class="sxs-lookup"><span data-stu-id="5e5b6-381">Lab 2</span></span>

## <a name="remote-debugging-with-visual-studio-2005"></a><span data-ttu-id="5e5b6-382">使用 Visual Studio 2005 的遠端偵錯</span><span class="sxs-lookup"><span data-stu-id="5e5b6-382">Remote Debugging with Visual Studio 2005</span></span>

<span data-ttu-id="5e5b6-383">這個實驗室將引導您完成使用 Visual Studio 2005 的遠端偵錯。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-383">This lab will walk you through remote debugging with Visual Studio 2005.</span></span>

<span data-ttu-id="5e5b6-384">如本實驗室中的影片逐步解說，請按一下這裡。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-384">Click here for a video walkthrough of this lab.</span></span>


![](improvements-in-visual-studio-2005/_static/image5.png)


[<span data-ttu-id="5e5b6-385">開啟全螢幕影片</span><span class="sxs-lookup"><span data-stu-id="5e5b6-385">Open Full-Screen Video</span></span>](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


<span data-ttu-id="5e5b6-386">這個實驗室中必須要有一個執行的 Visual Studio 2005 與其他執行 IIS 5 或更高的兩部機器。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-386">This lab requires you to have two machines, one running Visual Studio 2005 and the other running IIS 5 or greater.</span></span>

1. <span data-ttu-id="5e5b6-387">開啟 Visual Studio 2005，並在遠端伺服器上建立新的網站。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-387">Open Visual Studio 2005 and create a new Web site on the remote server.</span></span>

> [!NOTE]
> <span data-ttu-id="5e5b6-388">在遠端的 IIS 執行個體，或透過 FTP，您可以建立網站。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-388">You can create the Web site on a remote IIS instance or via FTP.</span></span>


1. <span data-ttu-id="5e5b6-389">從遠端的 Web 伺服器中，找出 msvsmon.exe 使用 UNC 路徑的開發電腦上並執行它。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-389">From the remote Web server, locate msvsmon.exe on the development machine using a UNC path and execute it.</span></span>  
 <span data-ttu-id="5e5b6-390">Msvsmon.exe 的預設位置是 //server/c$/Program Files/Microsoft Visual Studio 8/Common7/IDE/遠端偵錯工具/x86。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-390">The default location for msvsmon.exe is //server/c$/Program Files/Microsoft Visual Studio 8/Common7/IDE/Remote Debugger/x86.</span></span>
2. <span data-ttu-id="5e5b6-391">如果系統提示您解除封鎖通訊埠進行遠端偵錯時，這麼做。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-391">If prompted to unblock ports for remote debugging, do so.</span></span>
3. <span data-ttu-id="5e5b6-392">從開發電腦中，開啟程式碼後置 Default.aspx，和頁面/（_l） 方法中設定中斷點。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-392">From the development machine, open the code-behind for Default.aspx and set a breakpoint in the Page/_Load method.</span></span>
4. <span data-ttu-id="5e5b6-393">開始偵錯從開發電腦。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-393">Start debugging from the development machine.</span></span>

<span data-ttu-id="5e5b6-394">如預期般運作，您應該叫用中斷點。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-394">You should hit the breakpoint as expected.</span></span>

## <a name="aspnet-development-server"></a><span data-ttu-id="5e5b6-395">ASP.NET 程式開發伺服器</span><span class="sxs-lookup"><span data-stu-id="5e5b6-395">ASP.NET Development Server</span></span>

<span data-ttu-id="5e5b6-396">將已經討論過，為 Visual Studio 2005 隨附稱為 ASP.NET 程式開發伺服器的 Web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-396">As weve already discussed, Visual Studio 2005 ships with a Web server called the ASP.NET Development Server.</span></span> <span data-ttu-id="5e5b6-397">（ASP.NET Development Server 有時稱為 Cassini。）此網頁伺服器是一個方便的方法，來瀏覽和偵錯的檔案系統上執行的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-397">(The ASP.NET Development Server is sometimes referred to as Cassini.) This Web server is a convenient means to browse and debug Web applications running on the file system.</span></span>

<span data-ttu-id="5e5b6-398">ASP.NET 程式開發伺服器是受限制的 Web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-398">The ASP.NET Development Server is a restricted Web server.</span></span> <span data-ttu-id="5e5b6-399">它不會允許遠端連接，它不允許從啟動 Web 伺服器的使用者以外的任何使用者的任何要求。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-399">It does not allow remote connections, it does not allow any requests from any user other than the user who started the Web server.</span></span> <span data-ttu-id="5e5b6-400">它也沒有提供 ASP 頁面的功能。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-400">It also does not have the capability of serving ASP pages.</span></span> <span data-ttu-id="5e5b6-401">只有 ASP.NET 資源和 HTML 資源 （包括影像、 CSS 檔案等） 會提供服務。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-401">Only ASP.NET resources and HTML resources (including images, CSS files, etc.) are served.</span></span>

<span data-ttu-id="5e5b6-402">ASP.NET Development Server 可以透過命令列啟動執行 WebDev.WebServer.exe 檔案位於 c:/Windows/Microsoft.NET/Framework/v2.0./*/* /  */*/\*.</span><span class="sxs-lookup"><span data-stu-id="5e5b6-402">The ASP.NET Development Server can be launched via the command line by running the WebDev.WebServer.exe file located at c:/Windows/Microsoft.NET/Framework/v2.0./*/*/*/*/\*.</span></span> <span data-ttu-id="5e5b6-403">下列對話方塊會顯示可用的參數。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-403">The following dialog displays the parameters that are available.</span></span>


![](improvements-in-visual-studio-2005/_static/image11.jpg)

<span data-ttu-id="5e5b6-404">**圖 14**</span><span class="sxs-lookup"><span data-stu-id="5e5b6-404">**Figure 14**</span></span>


> [!NOTE]
> <span data-ttu-id="5e5b6-405">透過命令列明確啟動時，不支援 ASP.NET 程式開發伺服器。</span><span class="sxs-lookup"><span data-stu-id="5e5b6-405">The ASP.NET Development Server is not supported when launched explicitly via the command line.</span></span>
