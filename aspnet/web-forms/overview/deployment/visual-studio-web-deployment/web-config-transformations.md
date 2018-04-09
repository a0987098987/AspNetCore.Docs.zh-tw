---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 使用 Visual Studio 的 ASP.NET Web 部署： Web.config 檔案轉換 |Microsoft 文件
author: tdykstra
description: 此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: 77ed0d8b2fe85adb009a3f4759030b7fba8fb9d7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a><span data-ttu-id="10966-103">使用 Visual Studio 的 ASP.NET Web 部署： Web.config 檔案轉換</span><span class="sxs-lookup"><span data-stu-id="10966-103">ASP.NET Web Deployment using Visual Studio: Web.config File Transformations</span></span>
====================
<span data-ttu-id="10966-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="10966-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="10966-105">下載入門專案</span><span class="sxs-lookup"><span data-stu-id="10966-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="10966-106">此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="10966-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="10966-107">數列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="10966-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="10966-108">總覽</span><span class="sxs-lookup"><span data-stu-id="10966-108">Overview</span></span>

<span data-ttu-id="10966-109">本教學課程會示範如何變更的程序自動化*Web.config*檔案時將它部署至不同目的地環境。</span><span class="sxs-lookup"><span data-stu-id="10966-109">This tutorial shows you how to automate the process of changing the *Web.config* file when you deploy it to different destination environments.</span></span> <span data-ttu-id="10966-110">大部分的應用程式中有設定*Web.config*部署應用程式時都必須使用不同的檔案。</span><span class="sxs-lookup"><span data-stu-id="10966-110">Most applications have settings in the *Web.config* file that must be different when the application is deployed.</span></span> <span data-ttu-id="10966-111">自動化程序可確保這些變更可防止您不必手動執行它們，每次您部署時，就可能發生冗長又容易出錯。</span><span class="sxs-lookup"><span data-stu-id="10966-111">Automating the process of making these changes keeps you from having to do them manually every time you deploy, which would be tedious and error prone.</span></span>

<span data-ttu-id="10966-112">提示： 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必檢查[疑難排解頁面](troubleshooting.md)。</span><span class="sxs-lookup"><span data-stu-id="10966-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a><span data-ttu-id="10966-113">Web Deploy 參數與 Web.config 轉換</span><span class="sxs-lookup"><span data-stu-id="10966-113">Web.config transformations versus Web Deploy parameters</span></span>

<span data-ttu-id="10966-114">有兩種方式變更的程序自動化*Web.config*檔案設定： [Web.config 轉換](https://msdn.microsoft.com/library/dd465326.aspx)和[Web Deploy 參數](https://msdn.microsoft.com/library/ff398068.aspx)。</span><span class="sxs-lookup"><span data-stu-id="10966-114">There are two ways to automate the process of changing *Web.config* file settings: [Web.config transformations](https://msdn.microsoft.com/library/dd465326.aspx) and [Web Deploy parameters](https://msdn.microsoft.com/library/ff398068.aspx).</span></span> <span data-ttu-id="10966-115">A *Web.config*轉換檔案包含會指定如何變更的 XML 標記*Web.config*部署到檔案。</span><span class="sxs-lookup"><span data-stu-id="10966-115">A *Web.config* transformation file contains XML markup that specifies how to change the *Web.config* file when it is deployed.</span></span> <span data-ttu-id="10966-116">您可以指定不同的變更，針對特定組建組態，以及針對特定的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="10966-116">You can specify different changes for specific build configurations and for specific publish profiles.</span></span> <span data-ttu-id="10966-117">預設建置組態偵錯和發行，而且您可以建立自訂的建置組態。</span><span class="sxs-lookup"><span data-stu-id="10966-117">The default build configurations are Debug and Release, and you can create custom build configurations.</span></span> <span data-ttu-id="10966-118">發行設定檔通常會對應到目的地環境。</span><span class="sxs-lookup"><span data-stu-id="10966-118">A publish profile typically corresponds to a destination environment.</span></span> <span data-ttu-id="10966-119">(您將學習到有關發行設定檔中的[部署至 IIS 做為測試環境](deploying-to-iis.md)教學課程。)</span><span class="sxs-lookup"><span data-stu-id="10966-119">(You'll learn more about publish profiles in the [Deploying to IIS as a Test Environment](deploying-to-iis.md) tutorial.)</span></span>

<span data-ttu-id="10966-120">Web 部署參數可以用來指定各種不同的設定，必須在部署期間，包括設定中找到設定*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="10966-120">Web Deploy parameters can be used to specify many different kinds of settings that must be configured during deployment, including settings that are found in *Web.config* files.</span></span> <span data-ttu-id="10966-121">當用來指定*Web.config*檔案變更，Web Deploy 參數都是更複雜的設定，但您不知道要部署之前設定的值時，它們很有用。</span><span class="sxs-lookup"><span data-stu-id="10966-121">When used to specify *Web.config* file changes, Web Deploy parameters are more complex to set up, but they are useful when you do not know the value to be set until you deploy.</span></span> <span data-ttu-id="10966-122">例如，在企業環境中，您可能會建立*部署套件*給予 IT 部門中的人員，若要安裝在生產環境中，以及該人員可以輸入連接字串或不這麼做的密碼了解。</span><span class="sxs-lookup"><span data-stu-id="10966-122">For example, in an enterprise environment, you might create a *deployment package* and give it to a person in the IT department to install in production, and that person has to be able to enter connection strings or passwords that you do not know.</span></span>

<span data-ttu-id="10966-123">此教學課程系列涵蓋的案例中，您事先知道，為了的一切*Web.config*檔案，因此您不需要使用 Web Deploy 參數。</span><span class="sxs-lookup"><span data-stu-id="10966-123">For the scenario that this tutorial series covers, you know in advance everything that has to be done to the *Web.config* file, so you do not need to use Web Deploy parameters.</span></span> <span data-ttu-id="10966-124">您需要設定一些轉換，而有所不同，所使用的組建組態，某些則可使用的發行設定檔而有所不同。</span><span class="sxs-lookup"><span data-stu-id="10966-124">You'll configure some transformations that differ depending on the build configuration used, and some that differ depending on the publish profile used.</span></span>

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a><span data-ttu-id="10966-125">在 Azure 中指定 Web.config 設定</span><span class="sxs-lookup"><span data-stu-id="10966-125">Specifying Web.config settings in Azure</span></span>

<span data-ttu-id="10966-126">如果*Web.config*您想要變更的檔案設定位於`<connectionStrings>`或`<appSettings>`項目，而且如果您要部署至 Azure App Service 中的 Web 應用程式，也可自動化期間變更的另一個選項部署。</span><span class="sxs-lookup"><span data-stu-id="10966-126">If the *Web.config* file settings that you want to change are in the `<connectionStrings>` or the `<appSettings>` element, and if you are deploying to Web Apps in Azure App Service, you have another option for automating changes during deployment.</span></span> <span data-ttu-id="10966-127">您可以輸入您想要在 Azure 中才會生效設定**設定**管理入口網站頁面，您的 web 應用程式 索引標籤 (向下捲動至**應用程式設定**和**連接字串**區段)。</span><span class="sxs-lookup"><span data-stu-id="10966-127">You can enter the settings that you want to take effect in Azure in the **Configure** tab of the management portal page for your web app (scroll down to the **app settings** and **connection strings** sections).</span></span> <span data-ttu-id="10966-128">當您部署專案時，Azure 會自動套用的變更。</span><span class="sxs-lookup"><span data-stu-id="10966-128">When you deploy the project, Azure automatically applies the changes.</span></span> <span data-ttu-id="10966-129">如需詳細資訊，請參閱[Windows Azure Web Sites： 如何應用程式字串與連接字串工作](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx)。</span><span class="sxs-lookup"><span data-stu-id="10966-129">For more information, see [Windows Azure Web Sites: How Application Strings and Connection Strings Work](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).</span></span>

## <a name="default-transformation-files"></a><span data-ttu-id="10966-130">預設轉換檔案</span><span class="sxs-lookup"><span data-stu-id="10966-130">Default transformation files</span></span>

<span data-ttu-id="10966-131">在**方案總管 中**，依序展開*Web.config*查看*Web.Debug.config*和*Web.Release.config*轉換的檔案會建立兩個預設建置組態的預設值。</span><span class="sxs-lookup"><span data-stu-id="10966-131">In **Solution Explorer**, expand *Web.config* to see the *Web.Debug.config* and *Web.Release.config* transformation files that are created by default for the two default build configurations.</span></span>

![Web.config_transform_files](web-config-transformations/_static/image1.png)

<span data-ttu-id="10966-133">您可以建立自訂的建置組態的轉換檔案的 Web.config 檔案上按一下滑鼠右鍵，然後選擇**新增 Config 轉換**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="10966-133">You can create transformation files for custom build configurations by right-clicking the Web.config file and choosing **Add Config Transforms** from the context menu.</span></span> <span data-ttu-id="10966-134">此教學課程中，您不需要這麼做，和功能表選項會停用，因為您尚未建立任何自訂的建置組態。</span><span class="sxs-lookup"><span data-stu-id="10966-134">For this tutorial you don't need to do that, and the menu option is disabled, because you haven't created any custom build configurations.</span></span>

<span data-ttu-id="10966-135">稍後您將建立三個以上轉換的檔案，每個測試、 暫存、 和生產的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="10966-135">Later you'll create three more transformation files, one each for the test, staging, and production publish profiles.</span></span> <span data-ttu-id="10966-136">典型的範例，因為它相依於目的地環境一樣處理中的發行設定檔轉換檔案的設定是不同的測試與實際執行的 WCF 端點。</span><span class="sxs-lookup"><span data-stu-id="10966-136">A typical example of a setting that you would handle in a publish profile transformation file because it depends on the destination environment is a WCF endpoint that is different for test versus production.</span></span> <span data-ttu-id="10966-137">您要建立發行設定檔轉換之後的教學課程中建立會移至與發行設定檔之後。</span><span class="sxs-lookup"><span data-stu-id="10966-137">You'll create publish profile transformation files in later tutorials after you create the publish profiles that they go with.</span></span>

## <a name="disable-debug-mode"></a><span data-ttu-id="10966-138">停用偵錯模式</span><span class="sxs-lookup"><span data-stu-id="10966-138">Disable debug mode</span></span>

<span data-ttu-id="10966-139">設定相依於組建組態，而不是目的地環境的範例`debug`屬性。</span><span class="sxs-lookup"><span data-stu-id="10966-139">An example of a setting that depends on build configuration rather than destination environment is the `debug` attribute.</span></span> <span data-ttu-id="10966-140">在發行組建，您通常會想停用偵錯不論何種環境中，您要部署到。</span><span class="sxs-lookup"><span data-stu-id="10966-140">For a Release build, you typically want debugging disabled regardless of which environment you are deploying to.</span></span> <span data-ttu-id="10966-141">因此，根據預設 Visual Studio 專案範本建立*Web.Release.config*檔案中移除的程式碼轉換`debug`屬性從`compilation`項目。</span><span class="sxs-lookup"><span data-stu-id="10966-141">Therefore, by default the Visual Studio project templates create *Web.Release.config* transform files with code that removes the `debug` attribute from the `compilation` element.</span></span> <span data-ttu-id="10966-142">以下是預設*Web.Release.config*： 標記為註解的一些範例轉換程式碼，除了它還包含中的程式碼`compilation`移除項目`debug`屬性：</span><span class="sxs-lookup"><span data-stu-id="10966-142">Here is the default *Web.Release.config*: in addition to some sample transformation code that is commented out, it includes code in the `compilation` element that removes the `debug` attribute:</span></span>

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

<span data-ttu-id="10966-143">`xdt:Transform="RemoveAttributes(debug)"`屬性會指定您想`debug`屬性，以移除`system.web/compilation`中已部署的項目*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="10966-143">The `xdt:Transform="RemoveAttributes(debug)"` attribute specifies that you want the `debug` attribute to be removed from the `system.web/compilation` element in the deployed *Web.config* file.</span></span> <span data-ttu-id="10966-144">這會在每次部署發行的組建完成。</span><span class="sxs-lookup"><span data-stu-id="10966-144">This will be done every time you deploy a Release build.</span></span>

## <a name="limit-error-log-access-to-administrators"></a><span data-ttu-id="10966-145">限制系統管理員的存取錯誤記錄檔</span><span class="sxs-lookup"><span data-stu-id="10966-145">Limit error log access to administrators</span></span>

<span data-ttu-id="10966-146">如果應用程式執行期間發生錯誤時，應用程式會顯示一般錯誤頁面取代系統產生的錯誤 頁面上，而且它會使用[Elmah NuGet 封裝](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx)記錄和報告的錯誤。</span><span class="sxs-lookup"><span data-stu-id="10966-146">If there's an error while the application runs, the application displays a generic error page in place of the system-generated error page, and it uses the [Elmah NuGet package](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) for error logging and reporting.</span></span> <span data-ttu-id="10966-147">`customErrors`應用程式中的項目*Web.config*檔案會指定錯誤頁面：</span><span class="sxs-lookup"><span data-stu-id="10966-147">The `customErrors` element in the application *Web.config* file specifies the error page:</span></span>

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

<span data-ttu-id="10966-148">若要查看錯誤頁面，暫時變更`mode`屬性`customErrors`"RemoteOnly"以 「 On 」 並執行應用程式，從 Visual Studio 中的項目。</span><span class="sxs-lookup"><span data-stu-id="10966-148">To see the error page, temporarily change the `mode` attribute of the `customErrors` element from "RemoteOnly" to "On" and run the application from Visual Studio.</span></span> <span data-ttu-id="10966-149">這類要求 URL 無效，而造成錯誤*Studentsxxx.aspx*。</span><span class="sxs-lookup"><span data-stu-id="10966-149">Cause an error by requesting an invalid URL, such as *Studentsxxx.aspx*.</span></span> <span data-ttu-id="10966-150">而非 IIS 產生"找不資源 」 的錯誤頁面上，您會看到*GenericErrorPage.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="10966-150">Instead of an IIS-generated "The resource cannot be found" error page, you see the *GenericErrorPage.aspx* page.</span></span>

![錯誤頁面](web-config-transformations/_static/image2.png)

<span data-ttu-id="10966-152">若要查看錯誤記錄檔，取代 URL 中的所有項目與通訊埠編號後面*elmah.axd* (例如， `http://localhost:51130/elmah.axd`) 按下 Enter:</span><span class="sxs-lookup"><span data-stu-id="10966-152">To see the error log, replace everything in the URL after the port number with *elmah.axd* (for example, `http://localhost:51130/elmah.axd`) and press Enter:</span></span>

![ELMAH 頁面](web-config-transformations/_static/image3.png)

<span data-ttu-id="10966-154">別忘了設定`customErrors`回"RemoteOnly"模式，當您完成時的項目。</span><span class="sxs-lookup"><span data-stu-id="10966-154">Don't forget to set the `customErrors` element back to "RemoteOnly" mode when you're done.</span></span>

<span data-ttu-id="10966-155">在開發電腦上並方便允許 [錯誤記錄] 頁面中，但會有安全性風險的生產環境中可用的存取。</span><span class="sxs-lookup"><span data-stu-id="10966-155">On your development computer it's convenient to allow free access to the error log page, but in production that would be a security risk.</span></span> <span data-ttu-id="10966-156">將生產站台，您想要新增授權規則，以限制存取錯誤記錄檔給系統管理員，並確定這項限制適用於您想測試和暫存也中。</span><span class="sxs-lookup"><span data-stu-id="10966-156">For the production site, you want to add an authorization rule that restricts error log access to administrators, and to make sure that the restriction works you want it in test and staging also.</span></span> <span data-ttu-id="10966-157">因此這是您想要實作每次部署發行的組建，因此它屬於另一項變更*Web.Release.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="10966-157">Therefore this is another change that you want to implement every time you deploy a Release build, and so it belongs in the *Web.Release.config* file.</span></span>

<span data-ttu-id="10966-158">開啟*Web.Release.config*再加入新`location`項目結束之前立即`configuration`標記，如下所示。</span><span class="sxs-lookup"><span data-stu-id="10966-158">Open *Web.Release.config* and add a new `location` element immediately before the closing `configuration` tag, as shown here.</span></span>

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

<span data-ttu-id="10966-159">`Transform` 「 插入 」 屬性值會造成這個`location`會成為同層級新增到任何現有項目`location`中的項目*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="10966-159">The `Transform` attribute value of "Insert" causes this `location` element to be added as a sibling to any existing `location` elements in the *Web.config* file.</span></span> <span data-ttu-id="10966-160">(已經有一個`location`項目，指定授權規則**更新信用額度**頁面。)</span><span class="sxs-lookup"><span data-stu-id="10966-160">(There is already one `location` element that specifies authorization rules for the **Update Credits** page.)</span></span>

<span data-ttu-id="10966-161">現在您可以預覽要確定您正確編碼它的轉換。</span><span class="sxs-lookup"><span data-stu-id="10966-161">Now you can preview the transform to make sure that you coded it correctly.</span></span>

<span data-ttu-id="10966-162">在**方案總管 中**，以滑鼠右鍵按一下*Web.Release.config*按一下**預覽轉換**。</span><span class="sxs-lookup"><span data-stu-id="10966-162">In **Solution Explorer**, right-click *Web.Release.config* and click **Preview Transform**.</span></span>

![預覽轉換功能表](web-config-transformations/_static/image4.png)

<span data-ttu-id="10966-164">頁面會隨即開啟，顯示您開發*Web.config*在左方和功能檔案已部署*Web.config*檔案看起來會像在右側，變更反白顯示。</span><span class="sxs-lookup"><span data-stu-id="10966-164">A page opens that shows you the development *Web.config* file on the left and what the deployed *Web.config* file will look like on the right, with changes highlighted.</span></span>

![偵錯轉換預覽](web-config-transformations/_static/image5.png)

![位置轉換預覽](web-config-transformations/_static/image6.png)

<span data-ttu-id="10966-167">(在預覽中，您可能會發現自己撰寫的某些其他變更轉換的： 這些通常並不會影響功能的泛空白字元移除。)</span><span class="sxs-lookup"><span data-stu-id="10966-167">( In the preview, you might notice some additional changes that you didn't write transforms for: these typically involve the removal of white space that doesn't affect functionality.)</span></span>

<span data-ttu-id="10966-168">當您測試網站部署之後時，您也將測試確認授權規則有效。</span><span class="sxs-lookup"><span data-stu-id="10966-168">When you test the site after deployment, you'll also test to verify that the authorization rule is effective.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="10966-169">**安全性注意事項**永遠不會公開在實際執行應用程式中，顯示錯誤詳細資料，或將該資訊儲存在公用位置。</span><span class="sxs-lookup"><span data-stu-id="10966-169">**Security Note** Never display error details to the public in a production application, or store that information in a public location.</span></span> <span data-ttu-id="10966-170">攻擊者可以使用錯誤的資訊來找出站台中的弱點。</span><span class="sxs-lookup"><span data-stu-id="10966-170">Attackers can use error information to discover vulnerabilities in a site.</span></span> <span data-ttu-id="10966-171">如果您使用您自己的應用程式中的 ELMAH，設定 ELMAH 安全性風險降到最低。</span><span class="sxs-lookup"><span data-stu-id="10966-171">If you use ELMAH in your own application, configure ELMAH to minimize security risks.</span></span> <span data-ttu-id="10966-172">在此教學課程中的 ELMAH 範例不應視為建議的設定。</span><span class="sxs-lookup"><span data-stu-id="10966-172">The ELMAH example in this tutorial should not be considered a recommended configuration.</span></span> <span data-ttu-id="10966-173">這是為了說明如何處理應用程式必須能夠在其中建立檔案的資料夾已選擇範例。</span><span class="sxs-lookup"><span data-stu-id="10966-173">It is an example that was chosen in order to illustrate how to handle a folder that the application must be able to create files in.</span></span> <span data-ttu-id="10966-174">如需詳細資訊，請參閱[保護 ELMAH 端點](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)。</span><span class="sxs-lookup"><span data-stu-id="10966-174">For more information, see [securing the ELMAH endpoint](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).</span></span>


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a><span data-ttu-id="10966-175">您會在中處理的設定發行設定檔轉換檔案</span><span class="sxs-lookup"><span data-stu-id="10966-175">A setting that you'll handle in publish profile transformation files</span></span>

<span data-ttu-id="10966-176">常見的案例是讓*Web.config*檔案必須在您將部署到每個環境中不同的設定。</span><span class="sxs-lookup"><span data-stu-id="10966-176">A common scenario is to have *Web.config* file settings that must be different in each environment that you deploy to.</span></span> <span data-ttu-id="10966-177">例如，呼叫 WCF 服務的應用程式可能需要測試和實際執行環境中的不同端點。</span><span class="sxs-lookup"><span data-stu-id="10966-177">For example, an application that calls a WCF service might need a different endpoint in test and production environments.</span></span> <span data-ttu-id="10966-178">Contoso 大學應用程式也包含這類的設定。</span><span class="sxs-lookup"><span data-stu-id="10966-178">The Contoso University application includes a setting of this kind also.</span></span> <span data-ttu-id="10966-179">此設定會控制，告訴您的環境，您會在中，例如開發、 測試或實際執行的網站頁面上的可見指示。</span><span class="sxs-lookup"><span data-stu-id="10966-179">This setting controls a visible indicator on a site's pages that tells you which environment you are in, such as development, test, or production.</span></span> <span data-ttu-id="10966-180">設定值會決定應用程式是否將會附加 「 （開發） 」 或 「 （測試） 」 中主要的標題以*Site.Master*主版頁面：</span><span class="sxs-lookup"><span data-stu-id="10966-180">The setting value determines whether the application will append "(Dev)" or "(Test)" to the main heading in the *Site.Master* master page:</span></span>

![環境標記](web-config-transformations/_static/image7.png)

<span data-ttu-id="10966-182">當預備或生產環境中執行應用程式時，會省略環境指標。</span><span class="sxs-lookup"><span data-stu-id="10966-182">The environment indicator is omitted when the application is running in staging or production.</span></span>

<span data-ttu-id="10966-183">Contoso 大學網頁讀取的設定中的值`appSettings`中*Web.config*檔案以判斷哪些應用程式正在執行中的環境：</span><span class="sxs-lookup"><span data-stu-id="10966-183">The Contoso University web pages read a value that is set in `appSettings` in the *Web.config* file in order to determine what environment the application is running in:</span></span>

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

<span data-ttu-id="10966-184">值應該是測試環境中，「 測試 」，而且"生產環境 」 針對預備和生產環境。</span><span class="sxs-lookup"><span data-stu-id="10966-184">The value should be "Test" in the test environment, and "Prod" for staging and production.</span></span>

<span data-ttu-id="10966-185">轉換檔案中的下列程式碼會實作這項轉換：</span><span class="sxs-lookup"><span data-stu-id="10966-185">The following code in a transform file will implement this transformation:</span></span>

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

<span data-ttu-id="10966-186">`xdt:Transform`屬性的值，"SetAttributes"表示這項轉換的目的是要變更屬性值中的現有項目*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="10966-186">The `xdt:Transform` attribute value "SetAttributes" indicates that the purpose of this transform is to change attribute values of an existing element in the *Web.config* file.</span></span> <span data-ttu-id="10966-187">`xdt:Locator`屬性的值"Match(key)"表示要修改的項目是指其`key`屬性相符項目`key`此處指定的屬性。</span><span class="sxs-lookup"><span data-stu-id="10966-187">The `xdt:Locator` attribute value "Match(key)" indicates that the element to be modified is the one whose `key` attribute matches the `key` attribute specified here.</span></span> <span data-ttu-id="10966-188">而只有另一個屬性的`add`項目是`value`，而這就是將變更中已部署*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="10966-188">The only other attribute of the `add` element is `value`, and that is what will be changed in the deployed *Web.config* file.</span></span> <span data-ttu-id="10966-189">程式碼如下所示原因`value`屬性`Environment``appSettings`設為 「 測試 」 的項目*Web.config*部署的檔案。</span><span class="sxs-lookup"><span data-stu-id="10966-189">The code shown here causes the `value` attribute of the `Environment` `appSettings` element to be set to "Test" in the *Web.config* file that is deployed.</span></span>

<span data-ttu-id="10966-190">您尚未建立發行設定檔轉換檔案屬於這個轉換。</span><span class="sxs-lookup"><span data-stu-id="10966-190">This transform belongs in the publish profile transform files, which you haven't created yet.</span></span> <span data-ttu-id="10966-191">您將建立和更新您建立的測試、 預備及生產環境的發行設定檔時，實作這項變更轉換檔案。</span><span class="sxs-lookup"><span data-stu-id="10966-191">You'll create and update the transform files that implement this change when you create the publish profiles for the test, staging, and production environments.</span></span> <span data-ttu-id="10966-192">您將會執行，[部署到 IIS](deploying-to-iis.md)和[部署到生產環境](deploying-to-production.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="10966-192">You'll do that in the [deploy to IIS](deploying-to-iis.md) and [deploy to production](deploying-to-production.md) tutorials.</span></span>

> [!NOTE]
> <span data-ttu-id="10966-193">在此設定，所以`<appSettings>`項目，您有另一個替代方式，針對您要部署至 Azure 應用程式服務，請參閱中的 Web 應用程式時，指定轉換[在 Azure 中的指定 Web.config 設定](#watransforms)前面本主題中。</span><span class="sxs-lookup"><span data-stu-id="10966-193">Because this setting is in the `<appSettings>` element, you have another alternative for specifying the transformation when you're deploying to Web Apps in Azure App Service See [Specifying Web.config settings in Azure](#watransforms) earlier in this topic.</span></span>


## <a name="setting-connection-strings"></a><span data-ttu-id="10966-194">設定連接字串</span><span class="sxs-lookup"><span data-stu-id="10966-194">Setting connection strings</span></span>

<span data-ttu-id="10966-195">雖然預設轉換檔案中包含的範例會示範如何更新連接字串，在大部分情況下您不需要設定連接字串轉換，因為您可以指定連接字串中的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="10966-195">Although the default transform file contains an example that shows how to update a connection string, in most cases you do not need to set up connection string transformations, because you can specify connection strings in the publish profile.</span></span> <span data-ttu-id="10966-196">您將會執行，[部署到 IIS](deploying-to-iis.md)和[部署到生產環境](deploying-to-production.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="10966-196">You'll do that in the [deploy to IIS](deploying-to-iis.md) and [deploy to production](deploying-to-production.md) tutorials.</span></span>

## <a name="summary"></a><span data-ttu-id="10966-197">總結</span><span class="sxs-lookup"><span data-stu-id="10966-197">Summary</span></span>

<span data-ttu-id="10966-198">現在您已經盡您可以使用*Web.config*之前建立的發行設定檔，以及您所見的已部署 Web.config 檔案中所要預覽的轉換。</span><span class="sxs-lookup"><span data-stu-id="10966-198">You have now done as much as you can with *Web.config* transformations before you create the publish profiles, and you've seen a preview of what will be in the deployed Web.config file.</span></span>

![位置轉換預覽](web-config-transformations/_static/image8.png)

<span data-ttu-id="10966-200">下列教學課程中，您將處理的部署需要設定專案屬性的設定工作。</span><span class="sxs-lookup"><span data-stu-id="10966-200">In the following tutorial, you'll take care of deployment set-up tasks that require setting project properties.</span></span>

## <a name="more-information"></a><span data-ttu-id="10966-201">更多資訊</span><span class="sxs-lookup"><span data-stu-id="10966-201">More Information</span></span>

<span data-ttu-id="10966-202">如需本教學課程所涵蓋之主題的詳細資訊，請參閱[變更目的地 Web.config 檔或 app.config 檔案中的設定，在部署期間使用 Web.config 轉換](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms)中 Web 部署的內容對應Visual Studio 和 ASP.NET。</span><span class="sxs-lookup"><span data-stu-id="10966-202">For more information about topics covered by this tutorial, see [Using Web.config transformations to change settings in the destination Web.config file or app.config file during deployment](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) in the Web Deployment Content Map for Visual Studio and ASP.NET.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="10966-203">[上一頁](preparing-databases.md)
> [下一頁](project-properties.md)</span><span class="sxs-lookup"><span data-stu-id="10966-203">[Previous](preparing-databases.md)
[Next](project-properties.md)</span></span>
