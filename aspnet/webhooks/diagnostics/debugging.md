---
uid: webhooks/diagnostics/debugging
title: "偵錯 ASP.NET Webhook |Microsoft 文件"
author: rick-anderson
description: "如何偵錯 ASP.NET Webhook。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 566ee353f6a947e3ef0efdfd0af3a81dff2147c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="1c51b-103">偵錯 ASP.NET Webhook</span><span class="sxs-lookup"><span data-stu-id="1c51b-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="1c51b-104">在 Azure 中偵錯</span><span class="sxs-lookup"><span data-stu-id="1c51b-104">Debugging in Azure</span></span>

<span data-ttu-id="1c51b-105">若要偵錯 Web 應用程式在 Azure 中執行時，請參閱本教學課程[疑難排解 web 應用程式中使用 Visual Studio 的 Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs)。</span><span class="sxs-lookup"><span data-stu-id="1c51b-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="1c51b-106">使用原始檔和符號偵錯</span><span class="sxs-lookup"><span data-stu-id="1c51b-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="1c51b-107">除了您自己的程式碼進行偵錯，就可以直接在 Microsoft ASP.NET Webhook，事實上偵錯所有.NET。</span><span class="sxs-lookup"><span data-stu-id="1c51b-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="1c51b-108">這適用於無論是否您在本機或遠端的偵錯。</span><span class="sxs-lookup"><span data-stu-id="1c51b-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="1c51b-109">首先，設定 Visual Studio 以尋找原始檔和符號，請前往**偵錯**然後**選項和設定**。</span><span class="sxs-lookup"><span data-stu-id="1c51b-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="1c51b-110">設定選項如下：</span><span class="sxs-lookup"><span data-stu-id="1c51b-110">Set the options like this:</span></span>

![選項和設定](_static/SourceSymbols.png)

<span data-ttu-id="1c51b-112">然後加入要連結[symbolsource.org](http://symbolsource.org)下載的來源和符號。</span><span class="sxs-lookup"><span data-stu-id="1c51b-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="1c51b-113">移至**符號**上述功能表 索引標籤並做為符號位置加入下列：</span><span class="sxs-lookup"><span data-stu-id="1c51b-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="1c51b-114">此外，請確定快取目錄的簡短名稱;否則檔案名稱可以變得太長導致不載入符號。</span><span class="sxs-lookup"><span data-stu-id="1c51b-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="1c51b-115">範例路徑為：</span><span class="sxs-lookup"><span data-stu-id="1c51b-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="1c51b-116">設定看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="1c51b-116">The settings should look similar to this:</span></span>

![選項符號檔位置範例](_static/SymSource.png)
