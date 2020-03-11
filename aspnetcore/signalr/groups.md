---
title: 管理 SignalR 中的使用者和群組
author: bradygaster
description: ASP.NET Core SignalR 使用者和群組管理的總覽。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/groups
ms.openlocfilehash: 7e8c85dcbc46daa68988374f499f19a9d2cead47
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665862"
---
# <a name="manage-users-and-groups-in-opno-locsignalr"></a>管理 SignalR 中的使用者和群組

依[Brennan Conroy](https://github.com/BrennanConroy)

SignalR 允許訊息傳送至與特定使用者相關聯的所有連接，以及命名的連線群組。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/groups/sample/) [（如何下載）](xref:index#how-to-download-a-sample)

## <a name="users-in-opno-locsignalr"></a>SignalR 中的使用者

SignalR 可讓您將訊息傳送至與特定使用者相關聯的所有連接。 根據預設，SignalR 會使用與連接相關聯的 `ClaimsPrincipal` 中的 `ClaimTypes.NameIdentifier` 作為使用者識別碼。 單一使用者可以有多個 SignalR 應用程式的連接。 例如，使用者可以連線到桌上型電腦以及電話。 每個裝置都有個別的 SignalR 連線，但全都與相同的使用者相關聯。 如果訊息傳送給使用者，與該使用者相關聯的所有連接都會收到訊息。 您的中樞中的 `Context.UserIdentifier` 屬性可以存取連接的使用者識別碼。

將使用者識別碼傳遞至中樞方法中的 `User` 函式，以將訊息傳送給特定使用者，如下列範例所示：

> [!NOTE]
> 使用者識別碼會區分大小寫。

[!code-csharp[Configure service](groups/sample/hubs/chathub.cs?range=29-32)]

## <a name="groups-in-opno-locsignalr"></a>SignalR 中的群組

「群組」（group）是與名稱相關聯之連接的集合。 訊息可以傳送至群組中的所有連接。 群組是傳送至連線或多個連接的建議方式，因為這些群組是由應用程式所管理。 連接可以是多個群組的成員。 這讓群組適用于類似聊天應用程式的專案，其中每個房間可以表示為一個群組。 您可以透過 `AddToGroupAsync` 和 `RemoveFromGroupAsync` 方法，在群組中新增或移除連接。

[!code-csharp[Hub methods](groups/sample/hubs/chathub.cs?range=15-27)]

當連線重新連接時，不會保留群組成員資格。 重新建立群組時，連接需要重新加入該群組。 無法計算群組的成員，因為如果應用程式調整為多部伺服器，則無法使用此資訊。

若要在使用群組時保護對資源的存取，請使用 ASP.NET Core 中的[驗證和授權](xref:signalr/authn-and-authz)功能。 如果您只在認證對該群組有效時，將使用者新增至群組，則傳送至該群組的訊息只會前往已授權的使用者。 不過，群組並不是安全性功能。 驗證宣告具有群組不提供的功能，例如到期和撤銷。 如果已撤銷使用者存取群組的許可權，您就必須手動偵測並將其從群組中移除。

> [!NOTE]
> 組名會區分大小寫。

## <a name="related-resources"></a>相關資源

* [開始使用](xref:tutorials/signalr)
* [中樞](xref:signalr/hubs)
* [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
