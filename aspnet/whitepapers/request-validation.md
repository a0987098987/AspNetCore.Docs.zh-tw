---
uid: whitepapers/request-validation
title: 要求驗證-防止指令碼攻擊 |Microsoft Docs
author: rick-anderson
description: 本文件說明位置，根據預設，應用程式無法處理未編碼的 HTML 內容 submitt 的 ASP.NET 要求驗證的功能...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: ''
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 783fb1ae27d88f9c6d6d3484d26d3e206e7f2fba
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372925"
---
<a name="request-validation---preventing-script-attacks"></a><span data-ttu-id="36189-103">要求驗證-防止指令碼攻擊</span><span class="sxs-lookup"><span data-stu-id="36189-103">Request Validation - Preventing Script Attacks</span></span>
====================
> <span data-ttu-id="36189-104">本文件說明位置，根據預設，應用程式無法處理未編碼的 HTML 內容提交給伺服器的 ASP.NET 要求驗證的功能。</span><span class="sxs-lookup"><span data-stu-id="36189-104">This paper describes the request validation feature of ASP.NET where, by default, the application is prevented from processing unencoded HTML content submitted to the server.</span></span> <span data-ttu-id="36189-105">當應用程式的設計可安全地處理 HTML 資料時，可以停用此要求驗證功能。</span><span class="sxs-lookup"><span data-stu-id="36189-105">This request validation feature can be disabled when the application has been designed to safely process HTML data.</span></span>
> 
> <span data-ttu-id="36189-106">適用於 ASP.NET 1.1 和 ASP.NET 2.0。</span><span class="sxs-lookup"><span data-stu-id="36189-106">Applies to ASP.NET 1.1 and ASP.NET 2.0.</span></span>


<span data-ttu-id="36189-107">要求驗證，因為 1.1 版的 ASP.NET 功能可防止伺服器接受內容包含未編碼的 HTML。</span><span class="sxs-lookup"><span data-stu-id="36189-107">Request validation, a feature of ASP.NET since version 1.1, prevents the server from accepting content containing un-encoded HTML.</span></span> <span data-ttu-id="36189-108">這項功能被設計來協助防止某些指令碼資料隱碼攻擊，藉此讓用戶端指令碼或 HTML 可以不知情的情況下提交至伺服器、 儲存，以及之後要提供給其他使用者。</span><span class="sxs-lookup"><span data-stu-id="36189-108">This feature is designed to help prevent some script-injection attacks whereby client script code or HTML can be unknowingly submitted to a server, stored, and then presented to other users.</span></span> <span data-ttu-id="36189-109">我們仍強烈建議驗證所有輸入的資料，並加以適當時 HTML 編碼。</span><span class="sxs-lookup"><span data-stu-id="36189-109">We still strongly recommend that you validate all input data and HTML encode it when appropriate.</span></span>

<span data-ttu-id="36189-110">例如，您會建立要求使用者的電子郵件地址，然後存放區的電子郵件地址，在資料庫中的網頁。</span><span class="sxs-lookup"><span data-stu-id="36189-110">For example, you create a Web page that requests a user's email address and then stores that email address in a database.</span></span> <span data-ttu-id="36189-111">如果使用者輸入&lt;指令碼&gt;警示 ("hello from 指令碼 」)&lt;/script&gt;而不是有效的電子郵件地址時呈現該資料時，此指令碼可以執行如果內容未正確編碼。</span><span class="sxs-lookup"><span data-stu-id="36189-111">If the user enters &lt;SCRIPT&gt;alert("hello from script")&lt;/SCRIPT&gt; instead of a valid email address, when that data is presented, this script can be executed if the content was not properly encoded.</span></span> <span data-ttu-id="36189-112">ASP.NET 要求驗證功能可防止這種情況。</span><span class="sxs-lookup"><span data-stu-id="36189-112">The request validation feature of ASP.NET prevents this from happening.</span></span>

## <a name="why-this-feature-is-useful"></a><span data-ttu-id="36189-113">為何這項功能很實用</span><span class="sxs-lookup"><span data-stu-id="36189-113">Why this feature is useful</span></span>

<span data-ttu-id="36189-114">許多網站並不知道它們已開啟簡單的指令碼資料隱碼攻擊。</span><span class="sxs-lookup"><span data-stu-id="36189-114">Many sites are not aware that they are open to simple script injection attacks.</span></span> <span data-ttu-id="36189-115">無論這些攻擊的目的是以破壞網站所顯示的 HTML，或可能執行 將使用者重新導向到駭客的站台的用戶端指令碼，指令碼資料隱碼攻擊是 Web 開發人員必須應付的問題。</span><span class="sxs-lookup"><span data-stu-id="36189-115">Whether the purpose of these attacks is to deface the site by displaying HTML, or to potentially execute client script to redirect the user to a hacker's site, script injection attacks are a problem that Web developers must contend with.</span></span>

<span data-ttu-id="36189-116">指令碼資料隱碼攻擊是所有的 web 開發人員，需要考量，無論它們使用 ASP.NET、 ASP 或其他 web 開發技術。</span><span class="sxs-lookup"><span data-stu-id="36189-116">Script injection attacks are a concern of all web developers, whether they are using ASP.NET, ASP, or other web development technologies.</span></span>

<span data-ttu-id="36189-117">ASP.NET 要求驗證功能主動防止這些攻擊，方法不是允許未編碼的 HTML 內容，處理伺服器，除非開發人員決定要讓該內容。</span><span class="sxs-lookup"><span data-stu-id="36189-117">The ASP.NET request validation feature proactively prevents these attacks by not allowing unencoded HTML content to be processed by the server unless the developer decides to allow that content.</span></span>

## <a name="what-to-expect-error-page"></a><span data-ttu-id="36189-118">發生的情況： 錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="36189-118">What to expect: Error Page</span></span>

<span data-ttu-id="36189-119">以下螢幕擷取畫面顯示一些範例 ASP.NET 程式碼：</span><span class="sxs-lookup"><span data-stu-id="36189-119">The screen shot below shows some sample ASP.NET code:</span></span>

![](request-validation/_static/image1.png)

<span data-ttu-id="36189-120">執行此程式碼會產生簡單的網頁，可讓您在文字方塊中輸入一些文字中，按一下按鈕，並在標籤控制項中顯示的文字：</span><span class="sxs-lookup"><span data-stu-id="36189-120">Running this code results in a simple page that allows you to enter some text in the textbox, click the button, and display the text in the label control:</span></span>

![](request-validation/_static/image2.png)

<span data-ttu-id="36189-121">然而，如果是 JavaScript，例如`<script>alert("hello!")</script>`輸入並提交會得到例外狀況：</span><span class="sxs-lookup"><span data-stu-id="36189-121">However, were JavaScript, such as `<script>alert("hello!")</script>` to be entered and submitted we would get an exception:</span></span>

![](request-validation/_static/image3.png)

<span data-ttu-id="36189-122">錯誤訊息指出 '有潛在危險 Request.Form 值偵測到'，並提供更多詳細資料，在完全所產生，以及如何變更行為的描述。</span><span class="sxs-lookup"><span data-stu-id="36189-122">The error message states that a 'potentially dangerous Request.Form value was detected' and provides more details in the description as to exactly what occurred and how to change the behavior.</span></span> <span data-ttu-id="36189-123">例如: </span><span class="sxs-lookup"><span data-stu-id="36189-123">For example:</span></span>

<span data-ttu-id="36189-124">要求驗證程式偵測到有潛在危險的用戶端輸入的值，並在要求處理已中止。</span><span class="sxs-lookup"><span data-stu-id="36189-124">Request validation has detected a potentially dangerous client input value, and processing of the request has been aborted.</span></span> <span data-ttu-id="36189-125">此值可能表示有人嘗試入侵您的應用程式，例如跨網站指令碼攻擊的安全性。</span><span class="sxs-lookup"><span data-stu-id="36189-125">This value may indicate an attempt to compromise the security of your application, such as a cross-site scripting attack.</span></span> <span data-ttu-id="36189-126">您可以藉由設定停用要求驗證`validateRequest=false`Page 指示詞中，或在 [設定] 區段中。</span><span class="sxs-lookup"><span data-stu-id="36189-126">You can disable request validation by setting `validateRequest=false` in the Page directive or in the configuration section.</span></span> <span data-ttu-id="36189-127">不過，強烈建議，您的應用程式明確檢查所有輸入在此情況下。</span><span class="sxs-lookup"><span data-stu-id="36189-127">However, it is strongly recommended that your application explicitly check all inputs in this case.</span></span>

## <a name="disabling-request-validation-on-a-page"></a><span data-ttu-id="36189-128">在頁面上的停用要求驗證</span><span class="sxs-lookup"><span data-stu-id="36189-128">Disabling request validation on a page</span></span>

<span data-ttu-id="36189-129">若要停用要求驗證，您必須設定的頁面上`validateRequest`屬性的頁面指示詞`false`:</span><span class="sxs-lookup"><span data-stu-id="36189-129">To disable request validation on a page you must set the `validateRequest` attribute of the Page directive to `false`:</span></span>

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> <span data-ttu-id="36189-130">停用要求驗證時，可以提交內容至頁面;這是正確編碼或處理頁面開發人員，以確保該內容的責任。</span><span class="sxs-lookup"><span data-stu-id="36189-130">When request validation is disabled, content can be submitted to a page; it is the responsibility of the page developer to ensure that content is properly encoded or processed.</span></span>

## <a name="disabling-request-validation-for-your-application"></a><span data-ttu-id="36189-131">停用要求驗證您的應用程式</span><span class="sxs-lookup"><span data-stu-id="36189-131">Disabling request validation for your application</span></span>

<span data-ttu-id="36189-132">若要停用要求驗證您的應用程式，您必須修改或建立您的應用程式的 Web.config 檔案並將 validateRequest 屬性設定為`<pages />`一節`false`:</span><span class="sxs-lookup"><span data-stu-id="36189-132">To disable request validation for your application, you must modify or create a Web.config file for your application and set the validateRequest attribute of the `<pages />` section to `false`:</span></span>

[!code-xml[Main](request-validation/samples/sample2.xml)]

<span data-ttu-id="36189-133">如果您想要停用要求驗證您的伺服器上所有應用程式，您可以進行此修改 Machine.config 檔。</span><span class="sxs-lookup"><span data-stu-id="36189-133">If you wish to disable request validation for all applications on your server, you can make this modification to your Machine.config file.</span></span>

> [!CAUTION]
> <span data-ttu-id="36189-134">停用要求驗證時，內容可以提交給您的應用程式;這是正確編碼或處理應用程式開發人員，以確保該內容的責任。</span><span class="sxs-lookup"><span data-stu-id="36189-134">When request validation is disabled, content can be submitted to your application; it is the responsibility of the application developer to ensure that content is properly encoded or processed.</span></span>

<span data-ttu-id="36189-135">若要關閉要求驗證，會修改下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="36189-135">The code below is modified to turn off request validation:</span></span>

![](request-validation/_static/image4.png)

<span data-ttu-id="36189-136">現在，如果文字方塊中輸入下列 JavaScript`<script>alert("hello!")</script>`的結果會是：</span><span class="sxs-lookup"><span data-stu-id="36189-136">Now if the following JavaScript was entered into the textbox `<script>alert("hello!")</script>` the result would be:</span></span>

![](request-validation/_static/image5.png)

<span data-ttu-id="36189-137">若要避免發生這種情況，要求驗證時關閉，我們需要為 HTML 編碼內容。</span><span class="sxs-lookup"><span data-stu-id="36189-137">To prevent this from happening, with request validation turned off, we need to HTML encode the content.</span></span>

## <a name="how-to-html-encode-content"></a><span data-ttu-id="36189-138">如何為 HTML 編碼的內容</span><span class="sxs-lookup"><span data-stu-id="36189-138">How to HTML encode content</span></span>

<span data-ttu-id="36189-139">如果您已停用要求驗證，最好以 HTML 編碼的內容會儲存供日後使用。</span><span class="sxs-lookup"><span data-stu-id="36189-139">If you have disabled request validation, it is good practice to HTML-encode content that will be stored for future use.</span></span> <span data-ttu-id="36189-140">HTML 編碼方式將會自動取代任何 '&lt;'或'&gt;' （以及其他數個符號） 使用其對應的 HTML 編碼表示法。</span><span class="sxs-lookup"><span data-stu-id="36189-140">HTML encoding will automatically replace any ‘&lt;' or ‘&gt;' (together with several other symbols) with their corresponding HTML encoded representation.</span></span> <span data-ttu-id="36189-141">例如，'&lt;'取代'&amp;l t;' 和 '&gt;'取代'&amp;gt;'。</span><span class="sxs-lookup"><span data-stu-id="36189-141">For example, ‘&lt;' is replaced by ‘&amp;lt;' and ‘&gt;' is replaced by ‘&amp;gt;'.</span></span> <span data-ttu-id="36189-142">瀏覽器會使用這些特殊的程式碼顯示 '&lt;'或'&gt;' 瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="36189-142">Browsers use these special codes to display the ‘&lt;' or ‘&gt;' in the browser.</span></span>

<span data-ttu-id="36189-143">內容可以輕鬆地 HTML 編碼在伺服器中使用`Server.HtmlEncode(string)`API。</span><span class="sxs-lookup"><span data-stu-id="36189-143">Content can be easily HTML-encoded on the server using the `Server.HtmlEncode(string)` API.</span></span> <span data-ttu-id="36189-144">內容也可以輕鬆地 HTML 解碼，也就是還原為標準 HTML 使用`Server.HtmlDecode(string)`方法。</span><span class="sxs-lookup"><span data-stu-id="36189-144">Content can also be easily HTML-decoded, that is, reverted back to standard HTML using the `Server.HtmlDecode(string)` method.</span></span>

![](request-validation/_static/image6.png)

<span data-ttu-id="36189-145">產生：</span><span class="sxs-lookup"><span data-stu-id="36189-145">Resulting in:</span></span>

![](request-validation/_static/image7.png)
