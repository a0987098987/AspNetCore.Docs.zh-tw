---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 第 7 部分： 成員資格和授權 |Microsoft 文件
author: jongalloway
description: 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 7 部分涵蓋的成員資格和授權。
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2010
ms.topic: article
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: a0f599da4691c5bb7c8e6f01625fc0e94ce0eac8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="part-7-membership-and-authorization"></a>第 7 部分： 成員資格和授權
====================
由[Jon Galloway](https://github.com/jongalloway)

> MVC Music Store 是介紹並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 針對 web 程式開發的教學課程應用程式。  
>   
> MVC Music Store 是輕量級範例存放區實作銷售音樂專輯上線，並實作基本的網站管理、 使用者登入和購物車的功能。  
>   
> 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 7 部分涵蓋的成員資格和授權。


目前瀏覽我們的網站的任何人存取我們的存放區管理員控制器。 讓我們來變更這項限制站台系統管理員的權限。

## <a name="adding-the-accountcontroller-and-views"></a>加入 AccountController 以及檢視

完整的 ASP.NET MVC 3 Web 應用程式範本與 ASP.NET MVC 3 空 Web 應用程式範本之間的差異是空白的範本不包含帳戶控制器。 我們會將帳戶控制器新增完整的 ASP.NET MVC 3 Web 應用程式範本所建立的新 ASP.NET MVC 應用程式從複製一些檔案。

建立新的 ASP.NET MVC 應用程式，使用完整的 ASP.NET MVC 3 Web 應用程式範本，並將下列檔案複製到我們的受測專案的相同目錄：

1. 控制站目錄複製 AccountController.cs
2. 模型目錄複製 AccountModels
3. 建立帳戶目錄檢視目錄內，並複製所有的四個檢視中

變更控制器和模型類別的命名空間，讓它們以 MvcMusicStore 開頭。 AccountController 類別應使用 MvcMusicStore.Controllers 命名空間，而且 AccountModels 類別應該使用 MvcMusicStore.Models 命名空間。

*注意： 這些檔案也會提供在從中複製我們站台設計檔案在本教學課程開頭 MvcMusicStore Assets.zip 下載。成員資格檔案位於程式碼目錄中。*

更新的方案看起來應該如下所示：

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>新增與 ASP.NET 設定站台系統管理使用者

我們在我們的網站所需的授權之前，我們需要建立的使用者具有存取權。 建立使用者的最簡單方式是使用內建 ASP.NET 組態網站。

遵循 [方案總管] 中的圖示，即可啟動 ASP.NET 組態網站。

![](mvc-music-store-part-7/_static/image2.png)

這會啟動組態網站。 按一下 [首頁] 螢幕上的 [安全性] 索引標籤，然後按一下 [啟用角色] 中的連結，在螢幕的中央。

![](mvc-music-store-part-7/_static/image3.png)

按一下 [建立或管理角色] 連結。

![](mvc-music-store-part-7/_static/image4.png)

輸入 「 系統管理員 」 做為角色名稱，然後按 [新增角色] 按鈕。

![](mvc-music-store-part-7/_static/image5.png)

按一下 [上一頁] 按鈕，然後按一下左邊的 [建立使用者] 連結。

![](mvc-music-store-part-7/_static/image6.png)

填入使用者資訊欄位左邊使用下列資訊：

| **欄位** | **值** |
| --- | --- |
| **使用者名稱** | 系統管理員 |
| **密碼** | password123 ！ |
| **確認密碼** | password123 ！ |
| **電子郵件** | （任何電子郵件地址將作用中） |
| **安全性問題** | （任意） |
| **安全性解答** | （任意） |

*注意： 您當然可以使用任何您想要的密碼。預設的密碼安全性設定需要的 7 個字元且包含一個非英數字元密碼。*

選取此使用者，系統管理員角色，然後按一下 [建立使用者] 按鈕。

![](mvc-music-store-part-7/_static/image7.png)

此時，您應該會看到訊息，指出使用者已成功建立。

![](mvc-music-store-part-7/_static/image8.png)

您現在可以關閉瀏覽器視窗。

## <a name="role-based-authorization"></a>以角色為基礎的授權

現在我們可以使用 [Authorize] 屬性，指定使用者必須系統管理員角色，才能存取任何類別中的控制器動作 StoreManagerController 限制存取。

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*附註： 可以放置 [Authorize] 屬性，以及控制器類別層級上特定動作方法。*

現在瀏覽至 /StoreManager 登入 對話方塊隨即開啟：

![](mvc-music-store-part-7/_static/image9.png)

我們新的系統管理員帳戶登入之後, 您可以前往專輯編輯畫面上做為前。

> [!div class="step-by-step"]
> [上一頁](mvc-music-store-part-6.md)
> [下一頁](mvc-music-store-part-8.md)
