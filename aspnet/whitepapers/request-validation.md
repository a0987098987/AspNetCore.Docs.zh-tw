---
uid: whitepapers/request-validation
title: 要求驗證-防止指令碼攻擊 |Microsoft Docs
author: rick-anderson
description: 本文件說明位置，根據預設，應用程式無法處理未編碼的 HTML 內容 submitt 的 ASP.NET 要求驗證的功能...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 087f30428602137e01f574825f3ebcd4db9285ff
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823607"
---
<a name="request-validation---preventing-script-attacks"></a>要求驗證-防止指令碼攻擊
====================
> 本文件說明位置，根據預設，應用程式無法處理未編碼的 HTML 內容提交給伺服器的 ASP.NET 要求驗證的功能。 當應用程式的設計可安全地處理 HTML 資料時，可以停用此要求驗證功能。
> 
> 適用於 ASP.NET 1.1 和 ASP.NET 2.0。


要求驗證，因為 1.1 版的 ASP.NET 功能可防止伺服器接受內容包含未編碼的 HTML。 這項功能被設計來協助防止某些指令碼資料隱碼攻擊，藉此讓用戶端指令碼或 HTML 可以不知情的情況下提交至伺服器、 儲存，以及之後要提供給其他使用者。 我們仍強烈建議驗證所有輸入的資料，並加以適當時 HTML 編碼。

例如，您會建立要求使用者的電子郵件地址，然後存放區的電子郵件地址，在資料庫中的網頁。 如果使用者輸入&lt;指令碼&gt;警示 ("hello from 指令碼 」)&lt;/script&gt;而不是有效的電子郵件地址時呈現該資料時，此指令碼可以執行如果內容未正確編碼。 ASP.NET 要求驗證功能可防止這種情況。

## <a name="why-this-feature-is-useful"></a>為何這項功能很實用

許多網站並不知道它們已開啟簡單的指令碼資料隱碼攻擊。 無論這些攻擊的目的是以破壞網站所顯示的 HTML，或可能執行 將使用者重新導向到駭客的站台的用戶端指令碼，指令碼資料隱碼攻擊是 Web 開發人員必須應付的問題。

指令碼資料隱碼攻擊是所有的 web 開發人員，需要考量，無論它們使用 ASP.NET、 ASP 或其他 web 開發技術。

ASP.NET 要求驗證功能主動防止這些攻擊，方法不是允許未編碼的 HTML 內容，處理伺服器，除非開發人員決定要讓該內容。

## <a name="what-to-expect-error-page"></a>發生的情況： 錯誤頁面

以下螢幕擷取畫面顯示一些範例 ASP.NET 程式碼：

![](request-validation/_static/image1.png)

執行此程式碼會產生簡單的網頁，可讓您在文字方塊中輸入一些文字中，按一下按鈕，並在標籤控制項中顯示的文字：

![](request-validation/_static/image2.png)

然而，如果是 JavaScript，例如`<script>alert("hello!")</script>`輸入並提交會得到例外狀況：

![](request-validation/_static/image3.png)

錯誤訊息指出 '有潛在危險 Request.Form 值偵測到'，並提供更多詳細資料，在完全所產生，以及如何變更行為的描述。 例如: 

要求驗證程式偵測到有潛在危險的用戶端輸入的值，並在要求處理已中止。 此值可能表示有人嘗試入侵您的應用程式，例如跨網站指令碼攻擊的安全性。 您可以藉由設定停用要求驗證`validateRequest=false`Page 指示詞中，或在 [設定] 區段中。 不過，強烈建議，您的應用程式明確檢查所有輸入在此情況下。

## <a name="disabling-request-validation-on-a-page"></a>在頁面上的停用要求驗證

若要停用要求驗證，您必須設定的頁面上`validateRequest`屬性的頁面指示詞`false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> 停用要求驗證時，可以提交內容至頁面;這是正確編碼或處理頁面開發人員，以確保該內容的責任。

## <a name="disabling-request-validation-for-your-application"></a>停用要求驗證您的應用程式

若要停用要求驗證您的應用程式，您必須修改或建立您的應用程式的 Web.config 檔案並將 validateRequest 屬性設定為`<pages />`一節`false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

如果您想要停用要求驗證您的伺服器上所有應用程式，您可以進行此修改 Machine.config 檔。

> [!CAUTION]
> 停用要求驗證時，內容可以提交給您的應用程式;這是正確編碼或處理應用程式開發人員，以確保該內容的責任。

若要關閉要求驗證，會修改下列程式碼：

![](request-validation/_static/image4.png)

現在，如果文字方塊中輸入下列 JavaScript`<script>alert("hello!")</script>`的結果會是：

![](request-validation/_static/image5.png)

若要避免發生這種情況，要求驗證時關閉，我們需要為 HTML 編碼內容。

## <a name="how-to-html-encode-content"></a>如何為 HTML 編碼的內容

如果您已停用要求驗證，最好以 HTML 編碼的內容會儲存供日後使用。 HTML 編碼方式將會自動取代任何 '&lt;'或'&gt;' （以及其他數個符號） 使用其對應的 HTML 編碼表示法。 例如，'&lt;'取代'&amp;l t;' 和 '&gt;'取代'&amp;gt;'。 瀏覽器會使用這些特殊的程式碼顯示 '&lt;'或'&gt;' 瀏覽器中。

內容可以輕鬆地 HTML 編碼在伺服器中使用`Server.HtmlEncode(string)`API。 內容也可以輕鬆地 HTML 解碼，也就是還原為標準 HTML 使用`Server.HtmlDecode(string)`方法。

![](request-validation/_static/image6.png)

產生：

![](request-validation/_static/image7.png)
