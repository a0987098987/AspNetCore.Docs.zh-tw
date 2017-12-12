---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
title: "儲存額外的使用者資訊 (VB) |Microsoft 文件"
author: rick-anderson
description: "在此教學課程中我們將回答這個問題，藉由建置非常初步的訪客簿應用程式。 在此情況下，我們將探討 modeli 不同的選項..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: ee4b924e-8002-4dc3-819f-695fca1ff867
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
msc.type: authoredcontent
ms.openlocfilehash: 8db63cb42fb04343150d2175a9d6fad1d5287a9b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="storing-additional-user-information-vb"></a>儲存額外的使用者資訊 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_VB.zip)或[下載 PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_vb.pdf)

> 在此教學課程中我們將回答這個問題，藉由建置非常初步的訪客簿應用程式。 在此情況下，我們會查看不同資料庫中的使用者資訊模型化的選項，然後了解如何將這項資料與成員資格 framework 所建立的使用者帳戶產生關聯。


## <a name="introduction"></a>簡介

ASP。網路的成員資格 framework 會提供彈性的介面，來管理使用者。 成員資格應用程式開發介面包含方法驗證認證、 擷取目前登入使用者的相關資訊，建立新的使用者帳戶，以及刪除使用者帳戶，和其他項目。 成員資格 framework 中的每個使用者帳戶包含只針對所需的驗證認證，並執行必要的使用者帳戶相關工作的屬性。 這證明方法和屬性[`MembershipUser`類別](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.aspx)的模型中的成員資格架構的使用者帳戶。 這個類別具有屬性，例如[ `UserName` ](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.username.aspx)， [ `Email` ](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.email.aspx)，和[ `IsLockedOut` ](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.islockedout.aspx)，如同方法和[ `GetPassword` ](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.getpassword.aspx)和[ `UnlockUser` ](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.unlockuser.aspx)。

有時候，應用程式必須儲存在成員資格 framework 未包含的其他使用者資訊。 例如，線上零售店可能需要讓每個使用者儲存其出貨和帳單地址、 付款資訊、 傳送喜好設定，並連絡電話號碼。 此外，在系統中的每筆訂單為特定使用者帳戶相關聯。

`MembershipUser`類別不包含屬性，例如`PhoneNumber`或`DeliveryPreferences`或`PastOrders`。 我們該如何追蹤應用程式所需的使用者資訊，讓它與成員資格架構整合？ 在此教學課程中我們將回答這個問題，藉由建置非常初步的訪客簿應用程式。 在此情況下，我們會查看不同資料庫中的使用者資訊模型化的選項，然後了解如何將這項資料與成員資格 framework 所建立的使用者帳戶產生關聯。 讓我們開始吧 ！

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>步驟 1： 建立訪客簿應用程式的資料模型

有各種不同的技術，可以用來擷取資料庫中的使用者資訊和其關聯的成員資格 framework 所建立的使用者帳戶。 為了說明這些技術，我們需要來增強教學課程的 web 應用程式，讓它會擷取某些類型的使用者相關資料。 (目前，應用程式服務所需的資料表只會包含應用程式的資料模型`SqlMembershipProvider`。)

讓我們來建立已驗證的使用者可以留下註解的其中一個非常簡單的訪客簿應用程式。 除了儲存訪客簿註解，讓我們允許每位使用者儲存其家用城鎮、 首頁和簽章。 如果提供，使用者的家用城鎮、 首頁及簽章會顯示在他已離開訪客簿中的每個訊息中。

### <a name="adding-theguestbookcommentstable"></a>加入`GuestbookComments`資料表

若要擷取的訪客簿註解，我們要建立資料庫資料表，名為`GuestbookComments`具有類似的資料行`CommentId`， `Subject`， `Body`，和`CommentDate`。 我們也必須將每一筆記錄`GuestbookComments`資料表參考保留註解的使用者。

若要將此資料表加入至我們的資料庫，請移至 Visual Studio 中的 資料庫總管和向下鑽研至`SecurityTutorials`資料庫。 「 資料表 」 資料夾上按一下滑鼠右鍵，然後選擇 加入新的資料表。 這會開啟可讓我們來定義新的資料表資料行的介面。


[![將新的資料表加入至 SecurityTutorials 資料庫](storing-additional-user-information-vb/_static/image2.png)](storing-additional-user-information-vb/_static/image1.png)

**圖 1**： 加入新的資料表，以便`SecurityTutorials`資料庫 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image3.png))


接下來，定義`GuestbookComments`的資料行。 加入名為資料行開始`CommentId`型別的`uniqueidentifier`。 此資料行中會唯一識別每個訪客簿中的註解，因此不允許`NULL`s 並將它標示為資料表的主索引鍵。 而不是提供值`CommentId`上每個欄位`INSERT`，我們可以表示新`uniqueidentifier`值應該自動產生此欄位上`INSERT`資料行的預設值設定為`NEWID()`。 加入這個第一個欄位，使它成為主索引鍵，並設定為預設值之後, 您的畫面看起來應該類似螢幕擷取畫面顯示在圖 2 中。


[![加入名為 CommentId 主要資料行](storing-additional-user-information-vb/_static/image5.png)](storing-additional-user-information-vb/_static/image4.png)

**圖 2**： 加入主要的資料行名為`CommentId`([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image6.png))


接下來，加入名為資料行`Subject`型別的`nvarchar(50)`和資料行名為`Body`型別的`nvarchar(MAX)`、 允許`NULL`中兩個資料行。 接下來，加入名為資料行`CommentDate`型別的`datetime`。 不允許`NULL`s 和組`CommentDate`資料行的預設值`getdate()`。

剩下的就是加入將使用者帳戶與每個訪客簿註解產生關聯的資料行。 其中一個選項，可加入資料行名為`UserName`型別的`nvarchar(256)`。 除了使用成員資格提供者時，這會是適合的選擇`SqlMembershipProvider`。 但當使用`SqlMembershipProvider`、 因為我們已在此教學課程系列，`UserName`中的資料行`aspnet_Users`資料表不保證是唯一的。 `aspnet_Users`資料表的主索引鍵是`UserId`是型別`uniqueidentifier`。 因此，`GuestbookComments`需要的資料行的資料表`UserId`型別的`uniqueidentifier`(不允許`NULL`值)。 請繼續並加入這欄。

> [!NOTE]
> 如我們所述[*在 SQL Server 中建立成員資格結構描述*](creating-the-membership-schema-in-sql-server-vb.md)教學課程，成員資格架構設計用來啟用多個 web 應用程式使用不同的使用者帳戶會共用相同使用者存放區。 它會藉由分割成不同的應用程式的使用者帳戶。 和中使用相同的使用者存放區的不同應用程式時，每個使用者名稱保證是唯一的應用程式中，可能使用相同的使用者名稱。 沒有複合`UNIQUE`中的條件約束`aspnet_Users`資料表`UserName`和`ApplicationId`欄位，但不是其中上只`UserName`欄位。 因此，很可能讓 aspnet\_使用者資料表擁有具有相同的兩個 （或以上） 資料錄`UserName`值。 不過，`UNIQUE`條件約束`aspnet_Users`資料表的`UserId`欄位 （因為它是主索引鍵）。 A`UNIQUE`條件約束很重要，因為沒有它，我們無法建立之間的外部索引鍵條件約束`GuestbookComments`和`aspnet_Users`資料表。


在新增之後`UserId`資料行中，依序按一下工具列中的 [儲存] 圖示上的資料表儲存。 命名新資料表`GuestbookComments`。

我們已處理的最後一個問題`GuestbookComments`資料表： 我們需要建立[foreign key 條件約束](https://msdn.microsoft.com/en-us/library/ms175464.aspx)之間`GuestbookComments.UserId`資料行和`aspnet_Users.UserId`資料行。 若要達成此目的，按一下工具列啟動外部索引鍵關聯性 對話方塊中的關聯性圖示。 （或者，您可以啟動此對話方塊中移至 資料表設計工具 功能表，然後選擇 關聯性。）

按一下 [外部索引鍵關聯性] 對話方塊的左下角中的 [新增] 按鈕。 這會新增新外部索引鍵條件約束，雖然我們仍需要定義關聯性中參與的資料表。


[![使用外部索引鍵關聯性 對話方塊來管理資料表的 Foreign Key 條件約束](storing-additional-user-information-vb/_static/image8.png)](storing-additional-user-information-vb/_static/image7.png)

**圖 3**： 用於管理資料表的 Foreign Key 條件約束的外部索引鍵關聯性對話方塊 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image9.png))


接下來，按一下 「 資料表和資料行規格 」 上的資料列右邊的省略符號圖示。 這樣就會啟動 [資料表和資料行] 對話方塊，我們可以從中指定主索引鍵的資料表和資料行的外部索引鍵資料行從`GuestbookComments`資料表。 具體來說，選取`aspnet_Users`和`UserId`作為主索引鍵資料表與資料行和`UserId`從`GuestbookComments`做外部索引鍵資料行的資料表 （請參閱圖 4）。 定義後的主要和外部索引鍵的資料表和資料行，按一下 [確定] 以返回 [外部索引鍵關聯性] 對話方塊。


[![建立外部索引鍵條件約束之間 aspnet_Users 和 GuesbookComments 資料表](storing-additional-user-information-vb/_static/image11.png)](storing-additional-user-information-vb/_static/image10.png)

**圖 4**: 外部索引鍵條件約束之間建立`aspnet_Users`和`GuesbookComments`資料表 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image12.png))


此時，已建立的外部索引鍵條件約束。 這個條件約束的存在可確保[關聯式完整性](http://en.wikipedia.org/wiki/Referential_integrity)，將不會參考到不存在的使用者帳戶的訪客簿項目，以及確保兩個資料表之間。 根據預設，外部索引鍵條件約束將不允許那里對應的子記錄，如果刪除父記錄。 也就是說，如果使用者對一或多個訪客簿註解，然後我們嘗試刪除該使用者帳戶，則刪除會失敗，除非他的訪客簿註解會先刪除。

Foreign key 條件約束可以設定為父記錄會被刪除時，自動刪除相關聯的子記錄。 換句話說，我們可以設定這個外部索引鍵條件約束，以便刪除其使用者帳戶時，會自動刪除使用者的訪客簿項目。 若要達成此目的，展開 < INSERT 和 UPDATE 規格 > 一節並將 [刪除規則] 屬性設定為 Cascade。


[![設定串聯刪除外部索引鍵條件約束](storing-additional-user-information-vb/_static/image14.png)](storing-additional-user-information-vb/_static/image13.png)

**圖 5**： 設定要串聯刪除 Foreign Key 條件約束 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image15.png))


若要儲存的外部索引鍵條件約束，請按一下 [關閉] 按鈕結束外部索引鍵關聯性。 然後按一下 [儲存] 圖示以儲存資料表，然後此工具列中的關聯性。

### <a name="storing-the-users-home-town-homepage-and-signature"></a>儲存使用者的家用城鎮、 首頁和簽章

`GuestbookComments`表格將說明如何儲存使用者帳戶與共用的一對多關聯性的資訊。 由於每個使用者帳戶可能有任意數目的相關聯的註解，此關聯性是由建立來保存 註解的集合，包括連結回特定使用者的每個註解的資料行的資料表模型化。 使用時`SqlMembershipProvider`，藉由建立名為資料行最好建立此連結`UserId`型別的`uniqueidentifier`及此資料行之間的外部索引鍵條件約束和`aspnet_Users.UserId`。

我們現在需要將三個資料行與儲存使用者的家用城鎮、 首頁和簽章，會出現在他的訪客簿註解每個使用者帳戶產生關聯。 有數個不同方式來完成這項作業：

- **加入新資料行 * * *`aspnet_Users`* * * 或 * * *`aspnet_Membership`* * * 的資料表。** 我不會建議這種方法，因為它會修改所使用的結構描述`SqlMembershipProvider`。 這項決策可能回顧天您日後。 例如，如果未來版本的 ASP.NET 會使用不同`SqlMembershipProvider`結構描述。 Microsoft 可能會包含一個工具，可移轉 ASP.NET 2.0`SqlMembershipProvider`資料到新的結構描述，但如果您已經修改 ASP.NET 2.0`SqlMembershipProvider`結構描述中，這類轉換可能不適用。

- **使用 ASP。網路的設定檔架構，定義設定檔屬性，如家用城鎮、 首頁和簽章。** ASP.NET 包含了設定檔架構是設計用來儲存使用者專屬的其他資料。 成員資格架構，例如設定檔 framework 是建置在提供者模型之上。 .NET Framework 隨附`SqlProfileProvider`存放區分析 SQL Server 資料庫中的資料。 事實上，我們的資料庫中已經有資料表由`SqlProfileProvider`(`aspnet_Profile`)，當我們新增回應用程式服務已加入[*在 SQL Server 中建立成員資格結構描述*](creating-the-membership-schema-in-sql-server-vb.md)教學課程。   
 設定檔架構的主要優點是，它可讓開發人員定義中的設定檔屬性`Web.config`– 沒有程式碼以供需要寫序列化設定檔資料，以及從基礎資料存放區。 簡單地說，是非常簡單，來定義一組設定檔屬性，並在程式碼中使用它們。 不過，設定檔系統不夠令人需要進行版本控制時，如果您的應用程式預期新的使用者特定屬性，以在稍後的時間或要移除或修改現有的加入，然後設定檔架構可能不是 最佳的選項。 此外，`SqlProfileProvider`儲存設定檔屬性以高非正規化的方式下, 一步 無法直接對設定檔資料 （例如，多少使用者擁有 New York 家用城鎮） 執行查詢。   
 如需有關設定檔架構的詳細資訊，請參閱本教學課程結尾處的 「 進一步讀數 」 一節。

- **將下列三個資料行加入至資料庫中的新資料表，並建立此資料表之間的一對一關聯性和 * * *`aspnet_Users`* * *。** 這種方法牽涉到更多工作的設定檔架構，但提供了相當大的彈性資料庫中其他使用者屬性所建立的模型。 這是我們將在本教學課程中使用的選項。

我們將建立新的資料表，稱為`UserProfiles`儲存家用城鎮、 首頁上，並為每個使用者的簽章。 在 [資料庫總管] 視窗中的 [資料表] 資料夾上按一下滑鼠右鍵，然後選擇要建立新的資料表。 名稱的第一個資料行`UserId`並將其類型設定為`uniqueidentifier`。 不允許`NULL`值，並標示為主索引鍵資料行。 接下來，加入名為資料行：`HomeTown`型別的`nvarchar(50)`;`HomepageUrl`型別的`nvarchar(100)`; 和類型的簽章`nvarchar(500)`。 每個這些三個資料行可以接受`NULL`值。


[![建立 UserProfiles 資料表](storing-additional-user-information-vb/_static/image17.png)](storing-additional-user-information-vb/_static/image16.png)

**圖 6**： 建立`UserProfiles`資料表 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image18.png))


儲存資料表並將其命名`UserProfiles`。 最後，建立之間的外部索引鍵條件約束`UserProfiles`資料表的`UserId`欄位和`aspnet_Users.UserId`欄位。 如同我們一樣之間的外部索引鍵條件約束`GuestbookComments`和`aspnet_Users`資料表，具有串聯刪除這個條件約束。 因為`UserId`欄位`UserProfiles`是主要索引鍵，這可確保會有超過一筆記錄`UserProfiles`資料表每個使用者帳戶。 這種類型的關聯性被指為一對一。

現在，我們已經建立的資料模型，我們準備要使用它。 步驟 2 和 3 中我們將探討如何目前登入的使用者可以檢視和編輯其主城鎮、 首頁和簽章資訊。 步驟 4 中，我們將建立來送出的訪客簿新註解，以及檢視現有的已驗證的使用者介面。

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>步驟 2： 顯示使用者的家用城鎮、 首頁和簽章

有各種不同的方式，讓目前登入的使用者，來檢視和編輯其主城鎮、 首頁和簽章資訊。 我們無法使用文字方塊中手動建立使用者介面和 Label 控制項抑或我們無法使用其中一種 Web 控制項，例如 DetailsView 控制項的資料。 若要執行資料庫`SELECT`和`UPDATE`陳述式我們可以撰寫 ADO.NET 我們的網頁程式碼後置類別中的程式碼或，或者，使用宣告式與 SqlDataSource 方法。 在理想情況下我們的應用程式會包含階層式的架構，我們可能是程式設計方式叫用網頁的程式碼後置類別或以宣告方式透過 ObjectDataSource 控制項。

因為這個教學課程系列著重於表單驗證、 授權、 使用者帳戶和角色，則不會有這些不同的資料存取選項或階層式的架構為何慣用透過直接執行 SQL 陳述式的完整討論從 ASP.NET 頁面。 我們即將逐步解說使用 DetailsView 和 SqlDataSource – 最快速而且簡單選項 – 但討論的概念確實可套用至替代 Web 控制項與資料存取邏輯。 如需在 ASP.NET 中的資料搭配使用的詳細資訊，請參閱我*[在 ASP.NET 2.0 中使用資料](../../data-access/index.md)*教學課程系列。

開啟`AdditionalUserInfo.aspx`頁面`Membership`資料夾並將 DetailsView 控制項加入至頁面上，將它的 ID 屬性設定為`UserProfile`和清除其`Width`和`Height`屬性。 展開 DetailsView 的智慧標籤，然後選擇將它繫結至新的資料來源控制項。 這樣就會啟動資料來源組態精靈 （請參閱圖 7）。 第一個步驟會要求您指定的資料來源類型。 因為我們將直接連接到`SecurityTutorials`資料庫，請選擇 [資料庫] 圖示上，指定`ID`為`UserProfileDataSource`。


[![新增名為 UserProfileDataSource SqlDataSource 控制項](storing-additional-user-information-vb/_static/image20.png)](storing-additional-user-information-vb/_static/image19.png)

**圖 7**： 加入新的 SqlDataSource 控制項命名`UserProfileDataSource`([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image21.png))


下一個畫面會提示輸入要使用的資料庫。 我們已經定義中的連接字串`Web.config`如`SecurityTutorials`資料庫。 此連接字串名稱中 – `SecurityTutorialsConnectionString` – 應該會在下拉式清單。 選取此選項，然後按一下 [下一步]。


[![從下拉式清單中選擇 SecurityTutorialsConnectionString](storing-additional-user-information-vb/_static/image23.png)](storing-additional-user-information-vb/_static/image22.png)

**圖 8**： 選擇`SecurityTutorialsConnectionString`從下拉式清單 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image24.png))


後續的畫面會詢問我們指定的資料表和查詢的資料行。 選擇`UserProfiles`資料表下拉式清單中，並檢查所有資料行。


[![將從 UserProfiles 資料表傳回所有資料行](storing-additional-user-information-vb/_static/image26.png)](storing-additional-user-information-vb/_static/image25.png)

**圖 9**： 帶回所有資料行`UserProfiles`資料表 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image27.png))


圖 9 傳回目前查詢*所有*中記錄的`UserProfiles`，但是您只想要在目前登入的使用者記錄中。 若要加入`WHERE`子句中，按一下 `WHERE`即可啟動 新增 按鈕`WHERE`子句對話方塊 （請參閱圖 10）。 您可以選取要篩選的資料行、 運算子和篩選參數的來源。 選取`UserId`做為資料行和"="做為運算子。

不幸的是要傳回目前登入的使用者沒有內建參數來源我們`UserId`值。 我們必須以程式設計方式擷取此值。 因此，設定為 [無，] 按一下 [新增] 按鈕以新增參數，然後按一下 [確定] 的 [來源] 下拉式清單。


[![使用者識別碼資料行上加入篩選參數](storing-additional-user-information-vb/_static/image29.png)](storing-additional-user-information-vb/_static/image28.png)

**圖 10**： 上加入 Filter 參數`UserId`資料行 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image30.png))


按一下 [確定] 之後您將返回圖 9 所示的畫面。 這個時間，不過，SQL 查詢，在畫面底部應包括`WHERE`子句。 按下一步移至 「 測試查詢 」 畫面。 您可以在這裡執行查詢並查看結果。 按一下 [完成] 5d; 以完成精靈。

完成資料來源組態精靈，Visual Studio 會建立 SqlDataSource 控制項，取決於精靈中指定的設定。 此外，它以手動方式加入 BoundFields 每個資料行傳回 SqlDataSource DetailsView `SelectCommand`。 若要顯示不需要`UserId`欄位中 DetailsView，因為使用者不需要知道此值。 您可以直接從 DetailsView 控制項的宣告式標記中移除此欄位，或按一下 「 編輯欄位 」 連結的智慧標記。

此時網頁的宣告式標記看起來應該如下所示：

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample1.aspx)]

我們需要以程式設計方式設定 SqlDataSource 控制項`UserId`參數，目前登入的使用者以`UserId`之前選取的資料。 這可藉由建立事件處理常式的 SqlDataSource`Selecting`事件，並加入下列程式碼那里：

[!code-vb[Main](storing-additional-user-information-vb/samples/sample2.vb)]

上述的程式碼開始，取得目前登入使用者的參考，藉由呼叫`Membership`類別的`GetUser`方法。 這會傳回`MembershipUser`物件，其`ProviderUserKey`屬性包含`UserId`。 `UserId`值接著會指派給 SqlDataSource`@UserId`參數。

> [!NOTE]
> `Membership.GetUser()`方法會傳回目前登入使用者的相關資訊。 如果匿名使用者已瀏覽頁面，則會傳回值為`Nothing`。 在這種情況下，這會導致`NullReferenceException`的程式碼時嘗試讀取下一行`ProviderUserKey`屬性。 當然，我們不需要擔心`Membership.GetUser()`傳回中為 Nothing`AdditionalUserInfo.aspx`頁面上，因為我們將設定 URL 授權在上一個教學課程中，以便只有驗證的使用者無法存取此資料夾中的 ASP.NET 資源。 如果您需要存取目前登入的使用者允許匿名存取的位置在頁面的相關資訊，請務必檢查`MembershipUser`從傳回的物件`GetUser()`方法不是 Nothing 之前參考其屬性。


如果您瀏覽`AdditionalUserInfo.aspx`頁面透過瀏覽器您會看到一個空白網頁，因為我們尚未將任何資料列加入`UserProfiles`資料表。 步驟 6 中我們將探討如何自訂適用於 CreateUserWizard 控制項，以自動新增至新的資料列`UserProfiles`資料表時建立新的使用者帳戶。 不過，現在，我們將需要手動建立記錄資料表中。

巡覽至 Visual Studio 中的 [資料庫總管] 並展開 [資料表] 資料夾。 以滑鼠右鍵按一下`aspnet_Users`資料表並選擇 顯示資料表資料 」 以查看資料表中的記錄; 執行相同的動作，`UserProfiles`資料表。 圖 11 顯示垂直並排顯示時，這些結果。 在 我的資料庫中目前沒有`aspnet_Users`Bruce、 Fred 和 Tito，記錄，但沒有記錄在`UserProfiles`資料表。


[![Aspnet_Users 的內容和 UserProfiles 資料表會顯示](storing-additional-user-information-vb/_static/image32.png)](storing-additional-user-information-vb/_static/image31.png)

**圖 11**: 內容`aspnet_Users`和`UserProfiles`資料表會顯示 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image33.png))


新增新記錄以`UserProfiles`手動輸入的值中的資料表`HomeTown`， `HomepageUrl`，和`Signature`欄位。 若要取得有效的最簡單的方式`UserId`在新的值`UserProfiles`記錄是選取`UserId`欄位中的特定使用者帳戶從`aspnet_Users`資料表，然後複製並貼到`UserId`欄位`UserProfiles`。 圖 12 顯示`UserProfiles`資料表之後 Bruce 已加入新的記錄。


[![記錄新增至 UserProfiles Bruce 的](storing-additional-user-information-vb/_static/image35.png)](storing-additional-user-information-vb/_static/image34.png)

**圖 12**: A 記錄已加入至`UserProfiles`如 Bruce ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image36.png))


返回`AdditionalUserInfo.aspx page`、 Bruce 為登入。 如圖 13 所示，Bruce 的設定會顯示。


[![目前瀏覽使用者是顯示 His 的設定](storing-additional-user-information-vb/_static/image38.png)](storing-additional-user-information-vb/_static/image37.png)

**圖 13**: 目前瀏覽使用者是顯示 His 設定 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image39.png))


> [!NOTE]
> 移往前或以手動方式新增記錄`UserProfiles`資料表針對每個成員資格使用者。 步驟 6 中我們將探討如何自訂適用於 CreateUserWizard 控制項，以自動新增至新的資料列`UserProfiles`資料表時建立新的使用者帳戶。


## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>步驟 3： 允許使用者編輯他首頁城鎮、 首頁和簽章

此時目前登入的使用者可以檢視其主城鎮、 首頁上，以及簽章設定，但他們不能尚未修改。 讓我們來更新 DetailsView 控制項，讓資料可供編輯。

我們要做的第一件事就是加入`UpdateCommand`SqlDataSource，如指定`UPDATE`陳述式執行和其相對應的參數。 選取 SqlDataSource，並從 屬性 視窗中，按一下即可啟動 命令及參數編輯器對話方塊的 UpdateQuery 屬性旁邊的省略符號。 輸入下列`UPDATE` 文字方塊中的陳述式：

[!code-sql[Main](storing-additional-user-information-vb/samples/sample3.sql)]

接下來，按一下 [重新整理參數] 按鈕，將會在 SqlDataSource 控制項的建立參數`UpdateParameters`中的參數集合`UPDATE`陳述式。 保留所有的參數集來源為 None，然後按一下 [確定] 按鈕以完成對話方塊。


[![指定 SqlDataSource UpdateCommand 和 UpdateParameters](storing-additional-user-information-vb/_static/image41.png)](storing-additional-user-information-vb/_static/image40.png)

**圖 14**： 指定 SqlDataSource`UpdateCommand`和`UpdateParameters`([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image42.png))


新增的項目因為我們對 SqlDataSource 控制項，DetailsView 控制項現在可以支援編輯。 在 DetailsView 的智慧標籤上，從選取 「 啟用編輯 」 核取方塊。 這會加入控制項的 CommandField`Fields`集合，其`ShowEditButton`屬性設定為 True。 在 DetailsView 會顯示在唯讀模式和更新和取消按鈕時顯示在編輯模式時，這會呈現編輯 按鈕。 不需要按一下 編輯使用者，不過，我們中可以有 DetailsView 呈現"一律可編輯的 「 狀態藉由設定 DetailsView 控制項的[`DefaultMode`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx)至`Edit`。

這些變更，DetailsView 控制項的宣告式標記看起來應該如下所示：

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample4.aspx)]

請注意 CommandField 的加法和`DefaultMode`屬性。

請繼續並測試此頁面，透過瀏覽器。 當具有對應的記錄中的使用者使用瀏覽`UserProfiles`，使用者的設定會顯示在可編輯的介面。


[![在 DetailsView 呈現可編輯的介面](storing-additional-user-information-vb/_static/image44.png)](storing-additional-user-information-vb/_static/image43.png)

**圖 15**: DetailsView 呈現可編輯的介面 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image45.png))


再試一次變更的值，然後按一下 [更新] 按鈕。 它會顯示一樣會發生任何事。 沒有回傳和值會儲存到資料庫，但是沒有儲存發生沒有視覺回應。

若要補救這種情況，返回 Visual Studio，加入上述 DetailsView 的標籤控制項。 設定其`ID`至`SettingsUpdatedMessage`、 其`Text`」 您的設定已更新，"的屬性和其`Visible`和`EnableViewState`屬性，以`False`。

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample5.aspx)]

我們需要顯示`SettingsUpdatedMessage`只要更新 DetailsView 的標籤。 若要達成此目的，建立事件處理常式在 DetailsView 的`ItemUpdated`事件並加入下列程式碼：

[!code-vb[Main](storing-additional-user-information-vb/samples/sample6.vb)]

返回`AdditionalUserInfo.aspx`頁面上透過瀏覽器及更新資料。 此時，很有幫助的狀態訊息會顯示。


[![短訊息會顯示當設定會更新](storing-additional-user-information-vb/_static/image47.png)](storing-additional-user-information-vb/_static/image46.png)

**圖 16**: 短訊息會顯示更新的設定時 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image48.png))


> [!NOTE]
> DetailsView 控制項的編輯介面讓人滿意。 它會使用標準大小的文字方塊中，但簽章欄位可能應該是多行文字方塊。 RegularExpressionValidator 應該用來確保首頁的 URL，如果輸入，會啟動以"http://"或"https://"。 此外，因為 DetailsView 控制項有其`DefaultMode`屬性設定為`Edit`，[取消] 按鈕不會執行任何動作。 它應該是或移除，按一下時，將使用者重新導向至其他網頁 (例如`~/Default.aspx`)。 做為練習保留這些增強功能時，我的讀取器。


### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>加入連結`AdditionalUserInfo.aspx`[Master] 頁面中

目前，網站不提供任何連結`AdditionalUserInfo.aspx`頁面。 連線到它的唯一方式是直接在瀏覽器的網址列中輸入網頁的 URL。 讓我們將連結加入到此頁面在`Site.master`主版頁面。

前文提過的主版頁面包含 LoginView Web 控制項在其`LoginContent`ContentPlaceHolder 顯示不同的標記為已驗證和匿名的訪客。 更新 LoginView 控制項`LoggedInTemplate`加入連結`AdditionalUserInfo.aspx`頁面。 這些變更後 LoginView 控制項的宣告式標記看起來應該如下所示：

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample7.aspx)]

請注意新增`lnkUpdateSettings`超連結控制項以`LoggedInTemplate`。 與就地這個連結，已驗證的使用者可以快速跳至頁面，檢視並修改其主城鎮、 首頁和簽章設定。

## <a name="step-4-adding-new-guestbook-comments"></a>步驟 4： 加入新的訪客簿註解

`Guestbook.aspx`頁面是已驗證的使用者可檢視的訪客簿並留下註解。 讓我們開始建立要加入新的訪客簿註解的介面。

開啟`Guestbook.aspx`Visual Studio 中的頁面上，並建構兩個文字方塊控制項，一個用於新的註解主體，一個用於其本文所組成的使用者介面。 將第一個文字方塊控制項的`ID`屬性`Subject`及其`Columns`為 40 的屬性，則將第二個`ID`至`Body`、 其`TextMode`至`MultiLine`，及其`Width`和`Rows`屬性，以"95%"和 8，分別。 若要完成的使用者介面，加入按鈕 Web 控制項，名為`PostCommentButton`並設定其`Text`「 張貼您的註解 」 的屬性。

由於主旨和本文中需要每個訪客簿註解，請針對每個文字方塊加入 RequiredFieldValidator。 設定`ValidationGroup`屬性的這些控制項，以 「 EnterComment"; 同樣地，設定`PostCommentButton`控制項的`ValidationGroup`"EnterComment"的屬性。 如需有關 ASP 的詳細資訊。網路的驗證控制項、 簽出[中 ASP.NET 表單驗證](http://www.4guysfromrolla.com/webtech/090200-1.shtml)，[將 ASP.NET 2.0 中的驗證控制項的分解](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)，而[驗證伺服器控制項教學課程](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp)在[W3Schools](http://www.w3schools.com/)。

之後製作的使用者介面頁面的宣告式標記看起來應該像下面這樣：

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample8.aspx)]

完整的使用者介面下, 一步是要插入新的記錄，到`GuestbookComments`資料表`PostCommentButton`按下。 達成這點可以數種方式： 我們可以撰寫 ADO.NET 程式碼中的按鈕`Click`事件處理常式，我們可以將 SqlDataSource 控制項加入至頁面，設定其`InsertCommand`，然後呼叫其`Insert`方法從`Click`事件處理常式。或者我們可以建立負責插入新的訪客簿註解，將中介層，並叫用此功能，從`Click`事件處理常式。 因為我們還在使用 SqlDataSource 在步驟 3 中，讓我們這裡使用的 ADO.NET 程式碼。

> [!NOTE]
> 用來以程式設計方式存取資料，從 Microsoft SQL Server 資料庫的 ADO.NET 類別位於`System.Data.SqlClient`命名空間。 您可能需要此命名空間匯入網頁的程式碼後置類別 (亦即， `Imports System.Data.SqlClient`)。


建立事件處理常式`PostCommentButton`的`Click`事件並加入下列程式碼：

[!code-vb[Main](storing-additional-user-information-vb/samples/sample9.vb)]

`Click`事件處理常式開始檢查使用者提供的資料無效。 如果不是，事件處理常式會在插入記錄之前結束。 假設您所提供的資料是否有效，目前登入的使用者`UserId`值會擷取並儲存在`currentUserId`本機變數。 需要此值，因為我們必須提供`UserId`值時插入記錄，到`GuestbookComments`。

接下來的連接字串`SecurityTutorials`資料庫擷取自`Web.config`和`INSERT`指定 SQL 陳述式。 A`SqlConnection`物件然後建立並開啟。 下一步`SqlCommand`物件建構和參數的值用於`INSERT`查詢指派。 `INSERT`然後執行陳述式並關閉連線。 事件處理常式結尾`Subject`和`Body`文字方塊`Text`，讓使用者的值不會保存跨回傳，將會清除的內容。

請繼續並測試這個瀏覽器中。 因為此頁面是在`Membership`資料夾不能存取為匿名的訪客。 因此，您必須先登入 （如果您還沒有）。 輸入值`Subject`和`Body`文字方塊按一下`PostCommentButton` 按鈕。 這會導致新的記錄加入至`GuestbookComments`。 在回傳時，會抹除的主旨和本文您提供的文字方塊。

按一下後`PostCommentButton`有按鈕是沒有視覺回應可註解加入至訪客簿。 我們仍需要更新以顯示現有的訪客簿註解，我們會執行步驟 5 中的此頁面。 一旦我們達到此目的，只加入註解會出現在清單中的註解，提供適當的視覺化回應。 現在，確認您的訪客簿註解已檢查的內容儲存`GuestbookComments`資料表。

圖 17 顯示的內容`GuestbookComments`資料表後兩個註解已保留。


[![您可以看到的訪客簿中的註解 GuestbookComments 資料表](storing-additional-user-information-vb/_static/image50.png)](storing-additional-user-information-vb/_static/image49.png)

**圖 17**： 您可以查看中的訪客簿註解`GuestbookComments`資料表 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image51.png))


> [!NOTE]
> 如果使用者嘗試插入的訪客簿註解，其中包含可能會擲回危險標記 – 例如 HTML – ASP.NET `HttpRequestValidationException`。 若要深入了解這個例外狀況，它會擲回原因，以及如何允許使用者提交具有潛在危險性的值，請參閱[要求驗證白皮書](../../../../whitepapers/request-validation.md)。


## <a name="step-5-listing-the-existing-guestbook-comments"></a>步驟 5： 列出現有的訪客簿註解

除了保留註解、 使用者瀏覽`Guestbook.aspx`頁面應該也可以檢視的訪客簿現有註解。 若要完成這項作業，加入名為 ListView 控制項`CommentList`頁面的底部。

> [!NOTE]
> ListView 控制項的新 asp.net 3.5 版。 它被設計來顯示項目清單以非常可自訂且彈性的配置，但仍然可以提供內建編輯、 插入、 刪除、 分頁和排序 GridView 等功能。 如果您使用 ASP.NET 2.0，您必須改用 DataList 或中繼器控制項。 如需有關如何使用清單檢視的詳細資訊，請參閱[Scott Guthrie](https://weblogs.asp.net/scottgu/)的部落格項目[asp: ListView 控制項](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)，和 我的文件： [ListView 控制項顯示的資料](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)。


開啟 ListView 的智慧標籤，然後從 [選擇資料來源] 下拉式清單中，將控制項繫結至新的資料來源。 如我們所見在步驟 2 中，這會啟動 資料來源組態精靈。 選取資料庫圖示，產生 SqlDataSource `CommentsDataSource`，按一下 [確定]。 接下來，選取`SecurityTutorialsConnectionString`連接字串從下拉式清單，並按一下 [下一步]。

此時在步驟 2 中我們要查詢的資料所指定挑選`UserProfiles`資料表下拉式清單中，選取要傳回的資料行 （請參閱上一步圖 9）。 此時，不過，我們想要製作提取回不只記錄的 SQL 陳述式`GuestbookComments`，但也註解的家用城鎮、 首頁、 簽章和使用者名稱。 因此，選取的 「 指定的自訂 SQL 陳述式或預存程序 」 選項按鈕，然後按一下 [下一步]。

這會顯示 「 定義自訂陳述式或預存程序 」 畫面。 按一下 [查詢產生器] 按鈕，以圖形方式建立查詢。 查詢產生器一開始會提示我們指定我們想要從查詢的資料表。 選取`GuestbookComments`， `UserProfiles`，和`aspnet_Users`資料表，並按一下 [確定]。 這會將所有的三個資料表加入設計介面。 由於有之間的 foreign key 條件約束`GuestbookComments`， `UserProfiles`，和`aspnet_Users`資料表，查詢產生器自動`JOIN`s 這些資料表。

剩下的就是指定要傳回的資料行。 從`GuestbookComments`資料表選取`Subject`， `Body`，和`CommentDate`資料行; 傳回`HomeTown`， `HomepageUrl`，和`Signature`中的資料行`UserProfiles`資料表;，並傳回`UserName`從`aspnet_Users`. 此外，請加入"`ORDER BY CommentDate DESC`"結尾`SELECT`查詢，以便先傳回最新的文章。 這些選取項目之後，您查詢產生器的介面應該類似螢幕擷取畫面圖 18。


[![建構查詢聯結 GuestbookComments、 UserProfiles 和 aspnet_Users 資料表](storing-additional-user-information-vb/_static/image53.png)](storing-additional-user-information-vb/_static/image52.png)

**圖 18**： 建構查詢`JOIN`s `GuestbookComments`， `UserProfiles`，和`aspnet_Users`資料表 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image54.png))


按一下 [確定] 關閉 [查詢產生器] 視窗，然後回到 「 定義自訂陳述式或預存程序 」 畫面。 按一下前進至 「 測試查詢 」 畫面上，其中您可以檢視查詢結果，依序按一下 [測試查詢] 按鈕旁邊。 當您準備好時，請按一下 [完成] 來完成設定資料來源精靈。

當我們完成設定資料來源精靈，在步驟 2 中，關聯的 DetailsView 控制項的`Fields`集合已更新為包含每個資料行所傳回的 BoundField `SelectCommand`。 清單檢視，不過，維持不變;我們仍需要定義它的版面配置。 透過其宣告式標記，或在其智慧標籤 」 設定 ListView"選項中，可以手動建構 ListView 的配置。 我通常偏好以手動方式定義標記，但使用為您最自然階層的任何方法。

我全數使用下列`LayoutTemplate`， `ItemTemplate`，和`ItemSeparatorTemplate`我的 ListView 控制項：

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample10.aspx)]

`LayoutTemplate`定義標記控制項所發出，而`ItemTemplate`呈現 SqlDataSource 所傳回的每個項目。 `ItemTemplate`的結果標記置於`LayoutTemplate`的`itemPlaceholder`控制項。 除了`itemPlaceholder`、`LayoutTemplate`包含 DataPager 控制項，這會限制在清單檢視顯示每一頁 （預設值） 的 10 個訪客簿註解，並呈現分頁介面。

我`ItemTemplate`顯示中的每個訪客簿註解主體`<h4>`具有以下主旨平均分攤工作量的主體項目。 請注意，用來顯示內文的語法採用所傳回的資料`Eval("Body")`資料繫結陳述式，將它轉換為字串，並取代分行符號與`<br />`項目。 若要顯示輸入提交的註解，因為 HTML 會忽略空白字元時換行需要這項轉換。 使用者的簽章會顯示下方斜體使用者的家用城鎮，他首頁、 日期和時間進行註解，以及保留註解之人員的使用者名稱的連結中的主體。

花點時間檢視透過瀏覽器頁面。 您應該會看到您加入的訪客簿在此處顯示的步驟 5 中的註解。


[![Guestbook.aspx 現在會顯示的訪客簿註解](storing-additional-user-information-vb/_static/image56.png)](storing-additional-user-information-vb/_static/image55.png)

**圖 19**:`Guestbook.aspx`現在會顯示的訪客簿註解 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image57.png))


請嘗試將新註解加入至訪客簿。 按一下後`PostCommentButton`按鈕頁面回傳和註解加入至資料庫，但 ListView 控制項不會更新以顯示新的註解。 這可以透過其中一種以進行修正：

- 更新`PostCommentButton`按鈕的`Click`事件處理常式，它會叫用的 ListView 控制項的`DataBind()`方法之後的新註解插入資料庫，或
- 設定 ListView 控制項的`EnableViewState`屬性`False`。 這種方式運作，因為停用控制項的檢視狀態，它必須重新繫結至基礎資料在每次回傳。

教學課程的網站下載此教學課程將說明這兩種技術。 ListView 控制項的`EnableViewState`屬性`False`並且以程式設計方式重新繫結至清單資料所需的程式碼出現在`Click`事件處理常式，但標記為註解。

> [!NOTE]
> 目前`AdditionalUserInfo.aspx`頁面可讓使用者檢視和編輯其主城鎮、 首頁和簽章的設定。 可能是很好更新`AdditionalUserInfo.aspx`顯示登入使用者的訪客簿註解。 也就是說，除了檢查和修改其資訊，使用者可以瀏覽`AdditionalUserInfo.aspx`頁面，以查看哪些訪客簿註解她已在過去。 我將保留這為一項工作有興趣的讀取器。


## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>步驟 6： 自訂適用於 CreateUserWizard 控制項包含首頁城鎮、 首頁和簽章的介面

`SELECT`所使用的查詢`Guestbook.aspx`頁面使用`INNER JOIN`合併相關的記錄之間`GuestbookComments`， `UserProfiles`，和`aspnet_Users`資料表。 如果不有任何記錄中的使用者`UserProfiles`訪客簿發出，註解的註解不會顯示在清單檢視，因為`INNER JOIN`只會傳回`GuestbookComments`記錄中的相符記錄時`UserProfiles`和`aspnet_Users`。 以及我們了解在步驟 3 中，如果使用者沒有記錄`UserProfiles`她無法檢視或編輯其設定在`AdditionalUserInfo.aspx`頁面。

不用說，因為我們的設計決策很重要的成員資格系統中每個使用者帳戶沒有相對應的記錄中`UserProfiles`資料表。 我們想要為對應的記錄加入至`UserProfiles`透過適用於 CreateUserWizard 每次建立新的成員資格使用者帳戶時。

中所述[*建立使用者帳戶*](creating-user-accounts-vb.md)教學課程中，新的成員資格使用者帳戶建立適用於 CreateUserWizard 控制項之後會引發其[`CreatedUser`事件](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). 我們可以建立此事件的事件處理常式為剛建立的使用者，取得使用者識別碼，然後插入一筆記錄，到`UserProfiles`針對使用預設值的資料表`HomeTown`， `HomepageUrl`，和`Signature`資料行。 不僅如此，很可能透過自訂適用於 CreateUserWizard 控制項的介面，以加入其他文字方塊會提示使用者輸入這些值。

讓我們先看看如何加入新的資料列`UserProfiles`資料表中`CreatedUser`事件處理常式和預設值。 接下來，我們會了解如何自訂適用於 CreateUserWizard 控制項的使用者介面，以包含新使用者的家用城鎮、 首頁和簽章所收集的其他表單欄位。

### <a name="adding-a-default-row-touserprofiles"></a>加入到預設資料列`UserProfiles`

在[*建立使用者帳戶*](creating-user-accounts-vb.md)教學課程中我們加入了適用於 CreateUserWizard 控制項`CreatingUserAccounts.aspx`頁面`Membership`資料夾。 為了有適用於 CreateUserWizard 控制項將記錄新增至`UserProfiles`資料表時建立使用者帳戶，我們需要更新適用於 CreateUserWizard 控制項的功能。 而不是執行這些變更`CreatingUserAccounts.aspx`頁面上，讓我們改為加入新的適用於 CreateUserWizard 控制項以`EnhancedCreateUserWizard.aspx`頁面上，並在此教學課程那里進行修改。

開啟`EnhancedCreateUserWizard.aspx`Visual Studio 中的頁面上，並從 [工具箱] 拖曳至網頁拖曳適用於 CreateUserWizard 控制項。 設定適用於 CreateUserWizard 控制項`ID`屬性`NewUserWizard`。 如我們所述[*建立使用者帳戶*](creating-user-accounts-vb.md)教學課程，適用於 CreateUserWizard 的預設使用者介面提示的訪客所需的資訊。 已提供這項資訊，控制項在內部會建立新的使用者帳戶在成員資格 framework 中，而不需我們需要撰寫一行程式碼。

適用於 CreateUserWizard 控制項在其工作流程期間引發的事件數目。 適用於 CreateUserWizard 控制項訪客提供要求資訊並提交表單之後，一開始會引發其[`CreatingUser`事件](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx)。 如果在建立過程中，問題[`CreateUserError`事件](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx)引發; 不過，如果已成功建立使用者，然後在[`CreatedUser`事件](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx)，就會引發。 在[*建立使用者帳戶*](creating-user-accounts-vb.md)教學課程中我們建立的事件處理常式`CreatingUser`事件以確保提供的使用者名稱未包含任何開頭或尾端空格，而且，使用者名稱中沒有出現任何位置的密碼。

若要加入的資料列中`UserProfiles`資料表剛建立的使用者，我們需要建立事件處理常式`CreatedUser`事件。 依時間`CreatedUser`引發事件，使用者帳戶已建立在成員資格 framework 中，讓我們能夠擷取帳戶的使用者識別碼值。

建立事件處理常式`NewUserWizard`的`CreatedUser`事件並加入下列程式碼：

[!code-vb[Main](storing-additional-user-information-vb/samples/sample11.vb)]

上述程式碼身藉由擷取剛加入的使用者帳戶的使用者識別碼。 這會透過使用`Membership.GetUser(username)`方法以傳回特定的使用者，然後將相關的資訊`ProviderUserKey`屬性，以擷取其使用者識別碼。 適用於 CreateUserWizard 控制項中的使用者所輸入的使用者名稱是可透過其[`UserName`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.username.aspx)。

接下來，從擷取連接字串`Web.config`和`INSERT`指定陳述式。 必要的 ADO.NET 物件具現化並執行命令。 程式碼指派[ `DBNull` ](https://msdn.microsoft.com/en-us/library/system.dbnull.aspx)執行個體`@HomeTown`， `@HomepageUrl`，和`@Signature`參數，已插入資料庫的效果`NULL`值`HomeTown`， `HomepageUrl`，和`Signature`欄位。

請瀏覽`EnhancedCreateUserWizard.aspx`頁面上透過瀏覽器，並建立新的使用者帳戶。 之後，請返回 Visual Studio，並檢查內容`aspnet_Users`和`UserProfiles`資料表 （如同我們在圖 12）。 您應該會看到新的使用者帳戶在`aspnet_Users`和對應`UserProfiles`資料列 (與`NULL`值`HomeTown`， `HomepageUrl`，和`Signature`)。


[![已加入新的使用者帳戶和 UserProfiles 記錄](storing-additional-user-information-vb/_static/image59.png)](storing-additional-user-information-vb/_static/image58.png)

**圖 20**: 新的使用者帳戶和`UserProfiles`已加入資料錄 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image60.png))


訪客具有提供其新的帳戶資訊，並按一下 [建立使用者] 按鈕，建立使用者帳戶，並加入一個資料列之後`UserProfiles`資料表。 適用於 CreateUserWizard 接著會顯示其`CompleteWizardStep`，其中顯示成功訊息並繼續 按鈕。 按一下 [繼續] 按鈕會導致回傳，但未採取任何動作，而讓使用者卡上`EnhancedCreateUserWizard.aspx`頁面。

我們可以指定要將使用者傳送至適用於 CreateUserWizard 控制項透過按一下 [繼續] 按鈕時的 URL [ `ContinueDestinationPageUrl`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx)。 設定`ContinueDestinationPageUrl`屬性為"~ / Membership/AdditionalUserInfo.aspx"。 這會帶到新的使用者`AdditionalUserInfo.aspx`，其中也可以檢視和更新其設定。

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>自訂適用於 CreateUserWizard 的介面，以提示輸入新使用者的家用城鎮、 首頁及簽章

適用於 CreateUserWizard 控制項的預設介面便足以讓簡單帳戶建立案例需要收集只有核心使用者帳戶資訊，例如使用者名稱、 密碼及電子郵件的位置。 但我們想要提示的訪客建立她的帳戶時輸入她家用城鎮、 首頁和簽章的該怎麼辦？ 便可自訂適用於 CreateUserWizard 控制項的介面，以收集其他資訊在註冊期間取得，而這項資訊可能用於`CreatedUser`基礎資料庫中插入其他記錄的事件處理常式。

適用於 CreateUserWizard 控制項擴充 ASP.NET 精靈控制項的控制項，可讓網頁開發人員定義一系列的排序`WizardSteps`。 精靈控制項轉譯使用中的步驟，並提供瀏覽介面，可將這些步驟的訪客。 精靈控制項非常適用於較長的工作分成幾個簡短步驟。 如需有關精靈控制項的詳細資訊，請參閱[逐步使用者介面建立與 ASP.NET 2.0 精靈控制項](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)。

適用於 CreateUserWizard 控制項的預設標記會定義兩個`WizardSteps`:`CreateUserWizardStep`和`CompleteWizardStep`。

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample12.aspx)]

第一個`WizardStep`， `CreateUserWizardStep`，呈現提示輸入使用者名稱、 密碼、 電子郵件、 等等的介面。 訪客提供這項資訊，並按一下 建立使用者 之後，她會顯示`CompleteWizardStep`，其中顯示成功訊息並繼續 按鈕。

若要自訂適用於 CreateUserWizard 控制項的介面，以加入其他表單欄位，我們可以：

- **建立新的一或多個 * * *`WizardStep`* * * 以包含額外的使用者介面項目**。 若要將新`WizardStep`至適用於 CreateUserWizard，按一下 「 新增/移除`WizardStep`s 」 連結來啟動其智慧標籤`WizardStep`集合編輯器。 您可以從該處加入、 移除或重新排序精靈中的步驟。 這是我們將使用此教學課程中的方法。

- **轉換 * * *`CreateUserWizardStep`* * * 的可編輯成 * * *`WizardStep`* * *。** 這會取代`CreateUserWizardStep`等同`WizardStep`其標記定義比對的使用者介面`CreateUserWizardStep`' s。 藉由轉換`CreateUserWizardStep`到`WizardStep`我們可以重新調整控制項位置，或將額外的使用者介面項目加入至這個步驟。 要轉換`CreateUserWizardStep`或`CompleteWizardStep`成可編輯`WizardStep`、 按一下 自訂建立使用者步驟 」 或 「 自訂完成逐步 」 連結從控制項的智慧標籤。

- **使用上述兩個選項的一些組合。**

適用於 CreateUserWizard 控制項在中按一下 [建立使用者] 按鈕時，會執行其使用者帳戶建立程序是一個重要的事情，記住其`CreateUserWizardStep`。 不論是否有其他`WizardStep`之後 s`CreateUserWizardStep`與否。

當加入自訂`WizardStep`至適用於 CreateUserWizard 控制項，以收集其他的使用者輸入，自訂`WizardStep`放之前或之後`CreateUserWizardStep`。 如果它前面`CreateUserWizardStep`額外的使用者輸入再收集來自自訂`WizardStep`供`CreatedUser`事件處理常式。 不過，如果自訂`WizardStep`之後`CreateUserWizardStep`時再以自訂`WizardStep`會顯示已建立新的使用者帳戶和`CreatedUser`已經引發事件。

圖 21 顯示工作流程時加入`WizardStep`之前`CreateUserWizardStep`。 因為已收集的其他使用者資訊的時間`CreatedUser`事件引發，我們只需要為 update`CreatedUser`事件處理常式，以擷取這些輸入使用的`INSERT`陳述式的參數值 （而非`DBNull.Value`).


[![其他的 WizardStep 之前 CreateUserWizardStep 時適用於 CreateUserWizard 工作流程](storing-additional-user-information-vb/_static/image62.png)](storing-additional-user-information-vb/_static/image61.png)

**圖 21**: 適用於 CreateUserWizard 工作流程時其他`WizardStep`Precedes `CreateUserWizardStep` ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image63.png))


如果自訂`WizardStep`放置*之後* `CreateUserWizardStep`，不過，建立使用者帳戶的處理程序，就會發生之前的使用者有機會來輸入她家用城鎮、 首頁或簽章。 在這種情況下，這項額外資訊必須要插入至資料庫之後建立的使用者帳戶，如圖 22 所示。


[![其他的 WizardStep 出現後 CreateUserWizardStep 時適用於 CreateUserWizard 工作流程](storing-additional-user-information-vb/_static/image65.png)](storing-additional-user-information-vb/_static/image64.png)

**圖 22**: 適用於 CreateUserWizard 工作流程時其他`WizardStep`出現之後`CreateUserWizardStep`([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image66.png))


22 圖所示的工作流程等候插入到資料錄`UserProfiles`步驟 2 完成後，直到資料表。 若造訪者在步驟 1 之後關閉瀏覽器，不過，我們將達到的狀態，其中已建立的使用者帳戶，但沒有記錄已加入至`UserProfiles`。 解決方法之一是讓資料錄`NULL`或預設值插入至`UserProfiles`中`CreatedUser`（可在步驟 1 之後引發） 事件處理常式，並更新步驟 2 完成後，此記錄。 如此可確保`UserProfiles`記錄將會新增使用者帳戶，即使在使用者結束註冊程序中途島，透過。

此教學課程中我們來建立新`WizardStep`後出現`CreateUserWizardStep`前`CompleteWizardStep`。 讓我們先取得中的 WizardStep 放置和設定，然後我們會查看程式碼。

從適用於 CreateUserWizard 控制項的智慧標籤上，選取 「 新增/移除`WizardStep`s"，它會啟動`WizardStep`集合編輯器對話方塊。 加入新`WizardStep`，設定其`ID`至`UserSettings`、 其`Title`到 [您的設定] 並將其`StepType`至`Step`。 然後將它定位，讓它出現後`CreateUserWizardStep`（「 登入您的新帳戶的"），以及之前`CompleteWizardStep`（「 完成 」），如圖 23 所示。


[![適用於 CreateUserWizard 控制項中加入新的 WizardStep](storing-additional-user-information-vb/_static/image68.png)](storing-additional-user-information-vb/_static/image67.png)

**圖 23**： 加入新`WizardStep`適用於 CreateUserWizard 控制項 ([按一下以檢視完整大小的影像](storing-additional-user-information-vb/_static/image69.png))


按一下 [確定] 關閉`WizardStep`集合編輯器對話方塊。 新`WizardStep`總合由適用於 CreateUserWizard 控制項的更新的宣告式標記：

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample13.aspx)]

請注意新`<asp:WizardStep>`項目。 我們需要加入使用者介面，以收集新使用者的家用城鎮、 首頁上，與此簽章。 宣告式語法中，或透過設計工具，您可以輸入此內容。 若要使用設計工具，請從下拉式清單，請參閱 < 設計工具中的步驟在智慧標籤在選取 「 您的設定 」 步驟。

> [!NOTE]
> 選取透過智慧標籤的下拉式清單中的步驟更新適用於 CreateUserWizard 控制項[`ActiveStepIndex`屬性](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx)，以指定的索引開始的步驟。 因此，如果您使用此下拉式清單來編輯設計工具中的 「 您的設定 」 步驟，請確定服務設回 「 登註冊您新帳戶 」，讓使用者第一次瀏覽時顯示此步驟`EnhancedCreateUserWizard.aspx`頁面。


建立使用者介面包含三個文字方塊控制項，名為 「 您的設定 」 步驟內`HomeTown`， `HomepageUrl`，和`Signature`。 之後建構這個介面時，適用於 CreateUserWizard 的宣告式標記看起來應該如下所示：

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample14.aspx)]

請繼續並造訪此頁透過瀏覽器，並建立新的使用者帳戶，指定主城鎮、 首頁和簽章的值。 完成之後`CreateUserWizardStep`成員資格架構中建立使用者帳戶和`CreatedUser`事件處理常式執行時，這會將新的資料列`UserProfiles`，但資料庫`NULL`值`HomeTown`， `HomepageUrl`，和`Signature`. 永遠不會使用家用城鎮、 首頁和簽章的輸入值。 最後結果就是新的使用者帳戶`UserProfiles`記錄其`HomeTown`， `HomepageUrl`，和`Signature`欄位尚未指定。

我們要在可接受使用者輸入的家用城鎮、 honepage 和簽章值，且更新適當的 「 您的設定 」 步驟之後執行程式碼`UserProfiles`記錄。 每次使用者在精靈中的步驟之間移動控制項，精靈的[`ActiveStepChanged`事件](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx)引發。 我們可以建立此事件和更新的事件處理常式`UserProfiles`資料表時已完成 「 您的設定 」 步驟。

加入事件處理常式適用於 CreateUserWizard`ActiveStepChanged`事件並加入下列程式碼：

[!code-vb[Main](storing-additional-user-information-vb/samples/sample15.vb)]

上述程式碼一開始會判斷如果我們已到達 [完成] 步驟。 因為立即在 [您的設定] 步驟之後，就會發生 「 完成 」 步驟，然後訪客達到 「 完成 」 逐步表示她剛剛完成 「 您的設定 」 步驟。

在這種情況下，我們需要以程式設計方式參考文字方塊控制項內`UserSettings WizardStep`。 做法是先使用`FindControl`方法來以程式設計方式參考`UserSettings WizardStep`，然後再一次參考文字方塊內`WizardStep`。 只要參考的文字方塊，我們可以開始執行`UPDATE`陳述式。 `UPDATE`陳述式有相同數目的參數做為`INSERT`陳述式中的`CreatedUser`事件處理常式，但我們這裡使用的使用者所提供的家用城鎮、 首頁和簽章值。

與就地此事件處理常式，請瀏覽`EnhancedCreateUserWizard.aspx`頁面上透過瀏覽器，並建立新的使用者帳戶指定主城鎮、 首頁和簽章的值。 建立新帳戶後您應該會重新導向至`AdditionalUserInfo.aspx`頁面上，其中只輸入家用城鎮、 首頁和簽章會顯示的資訊。

> [!NOTE]
> 我們的網站目前有兩個頁面的訪客可以從中建立新的帳戶：`CreatingUserAccounts.aspx`和`EnhancedCreateUserWizard.aspx`。 網站的 sitemap 和登入頁面指向`CreatingUserAccounts.aspx` 頁面上，但`CreatingUserAccounts.aspx`頁面不會提示使用者輸入其主城鎮、 首頁和簽章資訊，而且不加入至對應的資料列`UserProfiles`。 因此，更新`CreatingUserAccounts.aspx`頁面，如此它提供這項功能或更新的 sitemap 與登入頁面，即可參考`EnhancedCreateUserWizard.aspx`而不是`CreatingUserAccounts.aspx`。 如果您選擇第二種選項，請務必更新`Membership`資料夾的`Web.config`檔案，以允許匿名使用者存取`EnhancedCreateUserWizard.aspx`頁面。


## <a name="summary"></a>總結

在此教學課程中我們探討了成員資格架構中的使用者帳戶相關的資料模型化的技術。 特別是，我們會探討模型與使用者帳戶，以及共用一對一關聯性的資料共用一個對多關聯性的實體。 此外，我們可了解這與資訊無法顯示、 插入和更新，透過使用 SqlDataSource 控制項和其他一些範例使用 ADO.NET 程式碼。

本教學課程完成我們查看使用者帳戶。 開頭為下一個教學課程中我們將會開啟我們注意到角色。 透過下幾個教學課程，我們將探討角色架構，請參閱 < 如何建立新的角色，如何將角色指派給使用者，來判斷哪些角色在使用者屬於，以及如何套用以角色為基礎的授權。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [存取及更新 ASP.NET 2.0 中的資料](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [ASP.NET 2.0 精靈控制項](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [建立與 ASP.NET 2.0 精靈控制項的逐步使用者介面](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [建立自訂的資料來源控制項參數](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [自訂適用於 CreateUserWizard 控制項](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [在 DetailsView 控制項快速入門](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [利用 ListView 控制項顯示資料](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [將 ASP.NET 2.0 中的驗證控制項的分解](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [編輯插入和刪除資料](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [在 ASP.NET 表單驗證](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [正在蒐集的自訂使用者註冊資訊](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [在 ASP.NET 2.0 設定檔](http://www.odetocode.com/Articles/440.aspx)
- [Asp: ListView 控制項](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [使用者設定檔快速入門](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP/ASP.NET 書籍的作者和創辦的 4GuysFromRolla.com，具有已經使用 Microsoft Web 技術從 1998 年。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿 *[Sam 教導您自己 ASP.NET 2.0 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 在可到達 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過在他的部落格[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝...

許多有用的檢閱者已檢閱本教學課程系列。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

>[!div class="step-by-step"]
[上一步](user-based-authorization-vb.md)
