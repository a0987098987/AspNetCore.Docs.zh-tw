---
uid: web-pages/overview/data/working-with-files
title: 使用 ASP.NET Web Pages (Razor) 網站中的檔案 |Microsoft 文件
author: tfitzmac
description: 本章節將說明如何讀取、 寫入、 附加、 刪除和上傳檔案。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: eee916e4-ba4c-439a-a24e-68df7d45a569
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/working-with-files
msc.type: authoredcontent
ms.openlocfilehash: 0f119f8fb4873e55292203f21a2efd8f26793ae4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2018
---
<a name="working-with-files-in-an-aspnet-web-pages-razor-site"></a>使用 ASP.NET Web Pages (Razor) 網站中的檔案
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明如何讀取、 寫入、 新增、 刪除和 ASP.NET Web Pages (Razor) 網站中的檔案上傳。
> 
> > [!NOTE]
> > 如果您想要上傳映像，以及操作它們 （例如翻轉或調整其大小），請參閱[處理映像，在 ASP.NET Web Pages 站台中](https://go.microsoft.com/fwlink/?LinkId=202897)。
> 
> 
> **您將學習：** 
> 
> - 如何建立文字檔，並寫入其中的資料。
> - 如何將資料附加到現有的檔案。
> - 如何讀取檔案，並顯示它。
> - 如何刪除網站的檔案。
> - 如何讓使用者上傳一或多個檔案。
> 
> 這些是 ASP.NET 程式設計文件中所引進的功能：
> 
> - `File`物件，可用來管理檔案。
> - `FileUpload`協助程式。
> - `Path`物件，提供可讓您操作路徑和檔案名稱的方法。
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
## <a name="creating-a-text-file-and-writing-data-to-it"></a>建立文字檔案並將資料寫入

除了在您的網站使用的資料庫，您可能會使用檔案。 比方說，您可能會使用文字檔來儲存站台資料的簡單方式。 (用來儲存資料的文字檔案有時稱為*一般檔案*。)文字檔案可以位於不同格式，例如*.txt*， *.xml*，或*.csv* （以逗號分隔值）。

如果您想要將資料儲存在文字檔中，您可以使用`File.WriteAllText`方法，以指定要建立的檔案和要寫入其中的資料。 在此程序，您將建立包含簡單的表單具有三個的頁面`input`（名字、 姓氏和電子郵件地址） 的項目和**送出** 按鈕。 當使用者提交表單時，您將會儲存使用者的輸入文字檔案中。

1. 建立新的資料夾，名為*應用程式\_資料*，如果已經存在。
2. 在您的網站根目錄中，建立新的檔案命名為*UserData.cshtml*。
3. 以下列內容取代現有的內容： 

    [!code-cshtml[Main](working-with-files/samples/sample1.cshtml)]

    HTML 標記會建立具有三個文字方塊的表單。 在程式碼中，您會使用`IsPost`屬性來判斷頁面是否已提交之前開始處理。

    第一個工作是以取得使用者輸入，並將它指派給變數。 然後，程式碼會將個別變數的值串連成一個逗號分隔的字串，然後儲存在不同的變數。 請注意，逗號分隔符號字串，包含在引號 （"，"），因為您實際上內嵌逗號到您要建立大型字串。 在您串連在一起的資料結尾，加入`Environment.NewLine`。 這樣會加入分行符號 （新行字元）。 您要建立與所有這種串連是一個字串，看起來像這樣：

    [!code-css[Main](working-with-files/samples/sample2.css)]

    （具有不可見的行結尾的符號。）

    您接著會建立一個變數 (`dataFile`)，其中包含要將資料儲存在檔案名稱與位置。 設定位置，需要一些特殊處理。 在網站中，它是不良作法為絕對路徑，例如程式碼中參考*C:\Folder\File.txt* web 伺服器上的檔案。 如果網站被移動，就會出錯的絕對路徑。 此外，裝載站台 （而非您自己電腦上） 您通常不甚至知道正確的路徑當您撰寫程式碼。

    但有時候 （如 now，供寫入的檔案) 則需要完整的路徑。 解決方案是使用`MapPath`方法`Server`物件。 這會傳回完整路徑到您的網站。 網站根目錄，您的使用者取得路徑`~`運算子 (represen 站台的虛擬根) 到`MapPath`。 (您也可以傳遞給子資料夾名稱，例如*~/App\_資料 /*，以取得該資料夾的路徑。)然後，您可以串連到任何方法以建立完整的路徑會傳回的其他資訊。 在此範例中，您可以將檔案名稱。 (您可以閱讀更多有關如何使用中的檔案和資料夾路徑[ASP.NET Web Pages 程式設計使用 Razor 語法的簡介](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths)。)

    此檔案會儲存在*應用程式\_資料*資料夾。 此資料夾是在 ASP.NET 中用來儲存資料檔案中所述的特殊資料夾[簡介使用 ASP.NET Web Pages 站台中的資料庫](https://go.microsoft.com/fwlink/?LinkId=195209)。

    `WriteAllText`方法`File`物件會將資料寫入檔案。 這個方法會採用兩個參數： 要寫入的檔案和要寫入的實際資料的名稱 （以路徑）。 請注意，第一個參數的名稱具有`@`字元做為前置詞。 這會告知的 ASP.NET 提供逐字字串常值，且該字元，像是"/"不應該以特殊方式解譯。 (如需詳細資訊，請參閱[ASP.NET Web 程式設計使用 Razor 語法的簡介](https://go.microsoft.com/fwlink/?LinkId=195205#ID_WorkingWithFileAndFolderPaths)。)

    > [!NOTE]
    > 為了讓您的程式碼儲存在檔案*應用程式\_資料*資料夾中，應用程式需要讀取-寫入該資料夾的權限。 在開發電腦上這不通常是發生問題。 不過，當您將您的網站發行至裝載提供者的 web 伺服器時，您可能需要明確設定這些權限。 如果您在裝載提供者的伺服器上執行此程式碼，並發生錯誤，請洽詢主機服務提供者了解如何設定這些權限。

- 執行網頁瀏覽器中。 

    ![](working-with-files/_static/image1.jpg)
- 在欄位中輸入值，然後按一下**送出**。
- 關閉瀏覽器。
- 返回專案，然後重新整理檢視。
- 開啟*data.txt*檔案。 您在表單中提交的資料位於檔案中。 

    ![[影像]](working-with-files/_static/image2.jpg)
- 關閉*data.txt*檔案。

<a id="Appending_Data"></a>
## <a name="appending-data-to-an-existing-file"></a>將資料附加到現有的檔案

在上述範例中，您已經使用`WriteAllText`建立在其中有一段資料的文字檔。 如果您再次呼叫這個方法，並將它傳遞相同的檔案名稱，會完全覆寫現有的檔案。 不過，在您建立檔案後您通常想要將新資料加入至檔案結尾。 您可以使用`AppendAllText`方法`File`物件。

1. 在網站上建立一份*UserData.cshtml*檔案，並命名為複製*UserDataMultiple.cshtml*。
2. 取代的程式碼區塊，才能開啟`<!DOCTYPE html>`的下列程式碼區塊的標記： 

    [!code-cshtml[Main](working-with-files/samples/sample3.cshtml)]

    此程式碼中有一項變更從先前的範例。 而不是使用`WriteAllText`，它會使用`the AppendAllText`方法。 方法很相似，不同之處在於`AppendAllText`將資料加入至檔案結尾。 如同`WriteAllText`，`AppendAllText`如果它不存在，建立檔案。
3. 執行網頁瀏覽器中。
4. 輸入欄位的值，然後按一下 **送出**。
5. 加入更多資料，然後再次提交表單。
6. 返回您的專案，以滑鼠右鍵按一下專案資料夾，然後按一下**重新整理**。
7. 開啟*data.txt*檔案。 它現在會包含您剛才輸入的新資料。 

    ![[影像]](working-with-files/_static/image3.jpg)

<a id="Reading_and_Displaying_Data"></a>
## <a name="reading-and-displaying-data-from-a-file"></a>讀取和顯示資料的檔案

即使您不需要將資料寫入文字檔案，您有時可能必須從一個讀取的資料。 若要這樣做，您可以再次使用`File`物件。 您可以使用`File`物件來個別讀取一行 （以分行符號分隔），或是讀取無論分隔方式的個別項目。

此程序會示範如何讀取，並顯示您在上一個範例中建立的資料。

1. 在您的網站根目錄中，建立新的檔案命名為*DisplayData.cshtml*。
2. 以下列內容取代現有的內容： 

    [!code-cshtml[Main](working-with-files/samples/sample4.cshtml)]

    程式碼會啟動所讀取的檔案，您建立在上述範例中名為的變數`userData`，使用這個方法呼叫：

    [!code-css[Main](working-with-files/samples/sample5.css)]

    若要這樣做的程式碼位於`if`陳述式。 當您想要讀取的檔案時，最好在其中使用`File.Exists`先判斷檔案是否有可用的方法。 程式碼也會檢查檔案是否為空白。

    頁面主體會包含兩個`foreach`迴圈，其中巢狀方式置於另一個。 外部`foreach`迴圈從資料檔案中一行取得一次。 在此情況下，線條會由檔案中的分行符號&#8212;亦即，每個資料項目是的那一行。 外部迴圈會建立新的項目 (`<li>`項目) 內的已排序清單 (`<ol>`項目)。

    內部迴圈會將每個資料列分割為使用逗號做為分隔符號項目 （欄位）。 (根據上一個範例，這表示每一行都包含三個欄位&#8212;名字、 姓氏和電子郵件地址中，每一個逗號分隔。)內部迴圈也會建立`<ul>`清單和顯示一個清單項目資料行中每個欄位。

    程式碼說明如何使用兩種資料類型、 陣列和`char`資料型別。 陣列是必要的因為`File.ReadAllLines`方法會傳回資料當做陣列。 `char`資料類型是必要的因為`Split`方法會傳回`array`，其中每個元素是型別`char`。 (如需陣列的詳細資訊，請參閱[ASP.NET Web 程式設計使用 Razor 語法的簡介](https://go.microsoft.com/fwlink/?LinkId=202890#ID_CollectionsAndObjects)。)
3. 執行網頁瀏覽器中。 會顯示您輸入上一個範例中的資料。 

    ![[影像]](working-with-files/_static/image4.jpg)

> [!TIP] 
> 
> **顯示從 Microsoft Excel 以逗號分隔的檔案資料**
> 
> 您可以使用 Microsoft Excel 來儲存為逗號分隔檔案的試算表中所包含的資料 (*.csv*檔案)。 當您這樣做時，檔案會儲存在不是以 Excel 格式的純文字。 試算表中的每個資料列來隔開分行符號，以在文字檔案中，而且每個資料項目以逗號分隔。 您可以使用上述範例所示的程式碼讀取 Excel 以逗號分隔檔案，只要變更您的程式碼中的資料檔案的名稱。


<a id="Deleting_Files"></a>
## <a name="deleting-files"></a>刪除檔案

若要從您的網站刪除檔案，您可以使用`File.Delete`方法。 此程序示範如何讓使用者刪除映像 (*.jpg*檔案) 從*映像*資料夾知道檔案的名稱。

> [!NOTE] 
> 
> **重要**在生產網站，您通常會限制已獲允許的人員進行變更的資料。 如需如何設定成員資格以及方式來授權使用者在站台上執行工作的相關資訊，請參閱[加入安全性和 ASP.NET Web Pages 站台中的成員資格](https://go.microsoft.com/fwlink/?LinkId=202904)。


1. 在網站上建立一個名為子*映像*。
2. 複製一或多個*.jpg*檔案至*映像*資料夾。
3. 在網站的根目錄，在建立新的檔案，名為*FileDelete.cshtml*。
4. 以下列內容取代現有的內容： 

    [!code-cshtml[Main](working-with-files/samples/sample6.cshtml)]

    此頁面包含的表單，使用者可以輸入的映像檔的名稱。 不進入*.jpg*副檔名; 藉由限制如下的檔案名稱，協助您防止使用者刪除您的網站上的任意檔案。

    程式碼會讀取使用者輸入，並接著建構的完整路徑的檔案名稱。 若要建立路徑，程式碼會使用目前的網站路徑 (傳回`Server.MapPath`方法)，則*映像*資料夾名稱、 使用者提供的名稱和".jpg"為常值字串。

    若要刪除的檔案，程式碼會呼叫`File.Delete`方法，將您剛才建構的完整路徑傳遞給它。 在結束標記時，程式碼會顯示確認訊息的檔案被刪除。
5. 執行網頁瀏覽器中。 

    ![[影像]](working-with-files/_static/image5.jpg)
6. 輸入要刪除，然後按一下 檔案名稱**送出**。 如果刪除了檔案，檔案的名稱會顯示在頁面底部。

<a id="Letting_Users_Upload_a_File"></a>
## <a name="letting-users-upload-a-file"></a>讓使用者上傳檔案

`FileUpload`協助程式可讓使用者將檔案上傳至您的網站。 下列程序會示範如何讓使用者上傳一個檔案。

1. 中所述，您的網站加入 ASP.NET Web Helpers Library[安裝 ASP.NET Web Pages 站台中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您並未在先前新增。
2. 在*應用程式\_資料*資料夾中，建立新資料夾並將其命名*UploadedFiles*。
3. 在根目錄中，建立新的檔案命名為*FileUpload.cshtml*。
4. 以下列內容取代現有的內容頁面中： 

    [!code-cshtml[Main](working-with-files/samples/sample7.cshtml)]

    頁面的本文部分會使用`FileUpload`helper 來建立上傳 方塊中，您可能很熟悉的按鈕：

    ![[影像]](working-with-files/_static/image6.jpg)

    您為設定的屬性`FileUpload`helper 指定您想要上傳檔案的單一方塊和您想要讀取的 [提交] 按鈕**上傳**。 （您將在本文稍後新增更多的方塊）。

    當使用者按一下**上傳**，位於頁面頂端的程式碼會取得檔案，並將其儲存。 `Request`您平常用來取得表單欄位值的物件也有`Files`陣列，其中包含檔案 （或檔案），已上傳。 您可以取得從陣列中的特定位置的個別檔案&#8212;，若要取得第一次上傳的檔案，您會收到`Request.Files[0]`、 取得第二個檔案中，您取得`Request.Files[1]`，依此類推。 （請記住，在程式設計中，通常計數從零開始）。

    當您擷取上傳的檔案時，您將它放在變數中 (在這裡， `uploadedFile`)，讓您可以操作。 若要確定上傳的檔案名稱，您只取得其`FileName`屬性。 不過，當使用者將檔案上傳，`FileName`包含使用者的原始名稱，其中包含的完整路徑。 它看起來可能像這樣：

    *C:\Users\Public\Sample.txt*

    您不希望該路徑資訊，不過，因為這是使用者的電腦，不適用於您的伺服器上的路徑。 您只想實際的檔案名稱 (*Sample.txt*)。 您可以使用帶狀出該檔案的路徑`Path.GetFileName`方法，就像這樣：

    [!code-csharp[Main](working-with-files/samples/sample8.cs)]

    `Path`物件是一個公用程式，有多種方法，就像這樣，您可以使用區域路徑、 合併路徑，然後依此類推。

    一旦上傳的檔案名稱後，您可以建立新的路徑，您要將上傳的檔案儲存在您的網站。 在此情況下，您將合併`Server.MapPath`，資料夾名稱 (*應用程式\_資料/UploadedFiles*)，與新已移除的檔案名稱，以建立新的路徑。 接著，您可以呼叫上傳的檔案`SaveAs`實際上儲存檔案的方法。
5. 執行網頁瀏覽器中。 

    ![[影像]](working-with-files/_static/image7.jpg)
6. 按一下**瀏覽**，然後選取要上傳的檔案。 

    ![[影像]](working-with-files/_static/image8.jpg)

    [下一步] 文字方塊中**瀏覽**按鈕將包含路徑和檔案位置。

    ![[影像]](working-with-files/_static/image9.jpg)
7. 按一下 [上傳] 。
8. 在網站上以滑鼠右鍵按一下專案資料夾，然後按一下 **重新整理**。
9. 開啟*UploadedFiles*資料夾。 您上傳之檔案位於資料夾中。 

    ![[影像]](working-with-files/_static/image10.jpg)

<a id="Letting_Users_Upload_Multiple_Files"></a>
## <a name="letting-users-upload-multiple-files"></a>讓使用者上傳多個檔案

在上述範例中，您可以讓使用者上傳一個檔案。 但是您可以使用`FileUpload`一次上傳多個檔案的協助程式。 這是很方便用來上傳的相片，過於冗長一次上傳一個檔案等案例。 (您可以閱讀上傳相片[處理映像，在 ASP.NET Web Pages 站台中](https://go.microsoft.com/fwlink/?LinkId=202897)。)這個範例示範如何讓使用者上傳兩個一次，雖然您可以使用相同的技巧，將更多的上傳。

1. 中所述，您的網站加入 ASP.NET Web Helpers Library[安裝 ASP.NET Web Pages 站台中的協助程式](https://go.microsoft.com/fwlink/?LinkId=252372)，如果您還沒有這麼做。
2. 建立名為的新頁面*FileUploadMultiple.cshtml*。
3. 以下列內容取代現有的內容頁面中：  

    [!code-cshtml[Main](working-with-files/samples/sample9.cshtml)]

    在此範例中，`FileUpload`網頁主體中的協助程式預設會設定讓使用者上傳兩個檔案。 因為`allowMoreFilesToBeAdded`設`true`，協助專家會呈現一個連結，可讓使用者加入多個上傳方塊：

    ![[影像]](working-with-files/_static/image11.jpg)

    若要處理的使用者上傳的檔案，程式碼會使用您在上一個範例中使用相同的基本技巧&#8212;取得檔案從`Request.Files`並將其儲存。 （各種的項目包括您需要如何做以讓正確的檔案名稱和路徑。）使用者可能會上載多個檔案，而您不知道許多創新這個時間。 若要了解，您可以取得`Request.Files.Count`。

    具有手中此數目，您可以逐一`Request.Files`、 擷取每個檔案，並將其儲存。 當您想要透過集合進行已知的次數執行迴圈時，您可以使用`for`迴圈，像這樣：

    [!code-csharp[Main](working-with-files/samples/sample10.cs)]

    變數`i`只暫存計數器就會從零到任何您所設定的時間上限。 在此情況下，上限為的檔案數目。 但計數器開始零，一般出現在計算中 ASP.NET 的案例，因為上限實際上是一個小於檔案計數。 （如果三個檔案上傳時，計數是 0 到 2）。

    `uploadedCount`變數總計，已成功上傳並儲存所有檔案。 此程式碼帳戶預期的檔案可能無法上傳的可能性。
4. 執行網頁瀏覽器中。 瀏覽器顯示頁面和其上傳兩個方塊。
5. 選取要上傳兩個檔案。
6. 按一下**加入另一個檔案**。 頁面會顯示新的 [上載] 方塊。 

    ![[影像]](working-with-files/_static/image12.jpg)
7. 按一下 [上傳] 。
8. 在網站上以滑鼠右鍵按一下專案資料夾，然後按一下 **重新整理**。
9. 開啟*UploadedFiles*資料夾，以查看已成功上傳的檔案。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源


[使用中的 ASP.NET Web Pages 站台中的映像](https://go.microsoft.com/fwlink/?LinkId=202897)

[匯出至 CSV 檔案](https://msdn.microsoft.com/library/ms155919.aspx)
