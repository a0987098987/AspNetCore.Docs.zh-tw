---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: 建立專案 |Microsoft Docs
author: Erikre
description: 本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Forms 應用程式的基本概念...
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: 81d3b07fa0710d03bc350b93a02f7d3aba2bb684
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814665"
---
<a name="create-the-project"></a>建立專案
====================
藉由[Erik Reitan](https://github.com/Erikre)

[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Forms 應用程式的基本概念。 Visual Studio 2013[含有 C# 原始程式碼專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附了本教學課程系列。


在本教學課程中，您將建立、 檢閱，和執行預設的專案在 Visual Studio 中，可讓您熟悉的 ASP.NET 功能。 此外，您將檢閱 Visual Studio 環境。

## <a name="what-youll-learn"></a>您將學到什麼：

- 如何建立新的 Web Form 專案。
- Web Form 專案檔案結構。
- 如何在 Visual Studio 中執行專案。
- 預設 Web forms 應用程式不同的功能。
- 有關如何使用 Visual Studio 環境的一些基本概念。

## <a name="creating-the-project"></a>建立專案

1. 開啟 Visual Studio。
2. 選取 **新的專案**從**檔案**Visual Studio 中的功能表。 

    ![建立專案-新的專案 功能表項目](create-the-project/_static/image1.png)
3. 選取 **範本** - &gt; **Visual C#**  - &gt; **Web**左側的 範本 群組。
4. 選擇**ASP.NET Web 應用程式**中間欄的範本。  
 本教學課程系列使用.NET Framework 4.5.2。
5. 命名您的專案*WingtipToys* ，然後選擇**確定** 按鈕。 

    ![建立專案-新增專案 對話方塊](create-the-project/_static/image2.png)

    > [!NOTE]
    > 在本教學課程系列中的專案名稱是**WingtipToys**。 建議使用這*確切*專案名稱，以便在整個教學課程系列提供的程式碼如預期般運作。

6. 按一下 [**變更驗證**] 按鈕。 選取 [**個別使用者帳戶**然後按一下**確定**] 按鈕。

7. 選取 [ **Web Form**範本，然後按一下**確定**] 按鈕。

    ![建立專案-新的專案範本](create-the-project/_static/image3.png)

專案將會需要一些時間來建立。 準備好時，開啟**Default.aspx**頁面。

![建立專案-新的專案範本](create-the-project/_static/image4.png)

您可以切換**設計**檢視和**來源**檢視所選取的選項，在 [中心] 視窗的底部。 **設計**檢視會顯示 ASP.NET 網頁、 主版頁面、 內容頁面、 HTML 網頁和使用者控制項使用 WYSIWYG 的檢視。 **來源**檢視會顯示您網頁上，您可以編輯的 HTML 標記。

> [!TIP] 
> 
> **了解 ASP.NET 架構**
> 
> ASP.NET Web Forms 可讓您使用熟悉的拖放、 事件驅動模型建置動態的網站。 在設計介面及數以百計的控制項和元件，可讓您快速建置複雜且功能強大 UI 導向網站的資料存取。 Wingtip 的 「 玩具 」 存放區根據 ASP.NET Web Form，但是許多您會了解在本教學課程系列中的概念也適用於所有 ASP.NET。
> 
> ASP.NET 提供四個主要的開發架構：
> 
> - [ASP.NET Web Form](../../../index.md)  
>  Web Form 架構為目標的開發人員偏好使用宣告式和控制項為基礎的程式設計，例如 Microsoft Windows Forms (WinForms) 和 WPF XAML / / Silverlight。 因此很熱門與開發人員尋找 web 程式開發的快速應用程式開發 (RAD) 環境，它會提供的 WYSIWYG 設計工具為導向的開發模型。 如果您是 web 程式設計的新手，並熟悉傳統 Microsoft RAD 用戶端開發工具 （例如，適用於 Visual Basic 和 Visual C#），您可以快速建立 web 應用程式，而不需要以 HTML 和 JavaScript 的經驗。
> - [ASP.NET MVC](../../../../mvc/index.md)  
>  ASP.NET MVC 為目標的開發人員感興趣的模式和測試導向開發、 關注點分離、 逆轉控制 (IoC) 和相依性插入 (DI) 準則。 此架構有助於區隔商務邏輯層，web 應用程式從其展示層。
> - [ASP.NET Web Pages](../../../../web-pages/index.md)  
>  ASP.NET 網頁為目標的開發人員想要簡單的 web 開發案例，像 PHP。 在網頁模型中，您會建立 HTML 網頁，然後在以動態方式控制如何呈現該標記的頁面新增伺服器端程式碼。 網頁專為輕量型的架構，並就知道 HTML，但可能沒有廣泛的程式設計體驗-例如人員、 學生或業餘開發人士於 ASP.NET 最簡單進入點。 它也是 web 開發人員知道 PHP 或類似的架構，可以開始使用 ASP.NET 的好方法。
> - [ASP.NET 單一頁面應用程式](../../../../single-page-application/index.md)  
>  ASP.NET 單一頁面應用程式 (SPA) 可協助您建置包含大量使用 HTML 5、 CSS 3 和 JavaScript 的用戶端互動的應用程式。 ASP.NET 和 Web 工具 2012.2 Update 提供新的範本，來建置單一頁面應用程式使用 knockout.js 和 ASP.NET Web API。 除了新的 SPA 範本，新的社群建立 SPA 範本是可供下載。
> 
> 除了四個主要的開發架構中，ASP.NET 也會提供一定要知道並熟悉，但未涵蓋在本教學課程系列中的其他技術：
> 
> - [ASP.NET Web API](../../../../web-api/index.md) -用於建置 HTTP 服務並擴及各種用戶端，包括瀏覽器和行動裝置的架構。
> - [ASP.NET SignalR](../../../../signalr/index.md) -簡化開發即時 web 功能的程式庫。


### <a name="reviewing-the-project"></a>檢閱專案

在 Visual Studio 中，**方案總管 中**視窗可讓您管理專案的檔案。 讓我們看看已新增至您的應用程式中的資料夾**方案總管 中**。 Web 應用程式範本中加入基本的資料夾結構：

![建立專案的方案總管](create-the-project/_static/image5.png)

Visual Studio 會建立一些初始的資料夾和檔案，為您的專案。 您會使用本教學課程稍後的第一個檔案如下所示：

| **檔案** | **目的** |
| --- | --- |
| *Default.aspx* | 通常第一次顯示的網頁瀏覽器中執行應用程式時。 |
| *Site.Master* | 頁面，可讓您建立應用程式中的頁面一致的版面配置和使用標準行為。 |
| *Global.asax* | 選用的檔案，其中包含 HTTP 模組或 ASP.NET 所引發的應用程式層級和工作階段層級事件回應的程式碼。 |
| *Web.config* | 應用程式的組態資料。 |

### <a name="running-the-default-web-application"></a>執行預設的 Web 應用程式

預設 Web 應用程式提供豐富的體驗，根據內建的功能和支援。 不會變更預設 Web form 專案中，應用程式已準備好在您本機的 Web 瀏覽器上執行的。

1. 按下***F5***鍵同時在 Visual Studio 中。   
 應用程式將會建置，並在您的 Web 瀏覽器中顯示。  

    ![建立專案-預設頁面](create-the-project/_static/image6.png)
2. 一旦您有已完成的檢閱執行中應用程式時，關閉瀏覽器視窗。

此預設 Web 應用程式中有三個主要的網頁： *Default.aspx* （首頁）， *about.aspx 的網頁*，並*Contact.aspx*。 這些頁面每一頁可以達到從上方導覽列中。 另外還有其他兩個帳戶的資料夾中所包含的頁面、 Register.aspx 頁面和 Login.aspx 頁面。 這兩個頁面可讓您使用 ASP.NET 成員資格功能來建立、 儲存及驗證使用者認證。

## <a name="aspnet-web-forms-background"></a>ASP.NET Web Form 背景

ASP.NET Web Form 是 Microsoft ASP.NET 技術，以動態方式在伺服器執行的程式碼會產生網頁輸出到瀏覽器或用戶端裝置為基礎的頁面。 ASP.NET Web Form 頁面自動轉譯功能，例如樣式、 配置和等等的正確符合瀏覽器的 HTML。 Web Form 是相容於任何.NET common language runtime，例如 Microsoft Visual Basic 和 Microsoft Visual C# 所支援的語言。 此外上, 建立 Web Form [Microsoft.NET Framework](https://msdn.microsoft.com/vstudio/aa496123)，其中提供的優點，例如受管理的環境、 型別安全和繼承。

ASP.NET Web Form 網頁執行時，頁面就會經歷週期，它會執行一系列的處理步驟。 這些步驟包括初始化時，具現化控制項、 還原和維護狀態、 執行事件處理常式程式碼，和呈現。 當您更熟悉 ASP.NET Web Form 的能力時，務必讓您了解[ASP.NET 網頁生命週期](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx)，讓您可以撰寫程式碼，在適當的生命週期階段，您想要的效果。

Web 伺服器收到頁面要求時，它發現頁面、 加以處理，將它傳送至瀏覽器，並接著會捨棄所有的頁面資訊。 如果使用者再次要求相同的頁面，伺服器就會重複整個序列，重新處理從零開始頁面。 換句話說，伺服器具有它有處理頁面沒有記憶體的頁面都是無狀態。 ASP.NET 網頁架構會自動處理維護您的頁面和它的控制項狀態的工作，並為您提供明確的方式維護狀態的應用程式特定資訊。

> [!TIP] 
> 
> **在 Web Form 應用程式範本中的 web 應用程式功能**
> 
> ASP.NET Web Forms 應用程式範本提供一組豐富的內建功能。 它不僅提供您*Home.aspx*頁面上， *about.aspx 的網頁*頁面上， *Contact.aspx*頁面上，但也包含 註冊使用者，並將儲存的成員資格功能他們的認證，讓他們可以登入您的網站。 本概觀會提供一些包含在 ASP.NET Web Forms 應用程式範本，以及如何在 Wingtip Toys 應用程式中使用它們的功能的詳細資訊。
> 
> **成員資格**
> 
> [ASP.NET](https://msdn.microsoft.com/library/yh26yfzy.aspx)身分識別會將您的使用者認證儲存在應用程式所建立的資料庫。 當您的使用者登入時，應用程式會驗證其認證，藉由讀取資料庫。 您的專案*帳戶*資料夾包含實作成員資格的各個部分的檔案： 註冊、 登入、 變更密碼，以及授權的存取。 此外，ASP.NET Web Form 支援 OAuth 和 OpenID。 這些驗證增強功能可讓使用者能夠登入您的網站使用這類帳戶，以及 Facebook、 Twitter、 Windows Live、 Google 中的現有認證。
> 
> ![建立專案的方案總管 （ASP.NET 身分識別）](create-the-project/_static/image7.png)
> 
> 根據預設，此範本會建立使用 SQL Server Express LocalDB，隨附於 Visual Studio Express 2013 for Web 的開發資料庫伺服器執行個體上的預設資料庫名稱的成員資格資料庫。
> 
> **SQL Server Express LocalDB**
> 
> [SQL Server Express LocalDB](https://technet.microsoft.com/library/hh510202.aspx)是輕量版的 SQL Server，有許多 SQL Server 資料庫的可程式性功能。 SQL Server Express LocalDB 會在使用者模式中執行，並已有一份安裝必要條件的快速的零設定安裝。 Microsoft SQL Server、 資料庫或 TRANSACT-SQL 程式碼中可移動從 SQL Server Express LocalDB 至 SQL Server 和 SQL Azure 不需要任何升級步驟。 因此，SQL Server Express LocalDB 可用做為開發人員環境為目標的 SQL Server 所有版本的應用程式。 SQL Server Express LocalDB 啟用功能，例如預存程序、 使用者定義函式和彙總、.NET Framework 的整合、 空間類型和其他人不在 SQL Server Compact。
> 
> **主版頁面**
> 
> [ASP.NET 主版頁面](https://msdn.microsoft.com/library/wtxbf3hh.aspx)定義一致的外觀和行為的所有頁面在您的應用程式。 主版頁面的版面配置合併為個別的內容頁，以產生使用者看到的最後一頁的內容。 在 Wingtip Toys 應用程式中，您會修改*Site.master*主版頁面，如此一來 Wingtip Toys 網站中的所有頁面都共用相同的特殊標誌和巡覽列。
> 
> **HTML5**
> 
> ASP.NET Web Forms 應用程式範本支援[HTML5](http://www.w3schools.com/html/html5_intro.asp)，這是 HTML 標記語言的最新版本。 HTML5 支援新的項目和功能，讓您更輕鬆地建立網站。
> 
> **Modernizr**
> 
> 不支援 HTML5 的瀏覽器，您可以使用[Modernizr](http://www.modernizr.com/)。 Modernizr 是可以在瀏覽器是否支援 HTML5 功能，並讓它們，如果沒有偵測到的開放原始碼 JavaScript 程式庫。 在 ASP.NET Web Forms 應用程式範本中，Modernizr 會安裝為 NuGet 套件。
> 
> **啟動程序**
> 
> Visual Studio 2013 專案範本會使用[Bootstrap](http://getbootstrap.com/)，由 Twitter 的版面配置和佈景主題的架構。 啟動程序會使用 CSS3 來提供表示配置可以動態地適應不同的瀏覽器視窗大小的回應式設計。 您也可以使用 Bootstrap 的佈景主題功能，輕鬆地達成應用程式的外觀與風格變更。 根據預設，Visual Studio 2013 中的 ASP.NET Web 應用程式範本會以 NuGet 套件形式包含啟動程序。
> 
> **NuGet 套件**
> 
> ASP.NET Web Forms 應用程式範本包含一組[NuGet](http://www.nuget.org/)封裝。 這些套件提供開放原始碼程式庫和工具的表單中的元件化的功能。 還有各種不同的套件，協助您建立和測試您的應用程式。 Visual Studio 可讓您更輕鬆地新增、 移除和更新 NuGet 套件。 開發人員可以建立和加入 NuGet 封裝也。
> 
> ![建立專案-NuGet 對話方塊](create-the-project/_static/image8.png)
> 
> 當您安裝套件時，NuGet 會將檔案複製到您的解決方案，並會自動將 需要的任何變更，例如新增參考，以及變更您的 Web 應用程式相關聯的組態。 如果您決定移除程式庫，NuGet 就會移除檔案，並反轉任何它所做的變更在您的專案，以便保留不混亂的情形。 NuGet 是可從**工具**Visual Studio 中的功能表。
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/)是快速且精簡 JavaScript 程式庫，可簡化 HTML 文件周遊、 事件處理、 加上動畫，以及快速的 web 開發的 Ajax 互動。 在 ASP.NET Web Forms 應用程式範本包含 jQuery JavaScript 程式庫 NuGet 套件的形式。
> 
> **低調驗證**
> 
> 內建的驗證程式控制項已設定為使用不顯眼的 JavaScript 用戶端驗證邏輯。 這會大幅減少的 JavaScript 內嵌在網頁標記中的轉譯，並減少整體的頁面大小。 低調驗證會加入全域設定中的 ASP.NET Web Forms 應用程式範本&lt;appSettings&gt;項目*Web.config*的應用程式根目錄的檔案。
> 
> **Entity Framework Code First**
> 
> Wingtip Toys 應用程式使用 ASP.NET Web Forms 應用程式範本中的功能，除了[Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx)，這是 NuGet 程式庫，當您使用資料時，可讓程式碼為主的開發。 簡單來說，它會建立您的應用程式，根據您所撰寫的程式碼為您的資料庫部分。 使用 Entity Framework，您可以擷取與操作資料當做強型別物件。 這可讓您專注在您的應用程式，而不是資料的存取方式的詳細資料的商務邏輯。
> 
> 如需有關已安裝的程式庫和套件包含 ASP.NET Web Forms 範本的詳細資訊，請參閱已安裝的 NuGet 套件的清單。 若要這樣做，請在 Visual Studio 中建立新的 Web Form 專案，選取**工具** - &gt; **程式庫套件管理員** - &gt; **管理方案的 NuGet 套件**，然後選取**已安裝的套件**中**管理 NuGet 套件** 對話方塊。


### <a name="touring-visual-studio"></a>Touring Visual Studio

在 Visual Studio 主視窗包含**方案總管 中**，則**伺服器總管**(**資料庫總管**Express 中)，則**屬性視窗**，則**工具箱**，則**工具列**，而**文件視窗**。

![建立專案-NuGet 對話方塊](create-the-project/_static/image9.png)

如需有關 Visual Studio 的詳細資訊，請參閱[視覺效果指南 Visual Web Developer](https://msdn.microsoft.com/library/ee410104.aspx)。

## <a name="summary"></a>總結

在本教學課程中，您已建立、 檢閱並執行預設的 Web Form 應用程式。 檢閱預設的 Web form 應用程式的不同功能並了解如何使用 Visual Studio 環境的一些基本概念。 在下列教學課程中，您將建立資料存取層。

## <a name="additional-resources"></a>其他資源

[選擇正確的程式設計模型](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[與網站專案的 web 應用程式專案](https://msdn.microsoft.com/library/dd547590.aspx)   
[ASP.NET Web Form 頁面概觀](https://msdn.microsoft.com/library/428509ah.aspx)

> [!div class="step-by-step"]
> [上一頁](introduction-and-overview.md)
> [下一頁](create_the_data_access_layer.md)
