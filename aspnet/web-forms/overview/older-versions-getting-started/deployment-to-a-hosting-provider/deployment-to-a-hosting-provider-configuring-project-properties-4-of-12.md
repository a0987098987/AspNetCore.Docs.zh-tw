---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: 使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 設定專案屬性-4，12 個 |Microsoft Docs
author: tdykstra
description: 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫，使用 Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: 4aa1e55f21e74ae8da67d6d13c35a8de5693ec3a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396259"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 設定專案屬性-4，12 個
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 包含 SQL Server Compact 資料庫。 如果您安裝 Web Publish Update，您也可以使用 Visual Studio 2010。 在數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 顯示 Visual Studio 2012 RC 版本之後引入的部署功能，示範如何部署 SQL Server Compact，以外的 SQL Server 版本，並示範如何部署至 Azure App Service Web Apps 的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>總覽

儲存專案檔中的專案內容中所設定的某些部署選項 ( *.csproj*或是 *.vbproj*檔案)。 在大部分情況下，這些設定的預設值是什麼您想要的但您可以使用**專案屬性**UI 內建於 Visual Studio 搭配使用這些設定，如果您需要變更它們。 在本教學課程中，您檢閱中的部署設定**專案屬性**。 您也會建立預留位置檔案會造成要部署的空資料夾。

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>在 [專案屬性] 視窗中設定部署設定

如您所見下列的教學課程中，大部分的設定會影響部署期間進行的作業會納入發行設定檔。 您應該要注意的幾個設定都位於**封裝/發行**索引標籤**專案屬性**視窗。 這些設定會指定每個組建組態 — 也就是有不同的設定，在發行組建，比您的偵錯組建。

在 **方案總管 中**，以滑鼠右鍵按一下**ContosoUniversity**專案，然後選取**屬性**，然後選取**封裝/發行 Web** 索引標籤。

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

視窗顯示時，則預設為顯示設定任何的建置組態目前作用中的解決方案。 如果**組態** 方塊中不會指出**作用 （發行）**，選取**版本**才能顯示 發行 組建組態設定。 您會將發行組建部署到您的測試和生產環境。

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

與**作用 （發行）** 或是**版本**選取，您看到當您部署使用 [發行] 組建組態時有效的值：

- 在 [**要部署的項目**] 方塊中，**只執行應用程式所需的檔案**已選取。 其他選項包括**此專案中的所有檔案**或是**此專案資料夾中的所有檔案**。 藉由保留預設選取項目不會變更您避免部署原始程式碼檔案，例如。 這項設定是包含 SQL Server Compact 的二進位檔案的資料夾必須包含在專案中的原因的原因。 如需有關這項設定的詳細資訊，請參閱 <<c0>  **為什麼不在我的專案資料夾中檔案的所有部署？** 中[ASP.NET Web 應用程式專案部署常見問題集](https://msdn.microsoft.com/library/ee942158.aspx)。
- **排除產生偵錯符號**已選取。 當您使用這個組建組態時，您將不會偵錯。
- **從應用程式中排除檔案\_Data 資料夾**未選取。 您的成員資格資料庫的 SQL Server Compact 檔案是在該資料夾中，您必須將它部署。 當您部署不包含資料庫變更的更新時，您會選取此核取方塊。
- **先行編譯此應用程式發行前的**未選取。 在大部分情況下，沒有需要先行編譯 web 應用程式專案。 如需有關這個選項的詳細資訊，請參閱 <<c0> [ 封裝/發行 Web 索引標籤中，專案屬性](https://msdn.microsoft.com/library/dd410108(v=vs.110).aspx)並[進階先行編譯設定對話方塊](https://msdn.microsoft.com/library/hh475319(v=vs.110).aspx)。
- **包含在封裝/發行 SQL 索引標籤中設定的所有資料庫**已選取此選項沒有任何作用現在因為您未設定，但**封裝/發行 SQL**  索引標籤。該索引標籤是用來是唯一的選項，部署 SQL Server 資料庫的舊版資料庫部署方法。 您將使用**封裝/發行 SQL**索引標籤中[移轉至 SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)教學課程。
- **Web Deployment 封裝設定**區段不適用，因為您使用單鍵發行在這些教學課程。

變更**組態**下拉式清單方塊，以偵錯以查看針對偵錯組建的預設設定。 這些值都相同，除了**排除產生偵錯符號**已清除，以便您可以偵錯，當您部署的偵錯組建。

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>讓確定 Elmah 資料夾取得部署

如您在先前的教學課程中所見[Elmah NuGet 套件](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx)提供的錯誤記錄和報告功能。 Contoso 大學應用程式中 Elmah 已設定為將錯誤詳細資料儲存在名為的資料夾*Elmah*:

![Elmah 資料夾](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

從部署中排除特定檔案或資料夾是常見的需求;另一個範例就是使用者可以上傳檔案的資料夾。 您不想記錄檔，或上傳您的開發環境，可供部署到生產環境中所建立的檔案。 如果您要將更新部署到生產環境，您不想要刪除在生產環境中存在的檔案，部署程序。 （根據如何設定部署選項，如果檔案存在於目的地站台但未在來源站台部署時，Web Deploy 刪除它從目的地。）

如稍早在本教學課程中，您所見**部署的項目**選項**封裝/發行 Web**索引標籤設定為**只需要執行此應用程式檔案**。 如此一來，在開發過程中建立的 Elmah 記錄檔將不會部署，這是您想要發生。 (若要部署，它們必須包含在專案及其**建置動作**屬性必須設為**內容**。 如需詳細資訊，請參閱 <<c0>  **為什麼不在我的專案資料夾中檔案的所有部署？** 中[ASP.NET Web 應用程式專案部署常見問題集](https://msdn.microsoft.com/library/ee942158.aspx))。 不過，Web Deploy 不會建立一個資料夾目的地站台中除非至少一個檔案複製到它。 因此，您會新增 *.txt*檔案到資料夾，以做為預留位置，以便將複製的資料夾。

在 [**方案總管] 中**，以滑鼠右鍵按一下*Elmah*資料夾中，選取**加入新項目**，並建立名為的檔案*Placeholder.txt*。 置於下列文字: 「 這個 」 預留位置檔案，以確保取得部署資料夾。 然後儲存檔案。 這就是您必須執行才能確定，Visual Studio 會部署這個檔案和資料夾，因為**建置動作**屬性 *.txt*檔案設定為**內容**預設。

您現在已完成所有的部署設定工作。 在下一個教學課程中，將 Contoso 大學網站部署到測試環境，並測試其存在。

> [!div class="step-by-step"]
> [上一頁](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
> [下一頁](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
