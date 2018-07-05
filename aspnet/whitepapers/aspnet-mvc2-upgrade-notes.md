---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: 升級至 ASP.NET MVC 2 的 ASP.NET MVC 1.0 應用程式 |Microsoft Docs
author: rick-anderson
description: 本文件說明兩者如何升級 ASP.NET MVC 1.0 應用程式以 ASP.NET MVC 2 手動並使用精靈。 這份文件也適用於 d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: ''
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: 354dbab3057068eb13b16c9aa31f66c72371ce7e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381912"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>升級至 ASP.NET MVC 2 的 ASP.NET MVC 1.0 應用程式
====================
> 本文件說明兩者如何升級 ASP.NET MVC 1.0 應用程式以 ASP.NET MVC 2 手動並使用精靈。 這份文件也已開放[下載](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)


## <a name="introduction"></a>簡介

ASP.NET MVC 2 可以在同一部伺服器上的與 ASP.NET MVC 1.0 並存安裝。 這可讓應用程式開發人員彈性來選擇升級至 ASP.NET MVC 2 的 ASP.NET MVC 1.0 應用程式的時機。

Visual Studio 2010 包含一個精靈，升級至 ASP.NET MVC 2 的 Visual Studio 2008 所建置的現有的 ASP.NET MVC 1.0 專案。 升級精靈是由 Visual Studio 2010 中開啟 ASP.NET MVC 1.0 專案啟動。

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>升級 Visual Studio 2008 SP1 上的 ASP.NET MVC 1.0 的精靈

若要升級至 Visual Studio 2008 SP1 中的 ASP.NET MVC 2 的 ASP.NET MVC 1.0 應用程式，使用 （不支援） 的 MvcAppConverter 應用程式。 您可以從下列 URL 下載此應用程式：

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>手動將 ASP.NET MVC 1.0 專案升級

若要手動升級到版本 2，現有的 ASP.NET MVC 1.0 應用程式，請遵循下列步驟：

1. 製作現有專案的備份。
2. 在文字編輯器中，開啟專案檔 （.csproj 或.vbproj 檔案副檔名的檔案） 並尋找 ProjectTypeGuid 項目。 該元素的值，來取代 GUID {603c0e0b-db56-11dc-be95-000d561079b0} 與 {F85E285D-A4E0-4152-9332-AB1D724D3325}。 當您完成時，該元素的值應該會顯示如下： 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. 在 Web 應用程式根資料夾中，編輯 Web.config 檔案。 搜尋 System.Web.Mvc，版本 = 1.0.0.0 和所有執行個體取代為 System.Web.Mvc，版本 = 2.0.0.0。
4. 位於 [Views] 資料夾中的 Web.config 檔重複上述步驟。
5. 使用 Visual Studio 中，開啟專案並在**方案總管**，展開**參考**節點。 刪除 system.web.mvc 的參考 （其指向 1.0 版組件）。 加入 system.web.mvc 的參考 (v2.0.0.0) 的參考。
6. 中的 [組態] 區段下的應用程式根目錄的 Web.config 檔案中加入下列的 bindingRedirect 項目：   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. 建立新的空白 ASP.NET MVC 2 應用程式。 從現有的應用程式到指令碼 資料夾的新應用程式的指令碼 資料夾中複製檔案。
8. 更新現有的應用程式 「™ s Site.css 檔案中的 CSS 樣式定義的 CSS 檔案。
9. 編譯應用程式，並加以執行。 如果發生任何錯誤，請參閱 > 一節的重大變更[What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038)頁面。
