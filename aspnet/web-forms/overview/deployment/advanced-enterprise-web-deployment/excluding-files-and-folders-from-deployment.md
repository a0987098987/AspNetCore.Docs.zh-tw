---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: 從部署中排除檔案和資料夾 |Microsoft Docs
author: jrjlee
description: 本主題描述如何您可以排除檔案和資料夾的 web 部署封裝時您建置和封裝 web 應用程式專案。
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: fd7357a94ab09effcec86f3725a37cfb2ef4746a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826026"
---
<a name="excluding-files-and-folders-from-deployment"></a><span data-ttu-id="3fa75-103">從部署中排除檔案和資料夾</span><span class="sxs-lookup"><span data-stu-id="3fa75-103">Excluding Files and Folders from Deployment</span></span>
====================
<span data-ttu-id="3fa75-104">藉由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="3fa75-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="3fa75-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="3fa75-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="3fa75-106">本主題描述如何您可以排除檔案和資料夾的 web 部署封裝時您建置和封裝 web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="3fa75-106">This topic describes how you can exclude files and folders from a web deployment package when you build and package a web application project.</span></span>


<span data-ttu-id="3fa75-107">本主題是構成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程的一部分本教學課程系列會使用範例解決方案&#x2014;[連絡管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="3fa75-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="3fa75-108">這些教學課程的核心的部署方法根據分割專案檔案方法中所述[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置流程控制的兩個專案檔&#x2014;包含建置適用於每個目的地環境中和包含環境特定建置和部署設定的指示。</span><span class="sxs-lookup"><span data-stu-id="3fa75-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="3fa75-109">在建置階段的特定環境的專案檔會合併到無從驗證環境的專案檔中，以構成一組完整的組建指示。</span><span class="sxs-lookup"><span data-stu-id="3fa75-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="overview"></a><span data-ttu-id="3fa75-110">總覽</span><span class="sxs-lookup"><span data-stu-id="3fa75-110">Overview</span></span>

<span data-ttu-id="3fa75-111">當您建置 Visual Studio 2010 中的 web 應用程式專案時，Web 發行管線 (WPP) 可讓您擴充此建置程序，封裝成可部署的 web 套件已編譯的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3fa75-111">When you build a web application project in Visual Studio 2010, the Web Publishing Pipeline (WPP) lets you extend this build process by packaging your compiled web application into a deployable web package.</span></span> <span data-ttu-id="3fa75-112">然後，您可以使用 Internet Information Services (IIS) Web Deployment Tool (Web Deploy) 至遠端 IIS web 伺服器，將此 web 封裝部署，或匯入 web 套件手動透過 IIS 管理員。</span><span class="sxs-lookup"><span data-stu-id="3fa75-112">You can then use the Internet Information Services (IIS) Web Deployment Tool (Web Deploy) to deploy this web package to a remote IIS web server, or import the web package manually through IIS Manager.</span></span> <span data-ttu-id="3fa75-113">此封裝程序會說明[建置和封裝 Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)。</span><span class="sxs-lookup"><span data-stu-id="3fa75-113">This packaging process is explained in [Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).</span></span>

<span data-ttu-id="3fa75-114">到底是您在控制取得包含在 web 中的內容套件嗎？</span><span class="sxs-lookup"><span data-stu-id="3fa75-114">So how do you control what gets included in your web package?</span></span> <span data-ttu-id="3fa75-115">在 Visual Studio 中的專案設定，透過基礎的專案檔中，許多案例提供足夠的控制。</span><span class="sxs-lookup"><span data-stu-id="3fa75-115">The project settings in Visual Studio, through the underlying project file, provide sufficient control for a lot of scenarios.</span></span> <span data-ttu-id="3fa75-116">不過，在某些情況下，您可能想要量身訂做您特定的目的地環境的 web 封裝的內容。</span><span class="sxs-lookup"><span data-stu-id="3fa75-116">However, in some cases you may want to tailor the contents of your web package to specific destination environments.</span></span> <span data-ttu-id="3fa75-117">例如，您可能要包含記錄檔的資料夾，當您應用程式部署至測試環境，但當您部署至預備或生產環境應用程式中排除的資料夾。</span><span class="sxs-lookup"><span data-stu-id="3fa75-117">For example, you might want to include a folder for log files when you deploy your application to a test environment but exclude the folder when you deploy the application to a staging or production environment.</span></span> <span data-ttu-id="3fa75-118">本主題將說明如何執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="3fa75-118">This topic will show you how to do this.</span></span>

## <a name="what-gets-included-by-default"></a><span data-ttu-id="3fa75-119">根據預設，取得包含哪些？</span><span class="sxs-lookup"><span data-stu-id="3fa75-119">What Gets Included by Default?</span></span>

<span data-ttu-id="3fa75-120">當您在 Visual Studio 中，設定您的 web 應用程式專案屬性**部署的項目**清單**封裝/發行 Web**頁面可讓您指定您想要包含在您的 web 部署封裝。</span><span class="sxs-lookup"><span data-stu-id="3fa75-120">When you configure your web application project properties in Visual Studio, the **Items to deploy** list on the **Package/Publish Web** page lets you specify what you want to include in your web deployment package.</span></span> <span data-ttu-id="3fa75-121">根據預設，這設定為**僅執行此應用程式所需的檔案**。</span><span class="sxs-lookup"><span data-stu-id="3fa75-121">By default, this is set to **Only files needed to run this application**.</span></span>

![](excluding-files-and-folders-from-deployment/_static/image1.png)

<span data-ttu-id="3fa75-122">當您選擇**僅執行此應用程式所需的檔案**，WPP 會嘗試判斷哪些檔案應該將 web 套件。</span><span class="sxs-lookup"><span data-stu-id="3fa75-122">When you choose **Only files needed to run this application**, the WPP will try to determine which files should be added to the web package.</span></span> <span data-ttu-id="3fa75-123">包括：</span><span class="sxs-lookup"><span data-stu-id="3fa75-123">This includes:</span></span>

- <span data-ttu-id="3fa75-124">所有組建都輸出的專案。</span><span class="sxs-lookup"><span data-stu-id="3fa75-124">All the build outputs for the project.</span></span>
- <span data-ttu-id="3fa75-125">任何檔案以建置動作為標示**內容**。</span><span class="sxs-lookup"><span data-stu-id="3fa75-125">Any files marked with a build action of **Content**.</span></span>

> [!NOTE]
> <span data-ttu-id="3fa75-126">此檔案被包含來判斷哪些檔案来包含的邏輯：</span><span class="sxs-lookup"><span data-stu-id="3fa75-126">The logic that determines which files to include is contained in this file:</span></span>   
> <span data-ttu-id="3fa75-127">*%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*</span><span class="sxs-lookup"><span data-stu-id="3fa75-127">*%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*</span></span>


## <a name="excluding-specific-files-and-folders"></a><span data-ttu-id="3fa75-128">排除特定的檔案和資料夾</span><span class="sxs-lookup"><span data-stu-id="3fa75-128">Excluding Specific Files and Folders</span></span>

<span data-ttu-id="3fa75-129">在某些情況下，您會想更細微的控制，這部署檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="3fa75-129">In some cases, you'll want more fine-grained control over which files and folders are deployed.</span></span> <span data-ttu-id="3fa75-130">如果您知道您想要提前排除哪些的檔案的時間，而且排除適用於所有的目的地環境，您可以輕鬆地設定**建置動作**的每個檔案**無**。</span><span class="sxs-lookup"><span data-stu-id="3fa75-130">If you know which files you want to exclude ahead of time, and the exclusion applies to all destination environments, you can simply set the **Build Action** of each file to **None**.</span></span>

<span data-ttu-id="3fa75-131">**若要從部署中排除特定檔案**</span><span class="sxs-lookup"><span data-stu-id="3fa75-131">**To exclude specific files from deployment**</span></span>

1. <span data-ttu-id="3fa75-132">在 [**方案總管**] 視窗中，以滑鼠右鍵按一下檔案，然後**屬性**。</span><span class="sxs-lookup"><span data-stu-id="3fa75-132">In the **Solution Explorer** window, right-click the file, and then click **Properties**.</span></span>
2. <span data-ttu-id="3fa75-133">在 **屬性**視窗，請在**建置動作**列中選取**None**。</span><span class="sxs-lookup"><span data-stu-id="3fa75-133">In the **Properties** window, in the **Build Action** row, select **None**.</span></span>

<span data-ttu-id="3fa75-134">不過，這種方法不一定很方便。</span><span class="sxs-lookup"><span data-stu-id="3fa75-134">However, this approach is not always convenient.</span></span> <span data-ttu-id="3fa75-135">例如，您可能想要不同的檔案和資料夾會包含根據您的目的地環境，並從 Visual Studio 外部。</span><span class="sxs-lookup"><span data-stu-id="3fa75-135">For example, you may want to vary which files and folders are included according to your destination environment, and from outside Visual Studio.</span></span> <span data-ttu-id="3fa75-136">例如，在連絡人管理員範例方案中，看看 ContactManager.Mvc 專案的內容：</span><span class="sxs-lookup"><span data-stu-id="3fa75-136">For example, in the Contact Manager sample solution, take a look at the contents of the ContactManager.Mvc project:</span></span>

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- <span data-ttu-id="3fa75-137">[內部] 資料夾包含一些 SQL 指令碼的開發人員用來建立、 卸除，並填入本機資料庫進行開發。</span><span class="sxs-lookup"><span data-stu-id="3fa75-137">The Internal folder contains some SQL scripts that the developer uses to create, drop, and populate local databases for development purposes.</span></span> <span data-ttu-id="3fa75-138">在此資料夾中為 nothing 應該部署至預備或生產環境。</span><span class="sxs-lookup"><span data-stu-id="3fa75-138">Nothing in this folder should be deployed to a staging or production environment.</span></span>
- <span data-ttu-id="3fa75-139">指令碼 資料夾包含數個 JavaScript 檔案。</span><span class="sxs-lookup"><span data-stu-id="3fa75-139">The Scripts folder contains several JavaScript files.</span></span> <span data-ttu-id="3fa75-140">許多這些檔案的單純以支援偵錯，或提供 Visual Studio 中的 IntelliSense。</span><span class="sxs-lookup"><span data-stu-id="3fa75-140">A lot of these files are included purely to support debugging or provide IntelliSense in Visual Studio.</span></span> <span data-ttu-id="3fa75-141">這當中有些檔案應該不會部署至預備或生產環境中。</span><span class="sxs-lookup"><span data-stu-id="3fa75-141">Some of these files should not be deployed to staging or production environments.</span></span> <span data-ttu-id="3fa75-142">不過，您可能要將其部署至開發人員測試環境，以利於進行疑難排解。</span><span class="sxs-lookup"><span data-stu-id="3fa75-142">However, you may want to deploy them to a developer test environment to facilitate troubleshooting.</span></span>

<span data-ttu-id="3fa75-143">雖然您無法管理您的專案檔，以排除特定檔案和資料夾，還有更簡單的方法。</span><span class="sxs-lookup"><span data-stu-id="3fa75-143">Although you could manipulate your project files to exclude specific files and folders, there is an easier way.</span></span> <span data-ttu-id="3fa75-144">WPP 還有一個機制，藉由建置名為的項目清單中排除檔案和資料夾**ExcludeFromPackageFolders**並**ExcludeFromPackageFiles**。</span><span class="sxs-lookup"><span data-stu-id="3fa75-144">The WPP includes a mechanism to exclude files and folders by building item lists named **ExcludeFromPackageFolders** and **ExcludeFromPackageFiles**.</span></span> <span data-ttu-id="3fa75-145">您可以將您自己的項目加入至這些清單，來擴充這項機制。</span><span class="sxs-lookup"><span data-stu-id="3fa75-145">You can extend this mechanism by adding your own items to these lists.</span></span> <span data-ttu-id="3fa75-146">若要這樣做，您需要完成下列高階步驟：</span><span class="sxs-lookup"><span data-stu-id="3fa75-146">To do this, you need to complete these high-level steps:</span></span>

1. <span data-ttu-id="3fa75-147">建立自訂的專案檔名為 *[專案名稱].wpp.targets*與您的專案檔相同的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="3fa75-147">Create a custom project file named *[project name].wpp.targets* in the same folder as your project file.</span></span>

    > [!NOTE]
    > <span data-ttu-id="3fa75-148">*.Wpp.targets*檔案必須與您的 web 應用程式專案檔相同的資料夾中，前往&#x2014;，例如*ContactManager.Mvc.csproj*&#x2014;而不是任何自訂的相同資料夾中用來控制組建和部署程序的專案檔案。</span><span class="sxs-lookup"><span data-stu-id="3fa75-148">The *.wpp.targets* file needs to go in the same folder as your web application project file&#x2014;for example, *ContactManager.Mvc.csproj*&#x2014;rather than in the same folder as any custom project files you use to control the build and deployment process.</span></span>
2. <span data-ttu-id="3fa75-149">在  *.wpp.targets*檔案中，新增**ItemGroup**項目。</span><span class="sxs-lookup"><span data-stu-id="3fa75-149">In the *.wpp.targets* file, add an **ItemGroup** element.</span></span>
3. <span data-ttu-id="3fa75-150">在  **ItemGroup**項目，新增**ExcludeFromPackageFolders**並**ExcludeFromPackageFiles**来排除特定檔案和資料夾做為必要項目。</span><span class="sxs-lookup"><span data-stu-id="3fa75-150">In the **ItemGroup** element, add **ExcludeFromPackageFolders** and **ExcludeFromPackageFiles** items to exclude specific files and folders as required.</span></span>

<span data-ttu-id="3fa75-151">這是基本的結構 *.wpp.targets*檔案：</span><span class="sxs-lookup"><span data-stu-id="3fa75-151">This is the basic structure of this *.wpp.targets* file:</span></span>


[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]


<span data-ttu-id="3fa75-152">請注意，每個項目會包含名為的項目中繼資料元素**FromTarget**。</span><span class="sxs-lookup"><span data-stu-id="3fa75-152">Note that each item includes an item metadata element named **FromTarget**.</span></span> <span data-ttu-id="3fa75-153">這是選擇性的值，並不會影響在建置程序;它只是表示特定的檔案或資料夾已省略為什麼如果有人檢閱組建記錄檔。</span><span class="sxs-lookup"><span data-stu-id="3fa75-153">This is an optional value that doesn't affect the build process; it simply serves to indicate why particular files or folders were omitted if someone reviews the build logs.</span></span>

## <a name="excluding-files-and-folders-from-a-web-package"></a><span data-ttu-id="3fa75-154">網頁套件中排除檔案和資料夾</span><span class="sxs-lookup"><span data-stu-id="3fa75-154">Excluding Files and Folders from a Web Package</span></span>

<span data-ttu-id="3fa75-155">下一個程序說明如何新增 *.wpp.targets* web 應用程式專案，以及如何使用檔案時所要排除特定檔案和資料夾從網頁套件建置您的專案檔案。</span><span class="sxs-lookup"><span data-stu-id="3fa75-155">The next procedure shows you how to add a *.wpp.targets* file to a web application project and how to use the file to exclude specific files and folders from the web package when you build your project.</span></span>

<span data-ttu-id="3fa75-156">**若要從 web 部署套件中排除檔案和資料夾**</span><span class="sxs-lookup"><span data-stu-id="3fa75-156">**To exclude files and folders from a web deployment package**</span></span>

1. <span data-ttu-id="3fa75-157">Visual Studio 2010 中開啟您的方案。</span><span class="sxs-lookup"><span data-stu-id="3fa75-157">Open your solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="3fa75-158">中**方案總管 中** 視窗中，以滑鼠右鍵按一下您 web 應用程式的專案節點 (例如**ContactManager.Mvc**)，指向**新增**，然後按一下  **新項目**。</span><span class="sxs-lookup"><span data-stu-id="3fa75-158">In the **Solution Explorer** window, right-click your web application project node (for example, **ContactManager.Mvc**), point to **Add**, and then click **New Item**.</span></span>
3. <span data-ttu-id="3fa75-159">在 **加入新項目**對話方塊中，選取**XML 檔案**範本。</span><span class="sxs-lookup"><span data-stu-id="3fa75-159">In the **Add New Item** dialog box, select the **XML File** template.</span></span>
4. <span data-ttu-id="3fa75-160">在 **名稱**方塊中，輸入 \*[專案名稱] \***.wpp.targets** (比方說， **ContactManager.Mvc.wpp.targets**)，然後按一下 **新增**.</span><span class="sxs-lookup"><span data-stu-id="3fa75-160">In the **Name** box, type *[project name]\*\*\*.wpp.targets*\* (for example, **ContactManager.Mvc.wpp.targets**), and then click **Add**.</span></span>

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > <span data-ttu-id="3fa75-161">如果您將新的項目加入專案的根節點時，檔案會建立專案檔相同資料夾中。</span><span class="sxs-lookup"><span data-stu-id="3fa75-161">If you add a new item to the root node of a project, the file is created in the same folder as the project file.</span></span> <span data-ttu-id="3fa75-162">您可以在 Windows 檔案總管中開啟資料夾來確認。</span><span class="sxs-lookup"><span data-stu-id="3fa75-162">You can verify this by opening the folder in Windows Explorer.</span></span>
5. <span data-ttu-id="3fa75-163">在檔案中新增**專案**項目並**ItemGroup**項目：</span><span class="sxs-lookup"><span data-stu-id="3fa75-163">In the file, add a **Project** element and an **ItemGroup** element:</span></span>

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. <span data-ttu-id="3fa75-164">如果您想要排除資料夾不進行網頁套件，新增**ExcludeFromPackageFolders**項目**ItemGroup**項目：</span><span class="sxs-lookup"><span data-stu-id="3fa75-164">If you want to exclude folders from the web package, add an **ExcludeFromPackageFolders** element to the **ItemGroup** element:</span></span>

   1. <span data-ttu-id="3fa75-165">在  **Include**屬性，請提供以分號分隔的清單，您想要排除的資料夾。</span><span class="sxs-lookup"><span data-stu-id="3fa75-165">In the **Include** attribute, provide a semicolon-separated list of the folders you want to exclude.</span></span>
   2. <span data-ttu-id="3fa75-166">在  **FromTarget**中繼資料元素，提供有意義的值，指出資料夾會被排除的原因，如名稱 *.wpp.targets*檔案。</span><span class="sxs-lookup"><span data-stu-id="3fa75-166">In the **FromTarget** metadata element, provide a meaningful value to indicate why the folders are being excluded, like the name of the *.wpp.targets* file.</span></span>

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. <span data-ttu-id="3fa75-167">如果您想要從網頁套件中排除檔案，新增**ExcludeFromPackageFiles**項目**ItemGroup**項目：</span><span class="sxs-lookup"><span data-stu-id="3fa75-167">If you want to exclude files from the web package, add an **ExcludeFromPackageFiles** element to the **ItemGroup** element:</span></span>

   1. <span data-ttu-id="3fa75-168">在  **Include**屬性，請提供您想要排除的檔案以分號分隔清單。</span><span class="sxs-lookup"><span data-stu-id="3fa75-168">In the **Include** attribute, provide a semicolon-separated list of the files you want to exclude.</span></span>
   2. <span data-ttu-id="3fa75-169">在  **FromTarget**中繼資料元素，提供有意義的值，指出為什麼要排除檔案，如名稱 *.wpp.targets*檔案。</span><span class="sxs-lookup"><span data-stu-id="3fa75-169">In the **FromTarget** metadata element, provide a meaningful value to indicate why the files are being excluded, like the name of the *.wpp.targets* file.</span></span>

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. <span data-ttu-id="3fa75-170">*[專案名稱].wpp.targets*檔案現在應該類似這樣：</span><span class="sxs-lookup"><span data-stu-id="3fa75-170">The *[project name].wpp.targets* file should now resemble this:</span></span>

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. <span data-ttu-id="3fa75-171">儲存並關閉 *[專案名稱].wpp.targets*檔案。</span><span class="sxs-lookup"><span data-stu-id="3fa75-171">Save and close the *[project name].wpp.targets* file.</span></span>

<span data-ttu-id="3fa75-172">下次當您建置和封裝您的 web 應用程式專案，WPP 將會自動偵測 *.wpp.targets*檔案。</span><span class="sxs-lookup"><span data-stu-id="3fa75-172">The next time you build and package your web application project, the WPP will automatically detect the *.wpp.targets* file.</span></span> <span data-ttu-id="3fa75-173">任何檔案和您指定的資料夾將不會包含在網頁套件中。</span><span class="sxs-lookup"><span data-stu-id="3fa75-173">Any files and folders you specified will not be included in the web package.</span></span>

## <a name="conclusion"></a><span data-ttu-id="3fa75-174">結論</span><span class="sxs-lookup"><span data-stu-id="3fa75-174">Conclusion</span></span>

<span data-ttu-id="3fa75-175">本主題說明如何排除特定檔案和資料夾，當您建立自訂建置 web 封裝時， *.wpp.targets*與您的 web 應用程式專案檔相同的資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="3fa75-175">This topic described how to exclude specific files and folders when you build a web package, by creating a custom *.wpp.targets* file in the same folder as your web application project file.</span></span>

## <a name="further-reading"></a><span data-ttu-id="3fa75-176">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="3fa75-176">Further Reading</span></span>

<span data-ttu-id="3fa75-177">如需有關如何使用自訂的 Microsoft Build Engine (MSBuild) 專案檔來控制部署程序的詳細資訊，請參閱 <<c0> [ 了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)並[了解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。</span><span class="sxs-lookup"><span data-stu-id="3fa75-177">For more information on using custom Microsoft Build Engine (MSBuild) project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span> <span data-ttu-id="3fa75-178">如需有關封裝和部署程序的詳細資訊，請參閱 <<c0> [ 建置和封裝 Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)，[設定網頁套件部署的參數](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)，和[部署網頁套件](../web-deployment-in-the-enterprise/deploying-web-packages.md)。</span><span class="sxs-lookup"><span data-stu-id="3fa75-178">For more information on the packaging and deployment process, see [Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Configuring Parameters for Web Package Deployment](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), and [Deploying Web Packages](../web-deployment-in-the-enterprise/deploying-web-packages.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3fa75-179">[上一頁](deploying-membership-databases-to-enterprise-environments.md)
> [下一頁](taking-web-applications-offline-with-web-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="3fa75-179">[Previous](deploying-membership-databases-to-enterprise-environments.md)
[Next](taking-web-applications-offline-with-web-deploy.md)</span></span>
