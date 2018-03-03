---
title: "設定身分識別主索引鍵資料類型"
author: AdrienTorris
description: "本文概述設定用於 ASP.NET Core 識別主索引鍵的所需的資料類型的步驟。"
manager: wpickett
ms.author: scaddie
ms.date: 09/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: ff1c3aff3ea833081a25ea5fc4f2c2b65823f536
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/02/2018
---
# <a name="configure-the-aspnet-core-identity-primary-key-data-type"></a><span data-ttu-id="6fa7f-103">設定 ASP.NET Core 識別主索引鍵資料類型</span><span class="sxs-lookup"><span data-stu-id="6fa7f-103">Configure the ASP.NET Core Identity primary key data type</span></span>

<span data-ttu-id="6fa7f-104">ASP.NET Core 身分識別可讓您設定用來表示主索引鍵的資料類型。</span><span class="sxs-lookup"><span data-stu-id="6fa7f-104">ASP.NET Core Identity allows you to configure the data type used to represent a primary key.</span></span> <span data-ttu-id="6fa7f-105">識別使用`string`預設的資料型別。</span><span class="sxs-lookup"><span data-stu-id="6fa7f-105">Identity uses the `string` data type by default.</span></span> <span data-ttu-id="6fa7f-106">您可以覆寫此行為。</span><span class="sxs-lookup"><span data-stu-id="6fa7f-106">You can override this behavior.</span></span>

## <a name="customize-the-primary-key-data-type"></a><span data-ttu-id="6fa7f-107">自訂主索引鍵資料類型</span><span class="sxs-lookup"><span data-stu-id="6fa7f-107">Customize the primary key data type</span></span>

1. <span data-ttu-id="6fa7f-108">建立的自訂實作[IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1)類別。</span><span class="sxs-lookup"><span data-stu-id="6fa7f-108">Create a custom implementation of the [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) class.</span></span> <span data-ttu-id="6fa7f-109">它代表要用來建立使用者物件的類型。</span><span class="sxs-lookup"><span data-stu-id="6fa7f-109">It represents the type to be used for creating user objects.</span></span> <span data-ttu-id="6fa7f-110">在下列範例中，預設值`string`類型取代`Guid`。</span><span class="sxs-lookup"><span data-stu-id="6fa7f-110">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

1. <span data-ttu-id="6fa7f-111">建立的自訂實作[IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1)類別。</span><span class="sxs-lookup"><span data-stu-id="6fa7f-111">Create a custom implementation of the [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) class.</span></span> <span data-ttu-id="6fa7f-112">它代表要用來建立角色物件的類型。</span><span class="sxs-lookup"><span data-stu-id="6fa7f-112">It represents the type to be used for creating role objects.</span></span> <span data-ttu-id="6fa7f-113">在下列範例中，預設值`string`類型取代`Guid`。</span><span class="sxs-lookup"><span data-stu-id="6fa7f-113">In the following example, the default `string` type is replaced with `Guid`.</span></span>
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]
    
1. <span data-ttu-id="6fa7f-114">建立自訂的資料庫內容類別。</span><span class="sxs-lookup"><span data-stu-id="6fa7f-114">Create a custom database context class.</span></span> <span data-ttu-id="6fa7f-115">它會繼承自 Entity Framework 資料庫內容類別用於身分識別。</span><span class="sxs-lookup"><span data-stu-id="6fa7f-115">It inherits from the Entity Framework database context class used for Identity.</span></span> <span data-ttu-id="6fa7f-116">`TUser`和`TRole`引數會參考在前一個步驟中，分別建立的自訂使用者和角色類別。</span><span class="sxs-lookup"><span data-stu-id="6fa7f-116">The `TUser` and `TRole` arguments reference the custom user and role classes created in the previous step, respectively.</span></span> <span data-ttu-id="6fa7f-117">`Guid`資料型別定義的主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="6fa7f-117">The `Guid` data type is defined for the primary key.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
1. <span data-ttu-id="6fa7f-118">新增身分識別服務應用程式的啟動類別中時，請註冊自訂的資料庫內容類別。</span><span class="sxs-lookup"><span data-stu-id="6fa7f-118">Register the custom database context class when adding the Identity service in the app's startup class.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6fa7f-119">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6fa7f-119">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    <span data-ttu-id="6fa7f-120">`AddEntityFrameworkStores`方法不會接受`TKey`引數，因為它未在 ASP.NET Core 1.x。</span><span class="sxs-lookup"><span data-stu-id="6fa7f-120">The `AddEntityFrameworkStores` method doesn't accept a `TKey` argument as it did in ASP.NET Core 1.x.</span></span> <span data-ttu-id="6fa7f-121">主索引鍵資料類型藉由分析推斷`DbContext`物件。</span><span class="sxs-lookup"><span data-stu-id="6fa7f-121">The primary key's data type is inferred by analyzing the `DbContext` object.</span></span>
    
    [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6fa7f-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6fa7f-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    <span data-ttu-id="6fa7f-123">`AddEntityFrameworkStores`方法會接受`TKey`指出主索引鍵的資料型別引數。</span><span class="sxs-lookup"><span data-stu-id="6fa7f-123">The `AddEntityFrameworkStores` method accepts a `TKey` argument indicating the primary key's data type.</span></span>
    
    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]
    
    ---

## <a name="test-the-changes"></a><span data-ttu-id="6fa7f-124">測試變更</span><span class="sxs-lookup"><span data-stu-id="6fa7f-124">Test the changes</span></span>

<span data-ttu-id="6fa7f-125">完成時的組態變更，表示主索引鍵的屬性會反映新的資料類型。</span><span class="sxs-lookup"><span data-stu-id="6fa7f-125">Upon completion of the configuration changes, the property representing the primary key reflects the new data type.</span></span> <span data-ttu-id="6fa7f-126">下列範例示範如何存取此 MVC 控制器中的屬性。</span><span class="sxs-lookup"><span data-stu-id="6fa7f-126">The following example demonstrates accessing the property in an MVC controller.</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
