---
title: "ASP.NET Core MVC web 應用程式開發介面中的自訂格式器"
author: tdykstra
description: "了解如何建立和使用 ASP.NET Core 中的 web Api 的自訂格式器。"
keywords: "ASP.NET Core web 應用程式開發介面，自訂格式器"
ms.author: tdykstra
manager: wpickett
ms.date: 02/08/2017
ms.topic: article
ms.assetid: 1fb6fdc2-e199-4469-9012-b909d1913422
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/custom-formatters
ms.openlocfilehash: af3b2174c73583832868d2062e6c7ab4689a1229
ms.sourcegitcommit: 9d3f27a1ee5b7014fb40e4f2ec9b2a9cd744751c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/31/2017
---
# <a name="custom-formatters-in-aspnet-core-mvc-web-apis"></a>ASP.NET Core MVC web 應用程式開發介面中的自訂格式器

由[Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core MVC web 應用程式開發介面中有內建支援的資料交換，使用 JSON、 XML 或純文字格式。 本文將說明如何藉由建立自訂的格式器加入其他格式的支援。

[檢視或從 GitHub 下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/Sample)。

## <a name="when-to-use-custom-formatters"></a>使用自訂的格式器的時機

當您想使用自訂的格式器[內容交涉](xref:mvc/models/formatting)支援內容類型不支援的內建的格式器 （JSON、 XML 和純文字） 的程序。

例如，如果您的 web API 的用戶端的部分可以處理[Protobuf](https://github.com/google/protobuf)格式，您可以使用 Protobuf 這些用戶端，因為它是更有效率。  您可能會想要傳送連絡人的姓名和地址中的 web API 或者[vCard](https://en.wikipedia.org/wiki/VCard) ，常用的格式來交換連絡人的資料格式。 本文提供的範例應用程式會實作簡單的 vCard 格式器。

## <a name="overview-of-how-to-use-a-custom-formatter"></a>如何使用自訂的格式器的概觀

建立及使用自訂格式子，步驟如下：

* 如果您想要將資料序列化至傳送到用戶端，請建立輸出格式子類別。
* 如果您想要從用戶端收到的資料還原序列化，請建立輸入格式子類別。 
* 您要的格式器的執行個體加入`InputFormatters`和`OutputFormatters`中的集合[MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions)。

下列章節將提供指導方針與範例程式碼的每個步驟。

## <a name="how-to-create-a-custom-formatter-class"></a>如何建立自訂格式子類別

若要建立格式器：

* 衍生自適當的基底類別的類別。
* 建構函式中指定有效的媒體類型和編碼。
* 覆寫`CanReadType` / `CanWriteType`方法
* 覆寫`ReadRequestBodyAsync` / `WriteResponseBodyAsync`方法
  
### <a name="derive-from-the-appropriate-base-class"></a>衍生自適當的基底類別

文字媒體類型 (例如，vCard)，衍生自[TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter)或[TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter)基底類別。

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

二進位型別，衍生自[InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter)或[OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter)基底類別。

### <a name="specify-valid-media-types-and-encodings"></a>指定有效的媒體類型和編碼

在建構函式，方法是加入指定有效的媒體類型和編碼`SupportedMediaTypes`和`SupportedEncodings`集合。

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]  
> 您無法在格式子類別的建構函式相依性插入。 例如，您無法取得記錄器所加入的記錄器參數的建構函式。 若要存取服務，您必須使用取得傳遞至方法的內容物件。 程式碼範例[下方](#read-write)示範如何執行這項操作。

### <a name="override-canreadtypecanwritetype"></a>覆寫 CanReadType/CanWriteType 

指定的類型，您可以還原序列化，或藉由覆寫序列化從`CanReadType`或`CanWriteType`方法。 例如，您可能只能夠建立從 vCard 文字`Contact`型別，反之亦然。

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a>CanWriteResult 方法

在某些情況下，您必須覆寫`CanWriteResult`而不是`CanWriteType`。 使用`CanWriteResult`如果下列條件成立：

  * 動作方法傳回的模型類別。
  * 有可能會在執行階段傳回的衍生的類別。
  * 您需要知道在執行階段，衍生類別的動作所傳回。  

例如，假設您的動作方法簽章傳回`Person`類型，但它可能會傳回`Student`或`Instructor`衍生自型別`Person`。 如果您想要您的格式子，只處理`Student`物件，請檢查類型[物件](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object)提供給內容物件中`CanWriteResult`方法。 請注意，不需要使用`CanWriteResult`動作方法傳回時`IActionResult`; 在此情況下，`CanWriteType`方法會接收的執行階段類型。

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a>覆寫 ReadRequestBodyAsync/WriteResponseBodyAsync 

進行還原序列化，或序列化中的實際工作`ReadRequestBodyAsync`或`WriteResponseBodyAsync`。  反白顯示的行，在下列範例示範如何取得服務從相依性插入容器 （您無法從取得建構函式參數）。

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a>如何設定要使用自訂的格式器的 MVC
 
若要使用自訂格式子，新增到格式器類別的執行個體`InputFormatters`或`OutputFormatters`集合。

[!code-csharp[Main](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

格式器會將其插入的順序進行評估。 第一個會優先使用。 

## <a name="next-steps"></a>後續步驟

請參閱[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/Sample)，它會實作簡單的 vCard 輸入和輸出格式器。  應用程式讀取並寫入 vCards 看起來像下列的範例：

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

若要查看 vCard 輸出、 執行應用程式和傳送 Get 要求的接受標頭 」 文字/vcard" `http://localhost:63313/api/contacts/` （執行時從 Visual Studio） 或`http://localhost:5000/api/contacts/`（當從命令列執行）。

若要加入 vCard 連絡人的記憶體中集合，Post 要求傳送至相同的 URL，Content-type 標頭 」 文字/vcard"與 vCard 在主體中，如上述範例格式化的文字。
