---
uid: web-pages/overview/data/working-with-files
title: 使用 ASP.NET Web Pages (Razor) 網站中的檔案 |Microsoft Docs
author: Rick-Anderson
description: 本章說明如何讀取、 寫入、 附加、 刪除和上傳檔案。
ms.author: riande
ms.date: 02/20/2014
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: 8b9176cb88f6e460fe5494167d4a5880456530aa
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021556"
---
<a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a>使用 ASP.NET Web Pages (Razor) 網站中的檔案
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 這篇文章說明如何讀取、 寫入、 附加、 刪除和上傳的 ASP.NET Web Pages (Razor) 網站中的檔案。
> 
> > [!NOTE]
> > 如果您想要將影像上傳及管理它們 （例如，翻轉或調整其大小），請參閱[使用 ASP.NET Web Pages 網站中的映像](https://go.microsoft.com/fwlink/?LinkId=202897)。
> 
> 
> **您將學到什麼：** 
> 
> - 如何建立文字檔，並寫入其中的資料。
> - 如何將資料附加到現有的檔案。
> - 如何讀取檔案，並從它顯示。
> - 如何從網站中刪除檔案。
> - 如何讓使用者上傳一或多個檔案。
> 
> 這些是 ASP.NET 程式設計文章中所引進的功能：
> 
> - `File`物件，可用來管理檔案。
> - `FileUpload`協助程式。
> - `Path`物件，提供方法，讓您管理的路徑和檔案名稱。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> 本教學課程也適用於 WebMatrix 3。


<a id="Creating_a_Text_File"></a>
## <a name="creating-a-text-file-and-writing-data-to-it"></a>建立文字檔案，並對其寫入資料

除了在您的網站使用的資料庫，您可能會使用檔案。 比方說，您可能會將文字檔案做簡單的方式來儲存站台資料。 (有時稱為用來儲存資料的文字檔*一般檔案*。)文字檔可以是不同的格式，例如 *.txt*， *.xml*，或 *.csv* （以逗號分隔值）。

如果您想要將資料儲存在文字檔中，您可以使用`File.WriteAllText`方法，以指定要建立的檔案和要寫入其中的資料。 在此程序中，您將建立包含三個簡單的表單的頁面`input`項目 （名字、 姓氏和電子郵件地址） 和**送出** 按鈕。 當使用者提交表單時，您將會儲存使用者的輸入文字檔案中。

1. 建立新的資料夾，名為*應用程式\_資料*，如果尚不存在。
2. 在您的網站根目錄中，建立新的檔案，名為*UserData.cshtml*。
3. 以下列內容取代現有的內容： 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    HTML 標記會建立具有三個文字方塊的表單。 在程式碼中，您會使用`IsPost`屬性來判斷頁面是否已提交之前開始處理。

    第一項工作是取得使用者輸入，並將它指派給變數。 然後，程式碼會將個別變數的值串連成一個逗號分隔的字串，然後儲存在不同的變數。 請注意，逗號分隔符號字串，包含在引號 （"，"），因為您實際上內嵌逗號到您要建立大型字串。 在您將串連在一起的資料結尾，您將新增`Environment.NewLine`。 這會將換行符號 （新行字元）。 您要建立與所有這種串連是看起來像這樣的字串：

    [!code-css[Main](working-with-files/samples/sample2.css)]

    （含不可見行結尾的符號。）

    接著，您建立的變數 (`dataFile`)，其中包含要將資料儲存在檔案名稱與位置。 設定位置需要一些特殊處理。 在網站中，並不適合在程式碼中參考為絕對路徑，例如*C:\Folder\File.txt* web 伺服器上的檔案。 如果移動網站時，絕對路徑會是錯誤。 此外，裝載站台 （而非您自己的電腦上） 您通常不甚至知道正確的路徑為何您要編寫指令碼時。

    但有時 （如 now、 寫入檔案)，您需要完整的路徑。 解決方法是使用`MapPath`方法的`Server`物件。 這是您的網站傳回的完整路徑。 若要取得的網站根目錄中，您的使用者路徑`~`運算子 (represen 站台的虛擬根) 到`MapPath`。 (您也可以傳遞給它，子資料夾名稱，例如 *~/App\_Data /*，以取得該資料夾的路徑。)然後，您可以串連至任何方法以建立完整的路徑會傳回的其他資訊。 在此範例中，您可以新增的檔案名稱。 (您可以深入了解如何使用中的檔案和資料夾路徑[ASP.NET Web Pages 程式設計使用 Razor 語法簡介](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths)。)

    檔案會儲存在*應用程式\_資料*資料夾。 這個資料夾是用來儲存資料檔案中所述的 ASP.NET 中的特殊資料夾[使用 ASP.NET Web Pages 網站中的資料庫簡介](https://go.microsoft.com/fwlink/?LinkId=195209)。

    `WriteAllText`方法的`File`物件會將資料寫入檔案。 這個方法會採用兩個參數： 要寫入的檔案和要寫入的實際資料的名稱 （使用路徑）。 請注意，第一個參數的名稱具有`@`字元作為前置詞。 這會告訴的 ASP.NET 提供逐字字串常值，且該字元像是"/"不應該以特殊方式解譯。 (如需詳細資訊，請參閱 < [ASP.NET Web 程式設計使用 Razor 語法簡介](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths)。)

    > [!NOTE]
    > 為了讓您儲存檔案中的程式碼*應用程式\_資料*資料夾中，應用程式需要讀取-寫入該資料夾的權限。 在您的開發電腦上不通常發生問題。 不過，當您發行您的網站以裝載提供者的 web 伺服器時，您可能需要明確地設定這些權限。 如果您裝載提供者的伺服器上執行此程式碼，而且發生錯誤，請與主機服務提供者，了解如何設定這些權限。

- 執行網頁瀏覽器中。 

    ![](working-with-files/_static/image1.jpg)
- 在欄位中輸入值，然後按一下**送出**。
- 關閉瀏覽器。
- 返回專案並重新整理檢視。
- 開啟*data.txt*檔案。 您提交表單中的資料位於檔案中。 

    ![[影像]](working-with-files/_static/image2.jpg)
- 關閉*data.txt*檔案。

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a>將資料附加到現有的檔案

在上述範例中，您已使用`WriteAllText`建立一個文字檔，裡面有一個資料片段。 如果您再次呼叫方法，並將它傳遞相同的檔案名稱，會完全覆寫現有的檔案。 不過，您已建立檔案之後您通常想要將新資料加入至檔案結尾。 您可以執行的方法使用`AppendAllText`方法的`File`物件。

1. 在網站中，建立一份*UserData.cshtml*檔案，並命名為複製*UserDataMultiple.cshtml*。
2. 取代程式碼區塊，才能開啟`<!DOCTYPE html>`以下列程式碼區塊的標記： 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    此程式碼中有一項變更，從先前的範例。 而不是使用`WriteAllText`，它會使用`the AppendAllText`方法。 方法很相似，不同之處在於`AppendAllText`將資料加入至檔案結尾。 如同`WriteAllText`，`AppendAllText`不存在時，會建立檔案。
3. 執行網頁瀏覽器中。
4. 輸入欄位的值，然後按一下**送出**。
5. 新增更多資料，並再次提交表單。
6. 返回您的專案，以滑鼠右鍵按一下專案資料夾，然後按一下**重新整理**。
7. 開啟*data.txt*檔案。 它現在會包含您剛才輸入的新資料。 

    ![[影像]](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a>讀取和顯示資料從檔案

即使您不需要將資料寫入到文字檔案，可能有時您必須從一個讀取的資料。 若要這樣做，您可以再次使用`File`物件。 您可以使用`File`物件來個別讀取每一行 （以分行符號分隔），或是讀取個別的項目，不論分隔的方式。

此程序會示範如何讀取並顯示您在上一個範例中建立的資料。

1. 在您的網站根目錄中，建立新的檔案，名為*DisplayData.cshtml*。
2. 以下列內容取代現有的內容： 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    程式碼會藉由讀取到變數，名為在前一個範例中所建立的檔案開始`userData`，使用這個方法呼叫：

    [!code-css[Main](working-with-files/samples/sample5.css)]

    若要這樣做的程式碼位於`if`陳述式。 當您想要讀取的檔案時，它是個不錯的主意，若要使用`File.Exists`方法，先判斷檔案是否可用。 程式碼也會檢查檔案是否是空的。

    頁面的主體包含兩個`foreach`迴圈巢狀方式置於另一個。 外部`foreach`迴圈一次取得一行，從資料檔。 在此情況下，檔案中的分行符號所定義的程式行&#8212;也就是每個資料項目位於它自己的那一行。 外部迴圈會建立新的項目 (`<li>`項目) 內的已排序清單 (`<ol>`項目)。

    內部迴圈會將每個資料列分割成使用逗號做為分隔符號項目 （欄位）。 (根據先前的範例，這表示每一行都包含三個欄位&#8212;名字、 姓氏和電子郵件地址，每個以逗號分隔。)內部迴圈也會建立`<ul>`清單和顯示一個清單項目中的資料線，每個欄位。

    程式碼說明如何使用兩種資料類型、 陣列和`char`資料型別。 陣列是必要的因為`File.ReadAllLines`方法會傳回資料當做陣列。 `char`資料類型是必要的因為`Split`方法會傳回`array`所在每個項目型別的`char`。 (如需陣列的詳細資訊，請參閱 < [ASP.NET Web 程式設計使用 Razor 語法簡介](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects)。)
3. 執行網頁瀏覽器中。 會顯示您在先前的範例輸入資料。 

    ![[影像]](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> **顯示從 Microsoft Excel 以逗號分隔檔案的資料**
> 
> 您可以使用 Microsoft Excel 來儲存為逗號分隔檔案的試算表中所包含的資料 (*.csv*檔案)。 這麼做之後，檔案會以純文字，不是以 Excel 格式儲存。 在試算表中的每個資料列分隔文字檔中的分行符號，每個資料項目以逗號分隔。 您可以使用上述範例所示的程式碼讀取 Excel 以逗號分隔的檔案，只要變更您的程式碼中的資料檔案的名稱。


<a id="Deleting_Files"></a>
## <a name="deleting-files"></a>刪除檔案

若要刪除的檔案，從您的網站，您可以使用`File.Delete`方法。 此程序示範如何讓使用者能夠刪除映像 (*.jpg*檔案) 從*映像*資料夾如果他們知道檔案的名稱。

> [!NOTE] 
> 
> **重要**在生產網站中，您通常會限制誰可以對資料進行變更。 有關如何設定成員資格，以及有關如何授權使用者在站台上執行工作的相關資訊，請參閱[新增的安全性和 ASP.NET Web Pages 網站的成員資格](https://go.microsoft.com/fwlink/?LinkId=202904)。


1. 在網站中，建立名稱為的子*映像*。
2. 複製一或多個 *.jpg*將檔案*映像*資料夾。
3. 在網站根目錄中，建立新的檔案，名為*FileDelete.cshtml*。
4. 以下列內容取代現有的內容： 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    此頁面包含的表單，其中使用者可以在這裡輸入影像檔的名稱。 不進入 *.jpg*副檔名; 藉由限制這類的檔案名稱，協助您防止使用者刪除網站上的任意檔案。

    程式碼會讀取使用者輸入，並接著會建構的完整路徑的檔案名稱。 若要建立的路徑，此程式碼會使用目前的網站路徑 (傳回`Server.MapPath`方法)，則*映像*資料夾名稱、 使用者提供的名稱和".jpg"做為常值的字串。

    若要刪除的檔案，程式碼會呼叫`File.Delete`方法，將您剛才建構的完整路徑傳遞給它。 在結束標記時，程式碼會顯示確認訊息已刪除的檔案。
5. 執行網頁瀏覽器中。 

    ![[影像]](working-with-files/_static/image5.jpg)
6. 輸入要刪除，然後按一下 檔案名稱**送出**。 如果刪除了檔案，檔案的名稱會顯示在頁面底部。

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a>讓使用者上傳檔案

`FileUpload`協助程式可讓使用者將檔案上傳至您的網站。 下列程序會示範如何讓使用者上傳一個檔案。

1. 將 ASP.NET Web Helpers Library 新增至您的網站，如中所述[安裝 ASP.NET Web Pages 網站中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您未在先前新增。
2. 在 *應用程式\_資料*資料夾中，建立新資料夾並將它命名*UploadedFiles*。
3. 在根目錄中，建立新的檔案，名為*FileUpload.cshtml*。
4. 以下列內容取代現有的內容頁面中： 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    頁面的內文部分使用`FileUpload`協助程式來建立上傳 方塊中，您應該已熟悉的按鈕：

    ![[影像]](working-with-files/_static/image6.jpg)

    您設定的屬性`FileUpload`協助程式會指定您想要上傳檔案的單一方塊，以及您想要讀取的 [提交] 按鈕**上傳**。 （您將在本文稍後新增更多的方塊）。

    當使用者按一下**上傳**，位於頁面頂端的程式碼取得的檔案，並將它儲存。 `Request`平常用來取得從表單欄位的值的物件還擁有`Files`陣列，其中包含 （或多個檔案），已上傳。 您可以取得個別的檔案，從陣列中的特定位置&#8212;，若要取得第一次上傳的檔案，您會收到`Request.Files[0]`，以取得第二個檔案中，會得到`Request.Files[1]`，依此類推。 （請記住，在程式設計中，計算通常從零開始）。

    當您擷取上傳的檔案時，您將它放在變數中 (在這裡， `uploadedFile`)，讓您可以操作。 若要判斷上傳的檔案名稱，您只取得其`FileName`屬性。 不過，當使用者將檔案上傳，`FileName`包含使用者的原始名稱，其中包含的完整路徑。 它可能會看起來像這樣：

    *C:\Users\Public\Sample.txt*

    您不想該路徑資訊，不過，因為這是使用者的電腦，不適用於您的伺服器上的路徑。 您只想實際檔案名稱 (*Sample.txt*)。 您可以使用去除只從路徑檔案`Path.GetFileName`方法，就像這樣：

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    `Path`物件是一個公用程式，有多種方法，就像這樣，您可以使用刪除路徑、 合併路徑，然後依此類推。

    一旦上傳的檔案名稱後，您可以建立新的路徑，您要上傳的檔案儲存在您的網站。 在此情況下，您將結合`Server.MapPath`，將資料夾的名稱 (*應用程式\_資料/UploadedFiles*)，與新已移除的檔案名稱，以建立新的路徑。 接著，您就可以呼叫上傳的檔案`SaveAs`實際上儲存檔案的方法。
5. 執行網頁瀏覽器中。 

    ![[影像]](working-with-files/_static/image7.jpg)
6. 按一下 **瀏覽**，然後選取要上傳的檔案。 

    ![[影像]](working-with-files/_static/image8.jpg)

    文字方塊旁的**瀏覽**按鈕將包含路徑和檔案的位置。

    ![[影像]](working-with-files/_static/image9.jpg)
7. 按一下 [上傳] 。
8. 在網站中，以滑鼠右鍵按一下專案資料夾，並再按**重新整理**。
9. 開啟*UploadedFiles*資料夾。 您上傳的檔案是在資料夾中。 

    ![[影像]](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a>讓使用者上傳多個檔案

在上述範例中，您可以讓使用者上傳一個檔案。 但您可以使用`FileUpload`一次上傳多個檔案的協助程式。 這很方便的案例，例如上傳的相片，其中一個檔案上傳一次是單調乏味。 (您可以閱讀有關將在相片上傳[使用 ASP.NET Web Pages 網站中的映像](https://go.microsoft.com/fwlink/?LinkId=202897)。)此範例示範如何讓使用者上傳兩個一次，雖然您可以使用相同的技巧上, 傳不止是這樣。

1. 將 ASP.NET Web Helpers Library 新增至您的網站，如中所述[安裝 ASP.NET Web Pages 網站中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有這麼做。
2. 建立名為的新頁面*FileUploadMultiple.cshtml*。
3. 以下列內容取代現有的內容頁面中：  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    在此範例中，`FileUpload`頁面的主體中的協助程式預設會設定讓使用者上傳兩個檔案。 因為`allowMoreFilesToBeAdded`設為`true`，helper 會呈現一個連結，可讓使用者將新增更多的上傳方塊：

    ![[影像]](working-with-files/_static/image11.jpg)

    若要處理的使用者上傳的檔案，程式碼會使用相同的基本技巧，您在上一個範例中使用&#8212;取得檔案從`Request.Files`然後將它儲存。 （各種項目包括您需要如何取得正確的檔案名稱和路徑。）使用者可能上傳多個檔案，而您不知道有許多這次的創新。 若要了解，您可以取得`Request.Files.Count`。

    使用此號碼在手中，您可以循環`Request.Files`，依次擷取每個檔案，並將它儲存。 當您想要循環播放，已知的數次，透過集合時，您可以使用`for`迴圈中的，像這樣：

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    變數`i`是只是暫時的計數器，就會從零到任何您所設定的時間上限。 在此情況下，最高上限是檔案數目。 但是，因為計數器便會啟動位於零，通常適用於計算在 ASP.NET 中的案例中，最高上限實際上是一個小於檔案計數。 （如果這三個檔案上傳時的計數是 0 到 2。）

    `uploadedCount`變數加總，已成功上傳並儲存所有檔案。 此程式碼負責預期的檔案可能無法上傳的可能性。
4. 執行網頁瀏覽器中。 瀏覽器顯示的頁面和其上傳兩個方塊。
5. 選取要上傳的兩個檔案。
6. 按一下 **加入另一個檔案**。 此頁面會顯示新的 [上傳] 方塊。 

    ![[影像]](working-with-files/_static/image12.jpg)
7. 按一下 [上傳] 。
8. 在網站中，以滑鼠右鍵按一下專案資料夾，並再按**重新整理**。
9. 開啟*UploadedFiles*資料夾，以查看已成功上傳的檔案。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源


[使用 ASP.NET Web Pages 網站中的映像](https://go.microsoft.com/fwlink/?LinkId=202897)

[匯出至 CSV 檔案](https://msdn.microsoft.com/library/ms155919.aspx)
