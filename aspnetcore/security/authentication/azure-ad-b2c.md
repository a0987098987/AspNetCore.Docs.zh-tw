---
title: "使用 Azure Active Directory B2C 的雲端驗證"
author: camsoper
description: "了解如何設定 ASP.NET Core 與 Azure Active Directory B2C 驗證。"
ms.author: casoper
manager: wpickett
ms.date: 01/12/2018
ms.topic: tutorial
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/azure-ad-b2c
custom: mvc
ms.openlocfilehash: 5c4716022c61e33b0301fa0077f911dcc4b3628c
ms.sourcegitcommit: 459cb3289741a3f46325e605a617dc926ee0563d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/22/2018
---
# <a name="cloud-authentication-with-azure-active-directory-b2c"></a><span data-ttu-id="a6d7f-103">使用 Azure Active Directory B2C 的雲端驗證</span><span class="sxs-lookup"><span data-stu-id="a6d7f-103">Cloud authentication with Azure Active Directory B2C</span></span>

<span data-ttu-id="a6d7f-104">作者 [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="a6d7f-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="a6d7f-105">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) 是您的 web 與行動裝置應用程式的雲端身分識別管理解決方案。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-105">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) is a cloud identity management solution for your web and mobile apps.</span></span> <span data-ttu-id="a6d7f-106">服務提供裝載於雲端和內部部署的應用程式的驗證。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-106">The service provides authentication for apps hosted in the cloud and on-premises.</span></span> <span data-ttu-id="a6d7f-107">驗證類型包括包含個別帳戶，社交網路帳戶以及同盟企業帳戶。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-107">Authentication types include include individual accounts, social network accounts, and federated enterprise accounts.</span></span>  <span data-ttu-id="a6d7f-108">此外，Azure AD B2C 可提供多重要素驗證，以最低組態。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-108">Additionally, Azure AD B2C can provide multi-factor authentication with minimal configuration.</span></span>

<span data-ttu-id="a6d7f-109">在本教學課程，了解如何：</span><span class="sxs-lookup"><span data-stu-id="a6d7f-109">In this tutorial, learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a6d7f-110">建立 Azure Active Directory B2C 租用戶</span><span class="sxs-lookup"><span data-stu-id="a6d7f-110">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="a6d7f-111">在 Azure AD B2C 註冊應用程式</span><span class="sxs-lookup"><span data-stu-id="a6d7f-111">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="a6d7f-112">使用 Visual Studio 來建立 ASP.NET Core web 應用程式設定為使用 Azure AD B2C 租用戶進行驗證</span><span class="sxs-lookup"><span data-stu-id="a6d7f-112">Use Visual Studio to create an ASP.NET Core web app configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="a6d7f-113">設定控制行為的 Azure AD B2C 租用戶的原則</span><span class="sxs-lookup"><span data-stu-id="a6d7f-113">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a6d7f-114">必要條件</span><span class="sxs-lookup"><span data-stu-id="a6d7f-114">Prerequisites</span></span>

<span data-ttu-id="a6d7f-115">此逐步解說需要下列條件：</span><span class="sxs-lookup"><span data-stu-id="a6d7f-115">The following are required for this walkthrough:</span></span>

* <span data-ttu-id="a6d7f-116">[Microsoft Azure 訂用帳戶](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-116">[Microsoft Azure subscription](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).</span></span> 
* <span data-ttu-id="a6d7f-117">[Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) （任何版本）</span><span class="sxs-lookup"><span data-stu-id="a6d7f-117">[Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (any edition)</span></span>

## <a name="create-the-azure-active-directory-b2c-tenant"></a><span data-ttu-id="a6d7f-118">建立 Azure Active Directory B2C 租用戶</span><span class="sxs-lookup"><span data-stu-id="a6d7f-118">Create the Azure Active Directory B2C tenant</span></span>

<span data-ttu-id="a6d7f-119">建立 Azure Active Directory B2C 租用戶[文件中所述](/azure/active-directory-b2c/active-directory-b2c-get-started)。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-119">Create an Azure Active Directory B2C tenant [as described in the documentation](/azure/active-directory-b2c/active-directory-b2c-get-started).</span></span> <span data-ttu-id="a6d7f-120">出現提示時，將租用戶與 Azure 訂用帳戶產生關聯是選擇性的在本教學課程。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-120">When prompted, associating the tenant with an Azure subscription is optional for this tutorial.</span></span>

## <a name="register-the-app-in-azure-ad-b2c"></a><span data-ttu-id="a6d7f-121">在 Azure AD B2C 中註冊應用程式</span><span class="sxs-lookup"><span data-stu-id="a6d7f-121">Register the app in Azure AD B2C</span></span>

<span data-ttu-id="a6d7f-122">在新建立的 Azure AD B2C 租用戶註冊您的應用程式使用[文件中的步驟](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app)下**註冊 web 應用程式**> 一節。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-122">In the newly created Azure AD B2C tenant, register your app using [the steps in the documentation](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) under the **Register a web app** section.</span></span> <span data-ttu-id="a6d7f-123">在停止**建立 web 應用程式的用戶端密碼**> 一節。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-123">Stop at the **Create a web app client secret** section.</span></span> <span data-ttu-id="a6d7f-124">此教學課程中，不需要用戶端密碼。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-124">A client secret isn't required for this tutorial.</span></span> 

<span data-ttu-id="a6d7f-125">使用下列值：</span><span class="sxs-lookup"><span data-stu-id="a6d7f-125">Use the following values:</span></span>

| <span data-ttu-id="a6d7f-126">設定</span><span class="sxs-lookup"><span data-stu-id="a6d7f-126">Setting</span></span>                       | <span data-ttu-id="a6d7f-127">值</span><span class="sxs-lookup"><span data-stu-id="a6d7f-127">Value</span></span>                     | <span data-ttu-id="a6d7f-128">注意</span><span class="sxs-lookup"><span data-stu-id="a6d7f-128">Notes</span></span>                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a6d7f-129">**名稱**</span><span class="sxs-lookup"><span data-stu-id="a6d7f-129">**Name**</span></span>                      | <span data-ttu-id="a6d7f-130">*\<應用程式名稱\>*</span><span class="sxs-lookup"><span data-stu-id="a6d7f-130">*\<app name\>*</span></span>            | <span data-ttu-id="a6d7f-131">輸入**名稱**描述給取用者應用程式的應用程式。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-131">Enter a **Name** for the app that describes your app to consumers.</span></span>                                                                                                                                 |
| <span data-ttu-id="a6d7f-132">**包含 web 應用程式/web 應用程式開發介面**</span><span class="sxs-lookup"><span data-stu-id="a6d7f-132">**Include web app / web API**</span></span> | <span data-ttu-id="a6d7f-133">[是]</span><span class="sxs-lookup"><span data-stu-id="a6d7f-133">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="a6d7f-134">**允許隱含流程**</span><span class="sxs-lookup"><span data-stu-id="a6d7f-134">**Allow implicit flow**</span></span>       | <span data-ttu-id="a6d7f-135">[是]</span><span class="sxs-lookup"><span data-stu-id="a6d7f-135">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="a6d7f-136">**回覆 URL**</span><span class="sxs-lookup"><span data-stu-id="a6d7f-136">**Reply URL**</span></span>                 | `https://localhost:44300` | <span data-ttu-id="a6d7f-137">回覆 Url 是端點，其中是 Azure AD B2C 會傳回任何您的應用程式要求的權杖。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-137">Reply URLs are endpoints where Azure AD B2C returns any tokens that your app requests.</span></span> <span data-ttu-id="a6d7f-138">Visual Studio 提供要使用的回覆 URL。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-138">Visual Studio provides the Reply URL to use.</span></span> <span data-ttu-id="a6d7f-139">現在請輸入`https://localhost:44300`完成表單。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-139">For now, enter `https://localhost:44300` to complete the form.</span></span> |
| <span data-ttu-id="a6d7f-140">**應用程式識別碼 URI**</span><span class="sxs-lookup"><span data-stu-id="a6d7f-140">**App ID URI**</span></span>                | <span data-ttu-id="a6d7f-141">保留空白</span><span class="sxs-lookup"><span data-stu-id="a6d7f-141">Leave blank</span></span>               | <span data-ttu-id="a6d7f-142">本教學課程中，不需要。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-142">Not required for this tutorial.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="a6d7f-143">**包含原生用戶端**</span><span class="sxs-lookup"><span data-stu-id="a6d7f-143">**Include native client**</span></span>     | <span data-ttu-id="a6d7f-144">否</span><span class="sxs-lookup"><span data-stu-id="a6d7f-144">No</span></span>                        |                                                                                                                                                                                                    |

> [!WARNING]
> <span data-ttu-id="a6d7f-145">如果設定非 localhost 回覆 URL，請留意[回覆 URL 清單中所允許的條件約束](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url)。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-145">If setting up a non-localhost Reply URL, be aware of the [constraints on what is allowed in the Reply URL list](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url).</span></span> 

<span data-ttu-id="a6d7f-146">註冊應用程式之後，會顯示租用戶中的應用程式的清單。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-146">After the app is registered, the list of apps in the tenant is displayed.</span></span> <span data-ttu-id="a6d7f-147">選取剛才已註冊的應用程式。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-147">Select the app that was just registered.</span></span> <span data-ttu-id="a6d7f-148">選取**複製**圖示右邊的**應用程式識別碼**欄位複製到剪貼簿的應用程式識別碼。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-148">Select the **Copy** icon to the right of the **Application ID** field to copy the Application ID to the clipboard.</span></span>

<span data-ttu-id="a6d7f-149">沒有項目比較可以在 Azure AD B2C 租用戶中設定在這個階段，但讓瀏覽器視窗保持開啟。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-149">Nothing more can be configured in the Azure AD B2C tenant at this time, but leave the browser window open.</span></span> <span data-ttu-id="a6d7f-150">在建立 ASP.NET Core 應用程式後，就更多的設定。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-150">There is more configuration after the ASP.NET Core app is created.</span></span>

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a><span data-ttu-id="a6d7f-151">在 Visual Studio 2017 中建立 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="a6d7f-151">Create an ASP.NET Core app in Visual Studio 2017</span></span>

<span data-ttu-id="a6d7f-152">Visual Studio Web 應用程式範本可以設定為使用 Azure AD B2C 租用戶進行驗證。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-152">The Visual Studio Web Application template can be configured to use the Azure AD B2C tenant for authentication.</span></span>

<span data-ttu-id="a6d7f-153">在 Visual Studio 中：</span><span class="sxs-lookup"><span data-stu-id="a6d7f-153">In Visual Studio:</span></span>

1. <span data-ttu-id="a6d7f-154">建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-154">Create a new ASP.NET Core Web Application.</span></span> 
2. <span data-ttu-id="a6d7f-155">選取**Web 應用程式**從範本清單。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-155">Select **Web Application** from the list of templates.</span></span>
3. <span data-ttu-id="a6d7f-156">選取**變更驗證** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-156">Select the **Change Authentication** button.</span></span>
    
    ![變更 [驗證] 按鈕](./azure-ad-b2c/_static/changeauth.png)

4. <span data-ttu-id="a6d7f-158">在**變更驗證**對話方塊中，選取**個別使用者帳戶**，然後選取**連接到雲端中現有的使用者存放區**下拉式清單中。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-158">In the **Change Authentication** dialog, select **Individual User Accounts**, and then select **Connect to an existing user store in the cloud** in the dropdown.</span></span> 
    
    ![變更 [驗證] 對話方塊](./azure-ad-b2c/_static/changeauthdialog.png)

5. <span data-ttu-id="a6d7f-160">完成表單具有下列值：</span><span class="sxs-lookup"><span data-stu-id="a6d7f-160">Complete the form with the following values:</span></span>
    
    | <span data-ttu-id="a6d7f-161">設定</span><span class="sxs-lookup"><span data-stu-id="a6d7f-161">Setting</span></span>                       | <span data-ttu-id="a6d7f-162">值</span><span class="sxs-lookup"><span data-stu-id="a6d7f-162">Value</span></span>                                             |
    |-------------------------------|---------------------------------------------------|
    | <span data-ttu-id="a6d7f-163">**網域名稱**</span><span class="sxs-lookup"><span data-stu-id="a6d7f-163">**Domain Name**</span></span>               | <span data-ttu-id="a6d7f-164">*\<B2C 租用戶的網域名稱\>*</span><span class="sxs-lookup"><span data-stu-id="a6d7f-164">*\<the domain name of your B2C tenant\>*</span></span>          |
    | <span data-ttu-id="a6d7f-165">**應用程式識別碼**</span><span class="sxs-lookup"><span data-stu-id="a6d7f-165">**Application ID**</span></span>            | <span data-ttu-id="a6d7f-166">*\<貼上剪貼簿中的應用程式識別碼\>*</span><span class="sxs-lookup"><span data-stu-id="a6d7f-166">*\<paste the Application ID from the clipboard\>*</span></span> |
    | <span data-ttu-id="a6d7f-167">**回呼路徑**</span><span class="sxs-lookup"><span data-stu-id="a6d7f-167">**Callback Path**</span></span>             | <span data-ttu-id="a6d7f-168">*\<使用預設值\>*</span><span class="sxs-lookup"><span data-stu-id="a6d7f-168">*\<use the default value\>*</span></span>                       |
    | <span data-ttu-id="a6d7f-169">**註冊或登入的原則**</span><span class="sxs-lookup"><span data-stu-id="a6d7f-169">**Sign-up or sign-in policy**</span></span> | `B2C_1_SiUpIn`                                    |
    | <span data-ttu-id="a6d7f-170">**重設密碼原則**</span><span class="sxs-lookup"><span data-stu-id="a6d7f-170">**Reset password policy**</span></span>     | `B2C_1_SSPR`                                      |
    | <span data-ttu-id="a6d7f-171">**編輯設定檔原則**</span><span class="sxs-lookup"><span data-stu-id="a6d7f-171">**Edit profile policy**</span></span>       | <span data-ttu-id="a6d7f-172">*\<保留空白\>*</span><span class="sxs-lookup"><span data-stu-id="a6d7f-172">*\<leave blank\>*</span></span>                                 |

    <span data-ttu-id="a6d7f-173">選取**複製**連結**回覆 URI**複製到剪貼簿的回覆 URI。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-173">Select the **Copy** link next to **Reply URI** to copy the Reply URI to the clipboard.</span></span> <span data-ttu-id="a6d7f-174">選取**確定**關閉**變更驗證**對話方塊。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-174">Select **OK** to close the **Change Authentication** dialog.</span></span> <span data-ttu-id="a6d7f-175">選取**確定**建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-175">Select **OK** to create the web app.</span></span>

## <a name="finish-the-b2c-app-registration"></a><span data-ttu-id="a6d7f-176">完成 B2C 應用程式註冊</span><span class="sxs-lookup"><span data-stu-id="a6d7f-176">Finish the B2C app registration</span></span>

<span data-ttu-id="a6d7f-177">傳回與 B2C 應用程式屬性仍開啟的瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-177">Return to the browser window with the B2C app properties still open.</span></span> <span data-ttu-id="a6d7f-178">變更暫存**回覆 URL**指定從 Visual Studio 的較早的值複製。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-178">Change the temporary **Reply URL** specified earlier to the value copied from Visual Studio.</span></span> <span data-ttu-id="a6d7f-179">選取**儲存**視窗的頂端。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-179">Select **Save** at the top of the window.</span></span>

> [!TIP]
> <span data-ttu-id="a6d7f-180">如果您沒有複製回覆 URL，在 web 專案屬性中，使用 SSL 位址，從 [偵錯] 索引標籤，並附加**CallbackPath**值*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-180">If you didn't copy the Reply URL, use the SSL address from the Debug tab in the web project properties, and append the **CallbackPath** value from *appsettings.json*.</span></span>

## <a name="configure-policies"></a><span data-ttu-id="a6d7f-181">設定原則</span><span class="sxs-lookup"><span data-stu-id="a6d7f-181">Configure policies</span></span>

<span data-ttu-id="a6d7f-182">使用 Azure AD B2C 文件中的步驟[建立註冊或登入的原則](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy)，然後[建立密碼重設原則](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy)。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-182">Use the steps in the Azure AD B2C documentation to [create a sign-up or sign-in policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy), and then [create a password reset policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy).</span></span> <span data-ttu-id="a6d7f-183">使用提供的文件中的範例值**身分識別提供者**，**註冊屬性**，和**應用程式宣告**。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-183">Use the example values provided in the documentation for **Identity providers**, **Sign-up attributes**, and **Application claims**.</span></span> <span data-ttu-id="a6d7f-184">使用**立即執行**是選擇性的按鈕來測試原則文件中所述。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-184">Using the **Run now** button to test the policies as described in the documentation is optional.</span></span>

> [!WARNING]
> <span data-ttu-id="a6d7f-185">確認原則名稱在中使用這些原則的文件中所述完全是**變更驗證**Visual Studio 中的對話方塊。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-185">Ensure the policy names are exactly as described in the documentation, as those policies were used in the **Change Authentication** dialog in Visual Studio.</span></span> <span data-ttu-id="a6d7f-186">原則名稱可以驗證在*appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-186">The policy names can be verified in *appsettings.json*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="a6d7f-187">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="a6d7f-187">Run the app</span></span>

<span data-ttu-id="a6d7f-188">在 Visual Studio 中，按**F5**建置並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-188">In Visual Studio, press **F5** to build and run the app.</span></span> <span data-ttu-id="a6d7f-189">Web 應用程式啟動之後，請選取**登入**。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-189">After the web app launches, select **Sign in**.</span></span>

![登入應用程式](./azure-ad-b2c/_static/signin.png)

<span data-ttu-id="a6d7f-191">瀏覽器重新導向至 Azure AD B2C 租用戶。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-191">The browser redirects to the Azure AD B2C tenant.</span></span> <span data-ttu-id="a6d7f-192">（如果已建立一個測試原則），使用現有的帳戶登入，或選取**立即註冊**來建立新的帳戶。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-192">Sign in with an existing account (if one was created testing the policies) or select **Sign up now** to create a new account.</span></span> <span data-ttu-id="a6d7f-193">**忘記密碼？**連結用來重設忘記的密碼。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-193">The **Forgot your password?** link is used to reset a forgotten password.</span></span>

![Azure AD B2C 登入](./azure-ad-b2c/_static/b2csts.png)

<span data-ttu-id="a6d7f-195">已成功登入之後，瀏覽器重新導向至 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-195">After successfully signing in, the browser redirects to the web app.</span></span>

![成功](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a><span data-ttu-id="a6d7f-197">後續步驟</span><span class="sxs-lookup"><span data-stu-id="a6d7f-197">Next steps</span></span>

<span data-ttu-id="a6d7f-198">在本教學課程中，您將學習如何：</span><span class="sxs-lookup"><span data-stu-id="a6d7f-198">In this tutorial, you will learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a6d7f-199">建立 Azure Active Directory B2C 租用戶</span><span class="sxs-lookup"><span data-stu-id="a6d7f-199">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="a6d7f-200">在 Azure AD B2C 註冊應用程式</span><span class="sxs-lookup"><span data-stu-id="a6d7f-200">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="a6d7f-201">使用 Visual Studio 來建立 ASP.NET Core Web 應用程式設定為使用 Azure AD B2C 租用戶進行驗證</span><span class="sxs-lookup"><span data-stu-id="a6d7f-201">Use Visual Studio to create an ASP.NET Core Web Application configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="a6d7f-202">設定控制行為的 Azure AD B2C 租用戶的原則</span><span class="sxs-lookup"><span data-stu-id="a6d7f-202">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

<span data-ttu-id="a6d7f-203">既然 ASP.NET Core 應用程式設定為進行驗證，使用 Azure AD B2C [Authorize 屬性](xref:security/authorization/simple)可用來保護您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-203">Now that the ASP.NET Core app is configured to use Azure AD B2C for authentication, the [Authorize attribute](xref:security/authorization/simple) can be used to secure your app.</span></span> <span data-ttu-id="a6d7f-204">繼續開發您的應用程式所學習：</span><span class="sxs-lookup"><span data-stu-id="a6d7f-204">Continue developing your app by learning to:</span></span>

* <span data-ttu-id="a6d7f-205">[自訂使用者介面，Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization)。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-205">[Customize the Azure AD B2C user interface](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).</span></span>
* <span data-ttu-id="a6d7f-206">[設定密碼複雜性需求](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity)。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-206">[Configure password complexity requirements](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).</span></span>
* <span data-ttu-id="a6d7f-207">[啟用多因素驗證](/azure/active-directory-b2c/active-directory-b2c-reference-mfa)。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-207">[Enable multi-factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).</span></span>
* <span data-ttu-id="a6d7f-208">設定其他身分識別提供者，例如[Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app)， [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app)， [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app)， [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app)， [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)，和其他人。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-208">Configure additional identity providers, such as [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app), and others.</span></span>
* <span data-ttu-id="a6d7f-209">[使用 Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet)擷取額外的使用者資訊，例如群組成員資格，從 Azure AD B2C 租用戶。</span><span class="sxs-lookup"><span data-stu-id="a6d7f-209">[Use the Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) to retrieve additional user information, such as group membership, from the Azure AD B2C tenant.</span></span>