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
<a name="part-8-final-pages-exception-handling-and-conclusion"></a>第 8 節： 最後的頁面、 例外狀況處理和結論
====================
藉由[Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks 示範建立功能強大、 可擴充的應用程式，適用於.NET 平台是如何富含簡單。 它會展示如何在 ASP.NET 4 中使用最棒的新功能，建置線上商店，包括購物、 簽出，以及系統管理。
> 
> 本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 8 節將連絡頁面，頁面上和例外狀況處理相關。 這是系列的結論。


## <a id="_Toc260221680"></a>  連絡人 頁面 （從 ASP.NET 傳送電子郵件）

建立名為 ContactUs.aspx 的新頁面

使用設計工具，建立下列特殊 core—1.1 加 ToolkitScriptManager AjaxdControlToolkit 編輯器控制項的表單。 。

![](tailspin-spyworks-part-8/_static/image1.jpg)

按兩下 [提交] 按鈕，以產生 click 事件處理常式程式碼後置檔案，並實作一個方法來以電子郵件傳送的連絡資訊。

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

這段程式碼會要求您的 web.config 檔案包含指定要用來傳送郵件的 SMTP 伺服器的組態區段中的項目。

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  關於頁面

建立名為 AboutUs.aspx 的頁面，並新增任何您喜歡的內容。

## <a id="_Toc260221682"></a>  全域例外狀況處理常式

最後，我們有整個應用程式擲回例外狀況，並發生未預期的情況下，冷也有我們的 web 應用程式中的原因未處理的例外狀況。

我們不想要顯示網站訪客未處理例外狀況。

![](tailspin-spyworks-part-8/_static/image2.jpg)

除了正在可怕的使用者體驗未處理例外狀況也可以是安全性問題。

若要解決此問題，我們將實作全域例外狀況處理常式。

若要這樣做，請開啟 Global.asax 檔案，並請注意下列的預先產生的事件處理常式。

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

加入程式碼來實作應用程式\_，如下所示的錯誤處理常式。

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

然後新增名為 Error.aspx，到解決方案的頁面，並新增此標記程式碼片段。

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

頁面中立即\_從要求的物件載入的錯誤訊息的事件處理常式擷取。

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  結論

我們已了解，ASP.NET WebForms 可輕鬆建立複雜的網站與資料庫存取權，成員資格，AJAX，等等。 很快。

希望本教學課程提供具有您要開始建置您自己的 ASP.NET WebForms 應用程式的工具 ！

> [!div class="step-by-step"]
> [上一步](tailspin-spyworks-part-7.md)
