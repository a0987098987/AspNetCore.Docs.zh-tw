---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
title: '反覆項目 #6 – 使用測試為導向的開發 (VB) |Microsoft 文件'
author: microsoft
description: 這個第六個反覆項目中我們將新功能加入我們的應用程式藉由撰寫單元測試的第一次，並撰寫單元測試的程式碼。 在這個反覆項目，...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: e1fd226f-3f8e-4575-a179-5c75b240333d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-vb
msc.type: authoredcontent
ms.openlocfilehash: 71b3425c5ca8cbfc1b89493c7afb26681f8bdc9d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877586"
---
<a name="iteration-6--use-test-driven-development-vb"></a>反覆項目 #6 – 使用測試為導向的開發 (VB)
====================
by [Microsoft](https://github.com/microsoft)

[下載程式碼](iteration-6-use-test-driven-development-vb/_static/contactmanager_6_vb1.zip)

> 這個第六個反覆項目中我們將新功能加入我們的應用程式藉由撰寫單元測試的第一次，並撰寫單元測試的程式碼。 在這個反覆項目，我們會加入連絡人群組。


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>建立連絡人管理 ASP.NET MVC 應用程式 (VB)
  

在這一系列的教學課程中，我們會建置整個連絡人管理應用程式從開始到完成。 請連絡系統管理員應用程式可讓您商店連絡資訊的名稱、 電話號碼和電子郵件地址-的人員清單。

我們會建置應用程式透過多個反覆項目。 與每個反覆項目，我們會逐漸改善應用程式。 這個多個反覆項目方法的目標是要讓您了解每個變更的原因。

- 反覆項目 #1-建立應用程式。 在第一次反覆運算中，我們建立連絡人管理員簡單的方式可能。 我們加入基本資料庫作業的支援： 建立、 讀取、 更新和刪除 (CRUD)。

- 反覆項目 #2-請看起來很棒的應用程式。 在這個反覆項目，我們可以改進應用程式的外觀修改 ASP.NET MVC 檢視主版頁面的預設值和階層式樣式表。

- 反覆項目 #3-加入表單驗證。 第三個反覆項目中，我們會加入基本表單驗證。 我們可以防止使用者提交表單，而不會完成必要的表單欄位。 此外，我們也會驗證電子郵件地址和電話號碼。

- 反覆項目 #4-請鬆散偶合的應用程式。 在此第三個反覆項目中，我們利用數種軟體設計模式，讓您更輕鬆地維護及修改連絡人管理員應用程式。 例如，我們可以重構應用程式使用的儲存機制模式和相依性插入模式。

- 反覆項目 #5-建立單元測試。 第五個反覆項目中，我們在我們的應用程式更輕鬆地維護及修改加入單元測試。 我們模擬資料模型類別，並建立單元測試，我們的控制器和驗證邏輯。

- 反覆項目 #6-使用測試為導向的開發。 這個第六個反覆項目中我們將新功能加入我們的應用程式藉由撰寫單元測試的第一次，並撰寫單元測試的程式碼。 在這個反覆項目，我們會加入連絡人群組。

- 反覆項目 #7： 加入 Ajax 功能。 在第七個反覆項目，改善回應性和效能的應用程式藉由新增 Ajax 支援。

## <a name="this-iteration"></a>這個反覆項目

在連絡人管理員應用程式的上一個反覆項目，我們會建立單元測試，以提供我們的程式碼的安全網。 建立單元測試的動機是為了讓我們的程式碼更有彈性，來變更。 單元測試，我們可以愉快地保存他們在我們的程式碼中進行任何變更，並立即知道我們是否有中斷現有的功能。

在這個反覆項目，我們可以使用 單元測試完全不同的用途。 在這個反覆項目，使用單元測試的呼叫應用程式設計原理一部分*測試導向開發*。 當您練習測試導向開發時，您會先撰寫測試，並再撰寫測試的程式碼。

更明確地說，當剛好發言測試導向開發，有三個步驟完成建立程式碼時 (紅 / 綠/重構):

1. 撰寫單元測試失敗 （紅色）
2. 撰寫程式碼的單元測試 （綠色）
3. 重構程式碼 （重構）

首先，您會撰寫單元測試。 單元測試應該表達您不需要針對您希望您的程式碼如何運作。 當您建立單元測試時，單元測試應該會失敗。 測試應該會失敗，因為但是尚未寫入測試任何應用程式程式碼。

接下來，您可以撰寫剛好足夠的程式碼單元測試通過的順序。 目標是 laziest、 sloppiest 和較快的可能方式撰寫的程式碼。 您應該不會浪費時間思考應用程式的架構。 相反地，您應該專注於撰寫少的程式碼不必滿足單元測試來表示的意圖。

最後，撰寫程式碼之後，您可以回溯，並考慮您的應用程式的整體架構。 在此步驟中，重寫 （重構） 藉由運用軟體設計的程式碼模式-儲存機制模式-例如，讓您的程式碼更容易維護。 因為單元測試所涵蓋的程式碼，您 fearlessly 可以重寫程式碼，在此步驟。

有許多優點所導致的練習測試為導向的開發。 第一個步驟，以測試為導向的開發會強迫您專注於實際需要撰寫的程式碼。 因為您經常會著重於只撰寫程式碼來傳遞特定的測試，您無法從踏入檢討和寫入大量不會使用您的程式碼。

第二，「 第一次測試 」 的設計方法也會強迫您撰寫的程式碼的使用方式觀點來從程式碼。 換句話說，當剛好發言測試導向開發，您要從使用者的觀點不斷地撰寫您的測試。 因此，測試為導向的開發會導致較為簡潔且更容易了解應用程式開發介面。

最後，測試為導向的開發會強迫您撰寫單元測試做為撰寫的應用程式的一般程序的一部分。 專案期限接近時，測試是通常超出視窗第一件事。 當剛好發言測試導向開發，相反地，您就比較可能是正向撰寫單元測試，因為測試導向開發會讓單元測試中央建置的應用程式的程序的相關。

> [!NOTE] 
> 
> 若要深入了解測試為導向的應用程式開發，建議您先閱讀 Michael Feathers 書籍**工作有效率地使用舊版程式碼中**。


在這個反覆項目，我們加入我們的連絡人管理員應用程式的新功能。 我們新增連絡人群組的支援。 您可以使用連絡人群組組織成類別目錄，例如商務連絡人與朋友群組。

我們會遵循的測試為導向的開發程序，將這項新功能新增至我們的應用程式。 我們會將第一次寫入我們的單元測試中，我們要撰寫所有程式碼對這些測試。

## <a name="what-gets-tested"></a>取得測試的項目

如我們所討論的上一個反覆項目，您通常不撰寫資料存取邏輯單元測試或檢視邏輯。 您不 t 寫入單元測試的資料存取邏輯，因為存取資料庫的相對比較慢的作業。 您不檢視邏輯 t 寫入單元測試，因為存取檢視需要才能組織好的 web 伺服器不是相對比較慢的作業。 除非可以執行測試一再重複速度非常快，您應該不 t 撰寫的單元測試

由於測試為導向的開發由單元測試所驅動的則我們一開始專注於撰寫控制器和商務邏輯。 我們避免碰觸的資料庫或檢視表。 我們贏了 t 修改資料庫或建立本教學課程結束前我們的檢視。 我們的開頭可以測試的項目。

## <a name="creating-user-stories"></a>建立使用者劇本

當剛好發言測試導向開發，您一律從開始撰寫測試。 這會立即引發問題： 如何決定哪個先撰寫測試？ 若要回答這個問題，您應該撰寫一組[*使用者劇本*](http://en.wikipedia.org/wiki/User_stories)。

使用者劇本會是非常簡短的軟體需求 （通常是一個句子） 描述。 其應為非技術性的需求，撰寫從使用者的觀點描述。

以下說明新的連絡人群組功能所需的功能的使用者劇本的集：

1. 使用者可以檢視連絡人群組的清單。
2. 使用者可以建立新的連絡人群組。
3. 使用者可以刪除現有的連絡人群組。
4. 建立新的連絡人時，使用者可以選取連絡人的群組。
5. 編輯現有連絡人時，使用者可以選取連絡人的群組。
6. 連絡人群組的清單會顯示在索引檢視中。
7. 當使用者按一下連絡人群組時，則會顯示比對的連絡人清單。

請注意，使用者劇本的這份清單是完全可了解客戶。 沒有未提及的技術實作詳細資料。

在程序時建立您的應用程式，使用者劇本的集可能會變得更精簡。 您可能會分成多個劇本 （需求） 的使用者劇本。 例如，您可能決定建立新的連絡人群組應該包含驗證。 送出沒有名稱的連絡人群組應該會傳回驗證錯誤。

建立使用者劇本清單之後，您就準備好開始撰寫第一個單元測試。 我們會先建立單元測試，來檢視連絡人群組的清單。

## <a name="listing-contact-groups"></a>列出連絡人群組

我們第一個使用者劇本是使用者應該能夠檢視連絡人群組的清單。 我們需要 express 此劇本與測試。

建立新的單元測試，以滑鼠右鍵按一下 [控制器] 資料夾在 ContactManager.Tests 專案中，選取**新增]、 [新增測試**，然後選取**單元測試**範本 （請參閱圖 1）。 新的單元測試 GroupControllerTest.vb，再按一下**確定** 按鈕。


[![GroupControllerTest 單元測試](iteration-6-use-test-driven-development-vb/_static/image1.jpg)](iteration-6-use-test-driven-development-vb/_static/image1.png)

**圖 01**: GroupControllerTest 單元測試 ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-vb/_static/image2.png))


第一個單元測試會包含在程式碼範例 1。 這項測試會驗證群組控制器的 index （） 方法傳回的一組群組。 測試會驗證，要資料檢視中傳回群組的集合。

**Listing 1 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample1.vb)]

當您第一次輸入程式碼，在 Visual Studio 中的列表 1 中時，您會取得大量的紅色曲線。 我們不會建立 GroupController 或群組的類別。

此時，我們可以 t 甚至組建我們這樣我們就可以 t 的應用程式執行的第一個單元測試。 S 良好。 會算為失敗的測試。 因此，我們現在有開始撰寫應用程式程式碼的權限。 我們必須撰寫程式碼來執行我們的測試。

列出 2 中的群組控制器類別包含程式碼將單元測試所需的最低限度。 Index 動作傳回群組 （群組類別定義中列出的 3） 以靜態方式自動程式碼的的清單。

**Listing 2 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample2.vb)]

**Listing 3 - Models\Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample3.vb)]

我們將為 GroupController 和群組類別加入至受測專案之後，第一個單元測試成功完成 （請參閱圖 2）。 我們已經通過的測試所需的最小工作。 它是慶祝的時間。


[![Success!](iteration-6-use-test-driven-development-vb/_static/image2.jpg)](iteration-6-use-test-driven-development-vb/_static/image3.png)

**圖 02**： 成功 ！ ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-vb/_static/image4.png))


## <a name="creating-contact-groups"></a>建立連絡人群組

現在我們可以移至第二個使用者劇本。 我們需要能夠建立新連絡人的群組。 我們需要這個意圖 express 與測試。

列出的 4 中的測試會驗證函式呼叫 create （） 方法與新的群組將群組加入至群組 index （） 方法所傳回的清單。 換句話說，如果建立新的群組則我應該能夠從 index （） 方法所傳回的群組的清單取得新的群組。

**Listing 4 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample4.vb)]

列出的 4 中的測試會呼叫群組控制站與新的連絡人群組 create （） 方法。 接下來，測試會驗證，群組控制站 index （） 方法的呼叫會傳回新的群組中檢視資料。

已修改的群組控制站，列出 5 中所包含變更傳遞新的測試所需的最低限度。

**Listing 5 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample5.vb)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>列出 5 中的群組控制站有新的 create （） 動作。 這個動作會將群組加入至群組的集合。 請注意，已修改的 index 動作傳回的群組集合的內容。

同樣地，我們也可以執行通過的單元測試所需的最低限度。 我們的群組控制站進行這些變更之後，所有我們的單元測試成功。

## <a name="adding-validation"></a>新增驗證

在使用者劇本，已不明確陳述這項需求。 不過，它可合理地需要群組有名稱。 否則，連絡人組織成群組將不會非常有用。

列出 6 包含新的測試表達此意圖。 這項測試會驗證嘗試建立群組，而不需要提供名稱會導致模型狀態中的驗證錯誤訊息。

**Listing 6 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample6.vb)]

為了滿足這項測試，我們需要將 Name 屬性新增至群組類別 （請參閱程式碼範例 7）。 此外，我們需要將稍微不同的驗證邏輯加入至群組 controller s create （） 動作 （請參閱列出 8）。

**Listing 7 - Models\Group.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample7.vb)]

**Listing 8 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample8.vb)]

請注意，群組控制站現在 create （） 動作包含驗證和資料庫的邏輯。 目前，群組控制站所使用的資料庫包含記憶體中集合而已。

## <a name="time-to-refactor"></a>要重構的時間

第三步中紅/綠/重構是重構部分。 此時，我們需要的程式碼，並考慮我們可以重構我們改善其設計的應用程式的方式。 重構階段是在我們認為硬碟的最佳方式，實作軟體設計的原則和模式的階段。

我們可以自由修改任何方式來改善程式碼的設計，我們選擇的程式碼。 我們有防止我們中斷現有功能的單元測試的安全網。

現在，我們的群組控制站是好的軟體設計的觀點混亂。 群組控制站包含盤的混亂的驗證和資料存取程式碼。 若要避免違反單一責任原則，我們需要將這些考量分成不同類別。

我們已重構的群組控制器類別包含在列出 9。 控制器已修改成使用 ContactManager 服務層。 這是我們與連絡人控制器所使用的相同服務層。

列出 10 包含新方法加入至 ContactManager 服務層，以支援驗證、 列出和建立群組。 IContactManagerService 介面已更新以包含新的方法。

列出 11 包含新的 FakeContactManagerRepository 類別可實作 IContactManagerRepository 介面。 不同於 EntityContactManagerRepository 類別也實作 IContactManagerRepository 介面，新 FakeContactManagerRepository 類別不會不會與資料庫通訊。 FakeContactManagerRepository 類別會做為 proxy 的記憶體中集合使用資料庫。 我們將在我們的單元測試中使用這個類別，像是假的儲存機制的層級。

**Listing 9 - Controllers\GroupController.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample9.vb)]

**Listing 10 - Controllers\ContactManagerService.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample10.vb)]

**Listing 11 - Controllers\FakeContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample11.vb)]


修改 IContactManagerRepository 介面需要使用在 EntityContactManagerRepository 類別中實作的 CreateGroup() 和 ListGroups() 方法。 Laziest 且最快的方法是新增虛設常式方法，看起來像這樣：

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample12.vb)]


最後，這些變更，我們的應用程式的設計會要求我們對我們的單元測試中進行一些修改。 我們現在需要執行單元測試時使用 FakeContactManagerRepository。 更新的 GroupControllerTest 類別被包含在列出 12。

**Listing 12 - Controllers\GroupControllerTest.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample13.vb)]

我們進行所有這些之後，變更同樣地，所有我們單元測試成功。 我們已經完成紅/綠/重構的整個週期。 我們已經實作的第一個的兩個使用者劇本。 我們現在有支援單元測試中的使用者劇本所表示的需求。 實作使用者劇本的其餘部分，必須重複相同的循環的 紅/綠/重構。

## <a name="modifying-our-database"></a>修改資料庫

不幸的是，即使我們已符合所有表示我們的單元測試的需求，我們是未完成的工作。 我們仍需要修改資料庫。

我們需要建立新群組的資料庫資料表。 請依照下列步驟：

1. 在 [伺服器總管] 視窗中，以滑鼠右鍵按一下 [資料表] 資料夾並選取功能表選項**加入新的資料表**。
2. 輸入 資料表設計工具，如下所述的兩個資料行。
3. 識別碼資料行標示為 primary key 和識別資料行。
4. 磁碟的圖示，即可儲存群組命名新的資料表。

<a id="0.12_table01"></a>


| **資料行名稱** | **資料類型** | **允許 null 值** |
| --- | --- | --- |
| ID | int | False |
| 名稱 | Nvarchar （50) | False |


接下來，我們需要從 [連絡人] 資料表中刪除所有的資料 (否則我們贏了將無法建立連絡人和群組的資料表之間的關聯性)。 請依照下列步驟：

1. 以滑鼠右鍵按一下 [連絡人] 資料表，然後選取功能表選項**顯示資料表資料**。
2. 刪除所有資料列。

接下來，我們必須定義在群組的資料庫資料表和現有的連絡人資料庫資料表之間的關聯性。 請依照下列步驟：

1. 按兩下以開啟 資料表設計工具的 伺服器總管 視窗中的 連絡人 資料表。
2. 將新的整數資料行加入至 [連絡人] 資料表名為 GroupId。
3. 按一下 [關聯] 按鈕以開啟外部索引鍵關聯性對話方塊 （請參閱圖 3）。
4. 按一下 [新增] 按鈕。
5. 按一下 [顯示資料表和資料行規格] 按鈕旁邊的省略符號按鈕。
6. 在 資料表和資料行 對話方塊中，選取 群組做為的主索引鍵資料表和主索引鍵資料行的識別碼。 選取連絡人做為外部索引鍵資料表和外部索引鍵資料行的識別碼 （請參閱圖 4）。 按一下 [確定] 按鈕。
7. 在下**INSERT 及 UPDATE 規格**，選取值**Cascade**如**刪除規則**。
8. 按一下 [關閉] 按鈕以關閉 [外部索引鍵關聯性] 對話方塊。
9. 按一下 [儲存] 按鈕將變更儲存到 [連絡人] 資料表。


[![建立資料庫資料表關聯性](iteration-6-use-test-driven-development-vb/_static/image3.jpg)](iteration-6-use-test-driven-development-vb/_static/image5.png)

**圖 03**： 建立資料庫資料表關聯性 ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-vb/_static/image6.png))


[![指定資料表關聯性](iteration-6-use-test-driven-development-vb/_static/image4.jpg)](iteration-6-use-test-driven-development-vb/_static/image7.png)

**圖 04**： 指定資料表關聯性 ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-vb/_static/image8.png))


### <a name="updating-our-data-model"></a>更新我們的資料模型

接下來，我們需要更新我們的資料模型，以代表新的資料庫資料表。 請依照下列步驟：

1. 按兩下 ContactManagerModel.edmx 檔案以開啟 Entity Designer 的 模型 資料夾中。
2. 在設計工具介面上按一下滑鼠右鍵，然後選取功能表選項**從資料庫更新模型**。
3. 在更新精靈中，選取群組的資料表，並按一下 [完成] 按鈕 （請參閱圖 5）。
4. 以滑鼠右鍵按一下群組實體，然後選取功能表選項**重新命名**。 變更名稱*群組*實體*群組*（單數）。
5. 以滑鼠右鍵按一下連絡人實體下方會顯示群組導覽屬性。 變更名稱*群組*導覽屬性來*群組*（單數）。


[![更新資料庫中的 Entity Framework 模型](iteration-6-use-test-driven-development-vb/_static/image5.jpg)](iteration-6-use-test-driven-development-vb/_static/image9.png)

**圖 05**： 更新資料庫中的 Entity Framework 模型 ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-vb/_static/image10.png))


完成這些步驟之後，您的資料模型將代表連絡人和群組的資料表。 實體設計工具應該會顯示這兩個實體 （請參閱圖 6）。


[![顯示群組和連絡人的實體設計工具](iteration-6-use-test-driven-development-vb/_static/image6.jpg)](iteration-6-use-test-driven-development-vb/_static/image11.png)

**圖 06**： 顯示群組和連絡人的實體設計工具 ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-vb/_static/image12.png))


### <a name="creating-our-repository-classes"></a>建立儲存機制類別

接下來，我們需要實作儲存機制類別。 進行這個反覆項目期間，我們加入數個新方法 IContactManagerRepository 介面撰寫程式碼，以符合我們的單元測試時。 IContactManagerRepository 介面的最終版本會包含在程式碼範例 14。

**列出 14-Models\IContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample14.vb)]

我們樂園 t 實際實作任何有關連絡人群組，我們實際 EntityContactManagerRepository 類別中使用的方法。 目前，EntityContactManagerRepository 類別具有虛設常式方法每 IContactManagerRepository 介面中所列的連絡人群組方法。 例如，ListGroups() 方法目前看起來像這樣：

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample15.vb)]

虛設常式方法會啟用我們編譯我們的應用程式，並傳遞單元測試。 不過，現在它是以實際實作這些方法的時間。 EntityContactManagerRepository 類別的最終版本會包含在列出 13。

**Listing 13 - Models\EntityContactManagerRepository.vb**

[!code-vb[Main](iteration-6-use-test-driven-development-vb/samples/sample16.vb)]

### <a name="creating-the-views"></a>建立檢視

當您使用預設 ASP.NET 檢視引擎 ASP.NET MVC 應用程式。 因此，您不要建立檢視以回應特定的單元測試。 不過，因為應用程式將不具有檢視毫無用處，我們可以 t 完成這個反覆項目，而不需要建立和修改連絡人管理員應用程式中包含的檢視。

我們需要建立下列新的檢視來管理連絡人群組 （請參閱圖 7）：

- Views\Group\Index.aspx-連絡人群組的顯示清單
- Views\Group\Delete.aspx-刪除連絡人群組的顯示確認表單


[![群組索引檢視](iteration-6-use-test-driven-development-vb/_static/image7.jpg)](iteration-6-use-test-driven-development-vb/_static/image13.png)

**圖 07**: 群組索引檢視 ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-vb/_static/image14.png))


我們需要修改下列現有檢視，讓它們包含連絡人群組：

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

您可以藉由查看此教學課程隨附的 Visual Studio 應用程式查看修改過的檢視。 例如，圖 8 說明連絡人索引檢視。


[![連絡人索引檢視](iteration-6-use-test-driven-development-vb/_static/image8.jpg)](iteration-6-use-test-driven-development-vb/_static/image15.png)

**圖 08**: 連絡人索引檢視 ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-vb/_static/image16.png))


## <a name="summary"></a>總結

在這個反覆項目，我們加入的新功能為連絡人管理員應用程式遵循測試為導向的開發應用程式的設計方法。 我們一開始會建立一組使用者劇本。 我們建立單元測試集對應至使用者劇本所表示的需求。 最後，我們會撰寫剛好足夠的程式碼，來滿足表示單元測試的需求。

我們已完成寫入程式碼來滿足表示單元測試的需求之後，我們已更新我們的資料庫和檢視表。 我們加入到資料庫的新群組的資料表，並更新我們 Entity Framework 資料模型。 我們也會建立和修改一組檢視。

在下一個反覆項目-最後一個反覆項目-我們重新撰寫以善用 Ajax 應用程式。 利用 Ajax，我們將會改善回應性和連絡人管理員應用程式的效能。

> [!div class="step-by-step"]
> [上一頁](iteration-5-create-unit-tests-vb.md)
> [下一頁](iteration-7-add-ajax-functionality-vb.md)
