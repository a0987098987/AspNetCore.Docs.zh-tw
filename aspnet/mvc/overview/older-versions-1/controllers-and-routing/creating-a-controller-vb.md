---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: "建立控制站 (VB) |Microsoft 文件"
author: StephenWalther
description: "在本教學課程中，作者： Stephen Walther 會示範如何將控制站新增至 ASP.NET MVC 應用程式。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: d2caf7fe137b48c016ff3cd52db9e36e1e8001c0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-controller-vb"></a>建立控制站 (VB)
====================
由[Stephen Walther](https://github.com/StephenWalther)

> 在本教學課程中，作者： Stephen Walther 會示範如何將控制站新增至 ASP.NET MVC 應用程式。


本教學課程的目標是以說明如何建立新的 ASP.NET MVC 控制站。 您了解如何使用 Visual Studio 加入控制器 功能表選項，藉由建立類別檔案，以手動方式建立控制站。

### <a name="using-the-add-controller-menu-option"></a>使用新增控制器功能表選項

若要建立新的控制站最簡單方式是在 Visual Studio 方案總管 視窗中的 控制器 資料夾上按一下滑鼠右鍵，然後選取**新增、 控制站**功能表選項 （請參閱圖 1）。 選取此功能表選項會開啟**加入控制器**對話方塊 （請參閱圖 2）。


[![新增專案 對話方塊](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)

**圖 01**： 加入新的控制站 ([按一下以檢視完整大小的影像](creating-a-controller-vb/_static/image2.png))


[![新增專案 對話方塊](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)

**圖 02**: 加入控制器 對話方塊 ([按一下以檢視完整大小的影像](creating-a-controller-vb/_static/image4.png))


請注意，控制器名稱的第一個部分會反白顯示**加入控制器**對話方塊。 每個控制站名稱必須以後置字元結尾*控制器*。 例如，您可以在其中建立名為控制器*ProductController*但不是將名為控制器*產品*。


如果您建立遺漏的控制站*控制器*後置詞，則您將無法叫用控制器。 不要執行這個動作--我所浪費我生命週期的無數小時後進行這種錯誤。


**列出 1-Controllers\ProductController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

在 [控制器] 資料夾中，您應該一律建立控制器。 否則，會違反您的 ASP.NET MVC 的慣例，其他開發人員必須了解您的應用程式更難。

### <a name="scaffolding-action-methods"></a>Scaffolding 動作方法

當您建立一個控制站時，您可以選擇自動產生建立、 更新和詳細資料的動作方法 （請參閱圖 3）。 如果您選取此選項會產生清單 2 控制器類別。


[![自動建立動作方法](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)

**圖 03**： 建立動作方法會自動 ([按一下以檢視完整大小的影像](creating-a-controller-vb/_static/image6.png))


**列出 2-Controllers\CustomerController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

這些產生的方法都是虛設常式方法。 您必須將建立、 更新和顯示詳細資料的客戶自己的實際邏輯。 但是，虛設常式方法會將您提供不錯的起點。

### <a name="creating-a-controller-class"></a>建立控制器類別

ASP.NET MVC 控制器是只在類別。 如果您想要的話，您可以略過方便的 Visual Studio 控制器 scaffolding，並以手動方式建立控制器類別。 請依照下列步驟：

1. 以滑鼠右鍵按一下 [控制器] 資料夾，然後選取功能表選項**新增]、 [新項目**選取**類別**範本 （請參閱圖 4）。
2. 將新類別 PersonController.vb，然後按一下**新增** 按鈕。
3. 修改產生的類別檔案，使該類別繼承自基底 System.Web.Mvc.Controller 類別 （請參閱列出 3）。


[![建立新的類別](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)

**圖 04**： 建立新的類別 ([按一下以檢視完整大小的影像](creating-a-controller-vb/_static/image8.png))


**列出 3-Controllers\PersonController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

列出的 3 中的控制站會公開一個名為 index （） 會傳回字串"Hello World ！"的動作。 您可以叫用此控制器動作，以執行您的應用程式，並要求的 URL 如下所示：

`http://localhost:40071/Person`

> [!NOTE] 
> 
> ASP.NET 程式開發伺服器會使用隨機的連接埠號碼 (例如，40071)。 輸入時要叫用控制器的 URL，您必須提供正確的連接埠號碼。 ASP.NET 程式開發伺服器，Windows 通知區域 （右下方的螢幕） 中將滑鼠停留在圖示，您可以判斷通訊埠編號。

>[!div class="step-by-step"]
[上一頁](adding-dynamic-content-to-a-cached-page-vb.md)
[下一頁](creating-an-action-vb.md)
