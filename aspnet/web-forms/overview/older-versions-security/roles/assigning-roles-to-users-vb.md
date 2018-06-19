---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
title: 將角色指派給使用者 (VB) |Microsoft 文件
author: rick-anderson
description: 本教學課程中我們將建立兩個 ASP.NET 網頁，以協助管理使用者屬於哪些角色。 第一頁會包含地查看哪些設備...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: fd208ee9-69cc-4467-9783-b4e039bdd1d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
msc.type: authoredcontent
ms.openlocfilehash: 959a73f53d4fdb114f222fe8bc830876b76c9d9e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892006"
---
<a name="assigning-roles-to-users-vb"></a>將角色指派給使用者 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.10.zip)或[下載 PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_vb.pdf)

> 本教學課程中我們將建立兩個 ASP.NET 網頁，以協助管理使用者屬於哪些角色。 第一頁會包含設備，以查看哪些使用者屬於指定角色，具備特定的使用者所屬的角色和指派或移除特定角色中的特定使用者的能力。 在第二個頁面中我們會增加適用於 CreateUserWizard 控制項使其包含的步驟，以指定新建立的使用者屬於哪些角色。 這是系統管理員可以建立新的使用者帳戶的案例中很有用。


## <a name="introduction"></a>簡介

<a id="_msoanchor_1"> </a>[上一個教學課程](creating-and-managing-roles-vb.md)檢查角色架構和`SqlRoleProvider`; 我們可了解如何使用`Roles`類別來建立、 擷取和刪除角色。 除了建立和刪除角色，我們需要能夠指派或移除角色中的使用者。 不幸的是，ASP.NET 並未隨附任何 Web 控制項，來管理使用者屬於哪些角色。 相反地，我們必須建立自己的 ASP.NET 網頁，來管理這些關聯。 好消息是加入和移除使用者角色是相當簡單。 `Roles`類別包含許多方法來將一個或多個使用者加入至一或多個角色。

本教學課程中我們將建立兩個 ASP.NET 網頁，以協助管理使用者屬於哪些角色。 第一頁會包含設備，以查看哪些使用者屬於指定角色，具備特定的使用者所屬的角色和指派或移除特定角色中的特定使用者的能力。 在第二個頁面中我們會增加適用於 CreateUserWizard 控制項使其包含的步驟，以指定新建立的使用者屬於哪些角色。 這是系統管理員可以建立新的使用者帳戶的案例中很有用。

讓我們開始吧 ！

## <a name="listing-what-users-belong-to-what-roles"></a>列出哪些使用者屬於哪些角色

首先在本教學課程是建立網頁的使用者可以指派給角色。 我們將自己有關如何將使用者指派角色之前，讓我們先專注於如何判斷哪些使用者所屬的角色。 有兩種方式顯示這項資訊: 「 角色 」 或 「 使用者。 」 我們無法允許訪客選取角色，然後顯示它們的所有使用者屬於角色 （「 依角色 」 顯示），或我們可以提示訪客能夠選取使用者，然後顯示它們的角色指派給該使用者 （「 依使用者 」 顯示）。

「 依角色 」 檢視很有用的情況下其中造訪者想要知道的一組使用者隸屬於特定的角色。訪客需要知道特定的使用者角色時，理想的 「 依使用者 」 檢視。 讓我們先我們包含 「 依角色 」 和 「 依使用者 」 的網頁介面。

我們將開始建立 「 依使用者 」 介面。 此介面將包含下拉式清單和核取方塊的清單。 下拉式清單會填入一組使用者在系統中。核取方塊將會列舉這些角色。 從下拉式清單中選取使用者將會檢查那些使用者所屬的角色。 瀏覽頁面的人員可以再選取或取消選取核取方塊，以加入或移除對應的角色選取的使用者。

> [!NOTE]
> 使用下拉式清單加入至清單的使用者帳戶不是網站的理想選擇其中可能有數百個使用者帳戶。 下拉式清單的設計可讓使用者能夠選取的選項在相對較短的清單中的一個項目。 它很快會變得不便隨著清單項目數目。 如果您要建置的網站，將會有潛在大量的使用者帳戶，您可能要考慮使用替代的使用者介面，例如可分頁 GridView 或列出可篩選介面提示的造訪者選擇代號，然後只顯示其使用者名稱以選取的字母為開頭的這些使用者。


## <a name="step-1-building-the-by-user-user-interface"></a>步驟 1： 建立 「 依使用者 」 使用者介面

開啟`UsersAndRoles.aspx`頁面。 在頁面頂端，加入名為標籤 Web 控制項`ActionStatus`和清除其`Text`屬性。 我們將使用此標籤，來提供意見反應，所執行之動作顯示類似的訊息，「 使用者 Tito 已新增至系統管理員角色中，"或者"使用者 Jisun 已經移除了監督員角色。 」 若要將這些訊息突顯出來，設定標籤的`CssClass`「 重要 」 的屬性。

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample1.aspx)]

接下來，加入下列 CSS 類別定義，以`Styles.css`樣式表：

[!code-css[Main](assigning-roles-to-users-vb/samples/sample2.css)]

這個 CSS 定義會指示瀏覽器顯示的標籤使用大型的紅色字型。 圖 1 顯示此效果，透過 Visual Studio 設計工具。


[![標籤的 CssClass 屬性會導致大型、 紅色字型](assigning-roles-to-users-vb/_static/image2.png)](assigning-roles-to-users-vb/_static/image1.png)

**圖 1**: 標籤`CssClass`屬性結果中的大型紅色字型 ([按一下以檢視完整大小的影像](assigning-roles-to-users-vb/_static/image3.png))


接下來，將 DropDownList 加入至頁面上，設定其`ID`屬性`UserList`，並設定其`AutoPostBack`屬性設定為 True。 若要列出的所有使用者在系統中，我們將使用此 DropDownList。 將繫結此 DropDownList MembershipUser 物件的集合。 因為我們想要 DropDownList 以顯示 MembershipUser 物件的使用者名稱屬性 （並使用它做為清單項目的值），設定 DropDownList`DataTextField`和`DataValueField`屬性為"UserName"。

DropDownList 下方加入名為中繼器`UsersRoleList`。 此中繼器會列出所有角色的系統中以一系列的核取方塊。 定義中繼器`ItemTemplate`使用下列的宣告式標記：

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample3.aspx)]

`ItemTemplate`標記包含名為單一核取方塊 Web 控制項`RoleCheckBox`。 核取方塊的`AutoPostBack`屬性設定為 True 而`Text`屬性繫結至`Container.DataItem`。 資料繫結語法是因為只要`Container.DataItem`是因為角色架構傳回角色名稱的清單，以字串陣列，而且我們會繫結至中繼器這個字串陣列。 顯示陣列繫結至資料的 Web 控制項的內容為什麼使用此語法的完整說明已超出本教學課程的範圍。 如需這個問題的詳細資訊，請參閱[繫結至資料的 Web 控制項的純量陣列](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx)。

此時您 「 依使用者 」 介面的宣告式標記看起來應該如下所示：

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample4.aspx)]

我們現在已準備好撰寫程式碼以將一組使用者帳戶到 DropDownList 和的中繼器的角色繫結。 在頁面的程式碼後置類別中，新增名為`BindUsersToUserList`和另一個名為`BindRolesList`，使用下列程式碼：

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample5.vb)]

`BindUsersToUserList`方法會擷取所有使用者帳戶透過在系統中[`Membership.GetAllUsers`方法](https://msdn.microsoft.com/library/dy8swhya.aspx)。 這會傳回[`MembershipUserCollection`物件](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx)，這是集合的[`MembershipUser`執行個體](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)。 此集合則繫結到`UserList`DropDownList。 `MembershipUser`執行個體的集合包含的各種屬性，如該結構`UserName`， `Email`， `CreationDate`，和`IsOnline`。 若要指示 DropDownList 以顯示的值`UserName`屬性，請確認`UserList`DropDownList 的`DataTextField`和`DataValueField`內容已設定為"UserName"。

> [!NOTE]
> `Membership.GetAllUsers`方法有兩個多載： 一個不接受任何輸入的參數並傳回所有使用者，另一個接受頁面索引和頁面大小的整數值，並傳回使用者的指定的子集。 大量的可分頁的使用者介面項目中顯示的使用者帳戶時，第二個多載可用來更有效率地瀏覽使用者因為它會傳回只精確的使用者帳戶而非全部的子集。


`BindRolesToList`方法會藉由呼叫啟動`Roles`類別的[`GetAllRoles`方法](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx)，它會傳回字串陣列，包含系統中的角色。 這個字串陣列，然後繫結至中繼器。

最後，我們需要先載入的頁面時，請呼叫這兩種方法。 將下列程式碼加入至 `Page_Load` 事件處理常式：

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample6.vb)]

此位置的程式碼，請花一點時間瀏覽的頁面，透過瀏覽器;您的畫面看起來應該類似於圖 2。 所有使用者帳戶會填入下拉式清單中，和下方，每個角色會顯示為核取方塊。 因為我們設定`AutoPostBack`內容 DropDownList 和核取方塊為 True，變更選取的使用者，或是檢查或取消核取的角色會導致回傳。 不執行任何動作，不過，因為我們尚未撰寫程式碼來處理這些動作時。 我們將會處理在接下來兩節中的這些工作。


[![頁面會顯示使用者和角色](assigning-roles-to-users-vb/_static/image5.png)](assigning-roles-to-users-vb/_static/image4.png)

**圖 2**: 頁面會顯示使用者和角色 ([按一下以檢視完整大小的影像](assigning-roles-to-users-vb/_static/image6.png))


### <a name="checking-the-roles-the-selected-user-belongs-to"></a>檢查角色選取的使用者屬於

當第一次載入頁面時，或每當訪客從下拉式清單中選取新使用者時，我們需要更新`UsersRoleList`的核取方塊，以便只有當選取的使用者屬於該角色，會檢查特定的角色的核取方塊。 若要達成此目的，建立名為方法`CheckRolesForSelectedUser`為下列程式碼：

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample7.vb)]

上述程式碼一開始會判斷選取的使用者是誰。 然後它會使用角色類別的[`GetRolesForUser(userName)`方法](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx)傳回指定之使用者的一組角色做為字串陣列。 接下來，會列舉中繼器的項目和每個項目`RoleCheckBox`核取方塊以程式設計方式所參考。 核取方塊才是它會對應至角色內含`selectedUsersRoles`字串陣列。

> [!NOTE]
> `Linq.Enumerable.Contains(Of String)(...)`語法不會編譯，如果您使用 ASP.NET 2.0 版。 `Contains(Of String)`方法屬於[LINQ 程式庫](http://en.wikipedia.org/wiki/Language_Integrated_Query)，這也是以 ASP.NET 3.5。 如果您仍在使用 ASP.NET 2.0 版中，使用[`Array.IndexOf(Of String)`方法](https://msdn.microsoft.com/library/eha9t187.aspx)改為。


`CheckRolesForSelectedUser`方法需要在兩個情況下呼叫： 第一次載入頁面時，只要使用`UserList`DropDownList 的選取的索引已變更。 因此，呼叫這個方法從`Page_Load`事件處理常式 (呼叫之後`BindUsersToUserList`和`BindRolesToList`)。 此外，事件處理常式建立 DropDownList`SelectedIndexChanged`事件，並從該處呼叫這個方法。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample8.vb)]

此位置的程式碼，您可以測試透過瀏覽器頁面。 不過，由於`UsersAndRoles.aspx`頁面目前缺少使用者指派角色的能力，沒有任何使用者角色。 我們將建立指派使用者至角色，很快，因此您可以採取這個程式碼可以運作 my word，並確認它之後，會以的介面，或您可以手動新增使用者至角色插入記錄成`aspnet_UsersInRoles`資料表中，以測試這個 functi現在 onality。

### <a name="assigning-and-removing-users-from-roles"></a>指派和從角色移除使用者

造訪者檢查，或取消核取一個核取方塊`UsersRoleList`中繼器我們需要加入或移除選取的使用者從對應的角色。 核取方塊的`AutoPostBack`屬性目前已設定為 True 時，這會導致回傳，每當 checked 或 unchecked 中繼器中的核取方塊。 簡單地說，我們建立所需的事件處理常式的核取方塊`CheckChanged`事件。 由於在中繼器控制項中核取方塊，我們需要手動加入事件處理常式配管。 將事件處理常式加入至程式碼後置類別，做為啟動`Protected`方法，就像這樣：

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample9.vb)]

我們將在不久後，此事件處理常式撰寫程式碼。 但首先讓我們先完成的事件處理配管。 從內中繼器的核取方塊`ItemTemplate`，新增`OnCheckedChanged="RoleCheckBox_CheckChanged"`。 這個語法繫結在一起`RoleCheckBox_CheckChanged`事件處理常式來`RoleCheckBox`的`CheckedChanged`事件。

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample10.aspx)]

我們最後一項工作是在完成`RoleCheckBox_CheckChanged`事件處理常式。 我們必須藉由參考引發事件，因為這個核取方塊執行個體告訴我們什麼角色已 checked 或 unchecked 透過核取方塊控制項開始其`Text`和`Checked`屬性。 使用所選使用者的使用者名稱以及此資訊，我們從新增或移除使用者透過角色`Roles`類別的[ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx)或[`RemoveUserFromRole`方法](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx)。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample11.vb)]

上述的程式碼會啟動以程式設計方式參考引發事件會透過可用的核取方塊`sender`輸入的參數。 如果勾選此核取方塊，選取的使用者便會加入指定的角色，否則從角色中移除。 在任一情況下，`ActionStatus`標籤會顯示訊息摘要剛才執行的動作。

請花一點時間，若要測試此頁面，透過瀏覽器。 選取使用者 Tito，然後再將 Tito 新增至系統管理員和監督員角色。


[![Tito 已加入系統管理員和監督員角色](assigning-roles-to-users-vb/_static/image8.png)](assigning-roles-to-users-vb/_static/image7.png)

**圖 3**: Tito 已加入系統管理員和監督員角色 ([按一下以檢視完整大小的影像](assigning-roles-to-users-vb/_static/image9.png))


接下來，從下拉式清單中選取使用者 Bruce。 沒有回傳，而且中繼器的核取方塊會更新透過`CheckRolesForSelectedUser`。 Bruce 不尚未屬於任何角色，因為兩個核取方塊未勾選。 接下來，加入 Bruce 監督員角色。


[![已加入 Bruce 監督員角色](assigning-roles-to-users-vb/_static/image11.png)](assigning-roles-to-users-vb/_static/image10.png)

**圖 4**： 監督員角色已新增 Bruce ([按一下以檢視完整大小的影像](assigning-roles-to-users-vb/_static/image12.png))


若要進一步驗證的功能`CheckRolesForSelectedUser`方法中，選取一個 Tito 或 Bruce 以外的使用者。 請注意會自動取消核取核取方塊的方式，此項表示應，它們不屬於任何角色。 返回 Tito。 應檢查系統管理員和監督員核取方塊。

## <a name="step-2-building-the-by-roles-user-interface"></a>步驟 2： 建立 「 依角色 」 使用者介面

現在我們已完成 」 的使用者 」 介面，並已準備好開始處理的 「 依角色 」 介面。 「 依角色 」 介面會提示使用者從下拉式清單中選取角色，接著會顯示一組屬於該角色在 GridView 中的使用者。

將另一個 DropDownList 控制項加入`UsersAndRoles.aspx page`。 這一個中繼器控制項下方，其命名`RoleList`，並設定其`AutoPostBack`屬性設定為 True。 下方，新增 GridView 並將其命名`RolesUserList`。 此 GridView 會列出屬於所選角色的使用者。 設定 GridView`AutoGenerateColumns`屬性設定為 False，將為 TemplateField 加入至方格的`Columns`集合，並設定其`HeaderText`「 使用者 」 的屬性。 定義 TemplateField `ItemTemplate` ，使其顯示資料繫結運算式的值`Container.DataItem`中`Text`屬性的名稱為的標籤`UserNameLabel`。

加入和設定 GridView 之後, 您 「 依角色 」 介面的宣告式標記看起來應該如下所示：

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample12.aspx)]

我們需要填入`RoleList`DropDownList 與系統中的角色集合。 若要完成這項作業，更新`BindRolesToList`讓其繫結的方法所傳回的字串陣列`Roles.GetAllRoles`方法`RolesList`DropDownList (以及`UsersRoleList`中繼器)。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample13.vb)]

中的最後兩行`BindRolesToList`方法已經加入至繫結之一組角色`RoleList`DropDownList 控制項。 圖 5 顯示檢視透過瀏覽器 – 下拉式清單，填入系統角色時的最終結果。


[![角色會顯示在 RoleList DropDownList](assigning-roles-to-users-vb/_static/image14.png)](assigning-roles-to-users-vb/_static/image13.png)

**圖 5**： 會顯示角色`RoleList`DropDownList ([按一下以檢視完整大小的影像](assigning-roles-to-users-vb/_static/image15.png))


### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>顯示屬於所選角色的使用者

當第一次載入頁面，或當新的角色從選取`RoleList`DropDownList，我們需要顯示屬於該角色在 GridView 的使用者清單。 建立名為方法`DisplayUsersBelongingToRole`使用下列程式碼：

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample14.vb)]

此方法會藉由取得從選取的角色啟動`RoleList`DropDownList。 然後它會使用[`Roles.GetUsersInRole(roleName)`方法](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx)擷取屬於該角色之使用者的使用者名稱的字串陣列。 這個陣列則繫結到`RolesUserList`GridView。

需要在兩個情況下呼叫這個方法： 以及時一開始載入的頁面中選取的角色`RoleList`DropDownList 變更。 因此，更新`Page_Load`事件處理常式，讓這個方法會叫用呼叫之後`CheckRolesForSelectedUser`。 接下來，建立事件處理常式`RoleList`的`SelectedIndexChanged`事件，並太從該處，呼叫這個方法。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample15.vb)]

這個程式碼的位置， `RolesUserList` GridView 應該會顯示屬於所選角色的使用者。 如圖 6 所示，監督員角色包含兩個成員： Bruce 和 Tito。


[![在 GridView 列出這些屬於所選角色的使用者](assigning-roles-to-users-vb/_static/image17.png)](assigning-roles-to-users-vb/_static/image16.png)

**圖 6**: GridView 列出那些使用者到屬於選取的角色 ([按一下以檢視完整大小的影像](assigning-roles-to-users-vb/_static/image18.png))


### <a name="removing-users-from-the-selected-role"></a>從選取的角色移除使用者

讓我們來加強`RolesUserList`GridView，使其包含的資料行 [移除] 按鈕。 按一下 「 移除 」 按鈕，針對特定的使用者將它們從該角色中移除。

啟動刪除按鈕欄位加入至 GridView。 讓這個欄位會顯示為已歸檔最左邊，並變更其`DeleteText`屬性 「 移除 」 從 「 刪除 」 （預設值）。


[![新增](assigning-roles-to-users-vb/_static/image20.png)](assigning-roles-to-users-vb/_static/image19.png)

**圖 7**: 「 移除 」 按鈕加入至 GridView ([按一下以檢視完整大小的影像](assigning-roles-to-users-vb/_static/image21.png))


按一下 [移除] 按鈕時回傳展示和 GridView`RowDeleting`就會引發事件。 我們需要建立此事件的事件處理常式和撰寫程式碼，從選取的角色移除使用者。 建立事件處理常式，然後加入下列程式碼：

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample16.vb)]

程式碼一開始會決定選取的角色名稱。 接著，它會以程式設計方式參考`UserNameLabel`控制從資料列的 [移除] 按鈕已按下以判斷要移除的使用者名稱。 然後會從呼叫透過角色移除使用者`Roles.RemoveUserFromRole`方法。 `RolesUserList` GridView 然後重新整理，並透過顯示一則訊息`ActionStatus`標籤控制項。

> [!NOTE]
> 「 移除 」 按鈕不需要任何類型的使用者的確認，再從角色移除使用者。 歡迎您加入某個層級的使用者確認。 確認動作的最簡單方式是透過用戶端確認對話方塊。 如需有關這項技術的詳細資訊，請參閱[新增用戶端確認時刪除](https://asp.net/learn/data-access/tutorial-42-vb.aspx)。


圖 8 顯示頁面之後使用者 Tito 已經移除了監督員群組。


[![還有，Tito 不再監督員](assigning-roles-to-users-vb/_static/image23.png)](assigning-roles-to-users-vb/_static/image22.png)

**圖 8**： 還有，Tito 不再監督員 ([按一下以檢視完整大小的影像](assigning-roles-to-users-vb/_static/image24.png))


### <a name="adding-new-users-to-the-selected-role"></a>將使用者新增到選取的角色

從選取的角色中移除使用者，以及此頁面的訪客也應該能夠將使用者新增到選取的角色。 將使用者加入到選取的角色的最佳介面取決於您預期的使用者帳戶數目。 如果您的網站將裝載在短短幾數十個使用者帳戶或更短，您可以在這裡使用 DropDownList。 如果可能有數千個使用者帳戶，您會想要包含允許透過搜尋特定帳戶，帳戶頁面上，或篩選的其他方式的使用者帳戶的造訪者的使用者介面。

此頁面讓我們使用 不論使用者帳戶數目系統中的運作方式非常簡單的介面。 也就是說，我們將使用文字方塊中，提示她想要新增到選取的角色之使用者的使用者名稱中所輸入的訪客。 如果沒有任何使用者具有該名稱存在，或如果使用者已經是角色的成員，我們會顯示在訊息`ActionStatus`標籤。 但是，如果使用者存在，且不是角色的成員，我們會將其加入至角色，並重新整理方格。

加入文字方塊和按鈕下方 GridView。 設定文字方塊的`ID`至`UserNameToAddToRole`並設定按鈕的`ID`和`Text`屬性`AddUserToRoleButton`和 「 新增使用者至角色 」，分別。

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample17.aspx)]

接下來，建立`Click`事件處理常式`AddUserToRoleButton`並加入下列程式碼：

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample18.vb)]

中的程式碼的大部分`Click`事件處理常式會執行各種的驗證檢查。 它會確保訪客提供中的使用者名稱`UserNameToAddToRole`文字方塊中，使用者存在於系統中，而且它們已不屬於選取的角色。 如果其中任何一項檢查失敗，有適當錯誤訊息會顯示在`ActionStatus`和結束事件處理常式。 如果所有檢查都通過，要將使用者新增到透過角色`Roles.AddUserToRole`方法。 接下來，文字方塊的`Text`清除屬性、 重新整理 GridView 和`ActionStatus`標籤會顯示訊息，指出指定的使用者已成功加入到選取的角色。

> [!NOTE]
> 若要確保指定的使用者已經不隸屬於選取的角色，我們使用[`Roles.IsUserInRole(userName, roleName)`方法](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx)，它會傳回布林值，指出是否*userName* 隸屬*roleName*。 我們將使用中的，這個方法一次<a id="_msoanchor_2"> </a>[下一個教學課程](role-based-authorization-vb.md)當我們會審視角色為基礎的授權。


透過瀏覽器網頁並選取從的監督員角色`RoleList`DropDownList。 請輸入使用者名稱無效，您應該會看到訊息，說明使用者不存在系統中。


[![您無法將不存在使用者加入角色](assigning-roles-to-users-vb/_static/image26.png)](assigning-roles-to-users-vb/_static/image25.png)

**圖 9**： 您無法加入不存在使用者至角色 ([按一下以檢視完整大小的影像](assigning-roles-to-users-vb/_static/image27.png))


現在，請嘗試加入的有效使用者。 請繼續並重新加入 Tito 監督員角色。


[![Tito 同樣是由主管 ！](assigning-roles-to-users-vb/_static/image29.png)](assigning-roles-to-users-vb/_static/image28.png)

**圖 10**: Tito 同樣是由主管 ！  ([按一下以檢視完整大小的影像](assigning-roles-to-users-vb/_static/image30.png))


## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>步驟 3： 跨更新 「 使用者 」 和 「 角色 」 介面

`UsersAndRoles.aspx`頁面提供兩個不同的介面來管理使用者和角色。 目前，這兩個介面的作用彼此所以有可能，一個介面中所做的變更將不會立即反映在其他。 例如，想像一下頁面的造訪者選取的監督員角色`RoleList`DropDownList，列出 Bruce 和 Tito 做為其成員。 下一步，造訪者選取從 Tito `UserList` DropDownList，簽入的系統管理員和監督員核取方塊`UsersRoleList`中繼器。 如果訪客然後取消核取中繼器從 「 主管 」 角色，Tito 從角色中移除監督員，但這項修改不會反映在 「 依角色 」 介面。 在 GridView 仍將顯示 Tito 視為監督員角色的成員。

若要修正此我們需要重新整理 GridView 每當角色是 checked 或 unchecked 從`UsersRoleList`中繼器。 同樣地，我們需要重新整理中繼器，每當使用者移除，或從 「 依角色 」 介面加入至角色。

藉由呼叫重新整理 」 的使用者 」 介面中中繼器`CheckRolesForSelectedUser`方法。 「 依角色 」 介面都可以在修改`RolesUserList`GridView 的`RowDeleting`事件處理常式和`AddUserToRoleButton`按鈕的`Click`事件處理常式。 因此，我們需要呼叫`CheckRolesForSelectedUser`從每一種方法的方法。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample19.vb)]

同樣地，在 「 依角色 」 介面 GridView 會重新整理藉由呼叫`DisplayUsersBelongingToRole`方法和 「 依使用者 」 介面透過修改`RoleCheckBox_CheckChanged`事件處理常式。 因此，我們需要呼叫`DisplayUsersBelongingToRole`從這個事件處理常式方法。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample20.vb)]

這些次要的程式碼變更，「 使用者 」 和 「 角色 」 介面現在正確跨更新。 若要確認這種情況，請瀏覽透過瀏覽器頁面，然後選取 Tito 和監督員從`UserList`和`RoleList`DropDownLists，分別。 請注意，當您針對從 「 依使用者 」 介面中中繼器 Tito 取消監督員角色，Tito 會自動移除在 「 依角色 」 介面 GridView。 回到監督員角色中將 Tito 加入從 「 依角色 」 介面時，會自動重新檢查 」 的使用者 」 介面中的監督員核取方塊。

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>步驟 4： 自訂以包含 「 指定的角色 」 的步驟適用於 CreateUserWizard

在<a id="_msoanchor_3"> </a> [*建立使用者帳戶*](../membership/creating-user-accounts-vb.md)教學課程中我們可了解如何使用適用於 CreateUserWizard Web 控制項來提供介面來建立新的使用者帳戶。 適用於 CreateUserWizard 控制項可以用於兩種方式之一：

- 訪客在網站上，建立自己的使用者帳戶作為和
- 做為方法，以建立新帳戶的系統管理員

在第一個使用案例中，訪客會移至站台，並填寫適用於 CreateUserWizard，以便在網站上註冊輸入他們的資訊。 在第二個案例中，系統管理員會建立新的帳戶，另一位人員。

當由系統管理員為某些其他人正在建立帳戶時，它可能很有幫助以允許系統管理員指定哪些新使用者帳戶所屬的角色。 在<a id="_msoanchor_4"> </a> [*儲存**其他使用者資訊*](../membership/storing-additional-user-information-vb.md)我們可了解如何新增其他自訂適用於 CreateUserWizard 教學課程`WizardSteps`. 讓我們看看如何適用於 CreateUserWizard 新增額外的步驟，才能指定新的使用者角色。

開啟`CreateUserWizardWithRoles.aspx`頁面上，加入名為適用於 CreateUserWizard 控制項`RegisterUserWithRoles`。 將控制項的`ContinueDestinationPageUrl`屬性為"~ / Default.aspx"。 因為這裡的想法是系統管理員將會使用此適用於 CreateUserWizard 控制項來建立新的使用者帳戶，將控制項的`LoginCreatedUser`屬性設定為 False。 這`LoginCreatedUser`屬性指定是否訪客自動剛建立以使用者身分，登入，而且它會預設為 True。 我們將它設定為 False 因為系統管理員建立新的帳戶，我們想要保留他自己的身分登入。

接下來，選取 「 新增/移除`WizardSteps`...」 從適用於 CreateUserWizard 的智慧標籤的選項，然後將新`WizardStep`，設定其`ID`至`SpecifyRolesStep`。 移動`SpecifyRolesStep WizardStep`如此就會自動收到 「 登註冊您新帳戶 」 的步驟之後，但 「 完成 」 步驟之前。 設定`WizardStep`的`Title`「 指定角色 」 的屬性其`StepType`屬性`Step`，且其`AllowReturn`屬性設定為 False。


[![新增](assigning-roles-to-users-vb/_static/image32.png)](assigning-roles-to-users-vb/_static/image31.png)

**圖 11**： 新增 「 指定角色 」`WizardStep`至適用於 CreateUserWizard ([按一下以檢視完整大小的影像](assigning-roles-to-users-vb/_static/image33.png))


這項變更後適用於 CreateUserWizard 的宣告式標記看起來應該如下所示：

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample21.aspx)]

在 [指定角色] `WizardStep`，加入名為 CheckBoxList`RoleList.`此 CheckBoxList 會列出可用的角色，新建立的使用者啟用瀏覽頁面所要檢查哪些角色的人員所屬。

我們所剩的兩個編碼工作： 首先我們必須填入`RoleList`CheckBoxList; 系統中的角色與第二，我們要建立的使用者新增到選取的角色，當使用者將 「 指定的角色 」 步驟中的 「 完成 」 步驟。 我們可以完成的第一個工作`Page_Load`事件處理常式。 下列的程式碼以程式設計方式參考`RoleList`核取方塊，在第一個瀏覽至頁面，並將系統中的角色繫結到它。

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample22.vb)]

上述程式碼應該看起來很熟悉。 在<a id="_msoanchor_5"> </a> [*儲存**其他使用者資訊*](../membership/storing-additional-user-information-vb.md)教學課程中我們使用兩個`FindControl`陳述式，以參考 Web 控制項從自訂內`WizardStep`。 並將角色繫結至 CheckBoxList 的程式碼取自稍早在本教學課程。

若要執行的第二個的程式設計工作，我們需要知道何時會完成的 「 指定的角色 」 步驟。 提醒您，適用於 CreateUserWizard 具有`ActiveStepChanged`引發的事件，每次訪客步驟之間瀏覽至另一個。 這裡我們可以判斷使用者是否已達到 「 完成 」 步驟。如果是這樣，我們需要將使用者加入到選取的角色。

建立事件處理常式`ActiveStepChanged`事件並加入下列程式碼：

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample23.vb)]

如果使用者只已達到 「 已完成 」 步驟，此事件處理常式列舉的項目`RoleList`CheckBoxList 和剛建立的使用者指派給選取的角色。

請瀏覽此頁面，透過瀏覽器。 適用於 CreateUserWizard 的第一個步驟是標準的 「 登註冊您新帳戶 」 步驟會提示輸入新使用者的使用者名稱、 密碼、 電子郵件和其他重要資訊。 輸入來建立名為 Wanda 的新使用者的資訊。


[![建立名為 Wanda 的新使用者](assigning-roles-to-users-vb/_static/image35.png)](assigning-roles-to-users-vb/_static/image34.png)

**圖 12**： 建立新的使用者名稱為 Wanda ([按一下以檢視完整大小的影像](assigning-roles-to-users-vb/_static/image36.png))


按一下 [建立使用者] 按鈕。 會在內部呼叫適用於 CreateUserWizard`Membership.CreateUser`方法，建立新的使用者帳戶，然後會前進到下一個步驟中，「 指定的角色。 」 此處會列出系統角色。 選取監督員核取方塊，然後按一下 [下一步]。


[![請 Wanda 監督員角色的成員](assigning-roles-to-users-vb/_static/image38.png)](assigning-roles-to-users-vb/_static/image37.png)

**圖 13**： 請 Wanda 監督員角色的成員 ([按一下以檢視完整大小的影像](assigning-roles-to-users-vb/_static/image39.png))


按一下 下一步會導致回傳和更新`ActiveStep`「 完成 」 步驟。 在`ActiveStepChanged`事件處理常式，最近建立的使用者帳戶指派給監督員角色。 若要確認，傳回到`UsersAndRoles.aspx`頁面上，選取監督員從`RoleList`DropDownList。 如圖 14 所示，監督員現在會啟動在三個使用者： Bruce、 Tito 和 Wanda。


[![Bruce、 Tito 和 Wanda 是所有的監督員](assigning-roles-to-users-vb/_static/image41.png)](assigning-roles-to-users-vb/_static/image40.png)

**圖 14**: Bruce、 Tito 和 Wanda 是所有的監督員 ([按一下以檢視完整大小的影像](assigning-roles-to-users-vb/_static/image42.png))


## <a name="summary"></a>總結

角色架構會提供方法來擷取特定使用者的角色和方法來判斷哪些使用者屬於指定角色的相關資訊。 此外，有許多方法來新增和移除一或多個使用者新增至一個或多個角色。 在此教學課程中我們著重於只其中兩種方式：`AddUserToRole`和`RemoveUserFromRole`。 有其他的變化，設計用來將多個使用者加入至單一角色，並將多個角色指派給單一使用者。

本教學課程也包含看擴充以包含適用於 CreateUserWizard 控制項`WizardStep`來指定新建立的使用者角色。 這類步驟可能有助於簡化程序建立新使用者的使用者帳戶的系統管理員。

此時，我們已經看到如何建立和刪除角色，以及如何加入和移除角色的使用者。 但我們還沒有查看套用以角色為基礎的授權。 在<a id="_msoanchor_6"> </a>[下列教學課程](role-based-authorization-vb.md)我們將探討定義角色的角色為基礎的 URL 授權規則以及如何將頁面層級功能限制在以目前登入的使用者角色。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET 網站管理工具概觀](https://msdn.microsoft.com/library/ms228053.aspx)
- [正在檢查 ASP。網路的成員資格、 角色和設定檔](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [復原您的網站管理工具](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP/ASP.NET 書籍的作者和創辦的 4GuysFromRolla.com，具有已經使用 Microsoft Web 技術從 1998 年。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿 *[Sam 教導您自己 ASP.NET 2.0 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 在可到達 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過在他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝...

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已本文菲。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行 [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](creating-and-managing-roles-vb.md)
> [下一頁](role-based-authorization-vb.md)
