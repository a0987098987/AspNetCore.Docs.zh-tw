---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-cs
title: 將角色指派給使用者 (C#) |Microsoft Docs
author: rick-anderson
description: 在本教學課程中，我們將建置兩個 ASP.NET 網頁，以協助管理哪些使用者屬於哪些角色。 第一頁會包含看有哪些功能...
ms.author: aspnetcontent
ms.date: 03/24/2008
ms.assetid: d522639a-5aca-421e-9a76-d73f95607f57
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-cs
msc.type: authoredcontent
ms.openlocfilehash: f8573afc2fd5f12611f88f8bdad7e14389017808
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820126"
---
<a name="assigning-roles-to-users-c"></a>將角色指派給使用者 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.10.zip)或[下載 PDF](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_cs.pdf)

> 在本教學課程中，我們將建置兩個 ASP.NET 網頁，以協助管理哪些使用者屬於哪些角色。 第一頁會包含功能若要查看哪些使用者屬於指定的角色，具備特定的使用者所屬的角色和指派或移除特定角色中的特定使用者的能力。 在第二個頁面中，我們會擴大 CreateUserWizard 控制項使其包含的步驟，以指定新建立的使用者屬於哪些角色。 這是系統管理員能夠建立新的使用者帳戶的案例中很有用。


## <a name="introduction"></a>簡介

<a id="_msoanchor_1"> </a>[先前的教學課程](creating-and-managing-roles-cs.md)檢查角色架構並`SqlRoleProvider`; 我們了解如何使用`Roles`類別來建立、 擷取及刪除角色。 除了建立及刪除角色，我們必須要能夠指派或移除角色中的使用者。 不幸的是，ASP.NET 並未隨附任何 Web 控制項，來管理哪些使用者屬於哪些角色。 相反地，我們必須建立自己的 ASP.NET 網頁，來管理這些關聯。 好消息是新增和移除使用者角色是相當簡單。 `Roles`類別包含數個方法來將一或多個使用者新增至一個或多個角色。

在本教學課程中，我們將建置兩個 ASP.NET 網頁，以協助管理哪些使用者屬於哪些角色。 第一頁會包含功能若要查看哪些使用者屬於指定的角色，具備特定的使用者所屬的角色和指派或移除特定角色中的特定使用者的能力。 在第二個頁面中，我們會擴大 CreateUserWizard 控制項使其包含的步驟，以指定新建立的使用者屬於哪些角色。 這是系統管理員能夠建立新的使用者帳戶的案例中很有用。

讓我們開始吧 ！

## <a name="listing-what-users-belong-to-what-roles"></a>列出哪些使用者屬於哪些角色

第一要務本教學課程是建立網頁的使用者可以指派給角色。 我們考慮自行如何將使用者指派給角色之前，讓我們先把焦點放在如何判斷哪些使用者屬於哪些角色。 有兩種方式來顯示這項資訊: 「 角色 」 或 「 使用者 」。 我們可能會允許訪客能夠選取角色，然後顯示它們的所有使用者屬於角色 （「 依角色 」 顯示），或我們無法提示來選取使用者，然後顯示它們的角色指派給該使用者 （「 依使用者 「 顯示器） 的訪客。

「 依角色 」 檢視適合用在情況下，造訪者想要知道使用者隸屬於特定的角色中，集合造訪者必須知道特定使用者的角色時，理想的 「 依使用者 」 檢視。 讓我們有我們同時包含 「 依角色 」 和 「 依使用者 」 的網頁介面。

我們將開始建立 「 依使用者 」 的介面。 此介面將包含下拉式清單和核取方塊的清單。 下拉式清單會填入一組使用者在系統中;核取方塊會列舉的角色。 從下拉式清單中選取使用者將會檢查這些使用者所屬的角色。 再檢查或取消選取核取方塊，以新增或移除對應的角色中的所選的使用者瀏覽頁面的人員。

> [!NOTE]
> 使用下拉式清單加入至清單的使用者帳戶不是網站的理想選擇，可能有數百個使用者帳戶。 下拉式清單可讓使用者選擇的選項在相對較短的清單中的一個項目。 快速地變得難以隨著清單項目數目。 如果您要建置的網站，將會有可能很大的數字的使用者帳戶，您可能要考慮使用替代的使用者介面，例如可分頁的 GridView 或列出的可篩選介面會提示選擇字母的訪客，然後只顯示使用者名稱開頭為所選的字母這些使用者。


## <a name="step-1-building-the-by-user-user-interface"></a>步驟 1： 建立 「 依使用者 」 使用者介面

開啟`UsersAndRoles.aspx`頁面。 在頁面頂端，新增名為的 Label Web 控制項`ActionStatus`並清除其`Text`屬性。 我們將使用此標籤來顯示這類訊息上執行的動作提供意見反應，「 使用者 Tito 已加入到系統管理員角色，「 或者 」 使用者 Jisun 都已獲得 「 主管 」 角色。 」 若要讓這些訊息脫穎而出，將標籤的`CssClass`"Important"的屬性。

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample1.aspx)]

接下來，新增下列 CSS 類別定義，以`Styles.css`樣式表：

[!code-css[Main](assigning-roles-to-users-cs/samples/sample2.css)]

這個 CSS 定義會指示瀏覽器顯示使用大型、 紅色字型的標籤。 [圖 1] 顯示此效果，透過 Visual Studio 設計工具。


[![標籤的 CssClass 屬性會導致大型的紅色字型](assigning-roles-to-users-cs/_static/image2.png)](assigning-roles-to-users-cs/_static/image1.png)

**圖 1**: 標籤`CssClass`導致大 」，紅色字型的屬性 ([按一下以檢視完整大小的影像](assigning-roles-to-users-cs/_static/image3.png))


接下來，將 DropDownList 新增至頁面上，設定其`ID`屬性，以`UserList`，並設定其`AutoPostBack`屬性設為 True。 若要列出的所有使用者在系統中，我們將使用此 DropDownList。 將繫結此 DropDownList MembershipUser 物件的集合。 因為我們希望 DropDownList 以顯示 MembershipUser 物件的使用者名稱屬性 （並使用它做為清單項目的值），設定 DropDownList`DataTextField`和`DataValueField`屬性為"UserName"。

下方的下拉式清單中，新增名為 Repeater `UsersRoleList`。 這個 Repeater 會列出所有角色的系統中以一系列的核取方塊。 定義 Repeater`ItemTemplate`使用下列的宣告式標記：

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample3.aspx)]

`ItemTemplate`標記包含單一的核取方塊 Web 控制項，名為`RoleCheckBox`。 此核取方塊`AutoPostBack`屬性設定為 True，`Text`屬性的繫結至`Container.DataItem`。 資料繫結語法做只是`Container.DataItem`是因為角色架構會傳回角色名稱的清單，以字串陣列，而且我們將會繫結至 Repeater 這個字串陣列。 為什麼要顯示繫結至資料 Web 控制項陣列的內容使用此語法的完整描述已超出本教學課程的範圍。 如需有關此問題的詳細資訊，請參閱[繫結至資料 Web 控制項的純量陣列](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx)。

此時您 「 依使用者 」 的介面的宣告式標記看起來應該如下所示：

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample4.aspx)]

我們現在已準備好撰寫程式碼，以將一組使用者帳戶至 DropDownList 和 Repeater 的規則集合繫結。 在頁面的程式碼後置類別中，新增名為`BindUsersToUserList`而另一個名為`BindRolesList`，使用下列程式碼：

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample5.cs)]

`BindUsersToUserList`方法則會透過在系統中擷取所有使用者帳戶[`Membership.GetAllUsers`方法](https://msdn.microsoft.com/library/dy8swhya.aspx)。 這會傳回[`MembershipUserCollection`物件](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx)，這是集合的[`MembershipUser`執行個體](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)。 這個集合則繫結到`UserList`DropDownList。 `MembershipUser`執行個體的集合包含各種不同的屬性，例如該結構`UserName`， `Email`， `CreationDate`，和`IsOnline`。 若要指示 DropDownList 以顯示的值`UserName`屬性，確定`UserList`DropDownList 的`DataTextField`和`DataValueField`屬性都已設定為"UserName"。

> [!NOTE]
> `Membership.GetAllUsers`方法有兩個多載︰ 一個可接受任何輸入的參數且傳回的所有使用者，以及其中的頁面索引和頁面大小的整數值會採用並傳回使用者的指定的子集。 大量的可分頁的使用者介面項目中所顯示的使用者帳戶時，第二個多載可用來更有效率地逐頁查看使用者因為它會傳回只是使用者帳戶而非所有人都精確子集。


`BindRolesToList`方法會呼叫`Roles`類別的[`GetAllRoles`方法](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx)，它會傳回字串陣列，包含系統中的角色。 此字串陣列，然後繫結至 Repeater。

最後，我們需要呼叫這兩種方法，當第一次載入此頁面。 將下列程式碼加入至 `Page_Load` 事件處理常式：

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample6.cs)]

使用此程式碼就緒之後，請花一點時間瀏覽的頁面，透過瀏覽器;您的畫面看起來應該類似於圖 2。 所有使用者帳戶會填入下拉式清單中，和下方，每個角色會顯示為核取方塊。 因為我們設定`AutoPostBack`DropDownList 和屬性的核取方塊設為 True，變更選取的使用者，或是檢查或取消勾選角色造成回傳。 不執行任何動作，不過，因為我們尚未撰寫程式碼來處理這些動作時。 我們將會處理這些工作，接下來兩節中。


[![此頁面會顯示使用者和角色](assigning-roles-to-users-cs/_static/image5.png)](assigning-roles-to-users-cs/_static/image4.png)

**圖 2**： 此頁面會顯示使用者和角色 ([按一下以檢視完整大小的影像](assigning-roles-to-users-cs/_static/image6.png))


### <a name="checking-the-roles-the-selected-user-belongs-to"></a>檢查角色選取的使用者屬於

當第一次載入頁面時，或造訪者從下拉式清單中選取新的使用者，我們需要更新`UsersRoleList`的核取方塊，以便只有當選取的使用者屬於該角色，選取指定的角色核取方塊。 若要達成此目的，建立名為方法`CheckRolesForSelectedUser`為下列程式碼：

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample7.cs)]

上述程式碼一開始會判斷選取的使用者。 然後它會使用角色類別的[`GetRolesForUser(userName)`方法](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx)傳回指定的使用者組的字串陣列形式的角色。 接下來，列舉 Repeater 的項目和每個項目`RoleCheckBox`核取方塊以程式設計方式所參考。 只有當它對應至該角色包含在已核取此核取方塊`selectedUsersRoles`字串陣列。

> [!NOTE]
> `selectedUserRoles.Contains<string>(...)`語法不會編譯，如果您使用 ASP.NET 2.0 版。 `Contains<string>`方法屬於[LINQ 文件庫](http://en.wikipedia.org/wiki/Language_Integrated_Query)，這是新 ASP.NET 3.5。 如果您仍在使用 ASP.NET 2.0 版中，使用[`Array.IndexOf<string>`方法](https://msdn.microsoft.com/library/eha9t187.aspx)改。


`CheckRolesForSelectedUser`需要在兩個情況下呼叫方法： 第一次載入頁面時，只要使用`UserList`DropDownList 的選取的索引變更。 因此，呼叫這個方法從`Page_Load`事件處理常式 (呼叫之後`BindUsersToUserList`和`BindRolesToList`)。 此外，建立事件處理常式的 DropDownList`SelectedIndexChanged`事件，並從該處呼叫這個方法。

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample8.cs)]

使用此程式碼就緒之後，您可以測試透過瀏覽器頁面。 不過，由於`UsersAndRoles.aspx`頁面目前缺少能夠將使用者指派給角色，沒有任何使用者角色。 我們將建立的介面，將使用者指派給角色很快，因此您可以採取我這段程式碼的運作方式的文字，並確認，它會更新版本中，或您可以手動將使用者加入角色插入記錄成`aspnet_UsersInRoles`資料表中，以測試此 functi現在 onality。

### <a name="assigning-and-removing-users-from-roles"></a>指派和從角色移除使用者

造訪者檢查，或取消核取方塊`UsersRoleList`我們需要新增或移除對應的角色中的所選的使用者的重複項。 核取方塊的`AutoPostBack`設定為 True，這會導致回傳，每當中繼器中的核取方塊已核取或取消核取。 簡單地說，我們需要建立事件處理常式 核取方塊的`CheckChanged`事件。 因為核取方塊是在中繼器控制項中，我們需要以手動方式加入事件處理常式配管。 將事件處理常式新增至程式碼後置類別，做為啟動`protected`方法，就像這樣：

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample9.cs)]

我們將會傳回這個事件處理常式撰寫程式碼，稍後。 但首先讓我們完成之事件處理常式配管。 從 Repeater 內的核取方塊`ItemTemplate`，新增`OnCheckedChanged="RoleCheckBox_CheckChanged"`。 此語法將`RoleCheckBox_CheckChanged`事件處理常式`RoleCheckBox`的`CheckedChanged`事件。

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample10.aspx)]

最後一項工作是完成`RoleCheckBox_CheckChanged`事件處理常式。 我們必須開始藉由參考引發事件，因為這個核取方塊執行個體告訴我們哪些角色已 checked 或 unchecked 透過 [checkbox] 控制項及其`Text`和`Checked`屬性。 使用此資訊連同使用者名稱的 選取的使用者，我們新增或移除使用者透過 azure 角色`Roles`類別的[ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx)或是[`RemoveUserFromRole`方法](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx)。

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample11.cs)]

上述程式碼一開始會以程式設計方式參考引發事件，其可透過核取方塊`sender`輸入的參數。 如果勾選此核取方塊，選取的使用者會加入指定的角色，否則為從角色中移除。 在任一情況下，`ActionStatus`標籤會顯示一則訊息摘述剛才執行的動作。

請花一點時間來測試此頁面，透過瀏覽器。 選取使用者 Tito，然後將 Tito 加入系統管理員 」 和 「 監督員 」 角色。


[![Tito 已新增至系統管理員和監督員的角色](assigning-roles-to-users-cs/_static/image8.png)](assigning-roles-to-users-cs/_static/image7.png)

**圖 3**： 系統管理員和監督員角色已新增 Tito ([按一下以檢視完整大小的影像](assigning-roles-to-users-cs/_static/image9.png))


接下來，從下拉式清單中選取使用者 Bruce。 沒有回傳和 Repeater 的核取方塊會透過更新`CheckRolesForSelectedUser`。 Bruce 不尚未屬於任何角色，因為兩個核取方塊未勾選。 接下來，Bruce 加入 「 主管 」 角色。


[![Bruce 已新增至 「 主管 」 角色](assigning-roles-to-users-cs/_static/image11.png)](assigning-roles-to-users-cs/_static/image10.png)

**圖 4**: Bruce 已新增至 「 主管 」 角色 ([按一下以檢視完整大小的影像](assigning-roles-to-users-cs/_static/image12.png))


若要進一步驗證功能`CheckRolesForSelectedUser`方法中，選取一個 Tito 或 Bruce 以外的使用者。 請注意情況下，核取方塊會自動取消核取的方式，用來表示，它們不屬於任何角色。 返回 Tito。 應檢查的系統管理員 」 和 「 監督員內的核取方塊。

## <a name="step-2-building-the-by-roles-user-interface"></a>步驟 2： 建立 「 依角色 」 的使用者介面

現在我們已完成 」 的使用者 」 介面，並已準備好開始處理 「 依角色 」 的介面。 「 依角色 」 介面會提示使用者從下拉式清單中選取角色，並顯示一組屬於 GridView 中該角色的使用者。

新增另一個 DropDownList 控制項來`UsersAndRoles.aspx`頁面。 放置這一個重複項控制項下方，將其命名`RoleList`，並設定其`AutoPostBack`屬性設為 True。 下方，新增 GridView 並將它命名`RolesUserList`。 此 GridView 會列出屬於所選角色的使用者。 設定 GridView`AutoGenerateColumns`屬性設定為 False，會加入方格中的 TemplateField`Columns`集合和集其`HeaderText`"Users"的屬性。 定義 TemplateField `ItemTemplate` ，以顯示資料繫結運算式的值`Container.DataItem`中`Text`屬性的名稱為標籤`UserNameLabel`。

加入及設定 GridView 之後, 您 「 依角色 」 的介面的宣告式標記看起來應該如下所示：

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample12.aspx)]

我們要填入`RoleList`DropDownList 與一組系統中的角色。 若要這麼做，更新`BindRolesToList`這是繫結的方法所傳回的字串陣列`Roles.GetAllRoles`方法來`RolesList`DropDownList (以及`UsersRoleList`重複項)。

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample13.cs)]

中的最後兩行`BindRolesToList`方法已加入至繫結之一組角色`RoleList`DropDownList 控制項。 圖 5 顯示檢視透過瀏覽器 – 下拉式清單，填入 系統角色時的最終結果。


[![角色會顯示在 RoleList DropDownList](assigning-roles-to-users-cs/_static/image14.png)](assigning-roles-to-users-cs/_static/image13.png)

**圖 5**： 會顯示角色`RoleList`DropDownList ([按一下以檢視完整大小的影像](assigning-roles-to-users-cs/_static/image15.png))


### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>顯示屬於所選角色的使用者

當第一次載入頁面，或從選取的新角色時`RoleList`下拉式清單中，我們需要顯示屬於該角色，在 gridview 裡的使用者清單。 建立一個名為方法`DisplayUsersBelongingToRole`使用下列程式碼：

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample14.cs)]

此方法會藉由取得從選取的角色啟動`RoleList`DropDownList。 然後它會使用[`Roles.GetUsersInRole(roleName)`方法](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx)擷取屬於該角色之使用者的使用者名稱的字串陣列。 這個陣列會接著繫結到`RolesUserList`GridView。

這個方法需要在兩個情況下呼叫： 一開始載入頁面時，以及當在選取的角色`RoleList`dropdownlist 進行變更。 因此，更新`Page_Load`事件處理常式，讓這個方法會叫用呼叫後面`CheckRolesForSelectedUser`。 接下來，建立的事件處理常式`RoleList`的`SelectedIndexChanged`事件，並從該處，呼叫這個方法時，也將太。

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample15.cs)]

此程式碼的位置， `RolesUserList` GridView 應該會顯示屬於所選角色的使用者。 如 [圖 6] 所示，監督員角色組成兩個成員： Bruce 和 Tito。


[![GridView 會列出這些屬於所選角色的使用者](assigning-roles-to-users-cs/_static/image17.png)](assigning-roles-to-users-cs/_static/image16.png)

**圖 6**: GridView 會列出這些使用者屬於選取的角色 ([按一下以檢視完整大小的影像](assigning-roles-to-users-cs/_static/image18.png))


### <a name="removing-users-from-the-selected-role"></a>從選取的角色移除使用者

讓我們擴大`RolesUserList`GridView 使其包含的資料行的 [移除] 按鈕。 按一下特定使用者的 [移除] 按鈕將它們從該角色中移除。

開始刪除按鈕欄位加入至 GridView。 讓這個欄位會顯示為已歸檔最左邊，並變更其`DeleteText`屬性從 [刪除] （預設值） 為 「 移除 」。


[![新增](assigning-roles-to-users-cs/_static/image20.png)](assigning-roles-to-users-cs/_static/image19.png)

**圖 7**： 加入 GridView 中的 [移除] 按鈕 ([按一下以檢視完整大小的影像](assigning-roles-to-users-cs/_static/image21.png))


按一下 [移除] 按鈕時回傳接踵而來和 GridView`RowDeleting`就會引發事件。 我們需要建立此事件的事件處理常式撰寫程式碼，從選取的角色移除使用者。 建立事件處理常式，然後加入下列程式碼：

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample16.cs)]

程式碼一開始會判斷選取的角色名稱。 然後以程式設計方式參考`UserNameLabel`從的 [移除] 按鈕已按下以判斷使用者的使用者名稱，若要移除的資料列的控制。 使用者從透過呼叫角色中移除`Roles.RemoveUserFromRole`方法。 `RolesUserList` GridView 然後重新整理，並透過顯示一則訊息`ActionStatus`標籤控制項。

> [!NOTE]
> [移除] 按鈕不需要任何類型的使用者從角色移除使用者之前，先確認。 歡迎您加入某種程度的使用者確認。 其中一個最簡單的方式，可確認動作是透過用戶端確認對話方塊。 如需有關這項技術的詳細資訊，請參閱 <<c0> [ 正在刪除時新增用戶端確認](https://asp.net/learn/data-access/tutorial-42-cs.aspx)。


從 主管 群組中移除使用者 Tito 後，圖 8 顯示頁面。


[![可惜的是，Tito 不再監督員](assigning-roles-to-users-cs/_static/image23.png)](assigning-roles-to-users-cs/_static/image22.png)

**圖 8**： 是很優 Tito 不再監督員 ([按一下以檢視完整大小的影像](assigning-roles-to-users-cs/_static/image24.png))


### <a name="adding-new-users-to-the-selected-role"></a>將新的使用者新增至選取的角色

從選取的角色移除使用者，以及此頁面的訪客也應該能夠將使用者新增至選取的角色。 將使用者新增至所選角色的最佳介面取決於您預期的使用者帳戶數目。 如果您的網站會存放在短短的數十個使用者帳戶或更小，您可以在這裡使用 dropdownlist 進行。 如果可能有數千個使用者帳戶，您會想要包括允許訪客能夠逐頁瀏覽的帳戶，搜尋特定的帳戶，或篩選中透過其他方式的使用者帳戶的使用者介面。

此頁面讓我們使用一個非常簡單的介面，不論在系統中的使用者帳戶數目。 也就是，我們將使用文字方塊中，提示輸入使用者的她想要新增至選取的角色名稱的訪客。 如果沒有具有該名稱的使用者存在，或如果使用者已經是角色的成員，我們會顯示在訊息`ActionStatus`標籤。 但是，如果使用者存在，且不是角色的成員，則我們會將它們新增至角色，並重新整理方格。

加入 TextBox 和 GridView 下方的按鈕。 設定文字方塊的`ID`要`UserNameToAddToRole`並將按鈕的`ID`並`Text`屬性，以`AddUserToRoleButton`和 「 新增使用者至角色 」，分別。

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample17.aspx)]

接下來，建立`Click`事件處理常式`AddUserToRoleButton`並加入下列程式碼：

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample18.cs)]

中的程式碼大多`Click`事件處理常式會執行各種不同的驗證檢查。 它可確保訪客提供中的使用者名稱`UserNameToAddToRole`文字方塊中，使用者存在於系統中，而且已不屬於所選的角色。 如果上述任何檢查失敗時，適當的訊息會顯示在`ActionStatus`和結束事件處理常式。 如果所有檢查都通過，要將使用者新增至透過角色`Roles.AddUserToRole`方法。 接下來，文字方塊中的`Text`屬性會被清除、 重新整理 GridView，而`ActionStatus`標籤會顯示訊息，指出指定的使用者已成功加入至選取的角色。

> [!NOTE]
> 若要確保指定的使用者已經不隸屬於所選的角色，我們使用[`Roles.IsUserInRole(userName, roleName)`方法](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx)，它會傳回布林值，指出是否*userName* 隸屬*roleName*。 我們將使用中的，這個方法一次<a id="_msoanchor_2"> </a>[下一個教學課程](role-based-authorization-cs.md)當我們查看以角色為基礎的授權。


瀏覽透過瀏覽器頁面，然後選取從 「 監督員角色`RoleList`DropDownList。 請嘗試輸入無效的使用者名稱，您應該會看到訊息，說明使用者不存在於系統。


[![您無法將不存在的使用者加入角色](assigning-roles-to-users-cs/_static/image26.png)](assigning-roles-to-users-cs/_static/image25.png)

**圖 9**： 您無法加入角色中的非存在的使用者 ([按一下以檢視完整大小的影像](assigning-roles-to-users-cs/_static/image27.png))


現在，嘗試新增有效的使用者。 請繼續並重新加入 Tito 監督員的角色。


[![Tito 同樣是監督員 ！](assigning-roles-to-users-cs/_static/image29.png)](assigning-roles-to-users-cs/_static/image28.png)

**圖 10**: Tito 同樣是監督員 ！  ([按一下以檢視完整大小的影像](assigning-roles-to-users-cs/_static/image30.png))


## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>步驟 3： 跨更新 「 使用者 」 和 「 角色 」 介面

`UsersAndRoles.aspx`頁面提供兩個不同的介面來管理使用者和角色。 目前，這兩個介面的作用彼此所以有可能，在一個介面中所做的變更將不會立即反映在其他。 例如，想像一下頁面的造訪者選取 「 主管 」 角色從`RoleList`下拉式清單中，它會列出 Bruce 和 Tito 做為其成員。 下一步，造訪者選取從 Tito`UserList`下拉式清單中，用來在檢查的系統管理員 」 和 「 監督員核取方塊`UsersRoleList`Repeater。 如果造訪者然後取消核取 Repeater 從 「 主管 」 角色，Tito 從角色中移除監督員內，但這項修改不會反映在 「 依角色 」 介面中。 GridView 仍會顯示 Tito 視為 「 主管 」 角色的成員。

若要修正此我們要重新整理 GridView，每當角色是 checked 或 unchecked 從`UsersRoleList`Repeater。 同樣地，我們需要重新整理 Repeater，每當使用者移除或新增至角色，從 「 依角色 」 的介面。

在 「 依使用者 」 介面 Repeater 會藉由呼叫重新整理`CheckRolesForSelectedUser`方法。 可修改的 「 依角色 」 介面`RolesUserList`GridView 的`RowDeleting`事件處理常式並`AddUserToRoleButton` 按鈕的`Click`事件處理常式。 因此，我們需要呼叫`CheckRolesForSelectedUser`從每一種方法的方法。

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample19.cs)]

同樣地，在 「 依角色 」 介面 GridView 會重新整理藉由呼叫`DisplayUsersBelongingToRole`方法，並在 「 依使用者 」 介面透過修改`RoleCheckBox_CheckChanged`事件處理常式。 因此，我們需要呼叫`DisplayUsersBelongingToRole`從這個事件處理常式的方法。

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample20.cs)]

這些次要的程式碼變更，「 使用者 」 和 「 角色 」 介面現在正確跨更新。 若要確認這點，請瀏覽透過瀏覽器頁面並選取 Tito 和監督員內，從`UserList`和`RoleList`dropdownlist 進行，分別。 請注意，當您針對從在 「 依使用者 」 介面 Repeater Tito 取消 「 主管 」 角色，Tito 會自動移除 「 依角色 」 介面中 GridView。 回到 「 主管 」 角色中將 Tito 加入從 「 依角色 」 的介面時，會自動重新檢查 「 依使用者 」 介面中的各有核取方塊。

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>步驟 4： 自訂以包含 「 指定的角色 」 的步驟 CreateUserWizard

在  <a id="_msoanchor_3"> </a> [*建立使用者帳戶*](../membership/creating-user-accounts-cs.md)教學課程中我們了解如何使用 CreateUserWizard Web 控制項來提供介面來建立新的使用者帳戶。 CreateUserWizard 控制項可以用於兩種方式之一：

- 做為的訪客在網站上，建立他們自己的使用者帳戶和
- 做為建立新帳戶的系統管理員

在第一個使用案例中，訪客會來到 站台，並填寫 CreateUserWizard，才能註冊網站上輸入他們的資訊。 在第二個案例中，系統管理員會建立新的帳戶，另一位人員。

當正在建立帳戶的一些其他人的系統管理員時，可能很有幫助以允許系統管理員指定哪些新的使用者帳戶所屬的角色。 在  <a id="_msoanchor_4"> </a> [*儲存**額外的使用者資訊*](../membership/storing-additional-user-information-cs.md)我們了解如何新增其他自訂 CreateUserWizard 的教學課程`WizardSteps`. 讓我們看看如何將額外的步驟新增至 CreateUserWizard，若要指定新的使用者角色。

開啟`CreateUserWizardWithRoles.aspx`頁面上，並新增名為的 CreateUserWizard 控制項`RegisterUserWithRoles`。 將控制項的`ContinueDestinationPageUrl`屬性，以"~ / Default.aspx"。 因為這意思是，系統管理員將會使用這個 CreateUserWizard 控制項來建立新的使用者帳戶，將控制項的`LoginCreatedUser`屬性設定為 False。 這`LoginCreatedUser`屬性指定是否訪客自動剛建立以使用者身分，登入，而且它預設為 True。 我們將它設定為 False 因為系統管理員建立新的帳戶時，我們希望能讓他自己的身分登入。

接下來，選取 「 新增/移除`WizardSteps`...」 選項從 CreateUserWizard 的智慧標籤，然後新增新`WizardStep`，將其`ID`至`SpecifyRolesStep`。 移動`SpecifyRolesStep WizardStep`以便說 「 登註冊您新帳戶 」 的步驟之後，但 「 完成 」 步驟之前。 設定`WizardStep`的`Title`屬性，以 「 指定角色 」，其`StepType`屬性設`Step`，並將其`AllowReturn`屬性設定為 False。


[![新增](assigning-roles-to-users-cs/_static/image32.png)](assigning-roles-to-users-cs/_static/image31.png)

**圖 11**： 新增 「 指定角色 」`WizardStep`以 CreateUserWizard ([按一下以檢視完整大小的影像](assigning-roles-to-users-cs/_static/image33.png))


這項變更之後 CreateUserWizard 的宣告式標記看起來應該如下所示：

[!code-aspx[Main](assigning-roles-to-users-cs/samples/sample21.aspx)]

在 [指定角色] `WizardStep`，新增名為 CheckBoxList `RoleList`。 此 CheckBoxList 會列出可用的角色，讓使用者瀏覽頁面檢查新建立的使用者屬於哪些角色。

但還是有兩個程式碼撰寫工作： 首先我們必須填入`RoleList`CheckBoxList 系統; 中的角色與第二，我們需要將所建立的使用者新增至選取的角色，當使用者從 「 指定的角色 」 步驟移至 「 完成 」 步驟。 我們可以完成的第一個工作`Page_Load`事件處理常式。 下列的程式碼以程式設計方式參考`RoleList`核取方塊，在第一個瀏覽至頁面，並將系統中的角色繫結至它。

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample22.cs)]

上述程式碼看起來應該很熟悉。 在  <a id="_msoanchor_5"> </a> [*儲存**額外的使用者資訊*](../membership/storing-additional-user-information-cs.md)教學課程中我們使用兩個`FindControl`陳述式來參考 Web 控制項從自訂內`WizardStep`。 並將角色繫結至 CheckBoxList 的程式碼取自稍早在本教學課程。

若要執行的第二個的程式設計工作，我們需要知道何時會完成的 「 指定的角色 」 步驟。 回想一下，CreateUserWizard 具有`ActiveStepChanged`引發的事件，每次造訪者一個步驟中瀏覽至另一個。 這裡我們可以判斷使用者是否已達到 「 完成 」 的步驟如果是這樣，我們需要將使用者新增至選取的角色。

建立事件處理常式`ActiveStepChanged`事件，並新增下列程式碼：

[!code-csharp[Main](assigning-roles-to-users-cs/samples/sample23.cs)]

如果使用者剛到達 「 已完成 」 步驟，事件處理常式會列舉的項目`RoleList`CheckBoxList 和剛建立的使用者指派給選取的角色。

請瀏覽此頁面，透過瀏覽器。 CreateUserWizard 的第一個步驟是標準的 [登註冊您新增帳戶] 步驟，也會提示您輸入新使用者的使用者名稱、 密碼、 電子郵件和其他重要資訊。 輸入要建立名為 Wanda 的新使用者的資訊。


[![建立名為 Wanda 新使用者](assigning-roles-to-users-cs/_static/image35.png)](assigning-roles-to-users-cs/_static/image34.png)

**圖 12**： 建立新的使用者名稱為 Wanda ([按一下以檢視完整大小的影像](assigning-roles-to-users-cs/_static/image36.png))


按一下 [建立使用者] 按鈕。 在內部呼叫 CreateUserWizard`Membership.CreateUser`方法，建立新的使用者帳戶，並接著會前進到下一個步驟中，「 指定的角色。 」 這裡會列出系統角色。 檢查各有核取方塊，然後按一下 [下一步]。


[![請 Wanda 監督員角色的成員](assigning-roles-to-users-cs/_static/image38.png)](assigning-roles-to-users-cs/_static/image37.png)

**圖 13**： 請 Wanda 監督員角色的成員 ([按一下以檢視完整大小的影像](assigning-roles-to-users-cs/_static/image39.png))


按一下 下一步，回傳和更新會導致`ActiveStep`「 完成 」 步驟。 在 `ActiveStepChanged`事件處理常式，最近建立的使用者帳戶指派給 「 監督員 」 角色。 若要確認這點，傳回到`UsersAndRoles.aspx`頁面上，選取監督員內，從`RoleList`DropDownList。 如 [圖 14] 所示，監督員內現在組成三個使用者： Bruce、 Tito 和 Wanda。


[![Bruce、 Tito 和 Wanda 是所有的監督員](assigning-roles-to-users-cs/_static/image41.png)](assigning-roles-to-users-cs/_static/image40.png)

**圖 14**: Bruce、 Tito 和 Wanda 是所有的監督員 ([按一下以檢視完整大小的影像](assigning-roles-to-users-cs/_static/image42.png))


## <a name="summary"></a>總結

角色架構會提供方法來擷取特定使用者的角色和方法來判斷哪些使用者屬於指定角色的相關資訊。 此外，有多種方法來新增和移除一或多個使用者新增至一或多個角色。 在本教學課程中我們著重於兩個只是一種方法：`AddUserToRole`和`RemoveUserFromRole`。 有設計用來將多個使用者新增至單一角色，並將多個角色指派給單一使用者的其他變化。

本教學課程也包含了解擴充 CreateUserWizard 控制項，以包含`WizardStep`來指定新建立之使用者的角色。 這個步驟有助於簡化程序建立新使用者的使用者帳戶的系統管理員。

此時，我們看到如何建立及刪除角色，以及如何新增和移除角色的使用者。 但是，我們還沒有看看套用以角色為基礎的授權。 在  <a id="_msoanchor_6"> </a>[下一個教學課程](role-based-authorization-cs.md)我們稍後會定義角色的角色為基礎的 URL 授權規則以及如何將頁面層級功能限制根據目前登入使用者的角色。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ASP.NET 網站管理工具概觀](https://msdn.microsoft.com/library/ms228053.aspx)
- [正在檢查 ASP。NET 的成員資格、 角色和設定檔](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [復原您自己的網站管理工具](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP 書籍的作者，他是 4GuysFromRolla.com 的創辦人，從事 Microsoft Web 技術工作自 1998 年。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是 *[Sams 教導您自己 ASP.NET 2.0 在 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 Scott 要聯絡[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝...

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Teresa murphy 徹底檢驗了。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在 [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](creating-and-managing-roles-cs.md)
> [下一頁](role-based-authorization-cs.md)
