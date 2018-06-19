---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: 簽出和與 PayPal 付款 |Microsoft 文件
author: Erikre
description: 此教學課程將告訴您建置使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for 我們的 ASP.NET Web Form 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 0dba613594686a28b82bc6d7701cda6e24b82e2e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2018
ms.locfileid: "30891889"
---
<a name="checkout-and-payment-with-paypal"></a>簽出和與 PayPal 付款
====================
由[Erik Reitan](https://github.com/Erikre)

[下載 Wingtip Toys 範例專案 (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)或[下載電子書 (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> 此教學課程將告訴您建置一個使用 ASP.NET 4.5 和 Microsoft Visual Studio Express 2013 for Web 的 ASP.NET Web Form 應用程式的基本概念。 Visual Studio 2013[與 C# 原始程式碼的專案](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)隨附此教學課程了。


本教學課程說明如何修改 Wingtip Toys 範例應用程式，包括使用者的授權、 註冊和使用 PayPal 付款。 使用者已登入必須購買產品的授權。 ASP.NET 4.5 Web Form 專案範本的內建的使用者註冊功能已包含許多您的需要。 您將加入 PayPal Express 簽出功能。 在本教學課程中，您會使用 PayPal 開發人員在測試環境中，因此會不傳輸任何實際的款項。 在本教學課程結束時，您將測試應用程式選取產品加入購物車，然後按一下 [簽出] 按鈕，再將資料傳輸到 PayPal 測試的網站。 PayPal 測試在網站上，您會確認您的傳送和付款資訊，然後再回頭本機 Wingtip Toys 範例應用程式，以確認並完成購買程序。

有數個特製化的經驗豐富的協力廠商付款處理器中線上購物，該位址延展性和安全性。 ASP.NET 開發人員應該考慮利用協力廠商付款解決方案之前實作購物和購買方案的優點。

> [!NOTE] 
> 
> Wingtip Toys 範例應用程式被設計成以 ASP.NET web 開發人員顯示 ASP.NET 的特定概念與功能。 此範例應用程式不是為所有可能的情況下就延展性和安全性最佳化。


## <a name="what-youll-learn"></a>您將學習：

- 如何限制存取特定網頁的資料夾中。
- 如何建立從匿名的購物車的已知的購物車。
- 如何為專案啟用 SSL。
- 如何將 OAuth 提供者加入至專案。
- 如何使用 PayPal 購買產品使用 PayPal 測試環境。
- 如何顯示詳細資料中的 paypal **DetailsView**控制項。
- 如何更新詳細資料取自 PayPal Wingtip Toys 應用程式的資料庫。

## <a name="adding-order-tracking"></a>加入訂單追蹤

在本教學課程中，您將建立兩個新的類別來追蹤資料從使用者建立的順序。 類別會追蹤出貨資訊、 訂單總計和付款確認的資料。

### <a name="add-the-order-and-orderdetail-model-classes"></a>新增的順序和 OrderDetail 模型類別

稍早在本教學課程的系列中已定義類別目錄、 產品、 結構描述和購物車的項目藉由建立`Category`， `Product`，和`CartItem`中的類別*模型*資料夾。 現在您會加入兩個新的類別來定義產品訂單和訂單的詳細資料的結構描述。

1. 在**模型**資料夾中，加入新的類別，名為*Order.cs*。   
   在編輯器中，會顯示新的類別檔案。
2. 以下列內容取代預設程式碼：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. 新增*OrderDetail.cs*類別*模型*資料夾。
4. 下列程式碼取代預設程式碼：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

`Order`和`OrderDetail`類別包含的結構描述來定義用於購買和出貨的訂單資訊。

此外，您必須更新管理的實體類別，以及提供資料存取資料庫的資料庫內容類別。 若要這樣做，您將加入新建立的順序和`OrderDetail`模型類別`ProductContext`類別。

1. 在**方案總管 中**、 尋找和開啟*ProductContext.cs*檔案。
2. 將反白顯示的程式碼加入*ProductContext.cs*檔案如下所示：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

如先前所述，本教學課程系列中的程式碼*ProductContext.cs*檔都會將`System.Data.Entity`命名空間，所以您需要存取的 Entity framework 的核心功能。 此功能包括查詢、 插入、 更新和刪除資料，藉由使用強型別物件的能力。 在上述程式碼`ProductContext`類別加入至新加入的 Entity Framework 存取`Order`和`OrderDetail`類別。

## <a name="adding-checkout-access"></a>新增簽出的存取權

Wingtip Toys 範例應用程式可讓匿名使用者檢閱，並將產品加入購物車。 不過，當匿名使用者選擇購買它們加入至購物車的產品時，他們必須登入至站台。 一旦登入，就可以存取受限制的網頁之 Web 應用程式的處理簽出和購買程序。 這些限制的頁面包含在*簽出*的應用程式資料夾。

### <a name="add-a-checkout-folder-and-pages"></a>簽出，將資料夾加入頁面

您即將建立*簽出*資料夾和它的客戶會看到在結帳程序中的頁面。 稍後在本教學課程，您將會更新這些頁面。

1. 以滑鼠右鍵按一下專案名稱 (**Wingtip Toys**) 中**方案總管 中**選取**加入新的資料夾**。 

    ![簽出和 PayPal-新增資料夾與付款](checkout-and-payment-with-paypal/_static/image1.png)
2. 將新的資料夾命名*簽出*。
3. 以滑鼠右鍵按一下*簽出*資料夾，然後選取**新增**-&gt;**新項目**。 

    ![簽出和 PayPal-新項目與付款](checkout-and-payment-with-paypal/_static/image2.png)
4. 隨即顯示 [ 新增項目] 對話方塊。
5. 選取**Visual C#**  - &gt; **Web**左側的 [範本] 群組。 然後從中間窗格中，選取**使用主版頁面的 Web Form**並將其命名*CheckoutStart.aspx*。 

    ![簽出 」 和 「 付款 PayPal-與加入新項目 對話方塊](checkout-and-payment-with-paypal/_static/image3.png)
6. 如往常一般，選取*Site.Master*主版頁面檔案。
7. 加入下列的其他頁面，以*簽出*資料夾使用上述相同的步驟：   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>加入 Web.config 檔案

透過新增*Web.config*檔案*簽出*資料夾中，您將能夠存取限於資料夾中包含的所有頁面。

1. 以滑鼠右鍵按一下*簽出*資料夾，然後選取**新增** - &gt; **新項目**。  
   隨即顯示 [ 新增項目] 對話方塊。
2. 選取**Visual C#**  - &gt; **Web**左側的 [範本] 群組。 然後從中間窗格中，選取**Web 組態檔**，接受預設名稱*Web.config*，然後選取**新增**。
3. 取代現有的 XML 內容中*Web.config*以下列檔案：  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. 儲存*Web.config*檔案。

*Web.config*檔案會指定所有未知的使用者的 Web 應用程式必須拒絕存取的頁面中包含*簽出*資料夾。 不過，如果使用者已註冊的帳戶和登入，它們會是已知的使用者，都可存取的網頁*簽出*資料夾。

請務必注意，ASP.NET 組態如下所示的階層，其中每個*Web.config*檔案適用於組態設定，在其所在的資料夾和所有其下的子目錄。

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>專案中啟用 SSL

 安全通訊端層 (SSL) 是定義為允許 Web 伺服器和 Web 用戶端透過加密使用更安全地通訊的通訊協定。 不使用 SSL，用戶端與伺服器之間傳送資料時，封包探測實體存取網路的任何人開啟。 此外，許多常見的驗證配置並不安全透過一般 HTTP。 特別是，基本驗證和表單驗證來傳送未加密的認證。 若要安全的這些驗證配置必須使用 SSL。 

1. 在**方案總管] 中**，按一下 [ **WingtipToys**專案，然後按下**F4**顯示**屬性**視窗。
2. 變更**啟用 SSL**至`true`。
3. 複製**SSL URL**因此您可以在稍後使用。   
 將 SSL URL`https://localhost:44300/`除非您先前建立 SSL 的網站 （如下所示）。   
    ![專案屬性](checkout-and-payment-with-paypal/_static/image4.png)
4. 在**方案總管 中**，以滑鼠右鍵按一下**WingtipToys**專案，然後按一下**屬性**。
5. 在左側的索引標籤上，按一下**Web**。
6. 變更**專案 Url**使用**SSL URL**您稍早儲存。   
    ![專案 Web 屬性](checkout-and-payment-with-paypal/_static/image5.png)
7. 按下儲存網頁**CTRL + S**。
8. 按 **Ctrl+F5** 執行應用程式。 Visual Studio 會顯示可讓您避免 SSL 警告的選項。
9. 按一下**是**信任 IIS Express SSL 憑證，並繼續。   
    ![IIS Express SSL 憑證的詳細資訊](checkout-and-payment-with-paypal/_static/image6.png)  
 會顯示安全性警告。
10. 按一下**是**將憑證安裝到您的 localhost。   
    ![安全性警告對話方塊](checkout-and-payment-with-paypal/_static/image7.png)  
 瀏覽器視窗隨即出現。

您現在可以輕鬆地測試在本機上使用 SSL 的 Web 應用程式。

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>新增的 OAuth 2.0 提供者

ASP.NET Web Form 提供增強的成員資格和驗證的選項。 這些增強功能包括 OAuth。 OAuth 是開放的通訊協定，允許使用簡單和標準的方法，從 web、 行動及桌面應用程式安全的授權。 ASP.NET Web Form 範本使用 OAuth 來作為驗證提供者會公開 Facebook、 Twitter、 Google 和 Microsoft。 雖然本教學課程會使用僅 Google 做為驗證提供者，您可以輕鬆地修改程式碼以使用任何提供者。 實作其他提供者的步驟會在本教學課程，您會看到的步驟非常類似。

除了驗證，本教學課程也會使用角色來實作授權。 只有在您將加入的使用者`canEdit`角色將無法變更資料 （建立、 編輯或刪除連絡人）。

> [!NOTE] 
> 
> Windows Live 應用程式只接受即時工作網站 URL，因此您無法使用本機網站 URL，測試登入。


下列步驟可讓您新增 Google 驗證提供者。

1. 開啟*應用程式\_Start\Startup.Auth.cs*檔案。
2. 移除註解字元從`app.UseGoogleAuthentication()`方法讓，方法會顯示為如下所示： 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. 瀏覽至[Google 開發人員主控台](https://console.developers.google.com/)。 您也必須使用您的 Google 開發人員的電子郵件帳戶 (gmail.com) 登入。 如果您沒有 Google 帳戶，請選取**建立帳戶**連結。   
   接下來，您會看到**Google 開發人員主控台**。   
    ![Google 開發人員主控台](checkout-and-payment-with-paypal/_static/image8.png)
4. 按一下**建立專案**按鈕並輸入專案名稱和識別碼 （您可以使用預設值）。 然後，按一下 **協議 核取方塊**和**建立** 按鈕。  

    ![Google-新的專案](checkout-and-payment-with-paypal/_static/image9.png)

   幾分鐘後將會建立新的專案，您的瀏覽器會顯示新的 [專案] 頁面。
5. 在左側的索引標籤上，按一下**Api &amp; auth**，然後按一下 **認證**。
6. 按一下**建立新的用戶端識別碼**下**OAuth**。   
   **建立用戶端識別碼**對話方塊隨即出現。   
    ![Google-建立用戶端識別碼](checkout-and-payment-with-paypal/_static/image10.png)
7. 在**建立用戶端識別碼** 對話方塊中，保留預設值**Web 應用程式**應用程式類型。
8. 設定**授權 JavaScript Origins**您稍早在本教學課程中使用的 SSL url (`https://localhost:44300/`除非您已建立其他 SSL 專案)。   
   此 URL 是您的應用程式的原點。 此範例中，您只能將輸入 localhost 測試 URL。 不過，您可以輸入 localhost 和生產環境的多個 Url。
9. 設定**授權重新導向 URI**如下： 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   此值為該 ASP.NET OAuth 的 URI 來與 google OAuth 伺服器通訊的使用者。 請記住您在上面使用的 SSL URL (`https://localhost:44300/`除非您已建立其他 SSL 專案)。
10. 按一下**建立用戶端識別碼** 按鈕。
11. Google 開發人員主控台的左窗格中，按一下 **同意畫面**功能表項目，然後設定您的電子郵件地址和產品名稱。 當您完成表單時，按一下 **儲存**。
12. 按一下**Api**功能表項目，向下的捲動並按一下**關閉**旁邊**Google + API**。   
    接受此選項會啟用 Google + API。
13. 您也必須更新**Microsoft.Owin** 3.0.0 版本的 NuGet 封裝。   
    從**工具**功能表上，選取**NuGet 套件管理員**，然後選取 **管理方案的 NuGet 套件**。  
    從**管理 NuGet 封裝** 視窗中，尋找及更新**Microsoft.Owin** 3.0.0 版本的封裝。
14. 在 Visual Studio 中，更新`UseGoogleAuthentication`方法*Startup.Auth.cs*複製及貼上頁面**用戶端識別碼**和**用戶端密碼**至方法。 **用戶端識別碼**和**用戶端密碼**值如下所示的範例，將無法運作。 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. 按**CTRL + F5**建置並執行應用程式。 按一下**登入**連結。
16. 在下**使用另一個服務登入**，按一下  **Google**。  
    ![登入](checkout-and-payment-with-paypal/_static/image11.png)
17. 如果您需要輸入您的認證，您將會重新導向至 google 站台，您將在其中輸入您的認證。  
    ![Google-登入](checkout-and-payment-with-paypal/_static/image12.png)
18. 輸入您的認證之後，系統會提示您授與您剛才建立的 web 應用程式的權限。  
    ![專案預設服務帳戶](checkout-and-payment-with-paypal/_static/image13.png)
19. 按一下**接受**。 您將會被重新導向至**註冊**頁面**WingtipToys**應用程式，您可以在其中註冊您的 Google 帳戶。  
    ![向您的 Google 帳戶](checkout-and-payment-with-paypal/_static/image14.png)
20. 您可以選擇變更為您的 Gmail 帳戶所使用的本機電子郵件註冊名稱，但您通常想要保留的預設電子郵件別名 （也就是一個用來驗證）。 按一下**登入**如上所示。

### <a name="modifying-login-functionality"></a>修改登入功能

如先前所述在此教學課程中，大部分的使用者註冊功能已包含在 ASP.NET Web Form 範本預設。 現在您將修改預設*Login.aspx*和*Register.aspx*頁面來呼叫`MigrateCart`方法。 `MigrateCart`方法關聯匿名的購物車中的新登入的使用者。 藉由建立關聯的使用者和購物車，Wingtip Toys 範例應用程式將能夠維護購物車之間瀏覽的使用者。

1. 在**方案總管 中**、 尋找和開啟*帳戶*資料夾。
2. 修改程式碼後置頁面，名為*Login.aspx.cs*包含以黃色反白顯示的程式碼，使其顯示，如下所示：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. 儲存*Login.aspx.cs*檔案。

現在，您可以忽略沒有定義的警告`MigrateCart`方法。 您將加入它稍後在本教學課程。

*Login.aspx.cs*支援登入方法的程式碼後置檔案。 藉由檢查 Login.aspx 頁面，您會看到此頁面包含的 「 登入 」 按鈕，當按一下 觸發程序`LogIn`上程式碼後置處理常式。

當`Login`方法*Login.aspx.cs*會呼叫名為購物籃中的新執行個體`usersShoppingCart`建立。 擷取和設定為購物車 (GUID) 識別碼`cartId`變數。 然後，`MigrateCart`呼叫方法時，傳遞兩者`cartId`和登入的使用者對這個方法的名稱。 購物車移轉時，用來識別匿名的購物車的 GUID 會取代使用者名稱。

除了修改*Login.aspx.cs*程式碼後置檔案，以移轉購物車，當使用者登入，您也必須修改*Register.aspx.cs 程式碼後置檔案*移轉購物車當使用者建立新的帳戶，登入。

1. 在*帳戶*資料夾中，開啟的程式碼後置檔案命名為*Register.aspx.cs*。
2. 修改包含的程式碼以黃色，程式碼後置檔案，使其顯示，如下所示：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. 儲存*Register.aspx.cs*檔案。 同樣地，忽略此警告關於`MigrateCart`方法。

請注意程式碼中所使用`CreateUser_Click`事件處理常式是非常類似的程式碼中使用`LogIn`方法。 當使用者註冊或登入站台，呼叫`MigrateCart`方法不會進行。

## <a name="migrating-the-shopping-cart"></a>移轉購物車

有更新的登入和註冊程序之後，您可以加入購物車使用移轉程式碼`MigrateCart`方法。

1. 在**方案總管 中**，尋找*邏輯*資料夾，然後開啟*ShoppingCartActions.cs*類別檔案。
2. 加入程式碼中的現有程式碼以黃色反白顯示*ShoppingCartActions.cs*檔案，以便中的程式碼*ShoppingCartActions.cs*檔案會出現，如下所示：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

`MigrateCart`方法用來尋找使用者的購物車的現有 cartId。 接下來，此程式碼所有的購物車項目間執行迴圈，並取代`CartId`屬性 (依照`CartItem`結構描述) 的登入的使用者名稱。

### <a name="updating-the-database-connection"></a>更新資料庫連接

如果您要遵照這個教學課程使用**預先建置**Wingtip Toys 範例應用程式，您必須重新建立的預設成員資格資料庫。 藉由修改預設的連接字串，成員資格資料庫將會建立的下次執行應用程式。

1. 開啟*Web.config*專案根目錄的檔案。
2. 更新預設的連接字串，使其顯示，如下所示：   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>將 PayPal 整合

PayPal 是網頁型計費平台可接受的線上商家付款。 本教學課程接下來會說明如何將 PayPal Express 簽出功能整合到您的應用程式。 Express 簽出可讓您的客戶使用 PayPal 支付它們新增至購物車的項目。

### <a name="create-paylpal-test-accounts"></a>建立 PaylPal 測試帳戶

若要使用測試環境的 PayPal，您必須建立並確認測試開發人員帳戶。 若要建立買方測試帳戶和賣方測試帳戶，您將使用開發人員測試帳戶。 開發人員測試帳戶認證也會讓 Wingtip Toys 範例應用程式存取 PayPal 測試環境。

1. 在瀏覽器中，瀏覽至 PayPal 開發人員網站測試：   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. 如果您沒有 PayPal 開發人員帳戶，建立新帳戶，即可**註冊**並遵循的步驟登入。 如果您有現有的 PayPal 開發人員帳戶，登入即可**登入**。 您必須測試 Wingtip Toys 範例應用程式，稍後在本教學課程 PayPal 開發人員帳戶。
3. 如果您只要已註冊 PayPal 開發人員帳戶，您可能需要驗證您的 PayPal PayPal 開發人員帳戶。 您可以依照 PayPal 傳送電子郵件帳戶的步驟來確認您的帳戶。 一旦確認 PayPal 開發人員帳戶，登入 PayPal 開發人員網站測試。
4. 您登入 PayPal 開發人員網站與您要建立 PayPal 買方測試帳戶，如果您已經沒有 PayPal 開發人員帳戶之後有一個。 若要建立買方測試帳戶、 PayPal 網站按一下**應用程式**索引標籤，然後按一下 **沙箱帳戶**。   
 **沙箱測試帳戶**頁面隨即顯示。   

    > [!NOTE] 
    > 
    > PayPal 開發人員網站已提供商家測試帳戶。

    ![簽出和 PayPal-沙箱測試帳戶與付款](checkout-and-payment-with-paypal/_static/image15.png)
5. 在沙箱測試的 帳戶 頁面中，按一下 **建立帳戶**。
6. 在**建立測試帳戶**頁面上選擇買方測試帳戶電子郵件和您選擇的密碼。   

    > [!NOTE] 
    > 
    > 您必須購買者的電子郵件地址和密碼才能測試 Wingtip Toys 範例應用程式，在本教學課程結尾處。

    ![簽出和 PayPal-沙箱測試帳戶與付款](checkout-and-payment-with-paypal/_static/image16.png)
7. 按一下 [建立買方測試帳戶**建立帳戶**] 按鈕。  
 **沙箱測試帳戶**頁面隨即顯示。 

    ![簽出和 PayPal-PaylPal 帳戶與付款](checkout-and-payment-with-paypal/_static/image17.png)
8. 在**沙箱測試帳戶**頁面上，按一下**促進**電子郵件帳戶。  
    **設定檔**和**通知**選項會出現。
9. 選取**設定檔**選項，然後按一下  **API 認證**檢視 API 商家測試帳戶的認證。
10. 將測試 API 認證複製到 [記事本]。

您必須顯示傳統測試 API 認證 （使用者名稱、 密碼和簽章） 進行 API 呼叫，Wingtip Toys 範例應用程式從測試環境的 PayPal。 您將在下一個步驟中加入認證。

### <a name="add-paypal-class-and-api-credentials"></a>加入 PayPal 類別和應用程式開發介面的認證

您會將大部分的 PayPal 程式碼為單一類別。 這個類別包含用來與 PayPal 通訊的方法。 此外，您將這個類別加入您 PayPal 的認證。

1. 在 Visual Studio 內 Wingtip Toys 範例應用程式，以滑鼠右鍵按一下**邏輯**資料夾，然後選取**新增** - &gt; **新項目**。   
   隨即顯示 [ 新增項目] 對話方塊。
2. 在下**Visual C#** 從**已安裝**左邊的窗格，選取**程式碼**。
3. 從中間窗格中，選取**類別**。 這個新類別命名**PayPalFunctions.cs**。
4. 按一下 [加入] 。  
   在編輯器中，會顯示新的類別檔案。
5. 下列程式碼取代預設程式碼：  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. 新增您稍早在本教學課程中，讓您可以函式呼叫 PayPal 測試環境來顯示商店 API 認證 （使用者名稱、 密碼和簽章）。  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> 在此範例應用程式只會將認證加入至 C# 檔案 (.cs)。 不過，在實作解決方案中，您應該考慮加密您在組態檔中的認證。


NVPAPICaller 類別包含大部分的 PayPal 功能。 類別中的程式碼可進行購買 PayPal 測試環境的測試所需的方法。 下列三個 PayPal 函式用來進行購買：

- `SetExpressCheckout` 函式
- `GetExpressCheckoutDetails` 函式
- `DoExpressCheckoutPayment` 函式

`ShortcutExpressCheckout`方法會測試採購資訊與產品詳細資料收集從購物車和呼叫`SetExpressCheckout`PayPal 函式。 `GetCheckoutDetails`方法確認採購詳細資料，並呼叫`GetExpressCheckoutDetails`之前測試購買 PayPal 函式。 `DoCheckoutPayment`方法呼叫來完成從測試環境的測試購買`DoExpressCheckoutPayment`PayPal 函式。 其餘的程式碼支援的 PayPal 方法和處理程序，例如字串的編碼方式、 解碼的字串、 處理陣列，以及決定認證。

> [!NOTE] 
> 
> PayPal 可讓您包含選擇性的採購單詳細資料，根據[PayPal 的 API 規格](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout)。 您可以藉由擴充 Wingtip Toys 範例應用程式中的程式碼，包含當地語系化的詳細資料、 產品描述、 稅金、 客戶服務編號，以及許多其他選擇性欄位。


請注意，傳回和 [取消] Url 中指定的**ShortcutExpressCheckout**方法使用的通訊埠編號。

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Visual Web Developer 中執行時使用 SSL 的 web 專案，連接埠 44300 常用的 web 伺服器。 如上所示，連接埠號碼是 44300。 當您執行應用程式時，您可以查看其他通訊埠編號。 連接埠號碼需要正確設定程式碼中，讓您可以成功執行 Wingtip Toys 範例應用程式在此教學課程結尾處。 本教學課程的下一節說明如何擷取本機主機通訊埠編號和 PayPal 類別的更新。

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>更新 PayPal 類別中的 LocalHost 的連接埠號碼

Wingtip Toys 範例應用程式瀏覽 PayPal 測試網站，然後返回 Wingtip Toys 範例應用程式的本機執行個體所購買的產品。 若要有 PayPal 傳回至正確的 URL，您必須指定連接埠號碼，在本機執行的範例應用程式，在上面提到的 PayPal 程式碼。

1. 以滑鼠右鍵按一下專案名稱 (**WingtipToys**) 中**方案總管 中**選取**屬性**。
2. 在左側的資料行中，選取**Web**  索引標籤。
3. 擷取連接埠號碼從**專案 Url**方塊。
4. 視需要更新`returnURL`和`cancelURL`PayPal 類別中 (`NVPAPICaller`) 中*PayPalFunctions.cs*檔案，以使用 web 應用程式的通訊埠編號：   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

現在您加入的程式碼會比對本機 Web 應用程式預期的連接埠。 PayPal 可以回到本機電腦上的正確的 URL。

### <a name="add-the-paypal-checkout-button"></a>新增 PayPal 簽出 按鈕

現在，主要的 PayPal 函式已加入到範例應用程式，您可以開始加入標記和程式碼需要呼叫這些函式。 首先，您必須加入使用者會看到在購物車網頁的 [簽出] 按鈕。

1. 開啟*ShoppingCart.aspx*檔案。
2. 捲動到檔案底部，然後尋找`<!--Checkout Placeholder -->`註解。
3. 取代註解與`ImageButton`控制，讓標記取代，如下所示：  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. 在*ShoppingCart.aspx.cs*檔案，之後`UpdateBtn_Click`檔案，結尾附近的事件處理常式加入`CheckOutBtn_Click`事件處理常式：  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. 此外，在*ShoppingCart.aspx.cs*檔案中，將參考加入`CheckoutBtn`，如此一來，新的映像按鈕參考，如下所示：  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. 儲存您的變更同時*ShoppingCart.aspx*檔案和*ShoppingCart.aspx.cs*檔案。
7. 從功能表中，選取**偵錯**-&gt;**建置 WingtipToys**。  
   專案將會重建與新加入**ImageButton**控制項。

### <a name="send-purchase-details-to-paypal"></a>PayPal 來傳送訂單詳細資料

當使用者按一下**簽出**購物車網頁上的按鈕 (*ShoppingCart.aspx*)，它們將會開始採購程序。 下列程式碼會呼叫購買產品的第一個 PayPal 函式。

1. 從*簽出*資料夾中，開啟的程式碼後置檔案命名為*CheckoutStart.aspx.cs*。   
   請務必開啟程式碼後置檔案。
2. 將現有的程式碼取代為下列程式碼：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

當應用程式的使用者按一下**簽出**購物車網頁瀏覽器 按鈕會瀏覽至*CheckoutStart.aspx*頁面。 當*CheckoutStart.aspx*頁面載入，`ShortcutExpressCheckout`方法呼叫。 此時，使用者會轉移至 PayPal 測試的網站。 PayPal 在網站上，使用者輸入他們的 PayPal 認證，檢閱採購詳細資料、 接受 PayPal 合約並 Wingtip Toys 範例應用程式會傳回其中`ShortcutExpressCheckout`方法完成。 當`ShortcutExpressCheckout`方法完成時，它會將使用者重新導向*CheckoutReview.aspx*頁面中指定`ShortcutExpressCheckout`方法。 這可讓使用者檢閱來自 Wingtip Toys 範例應用程式中的訂單詳細資料。

### <a name="review-order-details"></a>檢閱訂單詳細資料

傳回從 PayPal 之後, *CheckoutReview.aspx* Wingtip Toys 範例應用程式的頁面會顯示訂單詳細資料。 此頁面可讓使用者檢閱再購買產品的訂單詳細資料。 *CheckoutReview.aspx*必須建立頁面，如下所示：

1. 在*簽出*資料夾中，開啟名為頁面*CheckoutReview.aspx*。
2. 以下列內容取代現有的標記：   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. 開啟程式碼後置頁面，名為*CheckoutReview.aspx.cs*和現有的程式碼替換成下列：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

**DetailsView**控制項用來顯示已從 PayPal 傳回的訂單詳細資料。 此外，上述程式碼將訂單詳細資料儲存至 Wingtip Toys 資料庫做為`OrderDetail`物件。 當使用者按一下**完成訂單**按鈕時，會被重新導向至*CheckoutComplete.aspx*頁面。

> [!NOTE] 
> 
> **秘訣**
> 
> 在標記中*CheckoutReview.aspx*頁面上，請注意，`<ItemStyle>`標記用來變更中的項目樣式**DetailsView**靠近頁面底部的控制項。 藉由檢視中的頁面**設計 檢視**(藉由選取**設計**在 Visual Studio 左下角)，然後選取**DetailsView**控制項，然後選取**智慧標籤**(在最上方的箭號圖示右邊的控制項)，您將能夠看到**DetailsView 工作**。
> 
> ![簽出 」 和 「 付款 PayPal-與編輯欄位](checkout-and-payment-with-paypal/_static/image18.png)
> 
> 藉由選取**編輯欄位**、**欄位**對話方塊隨即出現。 在此對話方塊中您可以輕鬆控制的視覺屬性，例如**ItemStyle**的**DetailsView**控制項。
> 
> ![簽出 」 和 「 付款與 PayPal-欄位 對話方塊](checkout-and-payment-with-paypal/_static/image19.png)


### <a name="complete-purchase"></a>完成購買程序

*CheckoutComplete.aspx*網頁可從 PayPal 進行購買。 如上面所述，使用者必須按一下**完成訂單**按鈕會瀏覽應用程式之前*CheckoutComplete.aspx*頁面。

1. 在*簽出*資料夾中，開啟名為頁面*CheckoutComplete.aspx*。
2. 以下列內容取代現有的標記：   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. 開啟程式碼後置頁面，名為*CheckoutComplete.aspx.cs*和現有的程式碼替換成下列：   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

當*CheckoutComplete.aspx*網頁載入時，`DoCheckoutPayment`方法呼叫。 如先前所述，`DoCheckoutPayment`方法完成購買程序從 PayPal 測試環境。 PayPal 完成訂單的訂單後*CheckoutComplete.aspx*頁面會顯示付款交易`ID`來購買。

### <a name="handle-cancel-purchase"></a>處理取消訂單

如果使用者決定取消購買時，它們會導向至*CheckoutCancel.aspx*頁面位置就會看到它們的順序已經取消。

1. 開啟此頁面*CheckoutCancel.aspx*中*簽出*資料夾。
2. 以下列內容取代現有的標記：   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>處理購買錯誤

採購程序期間發生的錯誤處理*CheckoutError.aspx*頁面。 程式碼後置的*CheckoutStart.aspx*  頁面上， *CheckoutReview.aspx*  頁面上，而*CheckoutComplete.aspx*頁面將每個重新導向至*CheckoutError.aspx*頁面上，如果發生錯誤。

1. 開啟此頁面*CheckoutError.aspx*中*簽出*資料夾。
2. 以下列內容取代現有的標記：   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

*CheckoutError.aspx*頁面會顯示錯誤詳細資料在結帳程序期間發生錯誤時。

## <a name="running-the-application"></a>執行應用程式

執行應用程式以了解如何購買的產品。 請注意，您將執行中的 PayPal 測試環境。 正在交換沒有實際的成本。

1. 請確定所有 Visual Studio 中儲存您的檔案。
2. 開啟網頁瀏覽器並瀏覽至[ https://developer.paypal.com ](https://developer.paypal.com/)。
3. 以您稍早在本教學課程中建立您 PayPal 開發人員帳戶登入。  
   PayPal 的開發人員沙箱，您需要在登入[ https://developer.paypal.com ](https://developer.paypal.com/)測試 express 簽出。 這只適用於 PayPal 的沙箱測試，不 PayPal 的實際環境。
4. 在 Visual Studio 中，按**F5**執行 Wingtip Toys 範例應用程式。  
   重建資料庫之後，瀏覽器會開啟並顯示*Default.aspx*頁面。
5. 將三個不同的產品加入購物車選取產品類別目錄，例如 「 汽車 」，然後按一下**加入購物車**旁邊每項產品。  
   購物車就會顯示您所選取的產品。
6. 按一下**PayPal**簽出 按鈕。 

    ![簽出 」 和 「 付款 PayPal-與購物車](checkout-and-payment-with-paypal/_static/image20.png)

   簽出，將會需要您具備 Wingtip Toys 範例應用程式的使用者帳戶。
7. 按一下**Google**上頁面的右邊，以現有的 gmail.com 電子郵件帳戶登入連結。  
   如果您沒有 gmail.com 帳戶，您可以建立另一個用於測試用途[www.gmail.com](https://www.gmail.com/)。您也可以使用標準的本機帳戶，依序按一下 「 註冊 」。 

    ![簽出 」 和 「 付款 PayPal-與登入](checkout-and-payment-with-paypal/_static/image21.png)
8. 使用您 gmail 帳戶和密碼登入。 

    ![簽出和 PayPal-Gmail 登入與付款](checkout-and-payment-with-paypal/_static/image22.png)
9. 按一下**登入**gmail 帳戶向您 Wingtip Toys 範例應用程式的使用者名稱 按鈕。 

    ![簽出 」 和 「 付款與 PayPal-註冊帳戶](checkout-and-payment-with-paypal/_static/image23.png)
10. PayPal 測試站台上新增您**買方**電子郵件地址和您稍早在本教學課程中建立的密碼，然後按一下 [**登入**] 按鈕。 

    ![簽出和 PayPal-PayPal 登入與付款](checkout-and-payment-with-paypal/_static/image24.png)
11. 同意 PayPal 原則並按一下**同意後繼續** 按鈕。  
    請注意，此頁面才會顯示您使用此 PayPal 帳戶第一次。 再次請注意，這是測試帳戶，沒有實際 money 交換。 

    ![簽出和 PayPal-PayPal 原則與付款](checkout-and-payment-with-paypal/_static/image25.png)
12. 檢閱在測試環境 [檢閱] 頁面，然後按一下 PayPal 的訂單資訊**繼續**。 

    ![簽出和 PayPal-檢閱資訊與付款](checkout-and-payment-with-paypal/_static/image26.png)
13. 在*CheckoutReview.aspx*頁面上，確認訂單金額，檢視產生的送貨地址。 然後，按一下 [**完成訂單**] 按鈕。 

    ![簽出和 PayPal-順序檢閱與付款](checkout-and-payment-with-paypal/_static/image27.png)
14. **CheckoutComplete.aspx**頁面會顯示付款交易識別碼。 

    ![簽出和 PayPal-完成簽出與付款](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>檢閱資料庫

藉由檢閱 Wingtip Toys 範例應用程式資料庫中更新的資料執行應用程式之後，您可以看到應用程式成功地錄製產品的訂單。

您可以檢查所包含的資料*Wingtiptoys.mdf*資料庫檔案使用**資料庫總管**視窗 (**伺服器總管**Visual Studio 中的視窗) 您可以如同稍早在本教學課程的系列。

1. 如果它仍為開啟，請關閉瀏覽器視窗。
2. 在 Visual Studio 中，選取**顯示所有檔案**上方的圖示**方案總管 中**可讓您展開**應用程式\_資料**資料夾。
3. 展開**應用程式\_資料**資料夾。  
 您可能需要選取**顯示所有檔案**資料夾圖示。
4. 以滑鼠右鍵按一下*Wingtiptoys.mdf*資料庫檔案，然後選取**開啟**。  
    **伺服器總管**隨即出現。
5. 展開**資料表**資料夾。
6. 以滑鼠右鍵按一下**訂單**資料表，然後選取**顯示資料表資料**。  
 **訂單**資料表就會顯示。
7. 檢閱**PaymentTransactionID**資料行，以確認成功的交易。 

    ![簽出和 PayPal-檢閱資料庫與付款](checkout-and-payment-with-paypal/_static/image29.png)
8. 關閉**訂單**資料表視窗中。
9. 在 [伺服器總管] 中，以滑鼠右鍵按一下**OrderDetails**資料表，然後選取**顯示資料表資料**。
10. 檢閱`OrderId`和`Username`值**OrderDetails**資料表。 請注意，這些值相符`OrderId`和`Username`值包含在**訂單**資料表。
11. 關閉**OrderDetails**資料表視窗中。
12. 以滑鼠右鍵按一下 Wingtip Toys 資料庫檔案 (*Wingtiptoys.mdf*) 並選取**關閉連接**。
13. 如果您看不**方案總管 中**視窗中，按一下**方案總管 中**底部**伺服器總管**視窗以顯示**方案總管**一次。

## <a name="summary"></a>總結

在本教學課程中加入訂單及訂單詳細資料的結構描述追蹤購買的產品。 您也會整合至 Wingtip Toys 範例應用程式的 PayPal 功能。

## <a name="additional-resources"></a>其他資源

[ASP.NET 組態概觀](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[將具有成員資格、 OAuth、 和 SQL Database 的安全的 ASP.NET Web Form 應用程式部署至 Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure-免費試用版](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>免責聲明

本教學課程包含範例程式碼。 這類的範例程式碼係依 「 現況 」 不含任何種類的擔保。 因此，Microsoft 不保證精確度、 完整性或範例程式碼的品質。 您同意自行承擔使用範例程式碼。 在任何情況下 Microsoft 會負責您以任何方式的任何程式碼範例內容，包括但不是限於任何錯誤或不作為概不在任何程式碼範例、 內容或任何遺失或損毀所造成的結果的任何程式碼範例使用任何種類。 被授權人有收到通知，並被授權人有同意出面代為辯護，請儲存並保留 Microsoft 無害遭受任何和所有遺失、 遺失、 傷害或損任何的毀種類包括，但不限於由 occasioned 或資料，以在張貼時，所引起的宣告傳輸、 使用或依賴包括但不是限於表示其中的檢視。

> [!div class="step-by-step"]
> [上一頁](shopping-cart.md)
> [下一頁](membership-and-administration.md)
