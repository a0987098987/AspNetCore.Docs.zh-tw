---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: "開始使用 OWIN 與 Katana |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/27/2013
ms.topic: article
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 8922aada723da9b149ec111902fcd883c8241dfb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-owin-and-katana"></a>OWIN 和 Katana 入門
====================
由[Mike Wasson](https://github.com/MikeWasson)

[開啟 Web 介面的.NET (OWIN)](http://owin.org/)定義.NET web 伺服器和 web 應用程式之間的抽象概念。 透過分離應用程式的 web 伺服器，OWIN 可讓您更輕鬆地建立適用於.NET web 開發的中介軟體。 OWIN 此外，可以更輕鬆地 web 應用程式移植到其他主機 &#8212; 例如，在 Windows 服務或其他處理程序中自我裝載。

OWIN 是社群所擁有的規格，而不實作。 Katana 專案是由 Microsoft 開發的開放原始碼 OWIN 元件的一組。 OWIN 和 Katana 的一般概觀，請參閱[的專案 Katana 概觀](an-overview-of-project-katana.md)。 在本文中，我將直接跳至程式碼以開始。

本教學課程使用[Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566)，但您也可以使用 Visual Studio 2012。 在 Visual Studio 2012，我請注意以下幾個步驟截然不同。

## <a name="host-owin-in-iis"></a>主機在 IIS 中的 OWIN

在本節中，我們將會裝載在 IIS 中的 OWIN。 此選項可讓您彈性與撰寫性 OWIN 管線以及 IIS 的完整功能集。 OWIN 應用程式使用這個選項，在 ASP.NET 要求管線中執行。

首先，建立新的 ASP.NET Web 應用程式專案。 （在 Visual Studio 2012 中，使用 ASP.NET 空 Web 應用程式專案類型）。

![](getting-started-with-owin-and-katana/_static/image1.png)

在**新增 ASP.NET 專案**對話方塊中，選取**空**範本。

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>新增 NuGet 封裝

接下來，加入必要的 NuGet 封裝。 從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。 在 [封裝管理員主控台] 視窗中，輸入下列命令：

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>加入啟動類別

接下來，將 OWIN 啟動類別加入。 在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**新增**，然後選取**新項目**。 在**加入新項目**對話方塊中，選取**Owin 啟動 「 類別**。 如需設定啟動類別的詳細資訊，請參閱[OWIN 啟動類別偵測](owin-startup-class-detection.md)。

![](getting-started-with-owin-and-katana/_static/image4.png)

將下列程式碼加入至 `Startup1.Configuration` 方法中：

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

這個程式碼加入至 OWIN 管線，接收的函式以實作簡單的中介軟體片段**Microsoft.Owin.IOwinContext**執行個體。 當伺服器收到的 HTTP 要求時，OWIN 管線叫用中介軟體。 中介軟體設定回應的內容類型，並將寫入回應主體。

> [!NOTE]
> Visual Studio 2013 中使用 OWIN 啟動 「 類別樣板。 如果您使用 Visual Studio 2012，只要加入新的空類別，名為`Startup1`，並貼上下列程式碼：


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>執行應用程式

按 f5 鍵開始偵錯。 Visual Studio 會開啟瀏覽器視窗，以`http://localhost:*port*/`。 網頁看起來應該如下所示：

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>主控台應用程式中自我裝載的 OWIN

所以可以輕鬆地將此應用程式的自訂處理序中自我裝載的 IIS 裝載。 使用 IIS 裝載時，IIS 可作為 HTTP 伺服器和處理序主控伺服器。 具有自我裝載，您的應用程式會建立處理程序並使用**HttpListener**與 HTTP 伺服器的類別。

在 Visual Studio 中建立新的主控台應用程式。 在 [封裝管理員主控台] 視窗中，輸入下列命令：

`Install-Package Microsoft.Owin.SelfHost -Pre`

新增`Startup1`本教學課程的第 1 部分專案的類別。 您不需要修改此類別。

實作應用程式的`Main`方法，如下所示。

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

當您執行主控台應用程式時，伺服器會啟動接聽`http://localhost:9000`。 如果您導覽至這個網頁瀏覽器的位址，您會看到"Hello world"頁面。

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>新增 OWIN Diagnostics

Microsoft.Owin.Diagnostics 套件包含中介軟體會攔截未處理例外狀況，並會顯示具有錯誤的詳細資料的 HTML 網頁。 此頁面函式一樣，ASP.NET 錯誤網頁，有時也稱為 「[黃色死亡畫面](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)」 (YSOD)。 像 YSOD Katana 錯誤頁面很有用在開發期間，但它是最好的作法是在生產模式中停用它。

若要安裝在您的專案中的診斷封裝，請在 [封裝管理員主控台] 視窗中輸入下列命令：

`install-package Microsoft.Owin.Diagnostics –Pre`

在變更程式碼程式`Startup1.Configuration`方法，如下所示：

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

現在，讓 Visual Studio 不會在例外狀況中斷，請執行應用程式，但不偵錯，使用 CTRL + F5。 應用程式的行為相同如往常一般，直到您瀏覽至`http://localhost/fail`，此時應用程式會擲回例外狀況。 錯誤頁面中介軟體將會攔截例外狀況，並顯示具有錯誤的相關資訊的 HTML 網頁。 您可以按一下索引標籤來查看堆疊、 查詢字串、 cookie、 要求標頭，以及 OWIN 環境變數。

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>後續步驟

- [OWIN 啟動類別偵測](owin-startup-class-detection.md)
- [使用 OWIN 自我裝載 ASP.NET Web API](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [使用 OWIN 自我裝載 SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
