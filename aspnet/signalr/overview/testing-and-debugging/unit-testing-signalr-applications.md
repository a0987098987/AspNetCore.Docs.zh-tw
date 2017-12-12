---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: "單元測試 SignalR 應用程式 |Microsoft 文件"
author: pfletcher
description: "本文說明如何使用 SignalR 2.0 的單元測試功能。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: e55efd644dd4b6fb57061ffb89a5c041136c7b5e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="unit-testing-signalr-applications"></a>單元測試的 SignalR 應用程式
====================
由[Patrick Fletcher](https://github.com/pfletcher)

> 本文說明如何使用 SignalR 2 的單元測試功能。 
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
> ## <a name="questions-and-comments"></a>問題和註解
> 
> 請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。 如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>單元測試 SignalR 應用程式

您可以使用 SignalR 2 中的單元測試功能，來建立 SignalR 應用程式的單元測試。 SignalR 2 還包含[IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx)介面，可用來建立模擬物件來模擬測試中樞的方法。

在本節中，您會將應用程式中建立的單元測試[快速入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)使用[XUnit.net](https://github.com/xunit/xunit)和[Moq](https://github.com/Moq/moq4)。

XUnit.net 會用來控制測試。Moq 將用來建立[模擬](http://en.wikipedia.org/wiki/Mock_object)進行測試的物件。 如果預期; 可以使用其他模擬架構[NSubstitute](http://nsubstitute.github.io/)也是不錯的選擇。 本教學課程示範如何設定兩種方式模擬物件： 首先，使用`dynamic`物件 （.NET Framework 4 中引入） 和第二個，使用的介面。

### <a name="contents"></a>內容

本教學課程包含下列各節。

- [使用動態執行單元測試](#dynamic)
- [單元測試的類型](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>使用動態執行單元測試

在本節中，您會加入應用程式中建立的單元測試[快速入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)使用動態物件。

1. 安裝[XUnit 執行器延伸模組](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099)for Visual Studio 2013。
2. 完成[快速入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)，或下載完成的應用程式從[MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)。
3. 如果您使用下載的入門應用程式版本，開啟**Package Manager Console**按一下**還原**SignalR 封裝加入專案。

    ![還原封裝](unit-testing-signalr-applications/_static/image1.png)
4. 將專案加入至單元測試的方案。 以滑鼠右鍵按一下方案中的**方案總管 中**選取**新增**，**新的專案...**.在下**C#**節點中，選取**Windows**節點。 選取**類別庫**。 將新專案**TestLibrary**按一下**確定**。

    ![建立測試程式庫](unit-testing-signalr-applications/_static/image2.png)
5. 在測試程式庫專案中加入 SignalRChat 專案的參考。 以滑鼠右鍵按一下**TestLibrary**專案，然後選取**新增**，**參考...**.選取**專案**節點下的**方案**節點，然後核取**SignalRChat**。 按一下 [確定]。

    ![加入專案參考](unit-testing-signalr-applications/_static/image3.png)
6. 將 SignalR、 Moq 和 XUnit 封裝加入**TestLibrary**專案。 在**Package Manager Console**，將**預設專案**下拉式清單來**TestLibrary**。 在主控台視窗中執行下列命令：

    - `Install-Package Microsoft.AspNet.SignalR`
    - `Install-Package Moq`
    - `Install-Package XUnit`

    ![安裝封裝](unit-testing-signalr-applications/_static/image4.png)
7. 建立測試檔案。 以滑鼠右鍵按一下**TestLibrary**專案，然後按一下**新增...**，**類別**。 將新類別**Tests.cs**。
8. Tests.cs 的內容取代為下列程式碼。

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    在上述程式碼測試用戶端會建立使用`Mock`物件從[Moq](https://github.com/Moq/moq4)型別的文件庫[IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (在 SignalR 2.1 中，指派`dynamic`類型參數。）`IHubCallerConnectionContext`介面是用您叫用方法，用戶端上的 proxy 物件。 `broadcastMessage`以便可由呼叫然後函式定義模擬用戶端`ChatHub`類別。 測試引擎會呼叫`Send`方法`ChatHub`類別，依序呼叫 模擬`broadcastMessage`函式。
9. 按下建置此方案**F6**。
10. 執行單元測試。 在 Visual Studio 中，選取**測試**， **Windows**，**測試總管**。 在 [測試總管] 視窗中，以滑鼠右鍵按一下**HubsAreMockableViaDynamic**選取**執行選取的測試**。

    ![測試總管](unit-testing-signalr-applications/_static/image5.png)
11. 請確認測試成功藉由在測試總管 視窗的下方窗格。 視窗會顯示測試成功。

    ![成功的測試](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>單元測試的類型

在本節中，您要加入的測試中建立的應用程式[快速入門教學課程](../getting-started/tutorial-getting-started-with-signalr.md)使用包含要測試之方法的介面。

1. 完成步驟 1-7[單元測試與動態](#dynamic)上述的教學課程。
2. Tests.cs 的內容取代為下列程式碼。

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    上述程式碼，已建立介面定義的簽章`broadcastMessage`測試引擎會建立模擬用戶端的方法。 模擬用戶端接著會建立使用`Mock`型別的物件[IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (在 SignalR 2.1 中，指派`dynamic`型別參數。)`IHubCallerConnectionContext`介面是用您叫用方法，用戶端上的 proxy 物件。

    測試接著會建立的執行個體`ChatHub`，然後建立的模擬版本`broadcastMessage`方法中，依序叫用呼叫`Send`集線器上的方法。
3. 按下建置此方案**F6**。
4. 執行單元測試。 在 Visual Studio 中，選取**測試**， **Windows**，**測試總管**。 在 [測試總管] 視窗中，以滑鼠右鍵按一下**HubsAreMockableViaDynamic**選取**執行選取的測試**。

    ![測試總管](unit-testing-signalr-applications/_static/image7.png)
5. 請確認測試成功藉由在測試總管 視窗的下方窗格。 視窗會顯示測試成功。

    ![成功的測試](unit-testing-signalr-applications/_static/image8.png)
