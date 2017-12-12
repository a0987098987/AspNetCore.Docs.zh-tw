---
title: "設定身分識別主索引鍵資料類型"
author: AdrienTorris
description: "本文概述設定用於 ASP.NET Core 識別主索引鍵的所需的資料類型的步驟。"
keywords: "ASP.NET Core，身分識別，主索引鍵"
ms.author: scaddie
manager: wpickett
ms.date: 09/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 5734a9aa86fb2831bd054593ad41c3e3bda4729e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="configure-the-aspnet-core-identity-primary-key-data-type"></a>設定 ASP.NET Core 識別主索引鍵資料類型

ASP.NET Core 身分識別可讓您設定用來表示主索引鍵的資料類型。 識別使用`string`預設的資料型別。 您可以覆寫此行為。

## <a name="customize-the-primary-key-data-type"></a>自訂主索引鍵資料類型

1. 建立的自訂實作[IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1)類別。 它代表要用來建立使用者物件的類型。 在下列範例中，預設值`string`類型取代`Guid`。

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

1. 建立的自訂實作[IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1)類別。 它代表要用來建立角色物件的類型。 在下列範例中，預設值`string`類型取代`Guid`。
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]
    
1. 建立自訂的資料庫內容類別。 它會繼承自 Entity Framework 資料庫內容類別用於身分識別。 `TUser`和`TRole`引數會參考在前一個步驟中，分別建立的自訂使用者和角色類別。 `Guid`資料型別定義的主索引鍵。

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
1. 新增身分識別服務應用程式的啟動類別中時，請註冊自訂的資料庫內容類別。

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    `AddEntityFrameworkStores`方法不會接受`TKey`引數，因為它未在 ASP.NET Core 1.x。 主索引鍵資料類型藉由分析推斷`DbContext`物件。
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    `AddEntityFrameworkStores`方法會接受`TKey`指出主索引鍵的資料型別引數。
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]
    
    ---

## <a name="test-the-changes"></a>測試變更

完成時的組態變更，表示主索引鍵的屬性會反映新的資料類型。 下列範例示範如何存取此 MVC 控制器中的屬性。

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
