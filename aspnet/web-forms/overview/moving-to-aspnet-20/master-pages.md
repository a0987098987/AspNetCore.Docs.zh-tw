---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: "主版頁面 |Microsoft 文件"
author: microsoft
description: "其中一個成功的網站的關鍵元件是一致的外觀及操作。 在 ASP.NET 中 1.x，開發人員使用使用者控制項來複寫常見頁面 elem。..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: bd9effd4b73a014d4d7bb825b382b8db34d636f1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="master-pages"></a>主版頁面
====================
由[Microsoft](https://github.com/microsoft)

> 其中一個成功的網站的關鍵元件是一致的外觀及操作。 在 ASP.NET 中 1.x，開發人員使用使用者控制項來跨 Web 應用程式複寫共同的頁面項目。 一定是可行的解決方案，使用使用者控制項也有一些缺點。 比方說，使用者控制項的位置中的變更會在站台需要多個頁面的變更。 使用者控制項也不會呈現之後所插入頁面的 [設計] 檢視中。


其中一個成功的網站的關鍵元件是一致的外觀及操作。 在 ASP.NET 中 1.x，開發人員使用使用者控制項來跨 Web 應用程式複寫共同的頁面項目。 一定是可行的解決方案，使用使用者控制項也有一些缺點。 比方說，使用者控制項的位置中的變更會在站台需要多個頁面的變更。 使用者控制項也不會呈現之後所插入頁面的 [設計] 檢視中。

ASP.NET 2.0 導入了主要頁面的方式來維護一致的外觀與風格，以及您很快就會知道主版頁面的使用者控制項方法代表顯著的改善。

## <a name="why-master-pages"></a>為什麼主版頁面嗎？

您可能會想知道為什麼主版頁面所需在 ASP.NET 2.0。 在所有情況下，網站開發人員已經使用使用者控制項在 ASP.NET 中 1.x 共用內容頁之間的區域。 沒有實際上有數種原因為何使用者控制項是無比最好的解決方案，來建立一般的版面配置。

使用者控制項不會實際定義的頁面配置。 相反地，他們定義的配置和網頁的部分功能。 這兩個之間的差異很重要的因為它會讓使用者控制方案的管理性更為困難。 例如，當您想要變更您的頁面上的使用者控制項的位置，您必須編輯使用者控制項所顯示的實際頁。 Thats 精確，如果您有幾個頁面，但在大型網站，其很快會變得站台管理惡夢 ！

定義通用的版面配置中使用使用者控制項的另一個缺點被根目錄結構中的 ASP.NET 本身。 如果變更使用者控制項的任何 public 成員，它會要求您重新編譯所有使用使用者控制項的頁面。 接著，ASP.NET 會重新編譯 JIT 網頁時，它們第一次存取。 這樣一來，同樣地，會產生不可擴充的架構和較大的站台的站台管理問題。

在 ASP.NET 2.0 的主版頁面依妥善定址兩者的這些問題 （以及更多）。

## <a name="how-master-pages-work"></a>主版頁面的運作方式

主版頁面是類似於其他頁面的範本。 應該由共用 （也就是功能表、 框線、 等等） 的其他頁面的頁面元素會加入至主版頁面。 當新頁面加入至網站時，您可以將它們與主版頁面。 主版頁面與相關聯的頁面稱為**內容頁面**。 根據預設，內容頁面會從主版頁面外觀。 不過，當您建立主版頁面時，您可以定義內容的頁面可以使用自己的內容取代頁面上的部分。 這些部分是透過定義在 ASP.NET 2.0; 引進了新的控制項**ContentPlaceHolder**控制項。

主版頁面都可以包含任意數目的 ContentPlaceHolder 控制項 （或根本沒有）。在內容頁面上，從 ContentPlaceHolder 控制項的內容會出現內**內容**控制項、 ASP.NET 2.0 中的另一個新的控制項。 根據預設，內容控制項的內容頁面是空的讓您能夠提供您自己的內容。 如果您想要使用內容控制項內的主版頁面的內容，您可以因此當您會看到此模組中的更新版本。 內容控制項會對應到 ContentPlaceHolder 控制項透過內容控制項的 ContentPlaceHolderID 屬性。 對應將下列程式碼至主版頁面上呼叫 mainBody ContentPlaceHolder 控制項內容控制項。

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> 通常，您將聽到描述為其他頁面的基底類別的主版頁面的人員。 Thats 實際上不正確。 主版頁面和內容頁面之間的關聯性不是其中一個繼承。


**圖 1**濆婞剢謅 Visual Studio 2005 會顯示在主版頁面和相關聯的內容頁面。 您可以看到的 ContentPlaceHolder 控制項在主版頁面和對應的內容控制項的內容頁面。 請注意，外部 ContentPlaceHolder 的主版頁面內容看得到，但是灰色內容頁面。 ContentPlaceHolder 內的內容可以一般內容頁面。 來自主版頁面的所有其他內容不變。


![主版頁面和其相關聯的內容頁面](master-pages/_static/image1.jpg)

**圖 1**： 主版頁面和其相關聯的內容頁面


## <a name="creating-a-master-page"></a>建立主版頁面

若要建立新的主版頁面：

1. 開啟 Visual Studio 2005，並建立新的網站。
2. 按一下 新增檔案、 檔案。
3. 中所示，從 [加入新項目] 對話方塊選擇主要檔案**圖 2**。
4. 按一下 新增。


![建立新的主版頁面](master-pages/_static/image2.jpg)

**圖 2**： 建立新的主版頁面


請注意，在主版頁面檔案的副檔名是*.master*。 這是一種主版頁面不同於一般的頁面。 其主要的差別在於替代@Page指示詞，主版頁面包含@Master指示詞。 切換至來源檢視的主要頁面上您剛才建立和檢閱的程式碼。

新的主版頁面預設將會擁有一個 ContentPlaceHolder 控制項。 在大部分情況下，它更具意義先建立一般的頁面項目，然後插入 ContentPlaceHolder 控制項至想要自訂內容的地方。 在這些情況下，開發人員會想要刪除預設 ContentPlaceHolder 控制項，並插入新的網頁開發。 ContentPlaceHolder 控制項不是可調整大小，儘管它們並顯示調整大小控點。 ContentPlaceHolder 控制項的大小會自動根據您有一個例外狀況; 它包含的內容如果您將區塊項目內的 ContentPlaceHolder 控制項例如表格儲存格時，它會根據項目的大小調整大小。

## <a name="lab-1-working-with-master-pages"></a>實驗室 1 使用主版頁面

在此實驗室中，您會建立新的主版頁面和定義三個 ContentPlaceHolder 控制項。 您接著建立新的內容頁面，並取代其中至少一個 ContentPlaceHolder 控制項的內容。

1. 建立主版頁面，並插入 ContentPlaceHolder 控制項。 

    1. 如上面所述，建立新的主版頁面。
    2. 刪除預設 ContentPlaceHolder 控制項。
    3. 選取 ContentPlaceHolder 控制項按一下灰色上控制項的框線，然後按下 DEL 鍵鍵盤上的刪除。
    4. 插入新資料表使用*標頭和側邊*範本，如圖 3 所示。 設成 90%每個變更的寬度和高度，使整個資料表會顯示在設計工具中。


![](master-pages/_static/image3.jpg)

**圖 3**


1. 將游標放在資料表的每個資料格，並設定*valign*屬性*頂端*。
2. 從 [工具箱] 插入 ContentPlaceHolder 控制項在頂端資料格的資料表 （標頭資料格。）
3. 當您插入這個 ContentPlaceHolder 控制項時，您會注意到如圖 4 所示，資料列高度會佔用幾乎整個頁面。 請勿在此時在意了解。


![ContentPlaceHolder 為相同的資料格是空的空間](master-pages/_static/image1.gif)

**圖 4**: ContentPlaceHolder 為相同的資料格是空的空間


1. ContentPlaceHolder 將控制項放置在其他兩個資料格。 插入其他 ContentPlaceHolder 控制項之後, 的表格儲存格大小應如您所預期。 頁面現在看起來應該像所顯示的網頁**圖 5**。


![所有的 ContentPlaceHolder 控制項與主機。 請注意，標頭資料格的儲存格高度現在它應該是](master-pages/_static/image2.gif)

**圖 5**: 母片 ContentPlaceHolder 的所有控制項。 請注意，標頭資料格的儲存格高度現在它應該是


1. 輸入您選擇的一些文字到每一個三個 ContentPlaceHolder 控制項。
2. 將主版頁面儲存為 exercise1.master。
3. 建立新的 Web 表單和其關聯 exercise1.master 主版頁面。
4. 選取新檔案，檔案在 Visual Studio 2005。
5. 選取**Web Form**在 [加入新項目] 對話方塊。
6. 請確定選取的主版頁面的核取方塊已核取，如圖 6 所示。


![加入新的內容頁](master-pages/_static/image3.gif)

**圖 6**： 加入新的內容頁


1. 按一下 新增。
2. 選取 exercise1.master 在 選取主版頁面對話方塊圖 7 所示。
3. 按一下 [確定] 以加入新的內容頁面。

新的內容頁面會出現在 Visual Studio 中，每個 ContentPlaceHolder 控制項在主版頁面的其中一個內容控制項。 根據預設，將內容控制項是空的好讓您可以加入您自己的內容。 如果 youd 喜歡的使用中的主版頁面的 ContentPlaceHolder 控制項的內容，只要按一下智慧標籤符號 （小型黑色箭頭控制項右上角），然後選擇*預設主機內容*從智慧標籤中所示**圖 8**。 當您這樣做時，功能表項目變更為*建立自訂內容*。 按一下它在該點移除主版頁面可讓您定義該特定的內容控制項的自訂內容的內容。


![設定為預設為主版頁面的頁面內容的內容控制項](master-pages/_static/image4.gif)

**圖 7**： 將內容控制項設定為預設為主版頁面的頁面內容


## <a name="connecting-master-page-and-content-pages"></a>連接主版頁面和內容頁面

主版頁面和內容頁面之間的關聯可以設定四種不同方式之一：

- **MasterPageFile**屬性@Page指示詞
- 設定**Page.MasterPageFile**程式碼中的屬性。
- **&lt;頁面&gt;**應用程式組態檔 (web.config 應用程式的根資料夾中) 中的項目
- **&lt;頁面&gt;**子資料夾的組態檔 (位於子資料夾中的 web.config) 中的項目

## <a name="masterpagefile-attribute"></a>MasterPageFile 屬性

MasterPageFile 屬性可讓您能輕鬆套用至特定的 ASP.NET 網頁的主版頁面。 它也是用來檢查時套用的主版頁面的方法**選取主版頁面**核取方塊，當您未在練習 1 中。

## <a name="setting-pagemasterpagefile-in-code"></a>程式碼中設定 Page.MasterPageFile

程式碼中設定的 MasterPageFile 屬性，您可以套用特定的主版頁面，您在執行階段的內容。 這是適用於您可能需要套用特定的主版頁面，根據使用者角色或某些其他條件的情況。 MasterPageFile 屬性必須設定 PreInit 方法中。 如果它設定為 PreInit 方法之後，將會擲回 InvalidOperationException。 頁面設定這個屬性也必須擁有內容控制項為頁面的最上層控制項。 否則 MasterPageFile 屬性設定時，HttpException 就會擲回。

## <a name="using-the-ltpagesgt-element"></a>使用&lt;頁面&gt;項目

您可以為您的網頁設定主版頁面中設定的 masterPageFile 屬性&lt;頁面&gt;web.config 檔案的項目。 使用此方法時，請注意，應用程式結構中較低的 web.config 檔案，可以覆寫此設定。 在中設定任何 MasterPageFile 屬性@Page指示詞也會覆寫此設定。 使用&lt;頁面&gt;項目可簡化建立*主要*會覆寫必要時在特定資料夾或檔案中的主版頁面。

## <a name="properties-in-master-pages"></a>主版頁面中的屬性

主版頁面可以公開屬性只讓這些屬性的主版頁面內是公用。 例如，下列的程式碼會定義名為 SomeProperty 的屬性：

[!code-csharp[Main](master-pages/samples/sample2.cs)]

若要存取 SomeProperty 屬性從 [內容] 頁面，您必須使用主要屬性如下所示：

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>巢狀主版頁面

主版頁面是完美的解決方案，跨大型的 Web 應用程式確保一般的外觀及操作。 不過，它不罕見有大型的站台共用的通用介面的某些部分，而其他部分共用不同的介面。 若要解決這項需求，多重主版頁面是完美的解決方案。 不過，該仍然不能處理大型應用程式可能有某些元件 （例如功能表，例如） 所共用的所有頁面和其他元件只有特定區段的站台所共用的事實。 該類型的情況中，巢狀主版頁面填入需要正確地切割。 如您所見，標準主版頁面包含在主版頁面和內容頁面。 在巢狀主版頁面的情況下，有兩個主要的頁面。父主機和子系主機。 子主版頁面也是內容頁面，且其主要父主版頁面。

以下是典型的主版頁面的程式碼：

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

在巢狀的主要案例中，這會是父 master。 另一個主版頁面會為其主版頁面中，使用此頁面，該程式碼會看起來像這樣：

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

請注意，在此案例中，子主機也父主機的內容頁面。 從父代的 ContentPlaceHolder 控制項取得其內容的內容控制項內的所有子主要內容隨即出現。

> [!NOTE]
> 找不到適用於巢狀主版頁面的設計工具支援。 當您開發使用巢狀主圖形時，您必須使用原始碼檢視。


這段影片會示範使用巢狀主版頁面的逐步解說。


![](master-pages/_static/image1.png)


[開啟全螢幕視訊](master-pages/_static/nested1.wmv)


![選取主版頁面](master-pages/_static/image4.jpg)

**圖 8**： 選取主版頁面
