---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: 安裝協助程式中 ASP.NET Web Pages (Razor) 網站 |Microsoft Docs
author: tfitzmac
description: 本文說明如何在 ASP.NET Web Pages (Razor) 網站安裝的協助程式。 協助程式是一種可重複使用的元件，包括程式碼和標記，以每個...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 38290fd47355e7893eddd1f867f47b113b54ca7e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37361802"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>安裝 ASP.NET Web Pages (Razor) 網站中的協助程式
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明如何在 ASP.NET Web Pages (Razor) 網站安裝的協助程式。 A*協助程式*是一種可重複使用的元件，包括程式碼和標記，以執行可能冗長或複雜的工作。
> 
> 您將學到什麼：
> 
> - 如何安裝網站，建立使用 WebMatrix 3 中的協助程式。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - WebMatrix 3


## <a name="overview-of-helpers"></a>協助程式的概觀

使用者通常會想要在網頁上執行某些工作需要大量的程式碼，或需要額外的知識。 範例包括顯示的圖表資料;將 Twitter [跟隨] 按鈕放在頁面的從您的網站; 傳送電子郵件裁剪或調整大小的影像;為您的網站使用 PayPal。 若要讓您輕鬆地執行這類作業，ASP.NET Web Pages 可讓您使用*協助程式*。 協助程式是您安裝站台，並可讓您的元件使用，只要一兩行 Razor 程式碼執行一般工作。

ASP.NET Web Pages 有幾個內建的協助程式。 不過，許多協助程式可使用 NuGet 套件管理員提供的封裝 （增益集） 中。 NuGet 可讓您選取要安裝的套件，然後它會負責安裝的所有詳細資料。

## <a name="installing-a-helper-in-webmatrix-3"></a>在 WebMatrix 3 安裝協助程式

1. 在 WebMatrix 3 中，按一下**NuGet**  按鈕。

    ![在 WebMatrix 中的 [NuGet 資源庫] 對話方塊](installing-helpers/_static/image1.png)
2. 這會啟動 NuGet 套件管理員，並顯示可用的套件。 在 [搜尋] 方塊中，輸入您想要安裝的協助程式是關鍵字。

    ![在 WebMatrix 中的 [NuGet 資源庫] 對話方塊](installing-helpers/_static/image2.png)
3. 選取封裝，然後按一下**安裝**。 按一下 **是**時詢問您是否要安裝套件，並指出您接受條款。

     如果這是您已安裝的協助程式的第一次，NuGet 會在您的網站的協助程式，使您的程式碼中建立資料夾。
4. 若要解除安裝的協助程式，請按一下**資源庫**按鈕，再按一下**已安裝**索引標籤，然後挑選您想要解除安裝的封裝。

## <a name="installing-the-twitter-helper"></a>安裝 Twitter 協助程式

Twitter API 的最新版本不相容，與您透過 NuGet 安裝 Twitter 協助程式。 相反地，請參閱 <<c0> [ 使用 WebMatrix 的 Twitter Helper](twitter-helper.md)主題以取得有關如何設定 Twitter 協助程式，在您的專案中的資訊。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源


[簡介的 ASP.NET Web Pages 2-程式設計基本概念](../getting-started/introducing-razor-syntax-c.md)

[使用 WebMatrix 的 twitter Helper](twitter-helper.md)
