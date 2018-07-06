---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: 開始使用 OWIN 和 Katana |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 58a3d234867821d5e23cce2f01e105dfab88ac33
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826655"
---
<a name="getting-started-with-owin-and-katana"></a>開始使用 OWIN 和 Katana
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

[Open Web Interface for.NET (OWIN)](http://owin.org/)定義.NET web 伺服器和 web 應用程式之間的抽象概念。 藉由將分離應用程式的 web 伺服器，OWIN 可讓您更輕鬆地建立.NET web 程式開發的中介軟體。 此外，OWIN 輕鬆連接至其他主機的 web 應用程式&#8212;，例如自我裝載在 Windows 服務或其他處理序。

OWIN 是社群所擁有的規格，而不實作。 Katana 專案是一份由 Microsoft 所開發的開放原始碼 OWIN 元件。 OWIN 和 Katana 的一般概觀，請參閱 <<c0> [ 概觀的 Katana 專案](an-overview-of-project-katana.md)。 在本文中，我會直接跳到程式碼，以開始。

本教學課程會使用[Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566)，但您也可以使用 Visual Studio 2012。 幾個步驟是不同的 Visual Studio 2012，我說明如下。

## <a name="host-owin-in-iis"></a>將 OWIN 裝載在 IIS

在本節中，我們將會裝載在 IIS 中的 OWIN。 此選項可讓您搭配 IIS 的成熟的功能集 OWIN 管線的編寫性與彈性。 OWIN 應用程式使用此選項，在 ASP.NET 要求管線中執行。

首先，建立新的 ASP.NET Web 應用程式專案。 （在 Visual Studio 2012 中，使用 ASP.NET 空白 Web 應用程式專案類型）。

![](getting-started-with-owin-and-katana/_static/image1.png)

在 **新的 ASP.NET 專案**對話方塊中，選取**空白**範本。

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>新增 NuGet 封裝

接下來，新增必要的 NuGet 套件。 從**工具**功能表上，選取**程式庫套件管理員**，然後選取**Package Manager Console**。 在 [套件管理員主控台] 視窗中，輸入下列命令：

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>加入 Startup 類別

接下來，新增 OWIN 啟動類別。 在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**新增**，然後選取**新項目**。 在 **加入新項目**對話方塊中，選取**Owin 啟動類別**。 如需設定啟動類別的詳細資訊，請參閱[OWIN 啟動類別偵測](owin-startup-class-detection.md)。

![](getting-started-with-owin-and-katana/_static/image4.png)

將下列程式碼加入 `Startup1.Configuration` 方法：

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

此程式碼會將簡單的一種中介軟體新增至 OWIN 管線中，實作為函式來接收**Microsoft.Owin.IOwinContext**執行個體。 當伺服器收到 HTTP 要求時，OWIN 管線叫用中介軟體。 中介軟體會設定回應的內容類型，並寫入回應主體。

> [!NOTE]
> Visual Studio 2013 中使用 [OWIN 啟動類別] 範本。 如果您使用 Visual Studio 2012，只是加入新的空類別，名為`Startup1`，並貼上下列程式碼：


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>執行應用程式

按 F5 開始偵錯。 Visual Studio 會開啟瀏覽器視窗，以`http://localhost:*port*/`。 網頁看起來應該如下所示：

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>將 OWIN 自我裝載的主控台應用程式

它很容易從自我裝載的自訂處理序中的 IIS 裝載轉換此應用程式。 使用 IIS 裝載，IIS 會做為 HTTP 伺服器，以及裝載服務的處理序。 與自我裝載，您的應用程式建立程序，並使用**HttpListener**與 HTTP 伺服器的類別。

在 Visual Studio 中建立新的主控台應用程式。 在 [套件管理員主控台] 視窗中，輸入下列命令：

`Install-Package Microsoft.Owin.SelfHost -Pre`

新增`Startup1`本教學課程的第 1 部分的類別加入專案。 您不需要修改此類別。

實作應用程式的`Main`方法，如下所示。

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

當您執行主控台應用程式時，便會開始在伺服器接聽`http://localhost:9000`。 如果您導覽到此位址在網頁瀏覽器中，您會看到"Hello world"頁面。

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>新增 OWIN 診斷

Microsoft.Owin.Diagnostics 封裝包含中介軟體會攔截未處理例外狀況，並會顯示具有錯誤詳細資料的 HTML 網頁。 此頁面函式十分類似 ASP.NET 錯誤頁面，有時也稱為 「[黃色死亡畫面](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)」 (YSOD)。 如同 YSOD，Katana 錯誤頁面適合用在開發期間，但建議在生產模式中停用它。

若要安裝在您的專案中的診斷套件，請在 [套件管理員主控台] 視窗中輸入下列命令：

`install-package Microsoft.Owin.Diagnostics –Pre`

變更程式碼中的您`Startup1.Configuration`方法，如下所示：

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

現在使用 CTRL + F5 執行應用程式，但不偵錯，讓 Visual Studio 不會在例外狀況中斷。 應用程式的行為相同，直到您瀏覽至`http://localhost/fail`，此時應用程式擲回例外狀況。 錯誤頁面中介軟體將會攔截例外狀況，並顯示具有錯誤的相關資訊的 HTML 網頁。 您可以按一下索引標籤來查看堆疊、 查詢字串、 cookie、 要求標頭和 OWIN 環境變數。

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>後續步驟

- [OWIN 啟動類別偵測](owin-startup-class-detection.md)
- [使用 OWIN 自我裝載 ASP.NET Web API](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [使用 OWIN 自我裝載 SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
