---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: ASP.NET Web API 2 中的追蹤 |Microsoft 文件
author: MikeWasson
description: 示範如何在 ASP.NET Web API 中啟用追蹤。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7392ae5d9bc4c3aab45a9373099a0ee18e873a4f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
ms.locfileid: "28044204"
---
<a name="tracing-in-aspnet-web-api-2"></a>ASP.NET Web API 2 中的追蹤
====================
由[Mike Wasson](https://github.com/MikeWasson)

> 當您嘗試偵錯 web 應用程式時，是沒有替代的一組良好的追蹤記錄檔。 本教學課程會示範如何在 ASP.NET Web API 中啟用追蹤。 您可以使用這項功能，來追蹤 Web API framework 功能之前和之後叫用您的控制器。 您也可以使用它來追蹤您自己的程式碼。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/downloads/) （也可用於 Visual Studio 2015）
> - Web API 2
> - [Microsoft.AspNet.WebApi.Tracing](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a>啟用 System.Diagnostics 中 Web API 追蹤

首先，我們將建立新的 ASP.NET Web 應用程式專案。 在 Visual Studio 中，從**檔案**功能表上，選取**新增**，然後**專案**。 在下**範本**， **Web**，選取**ASP.NET Web 應用程式**。

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

選擇 Web API 專案範本。

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

從**工具**功能表上，選取**程式庫套件管理員**，然後**套件管理主控台**。

在 [封裝管理員主控台] 視窗中，輸入下列命令。

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

第一個命令會安裝最新的 Web API 追蹤封裝。 它也會更新核心 Web 應用程式開發介面封裝的封裝。 第二個命令將 WebApi.WebHost 套件更新為最新版本。

> [!NOTE]
> 如果您想要以特定版本的 Web API 為目標，使用當您安裝追蹤封裝-版本旗標。


在應用程式中開啟檔案 WebApiConfig.cs\_開始資料夾。 將下列程式碼加入**註冊**方法。

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

這個程式碼加入[SystemDiagnosticsTraceWriter](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) Web API 管線的類別。 **SystemDiagnosticsTraceWriter**類別會將寫入追蹤[System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace)。

若要查看追蹤，請在偵錯工具中執行應用程式。 在瀏覽器中，瀏覽至`/api/values`。

![](tracing-in-aspnet-web-api/_static/image5.png)

在 Visual Studio 中的 [輸出] 視窗會寫入追蹤陳述式。 (從**檢視**功能表上，選取**輸出**)。

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

因為**SystemDiagnosticsTraceWriter**寫入追蹤**System.Diagnostics.Trace**，您可以註冊其他追蹤接聽項; 例如，要寫入追蹤記錄檔。 如需有關追蹤寫入器的詳細資訊，請參閱[追蹤接聽項](https://msdn.microsoft.com/library/4y5y10s7.aspx)MSDN 上的主題。

### <a name="configuring-systemdiagnosticstracewriter"></a>設定的 SystemDiagnosticsTraceWriter

下列程式碼會示範如何設定追蹤寫入器。

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

有兩個您可以控制的設定：

- IsVerbose： 如果為 false，每個追蹤中包含最少資訊。 如果為 true，則追蹤會包含更多資訊。
- MinimumLevel： 設定最低追蹤層級。 追蹤層級，依照順序，是偵錯、 資訊、 警告、 錯誤和嚴重錯誤。

## <a name="adding-traces-to-your-web-api-application"></a>將追蹤加入至您的 Web API 應用程式

將追蹤寫入器可讓您立即存取 Web API 管線所建立的追蹤。 您也可以使用追蹤寫入器來追蹤您自己的程式碼：

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

若要取得追蹤寫入器，呼叫**HttpConfiguration.Services.GetTraceWriter**。 在控制站，這個方法是透過可存取**ApiController.Configuration**屬性。

若要撰寫追蹤，您可以呼叫**ITraceWriter.Trace**方法直接，但[ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx)類別定義會更好記某些擴充方法。 例如，**資訊**如上所示的方法建立的追蹤使用追蹤層級**資訊**。

## <a name="web-api-tracing-infrastructure"></a>Web API 追蹤基礎結構

本節說明如何撰寫 Web API 的自訂追蹤寫入器。

用於 Microsoft.AspNet.WebApi.Tracing 套件的建置 Web 應用程式開發介面中的多個一般追蹤基礎結構的頂端。 不使用用於 Microsoft.AspNet.WebApi.Tracing，您可以也插入其他除掉，然後再追蹤/程式庫，例如[NLog](http://nlog-project.org/)或[log4net](http://logging.apache.org/log4net/)。

若要收集追蹤，實作**ITraceWriter**介面。 以下是簡單的範例：

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

**ITraceWriter.Trace**方法會建立追蹤。 呼叫端指定類別目錄 」 和 「 追蹤層級。 類別可以是任何使用者定義的字串。 實作**追蹤**應該執行下列動作：

1. 建立新**TraceRecord**。 使用初始化要求、 類別和追蹤層級，如下所示。 呼叫端會提供這些值。
2. 叫用*traceAction*委派。 在此委派，呼叫端必須填滿的其餘部分**TraceRecord**。
3. 寫入**TraceRecord**，使用任何您喜歡的記錄方法。 此處顯示的範例只會呼叫至**System.Diagnostics.Trace**。

## <a name="setting-the-trace-writer"></a>設定追蹤寫入器

若要啟用追蹤，您必須設定 Web API 來使用您**ITraceWriter**實作。 您透過**HttpConfiguration**物件，如下列程式碼所示：

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

只有一個追蹤寫入器可以使用。 根據預設，Web API 設定&quot;無作業&quot;不做任何動作的追蹤。 (&quot;無作業&quot;追蹤存在，因此不需要檢查追蹤寫入器是否追蹤程式碼**null**之前寫入追蹤。)

## <a name="how-web-api-tracing-works"></a>Web API 追蹤的運作

追蹤中的 Web API 所使用的 Web 應用程式開發介面使用*外觀*模式： 當啟用追蹤時，Web 應用程式開發介面包裝要求管線包含執行追蹤呼叫類別的各種組件。

例如，當選取控制站，管線會使用**IHttpControllerSelector**介面。 利用啟用的追蹤，pipleline 插入實作的類別**IHttpControllerSelector**但呼叫傳遞給實際的實作：

![Web API 追蹤會使用外觀樣式。](tracing-in-aspnet-web-api/_static/image8.png)

這種設計的優點包括：

- 如果您不需要新增追蹤寫入器，追蹤元件未具現化，並不造成任何效能影響。
- 如果您取代預設的服務，例如**IHttpControllerSelector**您自己自訂的實作，追蹤受到影響，因為追蹤必須由包裝函式物件完成。

您也可以取代您自己自訂的架構，整個 Web API 追蹤架構，來取代預設**ITraceManager**服務：

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

實作**ITraceManager.Initialize**初始化追蹤系統。 請注意這會取代*整個*追蹤架構，包括所有內建 Web API 的追蹤程式碼。
