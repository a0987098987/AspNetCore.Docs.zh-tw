---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
title: "在 SQL Server (VB) 中建立成員資格結構描述 |Microsoft 文件"
author: rick-anderson
description: "本教學課程一開始會檢查必要的結構描述加入資料庫，才能使用 SqlMembershipProvider 的技術。 接下來，我們 wi-fi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 112a674d-716f-41a6-99b8-4074d65a54c0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 181741dc7e0fb7e1073f3783d96f59ac905f5e63
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-the-membership-schema-in-sql-server-vb"></a>在 SQL Server (VB) 中建立成員資格結構描述
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_VB.zip)或[下載 PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_vb.pdf)

> 本教學課程一開始會檢查必要的結構描述加入資料庫，才能使用 SqlMembershipProvider 的技術。 接下來，我們會檢查結構描述中的索引鍵的資料表，並討論其用途和重要性。 本教學課程結尾看看如何判斷 ASP.NET 應用程式成員資格架構應該使用哪一個提供者。


## <a name="introduction"></a>簡介

檢查使用表單驗證來識別網站訪客先前兩個教學課程。 表單驗證架構可讓開發人員將使用者登入網站，並透過使用驗證票證的網頁瀏覽跨記得輕鬆。 `FormsAuthentication`類別包含產生票證，並將其加入訪客的 cookie 的方法。 `FormsAuthenticationModule`會檢查所有連入要求，對於具有有效的驗證票證，建立並將相關聯`GenericPrincipal`和`FormsIdentity`與目前要求的物件。 表單驗證是只是一種機制訪客的驗證票證授與登入中，然後在後續要求中，剖析以判斷使用者的身分識別的票證時。 若要支援使用者帳戶是 web 應用程式，我們仍需要實作使用者存放區，並將功能加入至驗證認證，註冊新的使用者和其他使用者帳戶相關工作的相關。

之前 ASP.NET 2.0 中，開發人員所實作的所有這些使用者帳戶相關工作的勾點上。 幸運的是 ASP.NET 團隊辨識此缺點，並導入的成員資格架構與 ASP.NET 2.0。 成員資格架構是一組.NET Framework 中提供完成核心使用者帳戶相關工作的程式設計介面的類別。 此架構建置在之上[提供者模型](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx)，可讓開發人員的自訂的實作插入標準化的 API。

中所述<a id="Tutorial1"> </a> [*安全性基本概念和 ASP.NET 支援*](../introduction/security-basics-and-asp-net-support-vb.md)教學課程中，.NET Framework 隨附兩個內建的成員資格提供者： [ `ActiveDirectoryMembershipProvider` ](https://msdn.microsoft.com/en-us/library/system.web.security.activedirectorymembershipprovider.aspx)和[ `SqlMembershipProvider` ](https://msdn.microsoft.com/en-us/library/system.web.security.sqlmembershipprovider.aspx)。 正如其名，`SqlMembershipProvider`做為使用者存放區會使用 Microsoft SQL Server 資料庫。 若要使用此提供者應用程式中，我們需要通知哪些資料庫做為存放區提供者。 您可以想像，`SqlMembershipProvider`預期要有特定的資料庫資料表、 檢視和預存程序的使用者存放區資料庫。 我們需要將這個預期的結構描述加入至選取的資料庫。

本教學課程一開始會檢查必要的結構描述加入資料庫，若要使用的技術`SqlMembershipProvider`。 接下來，我們會檢查結構描述中的索引鍵的資料表，並討論其用途和重要性。 本教學課程結尾看看如何判斷 ASP.NET 應用程式成員資格架構應該使用哪一個提供者。

讓我們開始吧 ！

## <a name="step-1-deciding-where-to-place-the-user-store"></a>步驟 1： 決定放置在使用者存放區位置

ASP.NET 應用程式的資料通常儲存在資料庫中的資料表數目。 當實作`SqlMembershipProvider`我們必須決定是否要將成員資格結構描述放在相同的應用程式資料的資料庫或替代資料庫的資料庫結構描述。

我建議您為應用程式資料在相同資料庫中尋找的成員資格結構描述，原因如下：

- **維護性**其資料會封裝在一個資料庫的應用程式是容易了解、 維護和部署於應用程式有兩個不同的資料庫。
- **關聯式完整性**尋找相同資料庫中的成員資格相關的資料表，為應用程式資料表它仍可建立[foreign key 條件約束](http://en.wikipedia.org/wiki/Foreign_key)之間的主索引鍵成員資格相關的資料表和相關的應用程式資料表。

使用者存放區和應用程式資料分離到不同的資料庫才有意義如果您有多個應用程式，每個使用不同的資料庫，但需要共用一般的使用者存放區。

### <a name="creating-a-database"></a>建立資料庫

我們已自第二個教學課程建置的應用程式尚未不需要資料庫。 我們需要一個現在，不過，在使用者存放區。 讓我們建立一個，然後為其新增所需的結構描述`SqlMembershipProvider`提供者 （請參閱步驟 2）。

> [!NOTE]
> 在此教學課程系列，我們將使用[Microsoft SQL Server 2005 Express 的 Edition](https://msdn.microsoft.com/en-us/sql/Aa336346.aspx)資料庫來儲存應用程式資料表和`SqlMembershipProvider`結構描述。 此決策的原因有二： 第一次，因為它的成本-免費的 Express Edition 是最 readably 存取版本的 SQL Server 2005;第二，SQL Server 2005 Express Edition 資料庫可以置於直接 web 應用程式的`App_Data`資料夾，讓您封裝資料庫和 web 應用程式一起在一個 ZIP 檔案，並沒有任何特殊的安裝指示來重新部署它很簡單或組態選項。 如果您想要跟著做使用非-Express Edition 的 SQL Server 版本，則可以自由。 會幾乎完全相同的步驟。 `SqlMembershipProvider`結構描述會使用任何版本的 Microsoft SQL Server 2000 及最多。

從 方案總管 中，以滑鼠右鍵按一下`App_Data`資料夾，然後選擇 加入新項目。 (如果您沒有看到`App_Data`資料夾在專案中，以滑鼠右鍵按一下方案總管] 中的專案上，選取加入 ASP.NET 資料夾，然後挑選`App_Data`。)從 [加入新項目] 對話方塊中，選擇 [加入新的 SQL 資料庫名為`SecurityTutorials.mdf`。 在本教學課程中我們將`SqlMembershipProvider`至此資料庫，則在後續教學課程中我們將建立其他的結構描述來擷取應用程式資料的資料表。


[![加入新的 SQL 資料庫名為 [App_Data] 資料夾 SecurityTutorials.mdf 資料庫](creating-the-membership-schema-in-sql-server-vb/_static/image2.png)](creating-the-membership-schema-in-sql-server-vb/_static/image1.png)

**圖 1**： 加入新的 SQL 資料庫 Named`SecurityTutorials.mdf`資料庫`App_Data`資料夾 ([按一下以檢視完整大小的影像](creating-the-membership-schema-in-sql-server-vb/_static/image3.png))


加入至資料庫`App_Data`資料夾會自動包含它在 [資料庫總管] 檢視中。 （在非-Express Edition 版本的 Visual Studio 中，[資料庫總管] 中稱為 [伺服器總管] 中。）移至 [資料庫總管] 並展開剛加入`SecurityTutorials`資料庫。 如果您看不 [資料庫總管] 畫面上，移至 [檢視] 功能表並選擇 [資料庫總管] 中，或按 Ctrl + Alt + S。 如圖 2 所示，`SecurityTutorials`資料庫是空的-它包含沒有任何資料表、 檢視和任何預存程序。


[![SecurityTutorials 資料庫目前是空的](creating-the-membership-schema-in-sql-server-vb/_static/image5.png)](creating-the-membership-schema-in-sql-server-vb/_static/image4.png)

**圖 2**:`SecurityTutorials`資料庫目前是空的 ([按一下以檢視完整大小的影像](creating-the-membership-schema-in-sql-server-vb/_static/image6.png))


## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>步驟 2： 加入`SqlMembershipProvider`資料庫結構描述

`SqlMembershipProvider`需要一組特定的資料表、 檢視和預存程序來安裝在使用者存放區資料庫中。 這些必要的資料庫物件可以使用加入[`aspnet_regsql.exe`工具](https://msdn.microsoft.com/en-us/library/ms229862.aspx)。 此檔案位於`%WINDIR%\Microsoft.Net\Framework\v2.0.50727\`資料夾。

> [!NOTE]
> `aspnet_regsql.exe`工具提供圖形化使用者介面和命令列功能。 是易記的多個使用者，我們將在本教學課程檢視圖形化介面。 命令列介面狀況中很有用的加法`SqlMembershipProvider`結構描述必須為自動化，例如組建指令碼或自動化測試案例。

`aspnet_regsql.exe`工具可用來新增或移除*ASP.NET 應用程式服務*到指定的 SQL Server 資料庫。 ASP.NET 應用程式服務包含的結構描述`SqlMembershipProvider`和`SqlRoleProvider`，以及其他 ASP.NET 2.0 架構的 SQL 為基礎提供者的結構描述。 我們需要提供資訊的兩個位元`aspnet_regsql.exe`工具：

- 是否要加入或移除應用程式服務和
- 要加入或移除應用程式服務結構描述資料庫

在資料庫使用時，提示您輸入`aspnet_regsql.exe`工具會詢問一些我們提供的安全性認證，連接到資料庫的資料庫所在的伺服器名稱和資料庫名稱。 如果您使用非 Express Edition 的 SQL Server，您應該已經知道這項資訊，因為它是使用透過 ASP.NET 網頁上的資料庫時，您必須提供連接字串透過相同的資訊。 使用中的 SQL Server 2005 Express Edition 資料庫時，判斷伺服器和資料庫名稱`App_Data`資料夾中，不過，會稍微更為複雜。

下一節會檢查指定的伺服器和資料庫名稱中的 SQL Server 2005 Express Edition 資料庫最直接的方式`App_Data`資料夾。 如果您不想要使用 SQL Server 2005 Express Edition 可自由安裝，請直接前往 [應用程式服務] 區段。

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theappdatafolder"></a>判斷伺服器和資料庫名稱中的 SQL Server 2005 Express Edition 資料庫`App_Data`資料夾

若要使用`aspnet_regsql.exe`我們需要知道的伺服器和資料庫名稱的工具。 伺服器名稱是`localhost\InstanceName`。 最有可能*InstanceName*是`SQLExpress`。 不過，如果您手動安裝 SQL Server 2005 Express Edition （也就是您未安裝它自動安裝 Visual Studio 時），則很可能您選取不同的執行個體名稱。

資料庫名稱是以判斷需要一點的技巧。 中的資料庫`App_Data`資料夾通常會有一個包含的資料庫名稱[全域唯一識別碼](http://en.wikipedia.org/wiki/Globally_Unique_Identifier)以及資料庫檔案的路徑。 我們需要決定此資料庫名稱以加入應用程式服務結構描述，透過`aspnet_regsql.exe`。

確定資料庫名稱的最簡單方式是檢查透過 SQL Server Management Studio。 SQL Server Management Studio 提供圖形化介面，以管理 SQL Server 2005 資料庫，但它並未隨附 Express Edition 的 SQL Server 2005。 好消息是[可以下載](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)免費 Express Edition 的 SQL Server Management Studio。

> [!NOTE]
> 如果您也可以安裝在桌面上，則可能已安裝 Management Studio 的完整版本的 SQL Server 2005 的非-Express Edition 版本。 若要判斷資料庫名稱，遵循相同的步驟，如下所述的 Express Edition，您可以使用完整版。

開始關閉 Visual Studio 以確定資料庫檔案上的 Visual Studio 所加諸任何鎖定，會關閉。 接下來，啟動 SQL Server Management Studio 並連接到`localhost\InstanceName`適用於 SQL Server 2005 Express Edition 資料庫。 如前文所述，機率是執行個體名稱是`SQLExpress`。 針對 [驗證] 選項中，選取 Windows 驗證。


[![連接到 SQL Server 2005 Express Edition 執行個體](creating-the-membership-schema-in-sql-server-vb/_static/image8.png)](creating-the-membership-schema-in-sql-server-vb/_static/image7.png)

**圖 3**： 連接到 SQL Server 2005 Express Edition 執行個體 ([按一下以檢視完整大小的影像](creating-the-membership-schema-in-sql-server-vb/_static/image9.png))


連接到 SQL Server 2005 Express Edition 執行個體之後，Management Studio 會顯示資料庫、 安全性設定、 伺服器物件和等等的資料夾。 如果您展開 [資料庫] 索引標籤您會看到`SecurityTutorials.mdf`資料庫是*不*登錄的資料庫執行個體-我們需要將資料庫附加到第一次。

以滑鼠右鍵按一下 資料庫 資料夾，並從內容功能表中選擇 附加。 這會顯示 [附加資料庫] 對話方塊。 從這裡開始，請按一下 [新增] 按鈕，瀏覽至`SecurityTutorials.mdf`資料庫，然後按一下 [確定]。 圖 4 顯示在 [附加資料庫] 對話方塊中之後,`SecurityTutorials.mdf`選取資料庫。 圖 5 顯示 Management Studio 物件總管 中，成功地附加資料庫之後。


[![附加 SecurityTutorials.mdf 資料庫](creating-the-membership-schema-in-sql-server-vb/_static/image11.png)](creating-the-membership-schema-in-sql-server-vb/_static/image10.png)

**圖 4**： 附加`SecurityTutorials.mdf`資料庫 ([按一下以檢視完整大小的影像](creating-the-membership-schema-in-sql-server-vb/_static/image12.png))


[![SecurityTutorials.mdf 資料庫會出現在 [資料庫] 資料夾](creating-the-membership-schema-in-sql-server-vb/_static/image14.png)](creating-the-membership-schema-in-sql-server-vb/_static/image13.png)

**圖 5**:`SecurityTutorials.mdf`資料庫出現在 [資料庫] 資料夾中 ([按一下以檢視完整大小的影像](creating-the-membership-schema-in-sql-server-vb/_static/image15.png))


如圖 5 所示，`SecurityTutorials.mdf`資料庫有而 abstruse 的名稱。 以更好記 （和比較好輸入） 讓我們來變更名稱。 以滑鼠右鍵按一下資料庫、 從內容功能表中，選擇 重新命名和重新命名`SecurityTutorialsDatabase`。 這不會變更檔案名稱，只是名稱的資料庫使用的 SQL server 識別。


[![將資料庫重新命名為 SecurityTutorialsDatabase](creating-the-membership-schema-in-sql-server-vb/_static/image17.png)](creating-the-membership-schema-in-sql-server-vb/_static/image16.png)

**圖 6**： 資料庫重新命名為`SecurityTutorialsDatabase`([按一下以檢視完整大小的影像](creating-the-membership-schema-in-sql-server-vb/_static/image18.png))


此時我們知道的伺服器和資料庫名稱`SecurityTutorials.mdf`資料庫檔案：`localhost\InstanceName`和`SecurityTutorialsDatabase`分別。 我們現在已經準備好安裝應用程式服務，透過`aspnet_regsql.exe`工具。

### <a name="installing-the-application-services"></a>安裝應用程式服務

若要啟動`aspnet_regsql.exe`工具，請移至 [開始] 功能表，並選擇 [執行]。 輸入`%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` 文字方塊中按一下 確定。 或者，您可以使用 Windows 檔案總管 向下鑽研至適當的資料夾，然後按兩下`aspnet_regsql.exe`檔案。 兩種方法將 net 相同的結果。

執行`aspnet_regsql.exe`工具沒有任何命令列引數會啟動 ASP.NET SQL Server 安裝精靈圖形化使用者介面。 精靈可讓您輕鬆地新增或移除指定的資料庫上的 ASP.NET 應用程式服務。 圖 7 所示在精靈的第一個畫面會描述工具的用途。


[![若要加入的成員資格結構描述中使用 ASP.NET SQL Server 安裝精靈可使](creating-the-membership-schema-in-sql-server-vb/_static/image20.png)](creating-the-membership-schema-in-sql-server-vb/_static/image19.png)

**圖 7**： 使用 ASP.NET SQL Server 安裝精靈就會加入成員資格結構描述 ([按一下以檢視完整大小的影像](creating-the-membership-schema-in-sql-server-vb/_static/image21.png))


精靈中的第二個步驟會詢問我們是否我們要加入應用程式服務，或將它們移除。 因為我們想要加入資料表、 檢視和預存程序所需的`SqlMembershipProvider`，選擇 [設定 SQL Server 的應用程式服務] 選項。 稍後，如果您想要從資料庫中移除這個結構描述，重新執行此精靈中，但改為從現有的資料庫選項中選擇 移除應用程式服務資訊。


[![選擇應用程式服務選項設定 SQL Server](creating-the-membership-schema-in-sql-server-vb/_static/image23.png)](creating-the-membership-schema-in-sql-server-vb/_static/image22.png)

**圖 8**： 為應用程式服務] 選項中選擇 [設定 SQL Server ([按一下以檢視完整大小的影像](creating-the-membership-schema-in-sql-server-vb/_static/image24.png))


第三個步驟提示的資料庫資訊： 伺服器名稱、 驗證資訊，以及資料庫名稱。 如果您已將其遵循本教學課程，並已加入`SecurityTutorials.mdf`資料庫`App_Data`，附加至`localhost\InstanceName`，和它重新命名為`SecurityTutorialsDatabase`，然後使用下列值：

- 伺服器：`localhost\InstanceName`
- Windows 驗證
- 資料庫：`SecurityTutorialsDatabase`


[![輸入資料庫的資訊](creating-the-membership-schema-in-sql-server-vb/_static/image26.png)](creating-the-membership-schema-in-sql-server-vb/_static/image25.png)

**圖 9**： 輸入資料庫的資訊 ([按一下以檢視完整大小的影像](creating-the-membership-schema-in-sql-server-vb/_static/image27.png))


輸入資料庫資訊後，按 下一步。 最後一個步驟摘要說明將採取的步驟。 若要安裝的應用程式服務，然後在完成精靈，請按下一步。

> [!NOTE]
> 如果您使用 Management Studio 來附加資料庫，並重新命名資料庫檔案時，務必卸離資料庫，然後關閉再重新開啟 Visual Studio 的 Management Studio。 若要卸離`SecurityTutorialsDatabase`資料庫、 以滑鼠右鍵按一下資料庫名稱，從 工作 功能表中，選擇 卸離。

完成精靈的詳細資訊，返回 Visual Studio 和瀏覽至 [資料庫總管] 中。 展開 [資料表] 資料夾。 您應該會看到一系列的資料表名稱開頭為前置詞`aspnet_`。 同樣地，可以檢視和預存程序資料夾下找到各種不同的檢視和預存程序。 這些資料庫物件組成的應用程式服務結構描述。 我們會檢查在步驟 3 中的成員資格和角色特定資料庫物件。


[![資料庫已加入各種不同的資料表、 檢視和預存程序](creating-the-membership-schema-in-sql-server-vb/_static/image29.png)](creating-the-membership-schema-in-sql-server-vb/_static/image28.png)

**圖 10**: 各種不同的資料表、 檢視和預存程序已新增至資料庫 ([按一下以檢視完整大小的影像](creating-the-membership-schema-in-sql-server-vb/_static/image30.png))


> [!NOTE]
> `aspnet_regsql.exe`工具的圖形化使用者介面安裝整個應用程式服務結構描述。 執行時，但`aspnet_regsql.exe`從命令列您可以指定哪些特定的應用程式服務元件安裝 （或移除）。 因此，如果您想要加入只在資料表、 檢視，和預存程序所需的`SqlMembershipProvider`和`SqlRoleProvider`提供者，執行`aspnet_regsql.exe`從命令列。 或者，您可以手動執行適當的 T-SQL 子集建立所使用的指令碼`aspnet_regsql.exe`。 這些指令碼位於`WINDIR%\Microsoft.Net\Framework\v2.0.50727\`資料夾的名稱可能類似`InstallCommon.sql`， `InstallMembership.sql`， `InstallRoles.sql`， `InstallProfile.sql`， `InstallSqlState.sql`，依此類推。

此時，我們也可以建立所需的資料庫物件`SqlMembershipProvider`。 不過，我們還是需要以指示應該使用的成員資格 framework `SqlMembershipProvider` (versus、 說， `ActiveDirectoryMembershipProvider`) 而且`SqlMembershipProvider`應該使用`SecurityTutorials`資料庫。 我們會探討如何指定要使用何種提供者，以及如何自訂在步驟 4 中選取的提供者設定。 但首先，我們深入探討剛才所建立的資料庫物件。

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>步驟 3： 在結構描述的核心資料表查詢

當使用 ASP.NET 應用程式中的成員資格和角色架構，提供者所封裝的實作詳細資料。 在未來我們將這些透過.NET Framework 架構與互動的教學課程`Membership`和`Roles`類別。 使用這些高層級 Api 時我們不需要考量自己有低層級的詳細資料，例如何種查詢執行或修改資料表`SqlMembershipProvider`和`SqlRoleProvider`。

根據這點，我們可以很輕易地使用成員資格和角色架構而不需要瀏覽步驟 2 中建立的資料庫結構描述。 不過，建立要儲存應用程式資料的資料表時我們可能需要建立與使用者或角色相關聯的實體。 它可幫助您熟悉`SqlMembershipProvider`和`SqlRoleProvider`結構描述時建立外部索引鍵之間的應用程式資料的資料表和步驟 2 中建立這些資料表的條件約束。 此外，在某些罕見的情況下，我們可能需要與使用者介面，並將直接在資料庫層級的角色 (而不是透過`Membership`或`Roles`類別)。

### <a name="partitioning-the-user-store-into-applications"></a>分割應用程式的使用者存放區

成員資格和角色架構的設計，可在許多不同的應用程式之間共用單一的使用者和角色存放區。 使用成員資格或角色架構的 ASP.NET 應用程式必須指定要使用哪一個應用程式磁碟分割。 簡單地說，多個 web 應用程式可以使用相同的使用者和角色存放區。 圖 11 顯示使用者和角色會分割成三個應用程式的存放區： HRSite、 CustomerSite 和 SalesSite。 這些三個 web 應用程式每個有自己的唯一使用者和角色，但它們都在相同的資料庫資料表中，實際儲存其使用者帳戶和角色資訊。


[![使用者帳戶，可能跨多個應用程式磁碟分割](creating-the-membership-schema-in-sql-server-vb/_static/image32.png)](creating-the-membership-schema-in-sql-server-vb/_static/image31.png)

**圖 11**： 使用者帳戶可能是分割區跨多個應用程式 ([按一下以檢視完整大小的影像](creating-the-membership-schema-in-sql-server-vb/_static/image33.png))


`aspnet_Applications`資料表是如何定義這些資料分割。 此資料表中的資料列會以每個使用資料庫來儲存使用者帳戶資訊的應用程式。 `aspnet_Applications`資料表有四個資料行： `ApplicationId`， `ApplicationName`， `LoweredApplicationName`，和`Description`。`ApplicationId` 型別[ `uniqueidentifier` ](https://msdn.microsoft.com/en-us/library/ms187942.aspx)和資料表的主索引鍵。`ApplicationName`提供每個應用程式的唯一人們易記名稱。

其他成員資格和角色相關的資料表連結至`ApplicationId`欄位`aspnet_Applications`。 例如，`aspnet_Users`資料表，其中包含每個使用者帳戶的記錄，具有`ApplicationId`外部索引鍵欄位，如 ditto`aspnet_Roles`資料表。 `ApplicationId`這些資料表中的欄位指定的應用程式磁碟分割的使用者帳戶，或所屬角色。

### <a name="storing-user-account-information"></a>儲存使用者帳戶資訊

使用者帳戶資訊儲存在兩個資料表：`aspnet_Users`和`aspnet_Membership`。 `aspnet_Users`資料表包含保存必要的使用者帳戶資訊的欄位。 三個最相關的資料行如下：

- `UserId`
- `UserName`
- `ApplicationId`

`UserId`主索引鍵 (且屬於型別`uniqueidentifier`)。 `UserName`型別`nvarchar(256)`和密碼，以及構成使用者的認證。 (使用者的密碼儲存在`aspnet_Membership`資料表。)`ApplicationId`連結中的特定應用程式的使用者帳戶`aspnet_Applications`。 沒有複合[`UNIQUE`條件約束](https://msdn.microsoft.com/en-us/library/ms191166.aspx)上`UserName`和`ApplicationId`資料行。 這可確保在給定的應用程式中是唯一的每個使用者名稱，但它可讓具有相同`UserName`以用於不同的應用程式。

`aspnet_Membership`資料表包含額外的使用者帳戶資訊，例如使用者的密碼、 電子郵件地址、 上次登入日期及時間，以及其他等等。 在記錄之間沒有一對一的對應關係`aspnet_Users`和`aspnet_Membership`資料表。 此關聯性保證的`UserId`欄位`aspnet_Membership`，做為資料表的主索引鍵。 像`aspnet_Users`資料表`aspnet_Membership`包含`ApplicationId`繫結合作對這項資訊至特定應用程式磁碟分割的欄位。

### <a name="securing-passwords"></a>保護密碼

密碼資訊儲存在`aspnet_Membership`資料表。 `SqlMembershipProvider`允許密碼儲存在資料庫中使用下列三種的技術之一：

- **清除**-密碼儲存在資料庫中以純文字。 我強烈不鼓勵使用此選項。 如果資料庫遭到入侵-由駭客人員尋找後門或某位的員工具有資料庫存取權的每個單一使用者的認證有對於製作。
- **雜湊**-密碼已雜湊使用單向雜湊演算法和隨機產生的 salt 值。 此雜湊的值 （以及 salt) 儲存在資料庫中。
- **加密**-密碼的加密的版本會儲存在資料庫中。

取決於所使用的密碼儲存體技巧`SqlMembershipProvider`設定中指定`Web.config`。 我們將探討自訂`SqlMembershipProvider`步驟 4 中的設定。 預設行為是儲存密碼雜湊。

負責將密碼儲存的資料行是`Password`， `PasswordFormat`，和`PasswordSalt`。 `PasswordFormat`為類型的欄位`int`其值會指出用來儲存密碼的技巧： 清除為 0; 1 代表 Hashed 2 加密。 `PasswordSalt`會指派隨機產生的字串，不論使用的密碼儲存體技術值`PasswordSalt`時計算的雜湊的密碼，才使用。 最後，`Password`資料行包含實際的密碼資料，可能是純文字密碼的雜湊的密碼或已加密的密碼。

表 1 說明這些三個資料行可能如下的各種不同的儲存體技術儲存 MySecret 的密碼時 ！ 。

| **儲存體技術&lt;\_o3a\_p /&gt;** | **密碼&lt;\_o3a\_p /&gt;** | **PasswordFormat&lt;\_o3a\_p /&gt;** | **PasswordSalt&lt;\_o3a\_p /&gt;** |
| --- | --- | --- | --- |
| 清除 | MySecret ！ | 0 | tTnkPlesqissc2y2SMEygA = = |
| 雜湊 | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM = | 1 | wFgjUfhdUFOCKQiI61vtiQ = = |
| 加密 | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | LSRzhGS/aa/oqAXGLHJNBw = = |

**表 1**： 儲存密碼 MySecret 的密碼相關欄位的範例值 ！

> [!NOTE]
> 特定的加密或使用雜湊演算法`SqlMembershipProvider`中的設定由`<machineKey>`項目。 我們將討論在步驟 3 中的這個組態項目<a id="Tutorial3"> </a> [*表單驗證設定和進階主題*](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)教學課程。

### <a name="storing-roles-and-role-associations"></a>儲存的角色與角色關聯

角色架構可讓開發人員定義一組角色，並指定哪些使用者屬於哪些角色。 透過兩個資料表的資料庫中擷取這項資訊：`aspnet_Roles`和`aspnet_UsersInRoles`。 在每一筆記錄`aspnet_Roles`資料表代表特定的應用程式的角色。 就像是`aspnet_Users`資料表`aspnet_Roles`資料表有三個資料行的相關討論：

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId`主索引鍵 (且屬於型別`uniqueidentifier`)。 `RoleName` 是 `nvarchar(256)` 類型。 和`ApplicationId`連結中的特定應用程式的使用者帳戶`aspnet_Applications`。 沒有複合`UNIQUE`條件約束`RoleName`和`ApplicationId`資料行，確保在給定的應用程式中的每個角色名稱唯一。

`aspnet_UsersInRoles`資料表做為使用者和角色之間的對應。 只有兩個資料行-`UserId`和`RoleId`-，它們共同組成複合主索引鍵。

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>步驟 4： 指定提供者，以及自訂它的設定

所有元件之架構的支援提供者模型，例如成員資格和角色架構-沒有自己的實作詳細資料，並改為來委派該項責任的提供者類別。 在 成員資格架構，`Membership`類別定義的 API 管理使用者帳戶，但它不會直接與任何使用者存放區互動。 相反地，`Membership`類別的方法遞交到設定的提供者的要求，我們將使用`SqlMembershipProvider`。 當我們叫用之方法的其中一個`Membership`類別如何成員資格架構知道委派呼叫`SqlMembershipProvider`？

`Membership`類別具有[`Providers`屬性](https://msdn.microsoft.com/en-us/library/system.web.security.membership.providers.aspx)成員資格 framework 所包含的所有已註冊的提供者類別可供使用的參考。 每個已註冊的提供者有相關聯的名稱和型別。 名稱提供人類看得方便的方式來參考中的特定提供者`Providers`集合，而型別會識別提供者類別。 此外，每個已註冊的提供者可能會包含組態設定。 成員資格 framework 的組態設定包括`PasswordFormat`和`requiresUniqueEmail`，還有許多其他。 所使用的組態設定的完整清單，請參閱表 2 `SqlMembershipProvider`。

`Providers`屬性的內容透過 web 應用程式的組態設定所指定。 根據預設，所有的 web 應用程式具有名為提供者`AspNetSqlMembershipProvider`型別的`SqlMembershipProvider`。 此預設成員資格提供者會在中註冊`machine.config`(位於`%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample1.xml)]

為上方所示，標記[`<membership>`元素](https://msdn.microsoft.com/en-us/library/1b9hw62f.aspx)定義成員資格架構時的組態設定[`<providers>`子項目](https://msdn.microsoft.com/en-us/library/6d4936ht.aspx)指定已註冊提供者。 提供者可能會加入或移除使用[ `<add>` ](https://msdn.microsoft.com/en-us/library/whae3t94.aspx)或[ `<remove>` ](https://msdn.microsoft.com/en-us/library/aykw9a6d.aspx)項目; 請改用[ `<clear>` ](https://msdn.microsoft.com/en-us/library/t062y6yc.aspx)移除所有目前的項目已註冊的提供者。 為上方所示，標記`machine.config`會加入名為提供者`AspNetSqlMembershipProvider`型別的`SqlMembershipProvider`。

除了`name`和`type`屬性`<add>`元素包含定義的各種設定的設定值的屬性。 表 2 列出可用`SqlMembershipProvider`-特定的組態設定，以及每個說明。

> [!NOTE]
> 表 2 中記下任何預設值參考中定義的預設值`SqlMembershipProvider`類別。 請注意，並非所有的組態設定`AspNetSqlMembershipProvider`對應的預設值到`SqlMembershipProvider`類別。 例如，如果未指定成員資格提供者中`requiresUniqueEmail`設定預設值為 true。 不過，`AspNetSqlMembershipProvider`藉由明確指定的值，這個預設值會覆寫`false`。

| **設定&lt;\_o3a\_p /&gt;** | **描述&lt;\_o3a\_p /&gt;** |
| --- | --- |
| `ApplicationName` | 前文提過成員資格架構可讓分割到多個應用程式的單一使用者存放區。 此設定表示成員資格提供者所使用的應用程式磁碟分割的名稱。 如果此值未明確指定，它設定為，在執行階段，應用程式的虛擬根路徑的值。 |
| `commandTimeout` | 指定的 SQL 命令逾時值 （以秒為單位）。 預設值為 30。 |
| `connectionStringName` | 中的連接字串名稱`<connectionStrings>`用來連接至使用者存放區資料庫的項目。 這是必要值。 |
| `description` | 提供已註冊的提供者的人們易記描述。 |
| `enablePasswordRetrieval` | 指定使用者是否可以擷取忘記的密碼。 預設值是 `false`。 |
| `enablePasswordReset` | 指出是否允許使用者重設其密碼。 預設值為 `true`。 |
| `maxInvalidPasswordAttempts` | 嘗試不成功的登入期間指定針對指定的使用者可能發生的最大數目`passwordAttemptWindow`使用者遭到鎖定之前。預設值為 5。 |
| `minRequiredNonalphanumericCharacters` | 必須出現在使用者的密碼中的非英數字元數目下限。 此值必須介於 0 到 128;預設值為 1。 |
| `minRequiredPasswordLength` | 密碼所需的字元數目下限。 此值必須介於 0 到 128;預設為 7。 |
| `name` | 已註冊的提供者的名稱。 這是必要值。 |
| `passwordAttemptWindow` | 追蹤嘗試的登入失敗時的分鐘數。 如果使用者提供了無效的登入認證`maxInvalidPasswordAttempts`指定視窗內的時間、 遭到鎖定。預設值為 10。 |
| `PasswordFormat` | 密碼儲存格式： `Clear`， `Hashed`，或`Encrypted`。 預設為 `Hashed`。 |
| `passwordStrengthRegularExpression` | 如果提供，這個規則運算式是評估強度的所選使用者的密碼時建立新的帳戶，或變更其密碼時使用。 預設值為空字串。 |
| `requiresQuestionAndAnswer` | 指定使用者是否必須回答其安全性問題，在擷取或重設其密碼。 預設值是 `true`。 |
| `requiresUniqueEmail` | 指出是否在指定的應用程式磁碟分割中的所有使用者帳戶必須都具有唯一的電子郵件地址。 預設值是 `true`。 |
| `type` | 指定的提供者的類型。 這是必要值。 |

**表 2**： 成員資格和`SqlMembershipProvider`組態設定

除了`AspNetSqlMembershipProvider`，其他成員資格提供者可能藉由新增類似的標記，以註冊應用程式的應用程式為基礎`Web.config`檔案。

> [!NOTE]
> 角色架構的運作方式大致相同的方式： 沒有預設的已註冊的角色提供者中`machine.config`和可自訂的已註冊的提供者，在應用程式的應用程式為基礎`Web.config`。 在未來的教學課程中，我們會檢查角色架構和其組態中的標記詳細資料。

### <a name="customizing-thesqlmembershipprovidersettings"></a>自訂`SqlMembershipProvider`設定

預設值`SqlMembershipProvider`(`AspNetSqlMembershipProvider`) 具有其`connectionStringName`屬性設為`LocalSqlServer`。 像`AspNetSqlMembershipProvider`提供者，連接字串名稱`LocalSqlServer`中定義`machine.config`。

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample2.xml)]

如您所見，此連接字串會定義資料庫位於 SQL 2005 Express Edition |DataDirectory|aspnetdb.mdf。 字串 |DataDirectory |轉譯在執行階段，以指向`~/App_Data/`目錄，所以資料庫路徑 |DataDirectory|aspnetdb.mdf 會轉譯為`~/App_Data` / `aspnet.mdf`。

如果我們並未指定任何成員資格提供者資訊在我們的應用程式中`Web.config`檔案，應用程式使用已註冊的預設成員資格提供者， `AspNetSqlMembershipProvider`。 如果`~/App_Data/aspnet.mdf`資料庫不存在，ASP.NET 執行階段會自動建立它，並新增應用程式服務結構描述。 不過，我們不想使用`aspnet.mdf`資料庫; 相反地，我們想要使用`SecurityTutorials.mdf`我們在步驟 2 中建立的資料庫。 其中一種方式可以完成這項修改：

- **指定的值 * * *`LocalSqlServer`* * * 中的連接字串名稱 * * *`Web.config`* * *。** 藉由覆寫`LocalSqlServer`中的連接字串名稱值`Web.config`，我們可以使用已註冊的預設成員資格提供者 (`AspNetSqlMembershipProvider`)，並讓它正確使用`SecurityTutorials.mdf`資料庫。 如果您是內容所指定的組態設定，不過沒有關係，這個方法是`AspNetSqlMembershipProvider`。 如需有關這項技術的詳細資訊，請參閱[Scott Guthrie](https://weblogs.asp.net/scottgu/)的部落格文章：[設定 ASP.NET 2.0 應用程式服務設定為使用 SQL Server 2000 或 SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)。
- **加入新的已註冊提供者的類型 * * *`SqlMembershipProvider`* * * 並設定其 * * *`connectionStringName`* * * 設定為指向 * * *`SecurityTutorials.mdf`* * * 資料庫。** 這個方法是在您要自訂其他的組態內容，以及資料庫連接字串的情況下很有用。 我自己的專案中我一律使用這個方法因為它的彈性和可讀性。

我們可以加入新的已註冊提供者參考之前`SecurityTutorials.mdf`資料庫中，我們必須先將適當的連接字串值中加入`<connectionStrings>`一節中`Web.config`。 下列標記加入新的連接字串，名為`SecurityTutorialsConnectionString`參考 SQL Server 2005 Express Edition`SecurityTutorials.mdf`資料庫`App_Data`資料夾。

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample3.xml)]

> [!NOTE]
> 如果您使用替代的資料庫檔案，請視需要更新連接字串。 如需有關如何建立正確的連接字串的詳細資訊，請參閱[ConnectionStrings.com](http://www.connectionstrings.com/)。

接下來，加入下列成員資格設定標記以`Web.config`檔案。 這個標記會註冊新的提供者，名為`SecurityTutorialsSqlMembershipProvider`。

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample4.xml)]

除了註冊`SecurityTutorialsSqlMembershipProvider`提供者，上述的標記定義`SecurityTutorialsSqlMembershipProvider`做為預設提供者 (透過`defaultProvider`屬性`<membership>`項目)。 前文提過成員資格 framework 可以有多個已註冊的提供者。 因為`AspNetSqlMembershipProvider`登錄中的第一個提供者為`machine.config`，除非另外我們它做為預設提供者。

目前，我們的應用程式具有兩個已註冊的提供者：`AspNetSqlMembershipProvider`和`SecurityTutorialsSqlMembershipProvider`。 不過，再註冊`SecurityTutorialsSqlMembershipProvider`我們可能會清除所有先前的提供者註冊提供者藉由新增[`<clear />`元素](https://msdn.microsoft.com/en-us/library/t062y6yc.aspx)之前我們`<add>`項目。 這會清除`AspNetSqlMembershipProvider`從已註冊的提供者清單，表示`SecurityTutorialsSqlMembershipProvider`會是唯一的已註冊的成員資格提供者。 如果我們使用這種方式，則我們不需要標記`SecurityTutorialsSqlMembershipProvider`為預設的提供者，因為它會是只有已註冊的成員資格提供者。 如需有關使用`<clear />`，請參閱[使用`<clear />`時新增提供者](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)。

請注意，`SecurityTutorialsSqlMembershipProvider`的`connectionStringName`設定參考剛加入`SecurityTutorialsConnectionString`連接字串名稱，且其`applicationName`已設定為 SecurityTutorials 的值。 此外，`requiresUniqueEmail`設定已設為`true`。 所有其他設定選項是中的值相同`AspNetSqlMembershipProvider`。 請放心地進行任何組態修改，如果您想。 例如，您可以加強密碼強度，藉由要求兩個非英數字元，而不是其中一個，或增加而不是七個八個字元的密碼長度。

> [!NOTE]
> 前文提過成員資格架構可讓分割到多個應用程式的單一使用者存放區。 成員資格提供者的`applicationName`設定指出提供者在使用者存放區使用時所使用之應用程式。 請務必明確設定的值`applicationName`組態設定，因為如果`applicationName`未明確設定，將它指派給在執行階段的 web 應用程式的虛擬根路徑。 這可正常運作，只要應用程式的虛擬根路徑不會變更，但是，如果您移動到不同的路徑，應用程式`applicationName`設定也會變更。 在這種情況下，成員資格提供者會開始使用不同的應用程式磁碟分割與先前未使用。 在移動之前建立的使用者帳戶都位於不同的應用程式磁碟分割中，這些使用者將不再能夠登入網站。 此主題的更深入討論，請參閱[一定會設定`applicationName`屬性時設定 ASP.NET 2.0 的成員資格以及其他提供者](https://weblogs.asp.net/scottgu/443634)。

## <a name="summary"></a>總結

我們現在有設定的應用程式服務的資料庫 (`SecurityTutorials.mdf`)，而且已經設定我們的 web 應用程式，使其使用的成員資格 framework`SecurityTutorialsSqlMembershipProvider`剛登錄的提供者。 這個已註冊的提供者是型別`SqlMembershipProvider`，且其`connectionStringName`設為適當的連接字串 (`SecurityTutorialsConnectionString`) 及其`applicationName`明確設定的值。

我們現在已準備好使用成員資格架構從我們的應用程式。 在下一個教學課程中，我們將檢查如何建立新的使用者帳戶。 之後，我們將探討驗證使用者，執行以使用者為基礎的授權，並儲存在資料庫中的其他使用者的相關資訊。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [一定會設定`applicationName`屬性時設定 ASP.NET 2.0 的成員資格以及其他提供者](https://weblogs.asp.net/scottgu/443634)
- [若要使用 SQL Server 2000 或 SQL Server 2005 設定 ASP.NET 2.0 應用程式服務](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [下載 SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [檢查 ASP.NET 2.0 s 成員資格、 角色和設定檔](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [`<add>`成員資格提供者的項目](https://msdn.microsoft.com/en-us/library/whae3t94.aspx)
- [`<membership>`項目](https://msdn.microsoft.com/en-us/library/1b9hw62f.aspx)
- [`<providers>`成員資格的項目](https://msdn.microsoft.com/en-us/library/6d4936ht.aspx)
- [使用`<clear />`時新增提供者](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [直接使用`SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>在本教學課程所包含的主題訓練影片

- [了解 ASP.NET 成員資格](../../../videos/authentication/understanding-aspnet-memberships.md)
- [使用成員資格的結構描述設定 SQL 工作](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [變更預設的成員資格結構描述中的成員資格設定](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>關於作者

Scott Mitchell，多個 ASP/ASP.NET 書籍的作者和創辦的 4GuysFromRolla.com，具有已經使用 Microsoft Web 技術從 1998 年。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿 *[Sam 教導您自己 ASP.NET 2.0 24 小時內](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*。 在可到達 Scott [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com)或透過在他的部落格[http://ScottOnWriting.NET](http://scottonwriting.net/)。

### <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 Alicja Maziarz。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)。

>[!div class="step-by-step"]
[上一頁](storing-additional-user-information-cs.md)
[下一頁](creating-user-accounts-vb.md)
