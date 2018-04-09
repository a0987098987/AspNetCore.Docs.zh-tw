---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: 傳送電子郵件從 ASP.NET Web Pages (Razor) 站台 |Microsoft 文件
author: tfitzmac
description: 本章節將說明如何從網站傳送自動化電子郵件訊息。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 9be242d238c627a9557fe7ff7e596974e5b7d1c8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="sending-email-from-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="a52cf-103">從 ASP.NET Web Pages (Razor) 站台傳送的電子郵件</span><span class="sxs-lookup"><span data-stu-id="a52cf-103">Sending Email from an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="a52cf-104">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a52cf-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a52cf-105">本文說明如何從網站傳送電子郵件，當您使用 ASP.NET Web Pages (Razor)。</span><span class="sxs-lookup"><span data-stu-id="a52cf-105">This article explains how to send an email message from a website when you use ASP.NET Web Pages (Razor).</span></span>
> 
> <span data-ttu-id="a52cf-106">您將學習：</span><span class="sxs-lookup"><span data-stu-id="a52cf-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="a52cf-107">如何從您的網站傳送電子郵件訊息。</span><span class="sxs-lookup"><span data-stu-id="a52cf-107">How to send an email message from your website.</span></span>
> - <span data-ttu-id="a52cf-108">如何將檔案附加到電子郵件。</span><span class="sxs-lookup"><span data-stu-id="a52cf-108">How to attach a file to an email message.</span></span>
> 
> <span data-ttu-id="a52cf-109">這是發行項中所引進的 ASP.NET 功能：</span><span class="sxs-lookup"><span data-stu-id="a52cf-109">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="a52cf-110">`WebMail`協助程式。</span><span class="sxs-lookup"><span data-stu-id="a52cf-110">The `WebMail` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a52cf-111">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="a52cf-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a52cf-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="a52cf-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="a52cf-113">本教學課程也適用於 ASP.NET Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="a52cf-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a><span data-ttu-id="a52cf-114">從您的網站傳送電子郵件訊息</span><span class="sxs-lookup"><span data-stu-id="a52cf-114">Sending Email Messages from Your Website</span></span>

<span data-ttu-id="a52cf-115">有各式各樣的情況下，您可能必須從您的網站傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="a52cf-115">There are all sorts of reasons why you might need to send email from your website.</span></span> <span data-ttu-id="a52cf-116">您可能會將確認訊息傳送給使用者，或您可能會傳送通知給自己 （例如，註冊新的使用者。）`WebMail` Helper 讓您輕鬆地傳送電子郵件。</span><span class="sxs-lookup"><span data-stu-id="a52cf-116">You might send confirmation messages to users, or you might send notifications to yourself (for example, that a new user has registered.) The `WebMail` helper makes it easy for you to send email.</span></span>

<span data-ttu-id="a52cf-117">若要使用`WebMail`協助程式，您必須擁有 SMTP 伺服器的存取權。</span><span class="sxs-lookup"><span data-stu-id="a52cf-117">To use the `WebMail` helper, you have to have access to an SMTP server.</span></span> <span data-ttu-id="a52cf-118">(代表 SMTP *Simple Mail Transfer Protocol*。)SMTP 伺服器是只將訊息轉寄至收件者伺服器的電子郵件伺服器&#8212;是輸出端的電子郵件。</span><span class="sxs-lookup"><span data-stu-id="a52cf-118">(SMTP stands for *Simple Mail Transfer Protocol*.) An SMTP server is an email server that only forwards messages to the recipient's server &#8212; it's the outbound side of email.</span></span> <span data-ttu-id="a52cf-119">如果您為您的網站使用主機服務提供者，它們可能是設定您的電子郵件，他們可以告訴您 SMTP 伺服器名稱是什麼。</span><span class="sxs-lookup"><span data-stu-id="a52cf-119">If you use a hosting provider for your website, they probably set you up with email and they can tell you what your SMTP server name is.</span></span> <span data-ttu-id="a52cf-120">若您使用公司網路內部時，系統管理員或 IT 部門可以通常提供您可以使用 SMTP 伺服器的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="a52cf-120">If you're working inside a corporate network, an administrator or your IT department can usually give you the information about an SMTP server that you can use.</span></span> <span data-ttu-id="a52cf-121">如果您在家工作，您甚至可以測試使用一般的電子郵件提供者，能告訴您的 SMTP 伺服器名稱。</span><span class="sxs-lookup"><span data-stu-id="a52cf-121">If you're working at home, you might even be able to test using your ordinary email provider, who can tell you the name of their SMTP server.</span></span> <span data-ttu-id="a52cf-122">您通常需要：</span><span class="sxs-lookup"><span data-stu-id="a52cf-122">You typically need:</span></span>

- <span data-ttu-id="a52cf-123">SMTP 伺服器的名稱。</span><span class="sxs-lookup"><span data-stu-id="a52cf-123">The name of the SMTP server.</span></span>
- <span data-ttu-id="a52cf-124">連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="a52cf-124">The port number.</span></span> <span data-ttu-id="a52cf-125">這幾乎都是 25。</span><span class="sxs-lookup"><span data-stu-id="a52cf-125">This is almost always 25.</span></span> <span data-ttu-id="a52cf-126">不過，您的 ISP 可能需要您使用連接埠 587。</span><span class="sxs-lookup"><span data-stu-id="a52cf-126">However, your ISP may require you to use port 587.</span></span> <span data-ttu-id="a52cf-127">如果您使用安全通訊端層 (SSL) 的電子郵件，您可能需要不同的通訊埠。</span><span class="sxs-lookup"><span data-stu-id="a52cf-127">If you are using secure sockets layer (SSL) for email, you might need a different port.</span></span> <span data-ttu-id="a52cf-128">請洽詢您的電子郵件提供者。</span><span class="sxs-lookup"><span data-stu-id="a52cf-128">Check with your email provider.</span></span>
- <span data-ttu-id="a52cf-129">認證 （使用者名稱、 密碼）。</span><span class="sxs-lookup"><span data-stu-id="a52cf-129">Credentials (user name, password).</span></span>

<span data-ttu-id="a52cf-130">在此程序，您可以建立兩個頁面。</span><span class="sxs-lookup"><span data-stu-id="a52cf-130">In this procedure, you create two pages.</span></span> <span data-ttu-id="a52cf-131">第一頁都有一個表單，讓使用者輸入的描述，如同它們已填入技術支援表單。</span><span class="sxs-lookup"><span data-stu-id="a52cf-131">The first page has a form that lets users enter a description, as if they were filling in a technical-support form.</span></span> <span data-ttu-id="a52cf-132">第一頁提交其第二個頁面的資訊。</span><span class="sxs-lookup"><span data-stu-id="a52cf-132">The first page submits its information to a second page.</span></span> <span data-ttu-id="a52cf-133">在第二個頁面中，程式碼會擷取使用者的資訊，然後傳送電子郵件訊息。</span><span class="sxs-lookup"><span data-stu-id="a52cf-133">In the second page, code extracts the user's information and sends an email message.</span></span> <span data-ttu-id="a52cf-134">它也會顯示訊息，確認已收到問題報告。</span><span class="sxs-lookup"><span data-stu-id="a52cf-134">It also displays a message confirming that the problem report has been received.</span></span>

![[影像]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> <span data-ttu-id="a52cf-136">為了簡化此範例中，程式碼會初始化`WebMail`helper 在頁面中，您使用的權限。</span><span class="sxs-lookup"><span data-stu-id="a52cf-136">To keep this example simple, the code initializes the `WebMail` helper right in the page where you use it.</span></span> <span data-ttu-id="a52cf-137">不過，實際的網站，它是，像這樣的初始化程式碼置於通用檔案，好，以便您初始化`WebMail`helper，您的網站中的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="a52cf-137">However, for real websites, it's a better idea to put initialization code like this in a global file, so that you initialize the `WebMail` helper for all files in your website.</span></span> <span data-ttu-id="a52cf-138">如需詳細資訊，請參閱[自訂全站台的行為的 ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers)。</span><span class="sxs-lookup"><span data-stu-id="a52cf-138">For more information, see [Customizing Site-Wide Behavior for ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers).</span></span>


1. <span data-ttu-id="a52cf-139">建立新的網站。</span><span class="sxs-lookup"><span data-stu-id="a52cf-139">Create a new website.</span></span>
2. <span data-ttu-id="a52cf-140">加入新的頁面名稱為*EmailRequest.cshtml*並加入下列標記：</span><span class="sxs-lookup"><span data-stu-id="a52cf-140">Add a new page named *EmailRequest.cshtml* and add the following markup:</span></span> 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    <span data-ttu-id="a52cf-141">請注意，`action`表單項目的屬性已設定為*ProcessRequest.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="a52cf-141">Notice that the `action` attribute of the form element has been set to *ProcessRequest.cshtml*.</span></span> <span data-ttu-id="a52cf-142">這表示提交表單，至該頁面，而不是目前頁面的上一頁。</span><span class="sxs-lookup"><span data-stu-id="a52cf-142">This means that the form will be submitted to that page instead of back to the current page.</span></span>
3. <span data-ttu-id="a52cf-143">加入新的頁面名稱為*ProcessRequest.cshtml*網站並加入下列程式碼和標記：</span><span class="sxs-lookup"><span data-stu-id="a52cf-143">Add a new page named *ProcessRequest.cshtml* to the website and add the following code and markup:</span></span>   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    <span data-ttu-id="a52cf-144">在程式碼中，您會取得提交至網頁表單欄位的值。</span><span class="sxs-lookup"><span data-stu-id="a52cf-144">In the code, you get the values of the form fields that were submitted to the page.</span></span> <span data-ttu-id="a52cf-145">然後呼叫`WebMail`協助專家的`Send`方法來建立並傳送電子郵件訊息。</span><span class="sxs-lookup"><span data-stu-id="a52cf-145">You then call the `WebMail` helper's `Send` method to create and send the email message.</span></span> <span data-ttu-id="a52cf-146">在此情況下，要使用的值所組成，您已送出表單中的值串連的文字。</span><span class="sxs-lookup"><span data-stu-id="a52cf-146">In this case, the values to use are made up of text that you concatenate with the values that were submitted from the form.</span></span>

    <span data-ttu-id="a52cf-147">此頁面的程式碼位於`try/catch`區塊。</span><span class="sxs-lookup"><span data-stu-id="a52cf-147">The code for this page is inside a `try/catch` block.</span></span> <span data-ttu-id="a52cf-148">如果任何原因嘗試傳送電子郵件無效 （例如，設定不正確） 中的程式碼`catch`區塊會執行，並設定`errorMessage`變數設為已發生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="a52cf-148">If for any reason the attempt to send an email doesn't work (for example, the settings aren't right), the code in the `catch` block runs and sets the `errorMessage` variable to the error that has occurred.</span></span> <span data-ttu-id="a52cf-149">(如需有關`try/catch`區塊或`<text>`標記中，請參閱[ASP.NET Web Pages 程式設計使用 Razor 語法的簡介](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors)。)</span><span class="sxs-lookup"><span data-stu-id="a52cf-149">(For more information about `try/catch` blocks or the `<text>` tag, see [Introduction to ASP.NET Web Pages Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors).)</span></span>

    <span data-ttu-id="a52cf-150">本文的頁面上，如果`errorMessage`變數是空的 （預設值），使用者會看到已傳送電子郵件訊息的訊息。</span><span class="sxs-lookup"><span data-stu-id="a52cf-150">In the body of the page, if the `errorMessage` variable is empty (the default), the user sees a message that the email message has been sent.</span></span> <span data-ttu-id="a52cf-151">如果`errorMessage`變數設為 true 時，使用者所看到的訊息已傳送訊息的問題。</span><span class="sxs-lookup"><span data-stu-id="a52cf-151">If the `errorMessage` variable is set to true, the user sees a message that there's been a problem sending the message.</span></span>

    <span data-ttu-id="a52cf-152">請注意，在的頁面會顯示錯誤訊息的部分，有額外的測試： `if(debuggingFlag)`。</span><span class="sxs-lookup"><span data-stu-id="a52cf-152">Notice that in the portion of the page that displays an error message, there's an additional test: `if(debuggingFlag)`.</span></span> <span data-ttu-id="a52cf-153">這是您可以設定為 true，如果您遇到傳送電子郵件的變數。</span><span class="sxs-lookup"><span data-stu-id="a52cf-153">This is a variable that you can set to true if you're having trouble sending email.</span></span> <span data-ttu-id="a52cf-154">當`debuggingFlag`為 true，而且如果沒有問題，傳送電子郵件，一則錯誤訊息會顯示顯示嘗試傳送電子郵件訊息時，ASP.NET 任何已回報。</span><span class="sxs-lookup"><span data-stu-id="a52cf-154">When `debuggingFlag` is true, and if there's a problem sending email, an additional error message is displayed that shows whatever ASP.NET has reported when it tried to send the email message.</span></span> <span data-ttu-id="a52cf-155">公平警告，不過： ASP.NET 報告它無法傳送電子郵件訊息時的錯誤訊息可以是泛型。</span><span class="sxs-lookup"><span data-stu-id="a52cf-155">Fair warning, though: the error messages that ASP.NET reports when it can't send an email message can be generic.</span></span> <span data-ttu-id="a52cf-156">例如，如果 ASP.NET 無法連絡 SMTP 伺服器 （例如，因為您在 伺服器名稱做錯誤），錯誤為`Failure sending mail`。</span><span class="sxs-lookup"><span data-stu-id="a52cf-156">For example, if ASP.NET can't contact the SMTP server (for example, because you made an error in the server name), the error is `Failure sending mail`.</span></span>

    > [!NOTE] 
    > 
    > <span data-ttu-id="a52cf-157">**重要**例外狀況物件所取得的錯誤訊息時 (`ex`程式碼中)，請勿*不*定期將透過該訊息傳遞給使用者。</span><span class="sxs-lookup"><span data-stu-id="a52cf-157">**Important** When you get an error message from an exception object (`ex` in the code), do *not* routinely pass that message through to users.</span></span> <span data-ttu-id="a52cf-158">例外狀況物件通常會包含使用者不應看到及，甚至可以是安全性弱點資訊。</span><span class="sxs-lookup"><span data-stu-id="a52cf-158">Exception objects often include information that users should not see and that can even be a security vulnerability.</span></span> <span data-ttu-id="a52cf-159">這就是為什麼這段程式碼包含變數`debuggingFlag`用做為參數來顯示錯誤訊息，以及為什麼預設變數設為 false。</span><span class="sxs-lookup"><span data-stu-id="a52cf-159">That's why this code includes the variable `debuggingFlag` that's used as a switch to display the error message, and why the variable by default is set to false.</span></span> <span data-ttu-id="a52cf-160">您應該設定為 true （然後因此顯示錯誤訊息），該變數*只*如果您遇到的問題傳送電子郵件，而且您需要偵錯。</span><span class="sxs-lookup"><span data-stu-id="a52cf-160">You should set that variable to true (and therefore display the error message) *only* if you're having a problem with sending email and you need to debug.</span></span> <span data-ttu-id="a52cf-161">一旦您已修正任何問題，設定`debuggingFlag`回為 false。</span><span class="sxs-lookup"><span data-stu-id="a52cf-161">Once you have fixed any problems, set `debuggingFlag` back to false.</span></span>

    <span data-ttu-id="a52cf-162">修改下列電子郵件的程式碼中的相關的設定：</span><span class="sxs-lookup"><span data-stu-id="a52cf-162">Modify the following email related settings in the code:</span></span>

   - <span data-ttu-id="a52cf-163">設定`your-SMTP-host`您具有存取權的 SMTP 伺服器的名稱。</span><span class="sxs-lookup"><span data-stu-id="a52cf-163">Set `your-SMTP-host` to the name of the SMTP server that you have access to.</span></span>
   - <span data-ttu-id="a52cf-164">設定`your-user-name-here`至您的 SMTP 伺服器帳戶的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="a52cf-164">Set `your-user-name-here` to the user name for your SMTP server account.</span></span>
   - <span data-ttu-id="a52cf-165">設定`your-account-password`您的 SMTP 伺服器帳戶的密碼。</span><span class="sxs-lookup"><span data-stu-id="a52cf-165">Set `your-account-password` to the password for your SMTP server account.</span></span>
   - <span data-ttu-id="a52cf-166">設定`your-email-address-here`您自己的電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="a52cf-166">Set `your-email-address-here` to your own email address.</span></span> <span data-ttu-id="a52cf-167">這是從訊息傳送的電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="a52cf-167">This is the email address that the message is sent from.</span></span> <span data-ttu-id="a52cf-168">(某些電子郵件提供者不要讓您指定不同`From`位址，且將會使用您的使用者名稱做為`From`位址。)</span><span class="sxs-lookup"><span data-stu-id="a52cf-168">(Some email providers don't let you specify a different `From` address and will use your user name as the `From` address.)</span></span>

     > [!TIP] 
     > 
     > <a id="configuring_email_settings"></a>
     > ### <a name="configuring-email-settings"></a><span data-ttu-id="a52cf-169">設定電子郵件設定</span><span class="sxs-lookup"><span data-stu-id="a52cf-169">Configuring Email Settings</span></span>
     > 
     > <span data-ttu-id="a52cf-170">它可以是一項挑戰有時也請確定您已正確設定 SMTP 伺服器、 通訊埠編號和等等。</span><span class="sxs-lookup"><span data-stu-id="a52cf-170">It can be a challenge sometimes to make sure you have the right settings for the SMTP server, port number, and so on.</span></span> <span data-ttu-id="a52cf-171">下列提供幾個秘訣：</span><span class="sxs-lookup"><span data-stu-id="a52cf-171">Here are a few tips:</span></span>
     > 
     > - <span data-ttu-id="a52cf-172">SMTP 伺服器名稱通常會像下面`smtp.provider.com`或`smtp.provider.net`。</span><span class="sxs-lookup"><span data-stu-id="a52cf-172">The SMTP server name is often something like `smtp.provider.com` or `smtp.provider.net`.</span></span> <span data-ttu-id="a52cf-173">不過，如果您是主機提供者發行您的網站，SMTP 伺服器名稱此時可能`localhost`。</span><span class="sxs-lookup"><span data-stu-id="a52cf-173">However, if you publish your site to a hosting provider, the SMTP server name at that point might be `localhost`.</span></span> <span data-ttu-id="a52cf-174">這是因為已發行的提供者伺服器上執行您的站台後，電子郵件伺服器可能會從您的應用程式的觀點來看本機。</span><span class="sxs-lookup"><span data-stu-id="a52cf-174">This is because after you've published and your site is running on the provider's server, the email server might be local from the perspective of your application.</span></span> <span data-ttu-id="a52cf-175">伺服器名稱中的這項變更可能表示您必須變更 SMTP 伺服器名稱做為您發佈程序的一部分。</span><span class="sxs-lookup"><span data-stu-id="a52cf-175">This change in server names might mean you have to change the SMTP server name as part of your publishing process.</span></span>
     > - <span data-ttu-id="a52cf-176">連接埠號碼通常是 25。</span><span class="sxs-lookup"><span data-stu-id="a52cf-176">The port number is usually 25.</span></span> <span data-ttu-id="a52cf-177">不過，某些提供者需要您使用的連接埠 587 或某些其他連接埠。</span><span class="sxs-lookup"><span data-stu-id="a52cf-177">However, some providers require you to use port 587 or some other port.</span></span>
     > - <span data-ttu-id="a52cf-178">請確定您使用正確的認證。</span><span class="sxs-lookup"><span data-stu-id="a52cf-178">Make sure that you use the right credentials.</span></span> <span data-ttu-id="a52cf-179">如果您已發行您的站台至主控提供者，使用提供者特別指出對於電子郵件的認證。</span><span class="sxs-lookup"><span data-stu-id="a52cf-179">If you've published your site to a hosting provider, use the credentials that the provider has specifically indicated are for email.</span></span> <span data-ttu-id="a52cf-180">這些可能與您用來發行的認證不同。</span><span class="sxs-lookup"><span data-stu-id="a52cf-180">These might be different from the credentials you use to publish.</span></span>
     > - <span data-ttu-id="a52cf-181">有時候您完全不需要認證。</span><span class="sxs-lookup"><span data-stu-id="a52cf-181">Sometimes you don't need credentials at all.</span></span> <span data-ttu-id="a52cf-182">如果您要傳送電子郵件使用您個人的 ISP，您的電子郵件提供者可能已經知道您的認證。</span><span class="sxs-lookup"><span data-stu-id="a52cf-182">If you're sending email using your personal ISP, your email provider might already know your credentials.</span></span> <span data-ttu-id="a52cf-183">發行之後，您可能需要使用比您本機電腦上進行測試時有不同的認證。</span><span class="sxs-lookup"><span data-stu-id="a52cf-183">After you publish, you might need to use different credentials than when you test on your local computer.</span></span>
     > - <span data-ttu-id="a52cf-184">如果您的電子郵件提供者會使用加密，您必須設定`WebMail.EnableSsl`至`true`。</span><span class="sxs-lookup"><span data-stu-id="a52cf-184">If your email provider uses encryption, you have to set `WebMail.EnableSsl` to `true`.</span></span>
4. <span data-ttu-id="a52cf-185">執行*EmailRequest.cshtml*瀏覽器中的。</span><span class="sxs-lookup"><span data-stu-id="a52cf-185">Run the *EmailRequest.cshtml* page in a browser.</span></span> <span data-ttu-id="a52cf-186">(請確定中選取頁面**檔案**才能執行這個工作區。)</span><span class="sxs-lookup"><span data-stu-id="a52cf-186">(Make sure the page is selected in the **Files** workspace before you run it.)</span></span>
5. <span data-ttu-id="a52cf-187">輸入您的名稱和問題描述，然後再按一下**送出** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="a52cf-187">Enter your name and a problem description, and then click the **Submit** button.</span></span> <span data-ttu-id="a52cf-188">正在重新導向至*ProcessRequest.cshtml*頁面上，這會確認您的訊息，並將您傳送電子郵件訊息。</span><span class="sxs-lookup"><span data-stu-id="a52cf-188">You're redirected to the *ProcessRequest.cshtml* page, which confirms your message and which sends you an email message.</span></span> 

    ![[影像]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a><span data-ttu-id="a52cf-190">使用電子郵件的檔案傳送</span><span class="sxs-lookup"><span data-stu-id="a52cf-190">Sending a File Using Email</span></span>

<span data-ttu-id="a52cf-191">您也可以傳送至電子郵件附加檔案。</span><span class="sxs-lookup"><span data-stu-id="a52cf-191">You can also send files that are attached to email messages.</span></span> <span data-ttu-id="a52cf-192">在此程序，您可以建立文字檔和兩個 HTML 網頁。</span><span class="sxs-lookup"><span data-stu-id="a52cf-192">In this procedure, you create a text file and two HTML pages.</span></span> <span data-ttu-id="a52cf-193">您將使用的文字檔案以電子郵件附件。</span><span class="sxs-lookup"><span data-stu-id="a52cf-193">You'll use the text file as an email attachment.</span></span>

1. <span data-ttu-id="a52cf-194">在網站上加入新的文字檔並將其命名*MyFile.txt*。</span><span class="sxs-lookup"><span data-stu-id="a52cf-194">In the website, add a new text file and name it *MyFile.txt*.</span></span>
2. <span data-ttu-id="a52cf-195">複製下列文字，並將它貼在檔案中：</span><span class="sxs-lookup"><span data-stu-id="a52cf-195">Copy the following text and paste it in the file:</span></span> 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. <span data-ttu-id="a52cf-196">建立名為的頁面*SendFile.cshtml*並加入下列標記：</span><span class="sxs-lookup"><span data-stu-id="a52cf-196">Create a page named *SendFile.cshtml* and add the following markup:</span></span> 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. <span data-ttu-id="a52cf-197">建立名為的頁面*ProcessFile.cshtml*並加入下列標記：</span><span class="sxs-lookup"><span data-stu-id="a52cf-197">Create a page named *ProcessFile.cshtml* and add the following markup:</span></span> 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. <span data-ttu-id="a52cf-198">修改下列電子郵件中的程式碼範例的相關的設定：</span><span class="sxs-lookup"><span data-stu-id="a52cf-198">Modify the following email related settings in the code from the example:</span></span>

    - <span data-ttu-id="a52cf-199">設定`your-SMTP-host`您具有存取權的 SMTP 伺服器的名稱。</span><span class="sxs-lookup"><span data-stu-id="a52cf-199">Set `your-SMTP-host` to the name of an SMTP server that you have access to.</span></span>
    - <span data-ttu-id="a52cf-200">設定`your-user-name-here`至您的 SMTP 伺服器帳戶的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="a52cf-200">Set `your-user-name-here` to the user name for your SMTP server account.</span></span>
    - <span data-ttu-id="a52cf-201">設定`your-email-address-here`您自己的電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="a52cf-201">Set `your-email-address-here` to your own email address.</span></span> <span data-ttu-id="a52cf-202">這是從訊息傳送的電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="a52cf-202">This is the email address that the message is sent from.</span></span>
    - <span data-ttu-id="a52cf-203">設定`your-account-password`您的 SMTP 伺服器帳戶的密碼。</span><span class="sxs-lookup"><span data-stu-id="a52cf-203">Set `your-account-password` to the password for your SMTP server account.</span></span>
    - <span data-ttu-id="a52cf-204">設定`target-email-address-here`您自己的電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="a52cf-204">Set `target-email-address-here` to your own email address.</span></span> <span data-ttu-id="a52cf-205">（如之前，您通常會傳送一封電子郵件給其他人，但為了測試，您可以將它傳送給您自己）。</span><span class="sxs-lookup"><span data-stu-id="a52cf-205">(As before, you'd normally send an email to someone else, but for testing, you can send it to yourself.)</span></span>
6. <span data-ttu-id="a52cf-206">執行*SendFile.cshtml*瀏覽器中的。</span><span class="sxs-lookup"><span data-stu-id="a52cf-206">Run the *SendFile.cshtml* page in a browser.</span></span>
7. <span data-ttu-id="a52cf-207">輸入您的名稱、 主旨和要附加的文字檔案的名稱 (*MyFile.txt*)。</span><span class="sxs-lookup"><span data-stu-id="a52cf-207">Enter your name, a subject line, and the name of the text file to attach (*MyFile.txt*).</span></span>
8. <span data-ttu-id="a52cf-208">按一下 `Submit` 按鈕。</span><span class="sxs-lookup"><span data-stu-id="a52cf-208">Click the `Submit` button.</span></span> <span data-ttu-id="a52cf-209">如往常一般，對話方塊重新導向至*ProcessFile.cshtml*頁面上，這會確認您的訊息和其傳送給您的電子郵件訊息與附加的檔案。</span><span class="sxs-lookup"><span data-stu-id="a52cf-209">As before, you're redirected to the *ProcessFile.cshtml* page, which confirms your message and which sends you an email message with the attached file.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="a52cf-210">其他資源</span><span class="sxs-lookup"><span data-stu-id="a52cf-210">Additional Resources</span></span>


- [<span data-ttu-id="a52cf-211">ASP.NET Web Pages (Razor) 疑難排解指南</span><span class="sxs-lookup"><span data-stu-id="a52cf-211">ASP.NET Web Pages (Razor) Troubleshooting Guide</span></span>](https://go.microsoft.com/fwlink/?LinkId=253001)
- [<span data-ttu-id="a52cf-212">簡易郵件傳輸通訊協定</span><span class="sxs-lookup"><span data-stu-id="a52cf-212">Simple Mail Transfer Protocol</span></span>](https://msdn.microsoft.com/library/aa480435.aspx)
- [<span data-ttu-id="a52cf-213">ASP.NET Web 網頁自訂全站台的行為</span><span class="sxs-lookup"><span data-stu-id="a52cf-213">Customizing Site-Wide Behavior for ASP.NET Web Pages</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
