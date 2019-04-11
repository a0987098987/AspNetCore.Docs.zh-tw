---
title: 管理使用者和 signalr 的群組
author: bradygaster
description: ASP.NET Core SignalR 使用者和群組管理的概觀。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/04/2018
uid: signalr/groups
ms.openlocfilehash: 45f2bb44e03a586b7fc186525fdd3a2645c820d5
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667748"
---
# <a name="manage-users-and-groups-in-signalr"></a>管理使用者和 signalr 的群組

由[brennan Conroy](https://github.com/BrennanConroy)提供

SignalR 可讓訊息傳送至特定使用者相關聯的所有連線以及具名群組的連線。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/groups/sample/) [（如何下載）](xref:index#how-to-download-a-sample)

## <a name="users-in-signalr"></a>SignalR 中的使用者

SignalR 可讓您將訊息傳送至特定使用者相關聯的所有連線。 根據預設，使用 SignalR`ClaimTypes.NameIdentifier`從`ClaimsPrincipal`連接做為使用者識別碼相關聯。 單一使用者可以有多個連線的 SignalR 應用程式。 比方說，使用者可以在他們的桌面，以及他們的手機上連線。 每個裝置都個別 SignalR 連線，但它們是所有具有相同的使用者相關聯。 如果訊息傳送給使用者時，所有與該使用者相關聯的連接就會收到的訊息。 可以存取連接的使用者識別碼`Context.UserIdentifier`中樞內的屬性。

傳遞至的使用者識別碼，將訊息傳送至特定使用者`User`函式在您的中樞方法，如下列範例所示：

> [!NOTE]
> 使用者識別碼會區分大小寫。

[!code-csharp[Configure service](groups/sample/hubs/chathub.cs?range=29-32)]

## <a name="groups-in-signalr"></a>Signalr 的群組

群組是連線名稱相關聯的集合。 訊息可以傳送至群組中的所有連線。 群組是傳送給多個連線或連線，因為群組由應用程式的建議的方式。 連接可以是多個群組的成員。 這使得群組非常適合類似交談應用程式，其中顯示每個房間，為群組。 可以加入或移除透過群組連線`AddToGroupAsync`和`RemoveFromGroupAsync`方法。

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

重新連線時，不會保存群組成員資格。 連接必須重新建立時，重新加入群組。 您不可能來計算群組的成員，因為這項資訊不提供，如果應用程式調整為多部伺服器。

若要保護資源的存取權，使用群組時，使用[驗證和授權](xref:signalr/authn-and-authz)ASP.NET Core 中的功能。 如果您只將使用者新增至群組的認證適用於該群組時，傳送至該群組的訊息將只會移至授權的使用者。 不過，群組不是一項安全性功能。 驗證宣告有群組未這麼做，例如到期及撤銷的功能。 如果已撤銷使用者的權限存取群組，您必須以手動方式偵測，並從群組中移除。

> [!NOTE]
> 群組名稱會區分大小寫。

## <a name="related-resources"></a>相關資源

* [開始使用](xref:tutorials/signalr)
* [中樞](xref:signalr/hubs)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
