---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
title: 儲存額外的使用者資訊 (VB) |Microsoft Docs
author: rick-anderson
description: 在本教學課程，我們將建置非常基本的訪客留言板應用程式回答這個問題。 在此情況下，我們將探討 modeli 的不同選項...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: ee4b924e-8002-4dc3-819f-695fca1ff867
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
msc.type: authoredcontent
ms.openlocfilehash: 06e28653b281461eff6548de6951f8463c827754
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381861"
---
<a name="storing-additional-user-information-vb"></a>儲存額外的使用者資訊 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_VB.zip)或[下載 PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_vb.pdf)

> 在本教學課程，我們將建置非常基本的訪客留言板應用程式回答這個問題。 在此情況下，我們將看看不同的使用者資訊，在資料庫中，模型化選項，然後了解如何建立此資料與成員資格架構所建立的使用者帳戶關聯。


## <a name="introduction"></a>簡介

ASP。NET 的成員資格架構會提供彈性的介面來管理使用者。 成員資格 API 包含用於驗證認證、 擷取目前登入使用者的相關資訊、 建立新的使用者帳戶，和刪除使用者帳戶，以及其他的方法。 在 成員資格架構中的每個使用者帳戶包含只針對所需的驗證認證，並執行基本的使用者帳戶相關工作的屬性。 這證明方法和屬性[`MembershipUser`類別](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx)，模型中的成員資格架構的使用者帳戶。 這個類別具有屬性，例如[ `UserName` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.username.aspx)， [ `Email` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.email.aspx)，以及[ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx)，且可以使用[ `GetPassword` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.getpassword.aspx)並[ `UnlockUser` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx)。

有時候，應用程式需要儲存不包含在成員資格架構的其他使用者資訊。 例如，線上零售商可能需要讓每位使用者儲存其出貨和帳單地址、 付款資訊、 傳遞喜好設定，並連絡電話號碼。 此外，在系統中的每筆訂單為特定使用者帳戶相關聯。

`MembershipUser`類別不包含屬性，例如`PhoneNumber`或是`DeliveryPreferences`或`PastOrders`。 我們該如何追蹤應用程式所需的使用者資訊並將它與成員資格架構整合？ 在本教學課程，我們將建置非常基本的訪客留言板應用程式回答這個問題。 在此情況下，我們將看看不同的使用者資訊，在資料庫中，模型化選項，然後了解如何建立此資料與成員資格架構所建立的使用者帳戶關聯。 讓我們開始吧 ！

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>步驟 1： 建立訪客留言板應用程式的資料模型

有各種技術，可用來擷取資料庫中的使用者資訊，並將它與成員資格架構所建立的使用者帳戶產生關聯。 為了說明這些技術，我們需要來增強教學課程的 web 應用程式，因此，它會擷取某一種使用者相關資料。 (目前，應用程式服務所需的資料表只會包含應用程式的資料模型`SqlMembershipProvider`。)

讓我們建立已驗證的使用者可以留下註解，其中一個非常簡單的訪客留言板應用程式。 除了儲存訪客留言板註解，我們可允許每位使用者將他的主要城鎮、 首頁和簽章。 如果提供，使用者的家用 town 首頁和簽章將會出現在他已離開訪客留言板中的每個訊息。

### <a name="adding-theguestbookcommentstable"></a>新增`GuestbookComments`資料表

若要擷取的訪客留言板註解，我們需要建立一個名為的資料庫資料表`GuestbookComments`具有資料行，例如`CommentId`， `Subject`， `Body`，和`CommentDate`。 我們也需要在每一筆記錄`GuestbookComments`資料表參考使用者留下留言。

將此資料表加入我們的資料庫，請移至 Visual Studio 中的 [資料庫總管] 並向下切入至`SecurityTutorials`資料庫。 以滑鼠右鍵按一下 資料表 資料夾，然後選擇 加入新的資料表。 這會顯示介面，讓我們定義新的資料表的資料行。


[![將新的資料表新增至 SecurityTutorials 資料庫](storing-additional-user-information-vb/_static/image2.png)](storing-additional-user-information-vb/_static/image1.png)

**圖 1**： 加入新的資料表，以便`SecurityTutorials`資料庫 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image3.png))


接下來，定義`GuestbookComments`的資料行。 新增名為的資料行開始`CommentId`型別的`uniqueidentifier`。 此資料行中會唯一識別訪客留言板中的每個註解，因此不允許`NULL`s，並將它標示為資料表的主索引鍵。 而不是提供的值`CommentId`每個欄位`INSERT`，我們可能表示新`uniqueidentifier`應該會自動產生值為這個欄位上`INSERT`資料行的預設值設定為`NEWID()`。 之後新增此第一個欄位，使它成為主索引鍵，並設定其預設值，您的畫面看起來應該類似螢幕擷取畫面的 圖 2 所示。


[![新增名為 CommentId 主要資料行](storing-additional-user-information-vb/_static/image5.png)](storing-additional-user-information-vb/_static/image4.png)

**圖 2**： 新增主要的資料行名為`CommentId`([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image6.png))


接下來，新增名為的資料行`Subject`型別的`nvarchar(50)`和名為資料行`Body`型別的`nvarchar(MAX)`，不允許`NULL`中兩個資料行。 接下來，新增名為的資料行`CommentDate`型別的`datetime`。 不允許`NULL`s 和 set`CommentDate`資料行的預設值`getdate()`。

所有剩下就是加入與每個訪客留言板註解產生關聯的使用者帳戶的資料行。 其中一個選項是新增名為的資料行`UserName`型別的`nvarchar(256)`。 這是適合的選擇，而不使用成員資格提供者時`SqlMembershipProvider`。 使用時，但`SqlMembershipProvider`，我們會在本教學課程系列中，如`UserName`中的資料行`aspnet_Users`資料表不保證是唯一的。 `aspnet_Users`資料表的主索引鍵是`UserId`型別，且`uniqueidentifier`。 因此，`GuestbookComments`的資料表需要名為的資料行`UserId`型別的`uniqueidentifier`(不允許`NULL`值)。 請繼續並加入此資料行。

> [!NOTE]
> 如我們所述[*在 SQL Server 中建立成員資格結構描述*](creating-the-membership-schema-in-sql-server-vb.md)教學課程中，成員資格架構可讓多個 web 應用程式使用不同的使用者帳戶會共用相同使用者存放區。 它會藉由分割到不同的應用程式的使用者帳戶。 雖然每個使用者名稱保證是唯一的應用程式內，可能使用相同的使用者存放區的不同應用程式中使用相同的使用者名稱。 沒有複合`UNIQUE`中的條件約束`aspnet_Users`資料表上`UserName`並`ApplicationId`欄位，但不是其中上只`UserName`欄位。 因此，就可能 aspnet\_有兩個 （含） 以上的記錄，具有相同的使用者資料表`UserName`值。 不過，`UNIQUE`條件約束`aspnet_Users`資料表的`UserId`欄位 （因為它是主索引鍵）。 A`UNIQUE`條件約束很重要，因為沒有它，我們無法建立之間的外部索引鍵條件約束`GuestbookComments`和`aspnet_Users`資料表。


在新增之後`UserId`資料行，儲存的資料表，按一下工具列中的 [儲存] 圖示。 新資料表命名`GuestbookComments`。

我們有最後的一個問題，以處理`GuestbookComments`資料表： 我們需要建立[foreign key 條件約束](https://msdn.microsoft.com/library/ms175464.aspx)之間`GuestbookComments.UserId`資料行和`aspnet_Users.UserId`資料行。 若要這麼做，請按一下工具列啟動外部索引鍵關聯性 對話方塊中的關聯性 圖示。 （或者，您可以啟動此對話方塊中移至 資料表設計工具 功能表並選擇 關聯性。）

按一下 [外部索引鍵關聯性] 對話方塊左下角的 [新增] 按鈕。 這會新增 新外部索引鍵條件約束，雖然我們仍需要在關係性中定義參與的資料表。


[![使用外部索引鍵關聯性 對話方塊來管理資料表的 Foreign Key 條件約束](storing-additional-user-information-vb/_static/image8.png)](storing-additional-user-information-vb/_static/image7.png)

**圖 3**： 若要管理資料表的 Foreign Key 條件約束中使用外部索引鍵關聯性對話方塊 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image9.png))


接下來，按一下 「 資料表和資料行規格 資料列右側的省略符號圖示。 這會啟動 資料表和資料行對話方塊中，我們可以從中指定主索引鍵的資料表和資料行的外部索引鍵資料行從`GuestbookComments`資料表。 特別的是，選取`aspnet_Users`並`UserId`作為主索引鍵資料表和資料行，並`UserId`從`GuestbookComments`做外部索引鍵資料行的資料表 （請參閱 圖 4）。 定義之後的主要與外部索引鍵的資料表和資料行，按一下 [確定] 以返回 [外部索引鍵關聯性] 對話方塊中。


[![建立外部索引鍵條件約束之間 aspnet_Users 和 GuesbookComments 資料表](storing-additional-user-information-vb/_static/image11.png)](storing-additional-user-information-vb/_static/image10.png)

**圖 4**： 建立外部索引鍵條件約束之間`aspnet_Users`並`GuesbookComments`資料表 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image12.png))


此時已建立的外部索引鍵條件約束。 這個條件約束可確保[關聯式完整性](http://en.wikipedia.org/wiki/Referential_integrity)之間兩個資料表，藉此防衛，絕對不會參考不存在的使用者帳戶的訪客留言板項目。 根據預設的外部索引鍵條件約束將不允許那里對應的子記錄，如果要刪除的父記錄。 也就是如果使用者提出的一或多個訪客留言板註解，然後我們嘗試刪除該使用者帳戶，則刪除會失敗，除非先刪除他訪客留言板註解。

Foreign key 條件約束可以設定為父記錄已刪除時，自動刪除相關聯的子記錄中。 換句話說，我們可以設定這個外部索引鍵條件約束，以便當她的使用者帳戶被刪除時，系統會自動刪除使用者的訪客留言板項目。 若要這麼做，展開 「 INSERT 和 UPDATE 規格 」 一節，設定 刪除規則 屬性為 Cascade。


[![設定串聯刪除外部索引鍵條件約束](storing-additional-user-information-vb/_static/image14.png)](storing-additional-user-information-vb/_static/image13.png)

**圖 5**： 設定串聯刪除外部索引鍵條件約束 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image15.png))


若要儲存的外部索引鍵條件約束，請按一下 關閉 按鈕結束 外部索引鍵關聯性。 然後按一下 [儲存] 圖示，在工具列中，以儲存資料表，然後此關聯性。

### <a name="storing-the-users-home-town-homepage-and-signature"></a>儲存使用者的首頁城鎮、 首頁，以及簽章

`GuestbookComments`表將說明如何儲存使用者帳戶與共用的一對多關聯性的資訊。 由於每個使用者帳戶可以有任意數目的相關聯的註解，此關聯性會建立模型所建立的資料表來保存註解的集合，包括連結回特定使用者的每個註解的資料行。 使用時`SqlMembershipProvider`，建立名為的資料行最適合建立此連結`UserId`型別的`uniqueidentifier`以及此資料行之間的外部索引鍵條件約束和`aspnet_Users.UserId`。

我們現在需要將每個使用者帳戶來儲存使用者的主要城鎮、 首頁和簽章，會出現在他的訪客留言板註解的三個資料行。 有數種不同的方式，來完成這項作業：

- <strong>將新的資料行加入</strong><strong>`aspnet_Users`</strong><strong>或是</strong><strong>`aspnet_Membership`</strong><strong>資料表。</strong> 我不會建議這種方法，因為它會修改所使用的結構描述`SqlMembershipProvider`。 這項決策可能返回您說話今後。 例如，如果未來的 ASP.NET 版本會使用不同`SqlMembershipProvider`結構描述。 Microsoft 可能包含工具，以便移轉在 ASP.NET 2.0`SqlMembershipProvider`新的結構描述，但如果您已修改 ASP.NET 2.0 資料`SqlMembershipProvider`結構描述中，這類轉換可能不適用。

- **使用 ASP。NET 的設定檔架構，定義主要城鎮、 首頁和簽章的設定檔屬性。** ASP.NET 包含的設定檔架構是設計用來儲存額外的使用者特定資料。 成員資格架構，例如設定檔架構是建置在提供者模型之上。 .NET Framework 隨附`SqlProfileProvider`存放區分析 SQL Server 資料庫中的資料。 事實上，我們的資料庫已經有所使用的資料表`SqlProfileProvider`(`aspnet_Profile`)，當我們新增回應用程式服務已加入[ *SQL Server 中建立成員資格結構描述*](creating-the-membership-schema-in-sql-server-vb.md)教學課程。   
  設定檔架構的主要優點是它可讓開發人員設定檔中定義屬性`Web.config`– 程式碼不需要寫入序列化設定檔資料與基礎資料存放區。 簡單地說，它也非常簡單，來定義一組設定檔屬性，並在程式碼中使用它們。 不過，設定檔的系統會保留令人滿意談到版本控制，因此如果您的應用程式，其中您預期新的使用者特定屬性，以在稍後的時間或移除或修改現有的新增，則可能不到設定檔架構 最佳選項。 此外，`SqlProfileProvider`高度反正規化的方式，使其下一步 無法直接對設定檔資料 （例如，New York 家用 town 有多少使用者） 執行查詢儲存的設定檔屬性。   
  如需有關設定檔架構的詳細資訊，請參閱在本教學課程結尾處的 「 進一步的數據 」 一節。

- <strong>將下列三個資料行加入至資料庫中的新資料表，並建立此資料表之間的一對一關聯性並</strong><strong>`aspnet_Users`</strong><strong>。</strong> 這種方法牽涉到比更多的工作設定檔架構中，但提供了最大的彈性，在資料庫中的其他使用者屬性會建立的模型。 這是我們將在本教學課程使用的選項。

我們將建立新的資料表，稱為`UserProfiles`儲存主要城鎮、 首頁，並為每個使用者的簽章。 在 [資料庫總管] 視窗中的 [資料表] 資料夾上按一下滑鼠右鍵，然後選擇建立新的資料表。 第一個資料行命名`UserId`並將其類型設定為`uniqueidentifier`。 不允許`NULL`值，並標示為主索引鍵資料行。 接下來，新增名為的資料行：`HomeTown`型別的`nvarchar(50)`;`HomepageUrl`型別的`nvarchar(100)`; 和簽章的型別`nvarchar(500)`。 每個這些三個資料行可以接受`NULL`值。


[![建立 UserProfiles 資料表](storing-additional-user-information-vb/_static/image17.png)](storing-additional-user-information-vb/_static/image16.png)

**圖 6**： 建立`UserProfiles`資料表 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image18.png))


儲存資料表並將它命名`UserProfiles`。 最後，建立之間的外部索引鍵條件約束`UserProfiles`資料表的`UserId`欄位和`aspnet_Users.UserId`欄位。 當我們使用之間的外部索引鍵條件約束`GuestbookComments`和`aspnet_Users`資料表，具有串聯刪除這個條件約束。 由於`UserId`欄位中`UserProfiles`是主要索引鍵，這可確保會有一個以上的記錄中`UserProfiles`資料表每個使用者帳戶。 這種類型的關聯性被指為一對一。

既然我們已經建立的資料模型，我們已準備好使用它。 步驟 2 和 3 中我們將探討目前登入的使用者可以檢視和編輯其首頁的城鎮、 首頁和簽章資訊的方式。 在步驟 4 中，我們將建立來送出至訪客留言板的新註解，以及檢視現有的已驗證的使用者介面。

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>步驟 2： 顯示使用者的首頁城鎮、 首頁和簽章

有各種不同的方式讓目前登入的使用者，來檢視和編輯其首頁的城鎮、 首頁和簽章資訊。 我們可以使用文字方塊中手動建立使用者介面和 Label 控制項，或者我們可以使用其中一種 Web 控制項，例如 DetailsView 控制項的資料。 若要執行的資料庫`SELECT`和`UPDATE`我們可以撰寫 ADO.NET 的陳述式會在我們的網頁程式碼後置類別中的程式碼，或或者，使用以 sqldatasource 進行的宣告式方法。 在理想情況下我們的應用程式會包含的階層式的架構，我們無法叫用程式設計方式從網頁的程式碼後置類別，或以宣告方式透過 ObjectDataSource 控制項。

因為本教學課程系列，著重於表單驗證、 授權、 使用者帳戶和角色，就不會討論這些不同的資料存取選項或階層式的架構為何偏好透過直接執行 SQL 陳述式從 [ASP.NET] 頁面中。 我要逐步解說使用 DetailsView 和 SqlDataSource – 最快速而且簡單選項 – 但所討論的概念絕對可以套用至替代 Web 控制項和資料存取邏輯。 如需有關使用 ASP.NET 中的資料的詳細資訊，請參閱我*[使用 ASP.NET 2.0 中的資料](../../data-access/index.md)* 教學課程系列。

開啟`AdditionalUserInfo.aspx`頁面中`Membership`資料夾和 DetailsView 控制項加入頁面上，將其 ID 屬性設定為`UserProfile`並清除其`Width`和`Height`屬性。 展開 DetailsView 的智慧標籤，然後選擇 繫結至新的資料來源控制項。 這會啟動 資料來源組態精靈 （請參閱 圖 7）。 第一個步驟會要求您指定的資料來源類型。 因為我們將直接連接到`SecurityTutorials`資料庫，請選擇 [資料庫] 圖示，指定`ID`做為`UserProfileDataSource`。


[![新增名為 UserProfileDataSource SqlDataSource 控制項](storing-additional-user-information-vb/_static/image20.png)](storing-additional-user-information-vb/_static/image19.png)

**圖 7**： 加入新的 SqlDataSource 控制項命名`UserProfileDataSource`([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image21.png))


下一個畫面會提示輸入要使用的資料庫。 我們已經定義中的連接字串`Web.config`針對`SecurityTutorials`資料庫。 這個的連接字串名稱 – `SecurityTutorialsConnectionString` – 應該是下拉式清單中。 選取此選項，然後按一下 [下一步]。


[![從下拉式清單中選擇 SecurityTutorialsConnectionString](storing-additional-user-information-vb/_static/image23.png)](storing-additional-user-information-vb/_static/image22.png)

**圖 8**： 選擇`SecurityTutorialsConnectionString`從下拉式清單 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image24.png))


後續的畫面會要求我們指定的資料表和查詢的資料行。 選擇`UserProfiles`資料表下拉式清單中，並檢查所有的資料行。


[![將從 UserProfiles 資料表傳回所有資料行](storing-additional-user-information-vb/_static/image26.png)](storing-additional-user-information-vb/_static/image25.png)

**圖 9**： 將傳回所有的資料行`UserProfiles`資料表 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image27.png))


圖 9 傳回在目前的查詢*所有*中的記錄`UserProfiles`，但是我們只想要在目前登入的使用者記錄中。 若要新增`WHERE`子句中，按一下`WHERE`按鈕，即可開啟 新增`WHERE`子句對話方塊 （請參閱 圖 10）。 這裡您可以選取要篩選的資料行、 運算子和篩選參數的來源。 選取`UserId`做為資料行和做為運算子 「 = 」。

不幸的是沒有內建的參數來傳回目前登入使用者的來源`UserId`值。 我們必須以程式設計方式取得此值。 因此，設定為 無， 按一下 新增按鈕以新增參數，然後按一下 確定 的 來源 下拉式清單。


[![UserId 資料行上加入 Filter 參數](storing-additional-user-information-vb/_static/image29.png)](storing-additional-user-information-vb/_static/image28.png)

**圖 10**： 上加入 Filter 參數`UserId`資料行 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image30.png))


按一下 確定 之後會回到您 圖 9 所示的畫面。 此時，不過，在畫面底部的 SQL 查詢應包含`WHERE`子句。 若要移至 [測試查詢] 畫面，請按下一步。 您可以在這裡執行查詢，並查看結果。 按一下 完成 以完成精靈。

完成 [資料來源組態精靈]，Visual Studio 會建立 SqlDataSource 控制項取決於精靈中指定的設定。 此外，它以手動方式加入 BoundFields 每個資料行傳回 SqlDataSource DetailsView `SelectCommand`。 若要顯示不需要`UserId`欄位在 DetailsView，因為使用者不需要知道此值。 您可以直接從 DetailsView 控制項的宣告式標記中移除此欄位，或從它的智慧標籤按一下 [編輯欄位] 連結。

此時您頁面的宣告式標記看起來應該如下所示：

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample1.aspx)]

我們需要以程式設計方式設定 SqlDataSource 控制項`UserId`參數，以目前登入使用者的`UserId`之前已選取的資料。 這可藉由建立事件處理常式，如 SqlDataSource`Selecting`事件，並新增下列程式碼那里：

[!code-vb[Main](storing-additional-user-information-vb/samples/sample2.vb)]

上述程式碼一開始會取得目前登入使用者的參考，藉由呼叫`Membership`類別的`GetUser`方法。 這會傳回`MembershipUser`物件，其`ProviderUserKey`屬性包含`UserId`。 `UserId`值接著會指派給 SqlDataSource`@UserId`參數。

> [!NOTE]
> `Membership.GetUser()`方法會傳回目前登入使用者的相關資訊。 如果匿名使用者造訪的頁面，則會傳回值為`Nothing`。 在此情況下，這會導致`NullReferenceException`的程式碼時嘗試讀取下一行`ProviderUserKey`屬性。 當然，我們不需要擔心`Membership.GetUser()`傳回中的 Nothing`AdditionalUserInfo.aspx`頁面上，因為我們已設定 URL 授權先前的教學課程中，讓只有經過驗證的使用者無法存取此資料夾中的 ASP.NET 資源。 如果您需要存取允許匿名存取的其中一個頁面中目前登入使用者的相關資訊，請務必確認`MembershipUser`所傳回的物件`GetUser()`方法不執行任何動作之前先參考其屬性。


如果您瀏覽`AdditionalUserInfo.aspx`頁面上透過瀏覽器則會看見空白頁因為我們尚未將任何資料列加入`UserProfiles`資料表。 步驟 6 中我們將探討如何自訂 CreateUserWizard 控制項可自動將新的資料列`UserProfiles`資料表時建立新的使用者帳戶。 不過，現在，我們將需要以手動方式在資料表中建立一筆記錄。

瀏覽至 Visual Studio 中的 資料庫總管，然後展開 資料表 資料夾。 以滑鼠右鍵按一下`aspnet_Users`資料表並選擇 顯示資料表資料 」 以查看資料表中的記錄; 執行相同的動作，`UserProfiles`資料表。 [圖 11] 顯示當垂直並排這些結果。 在 我的資料庫中目前沒有`aspnet_Users`Bruce、 Fred 和 Tito，記錄，但在沒有記錄`UserProfiles`資料表。


[![Aspnet_Users 的內容和 UserProfiles 資料表會顯示](storing-additional-user-information-vb/_static/image32.png)](storing-additional-user-information-vb/_static/image31.png)

**圖 11**: 內容`aspnet_Users`並`UserProfiles`資料表會顯示出來 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image33.png))


新增新的記錄，以`UserProfiles`手動輸入值的表格`HomeTown`， `HomepageUrl`，和`Signature`欄位。 若要取得有效的最簡單方式`UserId`在新的值`UserProfiles`記錄是選取`UserId`欄位中的特定使用者帳戶從`aspnet_Users`資料表，複製並貼到`UserId`欄位`UserProfiles`。 [圖 12] 顯示`UserProfiles`資料表之後 Bruce 已新增新的記錄。


[![記錄已加入 UserProfiles Bruce](storing-additional-user-information-vb/_static/image35.png)](storing-additional-user-information-vb/_static/image34.png)

**圖 12**: 記錄已新增至`UserProfiles`Bruce 為 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image36.png))


返回`AdditionalUserInfo.aspx page`、 登入身分 Bruce。 如 [圖 13] 所示，Bruce 的設定會顯示。


[![目前瀏覽使用者會顯示服務。 他的設定](storing-additional-user-information-vb/_static/image38.png)](storing-additional-user-information-vb/_static/image37.png)

**圖 13**: 目前瀏覽使用者會顯示服務。 他的設定 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image39.png))


> [!NOTE]
> 繼續進行，並以手動方式新增記錄`UserProfiles`資料表針對每個成員資格使用者。 步驟 6 中我們將探討如何自訂 CreateUserWizard 控制項可自動將新的資料列`UserProfiles`資料表時建立新的使用者帳戶。


## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>步驟 3： 可讓使用者編輯他家城鎮、 首頁和簽章

此時目前登入的使用者可以檢視其主要城鎮、 首頁和簽章設定，但他們還不能修改它們。 讓我們更新 DetailsView 控制項，讓資料可供編輯。

我們要做的第一件事是新增`UpdateCommand`SqlDataSource，用於指定`UPDATE`要執行陳述式和其對應的參數。 選取 SqlDataSource，然後從 屬性 視窗中，按一下以顯示 命令及參數編輯器對話方塊的 UpdateQuery 屬性旁邊的省略符號。 輸入下列`UPDATE`文字方塊的陳述式：

[!code-sql[Main](storing-additional-user-information-vb/samples/sample3.sql)]

接下來，按一下 [重新整理參數] 按鈕，這將會在 SqlDataSource 控制項的建立參數`UpdateParameters`集合中的參數的每個`UPDATE`陳述式。 保留所有的參數集來源為 None，然後按一下 [確定] 按鈕完成對話方塊。


[![指定 SqlDataSource UpdateCommand 和 UpdateParameters](storing-additional-user-information-vb/_static/image41.png)](storing-additional-user-information-vb/_static/image40.png)

**圖 14**： 指定 SqlDataSource`UpdateCommand`並`UpdateParameters`([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image42.png))


新增的項目因為我們對 SqlDataSource 控制項 DetailsView 控制項現在支援編輯。 從 DetailsView 的智慧標籤，檢查 「 啟用編輯 」 的核取方塊。 這會將控制項加入的 CommandField`Fields`集合其`ShowEditButton`屬性設為 True。 DetailsView 會顯示在唯讀模式和更新，和 [取消] 按鈕時顯示在編輯模式時，這會使得 [編輯] 按鈕。 而不是要求使用者按一下 [編輯]，不過，我們可以在 DetailsView 轉譯 」 一律可編輯 」 狀態設定 DetailsView 控制項的[`DefaultMode`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx)至`Edit`。

經過這些變更，您的 DetailsView 控制項宣告式標記看起來應該如下所示：

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample4.aspx)]

請注意 CommandField 新增和`DefaultMode`屬性。

請繼續進行，並測試透過瀏覽器的此頁面。 使用具有對應的記錄中的使用者瀏覽時`UserProfiles`，使用者的設定會顯示在可編輯的介面。


[![DetailsView 呈現可編輯的介面](storing-additional-user-information-vb/_static/image44.png)](storing-additional-user-information-vb/_static/image43.png)

**圖 15**: DetailsView 呈現可編輯介面 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image45.png))


請嘗試變更值，然後按一下 [更新] 按鈕。 似乎沒有任何反應。 沒有回傳和值會儲存到資料庫，但沒有任何儲存發生的視覺化回饋。

若要解決此問題，返回 Visual Studio 並加入上述 DetailsView 的 Label 控制項。 設定其`ID`來`SettingsUpdatedMessage`、 其`Text`「 您的設定已更新，"的屬性並將其`Visible`並`EnableViewState`屬性，以`False`。

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample5.aspx)]

我們要顯示`SettingsUpdatedMessage`DetailsView 更新時加上標籤。 若要達成此目的，建立事件處理常式 DetailsView`ItemUpdated`事件，並新增下列程式碼：

[!code-vb[Main](storing-additional-user-information-vb/samples/sample6.vb)]

返回`AdditionalUserInfo.aspx`頁面上透過瀏覽器，並更新的資料。 此時，很有幫助的狀態訊息會顯示。


[![簡短訊息會顯示當更新設定](storing-additional-user-information-vb/_static/image47.png)](storing-additional-user-information-vb/_static/image46.png)

**圖 16**： 已更新的設定時，系統會顯示簡短訊息 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image48.png))


> [!NOTE]
> 在 DetailsView 控制項的編輯介面分葉令人滿意。 它會使用標準大小的文字方塊中，但是簽章欄位應該可能是多行文字方塊中。 RegularExpressionValidator 應該用來確保，首頁 URL，如果輸入，開頭為"http://"或"https://"中。 此外，自 DetailsView 控制項具有其`DefaultMode`屬性設定為`Edit`，[取消] 按鈕不會執行任何動作。 它應該是要移除或，按一下時，將使用者重新導向至其他網頁 (例如`~/Default.aspx`)。 我可以作為練習，保留這些增強功能來讀取器。


### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>新增連結`AdditionalUserInfo.aspx`主版頁面的頁面

目前，網站不提供任何連結`AdditionalUserInfo.aspx`頁面。 連線到它的唯一方法是瀏覽器的網址列中直接輸入網頁的 URL。 讓我們將在此頁面的連結`Site.master`主版頁面。

還記得主版頁面包含 LoginView Web 控制項在其`LoginContent`ContentPlaceHolder 顯示不同的標記為已驗證和匿名訪客。 更新 LoginView 控制項`LoggedInTemplate`可包含連結`AdditionalUserInfo.aspx`頁面。 之後進行這些變更 LoginView 控制項的宣告式標記看起來應該如下所示：

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample7.aspx)]

請注意新增`lnkUpdateSettings`超連結控制項以`LoggedInTemplate`。 與就地這個連結，已驗證的使用者可以快速跳至的頁面來檢視和修改其主的城鎮、 首頁和簽章設定。

## <a name="step-4-adding-new-guestbook-comments"></a>步驟 4： 加入新的訪客留言板註解

`Guestbook.aspx`頁面是 已驗證的使用者可檢視訪客留言板並留下註解。 讓我們開始建立要加入新的訪客留言板註解的介面。

開啟`Guestbook.aspx`Visual Studio 中的頁面上，並建構使用者介面，其中包含兩個 TextBox 控制項，一個用於新的註解主體，一個用於其主體。 將第一個文字方塊控制項的`ID`屬性，以`Subject`及其`Columns`為 40 的屬性，設定第二個`ID`來`Body`、 其`TextMode`至`MultiLine`，並將其`Width`和`Rows`屬性"95%"和 8，分別。 若要完成的使用者介面，將名為 Button Web 控制項加入`PostCommentButton`並設定其`Text`「 張貼您的註解 」 的屬性。

由於每個訪客留言板註解時需要主旨和本文，請針對每個文字方塊加入 RequiredFieldValidator。 設定`ValidationGroup`屬性的這些控制項，以 「 EnterComment"; 同樣地，設定`PostCommentButton`控制項的`ValidationGroup`"EnterComment"的屬性。 如需有關 ASP 的詳細資訊。NET 的驗證控制項，請參閱[在 ASP.NET 中的表單驗證](http://www.4guysfromrolla.com/webtech/090200-1.shtml)，[剖析 ASP.NET 2.0 中的驗證控制項](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)，而[驗證伺服器控制項教學課程](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp)在  [W3Schools](http://www.w3schools.com/)。

之後製作使用者介面頁面的宣告式標記看起來應該像下面這樣：

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample8.aspx)]

使用者介面完成後，我們的下一個工作是插入新記錄到`GuestbookComments`資料表`PostCommentButton`按下。 這可在數種方式： 我們可以撰寫 ADO.NET 程式碼中的按鈕`Click`事件處理常式，我們可以 SqlDataSource 控制項加入頁面，設定其`InsertCommand`，然後呼叫其`Insert`方法從`Click`事件處理常式;或者我們可以建立負責插入新的訪客留言板註解的中介層，並且叫用這項功能從`Click`事件處理常式。 由於我們探討使用 SqlDataSource 在步驟 3 中，我們這裡使用 ADO.NET 程式碼。

> [!NOTE]
> 用來以程式設計方式存取資料，從 Microsoft SQL Server 資料庫的 ADO.NET 類別位於`System.Data.SqlClient`命名空間。 您可能需要將此命名空間匯入您的頁面程式碼後置類別 (亦即， `Imports System.Data.SqlClient`)。


建立事件處理常式`PostCommentButton`的`Click`事件，並新增下列程式碼：

[!code-vb[Main](storing-additional-user-information-vb/samples/sample9.vb)]

`Click`事件處理常式會先檢查該使用者提供的資料是否有效。 如果不是，事件處理常式會結束之前插入記錄。 假設所提供的資料是否有效，目前登入的使用者`UserId`值會擷取並儲存在`currentUserId`本機變數。 需要此值，因為我們必須提供`UserId`值插入至記錄`GuestbookComments`。

接下來的連接字串`SecurityTutorials`資料庫擷取自`Web.config`和`INSERT`指定 SQL 陳述式。 A`SqlConnection`物件然後建立並開啟。 下一步`SqlCommand`物件建構和參數的值用於`INSERT`指派查詢。 `INSERT`然後執行陳述式，並且在連線關閉。 事件處理常式的結尾`Subject`並`Body`文字方塊`Text`屬性會被清除，讓使用者的值不會在回傳之間保存。

請繼續並測試此頁面在瀏覽器中。 因為此頁面是在`Membership`資料夾不能存取匿名訪客。 因此，您必須先登入 （如果您尚未這麼做）。 輸入值`Subject`並`Body`的文字方塊，按一下 [ `PostCommentButton` ] 按鈕。 這會導致新的記錄新增至`GuestbookComments`。 在回傳時，會依照從文字方塊會抹除的主旨和本文所提供。

按一下後`PostCommentButton`有按鈕是沒有視覺回應可註解加入訪客留言板。 我們仍需要更新此頁面以顯示現有的訪客留言板註解，我們將在步驟 5 中這麼做。 一旦我們達到此目的時，剛加入的註解會出現在清單中的註解，提供適當的視覺化回饋。 現在，請確認所檢查的內容，儲存您的訪客留言板註解`GuestbookComments`資料表。

[圖 17] 顯示的內容`GuestbookComments`資料表後兩個註解已保留。


[![您可以看到訪客留言板中的註解 GuestbookComments 資料表](storing-additional-user-information-vb/_static/image50.png)](storing-additional-user-information-vb/_static/image49.png)

**圖 17**： 您可以看到在訪客留言`GuestbookComments`資料表 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image51.png))


> [!NOTE]
> 如果使用者嘗試插入訪客留言板註解，其中包含可能會擲回危險的標記 – 例如 HTML – ASP.NET `HttpRequestValidationException`。 若要深入了解這個例外狀況，它會擲回原因，以及如何允許使用者提交潛在的危險值，請參閱[要求驗證 （英文） 白皮書](../../../../whitepapers/request-validation.md)。


## <a name="step-5-listing-the-existing-guestbook-comments"></a>步驟 5： 列出現有的訪客留言板註解

除了留下註解，使用者瀏覽`Guestbook.aspx`頁面也應該能夠檢視訪客留言板的現有註解。 若要達成此目的，將新增 ListView 控制項，名為`CommentList`到頁面底部。

> [!NOTE]
> ListView 控制項是 asp.net 3.5 版的新功能。 它是在非常可自訂且彈性的配置中，顯示一份項目，但仍然可以提供內建編輯、 插入、 刪除、 分頁和排序 GridView 之類的功能。 如果您使用 ASP.NET 2.0，您必須改為使用 DataList 或 Repeater 控制項。 如需有關如何使用 ListView 的詳細資訊，請參閱[Scott Guthrie](https://weblogs.asp.net/scottgu/)的部落格文章[asp: ListView 控制項](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)，和我的文章[顯示的資料，與 ListView 控制項](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)。


開啟 ListView 的智慧標籤，然後從選擇資料來源 下拉式清單中，將控制項繫結至新的資料來源。 如我們在步驟 2 中所見的這會啟動 資料來源組態精靈。 選取 [資料庫] 圖示、 名稱產生 SqlDataSource `CommentsDataSource`，按一下 [確定]。 接下來，選取`SecurityTutorialsConnectionString`連線字串，從下拉式清單，然後按一下 [下一步]。

此時在步驟 2 中我們要查詢的資料所指定挑選`UserProfiles`資料表下拉式清單中，然後選取要傳回的資料行 （請參閱上一步 圖 9）。 此時，不過，我們想要製作可取回不只從資料錄的 SQL 陳述式`GuestbookComments`，但也評論者的主要城鎮、 首頁、 簽章，以及使用者名稱。 因此，選取 [指定自訂 SQL 陳述式或預存程序] 選項按鈕，然後按一下 [下一步]。

這會顯示 「 定義自訂陳述式或預存程序 」 畫面。 按一下 [查詢產生器] 按鈕，以圖形方式建立查詢。 查詢產生器一開始會提示我們指定我們想要從查詢的資料表。 選取  `GuestbookComments`， `UserProfiles`，和`aspnet_Users`資料表，並按一下 確定。 這會新增所有三個資料表至設計介面。 由於有之間的 foreign key 條件約束`GuestbookComments`， `UserProfiles`，並`aspnet_Users`資料表，查詢產生器自動`JOIN`s 這些資料表。

剩下的就是指定要傳回的資料行。 從`GuestbookComments`資料表選取`Subject`， `Body`，和`CommentDate`資料行，傳回`HomeTown`， `HomepageUrl`，以及`Signature`中的資料行`UserProfiles`資料表;，並傳回`UserName`從`aspnet_Users`. 此外，新增 「`ORDER BY CommentDate DESC`」 的結尾`SELECT`查詢，以便先傳回最新的文章。 完成之後這些選取項目，您的查詢產生器介面看起來應該類似 圖 18 螢幕擷取畫面。


[![建構查詢會聯結 GuestbookComments、 UserProfiles 和 aspnet_Users 資料表](storing-additional-user-information-vb/_static/image53.png)](storing-additional-user-information-vb/_static/image52.png)

**圖 18**: 建構的查詢`JOIN`s `GuestbookComments`， `UserProfiles`，並`aspnet_Users`資料表 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image54.png))


按一下 [確定] 關閉 [查詢產生器] 視窗並返回 「 定義自訂陳述式或預存程序 」 畫面。 按一下旁邊請前進到 測試查詢 畫面中，您可以在其中檢視測試查詢 按鈕，即可查詢結果。 當您準備好，按一下 [完成] 以完成設定資料來源精靈。

當我們完成步驟 2 中，相關聯的 DetailsView 控制項的 設定資料來源精靈`Fields`集合已更新為包含每個資料行所傳回的 BoundField `SelectCommand`。 ListView，不過，仍會保持不變;我們仍需要定義它的版面配置。 透過其宣告式標記或從它的智慧標籤的 「 設定 ListView 」 選項，可以手動建構 ListView 的版面配置。 我通常偏好使用以手動的方式，定義標記，但使用任何方法是最自然給您。

我最後會使用下列`LayoutTemplate`， `ItemTemplate`，和`ItemSeparatorTemplate`我 ListView 控制項：

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample10.aspx)]

`LayoutTemplate`標記會定義控制項所發出，而`ItemTemplate`呈現 SqlDataSource 所傳回的每個項目。 `ItemTemplate`的結果標記置於`LayoutTemplate`的`itemPlaceholder`控制項。 除了`itemPlaceholder`，則`LayoutTemplate`包含限制為顯示每一頁 （預設值） 的 10 個訪客留言板註解 ListView 的 DataPager 控制項和呈現分頁介面。

我`ItemTemplate`會顯示在每個訪客留言板註解主體`<h4>`平均分攤工作量下主體內文的項目。 請注意用來顯示本文的語法採用所傳回的資料`Eval("Body")`進行資料繫結陳述式，將它轉換為字串，並取代分行符號與`<br />`項目。 這項轉換才能顯示時送出的意見，因為空白字元會被忽略由 HTML 所輸入的分行符號。 使用者的簽章會顯示下斜體、 後面接著使用者的家用鎮，他的首頁、 日期和時間進行註解，以及保留註解之人員的使用者名稱的連結中的主體。

請花一點時間檢視透過瀏覽器頁面。 您應該會看到您加入訪客留言板，在此處顯示的步驟 5 中的註解。


[![Guestbook.aspx 現在會顯示訪客留言板的註解](storing-additional-user-information-vb/_static/image56.png)](storing-additional-user-information-vb/_static/image55.png)

**圖 19**:`Guestbook.aspx`現在會顯示訪客留言板的註解 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image57.png))


嘗試將新註解加入訪客留言板。 按一下`PostCommentButton`按鈕的頁面回傳和註解加入至資料庫，但是 ListView 控制項不會更新以顯示新的註解。 這可以透過下列方式修正：

- 更新`PostCommentButton`按鈕的`Click`事件處理常式，因此它會叫用 ListView 控制項的`DataBind()`方法之後插入資料庫中的新註解或
- 設定 ListView 控制項的`EnableViewState`屬性設`False`。 這種方法是因為停用控制項的檢視狀態，它必須重新繫結至基礎資料，在每次回傳。

教學課程的網站可從本教學課程會說明這兩種技巧。 ListView 控制項的`EnableViewState`屬性，以`False`而以程式設計方式重新繫結至 ListView 的資料所需的程式碼存在於`Click`事件處理常式，但標記為註解。

> [!NOTE]
> 目前`AdditionalUserInfo.aspx`頁面可讓使用者檢視和編輯其首頁的城鎮、 首頁和簽章設定。 可能是很好更新`AdditionalUserInfo.aspx`來顯示使用者的訪客留言板註解中的 登入。 也就是除了檢查和修改其資訊，使用者可以瀏覽`AdditionalUserInfo.aspx`頁面，以查看哪些訪客留言板註解她會在過去。 我不要更動此練習中，有興趣的讀者。


## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>步驟 6： 自訂 CreateUserWizard 控制項包含首頁城鎮、 首頁和簽章的介面

`SELECT`所使用的查詢`Guestbook.aspx`頁面上會使用`INNER JOIN`合併相關的記錄之間`GuestbookComments`， `UserProfiles`，和`aspnet_Users`資料表。 如果不有任何記錄中的使用者`UserProfiles`訪客留言板發出註解，不會在 ListView 中顯示的註解，因為`INNER JOIN`只會傳回`GuestbookComments`記錄中的相符記錄時`UserProfiles`和`aspnet_Users`。 以及我們看到在步驟 3 中，如果使用者沒有資料錄`UserProfiles`她無法檢視或編輯其設定`AdditionalUserInfo.aspx`頁面。

不用說，因為我們的設計決策很重要的成員資格系統中每個使用者帳戶沒有相對應的記錄中`UserProfiles`資料表。 我們想要為對應的記錄新增至`UserProfiles`透過 CreateUserWizard 每當建立新的成員資格使用者帳戶時。

中所述[*建立使用者帳戶*](creating-user-accounts-vb.md)教學課程中，新的成員資格使用者帳戶建立 CreateUserWizard 控制項之後會引發其[`CreatedUser`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). 我們可以建立此事件的事件處理常式、 取得使用者識別碼，針對剛建立的使用者，並再插入到資料錄`UserProfiles`的預設值表`HomeTown`， `HomepageUrl`，和`Signature`資料行。 不僅如此，就可以藉由自訂 CreateUserWizard 控制項的介面，以加入其他文字方塊會提示使用者輸入這些值。

讓我們先看看如何新增新的資料列`UserProfiles`資料表中`CreatedUser`事件處理常式，使用預設值。 接下來，我們會看到如何自訂 CreateUserWizard 控制項的使用者介面，以包含其他表單欄位，以收集新使用者的主要城鎮、 首頁和簽章。

### <a name="adding-a-default-row-touserprofiles"></a>加入預設資料列`UserProfiles`

在  [*建立使用者帳戶*](creating-user-accounts-vb.md)教學課程，我們新增 CreateUserWizard 控制項`CreatingUserAccounts.aspx`頁面`Membership`資料夾。 為了有 CreateUserWizard 控制項新增至記錄`UserProfiles`資料表使用者帳戶建立後，我們需要更新 CreateUserWizard 控制項的功能。 與其讓這些變更才`CreatingUserAccounts.aspx`頁面上，讓我們改為新增至 CreateUserWizard 控制項`EnhancedCreateUserWizard.aspx`頁面，並在本教學課程那里進行修改。

開啟`EnhancedCreateUserWizard.aspx`頁面上，在 Visual Studio 中，並將 CreateUserWizard 控制項從 [工具箱] 拖曳至頁面。 將 CreateUserWizard 控制項的`ID`屬性設`NewUserWizard`。 如我們所述[*建立使用者帳戶*](creating-user-accounts-vb.md)教學課程，CreateUserWizard 的預設使用者介面提示的訪客所需的資訊。 一旦已提供此資訊，控制項在內部會建立新的使用者帳戶在成員資格架構中，所有沒有我們不必撰寫一行程式碼。

CreateUserWizard 控制項在其工作流程期間引發的事件數目。 CreateUserWizard 控制項訪客提供要求資訊，並提交表單之後，一開始會引發其[`CreatingUser`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx)。 如果在建立過程中，問題[`CreateUserError`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx)引發; 不過，如果已成功建立使用者，則[`CreatedUser`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx)，就會引發。 在  [*建立使用者帳戶*](creating-user-accounts-vb.md)教學課程中我們建立的事件處理常式`CreatingUser`事件以確保提供的使用者名稱未包含任何前置或尾端空格及，使用者名稱中沒有出現任何位置的密碼。

若要新增的資料列`UserProfiles`資料表針對剛建立的使用者，我們需要建立的事件處理常式`CreatedUser`事件。 依時間`CreatedUser`已引發事件、 使用者帳戶已建立在成員資格架構，讓我們能夠擷取帳戶的使用者識別碼值。

建立事件處理常式`NewUserWizard`的`CreatedUser`事件，並新增下列程式碼：

[!code-vb[Main](storing-additional-user-information-vb/samples/sample11.vb)]

上述程式碼就藉由擷取剛加入的使用者帳戶的使用者識別碼。 這透過來完成`Membership.GetUser(username)`方法來傳回特定的使用者，然後將相關資訊`ProviderUserKey`屬性，以擷取其使用者識別碼。 CreateUserWizard 控制項中使用者所輸入的使用者名稱是可透過其[`UserName`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx)。

接下來，從擷取的連接字串`Web.config`和`INSERT`指定陳述式。 所需的 ADO.NET 物件會具現化並執行命令。 此程式碼指派[ `DBNull` ](https://msdn.microsoft.com/library/system.dbnull.aspx)執行個體`@HomeTown`， `@HomepageUrl`，和`@Signature`效果插入資料庫的參數`NULL`值`HomeTown`， `HomepageUrl`，和`Signature`欄位。

請瀏覽`EnhancedCreateUserWizard.aspx`頁面上透過瀏覽器，並建立新的使用者帳戶。 完成之後，請返回 Visual Studio，並檢查的內容`aspnet_Users`和`UserProfiles`資料表 （如同上一步 圖 12）。 您應該會看到新的使用者帳戶，在`aspnet_Users`與其相對應`UserProfiles`資料列 (與`NULL`的值`HomeTown`， `HomepageUrl`，和`Signature`)。


[![已加入新的使用者帳戶和 UserProfiles 記錄](storing-additional-user-information-vb/_static/image59.png)](storing-additional-user-information-vb/_static/image58.png)

**圖 20**: 新的使用者帳戶和`UserProfiles`已新增記錄 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image60.png))


訪客有提供其新的帳戶資訊，並按一下 [建立使用者] 按鈕，建立使用者帳戶，並加入一個資料列之後`UserProfiles`資料表。 CreateUserWizard 接著會顯示其`CompleteWizardStep`，顯示成功訊息並繼續 按鈕。 按一下 [繼續] 按鈕會導致回傳，但不採取任何動作，讓使用者卡在`EnhancedCreateUserWizard.aspx`頁面。

我們可以指定以 CreateUserWizard 控制項透過按一下 [繼續] 按鈕時傳送給使用者的 URL [ `ContinueDestinationPageUrl`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx)。 設定`ContinueDestinationPageUrl`屬性，以"~ / Membership/AdditionalUserInfo.aspx"。 這會採用新的使用者`AdditionalUserInfo.aspx`，其中也可以檢視和更新其設定。

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>自訂 CreateUserWizard 的介面，以提示新使用者的首頁城鎮、 首頁和簽章

CreateUserWizard 控制項的預設介面是能夠以簡單帳戶建立案例需要收集唯一核心使用者帳戶資訊，例如使用者名稱、 密碼和電子郵件的位置。 但是，萬一我們想要提示輸入她的主要城鎮、 首頁和簽章建立她的帳戶時，訪客？ 可自訂 CreateUserWizard 控制項的介面，以收集其他資訊，在註冊，以及這項資訊可用於`CreatedUser`插入基礎資料庫中的其他記錄的事件處理常式。

CreateUserWizard 控制項擴充 ASP.NET Wizard 控制項，也就是可讓網頁開發人員定義的已排序的一系列的控制項`WizardSteps`。 精靈控制項呈現作用中的步驟，並提供瀏覽介面，可讓這些步驟之間移動的訪客。 精靈控制項很適合用於細分成幾個簡短步驟的作業時間太長。 如需有關精靈控制項的詳細資訊，請參閱[建立與 ASP.NET 2.0 精靈控制項的逐步使用者介面](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)。

CreateUserWizard 控制項的預設標記會定義兩個`WizardSteps`:`CreateUserWizardStep`和`CompleteWizardStep`。

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample12.aspx)]

第一個`WizardStep`， `CreateUserWizardStep`，呈現會提示您輸入使用者名稱、 密碼、 電子郵件、 等等的介面。 訪客提供這項資訊，然後按一下 建立使用者 之後，她會顯示`CompleteWizardStep`，這會顯示成功訊息並繼續 按鈕。

若要自訂 CreateUserWizard 控制項的介面，以加入額外的表單欄位，我們可以：

- <strong>建立新的一或多個</strong><strong>`WizardStep`</strong><strong>以包含額外的使用者介面項目</strong>。 若要加入新`WizardStep`以 CreateUserWizard，按一下 「 新增/移除`WizardStep`s"連結來啟動其智慧標籤`WizardStep`集合編輯器。 從該處，您可以新增、 移除或重新排序精靈中的步驟。 這是本教學課程中，我們將使用的方法。

- <strong>轉換</strong><strong>`CreateUserWizardStep`</strong><strong>成可編輯</strong><strong>`WizardStep`</strong><strong>。</strong> 這會取代`CreateUserWizardStep`對等`WizardStep`其標記會定義比對的使用者介面`CreateUserWizardStep`' s。 藉由轉換`CreateUserWizardStep`成`WizardStep`我們可以重新調整控制項位置，或將額外的使用者介面項目新增至這個步驟。 要轉換`CreateUserWizardStep`或是`CompleteWizardStep`成可編輯`WizardStep`，請按一下 「 自訂建立使用者步驟 」，或從控制項的智慧標籤的 [自訂完成的步驟] 連結。

- **使用上述的兩個選項的一些組合。**

要牢記在心的一個重要的是 CreateUserWizard 控制項在中按一下 [建立使用者] 按鈕時，會執行其使用者帳戶建立程序及其`CreateUserWizardStep`。 如果有其他並不重要`WizardStep`之後的 s`CreateUserWizardStep`與否。

當加入自訂`WizardStep`CreateUserWizard 控制項來收集其他的使用者輸入，自訂`WizardStep`之前或之後可放置`CreateUserWizardStep`。 如果它前面`CreateUserWizardStep`從自訂的其他使用者輸入再收集`WizardStep`適用於`CreatedUser`事件處理常式。 不過，如果自訂`WizardStep`之後`CreateUserWizardStep`依時間自訂`WizardStep`會顯示已建立新的使用者帳戶和`CreatedUser`已經引發事件。

[圖 21] 顯示工作流程時，加入`WizardStep`前面`CreateUserWizardStep`。 因為已收集的其他使用者資訊的時間`CreatedUser`事件引發時，我們只需要是更新`CreatedUser`事件處理常式來擷取這些輸入，並將這些方案用於`INSERT`陳述式的參數值 （而非`DBNull.Value`).


[![當其他 WizardStep 前面 CreateUserWizardStep 時 CreateUserWizard 工作流程](storing-additional-user-information-vb/_static/image62.png)](storing-additional-user-information-vb/_static/image61.png)

**圖 21**: CreateUserWizard 工作流程時額外`WizardStep`Precedes `CreateUserWizardStep` ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image63.png))


如果自訂`WizardStep`放置*之後* `CreateUserWizardStep`，不過，建立使用者帳戶的程序，就會發生前使用者有機會輸入她的主要城鎮、 首頁或簽章。 在此情況下，這項額外資訊必須要插入至資料庫之後建立的使用者帳戶，如圖 22 所示。


[![當其他 WizardStep 之後 CreateUserWizardStep CreateUserWizard 工作流程](storing-additional-user-information-vb/_static/image65.png)](storing-additional-user-information-vb/_static/image64.png)

**圖 22**: CreateUserWizard 工作流程時額外`WizardStep`出現之後`CreateUserWizardStep`([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image66.png))


圖 22 所示的工作流程等候插入到資料錄`UserProfiles`步驟 2 完成後，直到資料表。 如果造訪者在步驟 1 之後，關閉她的瀏覽器，不過，我們將達到的狀態，其中的使用者帳戶已建立，但是沒有記錄已新增至`UserProfiles`。 有個解決方法是將具有的資料錄`NULL`或預設值插入至`UserProfiles`在`CreatedUser`（這在步驟 1 之後，就會引發） 事件處理常式和此記錄步驟 2 完成之後再更新。 這可確保`UserProfiles`將使用者帳戶新增記錄，即使在使用者結束註冊程序中途。

本教學課程中建立新`WizardStep`後出現`CreateUserWizardStep`之前`CompleteWizardStep`。 讓我們先取得放置 WizardStep 中的，並設定，然後我們將探討程式碼。

從 CreateUserWizard 控制項的智慧標籤，選取 「 新增/移除`WizardStep`s"，它會啟動`WizardStep`集合編輯器對話方塊。 加入新`WizardStep`，將其`ID`要`UserSettings`、 其`Title`到 [您的設定] 並將其`StepType`至`Step`。 然後將它定位好得之後`CreateUserWizardStep`（「 註冊您的新帳戶的"），以及之前`CompleteWizardStep`（「 完成 」），如圖 23 所示。


[![加入新的 WizardStep 至 CreateUserWizard 控制項](storing-additional-user-information-vb/_static/image68.png)](storing-additional-user-information-vb/_static/image67.png)

**圖 23**： 加入新`WizardStep`CreateUserWizard 控制項 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image69.png))


按一下 [確定] 關閉`WizardStep`集合編輯器對話方塊。 新`WizardStep`CreateUserWizard 控制項的已更新的宣告式標記為證明：

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample13.aspx)]

請注意新`<asp:WizardStep>`項目。 我們要新增的使用者介面，以收集新使用者的主要城鎮、 首頁和這裡的簽章。 宣告式語法中，或透過設計工具，您可以輸入此內容。 若要使用設計工具，請從下拉式清單中，智慧標籤，以查看在設計工具中的步驟中選取 「 您的設定 」 步驟。

> [!NOTE]
> 選取透過智慧標籤的下拉式清單中的步驟更新 CreateUserWizard 控制項[`ActiveStepIndex`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx)，以指定的索引開始的步驟。 因此，如果您使用此下拉式清單編輯設計工具中的 「 您的設定 」 步驟時，請確定它重設為 [登註冊您新增帳戶]，讓使用者第一次瀏覽時，會顯示這個步驟`EnhancedCreateUserWizard.aspx`頁面。


建立包含三個文字方塊控制項，名為 「 您的設定 」 步驟中的使用者介面`HomeTown`， `HomepageUrl`，和`Signature`。 之後建構這個介面，CreateUserWizard 的宣告式標記看起來應該如下所示：

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample14.aspx)]

請繼續瀏覽此頁面，透過瀏覽器和建立新的使用者帳戶，指定主要城鎮、 首頁，以及簽章值。 完成之後`CreateUserWizardStep`成員資格架構中建立的使用者帳戶和`CreatedUser`事件處理常式執行時，這會加入新的資料列`UserProfiles`，但資料庫`NULL`值`HomeTown`， `HomepageUrl`，及`Signature`. 永遠不會使用主要城鎮、 首頁和簽章的輸入值。 最後結果就是新的使用者帳戶`UserProfiles`記錄其`HomeTown`， `HomepageUrl`，和`Signature`欄位尚未指定。

我們需要執行程式碼之後，接受使用者輸入的主要城鎮、 honepage 和簽章值並更新適當的 「 您的設定 」 步驟`UserProfiles`記錄。 每次使用者在精靈中的步驟之間移動控制項時，精靈的[`ActiveStepChanged`事件](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx)引發。 我們可以建立此事件和更新的事件處理常式`UserProfiles`資料表時的 「 您的設定 」 步驟已完成。

新增事件處理常式，如 CreateUserWizard`ActiveStepChanged`事件，並新增下列程式碼：

[!code-vb[Main](storing-additional-user-information-vb/samples/sample15.vb)]

上述程式碼一開始會判斷如果我們已到達 「 已完成 」 步驟。 因為後面的 「 您的設定 」 步驟，就會發生 「 完成 」 步驟，然後造訪者到達時 「 完成 」 步驟，這表示她剛完成的 「 您的設定 」 步驟。

在此情況下，我們需要以程式設計方式參考中的文字方塊控制項`UserSettings WizardStep`。 這透過來完成第一個`FindControl`方法來以程式設計方式參考`UserSettings WizardStep`，然後再次從參考的文字方塊`WizardStep`。 當參考的文字方塊時，我們已經準備好執行`UPDATE`陳述式。 `UPDATE`陳述式具有相同數目的參數`INSERT`中的陳述式`CreatedUser`事件處理常式，但此處我們會使用使用者所提供的主要城鎮、 首頁和簽章值。

使用就地這個事件處理常式，請瀏覽`EnhancedCreateUserWizard.aspx`頁面上透過瀏覽器，並建立新的使用者帳戶，指定主要城鎮、 首頁，以及簽章值。 建立新的帳戶之後您應該重新導向至`AdditionalUserInfo.aspx`頁面上，其中會顯示剛輸入主要城鎮、 首頁和簽章資訊。

> [!NOTE]
> 我們的網站目前有兩個頁面讓訪客可以從中建立新的帳戶：`CreatingUserAccounts.aspx`和`EnhancedCreateUserWizard.aspx`。 網站的網站地圖和登入頁面是指向`CreatingUserAccounts.aspx` 頁面上，但`CreatingUserAccounts.aspx`不會提示使用者輸入其首頁的城鎮、 首頁和簽章資訊頁面，並不會新增至對應的資料列`UserProfiles`。 因此，更新`CreatingUserAccounts.aspx`頁面上，讓它提供這項功能或更新參考網站地圖和登入頁面`EnhancedCreateUserWizard.aspx`而不是`CreatingUserAccounts.aspx`。 如果您選擇第二種選項，請務必更新`Membership`資料夾的`Web.config`檔案，以便允許匿名使用者存取`EnhancedCreateUserWizard.aspx`頁面。


## <a name="summary"></a>總結

在本教學課程中，我們討論過的成員資格架構內的使用者帳戶相關的資料模型化技巧。 特別是，我們討論過模型與使用者帳戶，以及共用的一對一關聯性的資料共用的一對多關聯性的實體。 此外，我們可了解這如何相關資訊可以顯示、 插入和更新，使用 SqlDataSource 控制項和其他一些範例使用 ADO.NET 程式碼。

本教學課程中完成我們看看使用者帳戶。 從下一個教學課程中我們將把焦點轉到角色。 接下來幾個教學課程，我們將探討角色架構，了解如何建立新的角色，如何將角色指派給使用者，以判斷哪些角色使用者屬於，以及如何將角色型授權套用。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [存取及更新在 ASP.NET 2.0 中的資料](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [ASP.NET 2.0 的精靈控制項](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [建立與 ASP.NET 2.0 的精靈控制項的逐步使用者介面](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [建立自訂的資料來源控制項參數](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [自訂 CreateUserWizard 控制項](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [在 DetailsView 控制項的快速入門](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [使用 ListView 控制項顯示資料](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [剖析 ASP.NET 2.0 中的驗證控制項](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [編輯插入和刪除資料](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [在 ASP.NET 中的表單驗證](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [正在蒐集自訂的使用者註冊資訊](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [ASP.NET 2.0 中的設定檔](http://www.odetocode.com/Articles/440.aspx)
- [Asp: ListView 控制項](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [使用者設定檔的快速入門](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP 書籍的作者，他是 4GuysFromRolla.com 的創辦人，從事 Microsoft Web 技術工作自 1998 年。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是 *[Sams 教導您自己 ASP.NET 2.0 在 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 Scott 要聯絡[ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝...

本教學課程系列是由許多實用的檢閱者檢閱。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

> [!div class="step-by-step"]
> [上一步](user-based-authorization-vb.md)
