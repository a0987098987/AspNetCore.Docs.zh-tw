---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
title: 部署資料庫 (C#) |Microsoft 文件
author: rick-anderson
description: ASP.NET web 應用程式部署需要取得必要的檔案和資源從開發環境到實際執行環境。 針對 da...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: ff537a10-9f1f-43fe-9bcb-3dda161ba8f5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 203bf64da887f31e5f0727fc57173d6a573095da
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="deploying-a-database-c"></a>部署資料庫 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_CS.zip)或[下載 PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_cs.pdf)

> ASP.NET web 應用程式部署需要取得必要的檔案和資源從開發環境到實際執行環境。 資料驅動的 web 應用程式包括資料庫結構描述和資料。 本教學課程是探索成功部署到生產環境資料庫從開發環境所需的步驟一系列中的第一個。


## <a name="introduction"></a>簡介

ASP.NET web 應用程式部署需要取得必要的檔案和資源從開發環境到實際執行環境。 我們探討了部署簡單的活頁簿檢閱過去六個教學課程的過程中，web 應用程式。 此示範站台所組成的數字的伺服器端資源-ASP.NET 網頁、 組態檔，`Web.sitemap`檔案和等位用戶端部署資源，例如映像和 CSS 檔案。 但呢資料驅動的 web 應用程式嗎？ 部署 web 應用程式會使用的資料庫，必須採取何種額外的步驟？

透過 下一步的幾個教學課程中，我們會解決資料驅動的 web 應用程式部署所需的步驟。 本教學課程一開始會檢查如何取得資料庫 s 結構描述和內容從開發環境到實際執行環境，而後續的教學課程會查看所需的設定變更。 之後，我們將探討挑戰的部署使用應用程式服務 （成員資格、 角色、 設定檔，等等） 的資料庫。

## <a name="examining-the-updated-book-reviews-web-application"></a>正在檢查更新的活頁簿檢閱 Web 應用程式

為了示範部署資料驅動的 web 應用程式中，我已更新活頁簿檢閱 web 應用程式從簡單的靜態網站資料驅動的一個。 因為之前，有兩個版本的應用程式在此教學課程的下載中： 使用 Web 應用程式專案模型，另一個使用的網站專案模型。

更新的活頁簿檢閱 web 應用程式會使用[SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx)資料庫，儲存在站台 s`App_Data`資料夾 (`~/App_Data/Reviews.mdf`)。 如果您有在電腦上安裝 SQL Server 2008 示範應該不會發生錯誤。 如果您有較舊版本的 SQL Server，您可以安裝免費 SQL Server 2008 Express Edition，或您可以使用指令碼可在本教學課程 s 下載自行建立資料庫的資料庫。

`Reviews.mdf`資料庫包含四個資料表：

- `Genres` -包含每個內容類型，例如技術、 小說，以及商務的記錄。
- `Books` -包含每個檢閱，並具有類似的資料行的記錄`Title`， `GenreId`， `ReviewDate`，和`Review`，和其他項目。
- `Authors` -包含每個投稿檢閱書籍的作者相關資訊。
- `BooksAuthors` -指定哪些作者撰寫哪些書籍的多對多聯結資料表。
  

圖 1 顯示這些四份資料表的 ER 圖表。


[![活頁簿檢閱 Web 應用程式資料庫是四個資料表所組成](deploying-a-database-cs/_static/image2.jpg)](deploying-a-database-cs/_static/image1.jpg) 

**圖 1**: 書籍檢閱 Web 應用程式資料庫是四個資料表所組成 ([按一下以檢視完整大小的影像](deploying-a-database-cs/_static/image3.jpg))


舊版的活頁簿檢閱網站有個別的 ASP.NET 頁面的每本書籍。 例如，沒有名為的頁面`~/Tech/TYASP35.aspx`包含檢閱*教導您自己 ASP.NET 3.5 24 小時內*。 這個新資料驅動型版本的網站已儲存在資料庫和單一 ASP.NET 網頁，Review.aspx?ID= 檢閱*[bookid]*，這會顯示指定的書籍的檢閱。 同樣地，沒有 Genre.aspx?ID=*genreId*頁面，其中列出檢閱的書籍，在指定的內容類型。

圖 2 和 3 顯示`Genre.aspx`和`Review.aspx`動作中的頁面。 請注意每個頁面的 [網址] 列中的 URL。 在圖 2 it s Genre.aspx？ ID = 85d164ba 1123年 4 c 47-82a0-c8ec75de7e0e。 因為 85d164ba-1123-4c47-82a0-c8ec75de7e0e`GenreId`技術內容類型、"檢閱技術 」 頁面的標題讀取和項目符號清單的值會列舉這些檢閱站台上的，落在此內容類型。


[![技術內容類型頁面](deploying-a-database-cs/_static/image5.jpg)](deploying-a-database-cs/_static/image4.jpg) 

**圖 2**: 技術內容類型頁面 ([按一下以檢視完整大小的影像](deploying-a-database-cs/_static/image6.jpg))


[![檢閱教導您自己 ASP.NET 3.5 在 24 小時](deploying-a-database-cs/_static/image8.jpg)](deploying-a-database-cs/_static/image7.jpg) 

**圖 3**: 檢閱*教導您自己 ASP.NET 3.5 24 小時內*([按一下以檢視完整大小的影像](deploying-a-database-cs/_static/image9.jpg))


活頁簿會檢閱 web 應用程式也包含讓系統管理員可以新增、 編輯和刪除內容類型，檢閱並作者資訊的系統管理區段。 目前，任何訪客可以存取管理 區段中。 在未來的教學課程中我們將新增使用者帳戶的支援，並只允許授權的使用者至 [管理] 頁面。

如果您要下載活頁簿檢閱應用程式請切記，其目的是要示範如何將資料導向應用程式部署。 它不會呈現延展應用程式的設計的最佳作法。 例如，沒有任何個別的資料存取層 (DAL);ASP.NET 網頁直接通訊透過 SqlDataSource 控制項或其程式碼後置類別中的 ADO.NET 程式碼的資料庫。 建立資料驅動的應用程式使用階層式的架構更深入了解，請參閱我[*使用資料*教學課程](../../data-access/index.md)。

## <a name="databases-on-development-versus-production"></a>在開發與實際執行的資料庫

當您著手開發資料導向的 web 應用程式上時，您必須指定資料庫連接字串，其中提供有關如何連接到資料庫的應用程式詳細資料。 指定此連接字串之間，其他項目、 資料庫伺服器、 資料庫名稱和安全性資訊。 通常，在開發期間的應用程式所使用的資料庫是不同於資料庫時使用它在生產環境中的 s。 有許多優點的開發和生產環境使用不同的資料庫。 有不同，資料庫開發表示不要不必擔心不慎修改或刪除即時資料。 它也可讓您將放在空測試資料，或對資料模型中的重大變更，而不必擔心對應用程式在生產環境中產生的影響。 缺點是需要不同的資料庫開發和生產環境中，當應用程式部署到資料庫 s 結構描述的資料庫和相關的任何變更，或也必須部署的資料。

第一次在部署之前，沒有在資料庫中，只有一個執行個體，該執行個體是在開發環境中。 當應用程式部署至生產環境適用於第一次我們必須將必要的伺服器端和用戶端檔案，複製不僅也將從開發環境的資料庫複製到生產環境。 這是其中我們獨立右現在與活頁簿檢閱 web 應用程式中的資料庫會位於`App_Data`不在我們的開發環境中的資料夾但有尚未被推入到生產環境。

一旦在部署應用程式有兩個資料庫的複本。 成熟的應用程式，因為新的功能可能會增加，必要的資料模型的變更 （例如將新的資料行加入至現有的資料表，在現有的資料行，加入新的資料表、 變更等等）。 接下來部署 web 應用程式時，所做的變更套用至開發環境中的資料庫必須套用的最後一個部署到實際執行資料庫。 未來的教學課程中討論的一些策略，來管理此程序。 本教學課程著重在將整個資料庫從開發環境至實際執行環境部署。

## <a name="deploying-the-database-to-the-production-environment"></a>將資料庫部署到生產環境

本教學課程的其餘部分會查看如何部署到生產環境資料庫從開發環境。 如果您是按照您需要先確定您的帳戶與您的 web 主機提供者包含 Microsoft SQL Server 資料庫支援。 您還需要有一些資訊同時角度來看，也就是資料庫伺服器名稱、 資料庫名稱，以及使用者名稱和密碼用來連接到資料庫。

如稍早在本教學課程中所述，活頁簿檢閱網站的資料庫是儲存在 SQL Server 2008 Express Edition 資料庫`App_Data`資料夾。 它會方面的原因，部署這類資料庫會是只要複製`App_Data`從開發環境到實際執行環境的資料夾。 然而，大部分的 web 主機提供者不支援在裝載資料庫`App_Data`資料夾，因為安全性考量。 相反地，web 主機提供的帳戶在其環境中的 SQL Server 資料庫伺服器上。 將資料庫從開發環境到生產環境部署需要取得您註冊您的 web 主機的資料庫伺服器上的資料庫。

如何取得您的資料庫從開發環境到實際執行環境？ 有幾種方式可以完成這項作業，視何種服務您的 web 主機提供。 您可以使用某些主機，例如 DiscountASP.NET，FTP 資料庫或實際的備份`.mdf`檔案到您的網站，然後，從 [控制台] 中還原備份的檔案或附加`.mdf`SQL Server 資料庫伺服器的檔案。 這類工具以將資料庫部署很簡單，只複製`App_Data`至生產環境，然後將它連接透過 [控制台] 資料夾。 這可能是第一次發行資料庫的最快速簡便方式。

另一種方法是使用資料庫發行精靈。 資料庫發行精靈 」 是 Windows 桌面應用程式將會產生其資料表中建立資料庫 s 結構描述-資料表、 預存程序、 檢視、 使用者定義函式和等位，並選擇性地將資料的 SQL 命令。 您可以再連接到 web 主機提供者的資料庫伺服器透過 SQL Server Management Studio，然後執行 複製實際執行資料庫指令碼。 更好，如果您的 web 主機提供者支援 Microsoft s [Database Publishing Services](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home)可以自動代表您的資料庫伺服器上執行的資料庫發行精靈所產生的指令碼。 資料庫發行精靈產生的指令碼，建立資料庫 s 結構描述和資料，因為它會使用不論您的 web 主機提供者是否提供功能，例如將上傳`.mdf`檔案。

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>產生 SQL 命令來建立資料庫結構描述和資料使用資料庫發行精靈

可讓 s 逐步解說將書籍檢閱資料庫部署到生產環境中使用資料庫發行精靈。 如果您使用 Visual Studio 2008 或更新版本，已安裝資料庫發行精靈。 如果您使用 Visual Studio 2005，則您必須先[下載並安裝](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)精靈。

開啟 Visual Studio，並瀏覽至`Reviews.mdf`資料庫。 如果您使用 Visual Web Developer 中，請移至 [資料庫總管] 中;如果您使用 Visual Studio，請使用 [伺服器總管] 中。 圖 4 顯示`Reviews.mdf`在 Visual Web Developer 的 [資料庫總管] 中的資料庫。 如圖 4 所示，`Reviews.mdf`資料庫由四個資料表、 三個預存程序，和使用者定義函式所組成。


[![在 [資料庫總管] 或伺服器總管中尋找資料庫](deploying-a-database-cs/_static/image11.jpg)](deploying-a-database-cs/_static/image10.jpg) 

**圖 4**: [資料庫總管] 或伺服器總管中找出資料庫 ([按一下以檢視完整大小的影像](deploying-a-database-cs/_static/image12.jpg))


以滑鼠右鍵按一下資料庫名稱，並從內容功能表中選擇 [發佈到提供者] 選項。 這會啟動 「 資料庫發行精靈 」 （請參閱圖 5）。 按一下旁邊提前過去的啟動顯示畫面。


[![資料庫發行精靈啟動顯示畫面](deploying-a-database-cs/_static/image14.jpg)](deploying-a-database-cs/_static/image13.jpg) 

**圖 5**: 資料庫發行精靈啟動顯示畫面 ([按一下以檢視完整大小的影像](deploying-a-database-cs/_static/image15.jpg))


在精靈的第二個畫面中列出的資料庫發行精靈 」 可存取的資料庫，並讓您選擇要選取的資料庫中的所有物件編寫指令碼，或是選擇哪些物件要編寫指令碼。 選取適當的資料庫，並將 [物件選取的資料庫中的所有指令碼] 選項處於選取狀態。

> [!NOTE]
> 如果您收到錯誤 「 資料庫中沒有任何物件*databaseName*此精靈可編寫指令碼的類型 」 時圖 6 所示的畫面中，按一下 [下一步]，請確定您的資料庫檔案的路徑不是過長。 它已找到，是否資料庫檔案的路徑太長，可能會發生此錯誤。


[![資料庫發行精靈啟動顯示畫面](deploying-a-database-cs/_static/image17.jpg)](deploying-a-database-cs/_static/image16.jpg) 

**圖 6**: 資料庫發行精靈啟動顯示畫面 ([按一下以檢視完整大小的影像](deploying-a-database-cs/_static/image18.jpg))


從下一個畫面中您可以產生指令碼檔案，或是 web 主機可支援此功能，如果資料庫直接發佈到 web 主機提供者的資料庫伺服器。 如圖 7 所示，無法寫入檔案的指令碼`C:\REVIEWS.MDF.sql`。


[![編寫資料庫備份至檔案，或直接發行至您的 Web 主機提供者](deploying-a-database-cs/_static/image20.jpg)](deploying-a-database-cs/_static/image19.jpg) 

**圖 7**： 編寫資料庫備份至檔案，或直接發行至您的 Web 主機提供者 ([按一下以檢視完整大小的影像](deploying-a-database-cs/_static/image21.jpg))


後續畫面提示您針對各種不同的指令碼選項。 您可以指定指令碼是否應包含 drop 陳述式來移除這些現有的物件。 預設值為 True，當第一次部署資料庫時，這也沒有問題。 您也可以指定目標資料庫是 SQL Server 2000、 SQL Server 2005 或 SQL Server 2008。 最後，您可以指出是否編寫指令碼的結構描述和資料，只有資料或只有結構描述。 結構描述是資料庫物件、 資料表、 預存程序、 檢視和等等的集合。 資料是位於資料表中的資訊。

如圖 8 所示，我發生了精靈設定為卸除現有的資料庫物件，來產生指令碼的 SQL Server 2008 資料庫，並將發行的結構描述和資料。


[![指定發行選項](deploying-a-database-cs/_static/image23.jpg)](deploying-a-database-cs/_static/image22.jpg) 

**圖 8**： 指定發行選項 ([按一下以檢視完整大小的影像](deploying-a-database-cs/_static/image24.jpg))


最後兩個畫面摘要即將進行並顯示狀態的指令碼的動作。 執行精靈的最後結果就是我們有包含實際執行上建立資料庫，並使用相同的資料填入上開發所需的 SQL 命令的指令碼檔案。

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>在生產環境資料庫上執行的 SQL 命令

現在我們有指令碼，其中包含要建立所有資料庫和其資料的 SQL 命令，所保留下來就是在生產資料庫上執行指令碼。 有些 web 主機提供者提供的文字方塊中，您可以在其中輸入您的資料庫上執行的 SQL 命令及其控制台中。 如果您有非常大的指令碼檔案，則此選項可能無法運作 (`REVIEWS.MDF.sql`指令碼檔案是超過 425 KB 的大小，例如)。

更好的方法是直接連接到實際執行資料庫伺服器使用 SQL Server Management Studio (SSMS)。 如果您有非 Express Edition 的 SQL Server 電腦上安裝然後您應該已有安裝 SSMS。 否則，您可以[下載並安裝](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)一份免費的 SQL Server Management Studio Express 的版本。

啟動 SSMS 並連接到 web 主機的資料庫伺服器使用 web 主機提供者所提供的資訊。


[![連接到 Web 主機提供者的資料庫伺服器](deploying-a-database-cs/_static/image26.jpg)](deploying-a-database-cs/_static/image25.jpg) 

**圖 9**： 連接到您的 Web 主機提供者 s 資料庫伺服器 ([按一下以檢視完整大小的影像](deploying-a-database-cs/_static/image27.jpg))


展開 [資料庫] 索引標籤，然後找出您的資料庫。 按一下工具列的左上角的 [新增查詢] 按鈕，貼上 SQL 命令，從資料庫發行精靈所建立的指令碼檔案中，按一下 [執行] 按鈕來實際執行資料庫伺服器上執行下列命令。 如果您的指令碼檔案是特別大可能需要幾分鐘才能執行命令。


[![連接到 Web 主機提供者的資料庫伺服器](deploying-a-database-cs/_static/image29.jpg)](deploying-a-database-cs/_static/image28.jpg) 

**圖 10**： 連接到您的 Web 主機提供者 s 資料庫伺服器 ([按一下以檢視完整大小的影像](deploying-a-database-cs/_static/image30.jpg))


S 都是這麼簡單 ！ 此時已經重複開發資料庫到生產環境。 如果您重新整理在 SSMS 中的資料庫應該會看到新的資料庫物件。 圖 11 顯示實際執行資料庫的資料表、 預存程序和使用者定義函數，其會鏡像上開發資料庫。 和實際執行資料庫的資料表我們指示資料庫發行精靈 」 將資料發行，因為在執行精靈時具有開發資料庫的資料表相同的資料。 圖 12 顯示中的資料`Books`生產資料庫上的資料表。


[![在生產資料庫上已有重複的資料庫物件](deploying-a-database-cs/_static/image32.jpg)](deploying-a-database-cs/_static/image31.jpg) 

**圖 11**: 資料庫物件已被重複的生產資料庫上 ([按一下以檢視完整大小的影像](deploying-a-database-cs/_static/image33.jpg))


[![實際執行資料庫包含相同資料做為開發資料庫](deploying-a-database-cs/_static/image35.jpg)](deploying-a-database-cs/_static/image34.jpg) 

**圖 12**： 實際執行資料庫開發資料庫上包含相同的資料 ([按一下以檢視完整大小的影像](deploying-a-database-cs/_static/image36.jpg))


此時我們只擁有開發資料庫部署到生產環境。 我們尚未有看部署 web 應用程式本身，或檢查實際執行應用程式使用的實際執行資料庫所需的組態變更。 在下一個教學課程中，我們將討論這些問題 ！

## <a name="summary"></a>總結

資料驅動的 web 應用程式部署需要複製到生產環境的開發過程中使用的資料庫。 許多 web 主機提供者會提供工具，可簡化部署資料庫的程序。 例如，與 DiscountASP.NET 您可以 FTP 資料庫`.mdf`檔案 （或備份），從 控制台，然後將資料庫附加至資料庫伺服器。 另一個選項，無論何種功能的運作方式您網站的主機提供者服務是 Microsoft 的資料庫發行精靈 工具，可產生的指令碼來建立開發資料庫 s 結構描述和資料的 SQL 命令。 一旦產生此指令碼可以在生產資料庫上執行。

既然書籍檢閱 web 應用程式的資料庫是實際執行，我們可以部署應用程式。 不過，web 應用程式的組態資訊指定連接字串至資料庫，而且該連接字串參考開發資料庫。 我們需要將網站部署到生產環境時，更新此連接字串資訊。 下一個教學課程會查看這些設定的差異，並逐步引導正式發行的資料導向書籍檢閱站台所需的步驟。

祝您程式設計 ！

#### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [下載 Microsoft SQL Server 資料庫發行精靈 1.1](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [下載 Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

> [!div class="step-by-step"]
> [上一頁](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
> [下一頁](configuring-the-production-web-application-to-use-the-production-database-cs.md)
