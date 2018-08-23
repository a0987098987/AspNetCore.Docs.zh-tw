---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: 從控制器存取模型的資料 |Microsoft Docs
author: shanselman
description: 這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。 建立簡單的 web 應用程式，從資料庫讀取與寫入。
ms.author: riande
ms.date: 08/14/2010
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: 76dc324134dc93c9552741fea9f1136abdc9184a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830780"
---
<a name="accessing-your-models-data-from-a-controller"></a>從控制器存取模型資料
====================
藉由[Scott Hanselman](https://github.com/shanselman)

> 這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。 您將建立簡單 web 應用程式，從資料庫讀取與寫入。 請瀏覽[ASP.NET MVC 學習中心](../../../index.md)來尋找其他 ASP.NET MVC 教學課程和範例。


這一節我們要建立新的 MoviesController 類別，並撰寫一些程式碼會擷取電影資料，並顯示回瀏覽器使用檢視範本。

以滑鼠右鍵按一下 [控制器] 資料夾，並讓新 MoviesController。

[![新增控制器](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

這會建立新的 「 MoviesController.cs"檔案，我們的專案中我們 \Controllers 資料夾下。 讓我們更新 MovieController 從我們的新填入資料庫中擷取的電影清單。

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

因此，我們只會擷取 1984 年的夏季之後發行的影片，我們要執行 LINQ 查詢。 我們將需要呈現的電影傳回這份清單，因此在方法中以滑鼠右鍵按一下並選取新增的檢視，來建立它的檢視範本。

[新增檢視] 對話方塊中我們將指出，我們會將清單傳遞&lt;Movies.Models.Movie&gt;檢視範本。 不同於前一次，我們使用 [新增檢視] 對話方塊，並選擇建立 「 空白 」 範本時，我們會指出這次我們要 Visual Studio 自動"scaffold 」 檢視範本的某些預設的內容。 將執行此作業，選取 「 清單 」 中的項目 」 檢視內容下拉式功能表。

請記住，如果您已建立新的類別，您必須編譯您的應用程式，它才會顯示在 [新增檢視] 對話方塊。

![新增檢視](getting-started-with-mvc-part5/_static/image3.png)

按一下 [新增]，系統會自動產生的程式碼以檢視我們會顯示我們的電影清單。 這是要變更的好時機&lt;h2&gt;像我們稍早使用 Hello World 的檢視，標題為類似 「 我的電影清單 」。

[![影片-Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

執行您的應用程式，並請 /Movies 瀏覽的網址列中。 現在我們使用控制器內的基本查詢從資料庫擷取資料，並回到知道電影的檢視中的資料。 該檢視會透過的電影清單，然後為我們建立資料的資料表。

[![電影清單-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

我們將不會實作編輯、 詳細資料和刪除功能，與此應用程式-因此我們不需要預設 scaffold 樣板為我們建立的連結。 開啟 /Movies/Index.aspx 檔案，並將它們移除。

以下是我們已更新的檢視範本外觀應該為何我們進行這些變更後的原始程式碼：

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

我們會將其刪除此範例中，它會建立我們就不需要的連結。 不過，因為這是下一步，我們會保留我們新建立的連結 ！ 以下是我們的應用程式與已移除該資料行的外觀。

[![電影清單-Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

我們現在有簡單的電影資料清單。 不過，如果我們按一下 [新建] 連結時，我們將會收到錯誤，因為它不連結 ！ 讓我們實作建立動作方法，並讓使用者在資料庫中，輸入新的電影。

> [!div class="step-by-step"]
> [上一頁](getting-started-with-mvc-part4.md)
> [下一頁](getting-started-with-mvc-part6.md)
