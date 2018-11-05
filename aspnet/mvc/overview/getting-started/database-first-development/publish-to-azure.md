---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: 將 MVC 資料庫第一個站台發佈至 Azure |Microsoft Docs
author: Rick-Anderson
description: 您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。 本教學課程的里...
ms.author: riande
ms.date: 12/22/2014
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 1d2c26c211c5c8d97076327d01fe59d5ba4dc9ac
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021627"
---
<a name="publish-mvc-database-first-site-to-azure"></a>將 MVC 資料庫第一個站台發佈至 Azure
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 您可以使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，來建立 web 應用程式，提供介面給現有的資料庫。 本系列教學課程會示範如何自動產生程式碼，可讓使用者顯示、 編輯、 建立及刪除位於資料庫資料表中的資料。 產生的程式碼會對應至資料庫資料表中的資料行。
> 
> 系列的這個部分會著重於將 web 應用程式和資料庫發佈至 Azure。 您可以閱讀本主題以了解發行的 web 應用程式和資料庫，但實際執行的步驟，您必須在本教學課程的開頭開始。 請參閱[開始使用](setting-up-database.md)。


## <a name="deploy-your-web-app-on-azure"></a>Web 應用程式在 Azure 上部署

您需要有 Azure 帳戶才能完成本教學課程：

- 您可以[免費申請 Azure 帳戶](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)-取得信用額度來試用 Azure 付費服務，您可以使用和甚至用後最多，您可保留帳戶，並使用免費的 Azure 服務。
- 您可以[啟用 MSDN 訂戶權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)-您的 MSDN 訂用帳戶信用額度每月都會提供您 Azure 付費服務，您可以使用。

若要發佈您的 web 應用程式，以滑鼠右鍵按一下專案，然後選取**發佈**。

![發佈站台](publish-to-azure/_static/image1.png)

選取 Microsoft Azure 網站。

![選取 Azure](publish-to-azure/_static/image2.png)

如果您未登入 Azure，提供您的 Azure 帳戶認證。 然後，選取 [新增] 建立新的 web 應用程式。

![新的站台](publish-to-azure/_static/image3.png)

建立您的 web 應用程式的唯一名稱。 如果您看到綠色勾號右邊的 [名稱] 欄位，您會知道是唯一的名稱。 選取您的 web 應用程式的區域。 選取 **建立新的伺服器**資料庫，並提供適用於這個新的資料庫伺服器的使用者名稱和密碼。 完成後，按一下**建立**。

![建立站台](publish-to-azure/_static/image4.png)

您的連線值現在所有設定。 您可以保留這些值維持不變。

![connection](publish-to-azure/_static/image5.png)

按 [ **下一步**]。

如需設定，指定兩個資料庫連線的通知-ApplicationDBContext 和 ContosoUniversityDataEntities。 ApplicationDBContext 是使用者帳戶資料表的連接。 這些值只會顯示資料庫的連接字串。 它並不表示在發行您的網站時，將會發行這些資料庫。 在您完成發佈的 web 應用程式之後，您將發行資料庫專案。

![](publish-to-azure/_static/image6.png)

資料庫連接旁邊的省略符號 （...） 會顯示連接字串的詳細資料。 按一下 ContosoUniversityDataEntities 旁邊的省略符號。

![](publish-to-azure/_static/image7.png)

請注意資料庫伺服器和資料庫的名稱。 隨機產生的伺服器名稱。 資料庫名稱是簡單的網站名稱 **\_db**附加。 稍後當您發行您的資料庫時，您將需要這兩個名稱。

按一下 [**確定**關閉資料庫連接字串] 視窗。

在 發佈 Web 視窗中，按一下**下一步**以查看預覽。

![](publish-to-azure/_static/image8.png)

您可以按一下 [開始預覽] 以查看要發行之檔案的清單。 由於這是您已發佈此站台的第一次，清單會為每個相關專案檔中。

按一下 [發行] 。

[輸出] 窗格會顯示您的發行集的結果。

![](publish-to-azure/_static/image9.png)

在發行集之後, 的站台會是 immedialely 在網頁瀏覽器中啟動。 已部署您的網站，而且您可以註冊站台，新的使用者不過，您 ContosoUniversityData 專案中的資料表有尚未發行。 如果您按一下學生連結的清單，您會收到錯誤。

## <a name="publish-database-to-sql-azure"></a>將資料庫發佈到 SQL Azure

之前發行您的資料庫，您必須確定您的本機電腦可以連線到資料庫伺服器。 您的資料庫伺服器的防火牆會限制機器可以連接到資料庫。 您需要將您的電腦的 IP 位址新增至允許的 IP 位址的防火牆。

登入您的 Azure 帳戶，透過 Azure 入口網站。

選取您新的資料庫，然後選取**管理**。

![管理](publish-to-azure/_static/image10.png)

您必須設定您的資料庫伺服器，以允許從您的電腦的連線。 當您選取 [管理] 時，系統會要求您加入目前的 IP 位址所允許的資料庫伺服器。 選取 [是]。

![新增 ip 位址](publish-to-azure/_static/image11.png)

沒有您在上一個步驟中新增的 IP 位址不是唯一的 IP 位址，您需要設定連線的機會。 您可以嘗試登入若要查看如果連線已正確設定資料庫。 提供使用者名稱和您稍早建立的密碼。

![登入](publish-to-azure/_static/image12.png)

如果您收到錯誤訊息，您需要新增其他 IP 位址。 按一下要查看錯誤的更多詳細的錯誤訊息。 在詳細資料，您會看到您要新增的 IP 位址。 請注意此 IP 位址。

![不允許](publish-to-azure/_static/image13.png)

關閉此 [登入] 視窗中，並返回 Azure 入口網站。

瀏覽至您的資料庫的儀表板。 按一下 **允許的 IP 位址管理**。

![管理 ip 位址](publish-to-azure/_static/image14.png)

您現在必須新增錯誤訊息中的 IP 位址。 變更要包含的一個錯誤訊息中，從允許的 IP 位址的範圍，或將該 IP 位址新增為個別的項目。

![新增新的地址](publish-to-azure/_static/image15.png)

將變更儲存到 允許的 IP 位址。

按一下 [管理]，並再試一次登入的資料庫。 您可能需要等候幾分鐘的時間之前允許的 IP 位址已正確設定防火牆。 您可以成功登入資料庫，當您設定完成資料庫的連接。

您可以將此管理視窗保持在開啟，因為您很快就會檢查資料庫部署的結果。

返回您的資料庫專案。 以滑鼠右鍵按一下專案，然後選取**發佈**。

![發行資料庫](publish-to-azure/_static/image16.png)

在 [發行資料庫] 視窗中，選取**編輯**。

![編輯](publish-to-azure/_static/image17.png)

提供伺服器的資料庫伺服器和您的驗證認證的名稱。 提供認證之後，選取您建立從清單中的可用資料庫的資料庫。 根據預設，Visual Studio 會設定資料庫欄位的名稱，您可能不會為您建立的資料庫相同的專案的名稱。

![](publish-to-azure/_static/image18.png)

按一下 [確定]。

您可能會想要儲存這個設定檔，因此您可以發佈在未來的更新，而不必重新輸入所有的連接資訊。 選取 [建立設定檔]。

![儲存設定檔](publish-to-azure/_static/image19.png)

它會建立名為專案中的檔案**ContosoUniversityData.publish.xml**。 下次您想要將資料庫發佈至 Azure，只會載入該設定檔。

現在，按一下 **發佈**在 Azure 上建立資料庫。

![發行](publish-to-azure/_static/image20.png)

執行一段時間之後, 會顯示發行的結果。

![結果](publish-to-azure/_static/image21.png)

現在，您可以移回至管理入口網站為您的資料庫。 重新整理 [設計] 檢視中，並注意到已部署 3 個資料表的預先填入資料。

![新的資料表](publish-to-azure/_static/image22.png)

現在您已準備好測試部署至 Azure web 應用程式。 瀏覽至 Azure 上的 web 應用程式 (例如 http://contosositeexample.azurewebsites.net/)。 按一下 學生清單的連結，您應該會看到適用於學生的 索引 檢視。

![view](publish-to-azure/_static/image23.png)

有時候，資料庫和連線需要一段時間才能正確設定。 如果您收到錯誤，請等候幾分鐘的時間，並再試一次。

## <a name="conclusion"></a>結論

這一系列將提供如何從現有的資料庫，可讓使用者編輯、 更新、 建立和刪除的資料產生程式碼的簡單範例。 它使用 ASP.NET MVC 5、 Entity Framework 與 ASP.NET Scaffolding 來建立專案。

Code First 開發簡介範例，請參閱[Getting Started with ASP.NET MVC 5](../introduction/getting-started.md)。

如需更進階的範例，請參閱 <<c0> [ 建立 ASP.NET MVC 4 應用程式的 Entity Framework 資料模型](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 請注意，您使用第一個資料庫中的資料使用 DbContext API 與您用來處理在 Code First 資料的 API 相同。 即使您想要使用第一個資料庫，您可以了解如何處理更複雜的案例，例如讀取及更新的相關的資料，處理並行衝突，Code First 的教學課程中，依此類推。 唯一的差別是在建立資料庫、 內容類別和實體類別的方式。

> [!div class="step-by-step"]
> [上一步](enhancing-data-validation.md)
