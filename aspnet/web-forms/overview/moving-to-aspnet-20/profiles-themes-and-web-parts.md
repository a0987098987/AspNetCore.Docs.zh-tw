---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: "設定檔、 主題和 Web 組件 |Microsoft 文件"
author: microsoft
description: "有一些組態中的重大變更和 ASP.NET 2.0 中的檢測。 新的 ASP.NET 組態 API 可讓設定變更，可供 pr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: c9fe97dbd5fe10cbde25b9daf5ddd35b2d7eaab5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="profiles-themes-and-web-parts"></a>設定檔、 主題和 Web 組件
====================
由[Microsoft](https://github.com/microsoft)

> 有一些組態中的重大變更和 ASP.NET 2.0 中的檢測。 新的 ASP.NET 組態 API 可讓您以程式設計方式進行組態變更。 此外，許多新的組態設定存在允許新的設定和測試設備。


ASP.NET 2.0 代表個人化的網站區域中大幅改進。 成員資格功能 weve 已經涵蓋，除了 ASP.NET 設定檔、 主題和 Web 組件顯著會提高網站中的個人化。

## <a name="aspnet-profiles"></a>ASP.NET 設定檔

ASP.NET 設定檔是與工作階段類似。 差別在於設定檔是永續性的而關閉瀏覽器時，工作階段會遺失。 工作階段與設定檔之間的另一個大差異是，設定檔強型別，因此您提供 IntelliSense 在開發程序。

機器組態檔或應用程式的 web.config 檔案中定義的設定檔。 （您無法在子資料夾的 web.config 檔案中定義的設定檔）。下列程式碼會定義要先儲存網站訪客和姓氏的設定檔。

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

設定檔屬性的預設資料類型為 System.String。 在上述範例中，不指定任何資料類型。 因此 FirstName 和 LastName 屬性都屬於 String 類型。 如先前所述，設定檔屬性強型別。 下列程式碼會將新屬性的型別 Int32 的天數。

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

設定檔通常會與 ASP.NET 表單驗證使用。 中表單驗證搭配使用時，每個使用者擁有個別的設定檔相關聯的使用者識別碼。 不過，它也可允許使用匿名應用程式使用的設定檔&lt;anonymousIdentification&gt;連同組態檔中的項目**allowAnonymous**做為屬性如下所示：

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

匿名使用者瀏覽的網站，當 ASP.NET 建立的執行個體**ProfileCommon**使用者。 此設定檔會使用儲存在 cookie 的瀏覽器上的唯一識別碼，將使用者識別為重複的訪客。 如此一來，您可以儲存以匿名方式瀏覽的使用者設定檔資訊。

## <a name="profile-groups"></a>設定檔群組

您可將屬性群組的設定檔。 依群組屬性，並模擬特定應用程式的多個設定檔。

下列組態會設定兩個群組; FirstName 和 LastName 屬性買主與潛在客戶。

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

為則設定屬性的特定群組，如下所示：

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>複雜的儲存物件

目前為止，我們所討論的範例設定檔中儲存簡單資料類型。 所以也可以藉由指定序列化使用的方法，儲存設定檔中的複雜資料型別**serializeAs**屬性，如下所示：

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

在此情況下，此類型為 PurchaseInvoice。 PurchaseInvoice 類別必須標示為可序列化，而且可以包含任何數目的屬性。 例如，如果 PurchaseInvoice 具有名**NumItemsPurchased**，您可以使用參照該程式碼中的屬性，如下所示：

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>設定檔的繼承

您可建立多個應用程式中使用的設定檔。 藉由建立衍生自 ProfileBase profile 類別，您可以重複使用的設定檔中數個應用程式使用**繼承**屬性如下所示：

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

在此情況下，類別**PurchasingProfile**看起來會像這樣：

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>設定檔提供者

ASP.NET 設定檔使用的提供者模型。 預設的提供者會將資訊儲存在應用程式中的 SQL Server Express 資料庫\_使用 SqlProfileProvider 提供者的 Web 應用程式資料資料夾。 如果資料庫不存在，ASP.NET 會自動建立它的設定檔，嘗試儲存資訊時。

在某些情況下，不過，您可能想要開發自己的設定檔提供者。 ASP.NET 設定檔功能可讓您輕鬆地使用不同的提供者。

建立自訂設定檔提供者時：

- 您要將設定檔資訊儲存在資料來源，例如 FoxPro 資料庫或 Oracle 資料庫中，不支援內含在.NET Framework 設定檔提供者。
- 您要管理使用不同於.NET Framework 隨附的提供者所使用的資料庫結構描述的資料庫結構描述的設定檔資訊。 常見範例是您想要整合現有的 SQL Server 資料庫中的使用者資料設定檔資訊。

### <a name="required-classes"></a>所需的類別

若要實作設定檔提供者，您可以建立繼承 System.Web.Profile.ProfileProvider 抽象類別的類別。 **ProfileProvider**抽象類別繼承 System.Configuration.SettingsProvider 抽象類別，繼承 System.Configuration.Provider.ProviderBase 抽象類別。 因為此繼承鏈結，除了必要成員**ProfileProvider**類別，您必須實作的必要的成員**SettingsProvider**和**ProviderBase**類別。

下表描述的屬性和方法，您必須實作從**ProviderBase**， **SettingsProvider**，和**ProfileProvider**抽象類別。

### <a name="providerbase-members"></a>ProviderBase 成員

| **成員** | **說明** |
| --- | --- |
| Initialize 方法 | 會提供者執行個體的名稱和組態設定的 NameValueCollection 的輸入。 用來設定選項和屬性值的提供者執行個體，包括實作特定值和電腦組態檔或 Web.config 檔案中指定的選項。 |

### <a name="settingsprovider-members"></a>SettingsProvider 成員

| **成員** | **說明** |
| --- | --- |
| ApplicationName 屬性 | 每個設定檔會儲存在應用程式名稱。 設定檔提供者會使用應用程式名稱來儲存設定檔資訊，分別為每個應用程式。 這可讓多個 ASP.NET 應用程式使用相同的資料來源，而不發生衝突，不同的應用程式中建立相同的使用者名稱。 或者，多個 ASP.NET 應用程式可以共用的設定檔資料來源指定相同的應用程式名稱。 |
| GetPropertyValues 方法 | 使用做為輸入 SettingsContext 和 SettingsPropertyCollection 的物件。 **SettingsContext**提供之使用者的相關資訊。 您可以使用主索引鍵的資訊來擷取使用者設定檔屬性資訊。 使用**SettingsContext**来取得的使用者名稱和使用者是否已驗證或匿名物件。 **SettingsPropertyCollection**包含 SettingsProperty 物件的集合。 每個**SettingsProperty**物件提供的名稱和類型的屬性，以及其他資訊，例如屬性和屬性是唯讀的預設值。 **GetPropertyValues**方法為基礎的 SettingsPropertyValue 物件中填入 SettingsPropertyValueCollection **SettingsProperty**物件提供作為輸入。 每個指定之使用者資料來源的值指派給 PropertyValue 屬性**SettingsPropertyValue**物件並將整個集合傳回。 呼叫方法也會更新指定的使用者設定檔的 LastActivityDate 值為目前的日期和時間。 |
| SetPropertyValues 方法 | 輸入**SettingsContext**和**SettingsPropertyValueCollection**物件。 **SettingsContext**提供之使用者的相關資訊。 您可以使用主索引鍵的資訊來擷取使用者設定檔屬性資訊。 使用**SettingsContext**来取得的使用者名稱和使用者是否已驗證或匿名物件。 **SettingsPropertyValueCollection**包含的集合**SettingsPropertyValue**物件。 每個**SettingsPropertyValue**物件提供名稱、 類型和值的屬性，以及其他資訊，例如屬性和屬性是唯讀的預設值。 **SetPropertyValues**方法會更新指定之使用者資料來源中的設定檔屬性值。 呼叫此方法也更新**LastActivityDate**和 LastUpdatedDate 值，指定的使用者設定檔的目前日期和時間。 |

### <a name="profileprovider-members"></a>ProfileProvider 成員

| **成員** | **說明** |
| --- | --- |
| DeleteProfiles 方法 | 使用做為字串陣列的使用者名稱和刪除資料來源的所有設定檔資訊和屬性值的指定名稱的輸入其中的應用程式名稱符合**ApplicationName**屬性值。 如果您的資料來源支援交易，建議您在交易中包含所有的刪除作業，而且您回復交易任何刪除作業失敗時擲回例外狀況。 |
| DeleteProfiles 方法 | 使用做為輸入 ProfileInfo 集合物件和資料來源的所有設定檔資訊和屬性值的每個設定檔會刪除其中的應用程式名稱符合**ApplicationName**屬性值。 如果您的資料來源支援交易，建議您在交易中包含所有的刪除作業並回復交易和任何刪除作業失敗時擲回例外狀況。 |
| DeleteInactiveProfiles 方法 | 當成輸入 ProfileAuthenticationOption 值和 DateTime 物件，刪除從資料來源的所有設定檔資訊和屬性值，則上次活動日期是小於或等於指定的日期和時間，而且應用程式名稱符合**ApplicationName**屬性值。 **ProfileAuthenticationOption**參數會指定是否只匿名設定檔，只會驗證設定檔，或所有設定檔會遭到刪除。 如果您的資料來源支援交易，建議您在交易中包含所有的刪除作業並回復交易和任何刪除作業失敗時擲回例外狀況。 |
| GetAllProfiles 方法 | 輸入**ProfileAuthenticationOption**值、 指定的頁面索引的整數、 整數，指定頁面大小，以及將設定檔的總計數的整數的參考。 傳回包含 ProfileInfoCollection **ProfileInfo**其中應用程式名稱必須符合的資料來源中的所有設定檔物件**ApplicationName**屬性值。 **ProfileAuthenticationOption**參數會指定是否只匿名設定檔，只會驗證設定檔，或會傳回所有設定檔。 所傳回的結果**GetAllProfiles**方法受到的頁面索引和頁面大小值。 頁面大小值指定最大數目**ProfileInfo**物件中傳回**ProfileInfoCollection**。 頁面索引值會指定要傳回，其中 1 代表第一頁的結果的分頁。 記錄總數的參數是 out 參數 (您可以使用**ByRef**在 Visual Basic 中) 設定為設定檔的總數。 例如，如果資料存放區包含 13 應用程式設定檔和頁面索引值為 5，頁面大小的 2 **ProfileInfoCollection**傳回包含第 6 到第十個設定檔。 方法傳回時，會記錄總數值設定為 13。 |
| GetAllInactiveProfiles 方法 | 輸入**ProfileAuthenticationOption**值**DateTime**物件、 指定的頁面索引的整數、 整數，指定頁面大小，以及將會設定為整數的參考若要設定檔的總計數。 傳回**ProfileInfoCollection**包含**ProfileInfo**其中上次活動日期是小於或等於指定的資料來源中的所有設定檔物件**日期時間** ，其中的應用程式名稱必須符合**ApplicationName**屬性值。 **ProfileAuthenticationOption**參數會指定是否只匿名設定檔，只會驗證設定檔，或會傳回所有設定檔。 所傳回的結果**GetAllInactiveProfiles**方法受到的頁面索引和頁面大小值。 頁面大小值指定最大數目**ProfileInfo**物件中傳回**ProfileInfoCollection**。 頁面索引值會指定要傳回，其中 1 代表第一頁的結果的分頁。 記錄總數的參數是 out 參數 (您可以使用**ByRef**在 Visual Basic 中) 設定為設定檔的總數。 例如，如果資料存放區包含 13 應用程式設定檔和頁面索引值為 5，頁面大小的 2 **ProfileInfoCollection**傳回包含第 6 到第十個設定檔。 方法傳回時，會記錄總數值設定為 13。 |
| FindProfilesByUserName 方法 | 輸入**ProfileAuthenticationOption**值字串，包含使用者名稱、 指定的頁面索引的整數、 整數，指定頁面大小，以及將會設定為的總計數的整數的參考設定檔。 傳回**ProfileInfoCollection**包含**ProfileInfo**物件，其中的使用者名稱符合指定的使用者名稱，其中的應用程式名稱必須符合，來源資料中的所有設定檔**ApplicationName**屬性值。 **ProfileAuthenticationOption**參數會指定是否只匿名設定檔，只會驗證設定檔，或會傳回所有設定檔。 如果您的資料來源支援其他的搜尋功能，例如萬用字元，您可以提供更廣泛的搜尋功能的使用者名稱。 所傳回的結果**FindProfilesByUserName**方法受到的頁面索引和頁面大小值。 頁面大小值指定最大數目**ProfileInfo**物件中傳回**ProfileInfoCollection**。 頁面索引值會指定要傳回，其中 1 代表第一頁的結果的分頁。 記錄總數的參數是 out 參數 (您可以使用**ByRef**在 Visual Basic 中) 設定為設定檔的總數。 例如，如果資料存放區包含 13 應用程式設定檔和頁面索引值為 5，頁面大小的 2 **ProfileInfoCollection**傳回包含第 6 到第十個設定檔。 方法傳回時，會記錄總數值設定為 13。 |
| FindInactiveProfilesByUserName 方法 | 輸入**ProfileAuthenticationOption**值、 字串，包含使用者名稱， **DateTime**物件、 指定的頁面索引的整數、 整數，指定頁面大小，以及將設定檔的總計數的整數的參考。 傳回**ProfileInfoCollection**包含**ProfileInfo**其中的使用者名稱符合指定的使用者名稱，其中的上次活動日期是在資料來源中的所有設定檔的物件少於或等於指定**DateTime**，和其中的應用程式名稱符合**ApplicationName**屬性值。 **ProfileAuthenticationOption**參數會指定是否只匿名設定檔，只會驗證設定檔，或會傳回所有設定檔。 如果您的資料來源支援其他的搜尋功能，例如萬用字元，您可以提供更廣泛的搜尋功能的使用者名稱。 所傳回的結果**FindInactiveProfilesByUserName**方法受到的頁面索引和頁面大小值。 頁面大小值指定最大數目**ProfileInfo**物件中傳回**ProfileInfoCollection**。 頁面索引值會指定要傳回，其中 1 代表第一頁的結果的分頁。 記錄總數的參數是 out 參數 (您可以使用**ByRef**在 Visual Basic 中) 設定為設定檔的總數。 例如，如果資料存放區包含 13 應用程式設定檔和頁面索引值為 5，頁面大小的 2 **ProfileInfoCollection**傳回包含第 6 到第十個設定檔。 方法傳回時，會記錄總數值設定為 13。 |
| GetNumberOfInActiveProfiles 方法 | 輸入**ProfileAuthenticationOption**值和**DateTime**物件，並傳回其中上次活動日期是小於或等於指定資料來源中的所有設定檔的計數**DateTime** ，其中的應用程式名稱必須符合**ApplicationName**屬性值。 **ProfileAuthenticationOption**參數會指定是否只匿名設定檔，只會驗證設定檔，或所有設定檔是要計算個數。 |

### <a name="applicationname"></a>ApplicationName

由於設定檔提供者儲存設定檔資訊，分別為每個應用程式，您必須確定您的資料結構描述包含應用程式名稱，並查詢與更新也包含應用程式名稱。 例如，用來擷取屬性值從資料庫為基礎的使用者名稱，以及設定檔是匿名的下列命令，並可確保**ApplicationName**查詢中是否包含值。

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>ASP.NET 主題

## <a name="what-are-aspnet-20-themes"></a>ASP.NET 2.0 主題有哪些？

其中一個最重要的 Web 應用程式的觀點整個站台是一致的外觀及操作。 ASP.NET 1.x 開發人員通常會使用階層式樣式表 (CSS) 來實作一致的外觀及操作。 ASP.NET 2.0 的佈景主題會大幅改善 CSS 因為他們提供 ASP.NET 開發人員定義的 ASP.NET 伺服器控制項，以及 HTML 項目外觀的能力。 ASP.NET 主題可以套用至個別控制項、 特定的網頁或整個 Web 應用程式中。 佈景主題，如果映像需要使用 CSS 檔案、 選擇性面板檔，以及選擇性的映像目錄的組合。 面板檔案控制 ASP.NET 伺服器控制項的視覺外觀。

## <a name="where-are-themes-stored"></a>在哪裡？ 儲存佈景主題

儲存佈景主題的位置不同根據其領域。 可以套用到任何應用程式的主題會儲存在下列資料夾：

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

特定應用程式特有的佈景主題會儲存在應用程式\_佈景主題\&lt;佈景主題\_名稱&gt;目錄中的網站的根目錄。

> [!NOTE]
> 面板檔案只應修改影響外觀的伺服器控制項屬性。

全域主題是可以套用到任何應用程式或網頁伺服器上執行的網站的佈景主題。 這些主題會儲存為 v2.x.xxxxx 目錄內的 ASP.NETClientfiles\Themes 目錄中的預設值。 或者，您可以在這裡將佈景主題檔案移入 aspnet\_用戶端/系統\_web / [version] /Themes/ [佈景主題\_名稱] 中您的網站的根資料夾。

特定應用程式佈景主題只會套用至檔案所在的應用程式。 這些檔案會儲存在應用程式\_佈景主題 /&lt;佈景主題\_名稱&gt;目錄中的網站的根目錄。

## <a name="the-components-of-a-theme"></a>佈景主題的元件

佈景主題是由一或多個 CSS 檔案、 選擇性面板檔案和選擇性的 [影像] 資料夾所組成。 CSS 檔案可以是任何您想 （也就是 default.css 或 theme.css 等），而且必須是根目錄中的主題資料夾的名稱。 CSS 檔案可用來定義一般 CSS 類別和特定的選取器的屬性。 要套用的 CSS 類別的其中一個頁面項目至**CSSClass**屬性使用。

面板檔案是 XML 檔案，其中包含 ASP.NET 伺服器控制項的屬性定義。 下面所列的程式碼是面板檔案的範例。

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

**圖 1**所示小型的 ASP.NET 網頁瀏覽沒有套用佈景主題。 **圖 2**套用佈景主題會顯示相同的檔案。 透過 CSS 檔案設定背景色彩和文字色彩。 使用上面所列的面板檔案所設定的按鈕和文字方塊中的外觀。


![無佈景主題](profiles-themes-and-web-parts/_static/image1.gif)

**圖 1**： 沒有佈景主題


![套用佈景主題](profiles-themes-and-web-parts/_static/image2.gif)

**圖 2**： 套用佈景主題


以上所列的面板檔案會定義所有文字方塊控制項和按鈕控制項的預設外觀。 這表示每個文字方塊控制項和插入到網頁上的按鈕控制項將會採取這個外觀。 您也可以定義可套用至特定執行個體使用這些控制項的面板**SkinID**控制項的屬性。

下列程式碼會定義按鈕控制項的外觀。 只有按鈕控制項與**SkinID**屬性**goButton**需要面板的外觀。

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

您只能有一個每種伺服器控制項類型的預設面板。 如果您需要額外的面板，您應該使用 SkinID 屬性。

## <a name="applying-themes-to-pages"></a>將佈景主題套用至網頁

您可以使用下列方法的任何一個套用佈景主題：

- 在&lt;頁面&gt;web.config 檔案的項目
- 在@Page頁面指示詞
- 以程式設計的方式

## <a name="applying-a-theme-in-the-configuration-file"></a>套用組態檔中的佈景主題

若要套用佈景主題的應用程式組態檔中，使用下列語法：

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

此處指定的佈景主題名稱必須符合佈景主題資料夾的名稱。 此資料夾可以存在其中任何一種稍早在本課程中所述的位置。 如果您嘗試套用佈景主題不存在，則會發生組態錯誤。

## <a name="applying-a-theme-in-the-page-directive"></a>在頁面指示詞

您也可以套用 @ Page 指示詞中的主題。 這個方法可讓您可針對特定頁面的佈景主題。

若要套用佈景主題@Page指示詞，請使用下列語法：

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

同樣地，在這裡指定的佈景主題必須符合主題資料夾，如先前所述。 如果您嘗試套用佈景主題不存在，則會發生組建失敗。 Visual Studio 也會反白顯示的屬性，並通知您這類的佈景主題存在。

## <a name="applying-a-theme-programmatically"></a>以程式設計的方式套用佈景主題

若要以程式設計的方式套用佈景主題，您必須指定**佈景主題**屬性中的頁面**頁面\_PreInit**方法。

若要以程式設計的方式套用佈景主題，請使用下列語法：

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

若要套用佈景主題 PreInit 方法，因為頁面生命週期中的必須。 如果您將它套用該時間點之後，網頁佈景主題將已經套用執行階段，並變更在該點已經太遲了生命週期中。 如果您將套用的佈景主題不存在， **HttpException** ，就會發生。 以程式設計的方式套用佈景主題時，建置警告便會發生任何伺服器控制項已經指定 SkinID 屬性。 這項警告被要通知您任何主題以宣告方式套用，並可略過。

## <a name="exercise-1--applying-a-theme"></a>練習 1： 套用佈景主題

在此練習中，您會將 ASP.NET 佈景主題套用至網站。

> [!IMPORTANT]
> 如果您使用 Microsoft Word 面板檔案中輸入資訊，請確定您未以智慧引號取代一般引號。 智慧引號將會造成問題的面板檔案。

1. 建立新的 ASP.NET 網站。
2. 以滑鼠右鍵按一下方案總管] 中的專案，然後選擇 [加入新項目。
3. 從檔案的清單中選擇 Web 組態檔，並按一下 新增。
4. 以滑鼠右鍵按一下方案總管] 中的專案，然後選擇 [加入新項目。
5. 選擇面板檔案，然後按一下 [新增]。
6. 按一下 [是] 會詢問 youd 要放置應用程式內的檔案時\_佈景主題的資料夾。
7. 以滑鼠右鍵按一下應用程式內的 SkinFile 資料夾\_方案總管] 中的主題資料夾，然後選擇 [加入新項目。
8. 從檔案的清單中選擇樣式表，並按一下 [新增]。 您現在有所有實作新的佈景主題的必要檔案。 不過，Visual Studio 具有名為 SkinFile 您的佈景主題 資料夾。 以滑鼠右鍵按一下該資料夾，並將名稱變更為 CoolTheme。
9. 開啟 SkinFile.skin 檔案並加入下列程式碼檔案的結尾： 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. 儲存 SkinFile.skin 檔案。
11. 開啟 StyleSheet.css。
12. 以下列內容取代所有的文字： 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. 儲存 StyleSheet.css 檔案。
14. 開啟 Default.aspx 頁面。
15. 加入 TextBox 控制項和按鈕控制項。
16. 儲存網頁。 現在您可以瀏覽 Default.aspx 頁面。 它應該顯示為一般的 Web 表單。
17. 開啟 web.config 檔案。
18. 加入下列開頭的正下方`<system.web>`標記： 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. 儲存 web.config 檔案。 現在您可以瀏覽 Default.aspx 頁面。 它應該會顯示與套用的佈景主題。
20. 如果它尚未開啟，Visual Studio 中開啟 Default.aspx 頁面。
21. 選取的按鈕。
22. 變更**SkinID** goButton 的屬性。 請注意，Visual Studio 會提供具有有效的 SkinID 值下拉式清單中的按鈕控制項。
23. 儲存網頁。 現在再次預覽您的瀏覽器中的頁面。 按鈕現在應該指出"go"，而且應該是較寬的外觀。

使用**SkinID**屬性，您可以輕鬆地設定不同的面板的特定類型的伺服器控制項的不同執行個體。

## <a name="the-stylesheettheme-property"></a>StyleSheetTheme 屬性

目前為止，我們談論只套用佈景主題使用佈景主題屬性。 使用佈景主題屬性時，面板檔案會覆寫伺服器控制項的任何宣告式設定。 例如，在練習 1 中，指定 SkinID"goButton 」 按鈕控制項的、 且變更按鈕的文字"go"。 您可能注意到，設計工具中的按鈕的文字屬性設定為 「 按鈕 」，但主題已覆寫的。 佈景主題一定會覆寫設計工具中的所有屬性設定。

如果您想要能夠使用佈景主題的面板檔案中定義的屬性會覆寫屬性中指定設計工具中，您可以使用**StyleSheetTheme**屬性，而非佈景主題屬性。 StyleSheetTheme 屬性等同於佈景主題屬性不同之處在於它不會覆寫所有屬性明確設定佈景主題屬性類似。

若要查看此動作中，開啟 web.config 檔案從專案中練習 1 並將變更&lt;頁面&gt;下的項目：

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

現在瀏覽 Default.aspx 頁面上，然後您會看到按鈕控制項已再次"Button"的文字屬性。 這是因為設計工具中的明確的屬性設定會覆寫 goButton SkinID 所設定的文字屬性。

## <a name="overriding-themes"></a>覆寫佈景主題

全域主題可覆寫相同名稱的應用程式中套用的佈景主題\_佈景主題的應用程式資料夾。 不過，在案例中，則為 true 的覆寫未套用佈景主題。 如果執行階段遇到應用程式中的佈景主題檔案\_佈景主題 資料夾中，將會套用佈景主題使用這些檔案，它會略過全域主題。

StyleSheetTheme 屬性可覆寫，且會覆寫程式碼中，如下所示：

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>Web 組件

ASP.NET Web 組件是網頁的一組整合式來建立網站，可讓使用者修改內容、 外觀和行為瀏覽器直接從控制項。 在網站上的所有使用者或個別使用者，可以套用所做的修改。 當使用者修改網頁及控制項時，這些設定可以儲存在未來的瀏覽器工作階段，名為個人化的功能保留使用者的個人喜好設定。 這些 Web 組件功能表示開發人員可以授權使用者動態地個人化的 Web 應用程式，開發人員或系統管理員介入。

您身為開發人員可以使用 Web 組件控制項集合，讓終端使用者：

- 個人化 頁面內容。 使用者可以將新的 Web 組件控制項加入頁面、 將它們移除，隱藏起來，或將其降至最低類似一般視窗。
- 個人化的頁面配置。 使用者可以在頁面上，將 Web 組件控制項拖曳到不同的區域，或變更其外觀、 屬性和行為。
- 匯出和匯入控制項。 使用者可以匯入或匯出 Web 組件控制項設定以用於其他頁面或網站，並保留內容、 外觀以及甚至控制項中的資料。 這會減少資料上使用者的項目和組態需求。
- 建立連線。 使用者可以建立控制項之間的連接，使圖表控制項無法，例如股票行情指示器控制項中顯示資料的圖表。 使用者無法個人化，不只連接本身，但的外觀和詳細資料圖表控制項顯示資料的方式。
- 管理及個人化網站層級設定。 授權的使用者可以設定網站層級設定，決定誰可以存取網站或網頁、 控制項設定以角色為基礎的存取權等等。 比方說，系統管理角色中的使用者無法將 Web 組件控制項設定為可由所有使用者共用，並防止使用者不是系統管理員從個人化共用的控制。

您通常會使用 Web 組件中有三種： 的使用 Web 組件控制項建立網頁、 建立個別的 Web 組件控制項，或建立完整的可個人化的 Web 應用程式，例如入口網站。

## <a name="page-development"></a>網頁程式開發

網頁開發人員可以使用視覺化設計工具，例如 Microsoft Visual Studio 2005，若要建立使用 Web 組件的頁面。 在使用工具，例如 Visual Studio Web 組件控制項集合的其中一項優點提供拖放建立及設定 Web 組件中的控制項的視覺化設計工具的功能。 比方說，您可以使用設計工具，將 Web 組件的區域或 Web 組件編輯器控制項，拖曳至設計介面，並設定權限的控制項，然後在設計工具中使用 Web 組件所提供的 UI 控制項集合。 這可加速開發 Web 組件的應用程式，並減少您必須撰寫的程式碼數量。

## <a name="control-development"></a>控制項開發

您可以使用任何現有的 ASP.NET 控制項，做為 Web 組件的控制項，包括標準的 Web 伺服器控制項、 自訂的伺服器控制項和使用者控制項。 您的環境最大程式設計的方式控制，您也可以建立自訂 Web 組件控制項衍生自 web 組件類別。 個別 Web 組件控制項的開發，您將通常建立使用者控制項並使用它做為 Web 組件控制項，或開發自訂的 Web 組件控制項。

開發自訂的 Web 組件控制項的範例，您無法建立控制項提供任何其他可能會很有用，加入封裝中成為可個人化的 Web 組件控制項的 ASP.NET 伺服器控制項所提供的功能： 行事曆、 清單、 財務資訊，新聞、 計算器、 rtf 文字控制項的更新內容，可以編輯的格線，連接到資料庫，動態更新其顯示，或見，旅行資訊的圖表。 如果您使用您的控制項提供視覺化設計工具中，任何網頁開發人員使用 Visual Studio 可以只將您的控制項拖曳至網頁組件區域，然後在設計階段設定而不需要撰寫額外的程式碼。

個人化是 Web 組件功能的基礎。 它可讓使用者修改-，或個人化版面配置、 外觀和行為的 Web 組件頁面上的控制項。 個人化的設定較長： 在保存不只是在目前的瀏覽器工作階段期間 （如同使用檢視狀態），但也在長期儲存，以便在使用者的設定會儲存為未來的瀏覽器工作階段。 Web 組件頁面預設會啟用個人化。

UI 結構元件依賴個人化，並提供的核心結構和服務所需的所有 Web 組件控制項。 每個 Web 組件頁面上所需的一個 UI 結構元件是 WebPartManager 控制項。 雖然不會看到這個控制項沒有協調網頁上的所有 Web 組件控制項的重要工作。 例如，它會追蹤所有個別的 Web 組件控制項。 它會管理 Web 組件區域 （包含 Web 組件頁面上的控制項），與哪一個區域中的控制項。 它也會追蹤，並控制頁面瀏覽、 連接、 編輯或目錄的模式，以及個人化變更套用至所有使用者或個別使用者是否可以是中，不同的顯示模式。 最後，它會起始並追蹤 Web 組件控制項之間的連接和通訊。

第二個類型的 UI 結構元件是區域。 區域做為 Web 組件頁面上的配置管理員。 它們包含組織衍生自組件類別 （組件控制項） 的控制項，並提供了可進行模組化的版面，以水平或垂直方向。 區域也包含; 每個控制項為提供通用且一致的 UI 項目 （例如頁首和頁尾的樣式、 標題、 框線樣式、 動作按鈕等等）這些通用項目稱為控制項的色彩。 區域的數個特製化的型別會使用不同的顯示模式，並使用不同的控制項。 不同類型的區域是由 Web 組件的基本控制項節所述。

Web 組件的 UI 控制項，全部都是衍生自**一部分**類別中，由 Web 組件頁面上的主要 UI 所組成。 Web 組件控制項集合具有彈性，含選項中提供了您建立組件控制項。 除了建立您自己自訂的 Web 組件控制項，您也可以使用現有的 ASP.NET 伺服器控制項，使用者控制項或自訂伺服器控制項的 Web 組件控制項。 建立 Web 組件頁面的最常用的基本控制項會詳述於下一節。

## <a name="web-parts-essential-controls"></a>Web 組件不可或缺的控制項

Web 組件控制項集合很大，但某些控制是不可或缺，因為所需要的 Web 組件來運作，或是因為是最常用的 Web 組件頁面上的控制項。 當您開始使用 Web 組件並建立基本的 Web 組件頁面，最好先熟悉下列表格中所述的基本 Web 組件控制項。

| **Web 組件控制項** | **說明** |
| --- | --- |
| WebPartManager | 管理頁面上的所有 Web 組件控制項。 一個 （且只有一個） **WebPartManager**控制項無須針對每個 Web 組件頁面。 |
| CatalogZone | 包含 CatalogPart 控制項。 使用這個區域建立類別目錄 Web 組件控制項的使用者可以從中選取要新增至網頁的控制項。 |
| EditorZone | 包含 EditorPart 控制項。 使用這個區域，讓使用者可以編輯並個人化 Web 組件頁面上的控制項。 |
| WebPartZone | 包含並提供撰寫的主要 UI 頁面的 web 組件控制項的整個版面配置。 當您建立 Web 組件控制項的網頁時，請使用這個區域。 網頁可以包含一或多個區域。 |
| Connectionszone 的動詞命令 | 包含 WebPartConnection 控制項，並提供使用者介面管理連接。 |
| Web 組件 (GenericWebPart) | 呈現的主要 UI。大部分的網頁組件的 UI 控制項歸類到此類別。 最大以程式設計方式控制，您可以建立自訂 Web 組件控制項衍生自基底**WebPart**控制項。 您也可以使用現有的伺服器控制項，使用者控制項或自訂控制項做為 Web 組件控制項。 任何這些控制項放置在區域中，每當**WebPartManager**控制項自動換行以**GenericWebPart**控制項在執行階段，因此，您可以使用 Web 組件功能。 |
| CatalogPart | 包含提供使用者可以加入至頁面的 Web 組件控制項的清單。 |
| WebPartConnection | 建立兩個頁面上的 Web 組件控制項之間的連接。 連接會定義 Web 組件控制項的其中一個為資料提供者 （），和另一個則為取用者。 |
| EditorPart | 可做為特定的編輯器控制項的基底類別。 |
| EditorPart 控制項 （AppearanceEditorPart、 LayoutEditorPart、 BehaviorEditorPart 和 PropertyGridEditorPart） | 允許使用者個人化的各種層面的頁面上的 Web 組件的 UI 控制項 |

## <a name="lab-create-a-web-part-page"></a>實驗室： 建立 Web 組件頁面

在此實驗室中，您將建立會保存透過 ASP.NET 設定檔資訊的 Web 組件頁面。

### <a name="creating-a-simple-page-with-web-parts"></a>建立簡單的頁面與網頁組件

在這個部分的逐步解說中，您可以建立使用 Web 組件控制項顯示靜態內容的網頁。 使用 Web 組件的第一個步驟是建立具有兩個必要的結構化元素的頁面。 首先，網頁組件必須追蹤和協調所有 Web 組件控制項 WebPartManager 控制項。 第二，Web 組件頁面需要一或多個區域 是複合控制項，可以包含網頁組件或其他伺服器控制項，所佔用的指定的區域的頁面。

> [!NOTE]
> 您不需要執行任何動作來啟用 Web 組件個人化。它會啟用 Web 組件控制項集合的預設值。 當您第一次會執行站台上的 Web 組件頁面時，ASP.NET 會設定預設的個人化提供者來儲存使用者個人化設定。 如需個人化的詳細資訊，請參閱 Web 組件個人化的概觀。


### <a name="to-create-a-page-for-containing-web-parts-controls"></a>若要建立包含 Web 組件頁面控制項

1. 關閉 [預設] 頁面，並將新頁面加入至名為 WebPartsDemo.aspx 的網站。
2. 切換至**設計**檢視。
3. 從**檢視**功能表上，請確定**隱藏式控制項**和**詳細資料**選項已選取，所以您可以查看配置標記和沒有 UI 的控制項。
4. 將插入點之前 **&lt;div&gt;** 標記放在設計介面，然後按 ENTER 即可加入新的一行上。 將新行字元前面的插入點，請按一下**區塊格式**下拉式清單控制項的功能表上，然後選取**標題 1**選項。 在標題中，將文字加入**網頁組件示範**。
5. 從**WebParts**  索引標籤的 工具箱 拖曳**WebPartManager**控制項拖曳到頁面上，只在新行字元之後再放置在 **&lt;div&gt;**標記。   
  
 **WebPartManager**控制項不會呈現任何輸出，因此它會顯示為設計工具介面上的灰色方塊。
6. 插入點內 **&lt;div&gt;** 標記。
7. 在**配置**功能表上，按一下 **插入表格**，並建立新的資料表具有一個資料列和三個資料行。 按一下**資料格屬性**按鈕，選取**頂端**從**垂直對齊**下拉式清單中，按一下 **確定**，按一下**確定**再次來建立資料表。
8. WebPartZone 控制項拖曳至左側的資料表資料行。 以滑鼠右鍵按一下**WebPartZone**控制項、 選擇 **屬性**，並設定下列屬性：   
  
 ID: SidebarZone   
  
 HeaderText: [資訊看板]
9. 拖曳第二個**WebPartZone**控制項放入中間的資料表資料行，並設定下列屬性：   
  
 ID: MainZone   
  
 HeaderText： 主要
10. 儲存檔案。

您的網頁現在有兩個不同的區域，您可以個別控制。 不過，兩個區域都任何內容，是建立內容的下一個步驟。 這個逐步解說中，您使用 Web 組件的控制項，僅顯示靜態內容。

所指定的 Web 組件區域配置 **&lt;zonetemplate&gt;** 項目。 在區域範本中，您可以加入任何 ASP.NET 控制項，是否為自訂的 Web 組件控制項、 使用者控制項或現有的伺服器控制項。 請注意，此處使用的標籤控制項，您只要加入靜態文字。 當您將一般的伺服器控制項中**WebPartZone**區域，ASP.NET 會視為控制項 Web 組件控制項在執行階段，可讓控制項上的 Web 組件功能。

**若要建立主要區域的內容**

1. 在**設計**檢視中，拖曳**標籤**控制項從**標準**到內部網路區域的 [內容] 區域的 [工具箱] 索引標籤的**識別碼**屬性設定為 MainZone。
2. 切換至**來源**檢視。 請注意，  **&lt;zonetemplate&gt;** 項目已加入至包裝**標籤**MainZone 中的控制項。
3. 加入名為屬性**標題**至 **&lt;asp： 標籤&gt;**項目，並將其值設定為內容。 移除文字 ="Label"屬性，從 **&lt;asp： 標籤&gt;**項目。 開頭和結尾標記之間 **&lt;asp： 標籤&gt;**項目，例如加入一些文字**歡迎使用我的首頁**內的一組 **&lt;h2&gt;** 項目標記。 您的程式碼應如下所示。 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. 儲存檔案。

接下來，建立使用者控制項，也可以加入至頁面做為 Web 組件的控制項。

### <a name="to-create-a-user-control"></a>建立使用者控制項

1. 將新的 Web 使用者控制項加入至您的站台做為搜尋控制項。 取消選取的選項**將原始程式碼放在個別檔案**。 在相同的目錄 WebPartsDemo.aspx 頁面上，將它加入並將它命名為 SearchUserControl.ascx。   
  
    > [!NOTE]
    > 此逐步解說中的使用者控制項未實作實際的搜尋功能。它只會用於示範 Web 組件的功能。
2. 切換至**設計**檢視。 從**標準** 索引標籤的 工具箱 拖曳到頁面的 TextBox 控制項。
3. 將插入點後您剛才加入的文字方塊，然後按 ENTER 即可加入新的一行。
4. 拖曳到頁面的按鈕控制項，在您剛才加入的文字方塊下方新的一行。
5. 切換至**來源**檢視。 請確定使用者控制項的原始程式碼，看起來像下列的範例。 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. 儲存並關閉檔案。

現在您可以將 Web 組件控制項新增到 [資訊看板] 區域。 您要將兩個控制項加入 [資訊看板] 區域，您在上一個程序中所建立的其中一個包含清單的連結，而另一個是使用者控制項。 連結會加入做為標準**標籤**伺服器控制項，類似於建立主要區域的靜態文字的方式。 不過，雖然中包含的個別伺服器控制項可以直接在此區域 （例如標籤控制項） 中包含使用者控制項，在此情況下不會。 而是您在上一個程序中建立使用者控制項的一部分。 這會顯示封裝所有控制項和使用者控制項，在您想要的額外功能，然後參考該控制項做為 Web 組件的控制項區域中的常見方式。

在執行階段，Web 組件控制項集合包裝 GenericWebPart 的控制項有兩個控制項。 當**GenericWebPart**控制項換行 Web 伺服器控制項，該泛型組件控制項父控制項，以及您可以透過將父控制項的 ChildControl 屬性存取的伺服器控制項。 這種泛型組件控制項可讓具有相同的基本行為和屬性，為 Web 組件控制項衍生自標準的 Web 伺服器控制項**WebPart**類別。

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>若要將 Web 組件控制項加入至 [資訊看板] 區域

1. 開啟 WebPartsDemo.aspx 頁面。
2. 切換至**設計**檢視。
3. 使用者控制您建立的網頁，SearchUserControl.ascx，拖曳**方案總管 中**進入區域其**識別碼**屬性設定為 SidebarZone，而且那里卸除它。
4. 儲存 WebPartsDemo.aspx 網頁。
5. 切換至**來源**檢視。
6. 內部 **&lt;asp: webpartzone&gt;**  SidebarZone，正上方加入使用者控制項，參考的項目加入 **&lt;asp： 標籤&gt;**包含連結，如下列範例所示的項目。 此外，請加入**標題**屬性加入使用者控制項的標記，其值為**搜尋**，如下所示。 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. 儲存並關閉檔案。

現在您可以測試您的網頁瀏覽器中瀏覽至它。 頁面會顯示兩個區域。 下列螢幕擷取畫面顯示的頁面。

**使用兩個區域網頁組件示範**


![Web 組件 VS 逐步解說 1 螢幕擷取畫面](profiles-themes-and-web-parts/_static/image3.gif)

**圖 3**: Web 組件 VS 逐步解說 1 螢幕擷取畫面


標題中的每個控制列是提供您可以在控制項執行的可用動作的動詞命令功能表存取的向下箭號。 按一下其中一個控制項，動詞命令功能表，然後按一下 **最小化**動詞命令，並請注意，此控制項最小化。 從 動作 功能表中，按一下 **還原**，控制權回到正常大小。

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>讓使用者編輯網頁及變更版面配置

Web 組件可讓使用者從一個區域拖曳到另一個變更 Web 組件控制項的版面配置功能。 除了可讓使用者移動**WebPart**到另一個區域從控制項，您可以允許使用者編輯控制項，包括其外觀、 配置和行為的各種特性。 Web 組件控制項集合提供基本的編輯功能，如**WebPart**控制項。 雖然您並不是在本逐步解說，您也可以建立自訂編輯器控制項，可讓使用者編輯的功能**WebPart**控制項。 如同變更的位置**WebPart**控制項中，編輯控制項的屬性依賴 ASP.NET 個人化，以儲存使用者所做的變更。

在這個部分的逐步解說中，您加入要編輯的基本特性的任何使用者的能力**WebPart**頁面上的控制項。 若要啟用這些功能，您將另一個自訂使用者控制項加入頁面中，連同 **&lt;asp: editorzone&gt;** 項目和兩個編輯控制項。

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>若要建立使用者控制項，可讓變更頁面配置

1. 在 Visual Studio 中，在**檔案**功能表上，選取**新增** 子功能表，然後按一下**檔案**選項。
2. 在**加入新項目**對話方塊中，選取**Web 使用者控制項**。 DisplayModeMenu.ascx 新檔案名稱。 取消選取的選項**將原始程式碼放在個別檔案**。
3. 按一下 [新增] 以建立新的控制項。
4. 切換至**來源**檢視。
5. 在新的檔案中，移除所有現有的程式碼並貼上下列程式碼。 此使用者控制項程式碼使用 Web 組件控制項集合的功能，可讓網頁上變更它的檢視，或顯示模式，也可讓您變更實體的外觀和版面配置頁面時的特定的顯示模式。 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. 儲存檔案，依序按一下儲存圖示的工具列上，或選取**儲存**上**檔案**功能表。

### <a name="to-enable-users-to-change-the-layout"></a>若要讓使用者變更版面配置

1. 開啟 WebPartsDemo.aspx 頁面上，並切換至**設計**檢視。
2. 將插入點放在**設計**後方檢視**WebPartManager**您先前加入的控制項。 將硬碟傳回加入的文字後面，使其沒有空行之後**WebPartManager**控制項。 在空白行上放置插入點。
3. 將您剛建立的使用者控制項拖曳 （檔案名為 DisplayModeMenu.ascx） WebPartsDemo.aspx 到頁面上，並將它放在空行上。
4. 將從 EditorZone 控制項拖曳**WebParts** WebPartsDemo.aspx 頁面中的其餘開啟資料表儲存格工具箱的區段。
5. 從**WebParts**區段的 [工具箱] 拖曳 AppearanceEditorPart 控制項和 LayoutEditorPart 控制項到**EditorZone**控制項。
6. 切換至**來源**檢視。 產生的程式碼中的表格儲存格看起來應該類似下列程式碼。 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. 儲存 WebPartsDemo.aspx 檔案。 您已建立使用者控制項，可讓您變更顯示模式和版面配置，並且參考主要 Web 網頁上的控制項。

您現在可以測試編輯網頁及變更配置的功能。

### <a name="to-test-layout-changes"></a>若要測試版面配置變更

1. 載入瀏覽器中。
2. 按一下**顯示模式**下拉功能表並選取**編輯**。 區域標題會顯示。
3. 拖曳**我的連結**控制項從 [資訊看板] 區域的主要區域底部標題列。 網頁看起來應該像下列螢幕擷取畫面。

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>使用我的連結控制移動網頁組件示範


![Web 組件 VS 逐步解說 2 螢幕擷取畫面](profiles-themes-and-web-parts/_static/image4.gif)

**圖 4**: Web 組件 VS 逐步解說 2 螢幕擷取畫面


1. 按一下**顯示模式**下拉功能表並選取**瀏覽**。 重新整理頁面時，區域名稱會消失，而**我的連結**控制會維持為位於所在位置。
2. 若要示範個人化可運作，關閉瀏覽器，然後再重新載入頁面。 您所做的變更會儲存供未來的瀏覽器工作階段。
3. 從**顯示模式**功能表上，選取**編輯**。   
  
 每個頁面上的控制項現在會顯示其標題列，其中包含動詞命令下拉式功能表中的向下箭號。
4. 按一下箭頭以顯示上的動詞命令功能表**我的連結**控制項。 按一下**編輯**動詞命令。   
  
 **EditorZone**控制項隨即出現，您加入顯示 EditorPart 控制項。
5. 在**外觀**> 一節的編輯控制項，變更**標題**我的最愛 使用**色彩類型**下拉式清單來選取**僅顯示標題**，然後按一下 **套用**。 下列螢幕擷取畫面顯示頁面處於編輯模式。

### <a name="web-parts-demo-page-in-edit-mode"></a>Web 組件示範頁面處於編輯模式


![Web 組件 VS 逐步解說 3 螢幕擷取畫面](profiles-themes-and-web-parts/_static/image5.gif)

**圖 5**: Web 組件 VS 逐步解說 3 螢幕擷取畫面


1. 按一下**顯示模式**功能表，然後選取**瀏覽**回到瀏覽模式。
2. 控制項現在有了更新的標題和無框線，如下列螢幕擷取畫面所示。

### <a name="edited-web-parts-demo-page"></a>編輯的 Web 組件示範頁面


![Web 組件 VS 逐步解說 4 螢幕擷取畫面](profiles-themes-and-web-parts/_static/image6.gif)

**圖 4**: Web 組件 VS 逐步解說 4 螢幕擷取畫面


### <a name="adding-web-parts-at-run-time"></a>在執行階段加入 Web 組件

您也可以允許使用者將網頁組件控制項加入至其頁面中，在執行階段。 若要這樣做，請設定頁面與 Web 組件的目錄，其中包含您想要讓使用者使用的 Web 組件控制項的清單。

**若要允許使用者在執行階段加入 Web 組件**

1. 開啟 WebPartsDemo.aspx 頁面上，並切換至**設計**檢視。
2. 從**WebParts**  索引標籤的 工具箱 拖曳至 CatalogZone 控制項右邊的資料行的資料表，下方**EditorZone**控制項。   
  
 兩個控制項可以是相同的資料表資料格，因為不會顯示在相同的時間。
3. 在 [屬性] 窗格中，將字串指派**新增網頁組件**的 HeaderText 屬性**CatalogZone**控制項。
4. 從**WebParts**區段的 [工具箱] 拖曳至 DeclarativeCatalogPart 控制項的內容區域的**CatalogZone**控制項。
5. 按一下右上角的箭號**DeclarativeCatalogPart**公開其工作功能表上，控制，然後選取 **編輯樣板**。
6. 從**標準**區段的 [工具箱] 拖曳**檔案上傳**控制項和**行事曆**控制項放入**WebPartsTemplate**區段**DeclarativeCatalogPart**控制項。
7. 切換至**來源**檢視。 檢查來源的程式碼的 **&lt;asp: catalogzone&gt;** 項目。 請注意， **DeclarativeCatalogPart**控制項包含 **&lt;webpartstemplate&gt;** 具有兩個括住的伺服器控制項，您可以加入到頁面項目從類別目錄。
8. 新增**標題**屬性為每個控制項，您加入至類別目錄中，使用每個項目在下列程式碼範例顯示的字串值。 即使不是屬性標題。 您可以正常上設定這些兩種伺服器控制項在設計階段，當使用者將這些控制項，以**WebPartZone**區域從執行階段類別目錄，它們是每個各加**GenericWebPart**控制項。 這可讓它們當做 Web 組件控制項，因此他們將無法顯示標題。   
  
 兩個控制項中所包含的程式碼**DeclarativeCatalogPart**控制項看起來應該如下。 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. 儲存網頁。

您現在可以測試類別目錄。

### <a name="to-test-the-web-parts-catalog"></a>若要測試 Web 組件類別目錄

1. 載入瀏覽器中。
2. 按一下**顯示模式**下拉功能表並選取**目錄**。   
  
 標題為 「 類別目錄**新增網頁組件**隨即出現。
3. 拖曳**我的最愛**回到頂端的 [資訊看板] 區域中，控制從主要區域，並將它放到那里。
4. 在**新增網頁組件**目錄中，選取兩個核取方塊，並選取**Main**從下拉式清單，其中包含可用的區域。
5. 按一下**新增**目錄中。 控制項會加入至 Main 區域。 如果您想，您可以加入多個控制項的執行個體從類別目錄網頁。   
  
 下列螢幕擷取畫面顯示在主要區域檔案上傳控制項和行事曆。 

![從類別目錄加入至主要區域的控制項](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. 按一下**顯示模式**下拉功能表並選取**瀏覽**。 目錄會消失，並在重新整理頁面。
7. 關閉瀏覽器。 重新載入頁面。 您所做的變更會保存。
