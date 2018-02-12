---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: "呼叫 Web API，從 Windows Phone 8 應用程式 (C#) |Microsoft 文件"
author: rmcmurray
description: "建立完整的端對端案例，提供給 Windows Phone 8 應用程式的書籍目錄的 ASP.NET Web API 應用程式所組成。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2013
ms.topic: article
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 2025f31f369153b93cd293884880c97635fc8ab8
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2018
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a>從 Windows Phone 8 應用程式 (C#) 呼叫 Web API
====================
由[Robert McMurray](https://github.com/rmcmurray)

在本教學課程中，您將學習如何建立完整的端對端案例，提供給 Windows Phone 8 應用程式的書籍目錄的 ASP.NET Web API 應用程式所組成。

### <a name="overview"></a>總覽

Rest 式服務，例如 ASP.NET Web API 伺服器端和用戶端應用程式的架構，藉以簡化建立以 HTTP 為基礎的應用程式開發人員。 而不是建立專屬通訊端通訊協定，通訊，Web API 的開發人員只需要發行他們的應用程式必要的 HTTP 方法 (例如： GET、 POST、 PUT、 DELETE)，而且用戶端應用程式開發人員只需要使用所需的應用程式的 HTTP 方法。

在此端對端教學課程中，您將學習如何使用 Web API 來建立下列專案：

- 在[本教學課程的第一個部分](#STEP1)，您將建立來管理書籍目錄的建立、 讀取、 更新和刪除 (CRUD) 作業的所有支援的 ASP.NET Web API 應用程式。 此應用程式將會使用[範例 XML 檔案 (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx)從 MSDN。
- 在[本教學課程的第二個](#STEP2)，您將建立互動式的 Windows Phone 8 應用程式從您的 Web API 應用程式擷取資料。

#### <a name="prerequisites"></a>必要條件

- Visual Studio 2013 安裝的 Windows Phone 8 SDK
- Windows 8 或更新版本上安裝 HYPER-V 的 64 位元系統
- 如需其他需求，請參閱*系統需求*區段[Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471)下載頁面。

> [!NOTE]
> 如果您要測試 Web 應用程式開發介面和您的本機系統上的 Windows Phone 8 專案之間的連線，您必須依照*[連接到 Web API 的應用程式在本機上的 Windows Phone 8 模擬器電腦](https://go.microsoft.com/fwlink/?LinkId=324014)*文件，以設定測試環境。


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a>步驟 1： 建立 Web 應用程式開發介面 Bookstore 專案

本端對端教學課程的第一個步驟是建立支援的 CRUD 作業; 所有的 Web API 專案請注意，您會將 Windows Phone 應用程式專案加入這個方案中[步驟 2](#STEP2)本教學課程。

1. 開啟**Visual Studio 2013**。
2. 按一下**檔案**，然後**新**，然後**專案**。
3. 當**新專案** 對話方塊隨即顯示，接著展開**已安裝**，然後**範本**，然後**Visual C#**，，然後**Web**。

    | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
    | --- |
    | 按一下以展開的影像 |
4. 反白顯示**ASP.NET Web 應用程式**，輸入**BookStore**為專案名稱，然後按一下**確定**。
5. 當**新增 ASP.NET 專案**對話方塊隨即出現，請選取**Web API**範本，然後再按一下**確定**。

    | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
    | --- |
    | 按一下以展開的影像 |
6. 當 Web API 專案開啟時，請從專案移除範例控制器：

    1. 展開**控制器**在 [方案總管] 中的資料夾。
    2. 以滑鼠右鍵按一下**ValuesController.cs**檔案，然後再按一下**刪除**。
    3. 按一下**確定**當系統提示您確認刪除。
7. 將 XML 資料檔加入至 Web API 專案。這個檔案包含 bookstore 目錄的內容：

    1. 以滑鼠右鍵按一下**應用程式\_資料**資料夾在 方案總管 中，然後按一下**新增**，然後按一下 **新項目**。
    2. 當**加入新項目**對話方塊隨即出現，請反白**XML 檔案**範本。
    3. 將檔案命名**Books.xml**，然後按一下 **新增**。
    4. 當**Books.xml**開啟檔案、 程式碼檔案中的取代的範例 XML **books.xml** MSDN 上的檔案： 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
    5. 儲存並關閉 XML 檔案。
8. Bookstore 模型加入至 Web API 專案;此模型包含 bookstore 應用程式的建立、 讀取、 更新和刪除 (CRUD) 邏輯：

    1. 以滑鼠右鍵按一下**模型**資料夾在 方案總管 中，然後按一下**新增**，然後按一下 **類別**。
    2. 當**加入新項目**] 對話方塊隨即顯示，將類別檔案**BookDetails.cs**，然後按一下 [**新增**。
    3. 當**BookDetails.cs**開啟檔案、 檔案中的程式碼替換成下列： 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
    4. 儲存並關閉**BookDetails.cs**檔案。
9. 將 bookstore 控制器加入至 Web API 專案：

    1. 以滑鼠右鍵按一下**控制器**資料夾在 方案總管 中，然後按一下**新增**，然後按一下 **控制器**。
    2. 當**新增 Scaffold**對話方塊隨即出現，請反白**Web API 2 控制器空白**，然後按一下 **新增**。
    3. 當**加入控制器**] 對話方塊隨即顯示，接著控制器**BooksController**，然後按一下 [**新增**。
    4. 當**BooksController.cs**開啟檔案、 檔案中的程式碼替換成下列： 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
    5. 儲存並關閉**BooksController.cs**檔案。
10. 建置的 Web API 應用程式檢查有錯誤。

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a>步驟 2： 加入 Windows Phone 8 Bookstore 目錄專案

此端對端案例中的下一個步驟是建立 Windows Phone 8 目錄應用程式。 此應用程式將會使用*Windows Phone 的資料繫結應用程式*範本預設的使用者介面，而且會使用您在建立 Web API 應用程式[步驟 1](#STEP1)本教學課程做為資料來源。

1. 以滑鼠右鍵按一下**BookStore**方案中的在 方案總管 中，然後按一下 **新增**，然後**新專案**。
2. 當**新專案** 對話方塊隨即顯示，接著展開**已安裝**，然後**Visual C#**，然後**Windows Phone**。
3. 反白顯示**Windows Phone 的資料繫結應用程式**，輸入**BookCatalog**為名稱，然後按一下**確定**。
4. 將此 Json.NET NuGet 封裝加入**BookCatalog**專案：

    1. 以滑鼠右鍵按一下**參考**如**BookCatalog**專案在方案總管 中，然後按一下**管理 NuGet 封裝**。
    2. 當**管理 NuGet 封裝** 對話方塊隨即顯示，接著展開**線上**區段，並反白顯示**nuget.org**。
    3. 輸入**Json.NET**在搜尋 欄位，然後按一下搜尋圖示。
    4. 反白顯示**Json.NET**中搜尋結果中，然後按一下**安裝**。
    5. 當安裝完成後時，按一下 **關閉**。
5. 新增**BookDetails**模型到**BookCatalog**專案; 這個項目包含 bookstore 類別的一般模型：

    1. 以滑鼠右鍵按一下**BookCatalog**方案總管 中的專案，然後按一下 **新增**，然後按一下 **新資料夾**。
    2. 將新的資料夾命名**模型**。
    3. 以滑鼠右鍵按一下**模型**資料夾在 方案總管 中，然後按一下**新增**，然後按一下 **類別**。
    4. 當**加入新項目**] 對話方塊隨即顯示，將類別檔案**BookDetails.cs**，然後按一下 [**新增**。
    5. 當**BookDetails.cs**開啟檔案、 檔案中的程式碼替換成下列： 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
    6. 儲存並關閉**BookDetails.cs**檔案。
6. 更新**MainViewModel.cs**類別，以包含 BookStore Web API 應用程式進行通訊的功能：

    1. 展開**ViewModels**方案總管 中，然後按兩下資料夾**MainViewModel.cs**檔案。
    2. 當**MainViewModel.cs**開啟檔案時，取代為下列程式碼檔案中的; 請注意，您將需要更新的值`apiUrl`常數，其 Web API 的實際 URL: 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
    3. 儲存並關閉**MainViewModel.cs**檔案。
7. 更新**MainPage.xaml**檔案以自訂應用程式名稱：

    1. 按兩下**MainPage.xaml**在 [方案總管] 中的檔案。
    2. 當**MainPage.xaml**開啟檔案時，找出下列幾行程式碼： 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
    3. 以下列內容取代這些行： 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
    4. 儲存並關閉**MainPage.xaml**檔案。
8. 更新**DetailsPage.xaml**檔案來自訂顯示的項目：

    1. 按兩下**DetailsPage.xaml**在 [方案總管] 中的檔案。
    2. 當**DetailsPage.xaml**開啟檔案時，找出下列幾行程式碼： 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
    3. 以下列內容取代這些行： 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
    4. 儲存並關閉**DetailsPage.xaml**檔案。
9. 建置 Windows Phone 應用程式檢查有錯誤。

### <a name="step-3-testing-the-end-to-end-solution"></a>步驟 3： 測試端對端解決方案

中所述*必要條件*> 一節，本教學課程，您會測試 Web 應用程式開發介面和 Windows Phone 8 之間的連線時的專案在本機系統上，您就必須依照中的指示 *[連接到本機電腦上的 Web API 應用程式的 Windows Phone 8 模擬器](https://go.microsoft.com/fwlink/?LinkId=324014)*文件，以設定測試環境。

設定測試環境之後，您必須將 Windows Phone 應用程式設定為啟始專案。 若要這樣做，請反白顯示**BookCatalog**應用程式在方案總管 中，然後按一下**設定為啟始專案**:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| 按一下以展開的影像 |

當您按下 F5 時，Visual Studio 會啟動兩個 Windows Phone 模擬器中，將會顯示&quot;稍候&quot;訊息時的應用程式資料會從您的 Web API:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| 按一下以展開的影像 |

如果所有項目成功，您應該會看到顯示類別目錄：

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| 按一下以展開的影像 |

如果您點選任何書名，應用程式會顯示活頁簿描述：

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| 按一下以展開的影像 |

如果應用程式無法與您的 Web API 通訊，就會顯示錯誤訊息：

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| 按一下以展開的影像 |

如果您點選 錯誤訊息上，將會顯示任何錯誤的其他詳細資料：

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
| --- |
| 按一下以展開的影像 |
