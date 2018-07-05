---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: 簽出與使用 PayPal 付款 |Microsoft Docs
author: Erikre
description: 本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Forms 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: e516bcccdecce72d4fa6c705b0227ce873429e3c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386658"
---
<a name="checkout-and-payment-with-paypal"></a>簽出與使用 PayPal 付款
====================
藉由[Erik Reitan](https://github.com/Erikre)

[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 本系列教學課程將教導您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Forms 應用程式的基本概念。 Visual Studio 2013[含有 C# 原始程式碼專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附了本教學課程系列。


本教學課程說明如何修改 Wingtip Toys 範例應用程式，以包含使用者的授權資料、 註冊與使用 PayPal 付款。 只有已登入的使用者必須購買產品的授權。 ASP.NET 4.5 Web Form 專案範本的內建的使用者註冊功能已包含許多您的需要。 您將新增 PayPal Express 簽出功能。 在本教學課程中，您會使用 PayPal 開發人員測試環境，因此會不傳輸任何實際的資金。 在本教學課程結束時，您將測試應用程式，選取要加入 「 購物車 」 中，按一下 [簽出] 按鈕，以及傳輸資料到 PayPal 測試網站的產品。 PayPal 測試網站上，您會確認您的傳送和付款資訊，然後返回本機 Wingtip Toys 範例應用程式，以確認及完成購買程序。

有數個特製化的經驗豐富的第三方付款處理器中線上購物，該位址延展性和安全性。 ASP.NET 開發人員應該考慮使用協力廠商付款解決方案之前實作購物和購買方案的優點。

> [!NOTE] 
> 
> Wingtip Toys 範例應用程式被設計成以 ASP.NET web 開發人員顯示 ASP.NET 的特定概念和可用的功能。 此範例應用程式不被適合所有可能的情況下，對於延展性和安全性。


## <a name="what-youll-learn"></a>您將學到什麼：

- 如何限制存取特定資料夾中的頁面。
- 如何建立從匿名的購物車 」 的已知的購物車。
- 如何為專案啟用 SSL。
- 如何將 OAuth 提供者新增至專案。
- 如何使用 PayPal 購買產品使用 PayPal 測試環境。
- 如何顯示詳細資料，從在 PayPal **DetailsView**控制項。
- 如何更新 Wingtip Toys 應用程式的資料庫，以取得從 PayPal 的詳細資訊。

## <a name="adding-order-tracking"></a>新增訂單追蹤

在本教學課程中，您將建立兩個新的類別，來追蹤資料從使用者所建立的順序。 類別會追蹤出貨資訊; 購買總計，以及付款確認的資料。

### <a name="add-the-order-and-orderdetail-model-classes"></a>新增的順序和 OrderDetail 模型類別

稍早在本教學課程系列中，您已定義之分類的產品，結構描述，而且建立購物車項目`Category`， `Product`，並`CartItem`中的類別*模型*資料夾。 現在您將加入兩個新的類別，來定義產品訂單及訂單的詳細資料的結構描述。

1. 在 **模型**資料夾中，加入新的類別，名為*Order.cs*。   
   新的類別檔案會顯示在編輯器中。
2. 您可以將預設的程式碼取代為下列：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. 新增*OrderDetail.cs*類別，即可*模型*資料夾。
4. 取代為下列程式碼中的預設程式碼：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

`Order`和`OrderDetail`類別包含的結構描述，以定義用於購買和出貨的訂單資訊。

此外，您必須更新資料庫內容類別，可管理的實體類別，並提供對資料庫的資料存取。 若要這樣做，您將加入新建立的順序並`OrderDetail`模型類別來`ProductContext`類別。

1. 在 **方案總管**，尋找並開啟*ProductContext.cs*檔案。
2. 將反白顯示的程式碼加入*ProductContext.cs*檔案，如下所示：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

如先前在本教學課程系列中的程式碼中所述*ProductContext.cs*檔都會將`System.Data.Entity`命名空間，所以您需要的 Entity framework 的所有核心功能的存取權。 這項功能包括查詢、 插入、 更新和刪除資料，藉由使用強型別物件的功能。 在上述程式碼`ProductContext`類別加入至新加入的 Entity Framework 存取`Order`和`OrderDetail`類別。

## <a name="adding-checkout-access"></a>新增簽出的存取權

Wingtip Toys 範例應用程式可讓匿名使用者檢閱，並將產品加入購物車。 不過，當匿名使用者選擇購買它們新增至購物車的產品時，他們必須登入至站台。 一旦登入，他們可以存取受限制的網頁之 Web 應用程式，處理簽出，並購買程序。 這些限制的頁面包含在*簽出*應用程式的資料夾。

### <a name="add-a-checkout-folder-and-pages"></a>新增簽出資料夾和網頁

您現在會建立*簽出*資料夾和它的客戶會看到在結帳程序中的頁面。 本教學課程稍後，您將會更新這些頁面。

1. 以滑鼠右鍵按一下專案名稱 (**Wingtip Toys**) 中**方案總管**，然後選取**加入新的資料夾**。 

    ![簽出和付款使用 PayPal-新的資料夾](checkout-and-payment-with-paypal/_static/image1.png)
2. 將新資料夾命名*簽出*。
3. 以滑鼠右鍵按一下*簽出*資料夾，然後選取**新增**-&gt;**新項目**。 

    ![簽出和付款使用 PayPal-新的項目](checkout-and-payment-with-paypal/_static/image2.png)
4. 隨即顯示 [ 新增項目] 對話方塊。
5. 選取  **Visual C#**  - &gt; **Web**左側的 範本 群組。 然後，從中間窗格中，選取**使用主版頁面的 Web Form**並將它命名*CheckoutStart.aspx*。 

    ![簽出與使用 PayPal 付款-加入新的項目 對話方塊](checkout-and-payment-with-paypal/_static/image3.png)
6. 同樣地，選取*Site.Master*主版頁面檔案。
7. 加入下列的其他頁面，以*簽出*資料夾使用上述相同步驟：   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>加入 Web.config 檔案

藉由新增新*Web.config*的檔案*簽出*資料夾中，您將能夠存取限於包含在資料夾中的所有頁面。

1. 以滑鼠右鍵按一下*簽出*資料夾，然後選取**新增** - &gt; **新項目**。  
   隨即顯示 [ 新增項目] 對話方塊。
2. 選取  **Visual C#**  - &gt; **Web**左側的 範本 群組。 然後，從中間窗格中，選取**Web 組態檔**，接受預設名稱*Web.config*，然後選取**新增**。
3. 取代現有的 XML 中的內容*Web.config*以下列檔案：  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. 儲存*Web.config*檔案。

*Web.config*檔案會指定所有未知的使用者，Web 應用程式必須拒絕存取的頁面中包含*簽出*資料夾。 不過，如果使用者已註冊的帳戶，並登入，他們會是已知的使用者，而且必須在頁面的存取權*簽出*資料夾。

請務必請注意，ASP.NET 組態如下所示的階層，其中每個*Web.config*檔案適用於組態設定中的資料夾和所有其下的子目錄。

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>為專案啟用 SSL

 安全通訊端層 (SSL) 是定義為允許 Web 伺服器和 Web 用戶端通訊更安全地透過加密的通訊協定。 不使用 SSL，用戶端和伺服器之間傳送的資料時，對封包探查實體存取網路的任何人開啟。 此外，數個常見的驗證結構描述是不安全的在一般的 HTTP。 特別是，基本驗證和表單驗證來傳送未加密的認證。 若要安全，這些驗證結構描述必須使用 SSL。 

1. 在 [**方案總管] 中**，按一下**WingtipToys**專案，然後按**F4**顯示**屬性**視窗。
2. 變更**啟用 SSL**至`true`。
3. 複製**SSL URL**讓您可以在稍後使用。   
 SSL URL 將是`https://localhost:44300/`除非您先前已建立 SSL Web Sites，（如下所示）。   
    ![專案屬性](checkout-and-payment-with-paypal/_static/image4.png)
4. 在 **方案總管**，以滑鼠右鍵按一下**WingtipToys**專案，然後按一下**屬性**。
5. 在左側索引標籤中，按一下**Web**。
6. 變更**專案 Url**使用**SSL URL**您稍早儲存。   
    ![專案 Web 屬性](checkout-and-payment-with-paypal/_static/image5.png)
7. 儲存頁面，按下**CTRL + S**。
8. 按 **Ctrl+F5** 執行應用程式。 Visual Studio 會顯示可避開 SSL 警告的選項。
9. 按一下 **是**信任 IIS Express SSL 憑證，並繼續。   
    ![IIS Express SSL 憑證的詳細資料](checkout-and-payment-with-paypal/_static/image6.png)  
 會顯示安全性警告。
10. 按一下 **是**將憑證安裝到您的 localhost。   
    ![安全性警告對話方塊](checkout-and-payment-with-paypal/_static/image7.png)  
 瀏覽器視窗隨即出現。

您現在可以輕鬆地測試在本機使用 SSL 的 Web 應用程式。

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>新增 OAuth 2.0 提供者

ASP.NET Web Forms 提供成員資格和驗證的增強功能的選項。 這些增強功能包括 OAuth。 OAuth 是開放式的通訊協定，可讓 web、 行動和桌面應用程式以簡單、 標準的方法中的安全授權。 ASP.NET Web Forms 範本使用 OAuth 來公開 Facebook、 Twitter、 Google 和 Microsoft 作為驗證提供者。 雖然本教學課程僅使用 Google 作為驗證提供者，您可以輕鬆地修改程式碼來使用任何提供者。 實作其他提供者的步驟會在本教學課程中，您會看到的步驟非常類似。

除了驗證之外，本教學課程也會使用角色來實作授權。 只有在您加入的使用者`canEdit`角色可以變更資料 （建立、 編輯或刪除連絡人）。

> [!NOTE] 
> 
> Windows Live 應用程式只接受即時工作網站 URL，因此您無法使用本機的網站 URL，測試登入。


下列步驟可讓您新增 Google 驗證提供者。

1. 開啟*應用程式\_Start\Startup.Auth.cs*檔案。
2. 移除註解字元`app.UseGoogleAuthentication()`方法讓，則方法會顯示為如下所示： 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. 瀏覽至[Google 開發人員主控台](https://console.developers.google.com/)。 您也必須使用您的 Google 開發人員電子郵件帳戶 (gmail.com) 登入。 如果您沒有 Google 帳戶，請選取**建立帳戶**連結。   
   接下來，您會看到**Google 開發人員主控台**。   
    ![Google 開發人員主控台](checkout-and-payment-with-paypal/_static/image8.png)
4. 按一下 **建立專案**按鈕，然後輸入專案名稱和識別碼 （您可以使用預設值）。 然後，按一下**協議 核取方塊**並**建立** 按鈕。  

    ![Google-新增專案](checkout-and-payment-with-paypal/_static/image9.png)

   在幾秒鐘的時間將會建立新的專案，您的瀏覽器會顯示新的 [專案] 頁面。
5. 在左側索引標籤中，按一下**Api &amp; auth**，然後按一下**認證**。
6. 按一下 **建立新的用戶端識別碼**下方**OAuth**。   
   **建立用戶端識別碼**就會顯示對話方塊。   
    ![Google-建立用戶端識別碼](checkout-and-payment-with-paypal/_static/image10.png)
7. 在 [**建立用戶端識別碼**] 對話方塊中，保留預設值**Web 應用程式**應用程式類型。
8. 設定**授權 JavaScript Origins**至您稍早在本教學課程中使用的 SSL URL (`https://localhost:44300/`除非您已建立 SSL 的其他專案)。   
   此 URL 是您的應用程式的原點。 此範例中，您將僅輸入 localhost 測試 URL。 不過，您可以輸入帳戶的 localhost 和生產環境的多個 Url。
9. 設定**Authorized Redirect URI**如下： 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   這個值是 URI ASP.NET OAuth 使用者與 google OAuth 伺服器進行通訊。 請記住您在上面使用的 SSL URL (`https://localhost:44300/`除非您已建立 SSL 的其他專案)。
10. 按一下 [**建立用戶端識別碼**] 按鈕。
11. 在 Google 開發人員主控台的左側功能表中，按一下 **同意畫面**功能表項目，然後設定您的電子郵件地址和產品名稱。 完成表單後，按一下**儲存**。
12. 按一下  **Api**功能表項目，向下的捲動，然後按一下  **off**旁**Google + API**。   
    接受此選項可讓 Google + API。
13. 您也必須更新**Microsoft.Owin** 3.0.0 版的 NuGet 封裝。   
    從**工具**功能表上，選取**NuGet 套件管理員**，然後選取**管理方案的 NuGet 套件**。  
    從**管理 NuGet 套件** 視窗中，尋找並更新**Microsoft.Owin** 3.0.0 版的封裝。
14. 在 Visual Studio 中，更新`UseGoogleAuthentication`方法*Startup.Auth.cs*藉由複製並貼上頁面**用戶端識別碼**並**用戶端祕密**至方法。 **用戶端識別碼**並**用戶端祕密**如下所示的值是範例，將無法運作。 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. 按下**CTRL + F5**以建置並執行應用程式。 按一下 **登入**連結。
16. 底下**另一個服務用來登入**，按一下**Google**。  
    ![登入](checkout-and-payment-with-paypal/_static/image11.png)
17. 如果您需要輸入認證時，您將會重新導向至 google 網站，您會在此輸入您的認證。  
    ![Google-登入](checkout-and-payment-with-paypal/_static/image12.png)
18. 您輸入認證之後，系統會提示您授與您剛才建立的 web 應用程式的權限。  
    ![專案預設服務帳戶](checkout-and-payment-with-paypal/_static/image13.png)
19. 按一下 **接受**。 您現在會重新導向回到**註冊**頁**WingtipToys**應用程式，您可以在此註冊您的 Google 帳戶。  
    ![使用您的 Google 帳戶註冊](checkout-and-payment-with-paypal/_static/image14.png)
20. 您可以選擇變更用於 Gmail 帳戶、 本機電子郵件註冊名稱，但您通常想要保留預設電子郵件別名 （也就是一個您用來驗證）。 按一下 **登入**如上所示。

### <a name="modifying-login-functionality"></a>修改登入功能

如先前所述在本教學課程系列中，大部分的使用者註冊功能已包含在 ASP.NET Web Forms 範本預設。 現在您將修改預設值*Login.aspx*並*Register.aspx*頁面來呼叫`MigrateCart`方法。 `MigrateCart`方法關聯匿名的購物車中的新登入的使用者。 藉由建立關聯的使用者和購物車，Wingtip Toys 範例應用程式將能夠維護購物車，每次造訪的使用者。

1. 在 **方案總管**，尋找並開啟*帳戶*資料夾。
2. 修改程式碼後置頁面，名為*Login.aspx.cs*加入以黃色反白顯示的程式碼，使它看起來像這樣：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. 儲存*Login.aspx.cs*檔案。

現在，您可以忽略沒有任何定義的警告`MigrateCart`方法。 您將新增它稍後在本教學課程。

*Login.aspx.cs*支援登入方法的程式碼後置檔案。 藉由檢查的 Login.aspx 頁面，您會看到此頁面包含的 「 登入 」 按鈕，當按一下 觸發程序`LogIn`上程式碼後置處理常式。

當`Login`方法*Login.aspx.cs*會呼叫名為購物籃中的新執行個體`usersShoppingCart`建立。 擷取的購物車 (GUID) 識別碼並將其設為`cartId`變數。 然後，`MigrateCart`呼叫方法時，傳遞兩者`cartId`和這個方法登入的使用者名稱。 購物車移轉時，用來識別匿名購物車的 GUID 會取代使用者名稱。

除了修改*Login.aspx.cs*程式碼後置檔案，以移轉購物車，當使用者登入，您也必須修改*Register.aspx.cs 程式碼後置檔案*移轉購物車當使用者建立新的帳戶及登入。

1. 在 *帳號*資料夾中，開啟的程式碼後置檔案命名為*Register.aspx.cs*。
2. 修改程式碼後置檔案包含下列程式碼以黃色，使其出現，如下所示：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. 儲存*Register.aspx.cs*檔案。 同樣地，關於忽略警告`MigrateCart`方法。

請注意程式碼中所使用`CreateUser_Click`事件處理常式是非常類似於您在中使用的程式碼`LogIn`方法。 當使用者註冊或登入站台，而呼叫`MigrateCart`方法會進行。

## <a name="migrating-the-shopping-cart"></a>移轉購物車

已更新的登入和註冊程序之後，您可以加入購物車 」 使用移轉的程式碼`MigrateCart`方法。

1. 在 **方案總管**，尋找*邏輯*資料夾，然後開啟*ShoppingCartActions.cs*類別檔案。
2. 加入現有的程式碼中的黃色反白顯示的程式碼*ShoppingCartActions.cs*檔案，以便中的程式碼*ShoppingCartActions.cs*檔案看起來像這樣：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

`MigrateCart`方法會使用現有 cartId 來尋找使用者的購物車。 接下來，所有的購物車項目執行迴圈的程式碼，並取代`CartId`屬性 (依照`CartItem`結構描述) 的登入的使用者名稱。

### <a name="updating-the-database-connection"></a>正在更新資料庫連接

如果您要遵循此教學課程使用**預先建置**Wingtip Toys 範例應用程式，您必須重新建立的預設成員資格資料庫。 藉由修改預設的連接字串，成員資格資料庫將會建立下一次執行應用程式。

1. 開啟*Web.config*專案根目錄的檔案。
2. 更新預設的連接字串，使它看起來像這樣：   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>整合 PayPal

PayPal 是網頁型計費平台可接受的線上商家的付款。 本教學課程接下來會說明如何將 PayPal 的 Express 簽出功能整合到您的應用程式。 快速簽出可讓您的客戶使用 PayPal 支付它們新增至購物車的項目。

### <a name="create-paylpal-test-accounts"></a>建立 PaylPal 測試帳戶

若要使用 PayPal 測試環境，您必須建立並驗證開發人員測試帳戶。 若要建立買方測試帳戶和賣方測試帳戶，您將使用開發人員測試帳戶。 開發人員測試帳戶認證也可讓 Wingtip Toys 範例應用程式存取 PayPal 測試環境。

1. 在瀏覽器中，瀏覽至 PayPal 的開發人員測試站台：   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. 如果您沒有 PayPal 開發人員帳戶，建立新的帳戶，即可**註冊**並遵循註冊步驟。 如果您有現有的 PayPal 開發人員帳戶，登入，即可**登入**。 您將需要 PayPal 開發人員帳戶來測試本教學課程稍後的 Wingtip Toys 範例應用程式。
3. 如果您只是已註冊 PayPal 開發人員帳戶，您可能需要驗證您使用 PayPal 的 PayPal 開發人員帳戶。 您可以遵循 PayPal 傳送電子郵件帳戶的步驟，以確認您的帳戶。 一旦確認您 PayPal 的開發人員帳戶，登入 PayPal 的開發人員測試站台。
4. 之後您登入 PayPal 開發人員網站與您 PayPal 開發人員帳戶，您需要建立 PayPal 買方測試帳戶，如果您不知道已經有一個。 若要在 PayPal 網站，按一下 建立購買者測試帳戶，**應用程式**索引標籤，然後按一下**沙箱帳戶**。   
 **沙箱測試帳戶**頁面會顯示。   

    > [!NOTE] 
    > 
    > PayPal 開發人員網站已提供商家的測試帳戶。

    ![簽出與使用 PayPal-沙箱測試帳戶的付款](checkout-and-payment-with-paypal/_static/image15.png)
5. 在沙箱測試的 [帳戶] 頁面中，按一下**建立帳戶**。
6. 在 **建立測試帳戶**頁面上選擇買方測試帳戶的電子郵件和您選擇的密碼。   

    > [!NOTE] 
    > 
    > 您必須購買者電子郵件地址和密碼才能測試 Wingtip Toys 範例應用程式，在本教學課程結尾處。

    ![簽出與使用 PayPal-沙箱測試帳戶的付款](checkout-and-payment-with-paypal/_static/image16.png)
7. 建立買方測試帳戶，方法是按一下**建立帳戶** 按鈕。  
 **沙箱測試帳戶**頁面隨即顯示。 

    ![簽出與使用 PayPal-PaylPal 帳戶的付款](checkout-and-payment-with-paypal/_static/image17.png)
8. 在 **沙箱測試帳戶**頁面上，按一下**促進**電子郵件帳戶。  
    **設定檔**並**通知**選項會出現。
9. 選取 **設定檔**選項，然後按一下**API 認證**若要檢視您的 API 認證商家的測試帳戶。
10. 將測試 API 認證複製到 [記事本]。

您必須顯示傳統測試 API 認證 （使用者名稱、 密碼和簽章） 進行 API 呼叫，Wingtip Toys 範例應用程式從測試環境的 PayPal。 您將在下一個步驟中新增認證。

### <a name="add-paypal-class-and-api-credentials"></a>新增 PayPal 類別和 API 認證

您會將大部分的 PayPal 程式碼，為單一類別。 這個類別包含用來使用 PayPal 進行通訊的方法。 此外，您會將您 PayPal 的認證新增至此類別。

1. 在 Visual Studio 內 Wingtip Toys 範例應用程式，以滑鼠右鍵按一下**邏輯**資料夾，然後選取**新增** - &gt; **新項目**。   
   隨即顯示 [ 新增項目] 對話方塊。
2. 底下**Visual C#** 從**已安裝**左邊的窗格，選取**程式碼**。
3. 從中間窗格中，選取**類別**。 這個新類別命名**PayPalFunctions.cs**。
4. 按一下 [加入] 。  
   新的類別檔案會顯示在編輯器中。
5. 取代為下列程式碼中的預設程式碼：  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. 新增您稍早在本教學課程中，讓您可以函式呼叫 PayPal 測試環境顯示 Merchant API 認證 （使用者名稱、 密碼和簽章）。  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> 在此範例應用程式只會將認證加入至 C# 檔案 (.cs)。 不過，在實作的解決方案中，您應該考慮加密您在組態檔中的認證。


NVPAPICaller 類別包含大部分的 PayPal 功能。 在類別中的程式碼提供購買從 PayPal 測試環境的測試所需的方法。 下列三個 PayPal 函式用來進行購買：

- `SetExpressCheckout` 函式
- `GetExpressCheckoutDetails` 函式
- `DoExpressCheckoutPayment` 函式

`ShortcutExpressCheckout`方法會收集測試採購資訊與產品詳細資料，從購物車，然後呼叫`SetExpressCheckout`PayPal 函式。 `GetCheckoutDetails`方法確認採購詳細資料，並呼叫`GetExpressCheckoutDetails`PayPal 函式之前先測試購買。 `DoCheckoutPayment`方法呼叫完成從測試環境的測試購買`DoExpressCheckoutPayment`PayPal 函式。 其餘的程式碼支援的 PayPal 方法和處理程序，例如字串的編碼方式、 解碼字串、 處理陣列，以及決定認證。

> [!NOTE] 
> 
> PayPal 可讓您包含選擇性的購買詳細資料，根據[PayPal 的 API 規格](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout)。 您可以藉由擴充 Wingtip Toys 範例應用程式中的程式碼，包含當地語系化詳細資料、 產品描述、 稅務、 客戶服務編號，以及許多其他選擇性欄位。


請注意，傳回與 [取消] Url 中指定**ShortcutExpressCheckout**方法使用的連接埠號碼。

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Visual Web Developer 中執行時使用 SSL 的 web 專案，通常使用連接埠 44300 web 伺服器。 如上所示，連接埠號碼是 44300。 當您執行應用程式時，您可以看到不同的連接埠號碼。 連接埠號碼必須正確設定程式碼中，您才能成功執行 Wingtip Toys 範例應用程式在本教學課程結尾處。 本教學課程的下一節會說明如何擷取本機主機通訊埠編號和 PayPal 類別的更新。

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>更新 PayPal 類別中的 LocalHost 連接埠號碼

Wingtip Toys 範例應用程式會購買產品，瀏覽到 PayPal 測試站台，並將傳回給 Wingtip Toys 範例應用程式的本機執行個體。 若要有 PayPal 傳回至正確的 URL，您必須指定連接埠號碼，在本機執行的應用程式中先前所述的 PayPal 程式碼範例。

1. 以滑鼠右鍵按一下專案名稱 (**WingtipToys**) 中**方案總管**，然後選取**屬性**。
2. 在左欄中，選取**Web**  索引標籤。
3. 擷取連接埠號碼**專案 Url**  方塊中。
4. 如有需要更新`returnURL`並`cancelURL`PayPal 類別中 (`NVPAPICaller`) 中*PayPalFunctions.cs*檔案，以使用您的 web 應用程式的連接埠號碼：   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

現在您加入的程式碼會比對您的本機 Web 應用程式的預期連接埠。 PayPal 能夠回到本機電腦上的正確的 URL。

### <a name="add-the-paypal-checkout-button"></a>新增 [PayPal 簽出] 按鈕

現在，主要的 PayPal 函式已新增至範例應用程式，您可以開始加入標記和呼叫這些函式所需的程式碼。 首先，您必須新增使用者會看到在購物車 頁面的 簽出 按鈕。

1. 開啟*ShoppingCart.aspx*檔案。
2. 捲動到檔案底部，然後尋找`<!--Checkout Placeholder -->`註解。
3. 使用註解取代`ImageButton`控制，讓標記取代，如下所示：  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. 在  *ShoppingCart.aspx.cs*檔案，之後`UpdateBtn_Click`檔案，結尾附近的事件處理常式加入`CheckOutBtn_Click`事件處理常式：  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. 此外，在*ShoppingCart.aspx.cs*檔案中，將參考加入`CheckoutBtn`，如此一來，新的影像按鈕參考，如下所示：  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. 將變更儲存至兩者*ShoppingCart.aspx*檔案並*ShoppingCart.aspx.cs*檔案。
7. 從功能表中，選取**偵錯**-&gt;**建置 WingtipToys**。  
   將會重建專案以加入新**ImageButton**控制項。

### <a name="send-purchase-details-to-paypal"></a>採購單詳細資料傳送至 PayPal

當使用者按一下**簽出**購物車 頁面上的按鈕 (*ShoppingCart.aspx*)，它們會開始在購買程序。 下列程式碼會呼叫第一個所需購買產品的 PayPal 函式。

1. 從*簽出*資料夾中，開啟的程式碼後置檔案命名為*CheckoutStart.aspx.cs*。   
   請務必開啟程式碼後置檔案。
2. 將現有的程式碼取代為下列程式碼：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

當應用程式的使用者按一下**簽出**在購物車頁面上，瀏覽器 按鈕會瀏覽至*CheckoutStart.aspx*頁面。 當*CheckoutStart.aspx*頁面會隨即載入，`ShortcutExpressCheckout`呼叫方法。 此時，使用者會轉移至 PayPal 測試的網站。 PayPal 在網站上，使用者會輸入他們的 PayPal 認證、 檢閱購買詳細資料、 接受 PayPal 合約和 「 Wingtip Toys 範例應用程式會傳回其中`ShortcutExpressCheckout`方法完成。 當`ShortcutExpressCheckout`方法完成時，它會將使用者重新導向*CheckoutReview.aspx*頁面中指定`ShortcutExpressCheckout`方法。 這可讓使用者檢閱 Wingtip Toys 範例應用程式中的從訂單詳細資料。

### <a name="review-order-details"></a>檢閱訂單詳細資料

傳回從 PayPal 之後, *CheckoutReview.aspx* Wingtip Toys 範例應用程式的頁面會顯示訂單詳細資料。 此頁面可讓使用者購買產品前請先檢閱訂單詳細資料。 *CheckoutReview.aspx*必須建立頁面，如下所示：

1. 在 *簽出*資料夾中，開啟名為網頁*CheckoutReview.aspx*。
2. 以下列內容取代現有的標記：   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. 開啟名為程式碼後置頁面*CheckoutReview.aspx.cs*並以下列內容取代現有的程式碼：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

**DetailsView**控制項用來顯示已從 PayPal 傳回訂單詳細資料。 而且，上述程式碼會將訂單詳細資料儲存至 Wingtip Toys 資料庫做`OrderDetail`物件。 當使用者按一下**Complete Order**  按鈕，就會重新導向至*CheckoutComplete.aspx*頁面。

> [!NOTE] 
> 
> **祕訣**
> 
> 在標記*CheckoutReview.aspx*頁面上，注意`<ItemStyle>`標記用來變更中項目的樣式**DetailsView**靠近頁面底部的控制項。 藉由檢視中的網頁**設計檢視**(藉由選取**設計**在 Visual Studio 左下角)，然後選取**DetailsView**控制項，然後選取**智慧標籤**(在頂端的箭號圖示右邊的控制項)，您將能夠看到**DetailsView 工作**。
> 
> ![簽出與使用 PayPal 付款-編輯欄位](checkout-and-payment-with-paypal/_static/image18.png)
> 
> 藉由選取**編輯欄位**，則**欄位**對話方塊會隨即出現。 在此對話方塊中您可以輕鬆地控制的視覺屬性，例如**ItemStyle**的**DetailsView**控制項。
> 
> ![簽出與使用 PayPal-欄位 對話方塊的付款](checkout-and-payment-with-paypal/_static/image19.png)


### <a name="complete-purchase"></a>完成購買程序

*CheckoutComplete.aspx*網頁可讓從 PayPal 的購買。 如先前所述，使用者必須按一下**Complete Order**按鈕時，應用程式會瀏覽至之前*CheckoutComplete.aspx*頁面。

1. 在 *簽出*資料夾中，開啟名為網頁*CheckoutComplete.aspx*。
2. 以下列內容取代現有的標記：   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. 開啟名為程式碼後置頁面*CheckoutComplete.aspx.cs*並以下列內容取代現有的程式碼：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

當*CheckoutComplete.aspx*載入頁面時，`DoCheckoutPayment`呼叫方法。 如前所述，`DoCheckoutPayment`方法完成購買程序，從 PayPal 測試環境。 PayPal 完成之後的訂單，採購*CheckoutComplete.aspx*頁面會顯示付款交易`ID`來購買。

### <a name="handle-cancel-purchase"></a>處理取消購買

如果使用者決定取消購買，它們會被導向至*CheckoutCancel.aspx*頁面，它們會在其中查看其訂單已取消。

1. 開啟名為頁面*CheckoutCancel.aspx*中*簽出*資料夾。
2. 以下列內容取代現有的標記：   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>處理購買錯誤

在購買程序期間發生的錯誤會由*CheckoutError.aspx*頁面。 程式碼後置*CheckoutStart.aspx*頁面上， *CheckoutReview.aspx*頁面上，而*CheckoutComplete.aspx*頁面將每個重新導向至*CheckoutError.aspx*頁面上，如果發生錯誤。

1. 開啟名為頁面*CheckoutError.aspx*中*簽出*資料夾。
2. 以下列內容取代現有的標記：   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

*CheckoutError.aspx*簽出程序期間發生錯誤時，將會顯示錯誤詳細資料的頁面。

## <a name="running-the-application"></a>執行應用程式

執行應用程式，以了解如何購買產品。 請注意，您將執行中的 PayPal 測試環境。 要交換沒有實際的金額。

1. 請確定所有檔案會儲存在 Visual Studio。
2. 開啟網頁瀏覽器並瀏覽至[ https://developer.paypal.com ](https://developer.paypal.com/)。
3. 使用您稍早在本教學課程中建立您 PayPal 開發人員帳戶的登入。  
   您必須在登入 PayPal 的開發人員沙箱[ https://developer.paypal.com ](https://developer.paypal.com/)測試快速簽出。 這只適用於 PayPal 的沙箱測試、 未 PayPal 的即時環境。
4. 在 Visual Studio 中按**F5**執行 Wingtip Toys 範例應用程式。  
   重建資料庫之後，瀏覽器會開啟並顯示*Default.aspx*頁面。
5. 選取產品類別目錄，例如 Cars，然後按一下 加入購物車的三個不同的產品**加入購物車**旁邊每項產品。  
   購物車就會顯示您已選取的產品。
6. 按一下 [ **PayPal**簽出] 按鈕。 

    ![簽出與使用 PayPal 付款-購物車](checkout-and-payment-with-paypal/_static/image20.png)

   正在簽出，將會需要您具備 Wingtip Toys 範例應用程式的使用者帳戶。
7. 按一下  **Google**上現有的 gmail.com 電子郵件帳戶登入頁面右側的連結。  
   如果您沒有 gmail.com 帳戶，您可以建立另一個用於測試用途[www.gmail.com](https://www.gmail.com/)。 您也可以使用標準的本機帳戶，依序按一下 [註冊]。 

    ![簽出與使用 PayPal 付款-登入](checkout-and-payment-with-paypal/_static/image21.png)
8. 使用您的 gmail 帳戶和密碼登入。 

    ![簽出與使用 PayPal-Gmail 登入的付款](checkout-and-payment-with-paypal/_static/image22.png)
9. 按一下 [**登入**gmail 帳戶向您 Wingtip Toys 範例應用程式的使用者名稱] 按鈕。 

    ![簽出與使用 PayPal-註冊帳戶的付款](checkout-and-payment-with-paypal/_static/image23.png)
10. PayPal 測試在網站上，新增您**買方**電子郵件地址和您稍早在本教學課程中建立的密碼，然後按一下**登入** 按鈕。 

    ![簽出與使用 PayPal-PayPal 登入的付款](checkout-and-payment-with-paypal/_static/image24.png)
11. 同意 PayPal 原則，然後按一下**同意後繼續** 按鈕。  
    請注意，此頁面才會顯示第一次您使用此 PayPal 帳戶。 再次請注意，這是測試帳戶，交換任何實際的成本。 

    ![簽出和使用 PayPal-原則 PayPal 付款](checkout-and-payment-with-paypal/_static/image25.png)
12. 檢閱訂單資訊在測試環境 檢閱 頁面，然後按一下 PayPal**繼續**。 

    ![簽出與使用 PayPal-檢閱資訊的付款](checkout-and-payment-with-paypal/_static/image26.png)
13. 在  *CheckoutReview.aspx*頁面上，確認訂單金額，檢視產生的送貨地址。 然後，按一下**Complete Order**  按鈕。 

    ![簽出和使用 PayPal-順序檢閱付款](checkout-and-payment-with-paypal/_static/image27.png)
14. **CheckoutComplete.aspx**頁面會顯示與付款交易識別碼。 

    ![簽出與使用 PayPal-簽出完整的付款](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>檢閱資料庫

藉由檢閱更新的資料，Wingtip Toys 範例應用程式資料庫中執行應用程式之後，您可以看到應用程式成功地錄製購買的產品。

您可以檢查所包含的資料*Wingtiptoys.mdf*使用的資料庫檔案**資料庫總管**視窗 (**伺服器總管**Visual Studio 中的視窗) 一樣稍早在本教學課程系列。

1. 如果它仍為開啟狀態，請關閉瀏覽器視窗。
2. 在 Visual Studio 中，選取**顯示所有檔案**頂端的圖示**方案總管**可讓您展開**應用程式\_資料**資料夾。
3. 依序展開**應用程式\_資料**資料夾。  
 您可能需要選取**顯示所有檔案**資料夾圖示。
4. 以滑鼠右鍵按一下*Wingtiptoys.mdf*資料庫檔案，然後選取**開啟**。  
    **伺服器總管**隨即出現。
5. 依序展開**資料表**資料夾。
6. 以滑鼠右鍵按一下**訂單**資料表，然後選取**顯示資料表資料**。  
 **訂單**資料表會顯示。
7. 檢閱**PaymentTransactionID**資料行，以確認成功的交易。 

    ![簽出與使用 PayPal-檢閱資料庫的付款](checkout-and-payment-with-paypal/_static/image29.png)
8. 關閉**訂單**資料表視窗中。
9. 在 [伺服器總管] 中，以滑鼠右鍵按一下**OrderDetails**資料表，然後選取**顯示資料表資料**。
10. 檢閱`OrderId`並`Username`中的值**OrderDetails**資料表。 請注意，這些值符合`OrderId`並`Username`值納入**訂單**資料表。
11. 關閉**OrderDetails**資料表視窗中。
12. 以滑鼠右鍵按一下 Wingtip Toys 資料庫檔案 (*Wingtiptoys.mdf*)，然後選取**親密**。
13. 如果您看不**方案總管 中** 視窗中，按一下**方案總管 中**底部**伺服器總管**視窗以顯示**方案總管 中**一次。

## <a name="summary"></a>總結

在本教學課程中新增訂單及訂單詳細資料的結構描述追蹤購買的產品。 您也會整合到 Wingtip Toys 範例應用程式的 PayPal 功能。

## <a name="additional-resources"></a>其他資源

[PřEHLED Konfigurace Technologie asp.net](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET Web Forms 應用程式部署至 Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure-免費試用版](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>免責聲明

本教學課程包含範例程式碼。 這類的範例程式碼係依 「 現況 」 不含任何種類的擔保。 因此，Microsoft 不保證精確度、 完整性或範例程式碼的品質。 若要自行承擔使用的範例程式碼即表示您同意。 在任何情況下 Microsoft 一概不給您以任何方式任何範例程式碼，內容，包括但不是限於任何錯誤或省略任何範例程式碼、 內容或任何遺失或損毀所造成的結果的任何範例程式碼使用任何種類。 您茲此通知和茲此同意賠償、 儲存及保留 Microsoft 無害因而遭受任何和所有的遺失、 遺失、 傷害或任何種類包括，但不限於由 occasioned 或所引發的回傳時，資料的損毀的宣告傳輸、 使用或依賴包括但不是限於其中表示的檢視。

> [!div class="step-by-step"]
> [上一頁](shopping-cart.md)
> [下一頁](membership-and-administration.md)
