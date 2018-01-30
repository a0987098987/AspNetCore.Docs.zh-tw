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
ms.openlocfilehash: 0b24fe2193d2c7a858667505bad9ed0b1d70a328
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
<a name="request-validation---preventing-script-attacks"></a>要求驗證-防止指令碼攻擊
====================
> 本白皮書說明的 ASP.NET，根據預設，應用程式將無法處理未編碼的 HTML 內容提交給伺服器的要求驗證功能。 當應用程式已設計為可安全地處理 HTML 資料時，可以停用此要求的驗證功能。
> 
> 適用於 ASP.NET 1.1 及 ASP.NET 2.0。


要求驗證，自 1.1 版的 ASP.NET 功能可防止伺服器接受內容包含未編碼的 HTML。 這項功能被設計來協助防止某些指令碼資料隱碼攻擊，讓用戶端指令碼或 HTML 可不知情的情況下提交到伺服器、 儲存，及接著呈現給其他使用者。 仍強烈建議您先驗證所有輸入的資料，以及 HTML 編碼，因此在適當的情況。

例如，您建立使用者的電子郵件地址，然後電子郵件地址在資料庫中的存放區要求的網頁。 如果使用者輸入&lt;指令碼&gt;警示 ("hello 從指令碼 」)&lt;/指令碼&gt;而不是有效的電子郵件地址，該資料時所出現，此指令碼可以執行如果內容未正確編碼。 ASP.NET 的要求驗證功能可防止這種情況。

## <a name="why-this-feature-is-useful"></a>此功能非常有用的原因

許多站台並不知道它們是簡單的指令碼資料隱碼攻擊。 無論這些攻擊的目的是藉由顯示 HTML、 破壞站台，或可能會執行將使用者重新導向到駭客的站台的用戶端指令碼，指令碼資料隱碼攻擊都是 Web 開發人員必須應付有問題。

指令碼資料隱碼攻擊是一項考量所有的 web 開發人員，不論它們使用 ASP.NET、 ASP 或其他 web 開發技術。

ASP.NET 要求的驗證功能主動避免這些攻擊藉由禁止未編碼的 HTML 內容來處理伺服器，除非開發人員決定，才能使用該內容。

## <a name="what-to-expect-error-page"></a>對預期會發生： 錯誤頁面

下列螢幕擷取畫面顯示一些範例 ASP.NET 程式碼：

![](request-validation/_static/image1.png)

執行此程式碼會產生簡單的頁面，可讓您在文字方塊中輸入一些文字中，按一下按鈕，並顯示標籤控制項中的文字：

![](request-validation/_static/image2.png)

然而，如果是 JavaScript 中，例如`<script>alert("hello!")</script>`輸入並提交我們會收到例外狀況：

![](request-validation/_static/image3.png)

錯誤訊息指出 '有潛在危險 Request.Form 偵測到值'，並提供更多詳細資料中完全所發生的情況以及如何變更行為的描述。 例如: 

要求驗證程式偵測到有潛在危險的用戶端的輸入的值，並要求的處理已中止。 這個值可能表示有人嘗試入侵您的應用程式，例如跨網站指令碼攻擊的安全性。 您可以藉由設定停用要求驗證`validateRequest=false`頁面指示詞或組態區段中。 不過，強烈建議，您的應用程式明確檢查所有輸入在此情況下。

## <a name="disabling-request-validation-on-a-page"></a>在頁面上停用要求驗證

若要停用要求驗證，在頁面上，您必須設定`validateRequest`頁面指示詞屬性`false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> 停用要求驗證時，內容可以送出至頁面。它是正確編碼或處理，以確保該內容的網頁開發人員的責任。

## <a name="disabling-request-validation-for-your-application"></a>停用要求驗證您的應用程式

若要停用要求驗證，您的應用程式，您必須修改或建立您的應用程式的 Web.config 檔案並將 validateRequest 屬性設定為`<pages />`區段`false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

如果您想要停用要求驗證您的伺服器上所有應用程式，您可以進行這項修改 Machine.config 檔案。

> [!CAUTION]
> 停用要求驗證時，內容可以提交給您的應用程式。它是正確編碼或處理以確保該內容的應用程式開發人員的責任。

若要關閉要求驗證下列程式碼行會有所修改：

![](request-validation/_static/image4.png)

現在，如果已在 textbox 中輸入下列 JavaScript`<script>alert("hello!")</script>`的結果會是：

![](request-validation/_static/image5.png)

若要避免此種情況，以關閉的要求驗證，我們需要 html 編碼的內容。

## <a name="how-to-html-encode-content"></a>如何為 HTML 編碼的內容

如果您已停用要求驗證，則會儲存供日後使用的 HTML 編碼的內容很好的作法。 HTML 編碼方式將會自動取代任何 '&lt;'或'&gt;' （搭配其他數個符號） 具有其對應的 HTML 編碼表示法。 例如，'&lt;'取代'&amp;lt;' 和 '&gt;'取代'&amp;gt;'。 瀏覽器會使用這些特殊的程式碼來顯示 '&lt;'或'&gt;' 在瀏覽器中。

內容可以輕鬆地 HTML 編碼則在伺服器使用`Server.HtmlEncode(string)`應用程式開發介面。 內容也可以輕鬆地進行 HTML 解碼，也就是說，還原為標準 HTML 使用`Server.HtmlDecode(string)`方法。

![](request-validation/_static/image6.png)

會產生：

![](request-validation/_static/image7.png)
