---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: 模擬 Entity Framework 時單元測試 ASP.NET Web API 2 |Microsoft 文件
author: tfitzmac
description: 本指南及應用程式示範如何建立單元測試，您使用 Entity Framework 的 Web API 2 應用程式。 它會顯示如何修改...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: abfde7edec85812de3560f4edefb110c3e374580
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152861"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>模擬 Entity Framework 時單元測試 ASP.NET Web API 2
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

[下載完成的專案](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> 本指南及應用程式示範如何建立單元測試，您使用 Entity Framework 的 Web API 2 應用程式。 它會顯示如何修改 scaffold 的控制器啟用傳遞內容物件來進行測試，以及如何建立測試物件可搭配 Entity Framework。
> 
> 如需單元測試的 ASP.NET Web API 的簡介，請參閱[單元測試的 ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md)。
> 
> 本教學課程假設您熟悉的 ASP.NET Web API 的基本概念。 如需入門教學課程，請參閱[開始使用 ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)。
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
- [建立模型類別](#modelclass)
- [加入控制器](#controller)
- [新增相依性插入](#dependency)
- [在測試專案中安裝 NuGet 封裝](#testpackages)
- [建立測試內容](#testcontext)
- [建立測試](#tests)
- [執行測試](#runtests)

如果您已經完成中的步驟[單元測試的 ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md)，您可以略過區段[加入控制器](#controller)。

<a id="prereqs"></a>
## <a name="prerequisites"></a>必要條件

Visual Studio 2017 Community、 Professional 或 Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>下載程式碼

下載[已完成的專案](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)。 可下載專案中包含單元測試程式碼為本主題和[單元測試 ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md)主題。

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>使用單元測試專案建立應用程式

您可以建立單元測試專案，建立您的應用程式時，或將單元測試專案加入至現有的應用程式。 本教學課程會示範建立單元測試專案，建立應用程式時。

建立新 ASP.NET Web 應用程式名為**StoreApp**。

在 [新增 ASP.NET 專案] 視窗中，選取**空**範本加入資料夾和核心 Web api 的參考。 選取**加入單元測試**選項。 單元測試專案會自動命名**StoreApp.Tests**。 您可以保留這個名稱。

![建立單元測試專案](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

建立應用程式之後, 您會看到它包含兩個專案- **StoreApp**和**StoreApp.Tests**。

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>建立模型類別

在您 StoreApp 的專案，加入至類別檔案**模型**資料夾名為**Product.cs**。 取代下列程式碼檔案的內容。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

建置方案。

<a id="controller"></a>
## <a name="add-the-controller"></a>加入控制器

以滑鼠右鍵按一下 [控制器] 資料夾，然後選取**新增**和**新的 Scaffold 項目**。 選取 Web API 2 控制器使用 Entity Framework 執行動作。

![加入新的控制器](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

設定下列的值：

- 控制器名稱： **ProductController**
- 模型類別：**產品**
- 資料內容類別: [選取**新的資料內容**按鈕，如下所示的值填入]

![指定控制器](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

按一下**新增**以自動產生的程式碼建立控制器。 程式碼包含了建立、 擷取、 更新及刪除產品類別的執行個體方法。 下列程式碼示範的方法為將產品。 請注意，方法會傳回的執行個體**IHttpActionResult**。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

IHttpActionResult 是其中一個在 Web API 2 中，新的功能，並簡化單元測試開發。

在下一步 區段中，您將自訂產生的程式碼，以便將測試物件傳遞至控制器。

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>新增相依性插入

目前，ProductController 類別是硬式編碼使用 StoreAppContext 類別的執行個體。 若要修改您的應用程式，並移除該硬式編碼相依性，您將使用模式，呼叫相依性插入。 中斷此相依性之後，您可以測試時傳遞模擬物件中。

以滑鼠右鍵按一下**模型**資料夾，並加入新的介面，名為**IStoreAppContext**。

使用下列程式碼取代程式碼。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

開啟 StoreAppContext.cs 檔案並進行下列反白顯示的變更。 要注意的重要變更會：

- StoreAppContext 類別會實作 IStoreAppContext 介面
- 實作 MarkAsModified 方法


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

開啟 ProductController.cs 檔案。 變更現有的程式碼，以符合反白顯示的程式碼。 這些變更在 StoreAppContext 中斷相依性，讓其他類別，不同物件中傳遞的內容類別。 這項變更可讓您在單元測試期間在測試內容中傳遞。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

沒有一個詳細的變更，您必須在 ProductController。 在**PutProduct**方法中，實體狀態設定為的程式修改 MarkAsModified 方法呼叫取代。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

建置方案。

現在您已經準備好設定測試專案。

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>在測試專案中安裝 NuGet 封裝

當您使用空白範本建立的應用程式時，單元測試專案 (StoreApp.Tests) 不包含任何已安裝的 NuGet 封裝。 其他範本，例如 Web API 範本，包括單元測試專案中的部分 NuGet 封裝。 此教學課程中，您必須包含 Entity Framework 封裝時，發生與 Microsoft ASP.NET Web API 2 核心封裝至測試專案。

StoreApp.Tests 專案上按一下滑鼠右鍵，然後選取**管理 NuGet 封裝**。 您必須選取 StoreApp.Tests 專案加入至該專案的封裝。

![管理封裝](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

從線上套件中，尋找並安裝 EntityFramework 封裝 （6.0 版或更新版本）。 如果似乎已經安裝了 EntityFramework 封裝時，您可能選取了 StoreApp 專案，而不是 StoreApp.Tests 專案。

![加入實體架構](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

尋找並安裝 Microsoft ASP.NET Web API 2 核心封裝。

![安裝 web api core 套件](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

關閉 [管理 NuGet 封裝] 視窗。

<a id="testcontext"></a>
## <a name="create-test-context"></a>建立測試內容

將類別命名為**TestDbSet**至測試專案。 這個類別可做為您的測試資料集的基底類別。 使用下列程式碼取代程式碼。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

將類別命名為**TestProductDbSet**含有下列程式碼測試專案。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

將類別命名為**TestStoreAppContext** ，並以下列程式碼取代現有的程式碼。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>建立測試

根據預設，您的測試專案包含名為的空白測試檔案**UnitTest1.cs**。 這個檔案會顯示您用於建立測試方法的屬性。 此教學課程中，您可以刪除此檔案，因為您會新增新的測試類別。

將類別命名為**TestProductController**至測試專案。 使用下列程式碼取代程式碼。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>執行測試

現在您已經準備好要執行測試。 所有的方法，以標記**TestMethod**屬性將進行測試。 從**測試**功能表項目，執行測試。

![執行測試](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

開啟**測試總管**視窗中，並注意測試的結果。

![測試結果](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
