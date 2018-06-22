---
title: Azure Active Directory B2C ASP.NET Core 中使用雲端驗證
author: camsoper
description: 了解如何設定 ASP.NET Core 與 Azure Active Directory B2C 驗證。
ms.date: 01/25/2018
ms.custom: mvc
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: caadeec57272ee2823452ed7c4b91e7aca07c3f4
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272418"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a><span data-ttu-id="fcfa9-103">Azure Active Directory B2C ASP.NET Core 中使用雲端驗證</span><span class="sxs-lookup"><span data-stu-id="fcfa9-103">Cloud authentication with Azure Active Directory B2C in ASP.NET Core</span></span>

<span data-ttu-id="fcfa9-104">作者 [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="fcfa9-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="fcfa9-105">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) 是 web 和行動裝置應用程式的雲端身分識別管理解決方案。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-105">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) is a cloud identity management solution for web and mobile apps.</span></span> <span data-ttu-id="fcfa9-106">服務提供裝載於雲端和內部部署的應用程式的驗證。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-106">The service provides authentication for apps hosted in the cloud and on-premises.</span></span> <span data-ttu-id="fcfa9-107">驗證類型包括個別帳戶，社交網路帳戶，以及同盟企業帳戶。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-107">Authentication types include individual accounts, social network accounts, and federated enterprise accounts.</span></span> <span data-ttu-id="fcfa9-108">此外，Azure AD B2C 可提供多重要素驗證，以最低組態。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-108">Additionally, Azure AD B2C can provide multi-factor authentication with minimal configuration.</span></span>

> [!TIP]
> <span data-ttu-id="fcfa9-109">Azure Active Directory (Azure AD) 的 Azure AD B2C 是個別產品的供應項目。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-109">Azure Active Directory (Azure AD) Azure AD B2C are separate product offerings.</span></span> <span data-ttu-id="fcfa9-110">Azure AD 租用戶代表組織中，而 Azure AD B2C 租用戶代表與信賴憑證者的合作對象應用程式使用的身分識別的集合。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-110">An Azure AD tenant represents an organization, while an Azure AD B2C tenant represents a collection of identities to be used with relying party applications.</span></span> <span data-ttu-id="fcfa9-111">若要進一步了解，請參閱[Azure AD B2C： 常見問題集 (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs)。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-111">To learn more, see [Azure AD B2C: Frequently asked questions (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).</span></span>

<span data-ttu-id="fcfa9-112">在本教學課程，了解如何：</span><span class="sxs-lookup"><span data-stu-id="fcfa9-112">In this tutorial, learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fcfa9-113">建立 Azure Active Directory B2C 租用戶</span><span class="sxs-lookup"><span data-stu-id="fcfa9-113">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="fcfa9-114">在 Azure AD B2C 註冊應用程式</span><span class="sxs-lookup"><span data-stu-id="fcfa9-114">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="fcfa9-115">使用 Visual Studio 來建立 ASP.NET Core web 應用程式設定為使用 Azure AD B2C 租用戶進行驗證</span><span class="sxs-lookup"><span data-stu-id="fcfa9-115">Use Visual Studio to create an ASP.NET Core web app configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="fcfa9-116">設定控制行為的 Azure AD B2C 租用戶的原則</span><span class="sxs-lookup"><span data-stu-id="fcfa9-116">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fcfa9-117">必要條件</span><span class="sxs-lookup"><span data-stu-id="fcfa9-117">Prerequisites</span></span>

<span data-ttu-id="fcfa9-118">此逐步解說需要下列條件：</span><span class="sxs-lookup"><span data-stu-id="fcfa9-118">The following are required for this walkthrough:</span></span>

* [<span data-ttu-id="fcfa9-119">Microsoft Azure 訂用帳戶</span><span class="sxs-lookup"><span data-stu-id="fcfa9-119">Microsoft Azure subscription</span></span>](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* <span data-ttu-id="fcfa9-120">[Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) （任何版本）</span><span class="sxs-lookup"><span data-stu-id="fcfa9-120">[Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (any edition)</span></span>

## <a name="create-the-azure-active-directory-b2c-tenant"></a><span data-ttu-id="fcfa9-121">建立 Azure Active Directory B2C 租用戶</span><span class="sxs-lookup"><span data-stu-id="fcfa9-121">Create the Azure Active Directory B2C tenant</span></span>

<span data-ttu-id="fcfa9-122">建立 Azure Active Directory B2C 租用戶[文件中所述](/azure/active-directory-b2c/active-directory-b2c-get-started)。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-122">Create an Azure Active Directory B2C tenant [as described in the documentation](/azure/active-directory-b2c/active-directory-b2c-get-started).</span></span> <span data-ttu-id="fcfa9-123">出現提示時，將租用戶與 Azure 訂用帳戶產生關聯是選擇性的在本教學課程。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-123">When prompted, associating the tenant with an Azure subscription is optional for this tutorial.</span></span>

## <a name="register-the-app-in-azure-ad-b2c"></a><span data-ttu-id="fcfa9-124">在 Azure AD B2C 中註冊應用程式</span><span class="sxs-lookup"><span data-stu-id="fcfa9-124">Register the app in Azure AD B2C</span></span>

<span data-ttu-id="fcfa9-125">在新建立的 Azure AD B2C 租用戶註冊您的應用程式使用[文件中的步驟](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app)下**註冊 web 應用程式**> 一節。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-125">In the newly created Azure AD B2C tenant, register your app using [the steps in the documentation](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) under the **Register a web app** section.</span></span> <span data-ttu-id="fcfa9-126">在停止**建立 web 應用程式的用戶端密碼**> 一節。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-126">Stop at the **Create a web app client secret** section.</span></span> <span data-ttu-id="fcfa9-127">此教學課程中，不需要用戶端密碼。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-127">A client secret isn't required for this tutorial.</span></span> 

<span data-ttu-id="fcfa9-128">使用下列值：</span><span class="sxs-lookup"><span data-stu-id="fcfa9-128">Use the following values:</span></span>

| <span data-ttu-id="fcfa9-129">設定</span><span class="sxs-lookup"><span data-stu-id="fcfa9-129">Setting</span></span>                       | <span data-ttu-id="fcfa9-130">值</span><span class="sxs-lookup"><span data-stu-id="fcfa9-130">Value</span></span>                     | <span data-ttu-id="fcfa9-131">注意</span><span class="sxs-lookup"><span data-stu-id="fcfa9-131">Notes</span></span>                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="fcfa9-132">**名稱**</span><span class="sxs-lookup"><span data-stu-id="fcfa9-132">**Name**</span></span>                      | <span data-ttu-id="fcfa9-133">*&lt;應用程式名稱&gt;*</span><span class="sxs-lookup"><span data-stu-id="fcfa9-133">*&lt;app name&gt;*</span></span>        | <span data-ttu-id="fcfa9-134">輸入**名稱**描述給取用者應用程式的應用程式。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-134">Enter a **Name** for the app that describes your app to consumers.</span></span>                                                                                                                                 |
| <span data-ttu-id="fcfa9-135">**包含 web 應用程式/web 應用程式開發介面**</span><span class="sxs-lookup"><span data-stu-id="fcfa9-135">**Include web app / web API**</span></span> | <span data-ttu-id="fcfa9-136">[是]</span><span class="sxs-lookup"><span data-stu-id="fcfa9-136">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="fcfa9-137">**允許隱含流程**</span><span class="sxs-lookup"><span data-stu-id="fcfa9-137">**Allow implicit flow**</span></span>       | <span data-ttu-id="fcfa9-138">[是]</span><span class="sxs-lookup"><span data-stu-id="fcfa9-138">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="fcfa9-139">**回覆 URL**</span><span class="sxs-lookup"><span data-stu-id="fcfa9-139">**Reply URL**</span></span>                 | `https://localhost:44300` | <span data-ttu-id="fcfa9-140">回覆 Url 是端點，其中是 Azure AD B2C 會傳回任何您的應用程式要求的權杖。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-140">Reply URLs are endpoints where Azure AD B2C returns any tokens that your app requests.</span></span> <span data-ttu-id="fcfa9-141">Visual Studio 提供要使用的回覆 URL。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-141">Visual Studio provides the Reply URL to use.</span></span> <span data-ttu-id="fcfa9-142">現在請輸入`https://localhost:44300`完成表單。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-142">For now, enter `https://localhost:44300` to complete the form.</span></span> |
| <span data-ttu-id="fcfa9-143">**應用程式識別碼 URI**</span><span class="sxs-lookup"><span data-stu-id="fcfa9-143">**App ID URI**</span></span>                | <span data-ttu-id="fcfa9-144">保留空白</span><span class="sxs-lookup"><span data-stu-id="fcfa9-144">Leave blank</span></span>               | <span data-ttu-id="fcfa9-145">本教學課程中，不需要。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-145">Not required for this tutorial.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="fcfa9-146">**包含原生用戶端**</span><span class="sxs-lookup"><span data-stu-id="fcfa9-146">**Include native client**</span></span>     | <span data-ttu-id="fcfa9-147">否</span><span class="sxs-lookup"><span data-stu-id="fcfa9-147">No</span></span>                        |                                                                                                                                                                                                    |

> [!WARNING]
> <span data-ttu-id="fcfa9-148">如果設定非 localhost 回覆 URL，請留意[回覆 URL 清單中所允許的條件約束](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url)。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-148">If setting up a non-localhost Reply URL, be aware of the [constraints on what is allowed in the Reply URL list](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url).</span></span> 

<span data-ttu-id="fcfa9-149">註冊應用程式之後，會顯示租用戶中的應用程式的清單。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-149">After the app is registered, the list of apps in the tenant is displayed.</span></span> <span data-ttu-id="fcfa9-150">選取剛才已註冊的應用程式。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-150">Select the app that was just registered.</span></span> <span data-ttu-id="fcfa9-151">選取**複製**圖示右邊的**應用程式識別碼**欄位，以將它複製到剪貼簿。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-151">Select the **Copy** icon to the right of the **Application ID** field to copy it to the clipboard.</span></span>

<span data-ttu-id="fcfa9-152">沒有項目比較可以在 Azure AD B2C 租用戶中設定在這個階段，但讓瀏覽器視窗保持開啟。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-152">Nothing more can be configured in the Azure AD B2C tenant at this time, but leave the browser window open.</span></span> <span data-ttu-id="fcfa9-153">在建立 ASP.NET Core 應用程式後，就更多的設定。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-153">There is more configuration after the ASP.NET Core app is created.</span></span>

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a><span data-ttu-id="fcfa9-154">在 Visual Studio 2017 中建立 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="fcfa9-154">Create an ASP.NET Core app in Visual Studio 2017</span></span>

<span data-ttu-id="fcfa9-155">Visual Studio Web 應用程式範本可以設定為使用 Azure AD B2C 租用戶進行驗證。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-155">The Visual Studio Web Application template can be configured to use the Azure AD B2C tenant for authentication.</span></span>

<span data-ttu-id="fcfa9-156">在 Visual Studio 中：</span><span class="sxs-lookup"><span data-stu-id="fcfa9-156">In Visual Studio:</span></span>

1. <span data-ttu-id="fcfa9-157">建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-157">Create a new ASP.NET Core Web Application.</span></span> 
2. <span data-ttu-id="fcfa9-158">選取**Web 應用程式**從範本清單。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-158">Select **Web Application** from the list of templates.</span></span>
3. <span data-ttu-id="fcfa9-159">選取**變更驗證** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-159">Select the **Change Authentication** button.</span></span>
    
    ![變更 [驗證] 按鈕](./azure-ad-b2c/_static/changeauth.png)

4. <span data-ttu-id="fcfa9-161">在**變更驗證**對話方塊中，選取**個別使用者帳戶**，然後選取**連接到雲端中現有的使用者存放區**下拉式清單中。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-161">In the **Change Authentication** dialog, select **Individual User Accounts**, and then select **Connect to an existing user store in the cloud** in the dropdown.</span></span> 
    
    ![變更 [驗證] 對話方塊](./azure-ad-b2c/_static/changeauthdialog.png)

5. <span data-ttu-id="fcfa9-163">完成表單具有下列值：</span><span class="sxs-lookup"><span data-stu-id="fcfa9-163">Complete the form with the following values:</span></span>
    
    | <span data-ttu-id="fcfa9-164">設定</span><span class="sxs-lookup"><span data-stu-id="fcfa9-164">Setting</span></span>                       | <span data-ttu-id="fcfa9-165">值</span><span class="sxs-lookup"><span data-stu-id="fcfa9-165">Value</span></span>                                                 |
    |-------------------------------|-------------------------------------------------------|
    | <span data-ttu-id="fcfa9-166">**網域名稱**</span><span class="sxs-lookup"><span data-stu-id="fcfa9-166">**Domain Name**</span></span>               | <span data-ttu-id="fcfa9-167">*&lt;B2C 租用戶的網域名稱&gt;*</span><span class="sxs-lookup"><span data-stu-id="fcfa9-167">*&lt;the domain name of your B2C tenant&gt;*</span></span>          |
    | <span data-ttu-id="fcfa9-168">**應用程式識別碼**</span><span class="sxs-lookup"><span data-stu-id="fcfa9-168">**Application ID**</span></span>            | <span data-ttu-id="fcfa9-169">*&lt;貼上剪貼簿中的應用程式識別碼&gt;*</span><span class="sxs-lookup"><span data-stu-id="fcfa9-169">*&lt;paste the Application ID from the clipboard&gt;*</span></span> |
    | <span data-ttu-id="fcfa9-170">**回呼路徑**</span><span class="sxs-lookup"><span data-stu-id="fcfa9-170">**Callback Path**</span></span>             | <span data-ttu-id="fcfa9-171">*&lt;使用預設值&gt;*</span><span class="sxs-lookup"><span data-stu-id="fcfa9-171">*&lt;use the default value&gt;*</span></span>                       |
    | <span data-ttu-id="fcfa9-172">**註冊或登入的原則**</span><span class="sxs-lookup"><span data-stu-id="fcfa9-172">**Sign-up or sign-in policy**</span></span> | `B2C_1_SiUpIn`                                        |
    | <span data-ttu-id="fcfa9-173">**重設密碼原則**</span><span class="sxs-lookup"><span data-stu-id="fcfa9-173">**Reset password policy**</span></span>     | `B2C_1_SSPR`                                          |
    | <span data-ttu-id="fcfa9-174">**編輯設定檔原則**</span><span class="sxs-lookup"><span data-stu-id="fcfa9-174">**Edit profile policy**</span></span>       | <span data-ttu-id="fcfa9-175">*&lt;保留空白&gt;*</span><span class="sxs-lookup"><span data-stu-id="fcfa9-175">*&lt;leave blank&gt;*</span></span>                                 |
    
    <span data-ttu-id="fcfa9-176">選取**複製**連結**回覆 URI**複製到剪貼簿的回覆 URI。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-176">Select the **Copy** link next to **Reply URI** to copy the Reply URI to the clipboard.</span></span> <span data-ttu-id="fcfa9-177">選取**確定**關閉**變更驗證**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-177">Select **OK** to close the **Change Authentication** dialog.</span></span> <span data-ttu-id="fcfa9-178">選取**確定**建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-178">Select **OK** to create the web app.</span></span>

## <a name="finish-the-b2c-app-registration"></a><span data-ttu-id="fcfa9-179">完成 B2C 應用程式註冊</span><span class="sxs-lookup"><span data-stu-id="fcfa9-179">Finish the B2C app registration</span></span>

<span data-ttu-id="fcfa9-180">傳回與 B2C 應用程式屬性仍開啟的瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-180">Return to the browser window with the B2C app properties still open.</span></span> <span data-ttu-id="fcfa9-181">變更暫存**回覆 URL**指定從 Visual Studio 的較早的值複製。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-181">Change the temporary **Reply URL** specified earlier to the value copied from Visual Studio.</span></span> <span data-ttu-id="fcfa9-182">選取**儲存**視窗的頂端。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-182">Select **Save** at the top of the window.</span></span>

> [!TIP]
> <span data-ttu-id="fcfa9-183">如果您沒有複製回覆 URL，在 web 專案屬性中，使用 SSL 位址，從 [偵錯] 索引標籤，並附加**CallbackPath**值*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-183">If you didn't copy the Reply URL, use the SSL address from the Debug tab in the web project properties, and append the **CallbackPath** value from *appsettings.json*.</span></span>

## <a name="configure-policies"></a><span data-ttu-id="fcfa9-184">設定原則</span><span class="sxs-lookup"><span data-stu-id="fcfa9-184">Configure policies</span></span>

<span data-ttu-id="fcfa9-185">使用 Azure AD B2C 文件中的步驟[建立註冊或登入的原則](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy)，然後[建立密碼重設原則](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy)。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-185">Use the steps in the Azure AD B2C documentation to [create a sign-up or sign-in policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy), and then [create a password reset policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy).</span></span> <span data-ttu-id="fcfa9-186">使用提供的文件中的範例值**身分識別提供者**，**註冊屬性**，和**應用程式宣告**。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-186">Use the example values provided in the documentation for **Identity providers**, **Sign-up attributes**, and **Application claims**.</span></span> <span data-ttu-id="fcfa9-187">使用**立即執行**是選擇性的按鈕來測試原則文件中所述。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-187">Using the **Run now** button to test the policies as described in the documentation is optional.</span></span>

> [!WARNING]
> <span data-ttu-id="fcfa9-188">確認原則名稱在中使用這些原則的文件中所述完全是**變更驗證**Visual Studio 中的對話方塊。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-188">Ensure the policy names are exactly as described in the documentation, as those policies were used in the **Change Authentication** dialog in Visual Studio.</span></span> <span data-ttu-id="fcfa9-189">原則名稱可以驗證在*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-189">The policy names can be verified in *appsettings.json*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="fcfa9-190">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="fcfa9-190">Run the app</span></span>

<span data-ttu-id="fcfa9-191">在 Visual Studio 中，按**F5**建置並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-191">In Visual Studio, press **F5** to build and run the app.</span></span> <span data-ttu-id="fcfa9-192">Web 應用程式啟動之後，請選取**登入**。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-192">After the web app launches, select **Sign in**.</span></span>

![登入應用程式](./azure-ad-b2c/_static/signin.png)

<span data-ttu-id="fcfa9-194">瀏覽器重新導向至 Azure AD B2C 租用戶。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-194">The browser redirects to the Azure AD B2C tenant.</span></span> <span data-ttu-id="fcfa9-195">（如果已建立一個測試原則），使用現有的帳戶登入，或選取**立即註冊**來建立新的帳戶。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-195">Sign in with an existing account (if one was created testing the policies) or select **Sign up now** to create a new account.</span></span> <span data-ttu-id="fcfa9-196">**忘記密碼？** 連結用來重設忘記的密碼。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-196">The **Forgot your password?** link is used to reset a forgotten password.</span></span>

![Azure AD B2C 登入](./azure-ad-b2c/_static/b2csts.png)

<span data-ttu-id="fcfa9-198">已成功登入之後，瀏覽器重新導向至 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-198">After successfully signing in, the browser redirects to the web app.</span></span>

![成功](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a><span data-ttu-id="fcfa9-200">後續步驟</span><span class="sxs-lookup"><span data-stu-id="fcfa9-200">Next steps</span></span>

<span data-ttu-id="fcfa9-201">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="fcfa9-201">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fcfa9-202">建立 Azure Active Directory B2C 租用戶</span><span class="sxs-lookup"><span data-stu-id="fcfa9-202">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="fcfa9-203">在 Azure AD B2C 註冊應用程式</span><span class="sxs-lookup"><span data-stu-id="fcfa9-203">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="fcfa9-204">使用 Visual Studio 來建立 ASP.NET Core Web 應用程式設定為使用 Azure AD B2C 租用戶進行驗證</span><span class="sxs-lookup"><span data-stu-id="fcfa9-204">Use Visual Studio to create an ASP.NET Core Web Application configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="fcfa9-205">設定控制行為的 Azure AD B2C 租用戶的原則</span><span class="sxs-lookup"><span data-stu-id="fcfa9-205">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

<span data-ttu-id="fcfa9-206">既然 ASP.NET Core 應用程式設定為進行驗證，使用 Azure AD B2C [Authorize 屬性](xref:security/authorization/simple)可用來保護您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-206">Now that the ASP.NET Core app is configured to use Azure AD B2C for authentication, the [Authorize attribute](xref:security/authorization/simple) can be used to secure your app.</span></span> <span data-ttu-id="fcfa9-207">繼續開發您的應用程式所學習：</span><span class="sxs-lookup"><span data-stu-id="fcfa9-207">Continue developing your app by learning to:</span></span>

* <span data-ttu-id="fcfa9-208">[自訂使用者介面，Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization)。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-208">[Customize the Azure AD B2C user interface](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).</span></span>
* <span data-ttu-id="fcfa9-209">[設定密碼複雜性需求](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity)。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-209">[Configure password complexity requirements](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).</span></span>
* <span data-ttu-id="fcfa9-210">[啟用多因素驗證](/azure/active-directory-b2c/active-directory-b2c-reference-mfa)。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-210">[Enable multi-factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).</span></span>
* <span data-ttu-id="fcfa9-211">設定其他身分識別提供者，例如[Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app)， [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app)， [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app)， [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app)， [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)，和其他人。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-211">Configure additional identity providers, such as [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app), and others.</span></span>
* <span data-ttu-id="fcfa9-212">[使用 Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet)擷取額外的使用者資訊，例如群組成員資格，從 Azure AD B2C 租用戶。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-212">[Use the Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) to retrieve additional user information, such as group membership, from the Azure AD B2C tenant.</span></span>
* <span data-ttu-id="fcfa9-213">[安全 ASP.NET Core web 應用程式開發介面使用 Azure AD B2C](xref:security/authentication/azure-ad-b2c-webapi)。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-213">[Secure an ASP.NET Core web API using Azure AD B2C](xref:security/authentication/azure-ad-b2c-webapi).</span></span>
* <span data-ttu-id="fcfa9-214">[從.NET web 應用程式中使用 Azure AD B2C 呼叫.NET web 應用程式開發介面](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet)。</span><span class="sxs-lookup"><span data-stu-id="fcfa9-214">[Call a .NET web API from a .NET web app using Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).</span></span>
