---
uid: web-forms/overview/deployment/visual-studio-web-deployment/project-properties
title: "使用 Visual Studio 的 ASP.NET Web 部署： 專案屬性 |Microsoft 文件"
author: tdykstra
description: "此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ec1cec4c-a75f-47af-a2ba-b1e2f971d24b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/project-properties
msc.type: authoredcontent
ms.openlocfilehash: 68a1892dcf8055d8cc898f471a96d86e8abb64de
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-project-properties"></a>使用 Visual Studio 的 ASP.NET Web 部署： 專案屬性
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。 數列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。


## <a name="overview"></a>概觀

儲存專案檔中的專案屬性中設定某些部署選項 ( *.csproj*或*.vbproj*檔案)。 在大部分情況下，這些設定的預設值都是您想要但是您可以使用**專案屬性**UI 的內建於 Visual Studio 搭配使用這些設定，如果您需要變更它們。 在本教學課程中，您檢閱部署設定中的**專案屬性**。 您也可以建立會導致空的資料夾部署的預留位置檔案。

## <a name="configure-deployment-settings-in-the-project-properties-window"></a>在 [專案屬性] 視窗中設定部署設定

大部分的設定會影響部署期間進行的作業包含在發行設定檔中，您會發現下列教學課程中。 您應該注意的一些設定位於**封裝/發行**索引標籤的**專案屬性**視窗。 這些設定會指定每個組建組態，也就是有不同的設定，在發行組建，超過您的偵錯組建。

在**方案總管 中**，以滑鼠右鍵按一下**ContosoUniversity**專案，然後選取**屬性**，然後選取**封裝/發行 Web** 索引標籤。

![[封裝/發行 Web] 索引標籤](project-properties/_static/image1.png)

當視窗出現時，則預設顯示設定的任何組建組態是目前作用中的方案。 如果**組態**方塊並不表示**作用中 （發行）**，選取**發行**才能顯示發行組建組態設定。 您要將發行的組建部署到您的測試和實際執行環境。

![選取的發行組建組態](project-properties/_static/image2.png)

與**作用中 （發行）**或**發行**您選取，請參閱您部署使用發行組建組態時，會有效的值：

- 在**要部署的項目** 方塊中，**只有執行應用程式所需的檔案**已選取。 其他選項包括**這個專案中的所有檔案**或**此專案資料夾中的所有檔案**。 您避免部署原始程式碼檔案中，例如藉由保留預設選取項目保持不變。 此設定為包含 SQL Server Compact 的二進位檔案的資料夾必須包含在專案中的原因的原因。 如需有關這項設定的詳細資訊，請參閱**為什麼沒有 我的專案資料夾中的檔案取得部署所有？**中[ASP.NET Web 應用程式專案部署常見問題集](https://msdn.microsoft.com/en-us/library/ee942158.aspx)。
- **排除產生偵錯符號**已選取。 您將不會使用這個組建組態時，偵錯。
- **包含在封裝/發行 SQL 索引標籤中設定的所有資料庫**已選取。 指定 Visual Studio 是否會將部署資料庫，以及檔案。 雖然核取方塊標籤只有提及**封裝/發行 SQL**索引標籤上，清除此核取方塊會一併停用資料庫部署中的發行設定檔設定。 您會進行的更新版本中，讓核取方塊必須維持選取狀態。 **封裝/發行 SQL**  索引標籤用於舊版的資料庫發行，您將不會使用這些教學課程中的方法。
- **Web Deployment 封裝設定**區段不適用，因為您正在使用單鍵發行這些教學課程。

變更**組態**設為偵錯，若要查看預設設定，用於偵錯組建的下拉式清單方塊。 這些值就是相同的除非**排除產生偵錯符號**已清除，以便您可以偵錯，當您部署偵錯組建。

## <a name="make-sure-that-the-elmah-folder-gets-deployed"></a>請確定取得部署 Elmah 資料夾

如同先前的教學課程[Elmah NuGet 封裝](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx)提供的錯誤記錄和報告功能。 Contoso 大學應用程式中 Elmah 已設定為將錯誤詳細資料儲存在名為的資料夾*Elmah*:

![Elmah 資料夾](project-properties/_static/image3.png)

從部署中排除特定檔案或資料夾是常見的需求。另一個範例就是使用者可以上傳檔案的資料夾。 您不想記錄檔，或開發環境，以便部署到生產環境中所建立的檔案上傳。 如果您要部署更新到生產環境，您不想刪除檔案，存在於實際執行部署程序。 （根據如何設定部署選項，如果檔案存在於目的地站台但未在來源站台部署時，Web Deploy 刪除目的地。）

如稍早在本教學課程中，您所見**要部署的項目**選項**封裝/發行 Web**  索引標籤設為**只需要執行此應用程式檔案**。 如此一來，可由開發中的 Elmah 的記錄檔將不會部署，這是您想要發生的情況。 (供部署，他們必須包含在專案及其**建置動作**屬性必須設為**內容**。 如需詳細資訊，請參閱**為什麼沒有 我的專案資料夾中的檔案取得部署所有？**中[ASP.NET Web 應用程式專案部署常見問題集](https://msdn.microsoft.com/en-us/library/ee942158.aspx))。 不過，Web Deploy 不會建立資料夾在目的地網站中沒有至少一個要複製到該檔案。 因此，您會加入*.txt*檔案到資料夾，以做為預留位置，以便將複製的資料夾。

在**方案總管 中**，以滑鼠右鍵按一下*Elmah*資料夾中，選取**加入新項目**，並建立名為文字檔*Placeholder.txt*。 放入其中的下列文字: 「 這是預留位置檔案，以確保取得部署資料夾 」。 然後儲存檔案。 這就是您必須執行才能確保，Visual Studio 會部署這個檔案和資料夾，因為**建置動作**屬性*.txt*檔設定成**內容**預設。

## <a name="summary"></a>總結

您現在已完成所有的部署設定工作。 在下一個教學課程中，將 Contoso 大學網站部署到測試環境和測試其存在。

>[!div class="step-by-step"]
[上一頁](web-config-transformations.md)
[下一頁](deploying-to-iis.md)
