---
uid: whitepapers/request-validation
title: "要求驗證-防止指令碼攻擊 |Microsoft 文件"
author: rick-anderson
description: "本白皮書說明，根據預設，應用程式將無法處理未編碼的 HTML 內容 submitt ASP.NET 的要求驗證功能..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 61a96b75fdc29bdd1510ed689ee0356ef30e03fc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="request-validation---preventing-script-attacks"></a><span data-ttu-id="11a04-103">要求驗證-防止指令碼攻擊</span><span class="sxs-lookup"><span data-stu-id="11a04-103">Request Validation - Preventing Script Attacks</span></span>
====================
> <span data-ttu-id="11a04-104">本白皮書說明的 ASP.NET，根據預設，應用程式將無法處理未編碼的 HTML 內容提交給伺服器的要求驗證功能。</span><span class="sxs-lookup"><span data-stu-id="11a04-104">This paper describes the request validation feature of ASP.NET where, by default, the application is prevented from processing unencoded HTML content submitted to the server.</span></span> <span data-ttu-id="11a04-105">當應用程式已設計為可安全地處理 HTML 資料時，可以停用此要求的驗證功能。</span><span class="sxs-lookup"><span data-stu-id="11a04-105">This request validation feature can be disabled when the application has been designed to safely process HTML data.</span></span>
> 
> <span data-ttu-id="11a04-106">適用於 ASP.NET 1.1 及 ASP.NET 2.0。</span><span class="sxs-lookup"><span data-stu-id="11a04-106">Applies to ASP.NET 1.1 and ASP.NET 2.0.</span></span>


<span data-ttu-id="11a04-107">要求驗證，自 1.1 版的 ASP.NET 功能可防止伺服器接受內容包含未編碼的 HTML。</span><span class="sxs-lookup"><span data-stu-id="11a04-107">Request validation, a feature of ASP.NET since version 1.1, prevents the server from accepting content containing un-encoded HTML.</span></span> <span data-ttu-id="11a04-108">這項功能被設計來協助防止某些指令碼資料隱碼攻擊，讓用戶端指令碼或 HTML 可不知情的情況下提交到伺服器、 儲存，及接著呈現給其他使用者。</span><span class="sxs-lookup"><span data-stu-id="11a04-108">This feature is designed to help prevent some script-injection attacks whereby client script code or HTML can be unknowingly submitted to a server, stored, and then presented to other users.</span></span> <span data-ttu-id="11a04-109">仍強烈建議您先驗證所有輸入的資料，以及 HTML 編碼，因此在適當的情況。</span><span class="sxs-lookup"><span data-stu-id="11a04-109">We still strongly recommend that you validate all input data and HTML encode it when appropriate.</span></span>

<span data-ttu-id="11a04-110">例如，您可以建立網頁，要求使用者的電子郵件地址，然後將該電子郵件地址儲存在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="11a04-110">For example, you create a Web page that requests a user's e-mail address and then stores that e-mail address in a database.</span></span> <span data-ttu-id="11a04-111">如果使用者輸入&lt;指令碼&gt;警示 ("hello 從指令碼 」)&lt;/指令碼&gt;而不是有效的電子郵件地址，該資料時所出現，此指令碼可以執行如果內容未正確編碼。</span><span class="sxs-lookup"><span data-stu-id="11a04-111">If the user enters &lt;SCRIPT&gt;alert("hello from script")&lt;/SCRIPT&gt; instead of a valid e-mail address, when that data is presented, this script can be executed if the content was not properly encoded.</span></span> <span data-ttu-id="11a04-112">ASP.NET 的要求驗證功能可防止這種情況。</span><span class="sxs-lookup"><span data-stu-id="11a04-112">The request validation feature of ASP.NET prevents this from happening.</span></span>

## <a name="why-this-feature-is-useful"></a><span data-ttu-id="11a04-113">此功能非常有用的原因</span><span class="sxs-lookup"><span data-stu-id="11a04-113">Why this feature is useful</span></span>

<span data-ttu-id="11a04-114">許多站台並不知道它們是簡單的指令碼資料隱碼攻擊。</span><span class="sxs-lookup"><span data-stu-id="11a04-114">Many sites are not aware that they are open to simple script injection attacks.</span></span> <span data-ttu-id="11a04-115">無論這些攻擊的目的是藉由顯示 HTML、 破壞站台，或可能會執行將使用者重新導向到駭客的站台的用戶端指令碼，指令碼資料隱碼攻擊都是 Web 開發人員必須應付有問題。</span><span class="sxs-lookup"><span data-stu-id="11a04-115">Whether the purpose of these attacks is to deface the site by displaying HTML, or to potentially execute client script to redirect the user to a hacker's site, script injection attacks are a problem that Web developers must contend with.</span></span>

<span data-ttu-id="11a04-116">指令碼資料隱碼攻擊是一項考量所有的 web 開發人員，不論它們使用 ASP.NET、 ASP 或其他 web 開發技術。</span><span class="sxs-lookup"><span data-stu-id="11a04-116">Script injection attacks are a concern of all web developers, whether they are using ASP.NET, ASP, or other web development technologies.</span></span>

<span data-ttu-id="11a04-117">ASP.NET 要求的驗證功能主動避免這些攻擊藉由禁止未編碼的 HTML 內容來處理伺服器，除非開發人員決定，才能使用該內容。</span><span class="sxs-lookup"><span data-stu-id="11a04-117">The ASP.NET request validation feature proactively prevents these attacks by not allowing unencoded HTML content to be processed by the server unless the developer decides to allow that content.</span></span>

## <a name="what-to-expect-error-page"></a><span data-ttu-id="11a04-118">對預期會發生： 錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="11a04-118">What to expect: Error Page</span></span>

<span data-ttu-id="11a04-119">下列螢幕擷取畫面顯示一些範例 ASP.NET 程式碼：</span><span class="sxs-lookup"><span data-stu-id="11a04-119">The screen shot below shows some sample ASP.NET code:</span></span>

![](request-validation/_static/image1.png)

<span data-ttu-id="11a04-120">執行此程式碼會產生簡單的頁面，可讓您在文字方塊中輸入一些文字中，按一下按鈕，並顯示標籤控制項中的文字：</span><span class="sxs-lookup"><span data-stu-id="11a04-120">Running this code results in a simple page that allows you to enter some text in the textbox, click the button, and display the text in the label control:</span></span>

![](request-validation/_static/image2.png)

<span data-ttu-id="11a04-121">然而，如果是 JavaScript 中，例如`<script>alert("hello!")</script>`輸入並提交我們會收到例外狀況：</span><span class="sxs-lookup"><span data-stu-id="11a04-121">However, were JavaScript, such as `<script>alert("hello!")</script>` to be entered and submitted we would get an exception:</span></span>

![](request-validation/_static/image3.png)

<span data-ttu-id="11a04-122">錯誤訊息指出 '有潛在危險 Request.Form 偵測到值'，並提供更多詳細資料中完全所發生的情況以及如何變更行為的描述。</span><span class="sxs-lookup"><span data-stu-id="11a04-122">The error message states that a 'potentially dangerous Request.Form value was detected' and provides more details in the description as to exactly what occurred and how to change the behavior.</span></span> <span data-ttu-id="11a04-123">例如: </span><span class="sxs-lookup"><span data-stu-id="11a04-123">For example:</span></span>

<span data-ttu-id="11a04-124">要求驗證程式偵測到有潛在危險的用戶端的輸入的值，並要求的處理已中止。</span><span class="sxs-lookup"><span data-stu-id="11a04-124">Request validation has detected a potentially dangerous client input value, and processing of the request has been aborted.</span></span> <span data-ttu-id="11a04-125">這個值可能表示有人嘗試入侵您的應用程式，例如跨網站指令碼攻擊的安全性。</span><span class="sxs-lookup"><span data-stu-id="11a04-125">This value may indicate an attempt to compromise the security of your application, such as a cross-site scripting attack.</span></span> <span data-ttu-id="11a04-126">您可以藉由設定停用要求驗證`validateRequest=false`頁面指示詞或組態區段中。</span><span class="sxs-lookup"><span data-stu-id="11a04-126">You can disable request validation by setting `validateRequest=false` in the Page directive or in the configuration section.</span></span> <span data-ttu-id="11a04-127">不過，強烈建議，您的應用程式明確檢查所有輸入在此情況下。</span><span class="sxs-lookup"><span data-stu-id="11a04-127">However, it is strongly recommended that your application explicitly check all inputs in this case.</span></span>

## <a name="disabling-request-validation-on-a-page"></a><span data-ttu-id="11a04-128">在頁面上停用要求驗證</span><span class="sxs-lookup"><span data-stu-id="11a04-128">Disabling request validation on a page</span></span>

<span data-ttu-id="11a04-129">若要停用要求驗證，在頁面上，您必須設定`validateRequest`頁面指示詞屬性`false`:</span><span class="sxs-lookup"><span data-stu-id="11a04-129">To disable request validation on a page you must set the `validateRequest` attribute of the Page directive to `false`:</span></span>

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> <span data-ttu-id="11a04-130">停用要求驗證時，內容可以送出至頁面。它是正確編碼或處理，以確保該內容的網頁開發人員的責任。</span><span class="sxs-lookup"><span data-stu-id="11a04-130">When request validation is disabled, content can be submitted to a page; it is the responsibility of the page developer to ensure that content is properly encoded or processed.</span></span>

## <a name="disabling-request-validation-for-your-application"></a><span data-ttu-id="11a04-131">停用要求驗證您的應用程式</span><span class="sxs-lookup"><span data-stu-id="11a04-131">Disabling request validation for your application</span></span>

<span data-ttu-id="11a04-132">若要停用要求驗證，您的應用程式，您必須修改或建立您的應用程式的 Web.config 檔案並將 validateRequest 屬性設定為`<pages />`區段`false`:</span><span class="sxs-lookup"><span data-stu-id="11a04-132">To disable request validation for your application, you must modify or create a Web.config file for your application and set the validateRequest attribute of the `<pages />` section to `false`:</span></span>

[!code-xml[Main](request-validation/samples/sample2.xml)]

<span data-ttu-id="11a04-133">如果您想要停用要求驗證您的伺服器上所有應用程式，您可以進行這項修改 Machine.config 檔案。</span><span class="sxs-lookup"><span data-stu-id="11a04-133">If you wish to disable request validation for all applications on your server, you can make this modification to your Machine.config file.</span></span>

> [!CAUTION]
> <span data-ttu-id="11a04-134">停用要求驗證時，內容可以提交給您的應用程式。它是正確編碼或處理以確保該內容的應用程式開發人員的責任。</span><span class="sxs-lookup"><span data-stu-id="11a04-134">When request validation is disabled, content can be submitted to your application; it is the responsibility of the application developer to ensure that content is properly encoded or processed.</span></span>

<span data-ttu-id="11a04-135">若要關閉要求驗證下列程式碼行會有所修改：</span><span class="sxs-lookup"><span data-stu-id="11a04-135">The code below is modified to turn off request validation:</span></span>

![](request-validation/_static/image4.png)

<span data-ttu-id="11a04-136">現在，如果已在 textbox 中輸入下列 JavaScript`<script>alert("hello!")</script>`的結果會是：</span><span class="sxs-lookup"><span data-stu-id="11a04-136">Now if the following JavaScript was entered into the textbox `<script>alert("hello!")</script>` the result would be:</span></span>

![](request-validation/_static/image5.png)

<span data-ttu-id="11a04-137">若要避免此種情況，以關閉的要求驗證，我們需要 html 編碼的內容。</span><span class="sxs-lookup"><span data-stu-id="11a04-137">To prevent this from happening, with request validation turned off, we need to HTML encode the content.</span></span>

## <a name="how-to-html-encode-content"></a><span data-ttu-id="11a04-138">如何為 HTML 編碼的內容</span><span class="sxs-lookup"><span data-stu-id="11a04-138">How to HTML encode content</span></span>

<span data-ttu-id="11a04-139">如果您已停用要求驗證，則會儲存供日後使用的 HTML 編碼的內容很好的作法。</span><span class="sxs-lookup"><span data-stu-id="11a04-139">If you have disabled request validation, it is good practice to HTML-encode content that will be stored for future use.</span></span> <span data-ttu-id="11a04-140">HTML 編碼方式將會自動取代任何 '&lt;'或'&gt;' （搭配其他數個符號） 具有其對應的 HTML 編碼表示法。</span><span class="sxs-lookup"><span data-stu-id="11a04-140">HTML encoding will automatically replace any ‘&lt;' or ‘&gt;' (together with several other symbols) with their corresponding HTML encoded representation.</span></span> <span data-ttu-id="11a04-141">例如，'&lt;'取代'&amp;lt;' 和 '&gt;'取代'&amp;gt;'。</span><span class="sxs-lookup"><span data-stu-id="11a04-141">For example, ‘&lt;' is replaced by ‘&amp;lt;' and ‘&gt;' is replaced by ‘&amp;gt;'.</span></span> <span data-ttu-id="11a04-142">瀏覽器會使用這些特殊的程式碼來顯示 '&lt;'或'&gt;' 在瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="11a04-142">Browsers use these special codes to display the ‘&lt;' or ‘&gt;' in the browser.</span></span>

<span data-ttu-id="11a04-143">內容可以輕鬆地 HTML 編碼則在伺服器使用`Server.HtmlEncode(string)`應用程式開發介面。</span><span class="sxs-lookup"><span data-stu-id="11a04-143">Content can be easily HTML-encoded on the server using the `Server.HtmlEncode(string)` API.</span></span> <span data-ttu-id="11a04-144">內容也可以輕鬆地進行 HTML 解碼，也就是說，還原為標準 HTML 使用`Server.HtmlDecode(string)`方法。</span><span class="sxs-lookup"><span data-stu-id="11a04-144">Content can also be easily HTML-decoded, that is, reverted back to standard HTML using the `Server.HtmlDecode(string)` method.</span></span>

![](request-validation/_static/image6.png)

<span data-ttu-id="11a04-145">會產生：</span><span class="sxs-lookup"><span data-stu-id="11a04-145">Resulting in:</span></span>

![](request-validation/_static/image7.png)
