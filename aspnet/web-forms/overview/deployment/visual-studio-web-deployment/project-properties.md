---
uid: web-forms/overview/deployment/visual-studio-web-deployment/project-properties
title: 使用 Visual Studio 的 ASP.NET Web 部署： 專案屬性 |Microsoft Docs
author: tdykstra
description: 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: ec1cec4c-a75f-47af-a2ba-b1e2f971d24b
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/project-properties
msc.type: authoredcontent
ms.openlocfilehash: 86e4e3ef5f126a5bf8abc91c2d5ce3d14b1e1c6c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837661"
---
<a name="aspnet-web-deployment-using-visual-studio-project-properties"></a>使用 Visual Studio 的 ASP.NET Web 部署： 專案屬性
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。 這個系列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。


## <a name="overview"></a>總覽

儲存專案檔中的專案內容中所設定的某些部署選項 ( *.csproj*或是 *.vbproj*檔案)。 在大部分情況下，這些設定的預設值是什麼您想要的但您可以使用**專案屬性**UI 內建於 Visual Studio 搭配使用這些設定，如果您需要變更它們。 在本教學課程中，您檢閱中的部署設定**專案屬性**。 您也會建立預留位置檔案會造成要部署的空資料夾。

## <a name="configure-deployment-settings-in-the-project-properties-window"></a>在 [專案屬性] 視窗中設定部署設定

如您所見下列的教學課程中，大部分的設定會影響部署期間進行的作業會納入發行設定檔。 您應該要注意的幾個設定都位於**封裝/發行**索引標籤**專案屬性**視窗。 這些設定會指定每個組建組態 — 也就是有不同的設定，在發行組建，比您的偵錯組建。

在 **方案總管 中**，以滑鼠右鍵按一下**ContosoUniversity**專案，然後選取**屬性**，然後選取**封裝/發行 Web** 索引標籤。

![[封裝/發行 Web] 索引標籤](project-properties/_static/image1.png)

視窗顯示時，則預設為顯示設定任何的建置組態目前作用中的解決方案。 如果**組態** 方塊中不會指出**作用 （發行）**，選取**版本**才能顯示 發行 組建組態設定。 您會將發行組建部署到您的測試和生產環境。

![選取 [發行] 組建組態](project-properties/_static/image2.png)

與**作用 （發行）** 或是**版本**選取，您看到當您部署使用 [發行] 組建組態時有效的值：

- 在 [**要部署的項目**] 方塊中，**只執行應用程式所需的檔案**已選取。 其他選項包括**此專案中的所有檔案**或是**此專案資料夾中的所有檔案**。 藉由保留預設選取項目不會變更您避免部署原始程式碼檔案，例如。 這項設定是包含 SQL Server Compact 的二進位檔案的資料夾必須包含在專案中的原因的原因。 如需有關這項設定的詳細資訊，請參閱 <<c0>  **為什麼不在我的專案資料夾中檔案的所有部署？** 中[ASP.NET Web 應用程式專案部署常見問題集](https://msdn.microsoft.com/library/ee942158.aspx)。
- **排除產生偵錯符號**已選取。 當您使用這個組建組態時，您將不會偵錯。
- **包含在封裝/發行 SQL 索引標籤中設定的所有資料庫**已選取。 指定 Visual Studio 是否會將部署資料庫，以及檔案。 雖然核取方塊標籤只提及**封裝/發行 SQL**索引標籤上，清除此核取方塊會也停用已在發行設定檔中的資料庫部署。 您將執行的更新版本中，因此必須維持選取狀態，核取方塊。 **封裝/發行 SQL**  索引標籤用於發行方法，您將不會使用這些教學課程中使用舊版資料庫。
- **Web Deployment 封裝設定**區段不適用，因為您使用單鍵發行在這些教學課程。

變更**組態**下拉式清單方塊，以偵錯以查看針對偵錯組建的預設設定。 這些值都相同，除了**排除產生偵錯符號**已清除，以便您可以偵錯，當您部署的偵錯組建。

## <a name="make-sure-that-the-elmah-folder-gets-deployed"></a>請確定 Elmah 資料夾取得部署

如您在先前的教學課程中所見[Elmah NuGet 套件](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx)提供的錯誤記錄和報告功能。 Contoso 大學應用程式中 Elmah 已設定為將錯誤詳細資料儲存在名為的資料夾*Elmah*:

![Elmah 資料夾](project-properties/_static/image3.png)

從部署中排除特定檔案或資料夾是常見的需求;另一個範例就是使用者可以上傳檔案的資料夾。 您不想記錄檔，或上傳您的開發環境，可供部署到生產環境中所建立的檔案。 如果您要將更新部署到生產環境，您不想要刪除在生產環境中存在的檔案，部署程序。 （根據如何設定部署選項，如果檔案存在於目的地站台但未在來源站台部署時，Web Deploy 刪除它從目的地。）

如稍早在本教學課程中，您所見**部署的項目**選項**封裝/發行 Web**索引標籤設定為**只需要執行此應用程式檔案**。 如此一來，在開發過程中建立的 Elmah 記錄檔將不會部署，這是您想要發生。 (若要部署，它們必須包含在專案及其**建置動作**屬性必須設為**內容**。 如需詳細資訊，請參閱 <<c0>  **為什麼不在我的專案資料夾中檔案的所有部署？** 中[ASP.NET Web 應用程式專案部署常見問題集](https://msdn.microsoft.com/library/ee942158.aspx))。 不過，Web Deploy 不會建立一個資料夾目的地站台中除非至少一個檔案複製到它。 因此，您會新增 *.txt*檔案到資料夾，以做為預留位置，以便將複製的資料夾。

在 [**方案總管] 中**，以滑鼠右鍵按一下*Elmah*資料夾中，選取**加入新項目**，並建立名為文字檔*Placeholder.txt*。 置於下列文字: 「 這個 」 預留位置檔案，以確保取得部署資料夾。 然後儲存檔案。 這就是您必須執行才能確定，Visual Studio 會部署這個檔案和資料夾，因為**建置動作**屬性 *.txt*檔案設定為**內容**預設。

## <a name="summary"></a>總結

您現在已完成所有的部署設定工作。 在下一個教學課程中，將 Contoso 大學網站部署到測試環境，並測試其存在。

> [!div class="step-by-step"]
> [上一頁](web-config-transformations.md)
> [下一頁](deploying-to-iis.md)
