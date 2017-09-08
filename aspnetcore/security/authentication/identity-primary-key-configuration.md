---
title: "設定識別主索引鍵資料類型"
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: e6661708d003aa50204e7f79d3070442a3440391
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity-primary-keys-data-type"></a><span data-ttu-id="67247-102">設定識別主索引鍵資料類型</span><span class="sxs-lookup"><span data-stu-id="67247-102">Configure Identity primary keys data type</span></span>

<span data-ttu-id="67247-103">ASP.NET Core 身分識別可讓您輕鬆地設定您要用於主索引鍵的資料類型。</span><span class="sxs-lookup"><span data-stu-id="67247-103">ASP.NET Core Identity allows you to easily configure the data type you want for the primary keys.</span></span> <span data-ttu-id="67247-104">根據預設，識別使用字串資料類型，但是您可以非常快速地覆寫此行為。</span><span class="sxs-lookup"><span data-stu-id="67247-104">By default, Identity uses string data type but you can very quickly override this behavior.</span></span>

## <a name="how-to"></a><span data-ttu-id="67247-105">如何</span><span class="sxs-lookup"><span data-stu-id="67247-105">How to</span></span>

1.  <span data-ttu-id="67247-106">第一個步驟是實作的身分識別模型，並覆寫您想要的資料類型的字串類型。</span><span class="sxs-lookup"><span data-stu-id="67247-106">The first step is to implement the Identity's model, and override the string type with the data type you want.</span></span>

    <span data-ttu-id="67247-107">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]</span><span class="sxs-lookup"><span data-stu-id="67247-107">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]</span></span>

    <span data-ttu-id="67247-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]</span><span class="sxs-lookup"><span data-stu-id="67247-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]</span></span>
    
2.  <span data-ttu-id="67247-109">實作您的模型與您要用於主索引鍵的資料類型識別的資料庫內容</span><span class="sxs-lookup"><span data-stu-id="67247-109">Implement the database context of Identity with your models and the data type you want for primary keys</span></span>

    <span data-ttu-id="67247-110">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]</span><span class="sxs-lookup"><span data-stu-id="67247-110">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]</span></span>
    
3.  <span data-ttu-id="67247-111">使用您的模型，當您宣告您的應用程式啟動類別中的識別服務需要主索引鍵的資料類型</span><span class="sxs-lookup"><span data-stu-id="67247-111">Use your models and the data type you want for primary keys when you declare the identity service in your application's startup class</span></span>

    <span data-ttu-id="67247-112">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]</span><span class="sxs-lookup"><span data-stu-id="67247-112">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]</span></span>
