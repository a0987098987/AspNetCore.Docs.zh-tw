---
uid: signalr/overview/advanced/dependency-injection
title: "SignalR 中的相依性插入 |Microsoft 文件"
author: MikeWasson
description: "軟體版本本主題中使用 Visual Studio 2013.NET 4.5 SignalR 第 2 版舊版的此主題的較早版本的相關資訊..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3732b5d0ea6de841a6c402bfd5ef4dfb7b7a9162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="dependency-injection-in-signalr"></a>SignalR 中的相依性插入
====================
由[Mike Wasson](https://github.com/MikeWasson)， [Patrick Fletcher](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>本主題中使用的軟體版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR 第 2 版
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>本主題的先前版本
> 
> 如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。
> 
> ## <a name="questions-and-comments"></a>問題和註解
> 
> 請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。 如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。


相依性插入是以移除物件，以更輕鬆，來取代物件的相依性，針對測試 （使用模擬物件），或變更執行階段行為之間的硬式編碼相依性的方法。 本教學課程會示範如何執行相依性插入 SignalR 中樞上。 它也會示範如何使用 SignalR IoC 容器。 相依性插入的一般架構 IoC 容器。

## <a name="what-is-dependency-injection"></a>相依性插入是什麼？

如果您已經熟悉相依性插入，請略過本節。

*相依性插入*(DI) 是一種模式的物件不負責建立自己的相依性。 以下是我們 DI 的簡單範例。 假設您有需要記錄訊息的物件。 您可以定義記錄介面：

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

在您的物件，您可以建立`ILogger`記錄訊息：

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

這麼做，但它不是最佳設計。 如果您想要取代`FileLogger`與另一個`ILogger`實作中，您就必須修改`SomeComponent`。 內許多其他物件使用`FileLogger`，您必須變更它們全部。 或如果您決定是否要進行`FileLogger`單一值，您還需要進行整個應用程式的變更。

更好的方法是 「 插入 」`ILogger`的物件，例如，藉由使用建構函式引數：

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

現在物件不是負責選取其中`ILogger`使用。 您可以 swich`ILogger`實作，而不需要變更依存於此物件。

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

此模式呼叫[建構函式插入](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection)。 另一個模式是 setter 資料隱碼，您用來設定透過 setter 方法或屬性的相依性。

## <a name="simple-dependency-injection-in-signalr"></a>簡單的相依性插入 SignalR 中

交談應用程式，從教學課程，請考慮[入門 SignalR](../getting-started/tutorial-getting-started-with-signalr.md)。 以下是來自該應用程式的中樞類別：

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

假設您想要儲存在伺服器上的交談訊息之前傳送。 您可能會定義一個介面來擷取這項功能，並將介面使用 DI`ChatHub`類別。

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

唯一的問題是，SignalR 應用程式並不直接建立中樞;SignalR 為您建立它們。 根據預設，SignalR 預期中樞類別具有無參數建構函式。 不過，您可以輕鬆地註冊函式來建立中樞執行個體，及使用這個函數來執行 DI。 註冊此函數，藉由呼叫**GlobalHost.DependencyResolver.Register**。

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

SignalR 會叫用此匿名函式，每當需要建立現在`ChatHub`執行個體。

## <a name="ioc-containers"></a>IoC 容器

先前的程式碼，就可以簡單的案例。 但您還是必須撰寫這：

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

在具有許多相依性的複雜應用程式，您可能需要撰寫大量的這個 「 情節 」 程式碼。 此程式碼可能難以維護，特別是當巢狀相依性。 此外，也難以單元測試。

其中一種解決方案是使用 IoC 容器。 IoC 容器是負責管理相依性的軟體元件。您使用容器，註冊型別，並將容器來建立物件。 容器會自動找出的相依性關聯性。 許多 IoC 容器也可讓您控制等物件存留期範圍。

> [!NOTE]
> "IoC 「 代表 」 的控制項反轉 」，這是一般模式架構會呼叫應用程式程式碼。 IoC 容器建構您的物件，其中"反轉 」 一般控制流程。


## <a name="using-ioc-containers-in-signalr"></a>SignalR 中使用 IoC 容器

交談的應用程式可能是過於簡單受益於 IoC 容器。 相反地，讓我們看看[StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample)範例。

StockTicker 範例會定義兩個主要類別：

- `StockTickerHub`： 管理用戶端連線中樞類別。
- `StockTicker`： 單一保存股票價格並定期更新它們。

`StockTickerHub`保存的參考，`StockTicker`單一值，而`StockTicker`保存的參考， **IHubConnectionContext**如`StockTickerHub`。 它會使用這個介面來與通訊`StockTickerHub`執行個體。 (如需詳細資訊，請參閱[伺服器廣播透過 ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md)。)

若要隔離這些相依性的位元，我們可以使用 IoC 容器。 首先，讓我們來簡化`StockTickerHub`和`StockTicker`類別。 下列程式碼中，我已標記為註解組件，我們不需要。

移除從無參數建構函式`StockTickerHub`。 相反地，我們將一律使用 DI 建立中樞中。

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

對於 StockTicker，移除單一執行個體。 更新版本中，我們將使用 IoC 容器控制 StockTicker 存留期。 此外，請在建構函式公開。

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

接下來，我們可以重構程式碼所建立的介面`StockTicker`。 我們將使用此介面來分離`StockTickerHub`從`StockTicker`類別。

Visual Studio 可讓此種類型的重構很容易。 開啟檔案 StockTicker.cs、 以滑鼠右鍵按一下`StockTicker`類別宣告，並選取**重構**...**擷取介面**。

![](dependency-injection/_static/image1.png)

在**擷取介面**] 對話方塊中，按一下 [**全選**。 保留其他預設值。 按一下 [確定]。

![](dependency-injection/_static/image2.png)

Visual Studio 會建立名為的新介面`IStockTicker`，也會變更`StockTicker`衍生自`IStockTicker`。

開啟檔案 IStockTicker.cs 和變更的介面**公用**。

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

在`StockTickerHub`類別中變更兩個執行個體`StockTicker`至`IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

建立`IStockTicker`介面不是絕對必要，但我想顯示如何 DI 可協助減少在您的應用程式元件之間的結合。

## <a name="add-the-ninject-library"></a>加入 Ninject 程式庫

有許多適用於.NET 的開放原始碼 IoC 容器。 此教學課程中，我將會使用[Ninject](http://www.ninject.org/)。 (其他常用的程式庫包含[城堡 Windsor](http://www.castleproject.org/)， [Spring.Net](http://www.springframework.net/)， [Autofac](https://code.google.com/p/autofac/)， [Unity](https://github.com/unitycontainer/unity)，和[StructureMap](http://docs.structuremap.net).)

使用 NuGet 套件管理員來安裝[Ninject 文件庫](https://nuget.org/packages/Ninject/3.0.1.10)。 在 Visual Studio 中，從**工具**功能表中選取**程式庫套件管理員** | **Package Manager Console**。 在 [封裝管理員主控台] 視窗中，輸入下列命令：

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>取代 SignalR 相依性解析程式

若要使用 Ninject SignalR 內，建立衍生自類別**DefaultDependencyResolver**。

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

這個類別會覆寫**GetService**和**GetServices**方法**DefaultDependencyResolver**。 SignalR 呼叫這些方法來建立在執行階段，包括中樞執行個體，以及由 SignalR 內部使用的各種服務的各種物件。

- **GetService**方法會建立類型的單一執行個體。 覆寫此方法，呼叫 Ninject 核心**TryGet**方法。 如果該方法會傳回 null，切換回預設的解析程式。
- **GetServices**方法會建立指定類型之物件的集合。 覆寫這個方法，以串連 Ninject 的結果與預設的解析程式的結果。

## <a name="configure-ninject-bindings"></a>設定 Ninject 繫結

現在我們將使用 Ninject 宣告型別繫結。

開啟您的應用程式 Startup.cs 類別 (確認您是手動建立依照中的封裝指示`readme.txt`，或將驗證新增至您的專案中建立)。 在`Startup.Configuration`方法，建立 Ninject 容器，它會呼叫 Ninject*核心*。

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

建立我們自訂相依性解析程式的執行個體：

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

建立的繫結`IStockTicker`，如下所示：

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

此程式碼會指出模式兩件事。 首先，每當應用程式需要`IStockTicker`，核心應該建立的執行個體`StockTicker`。 第二個，`StockTicker`類別應該是建立為單一物件。 Ninject 會建立一個執行個體的物件，並傳回每個要求的相同執行個體。

建立的繫結**IHubConnectionContext** ，如下所示：

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

此程式碼 creatres 匿名函式會傳回**IHubConnection**。 **WhenInjectedInto**方法會告知使用此函式只有在建立時 Ninject`IStockTicker`執行個體。 原因是，會建立 SignalR **IHubConnectionContext**執行個體內部，而我們不想要覆寫 SignalR 如何建立它們。 此函式僅適用於我們`StockTicker`類別。

傳遞至相依性解析程式**MapSignalR**加中樞組態的方法：

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

更新 Startup.ConfigureSignalR 類別中的方法範例的啟動新的參數：

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

現在 SignalR 會使用在指定的解決器**MapSignalR**，而不是預設的解析程式。

以下是完整的程式碼清單`Startup.Configuration`。

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

若要執行 Visual Studio StockTicker 應用程式，請按 F5。 在瀏覽器視窗中，瀏覽至`http://localhost:*port*/SignalR.Sample/StockTicker.html`。

![](dependency-injection/_static/image3.png)

應用程式具有完全相同的功能為之前。 (如需說明，請參閱[伺服器廣播透過 ASP.NET SignalR](../getting-started/tutorial-server-broadcast-with-signalr.md)。)我們尚未改變行為。剛剛您更輕鬆地測試、 維護及發展的程式碼。
