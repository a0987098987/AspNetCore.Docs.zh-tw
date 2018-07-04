---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: SignalR 持續連線的驗證和授權 (SignalR 1.x) |Microsoft Docs
author: pfletcher
description: 本主題描述如何強制執行授權的持續連線。 如需將安全性整合至 SignalR 應用程式的一般資訊...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/21/2013
ms.topic: article
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 471bce03a37a401c2afe3735afef6f838555cc26
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365106"
---
<a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>SignalR 持續連線的驗證和授權 (SignalR 1.x)
====================
藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)

> 本主題描述如何強制執行授權的持續連線。 如需將安全性整合至 SignalR 應用程式的一般資訊，請參閱[安全性簡介](index.md)。


## <a name="enforce-authorization"></a>強制執行授權

若要強制執行時使用授權規則[PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx)您必須覆寫`AuthorizeRequest`方法。 您無法使用`Authorize`具有持續性連線屬性。 `AuthorizeRequest` SignalR 架構，以確認使用者已獲授權執行要求的動作每個要求之前所呼叫方法。 `AuthorizeRequest`方法不會從用戶端呼叫; 相反地，透過您的應用程式的標準驗證機制來驗證使用者。

下列範例顯示如何限制已驗證的使用者的要求。

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

您可以新增任何自訂的授權邏輯中的 AuthorizeRequest 方法;例如，檢查使用者是否屬於特定的角色。
