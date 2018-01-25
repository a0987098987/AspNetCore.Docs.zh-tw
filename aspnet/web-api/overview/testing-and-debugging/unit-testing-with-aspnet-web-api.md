---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: "單元測試 ASP.NET Web API 2 |Microsoft 文件"
author: tfitzmac
description: "本指南及應用程式示範如何建立簡單的單元測試您的 Web API 2 應用程式。 本教學課程會示範如何加入單元測試專案..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2014
ms.topic: article
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4d6102dd81589e41894d8ecd95bf9ddd761a65bd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="unit-testing-aspnet-web-api-2"></a>單元測試 ASP.NET Web API 2
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

[下載完成的專案](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> 本指南及應用程式示範如何建立簡單的單元測試您的 Web API 2 應用程式。 本教學課程會示範如何在您的方案中包含單元測試專案和寫入測試方法，請檢查從控制器方法的傳回的值。
> 
> 本教學課程假設您熟悉的 ASP.NET Web API 的基本概念。 如需入門教學課程，請參閱[開始使用 ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)。
> 
> 本主題中的單元測試會刻意限制為簡單資料案例。 單元測試資料的更進階的案例，請參閱[模擬 Entity Framework 時單元測試 ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Web API 2


## <a name="in-this-topic"></a>本主題內容

此主題包括下列章節：

- [必要條件](#prereqs)
- [下載程式碼](#download)
- [使用單元測試專案建立應用程式](#appwithunittest)

    - [建立應用程式時，加入單元測試專案](#whencreate)
    - [將單元測試專案加入至現有的應用程式](#addtoexisting)
- [設定 Web API 2 應用程式](#setupproject)
- [在測試專案中安裝 NuGet 封裝](#testpackages)
- [建立測試](#tests)
- [執行測試](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a>必要條件

Visual Studio 2017 Community、 Professional 或 Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>下載程式碼

下載[已完成的專案](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)。 可下載專案中包含單元測試程式碼為本主題和[模擬 Entity Framework 時單元測試 ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)主題。

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>使用單元測試專案建立應用程式

您可以建立單元測試專案，建立您的應用程式時，或將單元測試專案加入至現有的應用程式。 本教學課程會示範這兩種方法來建立單元測試專案。 若要依照本教學課程，您可以使用兩種方法。

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a>建立應用程式時，加入單元測試專案

建立新 ASP.NET Web 應用程式名為**StoreApp**。

![建立專案](unit-testing-with-aspnet-web-api/_static/image1.png)

在 [新增 ASP.NET 專案] 視窗中，選取**空**範本加入資料夾和核心 Web api 的參考。 選取**加入單元測試**選項。 單元測試專案會自動命名**StoreApp.Tests**。 您可以保留這個名稱。

![建立單元測試專案](unit-testing-with-aspnet-web-api/_static/image2.png)

建立應用程式之後, 您會看到它包含兩個專案。

![兩個專案](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a>將單元測試專案加入至現有的應用程式

如果您未建立單元測試專案建立您的應用程式時，您可以新增一個在任何時間。 例如，假設您已經有一個名為 StoreApp，而且您想要加入單元測試。 若要加入單元測試專案，以滑鼠右鍵按一下您的方案，然後選取**新增**和**新專案**。

![將新專案加入方案](unit-testing-with-aspnet-web-api/_static/image4.png)

選取**測試**左的窗格中，然後選取**單元測試專案**專案類型。 將專案命名**StoreApp.Tests**。

![新增單元測試專案](unit-testing-with-aspnet-web-api/_static/image5.png)

您會看到 單元測試專案方案中。

在單元測試專案加入原始專案的專案參考。

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a>設定 Web API 2 應用程式

在您 StoreApp 的專案，加入至類別檔案**模型**資料夾名為**Product.cs**。 取代下列程式碼檔案的內容。

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

建置方案。

以滑鼠右鍵按一下 [控制器] 資料夾，然後選取**新增**和**新的 Scaffold 項目**。 選取**Web API 2 控制器-空白**。

![加入新的控制器](unit-testing-with-aspnet-web-api/_static/image6.png)

將控制器名稱設定為**SimpleProductController**，然後按一下**新增**。

![指定控制器](unit-testing-with-aspnet-web-api/_static/image7.png)

將現有的程式碼取代為下列程式碼。 若要簡化此範例中，資料會儲存在清單中，而不是資料庫。 此類別中定義之清單中代表實際執行資料。 請注意，控制器會包含一份產品物件當作參數的建構函式。 這個建構函式可讓您將測試資料時執行單元測試。 控制器也包含兩個**非同步**說明單元測試的非同步方法的方法。 這些非同步方法呼叫實作了**Task.FromResult**若要減少多餘的程式碼，但通常方法會加入資源密集作業。

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

GetProduct 方法傳回的執行個體**IHttpActionResult**介面。 IHttpActionResult 是其中一個在 Web API 2 中，新的功能，並簡化單元測試開發。 實作 IHttpActionResult 介面的類別位於[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)命名空間。 這些類別代表與動作的要求中的可能回應，而且它們對應到 HTTP 狀態碼。

建置方案。

現在您已經準備好設定測試專案。

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>在測試專案中安裝 NuGet 封裝

當您使用空白範本建立的應用程式時，單元測試專案 (StoreApp.Tests) 不包含任何已安裝的 NuGet 封裝。 其他範本，例如 Web API 範本，包括單元測試專案中的部分 NuGet 封裝。 此教學課程中，您必須包含在測試專案的 Microsoft ASP.NET Web API 2 核心封裝。

StoreApp.Tests 專案上按一下滑鼠右鍵，然後選取**管理 NuGet 封裝**。 您必須選取 StoreApp.Tests 專案加入至該專案的封裝。

![管理封裝](unit-testing-with-aspnet-web-api/_static/image8.png)

尋找並安裝 Microsoft ASP.NET Web API 2 核心封裝。

![安裝 web api core 套件](unit-testing-with-aspnet-web-api/_static/image9.png)

關閉 [管理 NuGet 封裝] 視窗。

<a id="tests"></a>
## <a name="create-tests"></a>建立測試

根據預設，您的測試專案包含名為 UnitTest1.cs 的空白測試檔案。 這個檔案會顯示您用於建立測試方法的屬性。 您的單元測試，您可以使用此檔案，或建立您自己的檔案。

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

此教學課程中，您將建立您自己的測試類別。 您可以刪除的 UnitTest1.cs 檔案。 將類別命名為**TestSimpleProductController.cs**，並以下列程式碼取代程式碼。

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>執行測試

現在您已經準備好要執行測試。 所有的方法，以標記**TestMethod**屬性將進行測試。 從**測試**功能表項目，執行測試。

![執行測試](unit-testing-with-aspnet-web-api/_static/image11.png)

開啟**測試總管**視窗中，並注意測試的結果。

![測試結果](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a>總結

您已經完成本教學課程。 將重點放在單元測試條件，已刻意簡化本教學課程中的資料。 單元測試資料的更進階的案例，請參閱[模擬 Entity Framework 時單元測試 ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md)。
