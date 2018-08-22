---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
title: 建置介面從許多 (C#) 中選取一個使用者帳戶 |Microsoft Docs
author: rick-anderson
description: 在本教學課程中，我們將建置具有分頁、 可篩選方格的使用者介面。 特別是，我們的使用者介面將包含一系列的 Linkbutton 的...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: 9e4e687c-b4ec-434f-a4ef-edb0b8f365e4
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
msc.type: authoredcontent
ms.openlocfilehash: 863ac36ae6a94ece841088db925c04deb3bf36c9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825544"
---
<a name="building-an-interface-to-select-one-user-account-from-many-c"></a>建置介面從許多 (C#) 中選取一個使用者帳戶
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.12.zip)或[下載 PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_cs.pdf)

> 在本教學課程中，我們將建置具有分頁、 可篩選方格的使用者介面。 特別是，我們的使用者介面將包含一系列的 Linkbutton 來篩選結果的使用者名稱和 GridView 控制項以顯示相符的使用者起始的字母為基礎。 我們一開始先列出所有的 GridView 中的使用者帳戶。 然後，在步驟 3 中，我們會新增 Linkbutton 的篩選條件。 步驟 4 會查看分頁篩選的結果。 建構在步驟 2 到 4 之間的介面將用於後續的教學課程中，執行特定的使用者帳戶的系統管理工作。


## <a name="introduction"></a>簡介

在  <a id="_msoanchor_1"> </a> [*將角色指派給使用者*](../roles/assigning-roles-to-users-cs.md)教學課程中，我們會建立系統管理員可以選取使用者，並管理其角色的基本介面。 具體而言，介面顯示下拉式清單中的所有使用者的系統管理員。 如果有，但是約十二個左右的使用者帳戶，但會難以使用數百或數千個帳戶的站台，適合使用這類介面。 分頁、 可篩選的方格是更適合的使用者介面，適用於具有大量使用者基底的網站。

在本教學課程中，我們將建置這類使用者介面。 特別是，我們的使用者介面將包含一系列的 Linkbutton 來篩選結果的使用者名稱和 GridView 控制項以顯示相符的使用者起始的字母為基礎。 我們一開始先列出所有的 GridView 中的使用者帳戶。 然後，在步驟 3 中，我們會新增 Linkbutton 的篩選條件。 步驟 4 會查看分頁篩選的結果。 建構在步驟 2 到 4 之間的介面將用於後續的教學課程中，執行特定的使用者帳戶的系統管理工作。

讓我們開始吧 ！

## <a name="step-1-adding-new-aspnet-pages"></a>步驟 1： 加入新的 ASP.NET 網頁

在本教學課程，後面兩個我們將檢查各種不同的系統管理相關的功能和功能。 我們需要一系列的 ASP.NET 頁面，來實作檢查在這些教學課程的主題。 讓我們來建立這些頁面，並更新站台對應。

藉由建立新的資料夾中名為專案啟動`Administration`。 接下來，將兩個新的 ASP.NET 網頁新增至資料夾中，連結每個頁面`Site.master`主版頁面。 名稱的頁面：

- `ManageUsers.aspx`
- `UserInformation.aspx`

也將兩個頁面新增至網站的根目錄：`ChangePassword.aspx`和`RecoverPassword.aspx`。

這些四個頁面，到目前為止，應有兩個內容控制項，一個用於每個主版頁面的 ContentPlaceHolders:`MainContent`和`LoginContent`。

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample1.aspx)]

我們想要顯示的主版頁面的預設標記`LoginContent`ContentPlaceHolder 這些頁面。 因此，移除的宣告式標記`Content2`內容控制項。 完成後，頁面的標記應包含一個內容控制項。

ASP.NET 頁面`Administration`資料夾主要僅供系統管理使用者。 我們新增至系統中的系統管理員角色<a id="_msoanchor_2"> </a> [*建立和管理角色*](../roles/creating-and-managing-roles-cs.md)教學課程中，這兩個頁面至這個角色限制存取。 若要達成此目的，將`Web.config`的檔案`Administration`資料夾，並設定其`<authorization>`承認的系統管理員角色中的使用者，並拒絕其他所有的項目。

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample2.xml)]

此時您專案的方案總管] 看起來應該類似螢幕擷取畫面的 [圖 1 所示。


[![四個新的網頁和 Web.config 檔案新增至網站](building-an-interface-to-select-one-user-account-from-many-cs/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image1.png)

**圖 1**： 四個新的頁面並`Web.config`檔案已加入至網站 ([按一下以檢視完整大小的影像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image3.png))


最後，更新站台對應 (`Web.sitemap`) 包含一個項目`ManageUsers.aspx`頁面。 新增下列 XML 程式碼之後`<siteMapNode>`我們新增的角色教學課程。

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample3.xml)]

更新站台對應，請瀏覽的網站，透過瀏覽器。 如 [圖 2] 所示，在左側的導覽現在會包含項目管理教學課程。


[![站台地圖包含標題為 [使用者管理] 節點](building-an-interface-to-select-one-user-account-from-many-cs/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image4.png)

**圖 2**： 站台對應包含節點標題為 [使用者管理] ([按一下以檢視完整大小的影像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image6.png))


## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>步驟 2： 列出 GridView 中的所有使用者帳戶

我們在本教學課程的最終目標是要建立分頁、 可篩選格線，讓系統管理員可以選取要管理的使用者帳戶。 讓我們先列出*所有*GridView 中的使用者。 這項操作完成，我們將加入的篩選和分頁介面和功能。

開啟`ManageUsers.aspx`頁面中`Administration`資料夾，並新增一個 GridView，設定其`ID`至`UserAccounts`。 等一下，我們會撰寫程式碼，以使用繫結的使用者帳戶集至 GridView`Membership`類別的`GetAllUsers`方法。 先前的教學課程所述，GetAllUsers 方法會傳回`MembershipUserCollection`物件，它是集合的`MembershipUser`物件。 每個`MembershipUser`集合中包含的屬性，例如`UserName`， `Email`， `IsApproved`，依此類推。

若要在 GridView 中顯示的所需的使用者帳戶資訊，請設定 GridView 的`AutoGenerateColumns`屬性設定為 False，並新增為 BoundFields `UserName`， `Email`，和`Comment`屬性和 CheckBoxFields 的`IsApproved`，`IsLockedOut`，和`IsOnline`屬性。 透過控制項的宣告式標記，或透過 [欄位] 對話方塊中，可以套用此設定。 圖 3 顯示的螢幕擷取畫面的欄位 對話方塊中自動產生欄位核取方塊已取消選取，並 BoundFields 和 CheckBoxFields 加入並設定之後。


[![將三個 BoundFields 及三個 CheckBoxFields 新增到 GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image7.png)

**圖 3**： 新增三個 BoundFields 和三個 CheckBoxFields 至 GridView ([按一下以檢視完整大小的影像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image9.png))


設定您的 GridView 之後, 請確定其宣告式標記如下所示：

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample4.aspx)]

接下來，我們需要撰寫程式碼，將使用者帳戶繫結至 GridView。 建立名為的方法`BindUserAccounts`若要執行這項工作，然後呼叫從`Page_Load`上第一個頁面瀏覽事件處理常式。

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample5.cs)]

請花一點時間測試透過瀏覽器頁面。 如 [圖 4] 所示， `UserAccounts` GridView 列出系統中的使用者名稱、 電子郵件地址及其他相關的帳戶資訊的所有使用者。


[![在 gridview 裡所列出的使用者帳戶](building-an-interface-to-select-one-user-account-from-many-cs/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image10.png)

**圖 4**: 的使用者帳戶會列在 gridview 裡 ([按一下以檢視完整大小的影像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image12.png))


## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>步驟 3： 篩選結果的第一個字母的使用者名稱

目前`UserAccounts`GridView 會顯示*所有*的使用者帳戶。 使用數百或數千個使用者帳戶的網站，因此務必該使用者就能夠快速地削減顯示的帳戶。 這可透過新增至頁面篩選 Linkbutton。 讓我們新增至頁面的 27 Linkbutton： 其中一個標題為 所有以及一個 LinkButton 的每個字母的字母。 如果訪客按一下所有 LinkButton，GridView 會顯示所有使用者。 如果使用者按一下特定的字母，只在其使用者名稱開頭為所選字母的使用者將會顯示。

第一個工作是新增 27 LinkButton 控制項。 其中一個選項是以宣告方式建立的 27 Linkbutton 一次。 更有彈性的方法是使用與重複項控制項`ItemTemplate`呈現 LinkButton，然後繫結至做為重複項的篩選選項`string`陣列。

藉由將頁面上方的重複項控制項開始`UserAccounts`GridView。 設定中繼器`ID`屬性設`FilteringUI`。 設定中繼器的範本，讓其`ItemTemplate`呈現 LinkButton 其`Text`和`CommandName`屬性繫結至目前的陣列項目。 如我們在中所見<a id="_msoanchor_3"> </a> [*將角色指派給使用者*](../roles/assigning-roles-to-users-cs.md)教學課程，即可使用`Container.DataItem`資料繫結語法。 使用 Repeater`SeparatorTemplate`來顯示每個連結之間的垂直線條。

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample6.aspx)]

若要填入這個 Repeater 具有所需的篩選選項，建立名為`BindFilteringUI`。 請務必呼叫這個方法從`Page_Load`上第一次頁面載入事件處理常式。

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample7.cs)]

這個方法會做為項目中指定的篩選選項`string`陣列`filterOptions`。 陣列中每個項目，如 Repeater 會呈現 LinkButton 及其`Text`和`CommandName`屬性指派給陣列元素的值。

[圖 5] 顯示`ManageUsers.aspx`頁面上透過瀏覽器檢視時。


[![中繼器列出 27 篩選 Linkbutton](building-an-interface-to-select-one-user-account-from-many-cs/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image13.png)

**圖 5**: Repeater 列出 27 篩選 Linkbutton ([按一下以檢視完整大小的影像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image15.png))


> [!NOTE]
> 使用者名稱可能會開始任何字元，包括數字和標點符號。 若要檢視這些帳戶，系統管理員必須使用所有的 LinkButton 選項。 或者，您可以新增 LinkButton 傳回開頭為數字的所有使用者帳戶。 我不要更動此練習的讀取器。


按一下任何篩選的 Linkbutton 造成回傳，並引發 Repeater`ItemCommand`事件，但不會在方格中的任何變更，因為我們至今還撰寫任何程式碼，以篩選結果。 `Membership`類別包含[`FindUsersByName`方法](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx)傳回其使用者名稱符合指定的搜尋模式的使用者帳戶。 我們可以使用這個方法來擷取其使用者名稱以字母開頭的所指定的使用者帳戶`CommandName`的已篩選的 LinkButton 已按下。

藉由更新開始`ManageUser.aspx`頁面的程式碼後置類別，使它包含一個名為屬性`UsernameToMatch`。 這個屬性會在回傳之間保存使用者名稱篩選條件字串：

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample8.cs)]

`UsernameToMatch`屬性會儲存它的值指派到`ViewState`使用 UsernameToMatch 的索引鍵的集合。 讀取這個屬性的值時，它會檢查值是否存在於`ViewState`集合; 如果沒有，它會傳回預設值為空字串。 `UsernameToMatch`屬性表現出常見的模式，也就保存到檢視狀態，讓屬性的任何變更會在回傳之間保存的值。 如需有關此模式的詳細資訊，請參閱[了解 ASP.NET 檢視狀態](https://msdn.microsoft.com/library/ms972976.aspx)。

接下來，更新`BindUserAccounts`方法的呼叫，因此`Membership.GetAllUsers`，它會呼叫`Membership.FindUsersByName`，並傳入的值`UsernameToMatch`屬性加上 SQL 萬用字元 %。

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample9.cs)]

若要顯示其使用者名稱開頭為字母 A 的使用者，請設定`UsernameToMatch`的屬性，然後呼叫`BindUserAccounts`。 這會導致呼叫`Membership.FindUsersByName("A%")`，這會傳回所有使用者的使用者名稱開頭為 a。 同樣地，傳回*所有*使用者、 指派至空字串`UsernameToMatch`屬性，讓`BindUserAccounts`方法將叫用`Membership.FindUsersByName("%")`，以便傳回所有的使用者帳戶。

建立事件處理常式，如 Repeater`ItemCommand`事件。 只要按一下其中一個篩選條件 Linkbutton; 時，會引發這個事件它會傳遞按一下的 LinkButton`CommandName`透過值`RepeaterCommandEventArgs`物件。 我們需要指派適當的值，來`UsernameToMatch`屬性，然後呼叫`BindUserAccounts`方法。 如果`CommandName`為 All，將空字串，以指派`UsernameToMatch`以便顯示所有使用者帳戶。 否則，請指派`CommandName`值`UsernameToMatch`。

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample10.cs)]

使用此程式碼就緒之後，測試篩選的功能。 當第一次瀏覽頁面時，會顯示所有使用者帳戶 （請參閱上一步 圖 5）。 按一下 LinkButton 造成回傳，並篩選結果，顯示開頭為 A 的使用者帳戶。


[![若要顯示這些使用者以特定的字母為開頭的使用者名稱使用的篩選 Linkbutton](building-an-interface-to-select-one-user-account-from-many-cs/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image16.png)

**圖 6**： 使用篩選的 Linkbutton 顯示這些使用者的使用者名稱開頭為特定字元 ([按一下以檢視完整大小的影像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image18.png))


## <a name="step-4-updating-the-gridview-to-use-paging"></a>步驟 4： 更新使用分頁 GridView

圖 5 和 6 所示的 GridView 會列出所有傳回的記錄`FindUsersByName`方法。 如果有數百或數千個使用者帳戶這可能會導致資訊多載 （在此情況下或一開始瀏覽頁面時按一下 所有 LinkButton） 檢視所有的帳戶時。 為了更容易管理的區塊 （chunk） 中出現的使用者帳戶，讓我們設定 GridView，以一次顯示 10 個使用者帳戶。

GridView 控制項提供兩種類型的分頁：

- **預設的分頁**-容易實作，但效率不佳。 簡單的說，分頁 GridView 的預設值會預期*所有*與其資料來源的記錄。 接著只會顯示適當的頁面的記錄。
- **自訂分頁**-需要更多工作來實作，但會更有效率，比預設分頁，因為使用自訂分頁的資料來源會傳回精確集顯示的資料錄。

逐頁查看數千筆記錄時，預設和自訂分頁的效能差異可能很相當長。 因為我們要建置這個介面，但前提那里可能會有數百或數千個使用者帳戶，讓我們使用自訂分頁。

> [!NOTE]
> 如需預設和自訂的分頁，以及實作自訂分頁挑戰之間的差異的更完整討論，請參閱[有效率地透過大型數量的資料分頁](https://asp.net/learn/data-access/tutorial-25-cs.aspx)。 預設和自訂分頁的效能差異的一些分析，請參閱[在 ASP.NET 中使用 SQL Server 2005 的自訂分頁](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)。


若要實作自訂分頁，我們首先需要某種機制，用來擷取精確的 GridView 所要顯示的資料錄子集。 好消息是，`Membership`類別的`FindUsersByName`方法具有多載，可讓我們指定的頁面索引和頁面大小，並傳回該範圍的記錄內的使用者帳戶。

特別是，這個多載具有下列簽章： [ `FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)` ](https://msdn.microsoft.com/library/fa5st8b2.aspx)。

*PageIndex*參數會指定要傳回; 的使用者帳戶頁面*pageSize*指出每頁顯示的多少筆記錄。 *TotalRecords*參數是`out`在使用者存放區傳回的總使用者帳戶數目的參數。

> [!NOTE]
> 所傳回的資料`FindUsersByName`會依照使用者名稱; 無法自訂排序條件。


若要使用自訂分頁，但只有當繫結到 ObjectDataSource 控制項，可以設定 GridView。 若要實作自訂分頁 ObjectDataSource 控制項，它需要兩個方法： 一個會傳遞開始的資料列索引和資料錄顯示，最大數目，並傳回該範圍內; 內的記錄的精確子集並透過正在呼叫的方法會傳回的資料錄總數。 `FindUsersByName`多載會接受的頁面索引和頁面大小，並傳回透過的記錄總數`out`參數。 因此會有的介面不相符。

其中一個選項是建立 proxy 類別會公開介面，介面 ObjectDataSource 預期，並在內部呼叫`FindUsersByName`方法。 另一個選項-以及一個我們將使用這篇文章-是建立自己的分頁介面，並使用，而不是 GridView 的內建的分頁介面。

### <a name="creating-a-first-previous-next-last-paging-interface"></a>建立第一個、 上一個，接下來，最後一個分頁介面

我們將使用第一個、 上一步下, 一步 和最後一個 Linkbutton 建置分頁介面。 第一個 LinkButton 按一下時，會將使用者帶到第一頁的資料，而上一步前一頁會傳回與他連絡。 同樣地下, 一個] 和 [最後一個會將使用者移至下一個和最後一個頁面中，分別。 新增四個 LinkButton 控制項下方`UserAccounts`GridView。

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample11.aspx)]

接下來，建立事件處理常式，針對每個 LinkButton`Click`事件。

[圖 7] 顯示的四個 Linkbutton 檢視透過 [Visual Web Developer 設計] 檢視時。


[![接下來，新增第一個、 上一個和最後一個 GridView 下方的 Linkbutton](building-an-interface-to-select-one-user-account-from-many-cs/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image19.png)

**[圖 7**： 新增第一個、 上一步下, 一步] 和最後一個 Linkbutton 下方 GridView ([按一下以檢視完整大小的影像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image21.png))


### <a name="keeping-track-of-the-current-page-index"></a>追蹤的目前的頁面索引

當使用者第一次瀏覽`ManageUsers.aspx`頁面或按下其中一個篩選按鈕，我們想要在 GridView 中顯示資料的第一頁。 不過，當使用者按一下瀏覽 Linkbutton 的其中一個時，我們需要更新的頁面索引。 若要維護的頁面索引和每頁顯示的記錄數目，加入頁面的程式碼後置類別中的下列兩個屬性：

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample12.cs)]

像是`UsernameToMatch`屬性，`PageIndex`屬性保存其檢視狀態的值。 唯讀`PageSize`屬性會傳回硬式編碼的值，而 10。 我邀請來更新此屬性，以使用與相同的模式有興趣的讀者`PageIndex`，然後再擴大`ManageUsers.aspx`頁面上，瀏覽頁面的人員可以指定多少個使用者帳戶來顯示每個頁面上。

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>擷取目前網頁的記錄為止，更新頁面索引、 啟用和停用分頁介面 Linkbutton

與就地分頁介面和`PageIndex`並`PageSize`屬性加入，我們已準備好更新`BindUserAccounts`方法，因此它會使用適當`FindUsersByName`多載。 此外，我們需要有這個方法，啟用或停用分頁介面，取決於所顯示頁面。 當您檢視資料的第一頁，就應該停用的第一個和上一步連結;檢視的最後一頁時，應該停用接下來，並持續。

以下列程式碼更新 `BindUserAccounts` 方法：

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample13.cs)]

請注意，透過正在分頁的記錄總數取決於最後一個參數`FindUsersByName`方法。 這是`out`參數，因此我們必須先宣告變數，以保留此值 (`totalRecords`)，然後將它與前置詞`out`關鍵字。

傳回指定的頁面的使用者帳戶之後，四個 Linkbutton 啟用或停用，取決於是否在檢視資料的第一個或最後一頁。

最後一個步驟是撰寫程式碼的四個 Linkbutton`Click`事件處理常式。 這些事件處理常式需要更新`PageIndex`屬性然後重新繫結至 GridView，透過對呼叫資料`BindUserAccounts`。 [名字]、 [上一個] 和 [下一步] 的事件處理常式是非常簡單。 `Click`事件處理常式的最後一個 LinkButton，不過，是比較複雜一點因為我們必須決定多少筆記錄會顯示以判斷最後一個頁面索引。

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample14.cs)]

圖 8 和 9 顯示作用中的自訂分頁介面。 [圖 8] 顯示`ManageUsers.aspx`頁面檢視所有使用者帳戶資料的第一頁時。 請注意，只有 10 小時，共 13 的帳戶會顯示出來。 按一下下一個或最後一個連結會導致回傳時，更新`PageIndex`成 1，而第二個頁面的使用者帳戶方格繫結 （請參閱 圖 9）。


[![會顯示第一個 10 使用者帳戶](building-an-interface-to-select-one-user-account-from-many-cs/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image22.png)

**圖 8**： 顯示的第一個 10 使用者帳戶 ([按一下以檢視完整大小的影像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image24.png))


[![按一下 [下一步] 連結會顯示使用者帳戶的第二頁](building-an-interface-to-select-one-user-account-from-many-cs/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image25.png)

**圖 9**： 按一下 [下一步] 連結會顯示第二個頁面的使用者帳戶 ([按一下以檢視完整大小的影像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image27.png))


## <a name="summary"></a>總結

若要從帳戶清單中選取使用者通常需要系統管理員。 在先前的教學課程中我們探討使用下拉式清單，填入使用者，但這種方法無法妥善調整。 在本教學課程中，我們探討更好的替代方案： 可篩選介面，其結果會顯示在分頁的 GridView。 此使用者介面後，系統管理員可以快速且有效地找出並選取一個千分位之間的使用者帳戶。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [在 ASP.NET 中使用 SQL Server 2005 的自訂分頁](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [有效率地分頁大量資料](https://asp.net/learn/data-access/tutorial-25-cs.aspx)
- [復原您自己的網站管理工具](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP 書籍的作者，他是 4GuysFromRolla.com 的創辦人，從事 Microsoft Web 技術工作自 1998 年。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是 *[Sams 教導您自己 ASP.NET 2.0 在 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 Scott 要聯絡[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Alicja Maziarz。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在 [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [下一步](recovering-and-changing-passwords-cs.md)
