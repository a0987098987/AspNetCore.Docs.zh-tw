---
<span data-ttu-id="e341b-101">標題： ' 設定連結器以 ASP.NET Core Blazor ' 作者：描述： ' 瞭解如何在建立應用程式時控制中繼語言（IL）連結器 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="e341b-101">title: 'Configure the Linker for ASP.NET Core Blazor' author: description: 'Learn how to control the Intermediate Language (IL) Linker when building a Blazor app.'</span></span>
<span data-ttu-id="e341b-102">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="e341b-102">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="e341b-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="e341b-103">'Blazor'</span></span>
- <span data-ttu-id="e341b-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="e341b-104">'Identity'</span></span>
- <span data-ttu-id="e341b-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="e341b-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="e341b-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="e341b-106">'Razor'</span></span>
- <span data-ttu-id="e341b-107">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="e341b-107">'SignalR' uid:</span></span> 

---
# <a name="configure-the-linker-for-aspnet-core-blazor"></a><span data-ttu-id="e341b-108">設定 ASP.NET Core 的連結器Blazor</span><span class="sxs-lookup"><span data-stu-id="e341b-108">Configure the Linker for ASP.NET Core Blazor</span></span>

<span data-ttu-id="e341b-109">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e341b-109">By [Luke Latham](https://github.com/guardrex)</span></span>

Blazor<span data-ttu-id="e341b-110">WebAssembly 會在組建期間執行[中繼語言（IL）](/dotnet/standard/managed-code#intermediate-language--execution)連結，以從應用程式的輸出元件修剪不必要的 IL。</span><span class="sxs-lookup"><span data-stu-id="e341b-110"> WebAssembly performs [Intermediate Language (IL)](/dotnet/standard/managed-code#intermediate-language--execution) linking during a build to trim unnecessary IL from the app's output assemblies.</span></span> <span data-ttu-id="e341b-111">在 Debug 設定中建立時，會停用連結器。</span><span class="sxs-lookup"><span data-stu-id="e341b-111">The linker is disabled when building in Debug configuration.</span></span> <span data-ttu-id="e341b-112">應用程式必須在發行設定中建立，才能啟用連結器。</span><span class="sxs-lookup"><span data-stu-id="e341b-112">Apps must build in Release configuration to enable the linker.</span></span> <span data-ttu-id="e341b-113">我們建議您在部署 WebAssembly apps 時建立發行 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="e341b-113">We recommend building in Release when deploying your Blazor WebAssembly apps.</span></span> 

<span data-ttu-id="e341b-114">連結應用程式會優化大小，但可能會有不利的影響。</span><span class="sxs-lookup"><span data-stu-id="e341b-114">Linking an app optimizes for size but may have detrimental effects.</span></span> <span data-ttu-id="e341b-115">使用反映或相關動態功能的應用程式可能會在修剪時中斷，因為連結器不知道此動態行為，而且無法判斷在執行時間的反映需要何種類型。</span><span class="sxs-lookup"><span data-stu-id="e341b-115">Apps that use reflection or related dynamic features may break when trimmed because the linker doesn't know about this dynamic behavior and can't determine in general which types are required for reflection at runtime.</span></span> <span data-ttu-id="e341b-116">若要修剪這類應用程式，連結器必須通知程式碼中的反映所需的任何類型，以及應用程式所相依的封裝或架構。</span><span class="sxs-lookup"><span data-stu-id="e341b-116">To trim such apps, the linker must be informed about any types required by reflection in the code and in packages or frameworks that the app depends on.</span></span> 

<span data-ttu-id="e341b-117">若要確保已修剪的應用程式在部署後能正常運作，請務必在開發時經常測試應用程式的發行組建。</span><span class="sxs-lookup"><span data-stu-id="e341b-117">To ensure the trimmed app works correctly once deployed, it's important to test Release builds of the app frequently while developing.</span></span>

<span data-ttu-id="e341b-118">您 Blazor 可以使用下列 MSBuild 功能來設定應用程式的連結：</span><span class="sxs-lookup"><span data-stu-id="e341b-118">Linking for Blazor apps can be configured using these MSBuild features:</span></span>

* <span data-ttu-id="e341b-119">使用[MSBuild 屬性](#control-linking-with-an-msbuild-property)來設定全域連結。</span><span class="sxs-lookup"><span data-stu-id="e341b-119">Configure linking globally with a [MSBuild property](#control-linking-with-an-msbuild-property).</span></span>
* <span data-ttu-id="e341b-120">使用[設定檔](#control-linking-with-a-configuration-file)來控制每個元件的連結。</span><span class="sxs-lookup"><span data-stu-id="e341b-120">Control linking on a per-assembly basis with a [configuration file](#control-linking-with-a-configuration-file).</span></span>

## <a name="control-linking-with-an-msbuild-property"></a><span data-ttu-id="e341b-121">使用 MSBuild 屬性的控制項連結</span><span class="sxs-lookup"><span data-stu-id="e341b-121">Control linking with an MSBuild property</span></span>

<span data-ttu-id="e341b-122">建立應用程式時，會啟用連結 `Release` 。</span><span class="sxs-lookup"><span data-stu-id="e341b-122">Linking is enabled when an app is built in `Release` configuration.</span></span> <span data-ttu-id="e341b-123">若要變更此項，請 `BlazorWebAssemblyEnableLinking` 在專案檔中設定 MSBuild 屬性：</span><span class="sxs-lookup"><span data-stu-id="e341b-123">To change this, configure the `BlazorWebAssemblyEnableLinking` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorWebAssemblyEnableLinking>false</BlazorWebAssemblyEnableLinking>
</PropertyGroup>
```

## <a name="control-linking-with-a-configuration-file"></a><span data-ttu-id="e341b-124">使用組態檔控制連結</span><span class="sxs-lookup"><span data-stu-id="e341b-124">Control linking with a configuration file</span></span>

<span data-ttu-id="e341b-125">透過提供 XML 設定檔，並將此檔案指定為專案檔中的 MSBuild 項目，即可根據組件控制連結：</span><span class="sxs-lookup"><span data-stu-id="e341b-125">Control linking on a per-assembly basis by providing an XML configuration file and specifying the file as a MSBuild item in the project file:</span></span>

```xml
<ItemGroup>
  <BlazorLinkerDescriptor Include="LinkerConfig.xml" />
</ItemGroup>
```

<span data-ttu-id="e341b-126">*LinkerConfig .xml*：</span><span class="sxs-lookup"><span data-stu-id="e341b-126">*LinkerConfig.xml*:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--
  This file specifies which parts of the BCL or Blazor packages must not be
  stripped by the IL Linker even if they aren't referenced by user code.
-->
<linker>
  <assembly fullname="mscorlib">
    <!--
      Preserve the methods in WasmRuntime because its methods are called by 
      JavaScript client-side code to implement timers.
      Fixes: https://github.com/dotnet/blazor/issues/239
    -->
    <type fullname="System.Threading.WasmRuntime" />
  </assembly>
  <assembly fullname="System.Core">
    <!--
      System.Linq.Expressions* is required by Json.NET and any 
      expression.Compile caller. The assembly isn't stripped.
    -->
    <type fullname="System.Linq.Expressions*" />
  </assembly>
  <!--
    In this example, the app's entry point assembly is listed. The assembly
    isn't stripped by the IL Linker.
  -->
  <assembly fullname="MyCoolBlazorApp" />
</linker>
```

<span data-ttu-id="e341b-127">如需詳細資訊和範例，請參閱[資料格式（mono/連結器 GitHub 存放庫）](https://github.com/mono/linker/blob/master/docs/data-formats.md)。</span><span class="sxs-lookup"><span data-stu-id="e341b-127">For more information and examples, see [Data Formats (mono/linker GitHub repository)](https://github.com/mono/linker/blob/master/docs/data-formats.md).</span></span>

## <a name="add-an-xml-linker-configuration-file-to-a-library"></a><span data-ttu-id="e341b-128">將 XML 連結器設定檔加入至程式庫</span><span class="sxs-lookup"><span data-stu-id="e341b-128">Add an XML linker configuration file to a library</span></span>

<span data-ttu-id="e341b-129">若要設定特定程式庫的連結器，請將 XML 連結器設定檔新增至程式庫做為內嵌資源。</span><span class="sxs-lookup"><span data-stu-id="e341b-129">To configure the linker for a specific library, add an XML linker configuration file into the library as an embedded resource.</span></span> <span data-ttu-id="e341b-130">內嵌資源的名稱必須與元件相同。</span><span class="sxs-lookup"><span data-stu-id="e341b-130">The embedded resource must have the same name as the assembly.</span></span>

<span data-ttu-id="e341b-131">在下列範例中，會將*LinkerConfig*指定為與程式庫的元件同名的內嵌資源：</span><span class="sxs-lookup"><span data-stu-id="e341b-131">In the following example, the *LinkerConfig.xml* file is specified as an embedded resource that has the same name as the library's assembly:</span></span>

```xml
<ItemGroup>
  <EmbeddedResource Include="LinkerConfig.xml">
    <LogicalName>$(MSBuildProjectName).xml</LogicalName>
  </EmbeddedResource>
</ItemGroup>
```

### <a name="configure-the-linker-for-internationalization"></a><span data-ttu-id="e341b-132">設定國際化的連結器</span><span class="sxs-lookup"><span data-stu-id="e341b-132">Configure the linker for internationalization</span></span>

<span data-ttu-id="e341b-133">根據預設， Blazor Blazor WebAssembly 應用程式的連結器設定會去除國際化資訊，但不包括明確要求的地區設定。</span><span class="sxs-lookup"><span data-stu-id="e341b-133">By default, Blazor's linker configuration for Blazor WebAssembly apps strips out internationalization information except for locales explicitly requested.</span></span> <span data-ttu-id="e341b-134">移除這些元件會將應用程式的大小降到最低。</span><span class="sxs-lookup"><span data-stu-id="e341b-134">Removing these assemblies minimizes the app's size.</span></span>

<span data-ttu-id="e341b-135">若要控制要保留哪些國際化元件，請 `<BlazorWebAssemblyI18NAssemblies>` 在專案檔中設定 MSBuild 屬性：</span><span class="sxs-lookup"><span data-stu-id="e341b-135">To control which I18N assemblies are retained, set the `<BlazorWebAssemblyI18NAssemblies>` MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <BlazorWebAssemblyI18NAssemblies>{all|none|REGION1,REGION2,...}</BlazorWebAssemblyI18NAssemblies>
</PropertyGroup>
```

| <span data-ttu-id="e341b-136">區域值</span><span class="sxs-lookup"><span data-stu-id="e341b-136">Region Value</span></span>     | <span data-ttu-id="e341b-137">Mono 區域元件</span><span class="sxs-lookup"><span data-stu-id="e341b-137">Mono region assembly</span></span>    |
| ---
<span data-ttu-id="e341b-138">標題： ' 設定連結器以 ASP.NET Core Blazor ' 作者：描述： ' 瞭解如何在建立應用程式時控制中繼語言（IL）連結器 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="e341b-138">title: 'Configure the Linker for ASP.NET Core Blazor' author: description: 'Learn how to control the Intermediate Language (IL) Linker when building a Blazor app.'</span></span>
<span data-ttu-id="e341b-139">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="e341b-139">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="e341b-140">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="e341b-140">'Blazor'</span></span>
- <span data-ttu-id="e341b-141">'Identity'</span><span class="sxs-lookup"><span data-stu-id="e341b-141">'Identity'</span></span>
- <span data-ttu-id="e341b-142">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="e341b-142">'Let's Encrypt'</span></span>
- <span data-ttu-id="e341b-143">'Razor'</span><span class="sxs-lookup"><span data-stu-id="e341b-143">'Razor'</span></span>
- <span data-ttu-id="e341b-144">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="e341b-144">'SignalR' uid:</span></span> 

-
<span data-ttu-id="e341b-145">標題： ' 設定連結器以 ASP.NET Core Blazor ' 作者：描述： ' 瞭解如何在建立應用程式時控制中繼語言（IL）連結器 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="e341b-145">title: 'Configure the Linker for ASP.NET Core Blazor' author: description: 'Learn how to control the Intermediate Language (IL) Linker when building a Blazor app.'</span></span>
<span data-ttu-id="e341b-146">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="e341b-146">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="e341b-147">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="e341b-147">'Blazor'</span></span>
- <span data-ttu-id="e341b-148">'Identity'</span><span class="sxs-lookup"><span data-stu-id="e341b-148">'Identity'</span></span>
- <span data-ttu-id="e341b-149">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="e341b-149">'Let's Encrypt'</span></span>
- <span data-ttu-id="e341b-150">'Razor'</span><span class="sxs-lookup"><span data-stu-id="e341b-150">'Razor'</span></span>
- <span data-ttu-id="e341b-151">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="e341b-151">'SignalR' uid:</span></span> 

-
<span data-ttu-id="e341b-152">標題： ' 設定連結器以 ASP.NET Core Blazor ' 作者：描述： ' 瞭解如何在建立應用程式時控制中繼語言（IL）連結器 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="e341b-152">title: 'Configure the Linker for ASP.NET Core Blazor' author: description: 'Learn how to control the Intermediate Language (IL) Linker when building a Blazor app.'</span></span>
<span data-ttu-id="e341b-153">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="e341b-153">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="e341b-154">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="e341b-154">'Blazor'</span></span>
- <span data-ttu-id="e341b-155">'Identity'</span><span class="sxs-lookup"><span data-stu-id="e341b-155">'Identity'</span></span>
- <span data-ttu-id="e341b-156">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="e341b-156">'Let's Encrypt'</span></span>
- <span data-ttu-id="e341b-157">'Razor'</span><span class="sxs-lookup"><span data-stu-id="e341b-157">'Razor'</span></span>
- <span data-ttu-id="e341b-158">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="e341b-158">'SignalR' uid:</span></span> 

-
<span data-ttu-id="e341b-159">標題： ' 設定連結器以 ASP.NET Core Blazor ' 作者：描述： ' 瞭解如何在建立應用程式時控制中繼語言（IL）連結器 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="e341b-159">title: 'Configure the Linker for ASP.NET Core Blazor' author: description: 'Learn how to control the Intermediate Language (IL) Linker when building a Blazor app.'</span></span>
<span data-ttu-id="e341b-160">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="e341b-160">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="e341b-161">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="e341b-161">'Blazor'</span></span>
- <span data-ttu-id="e341b-162">'Identity'</span><span class="sxs-lookup"><span data-stu-id="e341b-162">'Identity'</span></span>
- <span data-ttu-id="e341b-163">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="e341b-163">'Let's Encrypt'</span></span>
- <span data-ttu-id="e341b-164">'Razor'</span><span class="sxs-lookup"><span data-stu-id="e341b-164">'Razor'</span></span>
- <span data-ttu-id="e341b-165">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="e341b-165">'SignalR' uid:</span></span> 

-
<span data-ttu-id="e341b-166">標題： ' 設定連結器以 ASP.NET Core Blazor ' 作者：描述： ' 瞭解如何在建立應用程式時控制中繼語言（IL）連結器 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="e341b-166">title: 'Configure the Linker for ASP.NET Core Blazor' author: description: 'Learn how to control the Intermediate Language (IL) Linker when building a Blazor app.'</span></span>
<span data-ttu-id="e341b-167">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="e341b-167">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="e341b-168">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="e341b-168">'Blazor'</span></span>
- <span data-ttu-id="e341b-169">'Identity'</span><span class="sxs-lookup"><span data-stu-id="e341b-169">'Identity'</span></span>
- <span data-ttu-id="e341b-170">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="e341b-170">'Let's Encrypt'</span></span>
- <span data-ttu-id="e341b-171">'Razor'</span><span class="sxs-lookup"><span data-stu-id="e341b-171">'Razor'</span></span>
- <span data-ttu-id="e341b-172">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="e341b-172">'SignalR' uid:</span></span> 

-
<span data-ttu-id="e341b-173">標題： ' 設定連結器以 ASP.NET Core Blazor ' 作者：描述： ' 瞭解如何在建立應用程式時控制中繼語言（IL）連結器 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="e341b-173">title: 'Configure the Linker for ASP.NET Core Blazor' author: description: 'Learn how to control the Intermediate Language (IL) Linker when building a Blazor app.'</span></span>
<span data-ttu-id="e341b-174">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="e341b-174">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="e341b-175">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="e341b-175">'Blazor'</span></span>
- <span data-ttu-id="e341b-176">'Identity'</span><span class="sxs-lookup"><span data-stu-id="e341b-176">'Identity'</span></span>
- <span data-ttu-id="e341b-177">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="e341b-177">'Let's Encrypt'</span></span>
- <span data-ttu-id="e341b-178">'Razor'</span><span class="sxs-lookup"><span data-stu-id="e341b-178">'Razor'</span></span>
- <span data-ttu-id="e341b-179">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="e341b-179">'SignalR' uid:</span></span> 

<span data-ttu-id="e341b-180">-------- |---標題： ' 設定 ASP.NET Core ' 作者的連結器 Blazor ：描述： ' 瞭解如何在建立應用程式時控制中繼語言（IL）連結器 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="e341b-180">-------- | --- title: 'Configure the Linker for ASP.NET Core Blazor' author: description: 'Learn how to control the Intermediate Language (IL) Linker when building a Blazor app.'</span></span>
<span data-ttu-id="e341b-181">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="e341b-181">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="e341b-182">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="e341b-182">'Blazor'</span></span>
- <span data-ttu-id="e341b-183">'Identity'</span><span class="sxs-lookup"><span data-stu-id="e341b-183">'Identity'</span></span>
- <span data-ttu-id="e341b-184">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="e341b-184">'Let's Encrypt'</span></span>
- <span data-ttu-id="e341b-185">'Razor'</span><span class="sxs-lookup"><span data-stu-id="e341b-185">'Razor'</span></span>
- <span data-ttu-id="e341b-186">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="e341b-186">'SignalR' uid:</span></span> 

-
<span data-ttu-id="e341b-187">標題： ' 設定連結器以 ASP.NET Core Blazor ' 作者：描述： ' 瞭解如何在建立應用程式時控制中繼語言（IL）連結器 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="e341b-187">title: 'Configure the Linker for ASP.NET Core Blazor' author: description: 'Learn how to control the Intermediate Language (IL) Linker when building a Blazor app.'</span></span>
<span data-ttu-id="e341b-188">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="e341b-188">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="e341b-189">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="e341b-189">'Blazor'</span></span>
- <span data-ttu-id="e341b-190">'Identity'</span><span class="sxs-lookup"><span data-stu-id="e341b-190">'Identity'</span></span>
- <span data-ttu-id="e341b-191">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="e341b-191">'Let's Encrypt'</span></span>
- <span data-ttu-id="e341b-192">'Razor'</span><span class="sxs-lookup"><span data-stu-id="e341b-192">'Razor'</span></span>
- <span data-ttu-id="e341b-193">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="e341b-193">'SignalR' uid:</span></span> 

-
<span data-ttu-id="e341b-194">標題： ' 設定連結器以 ASP.NET Core Blazor ' 作者：描述： ' 瞭解如何在建立應用程式時控制中繼語言（IL）連結器 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="e341b-194">title: 'Configure the Linker for ASP.NET Core Blazor' author: description: 'Learn how to control the Intermediate Language (IL) Linker when building a Blazor app.'</span></span>
<span data-ttu-id="e341b-195">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="e341b-195">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="e341b-196">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="e341b-196">'Blazor'</span></span>
- <span data-ttu-id="e341b-197">'Identity'</span><span class="sxs-lookup"><span data-stu-id="e341b-197">'Identity'</span></span>
- <span data-ttu-id="e341b-198">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="e341b-198">'Let's Encrypt'</span></span>
- <span data-ttu-id="e341b-199">'Razor'</span><span class="sxs-lookup"><span data-stu-id="e341b-199">'Razor'</span></span>
- <span data-ttu-id="e341b-200">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="e341b-200">'SignalR' uid:</span></span> 

-
<span data-ttu-id="e341b-201">標題： ' 設定連結器以 ASP.NET Core Blazor ' 作者：描述： ' 瞭解如何在建立應用程式時控制中繼語言（IL）連結器 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="e341b-201">title: 'Configure the Linker for ASP.NET Core Blazor' author: description: 'Learn how to control the Intermediate Language (IL) Linker when building a Blazor app.'</span></span>
<span data-ttu-id="e341b-202">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="e341b-202">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="e341b-203">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="e341b-203">'Blazor'</span></span>
- <span data-ttu-id="e341b-204">'Identity'</span><span class="sxs-lookup"><span data-stu-id="e341b-204">'Identity'</span></span>
- <span data-ttu-id="e341b-205">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="e341b-205">'Let's Encrypt'</span></span>
- <span data-ttu-id="e341b-206">'Razor'</span><span class="sxs-lookup"><span data-stu-id="e341b-206">'Razor'</span></span>
- <span data-ttu-id="e341b-207">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="e341b-207">'SignalR' uid:</span></span> 

-
<span data-ttu-id="e341b-208">標題： ' 設定連結器以 ASP.NET Core Blazor ' 作者：描述： ' 瞭解如何在建立應用程式時控制中繼語言（IL）連結器 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="e341b-208">title: 'Configure the Linker for ASP.NET Core Blazor' author: description: 'Learn how to control the Intermediate Language (IL) Linker when building a Blazor app.'</span></span>
<span data-ttu-id="e341b-209">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="e341b-209">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="e341b-210">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="e341b-210">'Blazor'</span></span>
- <span data-ttu-id="e341b-211">'Identity'</span><span class="sxs-lookup"><span data-stu-id="e341b-211">'Identity'</span></span>
- <span data-ttu-id="e341b-212">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="e341b-212">'Let's Encrypt'</span></span>
- <span data-ttu-id="e341b-213">'Razor'</span><span class="sxs-lookup"><span data-stu-id="e341b-213">'Razor'</span></span>
- <span data-ttu-id="e341b-214">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="e341b-214">'SignalR' uid:</span></span> 

-
<span data-ttu-id="e341b-215">標題： ' 設定連結器以 ASP.NET Core Blazor ' 作者：描述： ' 瞭解如何在建立應用程式時控制中繼語言（IL）連結器 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="e341b-215">title: 'Configure the Linker for ASP.NET Core Blazor' author: description: 'Learn how to control the Intermediate Language (IL) Linker when building a Blazor app.'</span></span>
<span data-ttu-id="e341b-216">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="e341b-216">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="e341b-217">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="e341b-217">'Blazor'</span></span>
- <span data-ttu-id="e341b-218">'Identity'</span><span class="sxs-lookup"><span data-stu-id="e341b-218">'Identity'</span></span>
- <span data-ttu-id="e341b-219">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="e341b-219">'Let's Encrypt'</span></span>
- <span data-ttu-id="e341b-220">'Razor'</span><span class="sxs-lookup"><span data-stu-id="e341b-220">'Razor'</span></span>
- <span data-ttu-id="e341b-221">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="e341b-221">'SignalR' uid:</span></span> 

-
<span data-ttu-id="e341b-222">標題： ' 設定連結器以 ASP.NET Core Blazor ' 作者：描述： ' 瞭解如何在建立應用程式時控制中繼語言（IL）連結器 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="e341b-222">title: 'Configure the Linker for ASP.NET Core Blazor' author: description: 'Learn how to control the Intermediate Language (IL) Linker when building a Blazor app.'</span></span>
<span data-ttu-id="e341b-223">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="e341b-223">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="e341b-224">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="e341b-224">'Blazor'</span></span>
- <span data-ttu-id="e341b-225">'Identity'</span><span class="sxs-lookup"><span data-stu-id="e341b-225">'Identity'</span></span>
- <span data-ttu-id="e341b-226">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="e341b-226">'Let's Encrypt'</span></span>
- <span data-ttu-id="e341b-227">'Razor'</span><span class="sxs-lookup"><span data-stu-id="e341b-227">'Razor'</span></span>
- <span data-ttu-id="e341b-228">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="e341b-228">'SignalR' uid:</span></span> 

-
<span data-ttu-id="e341b-229">標題： ' 設定連結器以 ASP.NET Core Blazor ' 作者：描述： ' 瞭解如何在建立應用程式時控制中繼語言（IL）連結器 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="e341b-229">title: 'Configure the Linker for ASP.NET Core Blazor' author: description: 'Learn how to control the Intermediate Language (IL) Linker when building a Blazor app.'</span></span>
<span data-ttu-id="e341b-230">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="e341b-230">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="e341b-231">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="e341b-231">'Blazor'</span></span>
- <span data-ttu-id="e341b-232">'Identity'</span><span class="sxs-lookup"><span data-stu-id="e341b-232">'Identity'</span></span>
- <span data-ttu-id="e341b-233">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="e341b-233">'Let's Encrypt'</span></span>
- <span data-ttu-id="e341b-234">'Razor'</span><span class="sxs-lookup"><span data-stu-id="e341b-234">'Razor'</span></span>
- <span data-ttu-id="e341b-235">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="e341b-235">'SignalR' uid:</span></span> 

-
<span data-ttu-id="e341b-236">標題： ' 設定連結器以 ASP.NET Core Blazor ' 作者：描述： ' 瞭解如何在建立應用程式時控制中繼語言（IL）連結器 Blazor 。 '</span><span class="sxs-lookup"><span data-stu-id="e341b-236">title: 'Configure the Linker for ASP.NET Core Blazor' author: description: 'Learn how to control the Intermediate Language (IL) Linker when building a Blazor app.'</span></span>
<span data-ttu-id="e341b-237">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="e341b-237">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="e341b-238">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="e341b-238">'Blazor'</span></span>
- <span data-ttu-id="e341b-239">'Identity'</span><span class="sxs-lookup"><span data-stu-id="e341b-239">'Identity'</span></span>
- <span data-ttu-id="e341b-240">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="e341b-240">'Let's Encrypt'</span></span>
- <span data-ttu-id="e341b-241">'Razor'</span><span class="sxs-lookup"><span data-stu-id="e341b-241">'Razor'</span></span>
- <span data-ttu-id="e341b-242">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="e341b-242">'SignalR' uid:</span></span> 

<span data-ttu-id="e341b-243">------------ | |`all`            |包含的所有元件 | |`cjk`             |  *I18N。CJK .dll* | |`mideast`         |  *I18N。務必 .dll* | |`none`（預設） |無 | |`other`           |  *I18N。其他 .dll* | |`rare`            |  *I18N。罕見 .dll* | |`west`            |  *I18N。West .dll*         |</span><span class="sxs-lookup"><span data-stu-id="e341b-243">------------ | | `all`            | All assemblies included | | `cjk`            | *I18N.CJK.dll*          | | `mideast`        | *I18N.MidEast.dll*      | | `none` (default) | None                    | | `other`          | *I18N.Other.dll*        | | `rare`           | *I18N.Rare.dll*         | | `west`           | *I18N.West.dll*         |</span></span>

<span data-ttu-id="e341b-244">使用逗號來分隔多個值（例如 `mideast,west` ）。</span><span class="sxs-lookup"><span data-stu-id="e341b-244">Use a comma to separate multiple values (for example, `mideast,west`).</span></span>

<span data-ttu-id="e341b-245">如需詳細資訊，請參閱[I18N： Pnetlib 國際化架構程式庫（mono/Mono GitHub 儲存機制）](https://github.com/mono/mono/tree/master/mcs/class/I18N)。</span><span class="sxs-lookup"><span data-stu-id="e341b-245">For more information, see [I18N: Pnetlib Internationalization Framework Library (mono/mono GitHub repository)](https://github.com/mono/mono/tree/master/mcs/class/I18N).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e341b-246">其他資源</span><span class="sxs-lookup"><span data-stu-id="e341b-246">Additional resources</span></span>

* <xref:performance/blazor/webassembly-best-practices#intermediate-language-il-linking>
