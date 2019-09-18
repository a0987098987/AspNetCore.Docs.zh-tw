---
title: 使用 Visual Studio Code 將 ASP.NET Core 應用程式發佈至 Azure
author: ricardoserradas
description: 了解如何使用 Visual Studio Code 將 ASP.NET Core 應用程式發佈至 Azure App Service
ms.author: riserrad
ms.custom: mvc
ms.date: 07/10/2019
uid: tutorials/publish-to-azure-webapp-using-vscode
ms.openlocfilehash: a5d92775d6245494c34bfe691d7ade663b2078d5
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082399"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-visual-studio-code"></a><span data-ttu-id="fa07a-103">使用 Visual Studio Code 將 ASP.NET Core 應用程式發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="fa07a-103">Publish an ASP.NET Core app to Azure with Visual Studio Code</span></span>

<span data-ttu-id="fa07a-104">作者：[Ricardo Serradas](https://twitter.com/ricardoserradas)</span><span class="sxs-lookup"><span data-stu-id="fa07a-104">By [Ricardo Serradas](https://twitter.com/ricardoserradas)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="fa07a-105">若要針對 App Service 部署問題進行疑難排解，請參閱 <xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="fa07a-105">To troubleshoot an App Service deployment issue, see <xref:test/troubleshoot-azure-iis>.</span></span>

## <a name="intro"></a><span data-ttu-id="fa07a-106">簡介</span><span class="sxs-lookup"><span data-stu-id="fa07a-106">Intro</span></span>

<span data-ttu-id="fa07a-107">在本教學課程中，您將了解如何建立 ASP.Net Core MVC 應用程式，並將其部署於 Visual Studio Code。</span><span class="sxs-lookup"><span data-stu-id="fa07a-107">With this tutorial, you'll learn how to create an ASP.Net Core MVC Application and deploy it within Visual Studio Code.</span></span>

## <a name="set-up"></a><span data-ttu-id="fa07a-108">設定</span><span class="sxs-lookup"><span data-stu-id="fa07a-108">Set up</span></span>

- <span data-ttu-id="fa07a-109">如果您沒有帳戶，請開啟[免費的 Azure 帳戶](https://azure.microsoft.com/free/dotnet/)。</span><span class="sxs-lookup"><span data-stu-id="fa07a-109">Open a [free Azure account](https://azure.microsoft.com/free/dotnet/) if you don't have one.</span></span>
- <span data-ttu-id="fa07a-110">安裝 [.NET Core SDK](https://dotnet.microsoft.com/download)</span><span class="sxs-lookup"><span data-stu-id="fa07a-110">Install [.NET Core SDK](https://dotnet.microsoft.com/download)</span></span>
- <span data-ttu-id="fa07a-111">安裝 [Visual Studio Code](https://code.visualstudio.com/Download) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="fa07a-111">Install [Visual Studio Code](https://code.visualstudio.com/Download)</span></span>
  - <span data-ttu-id="fa07a-112">將 [C# 延伸模組](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)安裝至 Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fa07a-112">Install the [C# Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) to Visual Studio Code</span></span>
  - <span data-ttu-id="fa07a-113">將 [Azure App Service 延伸模組](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)安裝至 Visual Studio Code 並加以設定後再繼續</span><span class="sxs-lookup"><span data-stu-id="fa07a-113">Instal the [Azure App Service Extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice) to Visual Studio Code and configure it before proceeding</span></span>

## <a name="create-an-aspnet-core-mvc-project"></a><span data-ttu-id="fa07a-114">建立 ASP.NET Core MVC 專案</span><span class="sxs-lookup"><span data-stu-id="fa07a-114">Create an ASP.Net Core MVC project</span></span>

<span data-ttu-id="fa07a-115">使用終端機巡覽至您希望在其上建立專案的資料夾，並使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="fa07a-115">Using a terminal, navigate to the folder you want the project to be created on and use the following command:</span></span>

```dotnetcli
dotnet new mvc
```

<span data-ttu-id="fa07a-116">您會擁有類似如下所示的資料夾結構：</span><span class="sxs-lookup"><span data-stu-id="fa07a-116">You'll have a folder structure similar to the following:</span></span>

```cmd
      appsettings.Development.json
      appsettings.json
<DIR> Controllers
<DIR> Models
      netcore-vscode.csproj
<DIR> obj
      Program.cs
<DIR> Properties
      Startup.cs
<DIR> Views
<DIR> wwwroot
```

## <a name="open-it-with-visual-studio-code"></a><span data-ttu-id="fa07a-117">使用 Visual Studio Code 加以開啟</span><span class="sxs-lookup"><span data-stu-id="fa07a-117">Open it with Visual Studio Code</span></span>

<span data-ttu-id="fa07a-118">建立專案後，您可以使用下列其中一個選項來以 Visual Studio Code 將其開啟：</span><span class="sxs-lookup"><span data-stu-id="fa07a-118">After your project is created, you can open it with Visual Studio Code by using one of the options below:</span></span>

### <a name="through-the-command-line"></a><span data-ttu-id="fa07a-119">透過命令列</span><span class="sxs-lookup"><span data-stu-id="fa07a-119">Through the command line</span></span>

<span data-ttu-id="fa07a-120">在您建立專案的資料夾中使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="fa07a-120">Use the following command within the folder you created the project:</span></span>

```cmd
> code .
```

<span data-ttu-id="fa07a-121">如果下列命令無法運作，請確認您的安裝是否已參考[此連結](https://code.visualstudio.com/docs/setup/setup-overview#_cross-platform)來正確設定。</span><span class="sxs-lookup"><span data-stu-id="fa07a-121">If the command below does not work, check if your installation is configured properly by referencing [this link](https://code.visualstudio.com/docs/setup/setup-overview#_cross-platform).</span></span>

### <a name="through-visual-studio-code-interface"></a><span data-ttu-id="fa07a-122">透過 Visual Studio Code 介面</span><span class="sxs-lookup"><span data-stu-id="fa07a-122">Through Visual Studio Code interface</span></span>

- <span data-ttu-id="fa07a-123">開啟 Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="fa07a-123">Open Visual Studio Code</span></span>
- <span data-ttu-id="fa07a-124">在功能表中，選取 `File > Open Folder`</span><span class="sxs-lookup"><span data-stu-id="fa07a-124">On the menu, select `File > Open Folder`</span></span>
- <span data-ttu-id="fa07a-125">選取您建立 MVC 專案的資料夾根目錄</span><span class="sxs-lookup"><span data-stu-id="fa07a-125">Select the root of the folder you created the MVC Project</span></span>

<span data-ttu-id="fa07a-126">開啟專案資料夾時，您會收到訊息指出遺失用於建置與偵錯的必要資產。</span><span class="sxs-lookup"><span data-stu-id="fa07a-126">When you open the project folder, you'll receive a message saying that required assets to build and debug are missing.</span></span> <span data-ttu-id="fa07a-127">接受協助來予以新增。</span><span class="sxs-lookup"><span data-stu-id="fa07a-127">Accept the help to add them.</span></span>

![已載入專案的 Visual Studio Code 介面](publish-to-azure-webapp-using-vscode/_static/folder-structure-restore-netcore.jpg)

<span data-ttu-id="fa07a-129">`.vscode` 資料夾將建立於專案結構下方。</span><span class="sxs-lookup"><span data-stu-id="fa07a-129">A `.vscode` folder will be created under the project structure.</span></span> <span data-ttu-id="fa07a-130">該資料夾會包含下列檔案：</span><span class="sxs-lookup"><span data-stu-id="fa07a-130">It will contain the following files:</span></span>

```cmd
launch.json
tasks.json
```

<span data-ttu-id="fa07a-131">這些是可協助您建置與偵錯 .NET Core Web 應用程式的公用程式檔案。</span><span class="sxs-lookup"><span data-stu-id="fa07a-131">These are utility files to help you build and debug your .NET Core Web App.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="fa07a-132">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="fa07a-132">Run the app</span></span>

<span data-ttu-id="fa07a-133">在將應用程式部署至 Azure 之前，請確定其可在本機電腦上正常執行。</span><span class="sxs-lookup"><span data-stu-id="fa07a-133">Before we deploy the app to Azure, make sure it is running properly on your local machine.</span></span>

- <span data-ttu-id="fa07a-134">按 F5 執行專案</span><span class="sxs-lookup"><span data-stu-id="fa07a-134">Press F5 to run the project</span></span>

<span data-ttu-id="fa07a-135">您的 Web 應用程式會在預設瀏覽器的新索引標籤上開始執行。</span><span class="sxs-lookup"><span data-stu-id="fa07a-135">Your web app will start running on a new tab of your default browser.</span></span> <span data-ttu-id="fa07a-136">啟動時您可能會看到隱私警告。</span><span class="sxs-lookup"><span data-stu-id="fa07a-136">You may notice a privacy warning as soon as it starts.</span></span> <span data-ttu-id="fa07a-137">這是因為您的應用程式會使用 HTTP 或 HTTPS 啟動，且會根據預設巡覽至 HTTPS 端點。</span><span class="sxs-lookup"><span data-stu-id="fa07a-137">This is because your app will start either using HTTP and HTTPS, and it navigates to the HTTPS endpoint by default.</span></span>

![在本機對應用程式偵錯時的隱私警告](publish-to-azure-webapp-using-vscode/_static/run-webapp-https-warning.jpg)

<span data-ttu-id="fa07a-139">若要保留偵錯工作階段，請按一下 `Advanced`，然後 `Continue to localhost (unsafe)`。</span><span class="sxs-lookup"><span data-stu-id="fa07a-139">To keep the debugging session, click `Advanced` and then `Continue to localhost (unsafe)`.</span></span>

## <a name="generate-the-deployment-package-locally"></a><span data-ttu-id="fa07a-140">在本機產生部署套件</span><span class="sxs-lookup"><span data-stu-id="fa07a-140">Generate the deployment package locally</span></span>

- <span data-ttu-id="fa07a-141">開啟 Visual Studio Code 終端機</span><span class="sxs-lookup"><span data-stu-id="fa07a-141">Open Visual Studio Code terminal</span></span>
- <span data-ttu-id="fa07a-142">使用下列命令，在稱為 `publish` 的子資料夾中產生 `Release` 套件：</span><span class="sxs-lookup"><span data-stu-id="fa07a-142">Use the following command to generate a `Release` package to a sub folder called `publish`:</span></span>
  - `dotnet publish -c Release -o ./publish`
- <span data-ttu-id="fa07a-143">新的 `publish` 資料夾將建立於專案結構下方</span><span class="sxs-lookup"><span data-stu-id="fa07a-143">A new `publish` folder will be created under the project structure</span></span>

![發佈資料夾結構](publish-to-azure-webapp-using-vscode/_static/publish-folder.jpg)

## <a name="publish-to-azure-app-service"></a><span data-ttu-id="fa07a-145">發佈至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="fa07a-145">Publish to Azure App Service</span></span>

<span data-ttu-id="fa07a-146">運用適用於 Visual Studio Code 的 Azure App Service 延伸模組，遵循下列步驟將該網站直接發佈至 Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="fa07a-146">Leveraging the Azure App Service extension for Visual Studio Code, follow the steps below to publish the website directly to the Azure App Service.</span></span>

### <a name="if-youre-creating-a-new-web-app"></a><span data-ttu-id="fa07a-147">您要建立新的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="fa07a-147">If you're creating a new Web App</span></span>

- <span data-ttu-id="fa07a-148">以右鍵按一下 `publish` 資料夾並選取 `Deploy to Web App...`</span><span class="sxs-lookup"><span data-stu-id="fa07a-148">Right click the `publish` folder and select `Deploy to Web App...`</span></span>
- <span data-ttu-id="fa07a-149">選取您希望建立 Web 應用程式的訂用帳戶</span><span class="sxs-lookup"><span data-stu-id="fa07a-149">Select the subscription you want to create the Web App</span></span>
- <span data-ttu-id="fa07a-150">選取 `Create New Web App`</span><span class="sxs-lookup"><span data-stu-id="fa07a-150">Select `Create New Web App`</span></span>
- <span data-ttu-id="fa07a-151">輸入 Web 應用程式的名稱</span><span class="sxs-lookup"><span data-stu-id="fa07a-151">Enter a name for the Web App</span></span>

<span data-ttu-id="fa07a-152">延伸模組將會建立新的 Web 應用程式，並會自動開始將套件部署至該應用程式。</span><span class="sxs-lookup"><span data-stu-id="fa07a-152">The extension will create the new Web App and will automatically start deploying the package to it.</span></span> <span data-ttu-id="fa07a-153">部署完成後，按一下 `Browse Website` 以驗證部署。</span><span class="sxs-lookup"><span data-stu-id="fa07a-153">Once the deployment is finished, click `Browse Website` to validate the deployment.</span></span>

![部署成功訊息](publish-to-azure-webapp-using-vscode/_static/deployment-succeeded-message.jpg)

<span data-ttu-id="fa07a-155">您按一下 `Browse Website` 後，您會使用預設的瀏覽器來巡覽：</span><span class="sxs-lookup"><span data-stu-id="fa07a-155">Once you click `Browse Website`, you'll navigate to it using your default browser:</span></span>

![已成功部署新 Web 應用程式](publish-to-azure-webapp-using-vscode/_static/new-webapp-deployed.jpg)

### <a name="if-youre-deploying-to-an-existing-web-app"></a><span data-ttu-id="fa07a-157">如果您要部署至現有的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="fa07a-157">If you're deploying to an existing Web App</span></span>

- <span data-ttu-id="fa07a-158">以右鍵按一下 `publish` 資料夾並選取 `Deploy to Web App...`</span><span class="sxs-lookup"><span data-stu-id="fa07a-158">Right click the `publish` folder and select `Deploy to Web App...`</span></span>
- <span data-ttu-id="fa07a-159">選取現有 Web 應用程式所在的訂用帳戶</span><span class="sxs-lookup"><span data-stu-id="fa07a-159">Select the subscription the existing Web App resides</span></span>
- <span data-ttu-id="fa07a-160">從清單中選取該 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="fa07a-160">Select the Web App from the list</span></span>
- <span data-ttu-id="fa07a-161">Visual Studio Code 會詢問您是否要覆寫現有的內容。</span><span class="sxs-lookup"><span data-stu-id="fa07a-161">Visual Studio Code will ask you if you want to overwrite the existing content.</span></span> <span data-ttu-id="fa07a-162">按一下 `Deploy` 以確認</span><span class="sxs-lookup"><span data-stu-id="fa07a-162">Click `Deploy` to confirm</span></span>

<span data-ttu-id="fa07a-163">延伸模組會將更新的內容部署至該 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="fa07a-163">The extension will deploy the updated content to the Web App.</span></span> <span data-ttu-id="fa07a-164">完成後，按一下 `Browse Website` 以驗證部署。</span><span class="sxs-lookup"><span data-stu-id="fa07a-164">Once it's done, click `Browse Website` to validate the deployment.</span></span>

![已成功部署現有 Web 應用程式](publish-to-azure-webapp-using-vscode/_static/existing-webapp-deployed.jpg)

## <a name="next-steps"></a><span data-ttu-id="fa07a-166">後續步驟</span><span class="sxs-lookup"><span data-stu-id="fa07a-166">Next steps</span></span>

- [<span data-ttu-id="fa07a-167">建立您的第一個 Azure DevOps 管線</span><span class="sxs-lookup"><span data-stu-id="fa07a-167">Create your first Azure DevOps pipeline</span></span>](/azure/devops/pipelines/create-first-pipeline)

## <a name="additional-resources"></a><span data-ttu-id="fa07a-168">其他資源</span><span class="sxs-lookup"><span data-stu-id="fa07a-168">Additional resources</span></span>

- [<span data-ttu-id="fa07a-169">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="fa07a-169">Azure App Service</span></span>](/azure/app-service/app-service-web-overview)
- [<span data-ttu-id="fa07a-170">Azure 資源群組</span><span class="sxs-lookup"><span data-stu-id="fa07a-170">Azure resource groups</span></span>](/azure/azure-resource-manager/resource-group-overview#resource-groups)