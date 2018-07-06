---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: 開始使用 AJAX Control Toolkit (C#) |Microsoft Docs
author: microsoft
description: 了解所有您需要知道若要開始使用 AJAX Control Toolkit。
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: 3682af50eb2b9052ac0b15c2cec084deec10031d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832155"
---
<a name="get-started-with-the-ajax-control-toolkit-c"></a>開始使用 AJAX Control Toolkit (C#)
====================
by [Microsoft](https://github.com/microsoft)

> 了解所有您需要知道若要開始使用 AJAX Control Toolkit。


AJAX Control Toolkit 包含 30 個以上可用的控制項，您可以使用 ASP.NET 應用程式中。 在本教學課程中，您將了解如何下載 AJAX Control Toolkit 和工具組將控制項新增到您的 Visual Studio/Visual Web Developer Express 工具箱。

## <a name="downloading-the-ajax-control-toolkit"></a>下載 AJAX Control Toolkit

[AJAX Control Toolkit](http://devexpress.com/act)由 ASP.NET 社群和 ASP.NET 團隊的成員所開發的開放原始碼專案。 


[![下載 AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)

**圖 01**： 下載 AJAX Control Toolkit ([按一下以檢視完整大小的影像](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))


您可以下載檔案之後，您需要解除封鎖檔案。 以滑鼠右鍵按一下檔案，選取 內容，按一下 **解除封鎖**按鈕 （請參閱 圖 2）。


[![解除封鎖的 AJAX 控制項工具組 ZIP 檔案](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)

**圖 02**： 解除封鎖的 AJAX 控制項工具組 ZIP 檔案 ([按一下以檢視完整大小的影像](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))


解除封鎖檔案之後，您可以將檔案解壓縮： 以滑鼠右鍵按一下檔案，然後選取**擷取所有**功能表選項。 現在，我們已準備好將此工具組新增至 Visual Studio/Visual Web Developer 工具箱。

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>新增至工具箱的 AJAX Control Toolkit

使用 AJAX Control Toolkit 最簡單方式是將此工具組新增至您的 Visual Studio/Visual Web Developer 工具箱 （請參閱 [圖 3]）。 這樣一來，您可以只將 toolkit 控制項拖曳至頁面時您想要使用它。


[![AJAX Control Toolkit 會出現在工具箱中](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)

**圖 03**： 出現在工具箱中的 AJAX Control Toolkit ([按一下以檢視完整大小的影像](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))


首先，您要新增至工具箱的 AJAX Control Toolkit 索引標籤。 請遵循下列步驟。

1. 選取功能表選項的檔案，新的網站，以建立新的 ASP.NET 網站。 按兩下 [方案總管] 視窗中的 Default.aspx，在編輯器中開啟檔案。
2. 以滑鼠右鍵按一下 一般 索引標籤下方的 工具箱，然後選取功能表選項**加入索引標籤**（請參閱 圖 4）。
3. 輸入名為 AJAX Control Toolkit 中的新索引標籤。


[![將新的索引標籤](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)

**圖 04**： 將新的索引標籤 ([按一下以檢視完整大小的影像](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))


接下來，您需要將 AJAX Control Toolkit 控制項新增至新的索引標籤。請依照下列步驟：

- 以滑鼠右鍵按一下下方的 [AJAX Control Toolkit] 索引標籤，然後選取功能表選項**選擇的項目 （請參閱 [圖 5]）**。
- 瀏覽至您解壓縮 AJAX Control Toolkit 和選取 AjaxControlToolkit.dll 組件的位置。


[![選擇要加入至工具箱項目](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)

**圖 05**： 選擇要加入至工具箱項目 ([按一下以檢視完整大小的影像](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))


完成這些步驟之後，所有的工具組控制項都會顯示在工具箱中。

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>升級至新版本的工具組

如果您已使用的版本還舊的工具組，而且現在需要將移至較新的版本是建議的步驟：

- 二進位檔-從您網站的 Bin 資料夾刪除 AjaxControlToolkit.dll 組件的舊版本。
- 工具箱項目-刪除 AJAX Control Toolkit 索引標籤，並遵循上述步驟，以重新建立具有新版 AjaxControlToolkit.dll 組件的索引標籤。

> [!div class="step-by-step"]
> [下一步](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
