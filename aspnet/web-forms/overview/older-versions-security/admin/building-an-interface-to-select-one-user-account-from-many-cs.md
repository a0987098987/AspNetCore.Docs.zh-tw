---
uid: web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
title: 建立介面，以選取一個使用者帳戶從許多 (C#) |Microsoft 文件
author: rick-anderson
description: 在本教學課程中，我們將建置使用者介面與分頁、 可篩選方格。 特別是，我們的使用者介面將會包含一系列的 LinkButtons...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 9e4e687c-b4ec-434f-a4ef-edb0b8f365e4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/building-an-interface-to-select-one-user-account-from-many-cs
msc.type: authoredcontent
ms.openlocfilehash: 304505b18e330425ea1dc8df87a552f3d8cd15f3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890667"
---
<a name="building-an-interface-to-select-one-user-account-from-many-c"></a>建立介面，以選取一個使用者帳戶從許多 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.12.zip)或[下載 PDF](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial12_SelectUser_cs.pdf)

> 在本教學課程中，我們將建置使用者介面與分頁、 可篩選方格。 特別是，我們的使用者介面將會包含一系列的 LinkButtons 來篩選結果的使用者名稱和 GridView 控制項以顯示比對使用者起始的字母為基礎。 我們會先列出所有 GridView 中的使用者帳戶。 然後，在步驟 3 中，我們將加入篩選 LinkButtons。 步驟 4 會查看分頁篩選的結果。 建構在步驟 2 到 4 的介面將用於後續的教學課程，針對特定使用者帳戶執行管理工作。


## <a name="introduction"></a>簡介

在<a id="_msoanchor_1"> </a> [*角色指派使用者給*](../roles/assigning-roles-to-users-cs.md)教學課程中，我們會建立系統管理員可以選取的使用者，並管理其角色的基本介面。 具體來說，介面出現下拉式清單的所有使用者的系統管理員。 當有數十個使用者帳戶，但會變得不便使用數百或數千個帳戶的站台，適合使用這類介面。 分頁、 可篩選的方格是更適合的使用者介面，網站具有大型使用者基底。

在本教學課程中，我們將建置這類使用者介面。 特別是，我們的使用者介面將會包含一系列的 LinkButtons 來篩選結果的使用者名稱和 GridView 控制項以顯示比對使用者起始的字母為基礎。 我們會先列出所有 GridView 中的使用者帳戶。 然後，在步驟 3 中，我們將加入篩選 LinkButtons。 步驟 4 會查看分頁篩選的結果。 建構在步驟 2 到 4 的介面將用於後續的教學課程，針對特定使用者帳戶執行管理工作。

讓我們開始吧 ！

## <a name="step-1-adding-new-aspnet-pages"></a>步驟 1： 加入新的 ASP.NET 網頁

在本教學課程和後面兩個我們將會檢查各種不同的系統管理相關的功能和功能。 我們需要一系列的 ASP.NET 頁面實作檢查這些教學課程中的主題。 讓我們來建立這些頁面，並更新站台對應。

藉由建立新的資料夾中名為專案啟動`Administration`。 接下來，將兩個新的 ASP.NET 網頁新增至資料夾中，連結與每個頁面`Site.master`主版頁面。 命名頁面：

- `ManageUsers.aspx`
- `UserInformation.aspx`

也將兩個頁面加入至網站的根目錄：`ChangePassword.aspx`和`RecoverPassword.aspx`。

這些四個頁面，此時，應有兩個內容控制項，各供一個主版頁面的 ContentPlaceHolders:`MainContent`和`LoginContent`。

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample1.aspx)]

我們想要顯示在主版頁面的預設標記`LoginContent`ContentPlaceHolder 這些頁面。 因此，移除的宣告式標記`Content2`內容控制項。 之後，請網頁的標記應該包含單一內容控制項。

ASP.NET 中的分頁`Administration`資料夾僅供系統管理使用者。 我們加入系統管理員角色中的系統<a id="_msoanchor_2"> </a> [*建立和管理角色*](../roles/creating-and-managing-roles-cs.md)教學課程; 限制的存取權給這個角色的這兩個分頁。 若要達成此目的，將`Web.config`檔案`Administration`資料夾及設定其`<authorization>`承認系統管理員角色中的使用者，並拒絕所有其他項目。

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample2.xml)]

此時您專案的方案總管 看起來應該類似螢幕擷取畫面中圖 1 所示。


[![四個新的頁面和 Web.config 檔案新增至網站](building-an-interface-to-select-one-user-account-from-many-cs/_static/image2.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image1.png)

**圖 1**： 四個新的頁面和`Web.config`檔案已加入至網站 ([按一下以檢視完整大小的影像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image3.png))


最後，更新站台對應 (`Web.sitemap`) 包含的項目`ManageUsers.aspx`頁面。 加入下列 XML 之後`<siteMapNode>`我們新增為角色教學課程。

[!code-xml[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample3.xml)]

與更新站台對應，請瀏覽的網站透過瀏覽器。 如圖 2 所示，現在在左側瀏覽項目中包含管理教學課程。


[![站台地圖包含標題為 使用者管理的節點](building-an-interface-to-select-one-user-account-from-many-cs/_static/image5.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image4.png)

**圖 2**： 站台對應包含節點標題為 [使用者管理] ([按一下以檢視完整大小的影像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image6.png))


## <a name="step-2-listing-all-user-accounts-in-a-gridview"></a>步驟 2： 列出在 GridView 中的所有使用者帳戶

此教學課程中我們結束的目標是建立分頁、 可篩選方格，透過此系統管理員可以選取要管理的使用者帳戶。 我們先來列出*所有*GridView 中的使用者。 一旦完成時，我們將加入的篩選和分頁介面和功能。

開啟`ManageUsers.aspx`頁面`Administration`資料夾並加入 GridView，設定其`ID`至`UserAccounts`。 在不久後，我們會撰寫程式碼，以將一組使用者帳戶繫結至 GridView 使用`Membership`類別的`GetAllUsers`方法。 如先前的教學課程所述，GetAllUsers 方法會傳回`MembershipUserCollection`物件，它是集合的`MembershipUser`物件。 每個`MembershipUser`集合中包含的屬性，例如`UserName`， `Email`， `IsApproved`，依此類推。

若要顯示在 GridView 的所需的使用者帳戶資訊，請設定 GridView 的`AutoGenerateColumns`屬性設定為 False 並加入為 BoundFields `UserName`， `Email`，和`Comment`屬性和如 CheckBoxFields `IsApproved`，`IsLockedOut`，和`IsOnline`屬性。 可以套用此設定，透過控制項的宣告式標記，或透過 [欄位] 對話方塊。 圖 3 顯示欄位的螢幕擷取畫面 對話方塊之後自動產生欄位核取方塊已取消選取且 BoundFields 和 CheckBoxFields 已加入並設定。


[![將三個 BoundFields 和三個 CheckBoxFields GridView 加入](building-an-interface-to-select-one-user-account-from-many-cs/_static/image8.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image7.png)

**圖 3**： 新增三個 BoundFields 和三個 CheckBoxFields 至 GridView ([按一下以檢視完整大小的影像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image9.png))


設定您的 GridView 之後, 請確定其宣告式標記類似下列：

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample4.aspx)]

接下來，我們必須撰寫程式碼，將使用者帳戶繫結至 GridView。 建立名為方法`BindUserAccounts`，執行這項工作，然後再呼叫從`Page_Load`在第一個頁面造訪的事件處理常式。

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample5.cs)]

請花一點時間來測試透過瀏覽器頁面。 如圖 4 所示， `UserAccounts` GridView 列出系統中的使用者名稱、 電子郵件地址和其他相關的帳戶資訊的所有使用者。


[![在 GridView 中所列出的使用者帳戶](building-an-interface-to-select-one-user-account-from-many-cs/_static/image11.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image10.png)

**圖 4**: GridView 中所列的使用者帳戶 ([按一下以檢視完整大小的影像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image12.png))


## <a name="step-3-filtering-the-results-by-the-first-letter-of-the-username"></a>步驟 3： 篩選結果的第一個字母的使用者名稱

目前`UserAccounts`GridView 顯示*所有*的使用者帳戶。 具有數百部或數千個使用者帳戶的網站，請務必該使用者要能夠快速削減顯示的帳戶。 這可以藉由新增至頁面篩選 LinkButtons 完成。 讓我們加入至頁面的 27 LinkButtons： 其中一個標題為所有以及一個 LinkButton 每個字母的字母。 如果訪客按一下所有 LinkButton，GridView 會顯示所有使用者。 如果使用者按一下特定的字母，將會顯示其使用者名稱以選取的字母為開頭的使用者。

我們第一項工作是加入 27 LinkButton 控制項。 其中一個選項是以宣告方式，建立 27 LinkButtons 一次。 更具彈性的方法是使用與中繼器控制項`ItemTemplate`，轉譯的 LinkButton 並接著繫結至做為重複項的篩選選項`string`陣列。

中繼器控制項加入頁面上方的開始`UserAccounts`GridView。 設定中繼器`ID`屬性`FilteringUI`。 設定中繼器的範本，讓其`ItemTemplate`呈現 LinkButton 其`Text`和`CommandName`屬性繫結至目前的陣列項目。 如我們所見中<a id="_msoanchor_3"> </a> [*角色指派使用者給*](../roles/assigning-roles-to-users-cs.md)教學課程，達成這點可以使用`Container.DataItem`資料繫結語法。 使用中繼器`SeparatorTemplate`顯示每個連結之間的垂直線條。

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample6.aspx)]

若要擴展這個中繼器具有所需的篩選選項，請建立名為方法`BindFilteringUI`。 請務必呼叫這個方法從`Page_Load`事件處理常式，第一個載入的頁面。

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample7.cs)]

這個方法會做為項目中指定的篩選選項`string`陣列`filterOptions`。 對於陣列中每個項目，中繼器會呈現 LinkButton 其`Text`和`CommandName`屬性指派給陣列元素的值。

圖 5 顯示`ManageUsers.aspx`頁面上透過瀏覽器檢視時。


[![中繼器列出 27 篩選 LinkButtons](building-an-interface-to-select-one-user-account-from-many-cs/_static/image14.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image13.png)

**圖 5**: 中繼器列出 27 篩選 LinkButtons ([按一下以檢視完整大小的影像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image15.png))


> [!NOTE]
> 使用者名稱的任何字元，包括數字和標點符號可能開頭。 若要檢視這些帳戶，系統管理員必須使用所有的 LinkButton 選項。 或者，您可以加入要傳回以數字開頭的所有使用者帳戶的 LinkButton。 我將保留此為一項工作讀取器。


按一下任何篩選 LinkButtons 導致回傳，並引發中繼器`ItemCommand`事件，但不會在方格中的任何變更，因為我們尚未以撰寫任何程式碼，以篩選結果。 `Membership`類別包含[`FindUsersByName`方法](https://technet.microsoft.com/library/system.web.security.membership.findusersbyname.aspx)傳回其使用者名稱符合指定的搜尋模式的這些使用者帳戶。 我們可以使用這個方法來擷取其使用者名稱的開頭所指定的代號僅擁有使用者帳戶`CommandName`的已篩選的 LinkButton 已按下。

藉由更新開始`ManageUser.aspx`網頁的程式碼後置類別，使其包含一個名為屬性`UsernameToMatch`。 這個屬性會保存在回傳的使用者名稱的篩選字串：

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample8.cs)]

`UsernameToMatch`屬性會儲存其值指派到`ViewState`使用 UsernameToMatch 的索引鍵的集合。 當讀取這個屬性的值時，它會檢查值是否存在中`ViewState`集合; 如果沒有，它會傳回預設值為空字串。 `UsernameToMatch`屬性表現常見的模式，也就保存至檢視狀態，以便對屬性的任何變更會保存在回傳值。 如需此模式的詳細資訊，請參閱[了解 ASP.NET 檢視狀態](https://msdn.microsoft.com/library/ms972976.aspx)。

接下來，更新`BindUserAccounts`方法的呼叫，而`Membership.GetAllUsers`，它會呼叫`Membership.FindUsersByName`的值傳遞給`UsernameToMatch`屬性加上 SQL 萬用字元 %。

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample9.cs)]

若要顯示僅擁有其使用者名稱開頭為字母 A 的使用者，請設定`UsernameToMatch`到 A 的屬性，然後呼叫`BindUserAccounts`。 這會導致呼叫`Membership.FindUsersByName("A%")`，這會傳回所有使用者的使用者名稱開頭為 a。 同樣地，若要都傳回*所有*使用者指派至空字串`UsernameToMatch`屬性以便`BindUserAccounts`方法將叫用`Membership.FindUsersByName("%")`，藉此傳回所有使用者帳戶。

建立事件處理常式的中繼器`ItemCommand`事件。 按一下其中一個篩選器 LinkButtons; 時，會引發這個事件它會傳遞按下後的 LinkButton`CommandName`值透過`RepeaterCommandEventArgs`物件。 我們需要指派的適當值`UsernameToMatch`屬性然後再呼叫`BindUserAccounts`方法。 如果`CommandName`為 All，將空字串，以指派`UsernameToMatch`以便顯示所有使用者帳戶。 否則，請指派`CommandName`值設定為`UsernameToMatch`。

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample10.cs)]

這個程式碼的位置中，測試篩選功能。 當第一次瀏覽頁面時，會顯示所有使用者帳戶 （請參閱上一步圖 5）。 按一下 LinkButton 導致回傳，並篩選結果，顯示開頭的使用者帳戶。


[![若要顯示這些使用者的使用者名稱開頭為特定字母中使用篩選 LinkButtons](building-an-interface-to-select-one-user-account-from-many-cs/_static/image17.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image16.png)

**圖 6**： 若要顯示這些使用者的使用者名稱開頭為特定字母中使用篩選的 LinkButtons ([按一下以檢視完整大小的影像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image18.png))


## <a name="step-4-updating-the-gridview-to-use-paging"></a>步驟 4： 更新使用分頁 GridView

數字 5 和 6 所示的 GridView 會列出所有傳回的記錄從`FindUsersByName`方法。 如果有數百或數千個使用者帳戶這可能會導致資訊超載檢視所有的帳戶 （在此情況下或一開始瀏覽頁面時按一下 所有的 LinkButton） 時。 若要協助更易於管理的區塊中出現的使用者帳戶，讓我們設定一次顯示 10 個使用者帳戶在 GridView。

GridView 控制項提供兩種類型的分頁：

- **預設分頁**-容易實作，但沒有效率。 簡單地說，使用預設分頁 GridView 預期*所有*與其資料來源的記錄。 接著只會顯示記錄的適當的頁面。
- **自訂分頁**-需要更多工作來實作，但比預設分頁因為使用自訂分頁的資料來源只傳回精確組要顯示的記錄更有效率。

進行上千筆記錄的分頁時，預設和自訂分頁的效能差異可能相當大。 因為我們會建置假設沒有這個介面可能是數百或數千個使用者帳戶，讓我們使用自訂分頁。

> [!NOTE]
> 預設和自訂分頁，以及實作自訂分頁所涉及的挑戰之間差異的更完整討論，請參閱[有效率地透過大型量的資料分頁](https://asp.net/learn/data-access/tutorial-25-cs.aspx)。 預設和自訂分頁的效能差異一些分析，請參閱[在 ASP.NET 中使用 SQL Server 2005 的自訂分頁](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)。


若要實作自訂分頁我們首先需要一些機制，用來擷取記錄正在顯示之 GridView 的精確子集。 好消息是`Membership`類別的`FindUsersByName`方法具有多載，可讓您指定的頁面索引和頁面大小，並傳回該記錄的範圍內的使用者帳戶。

特別是，這個多載具有下列簽章： [ `FindUsersByName(usernameToMatch, pageIndex, pageSize, totalRecords)` ](https://msdn.microsoft.com/library/fa5st8b2.aspx)。

*PageIndex*參數會指定要傳回; 的使用者帳戶頁面*pageSize*指出每頁顯示的多少筆記錄。 *TotalRecords*參數是`out`在使用者存放區傳回的總使用者帳戶數目的參數。

> [!NOTE]
> 所傳回的資料`FindUsersByName`依照使用者名稱; 無法自訂排序準則。


若要利用自訂分頁，但只有當繫結至 ObjectDataSource 控制項可以設定 GridView。 要實作自訂分頁 ObjectDataSource 控制項，它需要兩個方法： 一個會傳遞開始的資料列索引和記錄，若要顯示，最大數目，並傳回落在該範圍; 的記錄的精確子集並透過正在呼叫的方法會傳回的記錄總數。 `FindUsersByName`多載接受索引頁面和頁面大小，然後傳回的總記錄數透過`out`參數。 因此會有的介面不相符。

其中一個選項是建立 proxy 類別，公開的介面 ObjectDataSource 預期，並從內部呼叫`FindUsersByName`方法。 另一個選項和一個我們會使用這個發行項為是建立自己的分頁介面及使用，而不是 GridView 的內建的分頁介面。

### <a name="creating-a-first-previous-next-last-paging-interface"></a>建立第一個，先前，接下來，最後一個分頁介面

我們使用第一個、 上一步下, 一步 和最後一個 LinkButtons 建置分頁介面。 第一個 LinkButton 按一下時，會將使用者導向至第一頁資料，而上一步會傳回他的上一頁。 同樣地下, 一個] 和 [最後一個將使用者移到下一個和最後一個頁面上，分別。 新增四個 LinkButton 控制項下方`UserAccounts`GridView。

[!code-aspx[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample11.aspx)]

接下來，建立事件處理常式的每個的 LinkButton`Click`事件。

圖 7 顯示四個 LinkButtons 透過 Visual Web Developer 設計檢視中檢視時。


[![接下來，加入第一個、 上一個和最後一個 LinkButtons 下方 GridView](building-an-interface-to-select-one-user-account-from-many-cs/_static/image20.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image19.png)

**圖 7**： 加入第一個、 上一步下, 一步和最後一個 LinkButtons 下方 GridView ([按一下以檢視完整大小的影像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image21.png))


### <a name="keeping-track-of-the-current-page-index"></a>追蹤的目前頁面索引

當使用者第一次造訪`ManageUsers.aspx`頁面或按一下其中一個篩選按鈕，我們會想要在 GridView 中顯示資料的第一頁。 不過，當使用者按一下瀏覽 LinkButtons 的其中一個時，我們需要更新的頁面索引。 若要維護的頁面索引和每頁顯示的記錄數目，加入頁面的程式碼後置類別中的下列兩個屬性：

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample12.cs)]

像`UsernameToMatch`屬性，`PageIndex`屬性保存其檢視狀態的值。 唯讀`PageSize`屬性會傳回硬式編碼值，10。 邀請更新此屬性，以使用相同的模式有興趣的讀取器`PageIndex`，然後再擴大`ManageUsers.aspx`頁面上，瀏覽頁面的人員可以指定多少的使用者帳戶，以顯示每個頁面上。

### <a name="retrieving-just-the-current-pages-records-updating-the-page-index-and-enabling-and-disabling-the-paging-interface-linkbuttons"></a>擷取目前頁面的記錄為止，更新頁面的索引，以及啟用和停用分頁介面 LinkButtons

與就地分頁介面和`PageIndex`和`PageSize`加入屬性，我們已準備好更新`BindUserAccounts`方法，使它會使用適當`FindUsersByName`多載。 此外，我們必須先啟用或停用分頁介面，根據哪個頁面會顯示這個方法。 檢視資料的第一頁時, 的第一個和上一個連結應該停用;檢視的最後一頁時，應該停用下一個和最後一個。

以下列程式碼更新 `BindUserAccounts` 方法：

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample13.cs)]

請注意，透過正在呼叫的記錄總數取決於最後一個參數`FindUsersByName`方法。 這是`out`參數，因此我們需要先宣告變數，以保留此值 (`totalRecords`) 並再將它與前置詞`out`關鍵字。

傳回指定的頁面的使用者帳戶之後，四個 LinkButtons 啟用或停用，根據是否正在檢視資料的第一個或最後一頁。

最後一個步驟是撰寫的程式碼的四個 LinkButtons'`Click`事件處理常式。 這些事件處理常式需要更新`PageIndex`屬性然後重新繫結至 GridView，透過對呼叫資料`BindUserAccounts`。 第一個、 上一個] 和 [下一步的事件處理常式就會非常簡單。 `Click`事件處理常式的最後一個的 LinkButton，不過，是比較複雜一點因為我們需要決定以判斷最後一個頁面索引顯示的多少筆記錄的。

[!code-csharp[Main](building-an-interface-to-select-one-user-account-from-many-cs/samples/sample14.cs)]

數字 8 和 9 示範自訂分頁介面的操作方式。 圖 8 顯示`ManageUsers.aspx`頁面上檢視所有使用者帳戶資料的第一頁時。 請注意，只有以 10 為 13 的帳戶會顯示。 按一下 下一步或最後一個連結會導致回傳時，更新`PageIndex`成 1，而第二個頁面的使用者帳戶新增至方格的繫結 （請參閱圖 9）。


[![會顯示第一個 10 使用者帳戶](building-an-interface-to-select-one-user-account-from-many-cs/_static/image23.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image22.png)

**圖 8**： 會顯示第一個 10 使用者帳戶 ([按一下以檢視完整大小的影像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image24.png))


[![按一下 [下一步] 連結會顯示第二個頁面的使用者帳戶](building-an-interface-to-select-one-user-account-from-many-cs/_static/image26.png)](building-an-interface-to-select-one-user-account-from-many-cs/_static/image25.png)

**圖 9**： 按一下 [下一步] 連結會顯示第二個頁面的使用者帳戶 ([按一下以檢視完整大小的影像](building-an-interface-to-select-one-user-account-from-many-cs/_static/image27.png))


## <a name="summary"></a>總結

若要從帳戶清單中選取使用者通常需要系統管理員。 在先前的教學課程中我們探討使用下拉式清單，填入使用者，但這種方法無法妥善調整。 在本教學課程中，我們探索較佳替代方式： 可篩選介面，其結果會顯示在分頁 GridView。 與此使用者介面，系統管理員可以快速且有效地找出並選取一個千分位之間的使用者帳戶。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [在 ASP.NET 中使用 SQL Server 2005 的自訂分頁](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)
- [有效率地大量的資料進行分頁](https://asp.net/learn/data-access/tutorial-25-cs.aspx)
- [復原您的網站管理工具](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP/ASP.NET 書籍的作者和創辦的 4GuysFromRolla.com，具有已經使用 Microsoft Web 技術從 1998 年。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿 *[Sam 教導您自己 ASP.NET 2.0 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 在可到達 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過在他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 Alicja Maziarz。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行 [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [下一步](recovering-and-changing-passwords-cs.md)
