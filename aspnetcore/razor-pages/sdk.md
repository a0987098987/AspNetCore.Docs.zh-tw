---
標題： ' ASP.NET Core Razor SDK ' 作者：描述： ' 瞭解 Razor ASP.NET Core 中的頁面，如何讓撰寫以頁面為焦點的案例更容易且更具生產力，而不是使用 MVC。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

---
# <a name="aspnet-core-razor-sdk"></a>ASP.NET Core Razor SDK

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="overview"></a>總覽

[!INCLUDE[](~/includes/2.1-SDK.md)]包含 `Microsoft.NET.Sdk.Razor` MSBuild SDK （ Razor sdk）。 RazorSDK：

::: moniker range=">= aspnetcore-3.0"

* 需要建立、封裝和發行包含 [Razor](xref:mvc/views/razor) ASP.NET CORE MVC 或專案之檔案的專案 [Blazor](xref:blazor/index) 。
* 包含一組預先定義的目標、屬性和專案，可讓您自訂 Razor （*. cshtml*或*razor*）檔案的編譯。

RazorSDK 包含 `Content` `Include` 屬性設定為 `**\*.cshtml` 和通配模式的專案 `**\*.razor` 。 已發行相符的檔案。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* 將建立、封裝和發行專案的體驗標準化，其中包含 [Razor](xref:mvc/views/razor) ASP.NET CORE MVC 專案的檔案。
* 包含一組預先定義的目標、屬性和專案，可讓您自訂檔案的編譯 Razor 。

RazorSDK 包含一個 `Content` 專案，並 `Include` 將屬性設定為 `**\*.cshtml` 萬用字元模式。 已發行相符的檔案。

::: moniker-end

## <a name="prerequisites"></a>必要條件

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a>使用 Razor SDK

大部分的 web 應用程式都不需要明確參考 Razor SDK。

::: moniker range=">= aspnetcore-3.0"

若要使用 Razor SDK 來建立包含 Razor 視圖或頁面的類別庫 Razor ，建議您從 [ Razor 類別庫（RCL）] 專案範本開始。 用來建立 Blazor （*razor*）檔案的 RCL 最少需要[AspNetCore 元件](https://www.nuget.org/packages/Microsoft.AspNetCore.Components)套件的參考。 用來建立 Razor 視圖或頁面（*. cshtml*檔案）的 RCL，至少需要 `netcoreapp3.0` 以或更新版本為目標，並 `FrameworkReference` 在其專案檔中具有[AspNetCore 中繼套件。](xref:fundamentals/metapackage-app)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

若要使用 Razor SDK 來建立包含 Razor 視圖或頁面的類別庫 Razor ：

* 使用 `Microsoft.NET.Sdk.Razor` 來取代 `Microsoft.NET.Sdk`：

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* 通常，必須要有套件參考， `Microsoft.AspNetCore.Mvc` 才能接收建立和編譯 Razor 頁面和視圖所需的其他相依性 Razor 。 您的專案至少應該將套件參考新增至：

  * `Microsoft.AspNetCore.Razor.Design`
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`

  `Microsoft.AspNetCore.Razor.Design`封裝會提供 Razor 專案的編譯工作和目標。

  `Microsoft.AspNetCore.Mvc` 中包含上述套件。 下列標記顯示的專案檔會使用 Razor SDK 來建立 Razor ASP.NET Core Razor Pages 應用程式的檔案：

  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> `Microsoft.AspNetCore.Razor.Design`和 `Microsoft.AspNetCore.Mvc.Razor.Extensions` 封裝包含在[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)中。 不過，版本較少的 `Microsoft.AspNetCore.App` 套件參考會針對不包含最新版本的應用程式提供中繼套件 `Microsoft.AspNetCore.Razor.Design` 。 專案必須參考一致版本的 `Microsoft.AspNetCore.Razor.Design` （或 `Microsoft.AspNetCore.Mvc` ）， Razor 才會包含的最新組建時間修正。 如需詳細資訊，請參閱[此 GitHub 問題](https://github.com/aspnet/Razor/issues/2553)。

::: moniker-end

### <a name="properties"></a>屬性

下列屬性會將 Razor 的 SDK 行為控制為專案組建的一部分：

* `RazorCompileOnBuild`：當時 `true` ，會編譯併發出 Razor 元件，做為建立專案的一部分。 預設為 `true`。
* `RazorCompileOnPublish`：當時 `true` ，會編譯併發出 Razor 元件，做為發行專案的一部分。 預設為 `true`。

下表中的屬性和專案可用來設定 SDK 的輸入和輸出 Razor 。

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> 從 ASP.NET Core 3.0 開始， Razor 如果 `RazorCompileOnBuild` 專案檔中的或 `RazorCompileOnPublish` MSBuild 屬性已停用，則預設不會提供 MVC 視圖或頁面。 應用程式必須加入 AspNetCore 的明確參考[ Razor 。Microsoft.aspnetcore.mvc.razor.runtimecompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation)套件（如果應用程式依賴執行時間編譯來處理*cshtml*檔案）。

::: moniker-end

| 項目 | 描述 |
| ----- | ---
標題： ' ASP.NET Core Razor SDK ' 作者：描述： ' 瞭解 Razor ASP.NET Core 中的頁面，如何讓撰寫以頁面為焦點的案例更容易且更具生產力，而不是使用 MVC。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Razor SDK ' 作者：描述： ' 瞭解 Razor ASP.NET Core 中的頁面，如何讓撰寫以頁面為焦點的案例更容易且更具生產力，而不是使用 MVC。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Razor SDK ' 作者：描述： ' 瞭解 Razor ASP.NET Core 中的頁面，如何讓撰寫以頁面為焦點的案例更容易且更具生產力，而不是使用 MVC。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

------ | |`RazorGenerate` |屬於程式碼產生之輸入的專案元素（*. cshtml*檔案）。 | |`RazorComponent` |做為元件程式碼產生之輸入的專案元素（*razor*檔案） Razor 。 | |`RazorCompile` |做為編譯目標輸入的專案元素（*.cs*檔案） Razor 。 使用此 `ItemGroup` 來指定要編譯到元件中的其他檔案 Razor 。 | |`RazorTargetAssemblyAttribute` |用來編寫元件屬性的專案元素 Razor 。 例如：  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">`| |`RazorEmbeddedResource` |做為內嵌資源加入至產生之元件的專案元素 Razor 。 |

::: moniker range=">= aspnetcore-3.0"

| 屬性 | 描述 |
| ---
標題： ' ASP.NET Core Razor SDK ' 作者：描述： ' 瞭解 Razor ASP.NET Core 中的頁面，如何讓撰寫以頁面為焦點的案例更容易且更具生產力，而不是使用 MVC。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Razor SDK ' 作者：描述： ' 瞭解 Razor ASP.NET Core 中的頁面，如何讓撰寫以頁面為焦點的案例更容易且更具生產力，而不是使用 MVC。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

---- |---標題： ' ASP.NET Core Razor SDK ' 作者：描述： ' 瞭解 Razor ASP.NET Core 中的頁面如何讓撰寫以頁面為焦點的案例更容易且更具生產力，而不是使用 MVC。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Razor SDK ' 作者：描述： ' 瞭解 Razor ASP.NET Core 中的頁面，如何讓撰寫以頁面為焦點的案例更容易且更具生產力，而不是使用 MVC。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Razor SDK ' 作者：描述： ' 瞭解 Razor ASP.NET Core 中的頁面，如何讓撰寫以頁面為焦點的案例更容易且更具生產力，而不是使用 MVC。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

------ | |`RazorTargetName` |產生之元件的檔案名（不含副檔名） Razor 。 | |`RazorOutputPath` |Razor輸出目錄。 | |`RazorCompileToolset` |用來判斷用來建立元件的工具組 Razor 。 有效值是 `Implicit`、`RazorSDK` 和 `PrecompilationTool`。 | |[EnableDefaultContentItems](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) |預設為 `true` 。 當時 `true` ，會包含*web.config*、 *. json*和*cshtml*檔案做為專案中的內容。 透過來參考時 `Microsoft.NET.Sdk.Web` ，也會包含*wwwroot*和 config 檔案底下的檔案。 | |`EnableDefaultRazorGenerateItems` |當時 `true` ，會從 *.cshtml* `Content` 專案的專案中包含 cshtml 檔案。 `RazorGenerate` | |`GenerateRazorTargetAssemblyInfo` |當時 `true` ，會產生包含所指定屬性的 *.cs*檔案 `RazorAssemblyAttribute` ，並在編譯輸出中包含檔案。 | |`EnableDefaultRazorTargetAssemblyInfoAttributes` |當時 `true` ，會將一組預設的元件屬性加入至 `RazorAssemblyAttribute` 。 | |`CopyRazorGenerateFilesToPublishDirectory` |當時 `true` ，會將 `RazorGenerate` 專案（*cshtml*）檔案複製到發行目錄。 通常， Razor 如果已發行的應用程式在組建階段或發行時間參與編譯，則不需要檔案。 預設為 `false`。 | |`PreserveCompilationReferences` |當時 `true` ，將參考元件專案複製到發行目錄。 一般來說，如果 Razor 在組建時間或發行時間發生編譯，則發行的應用程式不需要參考元件。 `true`如果您已發佈的應用程式需要執行時間編譯，請設定為。 例如， `true` 如果應用程式在執行時間修改*cshtml*檔案，或使用內嵌的視圖，請將值設定為。 預設為 `false`。 | |`IncludeRazorContentInPack` |當時 `true` ，所有 Razor 內容專案（*cshtml*檔案）都會標示為包含在產生的 NuGet 套件中。 預設為 `false`。 | |`EmbedRazorGenerateSources` |當時 `true` ，會將 RazorGenerate （*cshtml*）專案當做內嵌檔案加入至產生的 Razor 元件。 預設為 `false`。 | |`UseRazorBuildServer` |當時 `true` ，會使用持續性組建伺服器進程來卸載程式碼產生工作。 預設值為 `UseSharedCompilation`。 | |`GenerateMvcApplicationPartsAssemblyAttributes` |當時 `true` ，SDK 會在執行時間產生 MVC 用來執行應用程式元件探索的其他屬性。 | |`DefaultWebContentItemExcludes` |要從以 Web 或 SDK 為目標之專案中的專案群組排除的專案元素的萬用字元模式 `Content` Razor | | `ExcludeConfigFilesFromBuildOutput` |當時 `true` ， *.config*和 *. json*檔案不會複製到組建輸出目錄。 | |`AddRazorSupportForMvc` |當時 `true` ， Razor 會將 SDK 設定為新增建立包含 mvc 視圖或頁面之應用程式時所需的 MVC 設定支援 Razor 。 針對以 Web SDK 為目標的 .NET Core 3.0 或更新版本專案，會以隱含方式設定此屬性 | |`RazorLangVersion` |Razor要設為目標的語言版本。 |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| 屬性 | 描述 |
| ---
標題： ' ASP.NET Core Razor SDK ' 作者：描述： ' 瞭解 Razor ASP.NET Core 中的頁面，如何讓撰寫以頁面為焦點的案例更容易且更具生產力，而不是使用 MVC。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Razor SDK ' 作者：描述： ' 瞭解 Razor ASP.NET Core 中的頁面，如何讓撰寫以頁面為焦點的案例更容易且更具生產力，而不是使用 MVC。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

---- |---標題： ' ASP.NET Core Razor SDK ' 作者：描述： ' 瞭解 Razor ASP.NET Core 中的頁面如何讓撰寫以頁面為焦點的案例更容易且更具生產力，而不是使用 MVC。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Razor SDK ' 作者：描述： ' 瞭解 Razor ASP.NET Core 中的頁面，如何讓撰寫以頁面為焦點的案例更容易且更具生產力，而不是使用 MVC。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

-
標題： ' ASP.NET Core Razor SDK ' 作者：描述： ' 瞭解 Razor ASP.NET Core 中的頁面，如何讓撰寫以頁面為焦點的案例更容易且更具生產力，而不是使用 MVC。 '
monikerRange： ms-chap： ms. custom： ms. date： no-loc：
- 'Blazor'
- 'Identity'
- 'Let's Encrypt'
- 'Razor'
- ' SignalR ' uid： 

------ | |`RazorTargetName` |產生之元件的檔案名（不含副檔名） Razor 。 | |`RazorOutputPath` |Razor輸出目錄。 | |`RazorCompileToolset` |用來判斷用來建立元件的工具組 Razor 。 有效值是 `Implicit`、`RazorSDK` 和 `PrecompilationTool`。 | |[EnableDefaultContentItems](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) |預設為 `true` 。 當時 `true` ，會包含*web.config*、 *. json*和*cshtml*檔案做為專案中的內容。 透過來參考時 `Microsoft.NET.Sdk.Web` ，也會包含*wwwroot*和 config 檔案底下的檔案。 | |`EnableDefaultRazorGenerateItems` |當時 `true` ，會從 *.cshtml* `Content` 專案的專案中包含 cshtml 檔案。 `RazorGenerate` | |`GenerateRazorTargetAssemblyInfo` |當時 `true` ，會產生包含所指定屬性的 *.cs*檔案 `RazorAssemblyAttribute` ，並在編譯輸出中包含檔案。 | |`EnableDefaultRazorTargetAssemblyInfoAttributes` |當時 `true` ，會將一組預設的元件屬性加入至 `RazorAssemblyAttribute` 。 | |`CopyRazorGenerateFilesToPublishDirectory` |當時 `true` ，會將 `RazorGenerate` 專案（*cshtml*）檔案複製到發行目錄。 通常， Razor 如果已發行的應用程式在組建階段或發行時間參與編譯，則不需要檔案。 預設為 `false`。 | |`CopyRefAssembliesToPublishDirectory` |當時 `true` ，將參考元件專案複製到發行目錄。 一般來說，如果 Razor 在組建時間或發行時間發生編譯，則發行的應用程式不需要參考元件。 `true`如果您已發佈的應用程式需要執行時間編譯，請設定為。 例如， `true` 如果應用程式在執行時間修改*cshtml*檔案，或使用內嵌的視圖，請將值設定為。 預設為 `false`。 | |`IncludeRazorContentInPack` |當時 `true` ，所有 Razor 內容專案（*cshtml*檔案）都會標示為包含在產生的 NuGet 套件中。 預設為 `false`。 | |`EmbedRazorGenerateSources` |當時 `true` ，會將 RazorGenerate （*cshtml*）專案當做內嵌檔案加入至產生的 Razor 元件。 預設為 `false`。 | |`UseRazorBuildServer` |當時 `true` ，會使用持續性組建伺服器進程來卸載程式碼產生工作。 預設值為 `UseSharedCompilation`。 | |`GenerateMvcApplicationPartsAssemblyAttributes` |當時 `true` ，SDK 會在執行時間產生 MVC 用來執行應用程式元件探索的其他屬性。 | |`DefaultWebContentItemExcludes` |要從以 Web 或 SDK 為目標之專案中的專案群組排除的專案元素的萬用字元模式 `Content` Razor | | `ExcludeConfigFilesFromBuildOutput` |當時 `true` ， *.config*和 *. json*檔案不會複製到組建輸出目錄。 | |`AddRazorSupportForMvc` |當時 `true` ， Razor 會將 SDK 設定為新增建立包含 mvc 視圖或頁面之應用程式時所需的 MVC 設定支援 Razor 。 針對以 Web SDK 為目標的 .NET Core 3.0 或更新版本專案，會以隱含方式設定此屬性 | |`RazorLangVersion` |Razor要設為目標的語言版本。 |

::: moniker-end

如需屬性的詳細資訊，請參閱[MSBuild 屬性](/visualstudio/msbuild/msbuild-properties)。

### <a name="targets"></a>目標

RazorSDK 會定義兩個主要目標：

* `RazorGenerate`：程式碼會從專案元素產生 *.cs*檔案 `RazorGenerate` 。 使用 `RazorGenerateDependsOn` 屬性來指定可在此目標之前或之後執行的其他目標。
* `RazorCompile`：將產生的 *.cs*檔案編譯成 Razor 元件。 使用 `RazorCompileDependsOn` 來指定可在此目標之前或之後執行的其他目標。
* `RazorComponentGenerate`：程式碼會產生專案元素的 *.cs*檔案 `RazorComponent` 。 使用 `RazorComponentGenerateDependsOn` 屬性來指定可在此目標之前或之後執行的其他目標。

### <a name="runtime-compilation-of-razor-views"></a>執行時間編譯的 Razor 視圖

* 根據預設， Razor SDK 不會發佈執行執行時間編譯所需的參考元件。 當應用程式模型依賴執行階段編譯時，這會導致編譯失敗&mdash;例如，發佈應用程式之後，應用程式使用內嵌的檢視或變更檢視。 將 `CopyRefAssembliesToPublishDirectory` 設為 `true` 以繼續發佈參考組件。

* 針對 web 應用程式，請確定您的應用程式是以 SDK 為目標 `Microsoft.NET.Sdk.Web` 。

## <a name="razor-language-version"></a>Razor語言版本

以 SDK 為目標時 `Microsoft.NET.Sdk.Web` ， Razor 會從應用程式的目標 framework 版本推斷語言版本。 針對以 SDK 為目標的專案 `Microsoft.NET.Sdk.Razor` ，或在罕見的情況下，應用程式需要的 Razor 語言版本與推斷的值不同，您可以 `<RazorLangVersion>` 在應用程式的專案檔中設定屬性來設定版本：

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

Razor的語言版本與所建立之執行時間的版本緊密整合。 不支援以不是針對執行時間設計的語言版本為目標，而且可能會產生組建錯誤。

## <a name="additional-resources"></a>其他資源

* [適用於 .NET Core 之 csproj 格式的新增項目](/dotnet/core/tools/csproj)
* [一般 MSBuild 專案專案](/visualstudio/msbuild/common-msbuild-project-items)
