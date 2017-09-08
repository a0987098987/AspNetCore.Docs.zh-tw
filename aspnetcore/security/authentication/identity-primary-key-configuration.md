---
title: "設定識別主索引鍵資料類型"
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: e6661708d003aa50204e7f79d3070442a3440391
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity-primary-keys-data-type"></a>設定識別主索引鍵資料類型

ASP.NET Core 身分識別可讓您輕鬆地設定您要用於主索引鍵的資料類型。 根據預設，識別使用字串資料類型，但是您可以非常快速地覆寫此行為。

## <a name="how-to"></a>如何

1.  第一個步驟是實作的身分識別模型，並覆寫您想要的資料類型的字串類型。

    [!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]

    [!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]
    
2.  實作您的模型與您要用於主索引鍵的資料類型識別的資料庫內容

    [!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
3.  使用您的模型，當您宣告您的應用程式啟動類別中的識別服務需要主索引鍵的資料類型

    [!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]
