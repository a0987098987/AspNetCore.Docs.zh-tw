---
uid: web-api/overview/advanced/http-cookies
title: ASP.NET Web API 中的 HTTP Cookie |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 09/17/2012
ms.assetid: 243db2ec-8f67-4a5e-a382-4ddcec4b4164
msc.legacyurl: /web-api/overview/advanced/http-cookies
msc.type: authoredcontent
ms.openlocfilehash: 21ba186c11f39bbeedd1c320b98476ba13af27e2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819156"
---
<a name="http-cookies-in-aspnet-web-api"></a>ASP.NET Web API 中的 HTTP Cookie
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

本主題描述如何傳送和接收 Web API 中的 HTTP cookie。

## <a name="background-on-http-cookies"></a>HTTP Cookie 的背景

本節提供 cookie 在 HTTP 層級的實作方式的簡短概觀。 如需詳細資訊，請參閱[RFC 6265](http://tools.ietf.org/html/rfc6265)。

Cookie 是一種伺服器會傳送 HTTP 回應中的資料。 用戶端 （選擇性） 儲存在 cookie，然後將它傳回 subsequet 要求。 這可讓用戶端與伺服器共用狀態。 若要設定的 cookie，伺服器會納入回應 Set-cookie 標頭。 Cookie 的格式為名稱 / 值組，以選擇性屬性。 例如: 

[!code-powershell[Main](http-cookies/samples/sample1.ps1)]

以下是具有屬性範例：

[!code-powershell[Main](http-cookies/samples/sample2.ps1)]

若要返回伺服器的 cookie，用戶端會包含 Cookie 標頭之後的要求中。

[!code-console[Main](http-cookies/samples/sample3.cmd)]

![](http-cookies/_static/image1.png)

HTTP 回應可以包含多個 Set-cookie 標頭。

[!code-powershell[Main](http-cookies/samples/sample4.ps1)]

用戶端傳回使用單一的 Cookie 標頭的多個 cookie。

[!code-console[Main](http-cookies/samples/sample5.cmd)]

範圍和持續時間的 cookie 就會受 Set-cookie 標頭中的下列屬性：

- **網域**： 會告知用戶端網域應接收的 cookie。 例如，如果網域是"example.com 」，用戶端 cookie 傳回給為 example.com，則每一個子網域。 如果未指定，網域會是原始伺服器。
- **路徑**： 將 cookie 限制為網域內指定的路徑。 如果未指定，則會使用要求 URI 的路徑。
- **到期**： 設定 cookie 的到期日。 當計時器到期，用戶端就會刪除 cookie。
- **最大壽命**： 設定 cookie 的最長使用期限。 最長使用期限到達時，用戶端就會刪除 cookie。

如果兩個`Expires`並`Max-Age`設定，`Max-Age`會優先使用。 如果沒有設定，用戶端目前的工作階段結束時，就會刪除的 cookie。 （「 工作階段 」 的確切意義取決於使用者代理程式。）

不過，請注意，用戶端可能會忽略 cookie。 例如，使用者可能會停用 cookie，基於隱私原因。 用戶端可能會刪除 cookie 之前過期，或限制儲存的 cookie 數目。 基於隱私原因，用戶端通常會拒絕 「 協力廠商 」 cookie，該網域不符合原始伺服器。 簡單地說，伺服器不應依賴取回它所設定的 cookie。

## <a name="cookies-in-web-api"></a>Web API 中的 cookie

若要將 cookie 加入至 HTTP 回應中，建立**CookieHeaderValue**執行個體，表示 cookie。 然後呼叫**AddCookies**擴充方法，其定義於**System.Net.Http。HttpResponseHeadersExtensions**類別，以新增 cookie。

例如，下列程式碼加入控制器動作中的 cookie:

[!code-csharp[Main](http-cookies/samples/sample6.cs)]

請注意， **AddCookies**接受陣列**CookieHeaderValue**執行個體。

若要從用戶端要求中擷取的 cookie，呼叫**GetCookies**方法：

[!code-csharp[Main](http-cookies/samples/sample7.cs)]

A **CookieHeaderValue**包含的集合**CookieState**執行個體。 每個**CookieState**代表一個 cookie。 使用索引子方法來取得**CookieState**依名稱所示。

## <a name="structured-cookie-data"></a>結構化的 Cookie 資料

許多瀏覽器限制多少的 cookie，它們會儲存&#8212;總數，以及每個網域的數目。 因此，它可用來將結構化的資料放入單一 cookie，而不是設定多個 cookie。

> [!NOTE]
> RFC 6265 未定義 cookie 資料的結構。


使用**CookieHeaderValue**類別，您可以將傳遞的 cookie 資料的名稱 / 值組清單。 這些名稱 / 值組會編碼為 URL 編碼格式的 Set-cookie 標頭中的資料：

[!code-csharp[Main](http-cookies/samples/sample8.cs)]

先前的程式碼會產生下列 Set-cookie 標頭：

[!code-powershell[Main](http-cookies/samples/sample9.ps1)]

**CookieState**類別會提供一個索引子方法，讓讀取要求訊息中的 cookie 中的子的值：

[!code-csharp[Main](http-cookies/samples/sample10.cs)]

## <a name="example-set-and-retrieve-cookies-in-a-message-handler"></a>範例： 設定和擷取的訊息處理常式中的 Cookie

先前的範例，示範了如何使用來自 Web API 控制器內的 cookie。 另一個選項是使用[訊息處理常式](http-message-handlers.md)。 訊息處理常式會叫用稍早於控制站的管線。 訊息處理常式可以從要求讀取 cookie，才能在要求到達控制器，或控制站產生回應之後，將 cookie 加入至回應。

![](http-cookies/_static/image2.png)

下列程式碼顯示建立工作階段識別碼的訊息處理常式。 工作階段識別碼會儲存在 cookie 中。 處理常式會檢查工作階段 cookie 的要求。 如果要求不包含 cookie，處理常式就會產生新的工作階段識別碼。 在任一情況下，處理常式會儲存在工作階段識別碼**HttpRequestMessage.Properties**屬性包。 它也會加入至 HTTP 回應的工作階段 cookie。

此實作不會驗證從用戶端的工作階段識別碼實際上所簽發的伺服器。 不使用它做為一種驗證 ！ 此範例的重點是要顯示 HTTP cookie 管理。

[!code-csharp[Main](http-cookies/samples/sample11.cs)]

控制器可以取得工作階段識別碼，從**HttpRequestMessage.Properties**屬性包。

[!code-csharp[Main](http-cookies/samples/sample12.cs)]
