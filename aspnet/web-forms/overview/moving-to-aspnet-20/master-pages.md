---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: 主版頁面 |Microsoft Docs
author: microsoft
description: 其中一個成功的 Web 站台的主要元件是一致的外觀及操作。 在 ASP.NET 1.x 中，開發人員使用使用者控制項來複寫常見頁面 elem。
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: f40eb338a1b6b8eebb6578dd7938e96a05b1617f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830193"
---
<a name="master-pages"></a>主版頁面
====================
by [Microsoft](https://github.com/microsoft)

> 其中一個成功的 Web 站台的主要元件是一致的外觀及操作。 在 ASP.NET 1.x 中，開發人員用使用者控制項來複寫通用頁面項目之間的 Web 應用程式。 雖然這當然是可行的解決方案，使用使用者控制項也有一些缺點。 比方說，在使用者控制項的位置的變更會在站台需要多個頁面的變更。 使用者控制項也不會呈現之後所插入到網頁上的 [設計] 檢視中。


其中一個成功的 Web 站台的主要元件是一致的外觀及操作。 在 ASP.NET 1.x 中，開發人員用使用者控制項來複寫通用頁面項目之間的 Web 應用程式。 雖然這當然是可行的解決方案，使用使用者控制項也有一些缺點。 比方說，在使用者控制項的位置的變更會在站台需要多個頁面的變更。 使用者控制項也不會呈現之後所插入到網頁上的 [設計] 檢視中。

ASP.NET 2.0 引進主版頁面的方式來維護一致的外觀及操作，以及您馬上就可以看到，主要頁面代表顯著的改進，透過將使用者控制項的方法。

## <a name="why-master-pages"></a>為什麼主版頁面嗎？

您可能會好奇為什麼主版頁面所需在 ASP.NET 2.0。 畢竟，網站開發人員已經使用使用者控制項在 ASP.NET 1.x 共用內容頁之間的區域。 有實際原因使用者控制項都是較不比最好的解決方案，來建立常用的版面配置的幾個原因。

使用者控制項未實際定義頁面配置。 相反地，它們定義的配置和頁面的部分功能。 這兩個之間的差異很重要的因為它可讓使用者控制解決方案的管理性更為困難。 比方說，當您想要變更使用者控制項在頁面上的位置，您必須編輯使用者控制項所在的實際頁面。 Thats 精確，如果您只有幾頁，但在大型的網站，它很快就會是站台管理惡夢一場 ！

定義常用的版面配置中使用使用者控制項的另一個缺點是 ASP.NET 本身的架構為基礎。 如果變更使用者控制項的任何 public 成員，它會要求您重新編譯所有使用的使用者控制項的頁面。 接著，ASP.NET 會則 opakovanou kompilaci JIT 時第一個頁面存取。 如此一來，同樣地，會產生不可擴充的架構和更大的站台的站台管理的問題。

這兩個這些問題 （及更多） 會妥善解決在 ASP.NET 2.0 主版頁面。

## <a name="how-master-pages-work"></a>主版頁面的運作方式

主版頁面相當於其他頁面的範本。 應該在其他頁面 （也就是功能表中，框線等） 之間共用的頁面元素會新增至主版頁面。 當新頁面加入至站台時，您可以將它們關聯的主版頁面。 主版頁面與相關聯的頁面會呼叫**內容頁面**。 根據預設，內容頁面會從主版頁面的外觀。 不過，當您建立主版頁面，您可以定義內容頁面可以取代自身的內容頁面上的部分。 這些部分定義時，使用 ASP.NET 2.0; 中導入新的控制項**ContentPlaceHolder**控制項。

主版頁面可以包含任意數目的 ContentPlaceHolder 控制項 （或根本沒有）。在 [內容] 頁面 ContentPlaceHolder 控制項的內容會出現內**內容**控制項、 ASP.NET 2.0 中的另一個新控制項。 根據預設，內容控制項的內容頁面是空的以便您可以提供您自己的內容。 如果您想要使用內容控制項內的主版頁面的內容，則可以因此當您將看到此模組中的更新版本。 內容控制項對應至 ContentPlaceHolder 控制項透過 ContentPlaceHolderID 屬性的內容控制項。 將內容控制項對應以下的程式碼至主版頁面上呼叫 mainBody ContentPlaceHolder 控制項中。

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> 您通常會聽到描述為其他頁面的基底類別的主版頁面的人員。 Thats 實際上不為 true。 主版頁面和內容頁面之間的關聯性不是其中一個繼承。


**圖 1**濆婞剢謅 Visual Studio 2005 會顯示主版頁面與相關聯的內容頁面。 您可以看到 ContentPlaceHolder 控制項中的主版頁面和對應內容控制項，在 [內容] 頁面中的。 請注意，主版頁面內容超出 ContentPlaceHolder 可見但灰色 [內容] 頁面。 ContentPlaceHolder 內的內容可以取代由內容頁面。 來自主版頁面的所有其他內容永遠不變。


![主版頁面和其相關聯的內容頁面](master-pages/_static/image1.jpg)

**圖 1**： 主版頁面和其相關聯的內容頁面


## <a name="creating-a-master-page"></a>建立主版頁面

若要建立新的主版頁面：

1. 開啟 Visual Studio 2005 並建立新的網站。
2. 按一下 新增檔案、 檔案。
3. 從 [加入新項目] 對話方塊選擇主要檔案，如中所示**圖 2**。
4. 按一下 新增。


![建立新的主版頁面](master-pages/_static/image2.jpg)

**圖 2**： 建立新的主版頁面


請注意，主版頁面檔案的副檔名<em>.master</em>。 這是其中一種主版頁面不同於一般的頁面。 其主要的差異在於替代@Page指示詞時，主版頁面包含@Master指示詞。 切換至來源檢視的主要頁面，您剛剛建立，並檢閱程式碼。

新的主版頁面預設會有一個 ContentPlaceHolder 控制項。 在大部分情況下，較為合理要先建立一般的頁面項目，然後插入 ContentPlaceHolder 控制項自訂內容想要的地方。 在這些情況下，開發人員會想要刪除預設 ContentPlaceHolder 控制項，並插入新的開發頁面。 ContentPlaceHolder 控制項不是可調整大小，儘管它們並顯示調整大小控點事實。 ContentPlaceHolder 控制項的大小會自動根據它有一個例外狀況; 包含的內容如果您將 ContentPlaceHolder 控制項內的區塊項目例如表格儲存格時，它會根據項目的大小的大小。

## <a name="lab-1-working-with-master-pages"></a>實驗室 1 使用主版頁面

在此實驗室中，您會建立新的主版頁面，並定義三個 ContentPlaceHolder 控制項。 然後，您會建立新的內容頁面，並將至少其中一個 ContentPlaceHolder 控制項的內容。

1. 建立主版頁面，並插入 ContentPlaceHolder 控制項。 

    1. 如上面所述，請建立新的主版頁面。
    2. 刪除預設 ContentPlaceHolder 控制項。
    3. 選取 ContentPlaceHolder 控制項按一下控制項的陰影的框線，然後加以刪除，請按下鍵盤上的 DEL 鍵。
    4. 插入新的資料表使用*標頭和側邊*範本，如 圖 3 所示。 90%每個變更的寬度和高度，使整個資料表會顯示在設計工具。


![](master-pages/_static/image3.jpg)

**圖 3**


1. 將游標放在資料表的每個資料格，並設定*valign*屬性設*頂端*。
2. 從 [工具箱] 中，插入 ContentPlaceHolder 控制項在上方表格的儲存格 （標頭資料格。）
3. 當您將這個 ContentPlaceHolder 控制項時，您會發現 圖 4 所示，將資料列高度會花費幾乎整個頁面。 不需要考量，此時。


![空白空間位於相同的儲存格為 ContentPlaceHolder](master-pages/_static/image1.gif)

**圖 4**： 空白空間位於相同的儲存格為 ContentPlaceHolder


1. ContentPlaceHolder 將控制項放置在其他兩個資料格。 一旦已插入其他 ContentPlaceHolder 控制項，如您所預期，應該是表格儲存格的大小。 頁面現在看起來應該像中所顯示的網頁**圖 5**。


![與所有 ContentPlaceHolder 控制項 Master。 請注意，標頭資料格的儲存格高度現在它應該是](master-pages/_static/image2.gif)

**圖 5**: 主要與所有 ContentPlaceHolder 控制項。 請注意，標頭資料格的儲存格高度現在它應該是


1. 輸入您選擇的一些文字到三個 ContentPlaceHolder 控制項的每個。
2. 將主版頁面儲存為 exercise1.master 中。
3. 建立新的 Web 表單，並將它與 exercise1.master 主版頁面產生關聯。
4. 選取 新增檔案，Visual Studio 2005 中的檔案。
5. 選取  **Web Form**在 加入新項目 對話方塊。
6. 請確定選取的主版頁面的核取方塊已核取，如 圖 6 所示。


![加入新的 [內容] 頁面](master-pages/_static/image3.gif)

**圖 6**： 加入新的 內容 頁面


1. 按一下 新增。
2. 選取 exercise1.master 在 選取主版頁面對話方塊如 圖 7 所示。
3. 按一下 [確定] 以加入新的內容頁面。

新的內容頁面會出現在 Visual Studio 中，每個 ContentPlaceHolder 控制項在主版頁面的其中一個內容控制項。 根據預設，內容控制項是空的好讓您可以加入自己的內容。 如果過您要使用來自 ContentPlaceHolder 控制項在主版頁面的內容，只要按一下智慧標籤符號 （小型黑色箭號控制項右上角），然後選擇*預設為 主機內容*從智慧標籤所示**圖 8**。 當您這樣做時，功能表項目變更為*建立自訂內容*。 此時，按一下它，是在主版頁面可讓您定義該特定的內容控制項的自訂內容中移除內容。


![設定預設主版頁面內容的內容控制項](master-pages/_static/image4.gif)

**圖 7**： 將內容控制項設定為預設主版頁面內容


## <a name="connecting-master-page-and-content-pages"></a>連線的主版頁面和內容頁面

可以使用四種不同方式的其中一個設定主版頁面和內容頁面之間的關聯：

- <strong>MasterPageFile</strong>屬性@Page指示詞
- 設定**Page.MasterPageFile**在程式碼中的屬性。
- **&lt;頁&gt;** 應用程式組態檔 (web.config 應用程式的根資料夾中) 中的項目
- **&lt;頁&gt;** 子組態檔 (web.config 的子資料夾中) 中的項目

## <a name="masterpagefile-attribute"></a>MasterPageFile 屬性

MasterPageFile 屬性可讓您能輕鬆套用到特定的 ASP.NET 頁面的主版頁面。 它也是用來套用主版頁面，當您選取的方法**選取主版頁面**核取方塊，當您未在練習 1 中。

## <a name="setting-pagemasterpagefile-in-code"></a>程式碼中設定 Page.MasterPageFile

藉由在程式碼中設定的 MasterPageFile 屬性，您可以套用特定的主版頁面，您在執行階段的內容。 這種情況下，您可能需要套用特定的主版頁面，根據使用者角色或其他準則。 MasterPageFile 屬性必須設定 PreInit 方法中。 如果它設定為 PreInit 方法之後，將會擲回 InvalidOperationException。 頁面設定這個屬性也必須有內容為頁面的最上層控制項的控制項。 否則 MasterPageFile 屬性設定時，HttpException 就會擲回。

## <a name="using-the-ltpagesgt-element"></a>使用&lt;頁&gt;項目

您可以為您的網頁設定主版頁面中設定的 masterPageFile 屬性&lt;頁&gt;web.config 檔案的項目。 使用此方法時，記住應用程式結構中較低的 web.config 檔案，可以覆寫此設定。 在中設定任何 MasterPageFile 屬性@Page指示詞也會覆寫此設定。 使用&lt;頁&gt;項目會建立簡單<em>主要</em>可以覆寫視特定的資料夾或檔案中的主版頁面。

## <a name="properties-in-master-pages"></a>主版頁面中的屬性

主版頁面可以公開屬性，只要讓這些屬性主版頁面內公用。 比方說，下列程式碼會定義名為 SomeProperty 的屬性：

[!code-csharp[Main](master-pages/samples/sample2.cs)]

若要存取 SomeProperty 屬性從 [內容] 頁面，您必須使用主要屬性如下：

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>巢狀主版頁面

主版頁面是完美的解決方案，以確保跨大型的 Web 應用程式的常見的外觀與風格。 不過，它不是常會有大型的站台共用的通用介面的某些部分，而其他部分共用不同的介面。 若要解決這項需求，多個主版頁面是完美的解決方案。 不過，仍然無法解決，大型的應用程式可能會有某些元件 （例如功能表，例如） 可共用的所有頁面和其他元件只在特定區段的站台之間共用的事實。 這樣的情況中，巢狀主版頁面填入需要妥善。 如您所見，一般的主版頁面包含主版頁面和內容頁面。 在巢狀主版頁面的情況下，有兩個主版頁面;父主要和下層主版。 子主版頁面也是內容頁面，其主要是父主版頁面。

以下是典型的主版頁面的程式碼：

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

在巢狀的主要案例中，這會是父 master。 另一個主版頁面會為其主版頁面中，使用此頁面，該程式碼會看起來像這樣：

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

請注意，在此案例中，子主要也是內容的父主版頁面。 從父代的 ContentPlaceHolder 控制項取得其內容的內容控制項內的所有子主機的內容會出現。

> [!NOTE]
> 找不到適用於巢狀主版頁面的設計工具的支援。 當您開發使用巢狀的主機時，您必須使用來源檢視。


這段影片示範使用巢狀主版頁面的逐步解說。


![](master-pages/_static/image1.png)


[開啟全螢幕影片](master-pages/_static/nested1.wmv)


![選取主版頁面](master-pages/_static/image4.jpg)

**圖 8**： 選取主版頁面
