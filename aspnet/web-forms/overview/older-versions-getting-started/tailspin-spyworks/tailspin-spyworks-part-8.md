---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: 第 8 節： 最後的頁面、 例外狀況處理和結論 |Microsoft Docs
author: JoeStagner
description: 本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 8 節將連絡頁面，頁面上和例外狀況的相關...
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 4fb0147a5cd8621e5341e4995960adf2f00fffc0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804713"
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="db71b-104">第 8 節： 最後的頁面、 例外狀況處理和結論</span><span class="sxs-lookup"><span data-stu-id="db71b-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>
====================
<span data-ttu-id="db71b-105">藉由[Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="db71b-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="db71b-106">Tailspin Spyworks 示範建立功能強大、 可擴充的應用程式，適用於.NET 平台是如何富含簡單。</span><span class="sxs-lookup"><span data-stu-id="db71b-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="db71b-107">它會展示如何在 ASP.NET 4 中使用最棒的新功能，建置線上商店，包括購物、 簽出，以及系統管理。</span><span class="sxs-lookup"><span data-stu-id="db71b-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="db71b-108">本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="db71b-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="db71b-109">第 8 節將連絡頁面，頁面上和例外狀況處理相關。</span><span class="sxs-lookup"><span data-stu-id="db71b-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="db71b-110">這是系列的結論。</span><span class="sxs-lookup"><span data-stu-id="db71b-110">This is the conclusion of the series.</span></span>


## <a id="_Toc260221680"></a>  <span data-ttu-id="db71b-111">連絡人 頁面 （從 ASP.NET 傳送電子郵件）</span><span class="sxs-lookup"><span data-stu-id="db71b-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="db71b-112">建立名為 ContactUs.aspx 的新頁面</span><span class="sxs-lookup"><span data-stu-id="db71b-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="db71b-113">使用設計工具，建立下列特殊 core—1.1 加 ToolkitScriptManager AjaxdControlToolkit 編輯器控制項的表單。</span><span class="sxs-lookup"><span data-stu-id="db71b-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxdControlToolkit.</span></span> <span data-ttu-id="db71b-114">。</span><span class="sxs-lookup"><span data-stu-id="db71b-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="db71b-115">按兩下 [提交] 按鈕，以產生 click 事件處理常式程式碼後置檔案，並實作一個方法來以電子郵件傳送的連絡資訊。</span><span class="sxs-lookup"><span data-stu-id="db71b-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="db71b-116">這段程式碼會要求您的 web.config 檔案包含指定要用來傳送郵件的 SMTP 伺服器的組態區段中的項目。</span><span class="sxs-lookup"><span data-stu-id="db71b-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  <span data-ttu-id="db71b-117">關於頁面</span><span class="sxs-lookup"><span data-stu-id="db71b-117">About Page</span></span>

<span data-ttu-id="db71b-118">建立名為 AboutUs.aspx 的頁面，並新增任何您喜歡的內容。</span><span class="sxs-lookup"><span data-stu-id="db71b-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a>  <span data-ttu-id="db71b-119">全域例外狀況處理常式</span><span class="sxs-lookup"><span data-stu-id="db71b-119">Global Exception Handler</span></span>

<span data-ttu-id="db71b-120">最後，我們有整個應用程式擲回例外狀況，並發生未預期的情況下，冷也有我們的 web 應用程式中的原因未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="db71b-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="db71b-121">我們不想要顯示網站訪客未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="db71b-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="db71b-122">除了正在可怕的使用者體驗未處理例外狀況也可以是安全性問題。</span><span class="sxs-lookup"><span data-stu-id="db71b-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="db71b-123">若要解決此問題，我們將實作全域例外狀況處理常式。</span><span class="sxs-lookup"><span data-stu-id="db71b-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="db71b-124">若要這樣做，請開啟 Global.asax 檔案，並請注意下列的預先產生的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="db71b-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="db71b-125">加入程式碼來實作應用程式\_，如下所示的錯誤處理常式。</span><span class="sxs-lookup"><span data-stu-id="db71b-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="db71b-126">然後新增名為 Error.aspx，到解決方案的頁面，並新增此標記程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="db71b-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="db71b-127">頁面中立即\_從要求的物件載入的錯誤訊息的事件處理常式擷取。</span><span class="sxs-lookup"><span data-stu-id="db71b-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  <span data-ttu-id="db71b-128">結論</span><span class="sxs-lookup"><span data-stu-id="db71b-128">Conclusion</span></span>

<span data-ttu-id="db71b-129">我們已了解，ASP.NET WebForms 可輕鬆建立複雜的網站與資料庫存取權，成員資格，AJAX，等等。</span><span class="sxs-lookup"><span data-stu-id="db71b-129">We've seen that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="db71b-130">很快。</span><span class="sxs-lookup"><span data-stu-id="db71b-130">pretty quickly.</span></span>

<span data-ttu-id="db71b-131">希望本教學課程提供具有您要開始建置您自己的 ASP.NET WebForms 應用程式的工具 ！</span><span class="sxs-lookup"><span data-stu-id="db71b-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="db71b-132">上一步</span><span class="sxs-lookup"><span data-stu-id="db71b-132">Previous</span></span>](tailspin-spyworks-part-7.md)
