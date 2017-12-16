---
uid: web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
title: "疑難排解 HTTP 405 錯誤，在發行後的 Web API 2 應用程式 |Microsoft 文件"
author: rmcmurray
description: "本教學課程說明如何針對 HTTP 405 錯誤進行疑難排解之後發行至實際執行 web 伺服器的 Web API 應用程式。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2014
ms.topic: article
ms.assetid: 07ec7d37-023f-43ea-b471-60b08ce338f7
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
msc.type: authoredcontent
ms.openlocfilehash: b87ae7420e1295030e90c30e97b1e331413ce263
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/15/2017
---
<a name="troubleshooting-http-405-errors-after-publishing-web-api-2-applications"></a>疑難排解 HTTP 405 錯誤，在發行後的 Web API 2 應用程式
====================
由[Robert McMurray](https://github.com/rmcmurray)

> 本教學課程說明如何針對 HTTP 405 錯誤進行疑難排解之後發行至實際執行 web 伺服器的 Web API 應用程式。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - [網際網路資訊服務 (IIS)](https://www.iis.net/) （版本 7 或更新版本）
> - [Web 應用程式開發介面](../../index.md)（版本 1 或 2）


Web API 應用程式通常會使用數個常見的 HTTP 動詞命令： GET、 POST、 PUT、 DELETE，有時也修補程式。 也就是說，開發人員可能會碰到情況下，這些動詞命令由其實際執行伺服器，將 Visual Studio 中或在程式開發伺服器上正常運作的 Web API 控制器的情況會導致在另一個 IIS 模組HTTP 405 錯誤部署到實際伺服器時。 幸運的是這個問題是輕鬆地解決，但解析保證的問題發生的原因說明。

## <a name="what-causes-http-405-errors"></a>什麼原因 HTTP 405 錯誤

了解如何疑難排解 HTTP 405 錯誤的第一步是了解 HTTP 405 錯誤實際上代表。 HTTP 是主要的控管文件[RFC 2616](http://www.ietf.org/rfc/rfc2616.txt)，其定義 HTTP 405 狀態程式碼，做為***不允許的方法***，並進一步說明這項狀態碼的情況下為其中&quot;方法指定在要求 URI 所識別的資源不允許要求行。&quot;換句話說，HTTP 指令動詞不允許 HTTP 用戶端要求的特定 url。

為簡短的檢閱，以下是幾個最常使用的 HTTP 方法如 RFC 2616、 RFC 4918 和 RFC 5789 中所定義：

| HTTP 方法 | 說明 |
| --- | --- |
| **取得** | 這個方法用來擷取資料的 URI，並從可能的最常用的 HTTP 方法。 |
| **標頭** | 這個方法是很像 GET 方法，不同之處在於它實際上不會在要求 URI 中擷取資料-只會擷取 HTTP 狀態。 |
| **POST** | 這個方法通常用來將新的資料傳送至 URI;POST 通常用於送出表單資料。 |
| **PUT** | 這個方法通常用來將原始資料傳送至的 URI。PUT 通常用於 JSON 或 XML 資料提交至 Web API 應用程式。 |
| **刪除** | 這個方法用來移除 URI 中的資料。 |
| **選項** | 這個方法通常用來擷取 uri 所支援的 HTTP 方法清單。 |
| **複製移動** | 這兩種方法可搭配 WebDAV，和其目的是一目了然。 |
| **MKCOL** | 這個方法用於 WebDAV，而且它用來建立指定之 uri 的集合 （例如目錄）。 |
| **PROPFIND PROPPATCH** | 這兩種方法可搭配 WebDAV，而且它們用來查詢或設定 URI 的屬性。 |
| **鎖定解除鎖定** | 這兩種方法可搭配 WebDAV，而且它們用來鎖定/解除鎖定撰寫時，要求 URI 所識別的資源。 |
| **修補程式** | 這個方法用來修改現有的 HTTP 資源。 |

當其中一種 HTTP 方法設定的伺服器上使用時，伺服器會回應的 HTTP 狀態和其他資料適用於要求。 (例如，GET 方法可能會收到 HTTP 200***確定***回應和 PUT 方法可能會收到 HTTP 201 ***Created***回應。)

如果未設定伺服器上使用的 HTTP 方法，伺服器會回應 HTTP 501***未實作***錯誤。

不過，當 HTTP 方法設定為使用在伺服器上，但它已停用指定的 URI，則伺服器將會以 HTTP 405 回應***不允許的方法***錯誤。

## <a name="example-http-405-error"></a>範例 HTTP 405 錯誤

下列範例 HTTP 要求和回應說明 HTTP 用戶端嘗試將值放在 web 伺服器上，Web API 應用程式和伺服器會傳回 HTTP 錯誤狀態的 PUT 方法不是允許的情況：


HTTP 要求：


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample1.cmd)]


HTTP 回應：


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample2.cmd)]


在此範例中，HTTP 用戶端傳送有效的 JSON 要求之 url 的 Web 應用程式開發介面上的應用程式的網頁伺服器，但伺服器傳回 HTTP 405 錯誤訊息指出 PUT 方法不允許的 URL。 相反地，如果在要求 URI 不符合 Web API 應用程式的路由，伺服器就會傳回 「 HTTP 404***找不到***錯誤。

## <a name="resolving-http-405-errors"></a>解決 HTTP 405 錯誤

有幾個原因可能不允許特定的 HTTP 指令動詞，但沒有前置造成此錯誤在 IIS 中的其中一個主要案例： 針對相同的動詞命令/方法所定義的多個處理常式，並且封鎖從預期的處理常式的其中一個處理常式處理要求。 說明，透過 IIS 會處理從上次根據訂單處理常式項目中 applicationHost.config 和 web.config 檔案，其中第一個相符的路徑、 動詞命令、 資源等，組合將用於處理要求的第一個處理常式。

下列範例是使用 PUT 方法將資料提交至 Web API 應用程式時，也傳回 HTTP 405 錯誤 IIS 伺服器的 applicationHost.config 檔案的摘錄。 在這段摘錄中，定義數個 HTTP 處理常式，和每個處理常式具有一組不同的 HTTP 方法設定的-在清單中的最後一個項目是靜態內容處理常式，也就是其他處理常式有 chanc 之後，可以使用的預設處理常式若要檢查要求 e:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample3.xml)]

在上述範例中，WebDAV 處理常式以及無副檔名 URL 處理常式 （使用於 Web 應用程式開發介面） 的 ASP.NET 會清楚地定義個別的 HTTP 方法清單。 請注意 ISAPI DLL 的處理常式已針對所有的 HTTP 方法中，雖然這項設定將不一定會產生錯誤。 不過，組態設定，例如 HTTP 405 錯誤疑難排解時，必須考量這項需求。

在上述範例中，ISAPI DLL 的處理常式不是這個問題;事實上，IIS 伺服器的 applicationHost.config 檔案中未定義問題-問題起因於 Visual Studio 中建立 Web API 應用程式時，web.config 檔案中所做的項目。 應用程式的 web.config 檔案中的下列摘錄顯示問題的位置：

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample4.xml)]

在這段摘錄中，ASP.NET 無副檔名 URL 處理常式已重新定義要包含其他 Web API 應用程式使用的 HTTP 方法。 不過，因為 WebDAV 處理常式定義一組類似的 HTTP 方法，就會發生衝突。 這種情況下，會定義 WebDAV 處理常式，並將其載入 iis，即使在包含 Web API 應用程式的網站已停用 WebDAV 中。 在處理 HTTP PUT 要求，IIS 會呼叫 WebDAV 模組，因為它定義為 PUT 動詞命令。 呼叫 WebDAV 模組時，它會檢查其組態，並會看到確認它已停用，因此它會傳回 HTTP 405***不允許的方法***類似的 WebDAV 要求的任何要求的錯誤。 若要解決此問題，您應該從 Web API 應用程式定義所在的網站的 HTTP 模組的清單中移除 WebDAV。 下列範例顯示，可能看起來：

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample5.xml)]

通常應用程式從開發環境中發行到生產環境中，這是因為處理常式/模組清單是您的開發和生產環境之間差異之後，就會發生這種情況。 例如，如果您使用 Visual Studio 2012 或 2013年開發 Web API 應用程式，IIS Express 8 是預設的 web 伺服器進行測試。 此開發 web 伺服器是隨附於伺服器產品的完整 IIS 功能的縮小版本，而且此開發 web 伺服器包含開發案例所新增的一些變更。 比方說，只有執行 IIS 的完整版本的生產環境 web 伺服器上但可能無法在實際使用時，通常有安裝 WebDAV 模組。 開發 IIS 的版本，(IIS Express)，安裝 WebDAV 模組，但 WebDAV 模組的項目故意標記為註解，因此除非您特別改變您的 IIS Express 設定，在 IIS Express 上都不會載入 WebDAV 模組若要新增至 IIS Express 安裝 WebDAV 功能的設定。 如此一來，web 應用程式可能會在您的開發電腦上正確運作，但是當您發行至生產環境 web 伺服器，Web API 應用程式時，可能會發生 HTTP 405 錯誤。

## <a name="summary"></a>總結

HTTP 405 錯誤會造成當要求 URL 的 web 伺服器不允許 HTTP 方法。 當特定的處理常式已定義特定動詞，但該處理常式會覆寫您預期會處理要求的處理常式時，通常會看到這種狀況。

如果您遇到的情況下，您在何處接收 HTTP 501 錯誤訊息，這表示的特定功能，不在伺服器上實作，這通常表示沒有任何處理常式 IIS 設定中所定義符合 HTTP 要求，其中可能表示的項目未正確安裝在您的系統上，或項目已修改您的 IIS 設定，確保沒有處理常式定義該支援特定的 HTTP 方法。 若要解決這個問題，您必須重新安裝任何應用程式，嘗試使用它有沒有對應的模組或處理常式定義 HTTP 方法。
