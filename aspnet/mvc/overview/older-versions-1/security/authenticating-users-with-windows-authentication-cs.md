---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
title: 驗證使用者使用 Windows 驗證 (C#) |Microsoft 文件
author: microsoft
description: 了解如何在 MVC 應用程式的內容中使用 Windows 驗證。 您了解如何啟用 Windows 驗證，在您的應用程式 web co...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 418bb07e-f369-4119-b4b0-08f890f7abb2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: 140d2232f7826e178301d1d2064e12657be23385
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875379"
---
<a name="authenticating-users-with-windows-authentication-c"></a><span data-ttu-id="b7397-104">驗證使用者使用 Windows 驗證 (C#)</span><span class="sxs-lookup"><span data-stu-id="b7397-104">Authenticating Users with Windows Authentication (C#)</span></span>
====================
<span data-ttu-id="b7397-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="b7397-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="b7397-106">了解如何在 MVC 應用程式的內容中使用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="b7397-106">Learn how to use Windows authentication in the context of an MVC application.</span></span> <span data-ttu-id="b7397-107">您了解如何啟用您的應用程式 web 組態檔中的 Windows 驗證以及如何設定 iis 驗證。</span><span class="sxs-lookup"><span data-stu-id="b7397-107">You learn how to enable Windows authentication within your application's web configuration file and how to configure authentication with IIS.</span></span> <span data-ttu-id="b7397-108">最後，您會學習如何使用 [Authorize] 屬性來限制存取權給特定的 Windows 使用者或群組的控制器動作。</span><span class="sxs-lookup"><span data-stu-id="b7397-108">Finally, you learn how to use the [Authorize] attribute to restrict access to controller actions to particular Windows users or groups.</span></span>


<span data-ttu-id="b7397-109">本教學課程的目標是要說明，如何利用內建於網際網路資訊服務的安全性功能，以藉由密碼來保護 MVC 應用程式中的檢視。</span><span class="sxs-lookup"><span data-stu-id="b7397-109">The goal of this tutorial is to explain how you can take advantage of the security features built into Internet Information Services to password protect the views in your MVC applications.</span></span> <span data-ttu-id="b7397-110">了解如何只被允許的特定 Windows 使用者或特定的 Windows 群組的成員叫用控制器動作。</span><span class="sxs-lookup"><span data-stu-id="b7397-110">You learn how to allow controller actions to be invoked only by particular Windows users or users who are members of particular Windows groups.</span></span>

<span data-ttu-id="b7397-111">當您建立公司內部的網站 （內部網路網站）並且想讓使用者能夠使用標準的 Windows 使用者名稱及密碼存取網站時，使用 Windows 驗證就很合理。</span><span class="sxs-lookup"><span data-stu-id="b7397-111">Using Windows authentication makes sense when you are building an internal company website (an intranet site) and you want your users to be able to use their standard Windows user names and passwords when accessing the website.</span></span> <span data-ttu-id="b7397-112">如果您要建置對外網站 （網際網路網站），請考慮改用表單驗證。</span><span class="sxs-lookup"><span data-stu-id="b7397-112">If you are building an outwards facing website (an Internet website) consider using Forms authentication instead.</span></span>

#### <a name="enabling-windows-authentication"></a><span data-ttu-id="b7397-113">啟用 Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="b7397-113">Enabling Windows Authentication</span></span>

<span data-ttu-id="b7397-114">當您建立新的 ASP.NET MVC 應用程式時，預設是不啟用 Windows 驗證的。</span><span class="sxs-lookup"><span data-stu-id="b7397-114">When you create a new ASP.NET MVC application, Windows authentication is not enabled by default.</span></span> <span data-ttu-id="b7397-115">表單驗證是MVC 應用程式預設啟用的驗證類型。</span><span class="sxs-lookup"><span data-stu-id="b7397-115">Forms authentication is the default authentication type enabled for MVC applications.</span></span> <span data-ttu-id="b7397-116">您必須修改 MVC 應用程式的 web 組態檔 (web.config)，以啟用 Windows 驗證。</span><span class="sxs-lookup"><span data-stu-id="b7397-116">You must enable Windows authentication by modifying your MVC application's web configuration (web.config) file.</span></span> <span data-ttu-id="b7397-117">尋找&lt;驗證&gt;區段，並將它修改為使用 Windows，而不是表單驗證，就像這樣：</span><span class="sxs-lookup"><span data-stu-id="b7397-117">Find the &lt;authentication&gt; section and modify it to use Windows instead of Forms authentication like this:</span></span>

[!code-xml[Main](authenticating-users-with-windows-authentication-cs/samples/sample1.xml)]

<span data-ttu-id="b7397-118">當您啟用 Windows 驗證時，您的網頁伺服器會負責驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="b7397-118">When you enable Windows authentication, your web server becomes responsible for authenticating users.</span></span> <span data-ttu-id="b7397-119">通常，當在建立及部署 ASP.NET MVC 應用程式時，會使用兩種不同的網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="b7397-119">Typically, there are two different types of web servers that you use when creating and deploying an ASP.NET MVC application.</span></span>

<span data-ttu-id="b7397-120">首先，在開發 MVC 應用程式時，您會使用 Visual Studio 隨附的 ASP.NET 程式開發 Web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="b7397-120">First, while developing an MVC application, you use the ASP.NET Development Web Server included with Visual Studio.</span></span> <span data-ttu-id="b7397-121">根據預設，ASP.NET 開發 Web 伺服器會以目前的 Windows 帳戶 （不論您登入 Windows 的帳戶） 執行所有頁面。</span><span class="sxs-lookup"><span data-stu-id="b7397-121">By default, the ASP.NET Development Web Server executes all pages in the context of the current Windows account (whatever account you used to log into Windows).</span></span>

<span data-ttu-id="b7397-122">ASP.NET 開發 Web 伺服器也支援 NTLM 驗證。</span><span class="sxs-lookup"><span data-stu-id="b7397-122">The ASP.NET Development Web Server also supports NTLM authentication.</span></span> <span data-ttu-id="b7397-123">您可以滑鼠右鍵按一下方案總管] 視窗中的專案名稱，然後選取 [屬性，以啟用 NTLM 驗證。</span><span class="sxs-lookup"><span data-stu-id="b7397-123">You can enable NTLM authentication by right-clicking the name of your project in the Solution Explorer window and selecting Properties.</span></span> <span data-ttu-id="b7397-124">接下來，選取 [Web] 頁籤，並勾選 NTLM 核取方塊 （請參閱圖 1）。 </span><span class="sxs-lookup"><span data-stu-id="b7397-124">Next, select the Web tab and check the NTLM checkbox (see Figure 1).</span></span>

<span data-ttu-id="b7397-125">**圖 1 – 啟用 ASP.NET 開發 Web 伺服器的 NTLM 驗證**</span><span class="sxs-lookup"><span data-stu-id="b7397-125">**Figure 1 – Enabling NTLM authentication for the ASP.NET Development Web Server**</span></span>

![clip_image002](authenticating-users-with-windows-authentication-cs/_static/image1.jpg)

<span data-ttu-id="b7397-127">另一種，正式環境的 web 應用程式中，使用 IIS 做為網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="b7397-127">For a production web application, on the hand, you use IIS as your web server.</span></span> <span data-ttu-id="b7397-128">IIS 支援的各種驗證類型有：</span><span class="sxs-lookup"><span data-stu-id="b7397-128">IIS supports several types of authentication including:</span></span>

- <span data-ttu-id="b7397-129">基本驗證 – 定義為 HTTP 1.0 通訊協定的一部分。</span><span class="sxs-lookup"><span data-stu-id="b7397-129">Basic Authentication – Defined as part of the HTTP 1.0 protocol.</span></span> <span data-ttu-id="b7397-130">使用者名稱和密碼以純文字 (以 Base64 編碼) 透過網際網路傳送。</span><span class="sxs-lookup"><span data-stu-id="b7397-130">Sends user names and passwords in clear text (Base64 encoded) across the Internet.</span></span> <span data-ttu-id="b7397-131">摘要式驗證 – 傳送密碼，而不是密碼本身，在網際網路上的雜湊。</span><span class="sxs-lookup"><span data-stu-id="b7397-131">- Digest Authentication – Sends a hash of a password, instead of the password itself, across the internet.</span></span> <span data-ttu-id="b7397-132">-整合的 Windows (NTLM) 驗證 – 最佳的型別要使用 windows 內部網路環境中使用的驗證。</span><span class="sxs-lookup"><span data-stu-id="b7397-132">- Integrated Windows (NTLM) Authentication – The best type of authentication to use in intranet environments using windows.</span></span> <span data-ttu-id="b7397-133">-驗證-啟用驗證使用用戶端憑證的憑證。</span><span class="sxs-lookup"><span data-stu-id="b7397-133">- Certificate Authentication – Enables authentication using a client-side certificate.</span></span> <span data-ttu-id="b7397-134">此憑證對應至 Windows 使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="b7397-134">The certificate maps to a Windows user account.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="b7397-135">這些不同類型的驗證的詳細概觀，請參閱[ https://msdn.microsoft.com/library/aa292114(VS.71).aspx ](https://msdn.microsoft.com/library/aa292114(VS.71).aspx)。</span><span class="sxs-lookup"><span data-stu-id="b7397-135">For a more detailed overview of these different types of authentication, see [https://msdn.microsoft.com/library/aa292114(VS.71).aspx](https://msdn.microsoft.com/library/aa292114(VS.71).aspx).</span></span>


<span data-ttu-id="b7397-136">若要啟用特定類型的驗證，您可以使用 Internet Information Services 管理員。</span><span class="sxs-lookup"><span data-stu-id="b7397-136">You can use Internet Information Services Manager to enable a particular type of authentication.</span></span> <span data-ttu-id="b7397-137">請注意，所有的驗證類型不是在每個作業系統的情況下可用。</span><span class="sxs-lookup"><span data-stu-id="b7397-137">Be aware that all types of authentication are not available in the case of every operating system.</span></span> <span data-ttu-id="b7397-138">此外，如果您使用 IIS 7.0 使用 Windows Vista，您必須啟用不同類型的 Windows 驗證，才會出現在 Internet Information Services 管理員。</span><span class="sxs-lookup"><span data-stu-id="b7397-138">Furthermore, if you are using IIS 7.0 with Windows Vista, you will need to enable the different types of Windows authentication before they appear in the Internet Information Services Manager.</span></span> <span data-ttu-id="b7397-139">開啟**控制台、 程式、 程式和功能，開啟或關閉 Windows 功能**，然後展開 Internet Information Services 節點 （請參閱圖 2）。</span><span class="sxs-lookup"><span data-stu-id="b7397-139">Open **Control Panel, Programs, Programs and Features, Turn Windows features on or off**, and expand the Internet Information Services node (see Figure 2).</span></span>

<span data-ttu-id="b7397-140">**圖 2-啟用 Windows IIS 功能**</span><span class="sxs-lookup"><span data-stu-id="b7397-140">**Figure 2 – Enabling Windows IIS features**</span></span>

![clip_image004](authenticating-users-with-windows-authentication-cs/_static/image2.jpg)

<span data-ttu-id="b7397-142">使用 Internet Information Services，您可以啟用或停用不同類型的驗證。</span><span class="sxs-lookup"><span data-stu-id="b7397-142">Using Internet Information Services, you can enable or disable different types of authentication.</span></span> <span data-ttu-id="b7397-143">例如，圖 3 說明停用匿名驗證並啟用整合式 Windows (NTLM) 驗證時使用的 IIS 7.0。</span><span class="sxs-lookup"><span data-stu-id="b7397-143">For example, Figure 3 illustrates disabling anonymous authentication and enabling Integrated Windows (NTLM) authentication when using IIS 7.0.</span></span>

<span data-ttu-id="b7397-144">**圖 3 – 啟用整合式的 Windows 驗證**</span><span class="sxs-lookup"><span data-stu-id="b7397-144">**Figure 3 – Enabling Integrated Windows Authentication**</span></span>

![clip_image006](authenticating-users-with-windows-authentication-cs/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a><span data-ttu-id="b7397-146">授權 Windows 使用者和群組</span><span class="sxs-lookup"><span data-stu-id="b7397-146">Authorizing Windows Users and Groups</span></span>

<span data-ttu-id="b7397-147">啟用 Windows 驗證之後，您可以使用 [Authorize] 屬性來控制存取控制站或控制器的動作。</span><span class="sxs-lookup"><span data-stu-id="b7397-147">After you enable Windows authentication, you can use the [Authorize] attribute to control access to controllers or controller actions.</span></span> <span data-ttu-id="b7397-148">這個屬性可以套用到整個的 MVC 控制器或特定控制器動作。</span><span class="sxs-lookup"><span data-stu-id="b7397-148">This attribute can be applied to an entire MVC controller or a particular controller action.</span></span>

<span data-ttu-id="b7397-149">比方說，Home 控制器中列出的 1 會公開三個名為 index （）、 CompanySecrets() 和 StephenSecrets() 的動作。</span><span class="sxs-lookup"><span data-stu-id="b7397-149">For example, the Home controller in Listing 1 exposes three actions named Index(), CompanySecrets(), and StephenSecrets().</span></span> <span data-ttu-id="b7397-150">任何人都可以叫用的 index 動作。</span><span class="sxs-lookup"><span data-stu-id="b7397-150">Anyone can invoke the Index() action.</span></span> <span data-ttu-id="b7397-151">不過，只有 Windows 本機管理員群組的成員可以叫用 CompanySecrets() 動作。</span><span class="sxs-lookup"><span data-stu-id="b7397-151">However, only members of the Windows local Managers group can invoke the CompanySecrets() action.</span></span> <span data-ttu-id="b7397-152">最後，只有 Windows 網域使用者名為 Stephen （位於 Redmond 網域） 可以叫用 StephenSecrets() 動作。</span><span class="sxs-lookup"><span data-stu-id="b7397-152">Finally, only the Windows domain user named Stephen (in the Redmond domain) can invoke the StephenSecrets() action.</span></span>

<span data-ttu-id="b7397-153">**列出 1 – Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="b7397-153">**Listing 1 – Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](authenticating-users-with-windows-authentication-cs/samples/sample2.cs)]

> [!NOTE] 
> 
> <span data-ttu-id="b7397-154">因為 Windows 使用者帳戶控制 (UAC)，使用 Windows Vista 或 Windows Server 2008 時，本機 Administrators 群組的方式不同於其他群組行為。</span><span class="sxs-lookup"><span data-stu-id="b7397-154">Because of Windows User Account Control (UAC), when working with Windows Vista or Windows Server 2008, the local Administrators group will behave differently than other groups.</span></span> <span data-ttu-id="b7397-155">除非您修改您電腦的 UAC 設定，[Authorize] 屬性將不會正確辨識本機 Administrators 群組的成員。</span><span class="sxs-lookup"><span data-stu-id="b7397-155">The [Authorize] attribute won't correctly recognize a member of the local Administrators group unless you modify your computer's UAC settings.</span></span>


<span data-ttu-id="b7397-156">只會發生什麼事當您嘗試以不正確的權限叫用控制器動作，取決於啟用驗證的類型。</span><span class="sxs-lookup"><span data-stu-id="b7397-156">Exactly what happens when you attempt to invoke a controller action without being the right permissions depends on the type of authentication enabled.</span></span> <span data-ttu-id="b7397-157">根據預設，使用 ASP.NET 程式開發伺服器時，您只會收到空白頁。</span><span class="sxs-lookup"><span data-stu-id="b7397-157">By default, when using the ASP.NET Development Server, you simply get a blank page.</span></span> <span data-ttu-id="b7397-158">使用提供的網頁**401 未授權**HTTP 回應狀態。</span><span class="sxs-lookup"><span data-stu-id="b7397-158">The page is served with a **401 Not Authorized** HTTP Response Status.</span></span>

<span data-ttu-id="b7397-159">如果，相反地，您使用 IIS 已停用匿名驗證和基本驗證已啟用，則持續發生登入對話方塊提示您要求的受保護的頁面每次 （請參閱圖 4）。</span><span class="sxs-lookup"><span data-stu-id="b7397-159">If, on the other hand, you are using IIS with Anonymous authentication disabled and Basic authentication enabled, then you keep getting a login dialog prompt each time you request the protected page (see Figure 4).</span></span>

<span data-ttu-id="b7397-160">**圖 4 – 基本驗證的登入 對話方塊**</span><span class="sxs-lookup"><span data-stu-id="b7397-160">**Figure 4 – Basic authentication login dialog**</span></span>

![clip_image008](authenticating-users-with-windows-authentication-cs/_static/image4.jpg)

#### <a name="summary"></a><span data-ttu-id="b7397-162">總結</span><span class="sxs-lookup"><span data-stu-id="b7397-162">Summary</span></span>

<span data-ttu-id="b7397-163">本教學課程將說明如何使用 Windows 驗證，以及在 ASP.NET MVC 應用程式的內容中。</span><span class="sxs-lookup"><span data-stu-id="b7397-163">This tutorial explained how you can use Windows authentication in the context of an ASP.NET MVC application.</span></span> <span data-ttu-id="b7397-164">您已學習如何啟用您的應用程式 web 組態檔中的 Windows 驗證以及如何設定 iis 驗證。</span><span class="sxs-lookup"><span data-stu-id="b7397-164">You learned how to enable Windows authentication within your application's web configuration file and how to configure authentication with IIS.</span></span> <span data-ttu-id="b7397-165">最後，您學會如何使用 [Authorize] 屬性來限制存取權給特定的 Windows 使用者或群組的控制器動作。</span><span class="sxs-lookup"><span data-stu-id="b7397-165">Finally, you learned how to use the [Authorize] attribute to restrict access to controller actions to particular Windows users or groups.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b7397-166">[上一頁](authenticating-users-with-forms-authentication-cs.md)
> [下一頁](preventing-javascript-injection-attacks-cs.md)</span><span class="sxs-lookup"><span data-stu-id="b7397-166">[Previous](authenticating-users-with-forms-authentication-cs.md)
[Next](preventing-javascript-injection-attacks-cs.md)</span></span>
