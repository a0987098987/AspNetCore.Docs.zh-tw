---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: 成員資格和管理 |Microsoft 文件
author: Erikre
description: 此教學課程將告訴您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Form 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: 166bc642ea2083f455be0648e424f0b0ae3b082c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="membership-and-administration"></a>成員資格和管理
====================
由[Erik Reitan](https://github.com/Erikre)

[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 此教學課程將告訴您建置一個使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Form 應用程式的基本概念。 Visual Studio 2013[與 C# 原始程式碼的專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附此教學課程了。


本教學課程會示範如何加入自訂安全性角色，並使用 ASP.NET Identity Wingtip Toys 範例應用程式的更新。 它也示範如何實作從中具有自訂安全性角色的使用者可以新增和移除的產品，從網站管理頁面。

[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)是用來建立 ASP.NET web 應用程式的成員資格系統，可在 ASP.NET 4.5。 Visual Studio 2013 Web Form 專案範本，以及適用於的範本中使用 ASP.NET Identity [ASP.NET MVC](../../../../mvc/index.md)， [ASP.NET Web API](../../../../web-api/index.md)，和[單一網頁應用程式 ASP.NET](../../../../single-page-application/index.md). 您可以也特別安裝 ASP.NET 識別系統使用空的 Web 應用程式啟動時，使用 NuGet。 不過，在此教學課程系列您使用**Web Form**projecttemplate，包含 ASP.NET Identity 系統。 ASP.NET Identity 輕鬆地整合應用程式資料的使用者特定設定檔資料。 此外，ASP.NET 識別可讓您選擇您的應用程式中的使用者設定檔的持續性模型。 您可以將資料儲存在 SQL Server 資料庫或其他資料存放區，包括*NoSQL*資料存放區，例如 Windows Azure 儲存體資料表。

本教學課程是上一個教學課程，Wingtip Toys 教學課程系列中標題為 「 簽出和付款與 PayPal"。

## <a name="what-youll-learn"></a>您將學習：

- 如何將自訂安全性角色和使用者加入至應用程式中使用程式碼。
- 如何管理資料夾和頁面限制存取。
- 如何提供隸屬於自訂角色之使用者的導覽。
- 如何使用模型繫結來填入[DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx)與產品類別目錄的控制項。
- 如何將檔案上傳至 web 應用程式使用[檔案上傳](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx)控制項。
- 如何使用驗證控制項來執行輸入的驗證。
- 如何新增和移除應用程式中的產品。

## <a name="these-features-are-included-in-the-tutorial"></a>教學課程中包含這些功能：

- ASP.NET Identity
- 設定及授權
- 模型繫結
- 不顯眼的驗證

ASP.NET Web Form 提供成員資格功能。 使用預設範本，您需要內建成員資格功能，您可以立即使用應用程式執行時。 本教學課程會示範如何使用 ASP.NET Identity 加入自訂安全性角色，並將使用者指派給該角色。 您將學習如何限制來管理資料夾的存取。 您要加入頁面可讓使用者使用自訂安全性角色來加入和移除產品，以及預覽產品之後已新增, 的系統管理資料夾。

## <a name="adding-a-custom-role"></a>加入自訂安全性角色

使用 ASP.NET Identity，新增自訂的角色，並將使用者指派給該角色，使用程式碼。

1. 在**方案總管 中**，以滑鼠右鍵按一下*邏輯*資料夾，並建立新的類別。
2. 將新類別*RoleActions.cs*。
3. 修改程式碼，使其顯示，如下所示：  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. 在**方案總管 中**，開啟*Global.asax.cs*檔案。
5. 修改*Global.asax.cs*檔案中加入程式碼中反白顯示黃色使其顯示，如下所示：  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. 請注意，`AddUserAndRole`會以紅色加上底線。 按兩下 AddUserAndRole 程式碼。  
   加上底線的字母"A"，反白顯示方法的開頭。
7. 將滑鼠停留在字母"A"，然後按一下 UI，可讓您產生方法 stub`AddUserAndRole`方法。 

    ![成員資格和 Advministration-產生方法虛設常式](membership-and-administration/_static/image1.png)
8. 按一下標題為的選項：  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. 開啟*RoleActions.cs*檔案從*邏輯*資料夾。  
   `AddUserAndRole`方法已經加入至類別檔案。
10. 修改*RoleActions.cs*藉由移除檔案`NotImplementedeException`及新增以黃色反白顯示程式碼，使其顯示，如下所示：  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

上述程式碼首先會建立成員資格資料庫的資料庫內容。 成員資格資料庫也會儲存為*.mdf*檔案*應用程式\_資料*資料夾。 您可以檢視此資料庫之後的第一個使用者登入此 web 應用程式。 

> [!NOTE] 
> 
> 如果您想要儲存成員資格資料，以及產品資料，您可以考慮使用相同**DbContext**您用來儲存上述程式碼中的產品資料。


 *內部*關鍵字是類型 （例如類別） 和類型成員 （例如方法或屬性） 的存取修飾詞。 內部類型或成員都只能在包含相同的組件檔案中存取*(.dll*檔案)。 當您建置您的應用程式組件檔案*(.dll*) 會建立包含您執行應用程式時所執行的程式碼。 

A`RoleStore`物件，提供角色管理，建立的資料庫內容。

> [!NOTE] 
> 
> 請注意，當`RoleStore`會建立物件，它會使用泛型`IdentityRole`型別。 這表示`RoleStore`只可包含`IdentityRole`物件。 也藉由使用泛型時，在記憶體中處理資源更好。


下一步`RoleManager`物件，會根據建立`RoleStore`您剛才建立的物件。 `RoleManager`物件會公開角色相關的 API，可以用來自動將變更儲存到`RoleStore`。 `RoleManager`只可包含`IdentityRole`物件，因為程式碼會使用`<IdentityRole>`泛型型別。

您呼叫`RoleExists`方法，以判斷是否存在於成員資格資料庫 」: canEdit 」 角色。 如果不是，您會建立角色。

建立`UserManager`物件似乎是更複雜比`RoleManager`控制項，不過會幾乎相同。 它只會在同一行而不是數個編碼。 在這裡，您要傳遞的參數具現化當做新的物件，包含在括號中。

接下來您建立 「 canEditUser 」 使用者藉由建立新`ApplicationUser`物件。 然後，如果您已成功建立使用者，您將使用者加入新的角色。

> [!NOTE] 
> 
> 錯誤處理將會在"ASP.NET 錯誤處理 」 教學課程稍後在本教學課程系列期間更新。


下一次啟動應用程式，名為"canEditUser"的使用者將名為":"canEdit 應用程式的角色。 稍後在本教學課程中，您將使用者身分登入 」 canEditUser 」 以顯示您將在本教學課程期間加入的額外功能。 如需 ASP.NET 識別的應用程式開發介面詳細資訊，請參閱[Microsoft.AspNet.Identity 命名空間](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx)。 如需初始化 ASP.NET 識別系統的其他詳細資訊，請參閱[AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs)。

### <a name="restricting-access-to-the-administration-page"></a>限制存取的管理頁面

Wingtip Toys 範例應用程式可讓匿名使用者和登入的使用者檢視及購買產品。 不過，登入的使用者具有 「 自訂 」: canEdit 」 角色可以存取受限制的頁面，以加入和移除產品。

#### <a name="add-an-administration-folder-and-page"></a>新增管理資料夾和頁面

接下來，您將建立名為的資料夾*Admin* "canEditUser 」 使用者屬於 Wingtip Toys 自訂角色範例應用程式。

1. 以滑鼠右鍵按一下專案名稱 (**Wingtip Toys**) 中**方案總管 中**選取**新增** - &gt; **新資料夾**.
2. 將新的資料夾命名*Admin*。
3. 以滑鼠右鍵按一下*Admin*資料夾，然後選取**新增** - &gt; **新項目**。   
   隨即顯示 [ 新增項目] 對話方塊。
4. 選取<strong>Visual C#</strong> - &gt; <strong>Web</strong>左側的 [範本] 群組。 從 中間 清單中選取<strong>使用主版頁面的 Web Form</strong>，其命名<em>AdminPage.aspx</em><strong>，</strong> ，然後選取 <strong>新增</strong>。
5. 選取*Site.Master*主版頁面中，為檔案，然後選擇 **確定**。

#### <a name="add-a-webconfig-file"></a>加入 Web.config 檔案

藉由新增*Web.config*檔案*Admin*資料夾中，您可以限制存取包含在資料夾中的頁面。

1. 以滑鼠右鍵按一下*Admin*資料夾，然後選取**新增** - &gt; **新項目**。  
   隨即顯示 [ 新增項目] 對話方塊。
2. 從 Visual C# web 範本清單中，選取<strong>Web 組態檔</strong>從中間的清單中，接受預設名稱<em>Web.config</em><strong>，</strong> ]，然後選取 [ <strong>新增</strong>。
3. 取代現有的 XML 內容中*Web.config*以下列檔案：  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

儲存*Web.config*檔案。 *Web.config*屬於應用程式 」: canEdit 」 角色的唯一使用者可以存取頁面中所包含的檔案指定*Admin*資料夾。

### <a name="including-custom-role-navigation"></a>包括自訂角色導覽

若要啟用自訂的 「: canEdit 」 角色的使用者瀏覽至 [管理] 區段中的應用程式，您必須加入至連結*Site.Master*頁面。 只有屬於 「: canEdit 」 角色的使用者可以看到**Admin**連結和存取管理 區段中。

1. 在 [方案總管] 中，尋找並開啟*Site.Master*頁面。
2. 若要建立 「: canEdit 」 角色的使用者的連結，加入下列的未排序清單的黃色反白顯示的標記`<ul>`元素讓，清單會顯示為如下所示：  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. 開啟*Site.Master.cs*檔案。 請**Admin**連結只加入程式碼中以黃色反白顯示 「 canEditUser 」 使用者可以看見`Page_Load`處理常式。 `Page_Load`處理常式將會出現，如下所示：   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

當網頁載入時，程式碼會檢查登入的使用者是否具有 「: canEdit 」 的角色。 如果使用者隸屬於 「: canEdit 」 角色，包含連結的 span 元素*AdminPage.aspx*頁 （而因此範圍內的連結） 就會顯示。

### <a name="enabling-product-administration"></a>啟用產品管理

目前為止，您已建立 」: canEdit 」 角色，並加入 「 canEditUser 」 使用者、 系統管理資料夾，以及管理頁面。 您已設定 頁面上，與管理資料夾的存取權限，且應用程式中新增 「: canEdit 」 角色的使用者之瀏覽連結。 接下來，您將加入至標記*AdminPage.aspx*頁面和程式碼*AdminPage.aspx.cs* ，可以讓使用者與 「: canEdit 」 角色新增和移除產品的程式碼後置檔案。

1. 在**方案總管 中**，開啟*AdminPage.aspx*檔案從*Admin*資料夾。
2. 以下列內容取代現有的標記：  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. 接下來，開啟*AdminPage.aspx.cs*程式碼後置檔案，以滑鼠右鍵按一下*AdminPage.aspx*按一下**檢視程式碼**。
4. 取代現有的程式碼中*AdminPage.aspx.cs*為下列程式碼的程式碼後置檔案：  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

在程式碼中所輸入的*AdminPage.aspx.cs*程式碼後置檔案，一種類別稱為`AddProducts`執行這項產品加入資料庫中的實際工作。 這個類別還不存在，因此將立即建立。

1. 在**方案總管 中**，以滑鼠右鍵按一下*邏輯*資料夾，然後選取**新增** - &gt; **新項目**。   
   隨即顯示 [ 新增項目] 對話方塊。
2. 選取**Visual C#**  - &gt; **程式碼**左側的 [範本] 群組。 然後，選取**類別**中間清單並將其命名*AddProducts.cs*。   
   會顯示新的類別檔案。
3. 將現有的程式碼取代為下列程式碼：  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

*AdminPage.aspx*頁面可讓使用者屬於 「: canEdit 」 角色新增及移除產品。 當加入新的產品時，產品的詳細驗證並接著輸入到資料庫。 新的產品會立即提供給 web 應用程式的所有使用者。

#### <a name="unobtrusive-validation"></a>不顯眼的驗證

使用者提供的產品詳細資料*AdminPage.aspx*頁面會驗證使用驗證控制項 (`RequiredFieldValidator`和`RegularExpressionValidator`)。 這些控制項自動使用不顯眼的驗證。 不顯眼的驗證可讓您使用 JavaScript 的用戶端的驗證邏輯，這表示頁面不需要往返於伺服器要驗證的驗證控制項。 根據預設，不顯眼的驗證包含在*Web.config*檔案會根據下列的組態設定：

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>規則運算式

上的產品價格*AdminPage.aspx*頁面會驗證使用**RegularExpressionValidator**控制項。 這個控制項驗證是否相關聯的輸入控制項 （"AddProductPrice"文字方塊） 的值符合規則運算式所指定的模式。 規則運算式是可讓您快速找出並符合特定的字元模式的模式比對標記法。 **RegularExpressionValidator**控制項包含一個名為屬性`ValidationExpression`，其中包含用來驗證價格輸入，如下所示的規則運算式：

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>檔案上傳控制項

在您加入的輸入和驗證的控制項，除了**檔案上傳**控制權傳輸至*AdminPage.aspx*頁面。 這個控制項提供的功能，將檔案上傳。 在此情況下，您只允許上傳的映像檔案。 程式碼後置檔案中 (*AdminPage.aspx.cs*)，當`AddProductButton`按一下時，程式碼檢查`HasFile`屬性**檔案上傳**控制項。 如果控制項有一個檔案，而且允許 （根據副檔名） 的檔案類型，則影像會儲存到*映像*資料夾和*映像/使*的應用程式資料夾。

#### <a name="model-binding"></a>模型繫結

稍早在本教學課程系列您可以使用模型繫結填入**ListView**控制項， **FormsView**控制項， **GridView**控制項和**DetailView**控制項。 在本教學課程中，您可以使用模型繫結填入**DropDownList**具有產品類別目錄的清單控制項。

您加入的標記*AdminPage.aspx*檔案包含**DropDownList**控制項稱為`DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

您可以使用模型繫結來填入這**DropDownList**藉由設定`ItemType`屬性和`SelectMethod`屬性。 `ItemType`屬性會指定您使用`WingtipToys.Models.Category`輸入擴展控制項時。 您藉由建立定義此類型在開始此教學課程系列的`Category`類別 （如下所示）。 `Category`類別位於*模型*資料夾內*Category.cs*檔案。

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

`SelectMethod`屬性**DropDownList**控制項可讓您指定您使用`GetCategories`方法 （如下所示） 也就是包含在程式碼後置檔案中 (*AdminPage.aspx.cs*)。

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

這個方法會指定`IQueryable`介面用來評估查詢，以針對`Category`型別。 傳回的值用來填入**DropDownList**中頁面的標記 (*AdminPage.aspx*)。

顯示的設定，來指定每個項目在清單中的文字`DataTextField`屬性。 `DataTextField`屬性使用`CategoryName`的`Category`（如上所示） 以顯示每個類別目錄中的類別**DropDownList**控制項。 選取一個項目時，傳遞的實際值**DropDownList**控制項根據`DataValueField`屬性。 `DataValueField`屬性設為`CategoryID`中定義`Category`類別 （如上所示）。

### <a name="how-the-application-will-work"></a>應用程式運作方式

當屬於 「: canEdit 」 角色的使用者第一次，巡覽至頁面`DropDownAddCategory` **DropDownList**控制項已填入，如上面所述。 `DropDownRemoveProduct` **DropDownList**控制項也會填入產品使用相同的方法。 使用者屬於 「: canEdit 」 角色會選取類別目錄類型，然後新增產品詳細資料 (**名稱**，**描述**，**價格**，和**映像檔**). 當屬於 「: canEdit 」 角色的使用者按一下**新增產品** 按鈕，`AddProductButton_Click`觸發事件處理常式。 `AddProductButton_Click`位於程式碼後置檔案中事件處理常式 (*AdminPage.aspx.cs*) 會檢查以確定它符合允許的檔案類型的映像檔*(.gif*， *.png*， *.jpeg*，或*.jpg*)。 然後，映像檔會儲存至資料夾，Wingtip Toys 範例應用程式。 接下來，新的產品會加入至資料庫。 若要完成 加入新的產品的新執行個體`AddProducts`類別會建立並命名為產品。 `AddProducts`類別具有名為方法`AddProduct`，和產品物件呼叫這個方法，將產品加入資料庫。

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

如果程式碼已成功將新的產品加入資料庫，並重新載入以查詢字串值`ProductAction=add`。

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

當重新載入頁面時，查詢字串會包含在 URL 中。 重新載入該頁面，屬於 「: canEdit 」 角色的使用者可以立即看到中的更新**DropDownList**控制項*AdminPage.aspx*頁面。 此外，藉由使用 URL 查詢字串，網頁可以顯示成功訊息給使用者屬於 「: canEdit 」 角色。

當*AdminPage.aspx*頁面上的重新載入，`Page_Load`事件被呼叫。

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

`Page_Load`事件處理常式會檢查查詢字串值，並決定是否顯示成功訊息。

## <a name="running-the-application"></a>執行應用程式

您可以在購物車中執行應用程式現在若要查看您可以加入、 刪除和更新項目。 購物車總計會反映在購物車中的所有項目的總成本。

1. 在 [方案總管] 中，按**F5**執行 Wingtip Toys 範例應用程式。  
   瀏覽器隨即開啟並顯示*Default.aspx*頁面。
2. 按一下**登入**在頁面頂端的連結。 

    ![成員資格和系統管理-登入連結](membership-and-administration/_static/image2.png)

   *Login.aspx*頁面隨即顯示。
3. 使用下列的使用者名稱和密碼：  
   使用者名稱： canEditUser@wingtiptoys.com  
   密碼： Pa $$ word1 

    ![成員資格和系統管理-登入頁面](membership-and-administration/_static/image3.png)
4. 按一下**登入**靠近頁面底部的按鈕。
5. 在下一個頁面的頂端，選取**Admin**連結以瀏覽至*AdminPage.aspx*頁面。 

    ![成員資格和系統管理-系統管理員連結](membership-and-administration/_static/image4.png)
6. 若要測試輸入的驗證，請按一下**新增產品**而不加入任何產品詳細資料 按鈕。 

    ![成員資格和系統管理-管理頁面](membership-and-administration/_static/image5.png)

   請注意，必要的欄位會顯示的訊息。
7. 加入新的產品的詳細資料，然後按一下 [**新增產品**] 按鈕。 

    ![成員資格和系統管理-新增產品](membership-and-administration/_static/image6.png)
8. 選取**產品**加入從頂端導覽功能表上，若要檢視新的產品。 

    ![成員資格和系統管理-顯示新的產品](membership-and-administration/_static/image7.png)
9. 按一下**Admin**連結回到 [管理] 頁面。
10. 在**移除產品**> 一節的頁面上，選取您在加入新的產品**DropDownListBox**。
11. 按一下**移除產品**按鈕即可移除應用程式的新產品。 

    ![成員資格和系統管理-移除產品](membership-and-administration/_static/image8.png)
12. 選取**產品**從頂端導覽功能表上，確認已移除該產品。
13. 按一下**登出**存在的系統管理模式。   
    請注意，不會再顯示在上方瀏覽窗格**Admin**功能表項目。

## <a name="summary"></a>總結

在本教學課程中，您可以加入自訂安全性角色，屬於的自訂角色來管理資料夾和頁面上，限制存取的使用者，以及提供屬於自訂角色使用者的瀏覽。 您用來填入的模型繫結**DropDownList**控制項的資料。 您實作**檔案上傳**控制項和驗證控制項。 此外，您已經學會如何加入和移除資料庫中的產品。 在下一個教學課程中，您將學習如何實作 ASP.NET 路由。

## <a name="additional-resources"></a>其他資源

[Web.config 的授權項目](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[將具有成員資格、 OAuth、 和 SQL Database 的安全的 ASP.NET Web Form 應用程式部署至 Azure 網站](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure-免費試用版](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [上一頁](checkout-and-payment-with-paypal.md)
> [下一頁](url-routing.md)
