---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: 檔案和資料夾排除部署 |Microsoft 文件
author: jrjlee
description: 本主題描述如何您可以排除檔案和資料夾從 web 部署套件時建置及封裝的 web 應用程式專案。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: c435448bf057bbef9127d66ffda24a07729f2322
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="excluding-files-and-folders-from-deployment"></a>從部署中排除檔案和資料夾
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何您可以排除檔案和資料夾從 web 部署套件時建置及封裝的 web 應用程式專案。


本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案&#x2014;[連絡人管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來表示實際層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 通訊的 web 應用程式Foundation (WCF) 服務與資料庫專案。

這些教學課程的核心的部署方法為基礎所說明的分割專案檔案方法[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置流程控制的兩個專案檔&#x2014;一個包含建置適用於每個目的地環境中和包含特定環境的建置和部署設定的指示。 在建置時，環境特定專案檔就會合併環境無從驗證專案檔來形成一組完整組建指示。

## <a name="overview"></a>總覽

當您建置 Visual Studio 2010 中的 web 應用程式專案時，Web 發行管線 (WPP) 可讓您擴充這個建置流程所編譯的 web 應用程式分成可部署 web 封裝的封裝。 然後，您可以用於網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 將此 web 封裝部署到遠端的 IIS 網頁伺服器，或匯入 web 套件手動透過 IIS 管理員。 此封裝程序中會說明[建置和封裝 Web 應用程式專案](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)。

您如何在控制取得包含在您的 web 內容套件嗎？ 在 Visual Studio 中的專案設定，透過基礎的專案檔中，有很多的案例提供足夠的控制。 不過，在某些情況下您可能要調整您的 web 封裝到特定目的地環境的內容。 比方說，您可能想要包含記錄檔的資料夾，當應用程式部署至測試環境，但當您部署至預備或生產環境應用程式中排除的資料夾。 本主題將說明如何執行這項操作。

## <a name="what-gets-included-by-default"></a>根據預設，取得包含內容？

當您在 Visual Studio 中，設定 web 應用程式專案屬性中**要部署的項目**清單**封裝/發行 Web**頁面可讓您指定您想要包含在您的 web 部署封裝。 根據預設，這設定為**僅執行此應用程式所需的檔案**。

![](excluding-files-and-folders-from-deployment/_static/image1.png)

當您選擇**僅執行此應用程式所需的檔案**，WPP 會嘗試判斷哪些檔案應該加入至 web 套件。 包括：

- 所有組建都輸出的專案。
- 任何檔案的建置動作以標示**內容**。

> [!NOTE]
> 決定要包含哪些檔案的邏輯包含在這個檔案：   
> *%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*


## <a name="excluding-specific-files-and-folders"></a>排除特定檔案和資料夾

在某些情況下，您會想更細部的掌控哪些檔案和資料夾部署。 如果您知道您想要排除之前有哪些的檔案的時間，而且排除套用到所有目的地環境，您可以直接將**建置動作**的每個檔案**無**。

**若要從部署中排除特定的檔案**

1. 在**方案總管 中**視窗中，以滑鼠右鍵按一下檔案，然後**屬性**。
2. 在**屬性**視窗，請在**建置動作**列中選取**無**。

不過，這個方法不一定很方便。 例如，您可能想要更改的檔案和資料夾都包含根據目的地環境中，以及從 Visual Studio 外部。 例如，在連絡人管理員範例方案中，看看 ContactManager.Mvc 專案的內容：

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- [內部] 資料夾包含一些 SQL 指令碼開發人員用來建立、 卸除，並填入本機資料庫，供開發應用程式。 在此資料夾中為 nothing 應該部署到預備或生產環境。
- 指令碼 資料夾包含數個 JavaScript 檔案。 許多這些檔案會包含只是為了支援偵錯，或提供 Visual Studio 中的 IntelliSense。 這當中有些檔案應該不會部署到預備環境或生產環境中。 不過，您可能想要將其部署至開發人員的測試環境，以利於進行疑難排解。

雖然您無法管理您的專案檔案，以排除特定檔案和資料夾，還有更簡單的方法。 WPP 還有一個機制，藉由建置名為的項目清單中排除檔案和資料夾**ExcludeFromPackageFolders**和**ExcludeFromPackageFiles**。 您可以擴充這個機制，將您自己的項目加入至這些清單。 若要這樣做，您需要完成下列高階步驟：

1. 建立自訂專案檔名為 *[專案名稱].wpp.targets*專案檔相同資料夾中。

    > [!NOTE]
    > *。 Wpp.targets*檔案必須與您的 web 應用程式專案檔相同的資料夾中移&#x2014;，例如*ContactManager.Mvc.csproj*&#x2014;而不是任何自訂相同資料夾中您用來控制組建和部署程序的專案檔案。
2. 在 *。 wpp.targets* file、 add **ItemGroup**項目。
3. 在**ItemGroup**項目，加入**ExcludeFromPackageFolders**和**ExcludeFromPackageFiles**来排除特定檔案和資料夾做為必要項目。

這是這個基本結構 *。 wpp.targets*檔案：


[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]


請注意每個項目包含名為的項目中繼資料元素**FromTarget**。 這是選擇性的值並不會影響在建置程序。它只是用來指出特定檔案或資料夾已省略為什麼如果有人檢閱組建記錄檔。

## <a name="excluding-files-and-folders-from-a-web-package"></a>從 Web 套件中排除檔案和資料夾

下一個程序會示範如何加入 *。 wpp.targets* web 應用程式專案，以及如何使用檔案時，排除特定檔案和資料夾從 web 套件建置您的專案檔案。

**若要從 web 部署套件中排除檔案和資料夾**

1. Visual Studio 2010 中開啟您的方案。
2. 在**方案總管] 中**視窗中，以滑鼠右鍵按一下您 web 應用程式的專案節點 (比方說， **ContactManager.Mvc**)，指向**新增**，然後按一下 [ **新項目**。
3. 在**加入新項目**對話方塊中，選取**XML 檔案**範本。
4. 在**名稱**方塊中，輸入 *[專案名稱] * * *.wpp.targets** (例如， **ContactManager.Mvc.wpp.targets**)，然後按一下 **新增**.

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > 如果您將新的項目加入專案的根節點時，檔案會建立專案檔相同資料夾中。 您可以驗證，請在 Windows 檔案總管 中開啟資料夾。
5. 在檔案中加入**專案**項目和**ItemGroup**項目：

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. 如果您想要排除資料夾從 web 封裝，加入**ExcludeFromPackageFolders**元素**ItemGroup**項目：

   1. 在**Include**屬性，請提供您想要排除的資料夾以分號分隔清單。
   2. 在**FromTarget**中繼資料元素，提供有意義的值，指出為什麼資料夾已被排除，如名稱 *。 wpp.targets*檔案。

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. 如果您想要排除的檔案從 web 封裝，加入**ExcludeFromPackageFiles**元素**ItemGroup**項目：

   1. 在**Include**屬性，請提供您想要排除的檔案以分號分隔清單。
   2. 在**FromTarget**中繼資料元素，提供有意義的值，指出檔案會被排除原因，如名稱 *。 wpp.targets*檔案。

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. *[專案名稱].wpp.targets*檔現在應該類似這樣：

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. 儲存並關閉 *[專案名稱].wpp.targets*檔案。

下次當您建置並封裝您的 web 應用程式專案，WPP 會自動偵測 *。 wpp.targets*檔案。 任何檔案和您指定的資料夾不會包含 web 套件中。

## <a name="conclusion"></a>結論

本主題描述如何排除特定檔案和資料夾，當您建立自訂建置 web 封裝， *。 wpp.targets*與您的 web 應用程式專案檔相同的資料夾中的檔案。

## <a name="further-reading"></a>進一步閱讀

如需有關如何使用自訂的 Microsoft Build Engine (MSBuild) 專案檔來控制部署程序的詳細資訊，請參閱[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)和[瞭解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。 如需有關封裝和部署程序的詳細資訊，請參閱[建置和封裝 Web 應用程式專案](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)， [Web 封裝部署的設定參數](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)，和[部署 Web 封裝](../web-deployment-in-the-enterprise/deploying-web-packages.md)。

> [!div class="step-by-step"]
> [上一頁](deploying-membership-databases-to-enterprise-environments.md)
> [下一頁](taking-web-applications-offline-with-web-deploy.md)
