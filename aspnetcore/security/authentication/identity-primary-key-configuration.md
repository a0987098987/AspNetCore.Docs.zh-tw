---
title: "設定識別主索引鍵資料類型"
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 2afcdf89b2a39d82a4ba72dc780be71ac98ab664
ms.sourcegitcommit: 76d42f09f3e0dd2f2105493eca6b29994aa47706
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/19/2017
---
# <a name="configure-identity-primary-keys-data-type"></a>設定識別主索引鍵資料類型

ASP.NET Core 身分識別可讓您輕鬆地設定您要用於主索引鍵的資料類型。 根據預設，識別使用字串資料類型，但是您可以非常快速地覆寫此行為。

## <a name="how-to"></a>如何

1.  第一個步驟是實作的身分識別模型，並覆寫您想要的資料類型的字串類型。

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]
    
2.  實作您的模型與您要用於主索引鍵的資料類型識別的資料庫內容

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
3.  使用您的模型，當您宣告您的應用程式啟動類別中的識別服務需要主索引鍵的資料類型

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]
