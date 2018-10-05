---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: 原始檔控制 （使用 Azure 建置真實世界的雲端應用程式） |Microsoft Docs
author: MikeWasson
description: 建置真實世界雲端應用程式與 Azure 的電子書是以 Scott Guthrie 所開發的簡報為依據。 它說明 13 模式與做法，他可以...
ms.author: riande
ms.date: 06/23/2015
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: 5df863762523b62759bb4f7849ca2635e5241b0a
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577791"
---
<a name="source-control-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="0b730-104">原始檔控制 （使用 Azure 建置真實世界的雲端應用程式）</span><span class="sxs-lookup"><span data-stu-id="0b730-104">Source Control (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="0b730-105">藉由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson]((https://twitter.com/RickAndMSFT))， [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="0b730-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="0b730-106">[下載修正此問題的專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="0b730-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="0b730-107">**建置真實世界雲端應用程式與 Azure**電子書以 Scott Guthrie 所開發的簡報為依據。</span><span class="sxs-lookup"><span data-stu-id="0b730-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="0b730-108">它說明 13 的模式，並可協助您的作法是成功開發適用於雲端的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0b730-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="0b730-109">電子書的相關資訊，請參閱[第 1 章](introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="0b730-109">For information about the e-book, see [the first chapter](introduction.md).</span></span>

<span data-ttu-id="0b730-110">原始檔控制是不可或缺的所有雲端開發專案，不只是小組的環境。</span><span class="sxs-lookup"><span data-stu-id="0b730-110">Source control is essential for all cloud development projects, not just team environments.</span></span> <span data-ttu-id="0b730-111">您不會將編輯原始程式碼，或甚至不需要復原函式和自動備份和原始檔控制的 Word 文件可讓您在專案層級，他們可以在其中儲存更多的時間，當發生錯誤的這些函式。</span><span class="sxs-lookup"><span data-stu-id="0b730-111">You wouldn't think of editing source code or even a Word document without an undo function and automatic backups, and source control gives you those functions at a project level where they can save even more time when something goes wrong.</span></span> <span data-ttu-id="0b730-112">與雲端原始檔控制服務，您再也不必擔心複雜的設定，而且您可以使用免費的 Azure 儲存機制原始檔控制最多 5 位使用者。</span><span class="sxs-lookup"><span data-stu-id="0b730-112">With cloud source control services, you no longer have to worry about complicated set-up, and you can use Azure Repos source control free for up to 5 users.</span></span>

<span data-ttu-id="0b730-113">本章的第一個部分會說明三個索引鍵的最佳作法，要牢記在心：</span><span class="sxs-lookup"><span data-stu-id="0b730-113">The first part of this chapter explains three key best practices to keep in mind:</span></span>

- <span data-ttu-id="0b730-114">[自動化指令碼視為原始程式碼](#scripts)和版本它們與您的應用程式程式碼。</span><span class="sxs-lookup"><span data-stu-id="0b730-114">[Treat automation scripts as source code](#scripts) and version them together with your application code.</span></span>
- <span data-ttu-id="0b730-115">[在 密碼永遠不檢查](#secrets)（敏感性資料，例如認證） 到原始程式碼存放庫。</span><span class="sxs-lookup"><span data-stu-id="0b730-115">[Never check in secrets](#secrets) (sensitive data such as credentials) into a source code repository.</span></span>
- <span data-ttu-id="0b730-116">[設定來源分支](#devops)啟用 DevOps 工作流程。</span><span class="sxs-lookup"><span data-stu-id="0b730-116">[Set up source branches](#devops) to enable the DevOps workflow.</span></span>

<span data-ttu-id="0b730-117">本章的其餘部分提供 Visual Studio、 Azure 和 Azure 儲存機制中的這些模式的實作一些範例：</span><span class="sxs-lookup"><span data-stu-id="0b730-117">The remainder of the chapter gives some sample implementations of these patterns in Visual Studio, Azure, and Azure Repos:</span></span>

- [<span data-ttu-id="0b730-118">在 Visual Studio 中的原始檔控制中加入指令碼</span><span class="sxs-lookup"><span data-stu-id="0b730-118">Add scripts to source control in Visual Studio</span></span>](#vsscripts)
- [<span data-ttu-id="0b730-119">在 Azure 中儲存敏感性資料</span><span class="sxs-lookup"><span data-stu-id="0b730-119">Store sensitive data in Azure</span></span>](#appsettings)
- [<span data-ttu-id="0b730-120">使用 Visual Studio 和 Azure 儲存機制中的 Git</span><span class="sxs-lookup"><span data-stu-id="0b730-120">Use Git in Visual Studio and Azure Repos</span></span>](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a><span data-ttu-id="0b730-121">自動化指令碼視為原始程式碼</span><span class="sxs-lookup"><span data-stu-id="0b730-121">Treat automation scripts as source code</span></span>

<span data-ttu-id="0b730-122">當您使用雲端專案上您要經常變更項目，且想要能夠快速地回應您的客戶所回報的問題。</span><span class="sxs-lookup"><span data-stu-id="0b730-122">When you're working on a cloud project you're changing things frequently and you want to be able to react quickly to issues reported by your customers.</span></span> <span data-ttu-id="0b730-123">快速回應牽涉到使用自動化指令碼中所述[自動執行的所有項目](automate-everything.md)一章。</span><span class="sxs-lookup"><span data-stu-id="0b730-123">Responding quickly involves using automation scripts, as explained in the [Automate Everything](automate-everything.md) chapter.</span></span> <span data-ttu-id="0b730-124">所有您用來建立您的環境中，指令碼部署給它，小數位數等等，必須與您的應用程式來源程式碼保持同步。</span><span class="sxs-lookup"><span data-stu-id="0b730-124">All of the scripts that you use to create your environment, deploy to it, scale it, etc., need to be in sync with your application source code.</span></span>

<span data-ttu-id="0b730-125">若要讓指令碼與程式碼保持同步，請將它們儲存在您的原始檔控制系統中。</span><span class="sxs-lookup"><span data-stu-id="0b730-125">To keep scripts in sync with code, store them in your source control system.</span></span> <span data-ttu-id="0b730-126">然後如果您需要回復變更，或快速檢修，不同於開發的程式碼的實際執行程式碼中，您不需要浪費時間嘗試追蹤的設定已變更，或哪些小組成員擁有您所需要的版本的複本。</span><span class="sxs-lookup"><span data-stu-id="0b730-126">Then if you ever need to roll back changes or make a quick fix to production code which is different from development code, you don't have to waste time trying to track down which settings have changed or which team members have copies of the version you need.</span></span> <span data-ttu-id="0b730-127">您可以保證，您需要的指令碼會在與同步程式碼基底，您需要它們，也可以保證所有小組成員會都使用相同的指令碼。</span><span class="sxs-lookup"><span data-stu-id="0b730-127">You're assured that the scripts you need are in sync with the code base that you need them for, and you're assured that all team members are working with the same scripts.</span></span> <span data-ttu-id="0b730-128">無論您需要將自動化測試和部署至生產環境或新的功能開發了 hot fix，也會有正確的指令碼需要更新的程式碼。</span><span class="sxs-lookup"><span data-stu-id="0b730-128">Then whether you need to automate testing and deployment of a hot fix to production or new feature development, you'll have the right script for the code that needs to be updated.</span></span>

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a><span data-ttu-id="0b730-129">未簽入祕密</span><span class="sxs-lookup"><span data-stu-id="0b730-129">Don't check in secrets</span></span>

<span data-ttu-id="0b730-130">它是適當安全的地方，如密碼等機密資料的太多人通常存取原始程式碼存放庫。</span><span class="sxs-lookup"><span data-stu-id="0b730-130">A source code repository is typically accessible to too many people for it to be an appropriately secure place for sensitive data such as passwords.</span></span> <span data-ttu-id="0b730-131">如果指令碼依賴密碼等機密資料時，會參數化為這些設定，讓它們不會儲存在原始程式碼中，並儲存您的祕密的任一處。</span><span class="sxs-lookup"><span data-stu-id="0b730-131">If scripts rely on secrets such as passwords, parameterize those settings so that they don't get saved in source code, and store your secrets somewhere else.</span></span>

<span data-ttu-id="0b730-132">比方說，Azure 可讓您下載包含的檔案會發行設定，才能自動建立發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="0b730-132">For example, Azure lets you download files that contain publish settings in order to automate the creation of publish profiles.</span></span> <span data-ttu-id="0b730-133">這些檔案包括使用者名稱和密碼已獲授權來管理您的 Azure 服務。</span><span class="sxs-lookup"><span data-stu-id="0b730-133">These files include user names and passwords that are authorized to manage your Azure services.</span></span> <span data-ttu-id="0b730-134">如果您使用這個方法用來建立發行設定檔，以及如果您檢查這些檔案加入原始檔控制中，存取您的儲存機制的任何人都可以看到這些使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="0b730-134">If you use this method to create publish profiles, and if you check in these files to source control, anyone with access to your repository can see those user names and passwords.</span></span> <span data-ttu-id="0b730-135">您可以安全地將密碼儲存在發行設定檔本身，因為它會加密，且位於 *.pubxml.user* ，依預設不會納入原始檔控制的檔案。</span><span class="sxs-lookup"><span data-stu-id="0b730-135">You can safely store the password in the publish profile itself because it's encrypted and it's in a *.pubxml.user* file that by default is not included in source control.</span></span>

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a><span data-ttu-id="0b730-136">結構以促進 DevOps 工作流程的來源分支</span><span class="sxs-lookup"><span data-stu-id="0b730-136">Structure source branches to facilitate DevOps workflow</span></span>

<span data-ttu-id="0b730-137">如何在您的存放庫中實作分支會影響您開發新功能和在生產環境中修正問題的能力。</span><span class="sxs-lookup"><span data-stu-id="0b730-137">How you implement branches in your repository affects your ability to both develop new features and fix issues in production.</span></span> <span data-ttu-id="0b730-138">以下是許多中型大小的小組使用的模式：</span><span class="sxs-lookup"><span data-stu-id="0b730-138">Here is a pattern that a lot of medium sized teams use:</span></span>

![來源分支結構](source-control/_static/image1.png)

<span data-ttu-id="0b730-140">主要的分支永遠符合在生產環境中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="0b730-140">The master branch always matches code that is in production.</span></span> <span data-ttu-id="0b730-141">下方主要分支會對應至不同的階段開發生命週期中。</span><span class="sxs-lookup"><span data-stu-id="0b730-141">Branches underneath master correspond to different stages in the development life cycle.</span></span> <span data-ttu-id="0b730-142">「 開發 」 分支是您用來實作新功能。</span><span class="sxs-lookup"><span data-stu-id="0b730-142">The development branch is where you implement new features.</span></span> <span data-ttu-id="0b730-143">對小型的小組您可能只有 master 和開發，但我們通常建議人員具備開發和主機之間的預備分支。</span><span class="sxs-lookup"><span data-stu-id="0b730-143">For a small team you might just have master and development, but we often recommend that people have a staging branch between development and master.</span></span> <span data-ttu-id="0b730-144">針對最終整合測試更新移動到生產環境之前，您可以使用預備環境。</span><span class="sxs-lookup"><span data-stu-id="0b730-144">You can use staging for final integration testing before an update is moved to production.</span></span>

<span data-ttu-id="0b730-145">適用於大型的小組可能有不同的分支，每個新功能;較小小組您可能必須每個人都以 「 開發 」 分支簽入。</span><span class="sxs-lookup"><span data-stu-id="0b730-145">For big teams there may be separate branches for each new feature; for a smaller team you might have everyone checking in to the development branch.</span></span>

<span data-ttu-id="0b730-146">如果您有的分支，每項功能，當 Feature A 已經準備好您合併其原始程式碼變更增加至開發分支，然後向下到其他的功能分支。</span><span class="sxs-lookup"><span data-stu-id="0b730-146">If you have a branch for each feature, when Feature A is ready you merge its source code changes up into the development branch and down into the other feature branches.</span></span> <span data-ttu-id="0b730-147">此合併處理序的原始程式碼可能很耗時的而且若要避免這項工作，同時仍保留功能不同，某些小組實作稱為 「 替代*[功能切換](http://en.wikipedia.org/wiki/Feature_toggle)* （也稱為*功能旗標*)。</span><span class="sxs-lookup"><span data-stu-id="0b730-147">This source code merging process can be time-consuming, and to avoid that work while still keeping features separate, some teams implement an alternative called *[feature toggles](http://en.wikipedia.org/wiki/Feature_toggle)* (also known as *feature flags*).</span></span> <span data-ttu-id="0b730-148">這表示所有的所有功能的程式碼是在相同的分支中，但您啟用或停用程式碼中使用參數的每個功能。</span><span class="sxs-lookup"><span data-stu-id="0b730-148">This means all of the code for all of the features is in the same branch, but you enable or disable each feature by using switches in the code.</span></span> <span data-ttu-id="0b730-149">例如，假設 Feature A 是它修正應用程式工作，新的欄位和 Feature B 新增快取的功能。</span><span class="sxs-lookup"><span data-stu-id="0b730-149">For example, suppose Feature A is a new field for Fix It app tasks, and Feature B adds caching functionality.</span></span> <span data-ttu-id="0b730-150">這兩項功能的程式碼可以是 「 開發 」 分支，但應用程式將只顯示新的欄位時變數設為 true，而且它只會使用快取設定不同的變數時為 true。</span><span class="sxs-lookup"><span data-stu-id="0b730-150">The code for both features can be in the development branch, but the app will only display the new field when a variable is set to true, and it will only use caching when a different variable is set to true.</span></span> <span data-ttu-id="0b730-151">如果功能的結果尚未準備好升級，但 Feature B 已就緒，您可以將所有關閉功能的切換到生產環境程式碼的升級，Feature B 開啟。</span><span class="sxs-lookup"><span data-stu-id="0b730-151">If Feature A isn't ready to be promoted but the Feature B is ready, you can promote all of the code to Production with the Feature A switch off and the Feature B switch on.</span></span> <span data-ttu-id="0b730-152">然後，您可以完成功能的並將它升級之後，所有與任何來源的程式碼合併。</span><span class="sxs-lookup"><span data-stu-id="0b730-152">You can then finish Feature A and promote it later, all with no source code merging.</span></span>

<span data-ttu-id="0b730-153">不論是否使用分支或切換功能，分支結構，就像這樣可讓您靈活且可重複的方式中的流程從開發到生產環境程式碼。</span><span class="sxs-lookup"><span data-stu-id="0b730-153">Whether or not you use branches or toggles for features, a branching structure like this enables you to flow your code from development into production in an agile and repeatable way.</span></span>

<span data-ttu-id="0b730-154">此結構也可讓您快速地回應客戶的意見反應。</span><span class="sxs-lookup"><span data-stu-id="0b730-154">This structure also enables you to react quickly to customer feedback.</span></span> <span data-ttu-id="0b730-155">如果您需要快速修正生產環境，您也可以執行，有效率地敏捷的方式。</span><span class="sxs-lookup"><span data-stu-id="0b730-155">If you need to make a quick fix to production, you can also do that efficiently in an agile way.</span></span> <span data-ttu-id="0b730-156">您可以建立從 master 或預備分支，以及何時就緒向上合併至 master 和向下到開發和功能分支。</span><span class="sxs-lookup"><span data-stu-id="0b730-156">You can create a branch off of master or staging, and when it's ready merge it up into master and down into development and feature branches.</span></span>

![Hotfix 分支](source-control/_static/image2.png)

<span data-ttu-id="0b730-158">而其為區隔生產和開發分支的分支結構，這類不實際執行的問題可以將放入您不必升級新功能的程式碼，以及您的生產環境修正的位置。</span><span class="sxs-lookup"><span data-stu-id="0b730-158">Without a branching structure like this with its separation of production and development branches, a production problem could put you in the position of having to promote new feature code along with your production fix.</span></span> <span data-ttu-id="0b730-159">新功能的程式碼可能無法完全經過測試，並可供生產環境，您可能必須進行許多變更尚未準備好支援的工作。</span><span class="sxs-lookup"><span data-stu-id="0b730-159">The new feature code might not be fully tested and ready for production and you might have to do a lot of work backing out changes that aren't ready.</span></span> <span data-ttu-id="0b730-160">或者，您可能會延遲您的修正，以測試變更，並讓它們準備部署。</span><span class="sxs-lookup"><span data-stu-id="0b730-160">Or you might have to delay your fix in order to test changes and get them ready to deploy.</span></span>

<span data-ttu-id="0b730-161">接下來，您會看到如何在 Visual Studio、 Azure 和 Azure 儲存機制中實作這三個模式的範例。</span><span class="sxs-lookup"><span data-stu-id="0b730-161">Next you'll see examples of how to implement these three patterns in Visual Studio, Azure, and Azure Repos.</span></span> <span data-ttu-id="0b730-162">這些是範例，而不是詳細的逐步作法-要-執行-it 指示;如需詳細指示，提供所有必要的內容，請參閱[資源](#resources)本章的最後一節。</span><span class="sxs-lookup"><span data-stu-id="0b730-162">These are examples rather than detailed step-by-step how-to-do-it instructions; for detailed instructions that provide all of the context necessary, see the [Resources](#resources) section at the end of the chapter.</span></span>

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a><span data-ttu-id="0b730-163">在 Visual Studio 中的原始檔控制中加入指令碼</span><span class="sxs-lookup"><span data-stu-id="0b730-163">Add scripts to source control in Visual Studio</span></span>

<span data-ttu-id="0b730-164">您可以新增至原始檔控制在 Visual Studio 中的指令碼，將其納入 Visual Studio 方案資料夾 （假設您的專案在原始檔控制中）。</span><span class="sxs-lookup"><span data-stu-id="0b730-164">You can add scripts to source control in Visual Studio by including them in a Visual Studio solution folder (assuming your project is in source control).</span></span> <span data-ttu-id="0b730-165">若要這麼做的其中一個方法如下。</span><span class="sxs-lookup"><span data-stu-id="0b730-165">Here's one way to do it.</span></span>

<span data-ttu-id="0b730-166">在您的方案資料夾中建立指令碼的資料夾 (相同的資料夾具有您 *.sln*檔案)。</span><span class="sxs-lookup"><span data-stu-id="0b730-166">Create a folder for the scripts in your solution folder (the same folder that has your *.sln* file).</span></span>

![自動化資料夾](source-control/_static/image3.png)

<span data-ttu-id="0b730-168">將指令碼檔案複製到資料夾。</span><span class="sxs-lookup"><span data-stu-id="0b730-168">Copy the script files into the folder.</span></span>

![自動化資料夾內容](source-control/_static/image4.png)

<span data-ttu-id="0b730-170">在 Visual Studio 中，加入專案中的方案資料夾。</span><span class="sxs-lookup"><span data-stu-id="0b730-170">In Visual Studio, add a solution folder to the project.</span></span>

![新的方案資料夾功能表選取項目](source-control/_static/image5.png)

<span data-ttu-id="0b730-172">並將指令碼檔案新增至方案資料夾。</span><span class="sxs-lookup"><span data-stu-id="0b730-172">And add the script files to the solution folder.</span></span>

![新增現有項目 功能表選取項目](source-control/_static/image6.png)

![[加入現有項目] 對話方塊](source-control/_static/image7.png)

<span data-ttu-id="0b730-175">指令碼檔案現在會包含在專案和原始檔控制正在追蹤其版本變更，以及對應原始程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="0b730-175">The script files are now included in your project and source control is tracking their version changes along with corresponding source code changes.</span></span>

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a><span data-ttu-id="0b730-176">在 Azure 中儲存敏感性資料</span><span class="sxs-lookup"><span data-stu-id="0b730-176">Store sensitive data in Azure</span></span>

<span data-ttu-id="0b730-177">如果您執行您的應用程式在 Azure 網站時，避免將認證儲存在原始檔控制中的一種方法是將它們儲存在 Azure 中。</span><span class="sxs-lookup"><span data-stu-id="0b730-177">If you run your application in an Azure Web Site, one way to avoid storing credentials in source control is to store them in Azure instead.</span></span>

<span data-ttu-id="0b730-178">比方說，它修正應用程式會儲存在其 Web.config 檔案兩個連接字串，必須在生產環境並可讓您的 Azure 儲存體帳戶存取金鑰的密碼。</span><span class="sxs-lookup"><span data-stu-id="0b730-178">For example, the Fix It application stores in its Web.config file two connection strings that will have passwords in production and a key that gives access to your Azure storage account.</span></span>

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

<span data-ttu-id="0b730-179">如果您將實際的生產環境值，這些設定，在您*Web.config*檔案，或如果您將它們放在*Web.Release.config*檔案來設定在部署期間，將 Web.config 轉換它們會儲存於原始程式碼儲存機制。</span><span class="sxs-lookup"><span data-stu-id="0b730-179">If you put actual production values for these settings in your *Web.config* file, or if you put them in the *Web.Release.config* file to configure a Web.config transform to insert them during deployment, they'll be stored in the source repository.</span></span> <span data-ttu-id="0b730-180">如果您輸入的資料庫連接字串至實際執行環境發行設定檔，密碼會在您 *.pubxml*檔案。</span><span class="sxs-lookup"><span data-stu-id="0b730-180">If you enter the database connection strings into the production publish profile, the password will be in your *.pubxml* file.</span></span> <span data-ttu-id="0b730-181">(您無法排除 *.pubxml*檔案從原始檔控制，但接著您會失去共用的其他部署設定。)</span><span class="sxs-lookup"><span data-stu-id="0b730-181">(You could exclude the *.pubxml* file from source control, but then you lose the benefit of sharing all the other deployment settings.)</span></span>

<span data-ttu-id="0b730-182">Azure 可讓您的替代方式**appSettings**和連接字串的區段*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="0b730-182">Azure gives you an alternative for the **appSettings** and connection strings sections of the *Web.config* file.</span></span> <span data-ttu-id="0b730-183">以下是相關的部分**組態**網站在 Azure 管理入口網站中的索引標籤：</span><span class="sxs-lookup"><span data-stu-id="0b730-183">Here is the relevant part of the **Configuration** tab for a web site in the Azure management portal:</span></span>

![appSettings 和入口網站中的 connectionStrings](source-control/_static/image8.png)

<span data-ttu-id="0b730-185">當您將專案部署到此網站和應用程式執行時，您已在 Azure 中儲存任何值會覆寫任何值位於 Web.config 檔案。</span><span class="sxs-lookup"><span data-stu-id="0b730-185">When you deploy a project to this web site and the application runs, whatever values you have stored in Azure override whatever values are in the Web.config file.</span></span>

<span data-ttu-id="0b730-186">您可以使用管理入口網站 」 或 「 指令碼，在 Azure 中設定這些值。</span><span class="sxs-lookup"><span data-stu-id="0b730-186">You can set these values in Azure by using either the management portal or scripts.</span></span> <span data-ttu-id="0b730-187">環境建立自動化指令碼所示[自動執行的所有項目](automate-everything.md)一章會建立 Azure SQL Database、 取得的儲存體和 SQL Database 的連接字串，並將這些機密資訊儲存在您的網站的設定。</span><span class="sxs-lookup"><span data-stu-id="0b730-187">The environment creation automation script you saw in the [Automate Everything](automate-everything.md) chapter creates an Azure SQL Database, gets the storage and SQL Database connection strings, and stores these secrets in the settings for your web site.</span></span>

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

<span data-ttu-id="0b730-188">請注意，指令碼會進行參數化，讓實際的值不保存至來源存放庫。</span><span class="sxs-lookup"><span data-stu-id="0b730-188">Notice that the scripts are parameterized so that actual values don't get persisted to the source repository.</span></span>

<span data-ttu-id="0b730-189">當您在本機執行您的開發環境中時，應用程式讀取您的本機 Web.config 檔案和您的連線字串中的 LocalDB 的 SQL Server 資料庫指向*應用程式\_資料*web 專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="0b730-189">When you run locally in your development environment, the app reads your local Web.config file and your connection string points to a LocalDB SQL Server database in the *App\_Data* folder of your web project.</span></span> <span data-ttu-id="0b730-190">當您在 Azure 中執行應用程式和應用程式會嘗試從 Web.config 檔案中讀取這些值時，取得上一步並使用是網站上，沒有什麼實際上是在 Web.config 檔案中所儲存的值。</span><span class="sxs-lookup"><span data-stu-id="0b730-190">When you run the app in Azure and the app tries to read these values from the Web.config file, what it gets back and uses are the values stored for the Web Site, not what's actually in Web.config file.</span></span>

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-azure-devops"></a><span data-ttu-id="0b730-191">使用 Git，在 Visual Studio 和 Azure 的 DevOps</span><span class="sxs-lookup"><span data-stu-id="0b730-191">Use Git in Visual Studio and Azure DevOps</span></span>

<span data-ttu-id="0b730-192">您可以使用任何原始檔控制環境來實作 DevOps 分支結構，使用稍早所呈現。</span><span class="sxs-lookup"><span data-stu-id="0b730-192">You can use any source control environment to implement the DevOps branching structure presented earlier.</span></span> <span data-ttu-id="0b730-193">分散式團隊[分散式版本控制系統](http://en.wikipedia.org/wiki/Distributed_revision_control)(DVCS) 可能適合; 對於其他團隊[集中式系統](http://en.wikipedia.org/wiki/Revision_control)可能比較適合。</span><span class="sxs-lookup"><span data-stu-id="0b730-193">For distributed teams a [distributed version control system](http://en.wikipedia.org/wiki/Distributed_revision_control) (DVCS) might work best; for other teams a [centralized system](http://en.wikipedia.org/wiki/Revision_control) might work better.</span></span>

<span data-ttu-id="0b730-194">[Git](http://git-scm.com/)是受歡迎的分散式的版本控制系統。</span><span class="sxs-lookup"><span data-stu-id="0b730-194">[Git](http://git-scm.com/) is a popular distributed version control system.</span></span> <span data-ttu-id="0b730-195">當您使用 Git 原始檔控制時，您將在本機電腦上有及其所有記錄的存放庫的完整複本。</span><span class="sxs-lookup"><span data-stu-id="0b730-195">When you use Git for source control, you have a complete copy of the repository with all of its history on your local computer.</span></span> <span data-ttu-id="0b730-196">許多人喜歡，因為可以更輕鬆繼續工作時您未連接到網路，您可以繼續執行認可和回復，建立並切換分支，並繼續下一步。</span><span class="sxs-lookup"><span data-stu-id="0b730-196">Many people prefer that because it's easier to continue working when you're not connected to the network -- you can continue to do commits and rollbacks, create and switch branches, and so forth.</span></span> <span data-ttu-id="0b730-197">即使您連線到網路，很容易且更快速地建立分支及切換分支時的所有項目是在本機。</span><span class="sxs-lookup"><span data-stu-id="0b730-197">Even when you're connected to the network, it's easier and quicker to create branches and switch branches when everything is local.</span></span> <span data-ttu-id="0b730-198">您也可以執行本機認可及回復，而不會影響其他開發人員。</span><span class="sxs-lookup"><span data-stu-id="0b730-198">You can also do local commits and rollbacks without having an impact on other developers.</span></span> <span data-ttu-id="0b730-199">並可批次認可之後，再將它們傳送到伺服器。</span><span class="sxs-lookup"><span data-stu-id="0b730-199">And you can batch commits before sending them to the server.</span></span>

<span data-ttu-id="0b730-200">[Azure 儲存機制](/azure/devops/repos/index?view=vsts)提供兩者[Git](/azure/devops/repos/git/?view=vsts)並[Team Foundation 版本控制](/azure/devops/repos/tfvc/index?view=vsts)(TFVC; 集中式原始檔控制)。</span><span class="sxs-lookup"><span data-stu-id="0b730-200">[Azure Repos](/azure/devops/repos/index?view=vsts) offers both [Git](/azure/devops/repos/git/?view=vsts) and [Team Foundation Version Control](/azure/devops/repos/tfvc/index?view=vsts) (TFVC; centralized source control).</span></span> <span data-ttu-id="0b730-201">開始使用 Azure DevOps[此處](https://app.vsaex.visualstudio.com/signup)。</span><span class="sxs-lookup"><span data-stu-id="0b730-201">Get started with Azure DevOps [here](https://app.vsaex.visualstudio.com/signup).</span></span>

<span data-ttu-id="0b730-202">Visual Studio 2017 包含內建，第一級[Git 支援](https://msdn.microsoft.com/library/hh850437.aspx)。</span><span class="sxs-lookup"><span data-stu-id="0b730-202">Visual Studio 2017 includes built-in, first-class [Git support](https://msdn.microsoft.com/library/hh850437.aspx).</span></span> <span data-ttu-id="0b730-203">以下是快速的運作方式的示範。</span><span class="sxs-lookup"><span data-stu-id="0b730-203">Here's a quick demo of how that works.</span></span>

<span data-ttu-id="0b730-204">使用 Visual Studio 中開啟專案，以滑鼠右鍵按一下中的解決方案**方案總管 中**，然後選擇**將方案加入至原始檔控制**。</span><span class="sxs-lookup"><span data-stu-id="0b730-204">With a project open in Visual Studio, right-click the solution in **Solution Explorer**, and then choose **Add Solution to Source Control**.</span></span>

![將方案加入原始檔控制](source-control/_static/image9.png)

<span data-ttu-id="0b730-206">Visual Studio 會詢問您想要使用 TFVC （集中式的版本控制） 或 Git。</span><span class="sxs-lookup"><span data-stu-id="0b730-206">Visual Studio asks if you want to use TFVC (centralized version control) or Git.</span></span>

![選擇原始檔控制](source-control/_static/image10.png)

<span data-ttu-id="0b730-208">當您選取 Git，並按一下**確定**，Visual Studio 在您的方案資料夾中建立新的本機 Git 存放庫。</span><span class="sxs-lookup"><span data-stu-id="0b730-208">When you select Git and click **OK**, Visual Studio creates a new local Git repository in your solution folder.</span></span> <span data-ttu-id="0b730-209">新的存放庫中未指定任何檔案您必須將其加入至儲存機制執行 Git 認可。</span><span class="sxs-lookup"><span data-stu-id="0b730-209">The new repository has no files yet; you have to add them to the repository by doing a Git commit.</span></span> <span data-ttu-id="0b730-210">在方案上按一下滑鼠右鍵**方案總管**，然後按一下**認可**。</span><span class="sxs-lookup"><span data-stu-id="0b730-210">Right-click the solution in **Solution Explorer**, and then click **Commit**.</span></span>

![認可](source-control/_static/image11.png)

<span data-ttu-id="0b730-212">Visual Studio 會自動設置的所有專案檔案進行認可，而且列在**Team Explorer**中**包含的變更**窗格。</span><span class="sxs-lookup"><span data-stu-id="0b730-212">Visual Studio automatically stages all of the project files for the commit and lists them in **Team Explorer** in the **Included Changes** pane.</span></span> <span data-ttu-id="0b730-213">(如果有一些您不想要包含在認可中，您可以選取它們，以滑鼠右鍵按一下，然後按一下 **排除**。)</span><span class="sxs-lookup"><span data-stu-id="0b730-213">(If there were some you didn't want to include in the commit, you could select them, right-click, and click **Exclude**.)</span></span>

![Team Explorer](source-control/_static/image12.png)

<span data-ttu-id="0b730-215">輸入認可註解，然後按一下**認可**，和 Visual Studio 執行認可，並且顯示認可識別碼。</span><span class="sxs-lookup"><span data-stu-id="0b730-215">Enter a commit comment and click **Commit**, and Visual Studio executes the commit and displays the commit ID.</span></span>

![Team Explorer 的變更](source-control/_static/image13.png)

<span data-ttu-id="0b730-217">現在如果您變更一些程式碼，使其不同的儲存機制中，您可以輕鬆地檢視差異。</span><span class="sxs-lookup"><span data-stu-id="0b730-217">Now if you change some code so that it's different from what's in the repository, you can easily view the differences.</span></span> <span data-ttu-id="0b730-218">以滑鼠右鍵按一下檔案，您已經變更，選取**Unmodified 與比較**，並取得未認可的變更會顯示比較顯示。</span><span class="sxs-lookup"><span data-stu-id="0b730-218">Right-click a file that you've changed, select **Compare with Unmodified**, and you get a comparison display that shows your uncommitted change.</span></span>

![與未修改的比較](source-control/_static/image14.png)

![差異顯示變更](source-control/_static/image15.png)

<span data-ttu-id="0b730-221">您可以輕鬆地看到您正在進行，並將它們簽入哪些變更。</span><span class="sxs-lookup"><span data-stu-id="0b730-221">You can easily see what changes you're making and check them in.</span></span>

<span data-ttu-id="0b730-222">假設您需要進行分支 – 您可以在 Visual Studio 中執行。</span><span class="sxs-lookup"><span data-stu-id="0b730-222">Suppose you need to make a branch – you can do that in Visual Studio too.</span></span> <span data-ttu-id="0b730-223">在  **Team Explorer**，按一下**新的分支**。</span><span class="sxs-lookup"><span data-stu-id="0b730-223">In **Team Explorer**, click **New Branch**.</span></span>

![Team Explorer 中的新分支](source-control/_static/image16.png)

<span data-ttu-id="0b730-225">輸入分支名稱，按一下**Create Branch**，以及您選取**簽出分支**，Visual Studio 會自動簽出新的分支。</span><span class="sxs-lookup"><span data-stu-id="0b730-225">Enter a branch name, click **Create Branch**, and if you selected **Checkout branch**, Visual Studio automatically checks out the new branch.</span></span>

![Team Explorer 中的新分支](source-control/_static/image17.png)

<span data-ttu-id="0b730-227">您現在可以對檔案進行變更，並簽入至這個分支。</span><span class="sxs-lookup"><span data-stu-id="0b730-227">You can now make changes to files and check them in to this branch.</span></span> <span data-ttu-id="0b730-228">此外，您可以輕鬆地切換分支和 Visual Studio 會自動同步處理到者為準的分支的檔案已簽出。在此範例中顯示標題中的網頁 *\_Layout.cshtml*已變更為 「 Hot Fix 1 」 在 HotFix1 分支。</span><span class="sxs-lookup"><span data-stu-id="0b730-228">And you can easily switch between branches and Visual Studio automatically syncs the files to whichever branch you have checked out. In this example the web page title in *\_Layout.cshtml* has been changed to "Hot Fix 1" in HotFix1 branch.</span></span>

![Hotfix1 分支](source-control/_static/image18.png)

<span data-ttu-id="0b730-230">如果您切換回主要分支的內容 *\_Layout.cshtml*檔案自動還原它們的主要分支中。</span><span class="sxs-lookup"><span data-stu-id="0b730-230">If you switch back to the master branch, the contents of the *\_Layout.cshtml* file automatically revert to what they are in the master branch.</span></span>

![主要分支](source-control/_static/image19.png)

<span data-ttu-id="0b730-232">此方式快速地建立分支和分支之間來回翻轉的簡單範例。</span><span class="sxs-lookup"><span data-stu-id="0b730-232">This a simple example of how you can quickly create a branch and flip back and forth between branches.</span></span> <span data-ttu-id="0b730-233">這項功能可讓使用分支結構的高度敏捷式軟體開發工作流程和自動化指令碼所示[自動執行的所有項目](automate-everything.md)一章。</span><span class="sxs-lookup"><span data-stu-id="0b730-233">This feature enables a highly agile workflow using the branch structure and automation scripts presented in the [Automate Everything](automate-everything.md) chapter.</span></span> <span data-ttu-id="0b730-234">例如，您可以在 「 開發 」 分支中工作，建立 hotfix 分支從主、 切換至新的分支、 讓您的變更和認可，並再切換回 「 開發 」 分支並繼續您先前進行的作業。</span><span class="sxs-lookup"><span data-stu-id="0b730-234">For example, you can be working in the Development branch, create a hot fix branch off of master, switch to the new branch, make your changes there and commit them, and then switch back to the Development branch and continue what you were doing.</span></span>

<span data-ttu-id="0b730-235">您已看這裡是您在 Visual Studio 中以本機 Git 儲存機制的運作方式。</span><span class="sxs-lookup"><span data-stu-id="0b730-235">What you've seen here is how you work with a local Git repository in Visual Studio.</span></span> <span data-ttu-id="0b730-236">在小組環境中您通常也發送變更到通用儲存機制。</span><span class="sxs-lookup"><span data-stu-id="0b730-236">In a team environment you typically also push changes up to a common repository.</span></span> <span data-ttu-id="0b730-237">Visual Studio 工具也可讓您以指向遠端 Git 存放庫。</span><span class="sxs-lookup"><span data-stu-id="0b730-237">The Visual Studio tools also enable you to point to a remote Git repository.</span></span> <span data-ttu-id="0b730-238">您可以使用 GitHub.com 基於這個目的，或者您可以使用[Git 和 Azure 儲存機制](/azure/devops/repos/git/overview?view=vsts)與所有其他 Azure DevOps 功能，例如工作項目和 bug 追蹤整合在一起。</span><span class="sxs-lookup"><span data-stu-id="0b730-238">You can use GitHub.com for that purpose, or you can use [Git and Azure Repos](/azure/devops/repos/git/overview?view=vsts) integrated with all the other Azure DevOps capabilities such as work item and bug tracking.</span></span>

<span data-ttu-id="0b730-239">這不是唯一的方法，您可以實作敏捷式軟體開發分支策略，當然。</span><span class="sxs-lookup"><span data-stu-id="0b730-239">This isn't the only way you can implement an agile branching strategy, of course.</span></span> <span data-ttu-id="0b730-240">您可以啟用相同的敏捷式軟體開發工作流程，以使用集中式原始檔控制儲存機制。</span><span class="sxs-lookup"><span data-stu-id="0b730-240">You can enable the same agile workflow using a centralized source control repository.</span></span>

## <a name="summary"></a><span data-ttu-id="0b730-241">總結</span><span class="sxs-lookup"><span data-stu-id="0b730-241">Summary</span></span>

<span data-ttu-id="0b730-242">測量您如何快速進行變更，並取得即時安全且可預測的方式為基礎的原始檔控制系統成功。</span><span class="sxs-lookup"><span data-stu-id="0b730-242">Measure the success of your source control system based on how quickly you can make a change and get it live in a safe and predictable way.</span></span> <span data-ttu-id="0b730-243">如果您發現自己害怕得進行變更，因為您只需要一天，或兩個在其上的手動測試，您可能會自問您不必執行 process-wise 或 test-wise，以便您可以進行該變更，以分鐘為單位，或在無法再最差超過一小時。</span><span class="sxs-lookup"><span data-stu-id="0b730-243">If you find yourself scared to make a change because you have to do a day or two of manual testing on it, you might ask yourself what you have to do process-wise or test-wise so that you can make that change in minutes or at worst no longer than an hour.</span></span> <span data-ttu-id="0b730-244">這麼做的其中一個策略是實作持續整合與持續傳遞，在中，我們將討論[下一步 一章](continuous-integration-and-continuous-delivery.md)。</span><span class="sxs-lookup"><span data-stu-id="0b730-244">One strategy for doing that is to implement continuous integration and continuous delivery, which we'll cover in the [next chapter](continuous-integration-and-continuous-delivery.md).</span></span>

<a id="resources"></a>
## <a name="resources"></a><span data-ttu-id="0b730-245">資源</span><span class="sxs-lookup"><span data-stu-id="0b730-245">Resources</span></span>

<span data-ttu-id="0b730-246">如需分支策略的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="0b730-246">For more information about branching strategies, see the following resources:</span></span>

- <span data-ttu-id="0b730-247">[建置與 Team Foundation Server 2012 的發行管線](https://msdn.microsoft.com/library/dn449957.aspx)。</span><span class="sxs-lookup"><span data-stu-id="0b730-247">[Building a Release Pipeline with Team Foundation Server 2012](https://msdn.microsoft.com/library/dn449957.aspx).</span></span> <span data-ttu-id="0b730-248">Microsoft Patterns and Practices 文件。</span><span class="sxs-lookup"><span data-stu-id="0b730-248">Microsoft Patterns and Practices documentation.</span></span> <span data-ttu-id="0b730-249">請參閱第 6 章，如需分支策略的討論。</span><span class="sxs-lookup"><span data-stu-id="0b730-249">See chapter 6 for a discussion of branching strategies.</span></span> <span data-ttu-id="0b730-250">提倡功能與切換功能分支，透過使用功能分支時，如果是代表很短期 （小時或天至多）。</span><span class="sxs-lookup"><span data-stu-id="0b730-250">Advocates feature toggles over feature branches, and if branches for features are used, advocates keeping them short-lived (hours or days at most).</span></span>
- <span data-ttu-id="0b730-251">[版本控制指南](https://aka.ms/vsarsolutions)。</span><span class="sxs-lookup"><span data-stu-id="0b730-251">[Version Control Guide](https://aka.ms/vsarsolutions).</span></span> <span data-ttu-id="0b730-252">ALM Ranger 指引分支策略。</span><span class="sxs-lookup"><span data-stu-id="0b730-252">Guide to branching strategies by the ALM Rangers.</span></span> <span data-ttu-id="0b730-253">在 [下載] 索引標籤上，請參閱分支 Strategies.pdf。</span><span class="sxs-lookup"><span data-stu-id="0b730-253">See Branching Strategies.pdf on the Downloads tab.</span></span>
- <span data-ttu-id="0b730-254">[使用功能切換進行軟體開發](https://msdn.microsoft.com/magazine/dn683796.aspx)。</span><span class="sxs-lookup"><span data-stu-id="0b730-254">[Software Development with Feature Toggles](https://msdn.microsoft.com/magazine/dn683796.aspx).</span></span> <span data-ttu-id="0b730-255">MSDN Magazine 文章。</span><span class="sxs-lookup"><span data-stu-id="0b730-255">MSDN Magazine article.</span></span>
- <span data-ttu-id="0b730-256">[功能切換](http://martinfowler.com/bliki/FeatureToggle.html)。</span><span class="sxs-lookup"><span data-stu-id="0b730-256">[Feature Toggle](http://martinfowler.com/bliki/FeatureToggle.html).</span></span> <span data-ttu-id="0b730-257">簡介功能切換/Martin Fowler 部落格上的功能旗標。</span><span class="sxs-lookup"><span data-stu-id="0b730-257">Introduction to feature toggles / feature flags on Martin Fowler's blog.</span></span>
- <span data-ttu-id="0b730-258">[功能切換 vs 功能分支](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx)。</span><span class="sxs-lookup"><span data-stu-id="0b730-258">[Feature Toggles vs Feature Branches](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx).</span></span> <span data-ttu-id="0b730-259">關於功能切換，Dylan smith 的另一個部落格文章。</span><span class="sxs-lookup"><span data-stu-id="0b730-259">Another blog post about feature toggles, by Dylan Smith.</span></span>

<span data-ttu-id="0b730-260">如需如何處理不會保留在原始檔控制存放庫中的機密資訊的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="0b730-260">For more information about how to handle sensitive information that should not be kept in source control repositories, see the following resources:</span></span>

- <span data-ttu-id="0b730-261">[將密碼和其他機密資料部署到 ASP.NET 和 Azure App Service 最佳作法](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)。</span><span class="sxs-lookup"><span data-stu-id="0b730-261">[Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
- <span data-ttu-id="0b730-262">[Azure 網站： 如何應用程式字串與連接字串的運作](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)。</span><span class="sxs-lookup"><span data-stu-id="0b730-262">[Azure Web Sites: How Application Strings and Connection Strings Work](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/).</span></span> <span data-ttu-id="0b730-263">說明 Azure 功能，以覆寫`appSettings`並`connectionStrings`中的資料*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="0b730-263">Explains the Azure feature that overrides `appSettings` and `connectionStrings` data in the *Web.config* file.</span></span>
- <span data-ttu-id="0b730-264">[自訂組態和應用程式設定 Azure 網站中-Stefan schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/)。</span><span class="sxs-lookup"><span data-stu-id="0b730-264">[Custom configuration and application settings in Azure Web Sites - with Stefan Schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/).</span></span>

<span data-ttu-id="0b730-265">保留出原始檔控制的機密資訊的其他方法的相關資訊，請參閱[ASP.NET MVC： 保留原始檔控制的私用設定](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx)。</span><span class="sxs-lookup"><span data-stu-id="0b730-265">For information about other methods for keeping sensitive information out of source control, see [ASP.NET MVC: Keep Private Settings Out of Source Control](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0b730-266">[上一頁](automate-everything.md)
> [下一頁](continuous-integration-and-continuous-delivery.md)</span><span class="sxs-lookup"><span data-stu-id="0b730-266">[Previous](automate-everything.md)
[Next](continuous-integration-and-continuous-delivery.md)</span></span>
