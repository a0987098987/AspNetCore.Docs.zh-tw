---
title: 管理使用者和 signalr 的群組
author: rachelappel
description: ASP.NET Core SignalR 使用者和群組管理的概觀。
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 3e5e310c84bc3ed5790d5b67a917bd54162ea163
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402521"
---
# <a name="manage-users-and-groups-in-signalr"></a>管理使用者和 signalr 的群組

藉由[brennan 第瑜吉](https://github.com/BrennanConroy)

SignalR 可讓訊息傳送至特定使用者相關聯的所有連線以及具名群組的連線。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [（如何下載）](xref:tutorials/index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>SignalR 中的使用者

SignalR 可讓您將訊息傳送至特定使用者相關聯的所有連線。 根據預設，使用 SignalR`ClaimTypes.NameIdentifier`從`ClaimsPrincipal`連接做為使用者識別碼相關聯。 單一使用者可以有多個連線的 SignalR 應用程式。 比方說，使用者可以在他們的桌面，以及他們的手機上連線。 每個裝置都個別 SignalR 連線，但它們是所有具有相同的使用者相關聯。 如果訊息傳送給使用者時，所有與該使用者相關聯的連接就會收到的訊息。 可以存取連接的使用者識別碼`Context.UserIdentifier`中樞內的屬性。

傳遞至的使用者識別碼，將訊息傳送至特定使用者`User`函式在您的中樞方法，如下列範例所示：

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
> 註冊您的自訂 SignalR 服務之前，必須呼叫 AddSignalR。

## <a name="groups-in-signalr"></a>Signalr 的群組

群組是連線名稱相關聯的集合。 訊息可以傳送至群組中的所有連線。 群組是傳送給多個連線或連線，因為群組由應用程式的建議的方式。 連接可以是多個群組的成員。 這使得群組非常適合類似交談應用程式，其中顯示每個房間，為群組。 可以加入或移除透過群組連線`AddToGroupAsync`和`RemoveFromGroupAsync`方法。

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

重新連線時，不會保存群組成員資格。 連接必須重新建立時，重新加入群組。 您不可能來計算群組的成員，因為這項資訊不提供，如果應用程式調整為多部伺服器。

> [!NOTE]
> 群組名稱會區分大小寫。

## <a name="related-resources"></a>相關資源

* [開始使用](xref:tutorials/signalr)
* [中樞](xref:signalr/hubs)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
