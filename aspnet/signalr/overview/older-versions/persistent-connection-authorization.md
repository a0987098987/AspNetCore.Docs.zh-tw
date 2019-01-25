---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: SignalR 持續連線的驗證和授權 (SignalR 1.x) |Microsoft Docs
author: bradygaster
description: 本主題描述如何強制執行授權的持續連線。 如需將安全性整合至 SignalR 應用程式的一般資訊...
ms.author: bradyg
ms.date: 10/21/2013
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: c4a2c9b4fa05e43ade98fb521c59f8645b43ffea
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837347"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>SignalR 持續連線的驗證和授權 (SignalR 1.x)
====================
藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> 本主題描述如何強制執行授權的持續連線。 如需將安全性整合至 SignalR 應用程式的一般資訊，請參閱[安全性簡介](index.md)。


## <a name="enforce-authorization"></a>強制執行授權

若要強制執行時使用授權規則[PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx)您必須覆寫`AuthorizeRequest`方法。 您無法使用`Authorize`具有持續性連線屬性。 `AuthorizeRequest` SignalR 架構，以確認使用者已獲授權執行要求的動作每個要求之前所呼叫方法。 `AuthorizeRequest`方法不會從用戶端呼叫; 相反地，透過您的應用程式的標準驗證機制來驗證使用者。

下列範例顯示如何限制已驗證的使用者的要求。

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

您可以新增任何自訂的授權邏輯中的 AuthorizeRequest 方法;例如，檢查使用者是否屬於特定的角色。
