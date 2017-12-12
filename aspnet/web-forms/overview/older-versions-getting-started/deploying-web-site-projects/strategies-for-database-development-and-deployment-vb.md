---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
title: "資料庫開發和部署 (VB) 的策略 |Microsoft 文件"
author: rick-anderson
description: "部署第一次的資料導向應用程式時您盲目地將資料庫複製到生產環境的開發環境中。 B...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 07b8905d-78ac-4252-97fb-8675b3fb0bbf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
msc.type: authoredcontent
ms.openlocfilehash: 8632ed2fe5c1a296747a0206de1c6f5c5bb59dd1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="strategies-for-database-development-and-deployment-vb"></a>資料庫開發和部署 (VB) 的策略
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載 PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial10_DBDevel_vb.pdf)

> 部署第一次的資料導向應用程式時您盲目地將資料庫複製到生產環境的開發環境中。 但是執行 blind 後續部署中的複本將會覆寫輸入實際執行資料庫的任何資料。 相反地，部署資料庫涉及套用後的最後一個部署到實際執行資料庫開發資料庫所做的變更。 本教學課程會檢查這些挑戰，並提供各種不同的策略，以協助 chronicling 並套用上次部署後對資料庫進行變更。


## <a name="introduction"></a>簡介

先前的教學課程所述，ASP.NET 應用程式部署需要在開發環境相關的內容複製到生產環境。 部署不是一次性的活動，但而是會驅使每次發行新版本的軟體，或當 bug 或已識別並解決安全性考量。 複製時的 ASP.NET 網頁，映像、 JavaScript 檔案和其他這類檔案到生產環境，您不需要考慮如何使用這些檔案已變更後的最後一個部署。 您可以盲目地將檔案複製到生產環境中，覆寫現有的內容。 不幸的是，這種不會延伸到部署資料庫。

部署第一次的資料導向應用程式時您盲目地將資料庫複製到生產環境的開發環境中。 但是執行 blind 後續部署中的複本將會覆寫輸入實際執行資料庫的任何資料。 相反地，部署資料庫時，需要套用*變更*上次部署到實際執行資料庫後對開發資料庫。 本教學課程會檢查這些挑戰，並提供各種不同的策略，以協助 chronicling 並套用上次部署後對資料庫進行變更。

## <a name="the-challenges-of-deploying-a-database"></a>部署資料庫的驗證題目

已部署第一次的資料導向應用程式之前，只有一個資料庫，也就是在開發環境，這就是為什麼部署第一次的資料導向應用程式時可以盲目資料庫複製的資料庫中實際執行環境的開發環境。 一旦部署應用程式有兩個複本的資料庫，但是： 一個在開發和生產環境中的其中一個。

部署之間的開發和生產資料庫可能會變得不同步。實際執行資料庫 s 結構描述會保持不變，而加入的新功能時，就可能會變更開發資料庫 s 結構描述。 您可能會新增或移除資料行、 資料表、 檢視或預存程序。 也可能會加入至開發資料庫的重要資料。 許多資料導向應用程式包含以硬式編碼的特定應用程式將無法編輯使用者的資料填入的查閱資料表。 例如，拍賣網站可能描述正在 auctioned 項目的條件的選項下拉式清單： 新增、 像是新、 好和公平。 而不是硬式編碼直接在下拉式清單中的這些選項是通常是較好的方式將它們放在資料庫資料表。 如果在開發期間，新的條件，名為不良時就會加入至資料表再部署應用程式相同的記錄必須加入至實際執行資料庫中的查閱資料表。

在理想情況下，將資料庫部署會包含資料庫複製到生產環境的開發。 但是請記住，您部署應用程式，並繼續開發之後，實際執行資料庫會被填入實際資料從實際的使用者。 因此，如果您只需從開發到生產環境，在下一個部署複製資料庫會覆寫生產資料庫，而且其現有的資料遺失。 最後結果就是，將資料庫部署分布於套用之後的最後一個部署至開發資料庫所做的變更。

部署資料庫涉及套用上次部署後變更結構描述以及可能的資料，因為變更的記錄必須維護 （或在部署階段決定），以便在實際執行可以套用這些變更。 有各種不同的管理，以及將變更套用至資料模型的技術。

### <a name="defining-the-baseline"></a>定義基準

若要維護應用程式 s 資料庫的變更，您需要有一些開始狀態，要將變更套用至的基準。 其中一種方式起始狀態可能是空的資料庫不含資料表、 檢視表或預存程序。 大型變更記錄檔中的這類比較基準結果，因為它必須包含建立所有資料庫的資料表、 檢視和預存程序，以及在初始部署後所做的任何變更。 另一端的範圍中，您可以設定基準為一開始會部署到生產環境的資料庫版本。 這項選擇會產生較小的變更記錄，因為它只會包含下列在首次部署資料庫所做的變更。 這是我偏好的方法。 而且當然您可以選擇更多的中介的道路方法，定義基準最初建立資料庫與資料庫第一次部署之間，某個時間點。

一旦您選擇基準請考慮產生可以執行來重新建立基準版本的 SQL 指令碼。 這類指令碼可讓您能夠快速地重新建立資料庫的基準版本。 這項功能特別適用於大型專案，其中可能有多個開發人員在專案或其他環境，例如測試或預備環境，其中的每個需要其自己的資料庫複本。

有各種工具來產生基準版本的 SQL 指令碼達成。 從 SQL Server Management Studio (SSMS) 您可以以滑鼠右鍵按一下資料庫，請移至 工作 子功能表，並選擇 產生指令碼選項。 這會啟動指令碼精靈，您可以指示來產生包含 s 物件建立資料庫的 SQL 命令的檔案。 另一個選項是資料庫發行精靈，可能會產生不只在資料庫資料表中建立資料庫結構描述，但是資料的 SQL 命令。 資料庫發行精靈 」 已仔細檢查回*部署資料庫*教學課程。 不論您使用何種工具，最後您應該有可用於重新建立您的資料庫的基準版本的指令碼檔案應該需要發生。

## <a name="documenting-the-database-changes-in-prose"></a>記載文章中的資料庫變更

在開發階段期間維持的資料模型的變更記錄最簡單的方式是將記錄文章中的變更。 例如，如果您已部署的應用程式開發期間將加入新資料行`Employees`資料表中移除資料行從`Orders`資料表，並加入新的資料表 (`ProductCategories`)，您會維護文字檔案或 Microsoft Word 文件具有下列的歷程記錄：

<a id="0.8_table01"></a>


| **變更日期** | **變更詳細資料** |
| --- | --- |
| 2009-02-03: | 加入的資料行`DepartmentID`(`int`，NOT NULL) 到`Employees`資料表。 加入從外部索引鍵條件約束`Departments.DepartmentID`至`Employees.DepartmentID`。 |
| 2009-02-05: | 移除資料行`TotalWeight`從`Orders`資料表。 中已擷取的資料相關聯`OrderDetails`記錄。 |
| 2009-02-12: | 建立`ProductCategories`資料表。 有三個資料行： `ProductCategoryID` (`int`， `IDENTITY`， `NOT NULL`)， `CategoryName` (`nvarchar(50)`， `NOT NULL`)，和`Active`(`bit`， `NOT NULL`)。 加入至主索引鍵條件約束`ProductCategoryID`，和預設值是 1 到`Active`。 |


有一些缺點。 簡單來說，是沒有希望自動化。 Anytime 這些變更需要套用至資料庫-例如，在部署應用程式，而開發人員必須以手動方式實作每個變更，一次。 此外，如果您需要重新建構基準使用變更日誌從資料庫的特定版本，這樣做會讓越來越多的時間隨著記錄的大小。 這個方法的另一個缺點是清楚以及每個變更記錄檔項目的詳細程度會保留給記錄變更的人員。 與多個開發人員小組在某些可能會比其他更詳細、 更容易閱讀，或更精確的項目。 此外，拼字錯誤和其他人相關的資料輸入錯誤，可能會。

記錄資料庫變更，文章中的主要優點是簡單。 您不 t 需要熟悉建立和改變資料庫物件的 SQL 語法。 相反地，您可以記錄文章中所做的變更，並加以實作透過 SQL Server Management Studio s 圖形化使用者介面。

維護您文章中的變更記錄檔，不可否認，不非常複雜而且獲利 t 用於特定專案中，例如，應該在範圍內，是大型的工作經常變更的資料模型，或是牽涉到多個開發人員。 但過小型、 經營的專案有只偶爾變更的資料模型和其中單人開發人員沒有強式背景中建立和改變資料庫物件的 SQL 語法相當也適用於這種方法。

> [!NOTE]
> 雖然變更記錄檔中的資訊，技術上來說，只有在需要部署階段建議保留變更的歷程記錄。 但是，而不是維護單一持續增長變更記錄檔，請考慮讓每個資料庫版本不同的變更記錄檔。 通常您會想要版本資料庫每次部署。 藉由維護的變更記錄檔記錄您可以從基準中，開始重新建立任何資料庫版本執行記錄變更的指令碼已從第 1 版開始並繼續進行，直到您到達版本需要重新建立。


## <a name="recording-the-sql-change-statements"></a>記錄 SQL 變更陳述式

維護文章中的變更記錄檔的主要缺點是缺乏自動化。 在理想情況下，實作生產資料庫，在部署階段的資料庫變更會很簡單，只要按一下按鈕即可執行指令碼，而不是需要以手動方式執行的指示清單。 這類的自動化就可以維護變更記錄檔，其中包含用來改變資料模型的 SQL 命令。

SQL 語法包含數個陳述式來建立和修改各種資料庫物件。 例如， [ *CREATE TABLE 陳述式*](https://msdn.microsoft.com/en-us/library/ms174979.aspx)、 在執行時，建立新的資料表，與指定的資料行和條件約束。 [ *ALTER TABLE 陳述式*](https://msdn.microsoft.com/en-us/library/ms190273.aspx)修改現有的資料表，加入、 移除或修改其資料行或條件約束。 另外還有陳述式來建立、 修改和卸除索引、 檢視、 使用者定義函數、 預存程序、 觸發程序和其他資料庫物件。

回到先前的範例中，影像加入至新的資料行的已部署應用程式開發期間`Employees`資料表中移除資料行從`Orders`資料表，並加入新的資料表 (`ProductCategories`)。 這類動作會導致下列的 SQL 命令以變更記錄檔：

[!code-sql[Main](strategies-for-database-development-and-deployment-vb/samples/sample1.sql)]

將這些變更推送到實際執行資料庫，在部署階段是一種單鍵操作： 開啟 SQL Server Management Studio、 連接到您的生產資料庫、 開啟 新增查詢 視窗、 貼上的內容變更記錄檔，再按一下 執行執行指令碼。

## <a name="using-a-comparison-tool-to-synchronize-the-data-models"></a>使用同步處理資料模型的比較工具

記錄資料庫變更文章中的很容易，但實作變更需要開發人員每一項變更其中一個在生產資料庫上一次。記載變更 SQL 命令可讓您方便而快速按一下按鈕，以在生產資料庫上執行這些變更，但需要學習和精通 SQL 陳述式和語法建立和改變資料庫物件。 資料庫的比較工具採取的最佳從這兩種方法，並捨棄最壞的情況。

資料庫的比較工具比較結構描述或兩個資料庫的資料，並顯示摘要報表，顯示資料庫有何不同。 然後，您也可以按一下按鈕時，您可以產生同步處理一或多個資料庫物件的 SQL 命令。 簡單地說，您可以使用資料庫的比較工具來比較開發和生產資料庫，在部署階段，產生的檔案，其中包含 SQL 命令，執行時，將變更套用到實際執行資料庫 s 結構描述，它反映開發資料庫 s 結構描述。

有各種不同的許多不同的廠商所提供的協力廠商資料庫比較工具。 其中一個這類的範例是[ *SQL 比較*](http://www.red-gate.com/products/SQL_Compare/)、 藉由[ *Red Gate Software*](http://www.red-gate.com/)。 可讓 s 逐步解說使用 SQL 比較來比較和同步處理的開發和生產資料庫結構描述的程序。

> [!NOTE]
> 在撰寫本文時目前版本的 SQL 比較已 7.1 版、 標準版為 $ 395 美元的成本計算方式。 您可以遵循下載免費的 14 天試用版。


啟動 SQL 比較時比較專案 對話方塊隨即開啟，顯示已儲存的 SQL 比較專案。 建立新的專案。 這會啟動此專案組態精靈，並提示要比較資料庫的相關資訊 （請參閱圖 1）。 輸入的開發和生產環境資料庫的資訊。


[![比較開發和生產資料庫](strategies-for-database-development-and-deployment-vb/_static/image2.jpg)](strategies-for-database-development-and-deployment-vb/_static/image1.jpg)

**圖 1**： 比較開發和生產資料庫 ([按一下以檢視完整大小的影像](strategies-for-database-development-and-deployment-vb/_static/image3.jpg))


> [!NOTE]
> 如果您開發環境的資料庫是 SQL Express Edition 資料庫檔案中的`App_Data`的網站，您必須在 SQL Server Express 資料庫伺服器中註冊資料庫，才能從 圖 1 所示的對話方塊中選取的資料夾。 完成這項作業的最簡單方式是開啟 SQL Server Management Studio (SSMS)、 連接到 SQL Server Express 資料庫伺服器，以及附加資料庫。 如果您不需要在電腦上安裝 SSMS 可以下載並安裝免費[ *SQL Server 2008 Management Studio Basic 版本*](https://www.microsoft.com/downloads/details.aspx?FamilyId=7522A683-4CB2-454E-B908-E805E9BD4E28&amp;displaylang=en)。


除了選取要比較的資料庫，您也可以指定各種不同的比較設定從 [選項] 索引標籤。您可能想要開啟的其中一個選項是 「 略過條件約束和索引名稱。 」 前文提過，前述教學課程中我們加入應用程式服務的開發和生產資料庫的資料庫物件。 如果您使用`aspnet_regsql.exe`生產資料庫上建立這些物件，則會找到開發和生產資料庫之間的主索引鍵和唯一條件約束名稱不同的工具。 因此，SQL 比較會加上旗標的所有應用程式服務資料表做為不同。 您可能可以將 「 略過條件約束和索引名稱 」 未核取和同步處理的條件約束名稱，或指示 SQL 比較要略過這些差異。

選取的資料庫之後比較 （和檢閱的比較選項），按一下 [現在比較] 按鈕，開始比較。 接下來的幾秒，透過 SQL 比較會檢查兩個資料庫的結構描述，並產生的報表有何不同。 我我們故意做了至開發資料庫，以顯示這類差異如何 SQL 比較介面中註明的一些修改。 如圖 2 所示，我已新增`BirthDate`資料行`Authors`移除資料表`ISBN`從資料行`Books`資料表，並加入新的資料表， `Ratings`，這要讓使用者造訪的網站速度檢閱過的活頁簿。

> [!NOTE]
> 在本教學課程中所做的資料模型變更是為了說明使用資料庫的比較工具。 您不會尋找資料庫中的這些變更在未來的教學課程。


[![SQL 比較列出開發和生產資料庫之間的差異](strategies-for-database-development-and-deployment-vb/_static/image5.jpg)](strategies-for-database-development-and-deployment-vb/_static/image4.jpg)

**圖 2**: SQL 比較列出開發之間的差異和實際執行資料庫 ([按一下以檢視完整大小的影像](strategies-for-database-development-and-deployment-vb/_static/image6.jpg))


SQL 比較細分成群組的資料庫物件，快速顯示哪些物件存在於這兩個資料庫但不同，物件存在於一個資料庫，但是，與哪些物件完全相同。 如您所見，有兩個資料庫中存在但不同的兩個物件：`Authors`資料表，在加入資料行，而`Books`使用其中一個移除的資料表。 只存在於開發資料庫，也就是新建立的一個物件`Ratings`資料表。 並沒有 117 完全相同的兩個資料庫中的物件。

選取的資料庫物件時，會顯示 SQL 差異視窗中，顯示這些物件有何不同。 SQL 差異視窗中，顯示在下方圖 2 中反白顯示，`Authors`開發資料庫中的資料表有`BirthDate`中找不到資料行`Authors`生產資料庫上的資料表。

檢閱這些差異，然後選取您想要同步處理哪些的物件下, 一個步驟之後產生的 SQL 命令需要更新生產資料庫 s 結構描述以符合開發資料庫。 這是透過同步處理精靈 」 來完成。 同步處理精靈 」 確認哪些物件加入至同步處理，並摘要說明動作計劃 （請參閱圖 3）。 您可以立即同步處理資料庫，或產生含有仍然可以同時執行的 SQL 命令的指令碼。


[![使用同步處理精靈 」 來同步處理您的資料庫結構描述](strategies-for-database-development-and-deployment-vb/_static/image8.jpg)](strategies-for-database-development-and-deployment-vb/_static/image7.jpg)

**圖 3**： 使用同步處理精靈 來同步處理您的資料庫結構描述 ([按一下以檢視完整大小的影像](strategies-for-database-development-and-deployment-vb/_static/image9.jpg))


資料庫的比較工具，例如 Red Gate Software 的 SQL 比較，請將變更套用至簡單，按一下，按一下 實際執行資料庫開發資料庫結構描述。

> [!NOTE]
> SQL 比較比較並同步處理兩個資料庫*結構描述*。 不幸的是，它不會比較並同步處理兩個資料庫資料表中的資料。 Red Gate Software 提供名為 product [ *SQL 資料比較*](http://www.red-gate.com/products/SQL_Data_Compare/)的比較並同步處理兩個資料庫之間的資料，但它是從 SQL 比較個別產品，另一個為 $ 395 美元的成本。


## <a name="taking-the-application-offline-during-deployment"></a>取得應用程式離線在部署期間

當我們我們看到這些教學課程中，整個部署是包含多個步驟的程序： 從開發環境的 ASP.NET 網頁、 主版頁面、 CSS 檔案、 JavaScript 檔案、 影像和其他必要的內容複製到生產環境環境。如果需要; 實際執行環境專屬的組態資訊，複製和上次部署後，將變更套用至資料模型。 根據檔案的數目和您的資料庫變更的複雜性，這些步驟長幾秒鐘的時間從數分鐘的時間才能完成。 在此期間 web 應用程式處於變動和瀏覽網站的使用者可能會發生錯誤或非預期的行為。

當部署網站最好必須完成部署之後，「 離線 」 才 web 應用程式。 離線應用程式 （並使其備份，一旦完成部署程序） 是容易，只要上傳檔案，然後再將其刪除。 開始使用 ASP.NET 2.0 中，只為的檔案是否存在`app_offline.htm`在應用程式根目錄接受整個網站 「 離線 」。 ASP.NET 頁面該站台上的任何要求自動回應的內容`app_offline.htm`檔案。 一旦移除該檔案時，應用程式恢復上線。

讓應用程式離線，部署期間很簡單，只上傳`app_offline.htm`檔案到生產環境 s 根目錄開始部署程序，並再將其刪除 （或為其他項目重新命名） 之前一次部署已完成。 如需這項技術的詳細資訊請參閱 John Peterson 的文章，採取[*離線 ASP.NET 應用程式*](http://www.15seconds.com/issue/061207.htm)。

## <a name="summary"></a>總結

將資料導向應用程式部署的主要挑戰著重於部署資料庫。 因為有兩個版本的資料庫-一個在開發環境中，一個實際執行環境中開發中加入新功能時，可能會同步這些兩個資料庫結構描述。 更多，因為實際執行的資料庫，做為要填入實際資料從實際的使用者，您無法覆寫生產資料庫修改的開發資料庫與部署應用程式 （ASP.NET 網頁，構成檔案時，就如同哪些 s映像檔案等等）。 相反地，將資料庫部署，需要實作的精確自從上次部署以來，對開發資料庫，在生產資料庫上的變更集合。

本教學課程探討了三個技巧來維護套用的資料庫變更記錄檔。 最簡單的方式是將記錄文章中的變更。 雖然這個策略，就會在實際執行資料庫手動程序上實作這些變更，它不需要知道 SQL 命令為建立和改變資料庫物件。 更複雜的方法，一個是在較大的專案或專案中與多位開發人員，更可行是記錄變更為一系列的 SQL 命令。 這可大幅 hastens 推出這些變更到目標資料庫。 達到最棒的是這兩種方法時，可以使用資料庫的比較工具，例如 Red Gate Software 的 SQL 比較。

本教學課程結束時，我們將焦點上部署的資料導向應用程式。 教學課程的下一組會查看如何在生產環境中的錯誤回應。 我們會探討如何顯示易懂的錯誤頁面，而是而不是黃色螢幕的死。 我們會看到如何記錄錯誤 s 詳細資料以及如何為這類錯誤發生時產生警示。

祝您程式設計 ！

>[!div class="step-by-step"]
[上一頁](configuring-a-website-that-uses-application-services-vb.md)
[下一頁](displaying-a-custom-error-page-vb.md)
