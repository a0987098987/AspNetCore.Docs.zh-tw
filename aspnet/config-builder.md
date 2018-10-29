---
uid: config-builder
title: 適用於 ASP.NET 的組態產生器
author: rick-anderson
description: 了解如何從外部來源中的 web.config 值以外的來源取得設定資料。
ms.author: riande
ms.date: 10/29/2018
ms.technology: aspnet
msc.type: content
ms.openlocfilehash: 4dcc62573fad13ec8b37b2c59e884eec7ca80b92
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148833"
---
# <a name="configuration-builders-for-aspnet"></a><span data-ttu-id="8682a-103">適用於 ASP.NET 的組態產生器</span><span class="sxs-lookup"><span data-stu-id="8682a-103">Configuration builders for ASP.NET</span></span>

<span data-ttu-id="8682a-104">藉由[Stephen Molloy](https://github.com/StephenMolloy)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8682a-104">By [Stephen Molloy](https://github.com/StephenMolloy) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8682a-105">組態產生器提供現代化和敏捷式軟體開發的機制，以從外部來源取得設定值的 ASP.NET 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8682a-105">Configuration builders provide a modern and agile mechanism for ASP.NET apps to get configuration values from external sources.</span></span>

<span data-ttu-id="8682a-106">組態產生器：</span><span class="sxs-lookup"><span data-stu-id="8682a-106">Configuration builders:</span></span>

* <span data-ttu-id="8682a-107">會提供於.NET Framework 4.7.1 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="8682a-107">Are available in .NET Framework 4.7.1 and later.</span></span>
* <span data-ttu-id="8682a-108">提供彈性的機制，用於讀取組態值。</span><span class="sxs-lookup"><span data-stu-id="8682a-108">Provide a flexible mechanism for reading configuration values.</span></span>
* <span data-ttu-id="8682a-109">解決一些應用程式的基本需求它們將移至容器及雲端具有焦點的環境。</span><span class="sxs-lookup"><span data-stu-id="8682a-109">Address some of the basic needs of apps as they move into a container and cloud focused environment.</span></span>
* <span data-ttu-id="8682a-110">可以用來提升保護組態資料所繪製，從先前無法使用的來源 （例如，Azure 金鑰保存庫和環境變數） 的.NET 組態系統中。</span><span class="sxs-lookup"><span data-stu-id="8682a-110">Can be used to improve protection of configuration data by drawing from sources previously unavailable (for example, Azure Key Vault and environment variables) in the .NET configuration system.</span></span>

## <a name="keyvalue-configuration-builders"></a><span data-ttu-id="8682a-111">索引鍵/值設定產生器</span><span class="sxs-lookup"><span data-stu-id="8682a-111">Key/value configuration builders</span></span>

<span data-ttu-id="8682a-112">組態產生器可以處理的常見案例是提供基本的索引鍵/值的取代機制，遵循下列索引鍵/值模式的組態區段。</span><span class="sxs-lookup"><span data-stu-id="8682a-112">A common scenario that can be handled by configuration builders is to provide a basic key/value replacement mechanism for configuration sections that follow a key/value pattern.</span></span> <span data-ttu-id="8682a-113">.NET Framework 的概念 ConfigurationBuilders 不限於特定的組態區段或模式。</span><span class="sxs-lookup"><span data-stu-id="8682a-113">The .NET Framework concept of ConfigurationBuilders is not limited to specific configuration sections or patterns.</span></span> <span data-ttu-id="8682a-114">不過，許多中的組態產生器`Microsoft.Configuration.ConfigurationBuilders`([github](https://github.com/aspnet/MicrosoftConfigurationBuilders)， [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) 工作內的索引鍵/值模式。</span><span class="sxs-lookup"><span data-stu-id="8682a-114">However, many of the configuration builders in `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) work within the key/value pattern.</span></span>

## <a name="keyvalue-configuration-builders-settings"></a><span data-ttu-id="8682a-115">索引鍵/值設定產生器</span><span class="sxs-lookup"><span data-stu-id="8682a-115">Key/value configuration builders settings</span></span>

<span data-ttu-id="8682a-116">下列設定會套用到所有的索引鍵/值設定產生器中`Microsoft.Configuration.ConfigurationBuilders`。</span><span class="sxs-lookup"><span data-stu-id="8682a-116">The following settings apply to all key/value configuration builders in `Microsoft.Configuration.ConfigurationBuilders`.</span></span>

### <a name="mode"></a><span data-ttu-id="8682a-117">模式</span><span class="sxs-lookup"><span data-stu-id="8682a-117">Mode</span></span>

<span data-ttu-id="8682a-118">組態產生器會使用外部索引鍵/值資訊的來源來填入選取的索引鍵/值項目，組態系統。</span><span class="sxs-lookup"><span data-stu-id="8682a-118">The configuration builders use an external source of key/value information to populate selected key/value elements of the configuration system.</span></span> <span data-ttu-id="8682a-119">具體而言，`<appSettings/>`和`<connectionStrings/>`各節會接收組態產生器中的特殊處理。</span><span class="sxs-lookup"><span data-stu-id="8682a-119">Specifically, the `<appSettings/>` and `<connectionStrings/>` sections receive special treatment from the configuration builders.</span></span> <span data-ttu-id="8682a-120">在三個模式下運作的產生器：</span><span class="sxs-lookup"><span data-stu-id="8682a-120">The builders work in three modes:</span></span>

* <span data-ttu-id="8682a-121">`Strict` -預設的模式。</span><span class="sxs-lookup"><span data-stu-id="8682a-121">`Strict` - The default mode.</span></span> <span data-ttu-id="8682a-122">在此模式中，設定產生器只能在運作上的已知索引鍵/值為主的組態區段。</span><span class="sxs-lookup"><span data-stu-id="8682a-122">In this mode, the configuration builder only operates on well-known key/value-centric configuration sections.</span></span> <span data-ttu-id="8682a-123">`Strict` 模式會列舉一節中的每個索引鍵。</span><span class="sxs-lookup"><span data-stu-id="8682a-123">`Strict` mode enumerates each key in the section.</span></span> <span data-ttu-id="8682a-124">如果外部來源中找到相符的索引鍵：</span><span class="sxs-lookup"><span data-stu-id="8682a-124">If a matching key is found in the external source:</span></span>

   * <span data-ttu-id="8682a-125">組態產生器會將產生的組態區段中的值取代從外部來源的值。</span><span class="sxs-lookup"><span data-stu-id="8682a-125">The configuration builders replace the value in the resulting configuration section with the value from the external source.</span></span>
* <span data-ttu-id="8682a-126">`Greedy` -這種模式與密切相關`Strict`模式。</span><span class="sxs-lookup"><span data-stu-id="8682a-126">`Greedy` - This mode is closely related to `Strict` mode.</span></span> <span data-ttu-id="8682a-127">而不是則限制為原始設定中存在的索引鍵：</span><span class="sxs-lookup"><span data-stu-id="8682a-127">Rather than being limited to keys that already exist in the original configuration:</span></span>

  * <span data-ttu-id="8682a-128">組態產生器會將所有的索引鍵/值組從外部來源新增到產生的組態區段。</span><span class="sxs-lookup"><span data-stu-id="8682a-128">The configuration builders adds all key/value pairs from the external source into the resulting configuration section.</span></span>

* <span data-ttu-id="8682a-129">`Expand` -上運作的原始 XML 之前將會剖析為組態區段物件。</span><span class="sxs-lookup"><span data-stu-id="8682a-129">`Expand` - Operates on the raw XML before it's parsed into a configuration section object.</span></span> <span data-ttu-id="8682a-130">它可以視為語彙基元字串中的擴充功能。</span><span class="sxs-lookup"><span data-stu-id="8682a-130">It can be thought of as an expansion of tokens in a string.</span></span> <span data-ttu-id="8682a-131">未經處理的 XML 字串符合模式的任何部分`${token}`是語彙基元展開的候選項目。</span><span class="sxs-lookup"><span data-stu-id="8682a-131">Any part of the raw XML string that matches the pattern `${token}` is a candidate for token expansion.</span></span> <span data-ttu-id="8682a-132">如果外部來源中發現沒有對應的值，則權杖會不會變更。</span><span class="sxs-lookup"><span data-stu-id="8682a-132">If no corresponding value is found in the external source, then the token is not changed.</span></span> <span data-ttu-id="8682a-133">在此模式中的產生器並不限於`<appSettings/>`和`<connectionStrings/>`區段。</span><span class="sxs-lookup"><span data-stu-id="8682a-133">Builders in this mode are not limited to the `<appSettings/>` and `<connectionStrings/>` sections.</span></span>

<span data-ttu-id="8682a-134">從下列標記*web.config*可讓[EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/)在`Strict`模式︰</span><span class="sxs-lookup"><span data-stu-id="8682a-134">The following markup from *web.config* enables the [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) in `Strict` mode:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

<span data-ttu-id="8682a-135">下列程式碼的讀取`<appSettings/>`並`<connectionStrings/>`先前所示*web.config*檔案：</span><span class="sxs-lookup"><span data-stu-id="8682a-135">The following code reads the `<appSettings/>` and `<connectionStrings/>` shown in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

<span data-ttu-id="8682a-136">上述程式碼會將屬性值：</span><span class="sxs-lookup"><span data-stu-id="8682a-136">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="8682a-137">中的值*web.config*檔案如果金鑰未設定環境變數中。</span><span class="sxs-lookup"><span data-stu-id="8682a-137">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="8682a-138">值的環境變數，如果設定。</span><span class="sxs-lookup"><span data-stu-id="8682a-138">The values of the environment variable, if set.</span></span>

<span data-ttu-id="8682a-139">比方說，`ServiceID`將包含：</span><span class="sxs-lookup"><span data-stu-id="8682a-139">For example, `ServiceID` will contain:</span></span>

* <span data-ttu-id="8682a-140">「 從 web.config ServiceID 值，「 如果環境變數`ServiceID`未設定。</span><span class="sxs-lookup"><span data-stu-id="8682a-140">"ServiceID value from web.config", if the environment variable `ServiceID` is not set.</span></span>
* <span data-ttu-id="8682a-141">值`ServiceID`環境變數，如果設定。</span><span class="sxs-lookup"><span data-stu-id="8682a-141">The value of the `ServiceID` environment variable, if set.</span></span>

<span data-ttu-id="8682a-142">下圖顯示`<appSettings/>`索引鍵/值來自上述*web.config*環境編輯器中的檔案集：</span><span class="sxs-lookup"><span data-stu-id="8682a-142">The following image shows the `<appSettings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![環境編輯器](config-builder/static/env.png)

<span data-ttu-id="8682a-144">注意： 您可能需要結束並重新啟動 Visual Studio 以查看變更環境變數中。</span><span class="sxs-lookup"><span data-stu-id="8682a-144">Note: You might need to exit and restart Visual Studio to see changes in environment variables.</span></span>

### <a name="prefix-handling"></a><span data-ttu-id="8682a-145">前置處理</span><span class="sxs-lookup"><span data-stu-id="8682a-145">Prefix handling</span></span>

<span data-ttu-id="8682a-146">索引鍵的前置詞可以簡化設定索引鍵，因為：</span><span class="sxs-lookup"><span data-stu-id="8682a-146">Key prefixes can simplify setting keys because:</span></span>

* <span data-ttu-id="8682a-147">.NET Framework 組態是複雜和巢狀。</span><span class="sxs-lookup"><span data-stu-id="8682a-147">The .NET Framework configuration is complex and nested.</span></span>
* <span data-ttu-id="8682a-148">基本和一般本質上，通常是外部索引鍵/值來源。</span><span class="sxs-lookup"><span data-stu-id="8682a-148">External key/value sources are commonly basic and flat by nature.</span></span> <span data-ttu-id="8682a-149">例如，未巢狀環境變數。</span><span class="sxs-lookup"><span data-stu-id="8682a-149">For example, environment variables are not nested.</span></span>

<span data-ttu-id="8682a-150">使用任何下列其中一個方法來插入兩`<appSettings/>`和`<connectionStrings/>`透過環境變數組態：</span><span class="sxs-lookup"><span data-stu-id="8682a-150">Use any of the following approaches to inject both `<appSettings/>` and `<connectionStrings/>` into the configuration via environment variables:</span></span>

* <span data-ttu-id="8682a-151">具有`EnvironmentConfigBuilder`在預設`Strict`模式和適當的索引鍵名稱在組態檔中。</span><span class="sxs-lookup"><span data-stu-id="8682a-151">With the `EnvironmentConfigBuilder` in the default `Strict` mode and the appropriate key names in the configuration file.</span></span> <span data-ttu-id="8682a-152">上述程式碼和標記會使用此方法。</span><span class="sxs-lookup"><span data-stu-id="8682a-152">The preceding code and markup takes this approach.</span></span> <span data-ttu-id="8682a-153">使用這種方法，您可以**未**有相同的已命名兩者中的索引鍵`<appSettings/>`和`<connectionStrings/>`。</span><span class="sxs-lookup"><span data-stu-id="8682a-153">Using this approach you can **not** have identically named keys in both `<appSettings/>` and `<connectionStrings/>`.</span></span>
* <span data-ttu-id="8682a-154">使用兩個`EnvironmentConfigBuilder`中的 s`Greedy`具有不同的前置詞模式和`stripPrefix`。</span><span class="sxs-lookup"><span data-stu-id="8682a-154">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes and `stripPrefix`.</span></span> <span data-ttu-id="8682a-155">使用此方法時，應用程式可以讀取`<appSettings/>`和`<connectionStrings/>`而不需要更新組態檔。</span><span class="sxs-lookup"><span data-stu-id="8682a-155">With this approach, the app can read `<appSettings/>` and `<connectionStrings/>` without needing to update the configuration file.</span></span> <span data-ttu-id="8682a-156">下一步 區段中， [stripPrefix](#stripprefix)，示範如何執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="8682a-156">The next section,  [stripPrefix](#stripprefix),  shows how to do this.</span></span>
* <span data-ttu-id="8682a-157">使用兩個`EnvironmentConfigBuilder`中的 s`Greedy`具有不同的前置詞的模式。</span><span class="sxs-lookup"><span data-stu-id="8682a-157">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes.</span></span> <span data-ttu-id="8682a-158">使用這種方法，因為索引鍵的名稱必須不同前置詞，不能有重複的索引鍵名稱。</span><span class="sxs-lookup"><span data-stu-id="8682a-158">With this approach you can't have duplicate key names as key names must differ by prefix.</span></span>  <span data-ttu-id="8682a-159">例如: </span><span class="sxs-lookup"><span data-stu-id="8682a-159">For example:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

<span data-ttu-id="8682a-160">上述標記中，使用相同的一般的索引鍵/值來源可用來填入兩個不同區段的組態。</span><span class="sxs-lookup"><span data-stu-id="8682a-160">With the preceding markup, the same flat key/value source can be used to populate configuration for two different sections.</span></span>

<span data-ttu-id="8682a-161">下圖顯示`<appSettings/>`並`<connectionStrings/>`索引鍵/值來自上述*web.config*環境編輯器中的檔案集：</span><span class="sxs-lookup"><span data-stu-id="8682a-161">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![環境編輯器](config-builder/static/prefix.png)

<span data-ttu-id="8682a-163">下列程式碼的讀取`<appSettings/>`並`<connectionStrings/>`包含在先前的索引鍵/值*web.config*檔案：</span><span class="sxs-lookup"><span data-stu-id="8682a-163">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

<span data-ttu-id="8682a-164">上述程式碼會將屬性值：</span><span class="sxs-lookup"><span data-stu-id="8682a-164">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="8682a-165">中的值*web.config*檔案如果金鑰未設定環境變數中。</span><span class="sxs-lookup"><span data-stu-id="8682a-165">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="8682a-166">值的環境變數，如果設定。</span><span class="sxs-lookup"><span data-stu-id="8682a-166">The values of the environment variable, if set.</span></span>

<span data-ttu-id="8682a-167">例如，使用 上一個*web.config*檔案，會設定索引鍵/值在前一個環境編輯器映像，和先前的程式碼中，下列值：</span><span class="sxs-lookup"><span data-stu-id="8682a-167">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="8682a-168">Key</span><span class="sxs-lookup"><span data-stu-id="8682a-168">Key</span></span>              | <span data-ttu-id="8682a-169">值</span><span class="sxs-lookup"><span data-stu-id="8682a-169">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="8682a-170">AppSetting_ServiceID</span><span class="sxs-lookup"><span data-stu-id="8682a-170">AppSetting_ServiceID</span></span>           | <span data-ttu-id="8682a-171">從環境變數的 AppSetting_ServiceID</span><span class="sxs-lookup"><span data-stu-id="8682a-171">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="8682a-172">AppSetting_default</span><span class="sxs-lookup"><span data-stu-id="8682a-172">AppSetting_default</span></span>            | <span data-ttu-id="8682a-173">Env AppSetting_default 值</span><span class="sxs-lookup"><span data-stu-id="8682a-173">AppSetting_default value from env</span></span> |
|       <span data-ttu-id="8682a-174">ConnStr_default</span><span class="sxs-lookup"><span data-stu-id="8682a-174">ConnStr_default</span></span>         | <span data-ttu-id="8682a-175">從 env ConnStr_default val</span><span class="sxs-lookup"><span data-stu-id="8682a-175">ConnStr_default val from env</span></span>|

### <a name="stripprefix"></a><span data-ttu-id="8682a-176">stripPrefix</span><span class="sxs-lookup"><span data-stu-id="8682a-176">stripPrefix</span></span>

<span data-ttu-id="8682a-177">`stripPrefix`： 布林值，預設為`false`。</span><span class="sxs-lookup"><span data-stu-id="8682a-177">`stripPrefix`: boolean, defaults to `false`.</span></span> 

<span data-ttu-id="8682a-178">上述的 XML 標記分隔的連接字串中的應用程式設定，但要求中的所有索引鍵*web.config*檔案，以使用指定的前置詞。</span><span class="sxs-lookup"><span data-stu-id="8682a-178">The preceding XML markup separates app settings from connection strings but requires all the keys in the *web.config* file to use the specified prefix.</span></span> <span data-ttu-id="8682a-179">例如，前置詞`AppSetting`必須新增至`ServiceID`金鑰 (「 AppSetting_ServiceID")。</span><span class="sxs-lookup"><span data-stu-id="8682a-179">For example, the prefix `AppSetting` must be added to the `ServiceID` key ("AppSetting_ServiceID").</span></span> <span data-ttu-id="8682a-180">具有`stripPrefix`，在不使用前置詞*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="8682a-180">With `stripPrefix`, the prefix is not used in the *web.config* file.</span></span> <span data-ttu-id="8682a-181">前置詞都需要組態產生器來源 （例如，在環境中。）我們預期大部分的開發人員會使用`stripPrefix`。</span><span class="sxs-lookup"><span data-stu-id="8682a-181">The prefix is required in the configuration builder source (for example, in the environment.) We anticipate most developers will use `stripPrefix`.</span></span>

<span data-ttu-id="8682a-182">應用程式通常移除前置詞。</span><span class="sxs-lookup"><span data-stu-id="8682a-182">Applications typically strip off the prefix.</span></span> <span data-ttu-id="8682a-183">下列*web.config*去除前置詞：</span><span class="sxs-lookup"><span data-stu-id="8682a-183">The following *web.config* strips the prefix:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

<span data-ttu-id="8682a-184">在上述*web.config*檔案，`default`索引鍵存在於同時`<appSettings/>`和`<connectionStrings/>`。</span><span class="sxs-lookup"><span data-stu-id="8682a-184">In the preceding *web.config* file, the `default` key is in both the `<appSettings/>` and `<connectionStrings/>`.</span></span>

<span data-ttu-id="8682a-185">下圖顯示`<appSettings/>`並`<connectionStrings/>`索引鍵/值來自上述*web.config*環境編輯器中的檔案集：</span><span class="sxs-lookup"><span data-stu-id="8682a-185">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![環境編輯器](config-builder/static/prefix.png)

<span data-ttu-id="8682a-187">下列程式碼的讀取`<appSettings/>`並`<connectionStrings/>`包含在先前的索引鍵/值*web.config*檔案：</span><span class="sxs-lookup"><span data-stu-id="8682a-187">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

<span data-ttu-id="8682a-188">上述程式碼會將屬性值：</span><span class="sxs-lookup"><span data-stu-id="8682a-188">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="8682a-189">中的值*web.config*檔案如果金鑰未設定環境變數中。</span><span class="sxs-lookup"><span data-stu-id="8682a-189">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="8682a-190">值的環境變數，如果設定。</span><span class="sxs-lookup"><span data-stu-id="8682a-190">The values of the environment variable, if set.</span></span>

<span data-ttu-id="8682a-191">例如，使用 上一個*web.config*檔案，會設定索引鍵/值在前一個環境編輯器映像，和先前的程式碼中，下列值：</span><span class="sxs-lookup"><span data-stu-id="8682a-191">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="8682a-192">Key</span><span class="sxs-lookup"><span data-stu-id="8682a-192">Key</span></span>              | <span data-ttu-id="8682a-193">值</span><span class="sxs-lookup"><span data-stu-id="8682a-193">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="8682a-194">ServiceID</span><span class="sxs-lookup"><span data-stu-id="8682a-194">ServiceID</span></span>           | <span data-ttu-id="8682a-195">從環境變數的 AppSetting_ServiceID</span><span class="sxs-lookup"><span data-stu-id="8682a-195">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="8682a-196">default</span><span class="sxs-lookup"><span data-stu-id="8682a-196">default</span></span>            | <span data-ttu-id="8682a-197">Env AppSetting_default 值</span><span class="sxs-lookup"><span data-stu-id="8682a-197">AppSetting_default value from env</span></span> |
|    <span data-ttu-id="8682a-198">default</span><span class="sxs-lookup"><span data-stu-id="8682a-198">default</span></span>         | <span data-ttu-id="8682a-199">從 env ConnStr_default val</span><span class="sxs-lookup"><span data-stu-id="8682a-199">ConnStr_default val from env</span></span>|

### <a name="tokenpattern"></a><span data-ttu-id="8682a-200">tokenPattern</span><span class="sxs-lookup"><span data-stu-id="8682a-200">tokenPattern</span></span>

<span data-ttu-id="8682a-201">`tokenPattern`： 字串，預設值為 `@"\$\{(\w+)\}"`</span><span class="sxs-lookup"><span data-stu-id="8682a-201">`tokenPattern`: String, defaults to `@"\$\{(\w+)\}"`</span></span>

<span data-ttu-id="8682a-202">`Expand`行為的產生器搜尋的原始 XML 的權杖看起來像`${token}`。</span><span class="sxs-lookup"><span data-stu-id="8682a-202">The `Expand` behavior of the builders searches the raw XML for tokens that look like `${token}`.</span></span> <span data-ttu-id="8682a-203">搜尋是使用預設的規則運算式`@"\$\{(\w+)\}"`。</span><span class="sxs-lookup"><span data-stu-id="8682a-203">Searching is done with the default regular expression `@"\$\{(\w+)\}"`.</span></span> <span data-ttu-id="8682a-204">符合的字元組`\w`非 XML 與許多組態來源允許更為嚴格。</span><span class="sxs-lookup"><span data-stu-id="8682a-204">The set of characters that matches `\w` is more strict than XML and many configuration sources allow.</span></span> <span data-ttu-id="8682a-205">使用`tokenPattern`當多個字元比`@"\$\{(\w+)\}"`所需的語彙基元的名稱。</span><span class="sxs-lookup"><span data-stu-id="8682a-205">Use `tokenPattern` when more characters than `@"\$\{(\w+)\}"` are required in the token name.</span></span>

<span data-ttu-id="8682a-206">`tokenPattern`： 字串：</span><span class="sxs-lookup"><span data-stu-id="8682a-206">`tokenPattern`: String:</span></span>

* <span data-ttu-id="8682a-207">可讓開發人員若要變更用於語彙基元的比對的 regex。</span><span class="sxs-lookup"><span data-stu-id="8682a-207">Allows developers to change the regex that is used for token matching.</span></span>
* <span data-ttu-id="8682a-208">若要確定它是語式正確，非危險的 regex 會不進行任何驗證。</span><span class="sxs-lookup"><span data-stu-id="8682a-208">No validation is done to make sure it is a well-formed, non-dangerous regex.</span></span>
* <span data-ttu-id="8682a-209">它必須包含擷取群組。</span><span class="sxs-lookup"><span data-stu-id="8682a-209">It must contain a capture group.</span></span> <span data-ttu-id="8682a-210">整個 regex 必須符合將整個語彙基元。</span><span class="sxs-lookup"><span data-stu-id="8682a-210">The entire regex must match the entire token.</span></span> <span data-ttu-id="8682a-211">第一個擷取必須是要查閱組態來源中的語彙基元名稱。</span><span class="sxs-lookup"><span data-stu-id="8682a-211">The first capture must be the token name to look up in the configuration source.</span></span>

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a><span data-ttu-id="8682a-212">在 Microsoft.Configuration.ConfigurationBuilders 組態產生器</span><span class="sxs-lookup"><span data-stu-id="8682a-212">Configuration builders in Microsoft.Configuration.ConfigurationBuilders</span></span>

### <a name="environmentconfigbuilder"></a><span data-ttu-id="8682a-213">EnvironmentConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="8682a-213">EnvironmentConfigBuilder</span></span>

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

<span data-ttu-id="8682a-214">[EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span><span class="sxs-lookup"><span data-stu-id="8682a-214">The [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span></span>

* <span data-ttu-id="8682a-215">是最簡單的組態產生器。</span><span class="sxs-lookup"><span data-stu-id="8682a-215">Is the simplest of the configuration builders.</span></span>
* <span data-ttu-id="8682a-216">從環境中讀取的值。</span><span class="sxs-lookup"><span data-stu-id="8682a-216">Reads values from the environment.</span></span>
* <span data-ttu-id="8682a-217">沒有任何其他組態選項。</span><span class="sxs-lookup"><span data-stu-id="8682a-217">Does not have any additional configuration options.</span></span>
* <span data-ttu-id="8682a-218">`name`屬性值為任意值。</span><span class="sxs-lookup"><span data-stu-id="8682a-218">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="8682a-219">**注意：** 在 Windows 容器環境中，在執行階段設定的變數只會插入至進入點程序的環境。</span><span class="sxs-lookup"><span data-stu-id="8682a-219">**Note:** In a Windows container environment, variables set at run time are only injected into the EntryPoint process environment.</span></span> <span data-ttu-id="8682a-220">應用程式，以服務方式執行，或非進入點程序無法挑選這些變數除非否則插入透過容器中的機制。</span><span class="sxs-lookup"><span data-stu-id="8682a-220">Apps that run as a service or a non-EntryPoint process do not pick up these variables unless they are otherwise injected through a mechanism in the container.</span></span> <span data-ttu-id="8682a-221">針對[IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-基礎容器的目前版本[ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41)會處理在*DefaultAppPool*只。</span><span class="sxs-lookup"><span data-stu-id="8682a-221">For [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-based containers, the current version of [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) handles this in the *DefaultAppPool* only.</span></span> <span data-ttu-id="8682a-222">開發自己的資料隱碼攻擊機制，不進入點程序，可能需要其他的 Windows 型容器變體。</span><span class="sxs-lookup"><span data-stu-id="8682a-222">Other Windows-based container variants may need to develop their own injection mechanism for non-EntryPoint processes.</span></span>

### <a name="usersecretsconfigbuilder"></a><span data-ttu-id="8682a-223">UserSecretsConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="8682a-223">UserSecretsConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="8682a-224">永遠不會儲存密碼、 機密的連接字串或其他來源的程式碼中的機密資料。</span><span class="sxs-lookup"><span data-stu-id="8682a-224">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="8682a-225">不應使用生產環境祕密進行開發或測試。</span><span class="sxs-lookup"><span data-stu-id="8682a-225">Production secrets should not be used for development or test.</span></span>

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

<span data-ttu-id="8682a-226">此組態產生器會提供類似的功能[ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets)。</span><span class="sxs-lookup"><span data-stu-id="8682a-226">This configuration builder provides a feature similar to [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).</span></span>

<span data-ttu-id="8682a-227">[UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/)可用在.NET Framework 專案中，但必須指定密碼檔案。</span><span class="sxs-lookup"><span data-stu-id="8682a-227">The [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) can be used in .NET Framework projects, but a secrets file must be specified.</span></span> <span data-ttu-id="8682a-228">或者，您可以定義`UserSecretsId`專案中的屬性檔案並建立正確的位置進行讀取的未經處理的密碼檔案。</span><span class="sxs-lookup"><span data-stu-id="8682a-228">Alternatively, you can define the `UserSecretsId` property in the project file and create the raw secrets file in the correct location for reading.</span></span> <span data-ttu-id="8682a-229">若要保留外部的相依性，從您的專案，祕密的檔案是 XML 格式。</span><span class="sxs-lookup"><span data-stu-id="8682a-229">To keep external dependencies out of your project, the secret file is XML formatted.</span></span> <span data-ttu-id="8682a-230">XML 格式化是實作詳細資料，格式應該不依賴。</span><span class="sxs-lookup"><span data-stu-id="8682a-230">The XML formatting is an implementation detail, and the format should not be relied upon.</span></span> <span data-ttu-id="8682a-231">如果您要共用*secrets.json*檔案中使用.NET Core 專案，請考慮使用[SimpleJsonConfigBuilder](#simplejsonconfig)。</span><span class="sxs-lookup"><span data-stu-id="8682a-231">If you need to share a *secrets.json* file with .NET Core projects, consider using the [SimpleJsonConfigBuilder](#simplejsonconfig).</span></span> <span data-ttu-id="8682a-232">`SimpleJsonConfigBuilder`格式化的.NET Core 也應該考慮的實作詳細資料，有可能變更。</span><span class="sxs-lookup"><span data-stu-id="8682a-232">The `SimpleJsonConfigBuilder` format for .NET Core should also be considered an implementation detail subject to change.</span></span>

<span data-ttu-id="8682a-233">組態屬性`UserSecretsConfigBuilder`:</span><span class="sxs-lookup"><span data-stu-id="8682a-233">Configuration attributes for `UserSecretsConfigBuilder`:</span></span>

* <span data-ttu-id="8682a-234">`userSecretsId` -這是用來識別 XML 祕密檔案的慣用的方法。</span><span class="sxs-lookup"><span data-stu-id="8682a-234">`userSecretsId` - This is the preferred method for identifying an XML secrets file.</span></span> <span data-ttu-id="8682a-235">它的運作方式類似於.NET Core，使用`UserSecretsId`專案來儲存這個識別項的屬性。</span><span class="sxs-lookup"><span data-stu-id="8682a-235">It works similar to .NET Core, which uses a `UserSecretsId` project property to store this identifier.</span></span> <span data-ttu-id="8682a-236">必須是唯一的字串，它不需要是 GUID。</span><span class="sxs-lookup"><span data-stu-id="8682a-236">The string must be unique, it doesn't need to be a GUID.</span></span> <span data-ttu-id="8682a-237">利用此屬性，`UserSecretsConfigBuilder`查看已知的本機位置 (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) 屬於此識別碼的密碼檔案。</span><span class="sxs-lookup"><span data-stu-id="8682a-237">With this attribute, the `UserSecretsConfigBuilder` look in a well-known local location (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) for a secrets file belonging to this identifier.</span></span>
* <span data-ttu-id="8682a-238">`userSecretsFile` -此選擇性屬性，指定包含機密資料的檔案。</span><span class="sxs-lookup"><span data-stu-id="8682a-238">`userSecretsFile` - An optional attribute specifying the file containing the secrets.</span></span> <span data-ttu-id="8682a-239">`~`字元會在開始時用來參考應用程式根目錄。</span><span class="sxs-lookup"><span data-stu-id="8682a-239">The `~` character can be used at the start to reference the application root.</span></span> <span data-ttu-id="8682a-240">任一此屬性或`userSecretsId`是必要的屬性。</span><span class="sxs-lookup"><span data-stu-id="8682a-240">Either this attribute or the `userSecretsId` attribute is required.</span></span> <span data-ttu-id="8682a-241">如果同時指定這兩者，`userSecretsFile`會優先使用。</span><span class="sxs-lookup"><span data-stu-id="8682a-241">If both are specified, `userSecretsFile` takes precedence.</span></span>
* <span data-ttu-id="8682a-242">`optional`： 布林值，預設值`true`-如果找不到祕密檔案，可防止發生例外狀況。</span><span class="sxs-lookup"><span data-stu-id="8682a-242">`optional`: boolean, default value `true` - Prevents an exception if the secrets file cannot be found.</span></span> 
* <span data-ttu-id="8682a-243">`name`屬性值為任意值。</span><span class="sxs-lookup"><span data-stu-id="8682a-243">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="8682a-244">祕密檔案具有下列格式：</span><span class="sxs-lookup"><span data-stu-id="8682a-244">The secrets file has the following format:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a><span data-ttu-id="8682a-245">AzureKeyVaultConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="8682a-245">AzureKeyVaultConfigBuilder</span></span>

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

<span data-ttu-id="8682a-246">[AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/)讀取中儲存值[Azure Key Vault](/azure/key-vault/key-vault-whatis)。</span><span class="sxs-lookup"><span data-stu-id="8682a-246">The [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) reads values stored in the [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span></span>

<span data-ttu-id="8682a-247">`vaultName` 是所需 （保存庫的名稱），或是向保存庫 URI。</span><span class="sxs-lookup"><span data-stu-id="8682a-247">`vaultName` is required (either the name of the vault or a URI to the vault).</span></span> <span data-ttu-id="8682a-248">其他屬性允許連接至哪一個保存庫相關的控制項，但未搭配環境中執行應用程式時才需要`Microsoft.Azure.Services.AppAuthentication`。</span><span class="sxs-lookup"><span data-stu-id="8682a-248">The other attributes allow control about which vault to connect to, but are only necessary if the application is not running in an environment that works with `Microsoft.Azure.Services.AppAuthentication`.</span></span> <span data-ttu-id="8682a-249">Azure 服務驗證程式庫用來自動盡可能選取從執行環境的連接資訊。</span><span class="sxs-lookup"><span data-stu-id="8682a-249">The Azure Services Authentication library is used to automatically pick up connection information from the execution environment if possible.</span></span> <span data-ttu-id="8682a-250">您可以會覆寫自動挑選的連接資訊所提供的連接字串。</span><span class="sxs-lookup"><span data-stu-id="8682a-250">You can override automatically pick up of connection information by providing a connection string.</span></span>

* <span data-ttu-id="8682a-251">`vaultName` -需要`uri`中未提供。</span><span class="sxs-lookup"><span data-stu-id="8682a-251">`vaultName` - Required if `uri` in not provided.</span></span> <span data-ttu-id="8682a-252">您要讀取索引鍵/值組的 Azure 訂用帳戶中指定的保存庫名稱。</span><span class="sxs-lookup"><span data-stu-id="8682a-252">Specifies the name of the vault in your Azure subscription from which to read key/value pairs.</span></span>
* <span data-ttu-id="8682a-253">`connectionString` -連接字串供[azureservicetokenprovider 會](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)</span><span class="sxs-lookup"><span data-stu-id="8682a-253">`connectionString` - A connection string usable by [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)</span></span>
* <span data-ttu-id="8682a-254">`uri` -連接到具有指定之其他 Key Vault 提供者`uri`值。</span><span class="sxs-lookup"><span data-stu-id="8682a-254">`uri` - Connects to other Key Vault providers with the specified `uri` value.</span></span> <span data-ttu-id="8682a-255">如果未指定，Azure (`vaultName`) 是保存庫提供者。</span><span class="sxs-lookup"><span data-stu-id="8682a-255">If not specified, Azure (`vaultName`) is the vault provider.</span></span>
* <span data-ttu-id="8682a-256">`version` -Azure Key Vault 祕密提供版本控制功能。</span><span class="sxs-lookup"><span data-stu-id="8682a-256">`version` - Azure Key Vault provides a versioning feature for secrets.</span></span> <span data-ttu-id="8682a-257">如果`version`指定時，產生器只會擷取相符的這個版本的祕密。</span><span class="sxs-lookup"><span data-stu-id="8682a-257">If `version` is specified, the builder only retrieves secrets matching this version.</span></span>
* <span data-ttu-id="8682a-258">`preloadSecretNames` -根據預設，此產生器從其中**所有**金鑰在金鑰保存庫的名稱，初始化時。</span><span class="sxs-lookup"><span data-stu-id="8682a-258">`preloadSecretNames` - By default, this builder querys **all** key names in the key vault when it is initialized.</span></span> <span data-ttu-id="8682a-259">若要避免讀取所有的索引鍵值，請將此屬性設定為`false`。</span><span class="sxs-lookup"><span data-stu-id="8682a-259">To prevent reading all key values, set this attribute to `false`.</span></span> <span data-ttu-id="8682a-260">這個設定設為`false`一次讀取一個密碼。</span><span class="sxs-lookup"><span data-stu-id="8682a-260">Setting this to `false` reads secrets one at a time.</span></span> <span data-ttu-id="8682a-261">讀取一次一個非常實用的如果保存庫可讓 「 取得 」 存取權，但不是 「 清單 」 存取的密碼。</span><span class="sxs-lookup"><span data-stu-id="8682a-261">Reading secrets one at a time can useful if the vault allows "Get" access but not "List" access.</span></span> <span data-ttu-id="8682a-262">**注意︰** 使用時`Greedy`模式中，`preloadSecretNames`必須是`true`（預設值。）</span><span class="sxs-lookup"><span data-stu-id="8682a-262">**Note:** When using `Greedy` mode, `preloadSecretNames` must be `true` (the default.)</span></span>

### <a name="keyperfileconfigbuilder"></a><span data-ttu-id="8682a-263">KeyPerFileConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="8682a-263">KeyPerFileConfigBuilder</span></span>

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

<span data-ttu-id="8682a-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/)是基本組態產生器會使用目錄的檔案，做為來源的值。</span><span class="sxs-lookup"><span data-stu-id="8682a-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) is a basic configuration builder that uses a directory's files as a source of values.</span></span> <span data-ttu-id="8682a-265">檔案的名稱為索引鍵中，而且內容值。</span><span class="sxs-lookup"><span data-stu-id="8682a-265">A file's name is the key, and the contents are the value.</span></span> <span data-ttu-id="8682a-266">協調的容器環境中執行時，此組態產生器可以是很有用。</span><span class="sxs-lookup"><span data-stu-id="8682a-266">This configuration builder can be useful when running in an orchestrated container environment.</span></span> <span data-ttu-id="8682a-267">像是 Docker Swarm 和 Kubernetes 所提供的系統`secrets`至其協調的 windows 容器，如此每個檔案的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="8682a-267">Systems like Docker Swarm and Kubernetes provide `secrets` to their orchestrated windows containers in this key-per-file manner.</span></span>

<span data-ttu-id="8682a-268">屬性詳細資料：</span><span class="sxs-lookup"><span data-stu-id="8682a-268">Attribute details:</span></span>

* <span data-ttu-id="8682a-269">`directoryPath` - 必要項。</span><span class="sxs-lookup"><span data-stu-id="8682a-269">`directoryPath` - Required.</span></span> <span data-ttu-id="8682a-270">指定的路徑尋找的值。</span><span class="sxs-lookup"><span data-stu-id="8682a-270">Specifies a path to look in for values.</span></span> <span data-ttu-id="8682a-271">密碼會儲存在 Windows 的 docker *C:\ProgramData\Docker\secrets*預設目錄。</span><span class="sxs-lookup"><span data-stu-id="8682a-271">Docker for Windows secrets are stored in the *C:\ProgramData\Docker\secrets* directory by default.</span></span>
* <span data-ttu-id="8682a-272">`ignorePrefix` 會排除在檔案開頭為此前置詞。</span><span class="sxs-lookup"><span data-stu-id="8682a-272">`ignorePrefix` - Files that start with this prefix are excluded.</span></span> <span data-ttu-id="8682a-273">預設值為"ignore"。</span><span class="sxs-lookup"><span data-stu-id="8682a-273">Defaults to "ignore.".</span></span>
* <span data-ttu-id="8682a-274">`keyDelimiter` 預設值是`null`。</span><span class="sxs-lookup"><span data-stu-id="8682a-274">`keyDelimiter` - Default value is `null`.</span></span> <span data-ttu-id="8682a-275">如果指定，組態產生器會周遊多個層級的目錄中，建立使用此分隔符號的索引鍵名稱。</span><span class="sxs-lookup"><span data-stu-id="8682a-275">If specified, the configuration builder traverses multiple levels of the directory, building up key names with this delimiter.</span></span> <span data-ttu-id="8682a-276">如果此值為`null`，組態產生器只會尋找最上層的目錄。</span><span class="sxs-lookup"><span data-stu-id="8682a-276">If this value is `null`, the configuration builder only looks at the top level of the directory.</span></span>
* <span data-ttu-id="8682a-277">`optional` 預設值是`false`。</span><span class="sxs-lookup"><span data-stu-id="8682a-277">`optional` -  Default value is `false`.</span></span> <span data-ttu-id="8682a-278">指定是否來源目錄不存在，組態產生器是否應該導致錯誤。</span><span class="sxs-lookup"><span data-stu-id="8682a-278">Specifies whether the configuration builder should cause errors if the source directory doesn't exist.</span></span>

### <a name="simplejsonconfigbuilder"></a><span data-ttu-id="8682a-279">SimpleJsonConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="8682a-279">SimpleJsonConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="8682a-280">永遠不會儲存密碼、 機密的連接字串或其他來源的程式碼中的機密資料。</span><span class="sxs-lookup"><span data-stu-id="8682a-280">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="8682a-281">不應使用生產環境祕密進行開發或測試。</span><span class="sxs-lookup"><span data-stu-id="8682a-281">Production secrets should not be used for development or test.</span></span>

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

<span data-ttu-id="8682a-282">.NET core 專案通常會使用組態的 JSON 檔案。</span><span class="sxs-lookup"><span data-stu-id="8682a-282">.NET Core projects frequently use JSON files for configuration.</span></span> <span data-ttu-id="8682a-283">[SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/)產生器可讓.NET Framework 中使用的.NET Core 的 JSON 檔案。</span><span class="sxs-lookup"><span data-stu-id="8682a-283">The [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) builder allows .NET Core JSON files to be used in the .NET Framework.</span></span> <span data-ttu-id="8682a-284">此組態產生器會提供從一般的索引鍵/值來源的.NET Framework 組態的特定索引鍵/值區域到基本的對應。</span><span class="sxs-lookup"><span data-stu-id="8682a-284">This configuration builder is provides a basic mapping from a flat key/value source into specific key/value areas of .NET Framework configuration.</span></span> <span data-ttu-id="8682a-285">此組態產生器 」**不**提供階層式組態。</span><span class="sxs-lookup"><span data-stu-id="8682a-285">This configuration builder does **not** provide for hierarchical configurations.</span></span> <span data-ttu-id="8682a-286">類似於字典，而不是複雜的階層式物件的 JSON 支援檔案。</span><span class="sxs-lookup"><span data-stu-id="8682a-286">The JSON backing file is similar to a dictionary, not a complex hierarchical object.</span></span> <span data-ttu-id="8682a-287">可以使用多層級的階層式檔案。</span><span class="sxs-lookup"><span data-stu-id="8682a-287">A multi-level hierarchical file can be used.</span></span> <span data-ttu-id="8682a-288">此提供者`flatten`s 附加在每個層級使用的屬性名稱的深度`:`做為分隔符號。</span><span class="sxs-lookup"><span data-stu-id="8682a-288">This provider `flatten`s the depth by appending the property name at each level using `:` as a delimiter.</span></span>

<span data-ttu-id="8682a-289">屬性詳細資料：</span><span class="sxs-lookup"><span data-stu-id="8682a-289">Attribute details:</span></span>

* <span data-ttu-id="8682a-290">`jsonFile` - 必要項。</span><span class="sxs-lookup"><span data-stu-id="8682a-290">`jsonFile` - Required.</span></span> <span data-ttu-id="8682a-291">指定要讀取從 JSON 檔案。</span><span class="sxs-lookup"><span data-stu-id="8682a-291">Specifies the JSON file to read from.</span></span> <span data-ttu-id="8682a-292">`~`字元會在開始時用來參考應用程式根目錄。</span><span class="sxs-lookup"><span data-stu-id="8682a-292">The `~` character can be used at the start to reference the app root.</span></span>
* <span data-ttu-id="8682a-293">`optional` -布林值，預設值是`true`。</span><span class="sxs-lookup"><span data-stu-id="8682a-293">`optional` - Boolean,  default value is `true`.</span></span> <span data-ttu-id="8682a-294">可避免擲回例外狀況，如果找不到 JSON 檔案。</span><span class="sxs-lookup"><span data-stu-id="8682a-294">Prevents throwing exceptions if the JSON file cannot be found.</span></span>
* <span data-ttu-id="8682a-295">`jsonMode` - `[Flat|Sectional]`.</span><span class="sxs-lookup"><span data-stu-id="8682a-295">`jsonMode` - `[Flat|Sectional]`.</span></span> <span data-ttu-id="8682a-296">`Flat` 是預設值。</span><span class="sxs-lookup"><span data-stu-id="8682a-296">`Flat` is the default.</span></span> <span data-ttu-id="8682a-297">當`jsonMode`是`Flat`，JSON 檔案是單一的一般索引鍵/值來源。</span><span class="sxs-lookup"><span data-stu-id="8682a-297">When `jsonMode` is `Flat`, the JSON file is a single flat key/value source.</span></span> <span data-ttu-id="8682a-298">`EnvironmentConfigBuilder`和`AzureKeyVaultConfigBuilder`也是單一的一般索引鍵/值來源。</span><span class="sxs-lookup"><span data-stu-id="8682a-298">The `EnvironmentConfigBuilder` and `AzureKeyVaultConfigBuilder` are also single flat key/value sources.</span></span> <span data-ttu-id="8682a-299">當`SimpleJsonConfigBuilder`中設定`Sectional`模式︰</span><span class="sxs-lookup"><span data-stu-id="8682a-299">When the `SimpleJsonConfigBuilder` is configured in `Sectional` mode:</span></span>

  * <span data-ttu-id="8682a-300">JSON 檔案會在概念上分成只在最高層級多個字典。</span><span class="sxs-lookup"><span data-stu-id="8682a-300">The JSON file is conceptually divided just at the top level into multiple dictionaries.</span></span>
  * <span data-ttu-id="8682a-301">每個字典只會套用至附加到它們的最上層的屬性名稱相符的組態區段。</span><span class="sxs-lookup"><span data-stu-id="8682a-301">Each of the dictionaries is only applied to the configuration section that matches the top-level property name attached to them.</span></span> <span data-ttu-id="8682a-302">例如: </span><span class="sxs-lookup"><span data-stu-id="8682a-302">For example:</span></span>

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a><span data-ttu-id="8682a-303">實作自訂的索引鍵/值的組態產生器</span><span class="sxs-lookup"><span data-stu-id="8682a-303">Implementing a custom key/value configuration builder</span></span>

<span data-ttu-id="8682a-304">如果組態產生器不符合您的需求，您可以撰寫的自訂承載。</span><span class="sxs-lookup"><span data-stu-id="8682a-304">If the configuration builders don't meet your needs, you can write a custom one.</span></span> <span data-ttu-id="8682a-305">`KeyValueConfigBuilder`替代模式和大部分的前置詞考量，會處理基底類別。</span><span class="sxs-lookup"><span data-stu-id="8682a-305">The `KeyValueConfigBuilder` base class handles substitution modes and most prefix concerns.</span></span> <span data-ttu-id="8682a-306">只需要實作的專案：</span><span class="sxs-lookup"><span data-stu-id="8682a-306">An implementing project need only:</span></span>

* <span data-ttu-id="8682a-307">繼承自基底類別，並實作基本的來源索引鍵/值組，透過`GetValue`和`GetAllValues`:</span><span class="sxs-lookup"><span data-stu-id="8682a-307">Inherit from the base class and implement a basic source of key/value pairs through the `GetValue` and `GetAllValues`:</span></span>
* <span data-ttu-id="8682a-308">新增[Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/)至專案。</span><span class="sxs-lookup"><span data-stu-id="8682a-308">Add the [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) to the project.</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

<span data-ttu-id="8682a-309">`KeyValueConfigBuilder`基底類別提供許多的工作且一致的行為索引鍵/值跨組態產生器。</span><span class="sxs-lookup"><span data-stu-id="8682a-309">The `KeyValueConfigBuilder` base class provides much of the work and consistent behavior across key/value configuration builders.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8682a-310">其他資源</span><span class="sxs-lookup"><span data-stu-id="8682a-310">Additional resources</span></span>

* [<span data-ttu-id="8682a-311">設定產生器的 GitHub 存放庫</span><span class="sxs-lookup"><span data-stu-id="8682a-311">Configuration Builders GitHub repository</span></span>](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [<span data-ttu-id="8682a-312">若要使用.NET 的 Azure 金鑰保存庫的服務對服務驗證</span><span class="sxs-lookup"><span data-stu-id="8682a-312">Service-to-service authentication to Azure Key Vault using .NET</span></span>](/azure/key-vault/service-to-service-authentication#connection-string-support)
