---
uid: signalr/overview/security/persistent-connection-authorization
title: "SignalR 持續連線的驗證和授權 |Microsoft 文件"
author: pfletcher
description: "本主題描述如何強制執行授權持續連線。 如需安全性整合的 SignalR 應用程式，一般資訊..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 9c6fff86ae6b1b65e6ba9922b6b8448643ef1f15
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="authentication-and-authorization-for-signalr-persistent-connections"></a>SignalR 持續連線的驗證和授權
====================
由[Patrick Fletcher](https://github.com/pfletcher)， [Tom FitzMacken](https://github.com/tfitzmac)

> 本主題描述如何強制執行授權持續連線。 如需安全性整合的 SignalR 應用程式的一般資訊，請參閱[安全性簡介](introduction-to-security.md)。 
> 
> ## <a name="software-versions-used-in-this-topic"></a>本主題中使用的軟體版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR 第 2 版
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>本主題的先前版本
> 
> 如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。
> 
> ## <a name="questions-and-comments"></a>問題和註解
> 
> 請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。 如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。


## <a name="enforce-authorization"></a>強制執行授權

若要強制執行時使用授權規則[PersistentConnection](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx)必須覆寫`AuthorizeRequest`方法。 您無法使用`Authorize`持續連線的屬性。 `AuthorizeRequest`由 SignalR 架構，以確認使用者已獲授權執行要求的動作的每個要求之前呼叫方法。 `AuthorizeRequest`方法不會從用戶端呼叫; 相反地，透過您的應用程式的標準驗證機制來驗證使用者。

下列範例顯示如何限制已驗證的使用者要求。

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

您可以將任何自訂的授權邏輯 AuthorizeRequest 方法;例如，檢查使用者是否屬於特定角色。
