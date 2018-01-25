---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: "建立 MVC 3 應用程式使用 Razor 和不顯眼的 JavaScript |Microsoft 文件"
author: microsoft
description: "使用者清單的範例 web 應用程式示範如何建立 ASP.NET MVC 3 應用程式使用 Razor 檢視引擎是簡單。 範例應用程式 s 中..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 29b45c07b5498542abbf22c4c3001b1cee41edc9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>建立 MVC 3 應用程式使用 Razor 和不顯眼的 JavaScript
====================
by [Microsoft](https://github.com/microsoft)

> 使用者清單的範例 web 應用程式示範如何建立 ASP.NET MVC 3 應用程式使用 Razor 檢視引擎是簡單。 範例應用程式示範如何使用新的 Razor 檢視引擎，使用 ASP.NET MVC 3 版和 Visual Studio 2010 來建立虛構的使用者清單網站包含功能，例如建立、 顯示、 編輯和刪除使用者。
> 
> 本教學課程描述才能建立使用者清單的範例 ASP.NET MVC 3 應用程式所採取的步驟。 本主題隨附了與 C# 和 VB 原始碼的 Visual Studio 專案：[下載](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114)。 如果您有本教學課程有關的問題，請張貼到[MVC 論壇](https://forums.asp.net/1146.aspx)。


## <a name="overview"></a>總覽

您將建置的應用程式是一個簡單的使用者清單的網站。 使用者可以輸入、 檢視和更新使用者資訊。

![範例站台](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

您可以下載 VB 和 C# 已完成的專案[這裡](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f)。

## <a name="creating-the-web-application"></a>建立 Web 應用程式

若要開始本教學課程，請開啟 Visual Studio 2010 並建立新專案使用*ASP.NET MVC 3 Web 應用程式*範本。 將應用程式命名&quot;Mvc3Razor&quot;。

[![新的 MVC 3 專案](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

在**新增 ASP.NET MVC 3 專案**對話方塊中，選取**網際網路應用程式**選取 Razor 檢視引擎，，然後按一下 **確定**。

![新的 ASP.NET MVC 3 專案對話方塊](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

本教學課程中您不使用 ASP.NET 成員資格提供者，因此您可以刪除與登入和成員資格相關聯的所有檔案。 在**方案總管 中**，移除下列檔案和目錄：

- *Controllers\AccountController*
- *Models\AccountModels*
- *Views\Shared\\_LogOnPartial*
- *Views\Account* （及此目錄中的所有檔案）

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

編輯 *\_Layout.cshtml*檔案，並將內部標記`<div>`名`logindisplay`訊息 *&quot;*登入已停用&quot;. 下列範例會顯示新標記：

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>加入模型

在**方案總管] 中**，以滑鼠右鍵按一下*模型*資料夾中，選取**新增**，然後按一下 [**類別**。

![新使用者 Mdl 類別](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

將類別命名為 `UserModel` 。 取代內容*UserModel*以下列程式碼檔案：

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

`UserModel`類別代表使用者。 類別的每個成員都以註解[需要](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)屬性從[DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空間。 中的屬性[DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空間提供 web 應用程式的自動用戶端和伺服器端驗證。

開啟`HomeController`類別，然後將`using`指示詞，好讓您可以存取`UserModel`和`Users`類別：

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

後方`HomeController`宣告中，加入下列註解和參考`Users`類別：

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

`Users`類別是經過簡化的記憶體中的資料存放區，您將使用在本教學課程。 在實際的應用程式中，您會使用資料庫來儲存使用者資訊。 前的幾行`HomeController`檔案會在下列範例所示：

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

建置應用程式，如此使用者模型將 scaffolding 精靈中下一個步驟。

## <a name="creating-the-default-view"></a>建立預設檢視

下一個步驟是加入動作方法和檢視來顯示的使用者。

刪除現有*Views\Home\Index*檔案。 您將建立新*索引*来顯示的使用者檔案。

在`HomeController`類別中，取代內容`Index`方法取代下列程式碼：

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

以滑鼠右鍵按一下`Index`方法，然後按一下**加入檢視**。

![加入檢視](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

選取**建立強型別檢視**選項。 如**檢視資料類別**，選取**Mvc3Razor.Models.UserModel**。 (如果您沒有看到**Mvc3Razor.Models.UserModel**中**檢視資料類別** 方塊中，您必須先建置專案。)請確定檢視引擎設**Razor**。 設定**檢視內容**至**清單**，然後按一下 **新增**。

![加入索引檢視](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

新的檢視自動 scaffolds 使用者資料傳遞至`Index`檢視。 檢查新產生*Views\Home\Index*檔案。 **新建**，**編輯**，**詳細資料**，和**刪除**連結無法運作，而其他頁面的功能。 執行網頁。 您看到使用者的清單。

![索引頁](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

開啟*Index.cshtml*檔案，並將`ActionLink`標記**編輯**，**詳細資料**，和**刪除**為下列程式碼:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

使用者名稱會做為識別碼尋找中的選定的記錄**編輯**，**詳細資料**，和**刪除**連結。

## <a name="creating-the-details-view"></a>建立詳細資料檢視

下一個步驟是加入`Details`動作方法，以顯示使用者詳細資料檢視。

![詳細資料](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

加入下列`Details`主控制器的方法：

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

以滑鼠右鍵按一下`Details`方法，然後選取**加入檢視**。 確認**檢視資料類別**方塊包含 **Mvc3Razor.Models.UserModel***。* 設定**檢視內容**至**詳細資料**，然後按一下 **新增**。

![加入詳細資料檢視](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

執行應用程式並選取詳細資料 連結。 自動 scaffolding 顯示模型中的每一個屬性。

![詳細資料](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>建立編輯檢視

加入下列`Edit`主控制器的方法。

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

加入檢視如同先前的步驟，但設定**檢視內容**至**編輯**。

![加入編輯檢視](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

執行應用程式，並編輯一位使用者的第一個和最後一個名稱。 如果違反任何`DataAnnotation`已套用至的條件約束`UserModel`類別，當您送出表單時，您會看到伺服端程式碼所產生的驗證錯誤。 比方說，如果您變更名字&quot;王&quot;至&quot;A&quot;，當您提交表單，表單上顯示下列錯誤：

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

在本教學課程中，您要將使用者名稱視為主索引鍵。 因此，無法變更使用者名稱屬性。 在*Edit.cshtml*檔案，就在`Html.BeginForm`陳述式中，設定使用者名稱是隱藏的欄位。 這會導致要傳入模型的屬性。 下列程式碼片段顯示的位置`Hidden`陳述式：

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

取代`TextBoxFor`和`ValidationMessageFor`標記的使用者名稱與`DisplayFor`呼叫。 `DisplayFor`方法顯示的屬性為唯讀狀態的項目。 下列範例顯示完整的標記。 原始`TextBoxFor`和`ValidationMessageFor`呼叫標記為註解 Razor 開始註解，然後結束註解字元 (`@* *@`)

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>啟用用戶端驗證

若要啟用 ASP.NET MVC 3 中的用戶端驗證，您必須設定兩個旗標，而且您必須包含三個的 JavaScript 檔案。

開啟應用程式的*Web.config*檔案。 確認`that ClientValidationEnabled`和`UnobtrusiveJavaScriptEnabled`設為應用程式設定中，則為 true。 下列片段從根*Web.config*檔案會顯示正確的設定：

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

設定`UnobtrusiveJavaScriptEnabled`來啟用不顯眼的 Ajax 和不顯眼的用戶端驗證，則為 true。 當您使用不顯眼的驗證時，驗證規則會變成 HTML5 屬性。 HTML5 屬性名稱可以包含小寫字母、 數字和連字號。

設定`ClientValidationEnabled`true 會啟用用戶端驗證。 藉由在應用程式中設定這些機碼*Web.config*檔案中，您可以啟用用戶端驗證和不顯眼的 JavaScript，整個應用程式。 您也可以啟用或停用這些設定個別的檢視，或使用下列程式碼的控制器方法：

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

您也需要呈現的檢視中包含數個 JavaScript 檔案。 若要在所有檢視中包含 JavaScript 的簡單方法是將其新增至*_layout.cshtml\\_Layout.cshtml*檔案。 取代`<head>`元素 *\_Layout.cshtml*以下列程式碼檔案：

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

前兩個 jQuery 指令碼會裝載由 Microsoft Ajax 內容傳遞網路 (CDN)。 利用 Microsoft Ajax CDN，您可以大幅改善應用程式的第一個叫用次數效能。

執行應用程式，然後按一下 [編輯] 連結。 在瀏覽器中檢視網頁原始檔。 瀏覽器來源顯示表單的許多屬性`data-val`（適用於資料驗證）。 啟用用戶端驗證和不顯眼的 JavaScript 時，與用戶端驗證規則的輸入的欄位會包含`data-val="true"`觸發不顯眼的用戶端驗證的屬性。 比方說，`City`模型中的欄位以裝飾[需要](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)屬性，這會產生 HTML，下列範例所示：

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

針對每個用戶端驗證規則，將屬性加入具有表單`data-val-rulename="message"`。 使用`City`欄位先前範例所示，必要的用戶端驗證規則會產生`data-val-required`屬性和訊息&quot;縣 （市） 欄位是必要的&quot;。 執行應用程式，請編輯一位使用者，並清除`City`欄位。 當您從欄位索引標籤上時，您會看到用戶端驗證錯誤訊息。

![所需的縣 （市)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

同樣地，用戶端驗證規則中每一個參數，將屬性加入具有表單`data-val-rulename-paramname=paramvalue`。 例如，`FirstName`附註屬性[StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)屬性，並且指定最小長度為 3 和最大長度為 8。 資料驗證規則，名為`length`具有參數名稱`max`和參數值 8。 下圖顯示針對所產生的 HTML`FirstName`欄位編輯一位使用者時：

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

如需不顯眼的用戶端驗證的詳細資訊，請參閱文章[ASP.NET MVC 3 中不顯眼的用戶端驗證](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)Brad Wilson 的部落格。

> [!NOTE]
> ASP.NET MVC 3 在 beta 版中，您有時需要送出表單時，才能啟動用戶端驗證。 這可能會變更的最終版本。


## <a name="creating-the-create-view"></a>建立建立檢視

下一個步驟是加入`Create`動作方法，若要讓使用者建立新的使用者檢視。 加入下列`Create`主控制器的方法：

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

加入檢視如同先前的步驟，但設定**檢視內容**至**建立**。

![建立檢視](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

執行應用程式中，選取**建立**連結，並加入新的使用者。 `Create`方法自動會利用用戶端和伺服器端驗證。 嘗試輸入使用者名稱包含空格，例如&quot;Ben X&quot;。 當您從 [使用者名稱] 欄位，用戶端驗證錯誤的索引標籤 (`White space is not allowed`) 隨即出現。

## <a name="add-the-delete-method"></a>新增 Delete 方法

若要完成本教學課程，加入下列`Delete`主控制器的方法：

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

新增`Delete`如同先前的步驟，設定檢視**檢視內容**至**刪除**。

![刪除檢視](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

您現在有簡單但功能完整 ASP.NET MVC 3 應用程式驗證。
