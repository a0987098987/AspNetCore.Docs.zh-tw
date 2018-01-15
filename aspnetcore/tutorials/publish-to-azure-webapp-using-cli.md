---
title: "使用命令列工具將 ASP.NET Core 應用程式發行到 Azure | Microsoft Docs"
description: "了解如何使用 Git 命令列用戶端將 ASP.NET Core 應用程式發行到 Azure App Service。"
services: multiple
keywords: "ASP.NET Core, Azure, App Service, Git, command line, 應用程式服務, 命令列"
author: camsoper
ms.author: casoper
manager: wpickett
ms.date: 11/03/2017
ms.topic: get-started-article
ms.prod: asp.net-core
ms.technology: aspnet
ms.custom: mvc
ms.devlang: dotnet
uid: tutorials/publish-to-azure-webapp-using-cli
ms.openlocfilehash: 6af5de584cbf8cd59d86a965592b958061014c95
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2018
---
# <a name="deploy-an-aspnet-core-application-to-azure-app-service-from-the-command-line"></a><span data-ttu-id="c7dd1-104">從命令列將 ASP.NET Core 應用程式部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c7dd1-104">Deploy an ASP.NET Core application to Azure App Service from the command line</span></span>

<span data-ttu-id="c7dd1-105">作者 [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="c7dd1-105">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="c7dd1-106">本教學課程將說明如何使用命令列工具來建置 ASP.NET Core 應用程式，並將其部署至 Microsoft Azure App Service。</span><span class="sxs-lookup"><span data-stu-id="c7dd1-106">This tutorial will show you how to build and deploy an ASP.NET Core application to Microsoft Azure App Service using command line tools.</span></span>  <span data-ttu-id="c7dd1-107">完成之後，您將會擁有建置在 ASP.NET MVC Core 中且裝載為 Azure App Service Web 應用程式的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c7dd1-107">When finished, you'll have a web application built in ASP.NET MVC Core hosted as an Azure App Service Web App.</span></span>  <span data-ttu-id="c7dd1-108">本教學課程是使用 Windows 命令列工具來撰寫，但也適用於 macOS 和 Linux 環境。</span><span class="sxs-lookup"><span data-stu-id="c7dd1-108">This tutorial is written using Windows command line tools, but can be applied to macOS and Linux environments, as well.</span></span>  

<span data-ttu-id="c7dd1-109">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="c7dd1-109">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c7dd1-110">使用 Azure CLI 來建立 Azure App Service 網站</span><span class="sxs-lookup"><span data-stu-id="c7dd1-110">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="c7dd1-111">使用 Git 命令列工具將 ASP.NET Core 應用程式部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c7dd1-111">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c7dd1-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="c7dd1-112">Prerequisites</span></span>

<span data-ttu-id="c7dd1-113">若要完成此課程，您會需要：</span><span class="sxs-lookup"><span data-stu-id="c7dd1-113">To complete this tutorial, you'll need:</span></span>

* <span data-ttu-id="c7dd1-114">[Microsoft Azure 訂用帳戶](https://azure.microsoft.com/free/)</span><span class="sxs-lookup"><span data-stu-id="c7dd1-114">A [Microsoft Azure subscription](https://azure.microsoft.com/free/)</span></span>
* [<span data-ttu-id="c7dd1-115">.NET Core</span><span class="sxs-lookup"><span data-stu-id="c7dd1-115">.NET Core</span></span>](https://www.microsoft.com/net/download/core)
* <span data-ttu-id="c7dd1-116">[Git](https://www.git-scm.com/) 命令列用戶端</span><span class="sxs-lookup"><span data-stu-id="c7dd1-116">[Git](https://www.git-scm.com/) command line client</span></span>

## <a name="create-a-web-application"></a><span data-ttu-id="c7dd1-117">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="c7dd1-117">Create a web application</span></span>

<span data-ttu-id="c7dd1-118">為 Web 應用程式建立新的目錄、建立新的 ASP.NET Core MVC 應用程式，然後在本機執行該網站。</span><span class="sxs-lookup"><span data-stu-id="c7dd1-118">Create a new directory for the web application, create a new ASP.NET Core MVC application, and then run the website locally.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="c7dd1-119">Windows</span><span class="sxs-lookup"><span data-stu-id="c7dd1-119">Windows</span></span>](#tab/windows)
```cmd
REM Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

REM Change to the new directory that was just created
cd MyApplication

REM Run the application
dotnet run
```

# <a name="othertabother"></a>[<span data-ttu-id="c7dd1-120">其他</span><span class="sxs-lookup"><span data-stu-id="c7dd1-120">Other</span></span>](#tab/other)
```bash
# Create a new ASP.NET Core MVC application
dotnet new razor -o MyApplication

# Change to the new directory that was just created
cd MyApplication

# Run the application
dotnet run
```
---

![命令列輸出](publish-to-azure-webapp-using-cli/_static/new_prj.png)

<span data-ttu-id="c7dd1-122">瀏覽至 http://localhost:5000 來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="c7dd1-122">Test the application by browsing to http://localhost:5000.</span></span>

![網站在本機上執行](publish-to-azure-webapp-using-cli/_static/app_test.png)


## <a name="create-the-azure-app-service-instance"></a><span data-ttu-id="c7dd1-124">建立 Azure App Service 執行個體</span><span class="sxs-lookup"><span data-stu-id="c7dd1-124">Create the Azure App Service instance</span></span>

<span data-ttu-id="c7dd1-125">使用 [Azure Cloud Shell](/azure/cloud-shell/quickstart)，建立資源群組、App Service 方案，以及 App Service Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c7dd1-125">Using the [Azure Cloud Shell](/azure/cloud-shell/quickstart), create a resource group, App Service plan, and an App Service web app.</span></span>

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

<span data-ttu-id="c7dd1-126">進行部署之前，使用以下命令來設定帳戶層級的部署認證：</span><span class="sxs-lookup"><span data-stu-id="c7dd1-126">Before deployment, set the account-level deployment credentials using the following command:</span></span>

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```

<span data-ttu-id="c7dd1-127">使用 Git 部署應用程式需要部署 URL。</span><span class="sxs-lookup"><span data-stu-id="c7dd1-127">A deployment URL is needed to deploy the application using Git.</span></span>  <span data-ttu-id="c7dd1-128">擷取如下的 URL。</span><span class="sxs-lookup"><span data-stu-id="c7dd1-128">Retrieve the URL like this.</span></span>

```azurecli-interactive
az webapp deployment source config-local-git -n $webappname -g DotNetAzureTutorial --query [url] -o tsv
```
<span data-ttu-id="c7dd1-129">記下所顯示結尾是 `.git` 的 URL。</span><span class="sxs-lookup"><span data-stu-id="c7dd1-129">Note the displayed URL ending in `.git`.</span></span> <span data-ttu-id="c7dd1-130">在下一步中會使用它。</span><span class="sxs-lookup"><span data-stu-id="c7dd1-130">It's used in the next step.</span></span>

## <a name="deploy-the-application-using-git"></a><span data-ttu-id="c7dd1-131">使用 Git 來部署應用程式</span><span class="sxs-lookup"><span data-stu-id="c7dd1-131">Deploy the application using Git</span></span>

<span data-ttu-id="c7dd1-132">您現在可以使用 Git 從本機電腦進行部署。</span><span class="sxs-lookup"><span data-stu-id="c7dd1-132">You're ready to deploy from your local machine using Git.</span></span>

> [!NOTE]
> <span data-ttu-id="c7dd1-133">您可以放心地忽略來自 Git 有關行尾結束符號的任何警告。</span><span class="sxs-lookup"><span data-stu-id="c7dd1-133">It's safe to ignore any warnings from Git about line endings.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="c7dd1-134">Windows</span><span class="sxs-lookup"><span data-stu-id="c7dd1-134">Windows</span></span>](#tab/windows)
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

# <a name="othertabother"></a>[<span data-ttu-id="c7dd1-135">其他</span><span class="sxs-lookup"><span data-stu-id="c7dd1-135">Other</span></span>](#tab/other)
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

<span data-ttu-id="c7dd1-136">Git 會提示您輸入先前設定的部署認證。</span><span class="sxs-lookup"><span data-stu-id="c7dd1-136">Git will prompt for the deployment credentials that were set earlier.</span></span>  <span data-ttu-id="c7dd1-137">驗證之後，應用程式就會被推送到遠端位置，然後進行建置及部署。</span><span class="sxs-lookup"><span data-stu-id="c7dd1-137">After authenticating, the application will be pushed to the remote location, built, and deployed.</span></span>

![Git 部署輸出](publish-to-azure-webapp-using-cli/_static/post_deploy.png)

## <a name="test-the-application"></a><span data-ttu-id="c7dd1-139">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="c7dd1-139">Test the application</span></span>

<span data-ttu-id="c7dd1-140">瀏覽至 `https://<web app name>.azurewebsites.net` 來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="c7dd1-140">Test the application by browsing to `https://<web app name>.azurewebsites.net`.</span></span>  <span data-ttu-id="c7dd1-141">若要顯示在 Cloud Shell (或 Azure CLI) 中的位址，請使用：</span><span class="sxs-lookup"><span data-stu-id="c7dd1-141">To display the address in the Cloud Shell (or Azure CLI), use the following:</span></span>

```azurecli-interactive
az webapp show -n $webappname -g DotNetAzureTutorial --query defaultHostName -o tsv
```

![應用程式在 Azure 中執行](publish-to-azure-webapp-using-cli/_static/app_deployed.png)

## <a name="clean-up"></a><span data-ttu-id="c7dd1-143">清除</span><span class="sxs-lookup"><span data-stu-id="c7dd1-143">Clean up</span></span>

<span data-ttu-id="c7dd1-144">完成測試應用程式及檢查程式碼和資源之後，請刪除資源群組，藉以刪除 Web 應用程式和方案。</span><span class="sxs-lookup"><span data-stu-id="c7dd1-144">When finished testing the app and inspecting the code and resources, delete the web app and plan by deleting the resource group.</span></span>

```azurecli-interactive
az group delete -n DotNetAzureTutorial
```

## <a name="next-steps"></a><span data-ttu-id="c7dd1-145">後續步驟</span><span class="sxs-lookup"><span data-stu-id="c7dd1-145">Next steps</span></span>

<span data-ttu-id="c7dd1-146">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="c7dd1-146">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c7dd1-147">使用 Azure CLI 來建立 Azure App Service 網站</span><span class="sxs-lookup"><span data-stu-id="c7dd1-147">Create an Azure App Service website using Azure CLI</span></span>
> * <span data-ttu-id="c7dd1-148">使用 Git 命令列工具將 ASP.NET Core 應用程式部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c7dd1-148">Deploy an ASP.NET Core application to Azure App Service using the Git command line tool</span></span>

<span data-ttu-id="c7dd1-149">接下來，學習使用命令列來部署使用 CosmosDB 的現有 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c7dd1-149">Next, learn to use the command line to deploy an existing web app that uses CosmosDB.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c7dd1-150">使用 .NET Core 從命令列部署至 Azure</span><span class="sxs-lookup"><span data-stu-id="c7dd1-150">Deploy to Azure from the command line with .NET Core</span></span>](/dotnet/azure/dotnet-quickstart-xplat)
