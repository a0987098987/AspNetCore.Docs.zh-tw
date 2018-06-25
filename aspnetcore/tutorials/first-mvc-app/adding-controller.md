---
title: 將控制器新增至 ASP.NET Core MVC 應用程式
author: rick-anderson
description: 了解如何將控制器新增至簡單的 ASP.NET Core MVC 應用程式。
ms.author: riande
ms.date: 02/28/2017
uid: tutorials/first-mvc-app/adding-controller
ms.openlocfilehash: bf4ac33103d525194524e7578902e6f985dbe7c2
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276587"
---
# <a name="add-a-controller-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="59147-103">將控制器新增至 ASP.NET Core MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="59147-103">Add a controller to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="59147-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="59147-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [adding-controller1](~/includes/mvc-intro/adding-controller1.md)]

* <span data-ttu-id="59147-105">在方案總管中，以滑鼠右鍵按一下 [控制器] > [新增] > [新增項目]。</span><span class="sxs-lookup"><span data-stu-id="59147-105">In **Solution Explorer**, right-click **Controllers > Add > New Item**</span></span>

![操作功能表](adding-controller/_static/add_controller.png)

* <span data-ttu-id="59147-107">選取 [控制器類別]</span><span class="sxs-lookup"><span data-stu-id="59147-107">Select **Controller Class**</span></span>
* <span data-ttu-id="59147-108">在 [新增項目] 對話方塊中，輸入 **HelloWorldController**。</span><span class="sxs-lookup"><span data-stu-id="59147-108">In the **Add New Item** dialog, enter **HelloWorldController**.</span></span>

![新增 MVC 控制器並將其命名](adding-controller/_static/ac.png)

[!INCLUDE [adding-controller2](~/includes/mvc-intro/adding-controller2.md)]

<span data-ttu-id="59147-110">使用 Visual Studio 的非偵錯模式 (Ctrl+F5) 時，您不需要在變更程式碼之後建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="59147-110">In Visual Studio, in non-debug mode (Ctrl+F5), you don't need to build the app after changing  code.</span></span> <span data-ttu-id="59147-111">只要儲存檔案，並重新整理瀏覽器，即可看到所做的變更。</span><span class="sxs-lookup"><span data-stu-id="59147-111">Just save the file, refresh your browser and you can see the changes.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="59147-112">[上一頁](start-mvc.md)
> [下一頁](adding-view.md)</span><span class="sxs-lookup"><span data-stu-id="59147-112">[Previous](start-mvc.md)
[Next](adding-view.md)</span></span>