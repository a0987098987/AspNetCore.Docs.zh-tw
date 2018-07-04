---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: 使用商務規則驗證建置模型 |Microsoft Docs
author: microsoft
description: 步驟 3 說明如何建立模型，我們可以使用這兩個查詢，並更新資料庫中我們 NerdDinner 的應用程式。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: 8e871a8c14dce80edbddb9d87dba929bf3356895
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379017"
---
<a name="build-a-model-with-business-rule-validations"></a>使用商務規則驗證建置模型
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 3 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，逐步解說如何建置一個小型的但在完成時，使用 ASP.NET MVC 1 的 web 應用程式。
> 
> 步驟 3 說明如何建立模型，我們可以使用這兩個查詢，並更新資料庫中我們 NerdDinner 的應用程式。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得開始使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或是[MVC Music 市集](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner 步驟 3： 建立模型

在 模型檢視控制器架構 「 模型 」 一詞是指代表資料的應用程式，以及與它整合驗證和商務規則的對應網域邏輯的物件。 模型在許多方面 「 核心 」 功能的 MVC 架構的應用程式，並稍後基本上，我們會看到磁碟機的行為。

ASP.NET MVC 架構支援使用任何資料存取技術和開發人員可以選擇各式各樣的豐富的.NET 資料選項，來實作其模型，包括： LINQ to Entities、 LINQ to SQL、 NHibernate、 LLBLGen Pro，SubSonic、 WilsonORM 或只是原始的 ADO。NET DataReaders 或資料集。

我們要建立簡單的模型非常緊密對應至我們的資料庫設計，並將一些自訂的驗證邏輯和商務規則中使用 LINQ to SQL NerdDinner 應用程式。 我們接著會實作儲存機制類別，可協助抽象離開應用程式，並可讓我們輕鬆單元測試的其餘部分的資料持續性實作。

### <a name="linq-to-sql"></a>LINQ to SQL

LINQ to SQL 是隨附於.NET 3.5 ORM （物件關聯式對應程式）。

LINQ to SQL 提供輕鬆地將資料庫資料表對應到我們可以對撰寫程式碼的.NET 類別。 我們將對應的 Dinners 和 RSVP 資料表為 Dinner 和 RSVP 類別，在資料庫裡面使用我們 NerdDinner 的應用程式。 Dinners 和 RSVP 資料表的資料行就對應至 Dinner 和 RSVP 類別上的屬性。 每個 Dinner 和 RSVP 物件將代表 Dinners 或 RSVP 資料庫中資料表內的個別資料列。

LINQ to SQL 可讓我們不必以手動方式建構 SQL 陳述式來擷取和更新 Dinner RSVP 資料庫資料的物件。 相反地，我們會定義 Dinner 和 RSVP 類別，它們所對應的方式，或從資料庫和其間的關聯性。 LINQ to SQL 會接著會負責產生適當的 SQL 執行邏輯，以在執行階段使用，當我們互動，並使用它們。

我們可以使用 LINQ 語言支援，在 VB 和 C# 來撰寫易懂的查詢擷取 Dinner 和 RSVP 資料庫中的物件。 這將我們要寫入的資料程式碼數量降到最低，可讓我們建置全新的應用程式。

### <a name="adding-linq-to-sql-classes-to-our-project"></a>LINQ to SQL 類別加入我們的專案

我們將開始在我們的專案中的 < 模型 > 資料夾上按一下滑鼠右鍵，然後選取**Add-&gt;新的項目**功能表命令：

![](build-a-model-with-business-rule-validations/_static/image1.png)

這會顯示 新增新的項目 」 對話方塊。 我們會依據 「 資料 」 類別目錄篩選，並選取 「 LINQ 到 SQL 的類別 」 範本，其中：

![](build-a-model-with-business-rule-validations/_static/image2.png)

我們會命名為"NerdDinner"的項目，然後按一下 [新增] 按鈕。 Visual Studio 會加入 NerdDinner.dbml 檔案我們 \Models 目錄] 下方，並接著開啟 [LINQ to SQL 物件關聯式設計工具：

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>使用 LINQ to SQL 建立資料模型類別

LINQ to SQL 可讓我們快速地從現有的資料庫結構描述中建立資料模型類別。 待辦事項我們想要在其中建立模型的此我們將在 [伺服器總管] 中，開啟 NerdDinner 資料庫，並選取的資料表：

![](build-a-model-with-business-rule-validations/_static/image4.png)

我們可以再將資料表拖曳至 LINQ 至 SQL 設計工具介面。 當我們執行這個 LINQ to SQL 將會自動建立 Dinner 和使用 （與類別屬性會對應至資料庫資料表資料行） 資料表的結構描述的 RSVP 類別：

![](build-a-model-with-business-rule-validations/_static/image5.png)

預設 LINQ to SQL 設計工具會自動 「 複數化 」 資料表和資料行名稱時它會建立資料庫結構描述為基礎的類別。 例如:"Dinners 」 資料表，在上述範例中會導致 「 Dinner 」 類別。 此類別命名可協助讓我們的模型與.NET 命名慣例，一致，並通常找到該設計工具的修正這向上方便 （尤其是在新增時遇到大量資料表）。 如果您不喜歡的類別或設計工具產生，但您一律可以覆寫它，並將它變更為任何您想要的名稱的屬性名稱。 您可以藉由編輯實體/屬性名稱內嵌在設計工具內，或藉由修改透過屬性方格。

根據預設，LINQ to SQL 設計工具也會檢查主索引鍵/外部索引鍵關聯性的資料表，並根據這些自動會建立預設 」 關聯性的關聯"，它會建立不同的模型類別之間。 例如，當我們拖曳 Dinners 和 RSVP 到 LINQ to SQL 設計工具上一對多關聯性之間的關聯這兩個資料表推斷基礎是以 RSVP 資料表有外部索引鍵 Dinners 資料表 (這會由中的箭號設計工具）：

![](build-a-model-with-business-rule-validations/_static/image6.png)

上述的關聯將會使 LINQ to SQL 將強型別 「 Dinner"屬性新增至開發人員可用來存取指定的 RSVP 相關聯的 Dinner RSVP 類別。 它也會導致 Dinner 類別擁有 「 RSVPs"集合屬性，可讓開發人員，以擷取和更新特定的 Dinner 與相關聯的 RSVP 物件。

以下您可以看到範例的 Visual Studio 中的 intellisense，當我們建立新的 RSVP 物件，並將它新增至 Dinner 像是集合。 請注意如何 LINQ to SQL 會自動加入 「 像是 「 集合 Dinner 物件上：

![](build-a-model-with-business-rule-validations/_static/image7.png)

藉由將 Dinner 像是集合中的 RSVP 物件中，我們會告訴 LINQ to SQL 建立外部索引鍵之間的關聯性 Dinner RSVP 資料列，在資料庫中的關聯：

![](build-a-model-with-business-rule-validations/_static/image8.png)

如果您不喜歡如何設計工具已建立模型，或名為資料表關聯，您可以覆寫它。 只要按一下設計工具內的關聯箭號，並存取透過屬性方格中，重新命名、 刪除或修改其屬性。 NerdDinner 應用程式，不過，預設關聯規則適用於我們所建置的資料模型類別，我們可以直接使用的預設行為。

### <a name="nerddinnerdatacontext-class"></a>NerdDinnerDataContext 類別

Visual Studio 會自動建立.NET 類別，代表的模型和資料庫使用 LINQ to SQL 設計工具定義的關聯性。 LINQ to SQL DataContext 類別也會產生每個 linq to SQL 設計工具檔案加入至方案。 我們名為我們的 LINQ to SQL 類別項目 」 NerdDinner"，因為建立的 DataContext 類別就會呼叫 「 NerdDinnerDataContext"。 這個 NerdDinnerDataContext 類別是我們將會與資料庫互動的主要方式。

我們 NerdDinnerDataContext 類別會公開兩個屬性-"Dinners"和"RSVPs-"，表示我們模型資料庫中的兩個資料表。 我們可以使用 C# 來從資料庫查詢和擷取 Dinner 和 RSVP 的物件寫入 LINQ 查詢，對這些屬性。

下列程式碼示範如何 NerdDinnerDataContext 物件具現化並執行比對以取得一連串的是發生在未來的 Dinners LINQ 查詢。 撰寫 LINQ 查詢時，visual Studio 會提供完整的 intellisense，並從它傳回的物件是強型別，而且還支援 intellisense:

![](build-a-model-with-business-rule-validations/_static/image9.png)

讓我們吃晚餐和 RSVP 物件查詢，除了 NerdDinnerDataContext 也會自動追蹤我們接下來我們透過它來擷取 Dinner 和 RSVP 物件所做的任何變更。 若要輕鬆地將變更儲存回資料庫-而不需要撰寫任何明確的 SQL 更新程式碼，我們可以使用這項功能。

例如，下列程式碼會示範如何使用 LINQ 查詢來擷取資料庫中的單一 Dinner 物件，更新兩個 Dinner 屬性中，然後將變更儲存回資料庫：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

NerdDinnerDataContext 物件，在上述程式碼中的自動追蹤對我們從它擷取 Dinner 物件的屬性變更。 當我們呼叫 「 SubmitChanges() 」 方法時，它會執行適當 SQL 「 更新 」 陳述式的資料庫來保存回更新的值。

### <a name="creating-a-dinnerrepository-class"></a>建立 DinnerRepository 類別

對於小型應用程式可能會很只有控制站直接用於 LINQ to SQL DataContext 類別和內嵌控制器中的 LINQ 查詢。 應用程式變得越來越大，不過，這個方法會變得難以維護和測試。 它也可能會導致我們複製相同的 LINQ 查詢，在多個位置。

可讓您更輕鬆地維護和測試的應用程式的其中一個方法是使用 「 存放庫 」 模式。 儲存機制類別可協助封裝查詢的資料，並持續性邏輯和摘要了從應用程式的資料持續性的實作詳細資料。 除了提供更簡潔的應用程式程式碼，使用儲存機制模式可以讓您更輕鬆地在未來，變更資料儲存區實作，它可協助簡化單元測試應用程式，而不需要實際的資料庫。

NerdDinner 應用程式中，我們會定義 DinnerRepository 類別具有下列簽章：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*附註： 在本章稍後我們將從這個類別來擷取 IDinnerRepository 介面並啟用我們控制站上使用它的相依性插入。一開始，不過，我們要輕鬆入門，並只使用直接 DinnerRepository 類別。*

若要實作此類別，我們將我們的 「 模型 」 資料夾上按一下滑鼠右鍵，然後選擇**Add-&gt;新的項目**功能表命令。 在 「 新增新的項目 對話方塊中，我們會選取 「 類別 」 範本，並將檔案命名為"DinnerRepository.cs 」:

![](build-a-model-with-business-rule-validations/_static/image10.png)

然後，我們可以實作 DinnerRespository 類別使用下列程式碼：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>擷取、 更新、 插入和刪除使用 DinnerRepository 類別

現在，我們建立我們 DinnerRepository 類別，讓我們看看一些程式碼範例示範常見的工作，我們可以用它做：

#### <a name="querying-examples"></a>查詢範例

下列程式碼會擷取單一的 Dinner 使用 DinnerID 值：


[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

下列程式碼會擷取所有即將推出的 dinners 和迴圈它們：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>插入和更新範例

下列程式碼示範如何將兩個新 dinners。 在其上呼叫"Save"（） 方法之前，加入/修改儲存機制不認可到資料庫。 讓所有的變更發生，或都不執行我們的存放庫儲存時，LINQ to SQL 會將所有變更會自動都包裝在資料庫交易 –:

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

下列程式碼會擷取現有的 Dinner 物件，並修改在其上的兩個屬性。 我們的存放庫上呼叫"Save"（） 方法時，會認可回資料庫變更：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

下列程式碼擷取 dinner，然後將加入其中的 RSVP。 它會使用像是集合 （因為兩者在資料庫中沒有主索引鍵/外部索引鍵關聯性） 為我們建立 LINQ to SQL Dinner 物件上。 這項變更會保存回資料庫為新的 RSVP 資料表資料列存放庫上呼叫"Save"（） 方法時：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>Delete 範例

下列程式碼會擷取現有的 Dinner 物件，並隨後會將標示要刪除它。 存放庫上呼叫"Save"（） 方法時它就會回到資料庫認可刪除：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>驗證和商務規則邏輯整合模型類別

整合驗證和商務規則邏輯是任何會使用資料的應用程式的重要部分。

#### <a name="schema-validation"></a>結構描述驗證

模型類別定義使用 LINQ to SQL 設計工具，在資料模型類別中屬性的資料型別會對應到資料庫資料表的資料類型。 例如:"datetime"Dinners 資料表中的"EventDate 」 資料行時，LINQ to SQL 所建立的資料模型類別都屬於類型"DateTime"（這是內建的.NET 資料類型）。 這表示如果您嘗試從程式碼中將整數或布林值指派給它，而且它會引發錯誤自動如果您嘗試在執行階段隱含地轉換成它的非有效的字串類型，您會收到編譯錯誤。

LINQ to SQL 將也會自動逸出 SQL 值會為您處理時使用的字串，以協助保護您 SQL 資料隱碼攻擊時使用它。

#### <a name="validation-and-business-rule-logic"></a>驗證和商務規則邏輯

結構描述驗證方法可以當做第一個步驟中，但很少足夠。 大部分的真實世界案例需要能夠指定更豐富的驗證邏輯，可以跨多個屬性、 執行程式碼，並且通常會有意識的模型狀態 (例如： 它正在建立/更新/刪除，或在網域專屬的狀態例如 「 封存 」）。 有各種不同的模式和架構，可用來定義，並將驗證規則套用至模型類別，而且有數個.NET 型架構有可用來協助進行這。 您可以使用幾乎任何的 ASP.NET MVC 應用程式內。

基於我們 NerdDinner 應用程式的目的，我們將使用相對較簡單且簡單的模式，我們會公開 IsValid 屬性和 GetRuleViolations() 方法在 Dinner 模型物件。 IsValid 屬性會傳回 true 或 false 取決於驗證和商務規則是否有效。 GetRuleViolations() 方法會傳回一份任何規則錯誤。

我們將為我們的 Dinner 模型實作 IsValid 及 GetRuleViolations()，藉由將我們的專案中的 「 部分類別 」。 部分類別可以用來將方法/屬性/事件加入至類別 （例如 LINQ to SQL 設計工具所產生的 Dinner 類別） 的 VS 設計工具所維護，並協助避免弄亂與我們的程式碼的工具。 我們可以將新的部分類別新增至我們的專案中，\Models 資料夾上按一下滑鼠右鍵，然後選取 ["新增新的項目] 功能表命令。 我們可以接著選擇 新增新的項目 」 對話方塊 「 類別 」 範本，並將它命名為 Dinner.cs 中。

![](build-a-model-with-business-rule-validations/_static/image11.png)

按一下 [新增] 按鈕後，會將 Dinner.cs 檔案新增至我們的專案，並在 IDE 中開啟它。 我們可以實作基本的規則/驗證強制執行 framework 使用下列程式碼：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

關於上述程式碼的一些注意事項：

- 晚餐類別開頭處以 「 部分 」 關鍵字 – 這表示其內所含的程式碼會結合的 LINQ to SQL 設計工具的產生和保留的類別，並編譯成單一類別。
- RuleViolation 類別是協助程式類別，我們會將它新增至專案，讓我們能夠提供更多詳細的規則違規。
- Dinner.GetRuleViolations() 方法會導致我們的驗證和商務規則，以進行評估 （我們將實作它們很快）。 它接著會傳回上一步一系列提供的任何規則錯誤的更多詳細的 RuleViolation 物件。
- Dinner.IsValid 屬性提供便利的協助程式屬性，指出 Dinner 物件是否有任何作用中的 RuleViolations。 它可以主動檢查隨時使用 Dinner 物件的開發人員 （並不會引發例外狀況）。
- Dinner.OnValidate() 部分方法會攔截程序，LINQ to SQL 提供可讓我們將會收到通知，每當 Dinner 物件即將保存在資料庫中。 我們上面的 OnValidate() 實作可確保 Dinner 在儲存之前，會有任何 RuleViolations。 如果它處於無效狀態，便會產生例外狀況，這會導致 LINQ to SQL 中止交易。

此方法提供一種簡單的架構，我們可以整合驗證和商務規則到。 現在讓我們加入我們 GetRuleViolations() 方法的規則如下：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

若要傳回的任何 RuleViolations 序列，我們會使用"yield return"C# 功能。 上述的第六個規則檢查只會強制執行字串屬性，在我們吃晚餐不能為 null 或空白。 最後一個規則會更有趣的是，和呼叫，我們可以新增至我們的專案，以確認 ContactPhone PhoneValidator.IsValidNumber() helper 方法的數字格式相符項目 Dinner 的國家/地區。

我們可以使用。若要實作此電話驗證支援的 NET 的規則運算式支援。 以下是簡單的 PhoneValidator 實作，我們可以新增至我們的專案，可讓我們新增 國家/地區特定的 Regex 模式檢查：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>處理驗證和商務邏輯違規

既然我們已新增上述驗證和商務規則的程式碼，我們嘗試建立或更新 Dinner 任何時候，我們的驗證邏輯規則會進行評估，並強制執行。

開發人員可以撰寫程式碼，像下面以主動地判斷 Dinner 物件是否有效，並擷取一份所有違規裡面，而不會引發任何例外狀況：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

如果我們嘗試以無效的狀態儲存 Dinner，DinnerRepository 上呼叫 save （） 方法時，就會引發例外狀況。 這是因為 LINQ to SQL 會自動呼叫我們 Dinner.OnValidate() 部分方法，它會將儲存 Dinner 的變更，和我們的程式碼加入引發例外狀況，如果任何規則違規在於 Dinner Dinner.OnValidate() 之前。 我們可以攔截此例外狀況和被動擷取一份的違規情事，若要修正問題：

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

在我們的模型層，並不在 UI 層中實作我們驗證和商務規則，因為它們會套用並在所有情況下，我們的應用程式內使用。 我們可以稍後變更或新增商務規則，我們 Dinner 物件適用於接受它們的所有程式碼。

有彈性地變更商務規則，在同一個地方，而不需 ripple 在整個應用程式和 UI 邏輯，這些變更是撰寫得當的應用程式，以及的 MVC 架構有助於鼓勵的優點。

### <a name="next-step"></a>下一個步驟

我們現在有一個模型，我們可以使用查詢和更新資料庫。

讓我們現在新增一些控制器和檢視的專案，我們可以使用來建置其周圍的 HTML UI 體驗。

> [!div class="step-by-step"]
> [上一頁](create-a-database.md)
> [下一頁](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
