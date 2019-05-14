---
title: 在 web Api 與 Azure Active Directory B2C 在 ASP.NET Core 中的驗證
author: camsoper
description: 了解如何設定 Azure Active Directory B2C 驗證與 ASP.NET Core Web API。 測試驗證的 web API 使用 Postman。
ms.author: casoper
ms.date: 09/21/2018
ms.custom: mvc, seodec18
uid: security/authentication/azure-ad-b2c-webapi
ms.openlocfilehash: 0eb8b533f44a1f72cfc3c4ec5ec060adb37eed6c
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610363"
---
# <a name="authentication-in-web-apis-with-azure-active-directory-b2c-in-aspnet-core"></a>在 web Api 與 Azure Active Directory B2C 在 ASP.NET Core 中的驗證

作者 [Cam Soper](https://twitter.com/camsoper)

[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) 是適用於 web 和行動裝置應用程式的雲端身分識別管理解決方案。 服務提供雲端和內部部署中託管的應用程式的驗證。 驗證類型包括個別帳戶，社交網路帳戶，以及同盟企業帳戶。 Azure AD B2C 也提供多重要素驗證，以最低組態。

Azure Active Directory (Azure AD) 與 Azure AD B2C 是個別的產品供應項目。 Azure AD 租用戶代表組織，而 Azure AD B2C 租用戶代表與信賴憑證者應用程式要使用的身分識別的集合。 若要進一步了解，請參閱[Azure AD B2C:常見問題集 (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs)。

Web Api 都沒有使用者介面，因為它們無法將使用者重新導向至安全權杖服務，例如 Azure AD B2C。 相反地，API 會從呼叫端已經驗證與 Azure AD B2C 使用者的應用程式，傳遞持有人權杖。 API 接著會驗證權杖，而不需要直接使用者互動。

在本教學課程，了解如何：

> [!div class="checklist"]
> * 建立 Azure Active Directory B2C 租用戶。
> * 在 Azure AD B2C 中註冊 Web API。
> * 使用 Visual Studio 建立 Web API 設定為使用 Azure AD B2C 租用戶進行驗證。
> * 設定控制的 Azure AD B2C 租用戶行為的原則。
> * 使用 Postman 來模擬 web 應用程式會顯示登入對話方塊中，擷取權杖，並用它來對 web API 提出要求。

## <a name="prerequisites"></a>必要條件

在此逐步解說需要下列條件：

* [Microsoft Azure 訂用帳戶](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)
* [Postman](https://www.getpostman.com/postman)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>建立 Azure Active Directory B2C 租用戶

建立 Azure AD B2C 租用戶[文件中所述](/azure/active-directory-b2c/active-directory-b2c-get-started)。 出現提示時，租用戶關聯至 Azure 訂用帳戶是選擇性的在本教學課程。

## <a name="configure-a-sign-up-or-sign-in-policy"></a>設定註冊或登入原則

使用 Azure AD B2C 文件中的步驟[建立註冊或登入原則](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy)。 命名原則**SiUpIn**。  使用提供的文件中的範例值**身分識別提供者**，**註冊屬性**，並**應用程式宣告**。 使用**立即執行**是選擇性的按鈕來測試原則，文件中所述。

## <a name="register-the-api-in-azure-ad-b2c"></a>在 Azure AD B2C 中註冊 API

API 使用新建立的 Azure AD B2C 租用戶中註冊[文件中的步驟](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-api)下方**註冊 web API**一節。

使用下列值：

| 設定                       | 值               | 注意                                                                                  |
|-------------------------------|---------------------|----------------------------------------------------------------------------------------|
| **名稱**                      | *{API name}*        | 請輸入**名稱**描述取用者應用程式的應用程式。                     |
| **包含 web 應用程式/web API** | 是                 |                                                                                        |
| **允許隱含流程**       | 是                 |                                                                                        |
| **回覆 URL**                 | `https://localhost` | 回覆 Url 會是 Azure AD B2C 傳回您的應用程式要求之任何權杖的所在端點。 |
| **應用程式識別碼 URI**                | *api*               | URI 不需要解析成實體地址。 它只需要是唯一的。     |
| **包含原生用戶端**     | 否                  |                                                                                        |

註冊 API 之後，會顯示租用戶中的應用程式和 Api 清單。 選取先前登錄的 API。 選取**複製**右邊的圖示**APPLICATION-ID**欄位，以將它複製到剪貼簿。 選取 **發佈的範圍**，並確認預設*user_impersonation*範圍會存在。

## <a name="create-an-aspnet-core-app-in-visual-studio"></a>在 Visual Studio 中建立 ASP.NET Core 應用程式

Visual Studio Web 應用程式範本可以設定要用於驗證的 Azure AD B2C 租用戶。

在 Visual Studio 中：

1. 建立新的 ASP.NET Core Web 應用程式。 
2. 選取  **Web API**從範本清單。
3. 選取 [**變更驗證**] 按鈕。

    ![變更 [驗證] 按鈕](./azure-ad-b2c-webapi/change-auth-button.png)

4. 在 **變更驗證**對話方塊中，選取**個別使用者帳戶** > **連線到雲端中現有的使用者存放區**。

    ![變更驗證 對話方塊](./azure-ad-b2c-webapi/change-auth-dialog.png)

5. 完成表單，使用下列值：

    | 設定                       | 值                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **網域名稱**               | *{您的 B2C 租用戶網域名稱}*                |
    | **應用程式識別碼**            | *{貼上剪貼簿中的應用程式識別碼}*       |
    | **註冊或登入原則** | B2C_1_SiUpIn                                          |

    選取  **確定**以關閉**變更驗證**對話方塊。 選取 **確定**建立 web 應用程式。

Visual Studio 建立 web API，具有名為控制器*ValuesController.cs*傳回硬式編碼的 GET 要求的值。 類別以裝飾[Authorize 屬性](xref:security/authorization/simple)，因此所有要求均都需要驗證。

## <a name="run-the-web-api"></a>執行 web API

在 Visual Studio 中執行的 API。 Visual Studio 會啟動瀏覽器指向 API 的根 URL。 請注意網址列中的 URL，並將在背景中執行的 API。

> [!NOTE]
> 因為沒有定義的根 URL 的控制站時，瀏覽器可能會顯示 404 （找不到頁面） 錯誤。 這是正常的現象。

## <a name="use-postman-to-get-a-token-and-test-the-api"></a>使用 Postman 來取得權杖，並測試 API

[Postman](https://getpostman.com/postman)是一項工具測試 web Api。 本教學課程中，Postman 會模擬存取代表使用者的 web API 的 web 應用程式。

### <a name="register-postman-as-a-web-app"></a>註冊為 web 應用程式的 Postman

Postman 會模擬來自 Azure AD B2C 租用戶取得權杖的 web 應用程式，因為它必須註冊租用戶中為 web 應用程式。 註冊使用 Postman[文件中的步驟](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app)下方**註冊 web 應用程式**一節。 在停止**建立 web 應用程式用戶端祕密**一節。 本教學課程中，不需要用戶端祕密。 

使用下列值：

| 設定                       | 值                            | 注意                           |
|-------------------------------|----------------------------------|---------------------------------|
| **名稱**                      | Postman                          |                                 |
| **包含 web 應用程式/web API** | 是                              |                                 |
| **允許隱含流程**       | 是                              |                                 |
| **回覆 URL**                 | `https://getpostman.com/postman` |                                 |
| **應用程式識別碼 URI**                | *{空白}*                  | 本教學課程中，不需要。 |
| **包含原生用戶端**     | 否                               |                                 |

新註冊的 web 應用程式需要存取 web API 上代表使用者的權限。  

1. 選取  **Postman**在應用程式，然後選取這份**API 存取**從左側功能表上。
1. 選取  **+ 新增**。
1. 在 **選取 API**下拉式清單中，選取 web API 的名稱。
1. 在 **選取範圍**下拉式清單中，確定已選取 所有範圍。
1. 選取  **Ok**。

請注意 Postman 應用程式的應用程式識別碼，因為它需要取得持有人權杖。

### <a name="create-a-postman-request"></a>建立 Postman 要求

啟動 Postman。 根據預設，顯示 Postman**新建**時啟動的對話方塊。 如果未顯示 [] 對話方塊中，選取 **+ 新增**左上角的按鈕。

從**新建**對話方塊：

1. 選取 **要求**。

    ![要求按鈕](./azure-ad-b2c-webapi/postman-create-new.png)

2. 請輸入*取得的值*中**要求名稱** 方塊中。
3. 選取  **+ 建立集合**來建立新的集合，以便儲存要求。 替集合命名*ASP.NET Core 教學課程*，然後選取核取記號。

    ![建立新的集合](./azure-ad-b2c-webapi/postman-create-collection.png)

4. 選取 [**儲存至 ASP.NET Core 教學課程**] 按鈕。

### <a name="test-the-web-api-without-authentication"></a>測試 web API，而不需要驗證

若要確認 web API 需要驗證，首先請未經驗證的要求。

1. 在 **輸入要求 URL**方塊中，輸入的 URL `ValuesController`。 URL 會與顯示在瀏覽器中使用的相同**api/值**附加。 例如， `https://localhost:44375/api/values` 。
2. 選取 [**傳送**] 按鈕。
3. 請注意回應的狀態是*401 未授權*。

    ![401 未授權的回應](./azure-ad-b2c-webapi/postman-401-status.png)

> [!IMPORTANT]
> 如果您收到 「 無法取得任何回應 」 的錯誤，您可能需要停用中的 SSL 憑證驗證[Postman 設定](https://learning.getpostman.com/docs/postman/launching_postman/settings)。

### <a name="obtain-a-bearer-token"></a>取得持有人權杖

若要讓 web API 已驗證的要求，持有人權杖是必要的。 Postman 可方便在登入 Azure AD B2C 租用戶，並取得權杖。

1. 在 **授權**索引標籤中，於**型別**下拉式清單中，選取**OAuth 2.0**。 在 **授權將資料加入至**下拉式清單中，選取**要求標頭**。 選取 **取得新存取權杖**。

    ![設定 [授權] 索引標籤](./azure-ad-b2c-webapi/postman-auth-tab.png)

2. 完成**取得新存取權杖**，如下所示的對話方塊：

   |                設定                 |                                             值                                             |                                                                                                                                    注意                                                                                                                                     |
   |----------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |      **權杖名稱**       |                                          *{語彙基元名稱}*                                       |                                                                                                                   輸入權杖的描述性名稱。                                                                                                                    |
   |      **授與類型**       |                                           隱含                                            |                                                                                                                                                                                                                                                                              |
   |     **回呼 URL**      |                                 `https://getpostman.com/postman`                              |                                                                                                                                                                                                                                                                              |
   |       **驗證 URL**        | `https://login.microsoftonline.com/{tenant domain name}/oauth2/v2.0/authorize?p=B2C_1_SiUpIn` |  取代 *{租用戶網域名稱}* 租用戶的網域名稱。 **重要**:此 URL 必須有相同的網域名稱，做為項目中找到`AzureAdB2C.Instance`在 web API *appsettings.json*檔案。 請參閱附註&dagger;。                                                  |
   |       **用戶端識別碼**       |                *{輸入 Postman 應用程式**應用程式識別碼**}*                              |                                                                                                                                                                                                                                                                              |
   |         **範圍**         |         `https://{tenant domain name}/{api}/user_impersonation openid offline_access`       | 取代 *{租用戶網域名稱}* 租用戶的網域名稱。 取代 *{api}* 應用程式識別碼 URI 與您提供給 web API 第一次登錄時 (在此情況下， `api`)。 URL 的模式是： `https://{tenant}.onmicrosoft.com/{api-id-uri}/{scope name}`。         |
   |         **狀態**         |                                      *{空白}*                                          |                                                                                                                                                                                                                                                                              |
   | **用戶端驗證** |                                本文中傳送用戶端認證                                |                                                                                                                                                                                                                                                                              |

    > [!NOTE]
    > &dagger; 在 Azure Active Directory B2C 入口網站中的 [原則設定] 對話方塊會顯示兩個可能的 Url:其中一個格式`https://login.microsoftonline.com/`{租用戶網域名稱} / {額外路徑資訊} 和其他格式`https://{tenant name}.b2clogin.com/`{租用戶網域名稱} / {額外路徑資訊}。 它有**重要**的網域中找到`AzureAdB2C.Instance`在 web API 的*appsettings.json*檔案符合所使用的 web 應用程式*appsettings.json*檔案。 這是用於在 Postman 中的 [驗證 URL] 欄位的相同網域。 請注意，Visual Studio 會使用稍微不同的 URL 格式，比在入口網站中顯示的內容。 只要符合定義域，URL 就會運作。

3. 選取 [**要求權杖**] 按鈕。

4. Postman 會開啟新視窗，其中包含 Azure AD B2C 租用戶的登入對話方塊。 （如果已建立一個測試原則），使用現有的帳戶登入，或選取**立即註冊**建立新的帳戶。 **忘記密碼？** 連結用來重設遺忘的密碼。

5. 在視窗關閉後成功登入，而**管理存取權杖**對話方塊隨即出現。 捲動到底部，然後選取**使用語彙基元** 按鈕。

    ![哪裡可以找到 「 使用權杖 」 的按鈕](./azure-ad-b2c-webapi/postman-access-token.png)

### <a name="test-the-web-api-with-authentication"></a>測試 web API 驗證

選取 [**傳送**再次傳送要求] 按鈕。 目前回應的狀態會*200 確定*JSON 承載，都可以看到回應**主體** 索引標籤。

![裝載和成功狀態](./azure-ad-b2c-webapi/postman-success.png)

## <a name="next-steps"></a>後續步驟

在本教學課程中，您將了解如何：

> [!div class="checklist"]
> * 建立 Azure Active Directory B2C 租用戶。
> * 在 Azure AD B2C 中註冊 Web API。
> * 使用 Visual Studio 建立 Web API 設定為使用 Azure AD B2C 租用戶進行驗證。
> * 設定控制的 Azure AD B2C 租用戶行為的原則。
> * 使用 Postman 來模擬 web 應用程式會顯示登入對話方塊中，擷取權杖，並用它來對 web API 提出要求。

繼續開發您的 API，以學習：

* [保護 ASP.NET Core web 應用程式使用 Azure AD B2C](xref:security/authentication/azure-ad-b2c)。
* [呼叫.NET web API，從.NET web 應用程式使用 Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet)。
* [自訂 Azure AD B2C 使用者介面](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization)。
* [設定密碼複雜度需求](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity)。
* [啟用 multi-factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa)。
* 設定額外的身分識別提供者，例如[Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app)， [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app)， [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app)， [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app)， [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)，和其他人。
* [使用 Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet)擷取其他使用者資訊，例如群組成員資格，從 Azure AD B2C 租用戶。
