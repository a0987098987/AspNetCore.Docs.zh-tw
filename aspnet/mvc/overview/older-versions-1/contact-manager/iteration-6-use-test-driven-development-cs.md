---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
title: '反覆項目 #6 – 使用測試導向開發 (C#) |Microsoft Docs'
author: microsoft
description: 在這個第六個反覆項目中，我們新功能加入我們的應用程式方法是先撰寫單元測試，並撰寫單元測試的程式碼。 在此反覆項目，...
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: 013c3c26-7dc3-41d1-8064-f233c86008b5
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
msc.type: authoredcontent
ms.openlocfilehash: 4e07b23931d487ba3875bd96606b8329d84485cf
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/01/2018
ms.locfileid: "39395983"
---
<a name="iteration-6--use-test-driven-development-c"></a>反覆項目 #6 – 使用測試導向開發 (C#)
====================
by [Microsoft](https://github.com/microsoft)

[下載程式碼](iteration-6-use-test-driven-development-cs/_static/contactmanager_6_cs1.zip)

> 在這個第六個反覆項目中，我們新功能加入我們的應用程式方法是先撰寫單元測試，並撰寫單元測試的程式碼。 在這個反覆項目，我們會新增連絡人群組。


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>建立連絡人管理 ASP.NET MVC 應用程式 (C#)
  

在本系列教學課程中，我們會建置整個連絡人管理應用程式從開始到完成。 請連絡系統管理員應用程式可讓您商店連絡資訊的名稱、 電話號碼和電子郵件地址-的人員清單。

我們會建置應用程式透過多個反覆項目。 每次反覆運算時，我們會逐漸改善應用程式。 此多個反覆項目方法的目標是要讓您了解每個變更的原因。

- 反覆項目 #1-建立應用程式。 在第一次反覆運算中，我們建立連絡人管理員中的最簡單的方式可能。 我們加入基本資料庫作業的支援： 建立、 讀取、 更新和刪除 (CRUD)。

- 反覆項目 #2-讓應用程式看起來不錯。 這個反覆項目，我們以改善應用程式的外觀的修改預設的 ASP.NET MVC 檢視主版頁面和階層式樣式表。

- 反覆項目 #3-新增表單驗證。 在第三個反覆項目，我們會加入基本表單驗證。 我們可以防止使用者提交表單，而不會完成必要的表單欄位。 此外，我們也會驗證電子郵件地址和電話號碼。

- 反覆項目 #4-進行鬆散偶合的應用程式。 在這個第四個反覆項目中，我們利用數種軟體設計模式，以讓它更容易維護及修改連絡人管理員應用程式。 比方說，我們可以重構應用程式使用儲存機制模式和相依性插入模式。

- 反覆項目 #5-建立單元測試。 在第五個反覆項目中，我們讓我們的應用程式容易維護及修改藉由新增單元測試。 我們模擬我們的資料模型類別，並建置我們的控制器和驗證邏輯單元測試。

- 反覆項目 #6-使用測試導向開發。 在這個第六個反覆項目中，我們新功能加入我們的應用程式方法是先撰寫單元測試，並撰寫單元測試的程式碼。 在這個反覆項目，我們會新增連絡人群組。

- 反覆項目 #7-新增 Ajax 功能。 在第七個反覆項目，改善回應性和我們的應用程式的效能透過新增對 Ajax 支援。

## <a name="this-iteration"></a>這個反覆項目

在連絡人管理員應用程式的上一個反覆項目，我們會建立單元測試，以提供我們的程式碼防護機制。 用於建立單元測試的動機是讓我們的程式碼變更更有彈性。 使用就地的單元測試，我們可以值得高興的是我們的程式碼進行任何變更，並立即了解我們的電腦已中斷的現有功能。

這個反覆項目，在中，我們會使用單元測試完全不同的用途。 在這個反覆項目，我們會使用單元測試做為一部分的呼叫的應用程式的設計原理*測試導向開發*。 當您將練習測試導向開發時，您會先撰寫測試，並再撰寫測試的程式碼。

更精確地說，當練習測試導向開發，有三個步驟完成時建立的程式碼 (紅色 / 綠色/重構):

1. 撰寫單元測試失敗 （紅色）
2. 撰寫程式碼，通過單元測試 （綠色）
3. 重構程式碼 (Refactor)

首先，您可以撰寫單元測試。 單元測試應該表達您打算如您預期您的程式碼如何運作。 當您第一次建立單元測試時，單元測試應該會失敗。 測試應該會失敗，因為但是尚未寫入測試任何應用程式程式碼。

接下來，您可以撰寫剛好足夠的程式碼以便將單元測試。 目標是 laziest、 sloppiest 又最快速的可能方式撰寫的程式碼。 您應該不會浪費時間來思考您的應用程式的架構。 相反地，您應該專注於撰寫少量的程式碼不必滿足單元測試所表示的意圖。

最後，您已撰寫足夠的程式碼之後，您可以回頭考慮您的應用程式的整體架構。 在此步驟中，重寫 (refactor) 您的程式碼利用軟體設計模式-儲存機制模式-例如，讓您的程式碼更容易維護。 您可以放心地請重寫您的程式碼，在此步驟中因為單元測試所涵蓋的程式碼。

有許多因練習測試導向開發的許多優點。 首先，以測試為導向的開發會強迫您專注於真正需要撰寫程式碼。 因為您經常會著重於只撰寫不足，無法將特定測試的程式碼，您無法從踏入檢討和寫入大量您永遠不會使用程式碼。

其次，「 第一次測試 」 的設計方法會強制您撰寫程式碼從您的程式碼的使用方式觀點來看。 換句話說，當練習測試導向開發，您要從使用者觀點不斷地撰寫測試。 因此，測試導向開發可能會導致更簡潔且更容易了解的 Api。

最後，測試導向開發也會強迫您撰寫單元測試做為撰寫的應用程式的一般程序的一部分。 隨著專案期限將近，測試是通常會在視窗第一件事。 當練習測試導向開發，相反地，您會更有可能是良性的相關撰寫單元測試，因為測試導向開發對單元測試集中建置的應用程式的程序。

> [!NOTE] 
> 
> 若要深入了解測試導向開發，建議您閱讀 Michael Feathers 書籍**Working Effectively with Legacy 程式碼**。


在此反覆項目，我們新增連絡人管理員應用程式的新功能。 我們新增連絡人群組的支援。 您可以使用連絡人群組，以將您的連絡人組織成類別目錄，例如業務和 Friend 群組。

我們將新增至應用程式的這項新功能，依照測試導向開發的程序。 我們將先撰寫我們的單元測試，我們將撰寫所有我們針對這些測試的程式碼。

## <a name="what-gets-tested"></a>接受測試的基準

如我們所討論的上一個反覆項目，您通常不撰寫資料存取邏輯的單元測試或檢視邏輯。 您不針對資料存取邏輯的 t 寫入單元測試，因為存取資料庫的相對比較慢的作業。 因為存取檢視需要加速作業相當緩慢的 web 伺服器，您可以不檢視邏輯的 t 寫入單元測試。 除非可以執行測試一再重複速度非常快，您應該不 t 撰寫的單元測試

因為測試為導向的開發由單元測試，則我們一開始專注於撰寫控制站和商務邏輯。 我們避免碰觸的資料庫或檢視表。 我們已贏得 t 修改資料庫或建立我們的檢視，直到本教學課程中的最尾端。 我們開始項目可進行測試。

## <a name="creating-user-stories"></a>建立使用者劇本

當練習測試導向開發，您一律從開始撰寫測試。 這是立即引發問題： 如何決定先撰寫測試？ 為了回答這個問題，您應該撰寫一組[**使用者劇本**](http://en.wikipedia.org/wiki/User_stories)。

使用者劇本是非常短暫的軟體需求 （通常是一個句子） 描述。 它應該是非技術性需求，從使用者觀點來看撰寫的描述。

以下說明新的連絡人群組功能所需的功能的使用者劇本的集合：

1. 使用者可以檢視連絡人群組的清單。
2. 使用者可以建立新的連絡人群組。
3. 使用者可以刪除現有的連絡人群組。
4. 建立新的連絡人時，使用者可以選取連絡人的群組。
5. 編輯現有的連絡人時，使用者可以選取連絡人的群組。
6. 連絡人的群組清單會顯示在 [索引] 檢視中。
7. 當使用者按一下連絡人群組時，則會顯示相符的連絡人的清單。

請注意，此清單中的使用者劇本客戶完全理解。 沒有任何值得一提的技術實作詳細資料。

而在建置您的應用程式，使用者劇本的集合可能會變得更精細。 您可能會分成多個劇本 （需求） 的使用者劇本。 例如，您可能決定建立新的連絡人群組應該包含驗證。 提交連絡人群組沒有名稱應該會傳回驗證錯誤。

您建立的使用者劇本清單之後，您就可以開始撰寫第一個單元測試。 我們先建立檢視的 連絡人群組清單的單元測試。

## <a name="listing-contact-groups"></a>清單連絡人的群組

我們的第一個使用者劇本是使用者應該能夠檢視連絡人群組的清單。 我們需要表達此劇本與測試。

建立新的單元測試，以滑鼠右鍵按一下 [控制器] 資料夾，在 ContactManager.Tests 專案中，選取**新增]、 [新增測試**，然後選取**單元測試**範本 （見 [圖 1]）。 名稱，新的單元測試 GroupControllerTest.cs，然後按一下**確定** 按鈕。


[![新增 GroupControllerTest 單元測試](iteration-6-use-test-driven-development-cs/_static/image1.jpg)](iteration-6-use-test-driven-development-cs/_static/image1.png)

**圖 01**： 新增 GroupControllerTest 單元測試 ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-cs/_static/image2.png))


第一個單元測試會包含在程式碼範例 1。 此測試會驗證群組控制器的 index （） 方法會傳回一組群組。 測試會驗證，群組就會傳回集合檢視中的資料。

**列表 1-Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample1.cs)]

當您第一次輸入程式碼，在 Visual Studio 中的列表 1 中時，您會取得大量的紅色曲線。 我們不會建立 GroupController 或群組的類別。

到目前為止，我們可以 t 甚至建置我們的應用程式讓我們可以 t 執行的第一個單元測試。 該良好的 s。 會被視為失敗的測試。 因此，我們現在有開始撰寫應用程式程式碼的權限。 我們必須撰寫足夠執行我們的測試的程式碼。

在 列表 2 中的群組控制器類別包含通過單元測試所需的程式碼的最低限度。 Index （） 動作傳回靜態程式碼的清單 （群組類別定義在 列表 3） 的群組。

**列表 2-Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample2.cs)]

**列表 3-Models\Group.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample3.cs)]

我們將 GroupController 和群組類別新增至我們的專案之後，第一個單元測試成功完成 （請參閱 圖 2）。 我們已經通過測試所需的最小工作。 它是要慶祝的時間。


[![成功 ！](iteration-6-use-test-driven-development-cs/_static/image2.jpg)](iteration-6-use-test-driven-development-cs/_static/image3.png)

**圖 02**： 成功 ！ ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-cs/_static/image4.png))


## <a name="creating-contact-groups"></a>建立連絡人群組

現在我們可以移至第二個使用者劇本。 我們必須要能夠建立新的連絡人群組。 我們需要與測試 express 這個意圖。

列表 4 中的測試會驗證呼叫 create （） 方法，以新的群組將群組加入到 index （） 方法所傳回的群組清單。 換句話說，如果我建立新的群組然後我應該能夠從 index （） 方法所傳回的群組清單取得新的群組。

**列表 4-Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample4.cs)]

列表 4 中的測試會呼叫的群組控制站與新的連絡人群組的 create （） 方法。 接下來，測試會驗證檢視資料，呼叫群組控制器 index （） 方法會傳回新的群組。

列表 5 中已修改的群組控制站會包含最低限度將新的測試所需的變更。

**列表 5-Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample5.cs)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>表 5 中的群組控制站會有一個新的 create （） 動作。 此動作會將群組加入群組的集合。 請注意，已修改的 index （） 動作来傳回的群組集合的內容。

同樣地，我們可以執行通過單元測試所需的最低限度。 我們的群組控制站進行這些變更之後，所有我們的單元測試成功。

## <a name="adding-validation"></a>新增驗證

在 使用者劇本，是不明確陳述這項需求。 不過，很合理地需要群組都具有一個名稱。 否則，連絡人組織成群組不是很有幫助。

列表 6 包含新的測試表示這個意圖。 這項測試會驗證嘗試建立群組，而不需要提供名稱會導致模型狀態中的驗證錯誤訊息。

**列表 6-Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample6.cs)]

為了滿足這項測試，我們必須將名稱屬性加入到群組類別 （請參閱列表 7）。 此外，我們需要將少許的驗證邏輯新增至群組控制器 s create （） 動作 （請參閱表 8）。

**列表 7-Models\Group.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample7.cs)]

**列表 8-Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample8.cs)]

請注意群組控制器現在 create （） 動作包含驗證和資料庫的邏輯。 目前，群組控制站所使用的資料庫包含不超過記憶體中集合。

## <a name="time-to-refactor"></a>重構的時間

第三個步驟，在 紅色/綠色/重構是重構部分。 到目前為止，我們要逐步從我們的程式碼，並考慮如何，我們可以重構我們的應用程式，以改善它的設計。 重構階段是在我們努力思考如何實作軟體設計原則和模式的最佳方式的階段。

我們可以自由修改我們的程式碼，以改善程式碼的設計，我們選擇的任何方式。 我們有防止我們小心破壞現有功能的單元測試的安全網。

現在，我們的群組控制站是從良好的軟體設計的觀點來看混亂。 群組控制站包含坎坷的混亂的驗證和資料存取程式碼。 若要避免違反 Single Responsibility Principle，我們需要這些考量分成不同類別。

我們的重構的群組控制器類別包含在 列表 9。 控制器已修改成使用 ContactManager 服務層。 這是我們會使用與連絡人控制器的相同服務層。

列表 10 包含新方法新增至 ContactManager 服務層，以支援驗證、 列出及建立群組。 IContactManagerService 介面已更新為包含新的方法。

列表 11 會包含新的 FakeContactManagerRepository 類別可實作 IContactManagerRepository 介面。 不同於也會實作 IContactManagerRepository 介面 EntityContactManagerRepository 類別，我們的新 FakeContactManagerRepository 類別不會不會與資料庫通訊。 FakeContactManagerRepository 類別會針對資料庫做為 proxy 中使用的記憶體中集合。 為假的儲存機制層，我們將在我們的單元測試中使用這個類別。

**列表 9-Controllers\GroupController.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample9.cs)]

**列表 10-Controllers\ContactManagerService.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample10.cs)]

**列表 11-Controllers\FakeContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample11.cs)]

修改介面需要 IContactManagerRepository EntityContactManagerRepository 類別中實作的 CreateGroup() 和 ListGroups() 方法使用。 若要這樣做的 laziest 且最快速的方式是加入看起來像這樣的虛設常式方法：   

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample12.cs)]


最後，這些變更，我們的應用程式的設計會要求我們對我們的單元測試進行一些修改。 我們現在需要執行單元測試時，使用 FakeContactManagerRepository。 列表 12 中，會包含更新的 GroupControllerTest 類別。

**列表 12-Controllers\GroupControllerTest.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample13.cs)]

我們進行所有這些之後，變更同樣地，所有我們的單元測試成功。 我們已完成紅色/綠色/重構整個的週期。 我們已實作的第一個的兩個使用者劇本。 我們現在有支援單元測試以使用者劇本的需求。 實作使用者劇本的其餘部分涉及重複相同的循環的紅色/綠色/重構。

## <a name="modifying-our-database"></a>修改資料庫

不幸的是，即使我們已滿足所有需求表示我們的單元測試，我們的工作不是。 我們還是需要修改資料庫。

我們需要建立新的群組資料庫資料表。 請依照下列步驟：

1. 在 [伺服器總管] 視窗中，以滑鼠右鍵按一下 [資料表] 資料夾，然後選取功能表選項**加入新的資料表**。
2. 輸入 資料表設計工具，如下所述的兩個資料行。
3. 識別碼資料行標示為主要金鑰和身分識別資料行。
4. 按一下磁碟的圖示，名稱群組儲存新的資料表。

<a id="0.11_table01"></a>


| **資料行名稱** | **資料類型** | **允許 null 值** |
| --- | --- | --- |
| ID | int | False |
| 名稱 | nvarchar(50) | False |


接下來，我們必須從 [連絡人] 資料表中刪除所有資料 (否則我們共贏得 t 能夠建立連絡人和群組的資料表之間的關聯性)。 請依照下列步驟：

1. 以滑鼠右鍵按一下 [連絡人] 資料表，然後選取功能表選項**顯示資料表資料**。
2. 刪除所有資料列。

接下來，我們需要定義群組的資料庫資料表和現有的連絡人資料庫資料表之間的關聯性。 請依照下列步驟：

1. 按兩下以開啟 資料表設計工具的 伺服器總管 視窗中的 連絡人 資料表。
2. 將新的整數資料行加入至 Contacts 資料表名為 GroupId。
3. 按一下 [關聯] 按鈕，以開啟外部索引鍵關聯性對話方塊 （見 [圖 3]）。
4. 按一下 [新增] 按鈕。
5. 按一下 [顯示資料表和資料行規格] 按鈕旁邊的省略符號按鈕。
6. 在 資料表和資料行 對話方塊中，選取 群組做為主要索引鍵資料行的識別碼與主索引鍵資料表。 選取連絡人做為外部索引鍵資料表和外部索引鍵資料行的 GroupId （請參閱 圖 4）。 按一下 [確定] 按鈕。
7. 底下**INSERT 及 UPDATE 規格**，選取值**Cascade** for**刪除規則**。
8. 按一下 [關閉] 按鈕以關閉 [外部索引鍵關聯性] 對話方塊。
9. 按一下 [儲存] 按鈕，以將變更儲存至 Contacts 資料表。


[![建立資料庫資料表關聯性](iteration-6-use-test-driven-development-cs/_static/image3.jpg)](iteration-6-use-test-driven-development-cs/_static/image5.png)

**圖 03**： 建立資料庫資料表關聯性 ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-cs/_static/image6.png))


[![指定資料表關聯性](iteration-6-use-test-driven-development-cs/_static/image4.jpg)](iteration-6-use-test-driven-development-cs/_static/image7.png)

**圖 04**： 指定資料表關聯性 ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-cs/_static/image8.png))


### <a name="updating-our-data-model"></a>更新的資料模型

接下來，我們需要更新我們的資料模型，以代表新的資料庫資料表。 請依照下列步驟：

1. 按兩下 ContactManagerModel.edmx 檔案以開啟 Entity Designer 的 模型 資料夾中。
2. 以滑鼠右鍵按一下設計工具介面，然後選取功能表選項**從資料庫更新模型**。
3. 在 [更新精靈] 中，選取群組的資料表，然後按一下 [完成] 按鈕 （請參閱 [圖 5]）。
4. 以滑鼠右鍵按一下群組實體，然後選取功能表選項**重新命名**。 變更的名稱*群組*實體*群組*（個別）。
5. 以滑鼠右鍵按一下連絡人實體下方會顯示群組導覽屬性。 變更的名稱*群組*導覽屬性來*群組*（個別）。


[![更新 Entity Framework 模型從資料庫](iteration-6-use-test-driven-development-cs/_static/image5.jpg)](iteration-6-use-test-driven-development-cs/_static/image9.png)

**圖 05**： 更新 Entity Framework 模型從資料庫 ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-cs/_static/image10.png))


完成這些步驟之後，您的資料模型會代表連絡人和群組的資料表。 實體設計工具應該會顯示這兩個實體 （請參閱 圖 6）。


[![顯示群組和連絡人的實體設計工具](iteration-6-use-test-driven-development-cs/_static/image6.jpg)](iteration-6-use-test-driven-development-cs/_static/image11.png)

**圖 06**： 顯示群組和連絡人的實體設計工具 ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-cs/_static/image12.png))


### <a name="creating-our-repository-classes"></a>建立儲存機制類別

接下來，我們要實作儲存機制類別。 在這個反覆項目的過程中，我們加入數個新方法 IContactManagerRepository 介面撰寫程式碼，以符合我們的單元測試時。 IContactManagerRepository 介面的最終版本會包含在 列表 14。

**列表 14-Models\IContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample14.cs)]

我們尚未 t 實際實作任何使用連絡人群組相關聯的方法。 目前，EntityContactManagerRepository 類別具有虛設常式方法的每個 IContactManagerRepository 介面中所列的連絡人群組方法。 比方說，ListGroups() 方法目前看起來像這樣：

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample15.cs)]

虛設常式方法可讓我們來編譯應用程式，並通過單元測試。 不過，現在就來實際實作這些方法。 EntityContactManagerRepository 類別的最終版本會包含在 列表 13。

**列表 13-Models\EntityContactManagerRepository.cs**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample16.cs)]

### <a name="creating-the-views"></a>建立檢視

當您使用預設 ASP.NET 檢視引擎 ASP.NET MVC 應用程式。 因此，您不要建立檢視以回應特定的單元測試。 不過，因為應用程式會是不具有檢視毫無用處，我們可以 t 完成此反覆項目，而不需要建立和修改連絡人管理員應用程式中包含的檢視。

我們需要建立下列新檢視，可管理連絡人群組 （請參閱 圖 7）：

- Views\Group\Index.aspx-連絡人群組的顯示清單
- Views\Group\Delete.aspx-刪除連絡人群組會顯示確認表單


[![群組索引檢視](iteration-6-use-test-driven-development-cs/_static/image7.jpg)](iteration-6-use-test-driven-development-cs/_static/image13.png)

**圖 07**: 群組索引檢視 ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-cs/_static/image14.png))


我們需要修改下列現有的檢視，讓它們包含連絡人群組：

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

您可以看到修改過的檢視，查看隨附此教學課程的 Visual Studio 應用程式。 例如，圖 8 說明連絡人索引檢視。


[![請連絡 [索引] 檢視](iteration-6-use-test-driven-development-cs/_static/image8.jpg)](iteration-6-use-test-driven-development-cs/_static/image15.png)

**圖 08**: 請連絡 [索引] 檢視 ([按一下以檢視完整大小的影像](iteration-6-use-test-driven-development-cs/_static/image16.png))


## <a name="summary"></a>總結

在這個反覆項目，我們新增的新功能至我們的連絡人管理員應用程式依照測試導向開發的應用程式的設計方法。 我們已開始建立一組使用者劇本。 我們會建立一組單元測試，對應至使用者劇本所表示的需求。 最後，我們會撰寫剛好足夠的程式碼，以滿足需求的單元測試來表示。

我們已完成寫入足夠的程式碼，以滿足單元測試所表示的需求之後，我們會更新我們的資料庫和檢視表。 我們加入我們的資料庫中的新群組的資料表，並更新我們的 Entity Framework 資料模型。 我們也會建立並修改一組檢視。

在下一個反覆項目-最後一個反覆項目-我們重新撰寫我們的應用程式，才能利用 Ajax。 利用 Ajax，我們將會改善回應性和連絡人管理員應用程式的效能。

> [!div class="step-by-step"]
> [上一頁](iteration-5-create-unit-tests-cs.md)
> [下一頁](iteration-7-add-ajax-functionality-cs.md)
