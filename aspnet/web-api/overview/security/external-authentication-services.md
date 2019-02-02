---
uid: web-api/overview/security/external-authentication-services
title: 外部驗證服務與 ASP.NET Web API (C#) |Microsoft Docs
author: rmcmurray
description: 描述如何使用 ASP.NET Web API 中的外部驗證服務。
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: de9b64e6c582059ec66ab352f60773f50af7b1ff
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667852"
---
# <a name="external-authentication-services-with-aspnet-web-api-c"></a><span data-ttu-id="dcb58-103">外部驗證服務與 ASP.NET Web API (C#)</span><span class="sxs-lookup"><span data-stu-id="dcb58-103">External Authentication Services with ASP.NET Web API (C#)</span></span>

<span data-ttu-id="dcb58-104">展開的安全性選項 visual Studio 2017 和 ASP.NET 4.7.2[單一頁面應用程式](../../../single-page-application/index.md)(SPA) 及[Web API](../../index.md)服務外部驗證服務，其中包含數個整合OAuth/OpenID 和社交媒體驗證服務：Microsoft 帳戶、 Twitter、 Facebook 和 Google。</span><span class="sxs-lookup"><span data-stu-id="dcb58-104">Visual Studio 2017 and ASP.NET 4.7.2 expand the security options for [Single Page Applications](../../../single-page-application/index.md) (SPA) and [Web API](../../index.md) services to integrate with external authentication services, which include several OAuth/OpenID and social media authentication services: Microsoft Accounts, Twitter, Facebook, and Google.</span></span>  

### <a name="in-this-walkthrough"></a><span data-ttu-id="dcb58-105">在本逐步解說</span><span class="sxs-lookup"><span data-stu-id="dcb58-105">In this Walkthrough</span></span>

- [<span data-ttu-id="dcb58-106">使用外部驗證服務</span><span class="sxs-lookup"><span data-stu-id="dcb58-106">Using External Authentication Services</span></span>](#USING)
- [<span data-ttu-id="dcb58-107">建立範例 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="dcb58-107">Creating the Sample Web Application</span></span>](#SAMPLE)
- [<span data-ttu-id="dcb58-108">啟用 Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="dcb58-108">Enabling Facebook Authentication</span></span>](#FACEBOOK)
- [<span data-ttu-id="dcb58-109">啟用 Google 驗證</span><span class="sxs-lookup"><span data-stu-id="dcb58-109">Enabling Google Authentication</span></span>](#GOOGLE)
- [<span data-ttu-id="dcb58-110">啟用 Microsoft 驗證</span><span class="sxs-lookup"><span data-stu-id="dcb58-110">Enabling Microsoft Authentication</span></span>](#MICROSOFT)
- [<span data-ttu-id="dcb58-111">啟用 Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="dcb58-111">Enabling Twitter Authentication</span></span>](#TWITTER)
- [<span data-ttu-id="dcb58-112">其他資訊</span><span class="sxs-lookup"><span data-stu-id="dcb58-112">Additional Information</span></span>](#MOREINFO)

    - [<span data-ttu-id="dcb58-113">結合外部驗證服務</span><span class="sxs-lookup"><span data-stu-id="dcb58-113">Combining External Authentication Services</span></span>](#COMBINE)
    - [<span data-ttu-id="dcb58-114">設定 IIS Express 要使用完整網域名稱</span><span class="sxs-lookup"><span data-stu-id="dcb58-114">Configuring IIS Express to use a Fully Qualified Domain Name</span></span>](#FQDN)
    - [<span data-ttu-id="dcb58-115">濆爧髍孮 Microsoft 驗證您的應用程式設定</span><span class="sxs-lookup"><span data-stu-id="dcb58-115">How to Obtain your Application Settings for Microsoft Authentication</span></span>](#OBTAIN)
    - [<span data-ttu-id="dcb58-116">選擇性：停用本機註冊</span><span class="sxs-lookup"><span data-stu-id="dcb58-116">Optional: Disable Local Registration</span></span>](#DISABLE)

### <a name="prerequisites"></a><span data-ttu-id="dcb58-117">必要條件</span><span class="sxs-lookup"><span data-stu-id="dcb58-117">Prerequisites</span></span>

<span data-ttu-id="dcb58-118">若要依照本逐步解說中的範例，您需要具備下列項目：</span><span class="sxs-lookup"><span data-stu-id="dcb58-118">To follow the examples in this walkthrough, you need to have the following:</span></span>

- <span data-ttu-id="dcb58-119">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="dcb58-119">Visual Studio 2017</span></span>
- <span data-ttu-id="dcb58-120">開發人員使用的應用程式識別碼和祕密金鑰的其中一個下列的社交媒體驗證服務帳戶：</span><span class="sxs-lookup"><span data-stu-id="dcb58-120">A developer account with the application identifier and secret key for one of the following social media authentication services:</span></span>

  - <span data-ttu-id="dcb58-121">Microsoft 帳戶 ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))</span><span class="sxs-lookup"><span data-stu-id="dcb58-121">Microsoft Accounts ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))</span></span>
  - <span data-ttu-id="dcb58-122">Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))</span><span class="sxs-lookup"><span data-stu-id="dcb58-122">Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))</span></span>
  - <span data-ttu-id="dcb58-123">Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))</span><span class="sxs-lookup"><span data-stu-id="dcb58-123">Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))</span></span>
  - <span data-ttu-id="dcb58-124">Google ([https://developers.google.com/](https://developers.google.com))</span><span class="sxs-lookup"><span data-stu-id="dcb58-124">Google ([https://developers.google.com/](https://developers.google.com))</span></span>

<a id="USING"></a>
## <a name="using-external-authentication-services"></a><span data-ttu-id="dcb58-125">使用外部驗證服務</span><span class="sxs-lookup"><span data-stu-id="dcb58-125">Using External Authentication Services</span></span>

<span data-ttu-id="dcb58-126">目前可供 web 開發人員說明，以降低開發的外部驗證服務的眾多時建立新的 web 應用程式的時間。</span><span class="sxs-lookup"><span data-stu-id="dcb58-126">The abundance of external authentication services that are currently available to web developers help to reduce development time when creating new web applications.</span></span> <span data-ttu-id="dcb58-127">Web 使用者通常有數個現有的帳戶，以受歡迎的 web 服務和社交媒體網站，因此當 web 應用程式實作，從外部 web 服務或社交媒體網站的驗證服務，它會將儲存的開發時間，將已使建立驗證實作。</span><span class="sxs-lookup"><span data-stu-id="dcb58-127">Web users typically have several existing accounts for popular web services and social media websites, therefore when a web application implements the authentication services from an external web service or social media website, it saves the development time that would have been spent creating an authentication implementation.</span></span> <span data-ttu-id="dcb58-128">使用外部驗證服務可以節省使用者毋需建立 web 應用程式的另一個帳戶，也不需要記住另一個使用者名稱和密碼。</span><span class="sxs-lookup"><span data-stu-id="dcb58-128">Using an external authentication service saves end users from having to create another account for your web application, and also from having to remember another username and password.</span></span>

<span data-ttu-id="dcb58-129">在過去，開發人員在過去兩個選擇： 建立自己的驗證實作，或了解如何整合到其應用程式的外部驗證服務。</span><span class="sxs-lookup"><span data-stu-id="dcb58-129">In the past, developers have had two choices: create their own authentication implementation, or learn how to integrate an external authentication service into their applications.</span></span> <span data-ttu-id="dcb58-130">在最基本的層級下, 圖說明簡單的要求流程的使用者代理程式 （web 瀏覽器） 從 web 應用程式設定為使用外部驗證服務要求資訊：</span><span class="sxs-lookup"><span data-stu-id="dcb58-130">At the most basic level, the following diagram illustrates a simple request flow for a user agent (web browser) that is requesting information from a web application that is configured to use an external authentication service:</span></span>

<span data-ttu-id="dcb58-131">[![](external-authentication-services/_static/image2.png "按一下以展開映像")](external-authentication-services/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-131">[![](external-authentication-services/_static/image2.png "Click to Expand the Image")](external-authentication-services/_static/image1.png)</span></span>

<span data-ttu-id="dcb58-132">在上圖中，使用者代理程式 （或在此範例中的網頁瀏覽器） 會提出要求的 web 瀏覽器重新導向至外部驗證服務與 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="dcb58-132">In the preceding diagram, the user agent (or web browser in this example) makes a request to a web application, which redirects the web browser to an external authentication service.</span></span> <span data-ttu-id="dcb58-133">使用者代理程式會將其憑證傳送到外部驗證服務，並在服務和使用者代理程式已成功驗證，如果外部驗證服務將使用者代理程式重新導向至原始的 web 應用程式，使用某種形式的語彙基元的使用者代理程式會傳送給 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="dcb58-133">The user agent sends its credentials to the external authentication service, and if the user agent has successfully authenticated, the external authentication service will redirect the user agent to the original web application with some form of token which the user agent will send to the web application.</span></span> <span data-ttu-id="dcb58-134">Web 應用程式會使用權杖，以確認使用者代理程式已成功驗證外部驗證服務，且 web 應用程式可能會使用權杖來收集使用者代理程式的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="dcb58-134">The web application will use the token to verify that the user agent has been successfully authenticated by the external authentication service, and the web application may use the token to gather more information about the user agent.</span></span> <span data-ttu-id="dcb58-135">完成應用程式之後處理的使用者代理程式的資訊，web 應用程式會傳回適當的回應取決於其授權設定的使用者代理程式。</span><span class="sxs-lookup"><span data-stu-id="dcb58-135">Once the application is done processing the user agent's information, the web application will return the appropriate response to the user agent based on its authorization settings.</span></span>

<span data-ttu-id="dcb58-136">在第二個範例中，與 web 應用程式和外部的授權伺服器交涉，使用者代理程式，並在 web 應用程式執行與外部的授權伺服器，以擷取使用者的其他資訊的其他通訊代理程式：</span><span class="sxs-lookup"><span data-stu-id="dcb58-136">In this second example, the user agent negotiates with the web application and external authorization server, and the web application performs additional communication with the external authorization server to retrieve additional information about the user agent:</span></span>

<span data-ttu-id="dcb58-137">[![](external-authentication-services/_static/image4.png "按一下以展開映像")](external-authentication-services/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-137">[![](external-authentication-services/_static/image4.png "Click to Expand the Image")](external-authentication-services/_static/image3.png)</span></span>

<span data-ttu-id="dcb58-138">Visual Studio 2017 和 ASP.NET 4.7.2 與外部驗證服務的整合更輕鬆地進行開發人員藉由提供內建的整合，如以下驗證服務：</span><span class="sxs-lookup"><span data-stu-id="dcb58-138">Visual Studio 2017 and ASP.NET 4.7.2 make integration with external authentication services easier for developers by providing built-in integration for the following authentication services:</span></span>

- <span data-ttu-id="dcb58-139">Facebook</span><span class="sxs-lookup"><span data-stu-id="dcb58-139">Facebook</span></span>
- <span data-ttu-id="dcb58-140">Google</span><span class="sxs-lookup"><span data-stu-id="dcb58-140">Google</span></span>
- <span data-ttu-id="dcb58-141">Microsoft 帳戶 （Windows Live ID 帳戶）</span><span class="sxs-lookup"><span data-stu-id="dcb58-141">Microsoft Accounts (Windows Live ID accounts)</span></span>
- <span data-ttu-id="dcb58-142">Twitter</span><span class="sxs-lookup"><span data-stu-id="dcb58-142">Twitter</span></span>

<span data-ttu-id="dcb58-143">在本逐步解說的範例將示範如何使用 Visual Studio 2017 中隨附的新 ASP.NET Web 應用程式範本中設定每個支援的外部驗證服務。</span><span class="sxs-lookup"><span data-stu-id="dcb58-143">The examples in this walkthrough will demonstrate how to configure each of the supported external authentication services by using the new ASP.NET Web Application template that ships with Visual Studio 2017.</span></span>

> [!NOTE]
> <span data-ttu-id="dcb58-144">如有必要，您可能需要將您的 FQDN 新增到您的外部驗證服務的設定。</span><span class="sxs-lookup"><span data-stu-id="dcb58-144">If necessary, you may need to add your FQDN to the settings for your external authentication service.</span></span> <span data-ttu-id="dcb58-145">這項需求根據安全性限制，某些外部驗證服務，而這需要您的應用程式設定中的 FQDN，以符合您的用戶端會使用的 FQDN。</span><span class="sxs-lookup"><span data-stu-id="dcb58-145">This requirement is based on security constraints for some external authentication services which require the FQDN in your application settings to match the FQDN that is used by your clients.</span></span> <span data-ttu-id="dcb58-146">（這個步驟很大的每個外部驗證服務，您必須參考文件，每個外部驗證服務，以查看這是否必要，以及如何設定這些設定）。如果您要設定 IIS Express 來測試此環境中使用 FQDN，請參閱[若要使用完整網域名稱設定 IIS Express](#FQDN)稍後在本逐步解說的區段。</span><span class="sxs-lookup"><span data-stu-id="dcb58-146">(The steps for this will vary greatly for each external authentication service; you will need to consult the documentation for each external authentication service to see if this is required and how to configure these settings.) If you need to configure IIS Express to use an FQDN for testing this environment, see the [Configuring IIS Express to use a Fully Qualified Domain Name](#FQDN) section later in this walkthrough.</span></span>


<a id="SAMPLE"></a>
## <a name="create-a-sample-web-application"></a><span data-ttu-id="dcb58-147">建立範例 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="dcb58-147">Create a Sample Web Application</span></span>

<span data-ttu-id="dcb58-148">下列步驟將引導您完成使用 ASP.NET Web 應用程式範本，建立範例應用程式，您將使用此範例應用程式的每個外部驗證服務，稍後在本逐步解說。</span><span class="sxs-lookup"><span data-stu-id="dcb58-148">The following steps will lead you through creating a sample application by using the ASP.NET Web Application template, and you will use this sample application for each of the external authentication services later in this walkthrough.</span></span>

<span data-ttu-id="dcb58-149">啟動 Visual Studio 2017，然後選取**新的專案**從 [開始] 頁面。</span><span class="sxs-lookup"><span data-stu-id="dcb58-149">Start Visual Studio 2017 and select **New Project** from the Start page.</span></span> <span data-ttu-id="dcb58-150">或從**檔案**功能表上，選取**新增**，然後**專案**。</span><span class="sxs-lookup"><span data-stu-id="dcb58-150">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<!-- [![](external-authentication-services/_static/image6.png "Click to Expand the Image")](external-authentication-services/_static/image5.png) -->

<span data-ttu-id="dcb58-151">當**新的專案**對話方塊隨即出現，請選取**已安裝**展開**Visual C#** 。</span><span class="sxs-lookup"><span data-stu-id="dcb58-151">When the **New Project** dialog box is displayed, select **Installed** and expand **Visual C#**.</span></span> <span data-ttu-id="dcb58-152">底下**Visual C#**，選取**Web**。</span><span class="sxs-lookup"><span data-stu-id="dcb58-152">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="dcb58-153">在專案範本清單中，選取**ASP.NET Web 應用程式 (.Net Framework)**。</span><span class="sxs-lookup"><span data-stu-id="dcb58-153">In the list of project templates, select **ASP.NET Web Application (.Net Framework)**.</span></span> <span data-ttu-id="dcb58-154">輸入您專案的名稱，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="dcb58-154">Enter a name for your project and click **OK**.</span></span>

<span data-ttu-id="dcb58-155">[![](external-authentication-services/_static/image71.png "按一下以展開映像")](external-authentication-services/_static/image71.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-155">[![](external-authentication-services/_static/image71.png "Click to Expand the Image")](external-authentication-services/_static/image71.png)</span></span>

<span data-ttu-id="dcb58-156">當**新的 ASP.NET 專案**顯示，請選取**單一頁面應用程式**範本，然後按一下**建立的專案**。</span><span class="sxs-lookup"><span data-stu-id="dcb58-156">When the **New ASP.NET Project** is displayed, select the **Single Page Application** template and click **Create Project**.</span></span>

<span data-ttu-id="dcb58-157">[![](external-authentication-services/_static/image72.png "按一下以展開映像")](external-authentication-services/_static/image72.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-157">[![](external-authentication-services/_static/image72.png "Click to Expand the Image")](external-authentication-services/_static/image72.png)</span></span>

<span data-ttu-id="dcb58-158">等候與 Visual Studio 2017 建立您的專案。</span><span class="sxs-lookup"><span data-stu-id="dcb58-158">Wait as Visual Studio 2017 creates your project.</span></span>

<!-- [![](external-authentication-services/_static/image12.png "Click to Expand the Image")](external-authentication-services/_static/image11.png) -->

<span data-ttu-id="dcb58-159">當 Visual Studio 2017 完成建立專案時，開啟*Startup.Auth.cs*檔案，它位於**應用程式\_啟動**資料夾。</span><span class="sxs-lookup"><span data-stu-id="dcb58-159">When Visual Studio 2017 has finished creating your project, open the *Startup.Auth.cs* file that is located in the **App\_Start** folder.</span></span>

<span data-ttu-id="dcb58-160">當您第一次建立專案時，沒有任何外部驗證服務中啟用*Startup.Auth.cs*檔案; 下圖說明您的程式碼可能類似，反白顯示位置區段會啟用外部驗證服務和任何相關的設定，才能使用您的 ASP.NET 應用程式中的 Microsoft 帳戶、 Twitter、 Facebook 或 Google 驗證：</span><span class="sxs-lookup"><span data-stu-id="dcb58-160">When you first create the project, none of the external authentication services are enabled in *Startup.Auth.cs* file; the following illustrates what your code might resemble, with the sections highlighted for where you would enable an external authentication service and any relevant settings in order to use Microsoft Accounts, Twitter, Facebook, or Google authentication with your ASP.NET application:</span></span>

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

<span data-ttu-id="dcb58-161">當您按 F5 以建置和偵錯您的 web 應用程式時，它會顯示登入畫面，您會看到尚未定義任何外部驗證服務。</span><span class="sxs-lookup"><span data-stu-id="dcb58-161">When you press F5 to build and debug your web application, it will display a login screen where you will see that no external authentication services have been defined.</span></span>

<span data-ttu-id="dcb58-162">[![](external-authentication-services/_static/image73.png "按一下以展開映像")](external-authentication-services/_static/image73.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-162">[![](external-authentication-services/_static/image73.png "Click to Expand the Image")](external-authentication-services/_static/image73.png)</span></span>

<span data-ttu-id="dcb58-163">在下列章節中，您將了解如何啟用每個外部驗證服務所提供的 Visual Studio 2017 中的 ASP.NET。</span><span class="sxs-lookup"><span data-stu-id="dcb58-163">In the following sections, you will learn how to enable each of the external authentication services that are provided with ASP.NET in Visual Studio 2017.</span></span>

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a><span data-ttu-id="dcb58-164">啟用 Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="dcb58-164">Enabling Facebook authentication</span></span>

<span data-ttu-id="dcb58-165">使用 Facebook 驗證會要求您建立 Facebook 開發人員帳戶，而您的專案需要的應用程式識別碼和來自 Facebook 的祕密金鑰才能運作。</span><span class="sxs-lookup"><span data-stu-id="dcb58-165">Using Facebook authentication requires you to create a Facebook developer account, and your project will require an application ID and secret key from Facebook in order to function.</span></span> <span data-ttu-id="dcb58-166">建立 Facebook 開發人員帳戶，並取得您的應用程式識別碼和祕密金鑰的相關資訊，請參閱[ https://go.microsoft.com/fwlink/?LinkID=252166 ](https://go.microsoft.com/fwlink/?LinkID=252166)。</span><span class="sxs-lookup"><span data-stu-id="dcb58-166">For information about creating a Facebook developer account and obtaining your application ID and secret key, see [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span></span>

<span data-ttu-id="dcb58-167">一旦您取得您的應用程式識別碼和祕密金鑰，請使用下列步驟來啟用 web 應用程式的 Facebook 驗證：</span><span class="sxs-lookup"><span data-stu-id="dcb58-167">Once you have obtained your application ID and secret key, use the following steps to enable Facebook authentication for your web application:</span></span>

1. <span data-ttu-id="dcb58-168">在 Visual Studio 2017 中開啟您的專案時，開啟*Startup.Auth.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="dcb58-168">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="dcb58-169">找出 [Facebook 驗證] 區段的程式碼：</span><span class="sxs-lookup"><span data-stu-id="dcb58-169">Locate the Facebook authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. <span data-ttu-id="dcb58-170">移除&quot; // &quot;字元的程式碼中，反白顯示的行取消註解，然後新增 您的應用程式識別碼和祕密金鑰。</span><span class="sxs-lookup"><span data-stu-id="dcb58-170">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your application ID and secret key.</span></span> <span data-ttu-id="dcb58-171">一旦您加入這些參數，您可以重新編譯您的專案：</span><span class="sxs-lookup"><span data-stu-id="dcb58-171">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. <span data-ttu-id="dcb58-172">當您按 f5 鍵在網頁瀏覽器中開啟 web 應用程式時，您會看到 Facebook，已定義為外部驗證服務：</span><span class="sxs-lookup"><span data-stu-id="dcb58-172">When you press F5 to open your web application in your web browser, you will see that Facebook has been defined as an external authentication service:</span></span>

    <span data-ttu-id="dcb58-173">[![](external-authentication-services/_static/image74.png "按一下以展開映像")](external-authentication-services/_static/image74.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-173">[![](external-authentication-services/_static/image74.png "Click to Expand the Image")](external-authentication-services/_static/image74.png)</span></span>
5. <span data-ttu-id="dcb58-174">當您按一下 [ **Facebook** ] 按鈕，您的瀏覽器會被重新導向至 Facebook 登入頁面：</span><span class="sxs-lookup"><span data-stu-id="dcb58-174">When you click the **Facebook** button, your browser will be redirected to the Facebook login page:</span></span>

    <span data-ttu-id="dcb58-175">[![](external-authentication-services/_static/image22.png "按一下以展開映像")](external-authentication-services/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-175">[![](external-authentication-services/_static/image22.png "Click to Expand the Image")](external-authentication-services/_static/image21.png)</span></span>
6. <span data-ttu-id="dcb58-176">您輸入您的 Facebook 認證，然後按一下 後**登入**，您的 web 瀏覽器會被重新導向回到您的 web 應用程式，這將會提示您輸入**使用者名稱**您想要關聯您Facebook 帳戶：</span><span class="sxs-lookup"><span data-stu-id="dcb58-176">After you enter your Facebook credentials and click **Log in**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Facebook account:</span></span>

    <span data-ttu-id="dcb58-177">[![](external-authentication-services/_static/image24.png "按一下以展開映像")](external-authentication-services/_static/image23.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-177">[![](external-authentication-services/_static/image24.png "Click to Expand the Image")](external-authentication-services/_static/image23.png)</span></span>
7. <span data-ttu-id="dcb58-178">在您輸入您的使用者名稱並按下後**註冊** 按鈕，您的 web 應用程式將會顯示預設**首頁**為您的 Facebook 帳戶：</span><span class="sxs-lookup"><span data-stu-id="dcb58-178">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Facebook account:</span></span>

    <span data-ttu-id="dcb58-179">[![](external-authentication-services/_static/image26.png "按一下以展開映像")](external-authentication-services/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-179">[![](external-authentication-services/_static/image26.png "Click to Expand the Image")](external-authentication-services/_static/image25.png)</span></span>

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a><span data-ttu-id="dcb58-180">啟用 Google 驗證</span><span class="sxs-lookup"><span data-stu-id="dcb58-180">Enabling Google Authentication</span></span>

<span data-ttu-id="dcb58-181">使用 Google 驗證會要求您建立 Google 開發人員帳戶，而您的專案需要的應用程式識別碼和祕密金鑰，將來自 Google 才能運作。</span><span class="sxs-lookup"><span data-stu-id="dcb58-181">Using Google authentication requires you to create a Google developer account, and your project will require an application ID and secret key from Google in order to function.</span></span> <span data-ttu-id="dcb58-182">建立 Google 開發人員帳戶，並取得您的應用程式識別碼和祕密金鑰的相關資訊，請參閱[ https://developers.google.com ](https://developers.google.com)。</span><span class="sxs-lookup"><span data-stu-id="dcb58-182">For information about creating a Google developer account and obtaining your application ID and secret key, see [https://developers.google.com](https://developers.google.com).</span></span>


<span data-ttu-id="dcb58-183">若要啟用 Google 驗證 web 應用程式，請使用下列步驟：</span><span class="sxs-lookup"><span data-stu-id="dcb58-183">To enable Google authentication for your web application, use the following steps:</span></span>

1. <span data-ttu-id="dcb58-184">在 Visual Studio 2017 中開啟您的專案時，開啟*Startup.Auth.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="dcb58-184">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="dcb58-185">找出 [Google 驗證] 區段的程式碼：</span><span class="sxs-lookup"><span data-stu-id="dcb58-185">Locate the Google authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. <span data-ttu-id="dcb58-186">移除&quot; // &quot;字元的程式碼中，反白顯示的行取消註解，然後新增 您的應用程式識別碼和祕密金鑰。</span><span class="sxs-lookup"><span data-stu-id="dcb58-186">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your application ID and secret key.</span></span> <span data-ttu-id="dcb58-187">一旦您加入這些參數，您可以重新編譯您的專案：</span><span class="sxs-lookup"><span data-stu-id="dcb58-187">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. <span data-ttu-id="dcb58-188">當您按 f5 鍵在網頁瀏覽器中開啟 web 應用程式時，您會看到，Google 已定義為外部驗證服務：</span><span class="sxs-lookup"><span data-stu-id="dcb58-188">When you press F5 to open your web application in your web browser, you will see that Google has been defined as an external authentication service:</span></span>

    <span data-ttu-id="dcb58-189">[![](external-authentication-services/_static/image75.png "按一下以展開映像")](external-authentication-services/_static/image75.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-189">[![](external-authentication-services/_static/image75.png "Click to Expand the Image")](external-authentication-services/_static/image75.png)</span></span>
5. <span data-ttu-id="dcb58-190">當您按一下 [ **Google** ] 按鈕，您的瀏覽器會被重新導向至 Google 登入頁面：</span><span class="sxs-lookup"><span data-stu-id="dcb58-190">When you click the **Google** button, your browser will be redirected to the Google login page:</span></span>

    <span data-ttu-id="dcb58-191">[![](external-authentication-services/_static/image32.png "按一下以展開映像")](external-authentication-services/_static/image31.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-191">[![](external-authentication-services/_static/image32.png "Click to Expand the Image")](external-authentication-services/_static/image31.png)</span></span>
6. <span data-ttu-id="dcb58-192">您輸入您的 Google 認證，然後按一下 後**登入**，Google 將會提示您確認您的 web 應用程式具有存取您的 Google 帳戶的權限：</span><span class="sxs-lookup"><span data-stu-id="dcb58-192">After you enter your Google credentials and click **Sign in**, Google will prompt you to verify that your web application has permissions to access your Google account:</span></span>

    <span data-ttu-id="dcb58-193">[![](external-authentication-services/_static/image34.png "按一下以展開映像")](external-authentication-services/_static/image33.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-193">[![](external-authentication-services/_static/image34.png "Click to Expand the Image")](external-authentication-services/_static/image33.png)</span></span>
7. <span data-ttu-id="dcb58-194">當您按一下  **Accept**，您的 web 瀏覽器會被重新導向回到您的 web 應用程式，這將會提示您輸入**使用者名稱**您想要將與您的 Google 帳戶產生關聯：</span><span class="sxs-lookup"><span data-stu-id="dcb58-194">When you click **Accept**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Google account:</span></span>

    <span data-ttu-id="dcb58-195">[![](external-authentication-services/_static/image36.png "按一下以展開映像")](external-authentication-services/_static/image35.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-195">[![](external-authentication-services/_static/image36.png "Click to Expand the Image")](external-authentication-services/_static/image35.png)</span></span>
8. <span data-ttu-id="dcb58-196">在您輸入您的使用者名稱並按下後**註冊** 按鈕，您的 web 應用程式將會顯示預設**首頁**為您的 Google 帳戶：</span><span class="sxs-lookup"><span data-stu-id="dcb58-196">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Google account:</span></span>

    <span data-ttu-id="dcb58-197">[![](external-authentication-services/_static/image38.png "按一下以展開映像")](external-authentication-services/_static/image37.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-197">[![](external-authentication-services/_static/image38.png "Click to Expand the Image")](external-authentication-services/_static/image37.png)</span></span>

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a><span data-ttu-id="dcb58-198">啟用 Microsoft 驗證</span><span class="sxs-lookup"><span data-stu-id="dcb58-198">Enabling Microsoft Authentication</span></span>

<span data-ttu-id="dcb58-199">Microsoft 驗證會要求您建立開發人員帳戶，以及它需要用戶端識別碼和用戶端祕密，才能運作。</span><span class="sxs-lookup"><span data-stu-id="dcb58-199">Microsoft authentication requires you to create a developer account, and it requires a client ID and client secret in order to function.</span></span> <span data-ttu-id="dcb58-200">如需建立 Microsoft 開發人員帳戶，並取得您的用戶端識別碼和用戶端祕密資訊，請參閱[ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070)。</span><span class="sxs-lookup"><span data-stu-id="dcb58-200">For information about creating a Microsoft developer account and obtaining your client ID and client secret, see [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070).</span></span>

<span data-ttu-id="dcb58-201">一旦您取得您的取用者金鑰和取用者祕密，請使用下列步驟來啟用 web 應用程式的 Microsoft 驗證：</span><span class="sxs-lookup"><span data-stu-id="dcb58-201">Once you have obtained your consumer key and consumer secret, use the following steps to enable Microsoft authentication for your web application:</span></span>

1. <span data-ttu-id="dcb58-202">在 Visual Studio 2017 中開啟您的專案時，開啟*Startup.Auth.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="dcb58-202">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="dcb58-203">找出程式碼的 Microsoft 驗證 的區段：</span><span class="sxs-lookup"><span data-stu-id="dcb58-203">Locate the Microsoft authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. <span data-ttu-id="dcb58-204">移除&quot; // &quot;取消註解的程式碼中，反白顯示的行，然後新增 您的用戶端識別碼和用戶端祕密的字元。</span><span class="sxs-lookup"><span data-stu-id="dcb58-204">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your client ID and client secret.</span></span> <span data-ttu-id="dcb58-205">一旦您加入這些參數，您可以重新編譯您的專案：</span><span class="sxs-lookup"><span data-stu-id="dcb58-205">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. <span data-ttu-id="dcb58-206">當您按 f5 鍵在網頁瀏覽器中開啟 web 應用程式時，您會看到，Microsoft 已定義為外部驗證服務：</span><span class="sxs-lookup"><span data-stu-id="dcb58-206">When you press F5 to open your web application in your web browser, you will see that Microsoft has been defined as an external authentication service:</span></span>

    <span data-ttu-id="dcb58-207">[![](external-authentication-services/_static/image42.png "按一下以展開映像")](external-authentication-services/_static/image41.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-207">[![](external-authentication-services/_static/image42.png "Click to Expand the Image")](external-authentication-services/_static/image41.png)</span></span>
5. <span data-ttu-id="dcb58-208">當您按一下 [ **Microsoft** ] 按鈕，您的瀏覽器會被重新導向至 Microsoft 登入頁面：</span><span class="sxs-lookup"><span data-stu-id="dcb58-208">When you click the **Microsoft** button, your browser will be redirected to the Microsoft login page:</span></span>

    <span data-ttu-id="dcb58-209">[![](external-authentication-services/_static/image44.png "按一下以展開映像")](external-authentication-services/_static/image43.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-209">[![](external-authentication-services/_static/image44.png "Click to Expand the Image")](external-authentication-services/_static/image43.png)</span></span>
6. <span data-ttu-id="dcb58-210">您輸入您的 Microsoft 認證，然後按一下 後**登入**，系統會提示您確認您的 web 應用程式具有存取您的 Microsoft 帳戶的權限：</span><span class="sxs-lookup"><span data-stu-id="dcb58-210">After you enter your Microsoft credentials and click **Sign in**, you will be prompted to verify that your web application has permissions to access your Microsoft account:</span></span>

    <span data-ttu-id="dcb58-211">[![](external-authentication-services/_static/image46.png "按一下以展開映像")](external-authentication-services/_static/image45.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-211">[![](external-authentication-services/_static/image46.png "Click to Expand the Image")](external-authentication-services/_static/image45.png)</span></span>
7. <span data-ttu-id="dcb58-212">當您按一下  **是**，您的 web 瀏覽器會被重新導向回到您的 web 應用程式，這將會提示您輸入**使用者名稱**您想要將與您的 Microsoft 帳戶產生關聯：</span><span class="sxs-lookup"><span data-stu-id="dcb58-212">When you click **Yes**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Microsoft account:</span></span>

    <span data-ttu-id="dcb58-213">[![](external-authentication-services/_static/image48.png "按一下以展開映像")](external-authentication-services/_static/image47.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-213">[![](external-authentication-services/_static/image48.png "Click to Expand the Image")](external-authentication-services/_static/image47.png)</span></span>
8. <span data-ttu-id="dcb58-214">在您輸入您的使用者名稱並按下後**註冊** 按鈕，您的 web 應用程式將會顯示預設**首頁**您的 Microsoft 帳戶：</span><span class="sxs-lookup"><span data-stu-id="dcb58-214">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Microsoft account:</span></span>

    <span data-ttu-id="dcb58-215">[![](external-authentication-services/_static/image50.png "按一下以展開映像")](external-authentication-services/_static/image49.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-215">[![](external-authentication-services/_static/image50.png "Click to Expand the Image")](external-authentication-services/_static/image49.png)</span></span>

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a><span data-ttu-id="dcb58-216">啟用 Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="dcb58-216">Enabling Twitter Authentication</span></span>

<span data-ttu-id="dcb58-217">Twitter 驗證會要求您建立開發人員帳戶，以及它需要取用者金鑰和取用者祕密，才能運作。</span><span class="sxs-lookup"><span data-stu-id="dcb58-217">Twitter authentication requires you to create a developer account, and it requires a consumer key and consumer secret in order to function.</span></span> <span data-ttu-id="dcb58-218">建立 Twitter 開發人員帳戶，並取得您的取用者金鑰和取用者祕密的相關資訊，請參閱[ https://go.microsoft.com/fwlink/?LinkID=252166 ](https://go.microsoft.com/fwlink/?LinkID=252166)。</span><span class="sxs-lookup"><span data-stu-id="dcb58-218">For information about creating a Twitter developer account and obtaining your consumer key and consumer secret, see [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).</span></span>

<span data-ttu-id="dcb58-219">一旦您取得您的取用者金鑰和取用者祕密，請使用下列步驟啟用 Twitter 驗證 web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="dcb58-219">Once you have obtained your consumer key and consumer secret, use the following steps to enable Twitter authentication for your web application:</span></span>

1. <span data-ttu-id="dcb58-220">在 Visual Studio 2017 中開啟您的專案時，開啟*Startup.Auth.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="dcb58-220">When your project is open in Visual Studio 2017, open the *Startup.Auth.cs* file.</span></span>

2. <span data-ttu-id="dcb58-221">找出程式碼的 Twitter 驗證 的區段：</span><span class="sxs-lookup"><span data-stu-id="dcb58-221">Locate the Twitter authentication section of code:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. <span data-ttu-id="dcb58-222">移除&quot; // &quot;取消註解的程式碼中，反白顯示的行，然後新增 您的取用者金鑰和取用者密碼的字元。</span><span class="sxs-lookup"><span data-stu-id="dcb58-222">Remove the &quot;//&quot; characters to uncomment the highlighted lines of code, and then add your consumer key and consumer secret.</span></span> <span data-ttu-id="dcb58-223">一旦您加入這些參數，您可以重新編譯您的專案：</span><span class="sxs-lookup"><span data-stu-id="dcb58-223">Once you have added those parameters, you can recompile your project:</span></span>

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. <span data-ttu-id="dcb58-224">當您按 f5 鍵在網頁瀏覽器中開啟 web 應用程式時，您會看到 Twitter，已定義為外部驗證服務：</span><span class="sxs-lookup"><span data-stu-id="dcb58-224">When you press F5 to open your web application in your web browser, you will see that Twitter has been defined as an external authentication service:</span></span>

    <span data-ttu-id="dcb58-225">[![](external-authentication-services/_static/image54.png "按一下以展開映像")](external-authentication-services/_static/image53.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-225">[![](external-authentication-services/_static/image54.png "Click to Expand the Image")](external-authentication-services/_static/image53.png)</span></span>
5. <span data-ttu-id="dcb58-226">當您按一下 [ **Twitter** ] 按鈕，您的瀏覽器會被重新導向至 Twitter 登入頁面：</span><span class="sxs-lookup"><span data-stu-id="dcb58-226">When you click the **Twitter** button, your browser will be redirected to the Twitter login page:</span></span>

    <span data-ttu-id="dcb58-227">[![](external-authentication-services/_static/image56.png "按一下以展開映像")](external-authentication-services/_static/image55.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-227">[![](external-authentication-services/_static/image56.png "Click to Expand the Image")](external-authentication-services/_static/image55.png)</span></span>
6. <span data-ttu-id="dcb58-228">您輸入您的 Twitter 認證，然後按一下 後**授權應用程式**，您的 web 瀏覽器會被重新導向回到您的 web 應用程式，這將會提示您輸入**使用者名稱**您想要與產生關聯您的 Twitter 帳戶：</span><span class="sxs-lookup"><span data-stu-id="dcb58-228">After you enter your Twitter credentials and click **Authorize app**, your web browser will be redirected back to your web application, which will prompt you for the **User name** that you want to associate with your Twitter account:</span></span>

    <span data-ttu-id="dcb58-229">[![](external-authentication-services/_static/image58.png "按一下以展開映像")](external-authentication-services/_static/image57.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-229">[![](external-authentication-services/_static/image58.png "Click to Expand the Image")](external-authentication-services/_static/image57.png)</span></span>
7. <span data-ttu-id="dcb58-230">您輸入您的使用者名稱並按下後**註冊** 按鈕，您的 web 應用程式將會顯示預設**首頁**為您的 Twitter 帳戶：</span><span class="sxs-lookup"><span data-stu-id="dcb58-230">After you have entered your user name and clicked the **Sign up** button, your web application will display the default **home page** for your Twitter account:</span></span>

    <span data-ttu-id="dcb58-231">[![](external-authentication-services/_static/image60.png "按一下以展開映像")](external-authentication-services/_static/image59.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-231">[![](external-authentication-services/_static/image60.png "Click to Expand the Image")](external-authentication-services/_static/image59.png)</span></span>

<a id="MOREINFO"></a>
## <a name="additional-information"></a><span data-ttu-id="dcb58-232">其他資訊</span><span class="sxs-lookup"><span data-stu-id="dcb58-232">Additional Information</span></span>

<span data-ttu-id="dcb58-233">如需建立使用 OAuth 和 OpenID 應用程式的詳細資訊，請參閱下列 Url:</span><span class="sxs-lookup"><span data-stu-id="dcb58-233">For additional information about creating applications that use OAuth and OpenID, see the following URLs:</span></span>

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a><span data-ttu-id="dcb58-234">結合外部驗證服務</span><span class="sxs-lookup"><span data-stu-id="dcb58-234">Combining External Authentication Services</span></span>

<span data-ttu-id="dcb58-235">更大的彈性，您可以定義多個外部驗證服務在相同的時間-這可讓您的 web 應用程式的使用者使用任何已啟用的外部驗證服務的帳戶：</span><span class="sxs-lookup"><span data-stu-id="dcb58-235">For greater flexibility, you can define multiple external authentication services at the same time - this allows your web application's users to use an account from any of the enabled external authentication services:</span></span>

<span data-ttu-id="dcb58-236">[![](external-authentication-services/_static/image62.png "按一下以展開映像")](external-authentication-services/_static/image61.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-236">[![](external-authentication-services/_static/image62.png "Click to Expand the Image")](external-authentication-services/_static/image61.png)</span></span>

<a id="FQDN"></a>
### <a name="configure-iis-express-to-use-a-fully-qualified-domain-name"></a><span data-ttu-id="dcb58-237">設定 IIS Express 以使用完整的網域名稱</span><span class="sxs-lookup"><span data-stu-id="dcb58-237">Configure IIS Express to use a fully qualified domain name</span></span>

<span data-ttu-id="dcb58-238">某些外部驗證提供者不支援測試您的應用程式使用 HTTP 位址，例如`http://localhost:port/`。</span><span class="sxs-lookup"><span data-stu-id="dcb58-238">Some external authentication providers do not support testing your application by using an HTTP address like `http://localhost:port/`.</span></span> <span data-ttu-id="dcb58-239">若要解決此問題，您可以新增靜態的完整格式網域名稱 (FQDN) 對應至主機檔案，並設定要用於測試/偵錯 FQDN 的 Visual Studio 2017 中的專案選項。</span><span class="sxs-lookup"><span data-stu-id="dcb58-239">To work around this issue, you can add a static Fully Qualified Domain Name (FQDN) mapping to your HOSTS file and configure your project options in Visual Studio 2017 to use the FQDN for testing/debugging.</span></span> <span data-ttu-id="dcb58-240">若要這樣做，請使用下列步驟：</span><span class="sxs-lookup"><span data-stu-id="dcb58-240">To do so, use the following steps:</span></span>

- <span data-ttu-id="dcb58-241">新增靜態 FQDN 對應您的主機檔案：</span><span class="sxs-lookup"><span data-stu-id="dcb58-241">Add a static FQDN mapping your HOSTS file:</span></span>

  1. <span data-ttu-id="dcb58-242">在 Windows 中，開啟提升權限的命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="dcb58-242">Open an elevated command prompt in Windows.</span></span>
  2. <span data-ttu-id="dcb58-243">輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="dcb58-243">Type the following command:</span></span>

      <span data-ttu-id="dcb58-244"><kbd>在 [記事本] %WinDir%\system32\drivers\etc\hosts</kbd></span><span class="sxs-lookup"><span data-stu-id="dcb58-244"><kbd>notepad %WinDir%\system32\drivers\etc\hosts</kbd></span></span>
  3. <span data-ttu-id="dcb58-245">主機檔案中加入的項目，如下所示：</span><span class="sxs-lookup"><span data-stu-id="dcb58-245">Add an entry like the following to the HOSTS file:</span></span>

      <span data-ttu-id="dcb58-246"><kbd>127.0.0.1 www.wingtiptoys.com</kbd></span><span class="sxs-lookup"><span data-stu-id="dcb58-246"><kbd>127.0.0.1 www.wingtiptoys.com</kbd></span></span>
  4. <span data-ttu-id="dcb58-247">儲存並關閉您的主機檔案。</span><span class="sxs-lookup"><span data-stu-id="dcb58-247">Save and close your HOSTS file.</span></span>

- <span data-ttu-id="dcb58-248">設定 Visual Studio 專案使用的 FQDN:</span><span class="sxs-lookup"><span data-stu-id="dcb58-248">Configure your Visual Studio project to use the FQDN:</span></span>

  1. <span data-ttu-id="dcb58-249">在 Visual Studio 2017 中開啟您的專案時，請按一下**專案**功能表，然後選取 專案屬性。</span><span class="sxs-lookup"><span data-stu-id="dcb58-249">When your project is open in Visual Studio 2017, click the **Project** menu, and then select your project's properties.</span></span> <span data-ttu-id="dcb58-250">例如，您可以選取**WebApplication1 屬性**。</span><span class="sxs-lookup"><span data-stu-id="dcb58-250">For example, you might select **WebApplication1 Properties**.</span></span>
  2. <span data-ttu-id="dcb58-251">選取 [ **Web** ] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="dcb58-251">Select the **Web** tab.</span></span>
  3. <span data-ttu-id="dcb58-252">輸入的 FQDN<strong>專案 Url</strong>。</span><span class="sxs-lookup"><span data-stu-id="dcb58-252">Enter your FQDN for the <strong>Project Url</strong>.</span></span> <span data-ttu-id="dcb58-253">例如，您會輸入<kbd> <http://www.wingtiptoys.com> </kbd>如果這是您新增至主機檔案的 FQDN 對應。</span><span class="sxs-lookup"><span data-stu-id="dcb58-253">For example, you would enter <kbd><http://www.wingtiptoys.com></kbd> if that was the FQDN mapping that you added to your HOSTS file.</span></span>

- <span data-ttu-id="dcb58-254">設定 IIS Express 以使用您的應用程式的 FQDN:</span><span class="sxs-lookup"><span data-stu-id="dcb58-254">Configure IIS Express to use the FQDN for your application:</span></span>

    1. <span data-ttu-id="dcb58-255">在 Windows 中，開啟提升權限的命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="dcb58-255">Open an elevated command prompt in Windows.</span></span>
    2. <span data-ttu-id="dcb58-256">輸入下列命令，以將變更為您的 IIS Express 資料夾：</span><span class="sxs-lookup"><span data-stu-id="dcb58-256">Type the following command to change to your IIS Express folder:</span></span>

        <span data-ttu-id="dcb58-257"><kbd>cd /d &quot;%ProgramFiles%\IIS Express&quot;</kbd></span><span class="sxs-lookup"><span data-stu-id="dcb58-257"><kbd>cd /d &quot;%ProgramFiles%\IIS Express&quot;</kbd></span></span>
    3. <span data-ttu-id="dcb58-258">輸入下列命令以將 FQDN 新增到您的應用程式：</span><span class="sxs-lookup"><span data-stu-id="dcb58-258">Type the following command to add the FQDN to your application:</span></span>

        <span data-ttu-id="dcb58-259"><kbd>appcmd.exe set config -section:system.applicationHost/sites /+&quot;[name='WebApplication1'].bindings.[protocol='http',bindingInformation='\*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd></span><span class="sxs-lookup"><span data-stu-id="dcb58-259"><kbd>appcmd.exe set config -section:system.applicationHost/sites /+&quot;[name='WebApplication1'].bindings.[protocol='http',bindingInformation='\*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd></span></span>

  <span data-ttu-id="dcb58-260">何處**WebApplication1**是您的專案名稱並**bindingInformation**包含您想要用於測試的連接埠號碼和 FQDN。</span><span class="sxs-lookup"><span data-stu-id="dcb58-260">Where **WebApplication1** is the name of your project and **bindingInformation** contains the port number and FQDN that you want to use for your testing.</span></span>

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a><span data-ttu-id="dcb58-261">濆爧髍孮 Microsoft 驗證您的應用程式設定</span><span class="sxs-lookup"><span data-stu-id="dcb58-261">How to Obtain your Application Settings for Microsoft Authentication</span></span>

<span data-ttu-id="dcb58-262">連結至 Windows Live，Microsoft 驗證應用程式是簡單的程序。</span><span class="sxs-lookup"><span data-stu-id="dcb58-262">Linking an application to Windows Live for Microsoft Authentication is a simple process.</span></span> <span data-ttu-id="dcb58-263">如果您未連結至 Windows Live 應用程式，您可以使用下列步驟：</span><span class="sxs-lookup"><span data-stu-id="dcb58-263">If you have not already linked an application to Windows Live, you can use the following steps:</span></span>

1. <span data-ttu-id="dcb58-264">瀏覽至[ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070)並輸入您的 Microsoft 帳戶名稱和密碼提示時，然後按一下**登入**:</span><span class="sxs-lookup"><span data-stu-id="dcb58-264">Browse to [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070) and enter your Microsoft account name and password when prompted, then click **Sign in**:</span></span>

   <!--  [![](external-authentication-services/_static/image64.png "Click to Expand the Image")](external-authentication-services/_static/image63.png) -->
2. <span data-ttu-id="dcb58-265">選取 **新增應用程式**並輸入您的應用程式出現提示時，名稱，然後按一下**建立**:</span><span class="sxs-lookup"><span data-stu-id="dcb58-265">Select **Add an app** and enter the name of your application when prompted, and then click **Create**:</span></span>

    <span data-ttu-id="dcb58-266">[![](external-authentication-services/_static/image79.png "按一下以展開映像")](external-authentication-services/_static/image79.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-266">[![](external-authentication-services/_static/image79.png "Click to Expand the Image")](external-authentication-services/_static/image79.png)</span></span>
3. <span data-ttu-id="dcb58-267">選取您的應用程式**名稱**和其應用程式屬性頁面隨即出現。</span><span class="sxs-lookup"><span data-stu-id="dcb58-267">Select your app under **Name** and its application properties page appears.</span></span>

4. <span data-ttu-id="dcb58-268">輸入您的應用程式的重新導向網域。</span><span class="sxs-lookup"><span data-stu-id="dcb58-268">Enter the redirect domain for your application.</span></span> <span data-ttu-id="dcb58-269">複製**應用程式識別碼**和 **應用程式祕密**，選取**產生密碼**。</span><span class="sxs-lookup"><span data-stu-id="dcb58-269">Copy the **Application ID** and, under **Application Secrets**, select **Generate Password**.</span></span> <span data-ttu-id="dcb58-270">複製顯示的密碼。</span><span class="sxs-lookup"><span data-stu-id="dcb58-270">Copy the password that appears.</span></span> <span data-ttu-id="dcb58-271">應用程式識別碼和密碼是您的用戶端識別碼和用戶端祕密。</span><span class="sxs-lookup"><span data-stu-id="dcb58-271">The application ID and password are your client ID and client secret.</span></span> <span data-ttu-id="dcb58-272">選取  **Ok** ，然後**儲存**。</span><span class="sxs-lookup"><span data-stu-id="dcb58-272">Select **Ok** and then **Save**.</span></span>

    <span data-ttu-id="dcb58-273">[![](external-authentication-services/_static/image77.png "按一下以展開映像")](external-authentication-services/_static/image77.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-273">[![](external-authentication-services/_static/image77.png "Click to Expand the Image")](external-authentication-services/_static/image77.png)</span></span>

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a><span data-ttu-id="dcb58-274">選擇項：停用本機註冊</span><span class="sxs-lookup"><span data-stu-id="dcb58-274">Optional: Disable Local Registration</span></span>

<span data-ttu-id="dcb58-275">目前的 ASP.NET 本機註冊功能不會防止自動的程式 (bot) 帳戶; 建立成員例如，藉由使用一種 bot 防護及驗證的技術，像是[CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md)。</span><span class="sxs-lookup"><span data-stu-id="dcb58-275">The current ASP.NET local registration functionality does not prevent automated programs (bots) from creating member accounts; for example, by using a bot-prevention and validation technology like [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md).</span></span> <span data-ttu-id="dcb58-276">基於這個原因，您應該移除的登入頁面上的本機登入表單和註冊連結。</span><span class="sxs-lookup"><span data-stu-id="dcb58-276">Because of this, you should remove the local login form and registration link on the login page.</span></span> <span data-ttu-id="dcb58-277">若要這樣做，請開啟 *\_Login.cshtml*在專案中，頁面上，然後再標記為註解的線條，「 本機登入面板和 [註冊] 連結。</span><span class="sxs-lookup"><span data-stu-id="dcb58-277">To do so, open the *\_Login.cshtml* page in your project, and then comment out the lines for the local login panel and the registration link.</span></span> <span data-ttu-id="dcb58-278">產生的頁面看起來應該像下列程式碼範例：</span><span class="sxs-lookup"><span data-stu-id="dcb58-278">The resulting page should look like the following code sample:</span></span>

[!code-html[Main](external-authentication-services/samples/sample10.html)]

<span data-ttu-id="dcb58-279">本機登入面板和 [註冊] 連結已停用，一旦您登入頁面只會顯示您已啟用外部驗證提供者：</span><span class="sxs-lookup"><span data-stu-id="dcb58-279">Once the local login panel and the registration link have been disabled, your login page will only display the external authentication providers that you have enabled:</span></span>

<span data-ttu-id="dcb58-280">[![](external-authentication-services/_static/image70.png "按一下以展開映像")](external-authentication-services/_static/image69.png)</span><span class="sxs-lookup"><span data-stu-id="dcb58-280">[![](external-authentication-services/_static/image70.png "Click to Expand the Image")](external-authentication-services/_static/image69.png)</span></span>
