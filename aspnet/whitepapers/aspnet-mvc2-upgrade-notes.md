---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: "ASP.NET MVC 1.0 應用程式升級為 ASP.NET MVC 2 |Microsoft 文件"
author: rick-anderson
description: "本文件說明如何將使用精靈和手動升級至 ASP.NET MVC 2 1.0 ASP.NET MVC 應用程式。 本文件也適用於 d..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: d1227f0738b2d2a396fed942f32ae5e75579596e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>升級至 ASP.NET MVC 2 1.0 的 ASP.NET MVC 應用程式
====================
> 本文件說明如何將使用精靈和手動升級至 ASP.NET MVC 2 1.0 ASP.NET MVC 應用程式。 本文件也會提供如[下載](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)


## <a name="introduction"></a>簡介

ASP.NET MVC 2 可以在同一部伺服器上的 ASP.NET MVC 1.0 並存安裝。 這可讓應用程式開發人員在選擇何時要升級至 ASP.NET MVC 2 ASP.NET MVC 1.0 應用程式的彈性。

Visual Studio 2010 包含一個精靈，該升級現有的 ASP.NET MVC 1.0 專案是以 ASP.NET MVC 2 Visual Studio 2008 建置。 在 Visual Studio 2010 中開啟 ASP.NET MVC 1.0 專案起始升級精靈。

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>升級 Visual Studio 2008 SP1 上的 ASP.NET MVC 1.0 的精靈

若要升級至 Visual Studio 2008 SP1 中的 ASP.NET MVC 2 的 ASP.NET MVC 1.0 應用程式，使用 （不受支援） MvcAppConverter 應用程式。 您可以從下列 URL 下載這個應用程式：

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>手動升級專案，ASP.NET MVC 1.0

若要手動升級現有的 ASP.NET MVC 1.0 應用程式第 2 版，請遵循下列步驟：

1. 製作現有專案的備份。
2. 在文字編輯器中，開啟專案檔 （.csproj 或.vbproj 檔案延伸模組的檔案），並尋找 ProjectTypeGuid 項目。 做為該元素的值，來取代 GUID {603c0e0b-db56-11dc-be95-000d561079b0} 與 {F85E285D-A4E0-4152-9332-AB1D724D3325}。 當您完成之後時，該元素的值應該，如下所示： 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. 在 Web 應用程式根資料夾中，編輯 Web.config 檔案。 搜尋 system.web.mvc 的參考，版本 = 1.0.0.0 和所有執行個體取代為 system.web.mvc 的參考，版本 = 2.0.0.0。
4. 檢視資料夾中的 Web.config 檔重複上述步驟。
5. 開啟專案中使用 Visual Studio 中，然後在**方案總管 中**，依序展開**參考**節點。 刪除 system.web.mvc 的參考 （指向 1.0 版組件）。 加入 system.web.mvc 的參考 (v2.0.0.0)。
6. 將下列的 bindingRedirect 項目加入至 [設定] 區段下的應用程式根目錄中的 Web.config 檔案：   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. 建立新的空白 ASP.NET MVC 2 應用程式。 新的應用程式的指令碼資料夾的現有應用程式的指令碼資料夾複製檔案。
8. 更新現有的細分 €™ s Site.css 檔案中的 CSS 樣式定義的 CSS 檔案。
9. 編譯應用程式並執行它。 如果發生任何錯誤，請參閱 > 一節的重大變更[What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038)頁面。
