---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: ASP.NET MVC 簡介 |Microsoft 文件
author: shanselman
description: 這是初學者教學課程介紹基本概念的 ASP.NET MVC。 建立簡單的 web 應用程式可讀取和寫入資料庫中。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 476d832e389b9b5a26fe2d552ca648c79b100056
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2018
---
<a name="intro-to-aspnet-mvc"></a>ASP.NET MVC 簡介
====================
由[Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > 如果本教學課程中可用的更新的版本[這裡](../../getting-started/introduction/getting-started.md)使用[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)。 新的教學課程會使用 ASP.NET MVC 5，本教學課程提供許多改良。
> 
> 
> 這是初學者教學課程介紹基本概念的 ASP.NET MVC。 您將建立簡單的 web 應用程式可讀取和寫入資料庫中。 請瀏覽[ASP.NET MVC 學習中心](../../../index.md)教學課程和範例，請尋找其他 ASP.NET MVC。


讓我們進行我們第一個 ASP.NET MVC Web 應用程式使用[Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/)。 我們要極小的電影清單應用程式，讓我們將建立並列出電影。

## <a name="what-youll-build"></a>您將建置

以下是您將建置的應用程式的兩個螢幕擷取畫面。 您必須具有不同的資料行的電影的簡單的資料表。

[![電影清單-Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

而且您必須建立表單，我們可以將電影新增至清單。

[![建立電影-Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>您將學習到的技術

本教學課程將告訴您建置 ASP.NET MVC Web 應用程式使用 Visual Studio 的基本概念。 您將學習：

- 如何建立新的 ASP.NET MVC 專案
- 如何建立新的資料庫與 SQL Server
- 如何建立 ASP.NET MVC 控制器和檢視
- 如何擷取及顯示資料
- 如何編輯資料，並啟用資料驗證
- 如何更新資料庫結構描述

## <a name="get-started"></a>開始使用

執行 Visual Web Developer 2010 Express （我稱它為 「 VWD 」 從現在起），然後選取新的專案，從 [開始] 畫面啟動。

Visual Web Developer 是 IDE 或整合式開發環境。 就像您使用 Microsoft Word 來寫入的文件時，您將使用 IDE 來建立應用程式。 沒有顯示各種可用的選項，以及您可能也使用來選取檔案 功能表頂端的工具列 |新的專案。

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>建立第一個應用程式

您可以建立使用 Visual Basic 或 Visual C# 應用程式。 現在，選取 Visual C# 在左邊，然後挑選 「 ASP.NET MVC 2 Web 應用程式 」。 「 電影 」 命名您的專案，然後按一下 [確定]。

[![新的專案](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

右邊是顯示在您的應用程式中的所有檔案和資料夾的 [方案總管]。 中間的大型視窗是時間的您編輯的程式碼並的支出最多。 Visual Studio 使用您剛才建立的 ASP.NET MVC 專案的預設範本，因此您有可用的應用程式立即而不做任何事 ！ 這是簡單"Hello World ！ 專案和它是我們的應用程式啟動的好地方。

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

選取 「 播放 」 按鈕，在工具列。

![開始偵錯](getting-started-with-mvc-part1/_static/image11.png)

它是指向會編譯您的程式，並在網頁瀏覽器中啟動您的應用程式的權限的綠色箭頭。

*注意： 您可以改為在鍵盤上按 F5 或選取偵錯-&gt;從 [偵錯] 功能表開始偵錯。*

這會導致 Visual Web Developer 中啟動開發 web 伺服器和執行 web 應用程式 （沒有任何組態或手動啟用此功能所需的步驟）。 它接著啟動瀏覽器，並將它設定為瀏覽應用程式的首頁。 請注意下方，瀏覽器的網址列中顯示"localhost"，並不像 example.com。這是因為 localhost 一律會指向您自己的本機電腦-在此情況下執行我們剛才建置的應用程式。

[![首頁](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

現成提供的這個預設範本提供您兩個頁面瀏覽和基本登入頁面。 讓我們來變更這個應用程式的運作方式和程序中更了解 ASP.NET MVC。 關閉您的瀏覽器，並可讓您變更一些程式碼。

> [!div class="step-by-step"]
> [下一步](getting-started-with-mvc-part2.md)
