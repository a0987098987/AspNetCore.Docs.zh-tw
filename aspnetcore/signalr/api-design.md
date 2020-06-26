---
title: SignalRAPI 設計考慮
author: anurse
description: 瞭解如何設計 SignalR 應用程式版本之間相容性的 api。
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/api-design
ms.openlocfilehash: 9ad8d30da552d3d3084534b8c7ca57386ad111ac
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85407794"
---
# <a name="signalr-api-design-considerations"></a>SignalRAPI 設計考慮

[Andrew Stanton-護士](https://twitter.com/anurse)

本文提供以建立為 SignalR 基礎的 api 指引。

## <a name="use-custom-object-parameters-to-ensure-backwards-compatibility"></a>使用自訂物件參數來確保回溯相容性

將參數新增至 SignalR 中樞方法（在用戶端或伺服器上）是一項*重大變更*。 這表示較舊的用戶端/伺服器在嘗試叫用方法時，如果沒有適當數目的參數，就會收到錯誤。 不過，將屬性新增至自訂物件參數並**不**是一種中斷變更。 這可以用來設計可在用戶端或伺服器上復原變更的相容 Api。

例如，假設有一個伺服器端 API，如下所示：

[!code-csharp[ParameterBasedOldVersion](api-design/sample/Samples.cs?name=ParameterBasedOldVersion)]

JavaScript 用戶端會使用呼叫這個方法 `invoke` ，如下所示：

[!code-typescript[CallWithOneParameter](api-design/sample/Samples.ts?name=CallWithOneParameter)]

如果您稍後將第二個參數新增至伺服器方法，舊版用戶端就不會提供此參數值。 例如：

[!code-csharp[ParameterBasedNewVersion](api-design/sample/Samples.cs?name=ParameterBasedNewVersion)]

當舊的用戶端嘗試叫用此方法時，會收到類似下面的錯誤：

```
Microsoft.AspNetCore.SignalR.HubException: Failed to invoke 'GetTotalLength' due to an error on the server.
```

在伺服器上，您會看到類似下面的記錄訊息：

```
System.IO.InvalidDataException: Invocation provides 1 argument(s) but target expects 2.
```

舊的用戶端只會傳送一個參數，但較新的伺服器 API 需要兩個參數。 使用自訂物件做為參數可提供更大的彈性。 讓我們重新設計原始 API，以使用自訂物件：

[!code-csharp[ObjectBasedOldVersion](api-design/sample/Samples.cs?name=ObjectBasedOldVersion)]

現在，用戶端會使用物件來呼叫方法：

[!code-typescript[CallWithObject](api-design/sample/Samples.ts?name=CallWithObject)]

將屬性新增至物件，而不是新增參數 `TotalLengthRequest` ：

[!code-csharp[ObjectBasedNewVersion](api-design/sample/Samples.cs?name=ObjectBasedNewVersion&highlight=4,9-13)]

當舊的用戶端傳送單一參數時， `Param2` 將會留下額外的屬性 `null` 。 您可以藉由檢查 `Param2` 並套用預設值，偵測舊版用戶端所傳送的訊息 `null` 。 新的用戶端可以傳送這兩個參數。

[!code-typescript[CallWithObjectNew](api-design/sample/Samples.ts?name=CallWithObjectNew)]

相同的技術適用于在用戶端上定義的方法。 您可以從伺服器端傳送自訂物件：

[!code-csharp[ClientSideObjectBasedOld](api-design/sample/Samples.cs?name=ClientSideObjectBasedOld)]

在用戶端上，您可以存取 `Message` 屬性，而不是使用參數：

[!code-typescript[OnWithObjectOld](api-design/sample/Samples.ts?name=OnWithObjectOld)]

如果您稍後決定要將訊息的傳送者新增至裝載，請將屬性新增至物件：

[!code-csharp[ClientSideObjectBasedNew](api-design/sample/Samples.cs?name=ClientSideObjectBasedNew&highlight=5)]

較舊的用戶端不會預期 `Sender` 值，因此會予以忽略。 新的用戶端可以藉由更新來接受它，以讀取新的屬性：

[!code-typescript[OnWithObjectNew](api-design/sample/Samples.ts?name=OnWithObjectNew&highlight=2-5)]

在此情況下，新的用戶端也會容忍不提供此值的舊伺服器 `Sender` 。 由於舊伺服器不會提供 `Sender` 值，因此用戶端會先檢查它是否存在，然後再進行存取。
