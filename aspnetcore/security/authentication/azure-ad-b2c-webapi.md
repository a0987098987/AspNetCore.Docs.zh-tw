---
title: "在 web 應用程式開發介面與 Azure Active Directory B2C 的雲端驗證"
author: camsoper
description: "了解如何設定 Azure Active Directory B2C 驗證與 ASP.NET Core Web API。 測試已驗證的 web 應用程式開發介面與郵差。"
ms.author: casoper
manager: wpickett
ms.date: 01/25/2018
ms.topic: tutorial
ms.technology: aspnet
ms.prod: asp.net-core
ms.custom: mvc
uid: security/authentication/azure-ad-b2c-webapi
ms.openlocfilehash: d768e2daf2464b282b097e935ef6c5f85e8705f5
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2018
---
# <a name="cloud-authentication-in-web-apis-with-azure-active-directory-b2c"></a>在 web 應用程式開發介面與 Azure Active Directory B2C 的雲端驗證

作者 [Cam Soper](https://twitter.com/camsoper)

[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) 是 web 和行動裝置應用程式的雲端身分識別管理解決方案。 服務提供裝載於雲端和內部部署的應用程式的驗證。 驗證類型包括個別帳戶，社交網路帳戶，以及同盟企業帳戶。 此外，Azure AD B2C 可提供多重要素驗證，以最低組態。

> [!TIP]
> Azure Active Directory (Azure AD) 的 Azure AD B2C 是個別產品的供應項目。 Azure AD 租用戶代表組織中，而 Azure AD B2C 租用戶代表與信賴憑證者的合作對象應用程式使用的身分識別的集合。 若要進一步了解，請參閱[Azure AD B2C： 常見問題集 (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs)。

Web 應用程式開發介面不會有使用者介面，因為它們無法將使用者重新導向至 Azure AD B2C 這類安全權杖服務。 相反地，API 會從呼叫的應用程式，這已經過驗證的使用者的 Azure AD B2C 傳遞持有人權杖。 接著，應用程式開發介面來驗證沒有直接的使用者互動的語彙基元。

在本教學課程，了解如何：

> [!div class="checklist"]
> * 建立 Azure Active Directory B2C 租用戶。
> * 在 Azure AD B2C 中註冊 Web 應用程式開發介面。
> * 您可以使用 Visual Studio 來建立 Web API 設定要用於驗證的 Azure AD B2C 租用戶。
> * 設定控制行為的 Azure AD B2C 租用戶的原則。
> * 使用郵差來模擬 web 應用程式會顯示登入 對話方塊中，擷取語彙基元，並用它來對 web API 提出要求。

## <a name="prerequisites"></a>必要條件

此逐步解說需要下列條件：

* [Microsoft Azure 訂用帳戶](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) （任何版本）
* [Postman](https://www.getpostman.com/postman)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>建立 Azure Active Directory B2C 租用戶

建立 Azure AD B2C 租用戶[文件中所述](/azure/active-directory-b2c/active-directory-b2c-get-started)。 出現提示時，將租用戶與 Azure 訂用帳戶產生關聯是選擇性的在本教學課程。

## <a name="configure-a-sign-up-or-sign-in-policy"></a>設定註冊或登入的原則

使用 Azure AD B2C 文件中的步驟[建立註冊或登入的原則](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy)。 命名原則**SiUpIn**。  使用提供的文件中的範例值**身分識別提供者**，**註冊屬性**，和**應用程式宣告**。 使用**立即執行**是選擇性的按鈕來測試原則文件中所述。

## <a name="register-the-api-in-azure-ad-b2c"></a>在 Azure AD B2C 中註冊應用程式開發介面

在新建立的 Azure AD B2C 租用戶註冊您的應用程式開發介面使用[文件中的步驟](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-api)下**註冊 web 應用程式開發介面**> 一節。

使用下列值：

| 設定                       | 值               | 注意                                                                                  |
|-------------------------------|---------------------|----------------------------------------------------------------------------------------|
| **名稱**                      | *&lt;API 名稱&gt;*  | 輸入**名稱**描述給取用者應用程式的應用程式。                     |
| **包含 web 應用程式/web 應用程式開發介面** | [是]                 |                                                                                        |
| **允許隱含流程**       | [是]                 |                                                                                        |
| **回覆 URL**                 | `https://localhost` | 回覆 Url 是端點，其中是 Azure AD B2C 會傳回任何您的應用程式要求的權杖。 |
| **應用程式識別碼 URI**                | *api*               | URI 不需要解析的實體位址。 它只需要是唯一的。     |
| **包含原生用戶端**     | 否                  |                                                                                        |

註冊應用程式開發介面之後，會顯示租用戶中的應用程式和應用程式開發介面清單。 選取剛才已註冊的 API。 選取**複製**圖示右邊的**應用程式識別碼**欄位，以將它複製到剪貼簿。 選取**發行領域**並確認預設*user_impersonation*範圍會存在。

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a>在 Visual Studio 2017 中建立 ASP.NET Core 應用程式

Visual Studio Web 應用程式範本可以設定為使用 Azure AD B2C 租用戶進行驗證。

在 Visual Studio 中：

1. 建立新的 ASP.NET Core Web 應用程式。 
2. 選取**Web API**從範本清單。
3. 選取**變更驗證** 按鈕。
    
    ![變更 [驗證] 按鈕](./azure-ad-b2c-webapi/change-auth-button.png)

4. 在**變更驗證**對話方塊中，選取**個別使用者帳戶**，然後選取**連接到雲端中現有的使用者存放區**下拉式清單中。 
    
    ![變更 [驗證] 對話方塊](./azure-ad-b2c-webapi/change-auth-dialog.png)

5. 完成表單具有下列值：
    
    | 設定                       | 值                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **網域名稱**               | *&lt;B2C 租用戶的網域名稱&gt;*          |
    | **應用程式識別碼**            | *&lt;貼上剪貼簿中的應用程式識別碼&gt;* |
    | **註冊或登入的原則** | `B2C_1_SiUpIn`                                        |
    
    選取**確定**關閉**變更驗證**對話方塊。 選取**確定**建立 web 應用程式。

Visual Studio 會建立名為的控制站的 web API *ValuesController.cs* ，傳回硬式編碼值為 GET 要求。 類別以裝飾[Authorize 屬性](xref:security/authorization/simple)，因此所有要求均都需要驗證。

## <a name="run-the-web-api"></a>執行 web 應用程式開發介面

在 Visual Studio 執行應用程式開發介面。 Visual Studio 會啟動瀏覽器指向的 API 根目錄 URL。 請注意網址列中的 URL，並將保留在背景執行的 API。

> [!NOTE]
> 因為不沒有定義根目錄 URL 的任何控制器，則瀏覽器會顯示 404 （找不到頁面） 錯誤。 這是正常的現象。

## <a name="use-postman-to-get-a-token-and-test-the-api"></a>使用郵差取得權杖，並測試應用程式開發介面

[郵差](https://getpostman.com/postman)是 web 應用程式開發介面的工具進行測試。 本教學課程，郵差模擬存取 web 應用程式開發介面代表使用者的 web 應用程式。

### <a name="register-postman-as-a-web-app"></a>為 web 應用程式註冊郵差

因為郵差模擬可以從 Azure AD B2C 租用戶取得權杖的 web 應用程式，它必須註冊租用戶中為 web 應用程式。 註冊使用郵差[文件中的步驟](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app)下**註冊 web 應用程式**> 一節。 在停止**建立 web 應用程式的用戶端密碼**> 一節。 此教學課程中，不需要用戶端密碼。 

使用下列值：

| 設定                       | 值                            | 注意                           |
|-------------------------------|----------------------------------|---------------------------------|
| **名稱**                      | 郵差                          |                                 |
| **包含 web 應用程式/web 應用程式開發介面** | [是]                              |                                 |
| **允許隱含流程**       | [是]                              |                                 |
| **回覆 URL**                 | `https://getpostman.com/postman` |                                 |
| **應用程式識別碼 URI**                | *&lt;保留空白&gt;*            | 本教學課程中，不需要。 |
| **包含原生用戶端**     | 否                               |                                 |

新註冊的 web 應用程式需要存取 web 應用程式開發介面代表使用者的權限。  

1. 選取**郵差**清單中的應用程式，然後選取**API 存取**從左側功能表。
2. 選取**+ 加入**。
3. 在**選取應用程式開發介面**下拉式清單中，選取 web 應用程式開發介面的名稱。
4. 在**選取範圍**下拉式清單中，確定已選取所有領域。
5. 選取**確定**。

請注意郵差應用程式的應用程式識別碼，因為它需要取得的承載權杖。

### <a name="create-a-postman-request"></a>建立郵差要求

啟動郵差。 根據預設，顯示郵差**新建**時啟動對話方塊。 如果未顯示對話方塊，選取**+ 新增**左上角的按鈕。

從**新建**對話方塊：

1. 選取**要求**。
    
    ![要求按鈕](./azure-ad-b2c-webapi/postman-create-new.png)

2. 輸入*取得的值*中**要求名稱**方塊。
3. 選取**+ 建立集合**來建立新的集合，用於儲存要求。 名稱集合*ASP.NET Core 教學課程*，然後選取核取記號。
    
    ![建立新的集合](./azure-ad-b2c-webapi/postman-create-collection.png)

4. 選取**儲存 ASP.NET Core 教學課程** 按鈕。

### <a name="test-the-web-api-withoutauthentication"></a>測試 web 應用程式開發介面 withoutauthentication

若要確認 web API 需要驗證，首先請未經驗證的要求。

1. 在**輸入要求 URL**方塊中，輸入的 URL `ValuesController`。 URL 是相同的瀏覽器中顯示**api/值**附加。 範例就是`https://localhost:44375/api/values`。
2. 選取**傳送** 按鈕。
3. 請注意回應的狀態是*401 未授權*。

    ![401 未經授權的回應](./azure-ad-b2c-webapi/postman-401-status.png)

### <a name="obtain-a-bearer-token"></a>取得持有人權杖。

若要讓已驗證的要求至 web API，持有人權杖。 郵差輕鬆地登入 Azure AD B2C 租用戶，並取得權杖。

1. 在**授權**索引標籤的**類型**下拉式清單中，選取**OAuth 2.0**。 在**授權將資料加入至**下拉式清單中，選取**要求標頭**。 選取**取得新存取權杖**。
    
    ![授權與 [設定] 索引標籤](./azure-ad-b2c-webapi/postman-auth-tab.png)

2. 完成**取得新的存取權杖**對話方塊，如下所示：
    
    | 設定                   | 值                                                                                         | 注意                                                                                      |
    |---------------------------|-----------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
    | **語彙基元名稱**            | *&lt;語彙基元名稱&gt;*                                                                          | 輸入語彙基元的描述性名稱。                                                    |
    | **授與類型**            | 隱含                                                                                      |                                                                                            |
    | **回呼 URL**          | `https://getpostman.com/postman`                                                              |                                                                                            |
    | **驗證 URL**              | `https://login.microsoftonline.com/<tenant domain name>/oauth2/v2.0/authorize?p=B2C_1_SiUpIn` | 取代*&lt;租用戶網域名稱&gt;*與不含括弧的租用戶的網域名稱。 |
    | **用戶端識別碼**             | *&lt;輸入郵差應用程式的<b>應用程式識別碼</b>&gt;*                                       |                                                                                            |
    | **用戶端密碼**         | *&lt;保留空白&gt;*                                                                         |                                                                                            |
    | **範圍**                 | `https://<tenant domain name>/api/user_impersonation openid offline_access`                   | 取代*&lt;租用戶網域名稱&gt;*與不含括弧的租用戶的網域名稱。 |
    | **用戶端驗證** | 在本文中傳送用戶端認證                                                               |                                                                                            |
    
3. 選取**要求語彙基元** 按鈕。

4. 郵差開啟新視窗包含 Azure AD B2C 租用戶的登入對話方塊。 （如果已建立一個測試原則），使用現有的帳戶登入，或選取**立即註冊**來建立新的帳戶。 **忘記密碼？**連結用來重設忘記的密碼。

5. 已成功登入之後，關閉視窗而**管理存取權杖**對話方塊隨即出現。 捲動到底部，然後選取**使用語彙基元** 按鈕。
    
    ![如何尋找 「 使用語彙基元 」 按鈕](./azure-ad-b2c-webapi/postman-access-token.png)

### <a name="test-the-web-api-with-authentication"></a>測試 web 應用程式開發介面使用驗證

選取**傳送** 按鈕，再傳送要求。 此時，回應狀態是*200 確定* ，JSON 裝載回應上看到**主體** 索引標籤。
    
![裝載及成功與否的狀態](./azure-ad-b2c-webapi/postman-success.png)

## <a name="next-steps"></a>後續步驟

在本教學課程中，您將了解如何：

> [!div class="checklist"]
> * 建立 Azure Active Directory B2C 租用戶。
> * 在 Azure AD B2C 中註冊 Web 應用程式開發介面。
> * 您可以使用 Visual Studio 來建立 Web API 設定要用於驗證的 Azure AD B2C 租用戶。
> * 設定控制行為的 Azure AD B2C 租用戶的原則。
> * 使用郵差來模擬 web 應用程式會顯示登入 對話方塊中，擷取語彙基元，並用它來對 web API 提出要求。

繼續學習來開發您的 API:

* [保護 ASP.NET Core web 應用程式使用 Azure AD B2C](xref:security/authentication/azure-ad-b2c)。
* [從.NET web 應用程式中使用 Azure AD B2C 呼叫.NET web 應用程式開發介面](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet)。
* [自訂使用者介面，Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization)。
* [設定密碼複雜性需求](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity)。
* [啟用多因素驗證](/azure/active-directory-b2c/active-directory-b2c-reference-mfa)。
* 設定其他身分識別提供者，例如[Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app)， [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app)， [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app)， [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app)， [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)，和其他人。
* [使用 Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet)擷取額外的使用者資訊，例如群組成員資格，從 Azure AD B2C 租用戶。
