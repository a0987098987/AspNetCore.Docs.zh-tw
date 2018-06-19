---
uid: whitepapers/side-by-side-with-10
title: .NET framework 1.0 和 1.1 版的 ASP.NET-並存執行 |Microsoft 文件
author: rick-anderson
description: 本白皮書說明如何在允許的 fram 任一版本上執行的 ASP.NET Web 應用程式的電腦上安裝.NET 1.0 和.NET 1.1...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 939b04d440955b184bf5f4c40a2ef8175641edfa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530177"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a><span data-ttu-id="2bebe-103">.NET Framework 1.0 和 1.1 版的 ASP.NET-並存執行</span><span class="sxs-lookup"><span data-stu-id="2bebe-103">ASP.NET Side-by-Side Execution of .NET Framework 1.0 and 1.1</span></span>
====================
> <span data-ttu-id="2bebe-104">本白皮書說明如何在任一版本的架構上執行的 ASP.NET Web 應用程式可讓您的電腦上安裝.NET 1.0 和.NET 1.1。</span><span class="sxs-lookup"><span data-stu-id="2bebe-104">This whitepaper describes how to install both .NET 1.0 and .NET 1.1 on your machine, allowing an ASP.NET Web application to run on either version of the framework.</span></span>
> 
> <span data-ttu-id="2bebe-105">適用於 ASP.NET 1.0 和 1.1 的 ASP.NET。</span><span class="sxs-lookup"><span data-stu-id="2bebe-105">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="2bebe-106">在 ASP.NET 中，可視為應用程式時安裝在相同電腦上，但使用不同版本的.NET Framework 並存執行。</span><span class="sxs-lookup"><span data-stu-id="2bebe-106">In ASP.NET, applications are said to be running side by side when they are installed on the same computer, but use different versions of the .NET Framework.</span></span> <span data-ttu-id="2bebe-107">下列主題描述如何設定 ASP.NET 應用程式的並存執行，並提供詳細的步驟：</span><span class="sxs-lookup"><span data-stu-id="2bebe-107">The following topic describes how to configure ASP.NET applications for side-by-side execution and provides detailed steps to:</span></span>

- [<span data-ttu-id="2bebe-108">維護期間安裝的 Web 應用程式的對應至.NET Framework 1.0 版</span><span class="sxs-lookup"><span data-stu-id="2bebe-108">Maintain your Web application's mapping to .NET Framework version 1.0 during installation</span></span>](#1)
- [<span data-ttu-id="2bebe-109">對應至特定版本的.NET framework 的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="2bebe-109">Map a Web application to a specific version of the .NET Framework</span></span>](#2)
- [<span data-ttu-id="2bebe-110">尋找網站使用的.NET framework 版本</span><span class="sxs-lookup"><span data-stu-id="2bebe-110">Find the version of the .NET Framework that a Web site is using</span></span>](#3)

<span data-ttu-id="2bebe-111">傳統上，當更新的電腦上的元件或應用程式時，較舊版本移除和取代成較新版本。</span><span class="sxs-lookup"><span data-stu-id="2bebe-111">Traditionally, when a component or application is updated on a computer, the older version is removed and replaced with the newer version.</span></span> <span data-ttu-id="2bebe-112">如果新的版本不相容的舊版本，這通常會中斷其他元件或應用程式所使用的應用程式。</span><span class="sxs-lookup"><span data-stu-id="2bebe-112">If the new version is not compatible with the previous version, this usually breaks other applications that use the component or application.</span></span> <span data-ttu-id="2bebe-113">.NET Framework 的並存執行，可讓多個版本的組件或同時在相同電腦上安裝的應用程式提供支援。</span><span class="sxs-lookup"><span data-stu-id="2bebe-113">The .NET Framework provides support for side-by-side execution, which allows multiple versions of an assembly or application to be installed on the same computer at the same time.</span></span> <span data-ttu-id="2bebe-114">可以同時安裝多個版本，因為 managed 應用程式可以選取要使用而不會影響使用不同版本的應用程式的版本。</span><span class="sxs-lookup"><span data-stu-id="2bebe-114">Because multiple versions can be installed simultaneously, managed applications can select which version to use without affecting applications that use a different version.</span></span>

<span data-ttu-id="2bebe-115">根據預設，.NET Framework 1.1 版的安裝期間所有現有的 ASP.NET 應用程式會自動重新設定成使用最新版本的.NET Framework。</span><span class="sxs-lookup"><span data-stu-id="2bebe-115">By default, during the installation of the .NET Framework version 1.1, all existing ASP.NET applications are automatically reconfigured to use the latest version of the .NET Framework.</span></span> <span data-ttu-id="2bebe-116">如果不想使用預設的.NET Framework 1.1 的 ASP.NET 應用程式，請按一下[這裡](#1)以了解如何防止這在安裝期間。</span><span class="sxs-lookup"><span data-stu-id="2bebe-116">If you do not want your ASP.NET applications to default to .NET Framework 1.1, click [here](#1) to learn how to prevent this during installation.</span></span>

<span data-ttu-id="2bebe-117">如果您為.NET Framework 1.1 更新您的 Web 伺服器，並將執行.NET Framework 1.0 的一或多個 Web 應用程式，您需要更新網際網路資訊服務 (IIS) 指令碼對應。</span><span class="sxs-lookup"><span data-stu-id="2bebe-117">If you update your Web server to .NET Framework 1.1 and want one or more Web applications to run .NET Framework 1.0, you need to update the Internet Information Services (IIS) Script Map.</span></span> <span data-ttu-id="2bebe-118">指令碼對應是將對應.aspx 檔案延伸模組特定的 Web 應用程式的.NET Framework 版本的機制。</span><span class="sxs-lookup"><span data-stu-id="2bebe-118">The script mapping is the mechanism to map the .aspx file extension for a specific Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="2bebe-119">按一下[這裡](#2)以了解如何對應至特定版本的.NET framework 的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2bebe-119">Click [here](#2) to learn how to map a Web application to a specific version of the .NET Framework.</span></span>

<span data-ttu-id="2bebe-120">您可以使用網際網路資訊管理員或 ASP.NET IIS 註冊工具 (Aspnet\_regiis.exe) 來尋找執行特定的 Web 應用程式的.NET Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="2bebe-120">You can use the Internet Information Manger or the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe) to find which .NET Framework version is running a particular Web application.</span></span> <span data-ttu-id="2bebe-121">按一下[這裡](#3)以了解如何尋找網站使用的.NET framework 版本。</span><span class="sxs-lookup"><span data-stu-id="2bebe-121">Click [here](#3) to learn how to find the version of the .NET Framework that a Web site is using.</span></span>

<span data-ttu-id="2bebe-122">移轉至.NET Framework 1.1 就是每一版.NET Framework 會使用自己的 Machine.config 檔案時，就會有一個匯入考量。</span><span class="sxs-lookup"><span data-stu-id="2bebe-122">One import consideration when migrating to .NET Framework 1.1 is that each version of the .NET Framework uses its own Machine.config file.</span></span> <span data-ttu-id="2bebe-123">如此一來，如果 Web 系統管理員變更至 Machine.config 檔案，這些變更必須移轉至.NET Framework 1.1 Machine.config 檔。</span><span class="sxs-lookup"><span data-stu-id="2bebe-123">As a result, if a Web administrator has made changes to the Machine.config file, those changes must be migrated to the .NET Framework 1.1 Machine.config file.</span></span>

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a><span data-ttu-id="2bebe-124">維護期間安裝.NET Framework 1.0 Web 應用程式的對應</span><span class="sxs-lookup"><span data-stu-id="2bebe-124">Maintaining your Web application's mapping to .NET Framework 1.0 during installation</span></span>

<span data-ttu-id="2bebe-125">根據預設，所有現有的 ASP.NET 應用程式會自動重新設定期間使用較新版本的.NET framework 安裝。</span><span class="sxs-lookup"><span data-stu-id="2bebe-125">By default, all existing ASP.NET applications are automatically reconfigured during installation to use the newer version of the .NET Framework.</span></span> <span data-ttu-id="2bebe-126">使用.NET framework 的較新版本，應用程式可以利用完整的改良措施和新的版本中所包含的新功能。</span><span class="sxs-lookup"><span data-stu-id="2bebe-126">Using the newer version of the .NET Framework, applications can take full advantage of improvements and new features included in the new release.</span></span> <span data-ttu-id="2bebe-127">在相同的時間，可能會想要更精確地控制哪些應用程式的 Web 系統管理員會更新，可防止自動重新對應所有現有的 ASP.NET 應用程式的.NET framework 的安裝期間。</span><span class="sxs-lookup"><span data-stu-id="2bebe-127">At the same time, the Web administrator, who might want granular control over which applications are updated, can prevent the automatic remapping of all existing ASP.NET applications during installation of the .NET Framework.</span></span>

<span data-ttu-id="2bebe-128">若要避免整個 ASP.NET 應用程式的.NET framework 的較新版本的自動重新對應，Web 系統管理員可以使用 /noaspupgrade 命令列選項與 Dotnetfx.exe 安裝程式。</span><span class="sxs-lookup"><span data-stu-id="2bebe-128">To prevent the automatic remapping of the entire ASP.NET application to the newer version of the .NET Framework, the Web administrator can use the /noaspupgrade command-line option with the Dotnetfx.exe setup program.</span></span>

<span data-ttu-id="2bebe-129">**若要避免總計重新對應至較新版本的 ASP.NET 應用程式**</span><span class="sxs-lookup"><span data-stu-id="2bebe-129">**To prevent total remapping of ASP.NET application to newer version**</span></span>

1. <span data-ttu-id="2bebe-130">移至**啟動**。</span><span class="sxs-lookup"><span data-stu-id="2bebe-130">Go to **Start**.</span></span>
2. <span data-ttu-id="2bebe-131">按一下**執行**。</span><span class="sxs-lookup"><span data-stu-id="2bebe-131">Click on **run**.</span></span>
3. <span data-ttu-id="2bebe-132">輸入 **cmd**。</span><span class="sxs-lookup"><span data-stu-id="2bebe-132">Type **cmd**.</span></span>
4. <span data-ttu-id="2bebe-133">按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="2bebe-133">Click **OK**.</span></span>  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. <span data-ttu-id="2bebe-134">從命令提示字元中，輸入下列行加入開始安裝的.NET framework: **Dotnetfx.exe 包裝 」 安裝 /noaspupgrade？**。</span><span class="sxs-lookup"><span data-stu-id="2bebe-134">From the command prompt, type the following line to start the installation of the .NET Framework: **Dotnetfx.exe /c:"install /noaspupgrade?**.</span></span>  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. <span data-ttu-id="2bebe-135">按一下**是**中 Microsoft.NET Framework 1.1 安裝程式。</span><span class="sxs-lookup"><span data-stu-id="2bebe-135">Click **Yes** in the Microsoft .NET Framework 1.1 Setup.</span></span> <span data-ttu-id="2bebe-136">這會啟動.NET Framework 1.1 的安裝程序。</span><span class="sxs-lookup"><span data-stu-id="2bebe-136">This will start the setup process of the .NET Framework 1.1.</span></span>  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a><span data-ttu-id="2bebe-137">對應至特定版本的.NET framework 的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="2bebe-137">Map a Web application to a specific version of the .NET Framework</span></span>

<span data-ttu-id="2bebe-138">.NET Framework 的每個版本都包括某個版本的 ASP.NET IIS 註冊工具 (Aspnet\_regiis.exe)。</span><span class="sxs-lookup"><span data-stu-id="2bebe-138">Each version of the .NET Framework includes a version of the ASP.NET IIS Registration Tool (Aspnet\_regiis.exe).</span></span> <span data-ttu-id="2bebe-139">此工具可讓系統管理員指定的特定版本的.NET Framework 下執行 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2bebe-139">This tool enables administrators to specify that a Web application be run under a particular version of the .NET Framework.</span></span> <span data-ttu-id="2bebe-140">這被指對應的.NET Framework 版本的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2bebe-140">This is referred to as mapping a Web application to a version of the .NET Framework.</span></span> <span data-ttu-id="2bebe-141">系統管理員必須選取 Aspnet\_regiis.exe 對應於要與 Web 應用程式相關聯的.NET framework 版本。</span><span class="sxs-lookup"><span data-stu-id="2bebe-141">Administrators must select the Aspnet\_regiis.exe that corresponds to the version of the .NET Framework that will be associated with the Web application.</span></span> <span data-ttu-id="2bebe-142">例如，想要指定的網站使用.NET Framework 1.1 的系統管理員必須使用 Aspnet\_regiis.exe 隨附於.NET Framework 1.1。</span><span class="sxs-lookup"><span data-stu-id="2bebe-142">For example, an administrator who wants to specify that a Web site use .NET Framework 1.1 must use the Aspnet\_regiis.exe that comes with .NET Framework 1.1.</span></span>

<span data-ttu-id="2bebe-143">Aspnet\_版本 1.0 regiis.exe 位於：</span><span class="sxs-lookup"><span data-stu-id="2bebe-143">The Aspnet\_regiis.exe for version 1.0 is located at:</span></span>

- <span data-ttu-id="2bebe-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705** \aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="2bebe-144">C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705**\aspnet\_regiis</span></span>

<span data-ttu-id="2bebe-145">Aspnet\_regiis.exe 版本 1，1 是位於：</span><span class="sxs-lookup"><span data-stu-id="2bebe-145">The Aspnet\_regiis.exe for version 1,1 is located at:</span></span>

- <span data-ttu-id="2bebe-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322** \aspnet\_regiis</span><span class="sxs-lookup"><span data-stu-id="2bebe-146">C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322**\aspnet\_regiis</span></span>

<span data-ttu-id="2bebe-147">Aspnet\_regiis.exe 提供兩個對應的 Web 應用程式的指令碼選項：</span><span class="sxs-lookup"><span data-stu-id="2bebe-147">The Aspnet\_regiis.exe provides two options for script mapping a Web application:</span></span>

- <span data-ttu-id="2bebe-148">**-s**設定指令碼對應和其子系的路徑中目錄。</span><span class="sxs-lookup"><span data-stu-id="2bebe-148">**-s** sets the script map in the path and in its child directories.</span></span>
- <span data-ttu-id="2bebe-149">**-sn**只在路徑中設定指令碼對應。</span><span class="sxs-lookup"><span data-stu-id="2bebe-149">**-sn** sets the script map in the path only.</span></span>

<span data-ttu-id="2bebe-150">此路徑定義中 W3SVC/ROOT 表單定義 Web 應用程式 IIS 中繼資料路徑 / {WebSiteNumber} / {應用程式\_名稱}。</span><span class="sxs-lookup"><span data-stu-id="2bebe-150">The path defines the Web application IIS metadata path, which is defined in the form of W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}.</span></span> <span data-ttu-id="2bebe-151">例如，呼叫預設的網站之下的入口網站 Web 應用程式，metabase 路徑是 W3SVC/1/ROOT/入口網站。</span><span class="sxs-lookup"><span data-stu-id="2bebe-151">For example, for a Web application called Portal located under the default Web site, the metabase path is W3SVC/1/ROOT/Portal.</span></span>

![](side-by-side-with-10/_static/image4.gif)

<span data-ttu-id="2bebe-152">請注意，您也可以使用稱為 Metabase 編輯器工具，以取得的 metabase 路徑。</span><span class="sxs-lookup"><span data-stu-id="2bebe-152">Note You can also use a tool called the Metabase Editor to get the metabase path.</span></span> <span data-ttu-id="2bebe-153">您可以從 「 Microsoft 支援服務 」 網站下載此工具[https://support.microsoft.com/default.aspx?scid=kb;en-us;232068。](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span><span class="sxs-lookup"><span data-stu-id="2bebe-153">You can download this tool from the Microsoft Support site at [https://support.microsoft.com/default.aspx?scid=kb;en-us;232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)</span></span>

- <span data-ttu-id="2bebe-154">執行 Aspnet\_regiis.exe-s W3SVC/1/ROOT/入口網站更新入口網站 IIS 的指令碼對應和其 subapplication。</span><span class="sxs-lookup"><span data-stu-id="2bebe-154">Run Aspnet\_regiis.exe -s W3SVC/1/ROOT/Portal to update the portal IIS script map and its subapplication.</span></span>  
  
    ![](side-by-side-with-10/_static/image5.gif)

- <span data-ttu-id="2bebe-155">執行 Aspnet\_regiis.exe-sn W3SVC/1/ROOT/入口網站更新的入口網站的 IIS 指令碼對應，而不會影響入口網站中的應用程式？ s 子目錄。</span><span class="sxs-lookup"><span data-stu-id="2bebe-155">Run Aspnet\_regiis.exe -sn W3SVC/1/ROOT/Portal to update the portal IIS script map, without affecting applications in the portal?s subdirectories.</span></span>  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a><span data-ttu-id="2bebe-156">尋找 Web 應用程式使用的.NET Framework 版本</span><span class="sxs-lookup"><span data-stu-id="2bebe-156">Find the .NET Framework version that a Web application is using</span></span>

<span data-ttu-id="2bebe-157">系統管理員可以使用網際網路服務管理員來尋找.NET Framework 版本會執行的網站。</span><span class="sxs-lookup"><span data-stu-id="2bebe-157">An administrator can use the Internet Service Manager to find which version of the .NET Framework runs a Web site.</span></span> <span data-ttu-id="2bebe-158">不同的作業系統版本會以不同的方式啟動網際網路服務管理員。</span><span class="sxs-lookup"><span data-stu-id="2bebe-158">Different operating system versions launch the Internet Service Manager differently.</span></span> <span data-ttu-id="2bebe-159">若要啟動 service manager，請遵循下面所列的步驟。</span><span class="sxs-lookup"><span data-stu-id="2bebe-159">To start the service manager, follow the steps listed below.</span></span>

<span data-ttu-id="2bebe-160">**若要啟動網際網路服務管理員**</span><span class="sxs-lookup"><span data-stu-id="2bebe-160">**To start Internet Service Manager**</span></span>

1. <span data-ttu-id="2bebe-161">移至**啟動**。</span><span class="sxs-lookup"><span data-stu-id="2bebe-161">Go to **Start**.</span></span>
2. <span data-ttu-id="2bebe-162">按一下**執行**。</span><span class="sxs-lookup"><span data-stu-id="2bebe-162">Click on **run**.</span></span>
3. <span data-ttu-id="2bebe-163">型別**inetmgr**。</span><span class="sxs-lookup"><span data-stu-id="2bebe-163">Type **inetmgr**.</span></span>  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. <span data-ttu-id="2bebe-164">從網際網路服務管理員中，選取您想要知道其版本的.NET framework 的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2bebe-164">From the Internet Service Manager, select the Web application whose version of the .NET Framework you want to know.</span></span>  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. <span data-ttu-id="2bebe-165">以滑鼠右鍵按一下 Web 應用程式，然後按一下**屬性。**</span><span class="sxs-lookup"><span data-stu-id="2bebe-165">Right-click on the Web application, and click on **Properties.**</span></span>  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. <span data-ttu-id="2bebe-166">從 [屬性] 視窗中，選取**組態。**</span><span class="sxs-lookup"><span data-stu-id="2bebe-166">From the Property window, select **Configuration.**</span></span>  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. <span data-ttu-id="2bebe-167">從應用程式對應資料表中，選取 **.aspx**，然後按一下**編輯**。</span><span class="sxs-lookup"><span data-stu-id="2bebe-167">From the application mapping table, select **.aspx**, and click **Edit**.</span></span>  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. <span data-ttu-id="2bebe-168">從**可執行檔** 文字方塊中，捲動的版本目錄檢視。</span><span class="sxs-lookup"><span data-stu-id="2bebe-168">From the **Executable** text box, look at the version directory by scrolling.</span></span> <span data-ttu-id="2bebe-169">如果版本目錄 v.1.1.4322，應用程式會對應到.NET Framework 1.1。</span><span class="sxs-lookup"><span data-stu-id="2bebe-169">If the version directory is v.1.1.4322, the application is mapped to .NET Framework 1.1.</span></span> <span data-ttu-id="2bebe-170">相反地，如果 v1.0.3705 版本目錄，應用程式的動作就會對應到.NET Framework 1.0。</span><span class="sxs-lookup"><span data-stu-id="2bebe-170">Conversely, if the version directory is v1.0.3705, the application is mapped to .NET Framework 1.0.</span></span>  
  
    ![](side-by-side-with-10/_static/image12.gif)
