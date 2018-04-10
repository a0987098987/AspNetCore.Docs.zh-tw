---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
title: 使用 AJAX 控制項 Toolkit 控制項和控制項擴充項 (C#) |Microsoft 文件
author: microsoft
description: 了解如何將 AJAX Control Toolkit 控制項和擴充項加入 ASP.NET 網頁。
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: c1e6b51c-3bc3-4bf7-9916-9991197af3dd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
msc.type: authoredcontent
ms.openlocfilehash: 3d7cea2452db01ca116849ffb17631db3b935668
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-c"></a>使用 AJAX 控制項 Toolkit 控制項和控制項擴充項 (C#)
====================
by [Microsoft](https://github.com/microsoft)

> 了解如何將 AJAX Control Toolkit 控制項和擴充項加入 ASP.NET 網頁。


AJAX Control Toolkit 包含一組控制項和控制項擴充項。 在這個簡短的教學課程中，您會學習如何加入 ASP.NET 網頁上的控制項和控制項擴充項。

> [!NOTE] 
> 
> 如需安裝 AJAX Control Toolkit，然後在 [Visual Studio/Visual Web Developer 工具箱] 中，將 AJAX Control Toolkit 的指示，請參閱本教學課程[開始使用 AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs.md)。


## <a name="using-ajax-control-toolkit-controls"></a>使用 AJAX 控制項 Toolkit 控制項

AJAX Control Toolkit 控制的運作方式類似標準的 ASP.NET 控制項。 您可以將控制項從 [工具箱] 拖曳到 ASP.NET 頁面。 您可以將控制項加入至 設計檢視 或 原始碼檢視中的頁面。

使用 AJAX Control Toolkit 從控制項時，沒有一項特殊需求。 頁面必須包含 ScriptManager 控制項。 ScriptManager 控制項負責包括所有必要的 AJAX Control Toolkit 控制項所需的 JavaScript。

例如，[AJAX Control Toolkit] 索引標籤包含控制項，名為編輯器控制項。 此控制項顯示豐富的 HTML 編輯器。 請遵循下列步驟來加入網頁中的編輯器控制項：

1. 建立新的 ASP.NET 網頁，名為 ShowEditor.aspx
2. 在 [工具箱] 中選取 [AJAX 擴充功能] 索引標籤 from beneath ScriptManager 控制項，並將控制項拖曳到頁面。
3. 在 工具箱 選取 from beneath AJAX Control Toolkit 索引標籤的編輯器控制項，然後將控制項拖曳到頁面 （請參閱圖 1）。 在設計工具應該如下圖 2。
4. 選取功能表選項來執行網站**偵錯，開始偵錯**或按下 F5 鍵。
5. 您應該會看到圖 3 中的頁面。


[![HTML 編輯器控制項選取](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)

**圖 01**: HTML 編輯器控制項選取 ([按一下以檢視完整大小的影像](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))


[![Visual Studio 設計工具與 ScriptManager 和編輯控制項](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)

**圖 02**: ScriptManager 和編輯控制項的 Visual Studio 設計工具 ([按一下以檢視完整大小的影像](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))


[![DisplayEditor.aspx 頁面](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)

**圖 03**: DisplayEditor.aspx 頁面 ([按一下以檢視完整大小的影像](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))


## <a name="using-ajax-control-toolkit-control-extenders"></a>使用 AJAX 控制項 Toolkit 控制項擴充項

AJAX Control Toolkit 也包含控制項擴充項。 正如其名，控制項擴充項會擴充現有控制項的功能。 例如，ConfirmButton 控制項擴充項會擴充標準的 ASP.NET 按鈕控制項。 擴充項變更按鈕控制項的行為，所以按鈕會顯示確認對話方塊中，當您按一下它。

控制項擴充項，就像 AJAX Control Toolkit 控制項需要 ScriptManager 控制項。 開始使用網頁中的控制項擴充項之前，您必須將 ScriptManager 控制項加入頁面。

請遵循下列步驟使用 ConfirmButton 控制項擴充項：

1. 建立新的 ASP.NET 網頁，名為 ShowConfirmButton.aspx
2. ScriptManager 將控制項加入至頁面控制項拖曳到 from beneath [AJAX 擴充功能] 索引標籤頁面。
3. 在工具箱 拖曳至設計工具介面拖曳 from beneath 標準索引標籤按鈕加入頁面標準的按鈕控制項。
4. 按一下**加入擴充項**工作選項 （請參閱圖 4）。
5. 在 選擇的擴充性 對話方塊中，選取 ConfirmButtonExtender （請參閱圖 5），然後按一下 確定 按鈕。
6. 在設計工具選取按鈕控制項，然後展開 Extender Button1\_ConfirmButtonExtender 節點，在 [屬性] 視窗中的 （請參閱圖 6）。 將值指派*真的？* ConfirmText 屬性。
7. 選取功能表選項執行頁面**偵錯，開始偵錯**或按 F5 鍵。


[![加入擴充項工作選項](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)

**圖 04**: 加入擴充項工作的選項 ([按一下以檢視完整大小的影像](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))


[![選取 ConfirmButton 控制項擴充項](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)

**圖 05**： 選取 ConfirmButton 控制項擴充項 ([按一下以檢視完整大小的影像](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))


[![設定 ConfirmButton 屬性](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)

**圖 06**: ConfirmButton 屬性設定 ([按一下以檢視完整大小的影像](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))


當頁面開啟時，您應該會看到一個按鈕。 當您按一下按鈕時，您會取得濆迶 7 的確認對話方塊。


[![顯示 [確認] 對話方塊](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)

**圖 07**： 顯示 [確認] 對話方塊 ([按一下以檢視完整大小的影像](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))


請注意，您通常請勿拖曳控制項擴充項拖曳到頁面。 相反地，您使用**加入擴充項**工作選項，將擴充項加入至已加入至網頁的控制項。 此外，請注意，您將控制項擴充項屬性開啟要擴充控制項的屬性工作表。

在單一 ASP.NET 控制項可以由多個控制項擴充項擴充。 擴充控制項的屬性工作表會列出所有與控制項關聯的控制項擴充項。

> [!div class="step-by-step"]
> [上一頁](get-started-with-the-ajax-control-toolkit-cs.md)
> [下一頁](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
