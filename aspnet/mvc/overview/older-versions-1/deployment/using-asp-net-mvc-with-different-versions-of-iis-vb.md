---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
title: 使用 ASP.NET MVC 與不同版本的 IIS (VB) |Microsoft Docs
author: microsoft
description: 在本教學課程中，您將了解如何使用不同版本的 Internet Information Services 中的 ASP.NET MVC 和 URL 路由。 您了解不同的策略...
ms.author: aspnetcontent
ms.date: 08/19/2008
ms.assetid: 1c1283b2-6956-4937-b568-d30de432ce23
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
msc.type: authoredcontent
ms.openlocfilehash: 4c0dc1ac08cfe06ad7ea35a6e6552ab1174ff989
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818200"
---
<a name="using-aspnet-mvc-with-different-versions-of-iis-vb"></a>使用 ASP.NET MVC 與不同版本的 IIS (VB)
====================
by [Microsoft](https://github.com/microsoft)

> 在本教學課程中，您將了解如何使用不同版本的 Internet Information Services 中的 ASP.NET MVC 和 URL 路由。 您將了解不同的策略來使用 IIS 7.0 （傳統模式）、 IIS 6.0 和舊版 IIS 的 ASP.NET MVC。


ASP.NET MVC 架構而定 ASP.NET 路由將瀏覽器要求路由傳送至控制器動作。 若要充分利用 ASP.NET 路由，您可能在您的 web 伺服器上執行其他設定步驟。 它完全取決於 Internet Information Services (IIS) 和要求處理模式，您的應用程式的版本。

以下是不同的 IIS 版本的摘要：

- IIS 的 7.0 （整合模式）-使用 ASP.NET 路由所需的任何特殊設定。
- IIS 7.0 （傳統模式）-您需要執行特殊的設定，以使用 ASP.NET 路由。
- IIS 6.0 或以下-您需要執行特殊的設定，以使用 ASP.NET 路由。

最新的 IIS 版本是版本 7.5 （Win7)。 IIS 7 的 IIS 是包含與 Windows Server 2008 與 VISTA SP1 和更新版本。 您也可以安裝 IIS 7.0 在 Vista 作業系統，Home Basic 除外的任何版本上 (請參閱[ https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx ](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx))。

IIS 7.0 支援兩種模式來處理要求。 您可以使用整合式的模式或傳統模式。 您不需要執行任何特殊組態步驟，使用 IIS 7.0 整合模式中時。 不過，您需要使用傳統模式中的 IIS 7.0 時執行其他設定。

Microsoft Windows Server 2003 包含 IIS 6.0。 使用 Windows Server 2003 作業系統時，您就無法升級至 IIS 7.0 IIS 6.0。 使用 IIS 6.0 時，您必須執行其他設定步驟。

Microsoft Windows XP Professional 包含 IIS 5.1。 當使用 IIS 5.1，您必須執行其他設定步驟。

最後，Microsoft Windows 2000 和 Microsoft Windows 2000 Professional 包含 IIS 5.0。 使用 IIS 5.0 時，您必須執行其他設定步驟。

## <a name="integrated-versus-classic-mode"></a>與傳統模式整合

IIS 7.0 可處理要求使用兩種不同的要求處理模式： 整合和傳統。 整合模式下提供較佳的效能和更多功能。 傳統模式是適用於處理回溯相容性與較早版本的 IIS。

要求處理模式取決於應用程式集區。 您可以判斷特定的 web 應用程式正在使用哪一個處理模式藉由判斷應用程式相關聯的應用程式集區。 請依照下列步驟：

1. 啟動 Internet Information Services 管理員
2. 在 連線 視窗中，選取 應用程式
3. 在 動作 視窗中，按一下**基本設定**連結以開啟 編輯應用程式 對話方塊 （請參閱 圖 1）
4. 記下所選取的應用程式集區。

根據預設，IIS 會設定為支援兩個應用程式集區： **DefaultAppPool**並**傳統.NET AppPool**。 如果選取 [DefaultAppPool]，在整合式的要求處理模式中執行您的應用程式。 如果已選取傳統.NET 應用程式集區，您的應用程式以傳統的要求處理模式中執行。


[![[新增專案] 對話方塊](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.png)

**圖 1**： 偵測要求處理模式 ([按一下以檢視完整大小的影像](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.png))


請注意，您可以修改要求處理模式中編輯應用程式 對話方塊中。 按一下 [選取] 按鈕，並變更應用程式相關聯的應用程式集區。 請注意，相容性問題時沒有從傳統變更 ASP.NET 應用程式，以整合模式。 如需詳細資訊，請參閱下列文章：

- 升級至 Windows Vista 和 Windows Server 2008 上，IIS 7.0 的 ASP.NET 1.1 [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)

- 與 IIS 7.0-ASP.NET 整合 [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)


如果 ASP.NET 應用程式使用 「 DefaultAppPool 」，您不需要執行任何額外的步驟，以取得 ASP.NET 路由，因此 ASP.NET MVC） 運作。 不過，如果 ASP.NET 應用程式設定成使用傳統.NET 應用程式時，會將集區，然後繼續閱讀，您會有更多工作來執行。

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>使用 ASP.NET MVC 與較舊版本的 IIS

如果您需要使用較舊版本比 IIS 7.0 之 IIS 的 ASP.NET MVC，或您需要使用傳統模式中的 IIS 7.0，您會有兩個選項。 首先，您可以修改的路由表，來使用檔案的副檔名。 比方說，而不是要求 /Store/Details 之類的 URL，您便可以要求 /Store.aspx/Details 之類的 URL。

第二個選項是建立所謂*萬用字元指令碼對應*。 萬用字元指令碼對應可讓您將每個要求對應至 ASP.NET 架構。

如果您不需要存取您的網頁伺服器 (例如，您的 ASP.NET MVC 應用程式由網際網路服務提供者裝載) 將會需要使用第一個選項。 如果您不想要修改您的 Url 的外觀，而且您可以存取您的 web 伺服器，您可以使用第二個選項。

我們將探討下列章節詳細說明每個選項。

## <a name="adding-extensions-to-the-route-table"></a>路由表中加入擴充功能

取得 ASP.NET 路由傳送至舊版 IIS 所使用的最簡單方式是修改您在 Global.asax 檔案中的路由表。 預設值，並在 列表 1 中的未修改的 Global.asax 檔案設定一個名為預設路由的路由。

**列表 1-Global.asax （未經修改）**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample1.vb)]

在 列表 1 中設定的預設路由可讓您看起來像這樣的路由 Url:

/ Home/Index

/ 產品/詳細資料/3

/ 產品

不幸的是，舊版 IIS 將不會將這些要求傳遞給 ASP.NET 架構。 因此，這些要求將不會路由至控制器。 例如，如果您的瀏覽器要求對 URL /Home/索引然後得到錯誤頁面 [圖 2] 中。


[![[新增專案] 對話方塊](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.png)

**圖 2**： 收到 404 找不到的錯誤 ([按一下以檢視完整大小的影像](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.png))


舊版 IIS 只會對應至 ASP.NET framework 的特定要求。 要求必須是正確的檔案副檔名的 URL。 比方說，/SomePage.aspx 要求取得對應至 ASP.NET 架構。 不過，/SomePage.htm 的要求則否。

因此，若要取得 ASP.NET 路由傳送至工作，我們必須修改預設路由使其包含對應至 ASP.NET framework 的副檔名。

這是使用名為指令碼`registermvc.wsf`。 它是隨附在中的 ASP.NET MVC 1 發行`C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`，但 ASP.NET 2 此指令碼已移至 ASP.NET Futures 網址[ http://aspnet.codeplex.com/releases/view/39978 ](http://aspnet.codeplex.com/releases/view/39978)。

執行此指令碼，是在 IIS 中向新的.mvc 副檔名。 將.mvc 副檔名登錄在之後，您可以修改您 Global.asax 檔案中的路由，以便路由使用.mvc 副檔名。

修改的 Global.asax 檔案列表 2 中的運作方式與較舊版本的 IIS。

**列表 2-Global.asax （使用擴充功能修改）**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample2.vb)]


重要事項： 請記得在變更 Global.asax 檔案之後，再次建置您的 ASP.NET MVC 應用程式。


有兩個重要的變更，以列表 2 中的 Global.asax 檔。 現在有兩個 Global.asax 中所定義的路由。 預設路由，而第一個路由的 URL 模式現在看起來像：

{controller}.mvc/{action}/{id}

新增.mvc 副檔名會變更 ASP.NET 路由模組攔截的檔案的類型。 透過這項變更，ASP.NET MVC 應用程式現在將路由傳送要求，如下所示：

/Home.mvc/Index/

/Product.mvc/Details/3

/Product.mvc/

第二個路由，也就是根路由，是新功能。 此根路由的 URL 模式為空字串。 此路由是必要的比對要求對應用程式的根目錄。 例如，根路由會比對的要求，看起來像這樣：

[http://www.YourApplication.com/](http://www.YourApplication.com/)

之後進行這些修改以您的路由表，您必須確定所有應用程式中的連結會使用這些新的 URL 模式相容。 換句話說，請確定所有的連結包含.mvc 副檔名。 如果您使用 Html.ActionLink() helper 方法來產生您的連結，然後您應該不需要進行任何變更。


而不是使用 registermvc.wcf 指令碼，您可以將新的延伸模組加入以手動方式對應至 ASP.NET framework 的 IIS。 當自行新增新的延伸模組，請確定核取方塊標示**確認該檔案已經存在**就不會檢查。


## <a name="hosted-server"></a>託管的伺服器

您永遠不能存取到您的 web 伺服器。 比方說，如果您裝載使用網際網路裝載提供者的 ASP.NET MVC 應用程式，接著您不一定可存取 iis。

在此情況下，您應該使用其中一個現有的檔案副檔名對應到 ASP.NET 架構。 副檔名對應到 ASP.NET 的範例包括.aspx、.axd 和.ashx 延伸模組。

比方說，修改過的 Global.asax 檔案列表 3 中會使用.aspx 擴充功能，而非.mvc 副檔名。

**列表 3-Global.asax （使用.aspx 擴充功能修改）**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample3.vb)]

列表 3 中的 Global.asax 檔正是與先前的 Global.asax 檔案，除了它使用.aspx 擴充功能，而非.mvc 副檔名相同。 您沒有使用副檔名.aspx 遠端網頁伺服器上執行任何設定。

## <a name="creating-a-wildcard-script-map"></a>建立萬用字元指令碼對應

如果您不想要修改的 ASP.NET MVC 應用程式的 Url，而且您可以存取您的 web 伺服器，您會有額外的選項。 您可以建立萬用字元指令碼對應將所有的要求對應至 ASP.NET framework web 伺服器。 如此一來，您可以使用預設 ASP.NET MVC 路由表 （以傳統模式） 的 IIS 7.0 或 IIS 6.0。

請注意，此選項可讓 IIS 以攔截對網頁伺服器提出的每個要求。 這包括影像、 傳統 ASP 網頁和 HTML 網頁的要求。 因此，啟用萬用字元指令碼對應至 ASP.NET 沒有影響效能。

以下是如何啟用萬用字元指令碼對應的 IIS 7.0:

1. 在 [連線] 視窗中選取您的應用程式
2. 請確定**功能**選取檢視
3. 按兩下**處理常式對應**按鈕
4. 按一下 **新增萬用字元指令碼對應**連結 （請參閱 圖 3）
5. 輸入的路徑 aspnet\_isapi.dll 檔案 （您可以從 Pagehandlerfactory-isapi 指令碼對應複製這個路徑）
6. 輸入名稱 MVC
7. 按一下 **確定**按鈕


[![[新增專案] 對話方塊](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.png)

**圖 3**： 使用 IIS 7.0 建立萬用字元指令碼對應 ([按一下以檢視完整大小的影像](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image6.png))


請遵循下列步驟來建立與 IIS 6.0 的萬用字元指令碼對應：

1. 以滑鼠右鍵按一下網站，然後選取 屬性
2. 選取 [**主目錄**] 索引標籤
3. 按一下 **組態**按鈕
4. 選取 [**對應**] 索引標籤
5. 按一下 **插入**按鈕 （請參閱 圖 4）
6. 貼上路徑 aspnet\_isapi.dll 到可執行檔的欄位 （您可以從指令碼對應.aspx 檔案複製這個路徑）
7. 標示的核取方塊取消核取 **確認該檔案已經存在**
8. 按一下 **確定**按鈕


[![[新增專案] 對話方塊](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image7.png)

**圖 4**： 使用 IIS 6.0 建立萬用字元指令碼對應 ([按一下以檢視完整大小的影像](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image8.png))


啟用萬用字元指令碼對應之後，您需要修改 Global.asax 檔案中的路由表，使其包含根路由。 否則，您會得到錯誤頁面圖 5 中，提出您的應用程式的根頁面的要求時。 您可以使用修改過的 Global.asax 檔案中列出的 4。


[![[新增專案] 對話方塊](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image9.png)

**圖 5**： 遺漏根路由錯誤 ([按一下以檢視完整大小的影像](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image10.png))


**列表 4-Global.asax （根路由與修改）**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample4.vb)]

IIS 7.0 或 IIS 6.0 中啟用萬用字元指令碼對應之後，您可以建立使用預設路由表，看起來像這樣的要求：

/

/ Home/Index

/ 產品/詳細資料/3

/ 產品

## <a name="summary"></a>總結

本教學課程的目標是要說明如何使用 ASP.NET MVC，使用較舊版本的 IIS （或傳統模式中的 IIS 7.0） 時。 我們將討論兩種方法可以取得 ASP.NET 路由傳送至舊版 IIS 的工作： 修改預設路由表，或建立萬用字元指令碼對應。

第一個選項需要您修改您的 ASP.NET MVC 應用程式中使用的 Url。 這個第一個選項的其中一個非常重要的優點是，您不需要 web 伺服器的存取權才能修改路由表。 這表示，您就可以使用此第一個選項，即使裝載 ASP.NET MVC 應用程式使用網際網路主機服務公司。

第二個選項是建立萬用字元指令碼對應。 此第二個選項的優點是您不需要修改您的 Url。 此第二個選項的缺點是，它可能會影響您的 ASP.NET MVC 應用程式的效能。

> [!div class="step-by-step"]
> [上一步](using-asp-net-mvc-with-different-versions-of-iis-cs.md)
