---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: 使用 Visual Studio 的 ASP.NET Web 部署： 設定資料夾權限 |Microsoft 文件
author: tdykstra
description: 此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 7efe267975835e889950983126088f1b637c28fb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a><span data-ttu-id="74ba5-103">使用 Visual Studio 的 ASP.NET Web 部署： 設定資料夾權限</span><span class="sxs-lookup"><span data-stu-id="74ba5-103">ASP.NET Web Deployment using Visual Studio: Setting Folder Permissions</span></span>
====================
<span data-ttu-id="74ba5-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="74ba5-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="74ba5-105">下載入門專案</span><span class="sxs-lookup"><span data-stu-id="74ba5-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="74ba5-106">此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="74ba5-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="74ba5-107">數列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="74ba5-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="74ba5-108">總覽</span><span class="sxs-lookup"><span data-stu-id="74ba5-108">Overview</span></span>

<span data-ttu-id="74ba5-109">在本教學課程中，您可以設定的資料夾權限*Elmah*資料夾中已部署的 web 站台，好讓應用程式可以在該資料夾中建立記錄檔。</span><span class="sxs-lookup"><span data-stu-id="74ba5-109">In this tutorial, you set folder permissions for the *Elmah* folder in the deployed web site so that the application can create log files in that folder.</span></span>

<span data-ttu-id="74ba5-110">當您在 Visual Studio 中使用 Visual Studio 程式開發伺服器 (Cassini) 或 IIS Express 測試 web 應用程式時，您的身分執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="74ba5-110">When you test a web application in Visual Studio using the Visual Studio Development Server (Cassini) or IIS Express, the application runs under your identity.</span></span> <span data-ttu-id="74ba5-111">您最有可能是開發電腦上系統管理員，並具有完整權限執行的任何資料夾中的任何檔案的任何動作。</span><span class="sxs-lookup"><span data-stu-id="74ba5-111">You are most likely an administrator on your development computer and have full authority to do anything to any file in any folder.</span></span> <span data-ttu-id="74ba5-112">但在 IIS 底下執行應用程式，它會在下執行應用程式集區，以指派的站台所定義的識別。</span><span class="sxs-lookup"><span data-stu-id="74ba5-112">But when an application runs under IIS, it runs under the identity defined for the application pool that the site is assigned to.</span></span> <span data-ttu-id="74ba5-113">這通常是有限的權限的系統定義的帳戶。</span><span class="sxs-lookup"><span data-stu-id="74ba5-113">This is typically a system-defined account that has limited permissions.</span></span> <span data-ttu-id="74ba5-114">依預設它具有讀取和執行權限，您的 web 應用程式檔案和資料夾，但沒有寫入權限。</span><span class="sxs-lookup"><span data-stu-id="74ba5-114">By default it has read and execute permissions on your web application's files and folders, but it doesn't have write access.</span></span>

<span data-ttu-id="74ba5-115">如果您的應用程式會建立或更新檔案是一般需要 web 應用程式中，這會造成問題。</span><span class="sxs-lookup"><span data-stu-id="74ba5-115">This becomes an issue if your application creates or updates files, which is a common need in web applications.</span></span> <span data-ttu-id="74ba5-116">Contoso 大學應用程式、 Elmah 建立 XML 檔案中的*Elmah*資料夾，才能儲存有關錯誤的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="74ba5-116">In the Contoso University application, Elmah creates XML files in the *Elmah* folder in order to save details about errors.</span></span> <span data-ttu-id="74ba5-117">即使您不使用 Elmah 類似，您的網站可能讓使用者將檔案上傳，或執行其他工作的資料寫入網站中的資料夾。</span><span class="sxs-lookup"><span data-stu-id="74ba5-117">Even if you don't use something like Elmah, your site might let users upload files or perform other tasks that write data to a folder in your site.</span></span>

<span data-ttu-id="74ba5-118">提示： 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必檢查[疑難排解頁面](troubleshooting.md)。</span><span class="sxs-lookup"><span data-stu-id="74ba5-118">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="test-error-logging-and-reporting"></a><span data-ttu-id="74ba5-119">測試錯誤記錄和報告</span><span class="sxs-lookup"><span data-stu-id="74ba5-119">Test error logging and reporting</span></span>

<span data-ttu-id="74ba5-120">若要查看如何在應用程式無法正常運作在 IIS 中 （雖然您在 Visual Studio 中進行測試時一樣），您可能會造成錯誤，通常會記錄的 Elmah，，然後開啟 Elmah 錯誤記錄檔，以查看詳細資料。</span><span class="sxs-lookup"><span data-stu-id="74ba5-120">To see how the application doesn't work correctly in IIS (although it did when you tested it in Visual Studio), you can cause an error that would normally be logged by Elmah, and then open the Elmah error log to see the details.</span></span> <span data-ttu-id="74ba5-121">如果 Elmah 無法建立 XML 檔案和存放區錯誤詳細資料，您會看到空白的錯誤報告。</span><span class="sxs-lookup"><span data-stu-id="74ba5-121">If Elmah was unable to create an XML file and store the error details, you see an empty error report.</span></span>

<span data-ttu-id="74ba5-122">開啟瀏覽器並移至`http://localhost/ContosoUniversity`，然後要求無效的 URL，例如*Studentsxxx.aspx*。</span><span class="sxs-lookup"><span data-stu-id="74ba5-122">Open a browser and go to `http://localhost/ContosoUniversity`, and then request an invalid URL like *Studentsxxx.aspx*.</span></span> <span data-ttu-id="74ba5-123">您會看到而不是系統產生的錯誤頁面*GenericErrorPage.aspx*頁面上，因為`customErrors`Web.config 檔案中的設定為"RemoteOnly"，而您在本機執行 IIS:</span><span class="sxs-lookup"><span data-stu-id="74ba5-123">You see a system-generated error page instead of the *GenericErrorPage.aspx* page because the `customErrors` setting in the Web.config file is "RemoteOnly" and you are running IIS locally:</span></span>

![HTTP 404 錯誤頁面](setting-folder-permissions/_static/image1.png)

<span data-ttu-id="74ba5-125">現在執行*Elmah.axd*以查看錯誤報告。</span><span class="sxs-lookup"><span data-stu-id="74ba5-125">Now run *Elmah.axd* to see the error report.</span></span> <span data-ttu-id="74ba5-126">您的認證登入系統管理員帳戶之後 (&quot;admin&quot;和&quot;devpwd&quot;)，因為 Elmah 無法建立 XML 檔案中的，您會看到空白錯誤記錄檔頁面*Elmah*資料夾：</span><span class="sxs-lookup"><span data-stu-id="74ba5-126">After you log in with the administrator account credentials (&quot;admin&quot; and &quot;devpwd&quot;), you see an empty error log page because Elmah was unable to create an XML file in the *Elmah* folder:</span></span>

![錯誤記錄檔的空白](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a><span data-ttu-id="74ba5-128">設定 Elmah 資料夾的寫入權限</span><span class="sxs-lookup"><span data-stu-id="74ba5-128">Set write permission on the Elmah folder</span></span>

<span data-ttu-id="74ba5-129">您可以手動設定資料夾權限，或者您可以將它自動在部署程序。</span><span class="sxs-lookup"><span data-stu-id="74ba5-129">You can set folder permissions manually or you can make it an automatic part of the deployment process.</span></span> <span data-ttu-id="74ba5-130">進行自動需要複雜 MSBuild 的程式碼，而且您只需要這樣做您部署的第一次，因為下列步驟以手動的方式。</span><span class="sxs-lookup"><span data-stu-id="74ba5-130">Making it automatic requires complex MSBuild code, and since you only have to do this the first time you deploy, the following steps how to do it manually.</span></span> <span data-ttu-id="74ba5-131">(如需如何讓此組件部署程序的資訊，請參閱[設定資料夾的權限上 Web Publish](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) Sayed Hashimi 部落格上。)</span><span class="sxs-lookup"><span data-stu-id="74ba5-131">(For information about how to make this part of the deployment process, see [Setting Folder Permissions on Web Publish](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) on Sayed Hashimi's blog.)</span></span>

1. <span data-ttu-id="74ba5-132">在**檔案總管**，瀏覽至*C:\inetpub\wwwroot\ContosoUniversity*。</span><span class="sxs-lookup"><span data-stu-id="74ba5-132">In **File Explorer**, navigate to *C:\inetpub\wwwroot\ContosoUniversity*.</span></span> <span data-ttu-id="74ba5-133">以滑鼠右鍵按一下*Elmah*資料夾中，選取**屬性**，然後選取**安全性** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="74ba5-133">Right-click the *Elmah* folder, select **Properties**, and then select the **Security** tab.</span></span>
2. <span data-ttu-id="74ba5-134">按一下 [編輯] 。</span><span class="sxs-lookup"><span data-stu-id="74ba5-134">Click **Edit**.</span></span>
3. <span data-ttu-id="74ba5-135">在**Elmah 的權限**對話方塊中，選取**DefaultAppPool**，然後選取**寫入**中核取方塊**允許**資料行。</span><span class="sxs-lookup"><span data-stu-id="74ba5-135">In the **Permissions for Elmah** dialog box, select **DefaultAppPool**, and then select the **Write** check box in the **Allow** column.</span></span>

    ![ELMAH 資料夾的權限](setting-folder-permissions/_static/image3.png)

    <span data-ttu-id="74ba5-137">(如果您沒有看到**DefaultAppPool**中**群組或使用者名稱** 清單中，您可能用於本教學課程中所指定的其他方法來在您的電腦上設定 IIS 和 ASP.NET 4。</span><span class="sxs-lookup"><span data-stu-id="74ba5-137">(If you don't see **DefaultAppPool** in the **Group or user names** list, you probably used some other method than the one specified in this tutorial to set up IIS and ASP.NET 4 on your computer.</span></span> <span data-ttu-id="74ba5-138">在此情況下，找出應用程式集區指派給 Contoso 大學應用程式和授與寫入權限給該識別使用何種身分識別。</span><span class="sxs-lookup"><span data-stu-id="74ba5-138">In that case, find out what identity is used by the application pool assigned to the Contoso University application, and grant write permission to that identity.</span></span> <span data-ttu-id="74ba5-139">請參閱有關應用程式集區識別連結在本教學課程結尾處）。按一下**確定**在兩個對話方塊。</span><span class="sxs-lookup"><span data-stu-id="74ba5-139">See the links about application pool identities at the end of this tutorial.) Click **OK** in both dialog boxes.</span></span>

## <a name="retest-error-logging-and-reporting"></a><span data-ttu-id="74ba5-140">重新測試錯誤記錄和報告</span><span class="sxs-lookup"><span data-stu-id="74ba5-140">Retest error logging and reporting</span></span>

<span data-ttu-id="74ba5-141">導致錯誤發生一次相同的方式 （不正確的 URL 要求） 來測試及執行**錯誤記錄檔**頁面。</span><span class="sxs-lookup"><span data-stu-id="74ba5-141">Test by causing an error again in the same way (request a bad URL) and run the **Error Log** page.</span></span> <span data-ttu-id="74ba5-142">目前頁面上，會出現錯誤。</span><span class="sxs-lookup"><span data-stu-id="74ba5-142">This time the error appears on the page.</span></span>

![ELMAH 錯誤記錄檔 頁面](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a><span data-ttu-id="74ba5-144">總結</span><span class="sxs-lookup"><span data-stu-id="74ba5-144">Summary</span></span>

<span data-ttu-id="74ba5-145">您現在已完成的所有工作所需 Contoso 大學在本機電腦上 IIS 中運作正常。</span><span class="sxs-lookup"><span data-stu-id="74ba5-145">You have now completed all of the tasks necessary to get Contoso University working correctly in IIS on your local computer.</span></span> <span data-ttu-id="74ba5-146">在下一個教學課程中，您就會將它部署至 Azure 的公開可用的站台。</span><span class="sxs-lookup"><span data-stu-id="74ba5-146">In the next tutorial, you will make the site publicly available by deploying it to Azure.</span></span>

## <a name="more-information"></a><span data-ttu-id="74ba5-147">詳細資訊</span><span class="sxs-lookup"><span data-stu-id="74ba5-147">More information</span></span>

<span data-ttu-id="74ba5-148">在此範例中，為什麼 Elmah 無法儲存記錄檔的原因，是很明顯。</span><span class="sxs-lookup"><span data-stu-id="74ba5-148">In this example, the reason why Elmah was unable to save log files was fairly obvious.</span></span> <span data-ttu-id="74ba5-149">您可以使用 IIS 追蹤問題的原因不那麼顯而易見。請參閱[疑難排解失敗的要求使用的追蹤在 IIS 7 中](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis)IIS.net 網站上。</span><span class="sxs-lookup"><span data-stu-id="74ba5-149">You can use IIS tracing in cases where the cause of the problem is not so obvious; see [Troubleshooting Failed Requests Using Tracing in IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) on the IIS.net site.</span></span>

<span data-ttu-id="74ba5-150">如需如何授與應用程式集區識別的權限的詳細資訊，請參閱[應用程式集區識別](https://www.iis.net/learn/manage/configuring-security/application-pool-identities)和[安全的內容在檔案系統 Acl 透過 IIS](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) IIS.net 網站上。</span><span class="sxs-lookup"><span data-stu-id="74ba5-150">For more information about how to grant permissions to application pool identities, see [Application Pool Identities](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) and [Secure Content in IIS Through File System ACLs](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) on the IIS.net site.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="74ba5-151">[上一頁](deploying-to-iis.md)
> [下一頁](deploying-to-production.md)</span><span class="sxs-lookup"><span data-stu-id="74ba5-151">[Previous](deploying-to-iis.md)
[Next](deploying-to-production.md)</span></span>
