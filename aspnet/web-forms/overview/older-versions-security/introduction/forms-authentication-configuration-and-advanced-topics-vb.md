---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-vb
title: 表單驗證組態和進階的主題 (VB) |Microsoft Docs
author: rick-anderson
description: 在本教學課程，我們會檢查各種表單驗證設定，並了解如何透過表單項目來修改它們。 這會需要詳細...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: 829d2f56-5c48-445b-b826-3418a450c788
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-vb
msc.type: authoredcontent
ms.openlocfilehash: 493cb81271ea1c0439f7b499c5b48e659d3589b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390857"
---
<a name="forms-authentication-configuration-and-advanced-topics-vb"></a>表單驗證組態和進階的主題 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_VB.zip)或[下載 PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_vb.pdf)

> 在本教學課程，我們會檢查各種表單驗證設定，並了解如何透過表單項目來修改它們。 這將需要深入了解自訂表單驗證票證逾時值，使用自訂的 URL （例如 SignIn.aspx 而不是 Login.aspx) 和 cookieless 表單驗證票證的登入頁面。


## <a name="introduction"></a>簡介

在 [先前的教學課程](an-overview-of-forms-authentication-vb.md)我們探討了 ASP.NET 應用程式中，從 Web.config 中指定組態設定，來建立頁面，即可顯示的記錄檔不同實作表單驗證所需的步驟已驗證和匿名使用者的內容。 您應該記得，我們將網站設定為使用表單驗證，藉由設定目的 mode 屬性&lt;驗證&gt;form 項目。 &lt;驗證&gt;項目可選擇性地包含&lt;form&gt;子項目，可能會指定表單驗證設定的分類。

在本教學課程中我們將檢查各種表單驗證設定，並了解如何透過修改&lt;form&gt;項目。 這將需要深入了解自訂表單驗證票證逾時值，使用自訂的 URL （例如 SignIn.aspx 而不是 Login.aspx) 和 cookieless 表單驗證票證的登入頁面。 我們也會更仔細檢查表單驗證票證的結構，並請參閱 ASP.NET 會以確保票券的資料是從檢查和竄改的安全預防措施。 最後，我們將探討如何將額外的使用者資料儲存在表單驗證票證，以及如何建立自訂的主體物件透過此資料的模型。

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>步驟 1： 檢查&lt;form&gt;組態設定

在 ASP.NET 表單驗證系統提供的可自訂的應用程式的應用程式為基礎的組態設定。 這包括設定，例如： 存留期的表單驗證票證;哪些種類的保護套用至票證;在哪些條件 cookieless 驗證票證才能使用;登入 頁面中，路徑和其他資訊。 若要修改預設值，將[ &lt;form&gt;項目](https://msdn.microsoft.com/library/1d3t3c61.aspx)為子系[&lt;驗證&gt;項目](https://msdn.microsoft.com/library/532aee0e.aspx)，指定這些屬性您想要自訂為 XML 屬性的值，就像這樣：

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample1.xml)]

表 1 摘要說明您可以透過自訂的屬性&lt;form&gt;項目。 Web.config 是一個 XML 檔案，因為在左側的資料行中的屬性名稱會區分大小寫。


| <strong>屬性</strong> |                                                                                                                                                                                                                                     <strong>描述</strong>                                                                                                                                                                                                                                      |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         無 cookie         |                                                                                                                這個屬性會指定在哪些情況下驗證票證會儲存在 cookie 中與內嵌在 URL 中。 允許值為： UseCookies;UseUri;自動偵測;和 UseDeviceProfile （預設值）。 步驟 2 會檢查此詳細的設定。                                                                                                                |
|         defaultUrl         |                                                                                                                                                         指出使用者重新導向至未於 querystring 中指定的 RedirectUrl 值是否從登入頁面登入後的 URL。 預設值為 default.aspx。                                                                                                                                                         |
|           網域           | 當使用以 cookie 為基礎的驗證票證，則此設定會指定 cookie s 網域值。 預設值是空字串，這會導致瀏覽器使用從中它已發行 （例如 www.yourdomain.com) 的網域。 在此情況下，將會在 cookie<strong>不</strong>子網域，例如 admin.yourdomain.com 進行要求時傳送。 如果您想要傳遞至所有子網域，您需要自訂網域屬性將它設定為 yourdomain.com cookie。 |
|  enableCrossAppRedirects   |                                                                                                                                                                   布林值，指出是否在同一部伺服器上的其他 web 應用程式重新導向至 Url 時，是否需要記住已驗證的使用者。 預設為 false。                                                                                                                                                                   |
|          loginUrl          |                                                                                                                                                                                                                      登入頁面的 URL。 預設值為 login.aspx。                                                                                                                                                                                                                      |
|            name            |                                                                                                                                                                                                   當使用以 cookie 為基礎的驗證票證的 cookie 名稱。 預設值為。ASPXAUTH。                                                                                                                                                                                                   |
|            路徑            |                                                                             當使用以 cookie 為基礎的驗證票證，則此設定會指定 cookie s path 屬性。 Path 屬性可讓開發人員限制至特定的目錄階層的 cookie 的範圍。 預設值是，/，而這會通知傳送到任何加入網域所提出的要求的驗證票證 cookie 的瀏覽器。                                                                              |
|         保護         |                                                                                                                                            指示何種技術用來保護表單驗證票證。 允許的值為： 全部 （預設值）;加密;無。和驗證。 在步驟 3 中詳細討論這些設定。                                                                                                                                            |
|         requireSSL         |                                                                                                                                                                                布林值，指出是否需要 SSL 連線來傳送驗證 cookie。 預設值為 false。                                                                                                                                                                                |
|     slidingExpiration      |                                                                                                 布林值，指出是否每次重設驗證 cookie s 逾時使用者造訪網站的單一工作階段期間。 預設值為 true。 驗證票證逾時原則也會討論更詳細地指定票證逾時值區段 s。                                                                                                 |
|          逾時           |                                                                                                                               指定以分鐘為單位，驗證票證 cookie 到期之前的時間。 預設值為 30。 驗證票證逾時原則也會討論更詳細地指定票證逾時值區段 s。                                                                                                                               |

**表 1**: 摘要&lt;form&gt;項目的屬性

在 ASP.NET 2.0 和更新版本，預設值的表單驗證的值會是硬式編碼 FormsAuthenticationConfiguration 類別在.NET Framework 中。 上的 Web.config 檔案中的應用程式的應用程式為基礎，就必須套用的任何修改。 這不同於 ASP.NET 1.x 中，預設表單驗證值儲存在 machine.config 檔案中 （而因此無法修改透過編輯 machine.config）。 在 ASP.NET 中的主題 1.x，或許值得對提及的表單驗證系統設定的數字有不同的預設值，在 ASP.NET 2.0 中和未來展望比在 ASP.NET 1.x。 如果您從 ASP.NET 1.x 環境移轉您的應用程式，務必注意這些差異。 請參閱[ &lt;form&gt;項目技術的文件](https://msdn.microsoft.com/library/1d3t3c61.aspx)如需差異的清單。

> [!NOTE]
> 數個表單驗證設定，例如逾時、 網域及路徑中，指定產生的表單驗證票證 cookie 的詳細資料。 如需有關 cookie、 其運作方式，以及它們的各種屬性的詳細資訊，請參閱[本教學課程中的 Cookie](http://www.quirksmode.org/js/cookies.html)。


### <a name="specifying-the-tickets-timeout-value"></a>指定票證逾時值

表單驗證票證是表示身分識別的權杖。 使用以 cookie 為基礎的驗證票證，會保留在 cookie 形式這個語彙基元，並將其傳送至 web 伺服器上每個要求中。 基本上，宣告的權杖的擁有權，我*username*，我已經登入，並使用，因此在網頁瀏覽系統可記住使用者的身分識別。

表單驗證票證不只包含使用者的身分識別，但也包含可協助確保完整性與安全性權杖的資訊。 畢竟，我們不想惡意使用者可以建立的盜版的語彙基元，或修改某些 underhanded 的方式與合適的語彙基元。

是一個這類票證中包含的位元*到期*，這是票證已不再有效的時間與日期。 它可確保 FormsAuthenticationModule 會檢查驗證票證，每次會將票證到期日不具有尚未成功。 如果是，它會忽略該票證，並識別為匿名使用者。 此保護有助於防範重新執行攻擊。 沒有到期日，如果駭客已能夠實體存取其電腦，並瀏覽其 cookie 可能取得使用者的有效驗證票證-她實習-他們可以將要求傳送至具有此竊取的驗證票證的伺服器和取得項目。 到期不會防止這種情況下，它會限制這類攻擊會成功期間的視窗。

> [!NOTE]
> 步驟 3 的表單驗證系統用來保護驗證票證的詳細資料其他技巧。


當建立驗證票證，表單驗證系統會判斷其到期 consulting 逾時設定。 表 1，逾時設定預設值為 30 分鐘，所述，這表示表單驗證票證建立時的日期和時間在未來 30 分鐘內設定其到期。

過期狀態定義絕對未來表單驗證票證到期的時間。 但是，開發人員通常想要實作滑動過期，就會在每次使用者重新審視站台重設的其中一個。 此行為取決於 slidingExpiration 設定。 如果設為 true （預設值），的每當 FormsAuthenticationModule 驗證使用者時，便會更新票證到期日。 如果設為 false，到期不會更新每個要求，因而導致票證過期完全逾時分鐘數超過第一次票證被建立。

> [!NOTE]
> 儲存驗證票證在過期狀態是為絕對日期和時間值，例如 2008 年 8 月 2 日上午 11:34。 此外，日期和時間會相對於 web 伺服器的本機時間。 此設計決策可能有一些有趣的副作用周圍日光節約時間 (DST)，也就是在美國境內的時鐘移動繼續進行 （假設在觀察到日光節約時間的地區設定中裝載 web 伺服器） 的一小時時。 請考慮會發生什麼情況的 DST 開始的時間附近的 30 分鐘到期與 ASP.NET 網站 （此為上午 2:00）。 想像一下訪客登入站台在 2008 年 3 月 11 日上午 1:55。 這會產生表單驗證票證到期在 2008 年 3 月 11 日上午 2:25 （在未來 30 分鐘）。 不過，一旦 2:00 AM 彙周圍，時鐘會跳到上午 3:00 因為 DST。 當使用者載入新的頁面六分鐘內 （上午 3:01） 登入之後時，票證已過期，將使用者重新導向至登入頁面會註明 FormsAuthenticationModule。 這和其他驗證票證逾時奇觀，以及因應措施的深入討論，請選擇一份 Stefan Schackow *Professional ASP.NET 2.0 安全性、 成員資格和角色管理*(ISBN:978-0-7645-9698-8)。


圖 1 說明的工作流程，當 slidingExpiration 設為 false，而且逾時設定為 30。 請注意，在登入時所產生的驗證票證包含到期日，在後續要求將不會更新這個值。 如果 FormsAuthenticationModule 找到票證已過期，它會捨棄它，並要求視為匿名。


[![以圖形表示的表單驗證票證到期時 slidingExpiration 為 false](forms-authentication-configuration-and-advanced-topics-vb/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image1.png)

**圖 01**： 表單驗證票證到期時 slidingExpiration 圖形表示法為 false ([按一下以檢視完整大小的影像](forms-authentication-configuration-and-advanced-topics-vb/_static/image3.png))


圖 2 顯示工作流程，當 slidingExpiration 設為 true，且逾時設定為 30。 已驗證的要求收到 （具有未過期的票證） 時屆到期會更新為在未來分鐘的逾時數中。


[![以圖形表示的表單驗證票證的 slidingExpiration 時，則為 true](forms-authentication-configuration-and-advanced-topics-vb/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image4.png)

**圖 02**： 表單驗證票證的圖形表示法 slidingExpiration 時，則為 true ([按一下以檢視完整大小的影像](forms-authentication-configuration-and-advanced-topics-vb/_static/image6.png))


當使用以 cookie 為基礎的驗證票證 （預設值），在本文中會變成更令人困惑，因為 cookie 也可以指定自己 expiries。 Cookie 的到期 （或無區域性） 會指示瀏覽器時 cookie 應該予以終結。 如果 cookie 缺少有效期限，它會終結瀏覽器關閉時。 如果到期，則 cookie 到期的日期保留在使用者電腦上儲存但已超過指定時間的過期狀態。 Cookie 終結時，瀏覽器，它不會再傳送至 web 伺服器。 因此，損毀是 cookie 的類似於使用者登出網站記錄。

> [!NOTE]
> 當然，使用者可以主動移除儲存在他們的電腦上的所有 cookie。 在 Internet Explorer 7，您會前往 工具，選項，然後按一下 瀏覽歷程記錄區段中的 刪除 按鈕。 從該處按一下 [刪除 cookies] 按鈕。


表單驗證系統會建立工作階段或到期日為基礎的 cookie 傳遞給的值而定*persistCookie*參數。 回想一下，FormsAuthentication 類別的 GetAuthCookie、 SetAuthCookie 和 RedirectFromLoginPage 方法會採用兩個輸入參數：*使用者名稱*並*persistCookie*。 我們在先前的教學課程中建立的登入頁面包含記住我 核取方塊，判斷是否已建立永續性 cookie。 持續性 cookie 則到期日為基礎;非持續性 cookie，則工作階段為基礎。

已經討論過的逾時和 slidingExpiration 概念適用於相同這兩個工作階段和到期日為基礎的 cookie。 沒有在執行中只能有一個很小的差異： 搭配使用時到期日為基礎的 cookie slidingTimeout 設為 true，cookie 的到期日，才會更新時超過一半的指定時間已過。

因此，票證逾時之後一小時 （60 分鐘），使用滑動期限，讓我們更新我們的網站驗證票證逾時原則。 若要影響這項變更，更新 Web.config 檔案中，新增&lt;form&gt;項目&lt;驗證&gt;以下列標記的項目：

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>使用 Login.aspx 以外的登入頁面 URL

FormsAuthenticationModule 自動將未經授權的使用者重新導向至登入頁面，因為它需要知道登入頁面的 URL。 此 URL 中的 loginUrl 屬性所指定&lt;form&gt;項目，預設值為 login.aspx。 如果您要移植現有的網站上，您可能已經有不同的 URL，其中已設為書籤並由搜尋引擎編製索引的登入頁面。 而不是重新命名現有的登入頁面到 login.aspx 和中斷連結和使用者的書籤，您可以改為修改 loginUrl 屬性以指向您的登入頁面。

比方說，如果您登入頁面命名為 SignIn.aspx，而且已位於使用者的目錄中，您可以指向 loginUrl 組態設定，以 ~/Users/SignIn.aspx 就像這樣：

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample3.xml)]

我們目前應用程式已經具有名為 Login.aspx 的登入頁面，因為沒有指定自訂值中的不需要&lt;form&gt;項目。

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>步驟 2： 使用 Cookieless 表單驗證票證

根據預設表單驗證系統會決定是否要儲存其驗證票證的 cookie 集合中，或將其內嵌在造訪該網站的使用者代理程式為基礎的 URL。 所有主流的桌面瀏覽器讓 Internet Explorer、 Firefox、 Opera 和 Safari 支援 cookie，但並非所有的行動裝置。

使用表單驗證系統的 cookie 原則取決於在無 cookie 的設定&lt;form&gt;項目，可以指派四個值之一：

- UseCookies-指定將一律使用以 cookie 為基礎的驗證票證。
- UseUri-表示將永遠不會使用以 cookie 為基礎的驗證票證。
- 不會自動偵測-如果裝置設定檔不支援以 cookie 為基礎的驗證票證的 cookie;如果裝置設定檔支援 cookie，探查機制用來判斷是否已啟用 cookie。
- UseDeviceProfile-預設值;只有當裝置設定檔支援 cookie，請使用以 cookie 為基礎的驗證票證。 會不使用任何探查機制。

自動偵測及 UseDeviceProfile 設定依賴*裝置設定檔*中查明是否會使用以 cookie 為基礎或無 cookie 驗證票證。 ASP.NET 會維護各種裝置和其功能，例如它們是否支援 cookie，其所支援的 JavaScript 等等的什麼版本的資料庫。 每次裝置網頁從 web 伺服器要求不會沿著*使用者代理程式*識別的裝置類型的 HTTP 標頭。 ASP.NET 會自動符合提供的使用者代理程式字串與對應其資料庫中指定的設定檔。

> [!NOTE]
> 裝置功能的這個資料庫會儲存在多個 XML 檔案符合[瀏覽器定義檔結構描述](https://msdn.microsoft.com/library/ms228122.aspx)。 預設的裝置設定檔檔案位於 %windir%\microsoft.net\framework\v2.0.50727\config\browsers。 您也可以將自訂的檔案加入您的應用程式的應用程式\_瀏覽器資料夾。 如需詳細資訊，請參閱 < [How To： 偵測到 ASP.NET Web Pages 中的瀏覽器類型](https://msdn.microsoft.com/library/3yekbd5b.aspx)。


因為預設值是 UseDeviceProfile，當其設定檔會報告它不支援 cookie 的裝置瀏覽網站時將使用 cookieless 表單驗證票證。

### <a name="encoding-the-authentication-ticket-in-the-url"></a>編碼 URL 中的驗證票證

Cookie 是自然的中型至特定網站，這就是為什麼預設表單驗證設定會使用 cookie，如果正在瀏覽的裝置支援它們的每個要求中包含從瀏覽器的資訊。 若不支援 cookie，就必須採用替代方式來將從用戶端的驗證票證傳遞至伺服器。 使用無 cookie 的環境中常見的解決方法是編碼在 URL 中的 cookie 資料。

若要查看如何內嵌這類資訊，在 url 的最佳方式是要強制站台使用無 cookie 驗證票證。 這可藉由將無 cookie 的組態設定至 UseUri:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample4.xml)]

一旦您進行這項變更，請瀏覽的網站，透過瀏覽器。 當匿名使用者的身分瀏覽，Url 看起來如同之前一樣。 比方說，瀏覽 Default.aspx 頁面時我的瀏覽器網址列會顯示下列 URL:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

不過，在登入時，表單驗證票證將內嵌 URL。 比方說，瀏覽登入頁面，並以 Sam 身分登入之後, 我回到 Default.aspx 頁面上，但 URL 這次是：

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

表單驗證票證已內嵌在 url 中。 字串 (F (jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv rfezq0gKadKX0YPZCkA2) 代表十六進位編碼的驗證票證資訊，而且是相同的資料通常儲存在 cookie 中。

為了讓運作的無 cookie 驗證票證，系統必須編碼在頁面上的所有 Url，以包含驗證票券資料，否則當使用者按一下連結時的驗證票證將會遺失。 幸好這個內嵌的邏輯會自動執行。 為了示範這項功能，請開啟 Default.aspx 頁面，並新增超連結控制項，將其文字和 NavigateUrl 屬性分別設定為 [測試] 連結和 SomePage.aspx。 它並不重要，根本沒有頁面在名為 SomePage.aspx 專案。

將變更儲存至 Default.aspx，然後透過瀏覽器瀏覽它。 使表單驗證票證會內嵌在 URL 中登入網站。 接下來，從 Default.aspx，按一下 [測試連結] 連結。 發生什麼情況？ 如果存在名為 SomePage.aspx 沒有頁面，然後 404 錯誤發生，但不是重要這裡。 相反地，著重於在瀏覽器中的 [網址] 列。 請注意，在 URL 中包含表單驗證票證 ！

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

在連線 URL SomePage.aspx 已自動轉換成包含驗證 ticket-URL 我們不必撰寫可靠的程式碼 ！ 表單驗證票證將會自動內嵌在執行不能以 http:// 開頭的任何超連結的 URL 或 /。 如果超連結出現 Response.Redirect 呼叫、 超連結控制項，或錨定 HTML 項目並不重要 (亦即， &lt;href ="..."&gt;...&lt;/a&gt;)。 只要 URL 不是像 http://www.someserver.com/SomePage.aspx 或 /SomePage.aspx，驗證票證會內嵌為我們的表單。

> [!NOTE]
> Cookieless 表單驗證票證遵守相同的逾時原則，以 cookie 為基礎的驗證票證。 不過，無 cookie 驗證票證會更容易重新執行攻擊，因為直接在 URL 中內嵌的驗證票證。 假設使用者瀏覽網站、 登入，然後將 URL 貼至電子郵件給同事。 如果達到在到期之前的同事按一下該連結，就會記錄的使用者身分傳送電子郵件 ！


## <a name="step-3-securing-the-authentication-ticket"></a>步驟 3： 保護驗證票證

透過網路傳輸的表單驗證票證 cookie 中或直接在 URL 中內嵌。 身分識別資訊，除了驗證票證也會包含使用者資料 （如步驟 4 所示）。 因此，很重要的票證資料會加密和 （甚至更重要的是），從表單驗證系統可以保證票證器未遭竄改。

若要確保票證資料的隱私權，表單驗證系統可以加密的票證資料。 無法加密的票證資料會透過網路以純文字傳送潛在的敏感資訊。

若要確保票證的真實性，表單驗證系統必須*驗證*票證。 驗證是確保特定的資料未被修改，並完成透過 act *[訊息驗證碼 (MAC)](http://en.wikipedia.org/wiki/Message_authentication_code)*。 簡單的說，MAC 是資訊的一小部分，識別需要驗證 （在此案例，票證） 的資料。 如果 MAC 所表示的資料遭到修改，然後在 MAC 和資料會符合。 此外，根本很密集駭客同時修改資料，並產生自己的 MAC 與修改過的資料。

時建立 （或修改） 的票證，表單驗證系統會建立 MAC，並將它附加至的票證資料。 後續要求抵達時，表單驗證系統會比較驗證票券資料的真實性的 MAC 和票證資料。 [圖 3] 以圖形方式說明此工作流程。


[![票證的確有透過 MAC](forms-authentication-configuration-and-advanced-topics-vb/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image7.png)

**圖 03**: 票證的確有透過 MAC ([按一下以檢視完整大小的影像](forms-authentication-configuration-and-advanced-topics-vb/_static/image9.png))


哪些安全性量值會套用至驗證票證中的 [保護] 設定而定&lt;form&gt;項目。 保護設定可能會指派給下列三個值之一：

- 所有-票證同時加密和數位簽章 （預設值）。
- 加密-只加密已套用-沒有 MAC，會產生。
- 無-票證不是加密或數位簽章。
- 產生驗證-MAC，但票證資料會透過網路以純文字傳送。

Microsoft 強烈建議使用所有的設定。

### <a name="setting-the-validation-and-decryption-keys"></a>設定驗證和解密金鑰

加密和雜湊演算法來加密和驗證的驗證票證使用表單驗證系統會透過可自訂[ &lt;machineKey&gt;項目](https://msdn.microsoft.com/library/w8h3skw9.aspx)Web.config 中。表 2 外框輪廓&lt;machineKey&gt;項目的屬性和其可能的值。

| **屬性** | **描述** |
| --- | --- |
| 解密 | 表示用於加密的演算法。 這個屬性可以具有下列四個值之一:-自動-預設值;決定 decryptionKey 屬性的長度為基礎的演算法。 使用 AES-[進階加密標準 (AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard)演算法。 DES-使用[的資料加密標準 (DES)](http://en.wikipedia.org/wiki/Data_Encryption_Standard)這個演算法會被視為弱式運算資源，而且不應該使用。 -3DES-使用[Triple DES](http://en.wikipedia.org/wiki/Triple_DES)演算法，其運作方式是套用 DES 演算法三次。 |
| decryptionKey | 使用的加密演算法的祕密金鑰。 此值必須是適當的長度 （以解密中的值為基礎）、 自動產生，或附加，其中一個值為十六進位字串 IsolateApps。 新增 IsolateApps 指示 ASP.NET 為每個應用程式使用唯一的值。 預設值為 AutoGenerate，IsolateApps。 |
| 驗證 | 表示用於驗證的演算法。 這個屬性可以具有下列四個值之一:-AES-使用進階加密標準 (AES) 演算法。 -MD5-使用[訊息摘要 5 (MD5)](http://en.wikipedia.org/wiki/MD5)演算法。 -SHA1-使用[SHA1](http://en.wikipedia.org/wiki/Sha1)演算法 （預設值）。 -3DES-會使用 Triple DES 演算法。 |
| validationKey | 使用的驗證演算法的祕密金鑰。 此值必須是適當的長度 （根據驗證中的值）、 自動產生，或附加，其中一個值為十六進位字串 IsolateApps。 新增 IsolateApps 指示 ASP.NET 為每個應用程式使用唯一的值。 預設值為 AutoGenerate，IsolateApps。 |

**表 2**: &lt;machineKey&gt;元素屬性

這些加密和驗證選項和專業人員的討論的各種演算法優缺點已超出本教學課程的範圍。 如深入探討這些問題，包括哪些加密及驗證的演算法，若要使用，指引哪些金鑰的長度，若要使用，以及如何妥善地產生這些金鑰，請參閱*Professional ASP.NET 2.0 安全性、 成員資格和角色管理*.

根據預設，用來加密及驗證的金鑰會自動產生每個應用程式，而這些金鑰會儲存在本機安全性授權 (LSA)。 簡單來說，預設設定保證唯一索引鍵的網頁伺服器的 web 伺服器和應用程式的應用程式為基礎。 因此，此預設行為並不適用於下列兩個案例：

- **Web 伺服陣列**-在[web 伺服陣列](http://en.wikipedia.org/wiki/Web_farm)案例中，單一 web 應用程式裝載於多部網頁伺服器的延展性和備援的目的。 每個傳入要求分派至伺服器陣列中，這表示使用者的工作階段的存留期，不同的伺服器可能會用來處理其各種要求的伺服器。 因此，每一部伺服器，以便建立表單驗證票證，必須使用相同的加密和驗證金鑰、 加密及驗證上一部伺服器可以解密和驗證伺服器陣列中不同的伺服器上。
- **跨應用程式票證共用**-單一 web 伺服器可以裝載多個 ASP.NET 應用程式。 如果您需要這些不同的應用程式共用單一的表單驗證票證，請務必其加密和驗證金鑰匹配。

Web 伺服陣列設定，或共用相同的伺服器上的應用程式的驗證票證在工作時，您必須設定&lt;machineKey&gt;受影響的應用程式中的項目，讓其 decryptionKey 和validationKey 值配對。

雖然上述狀況都不會套用至我們的範例應用程式，但我們仍會指定明確的 Validationkey 與 validationKey 值，並定義要使用的演算法。 新增&lt;machineKey&gt;設為 Web.config 檔案：

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample5.xml)]

如需詳細資訊請參閱[How To： 在 ASP.NET 2.0 中的設定 MachineKey](https://msdn.microsoft.com/library/ms998288.aspx)。

> [!NOTE]
> Validationkey 與 validationKey 值取自[Steve Gibson](http://www.grc.com/stevegibson.htm)的[完美密碼網頁](https://www.grc.com/passwords.htm)，以在每個頁面造訪產生 64 隨機的十六進位字元。 若要可降低這些機碼，讓它們進入生產應用程式的可能性，便建議使用隨機產生的項目從 [完美的密碼] 頁面取代上述的金鑰。


## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>步驟 4： 將其他使用者資料儲存在票證

許多 web 應用程式顯示的相關資訊，或目前登入使用者的基礎頁面的顯示。 例如，網頁可能會顯示使用者的名稱和日期她上次登入在每個頁面的上方。 表單驗證票證會儲存使用者目前登入的使用者名稱，但需要的任何其他資訊時，頁面必須移至查閱不會儲存在驗證票證資訊的使用者存放區-通常是資料庫層。

我們可以加上一些程式碼，來儲存表單驗證票證額外的使用者資訊。 這類資料可透過表示[FormsAuthenticationTicket 類別](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)的[UserData 屬性](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx)。 這是將少量的使用者，經常需要的相關資訊的好地方。 在 [UserData] 屬性中包含的驗證票證 cookie，並像其他票證欄位，會加密和驗證所指定的值會根據表單驗證系統的組態。 根據預設，保留使用者資料會是空字串。

若要將使用者資料儲存在驗證票證，我們要撰寫的程式碼中擷取使用者特定資訊，並將它儲存在票證中的登入頁面。 因為 UserData 是字串類型屬性，在其中儲存的資料必須正確地序列化為字串。 比方說，假設我們的使用者存放區包含每位使用者的出生日期以及其雇主的名稱，而我們想要儲存這些兩個屬性值中的驗證票證。 我們無法藉由串連使用者的日期的生日的字串，以垂直線 (|)，後面接著雇主名稱，以將這些值序列化成字串。 使用者在 1974 年 8 月 15，適用於 Northwind Traders，就要將 [UserData] 屬性的字串： 1974年-08-15 |Northwind Traders。

每當我們需要存取儲存在票證中的資料，我們可以這麼做抓取目前的要求 FormsAuthenticationTicket，然後還原序列化 [UserData] 屬性。 生日及雇主名稱範例日期，如果我們會將 UserData 字串分割成兩個分隔字元 (|) 為基礎的子字串。


[![其他使用者資訊可以儲存在驗證票證](forms-authentication-configuration-and-advanced-topics-vb/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image10.png)

**圖 04**： 其他使用者資訊可以儲存在驗證票證 ([按一下以檢視完整大小的影像](forms-authentication-configuration-and-advanced-topics-vb/_static/image12.png))


### <a name="writing-information-to-userdata"></a>寫入資訊至 UserData

不幸的是，將使用者專屬資訊新增至表單驗證票證不是其中一個預期的那麼簡單。 FormsAuthenticationTicket 類別的 [UserData] 屬性是唯讀的並只能透過 FormsAuthenticationTicket 類別建構函式指定。 當指定 [UserData] 屬性的建構函式中，我們也需要提供票證的其他值： 使用者名稱、 發行日期、 到期日和等等。 我們在先前的教學課程中建立的登入頁面，這是所有我們由 FormsAuthentication 類別處理。 當加入 FormsAuthenticationTicket UserData，我們必須撰寫程式碼以將複寫的許多功能所提供的 FormsAuthentication 類別。

讓我們來探索必要的程式碼使用 UserData 藉由更新的 Login.aspx 頁面，來記錄使用者驗證票證相關的其他資訊。 假設我們的使用者存放區包含資訊適用於使用者的公司和職稱，和我們想要擷取這項資訊驗證票證。 更新的 Login.aspx 頁面 LoginButton Click 事件處理常式，使程式碼看起來如下所示：

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample6.vb)]

讓我們逐步執行一行此程式碼一次。 方法會定義四個字串陣列： 使用者、 密碼、 companyName 和 titleAtCompany。 這些陣列有使用者名稱、 密碼、 公司名稱和標題的使用者帳戶的系統中的其中有三個： Scott、 Jisun 和 Sam。 在實際的應用程式中，會從使用者存放區，不硬式編碼在頁面的原始程式碼中查詢這些值。

在上一個教學課程中，如果提供的認證不正確我們只要呼叫 FormsAuthentication.RedirectFromLoginPage UserName.Text (RememberMe.Checked），執行下列步驟：

1. 建立表單驗證票證
2. 寫入適當的存放區中的票證。 以 cookie 為基礎的驗證票證，它會使用的瀏覽器的 cookie 集合;cookieless 驗證票證的票券資料序列化成 URL
3. 重新導向到適當的網頁的使用者

上述程式碼中，會複寫這些步驟。 首先，我們會將最終會儲存在 [UserData] 屬性的字串會形成藉由結合公司名稱和標題，用來分隔兩個值使用縱線字元 (|)。

Dim userDataString As String = String.Concat(companyName(i), "|", titleAtCompany(i))

接下來，FormsAuthentication.GetAuthCookie 叫用方法時，這會建立驗證票證，加密和驗證根據組態設定，並將它放在 HttpCookie 物件。

變暗 authCookie 為 HttpCookie = FormsAuthentication.GetAuthCookie （UserName.Text、 RememberMe.Checked）

若要使用內嵌在 cookie 內 FormAuthenticationTicket，我們要呼叫 FormAuthentication 類別[解密方法](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx)，並傳入的 cookie 值。

Dim ticket As FormsAuthenticationTicket = FormsAuthentication.Decrypt(authCookie.Value)

然後，我們建立*新*FormsAuthenticationTicket 執行個體根據現有的 FormsAuthenticationTicket 值。 不過，這個新的票證包含使用者特定資訊 (userDataString)。

變暗 newTicket 為 FormsAuthenticationTicket = 新 FormsAuthenticationTicket(ticket.版本，票證。名稱，票證。IssueDate、 票證。票證的到期日。IsPersistent userDataString)

我們再加密 （和驗證） 新的 FormsAuthenticationTicket 執行個體，藉由呼叫[加密方法](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx)，並將此加密 （和已驗證） 的資料放入 authCookie 的上一步。

authCookie.Value = FormsAuthentication.Encrypt(newTicket)

最後，authCookie 加入至 Response.Cookies 集合並 GetRedirectUrl 方法會呼叫來決定適當的頁面，以傳送給使用者。

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample7.vb)]

這段程式碼需要因為 [UserData] 屬性是唯讀的而且 FormsAuthentication 類別不提供任何方法來指定其 GetAuthCookie、 SetAuthCookie 或 RedirectFromLoginPage 的方法中的 UserData 資訊。

> [!NOTE]
> 我們剛剛檢查過的程式碼會將使用者特定資訊儲存在 cookie 為基礎的驗證票證。 類別負責序列化表單驗證票證的 url 為內部.NET framework 物件。 長話短說，您無法儲存使用者資料在 cookieless 表單驗證票證。


### <a name="accessing-the-userdata-information"></a>存取保留使用者資料資訊

現在每位使用者的公司名稱和標題會儲存在表單驗證票證的 UserData 屬性在登入時。 這項資訊可以從任何頁面上的驗證票證存取，而不需要某趟車程支付的使用者存放區。 若要說明如何擷取這項資訊，從 [UserData] 屬性，讓我們將更新 Default.aspx，使其歡迎訊息包含不只使用者的名稱，但也它們適用於公司和職稱。

目前，Default.aspx 包含 Label 控制項，名為 WelcomeBackMessage AuthenticatedMessagePanel 面板。 此面板只會顯示已驗證的使用者。 更新 Default.aspx 的頁面中的程式碼\_Load 事件處理常式，使它看起來如下所示：

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample8.vb)]

如果 Request.IsAuthenticated 為 True，則 WelcomeBackMessage 的 Text 屬性第一次設定歡迎您再次啟用，為*username*。 然後，User.Identity 屬性會轉換成 FormsIdentity 物件，以便我們可以存取基礎 FormsAuthenticationTicket。 一旦我們擁有 FormsAuthenticationTicket，我們會還原 [UserData] 屬性序列化至的公司名稱和標題中。 這被透過分割以縱線字元字串。 然後 WelcomeBackMessage 標籤中顯示的公司名稱和標題。

圖 5 顯示作用中的此顯示畫面的螢幕擷取畫面。 以Scott的身份登錄，並顯示包含Scott的公司和頭銜的歡迎信息。


[![會顯示目前登入使用者的公司和標題](forms-authentication-configuration-and-advanced-topics-vb/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image13.png)

**圖 05**： 會顯示目前登入使用者的公司和標題 ([按一下以檢視完整大小的影像](forms-authentication-configuration-and-advanced-topics-vb/_static/image15.png))


> [!NOTE]
> 驗證票證的 UserData 屬性可做為快取的使用者存放區中。 像是任何快取，它必須修改基礎資料時，會更新。 比方說，如果網頁上的使用者可以更新其設定檔，在 [UserData] 屬性中快取的欄位必須重新整理以反映使用者所做的變更。


## <a name="step-5-using-a-custom-principal"></a>步驟 5： 使用自訂主體

在每個傳入要求上 FormsAuthenticationModule 會嘗試驗證使用者。 如果未過期的驗證票證，則 FormsAuthenticationModule 會將 HttpContext.User 屬性指派至新的 GenericPrincipal 物件。 這個 GenericPrincipal 物件具有 FormsIdentity，包含表單驗證票證的參考類型的身分識別。 GenericPrincipal 類別包含實作 IPrincipal 的類別所需的裸機最小功能-它只是具有 Identity 屬性和 IsInRole 方法。

主體的物件有兩個責任： 指出哪些使用者所屬的角色，並提供身分識別資訊。 這是透過 IPrincipal 介面的 IsInRole (*roleName*) 方法和身分識別屬性，分別。 GenericPrincipal 類別允許透過其建構函式; 指定之角色名稱的字串陣列其 IsInRole (*roleName*) 方法只會檢查是否要傳遞中*roleName*存在於字串陣列。 當 FormsAuthenticationModule 建立 GenericPrincipal 時，傳遞空字串陣列中 GenericPrincipal 的建構函式。 因此，任何對 IsInRole 的呼叫一律會傳回 False。

GenericPrincipal 類別符合大部分的表單型的驗證情況下，不使用角色的需求。 這種情況下，其中的預設角色處理是不足或當您需要自訂的 IIdentity 物件相關聯的使用者，您可以在驗證工作流程期間建立的自訂 IPrincipal 物件，並將它指派給 HttpContext.User 屬性。

> [!NOTE]
> 我們在未來將會看到教學課程中，當 ASP。NET 的角色架構會啟用它會建立類型的自訂主體物件[RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx)並覆寫的表單驗證建立 GenericPrincipal 物件。 這麼做是要自訂主體的 IsInRole 方法，以便與角色架構的 API。


因為我們有不關心自行角色尚未，我們必須在此時建立自訂主體的唯一理由是自訂的 IIdentity 物件的主體產生關聯。 步驟 4 中，我們討論過其他使用者資訊儲存在特定的驗證票證的 UserData 屬性中，使用者的公司名稱和其標題。 不過，UserData 資訊才可透過驗證票證存取，而且只，然後做為序列化的字串，這表示每當我們想要檢視使用者資訊儲存在票證中我們需要剖析 [UserData] 屬性。

我們可以建立一個類別來實作 IIdentity 和包含公司名稱和標題的屬性，以改善開發人員體驗。 如此一來，開發人員可以存取目前登入使用者的公司名稱和標題，直接透過 CompanyName 和 標題的屬性，而不需要知道如何剖析 UserData 屬性。

### <a name="creating-the-custom-identity-and-principal-classes"></a>建立自訂身分識別和主體類別

本教學課程中，讓我們來建立自訂的 principal 和 identity 物件 」 應用程式中\_程式碼資料夾。 新增應用程式開始\_程式碼到您的專案資料夾-以滑鼠右鍵按一下方案總管 中的專案名稱、 選擇加入 ASP.NET 資料夾，然後選擇應用程式\_程式碼。 應用程式\_程式碼資料夾是特殊的 ASP.NET 資料夾所在之網站的特定類別的檔案。

> [!NOTE]
> 應用程式\_管理您透過網站專案模型的專案時，應該只使用程式碼資料夾。 如果您使用[Web 應用程式專案模型](https://msdn.microsoft.com/asp.net/Aa336618.aspx)、 建立標準的資料夾，並將類別加入至該。 比方說，您可以加入名為類別的新資料夾，並將您的程式碼放至該處。


接下來，在應用程式中新增兩個新的類別檔案\_程式碼資料夾，一個具名的 CustomIdentity.vb，另一個名為 CustomPrincipal.vb。


[![專案中加入的 CustomIdentity 和 CustomPrincipal 類別](forms-authentication-configuration-and-advanced-topics-vb/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image16.png)

**圖 06**： 您的專案中加入的 CustomIdentity 和 CustomPrincipal 類別 ([按一下以檢視完整大小的影像](forms-authentication-configuration-and-advanced-topics-vb/_static/image18.png))


CustomIdentity 類別負責實作 IIdentity 介面，可定義 AuthenticationType、 IsAuthenticated 和名稱屬性。 除了這些必要的屬性，我們感興趣公開基礎表單驗證票證，以及使用者的公司名稱和標題的屬性。 輸入下列程式碼到 CustomIdentity 類別。

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample9.vb)]

請注意，此類別包含 FormsAuthenticationTicket 成員變數 (\_票證) 以及透過建構函式時必須提供此票證資訊。 此票證資料會在傳回的身分識別的名稱;其 UserData 屬性會傳回 CompanyName 和 標題屬性的值進行剖析。

接下來，建立 CustomPrincipal 類別。 因為我們不關心的角色在此時，CustomPrincipal 類別的建構函式會接受只有 CustomIdentity 物件;它的 IsInRole 方法一律會傳回 False。

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample10.vb)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>CustomPrincipal 物件指派給內送要求的安全性內容

此外，我們現在會有該類別可擴充預設 IIdentity 規格，以包含公司名稱和標題的屬性，以及使用自訂身分識別的自訂主體類別。 我們已準備好進入 ASP.NET 管線，並將我們自訂的主體物件指派給內送要求的安全性內容。

ASP.NET 管線會採用傳入的要求，並處理它透過幾個步驟。 在每個步驟中，會引發特定事件，讓開發人員善用 ASP.NET 管線，並修改其生命週期中的特定點的要求。 FormsAuthenticationModule，比方說，等候要引發的 ASP.NET [AuthenticateRequest 事件](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx)，此時它會檢查驗證 ticket 的連入要求。 如果找到已驗證票證，GenericPrincipal 物件建立，並指派給 HttpContext.User 屬性。

ASP.NET 管線引發 AuthenticateRequest 事件之後[PostAuthenticateRequest 事件](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx)，這是我們可以在其中取代所使用的執行個體 FormsAuthenticationModule 建立 GenericPrincipal 物件我們CustomPrincipal 物件。 [圖 7] 描述此工作流程。


[![GenericPrincipal 取代 CustomPrincipal PostAuthenticationRequest 事件中](forms-authentication-configuration-and-advanced-topics-vb/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image19.png)

**圖 07**: GenericPrincipal 取代 CustomPrincipal PostAuthenticationRequest 事件中 ([按一下以檢視完整大小的影像](forms-authentication-configuration-and-advanced-topics-vb/_static/image21.png))


為了回應 ASP.NET 管線事件中執行程式碼，我們可以在 Global.asax 中建立適當的事件處理常式，或建立自己的 HTTP 模組。 本教學課程中讓我們在 Global.asax 中建立的事件處理常式。 開始將 Global.asax 新增至您的網站。 以滑鼠右鍵按一下方案總管 中的專案名稱，並新增名為 Global.asax 的全域應用程式類別類型的項目。


[![Global.asax 檔案加入您的網站](forms-authentication-configuration-and-advanced-topics-vb/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-vb/_static/image22.png)

**圖 08**： 將 Global.asax 檔案新增至您的網站 ([按一下以檢視完整大小的影像](forms-authentication-configuration-and-advanced-topics-vb/_static/image24.png))


預設 Global.asax 範本包含事件處理常式，如有多個 ASP.NET 管線事件，包括開始時，結束並[錯誤事件](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx)，其他項目。 請放心地移除這些事件處理常式，因為我們不需要他們為此應用程式。 我們感興趣的事件是 PostAuthenticateRequest。 更新 Global.asax 檔案，所以其標記看起來如下所示：

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample11.aspx)]

應用程式\_OnPostAuthenticateRequest 方法執行的每當 ASP.NET 執行階段引發 PostAuthenticateRequest 事件，在每個連入的頁面要求上一次發生這種情況。 事件處理常式會先檢查以查看使用者是否經過驗證，並透過表單驗證的驗證。 如果是的話，新的 CustomIdentity 物件建立，並傳入其建構函式中的目前要求的驗證票證。 接下來，會 CustomPrincipal 物件建立，並在其建構函式中傳遞的剛建立 CustomIdentity 物件。 最後，目前要求的安全性內容被指派給新建立的 CustomPrincipal 物件中。

請注意最後一個步驟-建立 CustomPrincipal 物件關聯與要求的安全性內容-會將主體指派給兩個屬性： HttpContext.User 和 Thread.CurrentPrincipal。 這兩項指派是必要的因為在 ASP.NET 中處理安全性內容的方式。 .NET Framework 關聯每個執行中的執行緒; 中的安全性內容這項資訊可從透過的 IPrincipal 物件[執行緒物件](https://msdn.microsoft.com/library/system.threading.thread.aspx)的[CurrentPrincipal 屬性](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx)。 令人困惑的是 ASP.NET 有它自己的安全性內容資訊 (HttpContext.User)。

在某些情況下，判斷安全性內容中; 時，會檢查 Thread.CurrentPrincipal 屬性在其他情況下，會使用 HttpContext.User。 例如，在.NET 中的安全性功能可讓開發人員以宣告方式狀態的哪些使用者或角色可以具現化類別，或叫用特定方法 (請參閱[新增授權規則的商務和資料層使用PrincipalPermissionAttributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx))。 基本上，這些宣告式技巧會決定透過 Thread.CurrentPrincipal 屬性的安全性內容。

在其他情況下，會使用 HttpContext.User 屬性。 例如，先前的教學課程中我們使用這個屬性來顯示目前登入使用者的使用者名稱。 很明顯地，然後，它是命令式的 Thread.CurrentPrincipal 和 HttpContext.User 屬性中的安全性內容資訊匹配。

ASP.NET 執行階段會自動同步為我們的這些屬性值。 不過，這項同步處理會發生 AuthenticateRequest 事件之後，但*之前*PostAuthenticateRequest 事件。 因此，我們 PostAuthenticateRequest 事件中加入自訂的主體時要使用特定手動指派 Thread.CurrentPrincipal，否則 Thread.CurrentPrincipal，而 HttpContext.User 將會同步。請參閱[Context.User vs。Thread.CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/)如需此問題的更詳細討論。

### <a name="accessing-the-companyname-and-title-properties"></a>存取的公司名稱和標題屬性

每當要求抵達時，會分派至應用程式的 ASP.NET 引擎\_OnPostAuthenticateRequest Global.asax 中的事件處理常式就會引發。 如果 FormsAuthenticationModule 成功驗證要求，事件處理常式就會建立新的 CustomPrincipal 物件與 CustomIdentity 物件根據表單驗證票證。 就地此邏輯，以存取目前登入使用者的公司名稱和標題的相關資訊會變得簡單無比。

返回頁面\_在 Default.aspx 中，其中在步驟 4 中我們要撰寫程式碼來擷取表單驗證票證和剖析 [UserData] 屬性，以顯示使用者的公司名稱和標題的 Load 事件處理常式。 現在使用 CustomPrincipal 和 CustomIdentity 物件，使用中，則不需要剖析出票證的 UserData 屬性的值。 相反地，只要取得 CustomIdentity 物件的參考，並使用其公司名稱和標題的屬性：

[!code-vb[Main](forms-authentication-configuration-and-advanced-topics-vb/samples/sample12.vb)]

## <a name="summary"></a>總結

在本教學課程中，我們檢查如何自訂表單驗證系統的設定，透過 Web.config。我們討論過如何處理驗證票證的到期日，以及如何使用的加密和驗證的防護措施來防止檢驗和修改的票證。 最後，我們討論過使用驗證票證的 UserData 屬性儲存在票證本身的其他使用者資訊以及如何使用自訂的 principal 和 identity 物件，以更適合開發人員的方式公開此資訊。

本教學課程結束時，我們在 ASP.NET 中的表單驗證檢查。 下一個教學課程中，會啟動我們的旅程，到成員資格架構。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [剖析表單驗證](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [說明： 在 ASP.NET 2.0 中的表單驗證](https://msdn.microsoft.com/library/aa480476.aspx)
- [如何： 保護 ASP.NET 2.0 中的表單驗證](https://msdn.microsoft.com/library/ms998310.aspx)
- [Professional ASP.NET 2.0 安全性、 成員資格和角色管理](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html)(ISBN: 978-0-7645-9698-8)
- [設定登入控制項的安全性](https://msdn.microsoft.com/library/ms178346.aspx)
- [&lt;驗證&gt;項目](https://msdn.microsoft.com/library/532aee0e.aspx)
- [&lt;Form&gt;項目&lt;驗證&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)
- [&lt;MachineKey&gt;項目](https://msdn.microsoft.com/library/w8h3skw9.aspx)
- [了解表格驗證票證和 Cookie](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>在本教學課程中所包含的主題的影片訓練

- [如何變更表單驗證屬性](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [如何在 ASP.NET 應用程式的安裝和使用無 Cookie 驗證](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [ASP 表單登入重新配置](../../../videos/authentication/asp-forms-login-relocation.md)
- [表單登入自訂金鑰組態](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [將自訂資料新增至驗證方法](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [使用自訂的主體物件](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP 書籍的作者，他是 4GuysFromRolla.com 的創辦人，從事 Microsoft Web 技術工作自 1998 年。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是 *[Sams 教導您自己 ASP.NET 2.0 在 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 Scott 要聯絡[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Alicja Maziarz。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com)。

> [!div class="step-by-step"]
> [上一步](an-overview-of-forms-authentication-vb.md)
