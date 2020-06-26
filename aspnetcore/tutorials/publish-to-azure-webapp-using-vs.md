---
title: 使用 Visual Studio 將 ASP.NET Core 應用程式發行到 Azure
author: rick-anderson
description: 了解如何使用 Visual Studio 將 ASP.NET Core 應用程式發行到 Azure App Service。
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: d805d57fd1e2d83d0148900993e4bf6108a13028
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85408405"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-visual-studio"></a>使用 Visual Studio 將 ASP.NET Core 應用程式發行到 Azure

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)
::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

::: moniker-end


如果您正在 macOS，請參閱[使用 Visual Studio for Mac 將 Web 應用程式發佈至 Azure App Service](https://docs.microsoft.com/visualstudio/mac/publish-app-svc?view=vsmac-2019) 。

若要針對 App Service 部署問題進行疑難排解，請參閱 <xref:test/troubleshoot-azure-iis>。

## <a name="set-up"></a>設定

* 如果您沒有帳戶，請開啟[免費的 Azure 帳戶](https://azure.microsoft.com/free/dotnet/)。 

## <a name="create-a-web-app"></a>建立 Web 應用程式

在 Visual Studio 起始畫面中，選取 [檔案] > [新增] > [專案...]****

![[檔案] 功能表](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

完成 [新增專案]**** 對話方塊：

* 選取 **ASP.NET Core Web 應用程式**。
* 選取 [下一步] 。

![[新增專案] 對話方塊](publish-to-azure-webapp-using-vs/_static/new_prj.png)

在 [新增 ASP.NET Core Web 應用程式]**** 對話方塊中：

* 選取 [ **Web 應用程式**]。
* 選取 [驗證] 底下的 [**變更**]。

![新增 ASP.NET Core Web 對話方塊](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

[變更驗證]**** 對話方塊會隨即出現。 

* 選取 [個別使用者帳戶]****。
* 選取 **[確定]** 以返回**新的 ASP.NET Core Web 應用程式**，然後選取 [**建立**]。

![[新增 ASP.NET Core Web 驗證] 對話方塊](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

Visual Studio 會建立解決方案。

## <a name="run-the-app"></a>執行應用程式

* 按 CTRL+F5 執行專案。
* 測試**隱私權**連結。

![在 Microsoft Edge 中於 localhost 開啟的 Web 應用程式](publish-to-azure-webapp-using-vs/_static/show.png)

### <a name="register-a-user"></a>註冊使用者

* 選取 [註冊]**** 並註冊新的使用者。 您可以使用虛構的電子郵件地址。 提交時，頁面會顯示下列錯誤：

    *「處理要求時資料庫作業失敗。應用程式資料庫內容的現有遷移可能會解決此問題。」*
* 選取 [套用移轉]****，並在頁面更新後重新整理頁面。

![處理要求時資料庫作業失敗。 為應用程式資料庫內容套用現有的移轉可能會解決此問題。](publish-to-azure-webapp-using-vs/_static/mig.png)

應用程式會顯示用來註冊新使用者和**登出**連結的電子郵件。

![在 Microsoft Edge 中開啟的 Web 應用程式。 [註冊] 連結會取代為文字「user1@example.com，您好！」](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a>將應用程式部署至 Azure

在方案總管中，以滑鼠右鍵按一下專案，然後選取 [發行]****。

![反白顯示 [發行] 連結的已開啟操作功能表](publish-to-azure-webapp-using-vs/_static/pub.png)

在 [發行]**** 對話方塊中：

* 選取 [ **Azure**]。
* 選取 [下一步] 。

![發佈對話方塊](publish-to-azure-webapp-using-vs/_static/maas1.png)

在 [發行]**** 對話方塊中：

* 選取 **[Azure App Service （Linux）**]。
* 選取 [下一步] 。

![發佈對話方塊：選取 Azure 服務](publish-to-azure-webapp-using-vs/_static/maas2.png)

在 [**發行**] 對話方塊中，選取 [**建立新的 Azure App Service ...** ]

![發佈對話方塊：選取 Azure 服務實例](publish-to-azure-webapp-using-vs/_static/maas3.png)

[**建立 App Service** ] 對話方塊隨即出現：

* 會填入 [應用程式名稱]****、[資源群組]**** 和 [App Service 方案]**** 輸入欄位。 您可以保留這些名稱，或變更它們。
* 選取 [建立]****。

![建立 App Service 對話方塊](publish-to-azure-webapp-using-vs/_static/newrg1.png)

建立完成後，會自動關閉對話方塊，而且 [**發行**] 對話方塊會再次取得焦點：

* 系統會自動選取剛建立的新實例。
* 選取 [完成]。

![發行對話方塊：選取 App Service 實例](publish-to-azure-webapp-using-vs/_static/select_as.png)

接下來，您會看到 [**發行設定檔摘要**] 頁面。 Visual Studio 偵測到此應用程式需要 SQL Server 資料庫，而且它會要求您進行設定。 選取 [設定] 。

![發行設定檔摘要頁面：設定 SQL Server 相依性](publish-to-azure-webapp-using-vs/_static/sql.png)

[**設定**相依性] 對話方塊隨即出現：

* 選取 [ **Azure SQL Database**]。
* 選取 [下一步] 。

![設定 SQL Server 相依性對話方塊](publish-to-azure-webapp-using-vs/_static/sql1.png)

在 [**設定 AZURE SQL database** ] 對話方塊中，選取 [**建立 SQL Database**

![[設定 Azure SQL Database] 對話方塊](publish-to-azure-webapp-using-vs/_static/sql2.png)

[**建立 Azure SQL Database**隨即出現：

* [**資料庫名稱**]、[**資源群組**]、[**資料庫伺服器**] 和 [ **App Service 方案**專案] 欄位都會填入。 您可以保留這些值或加以變更。
* 輸入所選**資料庫伺服器**的**資料庫管理員使用者名稱**和**資料庫系統管理員密碼**（請注意，您使用的帳戶必須具有建立新 Azure SQL 資料庫所需的許可權）
* 選取 [建立]****。

![新增 Azure SQL Database 對話方塊](publish-to-azure-webapp-using-vs/_static/sql_create.png)

建立完成後，對話方塊會自動關閉，而且 [**設定 Azure SQL Database** ] 對話方塊會再次取得焦點：

* 系統會自動選取剛建立的新實例。
* 選取 [下一步] 。

![[設定 Azure SQL Database] 對話方塊](publish-to-azure-webapp-using-vs/_static/sql_select.png)

在 [**設定 Azure SQL Database** ] 對話方塊的下一個步驟中：

* 輸入**資料庫連接的 [使用者名稱**] 和 [**資料庫連接密碼**] 欄位。 這些是您的應用程式將在執行時間用來連接到資料庫的詳細資料。 最佳做法是避免使用與上一個步驟中所使用的系統管理員使用者名稱 & 密碼相同的詳細資料。
* 選取 [完成]。

![設定 Azure SQL Database 對話方塊、連接字串詳細資料](publish-to-azure-webapp-using-vs/_static/sql_connection.png)

在 [**發行設定檔] 摘要**頁面中，選取 [**設定**]：

![發行設定檔摘要頁面：編輯設定](publish-to-azure-webapp-using-vs/_static/pp_configured.png)

在 [發行]**** 對話方塊的 [設定]**** 頁面上：

* 展開 [資料庫]**** 並選取 [在執行階段使用此連接字串]****。
* 展開 [ **Entity Framework 遷移**]，然後選取 [**在發行時套用此遷移**]。

* 選取 [儲存]****。 Visual Studio 會回到 [發行]**** 對話方塊。 

![[發行] 對話方塊：設定面板](publish-to-azure-webapp-using-vs/_static/pp_settings.png)

按一下 **[發行]**。 Visual Studio 會將您的應用程式發佈至 Azure。 部署完成時，會在瀏覽器中開啟應用程式。

![[發行] 對話方塊：設定面板](publish-to-azure-webapp-using-vs/_static/pp_publish.png)

### <a name="update-the-app"></a>更新應用程式

* 編輯*Pages/Index. cshtml* Razor 頁面並變更其內容。 例如，您可以修改段落使其說出 "Hello ASP.NET Core!"：

    [!code-html[Index](publish-to-azure-webapp-using-vs/sample/index.cshtml?highlight=10&range=1-12)]

* 再次選取 [發行**設定檔] 摘要**頁面中的 [**發佈**]。

![發行設定檔摘要頁面](publish-to-azure-webapp-using-vs/_static/pp_publish.png)

* 發行應用程式後，請驗證您進行的變更在 Azure 上有效。

![驗證工作是否完成](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a>清除

當您完成測試應用程式時，請移至 [Azure 入口網站](https://portal.azure.com/)並刪除應用程式。

* 選取 [資源群組]****，然後選取您建立的資源群組。

![Azure 入口網站：資訊看板功能表中的 [資源群組]](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* 在 [資源群組]**** 頁面中，選取 [刪除]****。

![Azure 入口網站：[資源群組] 頁面](publish-to-azure-webapp-using-vs/_static/rgd.png)

* 輸入資源群組的名稱，並選取 [刪除]****。 您在本教學課程中建立的應用程式和其他所有資源都會立即從 Azure 中刪除。

### <a name="next-steps"></a>後續步驟

* <xref:host-and-deploy/azure-apps/azure-continuous-deployment>

## <a name="additional-resources"></a>其他資源

* 針對 Visual Studio Code，請參閱[發佈設定檔](xref:host-and-deploy/visual-studio-publish-profiles#publish-profiles)。
* [Azure App Service](/azure/app-service/app-service-web-overview)
* [Azure 資源群組](/azure/azure-resource-manager/resource-group-overview#resource-groups)
* [Azure SQL Database](/azure/sql-database/)
* <xref:host-and-deploy/visual-studio-publish-profiles>
* <xref:test/troubleshoot-azure-iis>
