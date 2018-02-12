---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: "組件 8： 最後的頁面、 例外狀況處理，以及結論 |Microsoft 文件"
author: JoeStagner
description: "此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 8 部將加入連絡人的頁面，頁面上，以及例外狀況的相關..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: fce1a20f9d1093b6c60542d8a786ddf54fdc922c
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2018
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="17e6f-104">組件 8： 最後的頁面、 例外狀況處理，以及結束時</span><span class="sxs-lookup"><span data-stu-id="17e6f-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>
====================
<span data-ttu-id="17e6f-105">由[Joe stagner 以](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="17e6f-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="17e6f-106">Tailspin Spyworks 示範建立功能強大、 可調整的應用程式的.NET 平台是如何 hierarchy 簡單。</span><span class="sxs-lookup"><span data-stu-id="17e6f-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="17e6f-107">它會顯示如何使用 ASP.NET 4 中的強大的新功能，建置線上商店，包括購物、 簽出，以及管理關閉。</span><span class="sxs-lookup"><span data-stu-id="17e6f-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="17e6f-108">此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。</span><span class="sxs-lookup"><span data-stu-id="17e6f-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="17e6f-109">第 8 部將加入連絡人的頁面，頁面上和例外狀況處理的相關。</span><span class="sxs-lookup"><span data-stu-id="17e6f-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="17e6f-110">這是數列的結論。</span><span class="sxs-lookup"><span data-stu-id="17e6f-110">This is the conclusion of the series.</span></span>


## <a id="_Toc260221680"></a><span data-ttu-id="17e6f-111">請連絡 頁面 （從 ASP.NET 傳送電子郵件）</span><span class="sxs-lookup"><span data-stu-id="17e6f-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="17e6f-112">建立名為 ContactUs.aspx 的新頁面</span><span class="sxs-lookup"><span data-stu-id="17e6f-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="17e6f-113">使用設計工具，建立採用特殊附註，以包含 ToolkitScriptManager 和 AjaxdControlToolkit 編輯器控制項的格式如下。</span><span class="sxs-lookup"><span data-stu-id="17e6f-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxdControlToolkit.</span></span> <span data-ttu-id="17e6f-114">。</span><span class="sxs-lookup"><span data-stu-id="17e6f-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="17e6f-115">按兩下"Submit"按鈕，以產生 click 事件處理常式程式碼後置檔案，並實作一個方法來以電子郵件傳送的連絡資訊。</span><span class="sxs-lookup"><span data-stu-id="17e6f-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="17e6f-116">此程式碼需要您的 web.config 檔案包含指定要用來傳送郵件的 SMTP 伺服器的組態區段中的項目。</span><span class="sxs-lookup"><span data-stu-id="17e6f-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a><span data-ttu-id="17e6f-117">有關頁面</span><span class="sxs-lookup"><span data-stu-id="17e6f-117">About Page</span></span>

<span data-ttu-id="17e6f-118">建立名為 AboutUs.aspx 的頁面，並新增任何您喜歡的內容。</span><span class="sxs-lookup"><span data-stu-id="17e6f-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a><span data-ttu-id="17e6f-119">全域例外狀況處理常式</span><span class="sxs-lookup"><span data-stu-id="17e6f-119">Global Exception Handler</span></span>

<span data-ttu-id="17e6f-120">最後，我們有整個應用程式擲回例外狀況，並發生未預期的情況下，冷也有我們的 web 應用程式中造成未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="17e6f-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="17e6f-121">我們不想要顯示網站訪客處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="17e6f-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="17e6f-122">除了正在可怕的使用者體驗未處理例外狀況也可以是安全性問題。</span><span class="sxs-lookup"><span data-stu-id="17e6f-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="17e6f-123">若要解決此問題，我們會實作全域例外狀況處理常式。</span><span class="sxs-lookup"><span data-stu-id="17e6f-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="17e6f-124">若要這樣做，請開啟 Global.asax 檔案並請注意下列預先產生的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="17e6f-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="17e6f-125">加入程式碼來實作應用程式\_錯誤處理常式，如下所示。</span><span class="sxs-lookup"><span data-stu-id="17e6f-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="17e6f-126">然後加入至方案名為 Error.aspx 的頁面，並加入此標記程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="17e6f-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="17e6f-127">此時網頁\_載入錯誤訊息的事件處理常式擷取要求的物件。</span><span class="sxs-lookup"><span data-stu-id="17e6f-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a><span data-ttu-id="17e6f-128">結論</span><span class="sxs-lookup"><span data-stu-id="17e6f-128">Conclusion</span></span>

<span data-ttu-id="17e6f-129">我們已看到，ASP.NET WebForms 輕鬆建立複雜的網站與資料庫存取權，成員資格、 AJAX 等。</span><span class="sxs-lookup"><span data-stu-id="17e6f-129">We've seen that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="17e6f-130">很快。</span><span class="sxs-lookup"><span data-stu-id="17e6f-130">pretty quickly.</span></span>

<span data-ttu-id="17e6f-131">希望本教學課程已提供您要開始建置您自己的 ASP.NET WebForms 應用程式所需的工具 ！</span><span class="sxs-lookup"><span data-stu-id="17e6f-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="17e6f-132">上一步</span><span class="sxs-lookup"><span data-stu-id="17e6f-132">Previous</span></span>](tailspin-spyworks-part-7.md)
