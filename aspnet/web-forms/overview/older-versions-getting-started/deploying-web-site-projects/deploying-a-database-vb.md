---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-vb
title: 部署資料庫 (VB) |Microsoft Docs
author: rick-anderson
description: 部署 ASP.NET web 應用程式需要取得必要的檔案和資源從開發環境到生產環境。 針對資料...
ms.author: aspnetcontent
ms.date: 04/23/2009
ms.assetid: 96ac3e69-04c7-4917-ad06-5f8968c3fbf1
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: c49b963cb5cfc40d8a0b03eb3ca722e3b789eab2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836271"
---
<a name="deploying-a-database-vb"></a>部署資料庫 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_VB.zip)或[下載 PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_vb.pdf)

> 部署 ASP.NET web 應用程式需要取得必要的檔案和資源從開發環境到生產環境。 資料驅動 web 應用程式，這包括資料庫結構描述和資料。 本教學課程是探索成功部署到生產環境的開發環境的資料庫所需的步驟一系列的第一個。


### <a name="introduction"></a>簡介

部署 ASP.NET web 應用程式需要取得必要的檔案和資源從開發環境到生產環境。 過去六個教學課程期間，我們討論過部署簡單的書籍評論 web 應用程式。 這個示範網站已包含數量的伺服器端資源-ASP.NET 網頁、 組態檔，`Web.sitemap`檔案及等項目-以及用戶端的資源，例如影像和 CSS 檔案。 但是，功能的相關資料導向 web 應用程式嗎？ 部署 web 應用程式使用資料庫，必須採取哪些額外的步驟？

透過下一步 的數個教學課程中，我們將會解決部署的資料驅動 web 應用程式所需的步驟。 本教學課程一開始會檢查如何取得資料庫 s 結構描述和內容從開發環境到生產環境中，而後續的教學課程探討所需的設定變更。 之後我們會探討部署資料庫使用的應用程式服務 （成員資格、 角色、 設定檔，等等） 的挑戰。

## <a name="examining-the-updated-book-reviews-web-application"></a>正在檢查更新的書籍評論 Web 應用程式

為了示範部署的資料驅動 web 應用程式中，我已更新的書籍評論 web 應用程式，從簡單的靜態網站資料驅動的一個。 因為之前，有兩個版本在此教學課程的下載的應用程式： 一個 Web 應用程式專案模型，另一個使用網站專案模型使用。

已更新的書籍評論 web 應用程式會使用[SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx)資料庫中，它會儲存在站台 s`App_Data`資料夾 (`~/App_Data/Reviews.mdf`)。 如果您有在電腦上安裝的 SQL Server 2008 示範應該會執行不會發生錯誤。 如果您有較舊版本的 SQL Server，您可以安裝免費 SQL Server 2008 Express Edition，或者您可以使用 s 本教學課程中，您可以使用指令碼下載到您自己建立資料庫的資料庫。

`Reviews.mdf`資料庫包含四個資料表：

- `Genres` -包含每個內容類型，例如技術、 故事，以及商務的記錄。
- `Books` -包含具有類似的資料行的每個檢閱的記錄`Title`， `GenreId`， `ReviewDate`，和`Review`，其他項目。
- `Authors` -包含每個已參與檢閱的書籍的作者的相關資訊。
- `BooksAuthors` -指定哪些作者撰寫哪些書籍的多對多聯結資料表。
  

圖 1 顯示 ER 圖表的這些四個資料表。


[![S 的書籍評論 Web 應用程式資料庫是包含的四個資料表](deploying-a-database-vb/_static/image2.jpg)](deploying-a-database-vb/_static/image1.jpg) 

**圖 1**: 的書籍評論 Web 應用程式資料庫是包含的四個資料表 ([按一下以檢視完整大小的影像](deploying-a-database-vb/_static/image3.jpg))


舊版的書籍評論網站有不同的 ASP.NET 網頁的每本書籍。 例如，沒有名為的頁面`~/Tech/TYASP35.aspx`所包含的檢閱*教導您自己 ASP.NET 3.5 24 小時內*。 網站的這個新資料導向版本有儲存在資料庫和單一 ASP.NET 網頁，Review.aspx?ID= 評論*Bookid>*，這會顯示指定的活頁簿的檢閱。 同樣地，沒有 Genre.aspx?ID=*genreId*頁面，其中列出所指定的內容類型中檢閱的書籍。

圖 2 和 3 的放映`Genre.aspx`和`Review.aspx`作用中的頁面。 請注意每個頁面的 [網址] 列中的 URL。 在 圖 2 it s Genre.aspx？ ID = 85d164ba 1123年 4 c 47-82a0-c8ec75de7e0e。 因為 85d164ba-1123-4c47-82a0-c8ec75de7e0e`GenreId`技術內容類型、 「 檢閱技術 」 頁面的標題讀取與項目符號清單值列舉落在此內容類型的站台上評論。


[![技術內容類型頁面](deploying-a-database-vb/_static/image5.jpg)](deploying-a-database-vb/_static/image4.jpg) 

**圖 2**: 技術內容類型頁面 ([按一下以檢視完整大小的影像](deploying-a-database-vb/_static/image6.jpg))


[![自學 ASP.NET 3.5 中 24 小時的檢閱](deploying-a-database-vb/_static/image8.jpg)](deploying-a-database-vb/_static/image7.jpg) 

**圖 3**: 檢閱*教導您自己 ASP.NET 3.5 24 小時內*([按一下以檢視完整大小的影像](deploying-a-database-vb/_static/image9.jpg))


活頁簿會檢閱 web 應用程式也包含管理 區段中，系統管理員可以新增、 編輯和刪除內容類型，檢閱和作者資訊。 目前，任何訪客可以存取 [管理] 區段。 在未來的教學課程中，我們將新增的使用者帳戶的支援，並只允許系統管理] 頁面的 [授權的使用者。

如果您要下載的書籍評論應用程式請記住，其目的是要示範如何將資料導向應用程式部署。 它不會呈現所使用的應用程式的設計而言的最佳作法。 例如，沒有任何個別的資料存取層 (DAL);ASP.NET 網頁直接通訊透過 SqlDataSource 控制項或其程式碼後置類別中的 ADO.NET 程式碼的資料庫。 如需更深入了解建立資料驅動的應用程式使用階層式的架構，請參閱我[*使用的資料*教學課程](../../data-access/index.md)。

## <a name="databases-on-development-versus-production"></a>在開發與生產資料庫

當您開始開發資料導向 web 應用程式時，您必須指定資料庫連接字串，其中提供有關如何連接到資料庫的應用程式詳細資料。 指定此連接字串之間其他項目、 資料庫伺服器、 資料庫名稱和安全性資訊。 大多數情況下，在開發期間的應用程式所使用的資料庫是不同於資料庫時使用它在生產環境中的 s。 有許多優點的開發與生產環境使用不同的資料庫。 有不同，資料庫開發表示不要也不必擔心不小心修改或刪除即時資料。 它也可讓您將放在虛擬測試資料或進行資料模型中的重大變更，而不必擔心對實際執行環境中的應用程式的影響。 具有不同的資料庫開發和生產環境中，當應用程式的缺點的資料庫結構描述部署資料庫和相關的任何變更，或也必須部署的資料。

第一個在部署之前，只有一個執行個體的資料庫，且該執行個體在開發環境中。 當部署到生產環境，第一次我們不僅複製所需的伺服器端和用戶端檔案，而且也將從開發環境的資料庫複製到生產環境應用程式。 這是，我們就能正確現在的書籍評論的 web 應用程式-資料庫會位於`App_Data`不在我們的開發環境中的資料夾但有尚未被推送至生產環境。

一旦部署應用程式有兩個資料庫的複本。 當應用程式已臻完備，可能會新增任何功能，需要資料模型變更 （例如加入新的資料行加入現有的資料表、 變更現有的資料行，加入新的資料表，等等）。 接下來部署 web 應用程式時，所做的變更套用至開發環境中的資料庫因為必須套用的最後一個部署至生產環境資料庫。 未來的教學課程將討論一些策略來管理此程序。 本教學課程著重於部署整個資料庫從開發環境到生產環境。

## <a name="deploying-the-database-to-the-production-environment"></a>將資料庫部署到生產環境

本教學課程的其餘部分會探討如何將資料庫從開發環境到生產環境部署。 如果您依照沿著您需要先確定您的帳戶與您的 web 主機提供者，包含 Microsoft SQL Server 資料庫支援。 您也必須有一些資訊在手邊，也就是資料庫伺服器名稱、 資料庫名稱，以及使用者名稱和密碼用來連接到資料庫。

如稍早在本教學課程中所述，書籍評論網站的資料庫是儲存在 SQL Server 2008 Express Edition 資料庫`App_Data`資料夾。 它會方面的原因，部署這類資料庫會是只要複製`App_Data`從開發環境到生產環境中的資料夾。 不過，大部分的 web 主機提供者不支援在裝載資料庫`App_Data`基於安全性原因的資料夾。 相反地，web 主機提供的帳戶在其環境中的 SQL Server 資料庫伺服器上。 將資料庫從開發環境到生產環境部署需要取得您註冊您的 web 主機的資料庫伺服器上的資料庫。

因此您該如何將資料庫從開發環境到生產環境？ 有幾種方式來完成這項作業取決於何種服務您的 web 主機供應項目。 您可以使用某些主機，例如獲得，FTP 資料庫或實際的備份`.mdf`檔案至您的網站，然後從 [控制台] 中還原備份的檔案或附加`.mdf`SQL Server 資料庫伺服器的檔案。 使用這類工具若要將資料庫部署很簡單，只要複製`App_Data`到生產環境，並再將它附加透過控制台中的資料夾。 這可能是第一次發佈您的資料庫的最簡單且最快速的方式。

另一種方法是使用資料庫發行精靈。 資料庫發行精靈會將產生的 SQL 命令，來建立資料庫 s 結構描述-資料表、 預存程序、 檢視、 使用者定義函式和等位，並選擇性地將資料在其表格中的 Windows 桌面應用程式。 您可以接著連接到您的 web 主機提供者的資料庫伺服器透過 SQL Server Management Studio，並執行此指令碼來複製實際執行資料庫。 更好，如果您的 web 主機提供者支援 Microsoft s [Database Publishing Services](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home)就可以自動代替您資料庫伺服器上執行的資料庫發行精靈所產生的指令碼。 資料庫發行精靈會產生建立的資料庫 s 結構描述和資料的指令碼，因為它不管您的 web 主機提供者是否提供的功能，例如附加已上傳`.mdf`檔案。

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>產生 SQL 命令，以建立資料庫結構描述和資料使用資料庫發行精靈

可讓 s 逐步引導您使用 「 資料庫發行精靈 」 的書籍評論資料庫部署到生產環境。 如果您使用 Visual Studio 2008 或以上版本之後，已安裝資料庫發行精靈。 如果您使用 Visual Studio 2005，則您必須先[下載並安裝](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)精靈。

開啟 Visual Studio，並瀏覽至`Reviews.mdf`資料庫。 如果您使用 Visual Web Developer 中，請移至 [資料庫總管] 中;如果您使用 Visual Studio，請使用 [伺服器總管] 中。 [圖 4] 顯示`Reviews.mdf`Visual Web Developer 中的 [資料庫總管] 中的資料庫。 如 [圖 4] 所示，`Reviews.mdf`資料庫由四個資料表、 三個預存程序，和使用者定義函式所組成。


[![在 [資料庫總管] 或伺服器總管中找出資料庫](deploying-a-database-vb/_static/image11.jpg)](deploying-a-database-vb/_static/image10.jpg) 

**[圖 4**： 在資料庫總管] 或 [伺服器總管] 找出資料庫 ([按一下以檢視完整大小的影像](deploying-a-database-vb/_static/image12.jpg))


以滑鼠右鍵按一下資料庫名稱，並從內容功能表中選擇 [發佈到提供者] 選項。 這會啟動 「 資料庫發行精靈 」 （請參閱 [圖 5]）。 按一下旁邊進階過去的啟動顯示畫面。


[![資料庫發行精靈啟動顯示畫面](deploying-a-database-vb/_static/image14.jpg)](deploying-a-database-vb/_static/image13.jpg) 

**圖 5**: 資料庫發行精靈啟動顯示畫面 ([按一下以檢視完整大小的影像](deploying-a-database-vb/_static/image15.jpg))


在精靈的第二個畫面會列出資料庫發行精靈可存取的資料庫，並讓您選擇是否在選取的資料庫中的所有物件編寫指令碼，或挑選要指令碼的物件。 選取適當的資料庫，並將選取 「 所有的指令碼物件中選取的資料庫 」 的選項。

> [!NOTE]
> 如果您收到錯誤 「 資料庫中沒有任何物件*databaseName*此精靈可編寫指令碼的類型 」 時在 [圖 6] 所示的畫面中，按一下 [下一步]，請確定您的資料庫檔案的路徑不是太長。 如中所述[此討論項目](http://www.codeplex.com/sqlhost/Thread/View.aspx?ThreadId=11014)資料庫發行精靈 專案頁面中，如果資料庫檔案的路徑太長，可能發生此錯誤。


[![資料庫發行精靈啟動顯示畫面](deploying-a-database-vb/_static/image17.jpg)](deploying-a-database-vb/_static/image16.jpg) 

**圖 6**: 資料庫發行精靈啟動顯示畫面 ([按一下以檢視完整大小的影像](deploying-a-database-vb/_static/image18.jpg))


從下一個畫面中，可以產生指令碼檔案或者，如果您的 web 主機可支援此功能，在資料庫直接發佈到 web 主機提供者的資料庫伺服器。 如 [圖 7] 所示，我遇到指令碼寫入檔案`C:\REVIEWS.MDF.sql`。


[![編寫資料庫備份至檔案，或直接發行至您的 Web 主機提供者](deploying-a-database-vb/_static/image20.jpg)](deploying-a-database-vb/_static/image19.jpg) 

**圖 7**： 編寫資料庫備份至檔案，或直接發行至您的 Web 主機提供者 ([按一下以檢視完整大小的影像](deploying-a-database-vb/_static/image21.jpg))


後續的畫面會提示您提供各種不同的指令碼選項。 您可以指定指令碼是否應包含 drop 陳述式來移除這些現有的物件。 預設為 True，當第一次部署資料庫時，這沒有問題。 您也可以指定目標資料庫是 SQL Server 2000、 SQL Server 2005 或 SQL Server 2008。 最後，您可以指定是否以指令碼的結構描述和資料，只是資料或只有結構描述。 結構描述是資料庫物件、 資料表、 預存程序、 檢視和等等的集合。 資料是位於資料表中的資訊。

圖 8 所示，我發生了精靈設定為卸除現有的資料庫物件，來產生指令碼針對 SQL Server 2008 資料庫，並將發行的結構描述和資料。


[![指定發佈選項](deploying-a-database-vb/_static/image23.jpg)](deploying-a-database-vb/_static/image22.jpg) 

**圖 8**： 指定發佈選項 ([按一下以檢視完整大小的影像](deploying-a-database-vb/_static/image24.jpg))


在最後兩個畫面摘述即將進行，並接著顯示的指令碼狀態的動作。 執行精靈的最後結果是我們有包含生產環境上建立資料庫，然後以相同的資料擴展上開發所需的 SQL 命令的指令碼檔案。

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>在生產環境資料庫上執行的 SQL 命令

既然我們已經的指令碼，其中包含 SQL 命令，以建立所有資料庫和其資料仍然是在生產資料庫上執行指令碼。 某些 web 主機提供者提供的文字方塊中，您可以在此輸入您的資料庫上執行的 SQL 命令其控制項台中。 如果您有非常大的指令碼檔案，則此選項可能無法運作 (`REVIEWS.MDF.sql`指令碼檔案是超過 425 KB 的大小，例如)。

更好的方法是直接連線到生產環境資料庫伺服器使用 SQL Server Management Studio (SSMS)。 如果您有非 Express Edition 的 SQL Server 安裝在您的電腦上則您可能已安裝 SSMS。 否則，您可以[下載並安裝](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)一份免費的 SQL Server Management Studio Express 的版本。

啟動 SSMS 並連接到 web 主機的資料庫伺服器使用您的 web 主機提供者所提供的資訊。


[![連接到 Web 主機提供者的資料庫伺服器](deploying-a-database-vb/_static/image26.jpg)](deploying-a-database-vb/_static/image25.jpg) 

**圖 9**： 連接到您的 Web 主機提供者 s 資料庫伺服器 ([按一下以檢視完整大小的影像](deploying-a-database-vb/_static/image27.jpg))


展開 [資料庫] 索引標籤，然後找出您的資料庫。 按一下工具列的左上角的 新增查詢 按鈕，SQL 命令，從資料庫發行精靈 中，所建立的指令碼檔案中貼上，按一下 執行 按鈕，在生產環境資料庫伺服器上執行這些命令。 如果您的指令碼檔案是特別大可能需要幾分鐘的時間來執行命令。


[![連接到 Web 主機提供者的資料庫伺服器](deploying-a-database-vb/_static/image29.jpg)](deploying-a-database-vb/_static/image28.jpg) 

**圖 10**： 連接到您的 Web 主機提供者 s 資料庫伺服器 ([按一下以檢視完整大小的影像](deploying-a-database-vb/_static/image30.jpg))


S 就是這麼簡單 ！ 此時開發資料庫已有重複至生產環境。 如果您重新整理 SSMS 中的資料庫應該會看到新的資料庫物件。 [圖 11] 顯示生產資料庫的資料表、 預存程序和使用者定義函式，在開發資料庫的鏡像。 而且因為我們指示資料庫發行精靈來發行資料時，生產資料庫的資料表就會有開發資料庫的資料表相同的資料執行精靈的次。 [圖 12] 顯示中的資料`Books`生產資料庫上的資料表。


[![有已在生產資料庫上有重複的資料庫物件](deploying-a-database-vb/_static/image32.jpg)](deploying-a-database-vb/_static/image31.jpg) 

**圖 11**: 資料庫物件具有已複製生產資料庫上 ([按一下以檢視完整大小的影像](deploying-a-database-vb/_static/image33.jpg))


[![生產資料庫包含相同的資料上開發資料庫](deploying-a-database-vb/_static/image35.jpg)](deploying-a-database-vb/_static/image34.jpg) 

**圖 12**： 實際執行資料庫開發資料庫上包含相同的資料 ([按一下以檢視完整大小的影像](deploying-a-database-vb/_static/image36.jpg))


目前我們只擁有開發資料庫部署到生產環境。 我們尚未討論過部署 web 應用程式本身或檢查將使用生產資料庫上實際執行的應用程式所需的組態變更。 在下一個教學課程中，我們將討論這些問題 ！

## <a name="summary"></a>總結

部署的資料驅動 web 應用程式需要複製到生產環境的開發期間使用的資料庫。 許多 web 主機提供者會提供工具，來簡化部署資料庫的程序。 比方說，獲得與您可以 FTP 資料庫`.mdf`檔案 （或備份），從 控制台，然後將資料庫附加至資料庫伺服器。 無論何種功能的運作方式的另一個選項您 web 主機提供者的供應項目是 Microsoft 的資料庫發行精靈 」 工具，可產生的指令碼來建立開發資料庫 s 結構描述和資料的 SQL 命令。 此指令碼產生之後您可以在生產資料庫上執行它。

書籍評論 web 應用程式的資料庫已在生產環境現在我們可以部署應用程式。 不過，web 應用程式的組態資訊指定連接字串至資料庫，而且該連接字串會參考開發資料庫。 我們需要將網站部署至生產環境時，更新此連接字串資訊。 下一個教學課程會探討這些組態差異，並逐步解說資料驅動的書籍評論網站發佈到生產環境所需的步驟。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [下載 Microsoft SQL Server 資料庫發行精靈 1.1](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [下載 Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

> [!div class="step-by-step"]
> [上一頁](core-differences-between-iis-and-the-asp-net-development-server-vb.md)
> [下一頁](configuring-the-production-web-application-to-use-the-production-database-vb.md)
