---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: 使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 12 個 6-設定資料夾權限 |Microsoft Docs
author: tdykstra
description: 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫，使用 Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 81d5107c7e25a10218d13175c47913c94e9ab3fd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832236"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 12 個 6-設定資料夾權限
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 包含 SQL Server Compact 資料庫。 如果您安裝 Web Publish Update，您也可以使用 Visual Studio 2010。 在數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 顯示 Visual Studio 2012 RC 版本之後引入的部署功能，示範如何部署 SQL Server Compact，以外的 SQL Server 版本，並示範如何部署至 Azure App Service Web Apps 的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>總覽

在本教學課程中，您可以設定資料夾權限*Elmah*資料夾中已部署的 web 站台，讓應用程式可以在該資料夾中建立記錄檔。

當您使用 Visual Studio 程式開發伺服器 (Cassini) 的 Visual Studio 中測試 web 應用程式時，則會在您的身分識別下執行應用程式。 您最有可能是您開發電腦上的系統管理員，並具有完整權限，採取任何動作來的任何資料夾中的任何檔案。 但是，當應用程式在 IIS 下執行，它會執行在站台指派給應用程式集區所定義的識別。 這通常是有限的權限的系統定義的帳戶。 依預設它具有讀取和執行 web 應用程式的檔案和資料夾的權限，但它沒有寫入權限。

如果您的應用程式建立或更新檔案，這是一般需要在 web 應用程式，這會成為問題。 在 Contoso 大學應用程式時，Elmah 會建立 XML 檔案中的*Elmah*資料夾，才能儲存有關錯誤的詳細資料。 即使您未使用 Elmah 類似，您的網站可能會讓使用者將檔案上傳，或執行其他工作，將資料寫入至您的網站中的資料夾。

提醒： 如果您收到錯誤訊息，或當您瀏覽本教學課程，項目無法運作，請務必檢查[疑難排解頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="testing-error-logging-and-reporting"></a>測試錯誤記錄和報告

若要查看如何應用程式無法正確運作在 IIS 中 （雖然當您在 Visual Studio 測試一樣），您可能會造成錯誤，通常會記錄由 Elmah，然後再開啟 Elmah 錯誤記錄檔，以查看詳細資料。 無法建立 XML 檔案，並將錯誤詳細資料儲存 Elmah 時，您會看到空白的錯誤報告。

開啟瀏覽器並移至`http://localhost/ContosoUniversity`，然後要求無效的 URL，例如*Studentsxxx.aspx*。 您會看到系統產生的錯誤頁面，而不是*GenericErrorPage.aspx*頁面上，因為`customErrors`Web.config 檔中的設定為"RemoteOnly"，而您在本機執行 IIS:

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

現在，執行*Elmah.axd*來查看錯誤報告。 因為 Elmah 無法建立 XML 檔案中的，您會看到空白的錯誤記錄檔頁面*Elmah*資料夾：

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>設定 Elmah 資料夾的寫入權限

您可以手動設定資料夾權限，或者您可以讓它自動部署程序的一部分。 進行自動需要複雜的 MSBuild 程式碼，而且由於您只需要執行這項操作第一次部署，本教學課程中只會顯示如何以手動方式執行此動作。 (如需如何讓這部分的部署程序的資訊，請參閱[設定資料夾權限，在 Web Publish](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) Sayed Hashimi 部落格上。)

在  **Windows 檔案總管**，瀏覽至*C:\inetpub\wwwroot\ContosoUniversity*。 以滑鼠右鍵按一下*Elmah*資料夾中，選取**屬性**，然後選取**安全性** 索引標籤。

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(如果您沒有看到**DefaultAppPool**中**群組或使用者名稱** 清單中，您可能用比這個教學課程中所指定的其他方法來在您的電腦上設定 IIS 和 ASP.NET 4。 在此情況下，找出哪些身分識別由應用程式集區指派給 Contoso 大學應用程式和寫入權限授與該身分識別。 請參閱有關應用程式集區身分識別連結在本教學課程結尾處）。

按一下 [編輯] 。 在**Elmah 的權限**對話方塊中，選取**DefaultAppPool**，然後選取**撰寫**中的核取方塊**允許**資料行。

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

按一下 **確定**在兩個對話方塊。

## <a name="retesting-error-logging-and-reporting"></a>重新測試錯誤記錄和報告

執行並測試相同的方式 （要求不正確的 URL） 再次導致錯誤發生**錯誤記錄檔**頁面。 目前頁面上，會出現錯誤。

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

您也需要寫入權限*應用程式\_資料*資料夾因為您在該資料夾中，SQL Server Compact 資料庫檔案，而且您想要能夠更新這些資料庫中的資料。 在此情況下，不過，您不必執行任何額外的動作，因為在部署程序會自動設定的寫入權限*應用程式\_資料*資料夾。

您現在已完成所有工作讓 Contoso 大學所需的本機電腦上正常運作在 IIS 中。 在下一個教學課程中，您就會將它部署至主機服務提供者的公開可用的站台。

## <a name="more-information"></a>更多資訊

在此範例中，為什麼 Elmah 無法儲存記錄檔的原因是很明顯。 您可以在問題的原因不明顯; 的情況下使用 IIS 追蹤請參閱[疑難排解失敗的要求使用的追蹤在 IIS 7 中](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis)IIS.net 網站上。

如需如何授與應用程式集區身分識別的權限的詳細資訊，請參閱[應用程式集區身分識別](https://www.iis.net/learn/manage/configuring-security/application-pool-identities)並[在檔案系統 Acl 透過 IIS 中的 內容保護](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls)IIS.net 網站上。

> [!div class="step-by-step"]
> [上一頁](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
> [下一頁](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
