---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: 組態和檢測 |Microsoft Docs
author: microsoft
description: 有一些組態中的重大變更和 ASP.NET 2.0 中的檢測。 新的 ASP.NET 組態 API 可讓進行 pr 的組態變更...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: dc4e75e8c97228bf14935d6bf4242a036d513816
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399301"
---
<a name="configuration-and-instrumentation"></a>組態和檢測
====================
by [Microsoft](https://github.com/microsoft)

> 有一些組態中的重大變更和 ASP.NET 2.0 中的檢測。 新的 ASP.NET 組態 API 來以程式設計方式進行組態變更。 此外，許多新的組態設定存在於允許新的組態和檢測。


有一些組態中的重大變更和 ASP.NET 2.0 中的檢測。 新的 ASP.NET 組態 API 來以程式設計方式進行組態變更。 此外，許多新的組態設定存在於允許新的組態和檢測。

本單元中，我們將討論 ASP.NET 組態 API 與讀取和寫入至 ASP.NET 組態檔，以及我們也將涵蓋 ASP.NET 檢測。 我們也將涵蓋用於 ASP.NET 追蹤的新功能。

## <a name="aspnet-configuration-api"></a>ASP.NET 組態 API

ASP.NET 組態 API 可讓您開發、 部署及管理應用程式組態資料使用單一的程式設計介面。 您可以使用 設定 API 以開發和以程式設計方式修改完成的 ASP.NET 設定，而不用直接編輯組態檔中的 XML。 此外，您可以使用組態 API 的主控台應用程式和您所開發的指令碼、 Web 為基礎的管理工具和 Microsoft Management Console (MMC) 嵌入式管理單元。

下列兩個組態管理工具使用的組態 API，並隨附於.NET Framework 2.0 版：

- ASP.NET MMC 嵌入式管理單元，您可以使用組態 API 來簡化管理工作，提供從組態階層架構的所有層級的本機組態資料的整合式的檢視。
- 網站管理工具，可讓您管理本機和遠端應用程式組態設定，包括裝載站台。

ASP.NET 組態 API 包含一組 ASP.NET 管理物件可供您以程式設計方式設定網站和應用程式。 管理物件會實作為.NET Framework 類別庫。 設定 API 的程式設計模型可協助確保程式碼的一致性和可靠性，方法是在編譯時期強制資料類型。 若要讓您更輕鬆地管理應用程式組態，組態 API 可讓您檢視從組態階層架構中的所有點合併為單一集合，而不是個別的集合，從不同的形式檢視資料的資料組態檔。 此外，組態 API 可讓您管理整個應用程式設定，而不用直接編輯組態檔中的 XML。 最後，API 會將支援的系統管理工具，例如 Web Site Administration Tool 簡化組態工作。 設定 API 以簡化部署工作支援在電腦上的組態檔的建立和執行組態指令碼，在多部電腦。

> [!NOTE]
> 組態 API 不支援建立 IIS 應用程式。


## <a name="working-with-local-and-remote-configuration-settings"></a>使用本機和遠端的組態設定

組態物件，表示套用至特定實體，例如電腦，或一種邏輯實體，例如應用程式或網站的組態設定的合併的檢視。 在本機電腦上或遠端伺服器上，可以存在於指定的邏輯實體。 當不到組態檔存在指定的實體時，組態物件表示的預設組態設定所定義在 Machine.config 檔案。

您可以使用其中一種開啟組態方法，從下列類別，以取得組態物件：

1. ConfigurationManager 類別，如果您的實體是用戶端應用程式。
2. WebConfigurationManager 類別，如果您的實體是 Web 應用程式。

這些方法會傳回組態物件，進而提供必要的方法和屬性，以處理基礎的組態檔。 您可以存取這些檔案進行讀取或寫入。

### <a name="reading"></a>讀取

您可以使用 GetSection 或 GetSectionGroup 方法來讀取設定資訊。 讀取的處理程序的使用者必須擁有讀取所有階層中的組態檔的權限。

> [!NOTE]
> 如果您使用靜態的 GetSection 方法可接受的路徑參數，path 參數必須參考程式碼正在執行的應用程式。 否則，會忽略此參數，並傳回目前執行的應用程式的組態資訊。


### <a name="writing"></a>撰寫

您可以使用其中一種儲存方法寫入組態資訊。 使用者或程序，必須提供寫入寫入組態檔，並在目前的組態階層層級，目錄的權限，以及讀取所有階層中的組態檔的權限。

若要產生的組態檔，代表指定之實體的繼承的組態設定，請使用下列儲存組態方法的其中一個：

1. 要建立新的組態檔的儲存方法。
2. 要產生新的組態檔，另一個位置的另存新檔方法。

## <a name="configuration-classes-and-namespaces"></a>設定類別和命名空間

是類似於每個其他許多組態類別和方法。 下表描述最常使用的組態類別和命名空間。

| **設定類別或命名空間** | **描述** |
| --- | --- |
| [System.Configuration](https://msdn.microsoft.com/library/system.configuration.aspx)命名空間 | 包含所有的.NET Framework 應用程式的主要組態類別。 區段處理常式類別用來從方法，例如 GetSection 和 GetSectionGroup 取得區段的組態資料。 這兩個方法為非靜態。 |
| System.Configuration.Configuration 類別 | 表示一組電腦、 應用程式、 Web 目錄，或其他資源的組態資料。 這個類別包含有用的方法，例如 GetSection 和 GetSectionGroup，更新組態設定和取得區段和區段群組的參考。 這個類別用於取得設計階段組態資料，例如 WebConfigurationManager 和 ConfigurationManager 類別方法的方法為傳回型別。 |
| System.Web.Configuration 命名空間 | 包含在 ASP.NET 組態區段定義 區段處理常式類別[ASP.NET 組態設定](https://msdn.microsoft.com/library/b5ysx397.aspx)。 區段處理常式類別用來從方法，例如 GetSection 和 GetSectionGroup 取得區段的組態資料。 |
| System.Web.Configuration.WebConfigurationManager 類別 | 提供有用的方法來取得執行階段和設計階段的組態設定的參考。 這些方法會使用 System.Configuration.Configuration 類別作為傳回型別。 您可以使用這個類別的靜態 GetSection 方法或非靜態 GetSection 方法 System.Configuration.ConfigurationManager 類別的交換。 針對 Web 應用程式設定，而不是 System.Configuration.ConfigurationManager 類別建議 System.Web.Configuration.WebConfigurationManager 類別。 |
| [System.Configuration.Provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx)命名空間 | 提供方式來自訂及擴充的組態提供者。 這是在組態系統中的所有提供者類別的基底類別。 |
| [System.Web.Management](https://msdn.microsoft.com/library/system.web.management.aspx)命名空間 | 包含類別和介面來管理和監視 Web 應用程式的健全狀況。 嚴格來說，此命名空間不會視為組態 API 的一部分。 例如，追蹤和引發的事件被透過此命名空間中的類別。 |
| [System.Management.Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx)命名空間 | 提供所需的檢測的應用程式公開其管理資訊和潛在取用者透過 Windows Management Instrumentation (WMI) 事件的類別。 ASP.NET 健康監視事件傳遞使用 WMI。 嚴格來說，此命名空間不會視為組態 API 的一部分。 |

## <a name="reading-from-aspnet-configuration-files"></a>ASP.NET 組態檔讀取

WebConfigurationManager 類別是從 ASP.NET 組態檔讀取的核心類別。 有三個步驟本質上的閱讀 ASP.NET 組態檔：

1. 取得組態物件，使用 OpenWebConfiguration 方法。
2. 取得所需的區段，在組態檔中的參考。
3. 從組態檔讀取所需的資訊。

物件代表的組態不代表特定的組態檔。 相反地，它代表的電腦、 應用程式或網站設定的合併的檢視。 下列程式碼範例會具現化組態物件，表示呼叫的 Web 應用程式的組態*ProductInfo*。

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> 請注意，是否 /ProductInfo 路徑不存在，上述程式碼會傳回所指定的預設組態在 machine.config 檔案中。


組態物件之後，您就可以向下鑽研到組態設定，然後使用 GetSection 或 GetSectionGroup 方法。 下列範例會取得上述 ProductInfo 應用程式的模擬設定的參考：

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>寫入至 ASP.NET 組態檔

讀取組態檔，如同的 WebConfigurationManager 類別會是寫入 Asp.NET 組態檔的核心。 另外還有寫入 ASP.NET 組態檔的三個步驟。

1. 取得組態物件，使用 OpenWebConfiguration 方法。
2. 取得所需的區段，在組態檔中的參考。
3. 將所需的資訊寫入從組態檔使用 儲存 或 另存新檔方法。

下列程式碼變更**偵錯**屬性&lt;編譯&gt;為 false 的項目：

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

執行此程式碼時，**偵錯**屬性&lt;編譯&gt;會設為 false 的項目*webApp*應用程式的 web.config 檔案。

## <a name="systemwebmanagement-namespace"></a>System.Web.Management 命名空間

System.Web.Management 命名空間提供類別和介面來管理及監視 ASP.NET 應用程式的健全狀況。

記錄被透過定義規則與提供者產生關聯的事件。 規則定義事件傳送至提供者的類型。 下列的基底事件可供您記錄：

| **WebBaseEvent** | 所有事件的基底事件類別。 包含所需屬性的所有事件，例如 事件程式碼，事件詳細資料代碼、 日期和時間引發事件，順序編號，事件訊息和事件詳細資料。 |
| --- | --- |
| **WebManagementEvent** | 管理事件，例如應用程式存留期、 要求、 錯誤和稽核事件的基底事件類別。 |
| **WebHeartbeatEvent** | 定期擷取有用的執行階段狀態資訊的應用程式所產生的事件。 |
| **WebAuditEvent** | 安全性稽核事件，用來標記授權失敗，解密失敗，例如條件的基底類別*等等。* |
| **WebRequestEvent** | 參考要求的所有事件基底類別。 |
| **WebBaseErrorEvent** | 適用於表示錯誤條件的所有事件的基底類別。 |

可用的提供者類型可讓您將事件輸出傳送到事件檢視器、 SQL Server、 Windows Management Instrumentation (WMI) 和電子郵件。 預先設定的提供者和事件對應減少讓記錄的事件輸出所需的工作量。

ASP.NET 2.0 會使用事件記錄檔提供者--蜪鎏來記錄應用程式定義域啟動和停止，以及記錄任何未處理的例外狀況為基礎的事件。 這有助於將涵蓋一些基本的案例。 例如，假設您的應用程式會擲回的例外狀況，但使用者不會儲存錯誤和無法重現。 與預設的事件記錄檔規則，您就能夠蒐集深入了解何種錯誤發生的例外狀況和堆疊的資訊。 如果您的應用程式會遺失工作階段狀態，則會套用另一個範例。 在此情況下，您可以查看事件記錄檔，以判斷是否回收應用程式定義域，並在應用程式定義域在第一時間停止的原因。

此外，監視系統健全狀況是可延伸的。 比方說，您可以定義自訂的 Web 事件引發應用程式內，然後定義規則，以將事件資訊傳送給提供者，例如您的電子郵件。 這可讓您輕鬆地結合您檢測的健全狀況監視提供者。 另舉一例，您可以處理訂單並設定規則，以將每個事件傳送到 SQL Server 資料庫，每次引發事件。 您也可以引發事件時的使用者無法登入多次列的資料，並將事件設定為使用電子郵件為基礎的提供者。

預設的提供者和事件的組態會儲存在通用的 Web.config 檔案中。 通用的 Web.config 檔案會儲存所有的網頁設定已儲存在 Machine.config 檔，在 ASP.NET 1 x。 通用的 Web.config 檔案位於下列目錄：

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

&lt;HealthMonitoring&gt;全域的 Web.config 檔案區段提供的預設組態設定。 您可以覆寫這些設定，或設定您自己的設定，藉由實作&lt;healthMonitoring&gt;在您的應用程式的 Web.config 檔案中的區段。

&lt;HealthMonitoring&gt;全域的 Web.config 檔案區段包含下列項目：

| **提供者** | 包含提供者的事件檢視器、 WMI 及 SQL Server 設定。 |
| --- | --- |
| **eventMappings** | 包含各種 WebBase 類別的對應。 如果您產生您自己的事件類別，您可以擴充此清單。 產生您自己的事件類別提供給您更精細的提供者傳送的資訊。 例如，您可以設定電子郵件傳送您自己的自訂事件時傳送到 SQL Server 的未處理例外狀況。 |
| **規則** | 提供者 eventMappings 的連結。 |
| **緩衝處理** | 搭配 SQL Server 和電子郵件提供者，以決定排清至提供者的事件的頻率。 |

以下是全域的 Web.config 檔案中的程式碼範例。

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>如何儲存到事件檢視器的事件

如前文所述，記錄事件的提供者在事件檢視器會為您設定通用的 Web.config 檔案中。 根據預設，所有事件為都基礎**WebBaseErrorEvent**並**WebFailureAuditEvent**記錄。 您可以新增額外的規則，將額外的資訊記錄到事件記錄檔。 例如，如果您想要記錄所有事件 (*亦即*，根據每個事件**WebBaseEvent**)，您可以將下列規則新增至您的 Web.config 檔案：

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

此規則會連結**所有事件**事件對應到事件記錄檔提供者。 EventMapping 和提供者會包含在通用的 Web.config 檔案中。

## <a name="how-to-store-events-to-sql-server"></a>如何儲存到 SQL Server 的事件

這個方法會使用**ASPNETDB**資料庫中，由 Aspnet 產生\_regsql.exe 工具。 預設的提供者會使用 LocalSqlServer 連接字串，其中應用程式中使用其中一個檔案為基礎的資料庫\_資料夾或 SQL Server 的本機 SQLExpress 執行個體。 通用的 Web.config 檔案中已 LocalSqlServer 連接字串和 SqlProvider。

通用的 Web.config 檔案中的 LocalSqlServer 連接字串看起來像這樣：

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

如果您想要使用另一個 SQL Server 執行個體，您必須使用 Aspnet\_regsql.exe 工具，可在 %windir%\microsoft.net\framework\v2.0。\*資料夾。 使用 Aspnet\_regsql.exe 工具來產生自訂**ASPNETDB**資料庫在 SQL Server 執行個體，然後將連接字串新增至您的應用程式組態檔，然後新增使用新的 提供者連接字串。 一旦**ASPNETDB**所建立的資料庫，您必須設定規則，以根據 sqlProvider 連結 eventMapping。

不論您使用預設值根據 SqlProvider，或是設定自己的提供者，您必須新增連結事件對應的提供者的規則。 下列規則會連結您先前建立的新提供者**所有事件**事件對應。 此規則會記錄為基礎的所有事件**WebBaseEvent**並將它們傳送至 MySqlWebEventProvider 供 MYASPNETDB 連接字串。 下列程式碼加入規則，以連結事件對應的提供者：

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

如果您想要只將錯誤傳送給 SQL Server，您可以新增下列規則：

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>如何將事件轉寄至 WMI

此外，您也可以將事件轉送至 WMI。 WMI 提供者會根據預設，為您設定通用的 Web.config 檔案中。

下列程式碼範例加入規則，以將事件轉送至 WMI:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

您必須新增規則，以便將 eventMapping 至提供者，以及接聽事件的 WMI 接聽程式的應用程式產生關聯。 下列程式碼範例加入規則，以連結到的 WMI 提供者**所有事件**事件對應：

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>如何將事件轉寄至電子郵件

您也可以將電子郵件的事件。 請小心您對應至您的電子郵件提供者，因為您可以不小心傳送給自己大量資訊的哪些事件規則可能是較適合 SQL Server 或事件記錄檔。 有兩個的電子郵件提供者;SimpleMailWebEventProvider 和 TemplatedMailWebEventProvider。 每個具有相同的組態屬性中的 「 範本 」 和 「 detailedTemplateErrors"的屬性，這兩者都只是用於 TemplatedMailWebEventProvider 除外。

> [!NOTE]
> 這些電子郵件提供者都不會為您進行設定。 您必須將它們新增至您的 Web.config 檔案。


這些兩個電子郵件提供者的主要差異是 SimpleMailWebEventProvider 傳送電子郵件，無法修改的一般範本中。 範例 Web.config 檔案會使用下列規則，將此電子郵件提供者加入至設定的提供者清單：

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

下列規則也會加入至繫結至的電子郵件提供者**所有事件**事件對應：

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>ASP.NET 2.0 追蹤

有三個主要的增強功能與 ASP.NET 2.0 中的追蹤。

1. 整合的追蹤功能
2. 以程式設計方式存取追蹤訊息
3. 改善的應用程式層級追蹤

## <a name="integrated-tracing-functionality"></a>整合追蹤功能

您現在可以路由到 ASP.NET 追蹤輸出，System.Diagnostics.Trace 類別所發出的訊息，並對 System.Diagnostics.Trace 的 ASP.NET 追蹤所發出的訊息路由傳送。 您也可以對 System.Diagnostics.Trace 轉送 ASP.NET 檢測事件。 這項功能由新**writeToDiagnosticsTrace**屬性&lt;追蹤&gt;項目。 此布林值，則為 true 時，ASP.NET 追蹤訊息會轉送給 System.Diagnostics 追蹤基礎結構使用的任何已註冊要在顯示追蹤訊息的接聽程式。

## <a name="programmatic-access-to-trace-messages"></a>以程式設計方式存取追蹤訊息

ASP.NET 2.0 是用來以程式設計方式存取所有的追蹤訊息，透過**TraceContextRecord**類別和**TraceRecords**集合。 存取追蹤訊息的最有效率的方式是註冊**TraceContextEventHandler** （也在 ASP.NET 2.0 新） 的委派來處理新**TraceFinished**事件。 您接著可以逐一追蹤訊息，您希望的位置。

下列程式碼範例將說明這點：

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

在上述範例中，我將 TraceRecords 集合執行迴圈，然後寫入回應資料流中的每個訊息。

## <a name="improved-application-level-tracing"></a>改善的應用程式層級追蹤

透過引進新的應用程式層級追蹤已獲得改善**mostRecent**屬性&lt;追蹤&gt;項目。 這個屬性會指定是否顯示最新的應用程式層級追蹤輸出，並會捨棄較舊的追蹤資料超過限制所指出的 requestLimit。 如果為 false，追蹤資料會顯示要求，直到達到 requestLimit 屬性為止。

## <a name="aspnet-command-line-tools"></a>ASP.NET 命令列工具

有數個的命令列工具，可協助 ASP.NET 的組態設定。 ASP.NET 開發人員應該很熟悉 aspnet\_regiis.exe 工具。 ASP.NET 2.0 提供三種其他命令列工具來協助組態設定。

會有下列的命令列工具：

| **工具** | **使用** |
| --- | --- |
| **aspnet\_regiis.exe** | 用來登錄 iis ASP.NET。 有兩個版本的此工具隨附 ASP.NET 2.0，一個適用於 32 位元系統 （在 [架構] 資料夾中），一個適用於 64 位元系統 （在 Framework64 資料夾。）在 32 位元作業系統上時，將未安裝 64 位元版本。 |
| **aspnet\_regsql.exe** | ASP.NET SQL Server 註冊工具用來在 ASP.NET 中，建立 SQL Server 提供者所使用的 Microsoft SQL Server 資料庫，或是新增或移除現有的資料庫中的選項。 Aspnet\_regsql.exe 檔案位於 [網頁伺服器上 drive:]\WINDOWS\Microsoft.NET\Framework\versionNumber 資料夾。 |
| **aspnet\_regbrowsers.exe** | ASP.NET 瀏覽器註冊工具會剖析並編譯成組件的所有系統範圍的瀏覽器定義並的組件安裝到全域組件快取。 此工具會使用瀏覽器定義檔案 (。瀏覽器檔案） 從.NET Framework 的瀏覽器的子目錄。 此工具可以找到 %SystemRoot%\Microsoft.NET\Framework\version\ 目錄中。 |
| **aspnet\_compiler.exe** | ASP.NET 編譯工具可讓您編譯的 ASP.NET Web 應用程式，就地或以部署至目標位置，例如生產環境伺服器。 就地編譯可協助應用程式的效能，因為使用者不會遇到應用程式的第一個要求的延遲而編譯的應用程式。 |

因為 aspnet\_regiis.exe 工具不熟悉 ASP.NET 2.0 中，我們將在此討論它。

## <a name="aspnet-sql-server-registration-tool---aspnetregsqlexe"></a>ASP.NET SQL Server 註冊工具 aspnet\_regsql.exe

您可以設定數種使用 ASP.NET SQL Server 註冊工具的選項。 您可以指定 SQL 連接、 指定哪一個 ASP.NET 應用程式服務會使用 SQL Server 來管理資訊，指出哪一個資料庫或資料表的 SQL 快取相依性，以及新增或移除支援使用 SQL Server 來儲存程序和工作階段狀態。

數個 ASP.NET 應用程式服務依賴提供者來管理儲存和擷取資料來源的資料。 每個提供者是針對資料來源。 ASP.NET 包含下列 ASP.NET 功能的 SQL Server 提供者：

- 成員資格 ( [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx)類別)。
- 角色管理 ( [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx)類別)。
- 設定檔 ( [SqlProfileProvider](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx)類別)。
- Web 組件個人化 ( [SqlPersonalizationProvider](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx)類別)。
- Web 事件 ( [SqlWebEventProvider](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx)類別)。

當您安裝 ASP.NET 時，您的伺服器的 Machine.config 檔案會包含指定每個提供者所依賴的 ASP.NET 功能的 SQL Server 提供者的組態項目。 設定這些提供者，根據預設，連接到 SQL Server Express 2005 的本機使用者執行個體。 如果您變更預設的提供者所使用的連接字串，然後您可以使用任何在電腦組態中，設定的 ASP.NET 功能之前，您必須安裝 SQL Server 資料庫和資料庫項目，為所選功能使用 Aspnet\_regsql.exe。 如果您使用 SQL 註冊工具指定的資料庫已經存在 （aspnetdb 會是預設資料庫如果沒有指定命令列上），則目前的使用者必須擁有在 SQL Server 以及用於建立結構描述中建立資料庫的權限lements 資料庫內。

### <a name="sql-cache-dependency"></a>SQL 快取相依性

ASP.NET 輸出快取的進階的功能是 SQL 快取相依性。 SQL 快取相依性支援兩個不同的作業模式： 使用 ASP.NET 實作資料表輪詢和使用 SQL Server 2005 的查詢通知功能的第二個模式的其中一個。 SQL 註冊工具可用來設定作業的資料表輪詢模式。

### <a name="session-state"></a>工作階段狀態

根據預設，工作階段狀態的值和資訊會儲存在 ASP.NET 處理序內的記憶體。 或者，您可以將工作階段資料儲存在 SQL Server 資料庫中，其中由多部網頁伺服器共用。 如果您使用 SQL 註冊工具的工作階段狀態為指定的資料庫不存在，則目前的使用者必須具有 SQL Server 中也用於建立結構描述資料庫內的項目建立資料庫權限。 如果資料庫不存在，則目前的使用者必須具有在現有的資料庫中建立結構描述元素的權限。

若要安裝 SQL Server 上的工作階段狀態資料庫，執行 Aspnet\_regsql.exe 工具，並提供使用命令的下列資訊：

- 名稱的 SQL server 執行個體，使用 **-S**選項。
- 具有執行 SQL Server 的電腦上建立資料庫的權限的帳戶登入認證。 使用 **-E**選項來使用目前登入的使用者，或是使用 **-U**選項以指定的使用者識別碼，連同 **-P**選項來指定密碼。
- **-Ssadd**新增工作階段狀態資料庫的命令列選項。

根據預設，您無法使用 Aspnet\_regsql.exe 工具來執行 SQL Server 2005 Express Edition 的電腦上安裝工作階段狀態資料庫。

### <a name="the-aspnet-browser-registration-tool---aspnetregbrowsersexe"></a>ASP.NET 瀏覽器註冊工具-aspnet\_regbrowsers.exe

在 ASP.NET 1.1 版中，Machine.config 檔案包含一個小節稱為&lt;browserCaps&gt;。 本節包含一系列的 XML 項目定義的規則運算式為基礎的各種瀏覽器的設定。 適用於 ASP.NET 2.0 版中，新的。瀏覽資訊檔會定義使用 XML 項目在特定瀏覽器的參數。 您新增資訊，您可以在新的瀏覽器上加入新。瀏覽器檔案位於 %SystemRoot%\Microsoft.NET\Framework\version\CONFIG\Browsers 您系統上的資料夾。

因為每次它需要的瀏覽器資訊，應用程式不閱讀的.config 檔案，您可以建立新項目。瀏覽資訊檔和執行的 Aspnet\_regbrowsers.exe 將必要的變更新增至組件。 這可讓伺服器立即存取新的瀏覽器資訊，因此您不需要關閉任何應用程式以取得資訊。 應用程式可以透過瀏覽器屬性的目前 HttpRequest 存取瀏覽器功能。

執行 aspnet 時，可以使用下列選項\_regbrowser.exe:

| **選項** | **描述** |
| --- | --- |
| **-?** | 顯示 Aspnet\_regbbrowsers.exe 命令視窗中的說明文字。 |
| **-i** | 建立執行階段瀏覽器的功能組件，並將它安裝在全域組件快取中。 |
| **-u** | 解除安裝全域組件快取執行階段瀏覽器的功能組件。 |

## <a name="the-aspnet-compilation-tool---aspnetcompilerexe"></a>ASP.NET 編譯工具-aspnet\_compiler.exe

ASP.NET 編譯工具可以使用兩個一般的方式： 就地編譯和部署，而目標的輸出目錄在指定的編譯。

### <a name="compiling-an-application-in-placehttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[編譯應用程式就地](https://msdn.microsoft.com/library/ms229863.aspx)

ASP.NET 編譯工具可以編譯的位置中的應用程式，也就是它是模仿應用程式，因此會造成規則編譯提出多個要求的行為。 先行編譯網站的使用者不會發生延遲，進行編譯的頁面上第一個要求所造成。

當您先行編譯的位置中的站台時，適用於下列項目：

- 站台會保留其檔案和目錄結構。
- 您必須擁有站台伺服器上所使用的所有程式設計語言的編譯器。
- 如果任何檔案會編譯失敗，整個網站會編譯失敗。

您也可以重新編譯應用程式就地之後加入新的原始程式檔。 此工具會編譯只將新的或變更檔案，除非您包含 **-c**選項。

> [!NOTE]
> 編譯包含巢狀的應用程式的應用程式不會編譯巢狀的應用程式。 巢狀的應用程式必須個別進行編譯。


### <a name="compiling-an-application-for-deploymenthttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[編譯應用程式部署](https://msdn.microsoft.com/library/ms229863.aspx)

您可以指定 targetDir 參數編譯應用程式以部署 （編譯的目標位置）。 TargetDir 可以是 Web 應用程式的最後一個位置，或已編譯的應用程式可以進一步進行部署。 使用 **-u**選項編譯的方式，您可以變更已編譯的應用程式中的某些檔案而不需要重新編譯應用程式。 Aspnet\_compiler.exe 會區分靜態和動態的檔案類型，並建立產生的應用程式時以不同方式處理它們。

- 靜態檔案類型是指不具相關聯的編譯器或建置提供者，例如檔案的具名有擴充功能，例如.css、.gif、.htm、.html、.jpg、.js，依此類推。 這些檔案只會複製到目標位置，以保留目錄結構中其相對位置。
- 相關聯的編譯器或建置提供者，包括具有 ASP.NET 特定檔案名稱副檔名，例如.asax、.ascx、.ashx、.aspx、.browser、.master、 等等的檔案就是動態的檔案類型。 ASP.NET 編譯工具會產生組件，從這些檔案。 如果 **-u**略過選項，工具也會建立檔案的副檔名。對應至其組件的原始程式檔的 COMPILED。 若要確保會保留應用程式來源目錄結構，此工具會在目標應用程式的對應位置中產生預留位置檔案。

您必須使用 **-u**選項以指出可以修改的已編譯的應用程式的內容。 否則為後續修改都會被忽略，或導致執行階段錯誤。

下表描述如何 ASP.NET 編譯工具處理的不同檔案類型的時機 **-u**選項會納入。

| **檔案類型** | **編譯器動作** |
| --- | --- |
| .ascx、.master.aspx | 這些檔案會被分割成標記和來源的程式碼，包括程式碼後置檔案和括住的任何程式碼&lt;指令碼 runat ="server"&gt;項目。 原始程式碼會編譯成組件，以衍生自的雜湊演算法的名稱和組件置於 Bin 目錄。 任何內嵌程式碼中，也就是說，程式碼括**&lt; %** 並**% &gt;** 方括號、 標記包含和不編譯。 原始程式檔同名的新檔案建立來包含標記並放在對應的輸出目錄中。 |
| .ashx、.asmx | 這些檔案不會經過編譯，並移至為是，且不會經過編譯的輸出目錄。 如果您想要編譯的處理常式程式碼，將程式碼放到應用程式中的原始程式碼檔\_程式碼目錄。 |
| .cs、.vb、.jsl、.cpp （不包括程式碼後置檔案，如稍早所列的檔案類型） | 這些檔案會編譯，並包含做為參考的組件中的資源。 原始程式檔不會複製到輸出目錄。 如果未參考的程式碼檔案，它不會編譯。 |
| 自訂檔案類型 | 不會編譯這些檔案。 這些檔案會複製到對應的輸出目錄。 |
| 在應用程式的原始程式碼檔\_程式碼子目錄 | 這些檔案會編譯成組件，並放置在 Bin 目錄。 |
| 應用程式中的.resx 和.resource 檔案\_GlobalResources 子目錄 | 這些檔案會編譯成組件，並放置在 Bin 目錄。 沒有任何應用程式\_GlobalResources 子目錄下主要輸出目錄中，建立並沒有來源目錄中的.resx 或.resources 檔案複製到輸出目錄。 |
| 應用程式中的.resx 和.resource 檔案\_LocalResources 子目錄 | 這些檔案不會經過編譯，並複製到對應的輸出目錄。 |
| 應用程式中的.skin 檔案\_佈景主題子目錄 | .Skin 檔案，而且靜態的佈景主題檔案，不會編譯，並複製到對應的輸出目錄。 |
| .browser Web.config 靜態檔案類型的 Bin 目錄中已存在的組件 | 這些檔案會複製到輸出目錄。 |

下表描述如何 ASP.NET 編譯工具處理的不同檔案類型的時機 **-u**省略選項。

| **檔案類型** | **編譯器動作** |
| --- | --- |
| .aspx、.asmx、.ashx、.master | 這些檔案會被分割成標記和來源的程式碼，包括程式碼後置檔案和括住的任何程式碼&lt;指令碼 runat ="server"&gt;項目。 原始程式碼會編譯成組件，以衍生自雜湊演算法的名稱。 產生的組件被放置在 Bin 目錄。 任何內嵌程式碼中，也就是說，程式碼括**&lt; %** 並**% &gt;** 方括號、 標記包含和不編譯。 編譯器會建立新的檔案，以包含與原始程式檔的名稱相同的標記。 這些產生的檔案會放置在 Bin 目錄中。 原始程式檔同名但副檔名，編譯器也會建立檔案。包含對應資訊的 COMPILED。 。已編譯的檔案會放置在對應到原始位置的原始程式檔的輸出目錄中。 |
| .ascx | 這些檔案會分割成標記和來源的程式碼。 原始程式碼會編譯成組件，並放置在 Bin 目錄，以衍生自雜湊演算法的名稱。 會不產生任何標記檔案。 |
| .cs、.vb、.jsl、.cpp （不包括程式碼後置檔案，如稍早所列的檔案類型） | 所產生的.ascx、.ashx 或.aspx 檔案的組件參考的原始程式碼會編譯成組件，並放置在 Bin 目錄。 會不複製任何原始程式檔。 |
| 自訂檔案類型 | 這些檔案會編譯動態檔案一樣。 根據它們所根據的檔案類型，編譯器可以將對應檔放在輸出目錄中。 |
| 應用程式中的檔案\_程式碼子目錄 | 這個子目錄中的原始程式碼檔會編譯成組件，並放置在 Bin 目錄。 |
| 應用程式中的檔案\_GlobalResources 子目錄 | 這些檔案會編譯成組件，並放置在 Bin 目錄。 沒有任何應用程式\_GlobalResources 子目錄主要輸出目錄下建立。 如果組態檔指定的 appliesTo ="All"，.resx 和.resources 檔案會複製到輸出目錄。 不會複製如果會參考這些[BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx)。 |
| 應用程式中的.resx 和.resource 檔案\_LocalResources 子目錄 | 這些檔案會編譯成組件的唯一名稱，並放置在 Bin 目錄。 沒有.resx 或.resource 檔案會複製到輸出目錄。 |
| 應用程式中的.skin 檔案\_佈景主題子目錄 | 佈景主題會編譯成組件，並放置在 Bin 目錄。 虛設常式檔案建立.skin 檔案並放在對應的輸出目錄中。 （例如.css) 的靜態檔案會複製到輸出目錄。 |
| .browser Web.config 靜態檔案類型的 Bin 目錄中已存在的組件 | 這些檔案會複製到輸出目錄。 |

### <a name="fixed-assembly-nameshttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[固定組件名稱](https://msdn.microsoft.com/library/ms229863.aspx##)

某些情況下，例如使用 MSI Windows Installer，Web 應用程式部署需要使用一致的檔案名稱和內容，以及一致的目錄結構，以找出組件或更新的組態設定。 在這些情況下，您可以使用**的-fixednames**選項來指定 ASP.NET 編譯工具應該編譯的組件的每個原始程式檔，而不是使用 where 多個頁面會編譯成組件。 這可能會導致大量的組件，因此如果您擔心延展性與您應該小心使用此選項。

### <a name="strong-name-compilationhttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[強式名稱編譯](https://msdn.microsoft.com/library/ms229863.aspx##)

**-Aptca**， **-delaysign**， **-keycontainer**並 **-keyfile**會提供選項，好讓您可以使用 Aspnet\_compiler.exe 建立強式名稱組件而不使用[強式名稱工具 (Sn.exe)](https://msdn.microsoft.com/library/k5b5tt23.aspx)分開。 這些選項，分別對應至**AllowPartiallyTrustedCallersAttribute**， **AssemblyDelaySignAttribute**， **AssemblyKeyNameAttribute**，和**AssemblyKeyFileAttribute**。

這些屬性的討論超出本課程的範圍。

## <a name="labs"></a>實驗室

每個下列的實驗室是根據前一個實驗室。 您必須按照步驟順序。

## <a name="lab-1-using-the-configuration-api"></a>實驗室 1： 使用組態 API

1. 建立新的網站，稱為*mod9lab*。
2. 將新的 Web 組態檔新增至站台。
3. 將下列內容新增至 web.config 檔案：


[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

這可確保您有將變更儲存至 web.config 檔案的權限。

1. 將新的 Label 控制項新增至 Default.aspx，並將變更的 ID **lblDebugStatus**。
2. 將新的按鈕控制項新增至 Default.aspx 中。
3. 變更按鈕控制項的 ID **btnToggleDebug**和所要的文字**切換偵錯狀態**。
4. 開啟 Default.aspx 的程式碼後置檔案的程式碼檢視，並新增**使用**陳述式**System.Web.Configuration** ，如下所示：


[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. 將兩個私用變數新增至類別和頁面\_Init 方法，如下所示：


[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. 將下列程式碼新增至頁面\_負載：


[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. 儲存並瀏覽 default.aspx。 請注意，Label 控制項，將會顯示目前的偵錯狀態。
2. 在設計工具中的按鈕控制項上按兩下，並將下列程式碼新增至按鈕控制項的 Click 事件：


[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. 儲存並瀏覽 default.aspx，然後按一下  按鈕。
2. 開啟 web.config 檔案之後每個按鈕按一下，然後觀察,**偵錯**屬性中&lt;編譯&gt;一節。

## <a name="lab-2-logging-application-restarts"></a>實驗室 2： 記錄應用程式重新啟動

在此實驗室中，您將建立可讓您切換記錄應用程式的關機、 創業公司及事件檢視器中的重新編譯的程式碼。

1. 新增 DropDownList default.aspx，並將 ID 變更 ddlLogAppEvents。
2. 設定**AutoPostBack** DropDownList 以屬性 **，則為 true**。
3. 將三個項目新增至 DropDownList 的項目集合。 製作**文字**第一個項目的*Select Value*和-1 的值。 請**文字**和**值**第二個項目的 **，則為 True**而**文字**和**值**第三個項目**False**。
4. 將新的標籤新增至 default.aspx 中。 變更以識別碼**lblLogAppEvents**。
5. 開啟 default.aspx 的程式碼後置檢視，並新增新的宣告變數的型別 HealthMonitoringSection，如下所示：


[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. 將下列程式碼新增至現有的程式碼，在頁面\_Init:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. 按兩下 DropDownList，並將下列程式碼新增至 SelectedIndexChanged 事件：


[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. 瀏覽 default.aspx。
2. 設定下拉式清單中為**False**。
3. 清除 事件檢視器中的應用程式記錄檔。
4. 按一下按鈕以變更應用程式的偵錯屬性。
5. 重新整理應用程式記錄檔，事件檢視器中。 

    1. 已記錄任何事件？
    2. 為什麼或原因為何？
6. 設定下拉式清單中，為 **，則為 True。**
7. 按一下按鈕來切換應用程式的偵錯屬性。
8. 重新整理應用程式登入事件檢視器。 

    1. 已記錄任何事件？
    2. 應用程式關閉的原因是什麼？
9. 開啟和關閉記錄體驗，並看看對 web.config 檔案的變更。

## <a name="more-information"></a>詳細資訊：

ASP.NET 2.0 的提供者模型可讓您建立自己的提供者不只是應用程式檢測，但許多其他用途以及成員資格、 設定檔等。如需撰寫自訂的提供者，將應用程式事件記錄到文字檔的詳細資訊，請瀏覽[此連結](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp)。
