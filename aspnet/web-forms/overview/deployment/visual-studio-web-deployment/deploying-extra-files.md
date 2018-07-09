---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 使用 Visual Studio 的 ASP.NET Web 部署： 部署額外的檔案 |Microsoft Docs
author: tdykstra
description: 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用...
ms.author: aspnetcontent
ms.date: 03/23/2015
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: 609c0be968e8f38d7be6e5f5c96a569a9a35c2eb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820846"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a><span data-ttu-id="cd2f0-103">使用 Visual Studio 的 ASP.NET Web 部署： 部署額外的檔案</span><span class="sxs-lookup"><span data-stu-id="cd2f0-103">ASP.NET Web Deployment using Visual Studio: Deploying Extra Files</span></span>
====================
<span data-ttu-id="cd2f0-104">藉由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="cd2f0-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="cd2f0-105">下載入門專案</span><span class="sxs-lookup"><span data-stu-id="cd2f0-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="cd2f0-106">本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="cd2f0-107">這個系列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="cd2f0-108">總覽</span><span class="sxs-lookup"><span data-stu-id="cd2f0-108">Overview</span></span>

<span data-ttu-id="cd2f0-109">本教學課程會示範如何擴充 Visual Studio web 發行管線，以在部署期間執行額外的工作。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-109">This tutorial shows how to extend the Visual Studio web publish pipeline to do an additional task during deployment.</span></span> <span data-ttu-id="cd2f0-110">工作是將複製至目的地網站的 [專案] 資料夾中所沒有的額外檔案。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-110">The task is to copy extra files that are not in the project folder to the destination web site.</span></span>

<span data-ttu-id="cd2f0-111">在本教學課程中，您將會複製一個額外的檔案： *robots.txt*。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-111">For this tutorial you'll copy one extra file: *robots.txt*.</span></span> <span data-ttu-id="cd2f0-112">您想要將此檔案部署至預備環境，而非生產環境。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-112">You want to deploy this file to staging but not to production.</span></span> <span data-ttu-id="cd2f0-113">在 [到生產環境部署](deploying-to-production.md)教學課程中，您加入專案中的這個檔案，並設定生產發行設定檔，以將其排除。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-113">In [the Deploying to Production](deploying-to-production.md) tutorial, you added this file to the project and configured the Production publish profile to exclude it.</span></span> <span data-ttu-id="cd2f0-114">在本教學課程中，您會看到使用替代方法來處理這種情況，其中一個是適用於任何您想要部署，但不想要包含在專案中的檔案。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-114">In this tutorial you'll see an alternative method to handle this situation, one that will be useful for any files that you want to deploy but don't want to include in the project.</span></span>

## <a name="move-the-robotstxt-file"></a><span data-ttu-id="cd2f0-115">移動 robots.txt 檔案</span><span class="sxs-lookup"><span data-stu-id="cd2f0-115">Move the robots.txt file</span></span>

<span data-ttu-id="cd2f0-116">若要準備不同的處理方法*robots.txt*，在本教學課程的這一節中您將檔案移至資料夾未包含在專案中，您刪除*robots.txt*從預備環境環境。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-116">To prepare for a different method of handling *robots.txt*, in this section of the tutorial you move the file to a folder that is not included in the project, and you delete *robots.txt* from the staging environment.</span></span> <span data-ttu-id="cd2f0-117">必須從預備環境，讓您可以驗證您的檔案部署到該環境的新方法正常運作中刪除檔案。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-117">It is necessary to delete the file from staging so that you can verify that your new method of deploying the file to that environment is working correctly.</span></span>

1. <span data-ttu-id="cd2f0-118">在 [**方案總管] 中**，以滑鼠右鍵按一下*robots.txt*檔案，然後按一下**從專案移除**。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-118">In **Solution Explorer**, right-click the *robots.txt* file and click **Exclude From Project**.</span></span>
2. <span data-ttu-id="cd2f0-119">使用 Windows 檔案總管，在方案資料夾中建立新的資料夾，並將它命名*ExtraFiles*。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-119">Using Windows File Explorer, create a new folder in the solution folder and name it *ExtraFiles*.</span></span>
3. <span data-ttu-id="cd2f0-120">移動*robots.txt*檔案*ContosoUniversity*專案資料夾*ExtraFiles*資料夾。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-120">Move the *robots.txt* file from the *ContosoUniversity* project folder to the *ExtraFiles* folder.</span></span>

    ![ExtraFiles 資料夾](deploying-extra-files/_static/image1.png)
4. <span data-ttu-id="cd2f0-122">使用您的 FTP 工具，刪除*robots.txt*從預備網站的檔案。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-122">Using your FTP tool, delete the *robots.txt* file from the staging web site.</span></span>

    <span data-ttu-id="cd2f0-123">或者，您可以選取**移除目的地上的其他檔案**下方**檔案發行選項**上**設定**的預備發行設定檔 索引標籤和重新發佈至預備環境。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-123">As an alternative, you can select **Remove additional files at destination** under **File Publish Options** on the **Settings** tab of the Staging publish profile, and republish to staging.</span></span>

## <a name="update-the-publish-profile-file"></a><span data-ttu-id="cd2f0-124">更新發行設定檔</span><span class="sxs-lookup"><span data-stu-id="cd2f0-124">Update the publish profile file</span></span>

<span data-ttu-id="cd2f0-125">您只需要*robots.txt*在預備環境，以便在預備唯一您需要更新才能將其部署的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-125">You only need *robots.txt* in staging, so the only publish profile you need to update in order to deploy it is Staging.</span></span>

1. <span data-ttu-id="cd2f0-126">在 Visual Studio 中開啟*Staging.pubxml*。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-126">In Visual Studio, open *Staging.pubxml*.</span></span>
2. <span data-ttu-id="cd2f0-127">結尾的檔案，在關閉前`</Project>`標記中加入下列標記：</span><span class="sxs-lookup"><span data-stu-id="cd2f0-127">At the end of the file, before the closing `</Project>` tag, add the following markup:</span></span>

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    <span data-ttu-id="cd2f0-128">此程式碼會建立新*目標*，將會收集其他一起部署的檔案。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-128">This code creates a new *target* that will collect additional files to be deployed.</span></span> <span data-ttu-id="cd2f0-129">目標組成一或更多的工作將執行 MSBuild，根據您指定的條件。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-129">A target is composed of one or more tasks that MSBuild will execute based on conditions you specify.</span></span>

    <span data-ttu-id="cd2f0-130">`Include`屬性會指定在其中尋找檔案的資料夾是*ExtraFiles*，位於專案資料夾相同層級。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-130">The `Include` attribute specifies that the folder in which to find the files is *ExtraFiles*, located at the same level as the project folder.</span></span> <span data-ttu-id="cd2f0-131">MSBuild 會從該資料夾，然後以遞迴方式從 （double 的星號會指定遞迴子資料夾） 的任何子資料夾中收集的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-131">MSBuild will collect all files from that folder and recursively from any subfolders (the double asterisk specifies recursive subfolders).</span></span> <span data-ttu-id="cd2f0-132">此程式碼您可以將放入多個檔案和檔案內的子資料夾*ExtraFiles*資料夾中，與所有部署。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-132">With this code you could put multiple files, and files in subfolders inside the *ExtraFiles* folder, and all will be deployed.</span></span>

    <span data-ttu-id="cd2f0-133">`DestinationRelativePath`項目會指定，檔案和資料夾，應該會複製到目的地網站上，在相同的檔案及資料夾結構的根資料夾中找到*ExtraFiles*資料夾。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-133">The `DestinationRelativePath` element specifies that the folders and files should be copied to the root folder of the destination web site, in the same file and folder structure as they are found in the *ExtraFiles* folder.</span></span> <span data-ttu-id="cd2f0-134">如果您想要複製*ExtraFiles*本身的資料夾`DestinationRelativePath`的值會是*ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-134">If you wanted to copy the *ExtraFiles* folder itself, the `DestinationRelativePath` value would be *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.</span></span>
3. <span data-ttu-id="cd2f0-135">結尾的檔案，在關閉前`</Project>`標記中加入下列標記會指定何時要執行新的目標。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-135">At the end of the file, before the closing `</Project>` tag, add the following markup that specifies when to execute the new target.</span></span>

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    <span data-ttu-id="cd2f0-136">此程式碼會使新`CustomCollectFiles`每當執行目標，將檔案複製到目的地資料夾時要執行的目標。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-136">This code causes the new `CustomCollectFiles` target to be executed whenever the target that copies files to the destination folder is executed.</span></span> <span data-ttu-id="cd2f0-137">有個別的目標，如發佈與部署套件建立，以及萬一您決定要使用的部署封裝，而不是發行部署兩個目標中插入新的目標。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-137">There is a separate target for publish versus deployment package creation, and the new target is injected in both targets in case you decide to deploy by using a deployment package instead of publishing.</span></span>

    <span data-ttu-id="cd2f0-138">*.Pubxml*檔現在看起來如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="cd2f0-138">The *.pubxml* file now looks like the following example:</span></span>

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. <span data-ttu-id="cd2f0-139">儲存並關閉*Staging.pubxml*檔案。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-139">Save and close the *Staging.pubxml* file.</span></span>

## <a name="publish-to-staging"></a><span data-ttu-id="cd2f0-140">發行至預備環境</span><span class="sxs-lookup"><span data-stu-id="cd2f0-140">Publish to staging</span></span>

<span data-ttu-id="cd2f0-141">使用單鍵發佈或命令列中，使用暫存設定檔來發佈應用程式。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-141">Using one-click publish or the command line, publish the application by using the Staging profile.</span></span>

<span data-ttu-id="cd2f0-142">如果您使用單鍵發行，您可以在 確認**Preview**視窗， *robots.txt*會被複製。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-142">If you use one-click publish, you can verify in the **Preview** window that *robots.txt* will be copied.</span></span> <span data-ttu-id="cd2f0-143">否則，請使用 FTP 工具來確認*robots.txt*檔案是在部署後網站的根資料夾中。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-143">Otherwise, use your FTP tool to verify that the *robots.txt* file is in the root folder of the web site after deployment.</span></span>

## <a name="summary"></a><span data-ttu-id="cd2f0-144">總結</span><span class="sxs-lookup"><span data-stu-id="cd2f0-144">Summary</span></span>

<span data-ttu-id="cd2f0-145">如此即完成本系列的教學課程將部署到協力廠商裝載提供者的 ASP.NET web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-145">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="cd2f0-146">如需任何這些教學課程所涵蓋的主題的詳細資訊，請參閱[ASP.NET 部署內容對應](https://go.microsoft.com/fwlink/p/?LinkId=282413)。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-146">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://go.microsoft.com/fwlink/p/?LinkId=282413).</span></span>

## <a name="more-information"></a><span data-ttu-id="cd2f0-147">詳細資訊</span><span class="sxs-lookup"><span data-stu-id="cd2f0-147">More information</span></span>

<span data-ttu-id="cd2f0-148">如果您知道如何使用 MSBuild 檔案，您可以自動化許多其他部署工作中撰寫程式碼 *.pubxml* （適用於設定檔的特定工作） 的檔案或專案 *.wpp.targets*檔案 （如工作適用於所有設定檔）。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-148">If you know how to work with MSBuild files, you can automate many other deployment tasks by writing code in *.pubxml* files (for profile-specific tasks) or the project *.wpp.targets* file (for tasks that apply to all profiles).</span></span> <span data-ttu-id="cd2f0-149">如需詳細資訊 *.pubxml*並 *.wpp.targets*檔案，請參閱[How to： 編輯發行設定檔 (.pubxml) 檔中的部署設定而.wpp.targets 檔案在 Visual Studio Web專案](https://msdn.microsoft.com/library/ff398069)。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-149">For more information about *.pubxml* and *.wpp.targets* files, see [How to: Edit Deployment Settings in Publish Profile (.pubxml) Files and the .wpp.targets File in Visual Studio Web Projects](https://msdn.microsoft.com/library/ff398069).</span></span> <span data-ttu-id="cd2f0-150">MSBuild 的程式碼的基本簡介，請參閱**專案檔的剖析**中[企業部署系列： 了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-150">For a basic introduction to MSBuild code, see **The Anatomy of a Project File** in [Enterprise Deployment Series: Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span> <span data-ttu-id="cd2f0-151">若要了解如何使用您自己的案例中執行工作的 MSBuild 檔案，請參閱本書：[在 Microsoft Build Engine： 使用 MSBuild 和 Team Foundation Build](http://msbuildbook.com) Sayed Ibraham Hashimi 和 William bartholomew< /。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-151">To learn how to work with MSBuild files to perform tasks for your own scenarios, see this book: [Inside the Microsoft Build Engine: Using MSBuild and Team Foundation Build](http://msbuildbook.com) by Sayed Ibraham Hashimi and William Bartholomew.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="cd2f0-152">謝誌</span><span class="sxs-lookup"><span data-stu-id="cd2f0-152">Acknowledgements</span></span>

<span data-ttu-id="cd2f0-153">我想要感謝下列人士提出重大貢獻的內容，本教學課程系列：</span><span class="sxs-lookup"><span data-stu-id="cd2f0-153">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="cd2f0-154">Alberto Poblacion、 MVP &amp; MCT，西班牙</span><span class="sxs-lookup"><span data-stu-id="cd2f0-154">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- <span data-ttu-id="cd2f0-155">Jarod Ferguson，資料平台開發 MVP、 美國。</span><span class="sxs-lookup"><span data-stu-id="cd2f0-155">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="cd2f0-156">Harsh Mittal，Microsoft</span><span class="sxs-lookup"><span data-stu-id="cd2f0-156">Harsh Mittal, Microsoft</span></span>
- <span data-ttu-id="cd2f0-157">[Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))</span><span class="sxs-lookup"><span data-stu-id="cd2f0-157">[Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [@jongalloway](http://twitter.com/jongalloway))</span></span>
- [<span data-ttu-id="cd2f0-158">Kristina Olson Microsoft</span><span class="sxs-lookup"><span data-stu-id="cd2f0-158">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="cd2f0-159">Mike 主教 Microsoft</span><span class="sxs-lookup"><span data-stu-id="cd2f0-159">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="cd2f0-160">Mohit Srivastava Microsoft</span><span class="sxs-lookup"><span data-stu-id="cd2f0-160">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="cd2f0-161">Raffaele Rialdi 義大利</span><span class="sxs-lookup"><span data-stu-id="cd2f0-161">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="cd2f0-162">Rick Anderson Microsoft</span><span class="sxs-lookup"><span data-stu-id="cd2f0-162">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="cd2f0-163">[Sayed Hashimi，Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="cd2f0-163">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="cd2f0-164">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="cd2f0-164">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="cd2f0-165">[Scott Hunter、 Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="cd2f0-165">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="cd2f0-166">Srđan Božović，塞爾維亞</span><span class="sxs-lookup"><span data-stu-id="cd2f0-166">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="cd2f0-167">[Vishal Joshi、 Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="cd2f0-167">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cd2f0-168">[上一頁](command-line-deployment.md)
> [下一頁](troubleshooting.md)</span><span class="sxs-lookup"><span data-stu-id="cd2f0-168">[Previous](command-line-deployment.md)
[Next](troubleshooting.md)</span></span>
