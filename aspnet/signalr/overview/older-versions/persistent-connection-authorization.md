---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: "SignalR 持續連線的驗證和授權 (SignalR 1.x) |Microsoft 文件"
author: pfletcher
description: "本主題描述如何強制執行授權持續連線。 如需安全性整合的 SignalR 應用程式，一般資訊..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 4c036ddf1e20e3a3be7b043d90b594292013f6c2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>SignalR 持續連線的驗證和授權 (SignalR 1.x)
====================
由[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)

> 本主題描述如何強制執行授權持續連線。 如需安全性整合的 SignalR 應用程式的一般資訊，請參閱[安全性簡介](index.md)。


## <a name="enforce-authorization"></a>強制執行授權

若要強制執行時使用授權規則[PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx)必須覆寫`AuthorizeRequest`方法。 您無法使用`Authorize`持續連線的屬性。 `AuthorizeRequest`由 SignalR 架構，以確認使用者已獲授權執行要求的動作的每個要求之前呼叫方法。 `AuthorizeRequest`方法不會從用戶端呼叫; 相反地，透過您的應用程式的標準驗證機制來驗證使用者。

下列範例顯示如何限制已驗證的使用者要求。

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

您可以將任何自訂的授權邏輯 AuthorizeRequest 方法;例如，檢查使用者是否屬於特定角色。
