---
title: SignalR 的 API 設計考量
author: anurse
description: 了解如何設計相容性的 SignalR 的 Api，在您的應用程式的版本。
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/api-design
ms.openlocfilehash: 3f17bf055b793e8fc91fbcc15f668928ca261f77
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2018
ms.locfileid: "51571548"
---
# <a name="signalr-api-design-considerations"></a>SignalR 的 API 設計考量

藉由[Andrew Stanton-nurse](https://twitter.com/anurse)

本文章提供建置 SignalR 為基礎的 Api 的指引。

## <a name="use-custom-object-parameters-to-ensure-backwards-compatibility"></a>使用自訂的物件參數，確保回溯相容性

加入 （用戶端或伺服器） 上的 SignalR 中樞方法的參數是*重大變更*。 這表示較舊的用戶端/伺服器將會收到錯誤，當使用者試著叫用方法，而不需要適當的參數數目。 不過，將屬性加入至自訂的物件參數是**不**一項重大變更。 這可用來設計相容的 Api，有彈性地在用戶端或伺服器上的變更。

例如，請考慮伺服器端 API，如下所示：

[!code-csharp[ParameterBasedOldVersion](api-design/sample/Samples.cs?name=ParameterBasedOldVersion)]

JavaScript 用戶端會呼叫此方法使用`invoke`，如下所示：

[!code-typescript[CallWithOneParameter](api-design/sample/Samples.ts?name=CallWithOneParameter)]

如果您稍後會將第二個參數新增至伺服器方法，較舊的用戶端不會提供此參數值。 例如: 

[!code-csharp[ParameterBasedNewVersion](api-design/sample/Samples.cs?name=ParameterBasedNewVersion)]

當舊的用戶端嘗試叫用這個方法時，它會取得如下的錯誤：

```
Microsoft.AspNetCore.SignalR.HubException: Failed to invoke 'GetTotalLength' due to an error on the server.
```

在伺服器上，您會看到如下的記錄檔訊息：

```
System.IO.InvalidDataException: Invocation provides 1 argument(s) but target expects 2.
```

舊的用戶端只會傳送一個參數，但較新的伺服器 API 所需的兩個參數。 使用自訂物件做為參數，讓您更大的彈性。 讓我們重新設計要使用自訂物件的原始 API:

[!code-csharp[ObjectBasedOldVersion](api-design/sample/Samples.cs?name=ObjectBasedOldVersion)]

現在，讓用戶端物件呼叫方法：

[!code-typescript[CallWithObject](api-design/sample/Samples.ts?name=CallWithObject)]

而不是新增參數，將屬性新增至`TotalLengthRequest`物件：

[!code-csharp[ObjectBasedNewVersion](api-design/sample/Samples.cs?name=ObjectBasedNewVersion&highlight=4,9-13)]

當舊的用戶端傳送的額外的參數`Param2`屬性會保留`null`。 您可以藉由檢查較舊的用戶端所傳送的訊息來偵測`Param2`針對`null`並套用預設值。 新的用戶端可以傳送兩個參數。

[!code-typescript[CallWithObjectNew](api-design/sample/Samples.ts?name=CallWithObjectNew)]

相同的技巧適用於在用戶端定義的方法。 您可以從伺服器端來傳送自訂的物件：

[!code-csharp[ClientSideObjectBasedOld](api-design/sample/Samples.cs?name=ClientSideObjectBasedOld)]

在您存取用戶端，`Message`屬性，而不是使用參數：

[!code-typescript[OnWithObjectOld](api-design/sample/Samples.ts?name=OnWithObjectOld)]

如果您稍後決定裝載中加入訊息的寄件者，將屬性加入物件：

[!code-csharp[ClientSideObjectBasedNew](api-design/sample/Samples.cs?name=ClientSideObjectBasedNew&highlight=5)]

不會預期較舊的用戶端`Sender`值，因此它們會忽略它。 新的用戶端可接受它藉由更新為新的屬性：

[!code-typescript[OnWithObjectNew](api-design/sample/Samples.ts?name=OnWithObjectNew&highlight=2-5)]

在此情況下，新的用戶端也是不提供舊伺服器的容錯`Sender`值。 因為舊的伺服器將不會提供`Sender`值，用戶端會檢查以查看它是否存在才能存取它。
