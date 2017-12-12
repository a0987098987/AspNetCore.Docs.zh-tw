---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: "開始使用 AJAX Control Toolkit (C#) |Microsoft 文件"
author: microsoft
description: "了解您只需要知道要開始使用 AJAX Control Toolkit。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: 8d3f4dd26a9f82dce78b1c3665f9da6b54bdacba
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="get-started-with-the-ajax-control-toolkit-c"></a>開始使用 AJAX Control Toolkit (C#)
====================
由[Microsoft](https://github.com/microsoft)

> 了解您只需要知道要開始使用 AJAX Control Toolkit。


AJAX Control Toolkit 包含 30 個以上可用的控制項，您可以在 ASP.NET 應用程式中使用。 在本教學課程中，您可以了解如何下載 AJAX Control Toolkit，並將工具組的控制項新增至您的 Visual Studio/Visual Web Developer Express 工具箱。

## <a name="downloading-the-ajax-control-toolkit"></a>下載 AJAX Control Toolkit

[AJAX Control Toolkit](http://devexpress.com/act)開放原始碼專案開發時由 ASP.NET 社群和 ASP.NET 團隊的成員。 


[![下載 AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)

**圖 01**： 下載 AJAX Control Toolkit ([按一下以檢視完整大小的影像](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))


下載檔案之後，您需要解除封鎖檔案。 以滑鼠右鍵按一下檔案，選取 內容，按一下 **解除封鎖**按鈕 （請參閱圖 2）。


[![解除封鎖 AJAX 控制項 Toolkit ZIP 檔案](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)

**圖 02**： 解除封鎖 AJAX 控制項 Toolkit ZIP 檔案 ([按一下以檢視完整大小的影像](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))


解除封鎖檔案之後，您可以將檔案解壓縮： 以滑鼠右鍵按一下檔案，然後選取**全部解壓縮**功能表選項。 現在，我們已經準備好要將此工具組加入至 Visual Studio/Visual Web Developer 的 [工具箱]。

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>在 [工具箱] 加入 AJAX Control Toolkit

若要使用 AJAX Control Toolkit 最簡單的方式是將此工具組加入至您的 Visual Studio/Visual Web Developer 工具箱 （請參閱圖 3）。 這樣一來，您可以只將 toolkit 控制項拖曳至網頁時要使用它。


[![AJAX Control Toolkit 會出現在 [工具箱]](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)

**圖 03**: AJAX Control Toolkit 會出現在工具箱 ([按一下以檢視完整大小的影像](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))


首先，您需要將 AJAX Control Toolkit 索引標籤加入 [工具箱]。 請遵循下列步驟。

1. 選取功能表選項的檔案，新的網站，以建立新的 ASP.NET 網站。 按兩下 [方案總管] 視窗中的 Default.aspx，在編輯器中開啟檔案。
2. 以滑鼠右鍵按一下 [一般] 索引標籤下方的 [工具箱]，然後選取功能表選項**加入索引標籤**（請參閱圖 4）。
3. 輸入新的索引標籤，名為 AJAX Control Toolkit。


[![將新的索引標籤](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)

**圖 04**： 將新的索引標籤 ([按一下以檢視完整大小的影像](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))


接著，您必須將 AJAX Control Toolkit 的控制項新增至新的索引標籤。請依照下列步驟：

- 以滑鼠右鍵按一下下方的 AJAX Control Toolkit 索引標籤，然後選取功能表選項**選擇的項目 （請參閱圖 5）**。
- 瀏覽至您解壓縮 AJAX Control Toolkit 和選取 AjaxControlToolkit.dll 組件的位置。


[![選擇要新增至工具箱項目](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)

**圖 05**： 選擇要新增至工具箱項目 ([按一下以檢視完整大小的影像](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))


完成這些步驟之後，所有工具組控制項仍會出現在工具箱中。

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>升級至新版的工具組

如果您已使用的版本還舊的工具組，而且現在需要將移到較新版本這裡是建議的步驟：

- 二進位檔-從網站的 Bin 資料夾刪除 AjaxControlToolkit.dll 組件的舊版本。
- 工具箱項目-刪除 AJAX Control Toolkit 索引標籤，並遵循上述步驟，以重新建立具有新版 AjaxControlToolkit.dll 組件的索引標籤。

>[!div class="step-by-step"]
[下一步](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
