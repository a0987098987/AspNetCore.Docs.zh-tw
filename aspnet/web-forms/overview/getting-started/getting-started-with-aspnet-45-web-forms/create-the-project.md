---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: 建立專案 |Microsoft 文件
author: Erikre
description: 此教學課程將告訴您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Form 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: b42e62b560e01d592c9f4cb61ea6199a15dc8bb6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="create-the-project"></a>建立專案
====================
由[Erik Reitan](https://github.com/Erikre)

[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 此教學課程將告訴您建置一個使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Form 應用程式的基本概念。 Visual Studio 2013[與 C# 原始程式碼的專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附此教學課程了。


本教學課程中您將建立、 檢閱並執行 Visual Studio，這可讓您熟悉的 ASP.NET 功能中的預設專案。 此外，您將檢閱 Visual Studio 環境。

## <a name="what-youll-learn"></a>您將學習：

- 如何建立新的 Web Form 專案。
- Web Form 專案檔案結構。
- 如何在 Visual Studio 中執行專案。
- 預設的 Web form 應用程式不同的功能。
- 有關如何使用 Visual Studio 環境的一些基本概念。

## <a name="creating-the-project"></a>建立專案

1. 開啟 Visual Studio。
2. 選取**新專案**從**檔案**Visual Studio 中的功能表。 

    ![建立專案的新專案 功能表項目](create-the-project/_static/image1.png)
3. 選取**範本** - &gt; **Visual C#**  - &gt; **Web**左側的 [範本] 群組。
4. 選擇**ASP.NET Web 應用程式**中心資料行中的範本。  
 此教學課程系列使用.NET Framework 4.5.2。
5. 命名您的專案*WingtipToys*選擇**確定** 按鈕。 

    ![建立專案-新增專案 對話方塊](create-the-project/_static/image2.png)

    > [!NOTE]
    > 在此教學課程系列中的專案名稱是**WingtipToys**。 建議使用此*確切*專案名稱，以便在整個教學課程系列提供的程式碼如預期般函式。
6. 接下來，選取**Web Form**範本，然後選擇 [**建立專案**] 按鈕。  

    ![新的專案範本建立專案-](create-the-project/_static/image3.png)

專案需要一些時間來建立。 準備好時，開啟**Default.aspx**頁面。

![新的專案範本建立專案-](create-the-project/_static/image4.png)

您可以切換**設計**檢視和**來源**選取選項 [中心] 視窗底部的檢視。 **設計**檢視會顯示 ASP.NET Web 網頁、 主版頁面、 內容頁面、 HTML 網頁和使用者控制項使用類似 WYSIWYG 的檢視。 **來源**檢視會顯示您網頁上，您可以編輯的 HTML 標記。

> [!TIP] 
> 
> **了解 ASP.NET 架構**
> 
> ASP.NET Web Forms 可讓您使用熟悉的拖放、 事件導向模型建置動態網站。 設計介面及數以百計的控制項和元件可讓您迅速建置存取資料的複雜且功能強大 UI 驅動網站。 Wingtip 玩具存放區根據 ASP.NET Web Form，但大部分您在此教學課程中學習的概念適用於所有的 ASP.NET。
> 
> ASP.NET 提供四個主要的開發架構：
> 
> - [ASP.NET Web Form](../../../index.md)  
>  Web Form 架構為目標的開發人員偏好宣告式和控制項為基礎的程式設計，例如 Microsoft Windows Forms (WinForms) 和 WPF/XAML/Silverlight。 它提供 WYSIWYG 設計工具為導向的開發模型，所以橫條圖通常與開發人員尋找 web 程式開發的快速應用程式開發 (RAD) 環境。 如果您是 web 程式設計的新手，並十分熟悉傳統 Microsoft RAD 用戶端開發工具 （例如，適用於 Visual Basic 和 Visual C#），您可以快速建立 web 應用程式，而不需要以 HTML 和 JavaScript 的體驗。
> - [ASP.NET MVC](../../../../mvc/index.md)  
>  ASP.NET MVC 為目標的開發人員感興趣的模式和測試為導向的開發、 重要性分離、 逆轉控制 (IoC)，與相依性插入 (DI) 等準則。 此架構有助於區隔其展示層 web 應用程式的商務邏輯層。
> - [ASP.NET Web Pages](../../../../web-pages/index.md)  
>  ASP.NET Web 網頁為目標的開發人員想要簡單的 web 開發劇本，沿著 PHP。 在網頁模型中，您會建立 HTML 網頁，然後將加入網頁伺服器端程式碼以動態方式控制該標記的呈現方式。 Web 網頁專為輕量型架構，而且 ASP.NET 的最簡單進入點的人知道 HTML，但是可能沒有廣泛的程式設計體驗-例如、 學生或之愛好者。 它也是針對 web 開發人員知道 PHP 或類似的架構，若要開始使用 ASP.NET 的好方法。
> - [ASP.NET 單一頁面應用程式](../../../../single-page-application/index.md)  
>  ASP.NET 單一頁面應用程式 (SPA) 可協助您建置包含大量使用 HTML 5、 CSS 3 和 JavaScript 的用戶端互動的應用程式。 ASP.NET 和 Web 工具 2012.2 更新隨附新的範本來建立使用解 knockout.js 和 ASP.NET Web API 的單一頁面應用程式。 除了新 SPA 範本，新的社群建立 SPA 範本也會提供下載。
> 
> 除了四個主要開發架構中，ASP.NET 也會提供一定要知道並熟悉，但未涵蓋在本教學課程系列的其他技術：
> 
> - [ASP.NET Web API](../../../../web-api/index.md) -建立連線的用戶端，包括瀏覽器和行動裝置的較大範圍的 HTTP 服務的架構。
> - [ASP.NET SignalR](../../../../signalr/index.md) -文件庫，以簡化開發即時 web 功能。


### <a name="reviewing-the-project"></a>檢閱專案

在 Visual Studio**方案總管 中**視窗可讓您管理之專案檔案。 讓我們看看已新增至您的應用程式中的資料夾**方案總管 中**。 Web 應用程式範本中加入基本資料夾結構：

![建立專案的方案總管](create-the-project/_static/image5.png)

Visual Studio 會建立一些初始資料夾和您的專案檔案。 您即將使用稍後在本教學課程的第一個檔案，如下所示：

| **檔案** | **目的** |
| --- | --- |
| *Default.aspx* | 通常是第一個顯示的網頁瀏覽器中執行應用程式時。 |
| *Site.Master* | 頁面，可讓您在您的應用程式中建立頁面一致配置與使用標準行為。 |
| *Global.asax* | 選擇性的檔案，包含回應 asp.net 或 HTTP 模組所引發的應用程式層級和工作階段層級事件的程式碼。 |
| *Web.config* | 應用程式的組態資料。 |

### <a name="running-the-default-web-application"></a>執行預設 Web 應用程式

預設 Web 應用程式提供豐富的體驗，根據內建功能和支援。 不需要任何變更預設的 Web form 專案，應用程式已準備好您的本機 Web 瀏覽器上執行的。

1. 按***F5***時 Visual Studio 中索引鍵。   
 應用程式將會建置，並在網頁瀏覽器中顯示。  

    ![建立專案-預設頁面](create-the-project/_static/image6.png)
2. 一旦完成的檢閱執行的應用程式，請關閉瀏覽器視窗。

此預設 Web 應用程式中有三個主要頁面： *Default.aspx* （主資料夾）， *about.aspx 的網頁*，和*Contact.aspx*。 每個網頁可以達到從上方導覽列中。 另外還有其他兩個帳戶資料夾中包含的頁面、 Register.aspx 頁面和 Login.aspx 頁面。 這兩個分頁可讓您建立、 儲存和驗證使用者認證來使用 ASP.NET 成員資格功能。

## <a name="aspnet-web-forms-background"></a>ASP.NET Web Form 背景

ASP.NET Web Form 是 Microsoft ASP.NET 技術，以動態方式在伺服器執行的程式碼會產生網頁瀏覽器或用戶端裝置的輸出為基礎的頁面。 ASP.NET Web Form 頁面自動轉譯功能，例如樣式、 配置和等等的正確瀏覽器相容的 HTML。 Web Form 的任何.NET common language runtime，例如 Microsoft Visual Basic 和 Microsoft Visual C# 所支援的語言與相容的。 此外，建立 Web Form [Microsoft.NET Framework](https://msdn.microsoft.com/vstudio/aa496123)，提供的優點，例如受管理的環境、 型別安全和繼承。

ASP.NET Web Form 網頁執行時，網頁就會經過週期，它會執行一系列處理步驟。 這些步驟包括初始化控制項具現化、 還原和維護狀態，執行事件處理常式程式碼，和呈現。 當您更熟悉的 ASP.NET Web Form 電源，很重要，以了解[ASP.NET 網頁生命週期](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx)，讓您可以撰寫程式碼，在適當的生命週期階段，您想要的效果。

當網頁伺服器收到頁面要求時，它會尋找頁面、 加以處理、 將它傳送至瀏覽器，，然後捨棄所有的頁面資訊。 如果使用者再次要求相同的頁面上，伺服器會重複整個序列中，重新處理包含全新頁面。 另一方面，伺服器擁有它有處理頁面沒有記憶體的頁面都是無狀態。 ASP.NET 網頁架構會自動處理維護您的頁面和其控制項的狀態的工作，並為您提供明確的方式來維護狀態的應用程式特定資訊。

> [!TIP] 
> 
> **Web Form 應用程式範本中的 web 應用程式功能**
> 
> ASP.NET Web Form 應用程式範本會提供一組豐富的內建功能。 它不只提供您*Home.aspx*  頁面上， *about.aspx 的網頁* 頁面上， *Contact.aspx*頁面上，但也包含註冊使用者，並將儲存的成員資格功能他們的認證，讓他們可以登入您的網站。 此概觀提供的一些功能包含在 ASP.NET Web Form 應用程式範本及 Wingtip Toys 應用程式中的使用方式的詳細資訊。
> 
> **成員資格**
> 
> [ASP.NET](https://msdn.microsoft.com/library/yh26yfzy.aspx)身分識別儲存在應用程式所建立的資料庫使用者的認證。 當您的使用者登入時，應用程式會藉由讀取資料庫驗證其認證。 您的專案*帳戶*資料夾包含實作的成員資格的各個部分的檔案： 註冊、 登入、 變更密碼，以及授權的存取。 此外，ASP.NET Web Form 支援 OAuth 和 OpenID。 這些驗證增強功能可讓使用者能夠登入您的網站使用現有的認證，從 Facebook、 Twitter、 Windows Live，和 Google 這類帳戶。
> 
> ![建立專案的方案總管 (ASP.NET Identity)](create-the-project/_static/image7.png)
> 
> 根據預設，範本會建立成員資格資料庫的 SQL Server Express LocalDB，隨附於 Visual Studio Express 2013 for Web 程式開發資料庫伺服器執行個體上使用預設的資料庫名稱。
> 
> **SQL Server Express LocalDB**
> 
> [SQL Server Express LocalDB](https://technet.microsoft.com/library/hh510202.aspx)是輕量型版本有許多可程式性功能的 SQL Server 資料庫的 SQL server。 SQL Server Express LocalDB 會在使用者模式中執行，並已安裝先決條件的簡短清單是快速的零設定安裝。 Microsoft SQL Server、 資料庫或 TRANSACT-SQL 程式碼中可以移動從 SQL Server Express LocalDB 至 SQL Server 和 SQL Azure 而不需要任何升級步驟。 因此，SQL Server Express LocalDB 可用做為開發人員環境的所有版本的 SQL Server 為都目標應用程式。 SQL Server Express LocalDB 啟用的功能，例如預存程序、 使用者定義函數和彙總、.NET Framework 整合、 空間類型與其他人不在 SQL Server Compact。
> 
> **主版頁面**
> 
> [ASP.NET 主版頁面](https://msdn.microsoft.com/library/wtxbf3hh.aspx)定義一致的外觀和行為的所有網頁應用程式中。 主版頁面的配置會利用個別的內容頁面上，以產生使用者會看到的最後一頁的內容合併。 Wingtip Toys 應用程式，在您修改*Site.master*主版頁面，讓 Wingtip Toys 網站中的所有網頁都共用相同的特殊標誌和巡覽列。
> 
> **HTML5**
> 
> ASP.NET Web Form 應用程式範本支援[HTML5](http://www.w3schools.com/html/html5_intro.asp)，這是最新版本的 HTML 標記語言。 HTML5 支援新的項目和功能，讓您輕鬆建立網站。
> 
> **Modernizr**
> 
> 不支援 HTML5 的瀏覽器，您可以使用[Modernizr](http://www.modernizr.com/)。 Modernizr 是開放原始碼 JavaScript 程式庫，可偵測到的瀏覽器是否支援 HTML5 功能，以及如果不啟用它們。 在 ASP.NET Web Form 應用程式範本中，Modernizr 是以 NuGet 套件安裝。
> 
> **Bootstrap**
> 
> Visual Studio 2013 專案範本使用[Bootstrap](http://getbootstrap.com/)，Twitter 所建立的配置和佈景主題的架構。 啟動程序會使用 CSS3 提供回應式設計，這表示配置可以動態的方式適應不同的瀏覽器視窗大小。 您也可以使用啟動程序的主題設定功能，以輕鬆地促使應用程式的外觀及操作中的變更。 根據預設，Visual Studio 2013 中的 ASP.NET Web 應用程式範本會以 NuGet 套件包含啟動程序。
> 
> **NuGet 封裝**
> 
> ASP.NET Web Form 應用程式範本包含一組[NuGet](http://www.nuget.org/)封裝。 這些套件提供開放原始碼程式庫和工具的表單中的元件化的功能。 沒有各種不同的封裝是為了協助您建立並測試您的應用程式。 Visual Studio 輕鬆地新增、 移除和更新 NuGet 套件。 開發人員可以建立並加入 NuGet 套件也。
> 
> ![建立專案-NuGet 對話方塊](create-the-project/_static/image8.png)
> 
> 當您安裝的套件時，NuGet 將檔案複製到您的方案，並會自動將所需的任何變更，例如將參考加入和變更您的 Web 應用程式相關聯的組態。 如果您決定移除媒體櫃，NuGet 會移除檔案，並會反轉任何它所做的變更您的專案，以便保留不混亂的情形。 NuGet 是可從**工具**Visual Studio 中的功能表。
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/)是快速且精簡 JavaScript 程式庫，可簡化 HTML 文件周遊、 事件處理、 建立動畫，以及快速 web 程式開發的 Ajax 互動。 JQuery JavaScript 程式庫是以 NuGet 套件包含 ASP.NET Web Form 應用程式範本中。
> 
> **不顯眼的驗證**
> 
> 內建的驗證程式控制項已設定為使用不顯眼的 JavaScript 用戶端驗證邏輯。 這會大幅減少呈現內嵌在網頁標記中的 JavaScript，並減少整體的頁面大小。 不顯眼的驗證全域加入 ASP.NET Web Form 應用程式範本中的設定為基礎&lt;appSettings&gt;元素*Web.config*應用程式根目錄的檔案。
> 
> **Entity Framework Code First**
> 
> Wingtip Toys 應用程式使用 ASP.NET Web Form 應用程式範本中的功能，除了[Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx)，這是 NuGet 程式庫的程式碼為主的開發時使用的資料。 簡而言之，它會建立您的應用程式，根據您所撰寫的程式碼為您的資料庫部分。 使用 Entity Framework，您可以擷取和操作資料當做強型別物件。 這可讓您專注在您的應用程式，而不是資料的存取方式的詳細資料的商務邏輯。
> 
> 如需有關已安裝的程式庫和套件包含 ASP.NET Web Form 範本的詳細資訊，請參閱 < 安裝的 NuGet 套件清單。 若要這樣做，請在 Visual Studio 中建立新的 Web Form 專案，選取**工具** - &gt; **程式庫套件管理員** - &gt; **管理NuGet Packages for Solution**，然後選取**安裝封裝**中**管理 NuGet 封裝** 對話方塊。


### <a name="touring-visual-studio"></a>Touring Visual Studio

Visual Studio 中的主視窗包含**方案總管 中**、**伺服器總管**(**資料庫總管**Express 中)，則**屬性視窗**、**工具箱**、**工具列**，而**文件視窗**。

![建立專案-NuGet 對話方塊](create-the-project/_static/image9.png)

如需 Visual Studio 的詳細資訊，請參閱[Visual 指南 Visual Web Developer](https://msdn.microsoft.com/library/ee410104.aspx)。

## <a name="summary"></a>總結

在本教學課程中，您已建立、 檢閱並執行預設的 Web Form 應用程式。 檢閱預設的 Web form 應用程式的不同功能並學習如何使用 Visual Studio 環境的一些基本概念。 在下列的教學課程中，您將建立資料存取層。

## <a name="additional-resources"></a>其他資源

[選擇正確的程式設計模型](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[與網站專案的 web 應用程式專案](https://msdn.microsoft.com/library/dd547590.aspx)   
[ASP.NET Web Form 頁面概觀](https://msdn.microsoft.com/library/428509ah.aspx)

> [!div class="step-by-step"]
> [上一頁](introduction-and-overview.md)
> [下一頁](create_the_data_access_layer.md)
