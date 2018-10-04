---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: 模擬 Entity Framework 時的單元測試 ASP.NET Web API 2 |Microsoft Docs
author: tfitzmac
description: 本指南及應用程式示範如何建立單元測試，您會使用 Entity Framework 的 Web API 2 應用程式。 它示範如何修改...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 8945f913abe8fb8397d07a5994000fff2348f1f7
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795375"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>模擬 Entity Framework 時的單元測試 ASP.NET Web API 2
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

[下載已完成的專案](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> 本指南及應用程式示範如何建立單元測試，您會使用 Entity Framework 的 Web API 2 應用程式。 它會顯示如何修改 scaffold 的控制器，以便傳遞內容物件來進行測試，以及如何建立使用 Entity Framework 的測試物件。
>
> 使用 ASP.NET Web API 進行單元測試的簡介，請參閱 <<c0> [ 單元測試使用 ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md)。
>
> 本教學課程假設您已熟悉 ASP.NET Web API 的基本概念。 如需入門教學課程，請參閱 < [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)。
>
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - Web API 2

## <a name="in-this-topic"></a>本主題內容

此主題包括下列章節：

- [必要條件](#prereqs)
- [下載程式碼](#download)
- [使用單元測試專案建立應用程式](#appwithunittest)
- [建立模型類別](#modelclass)
- [新增控制器](#controller)
- [新增相依性插入](#dependency)
- [在測試專案中安裝 NuGet 套件](#testpackages)
- [建立測試內容](#testcontext)
- [建立測試](#tests)
- [執行測試](#runtests)

如果您已經完成中的步驟[單元測試使用 ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md)，您可以跳到 [] 區段[新增控制器](#controller)。

<a id="prereqs"></a>
## <a name="prerequisites"></a>必要條件

Visual Studio 2017 Community、 Professional 或 Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>下載程式碼

下載[已完成的專案](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)。 可下載的專案包含單元測試程式碼以及本主題中[單元測試 ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md)主題。

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>使用單元測試專案建立應用程式

您可以建立單元測試專案，建立您的應用程式時，或將單元測試專案新增至現有的應用程式。 本教學課程會示範建立應用程式時，建立單元測試專案。

建立名為新 ASP.NET Web 應用程式**StoreApp**。

在 [新增 ASP.NET 專案] 視窗中，選取**空**範本加入資料夾和核心 Web api 的參考。 選取 **加入單元測試**選項。 單元測試專案會自動命名**StoreApp.Tests**。 您可以保留此名稱。

![建立單元測試專案](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

之後建立的應用程式，您會看到它包含兩個專案- **StoreApp**並**StoreApp.Tests**。

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>建立模型類別

在您 StoreApp 專案中加入類別檔案，以**模型**名為資料夾**Product.cs**。 取代下列程式碼檔案的內容。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

建置方案。

<a id="controller"></a>
## <a name="add-the-controller"></a>新增控制器

以滑鼠右鍵按一下 [控制器] 資料夾，然後選取**新增**並**新增 Scaffold 項目**。 您可以選取 Web API 2 控制器與動作，使用 Entity Framework。

![新增控制器](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

設定下列的值：

- 控制器名稱： **ProductController**
- 模型類別：**產品**
- 資料內容類別: [選取**新的資料內容**按鈕，如下所示的值填入]

![指定控制器](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

按一下 **新增**自動產生的程式碼中建立控制站。 程式碼包含了建立、 擷取、 更新和刪除產品類別的執行個體方法。 下列程式碼顯示方法為新增的產品。 請注意此方法傳回的執行個體**IHttpActionResult**。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

IHttpActionResult 是其中一個新功能，在 Web API 2 中，而且它可簡化單元測試開發。

在下一步 區段中，您要自訂產生的程式碼，以便將測試物件傳遞到控制器。

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>新增相依性插入

目前，ProductController 類別是硬式編碼為使用 StoreAppContext 類別的執行個體。 若要修改您的應用程式，並移除該硬式編碼相依性，您將使用稱為 「 相依性插入模式。 藉由這個相依性，您可以測試時傳遞模擬 （mock） 物件中。

以滑鼠右鍵按一下**模型**資料夾，並新增名為的新介面**IStoreAppContext**。

使用下列程式碼取代程式碼。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

開啟 StoreAppContext.cs 檔案並進行下列醒目提示的變更。 要注意的重要變更如下：

- StoreAppContext 類別會實作 IStoreAppContext 介面
- 實作 MarkAsModified 方法


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

開啟 ProductController.cs 檔案。 變更現有的程式碼，以符合反白顯示的程式碼。 這些變更在 StoreAppContext 中斷相依性，並啟用以不同的物件中傳遞的內容類別的其他類別。 這項變更可讓您在單元測試期間通過的測試內容中。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

沒有一個詳細的變更，您必須在 ProductController。 在  **PutProduct**方法中，將實體狀態設定為的那一行修改 MarkAsModified 方法的呼叫取代。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

建置方案。

您現在已準備好要設定測試專案。

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>在測試專案中安裝 NuGet 套件

當您使用空白範本建立應用程式時，單元測試專案 (StoreApp.Tests) 不包含任何已安裝的 NuGet 套件。 其他範本，例如 Web API 範本中，包括單元測試專案中的某些 NuGet 套件。 本教學課程中，您必須包含 Entity Framework 套件和 Microsoft ASP.NET Web API 2 核心封裝至測試專案。

StoreApp.Tests 專案上按一下滑鼠右鍵，然後選取**管理 NuGet 套件**。 您必須選取 StoreApp.Tests 專案，將套件新增至該專案。

![管理套件](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

從線上封裝中，尋找並安裝 EntityFramework 封裝 （6.0 版或更新版本）。 如果有出現 EntityFramework 套件已安裝，您可能選取了 StoreApp 專案，而不是 StoreApp.Tests 專案。

![新增 Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

尋找並安裝 Microsoft ASP.NET Web API 2 核心套件。

![安裝 web api 的核心套件](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

關閉 [管理 NuGet 封裝] 視窗。

<a id="testcontext"></a>
## <a name="create-test-context"></a>建立測試內容

新增名為類別**TestDbSet**至測試專案。 這個類別做為您的測試資料集的基底類別。 使用下列程式碼取代程式碼。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

新增名為類別**TestProductDbSet**含有下列程式碼測試專案。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

新增名為類別**TestStoreAppContext**並以下列程式碼取代現有的程式碼。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>建立測試

根據預設，您的測試專案包含名為的空白測試檔案**UnitTest1.cs**。 此檔案會顯示您用來建立測試方法的屬性。 本教學課程中，您可以刪除此檔案，因為您將加入新的測試類別。

新增名為類別**TestProductController**至測試專案。 使用下列程式碼取代程式碼。

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>執行測試

您現在已準備好執行測試。 所有方法標記著**TestMethod**將測試屬性。 從**測試**功能表項目，執行測試。

![執行測試](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

開啟**測試總管** 視窗中，並注意測試的結果。

![測試結果](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
