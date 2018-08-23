---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 第 7 節： 成員資格和授權 |Microsoft Docs
author: jongalloway
description: 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 7 節涵蓋的成員資格和授權。
ms.author: riande
ms.date: 10/13/2010
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: 44205f0ef59e00ad9fb1c45fdc0ba8934b5804cc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833004"
---
<a name="part-7-membership-and-authorization"></a>第 7 節： 成員資格和授權
====================
藉由[Jon Galloway](https://github.com/jongalloway)

> MVC Music 市集是介紹，並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 進行 web 開發的教學課程應用程式。  
>   
> MVC Music 市集是銷售線上音樂 album 和實作基本的網站管理、 使用者登入時，和 「 購物車 」 功能的輕量級的範例存放區實作。  
>   
> 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 第 7 節涵蓋的成員資格和授權。


目前瀏覽我們的網站的任何人都能存取我們的存放區管理員控制器。 讓我們變更此限制的網站系統管理員的權限。

## <a name="adding-the-accountcontroller-and-views"></a>加入 AccountController 以及檢視

完整的 ASP.NET MVC 3 Web 應用程式範本和 ASP.NET MVC 3 空白 Web 應用程式範本的其中一個差異是空白的範本不包含帳戶控制器。 我們將新增此帳戶控制器會藉由複製幾個檔案從新的 ASP.NET MVC 應用程式，完整的 ASP.NET MVC 3 Web 應用程式範本所建立的。

建立新的 ASP.NET MVC 應用程式，使用完整的 ASP.NET MVC 3 Web 應用程式範本，並將下列檔案複製到相同的目錄，在我們的專案：

1. 在 [Controllers] 目錄中的複製 AccountController.cs
2. 在模型的目錄中的複製 AccountModels
3. 建立檢視目錄內的帳戶目錄，並複製中的所有四個檢視

變更控制器和模型類別的命名空間，因此它們以 MvcMusicStore 開頭。 AccountController 類別應該使用 MvcMusicStore.Controllers 命名空間中，而 AccountModels 類別應該使用 MvcMusicStore.Models 命名空間。

*注意： 這些檔案也是從中複製網站設計檔案在本教學課程開頭 MvcMusicStore Assets.zip 下載中取得。成員資格檔案位於程式碼目錄中。*

更新的方案看起來應該如下所示：

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>新增與 ASP.NET 設定站台系統管理使用者

我們在我們的網站所需的授權之前，我們需要建立的使用者具有存取權。 建立使用者的最簡單方式是使用內建的 ASP.NET 組態網站。

按一下 依照 方案總管 中的圖示來啟動 ASP.NET 組態網站。

![](mvc-music-store-part-7/_static/image2.png)

這會啟動組態網站。 在主畫面上，[安全性] 索引標籤上按一下，然後按一下 [啟用角色] 中的連結在螢幕的中央。

![](mvc-music-store-part-7/_static/image3.png)

按一下 [建立] 或 [管理角色] 連結。

![](mvc-music-store-part-7/_static/image4.png)

輸入 「 系統管理員 」 作為角色名稱，然後按 [新增角色] 按鈕。

![](mvc-music-store-part-7/_static/image5.png)

按一下 [上一頁] 按鈕，然後按一下左側的 [建立使用者] 連結。

![](mvc-music-store-part-7/_static/image6.png)

填妥使用者資訊欄位，在左側，使用下列資訊：

| **欄位** | **值** |
| --- | --- |
| **使用者名稱** | 系統管理員 |
| **密碼** | password123 ！ |
| **確認密碼** | password123 ！ |
| **電子郵件** | （可使用任何電子郵件地址） |
| **安全性問題** | （任意） |
| **安全性解答** | （任意） |

*注意： 您當然可以使用任何您想要的密碼。預設密碼安全性設定需要的 7 個字元且包含一個非英數字元密碼。*

選取此使用者，系統管理員角色，然後按一下 [建立使用者] 按鈕。

![](mvc-music-store-part-7/_static/image7.png)

此時，您應該會看到一則訊息指出使用者已成功建立。

![](mvc-music-store-part-7/_static/image8.png)

您現在可以關閉瀏覽器視窗。

## <a name="role-based-authorization"></a>以角色為基礎的授權

現在我們可以使用 [Authorize] 屬性，指定使用者必須系統管理員角色，才能存取任何類別中的控制器動作 StoreManagerController 限制存取。

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*附註： 可以放置 [Authorize] 屬性，以及在控制器類別層級上特定的動作方法。*

現在瀏覽至 /StoreManager 顯示的登入對話方塊：

![](mvc-music-store-part-7/_static/image9.png)

使用我們新的系統管理員帳戶登入之後, 我們得以將前往專輯編輯畫面，為之前。

> [!div class="step-by-step"]
> [上一頁](mvc-music-store-part-6.md)
> [下一頁](mvc-music-store-part-8.md)
