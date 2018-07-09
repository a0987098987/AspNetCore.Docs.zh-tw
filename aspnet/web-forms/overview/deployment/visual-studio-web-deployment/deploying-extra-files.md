---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 使用 Visual Studio 的 ASP.NET Web 部署： 部署額外的檔案 |Microsoft Docs
author: tdykstra
description: 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用...
ms.author: aspnetcontent
ms.date: 03/23/2015
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: 609c0be968e8f38d7be6e5f5c96a569a9a35c2eb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820846"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>使用 Visual Studio 的 ASP.NET Web 部署： 部署額外的檔案
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。 這個系列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。


## <a name="overview"></a>總覽

本教學課程會示範如何擴充 Visual Studio web 發行管線，以在部署期間執行額外的工作。 工作是將複製至目的地網站的 [專案] 資料夾中所沒有的額外檔案。

在本教學課程中，您將會複製一個額外的檔案： *robots.txt*。 您想要將此檔案部署至預備環境，而非生產環境。 在 [到生產環境部署](deploying-to-production.md)教學課程中，您加入專案中的這個檔案，並設定生產發行設定檔，以將其排除。 在本教學課程中，您會看到使用替代方法來處理這種情況，其中一個是適用於任何您想要部署，但不想要包含在專案中的檔案。

## <a name="move-the-robotstxt-file"></a>移動 robots.txt 檔案

若要準備不同的處理方法*robots.txt*，在本教學課程的這一節中您將檔案移至資料夾未包含在專案中，您刪除*robots.txt*從預備環境環境。 必須從預備環境，讓您可以驗證您的檔案部署到該環境的新方法正常運作中刪除檔案。

1. 在 [**方案總管] 中**，以滑鼠右鍵按一下*robots.txt*檔案，然後按一下**從專案移除**。
2. 使用 Windows 檔案總管，在方案資料夾中建立新的資料夾，並將它命名*ExtraFiles*。
3. 移動*robots.txt*檔案*ContosoUniversity*專案資料夾*ExtraFiles*資料夾。

    ![ExtraFiles 資料夾](deploying-extra-files/_static/image1.png)
4. 使用您的 FTP 工具，刪除*robots.txt*從預備網站的檔案。

    或者，您可以選取**移除目的地上的其他檔案**下方**檔案發行選項**上**設定**的預備發行設定檔 索引標籤和重新發佈至預備環境。

## <a name="update-the-publish-profile-file"></a>更新發行設定檔

您只需要*robots.txt*在預備環境，以便在預備唯一您需要更新才能將其部署的發行設定檔。

1. 在 Visual Studio 中開啟*Staging.pubxml*。
2. 結尾的檔案，在關閉前`</Project>`標記中加入下列標記：

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    此程式碼會建立新*目標*，將會收集其他一起部署的檔案。 目標組成一或更多的工作將執行 MSBuild，根據您指定的條件。

    `Include`屬性會指定在其中尋找檔案的資料夾是*ExtraFiles*，位於專案資料夾相同層級。 MSBuild 會從該資料夾，然後以遞迴方式從 （double 的星號會指定遞迴子資料夾） 的任何子資料夾中收集的所有檔案。 此程式碼您可以將放入多個檔案和檔案內的子資料夾*ExtraFiles*資料夾中，與所有部署。

    `DestinationRelativePath`項目會指定，檔案和資料夾，應該會複製到目的地網站上，在相同的檔案及資料夾結構的根資料夾中找到*ExtraFiles*資料夾。 如果您想要複製*ExtraFiles*本身的資料夾`DestinationRelativePath`的值會是*ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*。
3. 結尾的檔案，在關閉前`</Project>`標記中加入下列標記會指定何時要執行新的目標。

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    此程式碼會使新`CustomCollectFiles`每當執行目標，將檔案複製到目的地資料夾時要執行的目標。 有個別的目標，如發佈與部署套件建立，以及萬一您決定要使用的部署封裝，而不是發行部署兩個目標中插入新的目標。

    *.Pubxml*檔現在看起來如下列範例所示：

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. 儲存並關閉*Staging.pubxml*檔案。

## <a name="publish-to-staging"></a>發行至預備環境

使用單鍵發佈或命令列中，使用暫存設定檔來發佈應用程式。

如果您使用單鍵發行，您可以在 確認**Preview**視窗， *robots.txt*會被複製。 否則，請使用 FTP 工具來確認*robots.txt*檔案是在部署後網站的根資料夾中。

## <a name="summary"></a>總結

如此即完成本系列的教學課程將部署到協力廠商裝載提供者的 ASP.NET web 應用程式。 如需任何這些教學課程所涵蓋的主題的詳細資訊，請參閱[ASP.NET 部署內容對應](https://go.microsoft.com/fwlink/p/?LinkId=282413)。

## <a name="more-information"></a>詳細資訊

如果您知道如何使用 MSBuild 檔案，您可以自動化許多其他部署工作中撰寫程式碼 *.pubxml* （適用於設定檔的特定工作） 的檔案或專案 *.wpp.targets*檔案 （如工作適用於所有設定檔）。 如需詳細資訊 *.pubxml*並 *.wpp.targets*檔案，請參閱[How to： 編輯發行設定檔 (.pubxml) 檔中的部署設定而.wpp.targets 檔案在 Visual Studio Web專案](https://msdn.microsoft.com/library/ff398069)。 MSBuild 的程式碼的基本簡介，請參閱**專案檔的剖析**中[企業部署系列： 了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。 若要了解如何使用您自己的案例中執行工作的 MSBuild 檔案，請參閱本書：[在 Microsoft Build Engine： 使用 MSBuild 和 Team Foundation Build](http://msbuildbook.com) Sayed Ibraham Hashimi 和 William bartholomew< /。

## <a name="acknowledgements"></a>謝誌

我想要感謝下列人士提出重大貢獻的內容，本教學課程系列：

- [Alberto Poblacion、 MVP &amp; MCT，西班牙](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Jarod Ferguson，資料平台開發 MVP、 美國。
- Harsh Mittal，Microsoft
- [Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Kristina Olson Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike 主教 Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava Microsoft
- [Raffaele Rialdi 義大利](http://www.iamraf.net/)
- [Rick Anderson Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi，Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott Hunter、 Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović，塞爾維亞](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi、 Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [上一頁](command-line-deployment.md)
> [下一頁](troubleshooting.md)
