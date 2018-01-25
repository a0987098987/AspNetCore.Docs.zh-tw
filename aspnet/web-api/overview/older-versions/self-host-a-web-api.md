---
uid: web-api/overview/older-versions/self-host-a-web-api
title: "自我裝載 ASP.NET Web API 1 (C#) |Microsoft 文件"
author: MikeWasson
description: "ASP.NET Web API 並不需要 IIS。 自我，您就可以在您自己的主控件程序裝載的 web API。 本教學課程會示範如何裝載 applic 主控台內的 web API..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2012
ms.topic: article
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 564f859e73a88ac9c5f27e9b8f7409ec126642f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="60ed0-105">自我裝載 ASP.NET Web API 1 (C#)</span><span class="sxs-lookup"><span data-stu-id="60ed0-105">Self-Host ASP.NET Web API 1 (C#)</span></span>
====================
<span data-ttu-id="60ed0-106">由[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="60ed0-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="60ed0-107">ASP.NET Web API 並不需要 IIS。</span><span class="sxs-lookup"><span data-stu-id="60ed0-107">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="60ed0-108">自我，您就可以在您自己的主控件程序裝載的 web API。</span><span class="sxs-lookup"><span data-stu-id="60ed0-108">You can self-host a web API in your own host process.</span></span> <span data-ttu-id="60ed0-109">本教學課程會示範如何裝載於主控台應用程式的 web API。</span><span class="sxs-lookup"><span data-stu-id="60ed0-109">This tutorial shows how to host a web API inside a console application.</span></span>
> 
> <span data-ttu-id="60ed0-110">**新的應用程式應該使用 OWIN 自我裝載的 Web API。**</span><span class="sxs-lookup"><span data-stu-id="60ed0-110">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="60ed0-111">請參閱[使用 OWIN 自我裝載 ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)。</span><span class="sxs-lookup"><span data-stu-id="60ed0-111">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="60ed0-112">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="60ed0-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="60ed0-113">Web API 1</span><span class="sxs-lookup"><span data-stu-id="60ed0-113">Web API 1</span></span>
> - <span data-ttu-id="60ed0-114">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="60ed0-114">Visual Studio 2012</span></span>


## <a name="create-the-console-application-project"></a><span data-ttu-id="60ed0-115">建立主控台應用程式專案</span><span class="sxs-lookup"><span data-stu-id="60ed0-115">Create the Console Application Project</span></span>

<span data-ttu-id="60ed0-116">啟動 Visual Studio，然後選取**新專案**從**啟動**頁面。</span><span class="sxs-lookup"><span data-stu-id="60ed0-116">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="60ed0-117">或從**檔案**功能表上，選取**新增**然後**專案**。</span><span class="sxs-lookup"><span data-stu-id="60ed0-117">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="60ed0-118">在**範本**窗格中，選取**已安裝的範本**展開**Visual C#**節點。</span><span class="sxs-lookup"><span data-stu-id="60ed0-118">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="60ed0-119">在下**Visual C#**，選取**Windows**。</span><span class="sxs-lookup"><span data-stu-id="60ed0-119">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="60ed0-120">在專案範本清單中選取**主控台應用程式**。</span><span class="sxs-lookup"><span data-stu-id="60ed0-120">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="60ed0-121">將專案命名&quot;SelfHost&quot;按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="60ed0-121">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="60ed0-122">設定目標架構 (Visual Studio 2010)</span><span class="sxs-lookup"><span data-stu-id="60ed0-122">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="60ed0-123">如果您使用 Visual Studio 2010，將目標 framework 變更為.NET Framework 4.0。</span><span class="sxs-lookup"><span data-stu-id="60ed0-123">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="60ed0-124">(根據預設，專案範本的目標[.Net Framework 用戶端設定檔](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile)。)</span><span class="sxs-lookup"><span data-stu-id="60ed0-124">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="60ed0-125">在 [方案總管] 中，以滑鼠右鍵按一下專案，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="60ed0-125">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="60ed0-126">在**目標 framework**下拉式清單中，將目標 framework 變更為.NET Framework 4.0。</span><span class="sxs-lookup"><span data-stu-id="60ed0-126">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="60ed0-127">當出現提示，以套用變更，請按一下 **是**。</span><span class="sxs-lookup"><span data-stu-id="60ed0-127">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="60ed0-128">安裝 NuGet 封裝管理員</span><span class="sxs-lookup"><span data-stu-id="60ed0-128">Install NuGet Package Manager</span></span>

<span data-ttu-id="60ed0-129">NuGet 封裝管理員是 Web 應用程式開發介面的組件加入至非 ASP.NET 專案最簡單的方式。</span><span class="sxs-lookup"><span data-stu-id="60ed0-129">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="60ed0-130">若要檢查是否已安裝的 NuGet 封裝管理員，請按一下**工具**Visual Studio 中的功能表。</span><span class="sxs-lookup"><span data-stu-id="60ed0-130">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="60ed0-131">如果您看到的功能表項目稱為**程式庫套件管理員**，就會有 NuGet 套件管理員。</span><span class="sxs-lookup"><span data-stu-id="60ed0-131">If you see a menu item called **Library Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="60ed0-132">若要安裝 NuGet 封裝管理員：</span><span class="sxs-lookup"><span data-stu-id="60ed0-132">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="60ed0-133">啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="60ed0-133">Start Visual Studio.</span></span>
2. <span data-ttu-id="60ed0-134">從**工具**功能表上，選取**擴充功能和更新**。</span><span class="sxs-lookup"><span data-stu-id="60ed0-134">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="60ed0-135">在**擴充功能和更新**對話方塊中，選取**線上**。</span><span class="sxs-lookup"><span data-stu-id="60ed0-135">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="60ed0-136">如果看不到 」 NuGet 封裝管理員 」，請在搜尋方塊中輸入"nuget 封裝管理員 」。</span><span class="sxs-lookup"><span data-stu-id="60ed0-136">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="60ed0-137">選取 NuGet 封裝管理員，然後按一下**下載**。</span><span class="sxs-lookup"><span data-stu-id="60ed0-137">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="60ed0-138">下載完成後，系統會提示您安裝。</span><span class="sxs-lookup"><span data-stu-id="60ed0-138">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="60ed0-139">在安裝完成之後，您可能會提示您重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="60ed0-139">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="60ed0-140">加入 Web API NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="60ed0-140">Add the Web API NuGet Package</span></span>

<span data-ttu-id="60ed0-141">安裝 NuGet 套件管理員之後，將 Web 應用程式開發介面 Self-Host 封裝加入您的專案。</span><span class="sxs-lookup"><span data-stu-id="60ed0-141">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="60ed0-142">從**工具**功能表上，選取**程式庫套件管理員**。</span><span class="sxs-lookup"><span data-stu-id="60ed0-142">From the **Tools** menu, select **Library Package Manager**.</span></span> <span data-ttu-id="60ed0-143">*請注意*： 如果您未看見這個功能表項目，請確定已正確安裝的 NuGet 套件管理員。</span><span class="sxs-lookup"><span data-stu-id="60ed0-143">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="60ed0-144">選取**管理方案的 NuGet 封裝...**</span><span class="sxs-lookup"><span data-stu-id="60ed0-144">Select **Manage NuGet Packages for Solution...**</span></span>
3. <span data-ttu-id="60ed0-145">在**管理 NugGet 封裝**對話方塊中，選取**線上**。</span><span class="sxs-lookup"><span data-stu-id="60ed0-145">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="60ed0-146">在 [搜尋] 方塊中，輸入&quot;Microsoft.AspNet.WebApi.SelfHost&quot;。</span><span class="sxs-lookup"><span data-stu-id="60ed0-146">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="60ed0-147">選取 ASP.NET Web API Self Host 封裝，然後按一下**安裝**。</span><span class="sxs-lookup"><span data-stu-id="60ed0-147">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="60ed0-148">此封裝會安裝之後，請按一下**關閉**以關閉對話方塊。</span><span class="sxs-lookup"><span data-stu-id="60ed0-148">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="60ed0-149">請務必安裝名為 Microsoft.AspNet.WebApi.SelfHost，不 AspNetWebApi.SelfHost 的套件。</span><span class="sxs-lookup"><span data-stu-id="60ed0-149">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="60ed0-150">建立模型和控制器</span><span class="sxs-lookup"><span data-stu-id="60ed0-150">Create the Model and Controller</span></span>

<span data-ttu-id="60ed0-151">本教學課程使用相同的模型和控制器類別做為[入門](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="60ed0-151">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="60ed0-152">加入名為公用類別`Product`。</span><span class="sxs-lookup"><span data-stu-id="60ed0-152">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="60ed0-153">加入名為公用類別`ProductsController`。</span><span class="sxs-lookup"><span data-stu-id="60ed0-153">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="60ed0-154">衍生此類別從**System.Web.Http.ApiController**。</span><span class="sxs-lookup"><span data-stu-id="60ed0-154">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="60ed0-155">如需此控制器中的程式碼的詳細資訊，請參閱[入門](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="60ed0-155">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="60ed0-156">此控制器會定義三個 GET 動作：</span><span class="sxs-lookup"><span data-stu-id="60ed0-156">This controller defines three GET actions:</span></span>

| <span data-ttu-id="60ed0-157">URI</span><span class="sxs-lookup"><span data-stu-id="60ed0-157">URI</span></span> | <span data-ttu-id="60ed0-158">描述</span><span class="sxs-lookup"><span data-stu-id="60ed0-158">Description</span></span> |
| --- | --- |
| <span data-ttu-id="60ed0-159">/ api/產品</span><span class="sxs-lookup"><span data-stu-id="60ed0-159">/api/products</span></span> | <span data-ttu-id="60ed0-160">取得所有產品的清單。</span><span class="sxs-lookup"><span data-stu-id="60ed0-160">Get a list of all products.</span></span> |
| <span data-ttu-id="60ed0-161">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="60ed0-161">/api/products/*id*</span></span> | <span data-ttu-id="60ed0-162">取得產品的識別碼。</span><span class="sxs-lookup"><span data-stu-id="60ed0-162">Get a product by ID.</span></span> |
| <span data-ttu-id="60ed0-163">/api/products/?category=*category*</span><span class="sxs-lookup"><span data-stu-id="60ed0-163">/api/products/?category=*category*</span></span> | <span data-ttu-id="60ed0-164">取得類別目錄的產品清單。</span><span class="sxs-lookup"><span data-stu-id="60ed0-164">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="60ed0-165">裝載 Web 應用程式開發介面</span><span class="sxs-lookup"><span data-stu-id="60ed0-165">Host the Web API</span></span>

<span data-ttu-id="60ed0-166">開啟 Program.cs 檔案並加入下列 using 陳述式：</span><span class="sxs-lookup"><span data-stu-id="60ed0-166">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="60ed0-167">將下列程式碼加入**程式**類別。</span><span class="sxs-lookup"><span data-stu-id="60ed0-167">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="60ed0-168">（選擇性）新增 HTTP URL 命名空間保留區</span><span class="sxs-lookup"><span data-stu-id="60ed0-168">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="60ed0-169">此應用程式接聽`http://localhost:8080/`。</span><span class="sxs-lookup"><span data-stu-id="60ed0-169">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="60ed0-170">根據預設，在特定的 HTTP 位址的接聽要求系統管理員權限。</span><span class="sxs-lookup"><span data-stu-id="60ed0-170">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="60ed0-171">當您執行本教學課程時，因此，您可能會收到這個錯誤: 「 HTTP 無法登錄 URL http://+:8080/"有兩種方式以避免此錯誤：</span><span class="sxs-lookup"><span data-stu-id="60ed0-171">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="60ed0-172">以提升的系統管理員權限，執行 Visual Studio 或</span><span class="sxs-lookup"><span data-stu-id="60ed0-172">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="60ed0-173">使用 Netsh.exe 給予您的帳戶權限，以保留 URL。</span><span class="sxs-lookup"><span data-stu-id="60ed0-173">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="60ed0-174">若要使用 Netsh.exe，以系統管理員權限開啟命令提示字元並輸入下列命令： 下列命令：</span><span class="sxs-lookup"><span data-stu-id="60ed0-174">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="60ed0-175">其中*machine\username*是您的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="60ed0-175">where *machine\username* is your user account.</span></span>

<span data-ttu-id="60ed0-176">當您完成的自我裝載的時請務必刪除保留區：</span><span class="sxs-lookup"><span data-stu-id="60ed0-176">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="60ed0-177">呼叫 Web API，從用戶端應用程式 (C#)</span><span class="sxs-lookup"><span data-stu-id="60ed0-177">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="60ed0-178">我們要撰寫簡單的主控台應用程式呼叫 web API。</span><span class="sxs-lookup"><span data-stu-id="60ed0-178">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="60ed0-179">將新的主控台應用程式專案加入方案：</span><span class="sxs-lookup"><span data-stu-id="60ed0-179">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="60ed0-180">在 [方案總管] 中，以滑鼠右鍵按一下方案，然後選取**加入新的專案**。</span><span class="sxs-lookup"><span data-stu-id="60ed0-180">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="60ed0-181">建立新的主控台應用程式，名為&quot;ClientApp&quot;。</span><span class="sxs-lookup"><span data-stu-id="60ed0-181">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="60ed0-182">使用 NuGet 套件管理員來新增 ASP.NET Web API Core Libraries 封裝：</span><span class="sxs-lookup"><span data-stu-id="60ed0-182">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="60ed0-183">從 [工具] 功能表中，選取**程式庫套件管理員**。</span><span class="sxs-lookup"><span data-stu-id="60ed0-183">From the Tools menu, select **Library Package Manager**.</span></span>
- <span data-ttu-id="60ed0-184">選取**管理方案的 NuGet 封裝...**</span><span class="sxs-lookup"><span data-stu-id="60ed0-184">Select **Manage NuGet Packages for Solution...**</span></span>
- <span data-ttu-id="60ed0-185">在**管理 NuGet 封裝**對話方塊中，選取**線上**。</span><span class="sxs-lookup"><span data-stu-id="60ed0-185">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="60ed0-186">在 [搜尋] 方塊中，輸入&quot;Microsoft.AspNet.WebApi.Client&quot;。</span><span class="sxs-lookup"><span data-stu-id="60ed0-186">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="60ed0-187">選取 Microsoft ASP.NET Web API 用戶端程式庫封裝，然後按一下**安裝**。</span><span class="sxs-lookup"><span data-stu-id="60ed0-187">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="60ed0-188">ClientApp 中加入 SelfHost 專案的參考：</span><span class="sxs-lookup"><span data-stu-id="60ed0-188">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="60ed0-189">在 [方案總管] 中，以滑鼠右鍵按一下 ClientApp 專案。</span><span class="sxs-lookup"><span data-stu-id="60ed0-189">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="60ed0-190">選取**將參考加入**。</span><span class="sxs-lookup"><span data-stu-id="60ed0-190">Select **Add Reference**.</span></span>
- <span data-ttu-id="60ed0-191">在**參考管理員**對話方塊下方**方案**，選取**專案**。</span><span class="sxs-lookup"><span data-stu-id="60ed0-191">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="60ed0-192">選取 SelfHost 專案。</span><span class="sxs-lookup"><span data-stu-id="60ed0-192">Select the SelfHost project.</span></span>
- <span data-ttu-id="60ed0-193">按一下 [確定 **Deploying Office Solutions**]。</span><span class="sxs-lookup"><span data-stu-id="60ed0-193">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="60ed0-194">開啟 Client/Program.cs 檔案。</span><span class="sxs-lookup"><span data-stu-id="60ed0-194">Open the Client/Program.cs file.</span></span> <span data-ttu-id="60ed0-195">加入下列**使用**陳述式：</span><span class="sxs-lookup"><span data-stu-id="60ed0-195">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="60ed0-196">新增靜態**HttpClient**執行個體：</span><span class="sxs-lookup"><span data-stu-id="60ed0-196">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="60ed0-197">加入下列方法，以依類別列出所有產品清單中依識別碼、 產品和產品清單。</span><span class="sxs-lookup"><span data-stu-id="60ed0-197">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="60ed0-198">每一種方法遵循相同的模式：</span><span class="sxs-lookup"><span data-stu-id="60ed0-198">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="60ed0-199">呼叫**HttpClient.GetAsync**將 GET 要求傳送到適當的 URI。</span><span class="sxs-lookup"><span data-stu-id="60ed0-199">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="60ed0-200">呼叫**HttpResponseMessage.EnsureSuccessStatusCode**。</span><span class="sxs-lookup"><span data-stu-id="60ed0-200">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="60ed0-201">如果 HTTP 回應狀態錯誤程式碼，則這個方法會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="60ed0-201">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="60ed0-202">呼叫**ReadAsAsync&lt;T&gt;** 還原序列化之 HTTP 回應中的 CLR 型別。</span><span class="sxs-lookup"><span data-stu-id="60ed0-202">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="60ed0-203">這個方法是擴充方法，定義於**System.Net.Http.HttpContentExtensions**。</span><span class="sxs-lookup"><span data-stu-id="60ed0-203">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="60ed0-204">**GetAsync**和**ReadAsAsync**方法都是非同步。</span><span class="sxs-lookup"><span data-stu-id="60ed0-204">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="60ed0-205">它們會傳回**工作**代表非同步作業的物件。</span><span class="sxs-lookup"><span data-stu-id="60ed0-205">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="60ed0-206">取得**結果**屬性會封鎖執行緒，直到作業完成為止。</span><span class="sxs-lookup"><span data-stu-id="60ed0-206">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="60ed0-207">如需有關使用 HttpClient，包括如何讓非封鎖的呼叫，請參閱[呼叫 Web API 的.NET 用戶端](../advanced/calling-a-web-api-from-a-net-client.md)。</span><span class="sxs-lookup"><span data-stu-id="60ed0-207">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="60ed0-208">然後再呼叫這些方法，設定 BaseAddress 屬性 HttpClient 執行個體上 「`http://localhost:8080`"。</span><span class="sxs-lookup"><span data-stu-id="60ed0-208">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="60ed0-209">例如: </span><span class="sxs-lookup"><span data-stu-id="60ed0-209">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="60ed0-210">這應該輸出。</span><span class="sxs-lookup"><span data-stu-id="60ed0-210">This should output the following.</span></span> <span data-ttu-id="60ed0-211">（請記住第一次執行 SelfHost 應用程式）。</span><span class="sxs-lookup"><span data-stu-id="60ed0-211">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
