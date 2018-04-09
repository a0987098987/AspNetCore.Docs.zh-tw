---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 使用 Visual Studio 的 ASP.NET Web 部署： 命令列部署 |Microsoft 文件
author: tdykstra
description: 此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: acc4a0e7f4744a3759b90e0f1b159da68b7c7362
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a><span data-ttu-id="9aa16-103">使用 Visual Studio 的 ASP.NET Web 部署： 命令列部署</span><span class="sxs-lookup"><span data-stu-id="9aa16-103">ASP.NET Web Deployment using Visual Studio: Command Line Deployment</span></span>
====================
<span data-ttu-id="9aa16-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="9aa16-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="9aa16-105">下載入門專案</span><span class="sxs-lookup"><span data-stu-id="9aa16-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="9aa16-106">此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="9aa16-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="9aa16-107">數列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="9aa16-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="9aa16-108">總覽</span><span class="sxs-lookup"><span data-stu-id="9aa16-108">Overview</span></span>

<span data-ttu-id="9aa16-109">本教學課程會示範如何叫用 Visual Studio web 發行管線就會從命令列。</span><span class="sxs-lookup"><span data-stu-id="9aa16-109">This tutorial shows you how to invoke the Visual Studio web publish pipeline from the command line.</span></span> <span data-ttu-id="9aa16-110">這是適用於您想要的案例[自動化部署程序](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md)而不以手動方式在 Visual Studio 中進行，通常使用[原始程式碼版本控制系統](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)。</span><span class="sxs-lookup"><span data-stu-id="9aa16-110">This is useful for scenarios where you want to [automate the deployment process](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) instead of doing it manually in Visual Studio, typically by using a [source code version control system](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).</span></span>

## <a name="make-a-change-to-deploy"></a><span data-ttu-id="9aa16-111">若要部署變更</span><span class="sxs-lookup"><span data-stu-id="9aa16-111">Make a change to deploy</span></span>

<span data-ttu-id="9aa16-112">目前 [關於] 頁面會顯示範本程式碼。</span><span class="sxs-lookup"><span data-stu-id="9aa16-112">Currently the About page displays the template code.</span></span>

![有關頁面範本程式碼](command-line-deployment/_static/image1.png)

<span data-ttu-id="9aa16-114">您會取代所顯示的學生註冊摘要的程式碼。</span><span class="sxs-lookup"><span data-stu-id="9aa16-114">You'll replace that with code that displays a summary of student enrollment.</span></span>

<span data-ttu-id="9aa16-115">開啟*about.aspx 的網頁*頁面上，刪除所有內部標記`MainContent``Content`項目，然後插入其所在位置的下列標記：</span><span class="sxs-lookup"><span data-stu-id="9aa16-115">Open the *About.aspx* page, delete all of the markup inside the `MainContent` `Content` element, and insert the following markup in its place:</span></span>

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

<span data-ttu-id="9aa16-116">執行專案並選取**有關**頁面。</span><span class="sxs-lookup"><span data-stu-id="9aa16-116">Run the project and select the **About** page.</span></span>

![About 頁面](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a><span data-ttu-id="9aa16-118">使用命令列部署至測試</span><span class="sxs-lookup"><span data-stu-id="9aa16-118">Deploy to Test by using the command line</span></span>

<span data-ttu-id="9aa16-119">您將不會部署另一個資料庫變更，因此停用 dbDacFx 資料庫 aspnet ContosoUniversity 資料庫的部署。</span><span class="sxs-lookup"><span data-stu-id="9aa16-119">You won't be deploying another database change, so disable dbDacFx database deployment for the aspnet-ContosoUniversity database.</span></span> <span data-ttu-id="9aa16-120">開啟**發行 Web**精靈，並在每個，這三個發行設定檔，清除**更新資料庫** 核取方塊**設定** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="9aa16-120">Open the **Publish Web** wizard, and in each of the three publish profiles, clear the **Update Database** check box on the **Settings** tab.</span></span>

<span data-ttu-id="9aa16-121">在 Windows 8 [開始] 頁面上，搜尋**VS2012 開發人員命令提示字元**。</span><span class="sxs-lookup"><span data-stu-id="9aa16-121">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**.</span></span>

<span data-ttu-id="9aa16-122">以滑鼠右鍵按一下圖示**VS2012 開發人員命令提示字元**按一下**系統管理員身分執行**。</span><span class="sxs-lookup"><span data-stu-id="9aa16-122">Right-click the icon for **Developer Command Prompt for VS2012** and click **Run as administrator**.</span></span>

<span data-ttu-id="9aa16-123">輸入下列命令，在命令提示字元中，方案檔案的路徑取代為您的方案檔的路徑：</span><span class="sxs-lookup"><span data-stu-id="9aa16-123">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file:</span></span>

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

<span data-ttu-id="9aa16-124">MSBuild 建置方案，並將其部署到測試環境。</span><span class="sxs-lookup"><span data-stu-id="9aa16-124">MSBuild builds the solution and deploys it to the test environment.</span></span>

![命令列輸出](command-line-deployment/_static/image3.png)

<span data-ttu-id="9aa16-126">開啟瀏覽器並移至`http://localhost/ContosoUniversity`，然後按一下 **有關**頁面，以驗證部署是否成功。</span><span class="sxs-lookup"><span data-stu-id="9aa16-126">Open a browser and go to `http://localhost/ContosoUniversity`, then click the **About** page to verify that the deployment was successful.</span></span>

<span data-ttu-id="9aa16-127">如果您尚未建立任何學生在測試中，您會看到下的空白頁面**學生主體統計資料**標題。</span><span class="sxs-lookup"><span data-stu-id="9aa16-127">If you haven't created any students in test, you'll see an empty page under the **Student Body Statistics** heading.</span></span> <span data-ttu-id="9aa16-128">移至**學生**頁面上，按一下**新增學生**，加入一些學生，然後返回**有關**頁面，即可查看學生統計資料。</span><span class="sxs-lookup"><span data-stu-id="9aa16-128">Go to the **Students** page, click **Add Student**, and add some students, and then return to the **About** page to see student statistics.</span></span>

![關於測試環境中的頁面](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a><span data-ttu-id="9aa16-130">索引鍵的命令列選項</span><span class="sxs-lookup"><span data-stu-id="9aa16-130">Key command line options</span></span>

<span data-ttu-id="9aa16-131">您輸入的命令傳遞至 MSBuild 的方案檔路徑和兩個屬性：</span><span class="sxs-lookup"><span data-stu-id="9aa16-131">The command that you entered passed the solution file path and two properties to MSBuild:</span></span>

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a><span data-ttu-id="9aa16-132">部署與部署個別專案方案</span><span class="sxs-lookup"><span data-stu-id="9aa16-132">Deploying the solution versus deploying individual projects</span></span>

<span data-ttu-id="9aa16-133">指定的方案檔導致要建置方案中的所有專案。</span><span class="sxs-lookup"><span data-stu-id="9aa16-133">Specifying the solution file causes all projects in the solution to be built.</span></span> <span data-ttu-id="9aa16-134">如果您在方案中有多個 web 專案，適用於下列 MSBuild 行為：</span><span class="sxs-lookup"><span data-stu-id="9aa16-134">If you have multiple web projects in the solution, the following MSBuild behavior applies:</span></span>

- <span data-ttu-id="9aa16-135">您在命令列指定的內容會傳遞至每個專案。</span><span class="sxs-lookup"><span data-stu-id="9aa16-135">The properties that you specify on the command line are passed to every project.</span></span> <span data-ttu-id="9aa16-136">因此，每個 web 專案必須具有您指定之名稱的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="9aa16-136">Therefore, each web project must have a publish profile with the name that you specify.</span></span> <span data-ttu-id="9aa16-137">如果您指定`/p:PublishProfile=Test`，每個 web 專案必須具有名為的發行設定檔*測試*。</span><span class="sxs-lookup"><span data-stu-id="9aa16-137">If you specify `/p:PublishProfile=Test`, each web project must have a publish profile named *Test*.</span></span>
- <span data-ttu-id="9aa16-138">已成功，甚至不會建立另一個時，您可能會發行一個專案。</span><span class="sxs-lookup"><span data-stu-id="9aa16-138">You might successfully publish one project when another one doesn't even build.</span></span> <span data-ttu-id="9aa16-139">如需詳細資訊，請參閱 stackoverflow 執行緒[MSBuild 因兩個封裝](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages)。</span><span class="sxs-lookup"><span data-stu-id="9aa16-139">For more information, see the stackoverflow thread [MSBuild fails with two packages](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).</span></span>

<span data-ttu-id="9aa16-140">如果您指定個別的專案，而不是方案，您必須加入的參數，指定 Visual Studio 版本。</span><span class="sxs-lookup"><span data-stu-id="9aa16-140">If you specify an individual project instead of a solution, you have to add a parameter that specifies the Visual Studio version.</span></span> <span data-ttu-id="9aa16-141">如果您使用 Visual Studio 2012 命令列會類似下列的範例：</span><span class="sxs-lookup"><span data-stu-id="9aa16-141">If you are using Visual Studio 2012 the command line would be similar to the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

<span data-ttu-id="9aa16-142">Visual Studio 2010 的版本號碼為 10.0。</span><span class="sxs-lookup"><span data-stu-id="9aa16-142">The version number for Visual Studio 2010 is 10.0.</span></span> <span data-ttu-id="9aa16-143">如需詳細資訊，請參閱[Visual Studio 專案相容性和 VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) Sayed Hashimi 部落格上。</span><span class="sxs-lookup"><span data-stu-id="9aa16-143">For more information, see [Visual Studio project compatability and VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) on Sayed Hashimi's blog.</span></span>

### <a name="specifying-the-publish-profile"></a><span data-ttu-id="9aa16-144">指定發行設定檔</span><span class="sxs-lookup"><span data-stu-id="9aa16-144">Specifying the publish profile</span></span>

<span data-ttu-id="9aa16-145">依名稱或完整路徑，您可以指定發行設定檔*.pubxml*檔案，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="9aa16-145">You can specify the publish profile by name or by the full path to the *.pubxml* file, as shown in the following example:</span></span>

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a><span data-ttu-id="9aa16-146">Web 發行支援的命令列發行方法</span><span class="sxs-lookup"><span data-stu-id="9aa16-146">Web publish methods supported for command-line publishing</span></span>

<span data-ttu-id="9aa16-147">三個發行方法支援的命令列發行：</span><span class="sxs-lookup"><span data-stu-id="9aa16-147">Three publish methods are supported for command line publishing:</span></span>

- <span data-ttu-id="9aa16-148">`MSDeploy` -使用 Web Deploy 發行。</span><span class="sxs-lookup"><span data-stu-id="9aa16-148">`MSDeploy` - Publish by using Web Deploy.</span></span>
- <span data-ttu-id="9aa16-149">`Package` 藉由建立 Web 部署套件發佈。</span><span class="sxs-lookup"><span data-stu-id="9aa16-149">`Package` - Publish by creating a Web Deploy Package.</span></span> <span data-ttu-id="9aa16-150">您必須從 MSBuild 命令會建立個別安裝套件。</span><span class="sxs-lookup"><span data-stu-id="9aa16-150">You have to install the package separately from the MSBuild command that creates it.</span></span>
- <span data-ttu-id="9aa16-151">`FileSystem` -發行檔案複製到指定的資料夾。</span><span class="sxs-lookup"><span data-stu-id="9aa16-151">`FileSystem` - Publish by copying files to a specified folder.</span></span>

### <a name="specifying-the-build-configuration-and-platform"></a><span data-ttu-id="9aa16-152">指定組建組態與平台</span><span class="sxs-lookup"><span data-stu-id="9aa16-152">Specifying the build configuration and platform</span></span>

<span data-ttu-id="9aa16-153">在 Visual Studio 或命令列上，必須設定組建組態與平台。</span><span class="sxs-lookup"><span data-stu-id="9aa16-153">The build configuration and platform must be set in Visual Studio or on the command line.</span></span> <span data-ttu-id="9aa16-154">發行設定檔包含屬性是名為`LastUsedBuildConfiguration`和`LastUsedPlatform`，但您無法設定這些屬性，以判斷如何建置專案。</span><span class="sxs-lookup"><span data-stu-id="9aa16-154">The publish profiles include properties that are named `LastUsedBuildConfiguration` and `LastUsedPlatform`, but you can't set these properties in order to determine how the project is built.</span></span> <span data-ttu-id="9aa16-155">如需詳細資訊，請參閱[MSBuild： 如何設定組態屬性](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx)Sayed Hashimi 部落格上。</span><span class="sxs-lookup"><span data-stu-id="9aa16-155">For more information, see [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) on Sayed Hashimi's blog.</span></span>

## <a name="deploy-to-staging"></a><span data-ttu-id="9aa16-156">部署到預備環境</span><span class="sxs-lookup"><span data-stu-id="9aa16-156">Deploy to staging</span></span>

<span data-ttu-id="9aa16-157">若要部署至 Azure，您必須新增至命令列的密碼。</span><span class="sxs-lookup"><span data-stu-id="9aa16-157">To deploy to Azure, you must add the password to the command line.</span></span> <span data-ttu-id="9aa16-158">如果您在 Visual Studio 中的發行設定檔中儲存的密碼，它已加密形式儲存在您*。 pubxml.user*檔案。</span><span class="sxs-lookup"><span data-stu-id="9aa16-158">If you saved the password in the publish profile in Visual Studio, it was stored in encrypted form in the your *.pubxml.user* file.</span></span> <span data-ttu-id="9aa16-159">當您執行命令列部署中，您必須傳遞命令列參數中的密碼，該檔案不會由 MSBuild 存取。</span><span class="sxs-lookup"><span data-stu-id="9aa16-159">That file is not accessed by MSBuild when you do a command line deployment, so you have to pass in the password in a command line parameter.</span></span>

1. <span data-ttu-id="9aa16-160">複製您需要從密碼*.publishsettings*稍早的預備網站下載的檔案。</span><span class="sxs-lookup"><span data-stu-id="9aa16-160">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the staging web site.</span></span> <span data-ttu-id="9aa16-161">密碼是值`userPWD`Web deploy 屬性`publishProfile`項目。</span><span class="sxs-lookup"><span data-stu-id="9aa16-161">The password is the value of the `userPWD` attribute for the Web Deploy `publishProfile` element.</span></span>

    ![Web 部署密碼](command-line-deployment/_static/image5.png)
2. <span data-ttu-id="9aa16-163">在 Windows 8 開始 頁面上，搜尋**VS2012 開發人員命令提示字元**，然後按一下以開啟 命令提示字元圖示。</span><span class="sxs-lookup"><span data-stu-id="9aa16-163">In the Windows 8 Start page, search for **Developer Command Prompt for VS2012**, and click the icon to open the command prompt.</span></span> <span data-ttu-id="9aa16-164">（您不需要其系統管理員身分開啟此時因為您不部署至 IIS 在本機電腦上）。</span><span class="sxs-lookup"><span data-stu-id="9aa16-164">(You don't have to open it as administrator this time because you aren't deploying to IIS on the local computer.)</span></span>
3. <span data-ttu-id="9aa16-165">輸入下列命令，在命令提示字元中的方案檔的路徑取代為您的方案檔和您的密碼與密碼的路徑：</span><span class="sxs-lookup"><span data-stu-id="9aa16-165">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    <span data-ttu-id="9aa16-166">請注意，此命令列包含額外的參數： `/p:AllowUntrustedCertificate=true`。</span><span class="sxs-lookup"><span data-stu-id="9aa16-166">Notice that this command line includes an extra parameter: `/p:AllowUntrustedCertificate=true`.</span></span> <span data-ttu-id="9aa16-167">正在寫入本教學課程，`AllowUntrustedCertificate`從命令列發行至 Azure 時，就必須設定屬性。</span><span class="sxs-lookup"><span data-stu-id="9aa16-167">As this tutorial is being written, the `AllowUntrustedCertificate` property must be set when you publish to Azure from the command line.</span></span> <span data-ttu-id="9aa16-168">釋放這個 bug 的修正後，您不需要該參數。</span><span class="sxs-lookup"><span data-stu-id="9aa16-168">When the fix for this bug is released, you won't need that parameter.</span></span>
4. <span data-ttu-id="9aa16-169">開啟瀏覽器並移至您的預備網站的 URL，然後按一下**有關**頁面，以驗證部署是否成功。</span><span class="sxs-lookup"><span data-stu-id="9aa16-169">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

    <span data-ttu-id="9aa16-170">如同稍早為測試環境，您可能需要建立統計資料，讓某些學生**有關**頁面。</span><span class="sxs-lookup"><span data-stu-id="9aa16-170">As you saw earlier for the test environment, you might have to create some students to see statistics on the **About** page.</span></span>

## <a name="deploy-to-production"></a><span data-ttu-id="9aa16-171">部署到生產環境</span><span class="sxs-lookup"><span data-stu-id="9aa16-171">Deploy to production</span></span>

<span data-ttu-id="9aa16-172">部署到生產環境的程序是供暫存程序類似。</span><span class="sxs-lookup"><span data-stu-id="9aa16-172">The process for deploying to production is similar to the process for staging.</span></span>

1. <span data-ttu-id="9aa16-173">複製您需要從密碼*.publishsettings*提早針對實際執行的網站下載的檔案。</span><span class="sxs-lookup"><span data-stu-id="9aa16-173">Copy the password that you need from the *.publishsettings* file that you downloaded earlier for the production web site.</span></span>
2. <span data-ttu-id="9aa16-174">開啟**VS2012 開發人員命令提示字元**。</span><span class="sxs-lookup"><span data-stu-id="9aa16-174">Open **Developer Command Prompt for VS2012**.</span></span>
3. <span data-ttu-id="9aa16-175">輸入下列命令，在命令提示字元中的方案檔的路徑取代為您的方案檔和您的密碼與密碼的路徑：</span><span class="sxs-lookup"><span data-stu-id="9aa16-175">Enter the following command at the command prompt, replacing the path to the solution file with the path to your solution file and the password with your password:</span></span>

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    <span data-ttu-id="9aa16-176">真實的生產網站，如果還沒有資料庫變更，您會通常複製*應用程式\_offline.htm*檔案至站台部署之前，並成功部署之後刪除它。</span><span class="sxs-lookup"><span data-stu-id="9aa16-176">For a real production site, if there was also a database change, you would typically copy the *app\_offline.htm* file to the site before deployment and delete it after successful deployment.</span></span>
4. <span data-ttu-id="9aa16-177">開啟瀏覽器並移至您的預備網站的 URL，然後按一下**有關**頁面，以驗證部署是否成功。</span><span class="sxs-lookup"><span data-stu-id="9aa16-177">Open a browser and go to the URL of your staging site, and then click the **About** page to verify that the deployment was successful.</span></span>

## <a name="summary"></a><span data-ttu-id="9aa16-178">總結</span><span class="sxs-lookup"><span data-stu-id="9aa16-178">Summary</span></span>

<span data-ttu-id="9aa16-179">您現在已使用命令列部署應用程式更新。</span><span class="sxs-lookup"><span data-stu-id="9aa16-179">You have now deployed an application update by using the command line.</span></span>

![關於測試環境中的頁面](command-line-deployment/_static/image6.png)

<span data-ttu-id="9aa16-181">在下一個教學課程中，您會看到範例說明如何擴充 web 發行管線。</span><span class="sxs-lookup"><span data-stu-id="9aa16-181">In the next tutorial, you will see an example of how to extend the web publish pipeline.</span></span> <span data-ttu-id="9aa16-182">此範例將說明如何部署不包含在專案中的檔案。</span><span class="sxs-lookup"><span data-stu-id="9aa16-182">The example will show you how to deploy files that are not included in the project.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9aa16-183">[上一頁](deploying-a-database-update.md)
> [下一頁](deploying-extra-files.md)</span><span class="sxs-lookup"><span data-stu-id="9aa16-183">[Previous](deploying-a-database-update.md)
[Next](deploying-extra-files.md)</span></span>
