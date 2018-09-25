---
title: 在 web Api 與 Azure Active Directory B2C 在 ASP.NET Core 中的雲端驗證
author: camsoper
description: 了解如何設定 Azure Active Directory B2C 驗證與 ASP.NET Core Web API。 測試驗證的 web API 使用 Postman。
ms.author: casoper
ms.date: 09/21/2018
ms.custom: mvc
uid: security/authentication/azure-ad-b2c-webapi
ms.openlocfilehash: 0efc95f508ef84d2728f503f1edd886ce6ae7a79
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028254"
---
# <a name="cloud-authentication-in-web-apis-with-azure-active-directory-b2c-in-aspnet-core"></a><span data-ttu-id="24fcd-104">在 web Api 與 Azure Active Directory B2C 在 ASP.NET Core 中的雲端驗證</span><span class="sxs-lookup"><span data-stu-id="24fcd-104">Cloud authentication in web APIs with Azure Active Directory B2C in ASP.NET Core</span></span>

<span data-ttu-id="24fcd-105">作者 [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="24fcd-105">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="24fcd-106">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) 是適用於 web 和行動裝置應用程式的雲端身分識別管理解決方案。</span><span class="sxs-lookup"><span data-stu-id="24fcd-106">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) is a cloud identity management solution for web and mobile apps.</span></span> <span data-ttu-id="24fcd-107">服務提供雲端和內部部署中託管的應用程式的驗證。</span><span class="sxs-lookup"><span data-stu-id="24fcd-107">The service provides authentication for apps hosted in the cloud and on-premises.</span></span> <span data-ttu-id="24fcd-108">驗證類型包括個別帳戶，社交網路帳戶，以及同盟企業帳戶。</span><span class="sxs-lookup"><span data-stu-id="24fcd-108">Authentication types include individual accounts, social network accounts, and federated enterprise accounts.</span></span> <span data-ttu-id="24fcd-109">此外，Azure AD B2C 可提供多重要素驗證，以最低組態。</span><span class="sxs-lookup"><span data-stu-id="24fcd-109">Additionally, Azure AD B2C can provide multi-factor authentication with minimal configuration.</span></span>

> [!TIP]
> <span data-ttu-id="24fcd-110">Azure Active Directory (Azure AD) 與 Azure AD B2C 是個別的產品供應項目。</span><span class="sxs-lookup"><span data-stu-id="24fcd-110">Azure Active Directory (Azure AD) and Azure AD B2C are separate product offerings.</span></span> <span data-ttu-id="24fcd-111">Azure AD 租用戶代表組織，而 Azure AD B2C 租用戶代表與信賴憑證者應用程式要使用的身分識別的集合。</span><span class="sxs-lookup"><span data-stu-id="24fcd-111">An Azure AD tenant represents an organization, while an Azure AD B2C tenant represents a collection of identities to be used with relying party applications.</span></span> <span data-ttu-id="24fcd-112">若要進一步了解，請參閱[Azure AD B2C： 常見問題集 (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs)。</span><span class="sxs-lookup"><span data-stu-id="24fcd-112">To learn more, see [Azure AD B2C: Frequently asked questions (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).</span></span>

<span data-ttu-id="24fcd-113">Web Api 都沒有使用者介面，因為它們無法將使用者重新導向至安全權杖服務，例如 Azure AD B2C。</span><span class="sxs-lookup"><span data-stu-id="24fcd-113">Since web APIs have no user interface, they're unable to redirect the user to a secure token service like Azure AD B2C.</span></span> <span data-ttu-id="24fcd-114">相反地，API 會從呼叫端已經驗證與 Azure AD B2C 使用者的應用程式，傳遞持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="24fcd-114">Instead, the API is passed a bearer token from the calling app, which has already authenticated the user with Azure AD B2C.</span></span> <span data-ttu-id="24fcd-115">API 接著會驗證權杖，而不需要直接使用者互動。</span><span class="sxs-lookup"><span data-stu-id="24fcd-115">The API then validates the token without direct user interaction.</span></span>

<span data-ttu-id="24fcd-116">在本教學課程，了解如何：</span><span class="sxs-lookup"><span data-stu-id="24fcd-116">In this tutorial, learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="24fcd-117">建立 Azure Active Directory B2C 租用戶。</span><span class="sxs-lookup"><span data-stu-id="24fcd-117">Create an Azure Active Directory B2C tenant.</span></span>
> * <span data-ttu-id="24fcd-118">在 Azure AD B2C 中註冊 Web API。</span><span class="sxs-lookup"><span data-stu-id="24fcd-118">Register a Web API in Azure AD B2C.</span></span>
> * <span data-ttu-id="24fcd-119">使用 Visual Studio 建立 Web API 設定為使用 Azure AD B2C 租用戶進行驗證。</span><span class="sxs-lookup"><span data-stu-id="24fcd-119">Use Visual Studio to create a Web API configured to use the Azure AD B2C tenant for authentication.</span></span>
> * <span data-ttu-id="24fcd-120">設定控制的 Azure AD B2C 租用戶行為的原則。</span><span class="sxs-lookup"><span data-stu-id="24fcd-120">Configure policies controlling the behavior of the Azure AD B2C tenant.</span></span>
> * <span data-ttu-id="24fcd-121">使用 Postman 來模擬 web 應用程式會顯示登入對話方塊中，擷取權杖，並用它來對 web API 提出要求。</span><span class="sxs-lookup"><span data-stu-id="24fcd-121">Use Postman to simulate a web app which presents a login dialog, retrieves a token, and uses it to make a request against the web API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="24fcd-122">必要條件</span><span class="sxs-lookup"><span data-stu-id="24fcd-122">Prerequisites</span></span>

<span data-ttu-id="24fcd-123">在此逐步解說需要下列條件：</span><span class="sxs-lookup"><span data-stu-id="24fcd-123">The following are required for this walkthrough:</span></span>

* [<span data-ttu-id="24fcd-124">Microsoft Azure 訂用帳戶</span><span class="sxs-lookup"><span data-stu-id="24fcd-124">Microsoft Azure subscription</span></span>](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* <span data-ttu-id="24fcd-125">[Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) （任何版本）</span><span class="sxs-lookup"><span data-stu-id="24fcd-125">[Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (any edition)</span></span>
* [<span data-ttu-id="24fcd-126">Postman</span><span class="sxs-lookup"><span data-stu-id="24fcd-126">Postman</span></span>](https://www.getpostman.com/postman)

## <a name="create-the-azure-active-directory-b2c-tenant"></a><span data-ttu-id="24fcd-127">建立 Azure Active Directory B2C 租用戶</span><span class="sxs-lookup"><span data-stu-id="24fcd-127">Create the Azure Active Directory B2C tenant</span></span>

<span data-ttu-id="24fcd-128">建立 Azure AD B2C 租用戶[文件中所述](/azure/active-directory-b2c/active-directory-b2c-get-started)。</span><span class="sxs-lookup"><span data-stu-id="24fcd-128">Create an Azure AD B2C tenant [as described in the documentation](/azure/active-directory-b2c/active-directory-b2c-get-started).</span></span> <span data-ttu-id="24fcd-129">出現提示時，租用戶關聯至 Azure 訂用帳戶是選擇性的在本教學課程。</span><span class="sxs-lookup"><span data-stu-id="24fcd-129">When prompted, associating the tenant with an Azure subscription is optional for this tutorial.</span></span>

## <a name="configure-a-sign-up-or-sign-in-policy"></a><span data-ttu-id="24fcd-130">設定註冊或登入原則</span><span class="sxs-lookup"><span data-stu-id="24fcd-130">Configure a sign-up or sign-in policy</span></span>

<span data-ttu-id="24fcd-131">使用 Azure AD B2C 文件中的步驟[建立註冊或登入原則](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy)。</span><span class="sxs-lookup"><span data-stu-id="24fcd-131">Use the steps in the Azure AD B2C documentation to [create a sign-up or sign-in policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy).</span></span> <span data-ttu-id="24fcd-132">命名原則**SiUpIn**。</span><span class="sxs-lookup"><span data-stu-id="24fcd-132">Name the policy **SiUpIn**.</span></span>  <span data-ttu-id="24fcd-133">使用提供的文件中的範例值**身分識別提供者**，**註冊屬性**，並**應用程式宣告**。</span><span class="sxs-lookup"><span data-stu-id="24fcd-133">Use the example values provided in the documentation for **Identity providers**, **Sign-up attributes**, and **Application claims**.</span></span> <span data-ttu-id="24fcd-134">使用**立即執行**是選擇性的按鈕來測試原則，文件中所述。</span><span class="sxs-lookup"><span data-stu-id="24fcd-134">Using the **Run now** button to test the policy as described in the documentation is optional.</span></span>

## <a name="register-the-api-in-azure-ad-b2c"></a><span data-ttu-id="24fcd-135">在 Azure AD B2C 中註冊 API</span><span class="sxs-lookup"><span data-stu-id="24fcd-135">Register the API in Azure AD B2C</span></span>

<span data-ttu-id="24fcd-136">API 使用新建立的 Azure AD B2C 租用戶中註冊[文件中的步驟](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-api)下方**註冊 web API**一節。</span><span class="sxs-lookup"><span data-stu-id="24fcd-136">In the newly created Azure AD B2C tenant, register your API using [the steps in the documentation](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-api) under the **Register a web API** section.</span></span>

<span data-ttu-id="24fcd-137">使用下列值：</span><span class="sxs-lookup"><span data-stu-id="24fcd-137">Use the following values:</span></span>

| <span data-ttu-id="24fcd-138">設定</span><span class="sxs-lookup"><span data-stu-id="24fcd-138">Setting</span></span>                       | <span data-ttu-id="24fcd-139">值</span><span class="sxs-lookup"><span data-stu-id="24fcd-139">Value</span></span>               | <span data-ttu-id="24fcd-140">注意</span><span class="sxs-lookup"><span data-stu-id="24fcd-140">Notes</span></span>                                                                                  |
|-------------------------------|---------------------|----------------------------------------------------------------------------------------|
| <span data-ttu-id="24fcd-141">**名稱**</span><span class="sxs-lookup"><span data-stu-id="24fcd-141">**Name**</span></span>                      | <span data-ttu-id="24fcd-142">*&lt;API 名稱&gt;*</span><span class="sxs-lookup"><span data-stu-id="24fcd-142">*&lt;API name&gt;*</span></span>  | <span data-ttu-id="24fcd-143">請輸入**名稱**描述取用者應用程式的應用程式。</span><span class="sxs-lookup"><span data-stu-id="24fcd-143">Enter a **Name** for the app that describes your app to consumers.</span></span>                     |
| <span data-ttu-id="24fcd-144">**包含 web 應用程式/web API**</span><span class="sxs-lookup"><span data-stu-id="24fcd-144">**Include web app / web API**</span></span> | <span data-ttu-id="24fcd-145">是</span><span class="sxs-lookup"><span data-stu-id="24fcd-145">Yes</span></span>                 |                                                                                        |
| <span data-ttu-id="24fcd-146">**允許隱含流程**</span><span class="sxs-lookup"><span data-stu-id="24fcd-146">**Allow implicit flow**</span></span>       | <span data-ttu-id="24fcd-147">是</span><span class="sxs-lookup"><span data-stu-id="24fcd-147">Yes</span></span>                 |                                                                                        |
| <span data-ttu-id="24fcd-148">**回覆 URL**</span><span class="sxs-lookup"><span data-stu-id="24fcd-148">**Reply URL**</span></span>                 | `https://localhost` | <span data-ttu-id="24fcd-149">回覆 Url 會是 Azure AD B2C 傳回您的應用程式要求之任何權杖的所在端點。</span><span class="sxs-lookup"><span data-stu-id="24fcd-149">Reply URLs are endpoints where Azure AD B2C returns any tokens that your app requests.</span></span> |
| <span data-ttu-id="24fcd-150">**應用程式識別碼 URI**</span><span class="sxs-lookup"><span data-stu-id="24fcd-150">**App ID URI**</span></span>                | <span data-ttu-id="24fcd-151">*api*</span><span class="sxs-lookup"><span data-stu-id="24fcd-151">*api*</span></span>               | <span data-ttu-id="24fcd-152">URI 不需要解析成實體地址。</span><span class="sxs-lookup"><span data-stu-id="24fcd-152">The URI doesn't need to resolve to a physical address.</span></span> <span data-ttu-id="24fcd-153">它只需要是唯一的。</span><span class="sxs-lookup"><span data-stu-id="24fcd-153">It only needs to be unique.</span></span>     |
| <span data-ttu-id="24fcd-154">**包含原生用戶端**</span><span class="sxs-lookup"><span data-stu-id="24fcd-154">**Include native client**</span></span>     | <span data-ttu-id="24fcd-155">否</span><span class="sxs-lookup"><span data-stu-id="24fcd-155">No</span></span>                  |                                                                                        |

<span data-ttu-id="24fcd-156">註冊 API 之後，會顯示租用戶中的應用程式和 Api 清單。</span><span class="sxs-lookup"><span data-stu-id="24fcd-156">After the API is registered, the list of apps and APIs in the tenant is displayed.</span></span> <span data-ttu-id="24fcd-157">選取剛剛已註冊的 API。</span><span class="sxs-lookup"><span data-stu-id="24fcd-157">Select the API that was just registered.</span></span> <span data-ttu-id="24fcd-158">選取**複製**右邊的圖示**APPLICATION-ID**欄位，以將它複製到剪貼簿。</span><span class="sxs-lookup"><span data-stu-id="24fcd-158">Select the **Copy** icon to the right of the **Application ID** field to copy it to the clipboard.</span></span> <span data-ttu-id="24fcd-159">選取 **發佈的範圍**，並確認預設*user_impersonation*範圍會存在。</span><span class="sxs-lookup"><span data-stu-id="24fcd-159">Select **Published scopes** and verify the default *user_impersonation* scope is present.</span></span>

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a><span data-ttu-id="24fcd-160">Visual Studio 2017 中建立 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="24fcd-160">Create an ASP.NET Core app in Visual Studio 2017</span></span>

<span data-ttu-id="24fcd-161">Visual Studio Web 應用程式範本可以設定要用於驗證的 Azure AD B2C 租用戶。</span><span class="sxs-lookup"><span data-stu-id="24fcd-161">The Visual Studio Web Application template can be configured to use the Azure AD B2C tenant for authentication.</span></span>

<span data-ttu-id="24fcd-162">在 Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="24fcd-162">In Visual Studio:</span></span>

1. <span data-ttu-id="24fcd-163">建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="24fcd-163">Create a new ASP.NET Core Web Application.</span></span> 
2. <span data-ttu-id="24fcd-164">選取  **Web API**從範本清單。</span><span class="sxs-lookup"><span data-stu-id="24fcd-164">Select **Web API** from the list of templates.</span></span>
3. <span data-ttu-id="24fcd-165">選取 [**變更驗證**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="24fcd-165">Select the **Change Authentication** button.</span></span>

    ![變更 [驗證] 按鈕](./azure-ad-b2c-webapi/change-auth-button.png)

4. <span data-ttu-id="24fcd-167">在**變更驗證**對話方塊中，選取**個別使用者帳戶**，然後選取**連接到雲端中現有的使用者存放區**下拉式清單中。</span><span class="sxs-lookup"><span data-stu-id="24fcd-167">In the **Change Authentication** dialog, select **Individual User Accounts**, and then select **Connect to an existing user store in the cloud** in the dropdown.</span></span> 

    ![變更驗證 對話方塊](./azure-ad-b2c-webapi/change-auth-dialog.png)

5. <span data-ttu-id="24fcd-169">完成表單，使用下列值：</span><span class="sxs-lookup"><span data-stu-id="24fcd-169">Complete the form with the following values:</span></span>

    | <span data-ttu-id="24fcd-170">設定</span><span class="sxs-lookup"><span data-stu-id="24fcd-170">Setting</span></span>                       | <span data-ttu-id="24fcd-171">值</span><span class="sxs-lookup"><span data-stu-id="24fcd-171">Value</span></span>                                                 |
    |-------------------------------|-------------------------------------------------------|
    | <span data-ttu-id="24fcd-172">**網域名稱**</span><span class="sxs-lookup"><span data-stu-id="24fcd-172">**Domain Name**</span></span>               | <span data-ttu-id="24fcd-173">*&lt;您的 B2C 租用戶網域名稱&gt;*</span><span class="sxs-lookup"><span data-stu-id="24fcd-173">*&lt;the domain name of your B2C tenant&gt;*</span></span>          |
    | <span data-ttu-id="24fcd-174">**應用程式識別碼**</span><span class="sxs-lookup"><span data-stu-id="24fcd-174">**Application ID**</span></span>            | <span data-ttu-id="24fcd-175">*&lt;貼上剪貼簿中的應用程式識別碼&gt;*</span><span class="sxs-lookup"><span data-stu-id="24fcd-175">*&lt;paste the Application ID from the clipboard&gt;*</span></span> |
    | <span data-ttu-id="24fcd-176">**註冊或登入原則**</span><span class="sxs-lookup"><span data-stu-id="24fcd-176">**Sign-up or sign-in policy**</span></span> | `B2C_1_SiUpIn`                                        |

    <span data-ttu-id="24fcd-177">選取  **確定**以關閉**變更驗證**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="24fcd-177">Select **OK** to close the **Change Authentication** dialog.</span></span> <span data-ttu-id="24fcd-178">選取 **確定**建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="24fcd-178">Select **OK** to create the web app.</span></span>

<span data-ttu-id="24fcd-179">Visual Studio 建立 web API，具有名為控制器*ValuesController.cs*傳回硬式編碼的 GET 要求的值。</span><span class="sxs-lookup"><span data-stu-id="24fcd-179">Visual Studio creates the web API with a controller named *ValuesController.cs* that returns hard-coded values for GET requests.</span></span> <span data-ttu-id="24fcd-180">類別以裝飾[Authorize 屬性](xref:security/authorization/simple)，因此所有要求均都需要驗證。</span><span class="sxs-lookup"><span data-stu-id="24fcd-180">The class is decorated with the [Authorize attribute](xref:security/authorization/simple), so all requests require authentication.</span></span>

## <a name="run-the-web-api"></a><span data-ttu-id="24fcd-181">執行 web API</span><span class="sxs-lookup"><span data-stu-id="24fcd-181">Run the web API</span></span>

<span data-ttu-id="24fcd-182">在 Visual Studio 中執行的 API。</span><span class="sxs-lookup"><span data-stu-id="24fcd-182">In Visual Studio, run the API.</span></span> <span data-ttu-id="24fcd-183">Visual Studio 會啟動瀏覽器指向 API 的根 URL。</span><span class="sxs-lookup"><span data-stu-id="24fcd-183">Visual Studio launches a browser pointed at the API's root URL.</span></span> <span data-ttu-id="24fcd-184">請注意網址列中的 URL，並將在背景中執行的 API。</span><span class="sxs-lookup"><span data-stu-id="24fcd-184">Note the URL in the address bar, and leave the API running in the background.</span></span>

> [!NOTE]
> <span data-ttu-id="24fcd-185">因為沒有定義的根 URL 的控制站時，瀏覽器可能會顯示 404 （找不到頁面） 錯誤。</span><span class="sxs-lookup"><span data-stu-id="24fcd-185">Since there is no controller defined for the root URL, the browser may display a 404 (page not found) error.</span></span> <span data-ttu-id="24fcd-186">這是正常的現象。</span><span class="sxs-lookup"><span data-stu-id="24fcd-186">This is expected behavior.</span></span>

## <a name="use-postman-to-get-a-token-and-test-the-api"></a><span data-ttu-id="24fcd-187">使用 Postman 來取得權杖，並測試 API</span><span class="sxs-lookup"><span data-stu-id="24fcd-187">Use Postman to get a token and test the API</span></span>

<span data-ttu-id="24fcd-188">[Postman](https://getpostman.com/postman)是一項工具測試 web Api。</span><span class="sxs-lookup"><span data-stu-id="24fcd-188">[Postman](https://getpostman.com/postman) is a tool for testing web APIs.</span></span> <span data-ttu-id="24fcd-189">本教學課程中，Postman 會模擬存取代表使用者的 web API 的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="24fcd-189">For this tutorial, Postman simulates a web app that accesses the web API on the user's behalf.</span></span>

### <a name="register-postman-as-a-web-app"></a><span data-ttu-id="24fcd-190">註冊為 web 應用程式的 Postman</span><span class="sxs-lookup"><span data-stu-id="24fcd-190">Register Postman as a web app</span></span>

<span data-ttu-id="24fcd-191">Postman 會模擬可以從 Azure AD B2C 租用戶取得權杖的 web 應用程式，因為它必須註冊租用戶中為 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="24fcd-191">Since Postman simulates a web app that can obtain tokens from the Azure AD B2C tenant, it must be registered in the tenant as a web app.</span></span> <span data-ttu-id="24fcd-192">註冊使用 Postman[文件中的步驟](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app)下方**註冊 web 應用程式**一節。</span><span class="sxs-lookup"><span data-stu-id="24fcd-192">Register Postman using [the steps in the documentation](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) under the **Register a web app** section.</span></span> <span data-ttu-id="24fcd-193">在停止**建立 web 應用程式用戶端祕密**一節。</span><span class="sxs-lookup"><span data-stu-id="24fcd-193">Stop at the **Create a web app client secret** section.</span></span> <span data-ttu-id="24fcd-194">本教學課程中，不需要用戶端祕密。</span><span class="sxs-lookup"><span data-stu-id="24fcd-194">A client secret isn't required for this tutorial.</span></span> 

<span data-ttu-id="24fcd-195">使用下列值：</span><span class="sxs-lookup"><span data-stu-id="24fcd-195">Use the following values:</span></span>

| <span data-ttu-id="24fcd-196">設定</span><span class="sxs-lookup"><span data-stu-id="24fcd-196">Setting</span></span>                       | <span data-ttu-id="24fcd-197">值</span><span class="sxs-lookup"><span data-stu-id="24fcd-197">Value</span></span>                            | <span data-ttu-id="24fcd-198">注意</span><span class="sxs-lookup"><span data-stu-id="24fcd-198">Notes</span></span>                           |
|-------------------------------|----------------------------------|---------------------------------|
| <span data-ttu-id="24fcd-199">**名稱**</span><span class="sxs-lookup"><span data-stu-id="24fcd-199">**Name**</span></span>                      | <span data-ttu-id="24fcd-200">Postman</span><span class="sxs-lookup"><span data-stu-id="24fcd-200">Postman</span></span>                          |                                 |
| <span data-ttu-id="24fcd-201">**包含 web 應用程式/web API**</span><span class="sxs-lookup"><span data-stu-id="24fcd-201">**Include web app / web API**</span></span> | <span data-ttu-id="24fcd-202">是</span><span class="sxs-lookup"><span data-stu-id="24fcd-202">Yes</span></span>                              |                                 |
| <span data-ttu-id="24fcd-203">**允許隱含流程**</span><span class="sxs-lookup"><span data-stu-id="24fcd-203">**Allow implicit flow**</span></span>       | <span data-ttu-id="24fcd-204">是</span><span class="sxs-lookup"><span data-stu-id="24fcd-204">Yes</span></span>                              |                                 |
| <span data-ttu-id="24fcd-205">**回覆 URL**</span><span class="sxs-lookup"><span data-stu-id="24fcd-205">**Reply URL**</span></span>                 | `https://getpostman.com/postman` |                                 |
| <span data-ttu-id="24fcd-206">**應用程式識別碼 URI**</span><span class="sxs-lookup"><span data-stu-id="24fcd-206">**App ID URI**</span></span>                | <span data-ttu-id="24fcd-207">*&lt;保留空白&gt;*</span><span class="sxs-lookup"><span data-stu-id="24fcd-207">*&lt;leave blank&gt;*</span></span>            | <span data-ttu-id="24fcd-208">本教學課程中，不需要。</span><span class="sxs-lookup"><span data-stu-id="24fcd-208">Not required for this tutorial.</span></span> |
| <span data-ttu-id="24fcd-209">**包含原生用戶端**</span><span class="sxs-lookup"><span data-stu-id="24fcd-209">**Include native client**</span></span>     | <span data-ttu-id="24fcd-210">否</span><span class="sxs-lookup"><span data-stu-id="24fcd-210">No</span></span>                               |                                 |

<span data-ttu-id="24fcd-211">新註冊的 web 應用程式需要存取 web API 上代表使用者的權限。</span><span class="sxs-lookup"><span data-stu-id="24fcd-211">The newly registered web app needs permission to access the web API on the user's behalf.</span></span>  

1. <span data-ttu-id="24fcd-212">選取  **Postman**在應用程式，然後選取這份**API 存取**從左側功能表上。</span><span class="sxs-lookup"><span data-stu-id="24fcd-212">Select **Postman** in the list of apps and then select **API access** from the menu on the left.</span></span>
2. <span data-ttu-id="24fcd-213">選取  **+ 新增**。</span><span class="sxs-lookup"><span data-stu-id="24fcd-213">Select **+ Add**.</span></span>
3. <span data-ttu-id="24fcd-214">在 **選取 API**下拉式清單中，選取 web API 的名稱。</span><span class="sxs-lookup"><span data-stu-id="24fcd-214">In the **Select API** dropdown, select the name of the web API.</span></span>
4. <span data-ttu-id="24fcd-215">在 **選取範圍**下拉式清單中，確定已選取 所有範圍。</span><span class="sxs-lookup"><span data-stu-id="24fcd-215">In the **Select Scopes** dropdown, ensure all scopes are selected.</span></span>
5. <span data-ttu-id="24fcd-216">選取  **Ok**。</span><span class="sxs-lookup"><span data-stu-id="24fcd-216">Select **Ok**.</span></span>

<span data-ttu-id="24fcd-217">請注意 Postman 應用程式的應用程式識別碼，因為它需要取得持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="24fcd-217">Note the Postman app's Application ID, as it's required to obtain a bearer token.</span></span>

### <a name="create-a-postman-request"></a><span data-ttu-id="24fcd-218">建立 Postman 要求</span><span class="sxs-lookup"><span data-stu-id="24fcd-218">Create a Postman request</span></span>

<span data-ttu-id="24fcd-219">啟動 Postman。</span><span class="sxs-lookup"><span data-stu-id="24fcd-219">Launch Postman.</span></span> <span data-ttu-id="24fcd-220">根據預設，顯示 Postman**新建**時啟動的對話方塊。</span><span class="sxs-lookup"><span data-stu-id="24fcd-220">By default, Postman displays the **Create New** dialog upon launching.</span></span> <span data-ttu-id="24fcd-221">如果未顯示 [] 對話方塊中，選取 **+ 新增**左上角的按鈕。</span><span class="sxs-lookup"><span data-stu-id="24fcd-221">If the dialog isn't displayed, select the **+ New** button in the upper left.</span></span>

<span data-ttu-id="24fcd-222">從**新建**對話方塊：</span><span class="sxs-lookup"><span data-stu-id="24fcd-222">From the **Create New** dialog:</span></span>

1. <span data-ttu-id="24fcd-223">選取 **要求**。</span><span class="sxs-lookup"><span data-stu-id="24fcd-223">Select **Request**.</span></span>

    ![要求按鈕](./azure-ad-b2c-webapi/postman-create-new.png)

2. <span data-ttu-id="24fcd-225">請輸入*取得的值*中**要求名稱** 方塊中。</span><span class="sxs-lookup"><span data-stu-id="24fcd-225">Enter *Get Values* in the **Request name** box.</span></span>
3. <span data-ttu-id="24fcd-226">選取  **+ 建立集合**來建立新的集合，以便儲存要求。</span><span class="sxs-lookup"><span data-stu-id="24fcd-226">Select **+ Create Collection** to create a new collection for storing the request.</span></span> <span data-ttu-id="24fcd-227">替集合命名*ASP.NET Core 教學課程*，然後選取核取記號。</span><span class="sxs-lookup"><span data-stu-id="24fcd-227">Name the collection *ASP.NET Core tutorials* and then select the checkmark.</span></span>

    ![建立新的集合](./azure-ad-b2c-webapi/postman-create-collection.png)

4. <span data-ttu-id="24fcd-229">選取 [**儲存至 ASP.NET Core 教學課程**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="24fcd-229">Select the **Save to ASP.NET Core tutorials** button.</span></span>

### <a name="test-the-web-api-without-authentication"></a><span data-ttu-id="24fcd-230">測試 web API，而不需要驗證</span><span class="sxs-lookup"><span data-stu-id="24fcd-230">Test the web API without authentication</span></span>

<span data-ttu-id="24fcd-231">若要確認 web API 需要驗證，首先請未經驗證的要求。</span><span class="sxs-lookup"><span data-stu-id="24fcd-231">To verify that the web API requires authentication, first make a request without authentication.</span></span>

1. <span data-ttu-id="24fcd-232">在 **輸入要求 URL**方塊中，輸入的 URL `ValuesController`。</span><span class="sxs-lookup"><span data-stu-id="24fcd-232">In the **Enter request URL** box, enter the URL for `ValuesController`.</span></span> <span data-ttu-id="24fcd-233">URL 會與顯示在瀏覽器中使用的相同**api/值**附加。</span><span class="sxs-lookup"><span data-stu-id="24fcd-233">The URL is the same as displayed in the browser with **api/values** appended.</span></span> <span data-ttu-id="24fcd-234">例如`https://localhost:44375/api/values`。</span><span class="sxs-lookup"><span data-stu-id="24fcd-234">An example would be `https://localhost:44375/api/values`.</span></span>
2. <span data-ttu-id="24fcd-235">選取 [**傳送**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="24fcd-235">Select the **Send** button.</span></span>
3. <span data-ttu-id="24fcd-236">請注意回應的狀態是*401 未授權*。</span><span class="sxs-lookup"><span data-stu-id="24fcd-236">Note the status of the response is *401 Unauthorized*.</span></span>

    ![401 未授權的回應](./azure-ad-b2c-webapi/postman-401-status.png)

> [!IMPORTANT]
> <span data-ttu-id="24fcd-238">如果您收到 「 無法取得任何回應 」 的錯誤，您可能需要停用中的 SSL 憑證驗證[Postman 設定](https://learning.getpostman.com/docs/postman/launching_postman/settings)。</span><span class="sxs-lookup"><span data-stu-id="24fcd-238">If you get a "Could not get any response" error, you may need to disable SSL certificate verification in the [Postman settings](https://learning.getpostman.com/docs/postman/launching_postman/settings).</span></span> 
 
### <a name="obtain-a-bearer-token"></a><span data-ttu-id="24fcd-239">取得持有人權杖</span><span class="sxs-lookup"><span data-stu-id="24fcd-239">Obtain a bearer token</span></span>

<span data-ttu-id="24fcd-240">若要讓 web API 已驗證的要求，持有人權杖是必要的。</span><span class="sxs-lookup"><span data-stu-id="24fcd-240">To make an authenticated request to the web API, a bearer token is required.</span></span> <span data-ttu-id="24fcd-241">Postman 可方便在登入 Azure AD B2C 租用戶，並取得權杖。</span><span class="sxs-lookup"><span data-stu-id="24fcd-241">Postman makes it easy to sign in to the Azure AD B2C tenant and obtain a token.</span></span>

1. <span data-ttu-id="24fcd-242">在 **授權**索引標籤中，於**型別**下拉式清單中，選取**OAuth 2.0**。</span><span class="sxs-lookup"><span data-stu-id="24fcd-242">On the **Authorization** tab, in the **TYPE** dropdown, select **OAuth 2.0**.</span></span> <span data-ttu-id="24fcd-243">在 **授權將資料加入至**下拉式清單中，選取**要求標頭**。</span><span class="sxs-lookup"><span data-stu-id="24fcd-243">In the **Add authorization data to** dropdown, select **Request Headers**.</span></span> <span data-ttu-id="24fcd-244">選取 **取得新存取權杖**。</span><span class="sxs-lookup"><span data-stu-id="24fcd-244">Select **Get New Access Token**.</span></span>

    ![設定 [授權] 索引標籤](./azure-ad-b2c-webapi/postman-auth-tab.png)

2. <span data-ttu-id="24fcd-246">完成**取得新存取權杖**，如下所示的對話方塊：</span><span class="sxs-lookup"><span data-stu-id="24fcd-246">Complete the **GET NEW ACCESS TOKEN** dialog as follows:</span></span>


   |                <span data-ttu-id="24fcd-247">設定</span><span class="sxs-lookup"><span data-stu-id="24fcd-247">Setting</span></span>                 |                                             <span data-ttu-id="24fcd-248">值</span><span class="sxs-lookup"><span data-stu-id="24fcd-248">Value</span></span>                                             |                                                                                                                                    <span data-ttu-id="24fcd-249">注意</span><span class="sxs-lookup"><span data-stu-id="24fcd-249">Notes</span></span>                                                                                                                                     |
   |----------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |      <span data-ttu-id="24fcd-250"><strong>權杖名稱</strong></span><span class="sxs-lookup"><span data-stu-id="24fcd-250"><strong>Token Name</strong></span></span>       |                                  <span data-ttu-id="24fcd-251"><em>&lt;權杖名稱&gt;</em></span><span class="sxs-lookup"><span data-stu-id="24fcd-251"><em>&lt;token name&gt;</em></span></span>                                  |                                                                                                                   <span data-ttu-id="24fcd-252">輸入權杖的描述性名稱。</span><span class="sxs-lookup"><span data-stu-id="24fcd-252">Enter a descriptive name for the token.</span></span>                                                                                                                    |
   |      <span data-ttu-id="24fcd-253"><strong>授與類型</strong></span><span class="sxs-lookup"><span data-stu-id="24fcd-253"><strong>Grant Type</strong></span></span>       |                                           <span data-ttu-id="24fcd-254">隱含</span><span class="sxs-lookup"><span data-stu-id="24fcd-254">Implicit</span></span>                                            |                                                                                                                                                                                                                                                                              |
   |     <span data-ttu-id="24fcd-255"><strong>回呼 URL</strong></span><span class="sxs-lookup"><span data-stu-id="24fcd-255"><strong>Callback URL</strong></span></span>      |                               `https://getpostman.com/postman`                                |                                                                                                                                                                                                                                                                              |
   |       <span data-ttu-id="24fcd-256"><strong>驗證 URL</strong></span><span class="sxs-lookup"><span data-stu-id="24fcd-256"><strong>Auth URL</strong></span></span>        | `https://login.microsoftonline.com/tfp/<tenant domain name>/B2C_1_SiUpIn/oauth2/v2.0/authorize` |                                                                                                  <span data-ttu-id="24fcd-257">取代<em>&lt;租用戶網域名稱&gt;</em>租用戶的網域名稱。</span><span class="sxs-lookup"><span data-stu-id="24fcd-257">Replace <em>&lt;tenant domain name&gt;</em> with the tenant's domain name.</span></span>                                                                                                  |
   |       <span data-ttu-id="24fcd-258"><strong>用戶端識別碼</strong></span><span class="sxs-lookup"><span data-stu-id="24fcd-258"><strong>Client ID</strong></span></span>       |                <span data-ttu-id="24fcd-259"><em>&lt;輸入 Postman 應用程式的<b>應用程式識別碼</b>&gt;</em></span><span class="sxs-lookup"><span data-stu-id="24fcd-259"><em>&lt;enter the Postman app's <b>Application ID</b>&gt;</em></span></span>                 |                                                                                                                                                                                                                                                                              |
   |         <span data-ttu-id="24fcd-260"><strong>範圍</strong></span><span class="sxs-lookup"><span data-stu-id="24fcd-260"><strong>Scope</strong></span></span>         |         `https://<tenant domain name>/<api>/user_impersonation openid offline_access`         | <span data-ttu-id="24fcd-261">取代<em>&lt;租用戶網域名稱&gt;</em>租用戶的網域名稱。</span><span class="sxs-lookup"><span data-stu-id="24fcd-261">Replace <em>&lt;tenant domain name&gt;</em> with the tenant's domain name.</span></span> <span data-ttu-id="24fcd-262">取代<em>&lt;api&gt;</em>應用程式識別碼 URI 與您提供給 web API 第一次登錄時 (在此情況下， `api`)。</span><span class="sxs-lookup"><span data-stu-id="24fcd-262">Replace <em>&lt;api&gt;</em> with the App ID URI you gave the web API when you first registered it (in this case, `api`).</span></span> <span data-ttu-id="24fcd-263">URL 的模式是： <em>https://{tenant}.onmicrosoft.com/{api-id-uri}/{scope 名稱}</em>。</span><span class="sxs-lookup"><span data-stu-id="24fcd-263">The pattern for the URL is: <em>https://{tenant}.onmicrosoft.com/{api-id-uri}/{scope name}</em>.</span></span> |
   |         <span data-ttu-id="24fcd-264"><strong>狀態</strong></span><span class="sxs-lookup"><span data-stu-id="24fcd-264"><strong>State</strong></span></span>         |                                 <span data-ttu-id="24fcd-265"><em>&lt;保留空白&gt;</em></span><span class="sxs-lookup"><span data-stu-id="24fcd-265"><em>&lt;leave blank&gt;</em></span></span>                                  |                                                                                                                                                                                                                                                                              |
   | <span data-ttu-id="24fcd-266"><strong>用戶端驗證</strong></span><span class="sxs-lookup"><span data-stu-id="24fcd-266"><strong>Client Authentication</strong></span></span> |                                <span data-ttu-id="24fcd-267">本文中傳送用戶端認證</span><span class="sxs-lookup"><span data-stu-id="24fcd-267">Send client credentials in body</span></span>                                |                                                                                                                                                                                                                                                                              |


3. <span data-ttu-id="24fcd-268">選取 [**要求權杖**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="24fcd-268">Select the **Request Token** button.</span></span>

4. <span data-ttu-id="24fcd-269">Postman 會開啟新視窗，其中包含 Azure AD B2C 租用戶的登入對話方塊。</span><span class="sxs-lookup"><span data-stu-id="24fcd-269">Postman opens a new window containing the Azure AD B2C tenant's sign in dialog.</span></span> <span data-ttu-id="24fcd-270">（如果已建立一個測試原則），使用現有的帳戶登入，或選取**立即註冊**建立新的帳戶。</span><span class="sxs-lookup"><span data-stu-id="24fcd-270">Sign in with an existing account (if one was created testing the policies) or select **Sign up now** to create a new account.</span></span> <span data-ttu-id="24fcd-271">**忘記密碼？** 連結用來重設遺忘的密碼。</span><span class="sxs-lookup"><span data-stu-id="24fcd-271">The **Forgot your password?** link is used to reset a forgotten password.</span></span>

5. <span data-ttu-id="24fcd-272">在視窗關閉後成功登入，而**管理存取權杖**對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="24fcd-272">After successfully signing in, the window closes and the **MANAGE ACCESS TOKENS** dialog appears.</span></span> <span data-ttu-id="24fcd-273">捲動到底部，然後選取**使用語彙基元** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="24fcd-273">Scroll down to the bottom and select the **Use Token** button.</span></span>

    ![哪裡可以找到 「 使用權杖 」 的按鈕](./azure-ad-b2c-webapi/postman-access-token.png)

### <a name="test-the-web-api-with-authentication"></a><span data-ttu-id="24fcd-275">測試 web API 驗證</span><span class="sxs-lookup"><span data-stu-id="24fcd-275">Test the web API with authentication</span></span>

<span data-ttu-id="24fcd-276">選取 [**傳送**再次傳送要求] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="24fcd-276">Select the **Send** button to send the request again.</span></span> <span data-ttu-id="24fcd-277">目前回應的狀態會*200 確定*JSON 承載，都可以看到回應**主體** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="24fcd-277">This time, the response status is *200 OK* and the JSON payload is visible on the response **Body** tab.</span></span>

![裝載和成功狀態](./azure-ad-b2c-webapi/postman-success.png)

## <a name="next-steps"></a><span data-ttu-id="24fcd-279">後續步驟</span><span class="sxs-lookup"><span data-stu-id="24fcd-279">Next steps</span></span>

<span data-ttu-id="24fcd-280">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="24fcd-280">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="24fcd-281">建立 Azure Active Directory B2C 租用戶。</span><span class="sxs-lookup"><span data-stu-id="24fcd-281">Create an Azure Active Directory B2C tenant.</span></span>
> * <span data-ttu-id="24fcd-282">在 Azure AD B2C 中註冊 Web API。</span><span class="sxs-lookup"><span data-stu-id="24fcd-282">Register a Web API in Azure AD B2C.</span></span>
> * <span data-ttu-id="24fcd-283">使用 Visual Studio 建立 Web API 設定為使用 Azure AD B2C 租用戶進行驗證。</span><span class="sxs-lookup"><span data-stu-id="24fcd-283">Use Visual Studio to create a Web API configured to use the Azure AD B2C tenant for authentication.</span></span>
> * <span data-ttu-id="24fcd-284">設定控制的 Azure AD B2C 租用戶行為的原則。</span><span class="sxs-lookup"><span data-stu-id="24fcd-284">Configure policies controlling the behavior of the Azure AD B2C tenant.</span></span>
> * <span data-ttu-id="24fcd-285">使用 Postman 來模擬 web 應用程式會顯示登入對話方塊中，擷取權杖，並用它來對 web API 提出要求。</span><span class="sxs-lookup"><span data-stu-id="24fcd-285">Use Postman to simulate a web app which presents a login dialog, retrieves a token, and uses it to make a request against the web API.</span></span>

<span data-ttu-id="24fcd-286">繼續開發您的 API，以學習：</span><span class="sxs-lookup"><span data-stu-id="24fcd-286">Continue developing your API by learning to:</span></span>

* <span data-ttu-id="24fcd-287">[保護 ASP.NET Core web 應用程式使用 Azure AD B2C](xref:security/authentication/azure-ad-b2c)。</span><span class="sxs-lookup"><span data-stu-id="24fcd-287">[Secure an ASP.NET Core web app using Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="24fcd-288">[呼叫.NET web API，從.NET web 應用程式使用 Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet)。</span><span class="sxs-lookup"><span data-stu-id="24fcd-288">[Call a .NET web API from a .NET web app using Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).</span></span>
* <span data-ttu-id="24fcd-289">[自訂 Azure AD B2C 使用者介面](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization)。</span><span class="sxs-lookup"><span data-stu-id="24fcd-289">[Customize the Azure AD B2C user interface](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).</span></span>
* <span data-ttu-id="24fcd-290">[設定密碼複雜度需求](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity)。</span><span class="sxs-lookup"><span data-stu-id="24fcd-290">[Configure password complexity requirements](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).</span></span>
* <span data-ttu-id="24fcd-291">[啟用 multi-factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa)。</span><span class="sxs-lookup"><span data-stu-id="24fcd-291">[Enable multi-factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).</span></span>
* <span data-ttu-id="24fcd-292">設定額外的身分識別提供者，例如[Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app)， [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app)， [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app)， [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app)， [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)，和其他人。</span><span class="sxs-lookup"><span data-stu-id="24fcd-292">Configure additional identity providers, such as [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app), and others.</span></span>
* <span data-ttu-id="24fcd-293">[使用 Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet)擷取其他使用者資訊，例如群組成員資格，從 Azure AD B2C 租用戶。</span><span class="sxs-lookup"><span data-stu-id="24fcd-293">[Use the Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) to retrieve additional user information, such as group membership, from the Azure AD B2C tenant.</span></span>
