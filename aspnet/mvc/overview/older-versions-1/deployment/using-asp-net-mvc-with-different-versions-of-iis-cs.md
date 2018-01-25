---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
title: "ASP.NET MVC 中使用不同版本的 IIS (C#) |Microsoft 文件"
author: microsoft
description: "在本教學課程中，您可以了解如何使用 ASP.NET MVC 和 URL 路由，處理不同版本的 Internet Information Services。 您了解不同的策略..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: b0cf4a34-2c1d-4717-bb54-ff029e722990
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-cs
msc.type: authoredcontent
ms.openlocfilehash: 8f2b98d5e5ae677fdac32336d542202a40290e21
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="using-aspnet-mvc-with-different-versions-of-iis-c"></a>ASP.NET MVC 中使用不同版本的 IIS (C#)
====================
by [Microsoft](https://github.com/microsoft)

> 在本教學課程中，您可以了解如何使用 ASP.NET MVC 和 URL 路由，處理不同版本的 Internet Information Services。 您了解不同的策略來使用 （傳統模式） 的 IIS 7.0、 IIS 6.0 和舊版 IIS 的 ASP.NET MVC。


ASP.NET MVC framework 取決於 ASP.NET 路由瀏覽器要求路由至控制器的動作。 以利用 ASP.NET 路由，您可能必須在網頁伺服器上執行其他設定步驟。 這完全取決於網際網路資訊服務 (IIS) 和處理模式，您的應用程式要求的版本。

以下是不同的 IIS 版本的摘要：

- IIS 的 7.0 （整合模式）-使用 ASP.NET 路由所需的任何特殊設定。
- IIS 7.0 （傳統模式）-您需要執行特殊的設定，若要使用 ASP.NET 路由。
- IIS 6.0 或以下-您需要執行特殊的設定，若要使用 ASP.NET 路由。

最新的 IIS 版本是版本 7.5 （在 Win7)。 IIS 的 IIS 7 是包含 Windows Server 2008 和 VISTA/SP1 及更高版本。 您也可以安裝 IIS 7.0 上 Vista 作業系統 Home Basic 以外的任何版本 (請參閱[https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx))。

IIS 7.0 支援兩種模式來處理要求。 您可以使用整合式的模式或傳統模式。 您不需要執行任何特殊組態步驟，在整合模式中使用 IIS 7.0 時。 不過，您需要使用傳統模式的 IIS 7.0 時執行其他設定。

Microsoft Windows Server 2003 包含 IIS 6.0。 使用 Windows Server 2003 作業系統時，您無法升級至 IIS 7.0 IIS 6.0。 使用 IIS 6.0 時，您必須執行其他設定步驟。

Microsoft Windows XP Professional 包含 IIS 5.1。 當使用 IIS 5.1，您必須執行其他設定步驟。

最後，Microsoft Windows 2000 和 Microsoft Windows 2000 Professional 包含 IIS 5.0。 使用 IIS 5.0 時，您必須執行其他設定步驟。

## <a name="integrated-versus-classic-mode"></a>與傳統模式整合

IIS 7.0 可處理要求使用兩種不同的要求處理模式： 整合和傳統。 整合式的模式會提供更佳的效能和更多的功能。 傳統模式包含是為了回溯相容性與 IIS 的舊版本。

要求處理模式取決於應用程式集區。 您可以判斷哪一個處理模式由特定 web 應用程式透過判斷應用程式相關聯的應用程式集區使用。 請依照下列步驟：

1. 啟動 Internet Information Services 管理員
2. 在 連線 視窗中，選取 應用程式
3. 在 動作 視窗中，按一下**基本設定**連結以開啟 編輯應用程式對話方塊 （請參閱圖 1）
4. 記下選取的應用程式集區。

根據預設，IIS 設定為支援兩個應用程式集區： **DefaultAppPool**和**傳統.NET AppPool**。 如果選取 [DefaultAppPool]，在整合式的要求處理模式中執行應用程式。 如果選取 [傳統.NET AppPool]，在傳統的要求處理模式中執行應用程式。

[![新增專案 對話方塊](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image1.png)

**圖 1**： 偵測要求處理模式 ([按一下以檢視完整大小的影像](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.png))

請注意，您可以修改 [編輯應用程式] 對話方塊中的要求處理模式。 按一下 [選取] 按鈕，然後變更應用程式相關聯的應用程式集區。 請注意，有相容性問題整合模式下從傳統變更 ASP.NET 應用程式時。 如需詳細資訊，請參閱下列文章：

- 升級至 Windows Vista 和 Windows Server 2008-上的 IIS 7.0 ASP.NET 1.1 [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)
- 與 IIS 7.0 的 ASP.NET 整合[https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)

如果 ASP.NET 應用程式使用 DefaultAppPool，您不需要執行任何額外的步驟，以取得 ASP.NET 路由 （以及 ASP.NET MVC） 運作。 不過，如果 ASP.NET 應用程式設定成使用傳統的.NET 應用程式集區，然後繼續閱讀，您會有更多工作來執行。

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>使用 ASP.NET MVC 與舊版本的 IIS

如果您需要使用 ASP.NET MVC 與較舊版本的 IIS 於 IIS 7.0，或您需要使用傳統模式的 IIS 7.0，您有兩個選項。 首先，您可以修改路由表新增至使用檔案的副檔名。 例如，而不是要求的 URL，例如 /Store/Details，您便可以要求的 URL，例如 /Store.aspx/Details。

第二個選項是建立所謂*萬用字元指令碼對應*。 萬用字元指令碼對應可讓您將每個要求對應至 ASP.NET 架構。

如果您沒有存取您的網頁伺服器 (例如，您的 ASP.NET MVC 應用程式由網際網路服務提供者) 則您必須使用第一個選項。 如果您不想要修改您的 Url 的外觀，而且您可以存取您的 web 伺服器，您可以使用第二個選項。

我們會探索下列章節詳細說明每個選項。

## <a name="adding-extensions-to-the-route-table"></a>將延伸加入至路由表

取得 ASP.NET 路由傳送至舊版 IIS 所使用的最簡單方式是修改程式 Global.asax 檔案中的路由表。 預設值和未修改的 Global.asax 檔案中列出的 1 會設定一個名為預設路由的路由。

**列出 1-Global.asax （未修改）**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample1.cs)]

在程式碼範例 1 中設定的預設路由可讓您看起來像這樣的路由 Url:

/ Home/Index

/Product/Details/3

/ 產品

不幸的是，較舊版本的 IIS 不會將這些要求傳遞至 ASP.NET 架構。 因此，這些要求將不會路由至控制器。 例如，如果您的瀏覽器要求對 URL /Home/索引然後您會得到錯誤頁面在圖 2 中。

[![新增專案 對話方塊](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.png)

**圖 2**： 收到 404 找不到的錯誤 ([按一下以檢視完整大小的影像](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.png))

舊版 IIS 只會對應至 ASP.NET framework 的特定要求。 要求必須是正確的檔案副檔名的 URL。 例如，/SomePage.aspx 要求取得對應至 ASP.NET 架構。 不過，/SomePage.htm 要求卻不符合。

因此，若要取得 ASP.NET 路由運作，我們必須修改預設路由，使其包含對應至 ASP.NET framework 的副檔名。

這是使用名為指令碼`registermvc.wsf`。 隨附的 ASP.NET MVC 1 版`C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`，但 ASP.NET 2 為準，此指令碼已移至 ASP.NET 未來位於[http://aspnet.codeplex.com/releases/view/39978](http://aspnet.codeplex.com/releases/view/39978)。

執行此指令碼向 IIS 註冊新的.mvc 副檔名。 註冊.mvc 副檔名之後，您可以修改您在 Global.asax 檔案中的路由，以便路由使用.mvc 副檔名。

修改的 Global.asax 檔案中列出的 2 的運作方式與舊版本的 IIS。

**列出 2-Global.asax （已修改與擴充功能）**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample2.cs)]

**重要**： 請記得要變更的 Global.asax 檔案後，再建置您的 ASP.NET MVC 應用程式。

有兩個重要 Global.asax 檔案中列出的 2 的變更。 現在有兩個在 Global.asax 中定義的路由。 預設的第一個路由，路由的 URL 模式現在看起來像：

{controller}.mvc/{action}/{id}

加入.mvc 副檔名的變更 ASP.NET 路由模組會攔截的檔案的類型。 透過這項變更，ASP.NET MVC 應用程式現在會路由傳送要求，如下所示：

/Home.mvc/Index/

/Product.mvc/Details/3

/Product.mvc/

第二個路由，也就是根的路徑，是新功能。 此 URL 的模式根路徑是空字串。 此路由是應用的必要的比對要求程式的根目錄。 例如，根路由會比對的要求，看起來像這樣：

[http://www.YourApplication.com/](http://www.YourApplication.com/)

進行這些修改以路由表之後, 您必須確定所有應用程式中的連結會使用這些新的 URL 模式相容。 換句話說，請確定所有連結的包含.mvc 副檔名。 如果您使用 Html.ActionLink() helper 方法以產生您的連結，然後您應該不需要進行任何變更。

不使用 registermvc.wcf 指令碼，您可以將新的延伸模組加入以手動方式對應至 ASP.NET framework 的 IIS。 當自行加入新的延伸，請確定核取方塊標示為**確認該檔案已經存在**就不會檢查。

## <a name="hosted-server"></a>託管的伺服器

您永遠不必存取您的 web 伺服器。 例如，如果裝載的 ASP.NET MVC 應用程式使用網際網路裝載提供者，就不一定需要 IIS 的存取。

在此情況下，您應該使用其中一個現有對應至 ASP.NET framework 的副檔名。 對應至 ASP.NET 的檔案副檔名的範例包括.aspx、.axd 和.ashx 擴充功能。

比方說，修改的 Global.asax 檔案中列出的 3 會而.mvc 副檔名不是使用.aspx 副檔名。

**列出 3-Global.asax （副檔名為.aspx 修改）**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample3.cs)]

Global.asax 檔案中列出的 3 是完全相同的與先前的 Global.asax 檔案，除了它使用.aspx 副檔名，而不是.mvc 副檔名的事實。 您沒有使用副檔名.aspx 遠端網頁伺服器上執行任何設定。

## <a name="creating-a-wildcard-script-map"></a>建立萬用字元指令碼對應

如果您不想要修改 ASP.NET MVC 應用程式的 Url，而且您可以存取您的 web 伺服器，您會有額外的選項。 您可以建立將所有的要求對應至 web 伺服器，以 ASP.NET framework 萬用字元指令碼對應。 這樣一來，您可以使用預設 ASP.NET MVC 路由表 （以傳統模式） 的 IIS 7.0 或 IIS 6.0。

請注意，這個選項會讓 IIS 以攔截對網頁伺服器提出的每個要求。 這包括如映像、 傳統 ASP 頁面，以及 HTML 網頁的要求。 因此，啟用萬用字元指令碼對應至 ASP.NET 沒有效能的影響。

以下是如何啟用萬用字元指令碼對應適用於 IIS 7.0:

1. 在 [連線] 視窗中選取您的應用程式
2. 請確定**功能**選取檢視
3. 按兩下**處理常式對應**按鈕
4. 按一下**新增萬用字元指令碼對應**連結 （請參閱圖 3）
5. 輸入的路徑 aspnet\_isapi.dll 檔案 （您可以從 PageHandlerFactory 指令碼對應複製這個路徑）
6. 輸入名稱 MVC
7. 按一下**確定**按鈕

[![新增專案 對話方塊](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.png)

**圖 3**： 萬用字元指令碼對應建立搭配 IIS 7.0 ([按一下以檢視完整大小的影像](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image6.png))

請遵循下列步驟以 IIS 6.0 建立萬用字元指令碼對應：

1. 以滑鼠右鍵按一下網站，然後選取 屬性
2. 選取**主目錄** 索引標籤
3. 按一下**組態**按鈕
4. 選取**對應** 索引標籤
5. 按一下**插入**按鈕 （請參閱圖 4）
6. 貼上的 aspnet 路徑\_isapi.dll 到可執行檔的欄位 （您可以從.aspx 檔案的指令碼對應複製這個路徑）
7. 取消選取核取方塊標示為**確認該檔案已經存在**
8. 按一下**確定**按鈕

[![新增專案 對話方塊](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image7.png)

**圖 4**: IIS 6.0 建立萬用字元指令碼對應 ([按一下以檢視完整大小的影像](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image8.png))

啟用萬用字元指令碼對應之後，您需要修改 Global.asax 檔中的路由表，使其包含根路徑。 否則，您會得到錯誤頁面圖 5，當您進行應用程式的根頁面的要求。 您可以使用已修改的 Global.asax 檔案中列出的 4。

[![新增專案 對話方塊](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image9.png)

**圖 5**： 遺漏根路由錯誤 ([按一下以檢視完整大小的影像](using-asp-net-mvc-with-different-versions-of-iis-cs/_static/image10.png))

**列出 4-Global.asax （已修改與根路由）**

[!code-csharp[Main](using-asp-net-mvc-with-different-versions-of-iis-cs/samples/sample4.cs)]

您的 IIS 7.0 或 IIS 6.0 中啟用萬用字元指令碼對應之後，您就可以提出要求可搭配預設路由表，看起來像這樣：

/

/ Home/Index

/Product/Details/3

/ 產品

## <a name="summary"></a>總結

本教學課程的目標是為了說明使用較舊版本的 IIS （或傳統模式中的 IIS 7.0） 時，如何使用 ASP.NET MVC。 我們將討論兩種方法可以取得 ASP.NET 路由使用較舊版本的 IIS： 預設路由表的修改或建立萬用字元指令碼對應。

第一個選項需要您修改您的 ASP.NET MVC 應用程式中使用的 Url。 這個第一個選項的其中一個非常重要的優點是，您不需要 web 伺服器的存取權才能修改路由表。 這表示，您就可以使用此第一個選項，透過網際網路連線中裝載您的 ASP.NET MVC 應用程式時，即使控管公司。

第二個選項是建立萬用字元指令碼對應。 此第二個選項的優點是，您不需要修改您的 Url。 這個第二個選項的缺點是它可能會影響您的 ASP.NET MVC 應用程式的效能。

>[!div class="step-by-step"]
[下一步](using-asp-net-mvc-with-different-versions-of-iis-vb.md)
