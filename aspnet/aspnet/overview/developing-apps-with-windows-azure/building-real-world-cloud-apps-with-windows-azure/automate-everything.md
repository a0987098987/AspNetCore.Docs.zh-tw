---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: 自動化所有項目 （建置使用 Azure 的真實世界的雲端應用程式） |Microsoft Docs
author: MikeWasson
description: 建置真實世界雲端應用程式與 Azure 的電子書是以 Scott Guthrie 所開發的簡報為依據。 它說明 13 模式與做法，他可以...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: 45d3d72454852217303050d17b678c4a5710dcb5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376779"
---
<a name="automate-everything-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="ef247-104">自動化 （建置使用 Azure 的真實世界的雲端應用程式） 的所有項目</span><span class="sxs-lookup"><span data-stu-id="ef247-104">Automate Everything (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="ef247-105">藉由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="ef247-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="ef247-106">[下載修正此問題的專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="ef247-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="ef247-107">**建置真實世界雲端應用程式與 Azure**電子書以 Scott Guthrie 所開發的簡報為依據。</span><span class="sxs-lookup"><span data-stu-id="ef247-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="ef247-108">它說明 13 的模式，並可協助您的作法是成功開發適用於雲端的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="ef247-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="ef247-109">如需簡介電子書，請參閱[第 1 章](introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="ef247-109">For an introduction to the e-book, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="ef247-110">我們將探討的前三個模式實際上會套用至任何軟體開發專案中，但特別是雲端專案。</span><span class="sxs-lookup"><span data-stu-id="ef247-110">The first three patterns we'll look at actually apply to any software development project, but especially to cloud projects.</span></span> <span data-ttu-id="ef247-111">此模式是關於開發工作自動化。</span><span class="sxs-lookup"><span data-stu-id="ef247-111">This pattern is about automating development tasks.</span></span> <span data-ttu-id="ef247-112">它是很重要的主題，因為手動程序是既緩慢又容易發生錯誤;多少個為可能有助於設定快速、 可靠且敏捷式軟體開發工作流程自動化。</span><span class="sxs-lookup"><span data-stu-id="ef247-112">It's an important topic because manual processes are slow and error-prone; automating as many of them as possible helps set up a fast, reliable, and agile workflow.</span></span> <span data-ttu-id="ef247-113">它是唯一重要的雲端開發的因為您可以輕鬆自動化許多難以或無法在內部部署環境中自動化的工作。</span><span class="sxs-lookup"><span data-stu-id="ef247-113">It's uniquely important for cloud development because you can easily automate many tasks that are difficult or impossible to automate in an on-premises environment.</span></span> <span data-ttu-id="ef247-114">例如，您可以設定整個測試的環境，包括新的 web 伺服器和後端 Vm、 資料庫、 blob 儲存體 （檔案）、 佇列等。</span><span class="sxs-lookup"><span data-stu-id="ef247-114">For example, you can set up whole test environments including new web server and back-end VMs, databases, blob storage (file storage), queues, etc.</span></span>

## <a name="devops-workflow"></a><span data-ttu-id="ef247-115">DevOps 工作流程</span><span class="sxs-lookup"><span data-stu-id="ef247-115">DevOps Workflow</span></span>

<span data-ttu-id="ef247-116">您將會越來越聽到詞彙 「 DevOps 」。</span><span class="sxs-lookup"><span data-stu-id="ef247-116">Increasingly you hear the term "DevOps."</span></span> <span data-ttu-id="ef247-117">從辨識，您必須將開發和作業的工作整合以有效率地開發軟體，開發一詞。</span><span class="sxs-lookup"><span data-stu-id="ef247-117">The term developed out of a recognition that you have to integrate development and operations tasks in order to develop software efficiently.</span></span> <span data-ttu-id="ef247-118">您想要啟用的工作流程的類型是指在其中您可以開發應用程式、 將它部署、 從生產環境的使用方式的了解、 變更以回應您的學習成果，和重複循環，快速且可靠地。</span><span class="sxs-lookup"><span data-stu-id="ef247-118">The kind of workflow you want to enable is one in which you can develop an app, deploy it, learn from production usage of it, change it in response to what you've learned, and repeat the cycle quickly and reliably.</span></span>

<span data-ttu-id="ef247-119">某些成功的雲端開發團隊會部署一天多次至即時環境。</span><span class="sxs-lookup"><span data-stu-id="ef247-119">Some successful cloud development teams deploy multiple times a day to a live environment.</span></span> <span data-ttu-id="ef247-120">用來部署主要 Azure 團隊更新每隔 2-3 月，但目前版本的次要更新每個 2-3 天和主要版本每隔 2 到 3 週。</span><span class="sxs-lookup"><span data-stu-id="ef247-120">The Azure team used to deploy a major update every 2-3 months, but now it releases minor updates every 2-3 days and major releases every 2-3 weeks.</span></span> <span data-ttu-id="ef247-121">放入該韻律非常有幫助您客戶的意見反應做出回應。</span><span class="sxs-lookup"><span data-stu-id="ef247-121">Getting into that cadence really helps you be responsive to customer feedback.</span></span>

<span data-ttu-id="ef247-122">若要這樣做，您必須啟用開發和部署的週期可重複、 可靠、 可預測的且具有低的週期時間。</span><span class="sxs-lookup"><span data-stu-id="ef247-122">In order to do that, you have to enable a development and deployment cycle that is repeatable, reliable, predictable, and has low cycle time.</span></span>

![DevOps 工作流程](automate-everything/_static/image1.png)

<span data-ttu-id="ef247-124">換句話說，如果您已了解功能和當客戶使用它，並提供意見反應之間的時間期間必須越短越好。</span><span class="sxs-lookup"><span data-stu-id="ef247-124">In other words, the period of time between when you have an idea for a feature and when the customers are using it and providing feedback must be as short as possible.</span></span> <span data-ttu-id="ef247-125">前三個模式-自動化原始檔控制的一切，而且持續整合與傳遞，我們建議您若要讓這種程序的最佳作法的所有相關資訊。</span><span class="sxs-lookup"><span data-stu-id="ef247-125">The first three patterns – automate everything, source control, and continuous integration and delivery -- are all about best practices that we recommend in order to enable that kind of process.</span></span>

## <a name="azure-management-scripts"></a><span data-ttu-id="ef247-126">Azure 管理指令碼</span><span class="sxs-lookup"><span data-stu-id="ef247-126">Azure management scripts</span></span>

<span data-ttu-id="ef247-127">在 [簡介本電子書](introduction.md)，您看到網頁型主控台，在 Azure 管理入口網站。</span><span class="sxs-lookup"><span data-stu-id="ef247-127">In the [introduction to this e-book](introduction.md), you saw the web-based console, the Azure Management Portal.</span></span> <span data-ttu-id="ef247-128">在管理入口網站可讓您監視和管理所有您已在 Azure 部署的資源。</span><span class="sxs-lookup"><span data-stu-id="ef247-128">The management portal enables you to monitor and manage all of the resources that you have deployed on Azure.</span></span> <span data-ttu-id="ef247-129">它是簡單的方法，來建立和刪除服務，例如 web 應用程式和 Vm，設定這些服務、 監視服務作業，以及進行其他操作。</span><span class="sxs-lookup"><span data-stu-id="ef247-129">It's an easy way to create and delete services such as web apps and VMs, configure those services, monitor service operation, and so forth.</span></span> <span data-ttu-id="ef247-130">很棒的工具，但使用它是手動程序。</span><span class="sxs-lookup"><span data-stu-id="ef247-130">It's a great tool, but using it is a manual process.</span></span> <span data-ttu-id="ef247-131">如果您要開發生產應用程式的任何大小，而且特別是在小組環境中，我們建議您瀏覽入口網站以了解並探索 Azure 中的 UI，然後再自動執行您要重複執行的處理程序。</span><span class="sxs-lookup"><span data-stu-id="ef247-131">If you're going to develop a production application of any size, and especially in a team environment, we recommend that you go through the portal UI in order to learn and explore Azure, and then automate the processes that you'll be doing repetitively.</span></span>

<span data-ttu-id="ef247-132">幾乎所有項目，您可以手動在管理入口網站或從 Visual Studio 也可以藉由呼叫 REST 管理 API。</span><span class="sxs-lookup"><span data-stu-id="ef247-132">Nearly everything that you can do manually in the management portal or from Visual Studio can also be done by calling the REST management API.</span></span> <span data-ttu-id="ef247-133">您可以撰寫使用指令碼[Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx)，或您可以使用開放原始碼架構，例如[Chef](http://www.opscode.com/chef/)或是[Puppet](http://puppetlabs.com/puppet/what-is-puppet)。</span><span class="sxs-lookup"><span data-stu-id="ef247-133">You can write scripts using [Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx), or you can use an open source framework such as [Chef](http://www.opscode.com/chef/) or [Puppet](http://puppetlabs.com/puppet/what-is-puppet).</span></span> <span data-ttu-id="ef247-134">您也可以在 Mac 或 Linux 環境中使用 Bash 命令列工具。</span><span class="sxs-lookup"><span data-stu-id="ef247-134">You can also use the Bash command-line tool in a Mac or Linux environment.</span></span> <span data-ttu-id="ef247-135">Azure 可讓您有指令碼 Api 針對所有這些不同的環境，而且它有[.NET 管理 API](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx)萬一您想要撰寫程式碼，而不是指令碼。</span><span class="sxs-lookup"><span data-stu-id="ef247-135">Azure has scripting APIs for all those different environments, and it has a [.NET management API](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) in case you want to write code instead of script.</span></span>

<span data-ttu-id="ef247-136">修正其應用程式中，我們已建立自動化的處理程序建立測試環境，並將專案部署到該環境中，一些 Windows PowerShell 指令碼，我們會檢閱一些這些指令碼的內容。</span><span class="sxs-lookup"><span data-stu-id="ef247-136">For the Fix It app we've created some Windows PowerShell scripts that automate the processes of creating a test environment and deploying the project to that environment, and we'll review some of the contents of those scripts.</span></span>

## <a name="environment-creation-script"></a><span data-ttu-id="ef247-137">環境建立指令碼</span><span class="sxs-lookup"><span data-stu-id="ef247-137">Environment creation script</span></span>

<span data-ttu-id="ef247-138">我們將探討第一個指令碼名為*新增 AzureWebsiteEnv.ps1*。</span><span class="sxs-lookup"><span data-stu-id="ef247-138">The first script we'll look at is named *New-AzureWebsiteEnv.ps1*.</span></span> <span data-ttu-id="ef247-139">它會建立在 Azure 環境，您可以部署此修正其應用程式進行測試。</span><span class="sxs-lookup"><span data-stu-id="ef247-139">It creates an Azure environment that you can deploy the Fix It app to for testing.</span></span> <span data-ttu-id="ef247-140">此指令碼執行的主要工作如下所示：</span><span class="sxs-lookup"><span data-stu-id="ef247-140">The main tasks that this script performs are the following:</span></span>

- <span data-ttu-id="ef247-141">建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="ef247-141">Create a web app.</span></span>
- <span data-ttu-id="ef247-142">建立儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="ef247-142">Create a storage account.</span></span> <span data-ttu-id="ef247-143">（需具備適用於 blob 和佇列，如您所見在後續章節）。</span><span class="sxs-lookup"><span data-stu-id="ef247-143">(Required for blobs and queues, as you'll see in later chapters.)</span></span>
- <span data-ttu-id="ef247-144">建立 SQL Database 伺服器和兩個資料庫： 應用程式資料庫，以及成員資格資料庫。</span><span class="sxs-lookup"><span data-stu-id="ef247-144">Create a SQL Database server and two databases: an application database, and a membership database.</span></span>
- <span data-ttu-id="ef247-145">設定儲存在 Azure 應用程式將用來存取儲存體帳戶和資料庫。</span><span class="sxs-lookup"><span data-stu-id="ef247-145">Store settings in Azure that the app will use to access the storage account and databases.</span></span>
- <span data-ttu-id="ef247-146">建立將用來自動化部署的設定檔。</span><span class="sxs-lookup"><span data-stu-id="ef247-146">Create settings files that will be used to automate deployment.</span></span>

### <a name="run-the-script"></a><span data-ttu-id="ef247-147">執行指令碼</span><span class="sxs-lookup"><span data-stu-id="ef247-147">Run the script</span></span>


> [!NOTE]
> <span data-ttu-id="ef247-148">本章的這個部分中顯示的指令碼和您輸入才能執行這些命令的範例。</span><span class="sxs-lookup"><span data-stu-id="ef247-148">This part of the chapter shows examples of scripts and the commands that you enter in order to run them.</span></span> <span data-ttu-id="ef247-149">此示範並不會提供您需要知道，才能執行指令碼的所有項目。</span><span class="sxs-lookup"><span data-stu-id="ef247-149">This a demo and doesn't provide everything you need to know in order to run the scripts.</span></span> <span data-ttu-id="ef247-150">逐步作法-要-執行-it 指示，請參閱[附錄： 修正它的範例應用程式](the-fix-it-sample-application.md#deploybase)。</span><span class="sxs-lookup"><span data-stu-id="ef247-150">For step-by-step how-to-do-it instructions, see [Appendix: The Fix It Sample Application](the-fix-it-sample-application.md#deploybase).</span></span>


<span data-ttu-id="ef247-151">若要執行 PowerShell 指令碼管理 Azure 服務，您必須安裝 Azure PowerShell 主控台，然後將它設定為使用您 Azure 訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="ef247-151">To run a PowerShell script that manages Azure services you have to install the Azure PowerShell console and configure it to work with your Azure subscription.</span></span> <span data-ttu-id="ef247-152">一旦設定好，您可以使用與下列類似的命令來執行修正它的環境建立指令碼：</span><span class="sxs-lookup"><span data-stu-id="ef247-152">Once you're set up, you can run the Fix It environment creation script with a command like this one:</span></span>

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

<span data-ttu-id="ef247-153">`Name`參數會指定要建立資料庫和儲存體帳戶時使用的名稱和`SqlDatabasePassword`參數會指定將建立 SQL Database 的系統管理員帳戶的密碼。</span><span class="sxs-lookup"><span data-stu-id="ef247-153">The `Name` parameter specifies the name to be used when creating the database and storage accounts, and the `SqlDatabasePassword` parameter specifies the password for the admin account that will be created for SQL Database.</span></span> <span data-ttu-id="ef247-154">還有，我們將探討稍後，您可以使用其他參數。</span><span class="sxs-lookup"><span data-stu-id="ef247-154">There are other parameters you can use that we'll look at later.</span></span>

![PowerShell 視窗](automate-everything/_static/image2.png)

<span data-ttu-id="ef247-156">指令碼完成之後您可以看到管理入口網站中建立的項目。</span><span class="sxs-lookup"><span data-stu-id="ef247-156">After the script finishes you can see in the management portal what was created.</span></span> <span data-ttu-id="ef247-157">您會發現兩個資料庫：</span><span class="sxs-lookup"><span data-stu-id="ef247-157">You'll find two databases:</span></span>

![資料庫](automate-everything/_static/image3.png)

<span data-ttu-id="ef247-159">儲存體帳戶：</span><span class="sxs-lookup"><span data-stu-id="ef247-159">A storage account:</span></span>

![儲存體帳戶](automate-everything/_static/image4.png)

<span data-ttu-id="ef247-161">和 web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="ef247-161">And a web app:</span></span>

![網站](automate-everything/_static/image5.png)

<span data-ttu-id="ef247-163">在 [**設定**] 索引標籤上的 web 應用程式中，您可以看到它有沒有儲存體帳戶設定，SQL 資料庫連接字串設定此修正其應用程式。</span><span class="sxs-lookup"><span data-stu-id="ef247-163">On the **Configure** tab for the web app, you can see that it has the storage account settings and SQL database connection strings set up for the Fix It app.</span></span>

![appSettings 和 connectionStrings](automate-everything/_static/image6.png)

<span data-ttu-id="ef247-165">*自動化*資料夾現在也包含 *&lt;websitename&gt;.pubxml*檔案。</span><span class="sxs-lookup"><span data-stu-id="ef247-165">The *Automation* folder now also contains a *&lt;websitename&gt;.pubxml* file.</span></span> <span data-ttu-id="ef247-166">這個檔案會儲存 MSBuild 用來部署應用程式以剛建立的 Azure 環境的設定。</span><span class="sxs-lookup"><span data-stu-id="ef247-166">This file stores settings that MSBuild will use to deploy the application to the Azure environment that was just created.</span></span> <span data-ttu-id="ef247-167">例如: </span><span class="sxs-lookup"><span data-stu-id="ef247-167">For example:</span></span>

[!code-xml[Main](automate-everything/samples/sample1.xml)]

<span data-ttu-id="ef247-168">如您所見，指令碼已建立完整的測試環境，並在 90 秒內完成整個程序。</span><span class="sxs-lookup"><span data-stu-id="ef247-168">As you can see, the script has created a complete test environment, and the whole process is done in about 90 seconds.</span></span>

<span data-ttu-id="ef247-169">如果您的小組的其他人想要建立測試環境，可以只執行指令碼。</span><span class="sxs-lookup"><span data-stu-id="ef247-169">If someone else on your team wants to create a test environment, they can just run the script.</span></span> <span data-ttu-id="ef247-170">不只是速度快，但是也可自信，它們都使用相同於所使用的環境。</span><span class="sxs-lookup"><span data-stu-id="ef247-170">Not only is it fast, but also they can be confident that they are using an environment identical to the one you're using.</span></span> <span data-ttu-id="ef247-171">您無法為信心，如果每個人都已進行設定以手動方式使用管理入口網站 UI 相當。</span><span class="sxs-lookup"><span data-stu-id="ef247-171">You couldn't be quite as confident of that if everyone was setting things up manually by using the management portal UI.</span></span>

### <a name="a-look-at-the-scripts"></a><span data-ttu-id="ef247-172">了解指令碼</span><span class="sxs-lookup"><span data-stu-id="ef247-172">A look at the scripts</span></span>

<span data-ttu-id="ef247-173">有實際三個的指令碼，進行這項工作。</span><span class="sxs-lookup"><span data-stu-id="ef247-173">There are actually three scripts that do this work.</span></span> <span data-ttu-id="ef247-174">您呼叫其中一個從命令列，並自動使用其他兩個來執行部分工作：</span><span class="sxs-lookup"><span data-stu-id="ef247-174">You call one from the command line and it automatically uses the other two to do some of the tasks:</span></span>

- <span data-ttu-id="ef247-175">*新 AzureWebSiteEnv.ps1*是主要的指令碼。</span><span class="sxs-lookup"><span data-stu-id="ef247-175">*New-AzureWebSiteEnv.ps1* is the main script.</span></span>

    - <span data-ttu-id="ef247-176">*新 AzureStorage.ps1*建立儲存體帳戶。</span><span class="sxs-lookup"><span data-stu-id="ef247-176">*New-AzureStorage.ps1* creates the storage account.</span></span>
    - <span data-ttu-id="ef247-177">*新 AzureSql.ps1*會建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="ef247-177">*New-AzureSql.ps1* creates the databases.</span></span>

### <a name="parameters-in-the-main-script"></a><span data-ttu-id="ef247-178">主要的指令碼中的參數</span><span class="sxs-lookup"><span data-stu-id="ef247-178">Parameters in the main script</span></span>

<span data-ttu-id="ef247-179">主要的指令碼中，*新增 AzureWebSiteEnv.ps1*，定義數個參數：</span><span class="sxs-lookup"><span data-stu-id="ef247-179">The main script, *New-AzureWebSiteEnv.ps1*, defines several parameters:</span></span>

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

<span data-ttu-id="ef247-180">兩個參數都是必要項目：</span><span class="sxs-lookup"><span data-stu-id="ef247-180">Two parameters are required:</span></span>

- <span data-ttu-id="ef247-181">指令碼會建立 web 應用程式的名稱。</span><span class="sxs-lookup"><span data-stu-id="ef247-181">The name of the web app that the script creates.</span></span> <span data-ttu-id="ef247-182">(這也會用於 URL: `<name>.azurewebsites.net`。)</span><span class="sxs-lookup"><span data-stu-id="ef247-182">(This is also used for the URL: `<name>.azurewebsites.net`.)</span></span>
- <span data-ttu-id="ef247-183">指令碼會建立資料庫伺服器的新系統管理員密碼。</span><span class="sxs-lookup"><span data-stu-id="ef247-183">The password for the new administrative user of the database server that the script creates.</span></span>

<span data-ttu-id="ef247-184">選擇性參數可讓您指定的資料中心位置 （預設為"West US"）、 資料庫伺服器管理員名稱 （預設為"dbuser 」），以及資料庫伺服器的防火牆規則。</span><span class="sxs-lookup"><span data-stu-id="ef247-184">Optional parameters enable you to specify the data center location (defaults to "West US"), database server administrator name (defaults to "dbuser"), and a firewall rule for the database server.</span></span>

### <a name="create-the-web-app"></a><span data-ttu-id="ef247-185">建立 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="ef247-185">Create the web app</span></span>

<span data-ttu-id="ef247-186">指令碼會執行第一件事是建立 web 應用程式藉由呼叫`New-AzureWebsite`cmdlet，將傳遞給它的 web 應用程式名稱和位置的參數值：</span><span class="sxs-lookup"><span data-stu-id="ef247-186">The first thing the script does is create the web app by calling the `New-AzureWebsite` cmdlet, passing in to it the web app name and location parameter values:</span></span>

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a><span data-ttu-id="ef247-187">建立儲存體帳戶</span><span class="sxs-lookup"><span data-stu-id="ef247-187">Create the storage account</span></span>

<span data-ttu-id="ef247-188">然後會執行主要指令碼<em>新增 AzureStorage.ps1</em>指令碼，請指定"<em>&lt;websitename&gt;</em>儲存體 」 儲存體帳戶名稱，和相同的資料中心位置web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="ef247-188">Then the main script runs the <em>New-AzureStorage.ps1</em> script, specifying "<em>&lt;websitename&gt;</em>storage" for the storage account name, and the same data center location as the web app.</span></span>

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

<span data-ttu-id="ef247-189">*新 AzureStorage.ps1*呼叫`New-AzureStorageAccount`cmdlet 來建立儲存體帳戶中，它會傳回帳戶名稱] 和 [存取金鑰的值。</span><span class="sxs-lookup"><span data-stu-id="ef247-189">*New-AzureStorage.ps1* calls the `New-AzureStorageAccount` cmdlet to create the storage account, and it returns the account name and access key values.</span></span> <span data-ttu-id="ef247-190">若要存取的 blob 和佇列儲存體帳戶中的，應用程式將會需要這些值。</span><span class="sxs-lookup"><span data-stu-id="ef247-190">The application will need these values in order to access the blobs and queues in the storage account.</span></span>

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

<span data-ttu-id="ef247-191">您可能不一定要建立新的儲存體帳戶;您可以加入的參數，選擇性指示以使用現有的儲存體帳戶，以加強指令碼。</span><span class="sxs-lookup"><span data-stu-id="ef247-191">You might not always want to create a new storage account; you could enhance the script by adding a parameter that optionally directs it to use an existing storage account.</span></span>

### <a name="create-the-databases"></a><span data-ttu-id="ef247-192">建立資料庫</span><span class="sxs-lookup"><span data-stu-id="ef247-192">Create the databases</span></span>

<span data-ttu-id="ef247-193">主要的指令碼再執行資料庫建立指令碼中，*新增 AzureSql.ps1*、 之後設定預設資料庫和防火牆規則名稱：</span><span class="sxs-lookup"><span data-stu-id="ef247-193">The main script then runs the database creation script, *New-AzureSql.ps1*, after setting up default database and firewall rule names:</span></span>

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

<span data-ttu-id="ef247-194">資料庫建立指令碼擷取開發電腦的 IP 位址，並設定防火牆規則，因此可以連線到開發電腦，並將其管理伺服器。</span><span class="sxs-lookup"><span data-stu-id="ef247-194">The database creation script retrieves the dev machine's IP address and sets a firewall rule so the dev machine can connect to and manage the server.</span></span> <span data-ttu-id="ef247-195">資料庫建立指令碼再經歷數個步驟來設定資料庫：</span><span class="sxs-lookup"><span data-stu-id="ef247-195">The database creation script then goes through several steps to set up the databases:</span></span>

- <span data-ttu-id="ef247-196">使用建立伺服器`New-AzureSqlDatabaseServer`cmdlet。</span><span class="sxs-lookup"><span data-stu-id="ef247-196">Creates the server by using the `New-AzureSqlDatabaseServer` cmdlet.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- <span data-ttu-id="ef247-197">建立防火牆規則，以讓開發人員機器來管理伺服器，以及啟用 web 應用程式連接到它。</span><span class="sxs-lookup"><span data-stu-id="ef247-197">Creates firewall rules to enable the dev machine to manage the server and to enable the web app to connect to it.</span></span> 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- <span data-ttu-id="ef247-198">建立資料庫內容，其中包含伺服器名稱和認證，使用`New-AzureSqlDatabaseServerContext`cmdlet。</span><span class="sxs-lookup"><span data-stu-id="ef247-198">Creates a database context that includes the server name and credentials, by using the `New-AzureSqlDatabaseServerContext` cmdlet.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    <span data-ttu-id="ef247-199">`New-PSCredentialFromPlainText` 是在指令碼中呼叫的函式`ConvertTo-SecureString`cmdlet 來加密密碼並傳回`PSCredential`物件時，相同的類型， `Get-Credential` cmdlet 會傳回。</span><span class="sxs-lookup"><span data-stu-id="ef247-199">`New-PSCredentialFromPlainText` is a function in the script that calls the `ConvertTo-SecureString` cmdlet to encrypt the password and returns a `PSCredential` object, the same type that the `Get-Credential` cmdlet returns.</span></span>
- <span data-ttu-id="ef247-200">藉由建立應用程式資料庫和成員資格資料庫`New-AzureSqlDatabase`cmdlet。</span><span class="sxs-lookup"><span data-stu-id="ef247-200">Creates the application database and the membership database by using the `New-AzureSqlDatabase` cmdlet.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- <span data-ttu-id="ef247-201">呼叫每個資料庫的本機定義的函式 tocreates 連接字串。</span><span class="sxs-lookup"><span data-stu-id="ef247-201">Calls a locally defined function tocreates a connection string for each database.</span></span> <span data-ttu-id="ef247-202">應用程式會使用這些連接字串來存取資料庫。</span><span class="sxs-lookup"><span data-stu-id="ef247-202">The application will use these connection strings to access the databases.</span></span> 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    <span data-ttu-id="ef247-203">取得 SQLAzureDatabaseConnectionString 是從提供給它的參數值中建立的連接字串的指令碼中定義的函式。</span><span class="sxs-lookup"><span data-stu-id="ef247-203">Get-SQLAzureDatabaseConnectionString is a function defined in the script that creates the connection string from the parameter values supplied to it.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- <span data-ttu-id="ef247-204">傳回與資料庫伺服器名稱和連接字串的雜湊資料表。</span><span class="sxs-lookup"><span data-stu-id="ef247-204">Returns a hash table with the database server name and the connection strings.</span></span>

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

<span data-ttu-id="ef247-205">修正其應用程式使用個別的成員資格和應用程式資料庫。</span><span class="sxs-lookup"><span data-stu-id="ef247-205">The Fix It app uses separate membership and application databases.</span></span> <span data-ttu-id="ef247-206">它也可將成員資格和應用程式資料放入單一資料庫。</span><span class="sxs-lookup"><span data-stu-id="ef247-206">It's also possible to put both membership and application data in a single database.</span></span>

### <a name="store-app-settings-and-connection-strings"></a><span data-ttu-id="ef247-207">市集應用程式設定和連接字串</span><span class="sxs-lookup"><span data-stu-id="ef247-207">Store app settings and connection strings</span></span>

<span data-ttu-id="ef247-208">Azure 有一項功能，可讓您儲存設定和自動覆寫它嘗試讀取時傳回給應用程式的連接字串`appSettings`或`connectionStrings`Web.config 檔案中的集合。</span><span class="sxs-lookup"><span data-stu-id="ef247-208">Azure has a feature that enables you to store settings and connection strings that automatically override what is returned to the application when it tries to read the `appSettings` or `connectionStrings` collections in the Web.config file.</span></span> <span data-ttu-id="ef247-209">這是要套用的替代[Web.config 轉換](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md)當您部署。</span><span class="sxs-lookup"><span data-stu-id="ef247-209">This is an alternative to applying [Web.config transformations](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) when you deploy.</span></span> <span data-ttu-id="ef247-210">如需詳細資訊，請參閱 <<c0> [ 在 Azure 中儲存敏感性資料](source-control.md#appsettings)稍後本電子書。</span><span class="sxs-lookup"><span data-stu-id="ef247-210">For more information, see [Store sensitive data in Azure](source-control.md#appsettings) later in this e-book.</span></span>

<span data-ttu-id="ef247-211">環境建立指令碼會將儲存在 Azure 中的所有`appSettings`和`connectionStrings`應用程式需要存取儲存體帳戶和資料庫，在 Azure 中執行時的值。</span><span class="sxs-lookup"><span data-stu-id="ef247-211">The environment creation script stores in Azure all of the `appSettings` and `connectionStrings` values that the application needs to access the storage account and databases when it runs in Azure.</span></span>

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

<span data-ttu-id="ef247-212">[New Relic](http://newrelic.com/)是我們示範在遙測架構[監視和遙測](monitoring-and-telemetry.md)一章。</span><span class="sxs-lookup"><span data-stu-id="ef247-212">[New Relic](http://newrelic.com/) is a telemetry framework that we demonstrate in the [Monitoring and Telemetry](monitoring-and-telemetry.md) chapter.</span></span> <span data-ttu-id="ef247-213">環境建立指令碼也會重新啟動 web 應用程式，以確定其挑選的 New Relic 設定。</span><span class="sxs-lookup"><span data-stu-id="ef247-213">The environment creation script also restarts the web app to make sure that it picks up the New Relic settings.</span></span>

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a><span data-ttu-id="ef247-214">準備部署</span><span class="sxs-lookup"><span data-stu-id="ef247-214">Preparing for deployment</span></span>

<span data-ttu-id="ef247-215">在此程序結束時，環境的建立指令碼會呼叫兩個函式來建立將用於部署指令碼的檔案。</span><span class="sxs-lookup"><span data-stu-id="ef247-215">At the end of the process, the environment creation script calls two functions to create files that will be used by the deployment script.</span></span>

<span data-ttu-id="ef247-216">其中一個函式會建立發行設定檔 *(&lt;websitename&gt;.pubxml*檔案)。</span><span class="sxs-lookup"><span data-stu-id="ef247-216">One of these functions creates a publish profile *(&lt;websitename&gt;.pubxml* file).</span></span> <span data-ttu-id="ef247-217">程式碼會呼叫 Azure REST API，以取得發佈設定，並將儲存中的資訊 *.publishsettings*檔案。</span><span class="sxs-lookup"><span data-stu-id="ef247-217">The code calls the Azure REST API to get the publish settings, and it saves the information in a *.publishsettings* file.</span></span> <span data-ttu-id="ef247-218">然後它會使用從該檔案，以及範本檔案的資訊 (*pubxml.template*) 來建立 *.pubxml*所屬的發行設定檔的檔案。</span><span class="sxs-lookup"><span data-stu-id="ef247-218">Then it uses the information from that file along with a template file (*pubxml.template*) to create the *.pubxml* file that contains the publish profile.</span></span> <span data-ttu-id="ef247-219">此兩步驟程序會模擬您在 Visual Studio 中如何： 下載 *.publishsettings*檔案，並匯入，若要建立發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="ef247-219">This two-step process simulates what you do in Visual Studio: download a *.publishsettings* file and import that to create a publish profile.</span></span>

<span data-ttu-id="ef247-220">其他函式使用另一個範本檔 (網站 environment.template) 來建立*網站 environment.xml*檔案，其中包含的設定部署指令碼將使用連同 *.pubxml*檔案。</span><span class="sxs-lookup"><span data-stu-id="ef247-220">The other function uses another template file (website-environment.template) to create a *website-environment.xml* file that contains settings the deployment script will use along with the *.pubxml* file.</span></span>

### <a name="troubleshooting-and-error-handling"></a><span data-ttu-id="ef247-221">疑難排解和錯誤處理</span><span class="sxs-lookup"><span data-stu-id="ef247-221">Troubleshooting and error handling</span></span>

<span data-ttu-id="ef247-222">指令碼就像是程式︰ 它們可能會失敗，以及這樣做時，您想要知道儘可能瞭解錯誤與造成它的原因。</span><span class="sxs-lookup"><span data-stu-id="ef247-222">Scripts are like programs: they can fail, and when they do you want to know as much as you can about the failure and what caused it.</span></span> <span data-ttu-id="ef247-223">基於這個理由，環境的建立指令碼的值變更`VerbosePreference`從變數`SilentlyContinue`到`Continue`以便顯示所有的詳細資訊訊息。</span><span class="sxs-lookup"><span data-stu-id="ef247-223">For this reason, the environment creation script changes the value of the `VerbosePreference` variable from `SilentlyContinue` to `Continue` so that all verbose messages are displayed.</span></span> <span data-ttu-id="ef247-224">它也會變更的值`ErrorActionPreference`從變數`Continue`到`Stop`，如此一來，即使當它遇到非終止錯誤的指令碼停止：</span><span class="sxs-lookup"><span data-stu-id="ef247-224">It also changes the value of the `ErrorActionPreference` variable from `Continue` to `Stop`, so that the script stops even when it encounters non-terminating errors:</span></span>

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

<span data-ttu-id="ef247-225">它沒有任何工作之前，指令碼，讓它可以計算經過的時間，完成時，就會儲存開始時間：</span><span class="sxs-lookup"><span data-stu-id="ef247-225">Before it does any work, the script stores the start time so that it can calculate the elapsed time when it's done:</span></span>

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

<span data-ttu-id="ef247-226">完成其工作之後，指令碼會顯示已耗用時間：</span><span class="sxs-lookup"><span data-stu-id="ef247-226">After it completes its work, the script displays the elapsed time:</span></span>

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

<span data-ttu-id="ef247-227">並針對每個金鑰的作業指令碼寫入詳細資訊訊息，例如：</span><span class="sxs-lookup"><span data-stu-id="ef247-227">And for every key operation the script writes verbose messages, for example:</span></span>

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a><span data-ttu-id="ef247-228">部署指令碼</span><span class="sxs-lookup"><span data-stu-id="ef247-228">Deployment script</span></span>

<span data-ttu-id="ef247-229">什麼*新增 AzureWebsiteEnv.ps1*環境建立的指令碼會執行*發佈 AzureWebsite.ps1*指令碼會執行應用程式部署。</span><span class="sxs-lookup"><span data-stu-id="ef247-229">What the *New-AzureWebsiteEnv.ps1* script does for environment creation, the *Publish-AzureWebsite.ps1* script does for application deployment.</span></span>

<span data-ttu-id="ef247-230">部署指令碼會取得從 web 應用程式名稱*網站 environment.xml*環境建立指令碼所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="ef247-230">The deployment script gets the name of the web app from the *website-environment.xml* file created by the environment creation script.</span></span>

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

<span data-ttu-id="ef247-231">它會取得部署的使用者密碼，從 *.publishsettings*檔案：</span><span class="sxs-lookup"><span data-stu-id="ef247-231">It gets the deployment user password from the *.publishsettings* file:</span></span>

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

<span data-ttu-id="ef247-232">它會執行[MSBuild](http://msbuildbook.com/)建置和部署專案的命令：</span><span class="sxs-lookup"><span data-stu-id="ef247-232">It executes the [MSBuild](http://msbuildbook.com/) command that builds and deploys the project:</span></span>

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

<span data-ttu-id="ef247-233">如果您已指定`Launch`參數在命令列上的，它會呼叫`Show-AzureWebsite`cmdlet 來開啟預設瀏覽器的網站 url。</span><span class="sxs-lookup"><span data-stu-id="ef247-233">And if you've specified the `Launch` parameter on the command line, it calls the `Show-AzureWebsite` cmdlet to open your default browser to the website URL.</span></span>

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

<span data-ttu-id="ef247-234">您可以使用與下列類似的命令來執行部署指令碼：</span><span class="sxs-lookup"><span data-stu-id="ef247-234">You can run the deployment script with a command like this one:</span></span>

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

<span data-ttu-id="ef247-235">完成時，瀏覽器會開啟與在雲端中執行的站台和`<websitename>.azurewebsites.net`URL。</span><span class="sxs-lookup"><span data-stu-id="ef247-235">And when it's done, the browser opens with the site running in the cloud at the `<websitename>.azurewebsites.net` URL.</span></span>

![修正應用程式部署至 Windows Azure](automate-everything/_static/image7.png)

## <a name="summary"></a><span data-ttu-id="ef247-237">總結</span><span class="sxs-lookup"><span data-stu-id="ef247-237">Summary</span></span>

<span data-ttu-id="ef247-238">這些指令碼與您可以確信一律會使用相同選項的相同順序執行相同的步驟。</span><span class="sxs-lookup"><span data-stu-id="ef247-238">With these scripts you can be confident that the same steps will always be executed in the same order using the same options.</span></span> <span data-ttu-id="ef247-239">這有助於確保小組的每位開發人員不會遺漏某些項目或弄亂的項目或部署在他自己實際上不會在另一個小組成員的環境或生產環境中運作方式相同的電腦上的自訂內容。</span><span class="sxs-lookup"><span data-stu-id="ef247-239">This helps ensure that each developer on the team doesn't miss something or mess something up or deploy something custom on his own machine that won't actually work the same way in another team member's environment or in production.</span></span>

<span data-ttu-id="ef247-240">類似的方式，您可以將大部分的 Azure 的管理功能，您可以在管理入口網站中，使用 REST API、 Windows PowerShell 指令碼、.NET 語言 API 或您可以在 Linux 或 mac 執行的 Bash 公用程式自動化</span><span class="sxs-lookup"><span data-stu-id="ef247-240">In a similar way, you can automate most Azure management functions that you can do in the management portal, by using the REST API, Windows PowerShell scripts, a .NET language API, or a Bash utility that you can run on Linux or Mac.</span></span>

<span data-ttu-id="ef247-241">在 [[下一步] 一章](source-control.md)我們將查看原始程式碼，並說明為何一定要在您的原始程式碼儲存機制中包含您的指令碼。</span><span class="sxs-lookup"><span data-stu-id="ef247-241">In the [next chapter](source-control.md) we'll look at source code and explain why it's important to include your scripts in your source code repository.</span></span>

## <a name="resources"></a><span data-ttu-id="ef247-242">資源</span><span class="sxs-lookup"><span data-stu-id="ef247-242">Resources</span></span>

- <span data-ttu-id="ef247-243">[安裝和設定適用於 Azure 的 Windows PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1)。</span><span class="sxs-lookup"><span data-stu-id="ef247-243">[Install and Configure Windows PowerShell for Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).</span></span> <span data-ttu-id="ef247-244">說明如何安裝 Azure PowerShell cmdlet，以及如何安裝憑證，您需要在您的電腦，以管理您的 Azure 帳戶。</span><span class="sxs-lookup"><span data-stu-id="ef247-244">Explains how to install the Azure PowerShell cmdlets and how to install the certificate that you need on your computer in order to manage your Azure account.</span></span> <span data-ttu-id="ef247-245">這是因為它也有連結，了解 PowerShell 本身的資源開始著手的好地方。</span><span class="sxs-lookup"><span data-stu-id="ef247-245">This is a great place to get started because it also has links to resources for learning PowerShell itself.</span></span>
- <span data-ttu-id="ef247-246">[Azure 指令碼中心](https://docs.microsoft.com/azure/automation/automation-runbook-gallery)。</span><span class="sxs-lookup"><span data-stu-id="ef247-246">[Azure Script Center](https://docs.microsoft.com/azure/automation/automation-runbook-gallery).</span></span> <span data-ttu-id="ef247-247">WindowsAzure.com 開發指令碼管理 Azure 服務，取得教學課程中，cmdlet 參考文件來源的程式碼和範例指令碼的連結資源的入口網站</span><span class="sxs-lookup"><span data-stu-id="ef247-247">WindowsAzure.com portal to resources for developing scripts that manage Azure services, with links to getting started tutorials, cmdlet reference documentation and source code, and sample scripts</span></span>
- <span data-ttu-id="ef247-248">[週末 Scripter： 開始使用 Azure 和 PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx)。</span><span class="sxs-lookup"><span data-stu-id="ef247-248">[Weekend Scripter: Getting Started with Azure and PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx).</span></span> <span data-ttu-id="ef247-249">部落格專用的 Windows powershell 時，在這篇文章會提供使用 PowerShell for Azure 的管理功能的絕佳簡介。</span><span class="sxs-lookup"><span data-stu-id="ef247-249">In a blog dedicated to Windows PowerShell, this post provides a great introduction to using PowerShell for Azure management functions.</span></span>
- <span data-ttu-id="ef247-250">[安裝和設定 Azure 跨平台命令列介面](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)。</span><span class="sxs-lookup"><span data-stu-id="ef247-250">[Install and Configure the Azure Cross-Platform Command-Line Interface](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).</span></span> <span data-ttu-id="ef247-251">快速入門教學課程適用於 Mac 和 Linux，以及 Windows 系統的 Azure 指令碼架構。</span><span class="sxs-lookup"><span data-stu-id="ef247-251">Getting-started tutorial for an Azure scripting framework that works on Mac and Linux as well as Windows systems.</span></span>
- <span data-ttu-id="ef247-252">[命令列工具區段下載 Azure Sdk 和工具的主題](https://azure.microsoft.com/downloads/)。</span><span class="sxs-lookup"><span data-stu-id="ef247-252">[Command-line tools section of the Download Azure SDKs and Tools topic](https://azure.microsoft.com/downloads/).</span></span> <span data-ttu-id="ef247-253">入口網站的文件和 Azure 命令列工具與相關的下載頁面。</span><span class="sxs-lookup"><span data-stu-id="ef247-253">Portal page for documentation and downloads related to command-line tools for Azure.</span></span>
- <span data-ttu-id="ef247-254">[將一切自動化使用 Azure 管理程式庫和.NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx)。</span><span class="sxs-lookup"><span data-stu-id="ef247-254">[Automating everything with the Azure Management Libraries and .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx).</span></span> <span data-ttu-id="ef247-255">Scott Hanselman 介紹 Azure 中的使用.NET 管理 API。</span><span class="sxs-lookup"><span data-stu-id="ef247-255">Scott Hanselman introduces the .NET management API for Azure.</span></span>
- <span data-ttu-id="ef247-256">[使用 Windows PowerShell 指令碼來發行至開發和測試環境](https://msdn.microsoft.com/library/azure/dn642480.aspx)。</span><span class="sxs-lookup"><span data-stu-id="ef247-256">[Using Windows PowerShell Scripts to Publish to Dev and Test Environments](https://msdn.microsoft.com/library/azure/dn642480.aspx).</span></span> <span data-ttu-id="ef247-257">MSDN 文件說明如何使用發佈 Visual Studio 會自動產生 web 專案的指令碼。</span><span class="sxs-lookup"><span data-stu-id="ef247-257">MSDN documentation that explains how to use publish scripts that Visual Studio automatically generates for web projects.</span></span>
- <span data-ttu-id="ef247-258">[PowerShell Tools for Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597)。</span><span class="sxs-lookup"><span data-stu-id="ef247-258">[PowerShell Tools for Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597).</span></span> <span data-ttu-id="ef247-259">Visual Studio 擴充功能在 Visual Studio 中新增適用於 Windows PowerShell 語言支援。</span><span class="sxs-lookup"><span data-stu-id="ef247-259">Visual Studio extension that adds language support for Windows PowerShell in Visual Studio.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ef247-260">[上一頁](introduction.md)
> [下一頁](source-control.md)</span><span class="sxs-lookup"><span data-stu-id="ef247-260">[Previous](introduction.md)
[Next](source-control.md)</span></span>
