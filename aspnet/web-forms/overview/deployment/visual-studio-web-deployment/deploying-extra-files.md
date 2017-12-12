---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: "使用 Visual Studio 的 ASP.NET Web 部署： 部署的額外檔案 |Microsoft 文件"
author: tdykstra
description: "此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: a34607b25f6cf502f5fbf2fe51bf1937f470159e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a><span data-ttu-id="7ccef-103">使用 Visual Studio 的 ASP.NET Web 部署： 部署額外的檔案</span><span class="sxs-lookup"><span data-stu-id="7ccef-103">ASP.NET Web Deployment using Visual Studio: Deploying Extra Files</span></span>
====================
<span data-ttu-id="7ccef-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="7ccef-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="7ccef-105">下載入門專案</span><span class="sxs-lookup"><span data-stu-id="7ccef-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="7ccef-106">此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="7ccef-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="7ccef-107">數列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="7ccef-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="7ccef-108">概觀</span><span class="sxs-lookup"><span data-stu-id="7ccef-108">Overview</span></span>

<span data-ttu-id="7ccef-109">本教學課程會示範如何擴充 Visual Studio web 發行管線部署期間進行一個額外的工作。</span><span class="sxs-lookup"><span data-stu-id="7ccef-109">This tutorial shows how to extend the Visual Studio web publish pipeline to do an additional task during deployment.</span></span> <span data-ttu-id="7ccef-110">工作是要複製到目的地網站的專案資料夾中所沒有的額外檔案。</span><span class="sxs-lookup"><span data-stu-id="7ccef-110">The task is to copy extra files that are not in the project folder to the destination web site.</span></span>

<span data-ttu-id="7ccef-111">在此教學課程中，您將會複製一個額外的檔案： *robots.txt*。</span><span class="sxs-lookup"><span data-stu-id="7ccef-111">For this tutorial you'll copy one extra file: *robots.txt*.</span></span> <span data-ttu-id="7ccef-112">您想要將此檔案部署到預備環境，但不是屬於實際執行環境。</span><span class="sxs-lookup"><span data-stu-id="7ccef-112">You want to deploy this file to staging but not to production.</span></span> <span data-ttu-id="7ccef-113">在[到生產環境部署](deploying-to-production.md)教學課程，您加入至專案的這個檔案，並設定生產發行設定檔，以將其排除。</span><span class="sxs-lookup"><span data-stu-id="7ccef-113">In [the Deploying to Production](deploying-to-production.md) tutorial, you added this file to the project and configured the Production publish profile to exclude it.</span></span> <span data-ttu-id="7ccef-114">在本教學課程中，您會看到使用替代方法來處理此情況下，一個適用於您想要部署但不想要包含在專案中的任何檔案。</span><span class="sxs-lookup"><span data-stu-id="7ccef-114">In this tutorial you'll see an alternative method to handle this situation, one that will be useful for any files that you want to deploy but don't want to include in the project.</span></span>

## <a name="move-the-robotstxt-file"></a><span data-ttu-id="7ccef-115">移動 robots.txt 檔案</span><span class="sxs-lookup"><span data-stu-id="7ccef-115">Move the robots.txt file</span></span>

<span data-ttu-id="7ccef-116">若要準備用於不同的處理方法*robots.txt*本節的教學課程中您將檔案移至資料夾未包含在專案中，且您刪除*robots.txt*從臨時區域環境。</span><span class="sxs-lookup"><span data-stu-id="7ccef-116">To prepare for a different method of handling *robots.txt*, in this section of the tutorial you move the file to a folder that is not included in the project, and you delete *robots.txt* from the staging environment.</span></span> <span data-ttu-id="7ccef-117">若要刪除檔案，從暫存，以便您可以驗證您的檔案部署到該環境的新方法正常運作必須。</span><span class="sxs-lookup"><span data-stu-id="7ccef-117">It is necessary to delete the file from staging so that you can verify that your new method of deploying the file to that environment is working correctly.</span></span>

1. <span data-ttu-id="7ccef-118">在**方案總管] 中**，以滑鼠右鍵按一下*robots.txt*檔案，然後按一下 [**從專案移除**。</span><span class="sxs-lookup"><span data-stu-id="7ccef-118">In **Solution Explorer**, right-click the *robots.txt* file and click **Exclude From Project**.</span></span>
2. <span data-ttu-id="7ccef-119">使用 Windows 檔案總管，在方案資料夾中建立新的資料夾，並將它命名*ExtraFiles*。</span><span class="sxs-lookup"><span data-stu-id="7ccef-119">Using Windows File Explorer, create a new folder in the solution folder and name it *ExtraFiles*.</span></span>
3. <span data-ttu-id="7ccef-120">移動*robots.txt*檔案從*ContosoUniversity*到專案資料夾*ExtraFiles*資料夾。</span><span class="sxs-lookup"><span data-stu-id="7ccef-120">Move the *robots.txt* file from the *ContosoUniversity* project folder to the *ExtraFiles* folder.</span></span>

    ![ExtraFiles 資料夾](deploying-extra-files/_static/image1.png)
4. <span data-ttu-id="7ccef-122">使用您的 FTP 工具，刪除*robots.txt*從預備網站的檔案。</span><span class="sxs-lookup"><span data-stu-id="7ccef-122">Using your FTP tool, delete the *robots.txt* file from the staging web site.</span></span>

    <span data-ttu-id="7ccef-123">或者，您可以選取**移除目的端的其他檔案**下**檔案發行選項**上**設定**預備發行設定檔 索引標籤和重新發行到預備環境。</span><span class="sxs-lookup"><span data-stu-id="7ccef-123">As an alternative, you can select **Remove additional files at destination** under **File Publish Options** on the **Settings** tab of the Staging publish profile, and republish to staging.</span></span>

## <a name="update-the-publish-profile-file"></a><span data-ttu-id="7ccef-124">更新發行設定檔</span><span class="sxs-lookup"><span data-stu-id="7ccef-124">Update the publish profile file</span></span>

<span data-ttu-id="7ccef-125">您只需要*robots.txt*中暫置，因此您需要更新，才能部署它唯一的發行設定檔暫存。</span><span class="sxs-lookup"><span data-stu-id="7ccef-125">You only need *robots.txt* in staging, so the only publish profile you need to update in order to deploy it is Staging.</span></span>

1. <span data-ttu-id="7ccef-126">在 Visual Studio 中開啟*Staging.pubxml*。</span><span class="sxs-lookup"><span data-stu-id="7ccef-126">In Visual Studio, open *Staging.pubxml*.</span></span>
2. <span data-ttu-id="7ccef-127">結尾的檔案，之後再關閉`</Project>`標記中，加入下列標記：</span><span class="sxs-lookup"><span data-stu-id="7ccef-127">At the end of the file, before the closing `</Project>` tag, add the following markup:</span></span>

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    <span data-ttu-id="7ccef-128">此程式碼會建立新*目標*，將會收集其他要部署的檔案。</span><span class="sxs-lookup"><span data-stu-id="7ccef-128">This code creates a new *target* that will collect additional files to be deployed.</span></span> <span data-ttu-id="7ccef-129">目標組成一或更多的工作將執行 MSBuild，根據您指定的條件。</span><span class="sxs-lookup"><span data-stu-id="7ccef-129">A target is composed of one or more tasks that MSBuild will execute based on conditions you specify.</span></span>

    <span data-ttu-id="7ccef-130">`Include`屬性會指定在其中尋找檔案的資料夾是*ExtraFiles*位於相同的層級的專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="7ccef-130">The `Include` attribute specifies that the folder in which to find the files is *ExtraFiles*, located at the same level as the project folder.</span></span> <span data-ttu-id="7ccef-131">MSBuild 會收集所有檔案從該資料夾，然後以遞迴方式從任何子資料夾 （double 星號指定遞迴的子資料夾）。</span><span class="sxs-lookup"><span data-stu-id="7ccef-131">MSBuild will collect all files from that folder and recursively from any subfolders (the double asterisk specifies recursive subfolders).</span></span> <span data-ttu-id="7ccef-132">這段程式碼與您無法將多個檔案和檔案子資料夾中*ExtraFiles*資料夾，以及所有將會部署。</span><span class="sxs-lookup"><span data-stu-id="7ccef-132">With this code you could put multiple files, and files in subfolders inside the *ExtraFiles* folder, and all will be deployed.</span></span>

    <span data-ttu-id="7ccef-133">`DestinationRelativePath`項目可讓您指定的資料夾和檔案應該複製到目的地網站上，在相同的檔案及資料夾結構的根資料夾中找到*ExtraFiles*資料夾。</span><span class="sxs-lookup"><span data-stu-id="7ccef-133">The `DestinationRelativePath` element specifies that the folders and files should be copied to the root folder of the destination web site, in the same file and folder structure as they are found in the *ExtraFiles* folder.</span></span> <span data-ttu-id="7ccef-134">如果您想要複製*ExtraFiles*資料夾本身，`DestinationRelativePath`值會是*ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*。</span><span class="sxs-lookup"><span data-stu-id="7ccef-134">If you wanted to copy the *ExtraFiles* folder itself, the `DestinationRelativePath` value would be *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.</span></span>
3. <span data-ttu-id="7ccef-135">結尾的檔案，之後再關閉`</Project>`標記中，加入下列標記會指定何時要執行新的目標。</span><span class="sxs-lookup"><span data-stu-id="7ccef-135">At the end of the file, before the closing `</Project>` tag, add the following markup that specifies when to execute the new target.</span></span>

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    <span data-ttu-id="7ccef-136">此程式碼會造成新`CustomCollectFiles`執行目標將檔案複製到目的地資料夾時要執行的目標。</span><span class="sxs-lookup"><span data-stu-id="7ccef-136">This code causes the new `CustomCollectFiles` target to be executed whenever the target that copies files to the destination folder is executed.</span></span> <span data-ttu-id="7ccef-137">沒有與部署套件建立、 發行的個別目標，且新的目標插入在這兩個目標，萬一您決定要使用的部署封裝，而不發行部署。</span><span class="sxs-lookup"><span data-stu-id="7ccef-137">There is a separate target for publish versus deployment package creation, and the new target is injected in both targets in case you decide to deploy by using a deployment package instead of publishing.</span></span>

    <span data-ttu-id="7ccef-138">*.Pubxml*檔現在看起來像下列的範例：</span><span class="sxs-lookup"><span data-stu-id="7ccef-138">The *.pubxml* file now looks like the following example:</span></span>

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. <span data-ttu-id="7ccef-139">儲存並關閉*Staging.pubxml*檔案。</span><span class="sxs-lookup"><span data-stu-id="7ccef-139">Save and close the *Staging.pubxml* file.</span></span>

## <a name="publish-to-staging"></a><span data-ttu-id="7ccef-140">發行到預備環境</span><span class="sxs-lookup"><span data-stu-id="7ccef-140">Publish to staging</span></span>

<span data-ttu-id="7ccef-141">使用單鍵發行或命令列中，發行該應用程式使用的暫存設定檔。</span><span class="sxs-lookup"><span data-stu-id="7ccef-141">Using one-click publish or the command line, publish the application by using the Staging profile.</span></span>

<span data-ttu-id="7ccef-142">如果您使用單鍵發行，您可以在 確認**預覽**視窗， *robots.txt*會被複製。</span><span class="sxs-lookup"><span data-stu-id="7ccef-142">If you use one-click publish, you can verify in the **Preview** window that *robots.txt* will be copied.</span></span> <span data-ttu-id="7ccef-143">否則，請使用您的 FTP 工具可讓您確認*robots.txt*檔案是在部署後網站的根資料夾中。</span><span class="sxs-lookup"><span data-stu-id="7ccef-143">Otherwise, use your FTP tool to verify that the *robots.txt* file is in the root folder of the web site after deployment.</span></span>

## <a name="summary"></a><span data-ttu-id="7ccef-144">總結</span><span class="sxs-lookup"><span data-stu-id="7ccef-144">Summary</span></span>

<span data-ttu-id="7ccef-145">如此即完成這一系列的 ASP.NET web 應用程式部署到協力廠商主機服務提供者上的教學課程。</span><span class="sxs-lookup"><span data-stu-id="7ccef-145">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="7ccef-146">如需任何這些教學課程涵蓋之主題的詳細資訊，請參閱[ASP.NET 部署內容地圖](https://go.microsoft.com/fwlink/p/?LinkId=282413)。</span><span class="sxs-lookup"><span data-stu-id="7ccef-146">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://go.microsoft.com/fwlink/p/?LinkId=282413).</span></span>

## <a name="more-information"></a><span data-ttu-id="7ccef-147">詳細資訊</span><span class="sxs-lookup"><span data-stu-id="7ccef-147">More information</span></span>

<span data-ttu-id="7ccef-148">如果您知道如何使用 MSBuild 檔案，您可以撰寫程式碼來自動化許多其他部署工作*.pubxml*檔案 （適用於特定設定檔的工作） 或專案*。 wpp.targets*檔案 （如工作適用於所有設定檔）。</span><span class="sxs-lookup"><span data-stu-id="7ccef-148">If you know how to work with MSBuild files, you can automate many other deployment tasks by writing code in *.pubxml* files (for profile-specific tasks) or the project *.wpp.targets* file (for tasks that apply to all profiles).</span></span> <span data-ttu-id="7ccef-149">如需詳細資訊*.pubxml*和*。 wpp.targets*檔，請參閱[如何： 編輯發行設定檔 (.pubxml) 檔中的部署設定而。 wpp.targets Visual Studio Web 中的檔案專案](https://msdn.microsoft.com/en-us/library/ff398069)。</span><span class="sxs-lookup"><span data-stu-id="7ccef-149">For more information about *.pubxml* and *.wpp.targets* files, see [How to: Edit Deployment Settings in Publish Profile (.pubxml) Files and the .wpp.targets File in Visual Studio Web Projects](https://msdn.microsoft.com/en-us/library/ff398069).</span></span> <span data-ttu-id="7ccef-150">MSBuild 的程式碼的基本簡介，請參閱**剖析專案檔**中[企業部署序列： 了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。</span><span class="sxs-lookup"><span data-stu-id="7ccef-150">For a basic introduction to MSBuild code, see **The Anatomy of a Project File** in [Enterprise Deployment Series: Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span> <span data-ttu-id="7ccef-151">若要了解如何使用 MSBuild 檔案，為您自己的案例中執行工作，請參閱此活頁簿：[內 Microsoft Build Engine： 使用 MSBuild 和 Team Foundation Build](http://msbuildbook.com) Sayed Ibraham Hashimi 和 William Bartholomew。</span><span class="sxs-lookup"><span data-stu-id="7ccef-151">To learn how to work with MSBuild files to perform tasks for your own scenarios, see this book: [Inside the Microsoft Build Engine: Using MSBuild and Team Foundation Build](http://msbuildbook.com) by Sayed Ibraham Hashimi and William Bartholomew.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="7ccef-152">謝誌</span><span class="sxs-lookup"><span data-stu-id="7ccef-152">Acknowledgements</span></span>

<span data-ttu-id="7ccef-153">我想要感謝下列重大貢獻內容本教學課程系列的人：</span><span class="sxs-lookup"><span data-stu-id="7ccef-153">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="7ccef-154">Alberto Poblacion、 MVP &amp; mct 規範、 西班牙</span><span class="sxs-lookup"><span data-stu-id="7ccef-154">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.microsoft.com/en-us/mvp/Alberto%20Poblacion%20Bolano-36772)
- <span data-ttu-id="7ccef-155">Jarod Ferguson，資料平台開發 MVP，美國</span><span class="sxs-lookup"><span data-stu-id="7ccef-155">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="7ccef-156">惡劣 Mittal、 Microsoft</span><span class="sxs-lookup"><span data-stu-id="7ccef-156">Harsh Mittal, Microsoft</span></span>
- <span data-ttu-id="7ccef-157">[Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))</span><span class="sxs-lookup"><span data-stu-id="7ccef-157">[Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [@jongalloway](http://twitter.com/jongalloway))</span></span>
- [<span data-ttu-id="7ccef-158">Kristina Olson Microsoft</span><span class="sxs-lookup"><span data-stu-id="7ccef-158">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="7ccef-159">Mike 教宗 Microsoft</span><span class="sxs-lookup"><span data-stu-id="7ccef-159">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="7ccef-160">Mohit Srivastava Microsoft</span><span class="sxs-lookup"><span data-stu-id="7ccef-160">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="7ccef-161">Raffaele Rialdi （義大利）</span><span class="sxs-lookup"><span data-stu-id="7ccef-161">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="7ccef-162">Rick Anderson Microsoft</span><span class="sxs-lookup"><span data-stu-id="7ccef-162">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="7ccef-163">[Sayed Hashimi、 Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="7ccef-163">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="7ccef-164">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="7ccef-164">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="7ccef-165">[Scott 獵人、 Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="7ccef-165">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="7ccef-166">Srđan Božović、 塞爾維亞</span><span class="sxs-lookup"><span data-stu-id="7ccef-166">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="7ccef-167">[Vishal Joshi、 Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="7ccef-167">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="7ccef-168">[上一頁](command-line-deployment.md)
[下一頁](troubleshooting.md)</span><span class="sxs-lookup"><span data-stu-id="7ccef-168">[Previous](command-line-deployment.md)
[Next](troubleshooting.md)</span></span>
