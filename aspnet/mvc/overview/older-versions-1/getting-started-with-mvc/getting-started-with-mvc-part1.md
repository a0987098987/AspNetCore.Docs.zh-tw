---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: ASP.NET MVC 簡介 |Microsoft Docs
author: shanselman
description: 這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。 建立簡單的 web 應用程式，從資料庫讀取與寫入。
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: ee39cf78835767d73bd9a1d4c9a2c4ca75aca5fa
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808858"
---
<a name="intro-to-aspnet-mvc"></a>ASP.NET MVC 簡介
====================
藉由[Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > 如果本教學課程中可用的更新的版本[此處](../../getting-started/introduction/getting-started.md)使用[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)。 新的教學課程會使用 ASP.NET MVC 5，透過本教學課程提供許多增強功能。
> 
> 
> 這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。 您將建立簡單 web 應用程式，從資料庫讀取與寫入。 請瀏覽[ASP.NET MVC 學習中心](../../../index.md)來尋找其他 ASP.NET MVC 教學課程和範例。


讓我們第一個 ASP.NET MVC Web 應用程式會使用[Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/)。 我們要讓一些電影清單應用程式，讓我們將建立並列出電影。

## <a name="what-youll-build"></a>您將建置

以下是兩個螢幕擷取畫面，您將建置的應用程式。 您必須具有各種不同的資料行的電影的簡單的資料表。

[![電影清單-Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

而且您必須建立表單讓我們可以將影片新增至清單。

[![建立電影-Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>您將學習到的技能

本教學課程將教導您建置使用 Visual Studio ASP.NET MVC Web 應用程式的基本概念。 您將了解：

- 如何建立新的 ASP.NET MVC 專案
- 如何使用 SQL Server 中建立新的資料庫
- 如何建立 ASP.NET MVC 控制器和檢視
- 如何擷取及顯示資料
- 如何編輯資料，並啟用資料驗證
- 如何更新資料庫結構描述

## <a name="get-started"></a>開始使用

從 [開始] 畫面執行 Visual Web Developer 2010 Express （我稱它 「 VWD"從現在起），然後選取新的專案啟動。

Visual Web Developer 是 IDE 或整合式開發環境。 就像您使用 Microsoft Word 來撰寫文件時，您將使用 IDE 來建立應用程式。 沒有顯示各種可用的選項，以及您也可以使用選取的檔案 功能表頂端的工具列 |新的專案。

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>建立第一個應用程式

您可以建立使用 Visual Basic 或 Visual C# 應用程式。 現在，選取 Visual C# 在左邊，然後選取 「 ASP.NET MVC 2 Web 應用程式 」。 您的專案命名為 「 Movies 」，然後按一下 [確定]。

[![新的專案](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

右邊是顯示在您的應用程式中的所有檔案和資料夾的 [方案總管]。 中間的大型視窗是時間的在您編輯您的程式碼和大部分。 Visual Studio 使用您剛才建立的 ASP.NET MVC 專案預設範本，因此您有運作中應用程式現在無須執行任何動作 ！ 這是簡單"Hello World ！ 專案和它是適合我們的應用程式啟動。

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

選取 「 播放 」 按鈕加入工具列。

![開始偵錯](getting-started-with-mvc-part1/_static/image11.png)

它是指向右側將編譯您的程式，並在網頁瀏覽器中啟動您的應用程式的綠色箭號。

*注意： 您可以改為在鍵盤上按下 F5 或選取 偵錯-&gt;從 偵錯 功能表啟動偵錯。*

這會導致 Visual Web Developer，以啟動開發 web 伺服器並執行我們的 web 應用程式 （沒有任何組態或啟用此功能所需的手動步驟）。 然後它就會啟動瀏覽器，並將它設定為瀏覽應用程式的首頁。 請注意下方，瀏覽器的網址列中顯示"localhost"，並不像 example.com。 這是因為 localhost 永遠會指向您自己的本機電腦-在此情況下執行我們剛建立的應用程式。

[![首頁](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

根據預設，這個預設範本會提供您兩個頁面來瀏覽和基本的登入頁面。 讓我們變更此應用程式的運作方式，並在程序有點了解 ASP.NET MVC。 關閉瀏覽器，並讓變更一些程式碼。

> [!div class="step-by-step"]
> [下一步](getting-started-with-mvc-part2.md)
