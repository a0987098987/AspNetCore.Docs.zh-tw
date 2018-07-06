---
uid: web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
title: 疑難排解 HTTP 405 錯誤，在發行後的 Web API 2 應用程式 |Microsoft Docs
author: rmcmurray
description: 本教學課程說明如何針對 HTTP 405 錯誤進行疑難排解之後發行的 Web API 應用程式，以在生產網頁伺服器。
ms.author: aspnetcontent
ms.date: 05/01/2014
ms.assetid: 07ec7d37-023f-43ea-b471-60b08ce338f7
msc.legacyurl: /web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
msc.type: authoredcontent
ms.openlocfilehash: 7dd7fd1fc6be9bc2f843c293222179a9774dff3c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827860"
---
<a name="troubleshooting-http-405-errors-after-publishing-web-api-2-applications"></a>疑難排解 HTTP 405 錯誤，在發行後的 Web API 2 應用程式
====================
藉由[Robert McMurray](https://github.com/rmcmurray)

> 本教學課程說明如何針對 HTTP 405 錯誤進行疑難排解之後發行的 Web API 應用程式，以在生產網頁伺服器。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - [Internet Information Services (IIS)](https://www.iis.net/) (7 或更新版本）
> - [Web API](../../index.md) （版本 1 或 2）


Web API 應用程式通常會使用數個常見的 HTTP 動詞命令： GET、 POST、 PUT、 DELETE 或修補程式。 也就是說，開發人員可能會遇到情況下，這些動詞命令由另一個適用於 Visual Studio 中，或在程式開發伺服器上的 Web API 控制器會傳回其中的情況下會導致其實際執行伺服器上的 IIS 模組HTTP 405 錯誤，它會部署到實際執行伺服器時。 幸好這個問題是輕鬆地解決，但解析需要時，發生問題的原因說明。

## <a name="what-causes-http-405-errors"></a>什麼原因 HTTP 405 錯誤

了解如何疑難排解 HTTP 405 錯誤的首要步驟是了解 HTTP 405 錯誤實際的意義。 HTTP 是主要的控管文件[RFC 2616](http://www.ietf.org/rfc/rfc2616.txt)，其定義為 HTTP 405 狀態碼***不允許的方法***，並進一步描述此狀態碼，為一種情況其中&quot;方法指定在要求 URI 所識別的資源不允許要求行。&quot;換句話說，HTTP 用戶端已要求的特定 URL 不允許的 HTTP 動詞命令。

為簡短回顧，這裡有幾個最常用的 HTTP 方法定義在 RFC 2616、 RFC 4918 和 RFC 5789:

| HTTP 方法 | 描述 |
| --- | --- |
| **取得** | 這個方法用來擷取資料的 URI，而且可能最常使用的 HTTP 方法。 |
| **標頭** | 這個方法，就像 GET 方法，不同之處在於它實際上不在要求 URI 中擷取的資料-它只會擷取的 HTTP 狀態。 |
| **貼文** | 這個方法通常用來將新的資料傳送至 URI;文章通常會用來提交表單資料中。 |
| **PUT** | 這個方法通常用來將原始資料傳送至的 URI;PUT 是通常用來提交對 Web API 應用程式的 JSON 或 XML 資料。 |
| **刪除** | 這個方法用來移除 URI 中的資料。 |
| **選項** | 這個方法通常用來擷取的 uri 所支援的 HTTP 方法清單。 |
| **複製移動** | WebDAV，搭配使用這兩種方法，其目的是一目了然。 |
| **MKCOL** | WebDAV，以使用這個方法，它用來建立指定之 uri 的集合 （例如目錄）。 |
| **PROPFIND PROPPATCH** | 這兩種方法可搭配 WebDAV，並用以查詢或設定 URI 的屬性。 |
| **鎖定解除鎖定** | 這兩種方法可搭配 WebDAV，它們用來鎖定/解除鎖定在撰寫時，要求 URI 所識別的資源。 |
| **修補程式** | 這個方法用來修改現有的 HTTP 資源。 |

當其中一種 HTTP 方法設定的伺服器上使用時，伺服器會回應 HTTP 狀態和其他適用於要求的資料。 (例如，GET 方法可能會收到 HTTP 200 ***[確定]*** 回應和 PUT 方法可能會收到 HTTP 201***建立***回應。)

如果使用伺服器上未設定的 HTTP 方法，伺服器會回應 HTTP 501***未實作***時發生錯誤。

不過，當 HTTP 方法設定為使用在伺服器上，但它已停用指定的 URI 時，伺服器會回應 HTTP 405***不允許的方法***時發生錯誤。

## <a name="example-http-405-error"></a>範例 HTTP 405 錯誤

下列範例 HTTP 要求和回應會說明 HTTP 用戶端嘗試將值放在 web 伺服器上，Web API 應用程式，而伺服器會傳回 HTTP 錯誤狀態的 PUT 方法不是允許的情況：


HTTP 要求：


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample1.cmd)]


HTTP 回應：


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample2.cmd)]


在此範例中，HTTP 用戶端傳送有效的 JSON 要求至 web 伺服器上的 Web API 應用程式的 URL，但伺服器傳回 HTTP 405 錯誤訊息表示 PUT 方法不允許的 URL。 相反地，如果在要求 URI 不符合 Web API 應用程式的路由，伺服器會傳回 HTTP 404***找不到***時發生錯誤。

## <a name="resolving-http-405-errors"></a>解決 HTTP 405 錯誤

有幾個原因，原因可能不允許特定的 HTTP 指令動詞，但沒有會造成此錯誤在 IIS 中的其中一個主要案例： 針對相同的動詞命令/方法所定義的多個處理常式和其中一個處理常式會封鎖從預期的處理常式處理要求。 藉由說明，IIS 會處理從上次依據的順序處理常式的項目中 applicationHost.config 與 web.config 檔案，其中第一個相符的路徑、 動詞命令、 資源等組合將用於處理要求的第一個處理常式。

下列範例是已傳回 HTTP 405 錯誤，將資料提交至 Web API 應用程式中使用 PUT 方法時的 IIS 伺服器的 applicationHost.config 檔案的摘錄。 在這段摘錄中，定義數個 HTTP 處理常式，和每個處理常式有一組不同的 HTTP 方法設定的-在清單中的最後一個項目是靜態內容處理常式，也就是另一個處理常式有 chanc 之後，可以使用的預設處理常式若要檢查要求的 e:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample3.xml)]

在上述範例中，WebDAV 處理常式並無副檔名 URL 處理常式 （這用來 Web API） 的 asp.net 會清楚地定義不同的 HTTP 方法的清單。 請注意 ISAPI DLL 的處理常式已針對所有的 HTTP 方法，雖然這項設定將不一定會造成錯誤。 不過，組態設定，例如針對 HTTP 405 錯誤進行疑難排解時，需要考量這項需求。

在上述範例中，ISAPI DLL 的處理常式不是問題;事實上，IIS 伺服器的 applicationHost.config 檔案中未定義的問題-問題起因於 Visual Studio 中建立 Web API 應用程式時已在 web.config 檔案中的項目。 應用程式的 web.config 檔案中的下列摘錄顯示問題的位置：

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample4.xml)]

在這段摘錄中，ASP.NET 無副檔名 URL 處理常式已重新定義要包含其他可搭配 Web API 應用程式的 HTTP 方法。 不過，因為 WebDAV 處理常式已定義一組類似的 HTTP 方法，就會發生衝突。 這種情況下，會定義 WebDAV 處理常式，並將其載入 iis，即使已停用包含 Web API 應用程式之網站的 WebDAV 中。 在處理 HTTP PUT 要求，IIS 會呼叫 WebDAV 模組，因為它定義 PUT 動詞命令。 呼叫 WebDAV 模組時，它會檢查其設定，並會看到，它已停用，因此它會傳回 HTTP 405***不允許的方法***類似的 WebDAV 要求的任何要求的錯誤。 若要解決此問題，您應該移除 WebDAV，從您的 Web API 應用程式定義所在之網站的 HTTP 模組的清單。 下列範例顯示，可能會看起來像：

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample5.xml)]

此案例中通常發生後的應用程式從開發環境發行至生產環境中，這是因為處理常式/模組的清單是您的開發和生產環境之間不同。 比方說，如果您使用 Visual Studio 2012 或 2013年開發 Web API 應用程式，IIS Express 8 是預設的 web 伺服器進行測試。 此程式開發 web 伺服器是完整的 IIS 功能，隨附於伺服器產品，規模版本，此開發 web 伺服器會包含已新增的開發案例的一些變更。 比方說，只有在執行完整版的 IIS 中，在生產網頁伺服器上但也不可能在實際使用，通常有安裝 WebDAV 模組。 開發版本的 IIS (IIS Express)、 WebDAV 會將模組安裝，但 WebDAV 模組的項目故意標記為註解，因此除非您特別改變您的 IIS Express 設定，在 IIS Express 上永遠不會載入 WebDAV 模組WebDAV 功能加入您的 IIS Express 安裝的設定。 如此一來，您的 web 應用程式可能在您的開發電腦上正常運作，但當您發行至實際執行 web 伺服器，Web API 應用程式時，您可能會發生 HTTP 405 錯誤。

## <a name="summary"></a>總結

HTTP 405 錯誤會造成當所要求 url 的 web 伺服器不允許 HTTP 方法。 當特定的處理常式已定義特定動詞命令，但該處理常式會覆寫您預期要處理要求的處理常式時，便常常會看到這種情況。

如果您遇到的情況下，您在何處接收 HTTP 501 錯誤訊息，這表示特定的功能，尚未在伺服器上實作，這通常表示會有任何處理常式在您的 IIS 設定中定義用來比對 HTTP 要求，其中可能表示的項目未正確安裝在您的系統上，或項目已修改您的 IIS 設定，因此，有沒有處理常式定義該支援特定的 HTTP 方法。 若要解決這個問題，您必須重新安裝任何應用程式，嘗試使用 HTTP 方法，它有沒有對應的模組或處理常式定義。
