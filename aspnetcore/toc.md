---
ms.openlocfilehash: a3ac1ac389892681e8c122d569e6325353ed6e88
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341676"
---
# [ASP.NET Core 文件](/aspnet/#pivot=core)

# 總覽
## [關於 ASP.NET Core](xref:index)
## [比較 ASP.NET Core 與 ASP.NET](xref:fundamentals/choose-between-aspnet-and-aspnetcore)
## [比較 .NET Core 與 .NET Framework](/dotnet/articles/standard/choosing-core-framework-server)

# [開始使用](xref:getting-started)

# 新功能
## [2.2 中的新功能](xref:aspnetcore-2.2)
## [2.1 的新功能](xref:aspnetcore-2.1)
## [2.0 的新功能](xref:aspnetcore-2.0)
## [1.1 的新功能](xref:aspnetcore-1.1)

# 教學課程
## Web API 應用程式
### [建立 Web API](xref:tutorials/first-web-api)
### [Web API (使用 MongoDB)](xref:tutorials/first-mongo-app)
## Web 應用程式
### [Razor 頁面](xref:tutorials/razor-pages/index)
### [MVC](xref:tutorials/first-mvc-app/index)

## 即時 Web 應用程式
### [使用 JavaScript 的 SignalR](xref:tutorials/signalr)
### [使用 TypeScript 的 SignalR](xref:tutorials/signalr-typescript-webpack)
## [建立原生行動應用程式的後端服務](xref:mobile/native-mobile-backend)

## 資料存取
### [使用 Razor Pages 的 EF Core](xref:data/ef-rp/index)
### [使用 MVC 的 EF Core，現有 DB](/ef/core/get-started/aspnetcore/existing-db)
### [使用 MVC 的 EF Core，新的 DB](/ef/core/get-started/aspnetcore/new-db)
### [使用 MVC 的 EF Core，長篇教學課程](xref:data/ef-mvc/index)

# Fundamentals
## [概觀](xref:fundamentals/index)
## [應用程式啟動](xref:fundamentals/startup)
## [相依性插入 (服務)](xref:fundamentals/dependency-injection)
## [路由傳送](xref:fundamentals/routing)
## [環境 (開發、階段、生產)](xref:fundamentals/environments)
## [組態](xref:fundamentals/configuration/index)
## [選項](xref:fundamentals/configuration/options)
## [記錄](xref:fundamentals/logging/index)
## [處理錯誤](xref:fundamentals/error-handling)
## 中介軟體
### [概觀](xref:fundamentals/middleware/index)
### [Factory 式中介軟體](xref:fundamentals/middleware/extensibility)
### [Factory 式中介軟體與協力廠商容器](xref:fundamentals/middleware/extensibility-third-party-container)
## 主機
### [概觀](xref:fundamentals/host/index)
### [Web 代管](xref:fundamentals/host/web-host)
### [一般代管](xref:fundamentals/host/generic-host)
## [伺服器](xref:fundamentals/servers/index)
## [初始化 HTTP 要求](xref:fundamentals/http-requests)

# Web 應用程式
## Razor Pages
### [概觀](xref:razor-pages/index)
### [Razor Pages 教學課程](xref:tutorials/razor-pages/index)
#### [開始使用](xref:tutorials/razor-pages/razor-pages-start)
#### [新增模型](xref:tutorials/razor-pages/model)
#### [Scaffolding](xref:tutorials/razor-pages/page)
#### [使用 DB](xref:tutorials/razor-pages/sql)
#### [更新頁面](xref:tutorials/razor-pages/da1)
#### [新增搜尋](xref:tutorials/razor-pages/search)
#### [新增欄位](xref:tutorials/razor-pages/new-field)
#### [新增驗證](xref:tutorials/razor-pages/validation)

## MVC
### [MVC 概觀](xref:mvc/overview)
### [MVC 教學課程](xref:tutorials/first-mvc-app/index)
#### [開始使用](xref:tutorials/first-mvc-app/start-mvc)
#### [新增控制器](xref:tutorials/first-mvc-app/adding-controller)
#### [新增檢視](xref:tutorials/first-mvc-app/adding-view)
#### [新增模型](xref:tutorials/first-mvc-app/adding-model)
#### [使用 DB](xref:tutorials/first-mvc-app/working-with-sql)
#### [控制器動作及檢視](xref:tutorials/first-mvc-app/controller-methods-views)
#### [新增搜尋](xref:tutorials/first-mvc-app/search)
#### [新增欄位](xref:tutorials/first-mvc-app/new-field)
#### [新增驗證](xref:tutorials/first-mvc-app/validation)
#### [檢查 Details 和 Delete 方法](xref:tutorials/first-mvc-app/details)

### [篩選](xref:razor-pages/filter)
### [Razor 類別庫](xref:razor-pages/ui-class)
### [路由及應用程式慣例](xref:razor-pages/razor-pages-conventions)
### [上傳檔案](xref:razor-pages/upload-files)
### [Razor SDK](xref:razor-pages/sdk)

### [檢視](xref:mvc/views/overview)
### [部分檢視](xref:mvc/views/partial)
### [控制器](xref:mvc/controllers/actions)
### [路由傳送](xref:mvc/controllers/routing)
### [將檔案上傳](xref:mvc/models/file-uploads)
### [相依性插入 - 控制器](xref:mvc/controllers/dependency-injection)
### [相依性插入 - 檢視](xref:mvc/views/dependency-injection)
### [單元測試](xref:mvc/controllers/testing)
## [工作階段和應用程式狀態](xref:fundamentals/app-state)
## 標籤協助程式
### [概觀](xref:mvc/views/tag-helpers/intro)
### [建立標記協助程式](xref:mvc/views/tag-helpers/authoring)
### [在表單中使用標記協助程式](xref:mvc/views/working-with-forms)
### [標記協助程式元件](xref:mvc/views/tag-helpers/th-components)
### 內建標記協助程式
#### [錨點](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)
#### [快取](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
#### [分散式快取](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
#### [環境](xref:mvc/views/tag-helpers/builtin-th/environment-tag-helper)
#### [表單](mvc/views/working-with-forms.md#the-form-tag-helper)
#### [影像](xref:mvc/views/tag-helpers/builtin-th/image-tag-helper)
#### [輸入](mvc/views/working-with-forms.md#the-input-tag-helper)
#### [Label](mvc/views/working-with-forms.md#the-label-tag-helper)
#### [Partial](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)
#### [選取](mvc/views/working-with-forms.md#the-select-tag-helper)
#### [文字區域](mvc/views/working-with-forms.md#the-textarea-tag-helper)
#### [驗證訊息](mvc/views/working-with-forms.md#the-validation-message-tag-helper)
#### [驗證摘要](mvc/views/working-with-forms.md#the-validation-summary-tag-helper)
## [版面配置](xref:mvc/views/layout)
## [靜態檔案](xref:fundamentals/static-files)
## [模型繫結](xref:mvc/models/model-binding)
## [模型驗證](xref:mvc/models/validation)
## [Razor 語法](xref:mvc/views/razor)
## 進階
### [檢視元件](xref:mvc/views/view-components)
### [檢視編譯](xref:mvc/views/view-compilation)
### [應用程式模型](xref:mvc/controllers/application-model)
### [篩選](xref:mvc/controllers/filters)
### [區域](xref:mvc/controllers/areas)
### [應用程式組件](xref:mvc/extensibility/app-parts)
### [自訂模型繫結](xref:mvc/advanced/custom-model-binding)
### [相容性版本](xref:mvc/compatibility-version)

# Web API 應用程式
## [概觀](xref:web-api/index)

## 教學課程
### [建立 Web API](xref:tutorials/first-web-api)
### [Web API (使用 MongoDB)](xref:tutorials/first-mongo-app)

## Swagger / OpenAPI
### [概觀](xref:tutorials/web-api-help-pages-using-swagger)
### [開始使用 Swashbuckle](xref:tutorials/get-started-with-swashbuckle)
### [開始使用 NSwag](xref:tutorials/get-started-with-nswag)
## [動作傳回型別](xref:web-api/action-return-types)
## [格式化回應資料](xref:web-api/advanced/formatting)
## [自訂格式器](xref:web-api/advanced/custom-formatters)

## [分析器](xref:web-api/advanced/analyzers)
## [慣例](xref:web-api/advanced/conventions)

# 即時應用程式
## [SignalR 概觀](xref:signalr/introduction)
## [支援的平台](xref:signalr/supported-platforms)
## 教學課程
### [使用 JavaScript 的 SignalR](xref:tutorials/signalr)
### [使用 TypeScript 的 SignalR](xref:tutorials/signalr-typescript-webpack)
## [範例](https://github.com/aspnet/SignalR-samples)
## 伺服器概念
### [中樞](xref:signalr/hubs)
### [HubContext](xref:signalr/hubcontext)
### [使用者和群組](xref:signalr/groups)
### [發佈至 Azure](xref:signalr/publish-to-azure-web-app)
### [API 設計考量](xref:signalr/api-design)
## 用戶端
### [.NET 用戶端](xref:signalr/dotnet-client)
### [.NET API 參考](/dotnet/api/microsoft.aspnetcore.signalr.client)
### [Java 用戶端](xref:signalr/java-client)
### [Java API 參考](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
### [JavaScript 用戶端](xref:signalr/javascript-client)
### [JavaScript API 參考](/javascript/api/?view=signalr-js-latest)
## 裝載和調整規模
### [概觀](xref:signalr/scale)
### [Azure SignalR Service](/azure/azure-signalr/signalr-overview)
### [Redis 後擋板](xref:signalr/redis-backplane)
## [組態](xref:signalr/configuration)
## [驗證與授權](xref:signalr/authn-and-authz)
## [安全性考量](xref:signalr/security)
## [MessagePack 中樞通訊協定](xref:signalr/messagepackhubprotocol)
## [資料流](xref:signalr/streaming)
## [比較 SignalR 與 SignalR Core](xref:signalr/version-differences)
## [未使用 SignalR 的 WebSocket](xref:fundamentals/websockets)

# 測試、偵錯及疑難排解
## [單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
## [Razor 頁面單元測試](xref:test/razor-pages-tests)
## [測試控制器](xref:mvc/controllers/testing)
## [遠端偵錯](/visualstudio/debugger/remote-debugging-azure)
## [快照集偵錯](/azure/application-insights/app-insights-snapshot-debugger)
## [Visual Studio 中的快照集偵錯](/visualstudio/debugger/debug-live-azure-applications)
## [整合測試](xref:test/integration-tests)
## [負載與壓力測試](xref:test/loadtests)
## [疑難排解](xref:test/troubleshoot)
## [記錄](xref:fundamentals/logging/index)

# 資料存取
## 教學課程
### 使用 Razor Pages 的 EF Core
#### [概觀](xref:data/ef-rp/index)
#### [開始使用](xref:data/ef-rp/intro)
#### [建立、讀取、更新及刪除](xref:data/ef-rp/crud)
#### [排序、篩選、分頁和分組](xref:data/ef-rp/sort-filter-page)
#### [移轉](xref:data/ef-rp/migrations)
#### [建立複雜的資料模型](xref:data/ef-rp/complex-data-model)
#### [讀取相關資料](xref:data/ef-rp/read-related-data)
#### [更新相關資料](xref:data/ef-rp/update-related-data)
#### [處理並行衝突](xref:data/ef-rp/concurrency)
### [使用 MVC 的 EF Core，新的 DB](/ef/core/get-started/aspnetcore/new-db)
### [使用 MVC 的 EF Core，現有 DB](/ef/core/get-started/aspnetcore/existing-db)
### 使用 MVC 的 EF Core，長篇教學課程
#### [概觀](xref:data/ef-mvc/index)
#### [開始使用](xref:data/ef-mvc/intro)
#### [建立、讀取、更新及刪除](xref:data/ef-mvc/crud)
#### [排序、篩選、分頁和分組](xref:data/ef-mvc/sort-filter-page)
#### [移轉](xref:data/ef-mvc/migrations)
#### [建立複雜的資料模型](xref:data/ef-mvc/complex-data-model)
#### [讀取相關資料](xref:data/ef-mvc/read-related-data)
#### [更新相關資料](xref:data/ef-mvc/update-related-data)
#### [處理並行衝突](xref:data/ef-mvc/concurrency)
#### [繼承](xref:data/ef-mvc/inheritance)
#### [進階主題](xref:data/ef-mvc/advanced)
## [使用 ASP.NET Core 的 EF 6](xref:data/entity-framework-6)
## 使用 Visual Studio 的 Azure 儲存體
### [已連線的服務](/azure/vs-azure-tools-connected-services-storage)
### [Blob 儲存體](/azure/vs-storage-aspnet5-getting-started-blobs/)
### [佇列儲存體](/azure/vs-storage-aspnet5-getting-started-queues/)
### [表格儲存體](/azure/vs-storage-aspnet5-getting-started-tables/)

# 用戶端開發
## [概觀](xref:client-side/index)
## [Gulp](xref:client-side/using-gulp)
## [Grunt](xref:client-side/using-grunt)
## LibMan
### [概觀](xref:client-side/libman/index)
### [CLI](xref:client-side/libman/libman-cli)
### [Visual Studio](xref:client-side/libman/libman-vs)
## [Bower](xref:client-side/bower)
## [LESS、SASS 及 Font Awesome](xref:client-side/less-sass-fa)
## [配套並縮短](xref:client-side/bundling-and-minification)
## [瀏覽器連結](xref:client-side/using-browserlink)
## 單頁應用程式
### [概觀](xref:spa/index)
### [Angular](xref:spa/angular)
### [React](xref:spa/react)
### [使用 Redux 的 React](xref:spa/react-with-redux)
### [JavaScriptServices](xref:client-side/spa-services)

# 裝載及部署
## [概觀](xref:host-and-deploy/index)
## 裝載於 Azure App Service
### [概觀](xref:host-and-deploy/azure-apps/index)
### [使用 Visual Studio 發佈](xref:tutorials/publish-to-azure-webapp-using-vs)
### [使用 CLI 工具發佈](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
### [使用 Visual Studio 及 Git 發佈](xref:host-and-deploy/azure-apps/azure-continuous-deployment)
### [使用 Azure Pipelines 的持續部署](/azure/devops/pipelines/get-started-yaml)
### [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)
### [疑難排解](xref:host-and-deploy/azure-apps/troubleshoot)
### [錯誤參考](xref:host-and-deploy/azure-iis-errors-reference)
## DevOps 
### [概觀](xref:azure/devops/index)
### [工具及下載](xref:azure/devops/tools-and-downloads)
### [部署到 App Service](xref:azure/devops/deploy-to-app-service)
### [持續整合與部署](xref:azure/devops/cicd)
### [監視及疑難排解](xref:azure/devops/monitor)
### [後續步驟](xref:azure/devops/next-steps)
## 裝載於使用 IIS 的 Windows
### [概觀](xref:host-and-deploy/iis/index)
### [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)
### [Visual Studio 中的 IIS 支援](xref:host-and-deploy/iis/development-time-iis-support)
### [IIS 模組](xref:host-and-deploy/iis/modules)
### [疑難排解](xref:host-and-deploy/iis/troubleshoot)
### [錯誤參考](xref:host-and-deploy/azure-iis-errors-reference)
## [Kestrel](xref:fundamentals/servers/kestrel)
## [HTTP.sys](xref:fundamentals/servers/httpsys)
## [Windows 服務中的主機](xref:host-and-deploy/windows-service)
## [Linux 上使用 Nginx 的主機](xref:host-and-deploy/linux-nginx)
## [Linux 上使用 Apache 的主機](xref:host-and-deploy/linux-apache)
## 裝載於 Docker
### [概觀](xref:host-and-deploy/docker/index)
### [建置 Docker 映像](/dotnet/articles/core/docker/building-net-docker-images)
### [Visual Studio Tools](xref:host-and-deploy/docker/visual-studio-tools-for-docker)
### [發行至 Docker 映像](/azure/vs-azure-tools-docker-hosting-web-apps-in-docker)
## [Proxy 和負載平衡器組態](xref:host-and-deploy/proxy-load-balancer)
## [裝載於 Web 伺服陣列](xref:host-and-deploy/web-farm)
## [Visual Studio 發行設定檔](xref:host-and-deploy/visual-studio-publish-profiles)
## [目錄結構](xref:host-and-deploy/directory-structure)
## [健康狀態檢查](xref:host-and-deploy/health-checks)

# 安全性及身分識別
## [概觀](xref:security/index)
## 驗證
### [身分識別簡介](xref:security/authentication/identity)
### [Scaffold 身分識別](xref:security/authentication/scaffold-identity)
### [將自訂使用者資料新增至身分識別](xref:security/authentication/add-user-data)
### [自訂身分識別](xref:security/authentication/customize_identity_model)
### [社群 OSS 驗證選項](xref:security/authentication/community)
### [設定身分識別](xref:security/authentication/identity-configuration)
### [使用 Windows 驗證](xref:security/authentication/windowsauth)
### [身分識別的自訂儲存體提供者](xref:security/authentication/identity-custom-storage-providers)
### 外部提供者
#### [概觀](xref:security/authentication/social/index)
#### [Facebook 驗證](xref:security/authentication/facebook-logins)
#### [Twitter 驗證](xref:security/authentication/twitter-logins)
#### [Google 驗證](xref:security/authentication/google-logins)
#### [Microsoft 驗證](xref:security/authentication/microsoft-logins)
#### [外部驗證提供者](xref:security/authentication/otherlogins)
#### [其他宣告](xref:security/authentication/social/additional-claims)
### [WS 同盟驗證](xref:security/authentication/ws-federation)
### [帳戶確認和密碼復原](xref:security/authentication/accconfirm)
### [在身分識別中啟用 QR 程式碼產生](xref:security/authentication/identity-enable-qrcodes)
### [使用 SMS 的雙因素驗證](xref:security/authentication/2fa)
### [使用沒有身分識別的 Cookie 驗證](xref:security/authentication/cookie)
### Azure Active Directory
#### [概觀](xref:security/authentication/azure-active-directory/index)
#### [將 Azure AD 整合到 Web 應用程式中](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-openidconnect-aspnetcore/)
#### [將 Azure AD B2C 整合到 Web 應用程式中](xref:security/authentication/azure-ad-b2c)
#### [將 Azure AD B2C 整合到 Web API 中](xref:security/authentication/azure-ad-b2c-webapi)
#### [從 WPF 呼叫 Web API](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-native-aspnetcore/)
#### [使用 Azure AD 在 Web 應用程式中呼叫 Web API](https://azure.microsoft.com/documentation/samples/active-directory-dotnet-webapp-webapi-openidconnect-aspnetcore/)
### [使用 IdentityServer4 保護 ASP.NET Core 應用程式](https://identityserver4.readthedocs.io/)
### [使用 Azure App Service 驗證 (簡單驗證) 保護 ASP.NET Core 應用程式](/azure/app-service/app-service-authentication-overview)
### [個別使用者帳戶](xref:security/authentication/individual)
## 授權
### [概觀](xref:security/authorization/introduction)
### [建立具有授權的 Web 應用程式](xref:security/authorization/secure-data)
### [Razor Pages 授權慣例](xref:security/authorization/razor-pages-authorization)
### [簡單授權](xref:security/authorization/simple)
### [角色型授權](xref:security/authorization/roles)
### [宣告式授權](xref:security/authorization/claims)
### [原則式授權](xref:security/authorization/policies)
### [授權原則提供者](xref:security/authorization/iauthorizationpolicyprovider)
### [要求處理常式中的相依性插入](xref:security/authorization/dependencyinjection)
### [資源型授權](xref:security/authorization/resourcebased)
### [檢視型授權](xref:security/authorization/views)
### [以配置限制身分識別](xref:security/authorization/limitingidentitybyscheme)
## 資料保護
### [概觀](xref:security/data-protection/introduction)
### [資料保護 API](xref:security/data-protection/using-data-protection)
### 取用者 API
#### [概觀](xref:security/data-protection/consumer-apis/overview)
#### [目的字串](xref:security/data-protection/consumer-apis/purpose-strings)
#### [目的階層和多租用戶](xref:security/data-protection/consumer-apis/purpose-strings-multitenancy)
#### [雜湊密碼](xref:security/data-protection/consumer-apis/password-hashing)
#### [限制受保護承載的存留期](xref:security/data-protection/consumer-apis/limited-lifetime-payloads)
#### [取消索引鍵已撤銷之承載的保護](xref:security/data-protection/consumer-apis/dangerous-unprotect)
### Configuration
#### [概觀](xref:security/data-protection/configuration/index)
#### [設定資料保護](xref:security/data-protection/configuration/overview)
#### [預設設定](xref:security/data-protection/configuration/default-settings)
#### [整個電腦的原則](xref:security/data-protection/configuration/machine-wide-policy)
#### [非 DI 感知案例](xref:security/data-protection/configuration/non-di-scenarios)
### 擴充性 API
#### [概觀](xref:security/data-protection/extensibility/index)
#### [核心加密擴充性](xref:security/data-protection/extensibility/core-crypto)
#### [金鑰管理擴充性](xref:security/data-protection/extensibility/key-management)
#### [其他 API](xref:security/data-protection/extensibility/misc-apis)
### 實作
#### [概觀](xref:security/data-protection/implementation/index)
#### [已驗證的加密詳細資料](xref:security/data-protection/implementation/authenticated-encryption-details)
#### [子機碼衍生和驗證的加密](xref:security/data-protection/implementation/subkeyderivation)
#### [內容標頭](xref:security/data-protection/implementation/context-headers)
#### [金鑰管理](xref:security/data-protection/implementation/key-management)
#### [金鑰儲存體提供者](xref:security/data-protection/implementation/key-storage-providers)
#### [待用時加密金鑰](xref:security/data-protection/implementation/key-encryption-at-rest)
#### [金鑰的不變性和設定](xref:security/data-protection/implementation/key-immutability)
#### [金鑰儲存體格式](xref:security/data-protection/implementation/key-storage-format)
#### [暫時資料保護提供者](xref:security/data-protection/implementation/key-storage-ephemeral)
### 相容性
#### [概觀](xref:security/data-protection/compatibility/index)
#### [取代 ASP.NET 中的 <machineKey>](xref:security/data-protection/compatibility/replacing-machinekey)
## [在開發過程中保護祕密](xref:security/app-secrets)
## [強制使用 HTTPS](xref:security/enforcing-ssl)
## [EU 一般資料保護規定 (GDPR) 支援](xref:security/gdpr)
## [Azure Key Vault 組態提供者](xref:security/key-vault-configuration)
## [防偽要求](xref:security/anti-request-forgery)
## [防止開啟重新導向攻擊](xref:security/preventing-open-redirects)
## [防止跨網站指令碼攻擊](xref:security/cross-site-scripting)
## [啟用跨原始來源要求 (CORS)](xref:security/cors)
## [在應用程式間共用 Cookie](xref:security/cookie-sharing)
## [IP 安全清單](xref:security/ip-safelist)

# 效能
## [概觀](xref:performance/performance-best-practices)
##  回應快取
### [概觀](xref:performance/caching/response)
### [記憶體內部快取](xref:performance/caching/memory)
### [分散式快取](xref:performance/caching/distributed)
### [回應快取中介軟體](xref:performance/caching/middleware)
## [回應壓縮](xref:performance/response-compression)
## [診斷工具](xref:performance/diagnostic-tools)
## [負載與壓力測試](xref:test/loadtests)

# 其他主題
## [全球化和當地語系化](xref:fundamentals/localization)
## [使用 Orchard Core 的可攜式物件當地語系化](xref:fundamentals/portable-object-localization)
## [URL 重寫](xref:fundamentals/url-rewriting)
## [檔案提供者](xref:fundamentals/file-providers)
## [要求的功能](xref:fundamentals/request-features)
## [存取 HttpContext](xref:fundamentals/httpcontext)
## [變更權杖](xref:fundamentals/change-tokens)
## [開啟 Web Interface for .NET (OWIN)](xref:fundamentals/owin)
## [使用託管服務的背景工作](xref:fundamentals/host/hosted-services)
## [裝載啟動組件](xref:fundamentals/configuration/platform-specific-configuration)
## [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)
## [Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)
## [使用 LoggerMessage 記錄](xref:fundamentals/logging/loggermessage)
## [使用檔案監看員](xref:tutorials/dotnet-watch)

# 移轉
## [2.2 至 3.0](xref:migration/22-to-30)
## [2.1 至 2.2](xref:migration/21-to-22)
## [2.0 至 2.1](xref:migration/20_21)
## 1.x 至 2.0
### [概觀](xref:migration/1x-to-2x/index)
### [驗證和身分識別](xref:migration/1x-to-2x/identity-2x)
## ASP.NET 至 ASP.NET Core
### [概觀](xref:migration/proper-to-2x/index)
### [MVC](xref:migration/mvc)
### [Web API](xref:migration/webapi)
### [組態](xref:migration/configuration)
### [驗證和身分識別](xref:migration/identity)
### [ClaimsPrincipal.Current](xref:migration/claimsprincipal-current)
### [身分識別的成員資格](xref:migration/proper-to-2x/membership-to-core-identity)
### [HTTP 模組到中介軟體](xref:migration/http-modules)
## [記錄 (非 ASP.NET Core)](xref:migration/logging-nonaspnetcore)

# [API 參考](/dotnet/api/?view=aspnetcore-2.1)

# [參與](https://github.com/aspnet/Docs/blob/master/CONTRIBUTING.md)
