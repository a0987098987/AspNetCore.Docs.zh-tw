---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: 設定檔、 佈景主題和 Web 組件 |Microsoft Docs
author: microsoft
description: 有一些組態中的重大變更和 ASP.NET 2.0 中的檢測。 新的 ASP.NET 組態 API 可讓進行 pr 的組態變更...
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: fa4405302f5b813dc3b99f612ef8efc8793cd6c8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815684"
---
<a name="profiles-themes-and-web-parts"></a>設定檔、 佈景主題和 Web 組件
====================
by [Microsoft](https://github.com/microsoft)

> 有一些組態中的重大變更和 ASP.NET 2.0 中的檢測。 新的 ASP.NET 組態 API 來以程式設計方式進行組態變更。 此外，許多新的組態設定存在於允許新的組態和檢測。


ASP.NET 2.0 表示個人化的網站區域中大幅提升。 除了成員資格功能將已涵蓋，ASP.NET 設定檔、 佈景主題和 Web 組件大幅會增強在網站中的個人化。

## <a name="aspnet-profiles"></a>ASP.NET 設定檔

ASP.NET 設定檔的類似工作階段。 差別在於設定檔是持續性，而瀏覽器關閉時，會遺失工作階段。 工作階段與設定檔之間的另一個大的差異是，設定檔強型別，因此能提供較 IntelliSense 在開發程序。

機器組態檔或應用程式的 web.config 檔案中定義的設定檔。 （您無法在子資料夾的 web.config 檔案中定義的設定檔）。下列程式碼會定義要先儲存網站訪客和姓氏的設定檔。

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

設定檔屬性的預設資料類型為 System.String。 在上述範例中，不指定任何資料型別。 因此 FirstName 和 LastName 屬性都屬於字串類型。 如先前所述，設定檔屬性強型別。 下列程式碼會將新屬性的型別 Int32 的時代。

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

設定檔通常會搭配 ASP.NET 表單驗證。 每位使用者中與表單驗證搭配使用時，有不同的設定檔相關聯，並提供其使用者識別碼。 不過，它也可允許使用匿名應用程式使用的設定檔&lt;anonymousIdentification&gt;連同組態檔中的項目**allowAnonymous**做為屬性如下所示：

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

當匿名使用者瀏覽網站時，ASP.NET 建立的執行個體**ProfileCommon**使用者。 此設定檔會使用儲存在 cookie 中的瀏覽器上的唯一識別碼，將使用者識別為唯一的訪客。 如此一來，您可以儲存匿名瀏覽的使用者設定檔資訊。

## <a name="profile-groups"></a>設定檔群組

您可將設定檔屬性群組。 群組的屬性，就可以模擬特定的應用程式的多個設定檔。

下列組態會設定兩個群組; 的 FirstName 和 LastName 屬性買主與潛在客戶。

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

就可以設定屬性的特定群組，如下所示：

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>儲存複雜物件

到目前為止，我們所討論的範例有儲存設定檔中的簡單資料型別。 您也可儲存設定檔中的複雜資料型別，藉由指定序列化使用的方法**serializeAs**屬性，如下所示：

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

在此情況下，此類型為 PurchaseInvoice。 PurchaseInvoice 類別必須標示為可序列化，而且可以包含任意數目的屬性。 例如，如果 PurchaseInvoice 有屬性，稱為**NumItemsPurchased**，可以為該屬性在程式碼中參考，如下所示：

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>設定檔的繼承

您可使用的設定檔在多個應用程式。 藉由建立衍生自 ProfileBase profile 類別，您可以使用重複使用數個應用程式中的設定檔**繼承**屬性，如下所示：

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

在此情況下，類別**PurchasingProfile**看起來會像這樣：

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>設定檔提供者

ASP.NET 設定檔使用的提供者模型。 預設的提供者會將資訊儲存在應用程式中的 SQL Server Express 資料庫\_使用 SqlProfileProvider 提供者的 Web 應用程式的 [資料] 資料夾。 如果資料庫不存在，ASP.NET 會自動建立它時的設定檔，嘗試儲存資訊。

不過，在某些情況下，您可能想開發您自己的設定檔提供者。 ASP.NET 設定檔功能可讓您輕鬆地使用不同的提供者。

建立自訂設定檔提供者時：

- 您需要將設定檔資訊儲存在資料來源，例如 FoxPro 資料庫或 Oracle 資料庫中，不支援內含在.NET Framework 的設定檔提供者所示。
- 您要管理使用不同於.NET Framework 隨附的提供者所使用的資料庫結構描述的資料庫結構描述的設定檔資訊。 常見範例是您想要整合現有的 SQL Server 資料庫中的使用者資料設定檔資訊。

### <a name="required-classes"></a>必要的類別

若要實作設定檔提供者，您可以建立繼承 System.Web.Profile.ProfileProvider 抽象類別的類別。 **ProfileProvider**抽象類別是繼承 System.Configuration.SettingsProvider 抽象類別，繼承 System.Configuration.Provider.ProviderBase 抽象類別。 因為這個繼承鏈結，除了所需的成員**ProfileProvider**類別，您必須實作的所需的成員**SettingsProvider**和**ProviderBase**類別。

下表描述的屬性和方法，您必須從實作**ProviderBase**， **SettingsProvider**，並**ProfileProvider**抽象類別。

### <a name="providerbase-members"></a>ProviderBase 成員

| **成員** | **描述** |
| --- | --- |
| Initialize 方法 | 當成輸入提供者執行個體的名稱和組態設定為 NameValueCollection。 用來設定選項和提供者執行個體，包括實作特定值和選項在機器組態或 Web.config 檔案中指定的屬性值。 |

### <a name="settingsprovider-members"></a>SettingsProvider 成員

| **成員** | **描述** |
| --- | --- |
| ApplicationName 屬性 | 每個設定檔會儲存應用程式名稱。 設定檔提供者會使用應用程式名稱來儲存設定檔資訊，分別為每個應用程式。 這可讓多個 ASP.NET 應用程式使用相同的資料來源，而不發生衝突，如果相同的使用者名稱在不同的應用程式建立。 或者，多個 ASP.NET 應用程式可以共用的設定檔資料來源指定相同的應用程式名稱。 |
| GetPropertyValues 方法 | 使用做為輸入的 SettingsContext 和 SettingsPropertyCollection 物件。 **SettingsContext**提供使用者的相關資訊。 您可以使用主索引鍵的資訊來擷取使用者設定檔屬性資訊。 使用**SettingsContext**来取得的使用者名稱和使用者是否已驗證或匿名物件。 **SettingsPropertyCollection**包含 SettingsProperty 物件的集合。 每個**SettingsProperty**物件提供的名稱和類型的屬性，以及其他資訊，例如屬性和屬性是唯讀的預設值。 **GetPropertyValues**方法為基礎的 SettingsPropertyValue 物件中填入 SettingsPropertyValueCollection **SettingsProperty**物件提供作為輸入。 從指定的使用者的資料來源的值時，會指派給 PropertyValue 屬性上，針對每個**SettingsPropertyValue**會傳回物件和整個集合。 呼叫方法也會更新指定的使用者設定檔的 LastActivityDate 值為目前的日期和時間。 |
| SetPropertyValues 方法 | 輸入當成**SettingsContext**並**SettingsPropertyValueCollection**物件。 **SettingsContext**提供使用者的相關資訊。 您可以使用主索引鍵的資訊來擷取使用者設定檔屬性資訊。 使用**SettingsContext**来取得的使用者名稱和使用者是否已驗證或匿名物件。 **SettingsPropertyValueCollection**包含的集合**SettingsPropertyValue**物件。 每個**SettingsPropertyValue**物件提供名稱、 類型和值的屬性，以及其他資訊，例如屬性和屬性是唯讀的預設值。 **SetPropertyValues**方法會更新指定之使用者的資料來源中的設定檔屬性值。 呼叫此方法也更新**LastActivityDate**和 LastUpdatedDate 值，指定的使用者設定檔的目前日期和時間。 |

### <a name="profileprovider-members"></a>ProfileProvider 成員

| **成員** | **描述** |
| --- | --- |
| DeleteProfiles 方法 | 其中的應用程式名稱必須符合做為輸入字串陣列的使用者名稱和從資料來源中刪除所有設定檔資訊和屬性的值指定的名稱會採用**ApplicationName**屬性值。 如果您的資料來源支援交易，則建議您在交易中包含所有的刪除作業，以及您回復交易並擲回例外狀況，如果任何刪除作業失敗。 |
| DeleteProfiles 方法 | 使用做為輸入的 ProfileInfo 集合物件，並從資料來源中刪除所有設定檔資訊和屬性值的每個設定檔其中應用程式名稱必須符合**ApplicationName**屬性值。 如果您的資料來源支援交易，則建議您在交易中包含所有的刪除作業和回復交易並擲回例外狀況，如果任何刪除作業失敗。 |
| DeleteInactiveProfiles 方法 | 將輸入 ProfileAuthenticationOption 值的 DateTime 物件，並刪除資料來源的所有設定檔資訊和屬性值的上次活動日期是小於或等於指定的日期和時間，而且其中的應用程式名稱比對**ApplicationName**屬性值。 **ProfileAuthenticationOption**參數會指定是否只是匿名的設定檔，只會驗證設定檔，或刪除所有設定檔。 如果您的資料來源支援交易，則建議您在交易中包含所有的刪除作業和回復交易並擲回例外狀況，如果任何刪除作業失敗。 |
| GetAllProfiles 方法 | 輸入當成**ProfileAuthenticationOption**值、 整數，指定的頁面索引、 指定頁面大小，以及參考將會設定為設定檔的總計數的整數的整數。 傳回包含 ProfileInfoCollection **ProfileInfo**物件，其中應用程式名稱必須符合資料來源中的所有設定檔**ApplicationName**屬性值。 **ProfileAuthenticationOption**參數會指定是否只是匿名的設定檔，只會驗證設定檔，或會傳回所有設定檔。 所傳回的結果**GetAllProfiles**方法會受限於頁面的索引和頁面大小值。 頁面大小值會指定最大數目**ProfileInfo**中所傳回的物件**ProfileInfoCollection**。 頁面索引值會指定要傳回，其中 1 代表第一頁的結果頁面。 記錄總數的參數是輸出參數 (您可以使用**ByRef** Visual Basic 中) 設定設定檔的總數。 例如，如果資料存放區包含 13 應用程式的設定檔和頁面的索引值為 5，以頁面大小的 2 **ProfileInfoCollection**傳回包含第 6 到第十個設定檔。 方法傳回時，會記錄總數值設定為 13。 |
| GetAllInactiveProfiles 方法 | 輸入當成**ProfileAuthenticationOption**的值， **DateTime**物件、 指定頁面索引的整數、 整數，指定頁面大小，並將設定為整數的參考若要設定檔的總計數。 傳回**ProfileInfoCollection** ，其中包含**ProfileInfo**物件，其中的上次活動日期是小於或等於指定的資料來源中的所有設定檔**日期時間** ，其中的應用程式名稱必須符合**ApplicationName**屬性值。 **ProfileAuthenticationOption**參數會指定是否只是匿名的設定檔，只會驗證設定檔，或會傳回所有設定檔。 所傳回的結果**GetAllInactiveProfiles**方法會受限於頁面的索引和頁面大小值。 頁面大小值會指定最大數目**ProfileInfo**中所傳回的物件**ProfileInfoCollection**。 頁面索引值會指定要傳回，其中 1 代表第一頁的結果頁面。 記錄總數的參數是輸出參數 (您可以使用**ByRef** Visual Basic 中) 設定設定檔的總數。 例如，如果資料存放區包含 13 應用程式的設定檔和頁面的索引值為 5，以頁面大小的 2 **ProfileInfoCollection**傳回包含第 6 到第十個設定檔。 方法傳回時，會記錄總數值設定為 13。 |
| FindProfilesByUserName 方法 | 輸入當成**ProfileAuthenticationOption**值字串，包含使用者名稱、 指定頁面索引的整數、 整數，指定頁面大小，以及將會設定為總數的整數的參考設定檔。 傳回**ProfileInfoCollection** ，其中包含**ProfileInfo**物件的使用者名稱符合指定的使用者名稱的位置，其中的應用程式名稱必須符合，來源資料中的所有設定檔**ApplicationName**屬性值。 **ProfileAuthenticationOption**參數會指定是否只是匿名的設定檔，只會驗證設定檔，或會傳回所有設定檔。 如果您的資料來源支援的其他搜尋功能，例如萬用字元，您可以提供更豐富的搜尋功能的使用者名稱。 所傳回的結果**FindProfilesByUserName**方法會受限於頁面的索引和頁面大小值。 頁面大小值會指定最大數目**ProfileInfo**中所傳回的物件**ProfileInfoCollection**。 頁面索引值會指定要傳回，其中 1 代表第一頁的結果頁面。 記錄總數的參數是輸出參數 (您可以使用**ByRef** Visual Basic 中) 設定設定檔的總數。 例如，如果資料存放區包含 13 應用程式的設定檔和頁面的索引值為 5，以頁面大小的 2 **ProfileInfoCollection**傳回包含第 6 到第十個設定檔。 方法傳回時，會記錄總數值設定為 13。 |
| FindInactiveProfilesByUserName 方法 | 輸入當成**ProfileAuthenticationOption**值、 字串，包含使用者名稱， **DateTime**物件、 指定頁面索引的整數、 整數，指定頁面大小，和參考為將會設定為設定檔的總計數的整數。 傳回**ProfileInfoCollection** ，其中包含**ProfileInfo**其中的使用者名稱符合指定的使用者名稱，其中的上次活動日期是資料來源中的所有設定檔的物件少於或為指定的相等**DateTime**，，其中的應用程式名稱必須符合**ApplicationName**屬性值。 **ProfileAuthenticationOption**參數會指定是否只是匿名的設定檔，只會驗證設定檔，或會傳回所有設定檔。 如果您的資料來源支援的其他搜尋功能，例如萬用字元，您可以提供更豐富的搜尋功能的使用者名稱。 所傳回的結果**FindInactiveProfilesByUserName**方法會受限於頁面的索引和頁面大小值。 頁面大小值會指定最大數目**ProfileInfo**中所傳回的物件**ProfileInfoCollection**。 頁面索引值會指定要傳回，其中 1 代表第一頁的結果頁面。 記錄總數的參數是輸出參數 (您可以使用**ByRef** Visual Basic 中) 設定設定檔的總數。 例如，如果資料存放區包含 13 應用程式的設定檔和頁面的索引值為 5，以頁面大小的 2 **ProfileInfoCollection**傳回包含第 6 到第十個設定檔。 方法傳回時，會記錄總數值設定為 13。 |
| GetNumberOfInActiveProfiles 方法 | 當成輸入**ProfileAuthenticationOption**值並**DateTime**物件，並傳回其中的上次活動日期是小於或等於指定資料來源中的所有設定檔的計數**日期時間**，其中的應用程式名稱必須符合**ApplicationName**屬性值。 **ProfileAuthenticationOption**參數會指定是否只是匿名的設定檔，只會驗證設定檔，或所有的設定檔是要計算個數。 |

### <a name="applicationname"></a>ApplicationName

由於設定檔提供者儲存設定檔資訊，分別為每個應用程式，您必須確定您的資料結構描述包含應用程式名稱，而且查詢和更新也包含應用程式名稱。 比方說，下列命令來擷取屬性值從資料庫為基礎的使用者名稱，以及設定檔是匿名的並可確保**ApplicationName**值會包含在查詢中。

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>ASP.NET 佈景主題

## <a name="what-are-aspnet-20-themes"></a>ASP.NET 2.0 主題有哪些？

其中一個最重要的層面，Web 應用程式的跨站台是一致的外觀及操作。 ASP.NET 1.x 開發人員通常會使用階層式樣式表 (CSS) 來實作一致的外觀及操作。 ASP.NET 2.0 主題能大為改善 CSS 是因為它讓 ASP.NET 開發人員能夠定義 ASP.NET 伺服器控制項，以及 HTML 元素的外觀。 ASP.NET 佈景主題可以套用至個別的控制項、 特定的網頁或整個 Web 應用程式中。 如果映像所需，佈景主題會使用 CSS 檔案、 選擇性面板檔案和選擇性的映像目錄的組合。 Soubor skinu 控制 ASP.NET 伺服器控制項的視覺外觀。

## <a name="where-are-themes-stored"></a>在哪裡？ 佈景主題儲存

儲存佈景主題的位置不同根據它們的範圍。 可以套用至任何應用程式的佈景主題會儲存在下列資料夾：

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

是特定的應用程式特有的佈景主題儲存在`App\_Themes\<Theme\_Name>`目錄中的網站的根目錄。

> [!NOTE]
> Soubor skinu 只應修改會影響外觀的伺服器控制項屬性。

全域主題是可以套用至任何應用程式或 Web 伺服器上執行的網站的佈景主題。 這些主題會儲存 ASP.NETClientfiles\Themes 目錄內的 v2.x.xxxxx 目錄中的預設值。 或者，您可以在這裡將佈景主題檔案移到 aspnet\_用戶端/系統\_web / [version] /Themes/ [佈景主題\_名稱] 中您網站的根資料夾。

應用程式特定的佈景主題只能套用至檔案所在的應用程式。 這些檔案會儲存在`App\_Themes/<theme\_name>`目錄中的網站的根目錄。

## <a name="the-components-of-a-theme"></a>佈景主題的元件

佈景主題是組成一或多個 CSS 檔案、 選擇性面板檔案，以及選擇性的 [影像] 資料夾。 CSS 檔案可以是任何您想 （也就是 default.css 或 theme.css 等），而且必須位於佈景主題資料夾的根目錄中的名稱。 CSS 檔案用來定義一般的 CSS 類別和特定的選取器屬性。 要套用的 CSS 類別的其中一個頁面項目**CSSClass**屬性使用。

面板檔案是包含為 ASP.NET 伺服器控制項的屬性定義的 XML 檔案。 下面所列的程式碼是範例面板檔案。

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

**圖 1**以下顯示一個小的 ASP.NET 網頁瀏覽沒有套用佈景主題。 **圖 2**套用佈景主題會顯示相同的檔案。 透過 CSS 檔案已背景色彩和文字色彩。 使用上面所列的面板檔案設定按鈕和文字方塊的外觀。


![無佈景主題](profiles-themes-and-web-parts/_static/image1.gif)

**圖 1**： 無佈景主題


![套用佈景主題](profiles-themes-and-web-parts/_static/image2.gif)

**圖 2**： 套用佈景主題


以上所列的面板檔案定義所有文字方塊控制項和按鈕控制項的預設的外觀。 這表示每個 TextBox 控制項和插入到網頁上的按鈕控制項將會對此外觀。 您也可以定義可套用至特定的執行個體使用這些控制項的外觀**SkinID**控制項的屬性。

下列程式碼定義的按鈕控制項的外觀。 只有按鈕控制項**SkinID**屬性**goButton**需要面板的外觀。

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

您只能有一個預設面板，每個伺服器控制項型別。 如果您需要額外的面板，您應該使用 SkinID 屬性。

## <a name="applying-themes-to-pages"></a>將佈景主題套用至頁面

您可以使用任何一種下列方法套用佈景主題：

- 在 &lt;頁&gt;web.config 檔案的項目
- 在 @Page頁面指示詞
- 以程式設計的方式

## <a name="applying-a-theme-in-the-configuration-file"></a>套用組態檔中的佈景主題

若要套用的佈景主題，應用程式組態檔中，使用下列語法：

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

在這裡指定佈景主題名稱必須符合佈景主題資料夾的名稱。 此資料夾可以存在於其中任何一種本課程稍早所述的位置。 如果您嘗試套用佈景主題不存在，則會發生組態錯誤。

## <a name="applying-a-theme-in-the-page-directive"></a>套用佈景主題在 Page 指示詞

您也可以套用在 @ Page 指示詞中的主題。 這個方法可讓您使用特定網頁的佈景主題。

若要套用佈景主題@Page指示詞，請使用下列語法：

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

同樣地，在這裡指定的佈景主題必須符合的佈景主題資料夾，如先前所述。 如果您嘗試套用佈景主題不存在，則會發生組建失敗。 Visual Studio 也會反白顯示的屬性，並通知您這類的佈景主題存在。

## <a name="applying-a-theme-programmatically"></a>以程式設計方式套用的佈景主題

若要以程式設計方式套用佈景主題，您必須指定**佈景主題**屬性中的頁面**頁\_PreInit**方法。

若要以程式設計方式套用佈景主題，請使用下列語法：

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

就必須套用佈景主題中的 PreInit 方法，因為網頁生命週期。 如果您套用它在該點之後，網頁佈景主題會已經套用執行階段，並變更在該點已經太遲生命週期中。 如果您套用佈景主題不存在**HttpException** ，就會發生。 以程式設計方式套用佈景主題時，如果任何伺服器控制項已經指定 SkinID 屬性，會發生建置警告。 這項警告被要通知您任何佈景主題以宣告方式套用，並可以忽略它。

## <a name="exercise-1--applying-a-theme"></a>練習 1： 套用佈景主題

在這個練習中，您會將 ASP.NET 佈景主題套用至網站。

> [!IMPORTANT]
> 如果您使用 Microsoft Word 來輸入面板檔案中的資訊，請確定您不會取代一般的引號與智慧引號。 智慧引號將會造成問題的面板檔案。

1. 建立新的 ASP.NET 網站。
2. 以滑鼠右鍵按一下方案總管] 中的專案，然後選擇 [加入新項目。
3. 從檔案的清單中選擇 Web 組態檔，並按一下 新增。
4. 以滑鼠右鍵按一下方案總管] 中的專案，然後選擇 [加入新項目。
5. 選擇面板檔案，然後按一下 [新增]。
6. 按一下 [是]，當系統詢問過您要將應用程式檔案放\_佈景主題資料夾。
7. 以滑鼠右鍵按一下應用程式內的 SkinFile 資料夾\_方案總管] 中的佈景主題資料夾，然後選擇 [加入新項目。
8. 從檔案的清單中選擇樣式表，並按一下 [新增]。 您現在擁有的所有檔案必須實作新的佈景主題。 不過，Visual Studio 具有名為您的佈景主題資料夾 SkinFile。 以滑鼠右鍵按一下該資料夾，並將名稱變更為 CoolTheme。
9. 開啟 SkinFile.skin 檔案並新增下列程式碼檔案的結尾： 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. 儲存 SkinFile.skin 檔案。
11. 開啟 StyleSheet.css。
12. 您可以將所有的文字，取代為下列： 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. 儲存 StyleSheet.css 檔案。
14. 開啟 Default.aspx 頁面。
15. 將 TextBox 控制項和按鈕控制項。
16. 儲存網頁。 現在瀏覽 Default.aspx 頁面。 它應該顯示為一般的 Web 表單。
17. 開啟 web.config 檔案。
18. 新增正下方開啟下列`<system.web>`標記： 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. 儲存 web.config 檔案。 現在瀏覽 Default.aspx 頁面。 它應該會顯示與套用的佈景主題。
20. 如果它尚未開啟，請在 Visual Studio 中開啟 Default.aspx 頁面。
21. 選取按鈕。
22. 變更**SkinID** goButton 的屬性。 請注意，Visual Studio 提供有效的 SkinID 值下拉式清單中的按鈕控制項。
23. 儲存網頁。 現在再次預覽您的瀏覽器頁面。 按鈕現在應該是 [執行]，並應更寬廣的外觀。

使用**SkinID**屬性，您可以輕鬆地設定不同的面板，特定類型的伺服器控制項的不同執行個體。

## <a name="the-stylesheettheme-property"></a>StyleSheetTheme 屬性

到目前為止，我們一直在討論只套用佈景主題使用佈景主題屬性。 使用佈景主題屬性時，面板檔案會覆寫伺服器控制項的任何宣告式設定。 比方說，在練習 1 中，指定 SkinID"goButton 」 按鈕控制項的和已變更至 [執行] 按鈕的文字。 您可能已經發現，在設計工具按鈕的 Text 屬性設定為 「 按鈕 」，但佈景主題已覆寫的。 佈景主題永遠會覆寫在設計工具中的任何屬性設定。

如果您想要能夠使用的佈景主題面板檔案中定義的屬性會覆寫屬性中指定設計工具中，您可以使用**StyleSheetTheme**而非佈景主題; 屬性的屬性。 StyleSheetTheme 屬性等同於佈景主題屬性不同之處在於它不會覆寫所有明確的屬性設定，例如佈景主題屬性。

按一下 `<pages>`顯示模式功能表，然後選取瀏覽返回瀏覽模式。

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

控制項現在有了更新的標題，並沒有框線，如下列螢幕擷取畫面所示。 已編輯的 Web 組件示範頁面

## <a name="overriding-themes"></a>Web 組件 VS 逐步解說 4 螢幕擷取畫面

全域的主題可覆寫相同名稱的應用程式中套用的佈景主題\_應用程式的佈景主題資料夾。 不過，佈景主題不會套用，則為 true 的覆寫案例。 如果執行階段發生在應用程式的佈景主題檔案\_佈景主題資料夾中，它會套用佈景主題使用這些檔案，並忽略全域的佈景主題。

StyleSheetTheme 屬性是可覆寫，而且可以覆寫在程式碼中，如下所示：

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>Web 組件

ASP.NET 網頁組件是網頁的一組整合的控制項來建立可讓終端使用者，若要修改的內容、 外觀和行為直接從瀏覽器的網站。 在網站上的所有使用者或個別使用者，可以套用所做的修改。 當使用者修改網頁及控制項時，可以儲存這些設定，來保留使用者的個人喜好設定，在未來的瀏覽器工作階段，名為個人化的功能。 這些 Web 組件功能表示開發人員可以讓使用者動態地個人化的 Web 應用程式，開發人員或系統管理員介入。

您身為開發人員可以使用 Web 組件控制項集合，讓使用者：

- 個人化頁面內容。 使用者可以將新的 Web 組件控制項加入頁面、 將它們移除、 隱藏它們，或將其降到最低像一般的 windows。
- 個人化頁面配置。 使用者可以在頁面上，將 Web 組件控制項拖曳到不同的區域，或變更其外觀、 屬性和行為。
- 匯出和匯入控制項。 使用者可以匯入或匯出 Web 組件控制項的設定以用於其他頁面或站台，保留內容、 外觀及甚至是控制項中的資料。 這會減少資料上使用者的項目和組態需求。
- 建立連線。 使用者可以建立控制項之間的連線以便比方說，chart 控制項時，可以在股票行情指示器控制項中顯示資料的圖形。 使用者可以個人化，不只連接本身，但的外觀和詳細資料的圖表控制項顯示資料的方式。
- 管理和個人化網站層級設定。 授權的使用者可以設定網站層級設定，決定誰可以存取網站或網頁、 控制項設定以角色為基礎的存取權等等。 比方說，系統管理角色中的使用者無法設定 Web 組件控制項的所有使用者共用，並避免使用者不是系統管理員從個人化共用的控制項。

您通常會使用 Web 組件中有三種： 建立的使用 Web 組件控制項的頁面，建立個別的 Web 組件控制項，或建立完整、 可個人化的 Web 應用程式，例如入口網站。

## <a name="page-development"></a>網頁程式開發

網頁程式開發人員可以使用視覺化設計工具，例如 Microsoft Visual Studio 2005 來建立使用 Web 組件的頁面。 在中使用的工具，例如 Visual Studio 為 Web 組件控制項集合的其中一個優點提供拖放建立及設定視覺化設計工具中的 Web 組件控制項的功能。 例如，您可以在其中使用設計工具，將 Web 組件的區域或 Web 組件編輯器控制項，拖曳到設計介面上，然後設定 向右調整控制項在設計工具中使用 Web 組件所提供的 UI 控制項集合。 這可以加快開發的 Web 組件的應用程式的速度並減少您必須撰寫的程式碼數量。

## <a name="control-development"></a>控制項開發

您可以使用任何現有的 ASP.NET 控制項做為 Web 組件控制項，包括標準的 Web 伺服器控制項、 自訂伺服器控制項和使用者控制項。 您環境的最大的程式設計控制項，您也可以建立衍生自 web 組件類別的自訂 Web 組件控制項。 適用於個別的 Web 組件控制項開發，您會通常建立使用者控制項並使用它做為 Web 組件控制項，或是開發自訂的 Web 組件控制項。

作為開發自訂的 Web 組件控制項的範例，您可以建立控制項提供任何其他可能有助於封裝做為可個人化的 Web 組件控制項的 ASP.NET 伺服器控制項所提供的功能： 行事曆、 清單、 財務資訊，新聞、 計算機、 rtf 文字控制項，更新內容，可編輯的格線，連接到資料庫圖表，動態更新其顯示，或天氣及旅遊資訊。 如果您與您的控制項提供視覺化設計工具，然後使用 Visual Studio 網頁開發人員可以只將您的控制項拖曳到網頁組件區域並將它設定在設計階段中，而不需要撰寫額外程式碼。

個人化是 Web 組件功能的基礎。 它可讓使用者修改，或個人化版面配置、 外觀及行為的頁面上的 Web 組件控制項。 個人化的設定是長時間執行： 不只是在目前的瀏覽器工作階段期間會保存這些 （如同檢視狀態），但也在長期的儲存體，以便在使用者的設定儲存，以供未來的瀏覽器工作階段。 Web 組件頁面的預設會啟用個人化。

UI 結構化元件依賴於個人化，並提供核心結構和服務所需的所有 Web 組件控制項。 每個 Web 組件頁面上所需的一個 UI 結構元件是 WebPartManager 控制項。 雖然不會看到這個控制項具有協調頁面上的所有 Web 組件控制項的重要工作。 比方說，它會追蹤所有個別的 Web 組件控制項。 它會管理 Web 組件區域 （包含網頁上的 Web 組件控制項的區域），以及哪些控制項是哪一個區域中。 它也會追蹤，並控制頁面可以是中，瀏覽、 連線、 編輯或目錄模式中，以及個人化變更套用至所有使用者或個別使用者不同的顯示模式。 最後，它會起始並追蹤 Web 組件控制項之間的連接和通訊。

第二個元件類型的 UI 結構是區域。 區域做為 Web 組件頁面上的配置管理員。 它們包含與組織組件類別 （組件控制項），衍生的控制項，並提供能夠進行水平或垂直方向的模組化的頁面配置。 區域也包含; 每個控制項為提供通用且一致的 UI 項目 （例如頁首及頁尾樣式、 標題、 框線樣式、 動作按鈕等等）這些常見的項目稱為控制項的色彩。 不同的顯示模式，並使用不同的控制項，則會使用數個特定的類型的區域。 不同類型的區域是由 Web 組件的基本控制項節所述。

Web 組件 UI 控制項，其中都是衍生自**一部分**類別中，包含 Web 組件頁面上的主要 UI。 Web 組件控制集具有彈性，內含在選項中它可讓您建立組件控制項。 除了建立您自己自訂的 Web 組件控制項，您也可以使用現有的 ASP.NET 伺服器控制項、 使用者控制項或自訂伺服器控制項做為 Web 組件控制項。 建立 Web 組件頁面最常用的基本控制項詳述於下一節。

## <a name="web-parts-essential-controls"></a>Web 組件不可或缺的控制項

Web 組件控制集很大，但某些控制項更為重要，因為它們所需的 Web 組件來運作，或是因為它們是最常用的 Web 組件頁面上的控制項。 當您開始使用 Web 組件，並建立基本的 Web 組件頁面，最好先熟悉基本的 Web 組件控制項下表中所述。

| **Web 組件控制項** | **描述** |
| --- | --- |
| WebPartManager | 管理頁面上的所有 Web 組件控制項。 一個 （且只有一個） **WebPartManager**的控制，則每個 Web 組件頁面。 |
| CatalogZone | 包含 CatalogPart 控制項。 使用此區域建立的使用者可以從中選取要新增至網頁的控制項的 Web 組件控制項的類別目錄。 |
| EditorZone | 包含 EditorPart 控制項。 您可以使用此區域，讓使用者能夠編輯及個人化頁面上的 Web 組件控制項。 |
| WebPartZone | 包含並提供整體的版面配置撰寫的頁面上的主要 UI 的網頁組件控制項。 當您建立與 Web 組件控制項的網頁時，請使用這個區域。 網頁可以包含一或多個區域。 |
| ConnectionsZone | 包含 WebPartConnection 控制項，並提供 UI 來管理連接。 |
| Web 組件 (GenericWebPart) | 呈現主要的 UI 中;大部分的 Web 組件 UI 控制項屬於此分類。 最大的程式設計控制項，您可以建立自訂的 Web 組件控制項衍生自基底**WebPart**控制項。 您也可以使用現有的伺服器控制項、 使用者控制項或自訂控制項做為 Web 組件控制項。 任何這些控制項放在區域中，每當**WebPartManager**控制項自動換行以**GenericWebPart**控制項在執行階段中，好讓您可以使用 Web 組件的功能。 |
| CatalogPart | 包含一份可用的使用者可以加入至網頁的 Web 組件控制項。 |
| WebPartConnection | 建立兩個頁面上的 Web 組件控制項之間的連線。 連接會定義其中一個 Web 組件的控制項為資料提供者 （），和其他取用者身分。 |
| EditorPart | 可特製化的編輯器控制項的基底類別。 |
| EditorPart 控制項 （AppearanceEditorPart、 LayoutEditorPart、 BehaviorEditorPart 和 PropertyGridEditorPart） | 允許使用者個人化頁面上的 Web 組件 UI 控制項的各個層面 |

## <a name="lab-create-a-web-part-page"></a>實驗室： 建立 Web 組件頁面

在此實驗室中，您將建立會持續透過 ASP.NET 設定檔資訊的網頁組件頁面。

### <a name="creating-a-simple-page-with-web-parts"></a>建立簡單的網頁與網頁組件

在這部分的逐步解說中，您可以建立使用 Web 組件控制項以顯示靜態內容的頁面。 使用 Web 組件的第一個步驟是建立一個具有兩個必要的結構化項目網頁。 首先，網頁組件必須追蹤，並協調所有的 Web 組件控制項 WebPartManager 控制項。 第二，在 Web 組件頁面需要一或多個區域，也就是複合控制項，其中包含 web 組件或其他伺服器控制項，並佔用網頁的指定的區域。

> [!NOTE]
> 您不需要採取任何動作來啟用 Web 組件個人化;它會啟用 Web 組件控制項集合的預設值。 當您第一次會執行站台上的 Web 組件頁面時，ASP.NET 會設定預設個人化提供者來儲存使用者個人化設定。 如需個人化的詳細資訊，請參閱 Web 組件個人化的概觀。


### <a name="to-create-a-page-for-containing-web-parts-controls"></a>若要建立包含 Web 組件的頁面控制項

1. 關閉 [預設] 頁面，並將新的頁面新增至名為 WebPartsDemo.aspx 的網站。
2. 若要切換**設計**檢視。
3. 從**檢視**功能表上，請確定**非視覺控制項**並**詳細資料**配置標記和沒有 UI 的控制項，您可以看到已選取的選項。
4. 將插入點之前`<div>`標籤設計介面，然後按 ENTER 即可加入新的一行。 將插入點，新行字元之前，請按一下**區塊格式**下拉式清單控制項之功能表上，然後選取**標題 1**選項。 在標題中，新增文字**網頁組件示範**。
5. 從**WebParts**  索引標籤的 工具箱 拖曳**WebPartManager**控制項拖曳到頁面上，只在新行字元之後再放置它`<div>`標記。   
  
   **WebPartManager**控制項不會呈現任何輸出，因此它會顯示為設計工具介面上的灰色方塊。
6. 放置插入點`<div>`標記。
7. 在 [**版面配置**] 功能表中，按一下**插入表格**，並建立新的資料表具有一個資料列和三個資料行。 按一下  **Cell Properties**按鈕，選取**頂端**從**垂直對齊**下拉式清單中，按一下**確定**，然後按一下**確定**再次用來建立資料表。
8. WebPartZone 控制項拖曳至左的資料表資料行。 以滑鼠右鍵按一下**WebPartZone**控制項中，選擇**屬性**，並設定下列屬性：   
  
   識別碼： SidebarZone   
  
   HeaderText： 資訊看板
9. 拖曳第二個**WebPartZone**控制項插入中間的資料表資料行，並設定下列屬性：   
  
   識別碼： MainZone   
  
   HeaderText： 主要
10. 儲存檔案。

您的網頁現在會有兩個不同的區域，您可以個別控制。 不過，兩個區域都任何內容，因此建立的內容是下一個步驟。 此逐步解說中，您使用僅顯示靜態內容的 Web 組件控制項。

指定 Web 組件區域的版面配置&lt;zonetemplate&gt;項目。 在區域樣板中中,，您可以新增任何的 ASP.NET 控制項，無論是自訂的 Web 組件控制項、 使用者控制項或現有的伺服器控制項。 請注意，此處使用 Label 控制項，而且，您只要加入靜態文字。 當您將在一般的伺服器控制項**WebPartZone**區域，ASP.NET 會將控制項視為 Web 組件控制項在執行階段，可讓控制項上的 Web 組件功能。

**若要建立主要區域的內容**

1. 在 **設計**檢視中，將**標籤**控制從**標準**到區域的 內容 區域的 工具箱 索引標籤其**識別碼**屬性設定為 MainZone。
2. 若要切換**來源**檢視。 請注意， &lt;zonetemplate&gt;項目已新增至包裝**標籤**MainZone 中的控制項。
3. 新增名為的屬性**標題**要&lt;asp: label&gt;項目，並將其值設定為內容。 移除文字 ="Label"屬性，從&lt;asp: label&gt;項目。 開頭和結尾標記之間&lt;asp: label&gt;項目，例如加入一些文字**歡迎使用我的首頁**內的一組&lt;h2&gt;項目標記。 您的程式碼應如下所示。 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. 儲存檔案。

接下來，建立使用者控制項，也可以加入至 Web 組件控制項與頁面。

### <a name="to-create-a-user-control"></a>建立使用者控制項

1. 將新的 Web 使用者控制項新增至您的站台做為搜尋控制項中。 取消選取的選項**置於個別的檔案中的原始碼**。 將它新增 WebPartsDemo.aspx 頁面上，與相同的目錄並將它命名為 SearchUserControl.ascx。   
  
    > [!NOTE]
    > 本逐步解說中的使用者控制項不會實作實際的搜尋功能;它只會用於示範 Web 組件的功能。
2. 若要切換**設計**檢視。 從**標準** 索引標籤的 工具箱 拖曳到頁面的文字方塊控制項。
3. 將插入點之後您剛才新增的文字方塊，然後按 ENTER 即可加入新的一行。
4. 拖曳到頁面的按鈕控制項，在您剛才加入的文字方塊底下的新行上。
5. 若要切換**來源**檢視。 請確定使用者控制項的原始程式碼，看起來如下列範例所示。 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. 儲存並關閉檔案。

現在您可以將 Web 組件控制項加入提要欄位區域。 您要加入兩個控制項的提要欄位區域，您在上一個程序中所建立的其中一個包含清單的連結，而另一個是使用者控制項。 連結會新增為標準**標籤**伺服器控制項，類似於您所建立的主要區域的靜態文字的方式。 不過，雖然中包含的個別伺服器控制項，但無法直接在此區域 （例如 label 控制項） 中包含使用者控制項，在此情況下不會。 相反地，它們是您在上一個程序中建立的使用者控制項的一部分。 這示範了封裝所有控制項和您想要在使用者控制項中的額外功能，然後再參考該控制項做為 Web 組件控制項的區域中的常見方式。

在執行階段，Web 組件控制項集合會包裝與 GenericWebPart 控制項的兩個控制項。 當**GenericWebPart**控制項包裝在 Web 伺服器控制項，泛型的組件控制項是父控制項，您可以透過父控制項的 ChildControl 屬性存取的伺服器控制項。 這種使用泛型組件控制項可讓具有相同的基本行為和屬性，為 Web 組件控制項衍生自標準的 Web 伺服器控制項**WebPart**類別。

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>若要將 Web 組件控制項新增至提要欄位區域

1. 開啟 WebPartsDemo.aspx 頁面。
2. 若要切換**設計**檢視。
3. 將使用者控制項您建立的網頁，SearchUserControl.ascx，從**方案總管**進入區域其**識別碼**屬性設定為 SidebarZone，並將它放那里。
4. 儲存 WebPartsDemo.aspx 頁面。
5. 若要切換**來源**檢視。
6. 內部&lt;asp: webpartzone&gt; SidebarZone，參考您的使用者控制項的正上方的項目加入&lt;asp: label&gt;具有項目所包含的連結，如下列範例所示。 此外，新增**標題**屬性設定為 使用者控制項的標記，其值為**搜尋**，如所示。 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. 儲存並關閉檔案。

現在您可以測試您的網頁瀏覽器中瀏覽至它。 此頁面會顯示兩個區域。 下列螢幕擷取畫面顯示的頁面。

**使用兩個區域的 web 組件示範頁面**


![Web 組件 VS 逐步解說 1 螢幕擷取畫面](profiles-themes-and-web-parts/_static/image3.gif)

**圖 3**: Web 組件 VS 逐步解說 1 螢幕擷取畫面


標題中的每個控制項的列是提供您可以在控制項執行的可用動作的動詞命令功能表存取的向下箭號。 按一下其中一個控制項，動詞命令功能表，然後按一下 **最小化**動詞命令，並請注意，控制項最小化。 從 [動作] 功能表中，按一下**還原**，控制權會回到其正常大小。

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>讓使用者編輯 頁面並變更版面配置

Web 組件提供的功能，讓使用者從一個區域拖曳到另一個來變更 Web 組件控制項的版面配置。 除了可讓使用者移動**WebPart**到另一個區域控制項，您可以允許使用者編輯控制項，包括其外觀、 配置和行為的各種特性。 Web 組件控制項集合提供的基本編輯功能**WebPart**控制項。 雖然您並不是在此逐步解說中，您也可以建立自訂編輯器控制項，可讓使用者編輯的功能**WebPart**控制項。 如同變更的位置**WebPart**控制項中，編輯控制項的屬性依賴 ASP.NET 個人化，以儲存使用者所做的變更。

在本逐步解說的這個部分，您新增的功能，讓使用者編輯任何的基本特性**WebPart**頁面上的控制項。 若要啟用這些功能，您將另一個自訂的使用者控制項加入頁面，連同&lt;asp: editorzone&gt;項目和兩個編輯控制項。

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>若要建立使用者控制項，可讓變更頁面配置

1. 在 Visual Studio 中，在**檔案**功能表上，選取**新增**] 子功能表，然後按一下 [**檔案**選項。
2. 在 **加入新項目**對話方塊中，選取**Web User Control**。 將新檔案命名 DisplayModeMenu.ascx。 取消選取的選項**置於個別檔案中的原始碼**。
3. 按一下 [新增] 以建立新的控制項。
4. 若要切換**來源**檢視。
5. 在新的檔案中，移除所有現有的程式碼並貼上下列程式碼。 此使用者控制項程式碼會使用 Web 組件控制項集合的功能，可讓頁面，即可變更其檢視，或顯示模式中，也可讓您變更實體的外觀和版面配置頁面時的特定顯示模式中。 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. 儲存檔案，按一下儲存圖示的工具列上，或選取**儲存**上**檔案**功能表。

### <a name="to-enable-users-to-change-the-layout"></a>若要讓使用者能夠變更版面配置

1. 開啟 WebPartsDemo.aspx 頁面上，並切換至**設計**檢視。
2. 將插入點放在**設計**後方檢視**WebPartManager**您先前加入的控制項。 使其沒有空白的行之後，強制加入的文字後面**WebPartManager**控制項。 將插入點放在空行上。
3. 將您剛才建立的使用者控制項拖曳 （檔案名稱為 DisplayModeMenu.ascx） WebPartsDemo.aspx 到頁面上，把它放在空行上。
4. 將從 EditorZone 控制項拖曳**WebParts**至 WebPartsDemo.aspx 頁面的其餘開啟資料表儲存格的 [工具箱] 的區段。
5. 從**WebParts**區段的 工具箱 拖曳 AppearanceEditorPart 控制項和 LayoutEditorPart 控制項**EditorZone**控制項。
6. 若要切換**來源**檢視。 產生的程式碼中的資料表資料格看起來應該類似下列的程式碼。 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. 儲存 WebPartsDemo.aspx 檔案。 您已建立的使用者控制項，可讓您變更顯示模式，以及變更頁面配置，您已參考主要的 Web 網頁上的控制項。

您現在可以測試 編輯頁面和配置的功能。

### <a name="to-test-layout-changes"></a>若要測試的配置變更

1. 將網頁瀏覽器中的載入。
2. 按一下 **顯示模式**下拉式選單，然後選取**編輯**。 區域標題會顯示。
3. 拖曳**我的連結**到主要區域底部的 從提要欄位區域其標題列控制項。 您的頁面看起來應該類似下列螢幕擷取畫面。

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>使用我的連結控制移動網頁組件示範


![Web 組件 VS 逐步解說 2 螢幕擷取畫面](profiles-themes-and-web-parts/_static/image4.gif)

**圖 4**: Web 組件 VS 逐步解說 2 螢幕擷取畫面


1. 按一下 **顯示模式**下拉式選單，然後選取**瀏覽**。 重新整理頁面時，區域名稱會消失，而**我的連結**控制會維持您放置的位置。
2. 若要示範個人化正常運作，關閉瀏覽器中，然後再重新載入頁面。 您所做的變更會儲存供將來瀏覽工作階段。
3. 從**顯示模式**功能表上，選取**編輯**。   
  
   每個頁面上的控制項現在會顯示其標題列，其中包含動詞命令下拉式功能表中的向下箭號。
4. 按一下箭頭以顯示上的動詞命令功能表**我的連結**控制項。 按一下 **編輯**動詞命令。   
  
   **EditorZone**控制項就會出現，顯示 EditorPart 控制您加入。
5. 在**外觀**編輯控制項，變更的區段**標題**[我的最愛] 中，使用**Chrome 型別**下拉式清單來選取**僅顯示標題**，然後按一下**套用**。 下列螢幕擷取畫面會顯示頁面處於編輯模式。

### <a name="web-parts-demo-page-in-edit-mode"></a>Web 組件示範頁面處於編輯模式


![Web 組件 VS 逐步解說 3 螢幕擷取畫面](profiles-themes-and-web-parts/_static/image5.gif)

**圖 5**: Web 組件 VS 逐步解說 3 螢幕擷取畫面


1. 按一下 **顯示模式**功能表，然後選取**瀏覽**返回瀏覽模式。
2. 控制項現在有了更新的標題，並沒有框線，如下列螢幕擷取畫面所示。

### <a name="edited-web-parts-demo-page"></a>已編輯的 Web 組件示範頁面


![Web 組件 VS 逐步解說 4 螢幕擷取畫面](profiles-themes-and-web-parts/_static/image6.gif)

**圖 4**: Web 組件 VS 逐步解說 4 螢幕擷取畫面


### <a name="adding-web-parts-at-run-time"></a>在執行階段加入 Web 組件

您也可以允許使用者在執行階段時，將 Web 組件控制項加入至其頁面。 若要這樣做，請使用 Web 組件目錄，其中包含您想要讓使用者使用的 Web 組件控制項的清單中設定頁面。

**若要允許使用者在執行階段將 Web 組件**

1. 開啟 WebPartsDemo.aspx 頁面上，並切換至**設計**檢視。
2. 從**WebParts**索引標籤的 [工具箱] 拖曳至 CatalogZone 控制項右側的資料行的資料表下方**EditorZone**控制項。   
  
   這兩個控制項可以是相同的資料表資料格，因為不會顯示在相同的時間。
3. 在 [屬性] 窗格中，將字串指派**新增網頁組件**HeaderText 屬性**CatalogZone**控制項。
4. 從**WebParts**區段的 [工具箱] 拖曳至 DeclarativeCatalogPart 控制項的內容區域**CatalogZone**控制項。
5. 按一下右上角的箭號**DeclarativeCatalogPart**控制公開其工作功能表上，然後按**編輯樣板**。
6. 從**標準**工具箱，拖曳的 區段**FileUpload**控制項和**行事曆**控制項放入**WebPartsTemplate**一節**DeclarativeCatalogPart**控制項。
7. 若要切換**來源**檢視。 檢查的原始程式碼&lt;asp: catalogzone&gt;項目。 請注意， **DeclarativeCatalogPart**控制項包含&lt;webpartstemplate&gt;具有兩個括住的伺服器控制項，您可以從目錄新增至您的頁面項目。
8. 新增**Title**到每個您新增至目錄中，使用顯示的字串值，如下列程式碼範例中的每個標題控制項的屬性。 即使不是屬性的標題。 您可以正常上設定這些兩種伺服器控制項在設計階段，當使用者新增到這些控制項**WebPartZone**區域在執行階段類別目錄，它們是每個各加**GenericWebPart**控制項。 這可讓它們當做 Web 組件控制項，因此它們可以顯示項目。   
  
   兩個控制項中所包含的程式碼**DeclarativeCatalogPart**控制項看起來應該如下。 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. 儲存網頁。

您現在可以測試目錄。

### <a name="to-test-the-web-parts-catalog"></a>若要測試 Web 組件類別目錄

1. 將網頁瀏覽器中的載入。
2. 按一下 **顯示模式**下拉式選單，然後選取**目錄**。   
  
   標題為 類別目錄**新增網頁組件**隨即出現。
3. 拖曳**我的最愛**回到頂端的 [資訊看板] 區域中，控制從主要區域，並那里卸除它。
4. 在 **新增網頁組件**目錄，選取這兩個核取方塊，然後選取**Main**從下拉式清單，其中包含可用的區域。
5. 按一下 **新增**目錄中。 新增控制項到主要區域。 如果您想，您可以從類別目錄中新增之控制項的多個執行個體至您的頁面。   
  
   下列螢幕擷取畫面顯示在主要區域中的檔案上傳控制項和行事曆的頁面。 

![從目錄加入至主要區域的控制項](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. 按一下 **顯示模式**下拉式選單，然後選取**瀏覽**。 目錄會消失，並在重新整理頁面。
7. 關閉瀏覽器。 重新載入頁面。 您所做的變更會保存。
