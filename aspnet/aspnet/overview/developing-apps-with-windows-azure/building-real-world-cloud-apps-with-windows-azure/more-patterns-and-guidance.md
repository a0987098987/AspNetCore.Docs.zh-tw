---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/more-patterns-and-guidance
title: "多個模式和指導方針 （使用 Azure 建置實際的雲端應用程式） |Microsoft 文件"
author: MikeWasson
description: "Scott Guthrie 所開發的簡報是以基礎建置真實世界雲端應用程式與 Azure 的電子書。 它說明 13 模式和做法，他可以..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 7e97cfc3-d830-4002-8ff7-5790d1ff49e6
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/more-patterns-and-guidance
msc.type: authoredcontent
ms.openlocfilehash: a388f2e0ca3e1f0ce24a6def2a2b91711a7bf5a7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="more-patterns-and-guidance-building-real-world-cloud-apps-with-azure"></a>多個模式和指導方針 （使用 Azure 建置實際的雲端應用程式）
====================
由[Mike Wasson](https://github.com/MikeWasson)， [Rick Anderson](https://github.com/Rick-Anderson)， [Tom Dykstra](https://github.com/tdykstra)

[下載修正專案](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)或[下載電子書](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **建置真實世界雲端應用程式與 Azure**電子書根據 Scott Guthrie 所開發的簡報。 它說明 13 模式，並可協助您的作法是成功開發雲端的 web 應用程式。 E 書籍的相關資訊，請參閱[第一章](introduction.md)。


您現在看到 13 模式會提供指引，說明成功在雲端運算。 這些是幾個套用至雲端應用程式的模式。 以下是一些雲端運算主題和資源可協助它們：

- 移轉至雲端的現有內部部署應用程式。 

    - [移動至雲端的應用程式](https://msdn.microsoft.com/library/ff728592.aspx)。 由 Microsoft Patterns and Practices 電子書。 同時也隨附[硬碟複製平裝](https://www.amazon.com/dp/1621140202)。
    - [移轉的 Microsoft ASP.NET 和 IIS.NET](https://go.microsoft.com/fwlink/?LinkId=400656)。 由 Robert McMurray 的案例研究。
    - [移動 4th &amp; Azure Web Sites 的鎮長](http://www.jeff.wilcox.name/2013/04/4thandmayor-azure-websites/)。 部落格文章 Jeff Wilcox chronicling 他從 Amazon Web Services 的 web 應用程式移至 Azure App Service 中的 Web 應用程式的體驗。
    - [將應用程式移動到 Azure： 哪些變更？](https://azure.microsoft.com/documentation/videos/web-sites-internals-and-the-file-system/) 簡短的影片，由 Stefan Schackow、 說明 Azure App Service Web 應用程式中的檔案系統存取權。
    - [Azure 的混合式雲端](https://www.amazon.com/dp/B00EOP4UQW)。 硬拷貝書籍或 Danny Garber、 Jamal Malik，和 Adam Fazio 電子書。
- 雲端應用程式唯一的安全性、 驗證和授權問題

    - [Azure 安全性指引](https://azure.microsoft.com/blog/2014/02/10/best-practices-windows-azure-websites-waws/)
    - [Microsoft Patterns and Practices-Azure 指引](https://msdn.microsoft.com/library/dn568099.aspx)。 請參閱閘道管理員模式中，同盟識別身分的模式。
    - [Azure 網路安全性](https://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx)。 由 Ashin Palekar 詘躩裛。

請參閱其他雲端運算模式與指引[Microsoft Patterns and Practices-Azure 指引](https://msdn.microsoft.com/library/dn568099.aspx)。

<a id="resources"></a>
## <a name="resources"></a>資源

每個這個 e 活頁簿中的章節如需有關該特定的主題提供資源的連結。 下列清單提供成功雲端開發與 Azure 中的連結的最佳作法和建議的模式概觀。

文件

- [Azure 雲端服務中大規模服務設計的最佳作法](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx)。 Mark Simms 和 Michael Thomassy 詘躩裛。
- [Failsafe： 具有恢復功能雲端架構指引](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx)。 Marc Mercuri、 Ulrich Homann 和 Andrew Townhill 詘躩裛。 FailSafe 影片系列網頁版本。
- [Azure 指引](https://azure.microsoft.com/develop/net/guidance/)開發 azure 應用程式與相關的官方文件入口網站頁面。

視訊

- [建立真實世界雲端應用程式與 Azure-第 1 部分](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324)和[第 2 部分](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)。 影片由 Scott Guthrie 這個 e 活頁簿為基礎的簡報。 在 2013 年 9 月，呈現技術 Ed 澳洲在。 較早版本的同一份展示檔已在 2013 年 6 月，傳遞在挪威開發人員會議 (NDC): [NDC 第 1 部分](http://vimeo.com/68215538)， [NDC 第 2 部分](http://vimeo.com/68215602)。
- [FailSafe： 建置可擴充、 彈性的雲端服務](https://channel9.msdn.com/Series/FailSafe)。 Ulrich Homann、 Marc Mercuri 和 Mark Simms 九部影片系列。 提供如何設計雲端應用程式架構的 400 層級檢視。 這一系列著重在理論上和建議的模式; 背後的原因如需作法的詳細資訊，請參閱 Mark Simms 建置大數列。
- [建置大： 從 Azure 客戶-第 1 部分學](https://channel9.msdn.com/Events/Build/2012/3-029)和[第 2 部分](https://channel9.msdn.com/Events/Build/2012/3-030)。 Simon Davies 和 Mark Simms、 類似保全數列但朝實際實作導向更多影片系列的兩個部分。

程式碼範例

- [修正它應用程式隨附此的電子書](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4?cdn_id=2013-12-03-002)。
- [雲端服務在 Azure 中的 Visual Studio 2012 C# 中的基本概念](http://aka.ms/csf)。 可下載的專案，在 Microsoft Code Gallery 網站中，包含程式碼和開發由 Microsoft 客戶諮詢團隊 (CAT) 的文件。 示範許多關注的領域保全和建置大影片系列和保全技術白皮書中的最佳作法。 程式碼庫頁面也會連結到大量的文件作者的專案-請參閱特別[雲端服務基本概念 wiki 集合](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx)專案描述最上方的藍色方塊中的連結。 這個專案與文件，還是主動開發，使其比類似，但較舊技術白皮書許多主題的相關資訊的較佳選擇。

複本書籍

- [雲端運算 Bible](https://www.amazon.com/dp/0470903562)。 由 Barrie Sosinsky。
- [釋放它 ！設計和部署可實際執行的軟體](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213)。 由 Michael T Nygard。
- [雲端架構模式： 使用 Microsoft Azure](http://shop.oreilly.com/product/0636920023777.do)。 由 Bill Wilder。
- [Windows Azure 平台](https://www.amazon.com/dp/1430235632)。 由 Tejaswi Redkar。
- [Windows Azure 程式設計模式針對創](https://www.amazon.com/dp/1849685606)。 由 Riccardo Becker。
- [Microsoft Windows Azure 開發 Cookbook](https://www.amazon.com/dp/1849682224)。 由 Neil Mackenzie。

最後，當您開始建置真實世界應用程式，以及在 Azure 中執行它們，很快您可能需要來自專家的協助。 您可以詢問下列問題的社群網站[Azure 論壇或 StackOverflow](https://azure.microsoft.com/support/forums/)，或您可以直接對 Azure 支援人員連絡 Microsoft。 Microsoft 提供數個層級的技術支援 Azure： 如需摘要和比較選項，請參閱[Azure 支援](https://azure.microsoft.com/support/plans/)。

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a>通知

這個內容已撰寫 Tom Dykstra、 Rick Anderson 的 Mike Wasson。 大部分的原始內容來自[Scott Guthrie](https://weblogs.asp.net/scottgu/)，以及他接著在上繪製資料從 Mark Simms 和 Microsoft 客戶諮詢團隊 (CAT)。

在 Microsoft 許多其他同事檢閱，並加上 「 草稿 」 和 「 程式碼註解：

- Tim Ammann-檢閱自動化章節。
- Christopher Bennage-檢閱並測試它修正程式碼。
- Ryan Berry-檢閱 CD/CI 章節。
- Vittorio Bertocci-檢閱 SSO 章節。
- Chris Clayton-這樣有助於解決的 PowerShell 指令碼中的技術問題。
- Conor Cunningham-檢閱資料的儲存體選項章節。
- Carlos Farre-檢閱並測試它修正程式碼的安全性問題。
- Larry Franks-檢閱遙測和監視的章節。
- Jonathan Gao-檢閱 Hadoop 和 MapReduce 區段之資料的儲存體選項章節。
- Sidney Higa-檢閱所有章節。
- Gordon Hogenson-檢閱來源控制章節。
- Tamra Myers-檢閱過的資料儲存體選項、 blob 和佇列的章節。
- Pranav Rastogi-檢閱 SSO 章節。
- June Blender Rogers-加入錯誤處理和 PowerShell 自動化指令碼的說明。
- Mani Subramanian-檢閱所有章節，並導致程式碼檢閱和修正該程式碼的測試程序。
- Shaun Tinline-jones-檢閱資料分割章節。
- Selcin Tukarslan 檢閱涵蓋 SQL Database 和 SQL Server 的章節。
- 愛德華 Wu-SSO 章節提供範例程式碼。
- Guang Yang-撰寫 PowerShell 自動化指令碼。

成員[Microsoft 開發人員指引 Advisory Council](http://aka.ms/DGAC) (DGAC) 也已檢閱並加上草稿註解：

- Jean Luc Boucho
- Catalin Gheorghiu
- Wouter de Kort
- Carlos dos Santos
- Neil Mackenzie
- Dennis Persson
- 請參閱 Sunil Sabat
- [Aleksey Sinyagin](http://www.linkedin.com/in/sinyagin)
- 帳單 Wagner
- Michael 木材

DGAC 的其他成員檢閱，並加上初步大綱註解：

- Damir Arh
- 愛德華 Bakker
- Srdjan Bozovic
- Ming Man 通道
- Gianni Rosa Gallina
- Paulo Morgado
- Jason Oliveira
- Alberto Poblacion
- Ryan Riley
- Perez Jones Tsisah
- Roger Whitehead
- Pawel Wilkosz

>[!div class="step-by-step"]
[上一頁](queue-centric-work-pattern.md)
[下一頁](the-fix-it-sample-application.md)
