---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: 讓 Web 應用程式離線使用 Web 部署 |Microsoft Docs
author: jrjlee
description: 本主題描述如何將 web 應用程式離線使用 Internet Information Services (IIS) Web 高於自動部署期間...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: b8dc1ff26bbbe7dee1ee90fd929b2e98de5360db
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829511"
---
<a name="taking-web-applications-offline-with-web-deploy"></a>讓 Web 應用程式離線使用 Web 部署
====================
藉由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何將 web 應用程式離線期間的自動化的部署使用 Internet Information Services (IIS) Web Deployment Tool (Web Deploy)。 瀏覽至 web 應用程式的使用者重新導向至*應用程式\_offline.htm*檔案直到部署完成為止。


本主題是構成一系列以名為 Fabrikam，Inc.的虛構公司的企業部署需求為基礎的教學課程的一部分本教學課程系列會使用範例解決方案&#x2014;[連絡管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows Communication 的 web 應用程式Foundation (WCF) 服務與資料庫專案。

這些教學課程的核心的部署方法根據分割專案檔案方法中所述[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，在建置流程控制的兩個專案檔&#x2014;包含建置適用於每個目的地環境中和包含環境特定建置和部署設定的指示。 在建置階段的特定環境的專案檔會合併到無從驗證環境的專案檔中，以構成一組完整的組建指示。

## <a name="task-overview"></a>工作概觀

在許多案例中，您會想要讓 web 應用程式離線，當您變更相關的元件，例如資料庫或 web 服務。 一般而言，在 IIS 和 ASP.NET 中，您達成此目的檔案，名為放*應用程式\_offline.htm* IIS 網站或 web 應用程式的根資料夾中。 *應用程式\_offline.htm*檔案是標準的 HTML 檔案，且通常會包含一個簡單的訊息，告知使用者的站台因維修而暫時無法使用。 雖然*應用程式\_offline.htm*檔案存在於網站的根資料夾中，IIS 會自動導向至檔案的任何要求。 當您完成時進行更新時，也會移除*應用程式\_offline.htm*檔案和網站會繼續如往常般處理要求。

當您使用 Web Deploy 來執行自動化或單一步驟部署至目標環境時，建議您將新增及移除*應用程式\_offline.htm*檔案到您的部署程序。 若要這樣做，您將需要完成下列高階工作：

- Microsoft Build Engine (MSBuild) 專案檔中您用來控制部署程序，請建立 MSBuild 目標會複製*應用程式\_offline.htm*檔案到目的地伺服器之前任何的部署工作開始。
- 新增另一個移除的 MSBuild 目標*應用程式\_offline.htm*從目的地伺服器部署的所有工作完成時的檔案。
- 在 web 應用程式專案中，建立 *.wpp.targets*檔案，可確保*應用程式\_offline.htm* Web Deploy 叫用時，檔案會加入至部署套件。

本主題將示範如何執行這些程序。 工作與本主題中的逐步解說假設您已建立的方案包含至少一個 web 應用程式專案，與您使用自訂的專案檔中所述，控制部署程序[中的 Web 部署企業](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)。 或者，您可以使用[Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)範例要遵循本主題中的範例方案。

## <a name="adding-an-appoffline-file-to-a-web-application-project"></a>新增應用程式\_至 Web 應用程式專案的離線檔案

您需要完成第一個工作是新增*應用程式\_離線*web 應用程式專案的檔案：

- 若要防止檔案被干擾開發程序 （您不想永久離線的應用程式），您應該呼叫它的項目以外*應用程式\_offline.htm*。 例如，無法將檔案命名*應用程式\_離線 template.htm*。
- 若要防止檔案被部署為的是，您應該將建置動作設定為**無**。

**若要新增應用程式\_至 web 應用程式專案的離線檔案**

1. Visual Studio 2010 中開啟您的方案。
2. 在**方案總管**視窗中，以滑鼠右鍵按一下您的 web 應用程式專案，指向**新增**，然後按一下 **新項目**。
3. 在 **加入新項目**對話方塊中，選取**HTML 網頁**。
4. 在 **名稱**方塊中，輸入**應用程式\_離線 template.htm**，然後按一下**新增**。

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. 加入一些簡單的 HTML，以通知使用者應用程式，就無法使用，並再儲存檔案。 不包含任何伺服器端標記 (例如，前面會加上任何標記"asp:")。 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. 在 [**方案總管**] 視窗中，以滑鼠右鍵按一下新的檔案，然後**屬性**。
7. 在 **屬性**視窗，請在**建置動作**列中選取**None**。

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-appoffline-file"></a>部署和刪除應用程式\_離線檔案

下一個步驟是修改您部署邏輯，以將檔案複製到目的地伺服器，部署程序的開頭和結尾移除它。

> [!NOTE]
> 下一個程序假設您使用自訂的 MSBuild 專案檔來控制您的部署程序中所述[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。 如果您要部署直接從 Visual Studio，您必須使用不同的方式。 Sayed Ibrahim Hashimi 描述中的其中一個這類方法[如何取得您 Web 應用程式離線期間發行](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx)。


若要部署*應用程式\_離線*檔案到目的地的 IIS 網站，您必須叫用使用 MSDeploy.exe [Web Deploy **contentPath**提供者](https://technet.microsoft.com/library/dd569034(WS.10).aspx)。 **ContentPath**提供者支援的實體目錄路徑和 IIS 網站或應用程式路徑，因此同步處理 Visual Studio 專案資料夾與 IIS web 應用程式之間的檔案的理想選擇。 若要部署的檔案，MSDeploy 命令看起來應該像這樣：


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]


若要移除目的地站台的部署程序結尾的檔案，MSDeploy 命令看起來應該像這樣：


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]


若要將這些命令自動化建置和部署程序的一部分，您需要將它們整合到您自訂的 MSBuild 專案檔。 下一個程序描述如何執行這項操作。

**部署及刪除應用程式\_離線檔案**

1. 在 Visual Studio 2010 中，開啟 MSBuild 專案檔，以控制您的部署程序。 在  [Contactmanager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)範例解決方案中，這是*Publish.proj*檔案。
2. 在根目錄**專案**項目，建立新**PropertyGroup**項目來儲存變數*應用程式\_離線*部署：

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. **SourceRoot**屬性中定義其他地方*Publish.proj*檔案。 它會指出相對於目前路徑的來源內容的根資料夾的位置&#x2014;換句話說，相對於位置*Publish.proj*檔案。
4. **ContentPath**提供者不會接受相對檔案路徑，因此您需要取得原始程式檔的絕對路徑，才能部署它。 您可以使用[ConvertToAbsolutePath](https://msdn.microsoft.com/library/bb882668.aspx)工作來執行這項操作。
5. 加入新**目標**名為的項目**GetAppOfflineAbsolutePath**。 在此目標，使用**ConvertToAbsolutePath**工作來取得的絕對路徑*應用程式\_離線範本*專案資料夾中的檔案。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. 此目標，採用的相對路徑*應用程式\_離線範本*專案資料夾中的檔案並將它儲存到新的屬性，可為絕對檔案路徑。 **BeforeTargets**屬性會指定您想要前執行這個目標**DeployAppOffline**目標，您將在下一個步驟中建立。
7. 新增名為的新目標**DeployAppOffline**。 這個目標，內叫用 MSDeploy.exe 命令，以部署您*應用程式\_離線*目的地 web 伺服器的檔案。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. 在此範例中， **ContactManagerIisPath**專案檔中其他地方定義屬性。 這是只需 IIS 應用程式路徑，格式 *[IIS 網站名稱] / [應用程式名稱]*。 包含目標中的條件，可讓使用者切換*應用程式\_離線*藉由變更屬性值，或提供的命令列參數部署開啟或關閉。
9. 新增名為的新目標**DeleteAppOffline**。 這個目標，內叫用 MSDeploy.exe 命令，以移除您*應用程式\_離線*來自目的地 web 伺服器的檔案。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. 最後一項工作是在您的專案檔的執行期間叫用這些新的目標，在適當時間點。 以各種方式，您可以執行這項操作。 例如，在*Publish.proj*檔案， **FullPublishDependsOn**屬性會指定必須執行的目標清單中排序時**FullPublish**預設目標會叫用。
11. 修改 MSBuild 專案檔，才能叫用**DeployAppOffline**並**DeleteAppOffline**在發行程序中的適當點的目標。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

當您執行您自訂的 MSBuild 專案檔案，*應用程式\_離線*檔案會在建置成功之後，立即部署至伺服器。 部署的所有工作完成後，它就會從伺服器刪除。

## <a name="adding-an-appoffline-file-to-deployment-packages"></a>新增應用程式\_離線的檔案，以部署套件

根據您設定您的部署方式，任何現有內容的目的地 IIS web 應用程式&#x2014;等*應用程式\_offline.htm*檔&#x2014;web 部署時可能會自動刪除到目的地的封裝。 若要確保*應用程式\_offline.htm*檔案仍會維持原狀部署期間，您需要部署直接在開頭的檔案除了包含本身的 web 部署套件中的檔案部署程序。

- 如果您已遵循本主題中先前的工作，您會新增*應用程式\_offline.htm*檔案，以您的 web 應用程式專案，在不同的檔案名稱 (我們使用*應用程式\_離線 template.htm*)，您將已設定的建置動作**None**。 這些變更是為了避免干擾開發和偵錯的檔案。 如此一來，您需要自訂封裝程序，以確保*應用程式\_offline.htm*檔案包含在 web 部署套件。

Web 發行管線 (WPP) 會使用名為的項目清單**FilesForPackagingFromProject**建置一份應該包含 web 部署套件中的檔案。 您可以將自己的項目加入至這份清單，來自訂 web 封裝的內容。 若要這樣做，您需要完成下列高階步驟：

1. 建立自訂的專案檔名為 *[專案名稱].wpp.targets*與您的專案檔相同的資料夾中。

    > [!NOTE]
    > *.Wpp.targets*檔案必須與您的 web 應用程式專案檔相同的資料夾中，前往&#x2014;，例如*ContactManager.Mvc.csproj*&#x2014;而不是任何自訂的相同資料夾中用來控制組建和部署程序的專案檔案。
2. 在  *.wpp.targets*檔案中，建立新的 MSBuild 目標來執行*之前* **CopyAllFilesToSingleFolderForPackage**目標。 這是建立要包含在封裝中的項目清單 WPP 目標。
3. 在新的目標，建立**ItemGroup**項目。
4. 在  **ItemGroup**項目，新增**FilesForPackagingFromProject**項目，然後指定*應用程式\_offline.htm*檔案。

*.Wpp.targets*檔案看起來應該像這樣：


[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]


請注意，在此範例的重點如下：

- **BeforeTargets**屬性會插入到藉由指定 WPP 這個目標應在執行之前，立即**CopyAllFilesToSingleFolderForPackage**目標。
- **FilesForPackagingFromProject**項目會使用**DestinationRelativePath**若要重新命名的檔案中繼資料值*應用程式\_離線 template.htm*若要*應用程式\_offline.htm* ，它會新增至清單。

下一個程序說明如何新增這 *.wpp.targets* web 應用程式專案的檔案。

**若要新增。 web 部署封裝的.wpp.targets 檔案**

1. Visual Studio 2010 中開啟您的方案。
2. 中**方案總管 中** 視窗中，以滑鼠右鍵按一下您 web 應用程式的專案節點 (例如**ContactManager.Mvc**)，指向**新增**，然後按一下  **新項目**。
3. 在 **加入新項目**對話方塊中，選取**XML 檔案**範本。
4. 在 **名稱**方塊中，輸入 *[專案名稱] * * *.wpp.targets** (比方說， **ContactManager.Mvc.wpp.targets**)，然後按一下 **新增**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > 如果您將新的項目加入專案的根節點時，檔案會建立專案檔相同資料夾中。 您可以在 Windows 檔案總管中開啟資料夾來確認。
5. 在檔案中，新增 MSBuild 標記先前所述。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. 儲存並關閉 *[專案名稱].wpp.targets*檔案。

下次當您建置和封裝您的 web 應用程式專案，WPP 將會自動偵測 *.wpp.targets*檔案。 *應用程式\_離線 template.htm*檔案會包含在產生的 web 部署封裝，做為*應用程式\_offline.htm*。

> [!NOTE]
> 如果您的部署失敗，*應用程式\_offline.htm*檔案會留在原處，和您的應用程式會保持離線狀態。 這通常是所要的行為。 若要讓您的應用程式恢復上線，您可以刪除*應用程式\_offline.htm*從您的 web 伺服器的檔案。 或者，如果您更正任何錯誤，並執行部署成功*應用程式\_offline.htm*檔案將會移除。


## <a name="conclusion"></a>結論

本主題說明如何取得發佈的 web 應用程式離線部署的持續時間*應用程式\_offline.htm*檔案到目的地伺服器，在開始部署程序，並移除在結束。 它也會介紹如何納入*應用程式\_offline.htm* web 部署套件中的檔案。

## <a name="further-reading"></a>進一步閱讀

如需有關封裝和部署程序的詳細資訊，請參閱 <<c0> [ 建置和封裝 Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)，[設定網頁套件部署的參數](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)，和[部署網頁套件](../web-deployment-in-the-enterprise/deploying-web-packages.md)。

如果您發行您的 web 應用程式，直接從 Visual Studio 中，而不是使用這些教學課程中所述的自訂 MSBuild 專案檔案方法，您必須使用稍有不同的方式來接受您的應用程式離線期間發佈程序。 如需詳細資訊，請參閱 <<c0> [ 如何在發行期間取得您 web 應用程式離線](https://go.microsoft.com/?linkid=9805135)（部落格文章）。

> [!div class="step-by-step"]
> [上一頁](excluding-files-and-folders-from-deployment.md)
> [下一頁](running-windows-powershell-scripts-from-msbuild-project-files.md)
