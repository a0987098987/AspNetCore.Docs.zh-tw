---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 使用 Visual Studio 的 ASP.NET Web 部署： 部署的額外檔案 |Microsoft 文件
author: tdykstra
description: 此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: 5cefcedde7715844a7d7a9db1455193564ef9805
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>使用 Visual Studio 的 ASP.NET Web 部署： 部署額外的檔案
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。 數列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。


## <a name="overview"></a>總覽

本教學課程會示範如何擴充 Visual Studio web 發行管線部署期間進行一個額外的工作。 工作是要複製到目的地網站的專案資料夾中所沒有的額外檔案。

在此教學課程中，您將會複製一個額外的檔案： *robots.txt*。 您想要將此檔案部署到預備環境，但不是屬於實際執行環境。 在[到生產環境部署](deploying-to-production.md)教學課程，您加入至專案的這個檔案，並設定生產發行設定檔，以將其排除。 在本教學課程中，您會看到使用替代方法來處理此情況下，一個適用於您想要部署但不想要包含在專案中的任何檔案。

## <a name="move-the-robotstxt-file"></a>移動 robots.txt 檔案

若要準備用於不同的處理方法*robots.txt*本節的教學課程中您將檔案移至資料夾未包含在專案中，且您刪除*robots.txt*從臨時區域環境。 若要刪除檔案，從暫存，以便您可以驗證您的檔案部署到該環境的新方法正常運作必須。

1. 在**方案總管] 中**，以滑鼠右鍵按一下*robots.txt*檔案，然後按一下 [**從專案移除**。
2. 使用 Windows 檔案總管，在方案資料夾中建立新的資料夾，並將它命名*ExtraFiles*。
3. 移動*robots.txt*檔案從*ContosoUniversity*到專案資料夾*ExtraFiles*資料夾。

    ![ExtraFiles 資料夾](deploying-extra-files/_static/image1.png)
4. 使用您的 FTP 工具，刪除*robots.txt*從預備網站的檔案。

    或者，您可以選取**移除目的端的其他檔案**下**檔案發行選項**上**設定**預備發行設定檔 索引標籤和重新發行到預備環境。

## <a name="update-the-publish-profile-file"></a>更新發行設定檔

您只需要*robots.txt*中暫置，因此您需要更新，才能部署它唯一的發行設定檔暫存。

1. 在 Visual Studio 中開啟*Staging.pubxml*。
2. 結尾的檔案，之後再關閉`</Project>`標記中，加入下列標記：

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    此程式碼會建立新*目標*，將會收集其他要部署的檔案。 目標組成一或更多的工作將執行 MSBuild，根據您指定的條件。

    `Include`屬性會指定在其中尋找檔案的資料夾是*ExtraFiles*位於相同的層級的專案資料夾。 MSBuild 會收集所有檔案從該資料夾，然後以遞迴方式從任何子資料夾 （double 星號指定遞迴的子資料夾）。 這段程式碼與您無法將多個檔案和檔案子資料夾中*ExtraFiles*資料夾，以及所有將會部署。

    `DestinationRelativePath`項目可讓您指定的資料夾和檔案應該複製到目的地網站上，在相同的檔案及資料夾結構的根資料夾中找到*ExtraFiles*資料夾。 如果您想要複製*ExtraFiles*資料夾本身，`DestinationRelativePath`值會是*ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*。
3. 結尾的檔案，之後再關閉`</Project>`標記中，加入下列標記會指定何時要執行新的目標。

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    此程式碼會造成新`CustomCollectFiles`執行目標將檔案複製到目的地資料夾時要執行的目標。 沒有與部署套件建立、 發行的個別目標，且新的目標插入在這兩個目標，萬一您決定要使用的部署封裝，而不發行部署。

    *.Pubxml*檔現在看起來像下列的範例：

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. 儲存並關閉*Staging.pubxml*檔案。

## <a name="publish-to-staging"></a>發行到預備環境

使用單鍵發行或命令列中，發行該應用程式使用的暫存設定檔。

如果您使用單鍵發行，您可以在 確認**預覽**視窗， *robots.txt*會被複製。 否則，請使用您的 FTP 工具可讓您確認*robots.txt*檔案是在部署後網站的根資料夾中。

## <a name="summary"></a>總結

如此即完成這一系列的 ASP.NET web 應用程式部署到協力廠商主機服務提供者上的教學課程。 如需任何這些教學課程涵蓋之主題的詳細資訊，請參閱[ASP.NET 部署內容地圖](https://go.microsoft.com/fwlink/p/?LinkId=282413)。

## <a name="more-information"></a>詳細資訊

如果您知道如何使用 MSBuild 檔案，您可以撰寫程式碼來自動化許多其他部署工作*.pubxml*檔案 （適用於特定設定檔的工作） 或專案*。 wpp.targets*檔案 （如工作適用於所有設定檔）。 如需詳細資訊*.pubxml*和*。 wpp.targets*檔，請參閱[如何： 編輯發行設定檔 (.pubxml) 檔中的部署設定而。 wpp.targets Visual Studio Web 中的檔案專案](https://msdn.microsoft.com/library/ff398069)。 MSBuild 的程式碼的基本簡介，請參閱**剖析專案檔**中[企業部署序列： 了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。 若要了解如何使用 MSBuild 檔案，為您自己的案例中執行工作，請參閱此活頁簿：[內 Microsoft Build Engine： 使用 MSBuild 和 Team Foundation Build](http://msbuildbook.com) Sayed Ibraham Hashimi 和 William Bartholomew。

## <a name="acknowledgements"></a>謝誌

我想要感謝下列重大貢獻內容本教學課程系列的人：

- [Alberto Poblacion、 MVP &amp; mct 規範、 西班牙](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Jarod Ferguson，資料平台開發 MVP，美國
- 惡劣 Mittal、 Microsoft
- [Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike 教宗 Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava Microsoft
- [Raffaele Rialdi （義大利）](http://www.iamraf.net/)
- [Rick Anderson Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi、 Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott 獵人、 Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović、 塞爾維亞](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi、 Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [上一頁](command-line-deployment.md)
> [下一頁](troubleshooting.md)
