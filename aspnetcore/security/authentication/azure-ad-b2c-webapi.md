---
title: 在 ASP.NET Core 中使用 Azure Active Directory B2C 的 web Api 進行驗證
author: camsoper
description: 探索如何使用 ASP.NET Core Web API 來設定 Azure Active Directory B2C 驗證。 使用 Postman 測試已驗證的 Web API。
ms.author: casoper
ms.date: 12/05/2019
ms.custom: mvc, seodec18
uid: security/authentication/azure-ad-b2c-webapi
ms.openlocfilehash: 9b18b19838a2d25944a2498b6eec1677e56b12cc
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/25/2019
ms.locfileid: "75358257"
---
# <a name="authentication-in-web-apis-with-azure-active-directory-b2c-in-aspnet-core"></a>在 ASP.NET Core 中使用 Azure Active Directory B2C 的 web Api 進行驗證

作者 [Cam Soper](https://twitter.com/camsoper)

<!-- Next update remove screenshots. They become obsolete too soon and are more work to update -->

[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) （Azure AD B2C）是適用于 web 和行動應用程式的雲端身分識別管理解決方案。 此服務會針對雲端和內部部署中裝載的應用程式提供驗證。 驗證類型包括個別帳戶、社交網路帳戶和同盟企業帳戶。 Azure AD B2C 也會以最少的設定提供多重要素驗證。

Azure Active Directory （Azure AD）和 Azure AD B2C 是個別的產品供應專案。 Azure AD 租使用者代表組織，而 Azure AD B2C 租使用者代表要與信賴憑證者應用程式搭配使用的身分識別集合。 若要深入瞭解，請參閱[Azure AD B2C：常見問題（FAQ）](/azure/active-directory-b2c/active-directory-b2c-faqs)。

由於 web Api 沒有使用者介面，因此無法將使用者重新導向至安全權杖服務，例如 Azure AD B2C。 相反地，會將持有人權杖從呼叫應用程式傳遞給 API，這已使用 Azure AD B2C 驗證使用者。 然後，API 會驗證權杖，而不會直接進行使用者互動。

在本教學課程中，您將了解如何：

> [!div class="checklist"]
> * 建立 Azure Active Directory B2C 租使用者。
> * 在 Azure AD B2C 中註冊 Web API。
> * 使用 Visual Studio 建立 Web API，並將其設定為使用 Azure AD B2C 的租使用者進行驗證。
> * 設定原則，以控制 Azure AD B2C 租使用者的行為。
> * 使用 Postman 來模擬 web 應用程式，其中顯示登入對話方塊、抓取權杖，並使用它對 Web API 提出要求。

## <a name="prerequisites"></a>必要條件：

此逐步解說需要下列各項：

* [Microsoft Azure 訂用帳戶](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)
* [Postman](https://www.getpostman.com/postman)

## <a name="create-the-azure-active-directory-b2c-tenant"></a>建立 Azure Active Directory B2C 租使用者

建立 Azure AD B2C 的租使用者，[如檔中所述](/azure/active-directory-b2c/active-directory-b2c-get-started)。 出現提示時，在本教學課程中，將租使用者與 Azure 訂用帳戶產生關聯是選擇性的。

## <a name="configure-a-sign-up-or-sign-in-policy"></a>設定註冊或登入原則

使用 Azure AD B2C 檔中的步驟來[建立註冊或登入原則](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions)。 將原則命名為**SiUpIn**。  使用適用于身分**識別提供者**、**註冊屬性**和**應用程式宣告**的檔中提供的範例值。 使用 [**立即執行**] 按鈕來測試原則（如檔中所述）是選擇性的。

## <a name="register-the-api-in-azure-ad-b2c"></a>在 Azure AD B2C 中註冊 API

在新建立的 Azure AD B2C 租使用者中，使用**註冊 Web API**一節下的檔中的[步驟](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application)來註冊您的 API。

使用下列值：

| 設定                       | {2&gt;值&lt;2}               | 注意事項                                                                                  |
|-------------------------------|---------------------|----------------------------------------------------------------------------------------|
| **Name**                      | *{API 名稱}*        | 輸入應用程式的**名稱**，以向取用者描述您的應用程式。                     |
| **包含 Web 應用程式 / Web API** | 是                 |                                                                                        |
| **允許隱含流程**       | 是                 |                                                                                        |
| **回覆 URL**                 | `https://localhost` | 回覆 URL 是 Azure AD B2C 傳回您應用程式要求之任何權杖的所在端點。 |
| **應用程式識別碼 URI**                | *api*               | URI 不需要解析為實體位址。 它只需要是唯一的。     |
| **包含原生用戶端**     | 否                  |                                                                                        |

註冊 API 之後，就會顯示租使用者中的應用程式和 Api 清單。 選取先前註冊的 API。 選取 [**應用程式識別碼**] 欄位右邊的**複製**圖示，將它複製到剪貼簿。 選取 [**已發佈的範圍**]，並確認預設的*user_impersonation*範圍是否存在。

## <a name="create-an-aspnet-core-app-in-visual-studio"></a>在 Visual Studio 中建立 ASP.NET Core 應用程式

Visual Studio Web 應用程式範本可以設定為使用 Azure AD B2C 租使用者進行驗證。

在 Visual Studio 中：

1. 建立新的 ASP.NET Core Web 應用程式。 
2. 從範本清單中選取 [ **WEB API** ]。
3. 選取 [**變更驗證**] 按鈕。

    ![[變更驗證] 按鈕](./azure-ad-b2c-webapi/change-auth-button.png)

4. 在 [**變更驗證**] 對話方塊中，選取 [**個別使用者帳戶**]， > 連線**到雲端中現有的使用者存放區**。

    ![[變更驗證] 對話方塊](./azure-ad-b2c-webapi/change-auth-dialog.png)

5. 使用下列值來完成表單：

    | 設定                       | {2&gt;值&lt;2}                                                 |
    |-------------------------------|-------------------------------------------------------|
    | **網域名稱**               | *{B2C 租使用者的功能變數名稱}*                |
    | **應用程式識別碼**            | *{貼上剪貼簿中的應用程式識別碼}*       |
    | **註冊或登入原則** | B2C_1_SiUpIn                                          |

    選取 **[確定]** 以關閉 [**變更驗證**] 對話方塊。 選取 **[確定]** 以建立 web 應用程式。

Visual Studio 會使用名為*ValuesController.cs*的控制器來建立 Web API，以傳回 GET 要求的硬式編碼值。 類別會以[授權](xref:security/authorization/simple)屬性標示，因此所有要求都需要驗證。

## <a name="run-the-web-api"></a>執行 Web API

在 Visual Studio 中，執行 API。 Visual Studio 會啟動指向 API 根 URL 的瀏覽器。 記下網址列中的 URL，並將 API 保留在背景中執行。

> [!NOTE]
> 由於根 URL 沒有定義的控制器，因此瀏覽器可能會顯示404（找不到頁面）錯誤。 這是正常的現象。

## <a name="use-postman-to-get-a-token-and-test-the-api"></a>使用 Postman 取得權杖並測試 API

[Postman](https://getpostman.com/postman)是用來測試 web api 的工具。 在本教學課程中，Postman 會模擬代表使用者存取 Web API 的 web 應用程式。

### <a name="register-postman-as-a-web-app"></a>將 Postman 註冊為 web 應用程式

由於 Postman 會模擬可從 Azure AD B2C 租使用者取得權杖的 web 應用程式，因此它必須在租使用者中註冊為 web 應用程式。 使用**註冊 web 應用程式**一節底下[檔中的步驟](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application)來註冊 Postman。 停止**建立 web 應用程式用戶端密碼**一節。 本教學課程不需要用戶端密碼。 

使用下列值：

| 設定                       | {2&gt;值&lt;2}                            | 注意事項                           |
|-------------------------------|----------------------------------|---------------------------------|
| **Name**                      | Postman                          |                                 |
| **包含 Web 應用程式 / Web API** | 是                              |                                 |
| **允許隱含流程**       | 是                              |                                 |
| **回覆 URL**                 | `https://getpostman.com/postman` |                                 |
| **應用程式識別碼 URI**                | *{保留空白}*                  | 本教學課程不需要。 |
| **包含原生用戶端**     | 否                               |                                 |

新註冊的 web 應用程式需要代表使用者存取 Web API 的許可權。  

1. 選取應用程式清單中的 [ **Postman** ]，然後從左側功能表中選取 [ **API 存取**]。
1. 選取 [+ 新增]。
1. 在 [**選取 API** ] 下拉式清單中，選取 Web API 的名稱。
1. 在 [**選取範圍**] 下拉式清單中，確定已選取 [所有範圍]。
1. 選取 [確定]。

請記下 Postman 應用程式的應用程式識別碼，因為需要取得持有人權杖。

### <a name="create-a-postman-request"></a>建立 Postman 要求

啟動 Postman。 根據預設，Postman 會在啟動時顯示 [**建立新**的] 對話方塊。 如果未顯示對話方塊，請選取左上方的 [ **+ 新增**] 按鈕。

從 [**建立新**的] 對話方塊：

1. 選取 [要求]。

    ![要求按鈕](./azure-ad-b2c-webapi/postman-create-new.png)

2. 在 [**要求名稱**] 方塊中輸入*取得值*。
3. 選取 [ **+ 建立集合**] 以建立新的集合來儲存要求。 將集合命名為*ASP.NET Core 教學*課程，然後選取核取記號。

    ![建立新的收藏](./azure-ad-b2c-webapi/postman-create-collection.png)

4. 選取 [**儲存至 ASP.NET Core 教學**課程] 按鈕。

### <a name="test-the-web-api-without-authentication"></a>在沒有驗證的情況下測試 Web API

若要確認 Web API 需要驗證，請先提出要求而不進行驗證。

1. 在 [**輸入要求 url** ] 方塊中，輸入 `ValuesController`的 URL。 URL 與在瀏覽器中以附加**api/值**顯示的相同。 例如，`https://localhost:44375/api/values`。
2. 選取 [Send] \(傳送\) 按鈕。
3. 請注意，回應的狀態是 [ *401 未授權*]。

    ![401未經授權的回應](./azure-ad-b2c-webapi/postman-401-status.png)

> [!IMPORTANT]
> 如果您收到「無法取得任何回應」錯誤，您可能需要在[Postman 設定](https://learning.getpostman.com/docs/postman/launching_postman/settings)中停用 SSL 憑證驗證。

### <a name="obtain-a-bearer-token"></a>取得持有人權杖

若要對 Web API 提出已驗證的要求，則需要持有人權杖。 Postman 可讓您輕鬆登入 Azure AD B2C 租使用者並取得權杖。

1. 在 [**授權**] 索引標籤的 [**類型**] 下拉式清單中，選取 [ **OAuth 2.0**]。 在 [**將授權資料新增至**] 下拉式清單中，選取 [**要求標頭**]。 選取 [取得新存取權杖]。

    ![具有設定的 [授權] 索引標籤](./azure-ad-b2c-webapi/postman-auth-tab.png)

2. 完成 [**取得新的存取權杖**] 對話方塊，如下所示：

   |                設定                 |                                             {2&gt;值&lt;2}                                             |                                                                                                                                    注意事項                                                                                                                                     |
   |----------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |      **權杖名稱**       |                                          *{token 名稱}*                                       |                                                                                                                   輸入權杖的描述性名稱。                                                                                                                    |
   |      **授與類型**       |                                           隱含                                            |                                                                                                                                                                                                                                                                              |
   |     **回呼 URL**      |                                 `https://getpostman.com/postman`                              |                                                                                                                                                                                                                                                                              |
   |       **驗證 URL**        | `https://login.microsoftonline.com/{tenant domain name}/oauth2/v2.0/authorize?p=B2C_1_SiUpIn` |  以租使用者的功能變數名稱取代 *{租使用者功能變數名稱}* 。 **重要**事項：此 URL 必須具有與 Web API 的*appsettings*中 `AzureAdB2C.Instance` 中找到的相同功能變數名稱。 請參閱附注&dagger;。                                                  |
   |       **用戶端識別碼**       |                *{輸入 Postman 應用程式的**應用程式識別碼**}*                              |                                                                                                                                                                                                                                                                              |
   |         **範圍**         |         `https://{tenant domain name}/{api}/user_impersonation openid offline_access`       | 以租使用者的功能變數名稱取代 *{租使用者功能變數名稱}* 。 以您在第一次註冊 Web API 時提供的應用程式識別碼 URI 取代 *{api}* （在此案例中，`api`）。 URL 的模式為： `https://{tenant}.onmicrosoft.com/{api-id-uri}/{scope name}`。         |
   |         **狀態**         |                                      *{保留空白}*                                          |                                                                                                                                                                                                                                                                              |
   | **用戶端驗證** |                                在主體中傳送用戶端認證                                |                                                                                                                                                                                                                                                                              |

    > [!NOTE]
    > &dagger; Azure Active Directory B2C 入口網站中的 [原則設定] 對話方塊會顯示兩個可能的 Url：一個格式為 `https://login.microsoftonline.com/`{租使用者功能變數名稱}/{additional 路徑資訊}，另一個格式 `https://{tenant name}.b2clogin.com/`{租使用者功能變數名稱}/{additional 路徑資訊}。 請務必**在**Web API 的*appsettings*中的 `AzureAdB2C.Instance` 中找到的網域與 web 應用程式的*appsettings*檔案中所使用的相同。 這是用於 Postman 中 [驗證 URL] 欄位的相同網域。 請注意，Visual Studio 使用與在入口網站中顯示的 URL 格式稍有不同。 只要定義域相符，URL 就可以運作。

3. 選取 [**要求權杖**] 按鈕。

4. Postman 會開啟一個新視窗，其中包含 Azure AD B2C 租使用者的登入對話方塊。 使用現有的帳戶登入（如果已建立測試原則），或選取 [**立即註冊**] 來建立新的帳戶。 [**忘記密碼？** ] 連結是用來重設忘記的密碼。

5. 成功登入之後，視窗會關閉，並顯示 [**管理存取權杖**] 對話方塊。 向下流覽至底部，然後選取 [**使用權杖**] 按鈕。

    ![哪裡可以找到 [使用權杖] 按鈕](./azure-ad-b2c-webapi/postman-access-token.png)

### <a name="test-the-web-api-with-authentication"></a>使用驗證測試 Web API

選取 [**傳送**] 按鈕，以再次傳送要求。 這次，回應狀態為*200 OK* ，而且 JSON 承載會顯示在 [回應**主體**] 索引標籤上。

![承載和成功狀態](./azure-ad-b2c-webapi/postman-success.png)

## <a name="next-steps"></a>後續步驟

在本教學課程中，您將了解如何：

> [!div class="checklist"]
> * 建立 Azure Active Directory B2C 租使用者。
> * 在 Azure AD B2C 中註冊 Web API。
> * 使用 Visual Studio 建立 Web API，並將其設定為使用 Azure AD B2C 的租使用者進行驗證。
> * 設定原則，以控制 Azure AD B2C 租使用者的行為。
> * 使用 Postman 來模擬 web 應用程式，其中顯示登入對話方塊、抓取權杖，並使用它對 Web API 提出要求。

藉由學習來繼續開發您的 API：

* [使用 Azure AD B2C 保護 ASP.NET Core web 應用程式](xref:security/authentication/azure-ad-b2c)。
* [使用 Azure AD B2C 從 .net web 應用程式呼叫 .net Web API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet)。
* [自訂 Azure AD B2C 使用者介面](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization)。
* [設定密碼複雜性需求](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity)。
* [啟用多重要素驗證](/azure/active-directory-b2c/active-directory-b2c-reference-mfa)。
* 設定其他身分識別提供者，例如[Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app)、 [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app)、 [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app)、 [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app)、 [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)等等。
* [使用 [Azure AD] 圖形 API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet)從 Azure AD B2C 租使用者中取出額外的使用者資訊，例如群組成員資格。
