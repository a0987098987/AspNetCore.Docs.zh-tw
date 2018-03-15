---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: "函式使用 Web 應用程式離線 Web 部署 |Microsoft 文件"
author: jrjlee
description: "本主題說明如何取得 web 應用程式離線期間自動化部署使用網際網路資訊服務 (IIS) Web 高於..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: 1c262ec7b834107524a18c6552b171f731452c91
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
---
<a name="taking-web-applications-offline-with-web-deploy"></a>函式使用 Web 應用程式離線 Web 部署
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何取得 web 應用程式離線以自動化部署使用網際網路資訊服務 (IIS) Web Deployment Tool (Web Deploy) 的持續時間。 瀏覽至 web 應用程式的使用者重新導向至*應用程式\_offline.htm*檔案部署完成之前。


本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案 & #x 2014;[連絡人管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)（& s) 來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 與 web 應用程式的 #x 2014;Communication Foundation (WCF) 服務，與資料庫專案。

這些教學課程的核心的部署方法為基礎所說明的分割專案檔案方法[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，這在建置程序由兩個專案中檔案 & #x 2014; 一個包含建置適用於每個目的地環境中和包含特定環境的建置和部署設定的指示。 在建置時，環境特定專案檔就會合併環境無從驗證專案檔來形成一組完整組建指示。

## <a name="task-overview"></a>工作概觀

在許多案例中，您會想讓 web 應用程式離線，當您變更相關的元件，例如資料庫或 web 服務。 一般而言，IIS 和 ASP.NET，在您完成這項作業放入名為*應用程式\_offline.htm* IIS 網站或 web 應用程式的根資料夾中。 *應用程式\_offline.htm*檔案是標準 HTML 檔案，而且通常會包含一個簡單的訊息，告知使用者的站台因維護而暫時無法使用。 雖然*應用程式\_offline.htm*檔案存在於網站的根資料夾中，IIS 會自動將任何要求導向至檔案。 當您完成進行更新時，請移除*應用程式\_offline.htm*檔案和網站會繼續如往常般處理要求。

當您使用 Web Deploy 來執行自動化或單一步驟部署至目標環境時，您可能想要合併新增和移除*應用程式\_offline.htm*檔案到您的部署程序。 若要這樣做，您必須完成下列高階工作：

- Microsoft Build Engine (MSBuild) 專案檔中您用來控制部署程序，建立 MSBuild 目標複製*應用程式\_offline.htm*檔案到目的地伺服器之前的任何部署工作開始。
- 加入另一個移除的 MSBuild 目標*應用程式\_offline.htm*從目的地伺服器部署的所有工作完成時的檔案。
- 在 web 應用程式專案中，建立*。 wpp.targets*檔案，可確保*應用程式\_offline.htm* Web Deploy 叫用時，檔案會加入至部署套件。

本主題將說明如何執行這些程序。 工作與本主題中的逐步解說假設，您已經建立的方案包含至少一個 web 應用程式專案，而且您使用自訂專案檔中所述，控制部署程序[中的 Web 部署企業](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md)。 或者，您可以使用[連絡人管理員](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)範例要遵循本主題中的範例方案。

## <a name="adding-an-appoffline-file-to-a-web-application-project"></a>加入應用程式\_加入 Web 應用程式專案的離線檔案

您需要完成第一個工作是加入*應用程式\_離線*您 web 應用程式專案的檔案：

- 若要防止檔案干擾開發程序 （您不想要永久離線應用程式），您應該呼叫它的項目以外*應用程式\_offline.htm*。 例如，您可以命名為檔案*應用程式\_離線 template.htm*。
- 若要防止檔案被部署為的是，您應該將建置動作設定為**無**。

**若要新增應用程式\_加入 web 應用程式專案的離線檔案**

1. Visual Studio 2010 中開啟您的方案。
2. 在**方案總管] 中**視窗中，以滑鼠右鍵按一下您的 web 應用程式專案，指向**新增**，然後按一下 [**新項目**。
3. 在**加入新項目**對話方塊中，選取**HTML 網頁**。
4. 在**名稱**方塊中，輸入**應用程式\_離線 template.htm**，然後按一下 **新增**。

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. 加入一些簡單的 HTML，通知使用者應用程式，就無法使用，並儲存檔案。 不包含任何伺服器端標記 (例如，前面會加上任何標記"asp:")。 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. 在**方案總管 中**視窗中，以滑鼠右鍵按一下新的檔案，然後**屬性**。
7. 在**屬性**視窗，請在**建置動作**列中選取**無**。

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-appoffline-file"></a>部署和刪除應用程式\_離線檔案

下一個步驟是修改您部署邏輯，並將檔案複製到目的地伺服器，部署程序的開頭和結尾移除它。

> [!NOTE]
> 下一個程序假設您要自訂的 MSBuild 專案檔使用來控制您的部署程序，如下所示[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。 如果您要部署直接從 Visual Studio，您必須使用不同的方式。 Sayed Ibrahim Hashimi 描述中的其中一個這類方法[如何取得您 Web 應用程式離線期間發佈](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx)。


若要部署*應用程式\_離線*檔案到目的地的 IIS 網站，您需要叫用使用 MSDeploy.exe [Web Deploy **contentPath**提供者](https://technet.microsoft.com/library/dd569034(WS.10).aspx)。 **ContentPath**提供者支援的實體目錄路徑和 IIS 網站或應用程式路徑，使其同步處理 Visual Studio 專案資料夾與 IIS web 應用程式之間的檔案的理想選擇。 若要部署的檔案，MSDeploy 命令應該會與以下相似：


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]


若要移除檔案，從目的地網站的部署程序結尾，MSDeploy 命令應該會與以下相似：


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]


若要自動化這些命令做為建置和部署處理程序的一部分，您需要將它們整合到自訂的 MSBuild 專案檔。 下一個程序描述如何執行這項操作。

**部署並刪除應用程式\_離線檔案**

1. 在 Visual Studio 2010 中，開啟 MSBuild 專案檔可控制您的部署程序。 在[連絡人管理員](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)範例方案中，這是*Publish.proj*檔案。
2. 在根**專案**項目，建立新**PropertyGroup**項目儲存為變數*應用程式\_離線*部署：

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. **SourceRoot**中其他位置定義屬性*Publish.proj*檔案。 它會指出相對於目前路徑 & #x 2014年; 換句話說的位置相對的來源內容的根資料夾的位置*Publish.proj*檔案。
4. **ContentPath**提供者將不接受相對檔案路徑，因此您需要取得原始程式檔的絕對路徑，才能部署它。 您可以使用[ConvertToAbsolutePath](https://msdn.microsoft.com/library/bb882668.aspx)工作來執行這項操作。
5. 加入新**目標**名**GetAppOfflineAbsolutePath**。 在此目標，使用**ConvertToAbsolutePath**工作來取得的絕對路徑*應用程式\_離線範本*專案資料夾中的檔案。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. 這個目標會採用的相對路徑*應用程式\_離線範本*專案資料夾中的檔案，並將它儲存到新的屬性做為絕對檔案路徑。 **之 BeforeTargets**屬性會指定您想要這個目標之前執行**DeployAppOffline**目標，您會在下一個步驟建立。
7. 加入名為新目標**DeployAppOffline**。 這個目標，內叫用 MSDeploy.exe 命令部署您*應用程式\_離線*目的地 web 伺服器的檔案。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. 在此範例中， **ContactManagerIisPath**專案檔中其他地方定義屬性。 這是只需 IIS 應用程式路徑，在表單中的*[IIS 網站名稱] / [應用程式名稱]*。 包含目標中的條件，可讓使用者切換*應用程式\_離線*部署開啟或關閉透過變更屬性值，或提供命令列參數。
9. 加入名為新目標**DeleteAppOffline**。 這個目標，內叫用 MSDeploy.exe 命令會移除您*應用程式\_離線*目的地 web 伺服器的檔案。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. 最後一項工作是在專案檔的執行期間叫用這些新的目標在合適的點。 您可以透過各種方式。 例如，在*Publish.proj*檔案， **FullPublishDependsOn**屬性會指定必須執行的目標清單中排序時**FullPublish**預設目標會叫用。
11. 修改 MSBuild 專案檔，才能叫用**DeployAppOffline**和**DeleteAppOffline**在發佈程序中的適當點目標。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

當您執行您自訂的 MSBuild 專案檔，*應用程式\_離線*檔案將會在建置成功之後，立即部署到伺服器。 部署的所有工作完成後，它就會從伺服器刪除。

## <a name="adding-an-appoffline-file-to-deployment-packages"></a>加入應用程式\_至部署套件的離線檔案

視您如何設定您的部署中，任何現有的內容在目的地 IIS web 應用程式 & #x 2014; 例如*應用程式\_offline.htm*檔案 & #x 2014; 當您部署 web 時可能會自動刪除目的地的封裝。 若要確保*應用程式\_offline.htm*檔案持續作用，部署期間，您需要將檔案直接在開始部署請另外包含本身的 web 部署套件中的檔案部署程序。

- 如果您已經遵循本主題中先前的工作，您會加入*應用程式\_offline.htm*至不同的檔案名稱底下的 web 應用程式專案的檔案 (我們使用*應用程式\_離線 template.htm*)，您將已設定的建置動作為**無**。 這些變更是為了避免干擾開發和偵錯中的檔案。 如此一來，您需要自訂封裝程序，以確保*應用程式\_offline.htm* web 部署套件中包含檔案。

Web 發行管線 (WPP) 會使用名為的項目清單**FilesForPackagingFromProject**建置的 web 部署套件中應包含的檔案清單。 您可以自訂 web 封裝的內容，將您自己的項目加入至這個清單。 若要這樣做，您需要完成下列高階步驟：

1. 建立自訂專案檔名為*[專案名稱].wpp.targets*專案檔相同資料夾中。

    > [!NOTE]
    > *。 Wpp.targets*檔案必須放在與您的 web 應用程式專案檔 & #x 2014; 相同的資料夾，例如*ContactManager.Mvc.csproj*（& d) 做為任何 #x 2014; 而在同一個資料夾您用來控制組建和部署程序的自訂專案檔案。
2. 在*。 wpp.targets*檔案中，建立新的 MSBuild 目標執行*之前* **CopyAllFilesToSingleFolderForPackage**目標。 這是建立要包含在封裝中的項目清單 WPP 目標。
3. 在新的目標建立**ItemGroup**項目。
4. 在**ItemGroup**項目，加入**FilesForPackagingFromProject**項目，並指定*應用程式\_offline.htm*檔案。

*。 Wpp.targets*檔案應該會與以下相似：


[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]


請注意，在此範例的重點如下：

- **之 BeforeTargets**屬性插入到藉由指定 WPP 應在執行前立即這個目標**CopyAllFilesToSingleFolderForPackage**目標。
- **FilesForPackagingFromProject**項目使用**DestinationRelativePath**中繼資料值從檔案重新命名*應用程式\_離線 template.htm*若要*應用程式\_offline.htm*將它加入至清單。

下一個程序會示範如何加入此*。 wpp.targets* web 應用程式專案的檔案。

**若要加入。 wpp.targets web 部署套件的檔案**

1. Visual Studio 2010 中開啟您的方案。
2. 在**方案總管] 中**視窗中，以滑鼠右鍵按一下您 web 應用程式的專案節點 (比方說， **ContactManager.Mvc**)，指向**新增**，然後按一下 [ **新項目**。
3. 在**加入新項目**對話方塊中，選取**XML 檔案**範本。
4. 在**名稱**方塊中，輸入*[專案名稱] * * *.wpp.targets** (例如， **ContactManager.Mvc.wpp.targets**)，然後按一下 **新增**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > 如果您將新的項目加入專案的根節點時，檔案會建立專案檔相同資料夾中。 您可以驗證，請在 Windows 檔案總管 中開啟資料夾。
5. 在檔案中，加入的 MSBuild 標記先前所述。

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. 儲存並關閉*[專案名稱].wpp.targets*檔案。

下次當您建置並封裝您的 web 應用程式專案，WPP 會自動偵測*。 wpp.targets*檔案。 *應用程式\_離線 template.htm*檔案將會包含在產生的 web 部署封裝，做為*應用程式\_offline.htm*。

> [!NOTE]
> 如果您的部署失敗，*應用程式\_offline.htm*檔案會保留在位置和您的應用程式角色將保持離線。 這通常是所要的行為。 若要將您的應用程式恢復上線，您可以刪除*應用程式\_offline.htm*檔案從您網頁伺服器。 或者，如果您更正任何錯誤，然後執行成功的部署，*應用程式\_offline.htm*檔案將會移除。


## <a name="conclusion"></a>結論

本主題描述如何取得 web 應用程式離線期間的部署，透過發行*應用程式\_offline.htm*檔案到目的地伺服器，在開始部署程序，並移除在結束。 它也會涵蓋如何納入*應用程式\_offline.htm* web 部署套件中的檔案。

## <a name="further-reading"></a>進一步閱讀

如需有關封裝和部署程序的詳細資訊，請參閱[建置和封裝 Web 應用程式專案](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md)， [Web 封裝部署的設定參數](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md)，和[部署 Web 封裝](../web-deployment-in-the-enterprise/deploying-web-packages.md)。

如果您發行您的 web 應用程式，直接從 Visual Studio 中，而不是使用這些教學課程中所述的自訂 MSBuild 專案檔案方法，您必須使用稍微不同的方法來採取應用程式離線發行期間程序。 如需詳細資訊，請參閱[發行期間進行 web 應用程式離線](https://go.microsoft.com/?linkid=9805135)（部落格文章）。

>[!div class="step-by-step"]
[上一頁](excluding-files-and-folders-from-deployment.md)
[下一頁](running-windows-powershell-scripts-from-msbuild-project-files.md)
