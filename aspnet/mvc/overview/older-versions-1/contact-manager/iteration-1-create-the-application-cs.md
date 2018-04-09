---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
title: '反覆項目 #1 – 建立應用程式 (C#) |Microsoft 文件'
author: microsoft
description: 在第一次反覆運算中，我們建立連絡人管理員簡單的方式可能。 我們加入基本資料庫作業的支援： 建立、 讀取、 更新和 D...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: db0f160b-901c-46d3-865e-7ab6cd4ed68d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
msc.type: authoredcontent
ms.openlocfilehash: 30f626511164363fea2195a05e73aeee5764933b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="iteration-1--create-the-application-c"></a>反覆項目 #1 – 建立應用程式 (C#)
====================
by [Microsoft](https://github.com/microsoft)

[下載程式碼](iteration-1-create-the-application-cs/_static/contactmanager_1_cs1.zip)

> 在第一次反覆運算中，我們建立連絡人管理員簡單的方式可能。 我們加入基本資料庫作業的支援： 建立、 讀取、 更新和刪除 (CRUD)。


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>建立連絡人管理 ASP.NET MVC 應用程式 (VB)

在這一系列的教學課程中，我們會建置整個連絡人管理應用程式從開始到完成。 請連絡系統管理員應用程式可讓您商店連絡資訊的名稱、 電話號碼和電子郵件地址-的人員清單。

我們會建置應用程式透過多個反覆項目。 與每個反覆項目，我們會逐漸改善應用程式。 這個多個反覆項目方法的目標是要讓您了解每個變更的原因。

- 反覆項目 #1-建立應用程式。 在第一次反覆運算中，我們建立連絡人管理員簡單的方式可能。 我們加入基本資料庫作業的支援： 建立、 讀取、 更新和刪除 (CRUD)。

- 反覆項目 #2-請看起來很棒的應用程式。 在這個反覆項目，我們可以改進應用程式的外觀修改 ASP.NET MVC 檢視主版頁面的預設值和階層式樣式表。

- 反覆項目 #3-加入表單驗證。 第三個反覆項目中，我們會加入基本表單驗證。 我們可以防止使用者提交表單，而不會完成必要的表單欄位。 此外，我們也會驗證電子郵件地址和電話號碼。

- 反覆項目 #4-請鬆散偶合的應用程式。 在此第三個反覆項目中，我們利用數種軟體設計模式，讓您更輕鬆地維護及修改連絡人管理員應用程式。 例如，我們可以重構應用程式使用的儲存機制模式和相依性插入模式。

- 反覆項目 #5-建立單元測試。 第五個反覆項目中，我們在我們的應用程式更輕鬆地維護及修改加入單元測試。 我們模擬資料模型類別，並建立單元測試，我們的控制器和驗證邏輯。

- 反覆項目 #6-使用測試為導向的開發。 這個第六個反覆項目中我們將新功能加入我們的應用程式藉由撰寫單元測試的第一次，並撰寫單元測試的程式碼。 在這個反覆項目，我們會加入連絡人群組。

- 反覆項目 #7： 加入 Ajax 功能。 在第七個反覆項目，改善回應性和效能的應用程式藉由新增 Ajax 支援。

## <a name="this-iteration"></a>這個反覆項目

此第一個反覆項目中，我們會建置基本的應用程式。 目標是要以最快速且最簡單的方式建置，請連絡管理員。 在稍後的反覆項目，我們可以改善應用程式的設計。

連絡人管理員應用程式是一個基本的資料庫驅動應用程式。 您可以使用應用程式來建立新的連絡人、 編輯現有的連絡人，以及刪除連絡人。

在這個反覆項目，我們會完成下列步驟：

1. ASP.NET MVC 應用程式
2. 建立資料庫來儲存我們連絡人
3. 產生資料庫與 Microsoft Entity Framework 的模型類別
4. 建立控制器動作和檢視，可以讓我們來列出所有資料庫中的連絡人
5. 建立控制器的動作和檢視，可讓我們在資料庫中建立新的連絡人
6. 建立控制器的動作，可讓我們來編輯現有連絡人資料庫中的檢視
7. 建立控制器的動作和檢視，可讓我們來刪除資料庫中現有的連絡人

## <a name="software-prerequisites"></a>軟體必要條件

在 ASP.NET MVC 應用程式，您必須有 Visual Studio 2008 或 Visual Web Developer 2008 （Visual Web Developer 中是不包含的所有進階功能的 Visual Studio 的 Visual Studio 的免費版本） 電腦上安裝。 您可以從下列網址下載試用版的 Visual Studio 2008 或 Visual Web Developer:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> 對於具有 Visual Web Developer 的 ASP.NET MVC 應用程式，您必須使用 Visual Web Developer 安裝 Service Pack 1。 無 Service Pack 1，您無法建立 Web 應用程式專案。


ASP.NET MVC 架構。 您可以從下列網址下載 ASP.NET MVC 架構：

[https://www.asp.net/mvc](../../../index.md)

在本教學課程中，我們可以使用 Microsoft Entity Framework 來存取資料庫。 Entity Framework 隨附於.NET Framework 3.5 Service Pack 1。 您可以從下列位置下載這個 service pack:

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

為執行每一個這些下載的替代方案，您可以利用 Web Platform Installer (Web PI)。 您可以從下列網址下載 Web PI:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>ASP.NET MVC Project

ASP.NET MVC Web 應用程式專案。 啟動 Visual Studio，然後選取功能表選項**檔案、 新的專案**。 **新專案**對話方塊隨即出現 （請參閱圖 1）。 選取**Web**專案類型和**ASP.NET MVC Web 應用程式**範本。 為新專案命名*ContactManager* ，然後按一下 [確定] 按鈕。


請確定您已從頂端的下拉式清單中選取的.NET Framework 3.5 右邊**新專案**對話方塊。 否則，贏了 t ASP.NET MVC Web 應用程式範本會出現。


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image1.jpg)](iteration-1-create-the-application-cs/_static/image1.png)

**圖 01**: 新增專案 對話方塊 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image2.png))


ASP.NET MVC 應用程式，**建立單元測試專案**對話方塊隨即出現。 您可以使用此對話方塊指出您要建立單元測試專案加入至您的方案，當您建立 ASP.NET MVC 應用程式。 雖然我們贏了 t 建置這個反覆項目中的單元測試，您應該選取選項**是，建立單元測試專案**由於我們想在稍後的反覆項目中加入單元測試。 加入測試專案，當您第一次建立新的 ASP.NET MVC 專案時就能輕鬆與 ASP.NET MVC 專案建立之後，加入測試專案。

> [!NOTE] 
> 
> 因為 Visual Web Developer 不支援測試專案，則不會收到建立單元測試專案 對話方塊時使用 Visual Web Developer。


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image2.jpg)](iteration-1-create-the-application-cs/_static/image3.png)

**圖 02**: 建立單元測試專案 對話方塊 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image4.png))


ASP.NET MVC 應用程式會出現在 [Visual Studio 方案總管] 視窗 （請參閱圖 3）。 如果您不要看到 方案總管 視窗，然後您可以選取功能表選項，以開啟此視窗**檢視、 方案總管 中**。 請注意，方案包含兩個專案： 在 ASP.NET MVC 專案和測試專案。 ASP.NET MVC 專案名為 ContactManager 和測試專案名為 ContactManager.Tests。


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image3.jpg)](iteration-1-create-the-application-cs/_static/image5.png)

**圖 03**: [方案總管] 視窗 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image6.png))


## <a name="deleting-the-project-sample-files"></a>刪除專案範例檔案

ASP.NET MVC 專案範本包含控制器和檢視的範例檔案。 建立新的 ASP.NET MVC 應用程式之前, 您應該刪除這些檔案。 您可以刪除檔案和資料夾在 [方案總管] 視窗中的檔案或資料夾上按一下滑鼠右鍵，然後選取功能表選項**刪除**。

您必須從 ASP.NET MVC 專案刪除下列檔案：

- \Controllers\HomeController.cs

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

此外，您需要從測試專案刪除下列檔案：

\Controllers\HomeControllerTest.cs

## <a name="creating-the-database"></a>建立資料庫

連絡人管理員應用程式是資料庫驅動的 web 應用程式。 我們可以使用資料庫來儲存連絡資訊。

ASP.NET MVC 架構與任何新型態的資料庫，包括 Microsoft SQL Server、 Oracle、 MySQL 和 IBM DB2 資料庫。 在本教學課程中，我們會使用 Microsoft SQL Server 資料庫。 當您安裝 Visual Studio 時，系統會提供安裝 Microsoft SQL Server Express 是免費版的 Microsoft SQL Server 資料庫的選項。

以滑鼠右鍵按一下應用程式建立新的資料庫\_Data 資料夾，在 [方案總管] 視窗，然後選取功能表選項**新增]、 [新項目**。 在**加入新項目**對話方塊中，選取**資料**類別目錄和**SQL Server 資料庫**範本 （請參閱圖 4）。 將新的資料庫 ContactManagerDB.mdf 命名，然後按一下 [確定] 按鈕。


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image4.jpg)](iteration-1-create-the-application-cs/_static/image7.png)

**圖 04**： 建立新的 Microsoft SQL Server Express 資料庫 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image8.png))


建立新的資料庫之後，資料庫會出現在應用程式中\_Data 資料夾，在 [方案總管] 視窗中的。 按兩下 ContactManager.mdf 檔案以開啟 [伺服器總管] 視窗，並連接到資料庫。

> [!NOTE] 
> 
> [伺服器總管] 視窗會呼叫在 Microsoft Visual Web Developer 的情況下的 [資料庫總管] 視窗。


若要建立新的資料庫物件，例如資料庫資料表、 檢視、 觸發程序和預存程序，您可以使用 [伺服器總管] 視窗。 以滑鼠右鍵按一下 [資料表] 資料夾，然後選取功能表選項**加入新的資料表**。 資料庫資料表設計工具隨即出現 （請參閱圖 5）。


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image5.jpg)](iteration-1-create-the-application-cs/_static/image9.png)

**圖 05**： 資料庫資料表設計工具 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image10.png))


我們需要建立包含下列資料行的資料表：

<a id="0.1_table01"></a>


| **資料行名稱** | **資料類型** | **允許 null 值** |
| --- | --- | --- |
| ID | int | False |
| FirstName | Nvarchar （50) | False |
| LastName | Nvarchar （50) | False |
| 電話 | Nvarchar （50) | False |
| Email | nvarchar （255) | False |


第一個資料行中，Id 資料行是特殊的。 您需要將識別碼資料行標示為識別資料行和主索引鍵資料行。 您指定的資料行是識別資料行的擴充資料行屬性 （圖 6 底部尋找），並捲動到 [識別規格] 屬性。 設定**（為識別）**屬性設為值**是**。

您將資料行標示為主索引鍵資料行選取資料行，然後按一下 索引鍵圖示的按鈕。 資料行標示為主索引鍵資料行之後，資料行旁邊會出現的索引鍵圖示 （請參閱圖 6）。

完成 建立資料表之後，按一下 儲存 按鈕 （具有磁碟片圖示的按鈕） 以儲存新的資料表。 將新的資料表命名*連絡人*。

完成建立連絡人資料庫資料表之後, 您應該加入一些記錄至資料表。 以滑鼠右鍵按一下 [伺服器總管] 視窗中的 [連絡人] 資料表，然後選取功能表選項**顯示資料表資料**。 在出現的方格中輸入一個或多個連絡人。

## <a name="creating-the-data-model"></a>建立資料模型

ASP.NET MVC 應用程式是由模型、 檢視和控制站所組成。 我們一開始建立模型類別，表示我們在上一節建立的 [連絡人] 資料表。

在本教學課程中，我們可以使用 Microsoft Entity Framework 從資料庫中自動產生的模型類別。

> [!NOTE] 
> 
> ASP.NET MVC 架構未繫結至 Microsoft 的 Entity Framework，以任何方式。 您可以使用 ASP.NET MVC 包括 NHibernate，LINQ to SQL 或 ADO.NET 的替代資料庫存取技術。


請遵循下列步驟來建立資料模型類別：

1. 在 [方案總管] 視窗中的 [模型] 資料夾上按一下滑鼠右鍵，然後選取**新增]、 [新項目**。 **加入新項目**對話方塊隨即出現 （請參閱圖 6）。
2. 選取**資料**類別目錄和**ADO.NET 實體資料模型**範本。 命名您的資料模型*ContactManagerModel.edmx*按一下**新增** 按鈕。 實體資料模型精靈 隨即出現 （請參閱圖 7）。
3. 在**選擇模型內容**步驟中，選取**從資料庫產生**（請參閱圖 7）。
4. 在**選擇資料連接**步驟選取 ContactManagerDB.mdf 資料庫，請輸入名稱*ContactManagerDBEntities* （請參閱圖 8） 的實體連接設定。
5. 在**選擇您的資料庫物件**步驟中，選取核取方塊標示為的資料表 （請參閱圖 9）。 資料模型會包含所有資料庫 （沒有其中一個，[連絡人] 資料表） 中所包含的資料表。 輸入的命名空間*模型*。 按一下 [完成] 按鈕以完成精靈。


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image6.jpg)](iteration-1-create-the-application-cs/_static/image11.png)

**圖 06**: 加入新項目對話方塊 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image12.png))


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image7.jpg)](iteration-1-create-the-application-cs/_static/image13.png)

**圖 07**： 選擇模型內容 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image14.png))


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image8.jpg)](iteration-1-create-the-application-cs/_static/image15.png)

**圖 08**： 選擇資料連接 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image16.png))


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image9.jpg)](iteration-1-create-the-application-cs/_static/image17.png)

**圖 09**： 選擇您的資料庫物件 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image18.png))


實體資料模型精靈完成之後，Entity Data Model Designer 隨即出現。 設計工具會在建立模型的每個資料表顯示對應的類別。 您應該會看到一個名為連絡人的類別。

實體資料模型精靈會產生根據資料庫資料表名稱的類別名稱。 您幾乎都需要變更由精靈所產生的類別名稱。 以滑鼠右鍵按一下連絡人類別設計工具中的，然後選取功能表選項**重新命名**。 連絡人 （單數） 中變更的連絡人 （複數） 的類別名稱。 變更類別名稱之後，類別應該類似於圖 10。


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image10.jpg)](iteration-1-create-the-application-cs/_static/image19.png)

**圖 10**: 連絡人類別 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image20.png))


此時，我們已經建立我們資料庫模型。 我們可以使用連絡人類別來代表在資料庫中特定的連絡人記錄。

## <a name="creating-the-home-controller"></a>建立主控制器

下一個步驟是建立我們主控制器。 主控制器是在 ASP.NET MVC 應用程式中叫用的預設控制器。

在 [方案總管] 視窗中的 [控制器] 資料夾上按一下滑鼠右鍵，然後選取功能表選項來建立首頁控制器類別**新增]、 [控制站**（請參閱圖 11）。 請注意標示的核取方塊**將動作方法，如 Create、 Update 和 Details 案例加入**。 請確定此核取方塊已選取後，再按一下**新增** 按鈕。


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image11.jpg)](iteration-1-create-the-application-cs/_static/image21.png)

**圖 11**： 加入主控制器 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image22.png))


當您建立主控制器時，您會取得列表 1 中的類別。

**列出 1-Controllers\HomeController.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample1.cs)]

## <a name="listing-the-contacts"></a>列出連絡人

若要顯示在 [連絡人] 資料庫資料表中的記錄，我們要建立 index 動作和索引檢視。

主控制器已經包含 index （） 動作。 我們需要修改這個方法，讓它看起來像是列出 2。

**Listing 2 - Controllers\HomeController.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample2.cs)]

請注意清單 2 Home 控制器類別包含名為私用欄位\_實體。 \_實體欄位代表實體資料模型中。 我們使用\_實體欄位以與資料庫通訊。

Index （） 方法會傳回代表的所有連絡人連絡人資料庫資料表中的檢視。 運算式\_實體。ContactSet.ToList() 傳回泛型清單的連絡人清單。

現在我們已建立索引控制器，接下來我們要建立索引檢視。 之前建立索引檢視時，請選取功能表選項，您的應用程式編譯**建置時，建置方案**。 您永遠應該編譯您的專案之前加入的檢視模型類別清單中顯示的順序**加入檢視**對話方塊。

在建立索引檢視的 index （） 方法上按一下滑鼠右鍵，然後選取功能表選項**加入檢視**（請參閱圖 12）。 選取此功能表選項會開啟**加入檢視**對話方塊 （請參閱圖 13）。


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image12.jpg)](iteration-1-create-the-application-cs/_static/image23.png)

**圖 12**： 加入索引檢視 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image24.png))


在**加入檢視** 對話方塊中，選取核取方塊標示為**建立強型別檢視**。 選取檢視資料類別 ContactManager.Models.Contact 和檢視的內容清單。 選取這些選項會產生一個檢視，顯示一份連絡人記錄。


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image13.jpg)](iteration-1-create-the-application-cs/_static/image25.png)

**圖 13**: 新增檢視對話方塊 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image26.png))


當您按一下**新增**產生按鈕、 列表 3 中的索引檢視。 請注意&lt;%@ 頁面 %&gt;會出現在檔案頂端的指示詞。 索引檢視繼承自 ViewPage&lt;IEnumerable&lt;ContactManager.Models.Contact&gt; &gt;類別。 換句話說，在檢視中的模型類別代表連絡人實體的清單。

索引檢視的主體會包含 「 foreach 迴圈 」 可逐一查看每個模型類別所代表的連絡人。 連絡人類別的每個屬性的值會顯示在 HTML 資料表中。

**列出 3-Views\Home\Index.aspx （未修改）**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample3.aspx)]

我們需要進行一個修改的索引檢視。 因為我們不會建立詳細資料檢視，我們可以移除詳細資料 連結。 尋找並移除索引檢視中的下列程式碼：

{id = 項目。識別碼}） %&gt;

修改索引檢視表之後，您可以執行連絡人管理員應用程式。 選取 開始偵錯功能表選項偵錯，或直接按 F5。 第一次執行應用程式，您會取得對話方塊圖 14。 選取的選項**修改 Web.config 檔案，以啟用偵錯**，然後按一下 [確定] 按鈕。


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image14.jpg)](iteration-1-create-the-application-cs/_static/image27.png)

**圖 14**： 啟用偵錯 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image28.png))


預設會傳回索引檢視。 這個檢視會列出所有的連絡人資料庫資料表中的資料 （請參閱圖 15）。


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image15.jpg)](iteration-1-create-the-application-cs/_static/image29.png)

**圖 15**: 索引檢視 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image30.png))


請注意，索引檢視包含標示為 建立新檢視底部的連結。 在下一步 區段中，您可以了解如何建立新的連絡人。

## <a name="creating-new-contacts"></a>建立新的連絡人

若要讓使用者能夠建立新的連絡人，我們需要將兩個 create （） 動作加入至主控制器。 我們需要建立一個 create （） 動作傳回 HTML 表單建立新的連絡人。 我們需要建立會執行實際的資料庫插入新連絡人的第二個 create （） 動作。

我們需要加入至主控制器的新 create （） 方法都包含在列出的 4。

**列出 4-Controllers\HomeController.cs （與建立方法）**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample4.cs)]

可以使用 HTTP GET 叫用第一個 create （） 方法，而第二個 create （） 方法可以只由 HTTP POST 叫用。 換句話說，只有在將 HTML 表單張貼時，第二個 create （） 方法就可以叫用。 第一個 create （） 方法只會傳回包含用於建立新連絡人的 HTML 表單的檢視。 第二個 create （） 方法會更感興趣： 它在資料庫中加入新的連絡人。

請注意，第二個 create （） 方法已修改成接受連絡人類別的執行個體。 從 HTML 表單張貼的表單值會繫結至這個連絡人類別由 ASP.NET MVC 架構會自動。 建立 HTML 表單中的每個表單欄位已指派給連絡人參數的屬性。

請注意，使用 [繫結] 屬性裝飾連絡人參數。 [繫結] 屬性用來從繫結中排除的連絡人 Id 屬性。 因為 Id 屬性代表身分識別屬性，我們不想要設定的 Id 屬性。

在 create （） 方法的主體，Entity Framework 用於資料庫中插入新的連絡人。 新的連絡人會加入至現有的連絡人集，並呼叫 savechanges （） 方法來將這些變更回推送到基礎資料庫。

您可以產生 HTML 表單任一 create （） 方法上按一下滑鼠右鍵，然後選取功能表選項以建立新的連絡人的**加入檢視**（請參閱圖 16）。


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image16.jpg)](iteration-1-create-the-application-cs/_static/image31.png)

**圖 16**： 加入建立檢視 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image32.png))


在**加入檢視**對話方塊中，選取**ContactManager.Models.Contact**類別和**建立**檢視內容的選項 （請參閱圖 17）。 當您按一下**新增**按鈕，建立檢視時自動產生。


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image17.jpg)](iteration-1-create-the-application-cs/_static/image33.png)

**圖 17**： 看到分裂頁面 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image34.png))


建立檢視的每一個連絡人類別的屬性包含表單欄位。 建立檢視的程式碼會包含在列出 5。

**列出 5-Views\Home\Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample5.aspx)]

修改 create （） 方法，並加入建立檢視之後，您可以執行連絡人管理員應用程式，並建立新的連絡人。 按一下**新建**出現在索引檢視中瀏覽至建立檢視的連結。 您應該會看到圖 18 中的檢視。


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image18.jpg)](iteration-1-create-the-application-cs/_static/image35.png)

**圖 18**: Create View ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image36.png))


## <a name="editing-contacts"></a>編輯連絡人

加入編輯連絡人的記錄功能是非常類似於將建立新的連絡人記錄功能。 首先，我們需要將兩個新的編輯方法加入至主控制器類別。 這些新 Edit() 方法都包含在程式碼範例 6。

**列出 6-Controllers\HomeController.cs （與編輯方法）**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample6.cs)]

第一個 Edit() 方法叫用 HTTP GET 作業。 Id 參數被傳遞給這個方法可代表正在編輯的連絡記錄的識別碼。 Entity Framework 用來擷取連絡人的比對的識別碼。會傳回包含編輯一筆記錄之 HTML 表單的檢視。

第二個 Edit() 方法會執行實際更新到資料庫。 這個方法會接受連絡人類別的執行個體做為參數。 ASP.NET MVC 架構會繫結表單欄位的編輯表單從這個類別自動。 編輯的連絡人 （我們需要的 Id 屬性的值） 時，請注意，您不 t 會包含 [繫結] 屬性。

Entity Framework 用來儲存至資料庫修改過的連絡人。 必須從資料庫第一次擷取原始的連絡人。 接下來，Entity Framework ApplyPropertyChanges() 方法稱為記錄的變更至連絡人。 最後，Entity Framework savechanges （） 方法稱為保存至基礎資料庫變更。

您可以產生包含 Edit() 方法上按一下滑鼠右鍵，然後選取 [新增檢視] 功能表選項的編輯表單的檢視。 在 [新增檢視] 對話方塊中，選取**ContactManager.Models.Contact**類別和**編輯**檢視內容 （請參閱圖 19）。


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image19.jpg)](iteration-1-create-the-application-cs/_static/image37.png)

**圖 19**： 加入編輯檢視 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image38.png))


當您按一下 [新增] 按鈕時，會自動產生新的編輯檢視。 產生的 HTML 表單包含欄位對應至每個連絡人類別 （請參閱程式碼範例 7） 的屬性。

**列出 7-Views\Home\Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>刪除連絡人

如果您想要刪除的連絡人，則您需要將兩個 delete 動作加入至主控制器類別。 第一個 delete 動作會顯示刪除確認表單。 第二個 delete 動作會執行實際的刪除。

> [!NOTE] 
> 
> 稍後，在反覆項目 #7 中，我們修改連絡管理員，以支援 Ajax 刪除一個步驟。


列出 8 中包含兩個新的 delete （） 方法。

**列出 8-Controllers\HomeController.cs （Delete 方法）**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample8.cs)]

第一個 delete （） 方法會傳回刪除連絡人記錄，從資料庫確認表單 （請參閱 Figure20）。 第二個 delete （） 方法會執行實際的刪除作業對資料庫。 從資料庫中擷取原始的連絡人之後，Entity Framework DeleteObject() 和 savechanges （） 方法會呼叫以執行資料庫刪除。


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image20.jpg)](iteration-1-create-the-application-cs/_static/image39.png)

**圖 20**： 刪除確認檢視 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image40.png))


我們要修改索引檢視，使其包含的連結，刪除 （請參閱圖 21） 的連絡人記錄。 您需要將下列程式碼加入至相同的資料表儲存格，其中包含編輯連結：

Html.ActionLink( { id=item.Id }) %&gt;


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image21.jpg)](iteration-1-create-the-application-cs/_static/image41.png)

**圖 21**： 索引檢視與編輯連結 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image42.png))


接下來，我們需要建立刪除確認檢視。 在首頁控制器類別中的 delete （） 方法上按一下滑鼠右鍵，然後選取 [新增檢視] 功能表選項。 [新增檢視] 對話方塊隨即出現 （請參閱圖 22）。

與不同的是如果清單中，建立、 和編輯檢視中，加入檢視 對話方塊不會包含建立刪除檢視的選項。 相反地，選取**ContactManager.Models.Contact**資料類別和**空**檢視內容。 選取空的檢視內容 選項將要求我們建立自己的檢視。


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image22.jpg)](iteration-1-create-the-application-cs/_static/image43.png)

**圖 22**： 加入刪除確認檢視 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image44.png))


刪除檢視的內容會包含在列出 9。 這個檢視包含表單，以確認是否應為特定連絡人刪除 （請參閱圖 21）。

**列出 9-Views\Home\Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>變更預設的控制站的名稱

它可能會對您造成困擾的控制器類別的名稱使用連絡人的名稱是 HomeController 類別。 應該不 t 控制器命名為 ContactController 嗎？

此問題很容易修正。 首先，我們需要重構主控制器的名稱。 在 Visual Studio 程式碼編輯器中開啟 HomeController 類別，以滑鼠右鍵按一下類別的名稱並選取功能表選項**重構，重新命名**。 選取此功能表選項會開啟 [重新命名] 對話方塊。


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image23.jpg)](iteration-1-create-the-application-cs/_static/image45.png)

**圖 23**： 重構的控制器名稱 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image46.png))


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image24.jpg)](iteration-1-create-the-application-cs/_static/image47.png)

**圖 24**： 使用重新命名對話方塊 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image48.png))


如果您重新命名控制器類別時，Visual Studio 會更新檢視 資料夾中資料夾的名稱。 Visual Studio 會將 \Views\Home 資料夾重新命名 \Views\Contact 資料夾。

進行這項變更之後，您的應用程式將不再有主控制器。 當您執行您的應用程式時，您會取得在圖 25 的錯誤頁面。


[![新增專案 對話方塊](iteration-1-create-the-application-cs/_static/image25.jpg)](iteration-1-create-the-application-cs/_static/image49.png)

**圖 25**： 預設的控制站 ([按一下以檢視完整大小的影像](iteration-1-create-the-application-cs/_static/image50.png))


我們需要更新使用連絡控制器，而不是主控制器的 Global.asax 檔中的預設路由。 開啟 Global.asax 檔案並修改預設路由 （請參閱列出 10） 所使用的預設控制站。

**列出 10-Global.asax.cs**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample10.cs)]

進行這些變更之後，請連絡管理員將會正確執行。 現在，它會使用連絡人控制器類別做為預設的控制站。

## <a name="summary"></a>總結

此第一個反覆項目中基本的連絡人管理員應用程式中建立最快速的方式可能。 我們已利用 Visual Studio 自動產生我們控制器和檢視表之初始程式碼。 我們也已利用 Entity Framework 自動產生資料庫模型類別。

目前，我們可以清單、 建立、 編輯和刪除連絡人記錄，請連絡系統管理員應用程式使用。 換句話說，我們可以執行所有資料庫驅動的 web 應用程式所需的基本資料庫作業。

不幸的是，我們的應用程式有一些問題。 第一個與我遲疑著要不要承認，不過，請連絡系統管理員應用程式不是最具吸引力的應用程式。 它需要一些設計工作。 中的下一個反覆項目中，我們將探討如何變更預設檢視主版頁面和階層式樣式表可以改善應用程式的外觀。

其次，我們未實作的任何表單驗證。 例如，沒有任何可防止您在建立連絡人表單提交不需要輸入任何為表單欄位值。 此外，您可以輸入不正確的電話號碼和電子郵件地址。 我們一開始解決問題的反覆項目 #3 中的表單驗證。

最後，並最重要的是，請連絡系統管理員應用程式的目前反覆項目無法輕鬆地修改或維護。 例如，將資料庫存取邏輯被本意右邊控制器動作。 這表示，我們無法修改資料存取程式碼，而不需修改我們控制站。 在稍後的反覆項目，我們會探討軟體設計樣式是我們可以實作以進行更有彈性，若要變更連絡管理員。

> [!div class="step-by-step"]
> [下一步](iteration-2-make-the-application-look-nice-cs.md)
