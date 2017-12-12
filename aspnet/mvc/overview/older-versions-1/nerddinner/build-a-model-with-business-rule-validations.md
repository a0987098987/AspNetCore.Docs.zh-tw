---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: "建立使用商務規則驗證模型 |Microsoft 文件"
author: microsoft
description: "步驟 3 說明如何建立模型，我們可以使用這兩個查詢，並更新資料庫中我們 NerdDinner 應用程式。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: dbe6370979f218988c168df3e80314ef9b338fbd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="build-a-model-with-business-rule-validations"></a>建立使用商務規則驗證模型
====================
由[Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 3 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，會逐步引導式如何建置小，但是完成，web 應用程式使用 ASP.NET MVC 1。
> 
> 步驟 3 說明如何建立模型，我們可以使用這兩個查詢，並更新資料庫中我們 NerdDinner 應用程式。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner 步驟 3： 建立模型

模型檢視控制器架構中 「 模型 」 一詞是指代表應用程式，以及與它整合驗證和商務規則的對應網域邏輯資料的物件。 模型在許多方面 「 活動 」 的 MVC 為基礎的應用程式，並稍後基本上，我們會看到磁碟機的行為。

ASP.NET MVC 架構支援使用任何資料存取技術，以及開發人員可以選擇各式各樣的豐富的.NET 資料選項，來實作其模型包括： LINQ to Entities 中，LINQ to SQL、 NHibernate、 LLBLGen Pro、 SubSonic、 WilsonORM 或只是未經處理的 ADO。NET Datareader 或資料集。

我們要建立簡單的模型相當緊密對應至我們的資料庫設計，並將某些自訂驗證邏輯和商務規則中使用 LINQ to SQL NerdDinner 應用程式。 我們再會實作儲存機制類別，可協助抽象離開於應用程式，以及可讓我們能夠輕鬆地將單元測試的其餘部分的資料持續性實作。

### <a name="linq-to-sql"></a>LINQ to SQL

LINQ to SQL 是隨附於.NET 3.5 ORM （物件關聯式對應程式）。

LINQ to SQL 提供輕鬆地將資料庫資料表對應到我們可以撰寫程式碼針對.NET 類別。 我們將 Dinner 和 RSVP 類別我們資料庫中對應的 Dinners 和 RSVP 資料表使用 NerdDinner 應用程式。 Dinners 和 RSVP 資料表的資料行，會對應至為 Dinner 和 RSVP 類別上的屬性。 每個 Dinner 和 RSVP 物件將代表 Dinners 或 RSVP 資料表在資料庫內的個別資料列。

LINQ to SQL 可讓我們避免手動建構 SQL 陳述式來擷取和更新 Dinner 與 RSVP 與資料庫資料的物件。 相反地，我們會定義 Dinner 和 RSVP 類別如何將它們對應到 azure 或從資料庫和它們之間的關聯性。 LINQ to SQL 會接著會負責產生我們互動和使用它們時，在執行階段使用適當的 SQL 執行邏輯。

我們可以使用 VB 和 C# 中的 LINQ 語言支援，來撰寫易懂的查詢，會擷取 Dinner 和 RSVP 從資料庫物件。 這會盡量縮短資料程式碼，我們必須撰寫，並可讓我們建立真的全新的應用程式。

### <a name="adding-linq-to-sql-classes-to-our-project"></a>將 LINQ to SQL 類別加入至受測專案

我們將開始在專案中，「 模型 」 資料夾上按一下滑鼠右鍵，然後選取**新增-&gt;新項目**功能表命令：

![](build-a-model-with-business-rule-validations/_static/image1.png)

這會顯示 「 加入新項目 對話方塊。 我們會依 「 資料 」 類別目錄篩選，然後選取 「 LINQ 到 SQL 的類別 」 範本內：

![](build-a-model-with-business-rule-validations/_static/image2.png)

我們會命名為"NerdDinner"的項目，然後按一下 [新增] 按鈕。 Visual Studio 會將我們 \Models 目錄下的 NerdDinner.dbml 檔案，並接著開啟 LINQ to SQL 物件關聯式設計工具：

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>建立資料模型類別搭配 LINQ to SQL

LINQ to SQL 可讓我們快速地從現有的資料庫結構描述中建立資料模型類別。 待辦事項我們想要在其中建立模型的此我們會在 [伺服器總管] 中開啟 NerdDinner 資料庫，並選取的資料表：

![](build-a-model-with-business-rule-validations/_static/image4.png)

我們可以再將資料表拖曳至 LINQ 到 SQL 設計工具介面。 當我們執行這個 LINQ to SQL 會自動建立 Dinner 和 RSVP 類別使用的結構描述的資料表 （對應至資料庫資料表資料行的類別內容）：

![](build-a-model-with-business-rule-validations/_static/image5.png)

預設 LINQ to SQL 設計工具自動"複數化 」 資料表和資料行名稱時，它會建立資料庫結構描述為基礎的類別。 例如:"Dinners 」 資料表，在上述範例中會導致 「 Dinner 」 類別。 此類別命名有助於讓我們的模型與.NET 的命名慣例，一致，而且我通常發現該設計工具修復這向上方便 （特別是當新增大量資料表）。 如果您不喜歡的類別或設計工具產生，但您可以一律覆寫它，並將它變更為任何您想要的名稱的屬性名稱。 您可以藉由編輯實體/屬性名稱內嵌在設計工具中，或藉由修改透過屬性方格。

根據預設，LINQ to SQL 設計工具也會檢查主索引鍵/外部索引鍵關聯性的資料表，並根據它們自動會建立預設 」 關聯性的關聯"建立不同的模型類別之間。 例如，當我們拖曳 Dinners 和 RSVP 兩者之間的一對多關聯性關聯資料表拖曳至 LINQ to SQL 設計工具推斷基礎的 RSVP 資料表有外部索引鍵 Dinners 資料表 (中的箭號表示這一點設計工具）：

![](build-a-model-with-business-rule-validations/_static/image6.png)

上述的關聯將會使 LINQ to SQL 會將強類型的 「 Dinner"屬性加入至開發人員可用來存取指定 RSVP 相關聯的 Dinner RSVP 類別。 它也會造成 Dinner 類別擁有 「 RSVPs 」 集合屬性，可讓開發人員，以擷取和更新 RSVP 特定 Dinner 相關聯的物件。

以下您可以看見範例的 Visual Studio 中的 intellisense，當我們建立新的 RSVP 物件，並將它加入至 Dinner 與會者集合。 請注意如何 LINQ to SQL 會自動加入 「 與會者 」 集合 Dinner 物件上：

![](build-a-model-with-business-rule-validations/_static/image7.png)

將 RSVP 物件加入至 Dinner 與會者集合我們就是在告訴 LINQ to SQL 關聯外部索引鍵之間的關聯性 Dinner RSVP 資料列，在資料庫中：

![](build-a-model-with-business-rule-validations/_static/image8.png)

如果您不喜歡設計工具已建立模型或具名資料表關聯的方式，您可以覆寫它。 只要按一下設計工具內的關聯箭號，並存取其透過重新命名、 刪除或修改它的屬性方格的屬性。 NerdDinner 應用程式，不過，預設關聯規則適用於我們所建置的資料模型類別，我們就可以使用的預設行為。

### <a name="nerddinnerdatacontext-class"></a>NerdDinnerDataContext 類別

Visual Studio 會自動建立.NET 類別，代表模型和資料庫使用 LINQ to SQL 設計工具定義的關聯性。 LINQ to SQL DataContext 類別也會產生每個 linq to SQL 設計工具檔案加入至方案。 我們名為我們的 LINQ to SQL 類別項目 」 NerdDinner"，因為建立的 DataContext 類別會呼叫 「 NerdDinnerDataContext"。 這個 NerdDinnerDataContext 類別是我們將會與資料庫互動的主要方式。

我們 NerdDinnerDataContext 類別會公開兩個屬性的"Dinners"和"RSVPs"-表示我們包含在模型資料庫中的兩個資料表。 我們可以使用 C# 來從資料庫查詢和擷取 Dinner 和 RSVP 的物件寫入 LINQ 查詢，對這些屬性。

下列程式碼會示範如何具現化 NerdDinnerDataContext 物件，並執行，以取得序列是未來的 Dinners 針對 LINQ 查詢。 撰寫 LINQ 查詢時，visual Studio 提供完整的 intellisense，並從中傳回強型別和物件也支援 intellisense:

![](build-a-model-with-business-rule-validations/_static/image9.png)

讓我們為 Dinner 和 RSVP 物件查詢，除了 NerdDinnerDataContext 也會自動追蹤我們接下來我們透過它來擷取 Dinner 和 RSVP 物件所做的任何變更。 若要輕鬆地將變更儲存回資料庫-而不需要撰寫任何明確的 SQL 更新程式碼，我們可以使用這項功能。

例如，下列程式碼會示範如何使用 LINQ 查詢從資料庫中擷取單一的 Dinner 物件、 更新兩個 Dinner 屬性，然後再將變更儲存回資料庫：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

NerdDinnerDataContext 物件上方的程式碼會自動追蹤對我們從它擷取 Dinner 物件屬性變更。 當我們稱為 「 SubmitChanges()"方法時，它會執行的適當 SQL 「 更新 」 陳述式到資料庫來保存回更新的值。

### <a name="creating-a-dinnerrepository-class"></a>建立 DinnerRepository 類別

對於小型應用程式可能會很有控制站工作直接針對 LINQ to SQL DataContext 類別和內嵌控制器中的 LINQ 查詢會有問題。 當應用程式變大，不過，這種方法就容易維護和測試。 它也會導致我們複製相同 LINQ 查詢中的多個位置。

可以讓您更輕鬆地維護並測試的應用程式的其中一個方法是使用 「 儲存機制 」 模式。 儲存機制類別可協助封裝資料查詢，並持續性邏輯和摘要了從應用程式的資料持續性的實作詳細資料。 除了讓應用程式程式碼更清楚，使用儲存機制模式可以讓您更輕鬆地在未來，變更資料儲存區實作，它可以幫助便利單元測試應用程式，而不需要實際的資料庫。

NerdDinner 應用程式中，我們會定義 DinnerRepository 類別具有下列簽章：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*注意： 本章稍後我們將 IDinnerRepository 介面擷取自這個類別，讓我們控制站上使用它的相依性插入。一開始，我們即將開始簡單和直接與 DinnerRepository 類別能夠運作。*

若要實作此類別，我們將在我們的 「 模型 」 資料夾上按一下滑鼠右鍵，然後選擇**新增-&gt;新項目**功能表命令。 「 加入新項目 對話方塊中我們會選取 「 類別 」 範本，並將檔案命名為"DinnerRepository.cs 」:

![](build-a-model-with-business-rule-validations/_static/image10.png)

然後，我們可以實作 DinnerRespository 類別使用下列程式碼：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>擷取、 更新、 插入和刪除使用 DinnerRepository 類別

現在，我們已建立 DinnerRepository 類別，讓我們看看一些程式碼範例示範如何使用它的一般工作：

#### <a name="querying-examples"></a>查詢範例

下列程式碼會擷取單一的 Dinner 使用 DinnerID 值：


[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

下列程式碼會擷取所有即將 dinners 和迴圈它們：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>插入和更新範例

下列程式碼示範如何將兩個新 dinners。 加入/修改儲存機制不是向資料庫認可，直到其上呼叫"save （）"方法。 就可能發生的所有變更或是沒有任何不要儲存我們的儲存機制時，LINQ to SQL 會自動在資料庫交易 – 包裝所有變更：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

下列程式碼擷取現有 Dinner 物件，並修改在其上的兩個屬性。 我們的儲存機制上呼叫"save （）"方法時，在傳回至資料庫認可變更：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

下列程式碼擷取 dinner，並將其加入指明它。 它會使用 （因為這兩個資料庫之間沒有主索引鍵/外部索引鍵關聯性） 為我們建立 LINQ to SQL Dinner 物件上的與會者集合。 這項變更保存回資料庫為新的 RSVP 資料表資料列儲存機制上呼叫"save （）"方法：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>刪除範例

下列程式碼擷取現有的 Dinner 物件，並隨後會將標示要刪除它。 儲存機制上呼叫"save （）"方法時它將會認可刪除資料庫：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>整合驗證及商務規則邏輯與模型類別

驗證和商務規則邏輯是重要的部分資料適用於任何應用程式整合。

#### <a name="schema-validation"></a>結構描述驗證

當使用 LINQ to SQL 設計工具來定義模型類別時，資料模型類別中屬性的資料類型會對應到資料庫資料表的資料類型。 例如:"datetime"Dinners 資料表中的"EventDate 」 資料行時，LINQ to SQL 所建立的資料模型類別，都屬於類型"DateTime"（這是內建的.NET 資料類型）。 這表示如果您嘗試從程式碼中，將整數或布林值給它，而且它會引發錯誤自動如果您嘗試在執行階段隱含轉換成它的無效字串型別，會發生編譯錯誤。

使用的字串，可協助防止 SQL 資料隱碼攻擊時使用它時 LINQ to SQL 也會自動將為您處理逸出 SQL 值。

#### <a name="validation-and-business-rule-logic"></a>驗證及商務規則邏輯

結構描述驗證第一個步驟中，但是便很少。 大部分的真實世界案例需要指定更豐富的驗證邏輯，跨越多個屬性、 執行程式碼，以及經常會有不同的模型狀態感知的功能 (例如： 它正在建立/更新/刪除，或網域特有狀態中例如 「 封存 」）。 有各種不同的模式和架構，可用來定義，並將驗證規則套用至模型類別，並有數個基礎.NET 架構有可以用來提供協助。 您可以使用幾乎任何的 ASP.NET MVC 應用程式中將它們。

基於我們 NerdDinner 應用程式的目的，我們會使用相對較簡單且直接的模式，我們展現 IsValid 屬性和 GetRuleViolations() 方法我們 Dinner 模型物件上。 IsValid 屬性會傳回 true 或 false，根據的驗證和商務規則是否有效。 GetRuleViolations() 方法會傳回任何規則錯誤的清單。

我們將為我們 Dinner 模型實作 IsValid 和 GetRuleViolations()，方法是加入受測專案的 「 部分類別 」。 部分類別可用來將方法/屬性/事件加入至類別維護的 VS 設計工具 （例如 Dinner 所產生的類別，LINQ to SQL 設計工具），並幫助避免從觸及我們的程式碼的工具。 我們可以 \Models 資料夾上按一下滑鼠右鍵，將新的部分類別加入至受測專案，然後選取 加入新項目 」 的功能表命令。 我們可以再選擇 加入新項目 」 對話方塊中的 「 類別 」 範本，並將它命名為 Dinner.cs 中。

![](build-a-model-with-business-rule-validations/_static/image11.png)

按一下 [新增] 按鈕，將 Dinner.cs 檔案加入專案，並在 IDE 中開啟。 接著可以實作基本的規則驗證強制 framework 使用程式碼下方：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

上述程式碼的一些注意事項：

- Dinner 類別開頭處以 「 部分 」 關鍵字 – 這表示，其中包含的程式碼會結合的 LINQ to SQL 設計工具的產生和保留的類別，並編譯成單一類別。
- RuleViolation 類別是我們會加入專案，讓我們提供更多詳細的規則違規的 helper 類別。
- Dinner.GetRuleViolations() 方法會導致我們驗證和商務規則進行評估 （我們會實作它們很快）。 它接著會傳回回 RuleViolation 物件提供任何規則錯誤的更多詳細的序列。
- Dinner.IsValid 屬性提供一個便利的協助程式屬性，指出 Dinner 物件是否有任何作用中 RuleViolations。 它能夠主動檢查由開發人員隨時使用 Dinner 物件 （並不會引發例外狀況）。
- Dinner.OnValidate() 部分方法是讓我們隨時 Dinner 物件即將保存在資料庫中會收到通知 LINQ to SQL 提供的攔截。 上述我們 OnValidate() 實作可確保當 Dinner 會在儲存之前會有任何 RuleViolations。 如果它處於無效的狀態，則會引發例外狀況，這會導致 LINQ to SQL 中止交易。

此方法提供一種簡單的架構，我們可以將驗證及整合到商務規則。 現在讓我們加入下列規則來 GetRuleViolations() 方法：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

我們使用 「 產生傳回"功能的 C# 傳回的任何 RuleViolations 序列。 上述的第六個規則檢查只會強制執行我們 Dinner 字串屬性不能為 null 或空白。 上一個規則更有趣的是，並呼叫 PhoneValidator.IsValidNumber() helper 方法，我們加入至受測專案可讓您確認 ContactPhone 數字格式相符項目 Dinner 的國家/地區。

我們可以使用。若要實作這項電話驗證支援網路的規則運算式支援。 以下是我們可以將加入的簡單 PhoneValidator 實作可讓我們加入國家/地區特定 Regex 模式檢查，我們專案：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>處理驗證和商務邏輯違規

既然我們已經加入上述驗證和商務規則程式碼，我們嘗試建立或更新 Dinner 任何時間，我們的驗證邏輯規則會進行評估並強制執行。

開發人員可以撰寫程式碼類似下面主動判斷 Dinner 物件是否有效，並擷取所有違規，而不會引發任何例外狀況的清單：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

如果我們嘗試儲存 Dinner 處於無效狀態，當我們 DinnerRepository 上呼叫 save （） 方法，就會引發例外狀況。 這是因為 LINQ to SQL 自動我們 Dinner.OnValidate() 部分方法之前呼叫它 Dinner 將變更儲存，而我們將程式碼加入 Dinner.OnValidate() Dinner 存在於任何規則違規時引發例外狀況。 我們可以捕捉此例外狀況及被動擷取違規，若要修正清單：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

因為我們驗證和商務規則中實作，在我們的模型層和不在 UI 層，會套用，會在所有情況下，我們的應用程式內使用。 我們稍後可以變更或新增商務規則，可搭配我們 Dinner 物件接受這些所有的程式碼。

有彈性地變更商務規則，在一個地方，而不需 ripple 整個應用程式和 UI 邏輯，這些變更是正負號的編寫完善的應用程式與 MVC framework 有助於鼓勵的好處。

### <a name="next-step"></a>下一個步驟

我們現在有我們可以使用查詢和更新資料庫的模型。

我們現在加入某些控制器和檢視的專案，我們可以使用它來建立其周圍的 HTML UI 體驗。

>[!div class="step-by-step"]
[上一頁](create-a-database.md)
[下一頁](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
