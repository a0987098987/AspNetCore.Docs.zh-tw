---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: 保護應用程式使用驗證和授權 |Microsoft Docs
author: microsoft
description: 步驟 9 示範如何加入驗證和授權來保護我們的 NerdDinner 應用程式，讓使用者需要註冊並登入若要建立站台...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: 0005b99dbf7d59e96313f025880c46cdec4838b6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801388"
---
<a name="secure-applications-using-authentication-and-authorization"></a>保護使用驗證和授權的應用程式
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 9 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，逐步解說如何建置一個小型的但在完成時，使用 ASP.NET MVC 1 的 web 應用程式。
> 
> 步驟 9 示範如何加入驗證和授權來保護我們的 NerdDinner 應用程式，讓使用者需要註冊和登入站台建立新的 dinners，以及只有裝載 dinner 的使用者可以稍後再進行編輯。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得開始使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或是[MVC Music 市集](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner 步驟 9： 驗證和授權

現在我們 NerdDinner 應用程式授與任何人瀏覽的網站能夠建立和編輯任何 dinner 的詳細資料。 讓我們將此變更，讓使用者需要註冊並登入網站來建立新的 dinners，並新增一項限制，以便只有裝載 dinner 的使用者可以稍後再進行編輯。

若要啟用此我們將使用的驗證和授權來保護我們的應用程式。

### <a name="understanding-authentication-and-authorization"></a>了解驗證和授權

*驗證*是用來識別和驗證存取的應用程式的用戶端的身分識別的程序。 來說更簡單的做法，這是關於 「 誰 」，他們瀏覽網站時，終端使用者識別。 ASP.NET 支援多種方式來驗證瀏覽器的使用者。 對於網際網路的 web 應用程式，最常見的驗證方法，使用稱為 「 表單驗證 」。 表單驗證可讓開發人員撰寫 HTML 登入表單在其應用程式，並驗證使用者提交針對資料庫或其他密碼認證存放區的使用者名稱/密碼。 如果使用者名稱/密碼組合不正確，開發人員則可以要求發出加密的 HTTP cookie 來識別使用者在未來的要求間的 ASP.NET。 我們將使用表單驗證的 NerdDinner 應用程式。

*授權*程序會判斷已驗證的使用者是否有權存取特定 URL/資源，或執行某些動作。 比方說，我們 NerdDinner 應用程式中我們會想要授權僅登入的使用者可以存取*Dinners/建立*URL，並建立新的 Dinners。 我們也要新增授權邏輯，讓只有裝載 dinner 的使用者可以編輯它 – 並拒絕所有其他使用者的編輯權限。

### <a name="forms-authentication-and-the-accountcontroller"></a>表單驗證和 AccountController

ASP.NET MVC 的預設 Visual Studio 專案範本會建立新的 ASP.NET MVC 應用程式時，會自動啟用表單驗證。 它也會自動將專案 – 可讓您輕鬆將站台內的安全性整合預先建立的帳戶登入頁面實作。

存取它的使用者未經驗證時，預設 Site.master 主版頁面會顯示 「 登入 」 連結在右上方的 站台：

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

按一下 「 登入 」 連結會帶使用者 */帳戶/登入*URL:

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

訪客尚未註冊做法是按一下 [註冊] 連結 – 這會需要他們 */帳戶/註冊*URL，並允許他們輸入帳戶的詳細資料：

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

按一下 [註冊] 按鈕建立新的使用者，在 ASP.NET 成員資格系統，並驗證連線至站台，使用表單驗證使用者。

Site.master 登入的使用者時，變更輸出 「 歡迎 [username] ！ 頁面右上方 訊息並呈現 「 登出 」 連結而不是 「 登入 」 其中一個。 按一下 「 登出 」 連結，將使用者記錄：

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

已新增至我們的專案的 Visual Studio 建立專案時 AccountController 類別中實作上述的登入、 登出和註冊功能。 AccountController 的使用者介面的實作使用 \Views\Account 目錄內的檢視範本：

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

AccountController 類別會使用 ASP.NET 表單驗證系統發出加密的驗證 cookie 和 ASP.NET 成員資格 API 來儲存和驗證使用者名稱/密碼。 ASP.NET Membership API 加以擴充，而且可讓任何密碼的認證存放區可用。 ASP.NET 隨附儲存使用者名稱/密碼，SQL database，或 Active Directory 中的內建的成員資格提供者實作。

我們可以設定我們 NerdDinner 應用程式應該使用開啟的專案根目錄"web.config"檔案，並尋找哪一個成員資格提供者&lt;成員資格&gt;其內的區段。 當建立專案時加入預設 web.config 註冊 SQL 成員資格提供者，並將它設定為使用名為"ApplicationServices"的連接字串來指定資料庫位置。

預設的 「 ApplicationServices"連接字串 (指定內&lt;connectionStrings&gt; web.config 檔案區段) 已設定為使用 SQL Express。 它會指向名為"ASPNETDB SQL Express 資料庫。MDF 」 的應用程式 」 應用程式\_Data"目錄。 如果此資料庫不存在的成員資格 API 可在應用程式中的第一次，ASP.NET 會自動建立資料庫，並佈建適當的成員資格資料庫結構描述，其中：

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

如果不使用 SQL Express 我們想要使用完整的 SQL Server 執行個體 （或連接到遠端資料庫），我們會需要待辦事項是更新 web.config 檔案中的 「 ApplicationServices"連接字串，並確定適當的成員資格結構描述已新增至它所指向的資料庫。 您可以執行"aspnet\_regsql.exe"\Windows\Microsoft.NET\Framework\v2.0.50727\ 目錄，將成員資格和其他 ASP.NET 應用程式服務的適當結構描述新增至資料庫內的公用程式。

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>授權 [Authorize] 篩選 Dinners/建立 URL

我們不必撰寫任何程式碼，以啟用安全的驗證和帳戶管理實作 NerdDinner 應用程式。 使用者可以向我們的應用程式和登入/登出的站台的新帳戶。

現在我們可以新增授權邏輯應用程式，並使用的訪客的使用者名稱與驗證狀態來控制什麼他們能夠與無法執行站台內。 讓我們先授權邏輯加入我們的 DinnersController 類別的 [建立] 動作方法。 具體來說，我們會要求使用者存取*Dinners/建立*URL 必須登入。 如果使用者未登入我們將會重新導向至登入頁面，讓他們可以登入。

實作此邏輯並不難。 我們只需要待辦事項是將 [Authorize] 篩選條件屬性加入至我們建立的動作方法就像這樣：

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC 支援能夠建立 「 動作篩選條件 」，可用來實作可重複使用透過宣告方式套用至動作方法的邏輯。 [Authorize] 篩選條件是其中一個 ASP.NET MVC 中，所提供的內建的動作篩選條件，它可讓開發人員以宣告方式將授權規則套用至動作方法和控制器類別。

套用不含任何參數 （如上述） 時要求動作方法的使用者必須登入 –，它會自動重新導向瀏覽器登入 url 如果它們不會強制執行 [Authorize] 篩選器。 執行此重新導向原本要求的 URL 做為查詢字串引數傳遞時 (例如: / 帳戶/登入？ReturnUrl = %2fDinners %2fcreate)。 AccountController 會再將使用者重新導向回到原來要求的 URL 之後登入。

[Authorize] 篩選器選擇性地支援能夠指定可用來要求的使用者同時登中和允許的使用者或允許的安全性角色的成員的清單中的 「 使用者 」 或 「 角色 」 屬性。 例如，下列程式碼只允許兩個特定的使用者、 「 scottgu"和"billg 」，以存取 Dinners/建立的 URL:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

內嵌程式碼中的特定使用者名稱通常不過是很不容易維護。 更好的方法是定義較高層級的 「 角色 」 的程式碼會檢查，然後將使用者對應到角色使用的資料庫 」 或 「 active directory 系統 （啟用實際的使用者對應清單，從程式碼外部儲存）。 ASP.NET 包含內建的角色管理 API，以及一組內建的角色提供者 （包括 SQL 和 Active Directory），可協助您執行此使用者/角色對應。 然後，我們可以更新為只允許特定的 「 管理員 」 角色中的使用者存取 Dinners/建立 URL 的程式碼：

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>建立時，使用 User.Identity.Name 屬性 Dinners

我們可以擷取之使用者的要求，使用控制器的基底類別上公開的 User.Identity.Name 屬性的目前登入的使用者名稱。

先前我們實作我們 create （） 動作方法的 HTTP POST 新版時我們有硬式編碼為靜態字串 Dinner"HostedBy"屬性。 我們可以立即更新此程式碼以改用 User.Identity.Name 屬性，以及自動建立 Dinner 的主機新增 RSVP:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

我們在 create （） 方法中加入 [Authorize] 屬性，因為 ASP.NET MVC 可確保在動作方法只執行如果網站上造訪 Dinners/建立 URL 的使用者登入。 因此，User.Identity.Name 屬性值一律會包含有效的使用者名稱。

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>編輯時，使用 User.Identity.Name 屬性 Dinners

讓我們現在新增授權邏輯，以限制使用者，讓他們只能編輯的 dinners 裝載他們自己的屬性。

若要協助，我們會先加入"IsHostedBy(username)"helper 方法我們 Dinner 物件 （在我們稍早建立的 Dinner.cs 部分類別）。 這個 helper 方法會傳回 true 或 false 取決於提供的使用者名稱是否符合 Dinner HostedBy 屬性，並封裝執行不區分大小寫的字串比較，其中所需的邏輯：

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

然後，我們將新增至 edit （） 動作方法 [Authorize] 屬性中我們 DinnersController 類別。 這可確保使用者必須登入，要求 */Dinners/編輯 / [id]* URL。

我們就可以加入程式碼，我們將使用 Dinner.IsHostedBy(username) 協助程式方法來驗證登入的使用者符合 Dinner 主應用程式的編輯方法。 如果使用者不是主應用程式，我們將會顯示 「 InvalidOwner 」 檢視，並終止要求。 若要這樣做的程式碼如下所示：

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

我們可以 \Views\Dinners 目錄上按一下滑鼠右鍵，然後選擇 [加入]-&gt;檢視功能表命令來建立新的 「 InvalidOwner 」 檢視。 我們將會填入它與下面的錯誤訊息：

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

然後，現在當使用者嘗試編輯它們並不擁有 dinner，他們會收到錯誤訊息：

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

我們可以 delete （） 動作方法的重複相同步驟，在我們的控制器，都要鎖定，刪除 Dinners，並確定只有 Dinner 主機可以將它刪除的權限。

### <a name="showinghiding-edit-and-delete-links"></a>顯示/隱藏編輯和刪除連結

我們要連結到我們 DinnersController 類別的編輯和刪除動作方法，從我們的詳細資料 URL:

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

我們會在目前顯示的編輯和刪除動作連結，不論訪客能夠詳細資料 URL 是 dinner 的主機。 讓我們來變更這使造訪的使用者是否 dinner 的擁有者，只會顯示連結。

在我們 DinnersController Details() 動作方法會擷取 Dinner 物件，並再將它當做模型物件，傳遞至我們的檢視範本：

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

我們可以更新檢視範本有條件地顯示/隱藏的編輯和刪除連結使用 Dinner.IsHostedBy() helper 方法，如下所示：

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>後續步驟

現在來看看我們如何啟用 dinners 使用 AJAX 的 RSVP 經過驗證的使用者。

> [!div class="step-by-step"]
> [上一頁](implement-efficient-data-paging.md)
> [下一頁](use-ajax-to-deliver-dynamic-updates.md)
