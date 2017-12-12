---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: "發行至 Azure 的 MVC 資料庫第一個站台 |Microsoft 文件"
author: tfitzmac
description: "使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，您可以建立 web 應用程式提供的介面到現有的資料庫。 此教學課程里..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/22/2014
ms.topic: article
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: f75b7192b4d97c88fcbcb4ad7fef88c83157c902
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="publish-mvc-database-first-site-to-azure"></a>發行 MVC 資料庫第一個站台至 Azure
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，您可以建立 web 應用程式提供的介面到現有的資料庫。 此教學課程的數列會顯示如何自動產生程式碼，可讓使用者顯示、 編輯、 建立和刪除存在於資料庫資料表中的資料。 產生的程式碼會對應至資料庫資料表中的資料行。
> 
> 數列的這個部分著重在發行至 Azure 的 web 應用程式和資料庫。 您可以閱讀本主題來了解發行 web 應用程式和資料庫，但實際執行的步驟，您必須在本教學課程開頭啟動。 請參閱[入門](setting-up-database.md)。


## <a name="deploy-your-web-app-on-azure"></a>部署 Azure 上的 web 應用程式

您需要 Azure 帳戶，以完成本教學課程：

- 您可以[開啟免費的 Azure 帳戶](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F)-取得信用額度您可以使用試用付費型 Azure 服務，而且即使他們用於之後可以使帳戶保持最多並使用免費的 Azure 服務。
- 您可以[啟用 MSDN 訂閱者權益](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)-您的 MSDN 訂用帳戶可讓您信用額度付費型 Azure 服務，您可以使用每個月。

若要發行 web 應用程式，以滑鼠右鍵按一下專案，然後選取**發行**。

![發佈站台](publish-to-azure/_static/image1.png)

選取 Microsoft Azure 網站。

![選取 Azure](publish-to-azure/_static/image2.png)

如果您未登入 Azure，提供您的 Azure 帳戶的認證。 然後，選取 [新增] 建立新的 web 應用程式。

![新的站台](publish-to-azure/_static/image3.png)

建立 web 應用程式的唯一名稱。 如果看到綠色的核取記號，右邊的 [名稱] 欄位，您就知道是唯一的名稱。 選取 web 應用程式的區域。 選取**建立新的伺服器**資料庫，並提供適用於這個新的資料庫伺服器的使用者名稱和密碼。 完成後，請按一下**建立**。

![建立站台](publish-to-azure/_static/image4.png)

您的連線值現在所有設定。 您可以將這些值不變。

![connection](publish-to-azure/_static/image5.png)

按 [ **下一步**]。

請注意兩個資料庫連接設定，會指定-ApplicationDBContext 和 ContosoUniversityDataEntities。 ApplicationDBContext 是使用者帳戶資料表的連接。 這些值只會顯示資料庫的連接字串。 它並不表示在發行您的網站時，將會發行這些資料庫。 在您完成 web 應用程式發佈後，您將發行資料庫專案。

![](publish-to-azure/_static/image6.png)

資料庫連接旁邊的省略符號 （...） 會顯示連接字串的詳細資料。 按一下 ContosoUniversityDataEntities 旁邊的省略符號。

![](publish-to-azure/_static/image7.png)

請注意資料庫伺服器和資料庫的名稱。 隨機產生的伺服器名稱。 資料庫名稱是簡單的網站名稱 **\_db**附加。 稍後當您發行您的資料庫時，您會需要這兩個名稱。

按一下**確定**關閉資料庫連接字串 視窗。

在 [發行 Web] 視窗中，按一下**下一步**查看預覽。

![](publish-to-azure/_static/image8.png)

您可以按一下 啟動預覽，請參閱要發佈之檔案的清單。 這是您已發佈此站台的第一次，因為清單是專案中每個相關的檔案。

按一下 [發行] 。

[輸出] 窗格會顯示您的發行集的結果。

![](publish-to-azure/_static/image9.png)

發行集，站台之後 immedialely 在網頁瀏覽器中啟動。 您的網站已部署，而且您可以註冊網站; 新的使用者不過，您 ContosoUniversityData 專案中的資料表有尚未發行。 如果您按一下的學生連結清單，您會收到錯誤。

## <a name="publish-database-to-sql-azure"></a>發行到 SQL Azure 資料庫

發行前您的資料庫，您必須確定您的本機電腦可以連線到資料庫伺服器。 您的資料庫伺服器的防火牆會限制哪些電腦可以連線至資料庫。 您需要將您的電腦的 IP 位址新增至允許的 IP 位址的防火牆。

您透過 Azure 入口網站的 Azure 帳戶來登入。

選取新的資料庫，然後選取**管理**。

![管理](publish-to-azure/_static/image10.png)

您必須設定您的資料庫伺服器，以允許從您的電腦連線。 選取 [管理]，系統會要求您加入目前的 IP 位址，為資料庫伺服器。 選取 [是]。

![新增 ip 位址](publish-to-azure/_static/image11.png)

沒有您在上一個步驟中加入的 IP 位址不是唯一您需要設定連線的 IP 位址的機會。 您可以嘗試登入連線已正確設定資料庫。 提供使用者名稱和您稍早建立的密碼。

![登入](publish-to-azure/_static/image12.png)

如果您收到錯誤訊息，您需要加入其他 IP 位址。 按一下要查看詳細錯誤的錯誤訊息。 在詳細資料，您會看到您要新增的 IP 位址。 請注意此 IP 位址。

![不允許](publish-to-azure/_static/image13.png)

關閉此登入 視窗中，並返回 Azure 入口網站。

瀏覽至儀表板為您的資料庫。 按一下**允許的 IP 位址管理**。

![管理 ip 位址](publish-to-azure/_static/image14.png)

您現在必須將 IP 位址從錯誤訊息。 變更要包含錯誤訊息中，從一個允許的 IP 位址範圍，或為個別項目加入該 IP 位址。

![加入新的位址](publish-to-azure/_static/image15.png)

將變更儲存至允許的 IP 位址。

按一下 [管理]，並再試一次登入資料庫。 您可能需要等候幾分鐘的時間之前允許的 IP 位址已正確設定防火牆。 您可以順利登入資料庫，當您設定完成資料庫的連接。

因為您將會查看資料庫部署的結果，您可以讓此管理視窗保持開啟。

返回您的資料庫專案。 以滑鼠右鍵按一下專案，然後選取**發行**。

![發行資料庫](publish-to-azure/_static/image16.png)

在 [發行資料庫] 視窗中，選取**編輯**。

![編輯](publish-to-azure/_static/image17.png)

提供伺服器的資料庫伺服器，您的驗證認證的名稱。 提供認證之後，選取您從清單中可用的資料庫建立的資料庫。 根據預設，Visual Studio 設定資料庫欄位的名稱可能不是您所建立的資料庫與相同專案的名稱。

![](publish-to-azure/_static/image18.png)

按一下 [確定]。

您可能會想要儲存這個設定檔，因此您可以發佈在未來的更新，而不需重新輸入的所有連接資訊。 選取**建立設定檔**。

![儲存設定檔](publish-to-azure/_static/image19.png)

它會建立名為專案中的檔案**ContosoUniversityData.publish.xml**。 下次您想要將資料庫發行至 Azure，只會載入該設定檔。

現在，按一下 **發行**Azure 上建立資料庫。

![發行](publish-to-azure/_static/image20.png)

開始執行之後一段時間，會顯示發行結果。

![結果](publish-to-azure/_static/image21.png)

現在，您可以移回至管理入口網站為您的資料庫。 重新整理 [設計] 檢視，並注意已部署 3 資料表的預先填入資料。

![新的資料表](publish-to-azure/_static/image22.png)

現在您已準備好要測試 web 應用程式部署至 Azure。 瀏覽至 Azure 上 （例如 http://contosositeexample.azurewebsites.net/) 的 web 應用程式。 按一下 學員清單的連結，您應該會看到學生的索引檢視。

![view](publish-to-azure/_static/image23.png)

有時候，資料庫和連線需要一段時間才能正確地設定。 如果您收到錯誤，請稍候幾分鐘，然後再試。

## <a name="conclusion"></a>結論

這一系列提供如何從現有的資料庫，可讓使用者編輯、 更新、 建立和刪除的資料產生程式碼的簡單範例。 它用於 ASP.NET MVC 5、 Entity Framework 和 ASP.NET Scaffolding 建立專案。

Code First 開發的簡介範例，請參閱[開始使用 ASP.NET MVC 5](../introduction/getting-started.md)。

如需更進階的範例，請參閱[建立 ASP.NET MVC 4 應用程式的 Entity Framework 資料模型](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 請注意，您用於第一個資料庫中的資料搭配使用 DbContext API 與您用於在第一個程式碼中使用資料的 API 相同。 即使您想要使用第一個資料庫，您可以了解如何處理複雜的案例，例如讀取及更新相關的資料處理並行衝突，從程式碼第一次的教學課程，依此類推。 唯一的差異是在建立資料庫、 內容類別和實體類別的方式。

>[!div class="step-by-step"]
[上一步](enhancing-data-validation.md)
