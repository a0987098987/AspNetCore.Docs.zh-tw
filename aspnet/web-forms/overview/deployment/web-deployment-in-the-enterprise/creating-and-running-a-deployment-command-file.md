---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: 建立和執行部署命令檔案 |Microsoft Docs
author: jrjlee
description: 本主題說明如何建置可讓您執行單一步驟中使用 Microsoft Build Engine (MSBuild) 專案檔，或重新部署的命令檔...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: 121df7482f7cbd70b191e518bae791f0642ccc7c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833480"
---
<a name="creating-and-running-a-deployment-command-file"></a><span data-ttu-id="ba375-103">建立和執行部署命令檔</span><span class="sxs-lookup"><span data-stu-id="ba375-103">Creating and Running a Deployment Command File</span></span>
====================
<span data-ttu-id="ba375-104">藉由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="ba375-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="ba375-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="ba375-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="ba375-106">本主題描述如何建立命令檔，可讓您執行以單一步驟、 高再現性的處理程序中使用 Microsoft Build Engine (MSBuild) 專案檔的部署。</span><span class="sxs-lookup"><span data-stu-id="ba375-106">This topic describes how to build a command file that will let you run a deployment using Microsoft Build Engine (MSBuild) project files as a single-step, repeatable process.</span></span>


<span data-ttu-id="ba375-107">本主題是構成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程的一部分本系列教學課程使用範例解決方案&#x2014; [Contactmanager](the-contact-manager-solution.md)方案&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="ba375-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="ba375-108">這些教學課程的核心的部署方法根據分割專案檔案方法中所述[了解建置程序](understanding-the-build-process.md)，在建置流程控制的兩個專案檔&#x2014;包含建置適用於每個目的地環境中和包含環境特定建置和部署設定的指示。</span><span class="sxs-lookup"><span data-stu-id="ba375-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="ba375-109">在建置階段的特定環境的專案檔會合併到無從驗證環境的專案檔中，以構成一組完整的組建指示。</span><span class="sxs-lookup"><span data-stu-id="ba375-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="process-overview"></a><span data-ttu-id="ba375-110">處理程序概觀</span><span class="sxs-lookup"><span data-stu-id="ba375-110">Process Overview</span></span>

<span data-ttu-id="ba375-111">本主題中，您將了解如何建立和執行會使用這些專案檔案來執行可重複部署到您的目標環境的命令檔。</span><span class="sxs-lookup"><span data-stu-id="ba375-111">In this topic, you'll learn how to create and run a command file that uses these project files to perform a repeatable deployment to your target environment.</span></span> <span data-ttu-id="ba375-112">基本上，命令檔只需要包含 MSBuild 命令的：</span><span class="sxs-lookup"><span data-stu-id="ba375-112">Essentially, the command file simply needs to contain an MSBuild command that:</span></span>

- <span data-ttu-id="ba375-113">告知 MSBuild，以執行環境無關*Publish.proj*檔案。</span><span class="sxs-lookup"><span data-stu-id="ba375-113">Tells MSBuild to execute the environment-agnostic *Publish.proj* file.</span></span>
- <span data-ttu-id="ba375-114">會告訴*Publish.proj*哪些檔案包含環境特定專案設定和其位置的檔案。</span><span class="sxs-lookup"><span data-stu-id="ba375-114">Tells the *Publish.proj* file which file contains the environment-specific project settings and where to find it.</span></span>

## <a name="create-an-msbuild-command"></a><span data-ttu-id="ba375-115">建立一個 MSBuild 命令</span><span class="sxs-lookup"><span data-stu-id="ba375-115">Create an MSBuild Command</span></span>

<span data-ttu-id="ba375-116">中所述[了解建置程序](understanding-the-build-process.md)，環境特定專案檔&#x2014;，例如*Env Dev.proj*&#x2014;是要匯入環境無關*Publish.proj*在建置階段的檔案。</span><span class="sxs-lookup"><span data-stu-id="ba375-116">As described in [Understanding the Build Process](understanding-the-build-process.md), the environment-specific project file&#x2014;for example, *Env-Dev.proj*&#x2014;is designed to be imported into the environment-agnostic *Publish.proj* file at build time.</span></span> <span data-ttu-id="ba375-117">在一起，這兩個檔案會提供一組完整的指示，告知 MSBuild 如何建置和部署您的解決方案。</span><span class="sxs-lookup"><span data-stu-id="ba375-117">Together, these two files provide a complete set of instructions that tell MSBuild how to build and deploy your solution.</span></span>

<span data-ttu-id="ba375-118">*Publish.proj*檔案使用**匯入**環境特定的專案檔案匯入的項目。</span><span class="sxs-lookup"><span data-stu-id="ba375-118">The *Publish.proj* file uses an **Import** element to import the environment-specific project file.</span></span>


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


<span data-ttu-id="ba375-119">因此，當您使用 MSBuild.exe 來建置和部署連絡管理員解決方案時，您需要：</span><span class="sxs-lookup"><span data-stu-id="ba375-119">As such, when you use MSBuild.exe to build and deploy the Contact Manager solution, you need to:</span></span>

- <span data-ttu-id="ba375-120">在上執行 MSBuild.exe *Publish.proj*檔案。</span><span class="sxs-lookup"><span data-stu-id="ba375-120">Run MSBuild.exe on the *Publish.proj* file.</span></span>
- <span data-ttu-id="ba375-121">藉由提供一個名為的命令列參數來指定環境特有的專案檔的位置**TargetEnvPropsFile**。</span><span class="sxs-lookup"><span data-stu-id="ba375-121">Specify the location of the environment-specific project file by supplying a command-line parameter named **TargetEnvPropsFile**.</span></span>

<span data-ttu-id="ba375-122">若要這樣做，您的 MSBuild 命令看起來應該像這樣：</span><span class="sxs-lookup"><span data-stu-id="ba375-122">To do this, your MSBuild command should resemble this:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


<span data-ttu-id="ba375-123">從這裡開始，它是簡單的步驟，以移至可重複、 單一步驟的部署。</span><span class="sxs-lookup"><span data-stu-id="ba375-123">From here, it's a simple step to move to a repeatable, single-step deployment.</span></span> <span data-ttu-id="ba375-124">您只需要為.cmd 檔案中加入您的 MSBuild 命令。</span><span class="sxs-lookup"><span data-stu-id="ba375-124">All you need to do is to add your MSBuild command to a .cmd file.</span></span> <span data-ttu-id="ba375-125">在 連絡管理員解決方案，以發行資料夾包含名為的檔案*發佈 Dev.cmd* ，就是這麼。</span><span class="sxs-lookup"><span data-stu-id="ba375-125">In the Contact Manager solution, the Publish folder includes a file named *Publish-Dev.cmd* that does exactly this.</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="ba375-126">**/Fl**參數會指示 MSBuild 來建立名為記錄檔*msbuild.log* MSBuild.exe 已叫用的工作目錄中。</span><span class="sxs-lookup"><span data-stu-id="ba375-126">The **/fl** switch instructs MSBuild to create a log file named *msbuild.log* in the working directory in which MSBuild.exe was invoked.</span></span>


<span data-ttu-id="ba375-127">若要部署或重新部署連絡管理員解決方案，您只需要執行*發佈 Dev.cmd*檔案。</span><span class="sxs-lookup"><span data-stu-id="ba375-127">To deploy or redeploy the Contact Manager solution, all you need to do is run the *Publish-Dev.cmd* file.</span></span> <span data-ttu-id="ba375-128">當您執行該檔案時，MSBuild 將會：</span><span class="sxs-lookup"><span data-stu-id="ba375-128">When you run the file, MSBuild will:</span></span>

- <span data-ttu-id="ba375-129">建置方案中的所有專案。</span><span class="sxs-lookup"><span data-stu-id="ba375-129">Build all the projects in the solution.</span></span>
- <span data-ttu-id="ba375-130">產生可部署 web 封裝 web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="ba375-130">Generate deployable web packages for the web application projects.</span></span>
- <span data-ttu-id="ba375-131">產生.dbschema 和.deploymanifest 之資料庫專案檔案。</span><span class="sxs-lookup"><span data-stu-id="ba375-131">Generate .dbschema and .deploymanifest files for the database projects.</span></span>
- <span data-ttu-id="ba375-132">將 web 套件部署到 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="ba375-132">Deploy the web packages to the web server.</span></span>
- <span data-ttu-id="ba375-133">將資料庫部署到資料庫伺服器。</span><span class="sxs-lookup"><span data-stu-id="ba375-133">Deploy the database to the database server.</span></span>

## <a name="run-the-deployment"></a><span data-ttu-id="ba375-134">執行部署</span><span class="sxs-lookup"><span data-stu-id="ba375-134">Run the Deployment</span></span>

<span data-ttu-id="ba375-135">當您已為您的目標環境中建立命令檔時，您應該能夠完成整個部署，只要執行檔案。</span><span class="sxs-lookup"><span data-stu-id="ba375-135">When you've created a command file for your target environment, you should be able to complete the entire deployment by simply running the file.</span></span>

<span data-ttu-id="ba375-136">**若要將連絡管理員解決方案部署到您的測試環境**</span><span class="sxs-lookup"><span data-stu-id="ba375-136">**To deploy the Contact Manager solution to your test environment**</span></span>

1. <span data-ttu-id="ba375-137">在您開發人員工作站上，開啟 Windows 檔案總管，並再瀏覽至的位置*發佈 Dev.cmd*檔案。</span><span class="sxs-lookup"><span data-stu-id="ba375-137">On your developer workstation, open Windows Explorer, and then browse to the location of the *Publish-Dev.cmd* file.</span></span>
2. <span data-ttu-id="ba375-138">按兩下檔案加以執行。</span><span class="sxs-lookup"><span data-stu-id="ba375-138">Double-click the file to run it.</span></span>
3. <span data-ttu-id="ba375-139">如果**開啟檔案-安全性警告** 對話方塊出現時，按一下**執行**。</span><span class="sxs-lookup"><span data-stu-id="ba375-139">If an **Open File – Security Warning** dialog box appears, click **Run**.</span></span>
4. <span data-ttu-id="ba375-140">如果您的組態設定和測試伺服器都設定正確，[命令提示字元] 視窗會顯示**建置成功**MSBuild 已完成處理的專案檔時，訊息。</span><span class="sxs-lookup"><span data-stu-id="ba375-140">If your configuration settings and test servers are set up correctly, the Command Prompt window will show a **Build succeeded** message when MSBuild has finished processing the project files.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. <span data-ttu-id="ba375-141">如果這是您已對此環境中部署解決方案的第一次，您必須將測試 web 伺服器機器帳戶，加入**db\_資料寫入元**並**db\_datareader**上的角色**ContactManager**資料庫。</span><span class="sxs-lookup"><span data-stu-id="ba375-141">If this is the first time you've deployed the solution to this environment, you'll need to add the test web server machine account to the **db\_datawriter** and **db\_datareader** roles on the **ContactManager** database.</span></span> <span data-ttu-id="ba375-142">此程序所述[設定資料庫伺服器進行 Web 部署發佈](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md)。</span><span class="sxs-lookup"><span data-stu-id="ba375-142">This procedure is described in [Configure a Database Server for Web Deploy Publishing](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="ba375-143">您只需要建立資料庫時，指派這些權限。</span><span class="sxs-lookup"><span data-stu-id="ba375-143">You only need to assign these permissions when you create the database.</span></span> <span data-ttu-id="ba375-144">根據預設，建置程序將不會重新建立每個部署上的資料庫&#x2014;相反地，它會在 比較最新的結構描述現有的資料庫，並進行所需的變更。</span><span class="sxs-lookup"><span data-stu-id="ba375-144">By default, the build process will not recreate the database on every deployment&#x2014;instead, it will compare the existing database to the latest schema and make only the changes required.</span></span> <span data-ttu-id="ba375-145">如此一來，您應該只需要對應這些資料庫角色的第一次部署解決方案。</span><span class="sxs-lookup"><span data-stu-id="ba375-145">As a result, you should only need to map these database roles the first time you deploy the solution.</span></span>
6. <span data-ttu-id="ba375-146">開啟 Internet Explorer 並瀏覽至 Contact Manager 應用程式的 URL (例如`http://testweb1:85/ContactManager/`)。</span><span class="sxs-lookup"><span data-stu-id="ba375-146">Open Internet Explorer and browse to the URL of the Contact Manager application (for example, `http://testweb1:85/ContactManager/`).</span></span>
7. <span data-ttu-id="ba375-147">確認應用程式可以正常運作，您就可以將加入連絡人。</span><span class="sxs-lookup"><span data-stu-id="ba375-147">Verify that the application works as expected and you're able to add contacts.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="ba375-148">結論</span><span class="sxs-lookup"><span data-stu-id="ba375-148">Conclusion</span></span>

<span data-ttu-id="ba375-149">建立命令檔，其中包含 MSBuild 指示提供您建置和部署到特定目的地環境的多專案方案的快速簡便方式。</span><span class="sxs-lookup"><span data-stu-id="ba375-149">Creating a command file containing your MSBuild instructions provides you with a quick and easy way of building and deploying a multi-project solution to a specific destination environment.</span></span> <span data-ttu-id="ba375-150">如果您要重複多個目的地環境中部署您的解決方案，您可以建立多個的命令檔。</span><span class="sxs-lookup"><span data-stu-id="ba375-150">If you need to repeatedly deploy your solution to multiple destination environments, you can create multiple command files.</span></span> <span data-ttu-id="ba375-151">在每個命令檔，MSBuild 命令會建立相同的通用專案檔中，但它會指定不同的環境特定專案的檔案。</span><span class="sxs-lookup"><span data-stu-id="ba375-151">In each command file, the MSBuild command will build the same universal project file, but it will specify a different environment-specific project file.</span></span> <span data-ttu-id="ba375-152">例如，發佈給開發人員或測試環境的命令檔可能包含此 MSBuild 命令：</span><span class="sxs-lookup"><span data-stu-id="ba375-152">For example, a command file to publish to a developer or test environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


<span data-ttu-id="ba375-153">若要發佈至預備環境的命令檔可能包含此 MSBuild 命令：</span><span class="sxs-lookup"><span data-stu-id="ba375-153">A command file to publish to a staging environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> <span data-ttu-id="ba375-154">如需如何自訂您自己的伺服器環境的特定環境的專案檔的指引，請參閱 <<c0> [ 設定的目標環境的部署屬性](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md)。</span><span class="sxs-lookup"><span data-stu-id="ba375-154">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


<span data-ttu-id="ba375-155">您也可以自訂每個環境的建置程序，藉由覆寫屬性，或設定在 MSBuild 命令的各種其他參數。</span><span class="sxs-lookup"><span data-stu-id="ba375-155">You can also customize the build process for each environment by overriding properties or setting various other switches in your MSBuild command.</span></span> <span data-ttu-id="ba375-156">如需詳細資訊，請參閱 < [MSBuild 命令列參考](https://msdn.microsoft.com/library/ms164311.aspx)。</span><span class="sxs-lookup"><span data-stu-id="ba375-156">For more information, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ba375-157">[上一頁](deploying-database-projects.md)
> [下一頁](manually-installing-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="ba375-157">[Previous](deploying-database-projects.md)
[Next](manually-installing-web-packages.md)</span></span>
