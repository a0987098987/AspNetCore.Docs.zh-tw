---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: 安裝協助程式中 ASP.NET Web Pages (Razor) 站台 |Microsoft 文件
author: tfitzmac
description: 本文說明如何安裝 ASP.NET Web Pages (Razor) 網站中的協助程式。 協助程式是包含程式碼和標記每個可重複使用元件...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 766fbb87ae8bcb8917eb8fa7f552c00792242cf6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896772"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>安裝 ASP.NET Web Pages (Razor) 網站中的協助程式
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明如何安裝 ASP.NET Web Pages (Razor) 網站中的協助程式。 A *helper*是可重複使用的元件，包括程式碼和標記，以執行可能很繁瑣或是複雜的工作。
> 
> 您將學習：
> 
> - 如何在使用 WebMatrix 3 所建立的網站上安裝的協助程式。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - WebMatrix 3


## <a name="overview-of-helpers"></a>協助程式的概觀

人們通常想要在網頁上執行某些工作需要大量的程式碼，或需要額外的知識。 範例包括顯示的圖表資料。將 Twitter [跟隨] 按鈕放在頁面上。傳送電子郵件是來自您的網站。裁剪或調整大小的影像。PayPal 用於您的網站。 若要可讓您輕鬆執行這類項目，ASP.NET Web 網頁可讓您使用*helper*。 協助程式是您安裝站台和，可讓您的元件會使用一條線或 Razor 程式碼的兩個執行一般工作。

ASP.NET Web 網頁會有幾個內建的協助程式。 然而，許多協助程式中的封裝 （增益集） 提供透過 NuGet 套件管理員。 NuGet 可讓您選取要安裝的套件，然後它會負責安裝的所有詳細資料。

## <a name="installing-a-helper-in-webmatrix-3"></a>在 WebMatrix 3 安裝協助程式

1. 在 WebMatrix 3 中，按一下 [ **NuGet** ] 按鈕。

    ![在 WebMatrix NuGet Gallery 對話方塊](installing-helpers/_static/image1.png)
2. 這會啟動 NuGet 封裝管理員，並顯示可用的封裝。 在 [搜尋] 方塊中，輸入您想要安裝的協助程式的關鍵字。

    ![在 WebMatrix NuGet Gallery 對話方塊](installing-helpers/_static/image2.png)
3. 選取封裝，然後按一下**安裝**。 按一下**是**當系統詢問您要安裝封裝，並指出您接受條款。

     如果這是您已安裝的協助程式第一次，NuGet 會構成協助程式程式碼的網站中建立資料夾。
4. 若要解除安裝協助程式，請按一下**圖庫**按鈕，再按一下**已安裝**索引標籤，然後挑選您想要解除安裝的封裝。

## <a name="installing-the-twitter-helper"></a>安裝 Twitter helper

無法與您透過 NuGet 所安裝的 Twitter helper 相容 Twitter API 的最新版本。 請改為參閱[使用 WebMatrix 的 Twitter Helper](twitter-helper.md)主題以取得如何設定您的專案中的 Twitter helper 的相關資訊。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源


[介紹的 ASP.NET Web Pages 2-程式設計基本概念](../getting-started/introducing-razor-syntax-c.md)

[有了 WebMatrix 的 twitter Helper](twitter-helper.md)
