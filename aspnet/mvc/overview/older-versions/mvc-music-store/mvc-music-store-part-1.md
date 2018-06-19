---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 第 1 部分： 概觀和檔案]-> [新增專案 |Microsoft 文件
author: jongalloway
description: 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 1 部分涵蓋概觀和檔案]-> [新的專案。
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 2082927d18c95563893da199d60347fa15952446
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868967"
---
<a name="part-1-overview-and-file-new-project"></a>第 1 部分： 概觀和檔案]-> [新增專案
====================
由[Jon Galloway](https://github.com/jongalloway)

> MVC Music Store 是介紹並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 針對 web 程式開發的教學課程應用程式。  
>   
> MVC Music Store 是輕量級範例存放區實作銷售音樂專輯上線，並實作基本的網站管理、 使用者登入和購物車的功能。  
>   
> 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 第 1 部分涵蓋的概觀和檔案-&gt;新的專案。


## <a name="overview"></a>總覽

MVC Music Store 是介紹並逐步說明如何使用 ASP.NET MVC 和 Visual Web Developer web 程式開發的教學課程應用程式。 我們將會啟動慢，因此初學者層級的 web 程式開發體驗也沒關係。

我們會建置應用程式是一個簡單的音樂存放區。 有三個應用程式的主要部分： 購物、 簽出，以及系統管理。

![](mvc-music-store-part-1/_static/image1.jpg)

訪客可以瀏覽相簿的影片：

![](mvc-music-store-part-1/_static/image2.jpg)

他們可以檢視單張專輯，並將它加入至購物車：

![](mvc-music-store-part-1/_static/image3.jpg)

它們可以檢閱其購物車，並移除不再需要的任何項目：

![](mvc-music-store-part-1/_static/image4.jpg)

繼續簽出將會提示他們登入或註冊的使用者帳戶。

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

建立帳戶後，它們可以透過填寫 傳送和付款資訊完成順序。 為了簡單起見，我們執行令人讚嘆的升級： 的所有項目是如果使用者輸入促銷代碼 「 可用 」 的免費 ！

![](mvc-music-store-part-1/_static/image5.jpg)

排序之後，他們會看到簡單確認畫面：

![](mvc-music-store-part-1/_static/image6.jpg)

除了客戶 faceing 頁面，我們也會建立顯示一份相簿的系統管理員可以建立、 編輯、 系統管理員區段，並刪除專輯：

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1.檔案-&gt;新專案

### <a name="installing-the-software"></a>安裝軟體

本教學課程一開始會先建立使用免費 Visual Web Developer 2010 Express （這是可用），新的 ASP.NET MVC 3 專案，然後我們會以累加方式將新增至建立的完整運作應用程式的功能。 過程中，我們將討論資料庫存取權、 表單張貼案例、 資料驗證使用 AJAX 頁面更新和驗證、 使用者登入，以及多個一致的頁面配置，請使用主版頁面。

您可以展開冒險逐步解說，或者，您可以下載完成的應用程式從[MVC Music Store](https://github.com/evilDave/MVC-Music-Store)。

您可以使用 Visual Studio 2010 SP1 或 Visual Web Developer 2010 Express SP1 （Visual Studio 2010 的免費版本） 建置應用程式。 我們將使用 SQL Server Compact （也免費） 來主控資料庫。 開始之前，請確定您已安裝下面所列的必要條件。


- [Visual Studio Web Developer Express SP1 必要條件]
- [ASP.NET MVC 3 Tools Update]
- [SQL Server Compact 4.0]-包括執行階段和工具的支援


### <a name="creating-a-new-aspnet-mvc-3-project"></a>建立新的 ASP.NET MVC 3 專案

我們會先從在 Visual Web Developer 的 [檔案] 功能表中選取 「 新專案 」。 這會開啟 [新增專案] 對話方塊。

![](mvc-music-store-part-1/_static/image5.png)

我們將選取的 Visual C#-&gt;網站範本左邊的群組，然後選擇 中心資料行中的 < ASP.NET MVC 3 Web 應用程式 」 範本。 MvcMusicStore 您專案的名稱，然後按 [確定] 按鈕。

![](mvc-music-store-part-1/_static/image8.jpg)

這會顯示次要 對話方塊，讓我們進行一些 MVC 特定設定，為受測專案。 選取下列選項：

專案範本： 選取 空白

檢視引擎-選取 Razor

使用 HTML5 語意標記-檢查

請確認您的設定，如下所示，然後按 [確定] 按鈕。

![](mvc-music-store-part-1/_static/image9.jpg)

這會建立專案。 讓我們看看我們的應用程式，在右邊的 [方案總管] 中已加入的資料夾。

![](mvc-music-store-part-1/_static/image10.jpg)

空白的 MVC 3 範本未完全空白 – 它會加入基本資料夾結構：

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC 會使用一些基本的命名慣例的資料夾名稱：

| **Folder** | **目的** |
| --- | --- |
| **/Controllers** | 控制站回應從瀏覽器中輸入時，決定要用來執行工作，並將回應傳回至使用者。 |
| **/Views** | 檢視保存我們 UI 的範本 |
| **/Models** | 模型保存和操作資料 |
| **/Content** | 此資料夾包含我們影像、 CSS 和任何其他靜態內容 |
| **/Scripts** | 此資料夾包含我們的 JavaScript 檔案 |

因為預設的 ASP.NET MVC framework 會使用 「 透過組態的慣例 」 的方法，而讓某些資料夾的命名慣例為基礎的預設假設甚至空的 ASP.NET MVC 應用程式中包含這些資料夾。 比方說，控制站預設會尋找檢視 [檢視] 資料夾中的您不必明確指定這項程式碼中。 會繼續使用預設慣例減少程式碼，您需要撰寫，也可讓它更容易了解您的專案的其他開發人員。 我們將說明當我們建立我們的應用程式的多個這些慣例。

> [!div class="step-by-step"]
> [下一步](mvc-music-store-part-2.md)
