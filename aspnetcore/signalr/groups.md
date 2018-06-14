---
title: 管理 SignalR 中使用者和群組
author: rachelappel
description: ASP.NET Core SignalR 使用者和群組管理的概觀。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/groups
ms.openlocfilehash: 2a2f129863cf7d5cdfa3c0e5b2d901af52144671
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/12/2018
ms.locfileid: "35358426"
---
# <a name="manage-users-and-groups-in-signalr"></a>管理 SignalR 中使用者和群組

由[Brennan 瑜吉](https://github.com/BrennanConroy)

SignalR 允許訊息傳送至所有與特定使用者相關聯的連接以及名為群組的連線。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [（如何下載）](xref:tutorials/index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>SignalR 的使用者

SignalR 可讓您將訊息傳送至所有與特定使用者相關聯的連接。 根據預設使用 SignalR`ClaimTypes.NameIdentifier`從`ClaimsPrincipal`的使用者識別碼與連接相關聯。 單一使用者可以有多個連線的 SignalR 應用程式。 例如，使用者無法連接其桌面，以及他們的電話號碼。 每個裝置都有個別的 SignalR 連線，但所有具有相同的使用者相關聯。 如果訊息傳送給使用者時，所有與該使用者相關聯的連接將會收到訊息。

傳送訊息至特定的使用者中，藉由傳遞使用者識別項`User`中樞方法的函式，如下列範例所示：

> [!NOTE]
> 使用者識別碼會區分大小寫。

```csharp
public Task SendPrivateMessage(string user, string message)
{
    return Clients.User(user).SendAsync("ReceiveMessage", message);
}
```

可以藉由建立自訂的使用者識別碼`IUserIdProvider`，並註冊在`ConfigureServices`。

[!code-csharp[UserIdProvider](groups/sample/customuseridprovider.cs?range=4-10)]

[!code-csharp[Configure service](groups/sample/startup.cs?range=21-22,39-42)]

> [!NOTE]
> 在註冊自訂的 SignalR 服務之前，必須先呼叫 AddSignalR。

## <a name="groups-in-signalr"></a>SignalR 中的群組

群組是連線名稱相關聯的集合。 訊息可以傳送到群組中的所有連線。 群組是傳送至多個連線或連線，因為群組由應用程式管理的建議的方式。 多個群組的成員時，可以在連線。 這使得群組非常適合類似交談應用程式，其中每個房間可以表示為群組。 可以加入或移除群組透過連線`AddToGroupAsync`和`RemoveFromGroupAsync`方法。

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

重新連線時，並不會保留群組成員資格。 連線需要重新建立時，重新加入群組。 您不可能來計算群組的成員，因為這項資訊不是使用應用程式會調整為多部伺服器。

> [!NOTE]
> 群組名稱會區分大小寫。

## <a name="related-resources"></a>相關資源

* [開始使用](xref:signalr/get-started)
* [中樞](xref:signalr/hubs)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
