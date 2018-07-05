---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 第 1 部分： 概觀和 檔案-> 新增專案 |Microsoft Docs
author: jongalloway
description: 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 1 部分涵蓋概觀和 檔案-> 新專案。
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 57856a4a78a650e4abe872004e5be5f8f3b2dbcd
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816335"
---
<a name="part-1-overview-and-file-new-project"></a>第 1 部分： 概觀和 檔案-> 新增專案
====================
藉由[Jon Galloway](https://github.com/jongalloway)

> MVC Music 市集是介紹，並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 進行 web 開發的教學課程應用程式。  
>   
> MVC Music 市集是銷售線上音樂 album 和實作基本的網站管理、 使用者登入時，和 「 購物車 」 功能的輕量級的範例存放區實作。  
>   
> 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 1 部分涵蓋的概觀和檔案-&gt;新專案。


## <a name="overview"></a>總覽

MVC Music 市集是介紹，並逐步說明如何使用 ASP.NET MVC 和 Visual Web Developer web 程式開發教學課程應用程式。 我們會慢慢開始，因此層級的 web 開發初學者體驗也沒關係。

我們將建置的應用程式是簡單的音樂播放存放區。 有三個應用程式的主要部分： 購物、 簽出，以及系統管理。

![](mvc-music-store-part-1/_static/image1.jpg)

訪客可以瀏覽相簿的內容類型：

![](mvc-music-store-part-1/_static/image2.jpg)

他們可以檢視單張專輯，並將它新增至購物車：

![](mvc-music-store-part-1/_static/image3.jpg)

它們可以檢閱其購物車，並移除不再需要的任何項目：

![](mvc-music-store-part-1/_static/image4.jpg)

繼續簽出將會提示他們登入或註冊的使用者帳戶。

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

建立帳戶後，他們可以完成順序來填寫 出貨和付款資訊。 為了簡單起見，我們執行令人讚嘆的升級： 所有項目都可用，如果他們輸入促銷代碼 「 免費 」 ！

![](mvc-music-store-part-1/_static/image5.jpg)

之後排序，就會看到簡單的確認畫面：

![](mvc-music-store-part-1/_static/image6.jpg)

客戶 faceing 除了頁面之外，我們也將建置會顯示一份相簿的系統管理員可以建立、 編輯、 系統管理員一節，並刪除 album:

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1.檔案-&gt;新專案

### <a name="installing-the-software"></a>安裝軟體

本教學課程一開始會先建立新的 ASP.NET MVC 3 專案，使用免費 Visual Web Developer 2010 Express （這是免費的），然後再將會以累加方式新增功能來建立完整的運作應用程式。 過程中，我們將討論資料庫存取、 表單張貼案例、 資料驗證，使用一致的頁面配置、 使用 AJAX 頁面更新和驗證、 使用者登入，以及多個主版頁面。

您可以照著逐步進行，或者您可以下載完成的應用程式，從[MVC Music 市集](https://github.com/evilDave/MVC-Music-Store)。

您可以使用 Visual Studio 2010 SP1 或 Visual Web Developer 2010 Express SP1 （Visual Studio 2010 的免費版本） 建置應用程式。 我們將使用 SQL Server Compact （同樣免費） 來裝載資料庫。 在開始之前，請確定您已安裝符合下列先決條件。


- [Visual Studio Web Developer Express SP1 必要條件]
- [ASP.NET MVC 3 Tools Update]
- [SQL Server Compact 4.0]-包括執行階段和工具支援


### <a name="creating-a-new-aspnet-mvc-3-project"></a>建立新的 ASP.NET MVC 3 專案

我們一開始是從在 Visual Web Developer 的 [檔案] 功能表中選取 [新增專案]。 這會顯示 [新增專案] 對話方塊。

![](mvc-music-store-part-1/_static/image5.png)

我們將選取的 Visual C#-&gt; Web 範本左側的群組，然後選擇中間欄中的 「 ASP.NET MVC 3 Web 應用程式 」 範本。 您的專案 MvcMusicStore 命名，然後按 [確定] 按鈕。

![](mvc-music-store-part-1/_static/image8.jpg)

這會顯示次要 對話方塊，讓我們能夠得到一些 MVC 的特定設定我們的專案。 選取下列選項：

專案範本-選取 空白

檢視引擎-選取 Razor

使用 HTML5 語意標記為已核取

請確認您的設定，如下所示，然後按 [確定] 按鈕。

![](mvc-music-store-part-1/_static/image9.jpg)

這會建立我們的專案。 讓我們看看我們在右邊的 [方案總管] 中的應用程式已新增的資料夾。

![](mvc-music-store-part-1/_static/image10.jpg)

空白的 MVC 3 範本不是完全空白-它會新增一個基本的資料夾結構：

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC 會使用資料夾名稱的一些基本的命名慣例：

| [資料夾] | **目的** |
| --- | --- |
| **/ 控制站** | 控制站回應輸入從瀏覽器中，決定要用來執行工作，並回應傳回給使用者的項目。 |
| **/ 檢視表** | 檢視保留我們的 UI 範本 |
| **/ 模型** | 模型保存和管理資料 |
| **/Content** | 此資料夾保留我們的映像、 CSS 和任何其他靜態內容 |
| **/Scripts** | 此資料夾保留我們的 JavaScript 檔案 |

即使空的 ASP.NET MVC 應用程式中包含這些資料夾，因為預設 ASP.NET MVC 架構會使用 「 convention over configuration"的方法和某些資料夾的命名慣例為基礎的預設假設。 比方說，控制站預設會尋找 [Views] 資料夾中的檢視的而不需明確指定這個程式碼中。 繼續使用預設慣例可以減少程式碼，您需要撰寫，也可讓它更容易供其他開發人員了解您的專案。 我們將說明當我們建立我們的應用程式的多個這些慣例。

> [!div class="step-by-step"]
> [下一步](mvc-music-store-part-2.md)
