---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 使用 Visual Studio 的 ASP.NET Web 部署： Web.config 檔轉換 |Microsoft Docs
author: tdykstra
description: 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: 4d8a7d2a6faa0b03fff4416778101b47df2dd26e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371761"
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a><span data-ttu-id="22168-103">使用 Visual Studio 的 ASP.NET Web 部署： Web.config 檔案轉換</span><span class="sxs-lookup"><span data-stu-id="22168-103">ASP.NET Web Deployment using Visual Studio: Web.config File Transformations</span></span>
====================
<span data-ttu-id="22168-104">藉由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="22168-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="22168-105">下載入門專案</span><span class="sxs-lookup"><span data-stu-id="22168-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="22168-106">本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="22168-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="22168-107">這個系列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="22168-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="22168-108">總覽</span><span class="sxs-lookup"><span data-stu-id="22168-108">Overview</span></span>

<span data-ttu-id="22168-109">本教學課程會示範如何將變更的程序自動化*Web.config*檔案時將它部署至不同目的地環境。</span><span class="sxs-lookup"><span data-stu-id="22168-109">This tutorial shows you how to automate the process of changing the *Web.config* file when you deploy it to different destination environments.</span></span> <span data-ttu-id="22168-110">大部分的應用程式中有設定*Web.config*應用程式部署時必須是不同的檔案。</span><span class="sxs-lookup"><span data-stu-id="22168-110">Most applications have settings in the *Web.config* file that must be different when the application is deployed.</span></span> <span data-ttu-id="22168-111">自動化程序可確保這些變更讓您不必手動執行它們，每次部署，這是很麻煩又容易出錯。</span><span class="sxs-lookup"><span data-stu-id="22168-111">Automating the process of making these changes keeps you from having to do them manually every time you deploy, which would be tedious and error prone.</span></span>

<span data-ttu-id="22168-112">提醒： 如果您收到錯誤訊息，或當您瀏覽本教學課程，項目無法運作，請務必檢查[疑難排解頁面](troubleshooting.md)。</span><span class="sxs-lookup"><span data-stu-id="22168-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a><span data-ttu-id="22168-113">與 Web 部署參數的 Web.config 轉換</span><span class="sxs-lookup"><span data-stu-id="22168-113">Web.config transformations versus Web Deploy parameters</span></span>

<span data-ttu-id="22168-114">有兩種方式可將變更的程序自動化*Web.config*檔案設定： [Web.config 轉換](https://msdn.microsoft.com/library/dd465326.aspx)並[Web Deploy 參數](https://msdn.microsoft.com/library/ff398068.aspx)。</span><span class="sxs-lookup"><span data-stu-id="22168-114">There are two ways to automate the process of changing *Web.config* file settings: [Web.config transformations](https://msdn.microsoft.com/library/dd465326.aspx) and [Web Deploy parameters](https://msdn.microsoft.com/library/ff398068.aspx).</span></span> <span data-ttu-id="22168-115">A *Web.config*轉換檔案包含指定如何變更的 XML 標記*Web.config*時部署檔案。</span><span class="sxs-lookup"><span data-stu-id="22168-115">A *Web.config* transformation file contains XML markup that specifies how to change the *Web.config* file when it is deployed.</span></span> <span data-ttu-id="22168-116">您可以指定不同的變更，針對特定組建組態，以及針對特定發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="22168-116">You can specify different changes for specific build configurations and for specific publish profiles.</span></span> <span data-ttu-id="22168-117">預設組建組態偵錯和發行，而且您可以建立自訂組建組態。</span><span class="sxs-lookup"><span data-stu-id="22168-117">The default build configurations are Debug and Release, and you can create custom build configurations.</span></span> <span data-ttu-id="22168-118">發行設定檔通常會對應到目的地環境。</span><span class="sxs-lookup"><span data-stu-id="22168-118">A publish profile typically corresponds to a destination environment.</span></span> <span data-ttu-id="22168-119">(您將了解發行中的設定檔[部署到 IIS 作為測試環境](deploying-to-iis.md)教學課程。)</span><span class="sxs-lookup"><span data-stu-id="22168-119">(You'll learn more about publish profiles in the [Deploying to IIS as a Test Environment](deploying-to-iis.md) tutorial.)</span></span>

<span data-ttu-id="22168-120">Web 部署參數可用來指定許多不同類型的設定必須設定在部署期間，包括設定中找到*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="22168-120">Web Deploy parameters can be used to specify many different kinds of settings that must be configured during deployment, including settings that are found in *Web.config* files.</span></span> <span data-ttu-id="22168-121">當用來指定*Web.config*檔案變更 Web 部署參數是更複雜的設定，但其適用時您不知道您將部署之前設定的值。</span><span class="sxs-lookup"><span data-stu-id="22168-121">When used to specify *Web.config* file changes, Web Deploy parameters are more complex to set up, but they are useful when you do not know the value to be set until you deploy.</span></span> <span data-ttu-id="22168-122">例如，在企業環境中，您可能會建立*部署套件*給予 IT 部門的人員，將安裝在生產環境，以及該人員具有能夠輸入連接字串或不這麼做的密碼了解。</span><span class="sxs-lookup"><span data-stu-id="22168-122">For example, in an enterprise environment, you might create a *deployment package* and give it to a person in the IT department to install in production, and that person has to be able to enter connection strings or passwords that you do not know.</span></span>

<span data-ttu-id="22168-123">本教學課程系列涵蓋的案例中，您事先知道的所有項目完成才能*Web.config*檔案，因此您不需要使用 Web Deploy 的參數。</span><span class="sxs-lookup"><span data-stu-id="22168-123">For the scenario that this tutorial series covers, you know in advance everything that has to be done to the *Web.config* file, so you do not need to use Web Deploy parameters.</span></span> <span data-ttu-id="22168-124">您需要設定一些轉換，而有所不同，組建組態和一些使用發行設定檔而有所不同。</span><span class="sxs-lookup"><span data-stu-id="22168-124">You'll configure some transformations that differ depending on the build configuration used, and some that differ depending on the publish profile used.</span></span>

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a><span data-ttu-id="22168-125">在 Azure 中的指定 Web.config 設定</span><span class="sxs-lookup"><span data-stu-id="22168-125">Specifying Web.config settings in Azure</span></span>

<span data-ttu-id="22168-126">如果*Web.config*您想要變更的檔案設定位於`<connectionStrings>`或`<appSettings>`項目，而且如果您要部署至 Azure App Service 中的 Web 應用程式，也可用於自動化期間的變更另一個選項部署。</span><span class="sxs-lookup"><span data-stu-id="22168-126">If the *Web.config* file settings that you want to change are in the `<connectionStrings>` or the `<appSettings>` element, and if you are deploying to Web Apps in Azure App Service, you have another option for automating changes during deployment.</span></span> <span data-ttu-id="22168-127">您可以輸入的設定，您想要在 Azure 中生效**設定**管理入口網站頁面 web 應用程式 索引標籤 (向下捲動至**應用程式設定**和**連接字串**章節)。</span><span class="sxs-lookup"><span data-stu-id="22168-127">You can enter the settings that you want to take effect in Azure in the **Configure** tab of the management portal page for your web app (scroll down to the **app settings** and **connection strings** sections).</span></span> <span data-ttu-id="22168-128">當您部署專案時，Azure 會自動套用所做的變更。</span><span class="sxs-lookup"><span data-stu-id="22168-128">When you deploy the project, Azure automatically applies the changes.</span></span> <span data-ttu-id="22168-129">如需詳細資訊，請參閱 < [Windows Azure 網站： 應用程式字串與連接字串的運作](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx)。</span><span class="sxs-lookup"><span data-stu-id="22168-129">For more information, see [Windows Azure Web Sites: How Application Strings and Connection Strings Work](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).</span></span>

## <a name="default-transformation-files"></a><span data-ttu-id="22168-130">預設轉換檔案</span><span class="sxs-lookup"><span data-stu-id="22168-130">Default transformation files</span></span>

<span data-ttu-id="22168-131">在**方案總管 中**，展開*Web.config*若要查看*Web.Debug.config*並*Web.Release.config*轉換檔案會建立兩個預設組建組態的預設值。</span><span class="sxs-lookup"><span data-stu-id="22168-131">In **Solution Explorer**, expand *Web.config* to see the *Web.Debug.config* and *Web.Release.config* transformation files that are created by default for the two default build configurations.</span></span>

![Web.config_transform_files](web-config-transformations/_static/image1.png)

<span data-ttu-id="22168-133">您可以建立自訂組建組態的轉換檔案，用滑鼠右鍵按一下 Web.config 檔案，然後選擇**新增設定轉換**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="22168-133">You can create transformation files for custom build configurations by right-clicking the Web.config file and choosing **Add Config Transforms** from the context menu.</span></span> <span data-ttu-id="22168-134">本教學課程中，您不需要這麼做，和功能表選項會停用，因為您尚未建立任何自訂的組建組態。</span><span class="sxs-lookup"><span data-stu-id="22168-134">For this tutorial you don't need to do that, and the menu option is disabled, because you haven't created any custom build configurations.</span></span>

<span data-ttu-id="22168-135">稍後您將建立三個多個轉換檔案，每個測試、 預備和生產環境的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="22168-135">Later you'll create three more transformation files, one each for the test, staging, and production publish profiles.</span></span> <span data-ttu-id="22168-136">典型的範例，因為這取決於目的地環境，您會處理在發行設定檔轉換檔中的設定是針對測試與生產環境不同的 WCF 端點。</span><span class="sxs-lookup"><span data-stu-id="22168-136">A typical example of a setting that you would handle in a publish profile transformation file because it depends on the destination environment is a WCF endpoint that is different for test versus production.</span></span> <span data-ttu-id="22168-137">您將建立發行設定檔轉換檔案中稍後的教學課程之後建立它們所採用的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="22168-137">You'll create publish profile transformation files in later tutorials after you create the publish profiles that they go with.</span></span>

## <a name="disable-debug-mode"></a><span data-ttu-id="22168-138">停用偵錯模式</span><span class="sxs-lookup"><span data-stu-id="22168-138">Disable debug mode</span></span>

<span data-ttu-id="22168-139">設定相依於組建組態，而不是目的地環境的範例`debug`屬性。</span><span class="sxs-lookup"><span data-stu-id="22168-139">An example of a setting that depends on build configuration rather than destination environment is the `debug` attribute.</span></span> <span data-ttu-id="22168-140">在發行組建中，您通常會想停用偵錯不論哪一個環境中，您要部署到。</span><span class="sxs-lookup"><span data-stu-id="22168-140">For a Release build, you typically want debugging disabled regardless of which environment you are deploying to.</span></span> <span data-ttu-id="22168-141">因此，根據預設 Visual Studio 專案範本建立*Web.Release.config*轉換以移除的程式碼的檔案`debug`屬性從`compilation`項目。</span><span class="sxs-lookup"><span data-stu-id="22168-141">Therefore, by default the Visual Studio project templates create *Web.Release.config* transform files with code that removes the `debug` attribute from the `compilation` element.</span></span> <span data-ttu-id="22168-142">以下是預設值*Web.Release.config*： 除了一些轉換程式碼範例標記為註解，包含在程式碼`compilation`移除的項目`debug`屬性：</span><span class="sxs-lookup"><span data-stu-id="22168-142">Here is the default *Web.Release.config*: in addition to some sample transformation code that is commented out, it includes code in the `compilation` element that removes the `debug` attribute:</span></span>

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

<span data-ttu-id="22168-143">`xdt:Transform="RemoveAttributes(debug)"`屬性會指定您想要`debug`屬性，以移除`system.web/compilation`中已部署的項目*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="22168-143">The `xdt:Transform="RemoveAttributes(debug)"` attribute specifies that you want the `debug` attribute to be removed from the `system.web/compilation` element in the deployed *Web.config* file.</span></span> <span data-ttu-id="22168-144">這會在每次您部署的發行組建完成。</span><span class="sxs-lookup"><span data-stu-id="22168-144">This will be done every time you deploy a Release build.</span></span>

## <a name="limit-error-log-access-to-administrators"></a><span data-ttu-id="22168-145">限制存取系統管理員的錯誤記錄檔</span><span class="sxs-lookup"><span data-stu-id="22168-145">Limit error log access to administrators</span></span>

<span data-ttu-id="22168-146">如果沒有錯誤的應用程式執行時，應用程式會顯示一般錯誤頁面，以系統產生的錯誤 頁面中，取代，而且它會使用[Elmah NuGet 套件](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx)記錄和報告的錯誤。</span><span class="sxs-lookup"><span data-stu-id="22168-146">If there's an error while the application runs, the application displays a generic error page in place of the system-generated error page, and it uses the [Elmah NuGet package](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) for error logging and reporting.</span></span> <span data-ttu-id="22168-147">`customErrors`應用程式中的項目*Web.config*檔案會指定錯誤頁面：</span><span class="sxs-lookup"><span data-stu-id="22168-147">The `customErrors` element in the application *Web.config* file specifies the error page:</span></span>

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

<span data-ttu-id="22168-148">若要查看錯誤頁面，暫時變更`mode`屬性的`customErrors`"RemoteOnly"，為 [開啟]，然後執行應用程式從 Visual Studio 中的元素。</span><span class="sxs-lookup"><span data-stu-id="22168-148">To see the error page, temporarily change the `mode` attribute of the `customErrors` element from "RemoteOnly" to "On" and run the application from Visual Studio.</span></span> <span data-ttu-id="22168-149">這類要求無效的 URL，而造成錯誤*Studentsxxx.aspx*。</span><span class="sxs-lookup"><span data-stu-id="22168-149">Cause an error by requesting an invalid URL, such as *Studentsxxx.aspx*.</span></span> <span data-ttu-id="22168-150">而非 IIS 產生 「 資源無法找到 」 的錯誤頁面上，您會看到*GenericErrorPage.aspx*頁面。</span><span class="sxs-lookup"><span data-stu-id="22168-150">Instead of an IIS-generated "The resource cannot be found" error page, you see the *GenericErrorPage.aspx* page.</span></span>

![錯誤頁面](web-config-transformations/_static/image2.png)

<span data-ttu-id="22168-152">若要查看錯誤記錄檔，取代 URL 中的所有項目之後使用的連接埠號碼*elmah.axd* (比方說， `http://localhost:51130/elmah.axd`) 然後按 Enter:</span><span class="sxs-lookup"><span data-stu-id="22168-152">To see the error log, replace everything in the URL after the port number with *elmah.axd* (for example, `http://localhost:51130/elmah.axd`) and press Enter:</span></span>

![ELMAH 頁面](web-config-transformations/_static/image3.png)

<span data-ttu-id="22168-154">別忘了設定`customErrors`回"RemoteOnly"模式完成後的項目。</span><span class="sxs-lookup"><span data-stu-id="22168-154">Don't forget to set the `customErrors` element back to "RemoteOnly" mode when you're done.</span></span>

<span data-ttu-id="22168-155">在您的開發電腦上就很方便地讓 [錯誤記錄] 頁面中，但會有安全性風險的生產環境中的免費存取。</span><span class="sxs-lookup"><span data-stu-id="22168-155">On your development computer it's convenient to allow free access to the error log page, but in production that would be a security risk.</span></span> <span data-ttu-id="22168-156">為生產網站中，您想要新增授權規則，將錯誤記錄檔的存取限制為系統管理員，並確定，限制適用於您想在測試和預備也。</span><span class="sxs-lookup"><span data-stu-id="22168-156">For the production site, you want to add an authorization rule that restricts error log access to administrators, and to make sure that the restriction works you want it in test and staging also.</span></span> <span data-ttu-id="22168-157">因此這是您想要實作每次部署發行組建裡，因此其所屬的另一個變更*Web.Release.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="22168-157">Therefore this is another change that you want to implement every time you deploy a Release build, and so it belongs in the *Web.Release.config* file.</span></span>

<span data-ttu-id="22168-158">開啟*Web.Release.config*再加入新`location`項目結束之前，立即`configuration`標記，如下所示。</span><span class="sxs-lookup"><span data-stu-id="22168-158">Open *Web.Release.config* and add a new `location` element immediately before the closing `configuration` tag, as shown here.</span></span>

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

<span data-ttu-id="22168-159">`Transform` "Insert"屬性值會導致這`location`成為同層級加入至任何現有的項目`location`中的項目*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="22168-159">The `Transform` attribute value of "Insert" causes this `location` element to be added as a sibling to any existing `location` elements in the *Web.config* file.</span></span> <span data-ttu-id="22168-160">(有一個已經`location`項目，指定授權規則**更新信用額度**頁面。)</span><span class="sxs-lookup"><span data-stu-id="22168-160">(There is already one `location` element that specifies authorization rules for the **Update Credits** page.)</span></span>

<span data-ttu-id="22168-161">現在您可以預覽要確定您正確地編寫它的轉換。</span><span class="sxs-lookup"><span data-stu-id="22168-161">Now you can preview the transform to make sure that you coded it correctly.</span></span>

<span data-ttu-id="22168-162">在 **方案總管 中**，以滑鼠右鍵按一下*Web.Release.config* ，按一下 **預覽轉換**。</span><span class="sxs-lookup"><span data-stu-id="22168-162">In **Solution Explorer**, right-click *Web.Release.config* and click **Preview Transform**.</span></span>

![預覽轉換功能表](web-config-transformations/_static/image4.png)

<span data-ttu-id="22168-164">頁面隨即開啟，顯示您開發*Web.config*左邊和項目上的檔案已部署*Web.config*檔案會看起來像右側以反白顯示的變更。</span><span class="sxs-lookup"><span data-stu-id="22168-164">A page opens that shows you the development *Web.config* file on the left and what the deployed *Web.config* file will look like on the right, with changes highlighted.</span></span>

![偵錯轉換的預覽](web-config-transformations/_static/image5.png)

![位置轉換預覽](web-config-transformations/_static/image6.png)

<span data-ttu-id="22168-167">(在預覽中，您可能會發現不是您撰寫一些額外變更轉換： 這些通常牽涉到將不會影響功能的泛空白字元移除。)</span><span class="sxs-lookup"><span data-stu-id="22168-167">( In the preview, you might notice some additional changes that you didn't write transforms for: these typically involve the removal of white space that doesn't affect functionality.)</span></span>

<span data-ttu-id="22168-168">當您測試網站，在部署後時，您也將測試確認授權規則有效。</span><span class="sxs-lookup"><span data-stu-id="22168-168">When you test the site after deployment, you'll also test to verify that the authorization rule is effective.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="22168-169">**安全性注意事項**永遠不會公開在生產環境應用程式中，顯示錯誤詳細資料，或將該資訊儲存在公用位置。</span><span class="sxs-lookup"><span data-stu-id="22168-169">**Security Note** Never display error details to the public in a production application, or store that information in a public location.</span></span> <span data-ttu-id="22168-170">若要探索的站台中的弱點，攻擊者可以使用資訊時發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="22168-170">Attackers can use error information to discover vulnerabilities in a site.</span></span> <span data-ttu-id="22168-171">如果您在自己的應用程式中使用 ELMAH，設定 ELMAH 安全性風險降到最低。</span><span class="sxs-lookup"><span data-stu-id="22168-171">If you use ELMAH in your own application, configure ELMAH to minimize security risks.</span></span> <span data-ttu-id="22168-172">在本教學課程的 ELMAH 範例不應視為建議的設定。</span><span class="sxs-lookup"><span data-stu-id="22168-172">The ELMAH example in this tutorial should not be considered a recommended configuration.</span></span> <span data-ttu-id="22168-173">例如，選擇以說明如何處理應用程式必須能夠在其中建立檔案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="22168-173">It is an example that was chosen in order to illustrate how to handle a folder that the application must be able to create files in.</span></span> <span data-ttu-id="22168-174">如需詳細資訊，請參閱 <<c0> [ 保護的 ELMAH 端點](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)。</span><span class="sxs-lookup"><span data-stu-id="22168-174">For more information, see [securing the ELMAH endpoint](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).</span></span>


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a><span data-ttu-id="22168-175">您將會處理在設定發行設定檔轉換檔案</span><span class="sxs-lookup"><span data-stu-id="22168-175">A setting that you'll handle in publish profile transformation files</span></span>

<span data-ttu-id="22168-176">常見的案例是有*Web.config*檔案必須在您將部署到每個環境中不同的設定。</span><span class="sxs-lookup"><span data-stu-id="22168-176">A common scenario is to have *Web.config* file settings that must be different in each environment that you deploy to.</span></span> <span data-ttu-id="22168-177">例如，呼叫 WCF 服務的應用程式可能需要在測試和生產環境不同的端點。</span><span class="sxs-lookup"><span data-stu-id="22168-177">For example, an application that calls a WCF service might need a different endpoint in test and production environments.</span></span> <span data-ttu-id="22168-178">Contoso 大學應用程式也包含這類的設定。</span><span class="sxs-lookup"><span data-stu-id="22168-178">The Contoso University application includes a setting of this kind also.</span></span> <span data-ttu-id="22168-179">此設定控制，告訴您哪一個環境，您會在中，例如開發、 測試或生產環境的站台的頁面上的可見指示。</span><span class="sxs-lookup"><span data-stu-id="22168-179">This setting controls a visible indicator on a site's pages that tells you which environment you are in, such as development, test, or production.</span></span> <span data-ttu-id="22168-180">設定值會決定應用程式是否將會附加"(Dev)"或"（測試） 」 的主標題中要*Site.Master*主版頁面：</span><span class="sxs-lookup"><span data-stu-id="22168-180">The setting value determines whether the application will append "(Dev)" or "(Test)" to the main heading in the *Site.Master* master page:</span></span>

![環境標記](web-config-transformations/_static/image7.png)

<span data-ttu-id="22168-182">在預備環境或生產環境中執行應用程式，則會忽略環境指標。</span><span class="sxs-lookup"><span data-stu-id="22168-182">The environment indicator is omitted when the application is running in staging or production.</span></span>

<span data-ttu-id="22168-183">Contoso 大學 web 網頁讀取的值中設定`appSettings`中*Web.config*檔案以判斷哪些應用程式正在執行的環境：</span><span class="sxs-lookup"><span data-stu-id="22168-183">The Contoso University web pages read a value that is set in `appSettings` in the *Web.config* file in order to determine what environment the application is running in:</span></span>

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

<span data-ttu-id="22168-184">值應該是測試環境中，「 測試 」 和 「 生產 」 來預備和生產環境。</span><span class="sxs-lookup"><span data-stu-id="22168-184">The value should be "Test" in the test environment, and "Prod" for staging and production.</span></span>

<span data-ttu-id="22168-185">轉換檔案中的下列程式碼會實作這項轉換：</span><span class="sxs-lookup"><span data-stu-id="22168-185">The following code in a transform file will implement this transformation:</span></span>

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

<span data-ttu-id="22168-186">`xdt:Transform`屬性的值，"SetAttributes"表示這個轉換的目的是要變更屬性值中的現有項目*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="22168-186">The `xdt:Transform` attribute value "SetAttributes" indicates that the purpose of this transform is to change attribute values of an existing element in the *Web.config* file.</span></span> <span data-ttu-id="22168-187">`xdt:Locator`屬性的值"Match(key)"表示要修改的項目是一個其`key`屬性相符項目`key`此處指定的屬性。</span><span class="sxs-lookup"><span data-stu-id="22168-187">The `xdt:Locator` attribute value "Match(key)" indicates that the element to be modified is the one whose `key` attribute matches the `key` attribute specified here.</span></span> <span data-ttu-id="22168-188">只有其他的屬性`add`項目是`value`，這將會變更的項目中已部署*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="22168-188">The only other attribute of the `add` element is `value`, and that is what will be changed in the deployed *Web.config* file.</span></span> <span data-ttu-id="22168-189">程式碼如下所示的原因`value`的屬性`Environment``appSettings`會設為"Test"中的項目*Web.config*部署的檔案。</span><span class="sxs-lookup"><span data-stu-id="22168-189">The code shown here causes the `value` attribute of the `Environment` `appSettings` element to be set to "Test" in the *Web.config* file that is deployed.</span></span>

<span data-ttu-id="22168-190">這項轉換屬於尚未建立發行設定檔轉換檔案。</span><span class="sxs-lookup"><span data-stu-id="22168-190">This transform belongs in the publish profile transform files, which you haven't created yet.</span></span> <span data-ttu-id="22168-191">您會建立並更新實作這項變更，當您建立的測試、 預備及生產環境的發行設定檔的轉換檔案。</span><span class="sxs-lookup"><span data-stu-id="22168-191">You'll create and update the transform files that implement this change when you create the publish profiles for the test, staging, and production environments.</span></span> <span data-ttu-id="22168-192">您一下[部署至 IIS](deploying-to-iis.md)並[部署到生產環境](deploying-to-production.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="22168-192">You'll do that in the [deploy to IIS](deploying-to-iis.md) and [deploy to production](deploying-to-production.md) tutorials.</span></span>

> [!NOTE]
> <span data-ttu-id="22168-193">因為此設定位在`<appSettings>`項目，您有另一個替代方式，在部署 Azure 應用程式服務，請參閱中的 Web 應用程式時，指定轉換[在 Azure 中的指定 Web.config 設定](#watransforms)稍早的本主題中。</span><span class="sxs-lookup"><span data-stu-id="22168-193">Because this setting is in the `<appSettings>` element, you have another alternative for specifying the transformation when you're deploying to Web Apps in Azure App Service See [Specifying Web.config settings in Azure](#watransforms) earlier in this topic.</span></span>


## <a name="setting-connection-strings"></a><span data-ttu-id="22168-194">設定連接字串</span><span class="sxs-lookup"><span data-stu-id="22168-194">Setting connection strings</span></span>

<span data-ttu-id="22168-195">雖然預設轉換檔包含的範例，示範如何更新連接字串，在大部分情況下您不需要設定連接字串的轉換，因為您可以在發行設定檔中指定連接字串。</span><span class="sxs-lookup"><span data-stu-id="22168-195">Although the default transform file contains an example that shows how to update a connection string, in most cases you do not need to set up connection string transformations, because you can specify connection strings in the publish profile.</span></span> <span data-ttu-id="22168-196">您一下[部署至 IIS](deploying-to-iis.md)並[部署到生產環境](deploying-to-production.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="22168-196">You'll do that in the [deploy to IIS](deploying-to-iis.md) and [deploy to production](deploying-to-production.md) tutorials.</span></span>

## <a name="summary"></a><span data-ttu-id="22168-197">總結</span><span class="sxs-lookup"><span data-stu-id="22168-197">Summary</span></span>

<span data-ttu-id="22168-198">現在您已經儘可能地使用*Web.config*轉換之前建立的發行設定檔，以及您已了解所要部署的 Web.config 檔案中的預覽。</span><span class="sxs-lookup"><span data-stu-id="22168-198">You have now done as much as you can with *Web.config* transformations before you create the publish profiles, and you've seen a preview of what will be in the deployed Web.config file.</span></span>

![位置轉換預覽](web-config-transformations/_static/image8.png)

<span data-ttu-id="22168-200">在下列教學課程中，您將負責部署需要設定專案屬性的設定工作。</span><span class="sxs-lookup"><span data-stu-id="22168-200">In the following tutorial, you'll take care of deployment set-up tasks that require setting project properties.</span></span>

## <a name="more-information"></a><span data-ttu-id="22168-201">更多資訊</span><span class="sxs-lookup"><span data-stu-id="22168-201">More Information</span></span>

<span data-ttu-id="22168-202">如需本教學課程所涵蓋的主題的詳細資訊，請參閱[若要變更目的 Web.config 檔案或 app.config 檔案中的設定，在部署期間使用 Web.config 轉換](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms)在 Web 部署的內容對應Visual Studio 及 ASP.NET。</span><span class="sxs-lookup"><span data-stu-id="22168-202">For more information about topics covered by this tutorial, see [Using Web.config transformations to change settings in the destination Web.config file or app.config file during deployment](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) in the Web Deployment Content Map for Visual Studio and ASP.NET.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="22168-203">[上一頁](preparing-databases.md)
> [下一頁](project-properties.md)</span><span class="sxs-lookup"><span data-stu-id="22168-203">[Previous](preparing-databases.md)
[Next](project-properties.md)</span></span>
