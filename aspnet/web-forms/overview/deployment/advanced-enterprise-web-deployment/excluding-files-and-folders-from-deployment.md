---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: 從部署中排除檔案和資料夾 |Microsoft Docs
author: jrjlee
description: 本主題描述如何您可以排除檔案和資料夾的 web 部署封裝時您建置和封裝 web 應用程式專案。
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: 33eecfea86842a93eac705e7823f1eba9519670e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816674"
---
<a name="excluding-files-and-folders-from-deployment"></a>從部署中排除檔案和資料夾
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何您可以排除檔案和資料夾的 web 部署封裝時您建置和封裝 web 應用程式專案。


本主題是構成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程的一部分本教學課程系列會使用範例解決方案&#x2014;[連絡管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。

這些教學課程的核心的部署方法根據分割專案檔案方法中所述[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置流程控制的兩個專案檔&#x2014;包含建置適用於每個目的地環境中和包含環境特定建置和部署設定的指示。 在建置階段的特定環境的專案檔會合併到無從驗證環境的專案檔中，以構成一組完整的組建指示。

## <a name="overview"></a>總覽

當您建置 Visual Studio 2010 中的 web 應用程式專案時，Web 發行管線 (WPP) 可讓您擴充此建置程序，封裝成可部署的 web 套件已編譯的 web 應用程式。 然後，您可以使用 Internet Information Services (IIS) Web Deployment Tool (Web Deploy) 至遠端 IIS web 伺服器，將此 web 封裝部署，或匯入 web 套件手動透過 IIS 管理員。 此封裝程序會說明[建置和封裝 Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)。

到底是您在控制取得包含在 web 中的內容套件嗎？ 在 Visual Studio 中的專案設定，透過基礎的專案檔中，許多案例提供足夠的控制。 不過，在某些情況下，您可能想要量身訂做您特定的目的地環境的 web 封裝的內容。 例如，您可能要包含記錄檔的資料夾，當您應用程式部署至測試環境，但當您部署至預備或生產環境應用程式中排除的資料夾。 本主題將說明如何執行這項操作。

## <a name="what-gets-included-by-default"></a>根據預設，取得包含哪些？

當您在 Visual Studio 中，設定您的 web 應用程式專案屬性**部署的項目**清單**封裝/發行 Web**頁面可讓您指定您想要包含在您的 web 部署封裝。 根據預設，這設定為**僅執行此應用程式所需的檔案**。

![](excluding-files-and-folders-from-deployment/_static/image1.png)

當您選擇**僅執行此應用程式所需的檔案**，WPP 會嘗試判斷哪些檔案應該將 web 套件。 包括：

- 所有組建都輸出的專案。
- 任何檔案以建置動作為標示**內容**。

> [!NOTE]
> 此檔案被包含來判斷哪些檔案来包含的邏輯：   
> *%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*


## <a name="excluding-specific-files-and-folders"></a>排除特定的檔案和資料夾

在某些情況下，您會想更細微的控制，這部署檔案和資料夾。 如果您知道您想要提前排除哪些的檔案的時間，而且排除適用於所有的目的地環境，您可以輕鬆地設定**建置動作**的每個檔案**無**。

**若要從部署中排除特定檔案**

1. 在 [**方案總管**] 視窗中，以滑鼠右鍵按一下檔案，然後**屬性**。
2. 在 **屬性**視窗，請在**建置動作**列中選取**None**。

不過，這種方法不一定很方便。 例如，您可能想要不同的檔案和資料夾會包含根據您的目的地環境，並從 Visual Studio 外部。 例如，在連絡人管理員範例方案中，看看 ContactManager.Mvc 專案的內容：

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- [內部] 資料夾包含一些 SQL 指令碼的開發人員用來建立、 卸除，並填入本機資料庫進行開發。 在此資料夾中為 nothing 應該部署至預備或生產環境。
- 指令碼 資料夾包含數個 JavaScript 檔案。 許多這些檔案的單純以支援偵錯，或提供 Visual Studio 中的 IntelliSense。 這當中有些檔案應該不會部署至預備或生產環境中。 不過，您可能要將其部署至開發人員測試環境，以利於進行疑難排解。

雖然您無法管理您的專案檔，以排除特定檔案和資料夾，還有更簡單的方法。 WPP 還有一個機制，藉由建置名為的項目清單中排除檔案和資料夾**ExcludeFromPackageFolders**並**ExcludeFromPackageFiles**。 您可以將您自己的項目加入至這些清單，來擴充這項機制。 若要這樣做，您需要完成下列高階步驟：

1. 建立自訂的專案檔名為 *[專案名稱].wpp.targets*與您的專案檔相同的資料夾中。

    > [!NOTE]
    > *.Wpp.targets*檔案必須與您的 web 應用程式專案檔相同的資料夾中，前往&#x2014;，例如*ContactManager.Mvc.csproj*&#x2014;而不是任何自訂的相同資料夾中用來控制組建和部署程序的專案檔案。
2. 在  *.wpp.targets*檔案中，新增**ItemGroup**項目。
3. 在  **ItemGroup**項目，新增**ExcludeFromPackageFolders**並**ExcludeFromPackageFiles**来排除特定檔案和資料夾做為必要項目。

這是基本的結構 *.wpp.targets*檔案：


[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]


請注意，每個項目會包含名為的項目中繼資料元素**FromTarget**。 這是選擇性的值，並不會影響在建置程序;它只是表示特定的檔案或資料夾已省略為什麼如果有人檢閱組建記錄檔。

## <a name="excluding-files-and-folders-from-a-web-package"></a>網頁套件中排除檔案和資料夾

下一個程序說明如何新增 *.wpp.targets* web 應用程式專案，以及如何使用檔案時所要排除特定檔案和資料夾從網頁套件建置您的專案檔案。

**若要從 web 部署套件中排除檔案和資料夾**

1. Visual Studio 2010 中開啟您的方案。
2. 中**方案總管 中** 視窗中，以滑鼠右鍵按一下您 web 應用程式的專案節點 (例如**ContactManager.Mvc**)，指向**新增**，然後按一下  **新項目**。
3. 在 **加入新項目**對話方塊中，選取**XML 檔案**範本。
4. 在 **名稱**方塊中，輸入 *[專案名稱] * * *.wpp.targets** (比方說， **ContactManager.Mvc.wpp.targets**)，然後按一下 **新增**.

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > 如果您將新的項目加入專案的根節點時，檔案會建立專案檔相同資料夾中。 您可以在 Windows 檔案總管中開啟資料夾來確認。
5. 在檔案中新增**專案**項目並**ItemGroup**項目：

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. 如果您想要排除資料夾不進行網頁套件，新增**ExcludeFromPackageFolders**項目**ItemGroup**項目：

   1. 在  **Include**屬性，請提供以分號分隔的清單，您想要排除的資料夾。
   2. 在  **FromTarget**中繼資料元素，提供有意義的值，指出資料夾會被排除的原因，如名稱 *.wpp.targets*檔案。

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. 如果您想要從網頁套件中排除檔案，新增**ExcludeFromPackageFiles**項目**ItemGroup**項目：

   1. 在  **Include**屬性，請提供您想要排除的檔案以分號分隔清單。
   2. 在  **FromTarget**中繼資料元素，提供有意義的值，指出為什麼要排除檔案，如名稱 *.wpp.targets*檔案。

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. *[專案名稱].wpp.targets*檔案現在應該類似這樣：

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. 儲存並關閉 *[專案名稱].wpp.targets*檔案。

下次當您建置和封裝您的 web 應用程式專案，WPP 將會自動偵測 *.wpp.targets*檔案。 任何檔案和您指定的資料夾將不會包含在網頁套件中。

## <a name="conclusion"></a>結論

本主題說明如何排除特定檔案和資料夾，當您建立自訂建置 web 封裝時， *.wpp.targets*與您的 web 應用程式專案檔相同的資料夾中的檔案。

## <a name="further-reading"></a>進一步閱讀

如需有關如何使用自訂的 Microsoft Build Engine (MSBuild) 專案檔來控制部署程序的詳細資訊，請參閱 <<c0> [ 了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)並[了解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。 如需有關封裝和部署程序的詳細資訊，請參閱 <<c0> [ 建置和封裝 Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)，[設定網頁套件部署的參數](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)，和[部署網頁套件](../web-deployment-in-the-enterprise/deploying-web-packages.md)。

> [!div class="step-by-step"]
> [上一頁](deploying-membership-databases-to-enterprise-environments.md)
> [下一頁](taking-web-applications-offline-with-web-deploy.md)
