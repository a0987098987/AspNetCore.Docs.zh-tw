---
標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

---
# <a name="host-aspnet-core-in-a-web-farm"></a>在 Web 伺服陣列上裝載 ASP.NET Core

依[Chris Ross](https://github.com/Tratcher)

*Web 伺服陣列*是一組兩個或多個 Web 伺服器 (或稱為「節點」**)，用於裝載應用程式的多個執行個體。 當使用者的要求抵達 Web 伺服陣列時，「負載平衡器」** 會將要求分散到 Web 伺服陣列的節點。 Web 伺服陣列改善：

* **可靠性/可用性**：當一或多個節點失敗時，負載平衡器可以將要求路由至其他正常運作的節點，以繼續處理要求。
* **容量/效能**：多個節點可以處理比單一伺服器更多的要求。 負載平衡器可以將要求分散到節點，藉以平衡工作負載。
* 擴充**性**：當需要更多或更少的容量時，可以增加或減少作用中節點的數目，以符合工作負載。 Web 伺服陣列的平台技術 (例如 [Azure App Service](https://azure.microsoft.com/services/app-service/)) 可以依系統管理員的要求自動新增或移除節點，也可以在沒有人為介入的情況下自動新增或移除節點。
* 可**維護性**： web 伺服陣列的節點可以依賴一組共用服務，這會導致系統管理變得更容易。 例如，Web 伺服陣列的節點可以依賴單一資料庫伺服器和靜態資源的通用網路位置，例如影像和可下載檔案。

本主題針對依賴共用資源的 Web 伺服陣列，說明其中所裝載 ASP.NET Core 應用程式的設定和相依性。

## <a name="general-configuration"></a>一般設定

<xref:host-and-deploy/index>  
了解如何設定裝載環境及部署 ASP.NET Core 應用程式。 在 Web 伺服陣列的每個節點上設定處理序管理員，以自動啟動和重新啟動應用程式。 每個節點都需要 ASP.NET Core 執行階段。 如需詳細資訊，請參閱文件的[主機和部署](xref:host-and-deploy/index)區段中的主題。

<xref:host-and-deploy/proxy-load-balancer>  
了解裝載在 Proxy 伺服器和負載平衡器 (通常會遮蔽重要要求資訊) 後方之應用程式的設定。

<xref:host-and-deploy/azure-apps/index>  
[Azure App Service](https://azure.microsoft.com/services/app-service/) 是 [Microsoft 雲端運算平台服務](https://azure.microsoft.com/)，用於裝載 Web 應用程式，包括 ASP.NET Core。 App Service 是受到完整管理的平台，可提供自動調整規模、負載平衡、修補和持續部署等功能。

## <a name="app-data"></a>應用程式資料

當應用程式擴展到多個執行個體時，便可能需要跨節點共用的應用程式狀態。 如果狀態是暫時性的，請考慮共用 [IDistributedCache](/dotnet/api/microsoft.extensions.caching.distributed.idistributedcache)。 如果共用狀態需要持續性，請考慮將共用狀態儲存在資料庫中。

## <a name="required-configuration"></a>必要設定

資料保護和快取需要部署到 Web 伺服陣列的應用程式設定。

### <a name="data-protection"></a>資料保護

應用程式使用 [ASP.NET Core 資料保護系統](xref:security/data-protection/introduction)來保護資料。 資料保護會依賴一組儲存在*金鑰環*中的密碼編譯金鑰。 初始化資料保護系統時，會套用在本機儲存金鑰環的[預設設定](xref:security/data-protection/configuration/default-settings)。 在預設設定下，Web 伺服陣列的每個節點上都會儲存一個唯一的金鑰環。 因此，每個 Web 伺服陣列節點都無法解密由任何其他節點上的應用程式加密的資料。 預設設定通常不適用於在 Web 伺服陣列中裝載應用程式。 實作共用金鑰環的替代方式，是一律將使用者要求路由至相同的節點。 如需有關 Web 伺服陣列部署的資料保護系統設定詳細資訊，請參閱 <xref:security/data-protection/configuration/overview>。

### <a name="caching"></a>Caching

在 Web 伺服陣列環境中，快取機制必須跨 Web 伺服陣列節點共用快取項目。 快取必須依賴於通用 Redis 快取、共用的 SQL Server 資料庫，或跨 Web 伺服陣列共用快取項目的自訂快取實作。 如需詳細資訊，請參閱<xref:performance/caching/distributed>。

## <a name="dependent-components"></a>相依元件

下列案例不需要額外的設定，但它們取決於需要 Web 伺服陣列設定的技術。

| 案例 | 相依於 &hellip; |
| ---
標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

---- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

---------- | |驗證 |資料保護（請參閱 <xref:security/data-protection/configuration/overview> ）。<br><br>如需詳細資訊，請參閱 <xref:security/authentication/cookie> 和 <xref:security/cookie-sharing>。 | |Identity |驗證和資料庫設定。<br><br>如需詳細資訊，請參閱<xref:security/authentication/identity>。 | |會話 |資料保護（加密的 cookie）（請參閱 <xref:security/data-protection/configuration/overview> ）和快取（請參閱 <xref:performance/caching/distributed> ）。<br><br>如需詳細資訊，請參閱[會話和狀態管理：會話狀態](xref:fundamentals/app-state#session-state)。 | |TempData |資料保護（加密的 cookie）（請參閱 <xref:security/data-protection/configuration/overview> ）或會話（請參閱[會話和狀態管理：會話狀態](xref:fundamentals/app-state#session-state)）。<br><br>如需詳細資訊，請參閱[會話和狀態管理： TempData](xref:fundamentals/app-state#tempdata)。 | |防偽 |資料保護（請參閱 <xref:security/data-protection/configuration/overview> ）。<br><br>如需詳細資訊，請參閱<xref:security/anti-request-forgery>。 |

## <a name="troubleshoot"></a>疑難排解

### <a name="data-protection-and-caching"></a>資料保護和快取

如果未為 Web 伺服陣列環境設定資料保護或快取，則在處理要求時，會發生間歇性的錯誤。 發生這種情況是因為節點不會共用相同的資源，且使用者的要求並不是一律路由傳送回相同的節點。

假設使用者使用 cookie 驗證來登入應用程式。 使用者在某個 Web 伺服陣列節點上登入應用程式。 如果使用者的下一個要求抵達他們登入的相同節點，則應用程式將能夠解密驗證 cookie，並允許存取應用程式的資源。 如果使用者的下一個要求抵達不同的節點，則應用程式無法解密來自使用者登入節點的驗證 cookie，且針對所請求資源的授權會失敗。

如果**間歇性**發生下列任何徵兆，則問題通常可追溯到 Web 伺服陣列環境的不正確資料保護或快取設定：

* 驗證中斷：驗證 cookie 設定錯誤或無法解密。 OAuth (Facebook、Microsoft、Twitter) 或 OpenIdConnect 登入失敗並出現錯誤「相互關聯失敗」。
* 授權中斷： Identity 遺失。
* 工作階段狀態將會遺失資料。
* 快取項目會消失。
* TempData 將會失敗。
* 貼文失敗：防偽檢查失敗。

如需有關 Web 伺服陣列部署的資料保護設定詳細資訊，請參閱 <xref:security/data-protection/configuration/overview>。 如需有關 Web 伺服陣列部署的快取設定詳細資訊，請參閱 <xref:performance/caching/distributed>。

## <a name="obtain-data-from-apps"></a>從應用程式取得資料

若 Web 伺服器陣列應用程式能夠回應要求、請使用終端機內嵌中介軟體從應用程式取得要求、連線與額外資料。 如如需詳細資訊與範例程式碼，請參閱 <xref:test/troubleshoot#obtain-data-from-an-app>。

## <a name="additional-resources"></a>其他資源

* [適用于 Windows 的自訂腳本擴充](/azure/virtual-machines/extensions/custom-script-windows)功能：在 Azure 虛擬機器上下載並執行腳本，這對於部署後設定和軟體安裝很有用。
* <xref:host-and-deploy/proxy-load-balancer>
 