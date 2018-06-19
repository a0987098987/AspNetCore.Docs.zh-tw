---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: ASP.NET MVC 1.0 應用程式升級為 ASP.NET MVC 2 |Microsoft 文件
author: rick-anderson
description: 本文件說明如何將使用精靈和手動升級至 ASP.NET MVC 2 1.0 ASP.NET MVC 應用程式。 本文件也適用於 d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: d1227f0738b2d2a396fed942f32ae5e75579596e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530057"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a><span data-ttu-id="55742-104">升級至 ASP.NET MVC 2 1.0 的 ASP.NET MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="55742-104">Upgrading an ASP.NET MVC 1.0 Application to ASP.NET MVC 2</span></span>
====================
> <span data-ttu-id="55742-105">本文件說明如何將使用精靈和手動升級至 ASP.NET MVC 2 1.0 ASP.NET MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="55742-105">This document describes both how to upgrade manually and with a wizard an ASP.NET MVC 1.0 Application to ASP.NET MVC 2.</span></span> <span data-ttu-id="55742-106">本文件也會提供如[下載](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span><span class="sxs-lookup"><span data-stu-id="55742-106">This document is also available for [Download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span></span>


## <a name="introduction"></a><span data-ttu-id="55742-107">簡介</span><span class="sxs-lookup"><span data-stu-id="55742-107">Introduction</span></span>

<span data-ttu-id="55742-108">ASP.NET MVC 2 可以在同一部伺服器上的 ASP.NET MVC 1.0 並存安裝。</span><span class="sxs-lookup"><span data-stu-id="55742-108">ASP.NET MVC 2 can be installed side by side with ASP.NET MVC 1.0 on the same server.</span></span> <span data-ttu-id="55742-109">這可讓應用程式開發人員在選擇何時要升級至 ASP.NET MVC 2 ASP.NET MVC 1.0 應用程式的彈性。</span><span class="sxs-lookup"><span data-stu-id="55742-109">This gives application developers flexibility in choosing when to upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2.</span></span>

<span data-ttu-id="55742-110">Visual Studio 2010 包含一個精靈，該升級現有的 ASP.NET MVC 1.0 專案是以 ASP.NET MVC 2 Visual Studio 2008 建置。</span><span class="sxs-lookup"><span data-stu-id="55742-110">Visual Studio 2010 includes a wizard that upgrades existing ASP.NET MVC 1.0 projects built with Visual Studio 2008 to ASP.NET MVC 2.</span></span> <span data-ttu-id="55742-111">在 Visual Studio 2010 中開啟 ASP.NET MVC 1.0 專案起始升級精靈。</span><span class="sxs-lookup"><span data-stu-id="55742-111">The upgrade wizard is initiated by opening an ASP.NET MVC 1.0 project in Visual Studio 2010.</span></span>

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a><span data-ttu-id="55742-112">升級 Visual Studio 2008 SP1 上的 ASP.NET MVC 1.0 的精靈</span><span class="sxs-lookup"><span data-stu-id="55742-112">Upgrade Wizard for ASP.NET MVC 1.0 on Visual Studio 2008 SP1</span></span>

<span data-ttu-id="55742-113">若要升級至 Visual Studio 2008 SP1 中的 ASP.NET MVC 2 的 ASP.NET MVC 1.0 應用程式，使用 （不受支援） MvcAppConverter 應用程式。</span><span class="sxs-lookup"><span data-stu-id="55742-113">To upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2 in Visual Studio 2008 SP1, use the (unsupported) MvcAppConverter application.</span></span> <span data-ttu-id="55742-114">您可以從下列 URL 下載這個應用程式：</span><span class="sxs-lookup"><span data-stu-id="55742-114">You can download this application from the following URL:</span></span>

[<span data-ttu-id="55742-115">https://go.microsoft.com/fwlink/?LinkID=185351</span><span class="sxs-lookup"><span data-stu-id="55742-115">https://go.microsoft.com/fwlink/?LinkID=185351</span></span>](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a><span data-ttu-id="55742-116">手動升級專案，ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="55742-116">Manually Upgrading an ASP.NET MVC 1.0 Project</span></span>

<span data-ttu-id="55742-117">若要手動升級現有的 ASP.NET MVC 1.0 應用程式第 2 版，請遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="55742-117">To manually upgrade an existing ASP.NET MVC 1.0 application to version 2, follow these steps:</span></span>

1. <span data-ttu-id="55742-118">製作現有專案的備份。</span><span class="sxs-lookup"><span data-stu-id="55742-118">Make a backup of the existing project.</span></span>
2. <span data-ttu-id="55742-119">在文字編輯器中，開啟專案檔 （.csproj 或.vbproj 檔案延伸模組的檔案），並尋找 ProjectTypeGuid 項目。</span><span class="sxs-lookup"><span data-stu-id="55742-119">In a text editor, open the project file (the file with the .csproj or .vbproj file extension) and find the ProjectTypeGuid element.</span></span> <span data-ttu-id="55742-120">做為該元素的值，來取代 GUID {603c0e0b-db56-11dc-be95-000d561079b0} 與 {F85E285D-A4E0-4152-9332-AB1D724D3325}。</span><span class="sxs-lookup"><span data-stu-id="55742-120">As the value of that element, replace the GUID {603c0e0b-db56-11dc-be95-000d561079b0} with {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span></span> <span data-ttu-id="55742-121">當您完成之後時，該元素的值應該，如下所示：</span><span class="sxs-lookup"><span data-stu-id="55742-121">When you are done, the value of that element should be as follows:</span></span> 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. <span data-ttu-id="55742-122">在 Web 應用程式根資料夾中，編輯 Web.config 檔案。</span><span class="sxs-lookup"><span data-stu-id="55742-122">In the Web application root folder, edit the Web.config file.</span></span> <span data-ttu-id="55742-123">搜尋 system.web.mvc 的參考，版本 = 1.0.0.0 和所有執行個體取代為 system.web.mvc 的參考，版本 = 2.0.0.0。</span><span class="sxs-lookup"><span data-stu-id="55742-123">Search for System.Web.Mvc, Version=1.0.0.0 and replace all instances with System.Web.Mvc, Version=2.0.0.0.</span></span>
4. <span data-ttu-id="55742-124">檢視資料夾中的 Web.config 檔重複上述步驟。</span><span class="sxs-lookup"><span data-stu-id="55742-124">Repeat the previous step for the Web.config file located in the Views folder.</span></span>
5. <span data-ttu-id="55742-125">開啟專案中使用 Visual Studio 中，然後在**方案總管 中**，依序展開**參考**節點。</span><span class="sxs-lookup"><span data-stu-id="55742-125">Open the project using Visual Studio, and in **Solution Explorer**, expand the **References** node.</span></span> <span data-ttu-id="55742-126">刪除 system.web.mvc 的參考 （指向 1.0 版組件）。</span><span class="sxs-lookup"><span data-stu-id="55742-126">Delete the reference to System.Web.Mvc (which points to the version 1.0 assembly).</span></span> <span data-ttu-id="55742-127">加入 system.web.mvc 的參考 (v2.0.0.0)。</span><span class="sxs-lookup"><span data-stu-id="55742-127">Add a reference to System.Web.Mvc (v2.0.0.0).</span></span>
6. <span data-ttu-id="55742-128">將下列的 bindingRedirect 項目加入至 [設定] 區段下的應用程式根目錄中的 Web.config 檔案：</span><span class="sxs-lookup"><span data-stu-id="55742-128">Add the following bindingRedirect element to the Web.config file in the application root under the configuraton section:</span></span>   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. <span data-ttu-id="55742-129">建立新的空白 ASP.NET MVC 2 應用程式。</span><span class="sxs-lookup"><span data-stu-id="55742-129">Create a new empty ASP.NET MVC 2 application.</span></span> <span data-ttu-id="55742-130">新的應用程式的指令碼資料夾的現有應用程式的指令碼資料夾複製檔案。</span><span class="sxs-lookup"><span data-stu-id="55742-130">Copy the files from the Scripts folder of the new application into the Scripts folder of the existing application.</span></span>
8. <span data-ttu-id="55742-131">更新現有的細分 €™ s Site.css 檔案中的 CSS 樣式定義的 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="55742-131">Update the existing applicationâ€™s CSS file with the CSS style definitions in the Site.css file.</span></span>
9. <span data-ttu-id="55742-132">編譯應用程式並執行它。</span><span class="sxs-lookup"><span data-stu-id="55742-132">Compile the application and run it.</span></span> <span data-ttu-id="55742-133">如果發生任何錯誤，請參閱 > 一節的重大變更[What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038)頁面。</span><span class="sxs-lookup"><span data-stu-id="55742-133">If any errors occur, refer to the Breaking Changes section of the [What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) page.</span></span>
