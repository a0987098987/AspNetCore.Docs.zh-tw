---
title: 使用命令列工具將 ASP.NET Core 應用程式發行到 Azure
author: camsoper
description: 了解如何使用 Git 命令列用戶端將 ASP.NET Core 應用程式發行到 Azure App Service。
manager: wpickett
ms.author: casoper
ms.custom: mvc
ms.date: 11/03/2017
ms.devlang: dotnet
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
services: multiple
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 3fc068096a4b8696340787aa15120a2f97d10164
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252434"
---
# <a name="publish-an-aspnet-core-app-to-azure-with-command-line-tools"></a><span data-ttu-id="b726c-103">使用命令列工具將 ASP.NET Core 應用程式發行到 Azure</span><span class="sxs-lookup"><span data-stu-id="b726c-103">Publish an ASP.NET Core app to Azure with command line tools</span></span>

<span data-ttu-id="b726c-104">作者 [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="b726c-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

[!INCLUDE [Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

<span data-ttu-id="b726c-105">本教學課程將說明如何使用命令列工具來建置 ASP.NET Core 應用程式，並將其部署至 Microsoft Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="b726c-105">This tutorial will show you how to build and deploy an ASP.NET Core app to Microsoft Azure App Service using command line tools.</span></span> <span data-ttu-id="b726c-106">完成之後，您將會擁有建置在 ASP.NET Core 中且裝載為 Azure App Service Web 應用程式的 Razor Pages Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b726c-106">When finished, you'll have a Razor Pages web app built in ASP.NET Core hosted as an Azure App Service Web App.</span></span> <span data-ttu-id="b726c-107">本教學課程是使用 Windows 命令列工具來撰寫，但也適用於 macOS 和 Linux 環境。</span><span class="sxs-lookup"><span data-stu-id="b726c-107">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>

<span data-ttu-id="b726c-108">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="b726c-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b726c-109">使用 Azure CLI 來建立 Azure App Service 網站</span><span class="sxs-lookup"><span data-stu-id="b726c-109">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="b726c-110">使用 Git 命令列工具將 ASP.NET Core 應用程式部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b726c-110">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b726c-111">必要條件</span><span class="sxs-lookup"><span data-stu-id="b726c-111">Prerequisites</span></span>

<span data-ttu-id="b726c-112">若要完成此課程，您會需要：</span><span class="sxs-lookup"><span data-stu-id="b726c-112">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="b726c-113">[Microsoft Azure 訂用帳戶](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="b726c-113">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]
* <span data-ttu-id="b726c-114">[Git](https://www.git-scm.com/) 命令列用戶端</span><span class="sxs-lookup"><span data-stu-id="b726c-114">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-app"></a><span data-ttu-id="b726c-115">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="b726c-115">Create a web app</span></span>

<span data-ttu-id="b726c-116">為 Web 應用程式建立新的目錄、建立新的 ASP.NET Core Razor Pages 應用程式，然後在本機執行該網站。</span><span class="sxs-lookup"><span data-stu-id="b726c-116">Create a new directory for the web app, create a new ASP.NET Core Razor Pages app, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b726c-117">Windows</span><span class="sxs-lookup"><span data-stu-id="b726c-117">Windows</span></span>](#tab/windows)

::: moniker range=">= aspnetcore-2.1"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
REM Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the app
dotnet run
```

::: moniker-end

# <a name="othertabother"></a>[<span data-ttu-id="b726c-119">其他</span><span class="sxs-lookup"><span data-stu-id="b726c-119">Other</span></span>](#tab/other)

::: moniker range=">= aspnetcore-2.1"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new webapp -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```bash
# Create a new ASP.NET Core Razor Pages app
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the app
dotnet run
```

::: moniker-end

---

![命令列輸出](publish-to-azure-webapp-using-cli/_static/new_prj.png)

<span data-ttu-id="b726c-122">瀏覽至 `http://localhost:5000` 來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="b726c-122">Test the app by browsing to `http://localhost:5000`.</span></span>

![網站在本機上執行](publish-to-azure-webapp-using-cli/_static/app_test.png)

## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="b726c-124">建立 Azure App Service 執行個體</span><span class="sxs-lookup"><span data-stu-id="b726c-124">Create the Azure App Service instance</span></span>

<span data-ttu-id="b726c-125">使用 [Azure Cloud Shell](/azure/cloud-shell/quickstart)，建立資源群組、App Service 方案，以及 App Service Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b726c-125">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

```azurecli-interactive
# Generate a unique Web App name
let randomNum=$RANDOM*$RANDOM
webappname=tutorialApp$randomNum

# Create the DotNetAzureTutorial resource group
az group create --name DotNetAzureTutorial --location EastUS

# Create an App Service plan.
az appservice plan create --name $webappname --resource-group DotNetAzureTutorial --sku FREE

# Create the Web App
az webapp create --name $webappname --resource-group DotNetAzureTutorial --plan $webappname
```

<span data-ttu-id="b726c-126">進行部署之前，使用以下命令來設定帳戶層級的部署認證：</span><span class="sxs-lookup"><span data-stu-id="b726c-126">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="b726c-127">使用 Git 部署應用程式需要部署 URL。</span><span class="sxs-lookup"><span data-stu-id="b726c-127">A deployment URL is needed to deploy the app using Git.</span></span> <span data-ttu-id="b726c-128">擷取如下的 URL。</span><span class="sxs-lookup"><span data-stu-id="b726c-128">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```

<span data-ttu-id="b726c-129">記下所顯示結尾是 `.git` 的 URL。</span><span class="sxs-lookup"><span data-stu-id="b726c-129">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="b726c-130">在下一步中會使用它。</span><span class="sxs-lookup"><span data-stu-id="b726c-130">It's used in the next step.</span></span>

## <a name="deploy-the-app-using-git"></a><span data-ttu-id="b726c-131">使用 Git 部署應用程式</span><span class="sxs-lookup"><span data-stu-id="b726c-131">Deploy the app using Git</span></span>

<span data-ttu-id="b726c-132">您現在可以使用 Git 從本機電腦進行部署。</span><span class="sxs-lookup"><span data-stu-id="b726c-132">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="b726c-133">您可以放心地忽略來自 Git 有關行尾結束符號的任何警告。</span><span class="sxs-lookup"><span data-stu-id="b726c-133">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="b726c-134">Windows</span><span class="sxs-lookup"><span data-stu-id="b726c-134">Windows</span></span>](#tab/windows)

```cmd
REM Initialize the local Git repository
git init

REM Add the contents of the working directory to the repo
git add --all

REM Commit the changes to the local repo
git commit -a -m "Initial commit"

REM Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

REM Push the local repository to the remote
git push azure master
```

# <a name="othertabother"></a>[<span data-ttu-id="b726c-135">其他</span><span class="sxs-lookup"><span data-stu-id="b726c-135">Other</span></span>](#tab/other)

```bash
# Initialize the local Git repository
git init

# Add the contents of the working directory to the repo
git add --all

# Commit the changes to the local repo
git commit -a -m "Initial commit"

# Add the URL as a Git remote repository
git remote add azure <THE GIT URL YOU NOTED EARLIER>

# Push the local repository to the remote
git push azure master
```

---

<span data-ttu-id="b726c-136">Git 會提示您輸入先前設定的部署認證。</span><span class="sxs-lookup"><span data-stu-id="b726c-136">Git prompts for the deployment credentials that were set earlier.</span></span> <span data-ttu-id="b726c-137">驗證之後，應用程式就會被推送到遠端位置，然後進行建置及部署。</span><span class="sxs-lookup"><span data-stu-id="b726c-137">After authenticating, the app will be pushed to the remote location, built, and deployed.</span></span>

![Git 部署輸出](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-app"></a><span data-ttu-id="b726c-139">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="b726c-139">Test the app</span></span>

<span data-ttu-id="b726c-140">瀏覽至 `https://<web app name>.azurewebsites.net` 來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="b726c-140">Test the app by browsing to `https://<web app name>.azurewebsites.net`.</span></span> <span data-ttu-id="b726c-141">若要顯示在 Cloud Shell (或 Azure CLI) 中的位址，請使用：</span><span class="sxs-lookup"><span data-stu-id="b726c-141">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![應用程式在 Azure 中執行](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="b726c-143">清除</span><span class="sxs-lookup"><span data-stu-id="b726c-143">Clean up</span></span>

<span data-ttu-id="b726c-144">完成測試應用程式及檢查程式碼和資源之後，請刪除資源群組，藉以刪除 Web 應用程式和方案。</span><span class="sxs-lookup"><span data-stu-id="b726c-144">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="b726c-145">後續步驟</span><span class="sxs-lookup"><span data-stu-id="b726c-145">Next steps</span></span>

<span data-ttu-id="b726c-146">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="b726c-146">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b726c-147">使用 Azure CLI 來建立 Azure App Service 網站</span><span class="sxs-lookup"><span data-stu-id="b726c-147">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="b726c-148">使用 Git 命令列工具將 ASP.NET Core 應用程式部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="b726c-148">Deploy an ASP.NET Core app to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="b726c-149">接下來，學習使用命令列來部署使用 CosmosDB 的現有 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b726c-149">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b726c-150">使用 .NET Core 從命令列部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="b726c-150">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
