---
title: "設定識別主索引鍵資料類型"
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 2afcdf89b2a39d82a4ba72dc780be71ac98ab664
ms.sourcegitcommit: 76d42f09f3e0dd2f2105493eca6b29994aa47706
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/19/2017
---
# <a name="configure-identity-primary-keys-data-type"></a><span data-ttu-id="18447-102">設定識別主索引鍵資料類型</span><span class="sxs-lookup"><span data-stu-id="18447-102">Configure Identity primary keys data type</span></span>

<span data-ttu-id="18447-103">ASP.NET Core 身分識別可讓您輕鬆地設定您要用於主索引鍵的資料類型。</span><span class="sxs-lookup"><span data-stu-id="18447-103">ASP.NET Core Identity allows you to easily configure the data type you want for the primary keys.</span></span> <span data-ttu-id="18447-104">根據預設，識別使用字串資料類型，但是您可以非常快速地覆寫此行為。</span><span class="sxs-lookup"><span data-stu-id="18447-104">By default, Identity uses string data type but you can very quickly override this behavior.</span></span>

## <a name="how-to"></a><span data-ttu-id="18447-105">如何</span><span class="sxs-lookup"><span data-stu-id="18447-105">How to</span></span>

1.  <span data-ttu-id="18447-106">第一個步驟是實作的身分識別模型，並覆寫您想要的資料類型的字串類型。</span><span class="sxs-lookup"><span data-stu-id="18447-106">The first step is to implement the Identity's model, and override the string type with the data type you want.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]
    
2.  <span data-ttu-id="18447-107">實作您的模型與您要用於主索引鍵的資料類型識別的資料庫內容</span><span class="sxs-lookup"><span data-stu-id="18447-107">Implement the database context of Identity with your models and the data type you want for primary keys</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
3.  <span data-ttu-id="18447-108">使用您的模型，當您宣告您的應用程式啟動類別中的識別服務需要主索引鍵的資料類型</span><span class="sxs-lookup"><span data-stu-id="18447-108">Use your models and the data type you want for primary keys when you declare the identity service in your application's startup class</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]
