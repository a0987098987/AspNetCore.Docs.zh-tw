---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: 建立和執行部署指令檔案 |Microsoft 文件
author: jrjlee
description: 本主題描述如何建立命令檔，以供您執行使用 Microsoft Build Engine (MSBuild) 專案檔做為單一步驟，重新部署...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: e5fb034a67bc9f2ea549af269eae51a49acc4d98
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="creating-and-running-a-deployment-command-file"></a><span data-ttu-id="28cd1-103">建立和執行部署指令檔</span><span class="sxs-lookup"><span data-stu-id="28cd1-103">Creating and Running a Deployment Command File</span></span>
====================
<span data-ttu-id="28cd1-104">由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="28cd1-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="28cd1-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="28cd1-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="28cd1-106">本主題描述如何建立命令檔，以供您執行使用 Microsoft Build Engine (MSBuild) 專案檔，以逐步執行、 可重複執行的處理程序的部署。</span><span class="sxs-lookup"><span data-stu-id="28cd1-106">This topic describes how to build a command file that will let you run a deployment using Microsoft Build Engine (MSBuild) project files as a single-step, repeatable process.</span></span>


<span data-ttu-id="28cd1-107">本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案&#x2014;[連絡人管理員](the-contact-manager-solution.md)方案&#x2014;來表示實際層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 通訊的 web 應用程式Foundation (WCF) 服務與資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="28cd1-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="28cd1-108">這些教學課程的核心的部署方法為基礎所說明的分割專案檔案方法[瞭解建置程序](understanding-the-build-process.md)，在建置流程控制的兩個專案檔&#x2014;一個包含建置適用於每個目的地環境中和包含特定環境的建置和部署設定的指示。</span><span class="sxs-lookup"><span data-stu-id="28cd1-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="28cd1-109">在建置時，環境特定專案檔就會合併環境無從驗證專案檔來形成一組完整組建指示。</span><span class="sxs-lookup"><span data-stu-id="28cd1-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="process-overview"></a><span data-ttu-id="28cd1-110">處理程序概觀</span><span class="sxs-lookup"><span data-stu-id="28cd1-110">Process Overview</span></span>

<span data-ttu-id="28cd1-111">在本主題中，您將學習如何建立及執行使用這些專案檔來執行可重複部署到您的目標環境的命令檔。</span><span class="sxs-lookup"><span data-stu-id="28cd1-111">In this topic, you'll learn how to create and run a command file that uses these project files to perform a repeatable deployment to your target environment.</span></span> <span data-ttu-id="28cd1-112">基本上，命令檔只需要包含 MSBuild 命令的：</span><span class="sxs-lookup"><span data-stu-id="28cd1-112">Essentially, the command file simply needs to contain an MSBuild command that:</span></span>

- <span data-ttu-id="28cd1-113">會告知執行環境無從驗證的 MSBuild *Publish.proj*檔案。</span><span class="sxs-lookup"><span data-stu-id="28cd1-113">Tells MSBuild to execute the environment-agnostic *Publish.proj* file.</span></span>
- <span data-ttu-id="28cd1-114">會告知*Publish.proj*檔案中的檔案包含特定環境的專案設定，以及到哪裡尋找它。</span><span class="sxs-lookup"><span data-stu-id="28cd1-114">Tells the *Publish.proj* file which file contains the environment-specific project settings and where to find it.</span></span>

## <a name="create-an-msbuild-command"></a><span data-ttu-id="28cd1-115">建立 MSBuild 命令</span><span class="sxs-lookup"><span data-stu-id="28cd1-115">Create an MSBuild Command</span></span>

<span data-ttu-id="28cd1-116">中所述[瞭解建置程序](understanding-the-build-process.md)，環境特定專案檔&#x2014;，例如*Env Dev.proj*&#x2014;設計來匯入環境無從驗證*Publish.proj*在建置階段檔案。</span><span class="sxs-lookup"><span data-stu-id="28cd1-116">As described in [Understanding the Build Process](understanding-the-build-process.md), the environment-specific project file&#x2014;for example, *Env-Dev.proj*&#x2014;is designed to be imported into the environment-agnostic *Publish.proj* file at build time.</span></span> <span data-ttu-id="28cd1-117">結合，這兩個檔案，提供一組完整的指示，告知 MSBuild 如何建置和部署您的方案。</span><span class="sxs-lookup"><span data-stu-id="28cd1-117">Together, these two files provide a complete set of instructions that tell MSBuild how to build and deploy your solution.</span></span>

<span data-ttu-id="28cd1-118">*Publish.proj*檔案使用**匯入**環境特定專案檔案匯入的項目。</span><span class="sxs-lookup"><span data-stu-id="28cd1-118">The *Publish.proj* file uses an **Import** element to import the environment-specific project file.</span></span>


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


<span data-ttu-id="28cd1-119">因此，當您使用 MSBuild.exe 來建置及部署方案，請連絡管理員，您需要：</span><span class="sxs-lookup"><span data-stu-id="28cd1-119">As such, when you use MSBuild.exe to build and deploy the Contact Manager solution, you need to:</span></span>

- <span data-ttu-id="28cd1-120">在執行 MSBuild.exe *Publish.proj*檔案。</span><span class="sxs-lookup"><span data-stu-id="28cd1-120">Run MSBuild.exe on the *Publish.proj* file.</span></span>
- <span data-ttu-id="28cd1-121">提供名為命令列參數來指定環境特定專案檔的位置**TargetEnvPropsFile**。</span><span class="sxs-lookup"><span data-stu-id="28cd1-121">Specify the location of the environment-specific project file by supplying a command-line parameter named **TargetEnvPropsFile**.</span></span>

<span data-ttu-id="28cd1-122">若要這樣做，MSBuild 命令應該會與以下相似：</span><span class="sxs-lookup"><span data-stu-id="28cd1-122">To do this, your MSBuild command should resemble this:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


<span data-ttu-id="28cd1-123">從這裡開始，它是簡單的步驟，以移至可重複、 單一步驟的部署。</span><span class="sxs-lookup"><span data-stu-id="28cd1-123">From here, it's a simple step to move to a repeatable, single-step deployment.</span></span> <span data-ttu-id="28cd1-124">您只需要為 MSBuild 命令加入至.cmd 檔案。</span><span class="sxs-lookup"><span data-stu-id="28cd1-124">All you need to do is to add your MSBuild command to a .cmd file.</span></span> <span data-ttu-id="28cd1-125">在連絡人管理員解決方案中，發行資料夾會包含名為*發行 Dev.cmd*會完全這。</span><span class="sxs-lookup"><span data-stu-id="28cd1-125">In the Contact Manager solution, the Publish folder includes a file named *Publish-Dev.cmd* that does exactly this.</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="28cd1-126">**/Fl**參數會指示 MSBuild 來建立名為記錄檔*msbuild.log* MSBuild.exe 已叫用的工作目錄中。</span><span class="sxs-lookup"><span data-stu-id="28cd1-126">The **/fl** switch instructs MSBuild to create a log file named *msbuild.log* in the working directory in which MSBuild.exe was invoked.</span></span>


<span data-ttu-id="28cd1-127">若要部署或重新部署方案，請連絡管理員，您只需要執行*發行 Dev.cmd*檔案。</span><span class="sxs-lookup"><span data-stu-id="28cd1-127">To deploy or redeploy the Contact Manager solution, all you need to do is run the *Publish-Dev.cmd* file.</span></span> <span data-ttu-id="28cd1-128">當您執行檔案時，則 MSBuild 將會：</span><span class="sxs-lookup"><span data-stu-id="28cd1-128">When you run the file, MSBuild will:</span></span>

- <span data-ttu-id="28cd1-129">建置方案中的所有專案。</span><span class="sxs-lookup"><span data-stu-id="28cd1-129">Build all the projects in the solution.</span></span>
- <span data-ttu-id="28cd1-130">產生可部署 web 封裝，web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="28cd1-130">Generate deployable web packages for the web application projects.</span></span>
- <span data-ttu-id="28cd1-131">產生.dbschema 和.deploymanifest 檔案的資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="28cd1-131">Generate .dbschema and .deploymanifest files for the database projects.</span></span>
- <span data-ttu-id="28cd1-132">將 web 套件部署至 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="28cd1-132">Deploy the web packages to the web server.</span></span>
- <span data-ttu-id="28cd1-133">將資料庫部署到資料庫伺服器。</span><span class="sxs-lookup"><span data-stu-id="28cd1-133">Deploy the database to the database server.</span></span>

## <a name="run-the-deployment"></a><span data-ttu-id="28cd1-134">執行部署</span><span class="sxs-lookup"><span data-stu-id="28cd1-134">Run the Deployment</span></span>

<span data-ttu-id="28cd1-135">當您已建立命令檔的目標環境時，您應該能夠只需執行該檔案以完成整個部署。</span><span class="sxs-lookup"><span data-stu-id="28cd1-135">When you've created a command file for your target environment, you should be able to complete the entire deployment by simply running the file.</span></span>

<span data-ttu-id="28cd1-136">**若要將連絡人管理員方案部署到您的測試環境**</span><span class="sxs-lookup"><span data-stu-id="28cd1-136">**To deploy the Contact Manager solution to your test environment**</span></span>

1. <span data-ttu-id="28cd1-137">在您開發人員工作站上，開啟 Windows 檔案總管，然後瀏覽至的位置*發行 Dev.cmd*檔案。</span><span class="sxs-lookup"><span data-stu-id="28cd1-137">On your developer workstation, open Windows Explorer, and then browse to the location of the *Publish-Dev.cmd* file.</span></span>
2. <span data-ttu-id="28cd1-138">按兩下該檔案來執行它。</span><span class="sxs-lookup"><span data-stu-id="28cd1-138">Double-click the file to run it.</span></span>
3. <span data-ttu-id="28cd1-139">如果**開啟檔案-安全性警告**對話方塊出現時，按一下**執行**。</span><span class="sxs-lookup"><span data-stu-id="28cd1-139">If an **Open File – Security Warning** dialog box appears, click **Run**.</span></span>
4. <span data-ttu-id="28cd1-140">如果將您的組態設定和測試伺服器設定正確，在命令提示字元視窗會顯示**建置成功**MSBuild 已完成處理的專案檔時，訊息。</span><span class="sxs-lookup"><span data-stu-id="28cd1-140">If your configuration settings and test servers are set up correctly, the Command Prompt window will show a **Build succeeded** message when MSBuild has finished processing the project files.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. <span data-ttu-id="28cd1-141">如果這是方案部署到此環境的第一次，您必須將測試 web 伺服器機器帳戶，加入**db\_datawriter**和**db\_datareader**角色**ContactManager**資料庫。</span><span class="sxs-lookup"><span data-stu-id="28cd1-141">If this is the first time you've deployed the solution to this environment, you'll need to add the test web server machine account to the **db\_datawriter** and **db\_datareader** roles on the **ContactManager** database.</span></span> <span data-ttu-id="28cd1-142">此程序所述[設定資料庫伺服器來進行 Web 部署發行](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md)。</span><span class="sxs-lookup"><span data-stu-id="28cd1-142">This procedure is described in [Configure a Database Server for Web Deploy Publishing](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="28cd1-143">您只需要建立資料庫時，指派這些權限。</span><span class="sxs-lookup"><span data-stu-id="28cd1-143">You only need to assign these permissions when you create the database.</span></span> <span data-ttu-id="28cd1-144">根據預設，在建置程序將無法重新建立每個部署上的資料庫&#x2014;相反地，它會比較現有的資料庫最新的結構描述並進行所需的變更。</span><span class="sxs-lookup"><span data-stu-id="28cd1-144">By default, the build process will not recreate the database on every deployment&#x2014;instead, it will compare the existing database to the latest schema and make only the changes required.</span></span> <span data-ttu-id="28cd1-145">如此一來，您應該只需要將這些資料庫角色對應您部署方案的第一次。</span><span class="sxs-lookup"><span data-stu-id="28cd1-145">As a result, you should only need to map these database roles the first time you deploy the solution.</span></span>
6. <span data-ttu-id="28cd1-146">開啟 Internet Explorer 並瀏覽至 連絡人管理員應用程式的 URL (例如， `http://testweb1:85/ContactManager/`)。</span><span class="sxs-lookup"><span data-stu-id="28cd1-146">Open Internet Explorer and browse to the URL of the Contact Manager application (for example, `http://testweb1:85/ContactManager/`).</span></span>
7. <span data-ttu-id="28cd1-147">確認應用程式可以正常運作，您可以新增連絡人。</span><span class="sxs-lookup"><span data-stu-id="28cd1-147">Verify that the application works as expected and you're able to add contacts.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="28cd1-148">結論</span><span class="sxs-lookup"><span data-stu-id="28cd1-148">Conclusion</span></span>

<span data-ttu-id="28cd1-149">建立包含 MSBuild 指示的命令檔可讓您建置與部署到特定目的地環境的多專案方案的快速簡便方式。</span><span class="sxs-lookup"><span data-stu-id="28cd1-149">Creating a command file containing your MSBuild instructions provides you with a quick and easy way of building and deploying a multi-project solution to a specific destination environment.</span></span> <span data-ttu-id="28cd1-150">如果您需要重複將方案部署至多個目的地環境，您可以建立多個指令檔。</span><span class="sxs-lookup"><span data-stu-id="28cd1-150">If you need to repeatedly deploy your solution to multiple destination environments, you can create multiple command files.</span></span> <span data-ttu-id="28cd1-151">在每個命令檔，MSBuild 命令會建立相同的通用專案檔中，但它會指定不同的環境特定專案檔。</span><span class="sxs-lookup"><span data-stu-id="28cd1-151">In each command file, the MSBuild command will build the same universal project file, but it will specify a different environment-specific project file.</span></span> <span data-ttu-id="28cd1-152">例如，要發行到開發人員或測試環境的命令檔可能包含此 MSBuild 命令：</span><span class="sxs-lookup"><span data-stu-id="28cd1-152">For example, a command file to publish to a developer or test environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


<span data-ttu-id="28cd1-153">若要發行到預備環境的命令檔可能包含此 MSBuild 命令：</span><span class="sxs-lookup"><span data-stu-id="28cd1-153">A command file to publish to a staging environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> <span data-ttu-id="28cd1-154">如需如何自訂伺服器環境的特定環境的專案檔案的指引，請參閱[目標環境設定部署屬性](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)。</span><span class="sxs-lookup"><span data-stu-id="28cd1-154">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


<span data-ttu-id="28cd1-155">您也可以自訂每個環境的建置程序來覆寫屬性，或在 MSBuild 命令中設定各種其他參數。</span><span class="sxs-lookup"><span data-stu-id="28cd1-155">You can also customize the build process for each environment by overriding properties or setting various other switches in your MSBuild command.</span></span> <span data-ttu-id="28cd1-156">如需詳細資訊，請參閱[MSBuild 命令列參考](https://msdn.microsoft.com/library/ms164311.aspx)。</span><span class="sxs-lookup"><span data-stu-id="28cd1-156">For more information, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="28cd1-157">[上一頁](deploying-database-projects.md)
> [下一頁](manually-installing-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="28cd1-157">[Previous](deploying-database-projects.md)
[Next](manually-installing-web-packages.md)</span></span>
