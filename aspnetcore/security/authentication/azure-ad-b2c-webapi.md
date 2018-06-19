---
title: 在 web 應用程式開發介面與 Azure Active Directory B2C 中 ASP.NET Core 雲端驗證
author: camsoper
description: 了解如何設定 Azure Active Directory B2C 驗證與 ASP.NET Core Web API。 測試已驗證的 web 應用程式開發介面與郵差。
ms.author: casoper
manager: wpickett
ms.date: 01/25/2018
ms.topic: tutorial
ms.technology: aspnet
ms.prod: asp.net-core
ms.custom: mvc
uid: security/authentication/azure-ad-b2c-webapi
ms.openlocfilehash: 621290f7e303f9157577b5c1b32646b750ed5159
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897800"
---
# <a name="cloud-authentication-in-web-apis-with-azure-active-directory-b2c-in-aspnet-core"></a><span data-ttu-id="b72e7-104">在 web 應用程式開發介面與 Azure Active Directory B2C 中 ASP.NET Core 雲端驗證</span><span class="sxs-lookup"><span data-stu-id="b72e7-104">Cloud authentication in web APIs with Azure Active Directory B2C in ASP.NET Core</span></span>

<span data-ttu-id="b72e7-105">作者 [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="b72e7-105">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="b72e7-106">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) 是 web 和行動裝置應用程式的雲端身分識別管理解決方案。</span><span class="sxs-lookup"><span data-stu-id="b72e7-106">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) is a cloud identity management solution for web and mobile apps.</span></span> <span data-ttu-id="b72e7-107">服務提供裝載於雲端和內部部署的應用程式的驗證。</span><span class="sxs-lookup"><span data-stu-id="b72e7-107">The service provides authentication for apps hosted in the cloud and on-premises.</span></span> <span data-ttu-id="b72e7-108">驗證類型包括個別帳戶，社交網路帳戶，以及同盟企業帳戶。</span><span class="sxs-lookup"><span data-stu-id="b72e7-108">Authentication types include individual accounts, social network accounts, and federated enterprise accounts.</span></span> <span data-ttu-id="b72e7-109">此外，Azure AD B2C 可提供多重要素驗證，以最低組態。</span><span class="sxs-lookup"><span data-stu-id="b72e7-109">Additionally, Azure AD B2C can provide multi-factor authentication with minimal configuration.</span></span>

> [!TIP]
> <span data-ttu-id="b72e7-110">Azure Active Directory (Azure AD) 和 Azure AD B2C 會個別產品的供應項目。</span><span class="sxs-lookup"><span data-stu-id="b72e7-110">Azure Active Directory (Azure AD) and Azure AD B2C are separate product offerings.</span></span> <span data-ttu-id="b72e7-111">Azure AD 租用戶代表組織中，而 Azure AD B2C 租用戶代表與信賴憑證者的合作對象應用程式使用的身分識別的集合。</span><span class="sxs-lookup"><span data-stu-id="b72e7-111">An Azure AD tenant represents an organization, while an Azure AD B2C tenant represents a collection of identities to be used with relying party applications.</span></span> <span data-ttu-id="b72e7-112">若要進一步了解，請參閱[Azure AD B2C： 常見問題集 (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs)。</span><span class="sxs-lookup"><span data-stu-id="b72e7-112">To learn more, see [Azure AD B2C: Frequently asked questions (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).</span></span>

<span data-ttu-id="b72e7-113">Web 應用程式開發介面不會有使用者介面，因為它們無法將使用者重新導向至 Azure AD B2C 這類安全權杖服務。</span><span class="sxs-lookup"><span data-stu-id="b72e7-113">Since web APIs have no user interface, they're unable to redirect the user to a secure token service like Azure AD B2C.</span></span> <span data-ttu-id="b72e7-114">相反地，API 會從呼叫的應用程式，這已經過驗證的使用者的 Azure AD B2C 傳遞持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="b72e7-114">Instead, the API is passed a bearer token from the calling app, which has already authenticated the user with Azure AD B2C.</span></span> <span data-ttu-id="b72e7-115">接著，應用程式開發介面來驗證沒有直接的使用者互動的語彙基元。</span><span class="sxs-lookup"><span data-stu-id="b72e7-115">The API then validates the token without direct user interaction.</span></span>

<span data-ttu-id="b72e7-116">在本教學課程，了解如何：</span><span class="sxs-lookup"><span data-stu-id="b72e7-116">In this tutorial, learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b72e7-117">建立 Azure Active Directory B2C 租用戶。</span><span class="sxs-lookup"><span data-stu-id="b72e7-117">Create an Azure Active Directory B2C tenant.</span></span>
> * <span data-ttu-id="b72e7-118">在 Azure AD B2C 中註冊 Web 應用程式開發介面。</span><span class="sxs-lookup"><span data-stu-id="b72e7-118">Register a Web API in Azure AD B2C.</span></span>
> * <span data-ttu-id="b72e7-119">您可以使用 Visual Studio 來建立 Web API 設定要用於驗證的 Azure AD B2C 租用戶。</span><span class="sxs-lookup"><span data-stu-id="b72e7-119">Use Visual Studio to create a Web API configured to use the Azure AD B2C tenant for authentication.</span></span>
> * <span data-ttu-id="b72e7-120">設定控制行為的 Azure AD B2C 租用戶的原則。</span><span class="sxs-lookup"><span data-stu-id="b72e7-120">Configure policies controlling the behavior of the Azure AD B2C tenant.</span></span>
> * <span data-ttu-id="b72e7-121">使用郵差來模擬 web 應用程式會顯示登入 對話方塊中，擷取語彙基元，並用它來對 web API 提出要求。</span><span class="sxs-lookup"><span data-stu-id="b72e7-121">Use Postman to simulate a web app which presents a login dialog, retrieves a token, and uses it to make a request against the web API.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b72e7-122">必要條件</span><span class="sxs-lookup"><span data-stu-id="b72e7-122">Prerequisites</span></span>

<span data-ttu-id="b72e7-123">此逐步解說需要下列條件：</span><span class="sxs-lookup"><span data-stu-id="b72e7-123">The following are required for this walkthrough:</span></span>

* [<span data-ttu-id="b72e7-124">Microsoft Azure 訂用帳戶</span><span class="sxs-lookup"><span data-stu-id="b72e7-124">Microsoft Azure subscription</span></span>](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* <span data-ttu-id="b72e7-125">[Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) （任何版本）</span><span class="sxs-lookup"><span data-stu-id="b72e7-125">[Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (any edition)</span></span>
* [<span data-ttu-id="b72e7-126">Postman</span><span class="sxs-lookup"><span data-stu-id="b72e7-126">Postman</span></span>](https://www.getpostman.com/postman)

## <a name="create-the-azure-active-directory-b2c-tenant"></a><span data-ttu-id="b72e7-127">建立 Azure Active Directory B2C 租用戶</span><span class="sxs-lookup"><span data-stu-id="b72e7-127">Create the Azure Active Directory B2C tenant</span></span>

<span data-ttu-id="b72e7-128">建立 Azure AD B2C 租用戶[文件中所述](/azure/active-directory-b2c/active-directory-b2c-get-started)。</span><span class="sxs-lookup"><span data-stu-id="b72e7-128">Create an Azure AD B2C tenant [as described in the documentation](/azure/active-directory-b2c/active-directory-b2c-get-started).</span></span> <span data-ttu-id="b72e7-129">出現提示時，將租用戶與 Azure 訂用帳戶產生關聯是選擇性的在本教學課程。</span><span class="sxs-lookup"><span data-stu-id="b72e7-129">When prompted, associating the tenant with an Azure subscription is optional for this tutorial.</span></span>

## <a name="configure-a-sign-up-or-sign-in-policy"></a><span data-ttu-id="b72e7-130">設定註冊或登入的原則</span><span class="sxs-lookup"><span data-stu-id="b72e7-130">Configure a sign-up or sign-in policy</span></span>

<span data-ttu-id="b72e7-131">使用 Azure AD B2C 文件中的步驟[建立註冊或登入的原則](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy)。</span><span class="sxs-lookup"><span data-stu-id="b72e7-131">Use the steps in the Azure AD B2C documentation to [create a sign-up or sign-in policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy).</span></span> <span data-ttu-id="b72e7-132">命名原則**SiUpIn**。</span><span class="sxs-lookup"><span data-stu-id="b72e7-132">Name the policy **SiUpIn**.</span></span>  <span data-ttu-id="b72e7-133">使用提供的文件中的範例值**身分識別提供者**，**註冊屬性**，和**應用程式宣告**。</span><span class="sxs-lookup"><span data-stu-id="b72e7-133">Use the example values provided in the documentation for **Identity providers**, **Sign-up attributes**, and **Application claims**.</span></span> <span data-ttu-id="b72e7-134">使用**立即執行**是選擇性的按鈕來測試原則文件中所述。</span><span class="sxs-lookup"><span data-stu-id="b72e7-134">Using the **Run now** button to test the policy as described in the documentation is optional.</span></span>

## <a name="register-the-api-in-azure-ad-b2c"></a><span data-ttu-id="b72e7-135">在 Azure AD B2C 中註冊應用程式開發介面</span><span class="sxs-lookup"><span data-stu-id="b72e7-135">Register the API in Azure AD B2C</span></span>

<span data-ttu-id="b72e7-136">在新建立的 Azure AD B2C 租用戶註冊您的應用程式開發介面使用[文件中的步驟](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-api)下**註冊 web 應用程式開發介面**> 一節。</span><span class="sxs-lookup"><span data-stu-id="b72e7-136">In the newly created Azure AD B2C tenant, register your API using [the steps in the documentation](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-api) under the **Register a web API** section.</span></span>

<span data-ttu-id="b72e7-137">使用下列值：</span><span class="sxs-lookup"><span data-stu-id="b72e7-137">Use the following values:</span></span>

| <span data-ttu-id="b72e7-138">設定</span><span class="sxs-lookup"><span data-stu-id="b72e7-138">Setting</span></span>                       | <span data-ttu-id="b72e7-139">值</span><span class="sxs-lookup"><span data-stu-id="b72e7-139">Value</span></span>               | <span data-ttu-id="b72e7-140">注意</span><span class="sxs-lookup"><span data-stu-id="b72e7-140">Notes</span></span>                                                                                  |
|-------------------------------|---------------------|----------------------------------------------------------------------------------------|
| <span data-ttu-id="b72e7-141">**名稱**</span><span class="sxs-lookup"><span data-stu-id="b72e7-141">**Name**</span></span>                      | <span data-ttu-id="b72e7-142">*&lt;API 名稱&gt;*</span><span class="sxs-lookup"><span data-stu-id="b72e7-142">*&lt;API name&gt;*</span></span>  | <span data-ttu-id="b72e7-143">輸入**名稱**描述給取用者應用程式的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b72e7-143">Enter a **Name** for the app that describes your app to consumers.</span></span>                     |
| <span data-ttu-id="b72e7-144">**包含 web 應用程式/web 應用程式開發介面**</span><span class="sxs-lookup"><span data-stu-id="b72e7-144">**Include web app / web API**</span></span> | <span data-ttu-id="b72e7-145">[是]</span><span class="sxs-lookup"><span data-stu-id="b72e7-145">Yes</span></span>                 |                                                                                        |
| <span data-ttu-id="b72e7-146">**允許隱含流程**</span><span class="sxs-lookup"><span data-stu-id="b72e7-146">**Allow implicit flow**</span></span>       | <span data-ttu-id="b72e7-147">[是]</span><span class="sxs-lookup"><span data-stu-id="b72e7-147">Yes</span></span>                 |                                                                                        |
| <span data-ttu-id="b72e7-148">**回覆 URL**</span><span class="sxs-lookup"><span data-stu-id="b72e7-148">**Reply URL**</span></span>                 | `https://localhost` | <span data-ttu-id="b72e7-149">回覆 Url 是端點，其中是 Azure AD B2C 會傳回任何您的應用程式要求的權杖。</span><span class="sxs-lookup"><span data-stu-id="b72e7-149">Reply URLs are endpoints where Azure AD B2C returns any tokens that your app requests.</span></span> |
| <span data-ttu-id="b72e7-150">**應用程式識別碼 URI**</span><span class="sxs-lookup"><span data-stu-id="b72e7-150">**App ID URI**</span></span>                | <span data-ttu-id="b72e7-151">*api*</span><span class="sxs-lookup"><span data-stu-id="b72e7-151">*api*</span></span>               | <span data-ttu-id="b72e7-152">URI 不需要解析的實體位址。</span><span class="sxs-lookup"><span data-stu-id="b72e7-152">The URI doesn't need to resolve to a physical address.</span></span> <span data-ttu-id="b72e7-153">它只需要是唯一的。</span><span class="sxs-lookup"><span data-stu-id="b72e7-153">It only needs to be unique.</span></span>     |
| <span data-ttu-id="b72e7-154">**包含原生用戶端**</span><span class="sxs-lookup"><span data-stu-id="b72e7-154">**Include native client**</span></span>     | <span data-ttu-id="b72e7-155">否</span><span class="sxs-lookup"><span data-stu-id="b72e7-155">No</span></span>                  |                                                                                        |

<span data-ttu-id="b72e7-156">註冊應用程式開發介面之後，會顯示租用戶中的應用程式和應用程式開發介面清單。</span><span class="sxs-lookup"><span data-stu-id="b72e7-156">After the API is registered, the list of apps and APIs in the tenant is displayed.</span></span> <span data-ttu-id="b72e7-157">選取剛才已註冊的 API。</span><span class="sxs-lookup"><span data-stu-id="b72e7-157">Select the API that was just registered.</span></span> <span data-ttu-id="b72e7-158">選取**複製**圖示右邊的**應用程式識別碼**欄位，以將它複製到剪貼簿。</span><span class="sxs-lookup"><span data-stu-id="b72e7-158">Select the **Copy** icon to the right of the **Application ID** field to copy it to the clipboard.</span></span> <span data-ttu-id="b72e7-159">選取**發行領域**並確認預設*user_impersonation*範圍會存在。</span><span class="sxs-lookup"><span data-stu-id="b72e7-159">Select **Published scopes** and verify the default *user_impersonation* scope is present.</span></span>

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a><span data-ttu-id="b72e7-160">在 Visual Studio 2017 中建立 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="b72e7-160">Create an ASP.NET Core app in Visual Studio 2017</span></span>

<span data-ttu-id="b72e7-161">Visual Studio Web 應用程式範本可以設定為使用 Azure AD B2C 租用戶進行驗證。</span><span class="sxs-lookup"><span data-stu-id="b72e7-161">The Visual Studio Web Application template can be configured to use the Azure AD B2C tenant for authentication.</span></span>

<span data-ttu-id="b72e7-162">在 Visual Studio 中：</span><span class="sxs-lookup"><span data-stu-id="b72e7-162">In Visual Studio:</span></span>

1. <span data-ttu-id="b72e7-163">建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b72e7-163">Create a new ASP.NET Core Web Application.</span></span> 
2. <span data-ttu-id="b72e7-164">選取**Web API**從範本清單。</span><span class="sxs-lookup"><span data-stu-id="b72e7-164">Select **Web API** from the list of templates.</span></span>
3. <span data-ttu-id="b72e7-165">選取**變更驗證** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b72e7-165">Select the **Change Authentication** button.</span></span>

    ![變更 [驗證] 按鈕](./azure-ad-b2c-webapi/change-auth-button.png)

4. <span data-ttu-id="b72e7-167">在**變更驗證**對話方塊中，選取**個別使用者帳戶**，然後選取**連接到雲端中現有的使用者存放區**下拉式清單中。</span><span class="sxs-lookup"><span data-stu-id="b72e7-167">In the **Change Authentication** dialog, select **Individual User Accounts**, and then select **Connect to an existing user store in the cloud** in the dropdown.</span></span> 

    ![變更 [驗證] 對話方塊](./azure-ad-b2c-webapi/change-auth-dialog.png)

5. <span data-ttu-id="b72e7-169">完成表單具有下列值：</span><span class="sxs-lookup"><span data-stu-id="b72e7-169">Complete the form with the following values:</span></span>

    | <span data-ttu-id="b72e7-170">設定</span><span class="sxs-lookup"><span data-stu-id="b72e7-170">Setting</span></span>                       | <span data-ttu-id="b72e7-171">值</span><span class="sxs-lookup"><span data-stu-id="b72e7-171">Value</span></span>                                                 |
    |-------------------------------|-------------------------------------------------------|
    | <span data-ttu-id="b72e7-172">**網域名稱**</span><span class="sxs-lookup"><span data-stu-id="b72e7-172">**Domain Name**</span></span>               | <span data-ttu-id="b72e7-173">*&lt;B2C 租用戶的網域名稱&gt;*</span><span class="sxs-lookup"><span data-stu-id="b72e7-173">*&lt;the domain name of your B2C tenant&gt;*</span></span>          |
    | <span data-ttu-id="b72e7-174">**應用程式識別碼**</span><span class="sxs-lookup"><span data-stu-id="b72e7-174">**Application ID**</span></span>            | <span data-ttu-id="b72e7-175">*&lt;貼上剪貼簿中的應用程式識別碼&gt;*</span><span class="sxs-lookup"><span data-stu-id="b72e7-175">*&lt;paste the Application ID from the clipboard&gt;*</span></span> |
    | <span data-ttu-id="b72e7-176">**註冊或登入的原則**</span><span class="sxs-lookup"><span data-stu-id="b72e7-176">**Sign-up or sign-in policy**</span></span> | `B2C_1_SiUpIn`                                        |

    <span data-ttu-id="b72e7-177">選取**確定**關閉**變更驗證**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="b72e7-177">Select **OK** to close the **Change Authentication** dialog.</span></span> <span data-ttu-id="b72e7-178">選取**確定**建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b72e7-178">Select **OK** to create the web app.</span></span>

<span data-ttu-id="b72e7-179">Visual Studio 會建立名為的控制站的 web API *ValuesController.cs* ，傳回硬式編碼值為 GET 要求。</span><span class="sxs-lookup"><span data-stu-id="b72e7-179">Visual Studio creates the web API with a controller named *ValuesController.cs* that returns hard-coded values for GET requests.</span></span> <span data-ttu-id="b72e7-180">類別以裝飾[Authorize 屬性](xref:security/authorization/simple)，因此所有要求均都需要驗證。</span><span class="sxs-lookup"><span data-stu-id="b72e7-180">The class is decorated with the [Authorize attribute](xref:security/authorization/simple), so all requests require authentication.</span></span>

## <a name="run-the-web-api"></a><span data-ttu-id="b72e7-181">執行 web 應用程式開發介面</span><span class="sxs-lookup"><span data-stu-id="b72e7-181">Run the web API</span></span>

<span data-ttu-id="b72e7-182">在 Visual Studio 執行應用程式開發介面。</span><span class="sxs-lookup"><span data-stu-id="b72e7-182">In Visual Studio, run the API.</span></span> <span data-ttu-id="b72e7-183">Visual Studio 會啟動瀏覽器指向的 API 根目錄 URL。</span><span class="sxs-lookup"><span data-stu-id="b72e7-183">Visual Studio launches a browser pointed at the API's root URL.</span></span> <span data-ttu-id="b72e7-184">請注意網址列中的 URL，並將保留在背景執行的 API。</span><span class="sxs-lookup"><span data-stu-id="b72e7-184">Note the URL in the address bar, and leave the API running in the background.</span></span>

> [!NOTE]
> <span data-ttu-id="b72e7-185">因為不沒有定義根目錄 URL 的任何控制器，則瀏覽器會顯示 404 （找不到頁面） 錯誤。</span><span class="sxs-lookup"><span data-stu-id="b72e7-185">Since there is no controller defined for the root URL, the browser displays a 404 (page not found) error.</span></span> <span data-ttu-id="b72e7-186">這是正常的現象。</span><span class="sxs-lookup"><span data-stu-id="b72e7-186">This is expected behavior.</span></span>

## <a name="use-postman-to-get-a-token-and-test-the-api"></a><span data-ttu-id="b72e7-187">使用郵差取得權杖，並測試應用程式開發介面</span><span class="sxs-lookup"><span data-stu-id="b72e7-187">Use Postman to get a token and test the API</span></span>

<span data-ttu-id="b72e7-188">[郵差](https://getpostman.com/postman)是 web 應用程式開發介面的工具進行測試。</span><span class="sxs-lookup"><span data-stu-id="b72e7-188">[Postman](https://getpostman.com/postman) is a tool for testing web APIs.</span></span> <span data-ttu-id="b72e7-189">本教學課程，郵差模擬存取 web 應用程式開發介面代表使用者的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b72e7-189">For this tutorial, Postman simulates a web app that accesses the web API on the user's behalf.</span></span>

### <a name="register-postman-as-a-web-app"></a><span data-ttu-id="b72e7-190">為 web 應用程式註冊郵差</span><span class="sxs-lookup"><span data-stu-id="b72e7-190">Register Postman as a web app</span></span>

<span data-ttu-id="b72e7-191">因為郵差模擬可以從 Azure AD B2C 租用戶取得權杖的 web 應用程式，它必須註冊租用戶中為 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b72e7-191">Since Postman simulates a web app that can obtain tokens from the Azure AD B2C tenant, it must be registered in the tenant as a web app.</span></span> <span data-ttu-id="b72e7-192">註冊使用郵差[文件中的步驟](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app)下**註冊 web 應用程式**> 一節。</span><span class="sxs-lookup"><span data-stu-id="b72e7-192">Register Postman using [the steps in the documentation](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) under the **Register a web app** section.</span></span> <span data-ttu-id="b72e7-193">在停止**建立 web 應用程式的用戶端密碼**> 一節。</span><span class="sxs-lookup"><span data-stu-id="b72e7-193">Stop at the **Create a web app client secret** section.</span></span> <span data-ttu-id="b72e7-194">此教學課程中，不需要用戶端密碼。</span><span class="sxs-lookup"><span data-stu-id="b72e7-194">A client secret isn't required for this tutorial.</span></span> 

<span data-ttu-id="b72e7-195">使用下列值：</span><span class="sxs-lookup"><span data-stu-id="b72e7-195">Use the following values:</span></span>

| <span data-ttu-id="b72e7-196">設定</span><span class="sxs-lookup"><span data-stu-id="b72e7-196">Setting</span></span>                       | <span data-ttu-id="b72e7-197">值</span><span class="sxs-lookup"><span data-stu-id="b72e7-197">Value</span></span>                            | <span data-ttu-id="b72e7-198">注意</span><span class="sxs-lookup"><span data-stu-id="b72e7-198">Notes</span></span>                           |
|-------------------------------|----------------------------------|---------------------------------|
| <span data-ttu-id="b72e7-199">**名稱**</span><span class="sxs-lookup"><span data-stu-id="b72e7-199">**Name**</span></span>                      | <span data-ttu-id="b72e7-200">郵差</span><span class="sxs-lookup"><span data-stu-id="b72e7-200">Postman</span></span>                          |                                 |
| <span data-ttu-id="b72e7-201">**包含 web 應用程式/web 應用程式開發介面**</span><span class="sxs-lookup"><span data-stu-id="b72e7-201">**Include web app / web API**</span></span> | <span data-ttu-id="b72e7-202">[是]</span><span class="sxs-lookup"><span data-stu-id="b72e7-202">Yes</span></span>                              |                                 |
| <span data-ttu-id="b72e7-203">**允許隱含流程**</span><span class="sxs-lookup"><span data-stu-id="b72e7-203">**Allow implicit flow**</span></span>       | <span data-ttu-id="b72e7-204">[是]</span><span class="sxs-lookup"><span data-stu-id="b72e7-204">Yes</span></span>                              |                                 |
| <span data-ttu-id="b72e7-205">**回覆 URL**</span><span class="sxs-lookup"><span data-stu-id="b72e7-205">**Reply URL**</span></span>                 | `https://getpostman.com/postman` |                                 |
| <span data-ttu-id="b72e7-206">**應用程式識別碼 URI**</span><span class="sxs-lookup"><span data-stu-id="b72e7-206">**App ID URI**</span></span>                | <span data-ttu-id="b72e7-207">*&lt;保留空白&gt;*</span><span class="sxs-lookup"><span data-stu-id="b72e7-207">*&lt;leave blank&gt;*</span></span>            | <span data-ttu-id="b72e7-208">本教學課程中，不需要。</span><span class="sxs-lookup"><span data-stu-id="b72e7-208">Not required for this tutorial.</span></span> |
| <span data-ttu-id="b72e7-209">**包含原生用戶端**</span><span class="sxs-lookup"><span data-stu-id="b72e7-209">**Include native client**</span></span>     | <span data-ttu-id="b72e7-210">否</span><span class="sxs-lookup"><span data-stu-id="b72e7-210">No</span></span>                               |                                 |

<span data-ttu-id="b72e7-211">新註冊的 web 應用程式需要存取 web 應用程式開發介面代表使用者的權限。</span><span class="sxs-lookup"><span data-stu-id="b72e7-211">The newly registered web app needs permission to access the web API on the user's behalf.</span></span>  

1. <span data-ttu-id="b72e7-212">選取**郵差**清單中的應用程式，然後選取**API 存取**從左側功能表。</span><span class="sxs-lookup"><span data-stu-id="b72e7-212">Select **Postman** in the list of apps and then select **API access** from the menu on the left.</span></span>
2. <span data-ttu-id="b72e7-213">選取 **+ 加入**。</span><span class="sxs-lookup"><span data-stu-id="b72e7-213">Select **+ Add**.</span></span>
3. <span data-ttu-id="b72e7-214">在**選取應用程式開發介面**下拉式清單中，選取 web 應用程式開發介面的名稱。</span><span class="sxs-lookup"><span data-stu-id="b72e7-214">In the **Select API** dropdown, select the name of the web API.</span></span>
4. <span data-ttu-id="b72e7-215">在**選取範圍**下拉式清單中，確定已選取所有領域。</span><span class="sxs-lookup"><span data-stu-id="b72e7-215">In the **Select Scopes** dropdown, ensure all scopes are selected.</span></span>
5. <span data-ttu-id="b72e7-216">選取**確定**。</span><span class="sxs-lookup"><span data-stu-id="b72e7-216">Select **Ok**.</span></span>

<span data-ttu-id="b72e7-217">請注意郵差應用程式的應用程式識別碼，因為它需要取得的承載權杖。</span><span class="sxs-lookup"><span data-stu-id="b72e7-217">Note the Postman app's Application ID, as it's required to obtain a bearer token.</span></span>

### <a name="create-a-postman-request"></a><span data-ttu-id="b72e7-218">建立郵差要求</span><span class="sxs-lookup"><span data-stu-id="b72e7-218">Create a Postman request</span></span>

<span data-ttu-id="b72e7-219">啟動郵差。</span><span class="sxs-lookup"><span data-stu-id="b72e7-219">Launch Postman.</span></span> <span data-ttu-id="b72e7-220">根據預設，顯示郵差**新建**時啟動對話方塊。</span><span class="sxs-lookup"><span data-stu-id="b72e7-220">By default, Postman displays the **Create New** dialog upon launching.</span></span> <span data-ttu-id="b72e7-221">如果未顯示對話方塊，選取 **+ 新增**左上角的按鈕。</span><span class="sxs-lookup"><span data-stu-id="b72e7-221">If the dialog isn't displayed, select the **+ New** button in the upper left.</span></span>

<span data-ttu-id="b72e7-222">從**新建**對話方塊：</span><span class="sxs-lookup"><span data-stu-id="b72e7-222">From the **Create New** dialog:</span></span>

1. <span data-ttu-id="b72e7-223">選取**要求**。</span><span class="sxs-lookup"><span data-stu-id="b72e7-223">Select **Request**.</span></span>

    ![要求按鈕](./azure-ad-b2c-webapi/postman-create-new.png)

2. <span data-ttu-id="b72e7-225">輸入*取得的值*中**要求名稱**方塊。</span><span class="sxs-lookup"><span data-stu-id="b72e7-225">Enter *Get Values* in the **Request name** box.</span></span>
3. <span data-ttu-id="b72e7-226">選取 **+ 建立集合**來建立新的集合，用於儲存要求。</span><span class="sxs-lookup"><span data-stu-id="b72e7-226">Select **+ Create Collection** to create a new collection for storing the request.</span></span> <span data-ttu-id="b72e7-227">名稱集合*ASP.NET Core 教學課程*，然後選取核取記號。</span><span class="sxs-lookup"><span data-stu-id="b72e7-227">Name the collection *ASP.NET Core tutorials* and then select the checkmark.</span></span>

    ![建立新的集合](./azure-ad-b2c-webapi/postman-create-collection.png)

4. <span data-ttu-id="b72e7-229">選取**儲存 ASP.NET Core 教學課程** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b72e7-229">Select the **Save to ASP.NET Core tutorials** button.</span></span>

### <a name="test-the-web-api-without-authentication"></a><span data-ttu-id="b72e7-230">測試 web 應用程式開發介面不需驗證</span><span class="sxs-lookup"><span data-stu-id="b72e7-230">Test the web API without authentication</span></span>

<span data-ttu-id="b72e7-231">若要確認 web API 需要驗證，首先請未經驗證的要求。</span><span class="sxs-lookup"><span data-stu-id="b72e7-231">To verify that the web API requires authentication, first make a request without authentication.</span></span>

1. <span data-ttu-id="b72e7-232">在**輸入要求 URL**方塊中，輸入的 URL `ValuesController`。</span><span class="sxs-lookup"><span data-stu-id="b72e7-232">In the **Enter request URL** box, enter the URL for `ValuesController`.</span></span> <span data-ttu-id="b72e7-233">URL 是相同的瀏覽器中顯示**api/值**附加。</span><span class="sxs-lookup"><span data-stu-id="b72e7-233">The URL is the same as displayed in the browser with **api/values** appended.</span></span> <span data-ttu-id="b72e7-234">範例就是`https://localhost:44375/api/values`。</span><span class="sxs-lookup"><span data-stu-id="b72e7-234">An example would be `https://localhost:44375/api/values`.</span></span>
2. <span data-ttu-id="b72e7-235">選取**傳送** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b72e7-235">Select the **Send** button.</span></span>
3. <span data-ttu-id="b72e7-236">請注意回應的狀態是*401 未授權*。</span><span class="sxs-lookup"><span data-stu-id="b72e7-236">Note the status of the response is *401 Unauthorized*.</span></span>

    ![401 未經授權的回應](./azure-ad-b2c-webapi/postman-401-status.png)

### <a name="obtain-a-bearer-token"></a><span data-ttu-id="b72e7-238">取得持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="b72e7-238">Obtain a bearer token</span></span>

<span data-ttu-id="b72e7-239">若要讓已驗證的要求至 web API，持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="b72e7-239">To make an authenticated request to the web API, a bearer token is required.</span></span> <span data-ttu-id="b72e7-240">郵差輕鬆地登入 Azure AD B2C 租用戶，並取得權杖。</span><span class="sxs-lookup"><span data-stu-id="b72e7-240">Postman makes it easy to sign in to the Azure AD B2C tenant and obtain a token.</span></span>

1. <span data-ttu-id="b72e7-241">在**授權**索引標籤的**類型**下拉式清單中，選取**OAuth 2.0**。</span><span class="sxs-lookup"><span data-stu-id="b72e7-241">On the **Authorization** tab, in the **TYPE** dropdown, select **OAuth 2.0**.</span></span> <span data-ttu-id="b72e7-242">在**授權將資料加入至**下拉式清單中，選取**要求標頭**。</span><span class="sxs-lookup"><span data-stu-id="b72e7-242">In the **Add authorization data to** dropdown, select **Request Headers**.</span></span> <span data-ttu-id="b72e7-243">選取**取得新存取權杖**。</span><span class="sxs-lookup"><span data-stu-id="b72e7-243">Select **Get New Access Token**.</span></span>

    ![授權與 [設定] 索引標籤](./azure-ad-b2c-webapi/postman-auth-tab.png)

2. <span data-ttu-id="b72e7-245">完成**取得新的存取權杖**對話方塊，如下所示：</span><span class="sxs-lookup"><span data-stu-id="b72e7-245">Complete the **GET NEW ACCESS TOKEN** dialog as follows:</span></span>


   |                <span data-ttu-id="b72e7-246">設定</span><span class="sxs-lookup"><span data-stu-id="b72e7-246">Setting</span></span>                 |                                             <span data-ttu-id="b72e7-247">值</span><span class="sxs-lookup"><span data-stu-id="b72e7-247">Value</span></span>                                             |                                                                                                                                    <span data-ttu-id="b72e7-248">注意</span><span class="sxs-lookup"><span data-stu-id="b72e7-248">Notes</span></span>                                                                                                                                     |
   |----------------------------------------|-----------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |      <span data-ttu-id="b72e7-249"><strong>語彙基元名稱</strong></span><span class="sxs-lookup"><span data-stu-id="b72e7-249"><strong>Token Name</strong></span></span>       |                                  <span data-ttu-id="b72e7-250"><em>&lt;語彙基元名稱&gt;</em></span><span class="sxs-lookup"><span data-stu-id="b72e7-250"><em>&lt;token name&gt;</em></span></span>                                  |                                                                                                                   <span data-ttu-id="b72e7-251">輸入語彙基元的描述性名稱。</span><span class="sxs-lookup"><span data-stu-id="b72e7-251">Enter a descriptive name for the token.</span></span>                                                                                                                    |
   |      <span data-ttu-id="b72e7-252"><strong>授與類型</strong></span><span class="sxs-lookup"><span data-stu-id="b72e7-252"><strong>Grant Type</strong></span></span>       |                                           <span data-ttu-id="b72e7-253">隱含</span><span class="sxs-lookup"><span data-stu-id="b72e7-253">Implicit</span></span>                                            |                                                                                                                                                                                                                                                                              |
   |     <span data-ttu-id="b72e7-254"><strong>回呼 URL</strong></span><span class="sxs-lookup"><span data-stu-id="b72e7-254"><strong>Callback URL</strong></span></span>      |                               `https://getpostman.com/postman`                                |                                                                                                                                                                                                                                                                              |
   |       <span data-ttu-id="b72e7-255"><strong>驗證 URL</strong></span><span class="sxs-lookup"><span data-stu-id="b72e7-255"><strong>Auth URL</strong></span></span>        | `https://login.microsoftonline.com/<tenant domain name>/oauth2/v2.0/authorize?p=B2C_1_SiUpIn` |                                                                                                  <span data-ttu-id="b72e7-256">取代<em>&lt;租用戶網域名稱&gt;</em>租用戶的網域名稱。</span><span class="sxs-lookup"><span data-stu-id="b72e7-256">Replace <em>&lt;tenant domain name&gt;</em> with the tenant's domain name.</span></span>                                                                                                  |
   |       <span data-ttu-id="b72e7-257"><strong>用戶端識別碼</strong></span><span class="sxs-lookup"><span data-stu-id="b72e7-257"><strong>Client ID</strong></span></span>       |                <span data-ttu-id="b72e7-258"><em>&lt;輸入郵差應用程式的<b>應用程式識別碼</b>&gt;</em></span><span class="sxs-lookup"><span data-stu-id="b72e7-258"><em>&lt;enter the Postman app's <b>Application ID</b>&gt;</em></span></span>                 |                                                                                                                                                                                                                                                                              |
   |     <span data-ttu-id="b72e7-259"><strong>用戶端密碼</strong></span><span class="sxs-lookup"><span data-stu-id="b72e7-259"><strong>Client Secret</strong></span></span>     |                                 <span data-ttu-id="b72e7-260"><em>&lt;保留空白&gt;</em></span><span class="sxs-lookup"><span data-stu-id="b72e7-260"><em>&lt;leave blank&gt;</em></span></span>                                  |                                                                                                                                                                                                                                                                              |
   |         <span data-ttu-id="b72e7-261"><strong>範圍</strong></span><span class="sxs-lookup"><span data-stu-id="b72e7-261"><strong>Scope</strong></span></span>         |         `https://<tenant domain name>/<api>/user_impersonation openid offline_access`         | <span data-ttu-id="b72e7-262">取代<em>&lt;租用戶網域名稱&gt;</em>租用戶的網域名稱。</span><span class="sxs-lookup"><span data-stu-id="b72e7-262">Replace <em>&lt;tenant domain name&gt;</em> with the tenant's domain name.</span></span> <span data-ttu-id="b72e7-263">取代<em>&lt;api&gt;</em> Web API 專案名稱。</span><span class="sxs-lookup"><span data-stu-id="b72e7-263">Replace <em>&lt;api&gt;</em> with the Web API project name.</span></span> <span data-ttu-id="b72e7-264">您也可以使用應用程式識別碼。</span><span class="sxs-lookup"><span data-stu-id="b72e7-264">You can also use Application ID.</span></span> <span data-ttu-id="b72e7-265">URL 的模式是： <em>https://{tenant}.onmicrosoft.com/{app_name_or_id}/{scope 名稱}</em>。</span><span class="sxs-lookup"><span data-stu-id="b72e7-265">The pattern for the URL is: <em>https://{tenant}.onmicrosoft.com/{app_name_or_id}/{scope name}</em>.</span></span> |
   | <span data-ttu-id="b72e7-266"><strong>用戶端驗證</strong></span><span class="sxs-lookup"><span data-stu-id="b72e7-266"><strong>Client Authentication</strong></span></span> |                                <span data-ttu-id="b72e7-267">在本文中傳送用戶端認證</span><span class="sxs-lookup"><span data-stu-id="b72e7-267">Send client credentials in body</span></span>                                |                                                                                                                                                                                                                                                                              |


3. <span data-ttu-id="b72e7-268">選取**要求語彙基元** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b72e7-268">Select the **Request Token** button.</span></span>

4. <span data-ttu-id="b72e7-269">郵差開啟新視窗包含 Azure AD B2C 租用戶的登入對話方塊。</span><span class="sxs-lookup"><span data-stu-id="b72e7-269">Postman opens a new window containing the Azure AD B2C tenant's sign in dialog.</span></span> <span data-ttu-id="b72e7-270">（如果已建立一個測試原則），使用現有的帳戶登入，或選取**立即註冊**來建立新的帳戶。</span><span class="sxs-lookup"><span data-stu-id="b72e7-270">Sign in with an existing account (if one was created testing the policies) or select **Sign up now** to create a new account.</span></span> <span data-ttu-id="b72e7-271">**忘記密碼？** 連結用來重設忘記的密碼。</span><span class="sxs-lookup"><span data-stu-id="b72e7-271">The **Forgot your password?** link is used to reset a forgotten password.</span></span>

5. <span data-ttu-id="b72e7-272">已成功登入之後，關閉視窗而**管理存取權杖**對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="b72e7-272">After successfully signing in, the window closes and the **MANAGE ACCESS TOKENS** dialog appears.</span></span> <span data-ttu-id="b72e7-273">捲動到底部，然後選取**使用語彙基元** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b72e7-273">Scroll down to the bottom and select the **Use Token** button.</span></span>

    ![如何尋找 「 使用語彙基元 」 按鈕](./azure-ad-b2c-webapi/postman-access-token.png)

### <a name="test-the-web-api-with-authentication"></a><span data-ttu-id="b72e7-275">測試 web 應用程式開發介面使用驗證</span><span class="sxs-lookup"><span data-stu-id="b72e7-275">Test the web API with authentication</span></span>

<span data-ttu-id="b72e7-276">選取**傳送** 按鈕，再傳送要求。</span><span class="sxs-lookup"><span data-stu-id="b72e7-276">Select the **Send** button to send the request again.</span></span> <span data-ttu-id="b72e7-277">此時，回應狀態是*200 確定* ，JSON 裝載回應上看到**主體** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="b72e7-277">This time, the response status is *200 OK* and the JSON payload is visible on the response **Body** tab.</span></span>

![裝載及成功與否的狀態](./azure-ad-b2c-webapi/postman-success.png)

## <a name="next-steps"></a><span data-ttu-id="b72e7-279">後續步驟</span><span class="sxs-lookup"><span data-stu-id="b72e7-279">Next steps</span></span>

<span data-ttu-id="b72e7-280">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="b72e7-280">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b72e7-281">建立 Azure Active Directory B2C 租用戶。</span><span class="sxs-lookup"><span data-stu-id="b72e7-281">Create an Azure Active Directory B2C tenant.</span></span>
> * <span data-ttu-id="b72e7-282">在 Azure AD B2C 中註冊 Web 應用程式開發介面。</span><span class="sxs-lookup"><span data-stu-id="b72e7-282">Register a Web API in Azure AD B2C.</span></span>
> * <span data-ttu-id="b72e7-283">您可以使用 Visual Studio 來建立 Web API 設定要用於驗證的 Azure AD B2C 租用戶。</span><span class="sxs-lookup"><span data-stu-id="b72e7-283">Use Visual Studio to create a Web API configured to use the Azure AD B2C tenant for authentication.</span></span>
> * <span data-ttu-id="b72e7-284">設定控制行為的 Azure AD B2C 租用戶的原則。</span><span class="sxs-lookup"><span data-stu-id="b72e7-284">Configure policies controlling the behavior of the Azure AD B2C tenant.</span></span>
> * <span data-ttu-id="b72e7-285">使用郵差來模擬 web 應用程式會顯示登入 對話方塊中，擷取語彙基元，並用它來對 web API 提出要求。</span><span class="sxs-lookup"><span data-stu-id="b72e7-285">Use Postman to simulate a web app which presents a login dialog, retrieves a token, and uses it to make a request against the web API.</span></span>

<span data-ttu-id="b72e7-286">繼續學習來開發您的 API:</span><span class="sxs-lookup"><span data-stu-id="b72e7-286">Continue developing your API by learning to:</span></span>

* <span data-ttu-id="b72e7-287">[保護 ASP.NET Core web 應用程式使用 Azure AD B2C](xref:security/authentication/azure-ad-b2c)。</span><span class="sxs-lookup"><span data-stu-id="b72e7-287">[Secure an ASP.NET Core web app using Azure AD B2C](xref:security/authentication/azure-ad-b2c).</span></span>
* <span data-ttu-id="b72e7-288">[從.NET web 應用程式中使用 Azure AD B2C 呼叫.NET web 應用程式開發介面](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet)。</span><span class="sxs-lookup"><span data-stu-id="b72e7-288">[Call a .NET web API from a .NET web app using Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).</span></span>
* <span data-ttu-id="b72e7-289">[自訂使用者介面，Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization)。</span><span class="sxs-lookup"><span data-stu-id="b72e7-289">[Customize the Azure AD B2C user interface](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).</span></span>
* <span data-ttu-id="b72e7-290">[設定密碼複雜性需求](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity)。</span><span class="sxs-lookup"><span data-stu-id="b72e7-290">[Configure password complexity requirements](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).</span></span>
* <span data-ttu-id="b72e7-291">[啟用多因素驗證](/azure/active-directory-b2c/active-directory-b2c-reference-mfa)。</span><span class="sxs-lookup"><span data-stu-id="b72e7-291">[Enable multi-factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).</span></span>
* <span data-ttu-id="b72e7-292">設定其他身分識別提供者，例如[Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app)， [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app)， [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app)， [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app)， [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)，和其他人。</span><span class="sxs-lookup"><span data-stu-id="b72e7-292">Configure additional identity providers, such as [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app), and others.</span></span>
* <span data-ttu-id="b72e7-293">[使用 Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet)擷取額外的使用者資訊，例如群組成員資格，從 Azure AD B2C 租用戶。</span><span class="sxs-lookup"><span data-stu-id="b72e7-293">[Use the Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) to retrieve additional user information, such as group membership, from the Azure AD B2C tenant.</span></span>
