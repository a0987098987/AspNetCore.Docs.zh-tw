---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: "組態和測試設備 |Microsoft 文件"
author: microsoft
description: "有一些組態中的重大變更和 ASP.NET 2.0 中的檢測。 新的 ASP.NET 組態 API 可讓設定變更，可供 pr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: f8d378d3332669ae4606dad8ada06de37e7dfd20
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="configuration-and-instrumentation"></a>組態和測試設備
====================
由[Microsoft](https://github.com/microsoft)

> 有一些組態中的重大變更和 ASP.NET 2.0 中的檢測。 新的 ASP.NET 組態 API 可讓您以程式設計方式進行組態變更。 此外，許多新的組態設定存在允許新的設定和測試設備。


有一些組態中的重大變更和 ASP.NET 2.0 中的檢測。 新的 ASP.NET 組態 API 可讓您以程式設計方式進行組態變更。 此外，許多新的組態設定存在允許新的設定和測試設備。

在此模組中，我們將討論 ASP.NET 組態 API 相關，所以讀取和寫入至 ASP.NET 組態檔，以及我們也將討論 ASP.NET 檢測。 我們也將討論 ASP.NET 追蹤中可用的新功能。

## <a name="aspnet-configuration-api"></a>ASP.NET 組態應用程式開發介面

ASP.NET 組態應用程式開發介面可讓您開發、 部署和管理應用程式組態資料使用單一的程式設計介面。 您可以使用組態 API 以開發和以程式設計方式修改完成的 ASP.NET 設定，而不用直接編輯組態檔中的 XML。 此外，您可以使用組態 API 在主控台應用程式與您所開發的指令碼、 Web 為基礎的管理工具和 Microsoft Management Console (MMC) 嵌入式管理單元。

下列兩種設定管理工具使用的組態應用程式開發介面，並隨附於.NET Framework 2.0 版：

- ASP.NET MMC 嵌入式管理單元，以簡化系統管理工作使用的組態應用程式開發介面，可提供本機組態資料從組態階層架構的所有層級的整合式的檢視。
- 網站管理工具，可讓您管理本機和遠端應用程式的組態設定，包括裝載站台。

ASP.NET 組態應用程式開發介面包含一組可讓您以程式設計方式設定網站和應用程式的 ASP.NET 管理物件。 管理物件會實作為.NET Framework 類別庫。 設定應用程式開發介面的程式設計模型可協助確保程式碼的一致性和可靠性，方法是在編譯時期強制資料類型。 若要讓您更輕鬆地管理應用程式組態，組態 API 可讓您檢視會從組態階層架構中的所有點合併，當做單一集合，而不是檢視的資料做為個別的集合，從不同的資料組態檔。 此外，設定應用程式開發介面可讓您管理整個應用程式組態，而不用直接編輯組態檔中的 XML。 最後，此 API 簡化設定工作所支援的系統管理工具，例如網站管理工具。 組態 API 支援的組態檔的電腦上建立和多部電腦上執行組態指令碼，藉以簡化部署。

> [!NOTE]
> 設定應用程式開發介面不支援建立 IIS 應用程式。


## <a name="working-with-local-and-remote-configuration-settings"></a>使用本機和遠端組態設定

組態物件代表套用至特定的實際實體，例如電腦，或邏輯實體，例如應用程式或網站的組態設定的合併的檢視。 在本機電腦上或遠端伺服器上，可以存在於指定的邏輯實體。 當沒有組態檔存在指定的實體時，組態物件表示的預設組態設定所定義的 Machine.config 檔案。

您可以使用其中一種開啟設定方法，從下列類別，以取得組態物件：

1. ConfigurationManager 類別，如果您的實體為用戶端應用程式。
2. WebConfigurationManager 類別，如果實體是 Web 應用程式。

這些方法會傳回設定物件，其中會提供必要的方法和屬性來處理基礎的組態檔。 您可以存取這些檔案進行讀取或寫入。

### <a name="reading"></a>讀取

您可以使用 GetSection 或 GetSectionGroup 方法來讀取設定資訊。 使用者讀取或處理序必須擁有讀取權限的所有階層中的組態檔案。

> [!NOTE]
> 如果您使用靜態 GetSection 方法採用的路徑參數，path 參數必須參考應用程式程式碼正在執行。 否則，會忽略此參數並傳回目前執行的應用程式的組態資訊。


### <a name="writing"></a>撰寫

您可以使用其中一種儲存方法寫入組態資訊。 使用者或處理程序，其必須寫入的寫入權限組態檔案和目錄，在目前的組態階層層級，以及讀取權限階層中的組態檔的所有。

若要產生的組態檔，代表指定之實體的繼承的組態設定，請使用下列儲存組態方法的其中一個：

1. 要建立新的組態檔的儲存方法。
2. SaveAs 方法，以產生新的組態檔，另一個位置。

## <a name="configuration-classes-and-namespaces"></a>組態類別和命名空間

是類似於每個其他許多組態類別和方法。 下表描述最常使用的組態類別和命名空間。

| **設定類別或命名空間** | **說明** |
| --- | --- |
| [System.Configuration](https://msdn.microsoft.com/en-us/library/system.configuration.aspx)命名空間 | 包含所有的.NET Framework 應用程式的主要組態類別。 區段處理常式類別可用來從方法，例如 GetSection 和 GetSectionGroup 取得區段的組態資料。 這兩種方法為非靜態。 |
| System.Configuration.Configuration 類別 | 代表一組的電腦、 應用程式、 Web 目錄或其他資源的設定資料。 這個類別包含有用的方法，例如 GetSection 和 GetSectionGroup，更新組態設定和取得參考區段或區段群組。 這個類別用於取得設計階段組態資料，例如 WebConfigurationManager 和 ConfigurationManager 類別方法的方法當做傳回型別。 |
| System.Web.Configuration 命名空間 | ASP.NET 組態區段會定義在包含區段處理常式類別[ASP.NET 組態設定](https://msdn.microsoft.com/en-us/library/b5ysx397.aspx)。 區段處理常式類別可用來從方法，例如 GetSection 和 GetSectionGroup 取得區段的組態資料。 |
| System.Web.Configuration.WebConfigurationManager 類別 | 提供有用的方法，以取得執行階段和設計階段的組態設定的參考。 這些方法會使用 System.Configuration.Configuration 類別做為傳回類型。 您可以使用這個類別的靜態 GetSection 方法或 System.Configuration.ConfigurationManager 類別非靜態 GetSection 方法交換使用。 對於 Web 應用程式設定，而不是 System.Configuration.ConfigurationManager 類別建議 System.Web.Configuration.WebConfigurationManager 類別。 |
| [System.Configuration.Provider](https://msdn.microsoft.com/en-us/library/system.configuration.provider.aspx)命名空間 | 提供方法，以自訂和擴充的組態提供者。 這是所有的提供者類別的基底類別中的組態系統。 |
| [System.Web.Management](https://msdn.microsoft.com/en-us/library/system.web.management.aspx)命名空間 | 包含類別和介面來管理和監視的 Web 應用程式的健全狀況。 嚴格來說，此命名空間不會視為組態 API 的一部分。 比方說，追蹤和事件引發便可達成此命名空間中的類別。 |
| [System.Management.Instrumentation](https://msdn.microsoft.com/en-us/library/system.management.instrumentation.aspx)命名空間 | 提供的應用程式公開其管理資訊和潛在取用者透過 Windows Management Instrumentation (WMI) 事件檢測所需的類別。 ASP.NET 健康監視使用 WMI 傳遞事件。 嚴格來說，此命名空間不會視為組態 API 的一部分。 |

## <a name="reading-from-aspnet-configuration-files"></a>ASP.NET 組態檔讀取

WebConfigurationManager 類別是 ASP.NET 組態檔讀取的核心類別。 有三個步驟基本上閱讀 ASP.NET 組態檔：

1. 取得組態物件使用 OpenWebConfiguration 方法。
2. 取得所需的區段的組態檔中的參考。
3. 從組態檔中讀取所需的資訊。

物件代表的組態不代表特定設定檔。 相反地，它代表電腦、 應用程式或網站設定的合併的檢視。 下列程式碼範例會具現化組態物件，代表呼叫的 Web 應用程式組態*ProductInfo*。

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> 請注意，是否 /ProductInfo 路徑不存在，上述程式碼會傳回所指定的預設組態，machine.config 檔案中。


組態物件之後，您可以再使用 GetSection 或 GetSectionGroup 方法鑽研的組態設定。 下列範例會取得上述 ProductInfo 應用程式的模擬設定的參考：

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>寫入至 ASP.NET 組態檔

讀取組態檔，如同 WebConfigurationManager 類別是寫入至 Asp.NET 組態檔。 也有三個步驟來寫入 ASP.NET 組態檔。

1. 取得組態物件使用 OpenWebConfiguration 方法。
2. 取得所需的區段的組態檔中的參考。
3. 寫入所需的資訊從組態檔使用 儲存 或 另存新檔方法。

下列程式碼變更**偵錯**屬性&lt;編譯&gt;為 false 的項目：

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

執行此程式碼時，**偵錯**屬性&lt;編譯&gt;項目會設為 false *webApp*應用程式的 web.config 檔案。

## <a name="systemwebmanagement-namespace"></a>System.Web.Management 命名空間

System.Web.Management 命名空間提供類別和介面來管理和監視的 ASP.NET 應用程式的健全狀況。

記錄被透過定義規則與提供者產生關聯的事件。 規則定義事件傳送至提供者的類型。 下列的基底事件可供您記錄：

| **WebBaseEvent** | 所有事件的基底事件類別。 包含所需的所有事件，例如 事件程式碼中，事件詳細資料代碼、 日期和時間引發事件，內容序號、 事件訊息及事件的詳細資料。 |
| --- | --- |
| **WebManagementEvent** | 管理事件，例如應用程式存留期、 要求、 錯誤和稽核事件基底事件類別。 |
| **WebHeartbeatEvent** | 定期擷取有用的執行階段狀態資訊的應用程式所產生的事件。 |
| **WebAuditEvent** | 安全性稽核事件，用來標記授權失敗，解密失敗，例如條件的基底類別*等等。* |
| **WebRequestEvent** | 所有資訊要求事件基底類別。 |
| **WebBaseErrorEvent** | 指出錯誤條件的所有事件基底類別。 |

可用提供者類型可讓您將事件輸出傳送到事件檢視器、 SQL Server、 Windows Management Instrumentation (WMI) 和電子郵件。 預先設定的提供者和事件對應減少記錄的事件輸出所需的工作量。

ASP.NET 2.0 使用事件記錄檔提供者--現成的應用程式定義域啟動和停止，以及記錄任何未處理的例外狀況為基礎的事件記錄。 這有助於涵蓋一些基本的案例。 例如，假設您的應用程式會擲回的例外狀況，但使用者不會儲存錯誤並無法重現。 使用預設的事件記錄檔規則，您可以收集進一步了解何種錯誤發生的例外狀況和堆疊的資訊。 如果您的應用程式正失去工作階段狀態，則適用於另一個範例。 在此情況下，您可以查看事件記錄檔，以判斷是否會回收應用程式定義域，並在應用程式定義域在第一次停止的原因。

此外，監視系統健全狀況是可延伸。 比方說，您可以定義自訂的 Web 事件、 應用程式中引發，然後定義規則來傳送事件資訊至提供者，例如您的電子郵件。 這可讓您輕鬆地結合您檢測健康監視提供者。 另舉一例，您無法處理訂單和設定規則，將每個事件傳送至 SQL Server 資料庫，每次引發事件。 您也可以引發事件時在使用者無法登入多次列的資料，並將事件設定為使用的電子郵件為基礎的提供者。

預設的提供者和事件的組態會儲存在通用的 Web.config 檔案。 通用的 Web.config 檔案會儲存所有網頁設定 ASP.NET 1 中的 Machine.config 檔案中所儲存 x。 通用的 Web.config 檔案位於下列目錄：

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

&lt;HealthMonitoring&gt;全域的 Web.config 檔案區段提供的預設組態設定。 您可以覆寫這些設定，或藉由實作您自己設定&lt;healthMonitoring&gt;您的應用程式的 Web.config 檔案中的一節。

&lt;HealthMonitoring&gt;全域的 Web.config 檔案區段包含下列項目：

| **提供者** | 包含提供者的事件檢視器、 WMI 和 SQL Server 設定。 |
| --- | --- |
| **eventMappings** | 包含各種 WebBase 類別的對應。 如果您產生您自己的事件類別，您可以擴充此清單。 產生您自己的事件類別可讓您更精細的資料粒度上您將資訊傳送至的提供者。 例如，您可以設定要傳送您的電子郵件的自訂事件時傳送至 SQL Server 的未處理例外狀況。 |
| **規則** | 提供者 eventMappings 的連結。 |
| **緩衝處理** | 搭配 SQL Server 和電子郵件提供者，以決定排清至提供者的事件的頻率。 |

以下是全域的 Web.config 檔案中的程式碼範例。

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>如何儲存事件到事件檢視器

如先前所述，以記錄事件提供者在事件檢視器會為您設定通用的 Web.config 檔案中。 根據預設，所有事件都根據**WebBaseErrorEvent**和**WebFailureAuditEvent**記錄。 您可以加入額外的規則，以記錄至事件記錄檔的其他資訊。 例如，如果您想要記錄所有事件 (*也就是*，根據每個事件**WebBaseEvent**)，您無法加入 Web.config 檔案中的下列規則：

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

此規則會連結**所有事件**事件對應到事件記錄檔提供者。 EventMapping 和提供者會包含在通用的 Web.config 檔案中。

## <a name="how-to-store-events-to-sql-server"></a>如何儲存至 SQL Server 事件

這個方法會使用**ASPNETDB**資料庫產生 Aspnet\_regsql.exe 工具。 預設的提供者會使用 LocalSqlServer 連接字串，其中的應用程式中使用其中一個檔案為基礎的資料庫\_資料夾或 SQL Server 的本機 SQLExpress 執行個體。 LocalSqlServer 連接字串和根據 SqlProvider 通用的 Web.config 檔案中設定。

通用的 Web.config 檔案中的 LocalSqlServer 連接字串看起來像這樣：

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

如果您想要使用其他 SQL Server 執行個體，您必須使用 Aspnet\_regsql.exe 工具位於 %windir%\microsoft.net\framework\v2.0。\*資料夾。 使用 Aspnet\_regsql.exe 工具來產生自訂**ASPNETDB**資料庫上的 SQL Server 執行個體，然後將連接字串加入至您的應用程式組態檔，然後使用新的新增提供者連接字串。 一旦**ASPNETDB**所建立的資料庫，您必須設定規則來根據 sqlProvider 連結 eventMapping。

無論您使用預設值根據 SqlProvider，或是設定自己的提供者，您必須新增連結事件對應與提供者的規則。 下列規則連結到先前建立的新提供者**所有事件**事件對應。 此規則會記錄為基礎的所有事件**WebBaseEvent**並傳送給 MySqlWebEventProvider 讓 MYASPNETDB 連接字串。 下列程式碼加入規則，以連結事件對應的提供者：

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

如果您想要只將錯誤傳送給 SQL Server，您可以加入下列規則：

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>如何將事件轉寄至 WMI

您也可以轉送事件的 wmi。 WMI 提供者會根據預設，為您設定通用的 Web.config 檔案中。

下列程式碼範例會加入規則，以事件轉送給 WMI:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

您必須新增規則，以便將 eventMapping 提供者，以及為接聽事件的 WMI 接聽程式的應用程式產生關聯。 下列程式碼範例會加入規則，以連結到 WMI 提供者**所有事件**事件對應：

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-e-mail"></a>如何將事件轉寄至電子郵件

您也可以轉送電子郵件的事件。 請小心您對應至您的電子郵件提供者，因為您可以不小心傳送給自己的大量資訊的哪些事件規則可能是較適合 SQL Server 或事件記錄檔。 有兩個電子郵件提供者。SimpleMailWebEventProvider 和 TemplatedMailWebEventProvider。 每個有相同的組態屬性，但 「 範本 」 和 「 detailedTemplateErrors"屬性，兩者都是只用於 TemplatedMailWebEventProvider 除外。

> [!NOTE]
> 這些電子郵件提供者都不會為您進行設定。 您必須將它們新增到您的 Web.config 檔案。


這些兩個電子郵件提供者之間的主要差異是 SimpleMailWebEventProvider 無法修改的一般範本傳送電子郵件。 範例 Web.config 檔案會藉由使用下列規則，將此電子郵件提供者加入至設定的提供者清單：

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

下列規則也會加入至繫結的電子郵件提供者**所有事件**事件對應：

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>ASP.NET 2.0 追蹤

有三個主要的增強功能與 ASP.NET 2.0 中的追蹤。

1. 整合的追蹤功能
2. 以程式設計方式存取追蹤訊息
3. 改良的應用程式層級追蹤

## <a name="integrated-tracing-functionality"></a>整合追蹤功能

現在可以 System.Diagnostics.Trace 類別會追蹤輸出，ASP.NET 所發出的訊息路由傳送，並將 System.Diagnostics.Trace ASP.NET 追蹤所發出的訊息路由。 您也可以轉送到 System.Diagnostics.Trace 的 ASP.NET 檢測事件。 這項功能由新**writeToDiagnosticsTrace**屬性&lt;追蹤&gt;項目。 此布林值為 true 時，ASP.NET 追蹤訊息會轉送給 System.Diagnostics 追蹤基礎結構，用於透過任何已註冊要顯示追蹤訊息的接聽程式。

## <a name="programmatic-access-to-trace-messages"></a>以程式設計方式存取追蹤訊息

ASP.NET 2.0 允許以程式設計方式存取所有的追蹤訊息，透過**TraceContextRecord**類別和**TraceRecords**集合。 存取追蹤訊息的最有效率的方式是註冊**TraceContextEventHandler**委派 （也新增 ASP.NET 2.0 中） 來處理新**TraceFinished**事件。 您接著可以迴圈的追蹤訊息，在您希望的位置。

下列程式碼範例將說明這點：

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

在上述範例中，我將 TraceRecords 集合執行迴圈，然後將每個訊息寫入至回應資料流。

## <a name="improved-application-level-tracing"></a>改良的應用程式層級追蹤

透過導入新的應用程式層級追蹤已獲得改善**mostRecent**屬性&lt;追蹤&gt;項目。 這個屬性會指定是否顯示最新的應用程式層級追蹤輸出，並會捨棄較舊的追蹤資料超出 requestLimit 所表示之限制。 如果為 false，追蹤資料會顯示要求，直到達到 requestLimit 屬性。

## <a name="aspnet-command-line-tools"></a>ASP.NET 命令列工具

有數個命令列工具，可協助 ASP.NET 的組態設定。 ASP.NET 開發人員應該要熟悉 aspnet\_regiis.exe 工具。 ASP.NET 2.0 提供三種其他命令列工具來協助組態設定。

下列命令列工具可用：

| **工具** | **使用** |
| --- | --- |
| **aspnet\_regiis.exe** | 可讓登錄 iis ASP.NET。 有兩個版本的此工具隨附 ASP.NET 2.0 （在架構資料夾中） 的 32 位元系統，一個適用於 64 位元系統，（Framework64 資料夾中。）在 32 位元作業系統上，將不會安裝 64 位元版本。 |
| **aspnet\_regsql.exe** | ASP.NET SQL Server 註冊工具用來建立 Microsoft SQL Server 資料庫以供 SQL Server 提供者在 ASP.NET 中，或新增或移除現有的資料庫中的選項。 Aspnet\_regsql.exe 檔案位於 [Web 伺服器上 drive:]\WINDOWS\Microsoft.NET\Framework\versionNumber 資料夾。 |
| **aspnet\_regbrowsers.exe** | ASP.NET 瀏覽器註冊工具會剖析並編譯成組件的所有系統範圍的瀏覽器定義並的組件安裝到全域組件快取。 此工具會使用瀏覽器定義檔案 (。瀏覽器檔案） 從.NET Framework 瀏覽器子目錄。 %SystemRoot%\Microsoft.NET\Framework\version\ 目錄中，就可以找到這個工具。 |
| **aspnet\_compiler.exe** | ASP.NET 編譯工具可讓您編譯的 ASP.NET Web 應用程式，或部署到目標位置，例如生產環境伺服器。 就地編譯可協助應用程式效能，因為使用者不會發生延遲的應用程式的第一個要求時編譯的應用程式。 |

因為 aspnet\_regiis.exe 工具不熟悉 ASP.NET 2.0 中，我們不會討論它這裡。

## <a name="aspnet-sql-server-registration-tool---aspnetregsqlexe"></a>ASP.NET SQL Server 註冊工具 aspnet\_regsql.exe

您可以設定幾種類型的使用 ASP.NET SQL Server 註冊工具選項。 您可以指定 SQL 連接、 指定哪一個 ASP.NET 應用程式服務來管理資訊，指出 SQL 快取相依性，使用的資料庫或資料表使用 SQL Server 和新增或移除的使用 SQL Server 來儲存程序和工作階段狀態支援。

數個 ASP.NET 應用程式服務依賴來管理儲存和擷取資料來源的資料提供者。 每個提供者會因資料來源。 ASP.NET 會包含在下列 ASP.NET 功能的 SQL Server 提供者：

- 成員資格 ( [SqlMembershipProvider](https://msdn.microsoft.com/en-us/library/system.web.security.sqlmembershipprovider.aspx)類別)。
- 角色管理 ( [SqlRoleProvider](https://msdn.microsoft.com/en-us/library/system.web.security.sqlroleprovider.aspx)類別)。
- 設定檔 ( [SqlProfileProvider](https://msdn.microsoft.com/en-us/library/system.web.profile.sqlprofileprovider.aspx)類別)。
- Web 組件個人化 ( [SqlPersonalizationProvider](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx)類別)。
- Web 事件 ( [SqlWebEventProvider](https://msdn.microsoft.com/en-us/library/system.web.management.sqlwebeventprovider.aspx)類別)。

當您安裝 ASP.NET 時，Machine.config 檔案，為您的伺服器會包含組態項目會指定每個提供者所依賴的 ASP.NET 功能的 SQL Server 提供者。 設定這些提供者，根據預設，連接到 SQL Server Express 2005 的本機使用者執行個體。 如果您變更的提供者所用的預設連接字串，然後您可以使用任何在電腦組態中，設定的 ASP.NET 功能之前，您必須安裝 SQL Server 資料庫和資料庫項目如您所選擇的功能使用 Aspnet\_regsql.exe。 如果您使用 SQL 註冊工具指定的資料庫不存在 （aspnetdb 將預設資料庫如果未在命令列上指定其中一個），則目前的使用者必須擁有 SQL Server 一併建立結構描述中建立資料庫權限lements 資料庫內。

### <a name="sql-cache-dependency"></a>SQL 快取相依性

一項進階的功能的 ASP.NET 輸出快取是 SQL 快取相依性。 SQL 快取相依性支援兩個不同的作業模式： 使用 ASP.NET 實作資料表輪詢和使用 SQL Server 2005 的查詢通知功能的第二個模式的其中一個。 SQL 註冊工具可用來設定資料表輪詢模式的作業。

### <a name="session-state"></a>工作階段狀態

根據預設，工作階段狀態值和資訊會儲存在 ASP.NET 處理序內的記憶體。 或者，您可以在 SQL Server 資料庫中，其中由多部網頁伺服器共用儲存工作階段資料。 如果您指定 SQL 註冊工具與工作階段狀態的資料庫不存在，目前使用者必須具有在 SQL Server 中一併建立結構描述資料庫內的項目建立資料庫權限。 如果資料庫不存在，目前的使用者必須擁有現有資料庫中建立結構描述元素的權限。

若要安裝 SQL Server 上的工作階段狀態資料庫，執行 Aspnet\_regsql.exe 工具，並提供與命令的下列資訊：

- 名稱的 SQL Server 執行個體，使用**-S**選項。
- 具有執行 SQL Server 的電腦上建立資料庫的權限的帳戶登入認證。 使用**-E**選項來使用目前登入的使用者，或是使用**-U**選項來指定使用者 ID，並搭配**-P**選項來指定密碼。
- **-Ssadd**命令列選項可新增工作階段狀態資料庫。

根據預設，您無法使用 Aspnet\_regsql.exe 工具來執行 SQL Server 2005 Express Edition 的電腦上安裝工作階段狀態資料庫。

### <a name="the-aspnet-browser-registration-tool---aspnetregbrowsersexe"></a>的 ASP.NET Browser Registration Tool-aspnet\_regbrowsers.exe

在 ASP.NET 1.1 版中，Machine.config 檔案包含一個區段呼叫&lt;browserCaps&gt;。 本節包含一系列的 XML 項目定義根據規則運算式的多種瀏覽器的設定。 適用於 ASP.NET 2.0 版中，新的。瀏覽器檔案會定義使用 XML 項目之特定瀏覽器的參數。 您新增新的瀏覽器上的資訊透過新增。瀏覽器檔案，位於 %SystemRoot%\Microsoft.NET\Framework\version\CONFIG\Browsers 您系統上的資料夾。

因為每次需要瀏覽器資訊的應用程式不是正在讀取.config 檔案，您可以建立新項目。瀏覽器檔和執行的 Aspnet\_regbrowsers.exe 加入組件所需的變更。 這可讓伺服器立即存取新的瀏覽器資訊，因此您不需要關閉任何應用程式來收取資訊。 應用程式可以透過目前 HttpRequest 的瀏覽器屬性存取的瀏覽器功能。

執行 aspnet 時，可以使用下列選項\_regbrowser.exe:

| **選項** | **說明** |
| --- | --- |
| **-?** | 顯示 Aspnet\_regbbrowsers.exe 命令視窗中的說明文字。 |
| **-i** | 建立執行階段瀏覽器能力組件，並將它安裝在全域組件快取中。 |
| **-u** | 解除安裝全域組件快取執行階段瀏覽器能力組件。 |

## <a name="the-aspnet-compilation-tool---aspnetcompilerexe"></a>ASP.NET Compilation Tool-aspnet\_compiler.exe

ASP.NET 編譯工具可用於兩種方法： 在就地編譯和部署，而指定目標的輸出目錄的編譯。

### <a name="compiling-an-application-in-placehttpsmsdnmicrosoftcomen-uslibraryms229863aspx"></a>[編譯應用程式中的位置](https://msdn.microsoft.com/en-us/library/ms229863.aspx)

ASP.NET 編譯工具可以編譯的應用程式中的位置，也就是它會模擬應用程式，因此會造成一般的編譯進行多個要求的行為。 先行編譯網站的使用者不會發生編譯第一次要求頁面所造成的延遲。

當您先行編譯的位置中的站台時，適用於下列項目：

- 站台會保留其檔案和目錄結構。
- 您必須擁有所有的站台伺服器上所使用的程式設計語言的編譯器。
- 如果任何檔案會編譯失敗，整個站台會發生編譯失敗。

您也可以重新編譯應用程式就地之後加入新的來源檔案。 此工具會編譯只將新的或變更檔案，除非您包含**-c**選項。

> [!NOTE]
> 包含巢狀的應用程式的應用程式的編譯不會編譯巢狀的應用程式。 巢狀的應用程式必須個別進行編譯。


### <a name="compiling-an-application-for-deploymenthttpsmsdnmicrosoftcomen-uslibraryms229863aspx"></a>[編譯應用程式部署](https://msdn.microsoft.com/en-us/library/ms229863.aspx)

您藉由指定 targetDir 參數編譯應用程式部署 （編譯到目標位置）。 TargetDir 可以是 Web 應用程式的最後一個位置，或編譯的應用程式可以進一步進行部署。 使用**-u**選項編譯的方式，您可以進行變更編譯的應用程式中的某些檔案而不需要重新編譯應用程式。 Aspnet\_compiler.exe 區別靜態和動態的檔案類型，並建立產生的應用程式時以不同方式處理它們。

- 靜態檔案類型是指不具有相關聯的編譯器或建置提供者，例如檔案的具名等有例如.css、.gif、.htm、.html、.jpg、.js 副檔名。 這些檔案只會複製到目標位置，以保留的目錄結構中其相對位置。
- 相關聯的編譯器或建置提供者，包括 ASP.NET 特定檔案名稱副檔名，例如.asax、.ascx、.ashx、.aspx、.browser、.master、 等等的檔案就是動態的檔案類型。 ASP.NET 編譯工具產生組件從這些檔案。 如果**-u**省略選項、 工具也會建立檔案與檔案名稱的副檔名。對應至其組件的原始程式檔的編譯。 若要確定會保留應用程式來源的目錄結構，此工具會產生預留位置檔案中的目標應用程式中對應的位置。

您必須使用**-u**選項以指出可以修改的已編譯的應用程式的內容。 否則，後續的修改會被忽略，或導致執行階段錯誤。

下表說明如何編譯 ASP.NET 工具處理的不同檔案類型時**-u**選項會納入。

| **檔案類型** | **編譯器動作** |
| --- | --- |
| .ascx、.master.aspx | 這些檔案會分為標記和來源的程式碼，將會包含任何程式碼和程式碼後置檔案包含&lt;指令碼 runat ="server"&gt;項目。 原始程式碼會編譯為組件，以衍生自雜湊演算法的名稱和組件會放在 Bin 目錄。 這就是任何內嵌程式碼，程式碼用括號 **&lt; %** 和 **% &gt;** 括號，會包含在標記並不會編譯。 與原始程式檔同名的新檔案會建立了包含標記，並放在對應的輸出目錄中。 |
| .ashx、.asmx | 這些檔案不會編譯，並移到輸出目錄，並不會編譯。 如果您想要編譯的處理常式程式碼，將程式碼放到應用程式中的原始程式碼檔\_程式碼目錄。 |
| .cs、.vb、.jsl、.cpp （不包括程式碼後置檔案，如稍早所列的檔案類型） | 這些檔案，編譯並包含做為參考它們的組件中的資源。 來源檔案不會複製到輸出目錄。 如果未參考的程式碼檔案，則不會編譯它。 |
| 自訂的檔案類型 | 這些檔案不會編譯。 這些檔案會複製到對應的輸出目錄。 |
| 原始程式碼檔的應用程式中\_程式碼子目錄 | 這些檔案會編譯成組件，並放在 Bin 目錄中。 |
| 應用程式中的.resx 和.resource 檔\_GlobalResources 子目錄 | 這些檔案會編譯成組件，並放在 Bin 目錄中。 任何應用程式\_GlobalResources 子目錄主要的輸出目錄中，之下建立，而且會將.resx 或.resources 檔案位於來源目錄複製到輸出目錄。 |
| 應用程式中的.resx 和.resource 檔\_LocalResources 子目錄 | 這些檔案不會編譯並複製到對應的輸出目錄。 |
| 應用程式中的.skin 檔案\_佈景主題的子目錄 | .Skin 檔和靜態的佈景主題檔案，不會編譯，並複製到對應的輸出目錄。 |
| .browser Web.config 靜態檔案類型的 Bin 目錄中已經存在的組件 | 這些檔案會複製到輸出目錄。 |

下表說明如何編譯 ASP.NET 工具處理的不同檔案類型時**-u**省略選項。

| **檔案類型** | **編譯器動作** |
| --- | --- |
| .aspx、.asmx、.ashx、.master | 這些檔案會分為標記和來源的程式碼，將會包含任何程式碼和程式碼後置檔案包含&lt;指令碼 runat ="server"&gt;項目。 原始程式碼會編譯為組件，以衍生自雜湊演算法的名稱。 產生的組件會放在 Bin 目錄。 這就是任何內嵌程式碼，程式碼用括號 **&lt; %** 和 **% &gt;** 括號，會包含在標記並不會編譯。 編譯器會建立新的檔案包含與原始程式檔的名稱相同的標記。 這些產生的檔案會放置在 Bin 目錄。 編譯器也會建立檔案與原始程式檔的名稱相同但副檔名為。包含對應資訊的編譯。 。編譯的檔案會放置在對應至原始位置的原始程式檔的輸出目錄。 |
| .ascx | 這些檔案會分割成標記和原始程式碼。 原始程式碼會編譯成組件，並放在 Bin 目錄中，以衍生自雜湊演算法的名稱。 不會產生標記檔案。 |
| .cs、.vb、.jsl、.cpp （不包括程式碼後置檔案，如稍早所列的檔案類型） | 從.ascx、.ashx 或.aspx 檔案產生的組件所參考的程式碼會編譯成組件，並放置於 Bin 目錄。 會不複製任何原始程式檔。 |
| 自訂的檔案類型 | 這些檔案會編譯像動態檔案。 根據它們所根據的檔案類型，編譯器可以將對應檔案放在輸出目錄中。 |
| 應用程式中的檔案\_程式碼子目錄 | 這個子目錄中原始程式碼檔會編譯成組件，並放在 Bin 目錄中。 |
| 應用程式中的檔案\_GlobalResources 子目錄 | 這些檔案會編譯成組件，並放在 Bin 目錄中。 任何應用程式\_主要輸出目錄下建立 GlobalResources 子目錄。 如果組態檔會指定 appliesTo = 「 全部 」，.resx 和.resources 檔案會複製到輸出目錄。 不會複製如果它們正由[BuildProvider](https://msdn.microsoft.com/en-us/library/system.web.configuration.buildprovider.aspx)。 |
| 應用程式中的.resx 和.resource 檔\_LocalResources 子目錄 | 這些檔案會編譯成組件的唯一名稱，並放在 Bin 目錄中。 沒有.resx 或.resource 檔案會複製到輸出目錄。 |
| 應用程式中的.skin 檔案\_佈景主題的子目錄 | 佈景主題會編譯成組件，並放在 Bin 目錄中。 虛設常式檔案建立.skin 檔案並放在對應的輸出目錄中。 靜態 （例如.css) 檔案會複製到輸出目錄。 |
| .browser Web.config 靜態檔案類型的 Bin 目錄中已經存在的組件 | 這些檔案會複製到輸出目錄。 |

### <a name="fixed-assembly-nameshttpsmsdnmicrosoftcomen-uslibraryms229863aspx"></a>[固定的組件名稱](https://msdn.microsoft.com/en-us/library/ms229863.aspx##)

某些情況下，例如使用 MSI Windows Installer，Web 應用程式部署需要使用一致的檔案名稱和內容，以及一致的目錄結構，以找出組件或更新的組態設定。 在這些情況下，您可以使用**-fixednames**選項來指定 ASP.NET 編譯工具應該編譯組件的每個來源檔案，而不是使用 where 多個頁面會編譯成組件。 這可能會導致大量的組件，因此如果您擔心延展性與您應小心使用此選項。

### <a name="strong-name-compilationhttpsmsdnmicrosoftcomen-uslibraryms229863aspx"></a>[強式名稱編譯](https://msdn.microsoft.com/en-us/library/ms229863.aspx##)

**-Aptca**， **-delaysign**， **-keycontainer**和**-keyfile**會提供選項，好讓您可以使用 Aspnet\_compiler.exe 建立強式名稱組件而不使用[強式名稱工具 (Sn.exe)](https://msdn.microsoft.com/en-us/library/k5b5tt23.aspx)分開。 這些選項，分別對應至**AllowPartiallyTrustedCallersAttribute**， **AssemblyDelaySignAttribute**， **AssemblyKeyNameAttribute**，和**AssemblyKeyFileAttribute**。

這些屬性的討論超出本課程的範圍。

## <a name="labs"></a>實驗室

每個下列的實驗室是根據前一個實驗室。 您必須按照步驟順序。

## <a name="lab-1-using-the-configuration-api"></a>實驗室 1： 使用組態 API

1. 建立新的網站呼叫*mod9lab*。
2. 將新的 Web 組態檔加入至網站。
3. Web.config 檔案中加入下列：


[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

這可確保您有將變更儲存到 web.config 檔案的權限。

1. 將新的 Label 控制項加入至 Default.aspx，並變更 ID **lblDebugStatus**。
2. 將新的按鈕控制項加入至 Default.aspx。
3. 變更按鈕控制項的 ID **btnToggleDebug**和以文字**切換偵錯狀態**。
4. 開啟 Default.aspx 的程式碼後置檔案的程式碼 檢視，然後加入**使用**陳述式**System.Web.Configuration** ，如下所示：


[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. 將兩個私用變數加入至類別和頁面\_Init 方法，如下所示：


[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. 將下列程式碼加入至頁面\_負載：


[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. 儲存並瀏覽 default.aspx。 請注意標籤控制項會顯示目前的偵錯狀態。
2. 在設計工具中的按鈕控制項上按兩下，將下列程式碼加入至按鈕控制項的 Click 事件：


[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. 儲存並瀏覽 default.aspx，然後按一下 [] 按鈕。
2. 開啟 web.config 檔案之後每個按鈕按一下，然後觀察,**偵錯**屬性&lt;編譯&gt;> 一節。

## <a name="lab-2-logging-application-restarts"></a>實驗室 2： 記錄重新啟動應用程式

在此實驗室中，您將建立可讓您切換記錄應用程式關閉、 啟動和 事件檢視器中的重新編譯的程式碼。

1. 新增 DropDownList default.aspx，然後將識別碼變更 ddlLogAppEvents。
2. 設定**AutoPostBack**屬性 DropDownList 以**true**。
3. 將三個項目加入至 DropDownList 的項目集合。 請**文字**第一個項目的*Select Value*和-1 的值。 請**文字**和**值**第二個項目的**True**和**文字**和**值**第三個項目**False**。
4. 將新的標籤加入至 default.aspx。 變更要識別碼**lblLogAppEvents**。
5. 開啟 default.aspx 的程式碼後置檢視，並加入新的宣告變數的型別 HealthMonitoringSection，如下所示：


[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. 將下列程式碼加入至網頁中的現有程式碼\_初始化：


[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. 按兩下 DropDownList 和 SelectedIndexChanged 事件中加入下列程式碼：


[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. 瀏覽 default.aspx。
2. 設定下拉式清單中，為**False**。
3. 清除 事件檢視器中的應用程式記錄檔。
4. 按一下按鈕來變更應用程式的偵錯屬性。
5. 重新整理應用程式記錄檔事件檢視器中。 

    1. 所記錄的任何事件
    2. 為什麼或原因為何？
6. 設定下拉式清單中，為**，則為 True。**
7. 按一下按鈕來切換應用程式的偵錯屬性。
8. 重新整理應用程式登入事件檢視器。 

    1. 所記錄的任何事件
    2. 應用程式關機的原因是什麼？
9. 嘗試開啟和關閉記錄功能，並查看 web.config 檔案所做的變更。

## <a name="more-information"></a>詳細資訊：

ASP.NET 2.0 的提供者模型可讓您建立自己的提供者不只是應用程式檢測，但許多其他用途以及例如成員資格、 設定檔等等。如需撰寫自訂的提供者，將應用程式事件記錄到文字檔的詳細資訊，請瀏覽[此連結](https://msdn.microsoft.com/library/default.asp?url=/library/en-us/dnaspp/html/ASPNETProvMod_Prt6.asp)。
