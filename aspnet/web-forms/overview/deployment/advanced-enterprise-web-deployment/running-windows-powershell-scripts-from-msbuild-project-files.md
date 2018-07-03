---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: 從 MSBuild 專案檔中執行 Windows PowerShell 指令碼 |Microsoft Docs
author: jrjlee
description: 本主題描述如何建置和部署程序的一部分執行的 Windows PowerShell 指令碼。 您可以在本機執行指令碼 (換句話說，在 b...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: ddb658d8a8f224a7c417321df3e17ce0610d2473
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362891"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a><span data-ttu-id="9377d-104">從 MSBuild 專案檔中執行 Windows PowerShell 指令碼</span><span class="sxs-lookup"><span data-stu-id="9377d-104">Running Windows PowerShell Scripts from MSBuild Project Files</span></span>
====================
<span data-ttu-id="9377d-105">藉由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="9377d-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="9377d-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="9377d-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="9377d-107">本主題描述如何建置和部署程序的一部分執行的 Windows PowerShell 指令碼。</span><span class="sxs-lookup"><span data-stu-id="9377d-107">This topic describes how to run a Windows PowerShell script as part of a build and deployment process.</span></span> <span data-ttu-id="9377d-108">您可以在本機的指令碼 （亦即，組建伺服器上執行），或遠端電腦上，例如在目的地 web 伺服器或資料庫伺服器上。</span><span class="sxs-lookup"><span data-stu-id="9377d-108">You can run a script locally (in other words, on the build server) or remotely, like on a destination web server or database server.</span></span>
> 
> <span data-ttu-id="9377d-109">有許多原因為何您可能想要執行部署後的 Windows PowerShell 指令碼。</span><span class="sxs-lookup"><span data-stu-id="9377d-109">There are lots of reasons why you might want to run a post-deployment Windows PowerShell script.</span></span> <span data-ttu-id="9377d-110">例如，您可能要：</span><span class="sxs-lookup"><span data-stu-id="9377d-110">For example, you might want to:</span></span>
> 
> - <span data-ttu-id="9377d-111">新增自訂的事件來源登錄。</span><span class="sxs-lookup"><span data-stu-id="9377d-111">Add a custom event source to the registry.</span></span>
> - <span data-ttu-id="9377d-112">產生上傳的檔案系統目錄。</span><span class="sxs-lookup"><span data-stu-id="9377d-112">Generate a file system directory for uploads.</span></span>
> - <span data-ttu-id="9377d-113">清除組建目錄。</span><span class="sxs-lookup"><span data-stu-id="9377d-113">Clean up build directories.</span></span>
> - <span data-ttu-id="9377d-114">寫入自訂記錄檔中的項目。</span><span class="sxs-lookup"><span data-stu-id="9377d-114">Write entries to a custom log file.</span></span>
> - <span data-ttu-id="9377d-115">傳送電子郵件邀請新佈建的 web 應用程式的使用者。</span><span class="sxs-lookup"><span data-stu-id="9377d-115">Send emails inviting users to a newly provisioned web application.</span></span>
> - <span data-ttu-id="9377d-116">建立使用者帳戶具有適當的權限。</span><span class="sxs-lookup"><span data-stu-id="9377d-116">Create user accounts with the appropriate permissions.</span></span>
> - <span data-ttu-id="9377d-117">設定 SQL Server 執行個體之間的複寫。</span><span class="sxs-lookup"><span data-stu-id="9377d-117">Configure replication between SQL Server instances.</span></span>
> 
> <span data-ttu-id="9377d-118">本主題將說明如何從 Microsoft Build Engine (MSBuild) 專案檔中的自訂目標本機和遠端執行 Windows PowerShell 指令碼。</span><span class="sxs-lookup"><span data-stu-id="9377d-118">This topic will show you how to run Windows PowerShell scripts both locally and remotely from a custom target in a Microsoft Build Engine (MSBuild) project file.</span></span>


<span data-ttu-id="9377d-119">本主題是構成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程的一部分本教學課程系列會使用範例解決方案&#x2014;[連絡管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="9377d-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="9377d-120">這些教學課程的核心的部署方法根據分割專案檔案方法中所述[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置流程控制的兩個專案檔&#x2014;包含建置適用於每個目的地環境中和包含環境特定建置和部署設定的指示。</span><span class="sxs-lookup"><span data-stu-id="9377d-120">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="9377d-121">在建置階段的特定環境的專案檔會合併到無從驗證環境的專案檔中，以構成一組完整的組建指示。</span><span class="sxs-lookup"><span data-stu-id="9377d-121">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="9377d-122">工作概觀</span><span class="sxs-lookup"><span data-stu-id="9377d-122">Task Overview</span></span>

<span data-ttu-id="9377d-123">若要執行的 Windows PowerShell 指令碼自動化或單一步驟的部署程序的一部分，您必須完成下列高階工作：</span><span class="sxs-lookup"><span data-stu-id="9377d-123">To run a Windows PowerShell script as part of an automated or single-step deployment process, you'll need to complete these high-level tasks:</span></span>

- <span data-ttu-id="9377d-124">您的方案和原始檔控制，請新增 Windows PowerShell 指令碼。</span><span class="sxs-lookup"><span data-stu-id="9377d-124">Add the Windows PowerShell script to your solution and to source control.</span></span>
- <span data-ttu-id="9377d-125">建立會叫用您的 Windows PowerShell 指令碼的命令。</span><span class="sxs-lookup"><span data-stu-id="9377d-125">Create a command that invokes your Windows PowerShell script.</span></span>
- <span data-ttu-id="9377d-126">逸出任何保留的 XML 字元，在您的命令。</span><span class="sxs-lookup"><span data-stu-id="9377d-126">Escape any reserved XML characters in your command.</span></span>
- <span data-ttu-id="9377d-127">建立自訂 MSBuild 專案檔中的目標，並使用**Exec**工作來執行您的命令。</span><span class="sxs-lookup"><span data-stu-id="9377d-127">Create a target in your custom MSBuild project file and use the **Exec** task to run your command.</span></span>

<span data-ttu-id="9377d-128">本主題將示範如何執行這些程序。</span><span class="sxs-lookup"><span data-stu-id="9377d-128">This topic will show you how to perform these procedures.</span></span> <span data-ttu-id="9377d-129">工作與本主題中的逐步解說假設您已經熟悉 MSBuild 目標和屬性，並了解如何使用自訂的 MSBuild 專案檔，以建置和部署程序。</span><span class="sxs-lookup"><span data-stu-id="9377d-129">The tasks and walkthroughs in this topic assume that you're already familiar with MSBuild targets and properties, and that you understand how to use a custom MSBuild project file to drive a build and deployment process.</span></span> <span data-ttu-id="9377d-130">如需詳細資訊，請參閱 <<c0> [ 了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)並[了解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。</span><span class="sxs-lookup"><span data-stu-id="9377d-130">For more information, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

## <a name="creating-and-adding-windows-powershell-scripts"></a><span data-ttu-id="9377d-131">建立並加入 Windows PowerShell 指令碼</span><span class="sxs-lookup"><span data-stu-id="9377d-131">Creating and Adding Windows PowerShell Scripts</span></span>

<span data-ttu-id="9377d-132">本主題中的工作會使用名為的 Windows PowerShell 指令碼範例**LogDeploy.ps1** ，說明如何從 MSBuild 執行指令碼。</span><span class="sxs-lookup"><span data-stu-id="9377d-132">The tasks in this topic use a sample Windows PowerShell script named **LogDeploy.ps1** to illustrate how to run scripts from MSBuild.</span></span> <span data-ttu-id="9377d-133">**LogDeploy.ps1**指令碼包含單一列項目寫入記錄檔的簡單函式：</span><span class="sxs-lookup"><span data-stu-id="9377d-133">The **LogDeploy.ps1** script contains a simple function that writes a single-line entry to a log file:</span></span>


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


<span data-ttu-id="9377d-134">**LogDeploy.ps1**指令碼接受兩個參數。</span><span class="sxs-lookup"><span data-stu-id="9377d-134">The **LogDeploy.ps1** script accepts two parameters.</span></span> <span data-ttu-id="9377d-135">第一個參數表示您要新增項目時，記錄檔的完整路徑和第二個參數表示您想要記錄的記錄檔中的部署目的地。</span><span class="sxs-lookup"><span data-stu-id="9377d-135">The first parameter represents the full path to the log file to which you want to add an entry, and the second parameter represents the deployment destination that you want to record in the log file.</span></span> <span data-ttu-id="9377d-136">當您執行指令碼時，它便會加入一條線，記錄檔，格式如下：</span><span class="sxs-lookup"><span data-stu-id="9377d-136">When you run the script, it adds a line to the log file in this format:</span></span>


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


<span data-ttu-id="9377d-137">若要讓**LogDeploy.ps1**指令碼可使用 MSBuild，您需要：</span><span class="sxs-lookup"><span data-stu-id="9377d-137">To make the **LogDeploy.ps1** script available to MSBuild, you need to:</span></span>

- <span data-ttu-id="9377d-138">加入原始檔控制中的指令碼。</span><span class="sxs-lookup"><span data-stu-id="9377d-138">Add the script to source control.</span></span>
- <span data-ttu-id="9377d-139">新增至您的方案，Visual Studio 2010 中的指令碼。</span><span class="sxs-lookup"><span data-stu-id="9377d-139">Add the script to your solution in Visual Studio 2010.</span></span>

<span data-ttu-id="9377d-140">您不需要部署具有您方案的內容，不論您是否計劃組建伺服器上或遠端電腦上執行指令碼的指令碼。</span><span class="sxs-lookup"><span data-stu-id="9377d-140">You don't need to deploy the script with your solution content, regardless of whether you plan to run the script on the build server or on a remote computer.</span></span> <span data-ttu-id="9377d-141">其中一個選項是將指令碼新增至方案資料夾。</span><span class="sxs-lookup"><span data-stu-id="9377d-141">One option is to add the script to a solution folder.</span></span> <span data-ttu-id="9377d-142">在連絡人管理員範例中，因為您想要使用 Windows PowerShell 指令碼部署程序的一部分，所以合理將指令碼新增至發佈方案資料夾。</span><span class="sxs-lookup"><span data-stu-id="9377d-142">In the Contact Manager example, because you want to use the Windows PowerShell script as part of the deployment process, it makes sense to add the script to the Publish solution folder.</span></span>

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

<span data-ttu-id="9377d-143">方案資料夾的內容會複製到組建伺服器作為來源資料。</span><span class="sxs-lookup"><span data-stu-id="9377d-143">The contents of solution folders are copied to build servers as source material.</span></span> <span data-ttu-id="9377d-144">不過，它們會形成任何專案輸出的任何部分。</span><span class="sxs-lookup"><span data-stu-id="9377d-144">However, they form no part of any project output.</span></span>

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a><span data-ttu-id="9377d-145">在組建伺服器上執行 Windows PowerShell 指令碼</span><span class="sxs-lookup"><span data-stu-id="9377d-145">Executing a Windows PowerShell Script on the Build Server</span></span>

<span data-ttu-id="9377d-146">在某些情況下，您可能想要建置您的專案的電腦上執行 Windows PowerShell 指令碼。</span><span class="sxs-lookup"><span data-stu-id="9377d-146">In some scenarios, you may want to run Windows PowerShell scripts on the computer that builds your projects.</span></span> <span data-ttu-id="9377d-147">比方說，您可能會使用 Windows PowerShell 指令碼來清除組建的資料夾或項目寫入自訂記錄檔。</span><span class="sxs-lookup"><span data-stu-id="9377d-147">For example, you might use a Windows PowerShell script to clean up build folders or write entries to a custom log file.</span></span>

<span data-ttu-id="9377d-148">語法，以從 MSBuild 專案檔中執行的 Windows PowerShell 指令碼是從一般的命令提示字元中執行的 Windows PowerShell 指令碼相同。</span><span class="sxs-lookup"><span data-stu-id="9377d-148">In terms of syntax, running a Windows PowerShell script from an MSBuild project file is the same as running a Windows PowerShell script from a regular command prompt.</span></span> <span data-ttu-id="9377d-149">您需要可執行 powershell.exe，並使用 **– 命令**交換器以提供您想要執行的 Windows PowerShell 命令。</span><span class="sxs-lookup"><span data-stu-id="9377d-149">You need to invoke the powershell.exe executable and use the **–command** switch to provide the commands you want Windows PowerShell to run.</span></span> <span data-ttu-id="9377d-150">(在 Windows PowerShell v2 中，您也可以使用 **– 檔案**切換)。</span><span class="sxs-lookup"><span data-stu-id="9377d-150">(In Windows PowerShell v2, you can also use the **–file** switch).</span></span> <span data-ttu-id="9377d-151">此命令應該採取這種格式：</span><span class="sxs-lookup"><span data-stu-id="9377d-151">The command should take this format:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


<span data-ttu-id="9377d-152">例如: </span><span class="sxs-lookup"><span data-stu-id="9377d-152">For example:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


<span data-ttu-id="9377d-153">如果您的指令碼的路徑包含空格，您需要加上連字號的單一引號括住的檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="9377d-153">If the path to your script includes spaces, you need to enclose the file path in single quotes preceded by an ampersand.</span></span> <span data-ttu-id="9377d-154">您無法使用雙引號括住，，因為您已經使用它們來括住的命令：</span><span class="sxs-lookup"><span data-stu-id="9377d-154">You can't use double quotes, because you've already used them to enclose the command:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


<span data-ttu-id="9377d-155">當您叫用此命令從 MSBuild 時，有一些其他考量。</span><span class="sxs-lookup"><span data-stu-id="9377d-155">There are a few additional considerations when you invoke this command from MSBuild.</span></span> <span data-ttu-id="9377d-156">首先，您應該在其中包含 **– NonInteractive**旗標，以確保以無訊息模式執行指令碼。</span><span class="sxs-lookup"><span data-stu-id="9377d-156">First, you should include the **–NonInteractive** flag to ensure that the script executes quietly.</span></span> <span data-ttu-id="9377d-157">接下來，您應該在其中包含 **-ExecutionPolicy**與適當的引數值的旗標。</span><span class="sxs-lookup"><span data-stu-id="9377d-157">Next, you should include the **–ExecutionPolicy** flag with an appropriate argument value.</span></span> <span data-ttu-id="9377d-158">這會指定 Windows PowerShell 會套用到您的指令碼，並可讓您覆寫預設的執行原則，可能會導致您的指令碼執行的執行原則。</span><span class="sxs-lookup"><span data-stu-id="9377d-158">This specifies the execution policy that Windows PowerShell will apply to your script and allows you to override the default execution policy, which may prevent execution of your script.</span></span> <span data-ttu-id="9377d-159">您可以選擇從這些引數的值：</span><span class="sxs-lookup"><span data-stu-id="9377d-159">You can choose from these argument values:</span></span>

- <span data-ttu-id="9377d-160">值為**Unrestricted**可讓 Windows PowerShell 中執行您的指令碼，不論是否簽署指令碼。</span><span class="sxs-lookup"><span data-stu-id="9377d-160">A value of **Unrestricted** will allow Windows PowerShell to execute your script, regardless of whether the script is signed.</span></span>
- <span data-ttu-id="9377d-161">值為**RemoteSigned**會允許執行未簽署的指令碼在本機電腦上所建立的 Windows PowerShell。</span><span class="sxs-lookup"><span data-stu-id="9377d-161">A value of **RemoteSigned** will allow Windows PowerShell to execute unsigned scripts that were created on the local machine.</span></span> <span data-ttu-id="9377d-162">不過，在其他地方所建立的指令碼必須經過簽署。</span><span class="sxs-lookup"><span data-stu-id="9377d-162">However, scripts that were created elsewhere must be signed.</span></span> <span data-ttu-id="9377d-163">（在實務上，您很難已在組建伺服器上本機建立的 Windows PowerShell 指令碼）。</span><span class="sxs-lookup"><span data-stu-id="9377d-163">(In practice, you're very unlikely to have created a Windows PowerShell script locally on a build server).</span></span>
- <span data-ttu-id="9377d-164">值為**AllSigned**可讓 Windows PowerShell 中執行簽署指令碼。</span><span class="sxs-lookup"><span data-stu-id="9377d-164">A value of **AllSigned** will allow Windows PowerShell to execute signed scripts only.</span></span>

<span data-ttu-id="9377d-165">是預設執行原則**Restricted**，這可防止 Windows PowerShell 執行任何指令碼檔案。</span><span class="sxs-lookup"><span data-stu-id="9377d-165">The default execution policy is **Restricted**, which prevents Windows PowerShell from running any script files.</span></span>

<span data-ttu-id="9377d-166">最後，您必須逸出任何保留的 XML 字元，就會發生在 Windows PowerShell 命令：</span><span class="sxs-lookup"><span data-stu-id="9377d-166">Finally, you need to escape any reserved XML characters that occur in your Windows PowerShell command:</span></span>

- <span data-ttu-id="9377d-167">取代單一引號**&amp;a p o s;**</span><span class="sxs-lookup"><span data-stu-id="9377d-167">Replace single quotes with **&amp;apos;**</span></span>
- <span data-ttu-id="9377d-168">取代以雙引號括住**&amp;q u o t;**</span><span class="sxs-lookup"><span data-stu-id="9377d-168">Replace double quotes with **&amp;quot;**</span></span>
- <span data-ttu-id="9377d-169">取代連字號與**&amp;amp;**</span><span class="sxs-lookup"><span data-stu-id="9377d-169">Replace ampersands with **&amp;amp;**</span></span>

- <span data-ttu-id="9377d-170">當您進行這些變更時，您的命令大致如下：</span><span class="sxs-lookup"><span data-stu-id="9377d-170">When you make these changes, your command will resemble this:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


<span data-ttu-id="9377d-171">在您自訂的 MSBuild 專案檔，您可以建立新的目標，並使用**Exec**工作來執行此命令：</span><span class="sxs-lookup"><span data-stu-id="9377d-171">Within your custom MSBuild project file, you can create a new target and use the **Exec** task to run this command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


<span data-ttu-id="9377d-172">在此範例中，請注意：</span><span class="sxs-lookup"><span data-stu-id="9377d-172">In this example, note that:</span></span>

- <span data-ttu-id="9377d-173">任何變數，例如參數值和 Windows PowerShell 可執行檔的位置會宣告為 MSBuild 屬性。</span><span class="sxs-lookup"><span data-stu-id="9377d-173">Any variables, like parameter values and the location of the Windows PowerShell executable, are declared as MSBuild properties.</span></span>
- <span data-ttu-id="9377d-174">若要讓使用者覆寫這些值，從命令列包含條件。</span><span class="sxs-lookup"><span data-stu-id="9377d-174">Conditions are included to enable users to override these values from the command line.</span></span>
- <span data-ttu-id="9377d-175">**MSDeployComputerName**專案檔中其他位置宣告屬性。</span><span class="sxs-lookup"><span data-stu-id="9377d-175">The **MSDeployComputerName** property is declared elsewhere in the project file.</span></span>

<span data-ttu-id="9377d-176">當您執行此目標，您的建置程序的一部分時，Windows PowerShell 會執行您的命令，並寫入您指定的檔案中的記錄項目。</span><span class="sxs-lookup"><span data-stu-id="9377d-176">When you execute this target as part of your build process, Windows PowerShell will run your command and write a log entry to the file you specified.</span></span>

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a><span data-ttu-id="9377d-177">在遠端電腦上執行 Windows PowerShell 指令碼</span><span class="sxs-lookup"><span data-stu-id="9377d-177">Executing a Windows PowerShell Script on a Remote Computer</span></span>

<span data-ttu-id="9377d-178">Windows PowerShell 是透過遠端電腦上執行指令碼[Windows 遠端管理](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx)(WinRM)。</span><span class="sxs-lookup"><span data-stu-id="9377d-178">Windows PowerShell is capable of running scripts on remote computers through [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span></span> <span data-ttu-id="9377d-179">若要這樣做，您必須使用[Invoke-command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet。</span><span class="sxs-lookup"><span data-stu-id="9377d-179">To do this, you need to use the [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet.</span></span> <span data-ttu-id="9377d-180">這可讓您執行您的指令碼，對一或多個遠端電腦，而不會將指令碼複製到遠端電腦。</span><span class="sxs-lookup"><span data-stu-id="9377d-180">This lets you execute your script against one or more remote computers without copying the script to the remote computers.</span></span> <span data-ttu-id="9377d-181">您執行指令碼的本機電腦，會傳回任何結果。</span><span class="sxs-lookup"><span data-stu-id="9377d-181">Any results are returned to the local computer from which you ran the script.</span></span>

> [!NOTE]
> <span data-ttu-id="9377d-182">在您使用之前**Invoke-command** cmdlet 來執行 Windows PowerShell 指令碼在遠端電腦上，您需要設定 WinRM 接聽程式，以接受遠端的訊息。</span><span class="sxs-lookup"><span data-stu-id="9377d-182">Before you use the **Invoke-Command** cmdlet to execute Windows PowerShell scripts on a remote computer, you need to configure a WinRM listener to accept remote messages.</span></span> <span data-ttu-id="9377d-183">您可以執行命令**winrm quickconfig**遠端電腦上。</span><span class="sxs-lookup"><span data-stu-id="9377d-183">You can do this by running the command **winrm quickconfig** on the remote computer.</span></span> <span data-ttu-id="9377d-184">如需詳細資訊，請參閱 <<c0> [ 安裝的 Windows 遠端管理與設定](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx)。</span><span class="sxs-lookup"><span data-stu-id="9377d-184">For more information, see [Installation and Configuration for Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span></span>


<span data-ttu-id="9377d-185">從 Windows PowerShell 視窗中，您會使用此語法來執行**LogDeploy.ps1**遠端電腦上的指令碼：</span><span class="sxs-lookup"><span data-stu-id="9377d-185">From a Windows PowerShell window, you'd use this syntax to run the **LogDeploy.ps1** script on a remote computer:</span></span>


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> <span data-ttu-id="9377d-186">使用的其他可以使用各種方式**Invoke-command**執行指令碼檔案，但這種方法是最直接當您需要提供參數值，並管理有空格的路徑。</span><span class="sxs-lookup"><span data-stu-id="9377d-186">There are various other ways of using **Invoke-Command** to run a script file, but this approach is the most straightforward when you need to provide parameter values and manage paths with spaces.</span></span>


<span data-ttu-id="9377d-187">當您執行命令提示字元中時，您需要 Windows PowerShell 可執行檔，並使用 **– 命令**參數，以提供您指示：</span><span class="sxs-lookup"><span data-stu-id="9377d-187">When you run this from a command prompt, you need to invoke the Windows PowerShell executable and use the **–command** parameter to provide your instructions:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


<span data-ttu-id="9377d-188">為之前，您需要提供一些額外的參數和逸出任何保留的 XML 字元，當您執行命令時從 MSBuild:</span><span class="sxs-lookup"><span data-stu-id="9377d-188">As before, you need to provide some additional switches and escape any reserved XML characters when you run the command from MSBuild:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


<span data-ttu-id="9377d-189">最後，同樣地，您可以使用**Exec**自訂 MSBuild 目標來執行您的命令中的工作：</span><span class="sxs-lookup"><span data-stu-id="9377d-189">Finally, as before, you can use the **Exec** task within a custom MSBuild target to execute your command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


<span data-ttu-id="9377d-190">當您執行此目標，您的建置程序的一部分時，Windows PowerShell 就會執行您的指令碼中所指定的電腦上 **– computername**引數。</span><span class="sxs-lookup"><span data-stu-id="9377d-190">When you execute this target as part of your build process, Windows PowerShell will run your script on the computer you specified in the **–computername** argument.</span></span>

## <a name="conclusion"></a><span data-ttu-id="9377d-191">結論</span><span class="sxs-lookup"><span data-stu-id="9377d-191">Conclusion</span></span>

<span data-ttu-id="9377d-192">本主題說明如何從 MSBuild 專案檔中執行的 Windows PowerShell 指令碼。</span><span class="sxs-lookup"><span data-stu-id="9377d-192">This topic described how to run a Windows PowerShell script from an MSBuild project file.</span></span> <span data-ttu-id="9377d-193">您可以使用這種方法，在本機或遠端電腦上執行 Windows PowerShell 指令碼，自動化或單一步驟建置和部署程序的一部分。</span><span class="sxs-lookup"><span data-stu-id="9377d-193">You can use this approach to run a Windows PowerShell script, either locally or on a remote computer, as part of an automated or single-step build and deployment process.</span></span>

## <a name="further-reading"></a><span data-ttu-id="9377d-194">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="9377d-194">Further Reading</span></span>

<span data-ttu-id="9377d-195">如需簽署 Windows PowerShell 指令碼和管理執行原則的指引，請參閱 <<c0> [ 執行的 Windows PowerShell 指令碼](https://technet.microsoft.com/library/ee176949.aspx)。</span><span class="sxs-lookup"><span data-stu-id="9377d-195">For guidance on signing Windows PowerShell scripts and managing execution policies, see [Running Windows PowerShell Scripts](https://technet.microsoft.com/library/ee176949.aspx).</span></span> <span data-ttu-id="9377d-196">如需從遠端電腦執行 Windows PowerShell 命令的指引，請參閱 <<c0> [ 執行遠端命令](https://technet.microsoft.com/library/dd819505.aspx)。</span><span class="sxs-lookup"><span data-stu-id="9377d-196">For guidance on running Windows PowerShell commands from a remote computer, see [Running Remote Commands](https://technet.microsoft.com/library/dd819505.aspx).</span></span>

<span data-ttu-id="9377d-197">如需有關如何使用自訂的 MSBuild 專案檔來控制部署程序的詳細資訊，請參閱 <<c0> [ 了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)並[了解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。</span><span class="sxs-lookup"><span data-stu-id="9377d-197">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9377d-198">[上一頁](taking-web-applications-offline-with-web-deploy.md)
> [下一頁](troubleshooting-the-packaging-process.md)</span><span class="sxs-lookup"><span data-stu-id="9377d-198">[Previous](taking-web-applications-offline-with-web-deploy.md)
[Next](troubleshooting-the-packaging-process.md)</span></span>
