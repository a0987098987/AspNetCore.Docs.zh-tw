---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: OWIN 啟動類別偵測 |Microsoft Docs
author: Praburaj
description: 本教學課程會示範如何設定載入的 OWIN 啟動類別。 如需有關 OWIN 的詳細資訊，請參閱 < 概觀的 Katana 專案。 本教學課程是...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
ms.technology: ''
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: d7e18001cbbfc67397f32ace53d347acf49d7537
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388694"
---
<a name="owin-startup-class-detection"></a><span data-ttu-id="6f060-105">OWIN 啟動類別偵測</span><span class="sxs-lookup"><span data-stu-id="6f060-105">OWIN Startup Class Detection</span></span>
====================
<span data-ttu-id="6f060-106">藉由[Praburaj Thiagarajan](https://github.com/Praburaj)， [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="6f060-106">by [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="6f060-107">本教學課程會示範如何設定載入的 OWIN 啟動類別。</span><span class="sxs-lookup"><span data-stu-id="6f060-107">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="6f060-108">如需有關 OWIN 的詳細資訊，請參閱[概觀的 Katana 專案](an-overview-of-project-katana.md)。</span><span class="sxs-lookup"><span data-stu-id="6f060-108">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="6f060-109">本教學課程中所編寫的 Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )，Praburaj Thiagarajan 和 Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) )。</span><span class="sxs-lookup"><span data-stu-id="6f060-109">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
> 
> ## <a name="prerequisites"></a><span data-ttu-id="6f060-110">必要條件</span><span class="sxs-lookup"><span data-stu-id="6f060-110">Prerequisites</span></span>
> 
> [<span data-ttu-id="6f060-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="6f060-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a><span data-ttu-id="6f060-112">OWIN 啟動類別偵測</span><span class="sxs-lookup"><span data-stu-id="6f060-112">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="6f060-113">每個 OWIN 應用程式有啟動類別，您可在其中指定應用程式管線的元件。</span><span class="sxs-lookup"><span data-stu-id="6f060-113">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="6f060-114">有許多種，您可以連接您的啟動類別的執行階段、 根據裝載模型選擇 （OwinHost、 IIS 和 IIS Express）。</span><span class="sxs-lookup"><span data-stu-id="6f060-114">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="6f060-115">本教學課程中所示的 startup 類別可以用於每個裝載的應用程式。</span><span class="sxs-lookup"><span data-stu-id="6f060-115">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="6f060-116">您可以連接的啟動類別裝載執行階段使用其中一種方法：</span><span class="sxs-lookup"><span data-stu-id="6f060-116">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>  

1. <span data-ttu-id="6f060-117">**命名慣例**: Katana 會尋找名為類別`Startup`比對組件名稱或全域命名空間的命名空間中。</span><span class="sxs-lookup"><span data-stu-id="6f060-117">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="6f060-118">**OwinStartup 屬性**： 這是大部分開發人員將移至指定的啟動類別的方法。</span><span class="sxs-lookup"><span data-stu-id="6f060-118">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="6f060-119">下列屬性會設定為啟動類別`TestStartup`類別中`StartupDemo`命名空間。</span><span class="sxs-lookup"><span data-stu-id="6f060-119">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span> 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   <span data-ttu-id="6f060-120">`OwinStartup`屬性會覆寫的命名慣例。</span><span class="sxs-lookup"><span data-stu-id="6f060-120">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="6f060-121">您也可以使用這個屬性指定好記的名稱，不過，使用易記的名稱需要您同時使用`appSetting`組態檔中的項目。</span><span class="sxs-lookup"><span data-stu-id="6f060-121">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="6f060-122">**在組態檔中的 appSetting 元素**:`appSetting`項目會覆寫`OwinStartup`屬性和命名慣例。</span><span class="sxs-lookup"><span data-stu-id="6f060-122">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="6f060-123">您可以有多個啟動類別 (每個使用`OwinStartup`屬性)，並設定啟動類別將會載入組態檔使用類似下列的標記中：</span><span class="sxs-lookup"><span data-stu-id="6f060-123">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   <span data-ttu-id="6f060-124">也可以使用下列機碼，明確指定的啟動類別和組件：</span><span class="sxs-lookup"><span data-stu-id="6f060-124">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   <span data-ttu-id="6f060-125">下列 XML 組態檔中的指定的好記的啟動類別名稱`ProductionConfiguration`。</span><span class="sxs-lookup"><span data-stu-id="6f060-125">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   <span data-ttu-id="6f060-126">必須使用上述的標記取代為下列`OwinStartup`屬性指定好記的名稱，並造成`ProductionStartup2`類別來執行。</span><span class="sxs-lookup"><span data-stu-id="6f060-126">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="6f060-127">若要停用新增 OWIN 啟動探索`appSetting owin:AutomaticAppStartup`值為`"false"`web.config 檔案中。</span><span class="sxs-lookup"><span data-stu-id="6f060-127">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="6f060-128">建立 ASP.NET Web 應用程式使用 OWIN 啟動</span><span class="sxs-lookup"><span data-stu-id="6f060-128">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="6f060-129">建立空的 Asp.Net web 應用程式並將它命名**StartupDemo**。</span><span class="sxs-lookup"><span data-stu-id="6f060-129">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="6f060-130">-安裝`Microsoft.Owin.Host.SystemWeb`使用 NuGet 套件管理員。</span><span class="sxs-lookup"><span data-stu-id="6f060-130">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="6f060-131">從**工具**功能表上，選取**程式庫套件管理員**，然後**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="6f060-131">From the **Tools** menu, select **Library Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="6f060-132">輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="6f060-132">Enter the following command:</span></span>  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. <span data-ttu-id="6f060-133">新增 OWIN 啟動類別。</span><span class="sxs-lookup"><span data-stu-id="6f060-133">Add an OWIN startup class.</span></span> <span data-ttu-id="6f060-134">Visual Studio 2013 中以滑鼠右鍵按一下專案，然後選取**加入類別**。-在**加入新項目**對話方塊中，輸入*OWIN*在搜尋欄位中，並將 Startup.cs 中，名稱然後按一下**新增**。</span><span class="sxs-lookup"><span data-stu-id="6f060-134">In Visual Studio 2013 right click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then click **Add**.</span></span>  
  
     ![](owin-startup-class-detection/_static/image1.png)   
  
   <span data-ttu-id="6f060-135">您想要新增的下次*Owin 啟動類別*，則會在可從**新增**功能表。</span><span class="sxs-lookup"><span data-stu-id="6f060-135">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>  
   
     ![](owin-startup-class-detection/_static/image2.png)  
  
   <span data-ttu-id="6f060-136">或者，您可以以滑鼠右鍵按一下專案，並選取**新增**，然後選取**新項目**，然後選取**Owin 啟動類別**。</span><span class="sxs-lookup"><span data-stu-id="6f060-136">Alternatively, you can right click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>  
  
     ![](owin-startup-class-detection/_static/image3.png)  
  
- <span data-ttu-id="6f060-137">取代產生的程式碼*Startup.cs*以下列檔案：</span><span class="sxs-lookup"><span data-stu-id="6f060-137">Replace the generated code in the *Startup.cs* file with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
  <span data-ttu-id="6f060-138">`app.Use` Lambda 運算式用來註冊指定的中介軟體元件至 OWIN 管線。</span><span class="sxs-lookup"><span data-stu-id="6f060-138">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="6f060-139">在此情況下我們設定了連入要求的連入要求回應之前的記錄。</span><span class="sxs-lookup"><span data-stu-id="6f060-139">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="6f060-140">`next`參數是委派 ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [工作](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) 管線中的下一個元件。</span><span class="sxs-lookup"><span data-stu-id="6f060-140">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="6f060-141">`app.Run` Lambda 運算式的內送要求管線，並提供回應的機制。</span><span class="sxs-lookup"><span data-stu-id="6f060-141">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="6f060-142">在上述程式碼中，我們有註解`OwinStartup`屬性，我們依賴執行名為類別的慣例`Startup`。-按***F5***執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="6f060-142">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="6f060-143">按 重新整理數次。</span><span class="sxs-lookup"><span data-stu-id="6f060-143">Hit refresh a few times.</span></span>  
  
    ![](owin-startup-class-detection/_static/image4.png)  
  <span data-ttu-id="6f060-144">注意： 本教學課程中的映像中顯示的數字不會符合您看到的數字。</span><span class="sxs-lookup"><span data-stu-id="6f060-144">Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="6f060-145">毫秒數的字串用來顯示新的回應，當您重新整理頁面。</span><span class="sxs-lookup"><span data-stu-id="6f060-145">The millisecond string is used to show a new response when you refresh the page.</span></span>  
  <span data-ttu-id="6f060-146">您可以看到中的追蹤資訊**輸出**視窗。</span><span class="sxs-lookup"><span data-stu-id="6f060-146">You can see the trace information in the **Output** window.</span></span>  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="6f060-147">新增更多的啟動類別</span><span class="sxs-lookup"><span data-stu-id="6f060-147">Add More Startup Classes</span></span>

<span data-ttu-id="6f060-148">在本節中，我們將新增另一個啟動類別。</span><span class="sxs-lookup"><span data-stu-id="6f060-148">In this section we'll add another Startup class.</span></span> <span data-ttu-id="6f060-149">您可以將多個 OWIN 啟動類別新增至您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="6f060-149">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="6f060-150">例如，您可能要建立開發、 測試和生產環境的啟動類別。</span><span class="sxs-lookup"><span data-stu-id="6f060-150">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="6f060-151">建立新的 OWIN 啟動類別並將它命名`ProductionStartup`。</span><span class="sxs-lookup"><span data-stu-id="6f060-151">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="6f060-152">使用下列程式碼取代產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="6f060-152">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="6f060-153">按下控制項 F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="6f060-153">Press Control F5 to run the app.</span></span> <span data-ttu-id="6f060-154">`OwinStartup`屬性會指定在執行生產環境的啟動類別。</span><span class="sxs-lookup"><span data-stu-id="6f060-154">The `OwinStartup` attribute specifies the production startup class is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="6f060-155">建立另一個的 OWIN 啟動類別並將它命名`TestStartup`。</span><span class="sxs-lookup"><span data-stu-id="6f060-155">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="6f060-156">使用下列程式碼取代產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="6f060-156">Replace the generated code with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   <span data-ttu-id="6f060-157">`OwinStartup`上述的屬性多載會指定`TestingConfiguration`作為*易記*Startup 類別的名稱。</span><span class="sxs-lookup"><span data-stu-id="6f060-157">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="6f060-158">開啟*web.config*檔案，並新增 OWIN 應用程式啟動金鑰，指定啟動類別的易記名稱：</span><span class="sxs-lookup"><span data-stu-id="6f060-158">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="6f060-159">按下控制項 F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="6f060-159">Press Control F5 to run the app.</span></span> <span data-ttu-id="6f060-160">應用程式設定項目會優先於，與測試組態執行。</span><span class="sxs-lookup"><span data-stu-id="6f060-160">The app settings element takes precedent, and the test configuration is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="6f060-161">移除*易記*名稱從`OwinStartup`屬性中`TestStartup`類別。</span><span class="sxs-lookup"><span data-stu-id="6f060-161">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="6f060-162">取代 OWIN 應用程式的啟動金鑰，在*web.config*以下列檔案：</span><span class="sxs-lookup"><span data-stu-id="6f060-162">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="6f060-163">還原`OwinStartup`Visual Studio 所產生的預設屬性程式碼的每個類別中的屬性：</span><span class="sxs-lookup"><span data-stu-id="6f060-163">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    <span data-ttu-id="6f060-164">每個以下的 OWIN 應用程式的啟動金鑰會導致執行的生產環境類別。</span><span class="sxs-lookup"><span data-stu-id="6f060-164">Each of the OWIN App startup keys below will cause the production class to run.</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    <span data-ttu-id="6f060-165">最後一個的啟動金鑰指定啟動組態方法。</span><span class="sxs-lookup"><span data-stu-id="6f060-165">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="6f060-166">下列的 OWIN 應用程式啟動金鑰可讓您變更組態類別的名稱`MyConfiguration`。</span><span class="sxs-lookup"><span data-stu-id="6f060-166">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="6f060-167">使用 Owinhost.exe</span><span class="sxs-lookup"><span data-stu-id="6f060-167">Using Owinhost.exe</span></span>

1. <span data-ttu-id="6f060-168">以下列標記取代 Web.config 檔案：</span><span class="sxs-lookup"><span data-stu-id="6f060-168">Replace the Web.config file with the following markup:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   <span data-ttu-id="6f060-169">Wins 的最後一個金鑰，因此在此情況下`TestStartup`指定。</span><span class="sxs-lookup"><span data-stu-id="6f060-169">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="6f060-170">安裝 Owinhost 從 PMC:</span><span class="sxs-lookup"><span data-stu-id="6f060-170">Install Owinhost from the PMC:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="6f060-171">瀏覽至應用程式資料夾 (包含的資料夾*Web.config*檔案)，並在命令提示字元和型別：</span><span class="sxs-lookup"><span data-stu-id="6f060-171">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   <span data-ttu-id="6f060-172">[命令] 視窗會顯示：</span><span class="sxs-lookup"><span data-stu-id="6f060-172">The command window will show:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="6f060-173">啟動 URL 的瀏覽器`http://localhost:5000/`。</span><span class="sxs-lookup"><span data-stu-id="6f060-173">Launch a browser with the URL `http://localhost:5000/`.</span></span>  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
   <span data-ttu-id="6f060-174">OwinHost 採用上面所列的啟動慣例。</span><span class="sxs-lookup"><span data-stu-id="6f060-174">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="6f060-175">在 [命令] 視窗中，按 Enter 以結束 OwinHost。</span><span class="sxs-lookup"><span data-stu-id="6f060-175">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="6f060-176">在 `ProductionStartup`類別中，新增下列 OwinStartup 屬性指定的易記名稱*ProductionConfiguration*。</span><span class="sxs-lookup"><span data-stu-id="6f060-176">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="6f060-177">在命令提示字元並輸入：</span><span class="sxs-lookup"><span data-stu-id="6f060-177">In the command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   <span data-ttu-id="6f060-178">載入實際執行啟動類別。</span><span class="sxs-lookup"><span data-stu-id="6f060-178">The Production startup class is loaded.</span></span>  
    ![](owin-startup-class-detection/_static/image9.png)  
   <span data-ttu-id="6f060-179">我們的應用程式有多個啟動類別，並在此範例中我們已延後載入執行階段之前的啟動類別。</span><span class="sxs-lookup"><span data-stu-id="6f060-179">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="6f060-180">測試下列執行階段啟動選項：</span><span class="sxs-lookup"><span data-stu-id="6f060-180">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
