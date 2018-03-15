---
title: "Azure Active Directory B2C ASP.NET Core 中使用雲端驗證"
author: camsoper
description: "了解如何設定 ASP.NET Core 與 Azure Active Directory B2C 驗證。"
manager: wpickett
ms.date: 01/25/2018
ms.topic: tutorial
ms.technology: aspnet
ms.prod: asp.net-core
ms.custom: mvc
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: a7bad452a68cf7fe7aa81645d79a0ee9e7719fe7
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a>Azure Active Directory B2C ASP.NET Core 中使用雲端驗證

作者 [Cam Soper](https://twitter.com/camsoper)

[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) 是 web 和行動裝置應用程式的雲端身分識別管理解決方案。 服務提供裝載於雲端和內部部署的應用程式的驗證。 驗證類型包括個別帳戶，社交網路帳戶，以及同盟企業帳戶。 此外，Azure AD B2C 可提供多重要素驗證，以最低組態。

> [!TIP]
> Azure Active Directory (Azure AD) 的 Azure AD B2C 是個別產品的供應項目。 Azure AD 租用戶代表組織中，而 Azure AD B2C 租用戶代表與信賴憑證者的合作對象應用程式使用的身分識別的集合。 若要進一步了解，請參閱[Azure AD B2C： 常見問題集 (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs)。

在本教學課程，了解如何：

> [!div class="checklist"]
> * 建立 Azure Active Directory B2C 租用戶
> * 在 Azure AD B2C 註冊應用程式
> * 使用 Visual Studio 來建立 ASP.NET Core web 應用程式設定為使用 Azure AD B2C 租用戶進行驗證
> * 設定控制行為的 Azure AD B2C 租用戶的原則

## <a name="prerequisites"></a>必要條件

此逐步解說需要下列條件：

* [Microsoft Azure 訂用帳戶](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) （任何版本）

## <a name="create-the-azure-active-directory-b2c-tenant"></a>建立 Azure Active Directory B2C 租用戶

建立 Azure Active Directory B2C 租用戶[文件中所述](/azure/active-directory-b2c/active-directory-b2c-get-started)。 出現提示時，將租用戶與 Azure 訂用帳戶產生關聯是選擇性的在本教學課程。

## <a name="register-the-app-in-azure-ad-b2c"></a>在 Azure AD B2C 中註冊應用程式

在新建立的 Azure AD B2C 租用戶註冊您的應用程式使用[文件中的步驟](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app)下**註冊 web 應用程式**> 一節。 在停止**建立 web 應用程式的用戶端密碼**> 一節。 此教學課程中，不需要用戶端密碼。 

使用下列值：

| 設定                       | 值                     | 注意                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **名稱**                      | *&lt;應用程式名稱&gt;*        | 輸入**名稱**描述給取用者應用程式的應用程式。                                                                                                                                 |
| **包含 web 應用程式/web 應用程式開發介面** | [是]                       |                                                                                                                                                                                                    |
| **允許隱含流程**       | [是]                       |                                                                                                                                                                                                    |
| **回覆 URL**                 | `https://localhost:44300` | 回覆 Url 是端點，其中是 Azure AD B2C 會傳回任何您的應用程式要求的權杖。 Visual Studio 提供要使用的回覆 URL。 現在請輸入`https://localhost:44300`完成表單。 |
| **應用程式識別碼 URI**                | 保留空白               | 本教學課程中，不需要。                                                                                                                                                                    |
| **包含原生用戶端**     | 否                        |                                                                                                                                                                                                    |

> [!WARNING]
> 如果設定非 localhost 回覆 URL，請留意[回覆 URL 清單中所允許的條件約束](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url)。 

註冊應用程式之後，會顯示租用戶中的應用程式的清單。 選取剛才已註冊的應用程式。 選取**複製**圖示右邊的**應用程式識別碼**欄位，以將它複製到剪貼簿。

沒有項目比較可以在 Azure AD B2C 租用戶中設定在這個階段，但讓瀏覽器視窗保持開啟。 在建立 ASP.NET Core 應用程式後，就更多的設定。

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>在 Visual Studio 2017 中建立 ASP.NET Core 應用程式

Visual Studio Web 應用程式範本可以設定為使用 Azure AD B2C 租用戶進行驗證。

在 Visual Studio 中：

1. 建立新的 ASP.NET Core Web 應用程式。 
2. 選取**Web 應用程式**從範本清單。
3. 選取**變更驗證** 按鈕。
    
    ![變更 [驗證] 按鈕](./azure-ad-b2c/_static/changeauth.png)

4. 在**變更驗證**對話方塊中，選取**個別使用者帳戶**，然後選取**連接到雲端中現有的使用者存放區**下拉式清單中。 
    
    ![變更 [驗證] 對話方塊](./azure-ad-b2c/_static/changeauthdialog.png)

5. 完成表單具有下列值：
    
    | 設定                       | 值                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **網域名稱**               | *&lt;B2C 租用戶的網域名稱&gt;*          |
    | **應用程式識別碼**            | *&lt;貼上剪貼簿中的應用程式識別碼&gt;* |
    | **回呼路徑**             | *&lt;使用預設值&gt;*                       |
    | **註冊或登入的原則** | `B2C_1_SiUpIn`                                        |
    | **重設密碼原則**     | `B2C_1_SSPR`                                          |
    | **編輯設定檔原則**       | *&lt;保留空白&gt;*                                 |
    
    選取**複製**連結**回覆 URI**複製到剪貼簿的回覆 URI。 選取**確定**關閉**變更驗證**對話方塊。 選取**確定**建立 web 應用程式。

## <a name="finish-the-b2c-app-registration"></a>完成 B2C 應用程式註冊

傳回與 B2C 應用程式屬性仍開啟的瀏覽器視窗。 變更暫存**回覆 URL**指定從 Visual Studio 的較早的值複製。 選取**儲存**視窗的頂端。

> [!TIP]
> 如果您沒有複製回覆 URL，在 web 專案屬性中，使用 SSL 位址，從 [偵錯] 索引標籤，並附加**CallbackPath**值*appsettings.json*。

## <a name="configure-policies"></a>設定原則

使用 Azure AD B2C 文件中的步驟[建立註冊或登入的原則](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy)，然後[建立密碼重設原則](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy)。 使用提供的文件中的範例值**身分識別提供者**，**註冊屬性**，和**應用程式宣告**。 使用**立即執行**是選擇性的按鈕來測試原則文件中所述。

> [!WARNING]
> 確認原則名稱在中使用這些原則的文件中所述完全是**變更驗證**Visual Studio 中的對話方塊。 原則名稱可以驗證在*appsettings.json*。

## <a name="run-the-app"></a>執行應用程式

在 Visual Studio 中，按**F5**建置並執行應用程式。 Web 應用程式啟動之後，請選取**登入**。

![登入應用程式](./azure-ad-b2c/_static/signin.png)

瀏覽器重新導向至 Azure AD B2C 租用戶。 （如果已建立一個測試原則），使用現有的帳戶登入，或選取**立即註冊**來建立新的帳戶。 **忘記密碼？**連結用來重設忘記的密碼。

![Azure AD B2C 登入](./azure-ad-b2c/_static/b2csts.png)

已成功登入之後，瀏覽器重新導向至 web 應用程式。

![成功](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a>後續步驟

在本教學課程中，您將了解如何：

> [!div class="checklist"]
> * 建立 Azure Active Directory B2C 租用戶
> * 在 Azure AD B2C 註冊應用程式
> * 使用 Visual Studio 來建立 ASP.NET Core Web 應用程式設定為使用 Azure AD B2C 租用戶進行驗證
> * 設定控制行為的 Azure AD B2C 租用戶的原則

既然 ASP.NET Core 應用程式設定為進行驗證，使用 Azure AD B2C [Authorize 屬性](xref:security/authorization/simple)可用來保護您的應用程式。 繼續開發您的應用程式所學習：

* [自訂使用者介面，Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization)。
* [設定密碼複雜性需求](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity)。
* [啟用多因素驗證](/azure/active-directory-b2c/active-directory-b2c-reference-mfa)。
* 設定其他身分識別提供者，例如[Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app)， [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app)， [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app)， [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app)， [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)，和其他人。
* [使用 Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet)擷取額外的使用者資訊，例如群組成員資格，從 Azure AD B2C 租用戶。
* [安全 ASP.NET Core web 應用程式開發介面使用 Azure AD B2C](xref:security/authentication/azure-ad-b2c-webapi)。
* [從.NET web 應用程式中使用 Azure AD B2C 呼叫.NET web 應用程式開發介面](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet)。
