---
uid: signalr/overview/older-versions/dependency-injection
title: 相依性插入 signalr 1.x |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 05/15/2013
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 905dea4918be731673c39e788069ce2dc78e1649
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910690"
---
<a name="dependency-injection-in-signalr-1x"></a>相依性插入 signalr 1.x
====================
藉由[Mike Wasson](https://github.com/MikeWasson)， [Patrick Fletcher](https://github.com/pfletcher)

相依性插入是移除硬式編碼物件，方便您將物件的相依性，針對測試 （使用模擬 （mock） 物件），或變更執行階段行為之間的相依性的方法。 本教學課程會示範如何對 SignalR 中樞的相依性插入。 它也會示範如何使用 SignalR 使用 IoC 容器。 IoC 容器是一般架構的相依性插入。

## <a name="what-is-dependency-injection"></a>相依性插入是什麼？

如果您已經熟悉如何使用相依性插入，請略過本節。

*相依性插入*(DI) 是一種模式的物件不負責建立自己的相依性。 以下是激發 DI 的簡單範例。 假設您有需要記錄訊息的物件。 您可以定義記錄介面：

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

在您的物件，您可以建立`ILogger`記錄訊息：

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

可以運作，但它不是最好的設計。 如果您想要取代`FileLogger`彼此`ILogger`實作中，您就必須修改`SomeComponent`。 內的其他許多物件所使用`FileLogger`，您必須將所有這些變更。 或如果您決定要`FileLogger`單一值，您也需要變更整個應用程式。

更好的方法是 「 插入 」`ILogger`物件 — 例如，藉由使用建構函式引數：

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

現在物件不是負責選取`ILogger`使用。 您可以 swich`ILogger`而不需要變更依存於此物件的實作。

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

這個模式稱為[建構函式插入](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection)。 另一個模式是 setter 資料隱碼攻擊，您用來設定透過 setter 方法或屬性的相依性。

## <a name="simple-dependency-injection-in-signalr"></a>SignalR 中簡單的相依性插入

交談應用程式，從教學課程，請考慮[開始使用 SignalR](../getting-started/tutorial-getting-started-with-signalr.md)。 以下是從該應用程式的中樞類別：

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

假設您想要儲存在伺服器上的交談訊息，然後才傳送。 您可以定義介面，用來擷取這項功能，並使用 DI 插入至介面`ChatHub`類別。

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

唯一的問題是，SignalR 應用程式並不直接建立中樞;SignalR 會為您建立它們。 根據預設，SignalR 預期中樞類別具有無參數建構函式。 不過，您可以輕鬆地註冊函式來建立中樞執行個體，並用此函式執行 DI。 藉由呼叫註冊函式**GlobalHost.DependencyResolver.Register**。

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

現在 SignalR 會叫用此匿名函式，每當它需要建立`ChatHub`執行個體。

## <a name="ioc-containers"></a>IoC 容器

先前的程式碼是簡單的情況下正常的。 但您還是必須撰寫下列程式碼：

[!code-css[Main](dependency-injection/samples/sample8.css)]

在具有許多相依性的複雜應用程式，您可能需要撰寫大量的這個 「 連接 」 程式碼。 此程式碼很難維護，尤其是相依性巢狀。 此外，也難以使用單元測試。

其中一個解決方案是使用 IoC 容器。 IoC 容器是負責管理相依性的軟體元件。您向容器註冊類型，然後再使用 建立物件的 容器。 容器會自動找出相依性關聯性。 許多 IoC 容器也可讓您控制物件存留期和範圍等項目。

> [!NOTE]
> 「 IoC 」 代表 「 的控制項反轉 」，這是一種架構會呼叫應用程式程式碼的一般模式。 IoC 容器建構您的物件，其中 「 反轉 」 一般控制流程。


## <a name="using-ioc-containers-in-signalr"></a>使用 signalr 的 IoC 容器

交談應用程式可能是太簡單才會受益於 IoC 容器。 相反地，讓我們看看[Stockservices.asmx](http://nuget.org/packages/microsoft.aspnet.signalr.sample)範例。

Stockservices.asmx 範例會定義兩個主要類別：

- `StockTickerHub`： 管理用戶端連線 hub 類別。
- `StockTicker`: 保存股票的價格，而且會定期更新它們的 singleton。

`StockTickerHub` 保留的參考`StockTicker`單一值，雖然`StockTicker`保存參考**IHubConnectionContext**的`StockTickerHub`。 它會使用此介面來與通訊`StockTickerHub`執行個體。 (如需詳細資訊，請參閱 <<c0> [ 伺服器廣播與 ASP.NET SignalR](index.md)。)

若要更隔離這些相依性，我們可以使用 IoC 容器。 首先，讓我們簡化`StockTickerHub`和`StockTicker`類別。 下列程式碼中，我已標記為註解的部分，我們不需要。

移除無參數建構函式從`StockTicker`。 相反地，我們一律會使用 DI 建立中樞。

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

對於 Stockservices.asmx，移除單一執行個體。 稍後，我們將使用 IoC 容器來控制 Stockservices.asmx 存留期。 此外，請建構函式公開。

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

接下來，我們可以重構程式碼所建立的介面`StockTicker`。 我們將使用此介面來分離`StockTickerHub`從`StockTicker`類別。

Visual Studio 可讓這種重構很容易。 開啟檔案 StockTicker.cs，以滑鼠右鍵按一下`StockTicker`類別宣告，然後選取**重構**...**擷取介面**。

![](dependency-injection/_static/image1.png)

在 [**擷取介面**] 對話方塊中，按一下**全選**。 保留其他預設值。 按一下 [確定 **Deploying Office Solutions**]。

![](dependency-injection/_static/image2.png)

Visual Studio 會建立名為的新介面`IStockTicker`，也會變更`StockTicker`衍生自`IStockTicker`。

開啟檔案 IStockTicker.cs，並變更至介面**公開**。

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

在 `StockTickerHub`類別中變更兩個執行個體`StockTicker`到`IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

建立`IStockTicker`介面不是絕對必要，但是我想要顯示 DI 如何協助減少在您的應用程式元件之間的結合。

## <a name="add-the-ninject-library"></a>新增 Ninject 程式庫

有許多適用於.NET 的開放原始碼 IoC 容器。 本教學課程中，我將使用[Ninject](http://www.ninject.org/)。 (其他熱門的程式庫包含[Castle Windsor](http://www.castleproject.org/)， [Spring.Net](http://www.springframework.net/)， [Autofac](https://code.google.com/p/autofac/)， [Unity](https://github.com/unitycontainer/unity)，和[StructureMap](http://docs.structuremap.net).)

使用 NuGet 套件管理員來安裝[Ninject 文件庫](https://nuget.org/packages/Ninject/3.0.1.10)。 在 Visual Studio 中，從**工具**功能表中，選取**NuGet 套件管理員** > **Package Manager Console**。 在 [套件管理員主控台] 視窗中，輸入下列命令：

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>將 SignalR 相依性解析程式

若要使用 Ninject SignalR 內，建立衍生自類別**DefaultDependencyResolver**。

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

此類別會覆寫**GetService**並**GetServices**方法**DefaultDependencyResolver**。 SignalR 呼叫這些方法來建立在執行階段，包括中樞執行個體，以及各種服務供內部使用 SignalR 的各種物件。

- **GetService**方法會建立類型的單一執行個體。 覆寫此方法以呼叫 Ninject 核心**TryGet**方法。 如果該方法會傳回 null，切換回預設的解析程式。
- **GetServices**方法會建立指定型別的物件的集合。 覆寫這個方法，以串連結果 Ninject 與預設的解析程式的結果。

## <a name="configure-ninject-bindings"></a>設定 Ninject 繫結

現在我們將使用 Ninject 宣告型別繫結。

開啟檔案 RegisterHubs.cs。 在 `RegisterHubs.Start`方法，建立 Ninject 容器，它會呼叫 Ninject*核心*。

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

建立我們的自訂相依性解析程式的執行個體：

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

建立的繫結`IStockTicker`，如下所示：

[!code-html[Main](dependency-injection/samples/sample17.html)]

此程式碼會指出兩件事。 首先，每當應用程式需要`IStockTicker`，核心應該建立的執行個體`StockTicker`。 第二個，`StockTicker`類別應該是建立為單一物件。 Ninject 會建立一個執行個體的物件，並傳回每個要求的相同執行個體。

建立的繫結**IHubConnectionContext** ，如下所示：

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

此程式碼 creatres 匿名函式會傳回**IHubConnection**。 **WhenInjectedInto**方法會告訴只有在建立時，才使用這個函式的 Ninject`IStockTicker`執行個體。 原因是，會建立 SignalR **IHubConnectionContext**執行個體就內部而言，而且我們不想要覆寫如何 SignalR 便會建立它們。 此函式僅適用於我們`StockTicker`類別。

傳遞至相依性解析程式**MapHubs**方法：

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

SignalR 將使用在指定的解決器現在**MapHubs**，而不是預設的解析程式。

以下是完整的程式碼清單`RegisterHubs.Start`。

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

若要在 Visual Studio 中執行 Stockservices.asmx 應用程式，請按 F5。 在瀏覽器視窗中，瀏覽至`http://localhost:*port*/SignalR.Sample/StockTicker.html`。

![](dependency-injection/_static/image3.png)

應用程式有功能仍然與之前完全相同。 (如需說明，請參閱[伺服器廣播與 ASP.NET SignalR](index.md)。)我們尚未變更行為。剛才您更輕鬆地測試、 維護及發展的程式碼。
