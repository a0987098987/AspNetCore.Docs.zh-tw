---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: 單元測試 SignalR 應用程式 |Microsoft Docs
author: pfletcher
description: 本文說明如何使用 SignalR 2.0 的單元測試功能。
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: f46c6edd2002bcc6e62f4e4fe313c9e50e35aaff
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840901"
---
<a name="unit-testing-signalr-applications"></a>單元測試的 SignalR 應用程式
====================
藉由[Patrick Fletcher](https://github.com/pfletcher)

> 這篇文章說明如何使用 SignalR 2 的單元測試功能。 
> 
> ## <a name="software-versions-used-in-this-topic"></a>本主題中使用的軟體版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR 第 2 版
>   
> 
> 
> ## <a name="questions-and-comments"></a>提出問題或意見
> 
> 您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。 如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>單元測試 SignalR 應用程式

您可以使用 SignalR 2 中的單元測試功能，建立您的 SignalR 應用程式的單元測試。 SignalR 2 包含[IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx)介面，可用來建立模擬 （mock） 物件，以模擬您的中樞方法的測試。

在本節中，您將建立的應用程式的單元測試[快速入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)使用[XUnit.net](https://github.com/xunit/xunit)並[Moq](https://github.com/Moq/moq4)。

XUnit.net 會用來控制測試;將用來建立的 Moq[模擬](http://en.wikipedia.org/wiki/Mock_object)進行測試的物件。 如有需要，可以使用其他 （mock） 架構[NSubstitute](http://nsubstitute.github.io/)也是不錯的選擇。 本教學課程示範如何設定兩種方式的模擬 （mock） 物件： 首先，使用`dynamic`物件 （.NET Framework 4 中引入） 和第二，使用的介面。

### <a name="contents"></a>內容

本教學課程包含下列各節。

- [動態使用單元測試](#dynamic)
- [單元測試類型](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>動態使用單元測試

在本節中，您將建立的應用程式的單元測試[快速入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)使用動態物件。

1. 安裝[XUnit 執行器延伸模組](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099)適用於 Visual Studio 2013。
2. 請完成[快速入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)，或下載已完成的應用程式，從[MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)。
3. 如果您使用下載的快速入門應用程式版本，開啟**Package Manager Console**然後按一下**還原**SignalR 封裝加入專案。

    ![還原套件](unit-testing-signalr-applications/_static/image1.png)
4. 將專案加入至單元測試的方案。 以滑鼠右鍵按一下 在您的解決方案**方案總管**，然後選取**新增**，**新專案...**.底下**C#** 節點中，選取**Windows**節點。 選取 **類別庫**。 將新專案命名**TestLibrary**然後按一下**確定**。

    ![建立測試程式庫](unit-testing-signalr-applications/_static/image2.png)
5. 在測試程式庫專案中加入 SignalRChat 專案的參考。 以滑鼠右鍵按一下**TestLibrary**專案，然後選取**新增**，**參考...**.選取 **專案**下方的節點**解決方案**節點，然後核取**SignalRChat**。 按一下 [確定 **Deploying Office Solutions**]。

    ![加入專案參考](unit-testing-signalr-applications/_static/image3.png)
6. 將 SignalR、 Moq 和 XUnit 的封裝，加入**TestLibrary**專案。 在  **Package Manager Console**，將**預設專案**下拉式清單來**TestLibrary**。 在主控台視窗中執行下列命令：

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![安裝套件](unit-testing-signalr-applications/_static/image4.png)
7. 建立測試檔案。 以滑鼠右鍵按一下**TestLibrary**專案，然後按一下 **加入...**，**類別**。 新類別命名**Tests.cs**。
8. Tests.cs 的內容取代為下列程式碼。

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    在上述程式碼，測試用戶端會使用建立`Mock`物件從[Moq](https://github.com/Moq/moq4)類型的文件庫[IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (在 SignalR 2.1 中，指派`dynamic`類型參數。）`IHubCallerConnectionContext`介面是用您叫用方法，用戶端上的 proxy 物件。 `broadcastMessage`函式會接著定義模擬 （mock） 的用戶端，讓它可以呼叫`ChatHub`類別。 測試引擎接著會呼叫`Send`方法`ChatHub`類別，而呼叫模擬 （mock）`broadcastMessage`函式。
9. 建置方案，藉由按下**F6**。
10. 執行單元測試。 在 Visual Studio 中，選取**測試**， **Windows**，**測試總管**。 在 [測試總管] 視窗中，以滑鼠右鍵按一下**HubsAreMockableViaDynamic** ，然後選取**執行選取的測試**。

    ![測試總管](unit-testing-signalr-applications/_static/image5.png)
11. 請確認測試成功藉由檢查下方的窗格，在 [測試總管] 視窗中。 視窗會顯示測試成功。

    ![成功的測試](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>單元測試類型

在本節中，您將建立的應用程式的測試[快速入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)使用介面，包含要測試的方法。

1. 完成步驟 1-7 中[單元測試與 Dynamic](#dynamic)上述的教學課程。
2. Tests.cs 的內容取代為下列程式碼。

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    在上述程式碼會建立介面定義的簽章`broadcastMessage`測試引擎會建立模擬 （mock） 的用戶端的方法。 模擬 （mock） 的用戶端接著會建立使用`Mock`類型的物件[IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (在 SignalR 2.1 中，指派`dynamic`做為 type 參數。)`IHubCallerConnectionContext`介面是用您叫用方法，用戶端上的 proxy 物件。

    測試再建立的執行個體`ChatHub`，然後建立模擬版本`broadcastMessage`方法中，依序叫用呼叫`Send`集線器上的方法。
3. 建置方案，藉由按下**F6**。
4. 執行單元測試。 在 Visual Studio 中，選取**測試**， **Windows**，**測試總管**。 在 [測試總管] 視窗中，以滑鼠右鍵按一下**HubsAreMockableViaDynamic** ，然後選取**執行選取的測試**。

    ![測試總管](unit-testing-signalr-applications/_static/image7.png)
5. 請確認測試成功藉由檢查下方的窗格，在 [測試總管] 視窗中。 視窗會顯示測試成功。

    ![成功的測試](unit-testing-signalr-applications/_static/image8.png)
