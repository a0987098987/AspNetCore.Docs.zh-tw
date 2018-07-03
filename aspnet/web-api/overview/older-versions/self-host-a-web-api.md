---
uid: web-api/overview/older-versions/self-host-a-web-api
title: 自我裝載 ASP.NET Web API 1 (C#) |Microsoft Docs
author: MikeWasson
description: ASP.NET Web API 不需要 IIS。 您可以在您自己的主控件程序中，自我裝載的 web API。 本教學課程會示範如何裝載 web API 應用程式的主控台內...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2012
ms.topic: article
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 28ba54acd7947a1c837fb5f73b292901e6b19260
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376254"
---
<a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="994a4-105">自我裝載 ASP.NET Web API 1 (C#)</span><span class="sxs-lookup"><span data-stu-id="994a4-105">Self-Host ASP.NET Web API 1 (C#)</span></span>
====================
<span data-ttu-id="994a4-106">藉由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="994a4-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="994a4-107">ASP.NET Web API 不需要 IIS。</span><span class="sxs-lookup"><span data-stu-id="994a4-107">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="994a4-108">您可以在您自己的主控件程序中，自我裝載的 web API。</span><span class="sxs-lookup"><span data-stu-id="994a4-108">You can self-host a web API in your own host process.</span></span> <span data-ttu-id="994a4-109">本教學課程會示範如何裝載於主控台應用程式的 web API。</span><span class="sxs-lookup"><span data-stu-id="994a4-109">This tutorial shows how to host a web API inside a console application.</span></span>
> 
> <span data-ttu-id="994a4-110">**新的應用程式應該使用 OWIN 自我裝載 Web API。**</span><span class="sxs-lookup"><span data-stu-id="994a4-110">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="994a4-111">請參閱[使用 OWIN 自我裝載 ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="994a4-111">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="994a4-112">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="994a4-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="994a4-113">Web API 1</span><span class="sxs-lookup"><span data-stu-id="994a4-113">Web API 1</span></span>
> - <span data-ttu-id="994a4-114">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="994a4-114">Visual Studio 2012</span></span>


## <a name="create-the-console-application-project"></a><span data-ttu-id="994a4-115">建立主控台應用程式專案</span><span class="sxs-lookup"><span data-stu-id="994a4-115">Create the Console Application Project</span></span>

<span data-ttu-id="994a4-116">啟動 Visual Studio，然後選取**新的專案**從**開始**頁面。</span><span class="sxs-lookup"><span data-stu-id="994a4-116">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="994a4-117">或從**檔案**功能表上，選取**新增**，然後**專案**。</span><span class="sxs-lookup"><span data-stu-id="994a4-117">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="994a4-118">在 **範本**窗格中，選取**已安裝的範本**展開**Visual C#** 節點。</span><span class="sxs-lookup"><span data-stu-id="994a4-118">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="994a4-119">底下**Visual C#**，選取**Windows**。</span><span class="sxs-lookup"><span data-stu-id="994a4-119">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="994a4-120">在專案範本清單中，選取**主控台應用程式**。</span><span class="sxs-lookup"><span data-stu-id="994a4-120">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="994a4-121">將專案命名為&quot;SelfHost&quot;然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="994a4-121">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="994a4-122">設定目標架構 (Visual Studio 2010)</span><span class="sxs-lookup"><span data-stu-id="994a4-122">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="994a4-123">如果您使用 Visual Studio 2010，將.NET Framework 4.0 目標 framework。</span><span class="sxs-lookup"><span data-stu-id="994a4-123">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="994a4-124">(根據預設，專案範本的目標[.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile)。)</span><span class="sxs-lookup"><span data-stu-id="994a4-124">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="994a4-125">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="994a4-125">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="994a4-126">在 **目標 framework**下拉式清單中，將目標 framework 變更為.NET Framework 4.0。</span><span class="sxs-lookup"><span data-stu-id="994a4-126">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="994a4-127">當出現提示，以套用變更，請按一下**是**。</span><span class="sxs-lookup"><span data-stu-id="994a4-127">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="994a4-128">安裝 NuGet 套件管理員</span><span class="sxs-lookup"><span data-stu-id="994a4-128">Install NuGet Package Manager</span></span>

<span data-ttu-id="994a4-129">NuGet 套件管理員是最簡單的方式，將 Web API 組件新增至非 ASP.NET 專案。</span><span class="sxs-lookup"><span data-stu-id="994a4-129">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="994a4-130">若要檢查是否已安裝 NuGet 套件管理員，請按一下**工具**Visual Studio 中的功能表。</span><span class="sxs-lookup"><span data-stu-id="994a4-130">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="994a4-131">如果您看到的功能表項目呼叫**程式庫套件管理員**，則您可以 NuGet 套件管理員。</span><span class="sxs-lookup"><span data-stu-id="994a4-131">If you see a menu item called **Library Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="994a4-132">若要安裝 NuGet 套件管理員：</span><span class="sxs-lookup"><span data-stu-id="994a4-132">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="994a4-133">啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="994a4-133">Start Visual Studio.</span></span>
2. <span data-ttu-id="994a4-134">從**工具**功能表上，選取**擴充功能和更新**。</span><span class="sxs-lookup"><span data-stu-id="994a4-134">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="994a4-135">在 **擴充功能和更新**對話方塊中，選取**線上**。</span><span class="sxs-lookup"><span data-stu-id="994a4-135">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="994a4-136">如果您沒有看到 [NuGet 套件管理員]，請在搜尋方塊中輸入 「 nuget 封裝管理員 」。</span><span class="sxs-lookup"><span data-stu-id="994a4-136">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="994a4-137">選取 NuGet 套件管理員，然後按一下**下載**。</span><span class="sxs-lookup"><span data-stu-id="994a4-137">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="994a4-138">下載完成之後，系統會提示您安裝。</span><span class="sxs-lookup"><span data-stu-id="994a4-138">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="994a4-139">安裝完成之後，您可能會提示重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="994a4-139">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="994a4-140">新增 Web API NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="994a4-140">Add the Web API NuGet Package</span></span>

<span data-ttu-id="994a4-141">NuGet 套件管理員安裝之後，將 Web API 的自我裝載封裝加入專案。</span><span class="sxs-lookup"><span data-stu-id="994a4-141">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="994a4-142">從**工具**功能表上，選取**程式庫套件管理員**。</span><span class="sxs-lookup"><span data-stu-id="994a4-142">From the **Tools** menu, select **Library Package Manager**.</span></span> <span data-ttu-id="994a4-143">*請注意*： 如果您不會看到此功能表項目，請確定已正確安裝該 NuGet 套件管理員。</span><span class="sxs-lookup"><span data-stu-id="994a4-143">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="994a4-144">選取**管理方案的 NuGet 套件...**</span><span class="sxs-lookup"><span data-stu-id="994a4-144">Select **Manage NuGet Packages for Solution...**</span></span>
3. <span data-ttu-id="994a4-145">在 **管理 Nuget 封裝**對話方塊中，選取**線上**。</span><span class="sxs-lookup"><span data-stu-id="994a4-145">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="994a4-146">在 [搜尋] 方塊中，輸入&quot;Microsoft.AspNet.WebApi.SelfHost&quot;。</span><span class="sxs-lookup"><span data-stu-id="994a4-146">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="994a4-147">選取 ASP.NET Web API 自助主應用程式封裝，然後按一下**安裝**。</span><span class="sxs-lookup"><span data-stu-id="994a4-147">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="994a4-148">套件會安裝之後，請按一下**關閉**以關閉對話方塊。</span><span class="sxs-lookup"><span data-stu-id="994a4-148">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="994a4-149">請務必安裝名為 Microsoft.AspNet.WebApi.SelfHost，不 AspNetWebApi.SelfHost 的套件。</span><span class="sxs-lookup"><span data-stu-id="994a4-149">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="994a4-150">建立模型和控制器</span><span class="sxs-lookup"><span data-stu-id="994a4-150">Create the Model and Controller</span></span>

<span data-ttu-id="994a4-151">本教學課程使用相同的模型和控制器類別作為[開始使用](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="994a4-151">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="994a4-152">新增名為公用類別`Product`。</span><span class="sxs-lookup"><span data-stu-id="994a4-152">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="994a4-153">新增名為公用類別`ProductsController`。</span><span class="sxs-lookup"><span data-stu-id="994a4-153">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="994a4-154">從這個類別的衍生**System.Web.Http.ApiController**。</span><span class="sxs-lookup"><span data-stu-id="994a4-154">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="994a4-155">如需有關此控制器中的程式碼的詳細資訊，請參閱[開始使用](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="994a4-155">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="994a4-156">此控制器會定義三個 GET 動作：</span><span class="sxs-lookup"><span data-stu-id="994a4-156">This controller defines three GET actions:</span></span>

| <span data-ttu-id="994a4-157">URI</span><span class="sxs-lookup"><span data-stu-id="994a4-157">URI</span></span> | <span data-ttu-id="994a4-158">描述</span><span class="sxs-lookup"><span data-stu-id="994a4-158">Description</span></span> |
| --- | --- |
| <span data-ttu-id="994a4-159">/ api/產品</span><span class="sxs-lookup"><span data-stu-id="994a4-159">/api/products</span></span> | <span data-ttu-id="994a4-160">取得所有產品的清單。</span><span class="sxs-lookup"><span data-stu-id="994a4-160">Get a list of all products.</span></span> |
| <span data-ttu-id="994a4-161">/api/產品/*識別碼*</span><span class="sxs-lookup"><span data-stu-id="994a4-161">/api/products/*id*</span></span> | <span data-ttu-id="994a4-162">取得產品的識別碼。</span><span class="sxs-lookup"><span data-stu-id="994a4-162">Get a product by ID.</span></span> |
| <span data-ttu-id="994a4-163">/api/products/?category=*category*</span><span class="sxs-lookup"><span data-stu-id="994a4-163">/api/products/?category=*category*</span></span> | <span data-ttu-id="994a4-164">依類別取得產品的清單。</span><span class="sxs-lookup"><span data-stu-id="994a4-164">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="994a4-165">裝載 Web API</span><span class="sxs-lookup"><span data-stu-id="994a4-165">Host the Web API</span></span>

<span data-ttu-id="994a4-166">開啟 Program.cs 檔案並新增下列 using 陳述式：</span><span class="sxs-lookup"><span data-stu-id="994a4-166">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="994a4-167">將下列程式碼加入**程式**類別。</span><span class="sxs-lookup"><span data-stu-id="994a4-167">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="994a4-168">（選擇性）新增 HTTP URL 命名空間保留區</span><span class="sxs-lookup"><span data-stu-id="994a4-168">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="994a4-169">此應用程式接聽`http://localhost:8080/`。</span><span class="sxs-lookup"><span data-stu-id="994a4-169">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="994a4-170">根據預設，在特定的 HTTP 位址上進行接聽要求系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="994a4-170">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="994a4-171">當您執行本教學課程時，因此，您可能會收到此錯誤: 「 HTTP 無法登錄 URL http://+:8080/」 有兩種方式可避免這個錯誤：</span><span class="sxs-lookup"><span data-stu-id="994a4-171">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="994a4-172">提高權限的系統管理員權限，以執行 Visual Studio 或</span><span class="sxs-lookup"><span data-stu-id="994a4-172">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="994a4-173">您可以使用 Netsh.exe，讓您的帳戶權限，來保留 URL。</span><span class="sxs-lookup"><span data-stu-id="994a4-173">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="994a4-174">若要使用 Netsh.exe，以系統管理員權限開啟命令提示字元並輸入下列命令： 下列命令：</span><span class="sxs-lookup"><span data-stu-id="994a4-174">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="994a4-175">何處*machine\username*是您的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="994a4-175">where *machine\username* is your user account.</span></span>

<span data-ttu-id="994a4-176">當您完成自我裝載時，請務必刪除保留區：</span><span class="sxs-lookup"><span data-stu-id="994a4-176">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="994a4-177">呼叫 Web API，從用戶端應用程式 (C#)</span><span class="sxs-lookup"><span data-stu-id="994a4-177">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="994a4-178">讓我們撰寫會呼叫 web API 的簡單主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="994a4-178">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="994a4-179">將新的主控台應用程式專案加入方案：</span><span class="sxs-lookup"><span data-stu-id="994a4-179">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="994a4-180">在 [方案總管] 中，以滑鼠右鍵按一下方案，然後選取**加入新的專案**。</span><span class="sxs-lookup"><span data-stu-id="994a4-180">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="994a4-181">建立新的主控台應用程式，名為&quot;ClientApp&quot;。</span><span class="sxs-lookup"><span data-stu-id="994a4-181">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="994a4-182">使用 NuGet 套件管理員來新增 ASP.NET Web API 核心程式庫套件：</span><span class="sxs-lookup"><span data-stu-id="994a4-182">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="994a4-183">從 [工具] 功能表中，選取**程式庫套件管理員**。</span><span class="sxs-lookup"><span data-stu-id="994a4-183">From the Tools menu, select **Library Package Manager**.</span></span>
- <span data-ttu-id="994a4-184">選取**管理方案的 NuGet 套件...**</span><span class="sxs-lookup"><span data-stu-id="994a4-184">Select **Manage NuGet Packages for Solution...**</span></span>
- <span data-ttu-id="994a4-185">在 **管理 NuGet 套件**對話方塊中，選取**線上**。</span><span class="sxs-lookup"><span data-stu-id="994a4-185">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="994a4-186">在 [搜尋] 方塊中，輸入&quot;Microsoft.AspNet.WebApi.Client&quot;。</span><span class="sxs-lookup"><span data-stu-id="994a4-186">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="994a4-187">選取 Microsoft ASP.NET Web API 用戶端程式庫套件，然後按一下**安裝**。</span><span class="sxs-lookup"><span data-stu-id="994a4-187">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="994a4-188">在 ClientApp 加入 SelfHost 專案的參考：</span><span class="sxs-lookup"><span data-stu-id="994a4-188">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="994a4-189">在 [方案總管] 中，以滑鼠右鍵按一下 ClientApp 專案。</span><span class="sxs-lookup"><span data-stu-id="994a4-189">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="994a4-190">選取 [新增參考]。</span><span class="sxs-lookup"><span data-stu-id="994a4-190">Select **Add Reference**.</span></span>
- <span data-ttu-id="994a4-191">在 [**參考管理員**] 對話方塊底下**解決方案**，選取**專案**。</span><span class="sxs-lookup"><span data-stu-id="994a4-191">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="994a4-192">選取 SelfHost 專案。</span><span class="sxs-lookup"><span data-stu-id="994a4-192">Select the SelfHost project.</span></span>
- <span data-ttu-id="994a4-193">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="994a4-193">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="994a4-194">開啟 Client/Program.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="994a4-194">Open the Client/Program.cs file.</span></span> <span data-ttu-id="994a4-195">新增下列**使用**陳述式：</span><span class="sxs-lookup"><span data-stu-id="994a4-195">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="994a4-196">新增靜態**HttpClient**執行個體：</span><span class="sxs-lookup"><span data-stu-id="994a4-196">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="994a4-197">新增下列方法來依類別列出所有產品清單的識別碼、 產品和產品清單。</span><span class="sxs-lookup"><span data-stu-id="994a4-197">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="994a4-198">每一種方法都遵循相同的模式：</span><span class="sxs-lookup"><span data-stu-id="994a4-198">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="994a4-199">呼叫**HttpClient.GetAsync**將 GET 要求傳送至適當的 URI。</span><span class="sxs-lookup"><span data-stu-id="994a4-199">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="994a4-200">呼叫**HttpResponseMessage.EnsureSuccessStatusCode**。</span><span class="sxs-lookup"><span data-stu-id="994a4-200">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="994a4-201">如果 HTTP 回應狀態錯誤碼，則這個方法會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="994a4-201">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="994a4-202">呼叫**ReadAsAsync&lt;T&gt;** 還原序列化的 HTTP 回應中的 CLR 型別。</span><span class="sxs-lookup"><span data-stu-id="994a4-202">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="994a4-203">這個方法是擴充方法，定義於**System.Net.Http.HttpContentExtensions**。</span><span class="sxs-lookup"><span data-stu-id="994a4-203">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="994a4-204">**GetAsync**並**ReadAsAsync**方法為非同步。</span><span class="sxs-lookup"><span data-stu-id="994a4-204">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="994a4-205">它們會傳回**任務**代表非同步作業的物件。</span><span class="sxs-lookup"><span data-stu-id="994a4-205">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="994a4-206">取得**結果**屬性會封鎖執行緒直到作業完成為止。</span><span class="sxs-lookup"><span data-stu-id="994a4-206">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="994a4-207">如需有關使用 HttpClient，包括如何建立非封鎖式呼叫，請參閱 <<c0> [ 呼叫 Web API 從.NET 用戶端](../advanced/calling-a-web-api-from-a-net-client.md)。</span><span class="sxs-lookup"><span data-stu-id="994a4-207">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="994a4-208">之前呼叫這些方法，設定 HttpClient 執行個體上的 BaseAddress 屬性 「`http://localhost:8080`"。</span><span class="sxs-lookup"><span data-stu-id="994a4-208">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="994a4-209">例如: </span><span class="sxs-lookup"><span data-stu-id="994a4-209">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="994a4-210">這應輸出下列項目。</span><span class="sxs-lookup"><span data-stu-id="994a4-210">This should output the following.</span></span> <span data-ttu-id="994a4-211">（請記得先執行 SelfHost 應用程式。）</span><span class="sxs-lookup"><span data-stu-id="994a4-211">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
