---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: 建置章節下載 EF 5 MVC 4 的教學課程 |Microsoft Docs
author: Rick-Anderson
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 應用程式...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 6b5d10ba9e878908953e999bd1fd44970acf4ca5
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911964"
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a><span data-ttu-id="0ec6d-103">建置章節下載 EF 5 MVC 4 的教學課程</span><span class="sxs-lookup"><span data-stu-id="0ec6d-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span></span>
====================
<span data-ttu-id="0ec6d-104">藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="0ec6d-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[<span data-ttu-id="0ec6d-105">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="0ec6d-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="0ec6d-106">Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ec6d-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="0ec6d-107">如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="0ec6d-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


## <a name="building-the-chapter-downloads"></a><span data-ttu-id="0ec6d-108">建置章節下載</span><span class="sxs-lookup"><span data-stu-id="0ec6d-108">Building the Chapter Downloads</span></span>

1. <span data-ttu-id="0ec6d-109">下載並解壓縮專案範例 zip 檔案。</span><span class="sxs-lookup"><span data-stu-id="0ec6d-109">Download and unzip the  project sample zip file.</span></span> <span data-ttu-id="0ec6d-110">解壓縮的下載封裝中，您會發現其他的 zip 檔案，其中的每一章完成作業。</span><span class="sxs-lookup"><span data-stu-id="0ec6d-110">In the unzipped download package, you will find additional zip files, one for the completion of each chapter.</span></span>
2. <span data-ttu-id="0ec6d-111">以滑鼠右鍵按一下所需的 zip 檔案，然後按一下**屬性**，然後按一下**解除封鎖** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0ec6d-111">Right click on the desired zip file, click **Properties**, and click the **Unblock** button.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. <span data-ttu-id="0ec6d-112">將檔案解壓縮。</span><span class="sxs-lookup"><span data-stu-id="0ec6d-112">Unzip the file.</span></span>
4. <span data-ttu-id="0ec6d-113">按兩下*CUx.sln*檔案以啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="0ec6d-113">Double-click the *CUx.sln* file to launch Visual Studio.</span></span>
5. <span data-ttu-id="0ec6d-114">從**工具**功能表上，按一下**NuGet 套件管理員**，然後**Package Manager Console**。</span><span class="sxs-lookup"><span data-stu-id="0ec6d-114">From the **Tools** menu, click **NuGet Package Manager**, then **Package Manager Console**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. <span data-ttu-id="0ec6d-115">在 套件管理員主控台 (PMC)，按一下**還原**。</span><span class="sxs-lookup"><span data-stu-id="0ec6d-115">In the Package Manager Console (PMC), click **Restore**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. <span data-ttu-id="0ec6d-116">結束 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="0ec6d-116">Exit Visual Studio.</span></span>
8. <span data-ttu-id="0ec6d-117">重新啟動 Visual Studio 中，在上述步驟中開啟方案檔，您已關閉。</span><span class="sxs-lookup"><span data-stu-id="0ec6d-117">Restart Visual Studio, opening the solution file you closed in the step above.</span></span>
9. <span data-ttu-id="0ec6d-118">在 套件管理員主控台 (PMC)，請輸入`Update-Database`命令：</span><span class="sxs-lookup"><span data-stu-id="0ec6d-118">In the Package Manager Console (PMC), enter the `Update-Database` command:</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > <span data-ttu-id="0ec6d-119">如果您收到下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="0ec6d-119">If you get the following error:</span></span>  
    >   
    >  <span data-ttu-id="0ec6d-120">*詞彙 更新資料庫 ' 無法辨識為 cmdlet、 函式、 指令碼檔案或可執行程式的名稱。請檢查名稱的拼字，或如果包含路徑的話，確認路徑正確，然後再試一次。*</span><span class="sxs-lookup"><span data-stu-id="0ec6d-120">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*</span></span>  
    > <span data-ttu-id="0ec6d-121">結束並重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="0ec6d-121">Exit and restart Visual Studio.</span></span>

    <span data-ttu-id="0ec6d-122">每個移轉將會執行，則會執行 seed 方法。</span><span class="sxs-lookup"><span data-stu-id="0ec6d-122">Each migration will run, then the seed method will run.</span></span> <span data-ttu-id="0ec6d-123">您現在可以執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="0ec6d-123">You can now run the app.</span></span>

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="0ec6d-124">上一步</span><span class="sxs-lookup"><span data-stu-id="0ec6d-124">Previous</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
