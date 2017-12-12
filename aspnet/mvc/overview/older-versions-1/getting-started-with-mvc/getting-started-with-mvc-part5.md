---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: "存取您的模型資料從控制器 |Microsoft 文件"
author: shanselman
description: "這是初學者教學課程介紹基本概念的 ASP.NET MVC。 您將建立簡單的 web 應用程式可讀取和寫入資料庫中。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 1a733accabcd10409f5611c31001397e97533fb6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="accessing-your-models-data-from-a-controller"></a>從控制器存取您的模型資料
====================
由[Scott Hanselman](https://github.com/shanselman)

> 這是初學者教學課程介紹基本概念的 ASP.NET MVC。 您將建立簡單的 web 應用程式可讀取和寫入資料庫中。 請瀏覽[ASP.NET MVC 學習中心](../../../index.md)教學課程和範例，請尋找其他 ASP.NET MVC。


本節中，我們會建立新的 MoviesController 類別，並撰寫一些程式碼會擷取我們電影資料並顯示它回到使用檢視範本的瀏覽器。

以滑鼠右鍵按一下 [控制器] 資料夾，並讓新 MoviesController。

[![加入控制器](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

這會建立新的 「 MoviesController.cs"檔案內受測專案我們 \Controllers 資料夾下。 讓我們來更新 MovieController 從我們的新填入資料庫擷取的電影清單。

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

我們正在執行 LINQ 查詢，好讓我們只擷取 1984年的夏天之後發行的電影。 我們需要呈現的電影回此清單，因此在方法中以滑鼠右鍵按一下，然後選取 加入檢視，來建立它的檢視範本。

[新增檢視] 對話方塊中我們將會指出我們會將清單傳遞&lt;Movies.Models.Movie&gt;我們檢視範本。 不同於前一次，我們使用 [新增檢視] 對話方塊，並選擇要建立的 「 空白 」 範本時，我們將會指出這次我們要 Visual Studio 自動"scaffold 」 檢視範本為我們具有某些預設內容。 將執行此作業，選取 「 清單 」 中的項目 」 檢視內容下拉式功能表。

請記住，當您有一個建立新的類別，您將需要編譯您的應用程式，讓它顯示在 [新增檢視] 對話方塊。

![加入檢視](getting-started-with-mvc-part5/_static/image3.png)

按一下 [新增]，系統會自動產生程式碼檢視為我們會顯示我們的電影清單。 這是變更的好時機&lt;h2&gt;標題為類似 「 我的電影清單 」，如同我們稍早與 Hello World 檢視。

[![影片-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

執行您的應用程式，並請 /Movies 瀏覽的網址列中。 現在我們已使用內部控制器的基本查詢從資料庫擷取資料，並傳回的資料到電影所知道的檢視。 該檢視微調透過的電影清單，然後我們建立資料的資料表。

[![電影清單-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

我們將不會實作與這個應用程式的編輯，詳細資料與刪除功能所以我們不需要為我們 scaffold 範本建立的預設連結。 開啟 /Movies/Index.aspx 檔案，並將它們移除。

以下是我們已更新的檢視表範本外觀應該為何一旦我們進行這些變更的程式碼：

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

我們會將其刪除此範例中，它建立我們不需要的連結。 不過，因為這是下一步，我們會保留我們建立新的連結 ！ 以下是我們的應用程式與移除該資料行的外觀。

[![電影清單-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

我們現在有我們電影資料的簡單清單。 不過，如果我們按一下 「 建立新 」 的連結，我們會先出現的錯誤，因為它不會接到 ！ 讓我們實作建立動作方法，並且讓使用者在資料庫中輸入新的電影。

>[!div class="step-by-step"]
[上一頁](getting-started-with-mvc-part4.md)
[下一頁](getting-started-with-mvc-part6.md)
