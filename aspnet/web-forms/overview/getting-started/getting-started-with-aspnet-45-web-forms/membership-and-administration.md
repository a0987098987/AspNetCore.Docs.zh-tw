---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: 成員資格和管理 |Microsoft Docs
author: Erikre
description: 本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Forms 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: 58eda260ccf2d0f237aaade65017760ed87a8c4f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368294"
---
<a name="membership-and-administration"></a>成員資格和管理
====================
藉由[Erik Reitan](https://github.com/Erikre)

[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Forms 應用程式的基本概念。 Visual Studio 2013[含有 C# 原始程式碼專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附了本教學課程系列。


本教學課程會示範如何更新 Wingtip Toys 範例應用程式，來新增自訂角色，並使用 ASP.NET 身分識別。 它也示範如何實作的自訂角色的使用者可以新增和移除網站中的產品管理頁面。

[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)是用來建置 ASP.NET web 應用程式的成員資格系統，並且使用 ASP.NET 4.5 中。 在 Visual Studio 2013 Web Form 專案範本，以及適用於的範本會使用 ASP.NET 身分識別[ASP.NET MVC](../../../../mvc/index.md)， [ASP.NET Web API](../../../../web-api/index.md)，和[ASP.NET 單一頁面應用程式](../../../../single-page-application/index.md). 您還會特別可以安裝與空的 Web 應用程式啟動時，使用 NuGet 的 ASP.NET 身分識別系統。 不過，在本教學課程系列中您使用**Web Form**projecttemplate，其中包含 ASP.NET 身分識別系統。 ASP.NET 身分識別可讓您更輕鬆地整合應用程式資料的特定使用者設定檔資料。 此外，ASP.NET 身分識別可讓您選擇 您的應用程式中的 使用者設定檔的持續性模型。 您可以將資料儲存在 SQL Server 資料庫或其他資料存放區，包括*NoSQL*資料存放區，例如 Windows Azure 儲存體資料表。

本教學課程是根據先前的教學課程，Wingtip Toys 教學課程系列中標題為 「 簽出和付款使用 PayPal"。

## <a name="what-youll-learn"></a>您將學到什麼：

- 如何將自訂的角色和使用者新增至應用程式中使用程式碼。
- 如何限制對管理資料夾，以及頁面存取。
- 如何提供隸屬於自訂的角色之使用者的瀏覽。
- 如何使用模型繫結來填入[DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx)與產品類別目錄的控制項。
- 如何將檔案上傳至 web 應用程式使用[FileUpload](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx)控制項。
- 如何使用驗證控制項來實作輸入的驗證。
- 如何新增和移除應用程式中的產品。

## <a name="these-features-are-included-in-the-tutorial"></a>在本教學課程中包含這些功能：

- ASP.NET Identity
- 組態和授權
- 模型繫結
- 低調驗證

ASP.NET Web Forms 提供成員資格功能。 藉由使用預設範本，您會有內建的成員資格功能，您可以立即使用應用程式執行時。 本教學課程會示範如何使用 ASP.NET 身分識別來加入自訂角色，並將使用者指派給該角色。 您將了解如何限制對管理資料夾的存取。 您將新增頁面可讓使用者使用自訂角色來新增和移除的產品，以及預覽產品之後已新增, 的系統管理 資料夾。

## <a name="adding-a-custom-role"></a>新增自訂角色

使用 ASP.NET 身分識別，新增自訂角色，並將使用者指派給該角色使用的程式碼。

1. 在**方案總管**，以滑鼠右鍵按一下*邏輯*資料夾，並建立新的類別。
2. 新類別命名*RoleActions.cs*。
3. 修改程式碼，讓它看起來像這樣：  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. 在 **方案總管**，開啟*Global.asax.cs*檔案。
5. 修改*Global.asax.cs*檔案中新增，使其出現，如下所示，以黃色醒目提示的程式碼：  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. 請注意，`AddUserAndRole`會以紅色加上底線。 按兩下 AddUserAndRole 程式碼。  
   字母"A"（反白顯示方法的開頭會加上底線。
7. 停留在字母"A"，然後按一下 UI，可讓您產生方法虛設常式`AddUserAndRole`方法。 

    ![成員資格和 Advministration-產生方法虛設常式](membership-and-administration/_static/image1.png)
8. 按一下標題為的選項：  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. 開啟*RoleActions.cs*檔案*邏輯*資料夾。  
   `AddUserAndRole`方法已加入至類別檔案。
10. 修改*RoleActions.cs*檔案，藉由移除`NotImplementedeException`和新增以黃色反白顯示的程式碼，使它看起來像這樣：  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

上述程式碼會先建立成員資格資料庫的資料庫內容。 成員資格資料庫也會儲存成 *.mdf*中的檔案*應用程式\_資料*資料夾。 您可以檢視此資料庫之後第一位使用者已登入此 web 應用程式。 

> [!NOTE] 
> 
> 如果您想要儲存成員資格資料，以及產品資料，您可以考慮使用相同**DbContext**您用來將產品的資料儲存在上述程式碼。


 *內部*關鍵字是類型 （例如類別） 和類型成員 （例如方法或屬性） 的存取修飾詞。 內部類型或成員都只能包含在相同的組件的檔案內存取 *(.dll*檔案)。 當您建置您的應用程式組件檔案 *(.dll*) 會建立包含您執行應用程式時所執行的程式碼。 

A`RoleStore`物件，可提供角色管理，會根據資料庫的內容建立。

> [!NOTE] 
> 
> 請注意，當`RoleStore`會建立物件，它會使用泛型`IdentityRole`型別。 這表示`RoleStore`只可包含`IdentityRole`物件。 也藉由使用泛型時，在記憶體中的資源會處理更好。


下一步`RoleManager`物件，會根據建立`RoleStore`您剛才建立的物件。 `RoleManager`物件會公開角色相關的 API，可用來自動將變更儲存到`RoleStore`。 `RoleManager`只可包含`IdentityRole`物件，因為程式碼會使用`<IdentityRole>`泛型型別。

您呼叫`RoleExists`方法，以判斷是否"canEdit"角色成員資格資料庫中存在。 如果不是，您會建立角色。

建立`UserManager`物件似乎是單純`RoleManager`控制，不過它是幾乎完全相同。 它只會在同一行而不是數個編碼。 在這裡，您傳遞的參數具現化作為新的物件，包含在括號中。

接下來您建立 「 canEditUser"使用者藉由建立新`ApplicationUser`物件。 然後，如果您已成功建立使用者，您將使用者新增至新的角色。

> [!NOTE] 
> 
> 稍後在本教學課程系列中的 「 ASP.NET 錯誤處理 」 教學課程期間，將會更新錯誤處理。


下一次應用程式啟動時，名為"canEditUser 」 的使用者會新增為名為"canEdit"的應用程式的角色。 稍後在本教學課程中，您將使用者身分登入 」 canEditUser 」 以顯示您將在本教學課程期間新增的其他功能。 如需 ASP.NET 身分識別的 API 詳細資訊，請參閱 < [Microsoft.AspNet.Identity 命名空間](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx)。 如需初始化 ASP.NET 身分識別系統的詳細資訊，請參閱 < [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs)。

### <a name="restricting-access-to-the-administration-page"></a>限制存取的管理頁面

Wingtip Toys 範例應用程式可讓匿名使用者和登入的使用者檢視及購買產品。 不過，有自訂"canEdit"角色的登入的使用者可以存取受限制的頁面，以新增和移除產品。

#### <a name="add-an-administration-folder-and-page"></a>新增管理資料夾和頁面

接下來，您將建立名為的資料夾*Admin* "canEditUser 」 使用者屬於 Wingtip Toys 的自訂角色的範例應用程式。

1. 以滑鼠右鍵按一下專案名稱 (**Wingtip Toys**) 中**方案總管**，然後選取**新增** - &gt; **新資料夾**.
2. 將新資料夾命名*Admin*。
3. 以滑鼠右鍵按一下*系統管理員*資料夾，然後選取**新增** - &gt; **新項目**。   
   隨即顯示 [ 新增項目] 對話方塊。
4. 選取  <strong>Visual C#</strong> - &gt; <strong>Web</strong>左側的 範本 群組。 從清單的中間，選取<strong>使用主版頁面的 Web Form</strong>，其命名<em>AdminPage.aspx</em><strong>，</strong> ，然後選取<strong>新增</strong>。
5. 選取  *Site.Master*主版頁面中，為檔案，然後再選擇**確定**。

#### <a name="add-a-webconfig-file"></a>加入 Web.config 檔案

藉由新增*Web.config*的檔案*管理員*資料夾中，您可以限制存取包含在資料夾中的頁面。

1. 以滑鼠右鍵按一下*系統管理員*資料夾，然後選取**新增** - &gt; **新項目**。  
   隨即顯示 [ 新增項目] 對話方塊。
2. 從 Visual C# web 範本的清單中，選取<strong>Web 組態檔</strong>從中間清單中，接受預設名稱<em>Web.config</em><strong>，</strong> ，然後選取<strong>新增</strong>。
3. 取代現有的 XML 中的內容*Web.config*以下列檔案：  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

儲存*Web.config*檔案。 *Web.config*檔案會指定屬於"canEdit"角色的應用程式的唯一使用者可以存取包含在頁面*管理員*資料夾。

### <a name="including-custom-role-navigation"></a>包括自訂角色瀏覽

若要啟用自訂"canEdit"角色的使用者，以瀏覽至 [管理] 區段中的應用程式，您必須新增連結*Site.Master*頁面。 只有屬於"canEdit"角色的使用者將能夠看到**Admin**連結和存取管理 區段中的色彩。

1. 在 [方案總管] 中，尋找並開啟*Site.Master*頁面。
2. 若要建立的連結"canEdit"角色的使用者，加入下列的未排序清單的黃色反白顯示的標記`<ul>`項目，讓清單會顯示為如下：  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. 開啟*Site.Master.cs*檔案。 製作**系統管理員**只加入程式碼中以黃色反白顯示"canEditUser 」 使用者可看見的連結`Page_Load`處理常式。 `Page_Load`處理常式將會出現，如下所示：   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

當頁面載入的程式碼會檢查登入的使用者是否具有"canEdit"角色。 如果使用者屬於"canEdit"角色，而範圍的項目，包含的連結*AdminPage.aspx*網頁 （且因此是在範圍內連結） 設為可見。

### <a name="enabling-product-administration"></a>啟用的產品管理

到目前為止，您已建立"canEdit"角色，並加入 「 canEditUser 」 使用者、 系統管理 資料夾，以及管理頁面。 您已設定 頁面上，與管理資料夾的存取權限和應用程式中新增"canEdit"角色的使用者之瀏覽連結。 接下來，您會將標記新增至*AdminPage.aspx*頁面上，並以程式碼*AdminPage.aspx.cs*程式碼後置檔案，可讓具有"canEdit"角色的使用者，新增和移除產品。

1. 中**方案總管**，開啟*AdminPage.aspx*從檔案*管理員*資料夾。
2. 以下列內容取代現有的標記：  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. 接下來，開啟*AdminPage.aspx.cs*程式碼後置檔案，以滑鼠右鍵按一下*AdminPage.aspx* ，然後按一下**檢視程式碼**。
4. 取代現有的程式碼中*AdminPage.aspx.cs*為下列程式碼的程式碼後置檔案：  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

在您所輸入的程式碼*AdminPage.aspx.cs*程式碼後置檔案，稱為類別`AddProducts`將產品加入至資料庫的實際的工作。 這個類別還不存在，因此會立即建立。

1. 在 [**方案總管] 中**，以滑鼠右鍵按一下*邏輯*資料夾，然後選取**新增** - &gt; **新項目**。   
   隨即顯示 [ 新增項目] 對話方塊。
2. 選取  **Visual C#**  - &gt; **程式碼**左側的 範本 群組。 然後，選取**類別**從中間清單並將它命名*AddProducts.cs*。   
   會顯示新的類別檔案。
3. 將現有的程式碼取代為下列程式碼：  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

*AdminPage.aspx*  頁面可讓屬於"canEdit"角色的使用者，新增和移除產品。 當加入新的產品時，產品的相關詳細資料已經過驗證，並接著將其輸入至資料庫。 Web 應用程式的所有使用者立即使用新的產品。

#### <a name="unobtrusive-validation"></a>低調驗證

使用者提供的產品詳細資料*AdminPage.aspx*頁面會使用驗證控制項進行驗證 (`RequiredFieldValidator`和`RegularExpressionValidator`)。 這些控制項，會自動使用低調驗證。 低調驗證可讓使用 JavaScript 的用戶端驗證邏輯，這表示頁面不需要往返於伺服器要驗證的驗證控制項。 根據預設，低調驗證會包含在*Web.config*檔案會根據下列的組態設定：

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>規則運算式

產品價格*AdminPage.aspx*使用驗證頁面**RegularExpressionValidator**控制項。 這個控制項驗證是否相關聯的輸入控制項 （"AddProductPrice 」 文字方塊） 的值符合規則運算式所指定的模式。 規則運算式是可讓您快速找出並符合特定的字元模式的模式比對標記法。 **RegularExpressionValidator**控制項包含一個名為屬性`ValidationExpression`，其中包含用來驗證價格輸入，如下所示的規則運算式：

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>FileUpload 控制項

在您加入的輸入與驗證控制項，除了**FileUpload**若要控制*AdminPage.aspx*頁面。 這個控制項提供的功能，將檔案上傳。 在此情況下，您只允許上傳的映像檔。 在程式碼後置檔案 (*AdminPage.aspx.cs*)，當`AddProductButton`按一下時，程式碼會檢查`HasFile`屬性**FileUpload**控制項。 如果控制項有一個檔案，而且如果允許檔案類型 （根據副檔名而定），則影像會儲存到*映像*資料夾並*映像/個大拇指朝*應用程式的資料夾。

#### <a name="model-binding"></a>模型繫結

稍早在本教學課程系列中您用來填入模型繫結**ListView**控制**FormsView**控制**GridView**控制項，和**DetailView**控制項。 在本教學課程中，您可以使用模型繫結填入**DropDownList**具有產品類別目錄的清單控制項。

您新增至標記*AdminPage.aspx*檔案包含**DropDownList**控制項稱為`DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

您可以使用模型繫結來填入這**DropDownList**藉由設定`ItemType`屬性和`SelectMethod`屬性。 `ItemType`屬性會指定您使用`WingtipToys.Models.Category`輸入填入控制項時。 您藉由建立定義此類型在開始本教學課程系列`Category`類別 （如下所示）。 `Category`類別位於*模型*資料夾內*Category.cs*檔案。

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

`SelectMethod`的屬性**DropDownList**控制項可讓您指定您使用`GetCategories`方法 （如下所示） 也就是包含在程式碼後置檔案 (*AdminPage.aspx.cs*)。

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

這個方法會指定`IQueryable`介面用來評估查詢，以針對`Category`型別。 傳回的值會用來填入**DropDownList**在網頁標記中 (*AdminPage.aspx*)。

顯示在清單中的每個項目是藉由設定所指定的文字`DataTextField`屬性。 `DataTextField`屬性會使用`CategoryName`的`Category`（如上所示） 以顯示每個類別目錄中的類別**DropDownList**控制項。 選取一個項目時所傳遞的實際值**DropDownList**控制根據`DataValueField`屬性。 `DataValueField`屬性設定為`CategoryID`中定義`Category`類別 （如上所示）。

### <a name="how-the-application-will-work"></a>應用程式如何運作

當第一次，屬於"canEdit"角色的使用者瀏覽至網頁`DropDownAddCategory` **DropDownList**上面所述，控制會填入。 `DropDownRemoveProduct` **DropDownList**控制項也會填入產品使用相同的方法。 選取類別目錄類型屬於"canEdit"角色的使用者，並加上產品詳細資料 (**名稱**，**描述**，**價格**，和**映像檔**). 當使用者屬於"canEdit"角色按一下**新增產品** 按鈕，`AddProductButton_Click`事件處理常式，就會觸發。 `AddProductButton_Click`事件處理常式位於程式碼後置檔案中 (*AdminPage.aspx.cs*) 會檢查以確定它符合允許的檔案類型的映像檔 *(.gif*， *.png*， *.jpeg*，或 *.jpg*)。 然後，映像檔會儲存至資料夾，Wingtip Toys 範例應用程式。 接下來，將新的產品加入至資料庫。 若要完成新增新的產品的新執行個體`AddProducts`類別會建立並命名為產品。 `AddProducts`類別具有名為方法`AddProduct`，產品物件呼叫這個方法，以將產品加入至資料庫。

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

如果程式碼已成功將新的產品加入資料庫中，頁面會重新查詢字串值載入`ProductAction=add`。

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

當頁面重新載入時，查詢字串會包含在 URL 中。 藉由重新載入頁面，屬於"canEdit"角色的使用者可以立即查看中的更新**DropDownList**上的控制項*AdminPage.aspx*頁面。 此外，藉由包含查詢字串的 url，網頁可以顯示成功的訊息屬於"canEdit"角色的使用者。

當*AdminPage.aspx*頁面上 重新載入，`Page_Load`事件被呼叫。

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

`Page_Load`事件處理常式會檢查查詢字串值，並決定是否要顯示成功訊息。

## <a name="running-the-application"></a>執行應用程式

購物車 」 中，您可以執行的應用程式現在以查看您可以新增、 刪除和更新項目。 購物車總計將會反映在購物車中的所有項目的總成本。

1. 在 [方案總管] 中，按下**F5**執行 Wingtip Toys 範例應用程式。  
   瀏覽器隨即開啟並顯示*Default.aspx*頁面。
2. 按一下 **登入**在頁面頂端的連結。 

    ![成員資格和系統管理-登入連結](membership-and-administration/_static/image2.png)

   *Login.aspx*頁面隨即顯示。
3. 使用下列的使用者名稱和密碼：  
   使用者名稱： canEditUser@wingtiptoys.com  
   密碼： Pa $$ word1 

    ![成員資格和系統管理-登入頁面](membership-and-administration/_static/image3.png)
4. 按一下 **登入**靠近頁面底部的按鈕。
5. 在下一個頁面頂端，選取**系統管理員**連結以瀏覽至*AdminPage.aspx*頁面。 

    ![成員資格和系統管理-系統管理員連結](membership-and-administration/_static/image4.png)
6. 若要測試輸入的驗證，請按一下**新增產品**而不加入任何產品詳細資料 按鈕。 

    ![成員資格和系統管理-系統管理 頁面](membership-and-administration/_static/image5.png)

   請注意，必要的欄位會顯示訊息。
7. 新增新的產品的詳細資料，然後按一下**新增產品** 按鈕。 

    ![成員資格和系統管理-新增產品](membership-and-administration/_static/image6.png)
8. 選取 **產品**加入從頂端導覽功能表，若要檢視新的產品。 

    ![成員資格和系統管理-顯示新的產品](membership-and-administration/_static/image7.png)
9. 按一下  **Admin**連結以返回 管理 頁面。
10. 在 **移除產品**一節的頁面上，選取您在加入新的產品**DropDownListBox**。
11. 按一下 **移除產品**按鈕即可移除應用程式中的新產品。 

    ![成員資格和系統管理-移除產品](membership-and-administration/_static/image8.png)
12. 選取 **產品**從頂端導覽功能表，確認已移除產品。
13. 按一下 **登出**存在於管理模式。   
    請注意，不會再顯示頂端瀏覽窗格**Admin**功能表項目。

## <a name="summary"></a>總結

在本教學課程中，您可以加入自訂的角色和使用者屬於自訂角色，也就是受限制的存取權管理資料夾，以及頁面上，並提供自訂的角色為屬於使用者的瀏覽。 您用來填入的模型繫結**DropDownList**控制項資料。 您已實作**FileUpload**控制項和驗證控制項。 此外，您已了解如何新增和移除資料庫中的產品。 在下一個教學課程中，您將了解如何實作 ASP.NET 路由。

## <a name="additional-resources"></a>其他資源

[Web.config 的 authorization 項目](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[ASP.NET 身分識別](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[將使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET Web Forms 應用程式部署至 Azure 網站](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure-免費試用版](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [上一頁](checkout-and-payment-with-paypal.md)
> [下一頁](url-routing.md)
