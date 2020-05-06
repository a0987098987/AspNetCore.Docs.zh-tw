---
title: ASP.NET Core Web API 中的自訂格式器
author: rick-anderson
description: 了解如何建立和使用 ASP.NET Core 中的 Web API 自訂格式器。
ms.author: riande
ms.date: 02/08/2017
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: web-api/advanced/custom-formatters
ms.openlocfilehash: 0836fc288a015adb9a6223c5a2b681b1b03bded4
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82777315"
---
# <a name="custom-formatters-in-aspnet-core-web-api"></a>ASP.NET Core Web API 中的自訂格式器

作者：[Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core MVC 支援使用輸入和輸出格式器在 Web Api 中進行資料交換。 [模型](xref:mvc/models/model-binding)系結會使用輸入格式器。 輸出格式器是用來[格式化回應](xref:web-api/advanced/formatting)。

架構會為 JSON 和 XML 提供內建的輸入和輸出格式器。 它會為純文字提供內建的輸出格式器，但不會提供純文字的輸入格式器。

本文說明如何藉由建立自訂的格式器來新增對其他格式的支援。 如需純文字的自訂輸入格式器範例，請參閱 GitHub 上的[TextPlainInputFormatter](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.Formatters/TextPlainInputFormatter.cs) 。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample)（[如何下載](xref:index#how-to-download-a-sample)）

## <a name="when-to-use-custom-formatters"></a>自訂格式器的使用時機

當您想要讓[內容協調](xref:web-api/advanced/formatting#content-negotiation)流程支援內建格式器不支援的內容類型時，請使用自訂格式器。

例如，您有些 Web API 用戶端可以處理 [Protobuf](https://github.com/google/protobuf) 格式，因此您希望搭配使用 Protobuf 與這些用戶端，以便更有效率。 或者，您可能會想要 Web API 以 [vCard](https://wikipedia.org/wiki/VCard) 格式 (其為交換連絡人資料的常用格式) 來傳送連絡人名稱和地址。 本文提供的範例應用程式會實作簡單的 vCard 格式器。

## <a name="overview-of-how-to-use-a-custom-formatter"></a>如何使用自訂格式器的概觀

自訂格式器的建立與使用步驟如下：

* 如果您想要將資料序列化以傳送到用戶端，請建立輸出格式器類別。
* 如果您想要將從用戶端接收的資料還原序列化，請建立輸入格式器類別。
* 將您的格式器執行個體新增至 [MvcOptions](/dotnet/api/microsoft.aspnetcore.mvc.mvcoptions) 中的 `InputFormatters` 和 `OutputFormatters` 集合。

下列各節將提供每個步驟的指引與程式碼範例。

## <a name="how-to-create-a-custom-formatter-class"></a>如何建立自訂格式器類別

若要建立格式器：

* 請從適當的基底類別衍生類別。
* 在建構函式中指定有效的媒體類型和編碼方式。
* 覆寫 `CanReadType`/`CanWriteType` 方法
* 覆寫 `ReadRequestBodyAsync`/`WriteResponseBodyAsync` 方法
  
### <a name="derive-from-the-appropriate-base-class"></a>從適當的基底類別衍生

如需文字媒體類型 (例如 vCard)，請從 [TextInputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) 或 [TextOutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) 基底類別來衍生。

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

若需要輸入格式器範例，請參考[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample)。

如需二進位類型，請從 [InputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.inputformatter) 或 [OutputFormatter](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformatter) 基底類別來衍生。

### <a name="specify-valid-media-types-and-encodings"></a>指定有效的媒體類型和編碼方式

在建構函式中，您可以新增 `SupportedMediaTypes` 和 `SupportedEncodings` 集合，以指定有效的媒體類型和編碼方式。

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

若需要輸入格式器範例，請參考[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample)。

> [!NOTE]
> 您無法在格式器類別中執行建構函式的相依性插入。 舉例來說，您無法藉由將記錄器參數新增至建構函式，而取得記錄器。 若要存取服務，您必須使用可傳入方法的內容物件。 [下列](#read-write)程式碼範例會示範這項操作。

### <a name="override-canreadtypecanwritetype"></a>覆寫 CanReadType/CanWriteType

您可以覆寫 `CanReadType` 或 `CanWriteType` 方法，以指定要序列化或還原序列化的類型。 例如，您可能只能透過 `Contact` 類型建立 vCard 文字，反之亦然。

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

若需要輸入格式器範例，請參考[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample)。

#### <a name="the-canwriteresult-method"></a>CanWriteResult 方法

在某些情況下，您必須覆寫 `CanWriteResult` 而不是 `CanWriteType`。 如果符合下列所有條件，請使用 `CanWriteResult`：

* 您的動作方法會傳回模型類別。
* 在執行階段期間，可能會傳回衍生的類別。
* 您必須知道在執行階段期間，動作傳回的衍生類別是哪一個。

例如，假設您的動作方法簽章傳回 `Person` 類型，但它可能會傳回衍生自 `Person` 的 `Student` 或 `Instructor` 類型。 如果您希望格式器只處理 `Student` 物件，請檢查您提供給 `CanWriteResult` 方法之內容物件中的[物件](/dotnet/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext.object#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object)類型。 請注意，當動作方法傳回 `IActionResult` 時，您不需要使用 `CanWriteResult`；在此情況下，`CanWriteType` 方法會接收執行階段類型。

<a id="read-write"></a>

### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a>覆寫 ReadRequestBodyAsync/WriteResponseBodyAsync

您可以在 `ReadRequestBodyAsync` 或 `WriteResponseBodyAsync` 中進行還原序列化或序列化的實際工作。 下列範例中，醒目標示的程式碼行示範如何透過相依性插入容器以取得服務 (您無法透過建構函式參數來取得)。

[!code-csharp[](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

若需要輸入格式器範例，請參考[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample)。

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a>如何設定 MVC 以使用自訂格式器

若要使用自訂格式器，請將格式器類別的執行個體新增至 `InputFormatters` 或 `OutputFormatters` 集合。

[!code-csharp[](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

系統會依據您插入格式器的順序進行評估。 第一個會優先使用。

## <a name="next-steps"></a>後續步驟

* [此文件的範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/custom-formatters/sample)，其會實作簡單的 vCard 輸入和輸出格式器。 應用程式會讀取並寫入 vCard，如下範例所示：

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
no-loc: [Blazor, "Identity", "Let's Encrypt", Razor, SignalR]
uid:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

若要查看 vCard 的輸出，請執行應用程式，並使用 Accept 標頭 "text/vcard" 將 Get 要求傳送給 `http://localhost:63313/api/contacts/` (從 Visual Studio 執行時) 或 `http://localhost:5000/api/contacts/` (從命令列執行時)。

若要將 vCard 新增至連絡人的記憶體內部集合，請使用 Content-Type 標頭 "text/vcard" 與主體中的 vCard 文字 (如上述範例的格式)，將 Post 要求傳送至相同的 URL。
