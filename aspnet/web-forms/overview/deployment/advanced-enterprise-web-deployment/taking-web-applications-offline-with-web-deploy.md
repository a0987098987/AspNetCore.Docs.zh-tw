---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: 函式使用 Web 應用程式離線 Web 部署 |Microsoft 文件
author: jrjlee
description: 本主題說明如何取得 web 應用程式離線期間自動化部署使用網際網路資訊服務 (IIS) Web 高於...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: 511201dc5646340b21023430fa319417f2b53ae2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="taking-web-applications-offline-with-web-deploy"></a><span data-ttu-id="608f8-103">函式使用 Web 應用程式離線 Web 部署</span><span class="sxs-lookup"><span data-stu-id="608f8-103">Taking Web Applications Offline with Web Deploy</span></span>
====================
<span data-ttu-id="608f8-104">由[Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="608f8-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="608f8-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="608f8-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="608f8-106">本主題描述如何取得 web 應用程式離線以自動化部署使用網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 的持續時間。</span><span class="sxs-lookup"><span data-stu-id="608f8-106">This topic describes how to take a web application offline for the duration of an automated deployment using the Internet Information Services (IIS) Web Deployment Tool (Web Deploy).</span></span> <span data-ttu-id="608f8-107">瀏覽至 web 應用程式的使用者重新導向至*應用程式\_offline.htm*檔案部署完成之前。</span><span class="sxs-lookup"><span data-stu-id="608f8-107">Users who browse to the web application are redirected to an *App\_offline.htm* file until the deployment is complete.</span></span>


<span data-ttu-id="608f8-108">本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案&#x2014;[連絡人管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來表示實際層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 通訊的 web 應用程式Foundation (WCF) 服務與資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="608f8-108">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="608f8-109">這些教學課程的核心的部署方法為基礎所說明的分割專案檔案方法[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置流程控制的兩個專案檔&#x2014;一個包含建置適用於每個目的地環境中和包含特定環境的建置和部署設定的指示。</span><span class="sxs-lookup"><span data-stu-id="608f8-109">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="608f8-110">在建置時，環境特定專案檔就會合併環境無從驗證專案檔來形成一組完整組建指示。</span><span class="sxs-lookup"><span data-stu-id="608f8-110">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="608f8-111">工作概觀</span><span class="sxs-lookup"><span data-stu-id="608f8-111">Task Overview</span></span>

<span data-ttu-id="608f8-112">在許多案例中，您會想讓 web 應用程式離線，當您變更相關的元件，例如資料庫或 web 服務。</span><span class="sxs-lookup"><span data-stu-id="608f8-112">In a lot of scenarios, you'll want to take a web application offline while you make changes to related components, like databases or web services.</span></span> <span data-ttu-id="608f8-113">一般而言，IIS 和 ASP.NET，在您完成這項作業放入名為*應用程式\_offline.htm* IIS 網站或 web 應用程式的根資料夾中。</span><span class="sxs-lookup"><span data-stu-id="608f8-113">Typically, in IIS and ASP.NET, you accomplish this by placing a file named *App\_offline.htm* in the root folder of the IIS website or web application.</span></span> <span data-ttu-id="608f8-114">*應用程式\_offline.htm*檔案是標準 HTML 檔案，而且通常會包含一個簡單的訊息，告知使用者的站台因維護而暫時無法使用。</span><span class="sxs-lookup"><span data-stu-id="608f8-114">The *App\_offline.htm* file is a standard HTML file and will usually contain a simple message advising the user that the site is temporarily unavailable due to maintenance.</span></span> <span data-ttu-id="608f8-115">雖然*應用程式\_offline.htm*檔案存在於網站的根資料夾中，IIS 會自動將任何要求導向至檔案。</span><span class="sxs-lookup"><span data-stu-id="608f8-115">While the *App\_offline.htm* file exists in the root folder of the website, IIS will automatically redirect any requests to the file.</span></span> <span data-ttu-id="608f8-116">當您完成進行更新時，請移除*應用程式\_offline.htm*檔案和網站會繼續如往常般處理要求。</span><span class="sxs-lookup"><span data-stu-id="608f8-116">When you've finished making updates, you remove the *App\_offline.htm* file and the website resumes serving requests as usual.</span></span>

<span data-ttu-id="608f8-117">當您使用 Web Deploy 來執行自動化或單一步驟部署至目標環境時，您可能想要合併新增和移除*應用程式\_offline.htm*檔案到您的部署程序。</span><span class="sxs-lookup"><span data-stu-id="608f8-117">When you use Web Deploy to perform automated or single-step deployments to a target environment, you may want to incorporate adding and removing the *App\_offline.htm* file into your deployment process.</span></span> <span data-ttu-id="608f8-118">若要這樣做，您必須完成下列高階工作：</span><span class="sxs-lookup"><span data-stu-id="608f8-118">To do this, you'll need to complete these high-level tasks:</span></span>

- <span data-ttu-id="608f8-119">Microsoft Build Engine (MSBuild) 專案檔中您用來控制部署程序，建立 MSBuild 目標複製*應用程式\_offline.htm*檔案到目的地伺服器之前的任何部署工作開始。</span><span class="sxs-lookup"><span data-stu-id="608f8-119">In the Microsoft Build Engine (MSBuild) project file that you use to control the deployment process, create an MSBuild target that copies an *App\_offline.htm* file to the destination server before any deployment tasks begin.</span></span>
- <span data-ttu-id="608f8-120">加入另一個移除的 MSBuild 目標*應用程式\_offline.htm*從目的地伺服器部署的所有工作完成時的檔案。</span><span class="sxs-lookup"><span data-stu-id="608f8-120">Add another MSBuild target that removes the *App\_offline.htm* file from the destination server when all deployment tasks are complete.</span></span>
- <span data-ttu-id="608f8-121">在 web 應用程式專案中，建立 *。 wpp.targets*檔案，可確保*應用程式\_offline.htm* Web Deploy 叫用時，檔案會加入至部署套件。</span><span class="sxs-lookup"><span data-stu-id="608f8-121">In the web application project, create a *.wpp.targets* file that ensures that an *App\_offline.htm* file is added to the deployment package when Web Deploy is invoked.</span></span>

<span data-ttu-id="608f8-122">本主題將說明如何執行這些程序。</span><span class="sxs-lookup"><span data-stu-id="608f8-122">This topic will show you how to perform these procedures.</span></span> <span data-ttu-id="608f8-123">工作與本主題中的逐步解說假設，您已經建立的方案包含至少一個 web 應用程式專案，而且您使用自訂專案檔中所述，控制部署程序[中的 Web 部署企業](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)。</span><span class="sxs-lookup"><span data-stu-id="608f8-123">The tasks and walkthroughs in this topic assume that you've already created a solution that contains at least one web application project, and that you use a custom project file to control the deployment process as described in [Web Deployment in the Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md).</span></span> <span data-ttu-id="608f8-124">或者，您可以使用[連絡人管理員](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)範例要遵循本主題中的範例方案。</span><span class="sxs-lookup"><span data-stu-id="608f8-124">Alternatively, you can use the [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) sample solution to follow the examples in the topic.</span></span>

## <a name="adding-an-appoffline-file-to-a-web-application-project"></a><span data-ttu-id="608f8-125">加入應用程式\_加入 Web 應用程式專案的離線檔案</span><span class="sxs-lookup"><span data-stu-id="608f8-125">Adding an App\_Offline File to a Web Application Project</span></span>

<span data-ttu-id="608f8-126">您需要完成第一個工作是加入*應用程式\_離線*您 web 應用程式專案的檔案：</span><span class="sxs-lookup"><span data-stu-id="608f8-126">The first task you need to complete is to add an *App\_offline* file to your web application project:</span></span>

- <span data-ttu-id="608f8-127">若要防止檔案干擾開發程序 （您不想要永久離線應用程式），您應該呼叫它的項目以外*應用程式\_offline.htm*。</span><span class="sxs-lookup"><span data-stu-id="608f8-127">To prevent the file from interfering with the development process (you don't want your application to be permanently offline), you should call it something other than *App\_offline.htm*.</span></span> <span data-ttu-id="608f8-128">例如，您可以命名為檔案*應用程式\_離線 template.htm*。</span><span class="sxs-lookup"><span data-stu-id="608f8-128">For example, you could name the file *App\_offline-template.htm*.</span></span>
- <span data-ttu-id="608f8-129">若要防止檔案被部署為的是，您應該將建置動作設定為**無**。</span><span class="sxs-lookup"><span data-stu-id="608f8-129">To prevent the file from being deployed as-is, you should set the build action to **None**.</span></span>

<span data-ttu-id="608f8-130">**若要新增應用程式\_加入 web 應用程式專案的離線檔案**</span><span class="sxs-lookup"><span data-stu-id="608f8-130">**To add an App\_offline file to a web application project**</span></span>

1. <span data-ttu-id="608f8-131">Visual Studio 2010 中開啟您的方案。</span><span class="sxs-lookup"><span data-stu-id="608f8-131">Open your solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="608f8-132">在**方案總管] 中**視窗中，以滑鼠右鍵按一下您的 web 應用程式專案，指向**新增**，然後按一下 [**新項目**。</span><span class="sxs-lookup"><span data-stu-id="608f8-132">In the **Solution Explorer** window, right-click your web application project, point to **Add**, and then click **New Item**.</span></span>
3. <span data-ttu-id="608f8-133">在**加入新項目**對話方塊中，選取**HTML 網頁**。</span><span class="sxs-lookup"><span data-stu-id="608f8-133">In the **Add New Item** dialog box, select **HTML Page**.</span></span>
4. <span data-ttu-id="608f8-134">在**名稱**方塊中，輸入**應用程式\_離線 template.htm**，然後按一下 **新增**。</span><span class="sxs-lookup"><span data-stu-id="608f8-134">In the **Name** box, type **App\_offline-template.htm**, and then click **Add**.</span></span>

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. <span data-ttu-id="608f8-135">加入一些簡單的 HTML，通知使用者應用程式，就無法使用，並儲存檔案。</span><span class="sxs-lookup"><span data-stu-id="608f8-135">Add some simple HTML to inform users that the application is unavailable, and then save the file.</span></span> <span data-ttu-id="608f8-136">不包含任何伺服器端標記 (例如，前面會加上任何標記"asp:")。</span><span class="sxs-lookup"><span data-stu-id="608f8-136">Do not include any server-side tags (for example, any tags that are prefixed with "asp:").</span></span> 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. <span data-ttu-id="608f8-137">在**方案總管 中**視窗中，以滑鼠右鍵按一下新的檔案，然後**屬性**。</span><span class="sxs-lookup"><span data-stu-id="608f8-137">In the **Solution Explorer** window, right-click the new file, and then click **Properties**.</span></span>
7. <span data-ttu-id="608f8-138">在**屬性**視窗，請在**建置動作**列中選取**無**。</span><span class="sxs-lookup"><span data-stu-id="608f8-138">In the **Properties** window, in the **Build Action** row, select **None**.</span></span>

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-appoffline-file"></a><span data-ttu-id="608f8-139">部署和刪除應用程式\_離線檔案</span><span class="sxs-lookup"><span data-stu-id="608f8-139">Deploying and Deleting an App\_Offline File</span></span>

<span data-ttu-id="608f8-140">下一個步驟是修改您部署邏輯，並將檔案複製到目的地伺服器，部署程序的開頭和結尾移除它。</span><span class="sxs-lookup"><span data-stu-id="608f8-140">The next step is to modify your deployment logic to copy the file to the destination server at the start of the deployment process and remove it at the end.</span></span>

> [!NOTE]
> <span data-ttu-id="608f8-141">下一個程序假設您要自訂的 MSBuild 專案檔使用來控制您的部署程序，如下所示[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。</span><span class="sxs-lookup"><span data-stu-id="608f8-141">The next procedure assumes that you're using a custom MSBuild project file to control your deployment process, as described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md).</span></span> <span data-ttu-id="608f8-142">如果您要部署直接從 Visual Studio，您必須使用不同的方式。</span><span class="sxs-lookup"><span data-stu-id="608f8-142">If you're deploying direct from Visual Studio, you'll need to use a different approach.</span></span> <span data-ttu-id="608f8-143">Sayed Ibrahim Hashimi 描述中的其中一個這類方法[如何取得您 Web 應用程式離線期間發佈](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx)。</span><span class="sxs-lookup"><span data-stu-id="608f8-143">Sayed Ibrahim Hashimi describes one such approach in [How to Take Your Web App Offline During Publishing](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx).</span></span>


<span data-ttu-id="608f8-144">若要部署*應用程式\_離線*檔案到目的地的 IIS 網站，您需要叫用使用 MSDeploy.exe [Web Deploy **contentPath**提供者](https://technet.microsoft.com/library/dd569034(WS.10).aspx)。</span><span class="sxs-lookup"><span data-stu-id="608f8-144">To deploy an *App\_offline* file to a destination IIS website, you need to invoke MSDeploy.exe using the [Web Deploy **contentPath** provider](https://technet.microsoft.com/library/dd569034(WS.10).aspx).</span></span> <span data-ttu-id="608f8-145">**ContentPath**提供者支援的實體目錄路徑和 IIS 網站或應用程式路徑，使其同步處理 Visual Studio 專案資料夾與 IIS web 應用程式之間的檔案的理想選擇。</span><span class="sxs-lookup"><span data-stu-id="608f8-145">The **contentPath** provider supports both physical directory paths and IIS website or application paths, which makes it the ideal choice for synchronizing a file between a Visual Studio project folder and an IIS web application.</span></span> <span data-ttu-id="608f8-146">若要部署的檔案，MSDeploy 命令應該會與以下相似：</span><span class="sxs-lookup"><span data-stu-id="608f8-146">To deploy the file, your MSDeploy command should resemble this:</span></span>


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]


<span data-ttu-id="608f8-147">若要移除檔案，從目的地網站的部署程序結尾，MSDeploy 命令應該會與以下相似：</span><span class="sxs-lookup"><span data-stu-id="608f8-147">To remove the file from the destination site at the end of the deployment process, your MSDeploy command should resemble this:</span></span>


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]


<span data-ttu-id="608f8-148">若要自動化這些命令做為建置和部署處理程序的一部分，您需要將它們整合到自訂的 MSBuild 專案檔。</span><span class="sxs-lookup"><span data-stu-id="608f8-148">To automate these commands as part of a build and deployment process, you need to integrate them into your custom MSBuild project file.</span></span> <span data-ttu-id="608f8-149">下一個程序描述如何執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="608f8-149">The next procedure describes how to do this.</span></span>

<span data-ttu-id="608f8-150">**部署並刪除應用程式\_離線檔案**</span><span class="sxs-lookup"><span data-stu-id="608f8-150">**To deploy and delete an App\_offline file**</span></span>

1. <span data-ttu-id="608f8-151">在 Visual Studio 2010 中，開啟 MSBuild 專案檔可控制您的部署程序。</span><span class="sxs-lookup"><span data-stu-id="608f8-151">In Visual Studio 2010, open the MSBuild project file that controls your deployment process.</span></span> <span data-ttu-id="608f8-152">在[連絡人管理員](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)範例方案中，這是*Publish.proj*檔案。</span><span class="sxs-lookup"><span data-stu-id="608f8-152">In the [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) sample solution, this is the *Publish.proj* file.</span></span>
2. <span data-ttu-id="608f8-153">在根**專案**項目，建立新**PropertyGroup**項目儲存為變數*應用程式\_離線*部署：</span><span class="sxs-lookup"><span data-stu-id="608f8-153">In the root **Project** element, create a new **PropertyGroup** element to store variables for the *App\_offline* deployment:</span></span>

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. <span data-ttu-id="608f8-154">**SourceRoot**中其他位置定義屬性*Publish.proj*檔案。</span><span class="sxs-lookup"><span data-stu-id="608f8-154">The **SourceRoot** property is defined elsewhere in the *Publish.proj* file.</span></span> <span data-ttu-id="608f8-155">它會指出相對於目前路徑的來源內容的根資料夾的位置&#x2014;換句話說，相對於位置*Publish.proj*檔案。</span><span class="sxs-lookup"><span data-stu-id="608f8-155">It indicates the location of the root folder for the source content relative to the current path&#x2014;in other words, relative to the location of the *Publish.proj* file.</span></span>
4. <span data-ttu-id="608f8-156">**ContentPath**提供者將不接受相對檔案路徑，因此您需要取得原始程式檔的絕對路徑，才能部署它。</span><span class="sxs-lookup"><span data-stu-id="608f8-156">The **contentPath** provider will not accept relative file paths, so you need to get an absolute path to your source file before you can deploy it.</span></span> <span data-ttu-id="608f8-157">您可以使用[ConvertToAbsolutePath](https://msdn.microsoft.com/library/bb882668.aspx)工作來執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="608f8-157">You can use the [ConvertToAbsolutePath](https://msdn.microsoft.com/library/bb882668.aspx) task to do this.</span></span>
5. <span data-ttu-id="608f8-158">加入新**目標**名**GetAppOfflineAbsolutePath**。</span><span class="sxs-lookup"><span data-stu-id="608f8-158">Add a new **Target** element named **GetAppOfflineAbsolutePath**.</span></span> <span data-ttu-id="608f8-159">在此目標，使用**ConvertToAbsolutePath**工作來取得的絕對路徑*應用程式\_離線範本*專案資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="608f8-159">Within this target, use the **ConvertToAbsolutePath** task to get an absolute path to the *App\_offline-template* file in your project folder.</span></span>

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. <span data-ttu-id="608f8-160">這個目標會採用的相對路徑*應用程式\_離線範本*專案資料夾中的檔案，並將它儲存到新的屬性做為絕對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="608f8-160">This target takes the relative path to the *App\_offline-template* file in your project folder and saves it to a new property as an absolute file path.</span></span> <span data-ttu-id="608f8-161">**之 BeforeTargets**屬性會指定您想要這個目標之前執行**DeployAppOffline**目標，您會在下一個步驟建立。</span><span class="sxs-lookup"><span data-stu-id="608f8-161">The **BeforeTargets** attribute specifies that you want this target to execute before the **DeployAppOffline** target, which you'll create in the next step.</span></span>
7. <span data-ttu-id="608f8-162">加入名為新目標**DeployAppOffline**。</span><span class="sxs-lookup"><span data-stu-id="608f8-162">Add a new target named **DeployAppOffline**.</span></span> <span data-ttu-id="608f8-163">這個目標，內叫用 MSDeploy.exe 命令部署您*應用程式\_離線*目的地 web 伺服器的檔案。</span><span class="sxs-lookup"><span data-stu-id="608f8-163">Within this target, invoke the MSDeploy.exe command that deploys your *App\_offline* file to the destination web server.</span></span>

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. <span data-ttu-id="608f8-164">在此範例中， **ContactManagerIisPath**專案檔中其他地方定義屬性。</span><span class="sxs-lookup"><span data-stu-id="608f8-164">In this example, the **ContactManagerIisPath** property is defined elsewhere in the project file.</span></span> <span data-ttu-id="608f8-165">這是只需 IIS 應用程式路徑，在表單中的 *[IIS 網站名稱] / [應用程式名稱]*。</span><span class="sxs-lookup"><span data-stu-id="608f8-165">This is simply an IIS application path, in the form *[IIS Website Name]/[Application Name]*.</span></span> <span data-ttu-id="608f8-166">包含目標中的條件，可讓使用者切換*應用程式\_離線*部署開啟或關閉透過變更屬性值，或提供命令列參數。</span><span class="sxs-lookup"><span data-stu-id="608f8-166">Including a condition in the target enables users to switch the *App\_offline* deployment on or off by changing a property value or providing a command-line parameter.</span></span>
9. <span data-ttu-id="608f8-167">加入名為新目標**DeleteAppOffline**。</span><span class="sxs-lookup"><span data-stu-id="608f8-167">Add a new target named **DeleteAppOffline**.</span></span> <span data-ttu-id="608f8-168">這個目標，內叫用 MSDeploy.exe 命令會移除您*應用程式\_離線*目的地 web 伺服器的檔案。</span><span class="sxs-lookup"><span data-stu-id="608f8-168">Within this target, invoke the MSDeploy.exe command that removes your *App\_offline* file from the destination web server.</span></span>

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. <span data-ttu-id="608f8-169">最後一項工作是在專案檔的執行期間叫用這些新的目標在合適的點。</span><span class="sxs-lookup"><span data-stu-id="608f8-169">The final task is to invoke these new targets at appropriate points during the execution of your project file.</span></span> <span data-ttu-id="608f8-170">您可以透過各種方式。</span><span class="sxs-lookup"><span data-stu-id="608f8-170">You can do this in various ways.</span></span> <span data-ttu-id="608f8-171">例如，在*Publish.proj*檔案， **FullPublishDependsOn**屬性會指定必須執行的目標清單中排序時**FullPublish**預設目標會叫用。</span><span class="sxs-lookup"><span data-stu-id="608f8-171">For example, in the *Publish.proj* file, the **FullPublishDependsOn** property specifies a list of targets that must be executed in order when the **FullPublish** default target is invoked.</span></span>
11. <span data-ttu-id="608f8-172">修改 MSBuild 專案檔，才能叫用**DeployAppOffline**和**DeleteAppOffline**在發佈程序中的適當點目標。</span><span class="sxs-lookup"><span data-stu-id="608f8-172">Modify your MSBuild project file to invoke the **DeployAppOffline** and **DeleteAppOffline** targets at appropriate points in the publishing process.</span></span>

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

<span data-ttu-id="608f8-173">當您執行您自訂的 MSBuild 專案檔，*應用程式\_離線*檔案將會在建置成功之後，立即部署到伺服器。</span><span class="sxs-lookup"><span data-stu-id="608f8-173">When you run your custom MSBuild project file, the *App\_offline* file will be deployed to the server immediately after a successful build.</span></span> <span data-ttu-id="608f8-174">部署的所有工作完成後，它就會從伺服器刪除。</span><span class="sxs-lookup"><span data-stu-id="608f8-174">It will then be deleted from the server once all the deployment tasks are complete.</span></span>

## <a name="adding-an-appoffline-file-to-deployment-packages"></a><span data-ttu-id="608f8-175">加入應用程式\_至部署套件的離線檔案</span><span class="sxs-lookup"><span data-stu-id="608f8-175">Adding an App\_Offline File to Deployment Packages</span></span>

<span data-ttu-id="608f8-176">根據您設定您的部署方式，任何現有內容目的地 IIS web 應用程式&#x2014;像*應用程式\_offline.htm*檔案&#x2014;web 部署時可能會自動刪除目的地的封裝。</span><span class="sxs-lookup"><span data-stu-id="608f8-176">Depending on how you configure your deployment, any existing content at the destination IIS web application&#x2014;like the *App\_offline.htm* file&#x2014;may be deleted automatically when you deploy a web package to the destination.</span></span> <span data-ttu-id="608f8-177">若要確保*應用程式\_offline.htm*檔案持續作用，部署期間，您需要將檔案直接在開始部署請另外包含本身的 web 部署套件中的檔案部署程序。</span><span class="sxs-lookup"><span data-stu-id="608f8-177">To ensure that the *App\_offline.htm* file remains in place for the duration of the deployment, you need to include the file within the web deployment package itself in addition to deploying the file directly at the start of the deployment process.</span></span>

- <span data-ttu-id="608f8-178">如果您已經遵循本主題中先前的工作，您會加入*應用程式\_offline.htm*至不同的檔案名稱底下的 web 應用程式專案的檔案 (我們使用*應用程式\_離線 template.htm*)，您將已設定的建置動作為**無**。</span><span class="sxs-lookup"><span data-stu-id="608f8-178">If you've followed the previous tasks in this topic, you'll have added the *App\_offline.htm* file to your web application project under a different filename (we used *App\_offline-template.htm*) and you'll have set the build action to **None**.</span></span> <span data-ttu-id="608f8-179">這些變更是為了避免干擾開發和偵錯中的檔案。</span><span class="sxs-lookup"><span data-stu-id="608f8-179">These changes are necessary to prevent the file from interfering with development and debugging.</span></span> <span data-ttu-id="608f8-180">如此一來，您需要自訂封裝程序，以確保*應用程式\_offline.htm* web 部署套件中包含檔案。</span><span class="sxs-lookup"><span data-stu-id="608f8-180">As a result, you need to customize the packaging process to ensure that the *App\_offline.htm* file is included in the web deployment package.</span></span>

<span data-ttu-id="608f8-181">Web 發行管線 (WPP) 會使用名為的項目清單**FilesForPackagingFromProject**建置的 web 部署套件中應包含的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="608f8-181">The Web Publishing Pipeline (WPP) uses an item list named **FilesForPackagingFromProject** to build a list of files that should be included in the web deployment package.</span></span> <span data-ttu-id="608f8-182">您可以自訂 web 封裝的內容，將您自己的項目加入至這個清單。</span><span class="sxs-lookup"><span data-stu-id="608f8-182">You can customize the contents of your web packages by adding your own items to this list.</span></span> <span data-ttu-id="608f8-183">若要這樣做，您需要完成下列高階步驟：</span><span class="sxs-lookup"><span data-stu-id="608f8-183">To do this, you need to complete these high-level steps:</span></span>

1. <span data-ttu-id="608f8-184">建立自訂專案檔名為 *[專案名稱].wpp.targets*專案檔相同資料夾中。</span><span class="sxs-lookup"><span data-stu-id="608f8-184">Create a custom project file named *[project name].wpp.targets* in the same folder as your project file.</span></span>

    > [!NOTE]
    > <span data-ttu-id="608f8-185">*。 Wpp.targets*檔案必須與您的 web 應用程式專案檔相同的資料夾中移&#x2014;，例如*ContactManager.Mvc.csproj*&#x2014;而不是任何自訂相同資料夾中您用來控制組建和部署程序的專案檔案。</span><span class="sxs-lookup"><span data-stu-id="608f8-185">The *.wpp.targets* file needs to go in the same folder as your web application project file&#x2014;for example, *ContactManager.Mvc.csproj*&#x2014;rather than in the same folder as any custom project files you use to control the build and deployment process.</span></span>
2. <span data-ttu-id="608f8-186">在 *。 wpp.targets*檔案中，建立新的 MSBuild 目標執行*之前* **CopyAllFilesToSingleFolderForPackage**目標。</span><span class="sxs-lookup"><span data-stu-id="608f8-186">In the *.wpp.targets* file, create a new MSBuild target that executes *before* the **CopyAllFilesToSingleFolderForPackage** target.</span></span> <span data-ttu-id="608f8-187">這是建立要包含在封裝中的項目清單 WPP 目標。</span><span class="sxs-lookup"><span data-stu-id="608f8-187">This is the WPP target that builds a list of things to include in the package.</span></span>
3. <span data-ttu-id="608f8-188">在新的目標建立**ItemGroup**項目。</span><span class="sxs-lookup"><span data-stu-id="608f8-188">In the new target, create an **ItemGroup** element.</span></span>
4. <span data-ttu-id="608f8-189">在**ItemGroup**項目，加入**FilesForPackagingFromProject**項目，並指定*應用程式\_offline.htm*檔案。</span><span class="sxs-lookup"><span data-stu-id="608f8-189">In the **ItemGroup** element, add a **FilesForPackagingFromProject** item and specify the *App\_offline.htm* file.</span></span>

<span data-ttu-id="608f8-190">*。 Wpp.targets*檔案應該會與以下相似：</span><span class="sxs-lookup"><span data-stu-id="608f8-190">The *.wpp.targets* file should resemble this:</span></span>


[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]


<span data-ttu-id="608f8-191">請注意，在此範例的重點如下：</span><span class="sxs-lookup"><span data-stu-id="608f8-191">These are the key points of note in this example:</span></span>

- <span data-ttu-id="608f8-192">**之 BeforeTargets**屬性插入到藉由指定 WPP 應在執行前立即這個目標**CopyAllFilesToSingleFolderForPackage**目標。</span><span class="sxs-lookup"><span data-stu-id="608f8-192">The **BeforeTargets** attribute inserts this target into the WPP by specifying that it should be executed immediately before the **CopyAllFilesToSingleFolderForPackage** target.</span></span>
- <span data-ttu-id="608f8-193">**FilesForPackagingFromProject**項目使用**DestinationRelativePath**中繼資料值從檔案重新命名*應用程式\_離線 template.htm*若要*應用程式\_offline.htm*將它加入至清單。</span><span class="sxs-lookup"><span data-stu-id="608f8-193">The **FilesForPackagingFromProject** item uses the **DestinationRelativePath** metadata value to rename the file from *App\_offline-template.htm* to *App\_offline.htm* as it's added to the list.</span></span>

<span data-ttu-id="608f8-194">下一個程序會示範如何加入此 *。 wpp.targets* web 應用程式專案的檔案。</span><span class="sxs-lookup"><span data-stu-id="608f8-194">The next procedure shows you how to add this *.wpp.targets* file to a web application project.</span></span>

<span data-ttu-id="608f8-195">**若要加入。 wpp.targets web 部署套件的檔案**</span><span class="sxs-lookup"><span data-stu-id="608f8-195">**To add a .wpp.targets file to a web deployment package**</span></span>

1. <span data-ttu-id="608f8-196">Visual Studio 2010 中開啟您的方案。</span><span class="sxs-lookup"><span data-stu-id="608f8-196">Open your solution in Visual Studio 2010.</span></span>
2. <span data-ttu-id="608f8-197">在**方案總管] 中**視窗中，以滑鼠右鍵按一下您 web 應用程式的專案節點 (比方說， **ContactManager.Mvc**)，指向**新增**，然後按一下 [ **新項目**。</span><span class="sxs-lookup"><span data-stu-id="608f8-197">In the **Solution Explorer** window, right-click your web application project node (for example, **ContactManager.Mvc**), point to **Add**, and then click **New Item**.</span></span>
3. <span data-ttu-id="608f8-198">在**加入新項目**對話方塊中，選取**XML 檔案**範本。</span><span class="sxs-lookup"><span data-stu-id="608f8-198">In the **Add New Item** dialog box, select the **XML File** template.</span></span>
4. <span data-ttu-id="608f8-199">在**名稱**方塊中，輸入 *[專案名稱] * * *.wpp.targets** (例如， **ContactManager.Mvc.wpp.targets**)，然後按一下 **新增**.</span><span class="sxs-lookup"><span data-stu-id="608f8-199">In the **Name** box, type *[project name]***.wpp.targets** (for example, **ContactManager.Mvc.wpp.targets**), and then click **Add**.</span></span>

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > <span data-ttu-id="608f8-200">如果您將新的項目加入專案的根節點時，檔案會建立專案檔相同資料夾中。</span><span class="sxs-lookup"><span data-stu-id="608f8-200">If you add a new item to the root node of a project, the file is created in the same folder as the project file.</span></span> <span data-ttu-id="608f8-201">您可以驗證，請在 Windows 檔案總管 中開啟資料夾。</span><span class="sxs-lookup"><span data-stu-id="608f8-201">You can verify this by opening the folder in Windows Explorer.</span></span>
5. <span data-ttu-id="608f8-202">在檔案中，加入的 MSBuild 標記先前所述。</span><span class="sxs-lookup"><span data-stu-id="608f8-202">In the file, add the MSBuild markup described previously.</span></span>

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. <span data-ttu-id="608f8-203">儲存並關閉 *[專案名稱].wpp.targets*檔案。</span><span class="sxs-lookup"><span data-stu-id="608f8-203">Save and close the *[project name].wpp.targets* file.</span></span>

<span data-ttu-id="608f8-204">下次當您建置並封裝您的 web 應用程式專案，WPP 會自動偵測 *。 wpp.targets*檔案。</span><span class="sxs-lookup"><span data-stu-id="608f8-204">The next time you build and package your web application project, the WPP will automatically detect the *.wpp.targets* file.</span></span> <span data-ttu-id="608f8-205">*應用程式\_離線 template.htm*檔案將會包含在產生的 web 部署封裝，做為*應用程式\_offline.htm*。</span><span class="sxs-lookup"><span data-stu-id="608f8-205">The *App\_offline-template.htm* file will be included in the resulting web deployment package as *App\_offline.htm*.</span></span>

> [!NOTE]
> <span data-ttu-id="608f8-206">如果您的部署失敗，*應用程式\_offline.htm*檔案會保留在位置和您的應用程式角色將保持離線。</span><span class="sxs-lookup"><span data-stu-id="608f8-206">If your deployment fails, the *App\_offline.htm* file will remain in place and your application will remain offline.</span></span> <span data-ttu-id="608f8-207">這通常是所要的行為。</span><span class="sxs-lookup"><span data-stu-id="608f8-207">This is typically the desired behavior.</span></span> <span data-ttu-id="608f8-208">若要將您的應用程式恢復上線，您可以刪除*應用程式\_offline.htm*檔案從您網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="608f8-208">To bring your application back online, you can delete the *App\_offline.htm* file from your web server.</span></span> <span data-ttu-id="608f8-209">或者，如果您更正任何錯誤，然後執行成功的部署，*應用程式\_offline.htm*檔案將會移除。</span><span class="sxs-lookup"><span data-stu-id="608f8-209">Alternatively, if you correct any errors and run a successful deployment, the *App\_offline.htm* file will be removed.</span></span>


## <a name="conclusion"></a><span data-ttu-id="608f8-210">結論</span><span class="sxs-lookup"><span data-stu-id="608f8-210">Conclusion</span></span>

<span data-ttu-id="608f8-211">本主題描述如何取得 web 應用程式離線期間的部署，透過發行*應用程式\_offline.htm*檔案到目的地伺服器，在開始部署程序，並移除在結束。</span><span class="sxs-lookup"><span data-stu-id="608f8-211">This topic described how to take a web application offline for the duration of a deployment, by publishing an *App\_offline.htm* file to the destination server at the start of the deployment process and removing it at the end.</span></span> <span data-ttu-id="608f8-212">它也會涵蓋如何納入*應用程式\_offline.htm* web 部署套件中的檔案。</span><span class="sxs-lookup"><span data-stu-id="608f8-212">It also covered how to include an *App\_offline.htm* file in a web deployment package.</span></span>

## <a name="further-reading"></a><span data-ttu-id="608f8-213">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="608f8-213">Further Reading</span></span>

<span data-ttu-id="608f8-214">如需有關封裝和部署程序的詳細資訊，請參閱[建置和封裝 Web 應用程式專案](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)， [Web 封裝部署的設定參數](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)，和[部署 Web 封裝](../web-deployment-in-the-enterprise/deploying-web-packages.md)。</span><span class="sxs-lookup"><span data-stu-id="608f8-214">For more information on the packaging and deployment process, see [Building and Packaging Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Configuring Parameters for Web Package Deployment](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), and [Deploying Web Packages](../web-deployment-in-the-enterprise/deploying-web-packages.md).</span></span>

<span data-ttu-id="608f8-215">如果您發行您的 web 應用程式，直接從 Visual Studio 中，而不是使用這些教學課程中所述的自訂 MSBuild 專案檔案方法，您必須使用稍微不同的方法來採取應用程式離線發行期間程序。</span><span class="sxs-lookup"><span data-stu-id="608f8-215">If you publish your web applications directly from Visual Studio, rather than using the custom MSBuild project file approach described in these tutorials, you'll need to use a slightly different approach to take your application offline during the publishing process.</span></span> <span data-ttu-id="608f8-216">如需詳細資訊，請參閱[發行期間進行 web 應用程式離線](https://go.microsoft.com/?linkid=9805135)（部落格文章）。</span><span class="sxs-lookup"><span data-stu-id="608f8-216">For more information, see [How to take your web app offline during publishing](https://go.microsoft.com/?linkid=9805135) (blog post).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="608f8-217">[上一頁](excluding-files-and-folders-from-deployment.md)
> [下一頁](running-windows-powershell-scripts-from-msbuild-project-files.md)</span><span class="sxs-lookup"><span data-stu-id="608f8-217">[Previous](excluding-files-and-folders-from-deployment.md)
[Next](running-windows-powershell-scripts-from-msbuild-project-files.md)</span></span>
