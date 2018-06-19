---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: 安全使用驗證和授權的應用程式 |Microsoft 文件
author: microsoft
description: 步驟 9 示範如何加入驗證和授權來保護 NerdDinner 應用程式中，以便使用者註冊需要和站台才能建立登入...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: 4a9b1e6d7d453bd8dc5a61b1f1cec4617af7d693
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874378"
---
<a name="secure-applications-using-authentication-and-authorization"></a>安全使用驗證和授權的應用程式
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 9 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，會逐步引導式如何建置小，但是完成，web 應用程式使用 ASP.NET MVC 1。
> 
> 步驟 9 示範如何加入驗證和授權來保護 NerdDinner 應用程式中，以便使用者需要註冊和登入站台建立新的 dinners，以及只裝載 dinner 的使用者可以稍後再進行編輯。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner 步驟 9： 驗證和授權

現在我們 NerdDinner 應用程式會授與任何人瀏覽網站的 建立和編輯任何 dinner 的詳細資料的能力。 讓我們來變更，這讓使用者需要註冊和登入的站台建立新的 dinners 加入限制，以便只有裝載 dinner 的使用者可以稍後再進行編輯。

若要啟用此我們會使用驗證和授權來保護應用程式。

### <a name="understanding-authentication-and-authorization"></a>了解驗證和授權

*驗證*是識別及驗證應用程式存取的用戶端的身分識別的程序。 它更簡單來說，是關於 「 誰 」，在瀏覽的網站時，使用者識別。 ASP.NET 支援多種方式來驗證使用者的瀏覽器。 網際網路的 web 應用程式最常見的驗證方法使用稱為 「 表單驗證 」。 表單驗證可讓開發人員撰寫 HTML 登入表單在其應用程式，並驗證使用者名稱/密碼使用者送出針對資料庫或其他密碼認證存放區。 如果使用者名稱/密碼組合不正確，開發人員可以再詢問 ASP.NET 發行加密的 HTTP cookie 來識別使用者在未來的要求。 我們會使用表單驗證與我們 NerdDinner 應用程式。

*授權*是判斷是否已驗證的使用者有權限來存取特定 URL/資源或執行某些動作的程序。 比方說，我們 NerdDinner 應用程式中我們會想要授權登入使用者可以存取 */Dinners/建立*URL，並建立新的 Dinners。 我們也會想要新增授權邏輯，讓只有裝載 dinner 的使用者可以編輯它 – 並拒絕所有其他使用者的編輯。

### <a name="forms-authentication-and-the-accountcontroller"></a>表單驗證和 AccountController

ASP.NET MVC 的預設 Visual Studio 專案範本建立新的 ASP.NET MVC 應用程式時，會自動啟用表單驗證。 也會自動將預先建立的帳戶登入頁面實作加入至專案 – 可輕鬆整合在站台內的安全性。

預設 Site.master 主版頁面會顯示 「 登入 」 連結在右上方的 站台，當不進行驗證時存取它的使用者：

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

按一下 「 登入 」 的連結會引導使用者 */帳戶/登入*URL:

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

訪客您尚未註冊可以這樣做，請按一下 「 註冊 」 連結 – 這會移至 */帳戶/暫存器*URL，讓他們輸入帳戶的詳細資料：

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

按一下 「 註冊 」 按鈕，將建立新的使用者，ASP.NET 成員資格系統，內，並驗證至站台使用表單驗證使用者。

Site.master 登入的使用者時，變更輸出 「 歡迎使用 [username] ！ 」 頁面右上方 訊息和呈現 「 登出 」 連結而不是 「 登入 」 其中一個。 按一下 「 登出 」 連結登出使用者：

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

上述的登入、 登出和註冊功能是已加入至受測專案由 Visual Studio 建立專案時 AccountController 類別內實作。 AccountController 的使用者介面會實作使用 \Views\Account 目錄內的檢視範本：

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

AccountController 類別來發出加密的驗證 cookie 與 ASP.NET 成員資格 API，來儲存及驗證使用者名稱/密碼使用 ASP.NET 表單驗證系統。 ASP.NET 成員資格 API 可延伸，並提供要使用任何密碼認證存放區。 ASP.NET 提供了與儲存使用者名稱/密碼的 SQL 資料庫，或 Active Directory 中的內建成員資格提供者實作。

我們可以設定我們 NerdDinner 應用程式應該使用開啟專案的根目錄中的"web.config"檔案，並尋找哪一個成員資格提供者&lt;成員資格&gt;區段內。 建立專案時加入預設 web.config 註冊 SQL 成員資格提供者，並將它設定為使用名為"ApplicationServices"的連接字串來指定資料庫位置。

預設值"ApplicationServices"連接字串 (內指定&lt;connectionStrings&gt; web.config 檔案區段) 設定為使用 SQL Express。 它所指向的 SQL Express 資料庫名為"ASPNETDB。MDF 」 在應用程式的 「 應用程式\_資料 」 目錄。 如果此資料庫不存在的成員資格 API 可以用在應用程式中的第一次，ASP.NET 會自動建立資料庫，並提供適當的成員資格資料庫結構描述，其中：

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

如果不使用 SQL Express 我們想要使用完整的 SQL Server 執行個體 （或連接到遠端資料庫），我們需要待辦事項是更新在 web.config 檔案中的"ApplicationServices"連接字串，並請確定適當的成員資格結構描述已加入到它所指向的資料庫。 您可以執行"aspnet\_regsql.exe 」 公用程式內加入資料庫中的成員資格和其他 ASP.NET 應用程式服務的適當結構描述的 \Windows\Microsoft.NET\Framework\v2.0.50727\ 目錄。

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>授權 Dinners/建立 URL，使用 [Authorize] 篩選器

我們不需要撰寫任何程式碼，以啟用安全的驗證和帳戶管理實作 NerdDinner 應用程式。 使用者可以註冊新帳戶，與我們的應用程式中，登入/登出的站台。

現在我們可以將授權邏輯加入至應用程式，並控制哪些它們可以和無法進行站台內使用的驗證狀態 」 和 「 訪客的使用者名稱。 讓我們先將授權邏輯加入至我們 DinnersController 類別的 [建立] 動作方法。 具體而言，我們將要求的使用者存取 */Dinners/建立*URL 必須以登入。 如果使用者未登入我們會重新導向至登入頁面，讓他們可以登入。

實作這個邏輯是很簡單。 我們只需要待辦事項是將 [Authorize] 篩選條件屬性加入至我們建立動作方法就像這樣：

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC 支援建立 「 動作篩選 」 可以用來實作可重複使用的邏輯是以宣告方式套用至動作方法的能力。 [Authorize] 篩選器是一個由 ASP.NET MVC 中，提供內建動作篩選條件，它可讓開發人員以宣告方式將授權規則套用至動作方法和控制器類別。

套用不含任何參數 （類似上述） 時提出動作方法要求的使用者必須登入 –，它會自動重新導向瀏覽器的登入 URL 如果它們不會強制執行 [Authorize] 篩選器。 執行這個重新導向原本要求的 URL 會傳遞做為查詢字串引數時 (例如: / 帳戶/登入？ReturnUrl = %2fDinners %2fcreate)。 AccountController 會再將使用者重新導向至原本要求的 URL 一旦他們登入。

[Authorize] 篩選選擇性地支援能夠指定可用來要求，使用者會同時記錄中另一項權限的使用者或允許的安全性角色的成員的清單中的 「 使用者 」 或 「 角色 」 屬性。 例如，下列程式碼只允許兩個特定的使用者、 「 scottgu"和"billg 」，以存取 Dinners/建立的 URL:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

內嵌程式碼中的特定使用者名稱通常會以雖然是很不容易維護。 更好的方法是定義較高層級的 「 角色 」 的程式碼會檢查，然後將使用者對應到使用資料庫或 active directory 系統 （啟用 外部程式碼與儲存的實際的使用者對應清單） 的角色。 ASP.NET 包含內建的角色管理 API，以及一組內建的角色提供者 （包括 SQL 和 Active Directory），可協助您執行此使用者/角色對應。 我們無法再更新，只允許特定"admin"角色中的使用者存取 Dinners/建立 URL 的程式碼：

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>建立時，使用 User.Identity.Name 屬性 Dinners

我們無法擷取使用者的要求使用控制器的基底類別上公開 User.Identity.Name 屬性的目前登入的使用者名稱。

舊版我們實作我們 create （） 的動作方法的 HTTP POST 版本時，我們必須硬式編碼成靜態字串 Dinner"HostedBy"屬性。 我們可以立即更新這個程式碼以改用 User.Identity.Name 屬性，以及建立 Dinner 主機自動新增 RSVP:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

我們在 create （） 方法中加入 [Authorize] 屬性，因為 ASP.NET MVC 可確保在動作方法只執行如果造訪 Dinners/建立 URL 的使用者登入站台上。 因此，User.Identity.Name 屬性值將永遠包含有效的使用者名稱。

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>編輯時，使用 User.Identity.Name 屬性 Dinners

讓我們現在加入授權邏輯，會限制使用者，讓他們只能編輯的 dinners 他們自行裝載的屬性。

為協助解決此，我們會先加入"IsHostedBy(username)"helper 方法 （在我們稍早建立的 Dinner.cs 部分類別） 我們 Dinner 物件。 這個 helper 方法會傳回 true 或 false，根據提供的使用者名稱是否符合 Dinner HostedBy 屬性，以及封裝執行不區分大小寫字串比較它們所需的邏輯：

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

然後，我們會新增至 Edit() 動作方法 [Authorize] 屬性內 DinnersController 類別。 這可確保使用者必須登入，要求 */Dinners/編輯 / [id]* URL。

然後我們可以將程式碼加入使用 Dinner.IsHostedBy(username) helper 方法來驗證登入使用者符合 Dinner 主機我們編輯方法。 如果使用者不是主機，我們將會顯示 「 InvalidOwner 」 檢視，並終止要求。 若要這樣做的程式碼看起來如下：

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

我們可以 \Views\Dinners 目錄上按一下滑鼠右鍵，然後選擇 [新增]-&gt;檢視功能表命令，以建立新的 「 InvalidOwner"檢視。 我們將會填入它具有下列錯誤訊息：

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

然後，現在當使用者嘗試編輯它們並不擁有 dinner，它們將會得到錯誤訊息：

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

我們可以鎖定，刪除 Dinners，並確保只有 Dinner 主機才能刪除它的權限我們控制器內重複 delete 動作方法相同的步驟。

### <a name="showinghiding-edit-and-delete-links"></a>顯示/隱藏編輯和刪除連結

我們要從我們詳細資料的 URL 連結我們 DinnersController 類別的編輯和刪除動作方法：

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

目前，我們會說明不論訪客的詳細資料的 URL 是 dinner 主機的編輯和刪除動作連結。 讓我們來變更這使如果正在瀏覽使用者 dinner 的擁有者，只會顯示連結。

我們 DinnersController 內 Details() 動作方法會擷取 Dinner 物件，並再將它當做模型物件，傳遞至我們檢視範本：

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

我們可以更新有條件地顯示/隱藏的編輯和刪除連結使用 helper 方法類似下面的 Dinner.IsHostedBy() 我們檢視的範本：

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>後續步驟

我們現在看我們如何啟用使用 AJAX dinners 的 RSVP 經過驗證的使用者。

> [!div class="step-by-step"]
> [上一頁](implement-efficient-data-paging.md)
> [下一頁](use-ajax-to-deliver-dynamic-updates.md)
