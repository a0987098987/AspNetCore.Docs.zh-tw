---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: "自訂全站台的行為的 ASP.NET Web Pages (Razor) 站台 |Microsoft 文件"
author: tfitzmac
description: "本章節說明如何讓設定整個網站或整個資料夾，而不是一頁。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: b1caa26a23517bd976addfefac89375ae965eb91
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a>ASP.NET Web Pages (Razor) 網站的自訂全站台的行為
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明如何在 ASP.NET Web Pages (Razor) 的網站中進行站台端設定頁面。
> 
> 您將學習：
> 
> - 如何可讓您執行程式碼值組 （全域值或協助程式設定） 網站中的所有頁面。
> - 如何執行程式碼，可讓您設定的資料夾中的所有頁面的值。
> - 如何在程式碼之前和之後執行網頁載入。
> - 如何將錯誤傳送到中央的錯誤頁面。
> - 如何將驗證新增至資料夾中的所有頁面。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3
> - ASP.NET Web Helpers Library （NuGet 套件）
>   
> 
> 本教學課程也適用於 ASP.NET Web Pages 3 和 Visual Studio 2013 （或 Visual Studio Express 2013 for Web），但您無法使用 ASP.NET Web Helpers Library。


<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a>加入 ASP.NET 網頁的網站啟動程式碼

對於大部分您撰寫 ASP.NET Web Pages 中的程式碼，個別的頁面可以包含該頁面所需的所有程式碼。 例如，如果頁面傳送電子郵件訊息時，很可能該作業的程式碼全數放在單一頁面中。 這可能包括設定初始化用於傳送電子郵件的程式碼 (也就是 SMTP 伺服器) 和傳送電子郵件訊息。

不過，在某些情況下，您可能想要在網站上的任何頁面執行前，先執行一些程式碼。 這可用於設定可用於站台的任何位置的值 (稱為*全域值*。)例如，一些協助程式會要求您提供如電子郵件的設定或帳戶金鑰的值。 它可能很方便的全域值中保留這些設定項目。

您可以藉由建立名為的頁面 *\_AppStart.cshtml*根目錄中的站台。 如果此頁面存在，它會執行第一次要求在站台中的任何頁面。 因此，它是執行程式碼以設定全域值的好地方。 (因為 *\_AppStart.cshtml*具有底線的前置詞，ASP.NET 將不會將頁面傳送至瀏覽器即使使用者直接要求。)

下圖顯示如何 *\_AppStart.cshtml*頁面上運作。 當要求進入頁面，以及這是否為任何第一個要求頁面在網站中，ASP.NET 會先檢查是否 *\_AppStart.cshtml*頁面存在。 如果是這樣，任何程式碼中 *\_AppStart.cshtml*頁面上執行，並執行要求的頁面。

![[影像]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a>為您的網站設定的全域值

1. 在 WebMatrix 網站的根資料夾中，建立名為 *\_AppStart.cshtml*。 檔案必須是網站的根目錄中。
2. 以下列內容取代現有的內容： 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    此程式碼將值儲存在`AppState`字典，會自動提供給網站中的所有頁面。 請注意，  *\_AppStart.cshtml*檔案中沒有任何標記。 頁面會執行程式碼，然後重新導向至原本要求的頁面。

    > [!NOTE]
    > 當您將程式碼放入 *\_AppStart.cshtml*檔案。 如果程式碼中發生任何錯誤 *\_AppStart.cshtml*檔案，將無法啟動網站。
3. 在根資料夾中，會建立名為的新頁面*AppName.cshtml*。
4. 以下列內容取代預設標記和程式碼： 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    此程式碼會擷取從值`AppState`物件中設定 *\_AppStart.cshtml*頁面。
5. 執行*AppName.cshtml*瀏覽器中的。 (請確定中選取頁面**檔案**才能執行這個工作區。)頁面會顯示全域值。 

    ![[影像]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a>協助程式的設定值

為充分利用 *\_AppStart.cshtml*檔案之設定的協助程式，您的網站中使用，以及具有初始化值。 典型的範例包括電子郵件設定`WebMail`helper 和私人和公用金鑰`ReCaptcha`協助程式。 在這些情況下，您可以設定的值一次 *\_AppStart.cshtml*然後他們是已經的所有頁面中設定您的網站。

此程序會示範如何設定`WebMail`設定全域。 (如需有關使用`WebMail`協助程式，請參閱[新增電子郵件給 ASP.NET Web Pages 站台中](../getting-started/11-adding-email-to-your-web-site.md)。)

1. 中所述，您的網站加入 ASP.NET Web Helpers Library[安裝 ASP.NET Web Pages 站台中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您尚未新增它。
2. 如果您還沒有 *\_AppStart.cshtml*檔案中，網站的根資料夾中建立名為 *\_AppStart.cshtml*。
3. 加入下列`WebMail`設定 *\_AppStart.cshtml*檔案： 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    修改下列電子郵件的程式碼中的相關的設定：

    - 設定`your-SMTP-host`您具有存取權的 SMTP 伺服器的名稱。
    - 設定`your-user-name-here`至您的 SMTP 伺服器帳戶的使用者名稱。
    - 設定`your-account-password`您的 SMTP 伺服器帳戶的密碼。
    - 設定`your-email-address-here`您自己的電子郵件地址。 這是從訊息傳送的電子郵件地址。 (某些電子郵件提供者不要讓您指定不同`From`位址，且將會使用您的使用者名稱做為`From`位址。)

    如需 SMTP 設定的詳細資訊，請參閱[設定電子郵件設定](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings)本文[從 ASP.NET Web Pages (Razor) 站台傳送的電子郵件](https://go.microsoft.com/fwlink/?LinkID=202899)和[問題傳送電子郵件](https://go.microsoft.com/fwlink/?LinkId=253001#email)中[ASP.NET Web Pages (Razor) 疑難排解指南](https://go.microsoft.com/fwlink/?LinkId=253001)。
- 儲存 *\_AppStart.cshtml*檔案並將它關閉。
- 在網站的根資料夾中，建立名為新頁面*TestEmail.cshtml*。
- 以下列內容取代現有的內容： 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
- 執行*TestEmail.cshtml*瀏覽器中的。
- 填寫 傳送電子郵件訊息給自己，然後按一下欄位**傳送**。
- 請檢查您的電子郵件，請確定您已收到訊息。

這個範例的重點是，您通常不會變更的設定 — 喜歡的 SMTP 伺服器和您的電子郵件認證名稱 — 中設定 *\_AppStart.cshtml*檔案。 如此一來您不需要再次設定它們每一頁中您用來傳送電子郵件。 （雖然如果基於某些原因，您需要變更這些設定，您可以設定它們個別網頁中）。在頁面中，您只會設定每次，例如收件者和電子郵件訊息本文通常會變更的值。

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a>執行程式碼之前和之後的資料夾中的檔案

您可以使用，如同 *\_AppStart.cshtml*網站中的執行之前，請撰寫程式碼，您可以撰寫程式碼執行之前 （和之後） 執行的特定資料夾中的任何頁面。 這是適用於等設定的同一個版面配置頁面在資料夾中，所有頁面，或檢查使用者登入之前執行的資料夾中的頁面。

頁面特別資料夾，您可以建立程式碼檔案中 *\_PageStart.cshtml*。 下圖顯示如何 *\_PageStart.cshtml*頁面上運作。 當要求進入網頁時，ASP.NET 先檢查 *\_AppStart.cshtml*頁面上，並執行的。 接著 ASP.NET 會檢查是否有 *\_PageStart.cshtml*頁面，然後如果是的話，會執行。 然後，它會執行要求的頁面。

內部 *\_PageStart.cshtml*  頁面上，您可以在您想要藉由執行要求的頁面處理期間指定的位置`RunPage`方法。 這可讓您要求的網頁執行前執行的程式碼，然後再次之後。 如果您不想加入`RunPage`中的所有程式碼 *\_PageStart.cshtml*執行，然後按一下 要求的頁面會自動執行。

![[影像]](18-customizing-site-wide-behavior/_static/image3.jpg)

ASP.NET 可讓您建立的階層 *\_PageStart.cshtml*檔案。 您可以放入 *\_PageStart.cshtml*根目錄中的站台和任何子資料夾中的檔案。 要求頁面時，  *\_PageStart.cshtml*檔案 （最接近的站台根目錄） 的最高的層級執行，後面接著 *\_PageStart.cshtml*檔案中的下一步子資料夾，以及下層的子資料夾結構，直到要求都會到達所要求的網頁所在的資料夾。 之後所有適用 *\_PageStart.cshtml*檔案已在執行時，會執行要求的頁面。

比方說，您可能會有下列的組合 *\_PageStart.cshtml*檔案和*Default.cshtml*檔案：

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

當您執行*/myfolder/default.cshtml*，您會看到下列訊息：

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a>執行資料夾中的所有頁面的初始化程式碼

為充分利用 *\_PageStart.cshtml*檔案是初始化的單一資料夾中的所有檔案相同的 [配置] 頁面。

1. 在根資料夾中，建立新的資料夾，名為*InitPages*。
2. 在*InitPages*資料夾，您的網站，建立名為 *\_PageStart.cshtml*和預設標記和程式碼替換成下列： 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. 在網站的根目錄，在建立資料夾，名為*共用*。
4. 在*共用*資料夾中，建立名為 *\_Layout1.cshtml*和預設標記和程式碼替換成下列： 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. 在*InitPages*資料夾中，建立名為*Content1.cshtml*和現有的內容取代為下列： 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. 在*InitPages*資料夾中，建立名為另一個檔案*Content2.cshtml* ，並以下列取代預設標記： 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. 執行*Content1.cshtml*瀏覽器中。 

    ![[影像]](18-customizing-site-wide-behavior/_static/image4.jpg)

    當*Content1.cshtml*頁面上執行時，  *\_PageStart.cshtml*檔案集`Layout`，還會設定`PageData["MyBackground"]`色彩。 在*Content1.cshtml*，套用配置和色彩。
8. 顯示*Content2.cshtml*瀏覽器中。 

    版面配置是相同的因為這兩個頁面使用相同的版面配置頁和色彩中初始化 *\_PageStart.cshtml*。

## <a name="using-pagestartcshtml-to-handle-errors"></a>使用\_PageStart.cshtml 來處理錯誤

另一個適用於 *\_PageStart.cshtml*檔案是建立一種方式來處理中任何可能發生程式設計錯誤 （例外狀況） *.cshtml*資料夾中的頁面。 此範例會示範如何執行這項操作。

1. 在根資料夾中，建立名為資料夾*InitCatch*。
2. 在*InitCatch*資料夾，您的網站，建立名為 *\_PageStart.cshtml*和現有的標記和程式碼替換成下列： 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    這個程式碼，在您嘗試執行要求的網頁明確呼叫`RunPage`方法內`try`區塊。 如果在要求中發生程式設計錯誤頁面內的程式碼`catch`封鎖執行。 在此情況下，程式碼會重新導向至網頁 (*Error.cshtml*) 並將 URL 的過程中發生錯誤的檔案名稱傳遞。 （您將建立頁面很快。）
3. 在*InitCatch*資料夾，您的網站，建立名為*Exception.cshtml*和現有的標記和程式碼替換成下列： 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    基於此範例中，您所要做的此頁面刻意建立錯誤嘗試開啟不存在的資料庫檔案。
4. 在根資料夾中，建立名為*Error.cshtml*和現有的標記和程式碼替換成下列： 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    在此頁面中，運算式`@Request["source"]`取得 URL 的值，並顯示它。
5. 在工具列中，按一下 **儲存**。
6. 執行*Exception.cshtml*瀏覽器中。 

    ![[影像]](18-customizing-site-wide-behavior/_static/image5.jpg)

    因為發生錯誤時*Exception.cshtml*、  *\_PageStart.cshtml*頁面重新導向至*Error.cshtml*檔案，其中會顯示訊息。

    如需例外狀況的詳細資訊，請參閱[ASP.NET Web Pages 程式設計使用 Razor 語法的簡介](https://go.microsoft.com/fwlink/?LinkID=251587)。

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-pagestartcshtml-to-restrict-folder-access"></a>使用\_PageStart.cshtml 限制資料夾存取

您也可以使用 *\_PageStart.cshtml*來限制存取權的資料夾中的所有檔案的檔案。

1. 在 WebMatrix 中，建立新的網站使用**網站從範本**選項。
2. 從可用範本中，選取**入門網站**。
3. 在根資料夾中，建立名為資料夾*AuthenticatedContent*。
4. 在*AuthenticatedContent*資料夾中，建立名為 *\_PageStart.cshtml*和現有的標記和程式碼替換成下列： 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    程式碼會啟動，防止被快取資料夾中的所有檔案。 （這是必要的狀況，像是公用電腦，您不想要用來下一位使用者一位使用者的快取的頁面）。接下來，程式碼會判斷是否使用者登入至站台之前，他們可以檢視任何頁面的資料夾中。 如果使用者未登入中，程式碼會重新導向至登入頁面。 登入頁面可以讓使用者返回包含名為的查詢字串值，如果原始要求的頁面`ReturnUrl`。
5. 建立新的頁面中*AuthenticatedContent*資料夾名為*Page.cshtml*。
6. 以下列內容取代預設標記：  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. 執行*Page.cshtml*瀏覽器中。 程式碼將您重新導向至登入頁面。 您必須註冊才能登入。 在您註冊並登入之後，您可以瀏覽至網頁，並檢視其內容。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源

[介紹程式設計使用 Razor 語法的 ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkID=251587)
