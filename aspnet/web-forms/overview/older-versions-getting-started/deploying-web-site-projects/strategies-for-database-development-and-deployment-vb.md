---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
title: 資料庫開發及部署 (VB) 的策略 |Microsoft Docs
author: rick-anderson
description: 部署第一次的資料導向應用程式時可以盲目地將資料庫複製到生產環境的開發環境中。 B...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 07b8905d-78ac-4252-97fb-8675b3fb0bbf
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
msc.type: authoredcontent
ms.openlocfilehash: f279b6eea7a3dc77ed32d6529c5bd90763d91f84
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380411"
---
<a name="strategies-for-database-development-and-deployment-vb"></a>資料庫開發及部署 (VB) 的策略
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載 PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial10_DBDevel_vb.pdf)

> 部署第一次的資料導向應用程式時可以盲目地將資料庫複製到生產環境的開發環境中。 但是，執行 blind 在後續的部署中的複本將會覆寫任何進入實際執行資料庫的資料。 相反地，將資料庫部署，涉及套用上次部署到實際執行資料庫後，開發資料庫所做的變更。 本教學課程會檢查這些挑戰，並提供各種不同的策略，可協助 chronicling 和套用上次部署後對資料庫進行的變更。


## <a name="introduction"></a>簡介

先前的教學課程所述，部署 ASP.NET 應用程式需要相關的內容複製到生產環境的開發環境。 部署不是一次性事件，但而是會驅使每次發行新版本的軟體，或當 bug 或已找出並解決安全性考量。 當複製的 ASP.NET 網頁時，影像、 JavaScript 檔案和其他這類檔案至生產環境，您不需要擔心如何使用這些檔案已變更上次部署後。 您可以盲目地將檔案複製到生產環境中，覆寫現有的內容。 不幸的是，此簡化程序並不支援將資料庫部署。

部署第一次的資料導向應用程式時可以盲目地將資料庫複製到生產環境的開發環境中。 但是，執行 blind 在後續的部署中的複本將會覆寫任何進入實際執行資料庫的資料。 相反地，部署資料庫時，需要套用*變更*自從上一次部署到生產環境資料庫以來，對開發資料庫。 本教學課程會檢查這些挑戰，並提供各種不同的策略，可協助 chronicling 和套用上次部署後對資料庫進行的變更。

## <a name="the-challenges-of-deploying-a-database"></a>部署資料庫上的挑戰

資料驅動的應用程式已部署第一次之前，只有一個資料庫，也就是在開發環境，這就是為什麼部署第一次的資料導向應用程式時即可盲目地資料庫複製中的資料庫開發環境到生產環境。 但一旦部署應用程式有兩個資料庫的複本： 一個在開發和生產環境中的其中一個。

部署之間的開發和生產環境資料庫可能會變得不同步。雖然生產資料庫 s 結構描述會維持不變，加入新功能時，就可能會變更開發資料庫 s 結構描述。 您可能會新增或移除資料行、 資料表、 檢視或預存程序。 也可能新增至開發資料庫的重要資料。 許多資料導向的應用程式包含以硬式編碼的特定應用程式不是使用者可編輯的資料填入查閱資料表。 例如，拍賣網站可能會有下拉式清單中，以說明正在 auctioned 項目的條件的選項： 新增、 像是新、 良好和公平。 而不是硬式編碼這些選項，直接在下拉式清單中的它通常會比較好將它們放在資料庫資料表。 如果您在開發期間，名為不佳的新條件加入至資料表再部署應用程式時這個相同的記錄，就必須新增至實際執行資料庫中的查閱資料表。

在理想情況下，將資料庫部署牽涉到資料庫複製到生產環境的開發。 但請記住，您已部署應用程式，並繼續開發之後，實際執行資料庫要填入來自實際使用者的實際資料。 因此，如果您要直接從開發到下一個部署在生產環境中複製資料庫會覆寫生產資料庫，而且其現有的資料遺失。 最後結果就是，將資料庫部署由套用自從上次部署以來，對開發資料庫的變更。

因為部署資料庫時，需要將結構描述，而且可能資料中的變更套用上次部署後，變更的歷程記錄必須維護 （或在部署階段決定），以便可以在實際執行上套用這些變更。 有各種不同的管理，並將變更套用至資料模型的技巧。

### <a name="defining-the-baseline"></a>定義比較基準

若要維護您的應用程式的資料庫的變更中，您需要有一些起始狀態的變更會套用至基準。 其中一種方式開始的狀態可能是空的資料庫沒有資料表、 檢視或預存程序。 這類基準會導致大型的變更記錄檔，因為它必須包含建立所有資料庫的資料表、 檢視和預存程序，以及在初始部署後所做的任何變更。 在另一端的範圍中，您可以設定基準為一開始會部署到生產環境資料庫的版本。 這項選擇會導致許多較小的變更記錄檔，因為它只包含下列第一次部署的資料庫所做的變更。 這是我偏好的方法。 而且當然可以選擇更多的中介的道路的方法，定義基準之間建立初始資料庫和資料庫第一次部署時，某個時間點。

一旦您有選擇基準會考慮產生可執行以重新建立的基準版本的 SQL 指令碼。 這類指令碼可讓您能夠快速地重新建立資料庫的基準版本。 這項功能特別適用於較大型的專案，其中可能有多位開發人員處理的專案或其他環境，例如測試或預備環境，其中的每個需要自己的資料庫複本。

有各種不同的工具來產生基準版本的 SQL 指令碼供您使用。 從 SQL Server Management Studio (SSMS) 您可以以滑鼠右鍵按一下資料庫，請移至 工作 子功能表，然後選擇 產生指令碼選項。 這會啟動指令碼精靈，您可以指示來產生此檔案包含 SQL 命令，以建立您的資料庫的物件。 另一個選項是資料庫發行精靈，可以產生不只在資料庫資料表中建立資料庫結構描述，但是資料的 SQL 命令。 資料庫發行精靈的詳細檢查的年代*將資料庫部署*教學課程。 不論您使用何種工具，您應該會有最終可用來重新建立資料庫的基準版本的指令碼檔案應該需要發生。

## <a name="documenting-the-database-changes-in-prose"></a>加入註解文字中的資料庫變更

在開發階段期間，維持資料模型變更的記錄檔的最簡單方式是在文字中記錄變更。 例如，如果已部署應用程式的開發期間將新增到新的資料行`Employees`資料表中移除的資料行`Orders`資料表，並加入新的資料表 (`ProductCategories`)，您會維護文字檔案或 Microsoft Word 文件使用下列的歷程記錄：

<a id="0.8_table01"></a>


| **變更日期** | **變更詳細資料** |
| --- | --- |
| 2009-02-03: | 加入的資料行`DepartmentID`(`int`，NOT NULL) 到`Employees`資料表。 加入從外部索引鍵條件約束`Departments.DepartmentID`至`Employees.DepartmentID`。 |
| 2009-02-05: | 已移除的資料行`TotalWeight`從`Orders`資料表。 中已擷取的資料相關聯`OrderDetails`記錄。 |
| 2009-02-12: | 建立`ProductCategories`資料表。 有三個資料行： `ProductCategoryID` (`int`， `IDENTITY`， `NOT NULL`)， `CategoryName` (`nvarchar(50)`， `NOT NULL`)，並`Active`(`bit`， `NOT NULL`)。 加入至主索引鍵條件約束`ProductCategoryID`，並預設值是 1 到`Active`。 |


有一些缺點，這種方法。 首先，沒有任何希望自動化。 隨時這些變更需要套用至資料庫-例如，當應用程式部署-開發人員必須以手動方式實作每個變更，一次一個。 此外，如果您需要重新建構特定版本的資料庫從基準線使用的變更記錄檔，這樣做會讓越來越多的時間的記錄檔大小增加時。 這個方法的另一個缺點是，保留的清晰度與詳細資料的每個變更記錄項目層級記錄變更的人員。 與多個開發人員小組中一些可能會比其他更詳細、 更容易閱讀，或更精確的項目。 此外，有錯字和其他人與相關的資料輸入錯誤都可能。

加入註解文字中的資料庫變更的主要優點是簡單。 您不 t 需要熟悉建立和改變資料庫物件的 SQL 語法。 相反地，您可以變更記錄中的文字，並透過 SQL Server Management Studio s 圖形化使用者介面實作它們。

維護您的變更記錄檔中的文字，無可否認地，不非常複雜且獲利 t 適用於特定專案，例如在範圍內，大型的經常變更的資料模型，或是牽涉到多個開發人員。 但是，我曾看過這種方法很適用於小型的工時專案具有只有偶爾變更的資料模型，其中單一的開發人員並沒有強式的背景中建立和改變資料庫物件的 SQL 語法。

> [!NOTE]
> 雖然變更記錄檔中的資訊，技術上來說，只需要部署階段建議保留變更的歷程記錄。 但是，而不是維護單一日益增長，變更記錄檔，請考慮讓每個資料庫版本的不同的變更記錄檔。 通常您會想要版本資料庫每個部署的時間。 藉由維護的變更記錄檔記錄您可以從基準中，重新建立任何資料庫版本藉由執行從第 1 版的變更記錄檔指令碼，並繼續執行直到您到達版本需要重新建立。


## <a name="recording-the-sql-change-statements"></a>記錄 SQL 變更陳述式

維護變更記錄檔中的文字的主要缺點是缺乏自動化。 在理想情況下，實作在部署階段的生產環境資料庫的資料庫變更會簡單，只要按一下按鈕以執行指令碼，而不需要以手動方式執行的指示清單。 這類自動化就可以藉由維護變更記錄檔，其中包含用來改變資料模型的 SQL 命令。

SQL 語法包含一些建立和修改各種資料庫物件的陳述式。 例如， [ *CREATE TABLE 陳述式*](https://msdn.microsoft.com/library/ms174979.aspx)、 執行時，建立新的資料表，使用指定的資料行和條件約束。 [ *ALTER TABLE 陳述式*](https://msdn.microsoft.com/library/ms190273.aspx)修改現有的資料表，新增、 移除或修改其資料行或條件約束。 另外還有陳述式來建立、 修改和卸除索引、 檢視、 使用者定義函數、 預存程序、 觸發程序和其他資料庫物件。

回到先前的範例中，映像新增到新的資料行的已部署應用程式的開發過程`Employees`資料表中移除的資料行`Orders`資料表，並加入新的資料表 (`ProductCategories`)。 這類動作會導致變更記錄檔，使用下列 SQL 命令：

[!code-sql[Main](strategies-for-database-development-and-deployment-vb/samples/sample1.sql)]

將這些變更推送到生產資料庫，在部署階段是一種單鍵操作： 開啟 SQL Server Management Studio、 連接到您的生產環境資料庫、 開啟 [新增查詢] 視窗、 貼上的變更記錄檔中，內容和按一下 [執行] 以執行指令碼。

## <a name="using-a-comparison-tool-to-synchronize-the-data-models"></a>使用的比較工具來同步處理的資料模型

加入註解文字中的資料庫變更很容易，但是實作變更需要開發人員每一項變更其中一個在生產資料庫上一次;記錄變更的 SQL 命令會使快速按一下按鈕，即可輕鬆地在生產資料庫上實作這些變更，但需要學習和精通 SQL 陳述式和語法建立和改變資料庫物件。 資料庫的比較工具需要最好的這兩種方法，並捨棄最壞的情況。

資料庫比較工具比較結構描述或兩個資料庫的資料，並顯示摘要報告，顯示您的資料庫有何不同。 然後，您也可以使用按一下按鈕時，您可以產生同步處理一或多個資料庫物件的 SQL 命令。 簡單的說，您可以使用資料庫比較工具來比較開發和生產資料庫，在部署階段，產生此檔案包含 SQL 命令，執行時，會將變更套用至實際執行資料庫 s 結構描述因此它鏡像開發資料庫 s 結構描述。

有各種不同的多家廠商所提供的協力廠商資料庫比較工具。 其中一個範例是[ *SQL 比較*](http://www.red-gate.com/products/SQL_Compare/)，依[ *Red Gate Software*](http://www.red-gate.com/)。 可讓 s 逐步解說使用 「 SQL 比較來比較和同步處理的開發和生產環境資料庫結構描述的程序。

> [!NOTE]
> 在撰寫本文時最新版 SQL 比較是版本 7.1 中，Standard Edition 為 $ 395 美元的成本計算方式。 您可以遵循下載 14 天的免費試用。


SQL 比較啟動時比較專案 對話方塊隨即開啟，顯示已儲存的 「 SQL 比較 」 專案。 建立新的專案。 這會啟動 專案組態精靈，它會提示您資料庫的相關資訊的比較 （見 圖 1）。 輸入開發和生產環境資料庫的資訊。


[![比較開發和生產資料庫](strategies-for-database-development-and-deployment-vb/_static/image2.jpg)](strategies-for-database-development-and-deployment-vb/_static/image1.jpg)

**圖 1**： 比較開發和生產資料庫 ([按一下以檢視完整大小的影像](strategies-for-database-development-and-deployment-vb/_static/image3.jpg))


> [!NOTE]
> 如果您的開發環境資料庫是 SQL Express Edition 資料庫中的檔案`App_Data`資料夾，您必須在 SQL Server Express 資料庫伺服器中註冊資料庫，才能從 [圖 1] 所示的對話方塊中選取的網站。 若要這麼做最簡單的方式是開啟 SQL Server Management Studio (SSMS)、 連接到 SQL Server Express 資料庫伺服器和附加資料庫。 如果您沒有 SSMS 安裝在您的電腦上您可以下載並安裝免費[ *SQL Server 2008 Management Studio Basic 版*](https://www.microsoft.com/downloads/details.aspx?FamilyId=7522A683-4CB2-454E-B908-E805E9BD4E28&amp;displaylang=en)。


選取要比較的資料庫，您也可以指定各種比較設定從 [選項] 索引標籤。您可能想要開啟的其中一個選項是 「 略過條件約束和索引名稱。 」 您應該記得，在先前的教學課程中我們已新增應用程式服務來開發和生產環境資料庫的資料庫物件。 如果您使用`aspnet_regsql.exe`工具，以在生產資料庫上建立這些物件，則您會發現開發和生產環境資料庫之間的主索引鍵和唯一條件約束名稱不同。 因此，SQL 比較會標示所有做為不同的應用程式服務資料表。 您可以將保留 「 略過條件約束和索引名稱 」 未核取和同步處理的條件約束名稱，或 SQL 比較要略過這些差異。

選取的資料庫之後比較 （和檢閱的比較選項），按一下 [開始比較現在比較] 按鈕。 接下來的幾秒，透過 SQL 比較會檢查兩個資料庫的結構描述，並產生報告有何不同。 我已特意不會進行一些修改，以顯示 SQL 比較介面中記下這類不一致的地方是如何開發資料庫。 如 [圖 2] 所示，我加入 ve`BirthDate`資料行`Authors`移除的資料表`ISBN`資料行`Books`資料表，並加入新的資料表， `Ratings`，這要讓使用者瀏覽站台速度的檢閱的書籍。

> [!NOTE]
> 在本教學課程中所做的資料模型變更是為了說明如何使用資料庫比較工具。 您不會在資料庫中找到這些變更，在未來的教學課程。


[![SQL 比較列出開發和生產資料庫之間的差異](strategies-for-database-development-and-deployment-vb/_static/image5.jpg)](strategies-for-database-development-and-deployment-vb/_static/image4.jpg)

**圖 2**: SQL 比較列出開發之間的差異和生產資料庫 ([按一下以檢視完整大小的影像](strategies-for-database-development-and-deployment-vb/_static/image6.jpg))


SQL 比較細分成群組的資料庫物件，快速顯示哪些物件存在於這兩個資料庫，但不同，物件存在於一個資料庫，但不是另一個，並且有哪些物件時完全相同。 如您所見，有兩個資料庫中存在但為不同的兩個物件：`Authors`資料表，它有新增的資料行，而`Books`有一個移除的資料表。 只存在於開發資料庫，也就是新建立的一個物件`Ratings`資料表。 而且有兩個資料庫中相同的 117 物件。

選取的資料庫物件時，會顯示差異 視窗中，因為它會顯示這些物件有何不同。 SQL 差異視窗中，顯示在底部 [圖 2] 中反白顯示，`Authors`開發資料庫中的資料表有`BirthDate`資料行中找不到`Authors`生產資料庫上的資料表。

檢閱差異，並選取您想要同步處理哪些的物件之後, 的下一個步驟是產生的 SQL 命令來更新實際執行資料庫 s 結構描述所需符合開發資料庫。 這是透過同步處理精靈 」 來完成。 同步處理精靈 」 確認哪些物件同步處理，並摘要說明動作計劃 （請參閱 [圖 3]）。 您可以立即同步處理資料庫，或產生含有可以暇地執行的 SQL 命令的指令碼。


[![使用同步處理精靈 」 來同步處理您的資料庫結構描述](strategies-for-database-development-and-deployment-vb/_static/image8.jpg)](strategies-for-database-development-and-deployment-vb/_static/image7.jpg)

**[圖 3**： 使用同步處理精靈] 來同步處理您的資料庫結構描述 ([按一下以檢視完整大小的影像](strategies-for-database-development-and-deployment-vb/_static/image9.jpg))


資料庫的比較工具，例如 Red Gate Software 的 SQL 比較，請將變更套用至開發資料庫結構描述一樣簡單，只要動動滑鼠按一下的生產環境資料庫。

> [!NOTE]
> SQL 比較比較並同步處理兩個資料庫*結構描述*。 不幸的是，它不會比較並同步處理兩個資料庫資料表中的資料。 Red Gate Software 提供的產品，名為[ *SQL Data Compare* ](http://www.red-gate.com/products/SQL_Data_Compare/) ，比較並同步處理兩個資料庫之間的資料，但它是個別的產品，從 SQL 比較，另一個為 $ 395 美元的成本。


## <a name="taking-the-application-offline-during-deployment"></a>讓應用程式離線部署期間

因為我們已看到在這些教學課程中，整個部署是包含多個步驟的程序： 從開發環境的 ASP.NET 網頁、 主版頁面、 CSS 檔案、 JavaScript 檔案、 影像和其他必要的內容複製到實際執行環境;複製實際執行環境特定組態資訊，如有需要和上次部署後，將變更套用至資料模型。 根據檔案的數目和您的資料庫變更的複雜性，這些步驟可以花費幾秒鐘到幾分鐘的時間完成。 在此時段期間的 web 應用程式是在 flux 中，瀏覽網站的使用者可能會遇到錯誤或非預期的行為。

部署網站時最好是 「 離線 」 需要 web 應用程式，直到部署完成為止。 離線應用程式 （並使其備份，一旦完成部署程序） 是簡單，只要上傳檔案，然後再將其刪除。 開始使用 ASP.NET 2.0 中，名為的檔案存在`app_offline.htm`中之應用程式根目錄會整個網站 「 離線 」。 任何在該網站上的 ASP.NET 頁面的要求自動回應的內容`app_offline.htm`檔案。 一旦移除該檔案之後，應用程式恢復上線。

取得應用程式離線，然後部署，很簡單，只要上傳`app_offline.htm`檔案到生產環境 s 根目錄之前開始部署程序，並再將其刪除 （或重新命名為其他項目） 一次部署已完成。 如需這項技術的詳細資訊請參閱 John Peterson 的文章中，採取[*離線 ASP.NET 應用程式*](http://www.15seconds.com/issue/061207.htm)。

## <a name="summary"></a>總結

主要的挑戰，資料驅動的應用程式部署都著重於部署資料庫。 因為有兩個版本的資料庫-一個開發環境中，一個在生產環境中這些兩個資料庫結構描述可能會不同步，因為在開發過程中會加入新的功能。 更多，因為做為填入實際資料來自實際使用者實際執行資料庫，您無法覆寫的生產環境資料庫修改的開發資料庫與部署應用程式 （ASP.NET 網頁，構成檔案時，就如同哪些 s映像檔...等等）。 相反地，將資料庫部署，需要實作的實際執行資料庫開發資料庫上次部署後變更一組精確。

本教學課程探討了三項技術來維護及套用資料庫變更的記錄檔。 簡單的方法是在文字中記錄變更。 雖然這種做法在生產資料庫是手動程序上實作這些變更，它不需要知道 SQL 命令來建立和改變資料庫物件。 更複雜的方法，和另一個則是更為可行較大的專案或專案中與多位開發人員，是將變更記錄為一系列的 SQL 命令。 這可大幅則推出這些變更到目標資料庫。 這兩種方法的最佳可藉由使用資料庫比較工具，例如 Red Gate Software 的 SQL 比較。

本教學課程結束時，我們著重於部署的資料導向應用程式。 下一組教學課程探討如何在生產環境中的錯誤回應。 我們將探討如何顯示易懂的錯誤頁面，而是而不是黃色的人員死亡畫面。 看如何將記錄錯誤 s 詳細資料以及如何發生這類錯誤時發出警示。

快樂地寫程式 ！

> [!div class="step-by-step"]
> [上一頁](configuring-a-website-that-uses-application-services-vb.md)
> [下一頁](displaying-a-custom-error-page-vb.md)
