---
title: 使用 Azure Active Directory B2C 在 ASP.NET Core 中的雲端驗證
author: camsoper
description: 了解如何設定 Azure Active Directory B2C 使用 ASP.NET Core 的驗證。
ms.author: casoper
ms.custom: mvc
ms.date: 02/27/2019
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: 3cb878aff7bf0c6c8efe7f3f0c0f06c74acef477
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538735"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a><span data-ttu-id="5a3e8-103">使用 Azure Active Directory B2C 在 ASP.NET Core 中的雲端驗證</span><span class="sxs-lookup"><span data-stu-id="5a3e8-103">Cloud authentication with Azure Active Directory B2C in ASP.NET Core</span></span>

<span data-ttu-id="5a3e8-104">作者 [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="5a3e8-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="5a3e8-105">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) 是適用於 web 和行動裝置應用程式的雲端身分識別管理解決方案。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-105">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) is a cloud identity management solution for web and mobile apps.</span></span> <span data-ttu-id="5a3e8-106">服務提供雲端和內部部署中託管的應用程式的驗證。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-106">The service provides authentication for apps hosted in the cloud and on-premises.</span></span> <span data-ttu-id="5a3e8-107">驗證類型包括個別帳戶，社交網路帳戶，以及同盟企業帳戶。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-107">Authentication types include individual accounts, social network accounts, and federated enterprise accounts.</span></span> <span data-ttu-id="5a3e8-108">此外，Azure AD B2C 可提供多重要素驗證，以最低組態。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-108">Additionally, Azure AD B2C can provide multi-factor authentication with minimal configuration.</span></span>

> [!TIP]
> <span data-ttu-id="5a3e8-109">Azure Active Directory (Azure AD) 與 Azure AD B2C 是個別的產品供應項目。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-109">Azure Active Directory (Azure AD) and Azure AD B2C are separate product offerings.</span></span> <span data-ttu-id="5a3e8-110">Azure AD 租用戶代表組織，而 Azure AD B2C 租用戶代表與信賴憑證者應用程式要使用的身分識別的集合。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-110">An Azure AD tenant represents an organization, while an Azure AD B2C tenant represents a collection of identities to be used with relying party applications.</span></span> <span data-ttu-id="5a3e8-111">若要進一步了解，請參閱[Azure AD B2C:常見問題集 (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs)。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-111">To learn more, see [Azure AD B2C: Frequently asked questions (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).</span></span>

<span data-ttu-id="5a3e8-112">在本教學課程，了解如何：</span><span class="sxs-lookup"><span data-stu-id="5a3e8-112">In this tutorial, learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5a3e8-113">建立 Azure Active Directory B2C 租用戶</span><span class="sxs-lookup"><span data-stu-id="5a3e8-113">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="5a3e8-114">在 Azure AD B2C 中註冊應用程式</span><span class="sxs-lookup"><span data-stu-id="5a3e8-114">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="5a3e8-115">使用 Visual Studio 建立 ASP.NET Core web 應用程式設定為使用 Azure AD B2C 租用戶進行驗證</span><span class="sxs-lookup"><span data-stu-id="5a3e8-115">Use Visual Studio to create an ASP.NET Core web app configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="5a3e8-116">設定原則來控制 Azure AD B2C 租用戶行為</span><span class="sxs-lookup"><span data-stu-id="5a3e8-116">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5a3e8-117">必要條件</span><span class="sxs-lookup"><span data-stu-id="5a3e8-117">Prerequisites</span></span>

<span data-ttu-id="5a3e8-118">在此逐步解說需要下列條件：</span><span class="sxs-lookup"><span data-stu-id="5a3e8-118">The following are required for this walkthrough:</span></span>

* [<span data-ttu-id="5a3e8-119">Microsoft Azure 訂用帳戶</span><span class="sxs-lookup"><span data-stu-id="5a3e8-119">Microsoft Azure subscription</span></span>](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [<span data-ttu-id="5a3e8-120">Visual Studio 2019</span><span class="sxs-lookup"><span data-stu-id="5a3e8-120">Visual Studio 2019</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)

## <a name="create-the-azure-active-directory-b2c-tenant"></a><span data-ttu-id="5a3e8-121">建立 Azure Active Directory B2C 租用戶</span><span class="sxs-lookup"><span data-stu-id="5a3e8-121">Create the Azure Active Directory B2C tenant</span></span>

<span data-ttu-id="5a3e8-122">建立 Azure Active Directory B2C 租用戶[文件中所述](/azure/active-directory-b2c/active-directory-b2c-get-started)。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-122">Create an Azure Active Directory B2C tenant [as described in the documentation](/azure/active-directory-b2c/active-directory-b2c-get-started).</span></span> <span data-ttu-id="5a3e8-123">出現提示時，租用戶關聯至 Azure 訂用帳戶是選擇性的在本教學課程。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-123">When prompted, associating the tenant with an Azure subscription is optional for this tutorial.</span></span>

## <a name="register-the-app-in-azure-ad-b2c"></a><span data-ttu-id="5a3e8-124">在 Azure AD B2C 中註冊應用程式</span><span class="sxs-lookup"><span data-stu-id="5a3e8-124">Register the app in Azure AD B2C</span></span>

<span data-ttu-id="5a3e8-125">在新建立的 Azure AD B2C 租用戶中註冊您的應用程式使用[文件中的步驟](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app)下方**註冊 web 應用程式**一節。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-125">In the newly created Azure AD B2C tenant, register your app using [the steps in the documentation](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) under the **Register a web app** section.</span></span> <span data-ttu-id="5a3e8-126">在停止**建立 web 應用程式用戶端祕密**一節。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-126">Stop at the **Create a web app client secret** section.</span></span> <span data-ttu-id="5a3e8-127">本教學課程中，不需要用戶端祕密。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-127">A client secret isn't required for this tutorial.</span></span> 

<span data-ttu-id="5a3e8-128">使用下列值：</span><span class="sxs-lookup"><span data-stu-id="5a3e8-128">Use the following values:</span></span>

| <span data-ttu-id="5a3e8-129">設定</span><span class="sxs-lookup"><span data-stu-id="5a3e8-129">Setting</span></span>                       | <span data-ttu-id="5a3e8-130">值</span><span class="sxs-lookup"><span data-stu-id="5a3e8-130">Value</span></span>                     | <span data-ttu-id="5a3e8-131">注意</span><span class="sxs-lookup"><span data-stu-id="5a3e8-131">Notes</span></span>                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5a3e8-132">**名稱**</span><span class="sxs-lookup"><span data-stu-id="5a3e8-132">**Name**</span></span>                      | <span data-ttu-id="5a3e8-133">*&lt;應用程式名稱&gt;*</span><span class="sxs-lookup"><span data-stu-id="5a3e8-133">*&lt;app name&gt;*</span></span>        | <span data-ttu-id="5a3e8-134">請輸入**名稱**描述取用者應用程式的應用程式。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-134">Enter a **Name** for the app that describes your app to consumers.</span></span>                                                                                                                                 |
| <span data-ttu-id="5a3e8-135">**包含 web 應用程式/web API**</span><span class="sxs-lookup"><span data-stu-id="5a3e8-135">**Include web app / web API**</span></span> | <span data-ttu-id="5a3e8-136">是</span><span class="sxs-lookup"><span data-stu-id="5a3e8-136">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="5a3e8-137">**允許隱含流程**</span><span class="sxs-lookup"><span data-stu-id="5a3e8-137">**Allow implicit flow**</span></span>       | <span data-ttu-id="5a3e8-138">是</span><span class="sxs-lookup"><span data-stu-id="5a3e8-138">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="5a3e8-139">**回覆 URL**</span><span class="sxs-lookup"><span data-stu-id="5a3e8-139">**Reply URL**</span></span>                 | `https://localhost:44300/signin-oidc` | <span data-ttu-id="5a3e8-140">回覆 Url 會是 Azure AD B2C 傳回您的應用程式要求之任何權杖的所在端點。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-140">Reply URLs are endpoints where Azure AD B2C returns any tokens that your app requests.</span></span> <span data-ttu-id="5a3e8-141">Visual Studio 會提供要使用的回覆 URL。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-141">Visual Studio provides the Reply URL to use.</span></span> <span data-ttu-id="5a3e8-142">現在，輸入`https://localhost:44300/signin-oidc`完成表單。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-142">For now, enter `https://localhost:44300/signin-oidc` to complete the form.</span></span> |
| <span data-ttu-id="5a3e8-143">**應用程式識別碼 URI**</span><span class="sxs-lookup"><span data-stu-id="5a3e8-143">**App ID URI**</span></span>                | <span data-ttu-id="5a3e8-144">保留空白</span><span class="sxs-lookup"><span data-stu-id="5a3e8-144">Leave blank</span></span>               | <span data-ttu-id="5a3e8-145">本教學課程中，不需要。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-145">Not required for this tutorial.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="5a3e8-146">**包含原生用戶端**</span><span class="sxs-lookup"><span data-stu-id="5a3e8-146">**Include native client**</span></span>     | <span data-ttu-id="5a3e8-147">否</span><span class="sxs-lookup"><span data-stu-id="5a3e8-147">No</span></span>                        |                                                                                                                                                                                                    |

> [!WARNING]
> <span data-ttu-id="5a3e8-148">如果設定非 localhost 回覆 URL，留意[條件約束 [回覆 URL] 清單中允許的項目](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url)。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-148">If setting up a non-localhost Reply URL, be aware of the [constraints on what is allowed in the Reply URL list](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url).</span></span> 

<span data-ttu-id="5a3e8-149">註冊 app 之後，會顯示租用戶中的應用程式清單。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-149">After the app is registered, the list of apps in the tenant is displayed.</span></span> <span data-ttu-id="5a3e8-150">選取剛剛已註冊的應用程式。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-150">Select the app that was just registered.</span></span> <span data-ttu-id="5a3e8-151">選取**複製**右邊的圖示**APPLICATION-ID**欄位，以將它複製到剪貼簿。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-151">Select the **Copy** icon to the right of the **Application ID** field to copy it to the clipboard.</span></span>

<span data-ttu-id="5a3e8-152">沒有項目更多可以設定 Azure AD B2C 租用戶中，在此階段中，但將瀏覽器視窗保持開啟。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-152">Nothing more can be configured in the Azure AD B2C tenant at this time, but leave the browser window open.</span></span> <span data-ttu-id="5a3e8-153">建立 ASP.NET Core 應用程式之後，沒有更多的設定。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-153">There is more configuration after the ASP.NET Core app is created.</span></span>

## <a name="create-an-aspnet-core-app-in-visual-studio"></a><span data-ttu-id="5a3e8-154">在 Visual Studio 中建立 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="5a3e8-154">Create an ASP.NET Core app in Visual Studio</span></span>

<span data-ttu-id="5a3e8-155">Visual Studio Web 應用程式範本可以設定要用於驗證的 Azure AD B2C 租用戶。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-155">The Visual Studio Web Application template can be configured to use the Azure AD B2C tenant for authentication.</span></span>

<span data-ttu-id="5a3e8-156">在 Visual Studio 中：</span><span class="sxs-lookup"><span data-stu-id="5a3e8-156">In Visual Studio:</span></span>

1. <span data-ttu-id="5a3e8-157">建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-157">Create a new ASP.NET Core Web Application.</span></span> 
2. <span data-ttu-id="5a3e8-158">選取  **Web 應用程式**從範本清單。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-158">Select **Web Application** from the list of templates.</span></span>
3. <span data-ttu-id="5a3e8-159">選取 [**變更驗證**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-159">Select the **Change Authentication** button.</span></span>
    
    ![變更 [驗證] 按鈕](./azure-ad-b2c/_static/changeauth.png)

4. <span data-ttu-id="5a3e8-161">在**變更驗證**對話方塊中，選取**個別使用者帳戶**，然後選取**連接到雲端中現有的使用者存放區**下拉式清單中。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-161">In the **Change Authentication** dialog, select **Individual User Accounts**, and then select **Connect to an existing user store in the cloud** in the dropdown.</span></span> 
    
    ![變更驗證 對話方塊](./azure-ad-b2c/_static/changeauthdialog.png)

5. <span data-ttu-id="5a3e8-163">完成表單，使用下列值：</span><span class="sxs-lookup"><span data-stu-id="5a3e8-163">Complete the form with the following values:</span></span>
    
    | <span data-ttu-id="5a3e8-164">設定</span><span class="sxs-lookup"><span data-stu-id="5a3e8-164">Setting</span></span>                       | <span data-ttu-id="5a3e8-165">值</span><span class="sxs-lookup"><span data-stu-id="5a3e8-165">Value</span></span>                                                 |
    |-------------------------------|-------------------------------------------------------|
    | <span data-ttu-id="5a3e8-166">**網域名稱**</span><span class="sxs-lookup"><span data-stu-id="5a3e8-166">**Domain Name**</span></span>               | <span data-ttu-id="5a3e8-167">*&lt;您的 B2C 租用戶網域名稱&gt;*</span><span class="sxs-lookup"><span data-stu-id="5a3e8-167">*&lt;the domain name of your B2C tenant&gt;*</span></span>          |
    | <span data-ttu-id="5a3e8-168">**應用程式識別碼**</span><span class="sxs-lookup"><span data-stu-id="5a3e8-168">**Application ID**</span></span>            | <span data-ttu-id="5a3e8-169">*&lt;貼上剪貼簿中的應用程式識別碼&gt;*</span><span class="sxs-lookup"><span data-stu-id="5a3e8-169">*&lt;paste the Application ID from the clipboard&gt;*</span></span> |
    | <span data-ttu-id="5a3e8-170">**回呼路徑**</span><span class="sxs-lookup"><span data-stu-id="5a3e8-170">**Callback Path**</span></span>             | <span data-ttu-id="5a3e8-171">*&lt;使用預設值&gt;*</span><span class="sxs-lookup"><span data-stu-id="5a3e8-171">*&lt;use the default value&gt;*</span></span>                       |
    | <span data-ttu-id="5a3e8-172">**註冊或登入原則**</span><span class="sxs-lookup"><span data-stu-id="5a3e8-172">**Sign-up or sign-in policy**</span></span> | `B2C_1_SiUpIn`                                        |
    | <span data-ttu-id="5a3e8-173">**重設密碼原則**</span><span class="sxs-lookup"><span data-stu-id="5a3e8-173">**Reset password policy**</span></span>     | `B2C_1_SSPR`                                          |
    | <span data-ttu-id="5a3e8-174">**編輯設定檔原則**</span><span class="sxs-lookup"><span data-stu-id="5a3e8-174">**Edit profile policy**</span></span>       | <span data-ttu-id="5a3e8-175">*&lt;保留空白&gt;*</span><span class="sxs-lookup"><span data-stu-id="5a3e8-175">*&lt;leave blank&gt;*</span></span>                                 |
    
    <span data-ttu-id="5a3e8-176">選取 **複製**連結**回覆 URI**回覆 URI 複製到剪貼簿。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-176">Select the **Copy** link next to **Reply URI** to copy the Reply URI to the clipboard.</span></span> <span data-ttu-id="5a3e8-177">選取  **確定**以關閉**變更驗證**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-177">Select **OK** to close the **Change Authentication** dialog.</span></span> <span data-ttu-id="5a3e8-178">選取 **確定**建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-178">Select **OK** to create the web app.</span></span>

## <a name="finish-the-b2c-app-registration"></a><span data-ttu-id="5a3e8-179">完成 B2C 應用程式註冊</span><span class="sxs-lookup"><span data-stu-id="5a3e8-179">Finish the B2C app registration</span></span>

<span data-ttu-id="5a3e8-180">傳回與仍處於開啟狀態的 B2C 應用程式內容的瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-180">Return to the browser window with the B2C app properties still open.</span></span> <span data-ttu-id="5a3e8-181">變更暫存**回覆 URL**指定從 Visual Studio 的較早的值複製。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-181">Change the temporary **Reply URL** specified earlier to the value copied from Visual Studio.</span></span> <span data-ttu-id="5a3e8-182">選取 **儲存**視窗的頂端。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-182">Select **Save** at the top of the window.</span></span>

> [!TIP]
> <span data-ttu-id="5a3e8-183">如果您之前未複製回覆 URL，在 web 專案內容中，使用 HTTPS 位址，從 [偵錯] 索引標籤，並附加**CallbackPath**值從*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-183">If you didn't copy the Reply URL, use the HTTPS address from the Debug tab in the web project properties, and append the **CallbackPath** value from *appsettings.json*.</span></span>

## <a name="configure-policies"></a><span data-ttu-id="5a3e8-184">設定原則</span><span class="sxs-lookup"><span data-stu-id="5a3e8-184">Configure policies</span></span>

<span data-ttu-id="5a3e8-185">使用 Azure AD B2C 文件中的步驟[建立註冊或登入原則](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy)，然後[建立密碼重設原則](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy)。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-185">Use the steps in the Azure AD B2C documentation to [create a sign-up or sign-in policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy), and then [create a password reset policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy).</span></span> <span data-ttu-id="5a3e8-186">使用提供的文件中的範例值**身分識別提供者**，**註冊屬性**，並**應用程式宣告**。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-186">Use the example values provided in the documentation for **Identity providers**, **Sign-up attributes**, and **Application claims**.</span></span> <span data-ttu-id="5a3e8-187">使用**立即執行**是選擇性的按鈕來測試原則，文件中所述。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-187">Using the **Run now** button to test the policies as described in the documentation is optional.</span></span>

> [!WARNING]
> <span data-ttu-id="5a3e8-188">請確定 原則名稱，文件中所述完全是因為這些原則所使用的**變更驗證**Visual Studio 中的對話方塊。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-188">Ensure the policy names are exactly as described in the documentation, as those policies were used in the **Change Authentication** dialog in Visual Studio.</span></span> <span data-ttu-id="5a3e8-189">中可驗證的原則名稱*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-189">The policy names can be verified in *appsettings.json*.</span></span>

## <a name="configure-the-underlying-openidconnectoptionsjwtbearercookie-options"></a><span data-ttu-id="5a3e8-190">設定基礎 OpenIdConnectOptions/JwtBearer/Cookie 選項</span><span class="sxs-lookup"><span data-stu-id="5a3e8-190">Configure the underlying OpenIdConnectOptions/JwtBearer/Cookie options</span></span>

<span data-ttu-id="5a3e8-191">若要直接設定基礎的選項，使用中的適當配置常數`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="5a3e8-191">To configure the underlying options directly, use the appropriate scheme constant in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<OpenIdConnectOptions>(
    AzureAD[B2C]Defaults.OpenIdScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<CookieAuthenticationOptions>(
    AzureAD[B2C]Defaults.CookieScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<JwtBearerOptions>(
    AzureAD[B2C]Defaults.JwtBearerAuthenticationScheme, options => 
    {
        // Omitted for brevity
    });
```

## <a name="run-the-app"></a><span data-ttu-id="5a3e8-192">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="5a3e8-192">Run the app</span></span>

<span data-ttu-id="5a3e8-193">在 Visual Studio 中按**F5**以建置並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-193">In Visual Studio, press **F5** to build and run the app.</span></span> <span data-ttu-id="5a3e8-194">Web 應用程式啟動之後，請選取**Accept**以接受使用 cookie （若出現提示），然後選取**登入**。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-194">After the web app launches, select **Accept** to accept the use of cookies (if prompted), and then select **Sign in**.</span></span>

![登入應用程式](./azure-ad-b2c/_static/signin.png)

<span data-ttu-id="5a3e8-196">瀏覽器重新導向至 Azure AD B2C 租用戶。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-196">The browser redirects to the Azure AD B2C tenant.</span></span> <span data-ttu-id="5a3e8-197">（如果已建立一個測試原則），使用現有的帳戶登入，或選取**立即註冊**建立新的帳戶。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-197">Sign in with an existing account (if one was created testing the policies) or select **Sign up now** to create a new account.</span></span> <span data-ttu-id="5a3e8-198">**忘記密碼？** 連結用來重設遺忘的密碼。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-198">The **Forgot your password?** link is used to reset a forgotten password.</span></span>

![Azure AD B2C 登入](./azure-ad-b2c/_static/b2csts.png)

<span data-ttu-id="5a3e8-200">已成功登入之後，瀏覽器重新導向至 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-200">After successfully signing in, the browser redirects to the web app.</span></span>

![成功](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a><span data-ttu-id="5a3e8-202">後續步驟</span><span class="sxs-lookup"><span data-stu-id="5a3e8-202">Next steps</span></span>

<span data-ttu-id="5a3e8-203">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="5a3e8-203">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5a3e8-204">建立 Azure Active Directory B2C 租用戶</span><span class="sxs-lookup"><span data-stu-id="5a3e8-204">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="5a3e8-205">在 Azure AD B2C 中註冊應用程式</span><span class="sxs-lookup"><span data-stu-id="5a3e8-205">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="5a3e8-206">使用 Visual Studio 建立 ASP.NET Core Web 應用程式設定為使用 Azure AD B2C 租用戶進行驗證</span><span class="sxs-lookup"><span data-stu-id="5a3e8-206">Use Visual Studio to create an ASP.NET Core Web Application configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="5a3e8-207">設定原則來控制 Azure AD B2C 租用戶行為</span><span class="sxs-lookup"><span data-stu-id="5a3e8-207">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

<span data-ttu-id="5a3e8-208">現在，ASP.NET Core 應用程式設定為使用 Azure AD B2C 進行驗證， [Authorize 屬性](xref:security/authorization/simple)可以用來保護您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-208">Now that the ASP.NET Core app is configured to use Azure AD B2C for authentication, the [Authorize attribute](xref:security/authorization/simple) can be used to secure your app.</span></span> <span data-ttu-id="5a3e8-209">繼續開發您的應用程式，以學習：</span><span class="sxs-lookup"><span data-stu-id="5a3e8-209">Continue developing your app by learning to:</span></span>

* <span data-ttu-id="5a3e8-210">[自訂 Azure AD B2C 使用者介面](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization)。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-210">[Customize the Azure AD B2C user interface](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).</span></span>
* <span data-ttu-id="5a3e8-211">[設定密碼複雜度需求](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity)。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-211">[Configure password complexity requirements](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).</span></span>
* <span data-ttu-id="5a3e8-212">[啟用 multi-factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa)。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-212">[Enable multi-factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).</span></span>
* <span data-ttu-id="5a3e8-213">設定額外的身分識別提供者，例如[Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app)， [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app)， [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app)， [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app)， [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)，和其他人。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-213">Configure additional identity providers, such as [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app), and others.</span></span>
* <span data-ttu-id="5a3e8-214">[使用 Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet)擷取其他使用者資訊，例如群組成員資格，從 Azure AD B2C 租用戶。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-214">[Use the Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) to retrieve additional user information, such as group membership, from the Azure AD B2C tenant.</span></span>
* <span data-ttu-id="5a3e8-215">[保護 ASP.NET Core web API 使用 Azure AD B2C](xref:security/authentication/azure-ad-b2c-webapi)。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-215">[Secure an ASP.NET Core web API using Azure AD B2C](xref:security/authentication/azure-ad-b2c-webapi).</span></span>
* <span data-ttu-id="5a3e8-216">[呼叫.NET web API，從.NET web 應用程式使用 Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet)。</span><span class="sxs-lookup"><span data-stu-id="5a3e8-216">[Call a .NET web API from a .NET web app using Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).</span></span>
