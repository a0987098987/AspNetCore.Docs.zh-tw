---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: 使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署至 IIS 做為測試環境-12 個 5 |Microsoft 文件
author: tdykstra
description: 這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫使用視覺化 Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: 16050455c161c8ced1f954bfce9c2d9a44c522b4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889666"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署至 IIS 做為測試環境-5 的 12
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 的 SQL Server Compact 資料庫。 如果您安裝 Web 發行更新，您也可以使用 Visual Studio 2010。 數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 顯示部署 Visual Studio 2012 RC 發行之後，引進的功能，示範如何將 SQL Server Compact 以外的 SQL Server 版本的部署和示範如何將部署至 Azure App Service Web 應用程式的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>總覽

本教學課程會示範如何在 ASP.NET web 應用程式部署至 IIS 在本機電腦上。

當您開發應用程式時，您通常會測試執行 Visual Studio 中。 根據預設，這表示您使用 Visual Studio 程式開發伺服器 (也稱為 Cassini)。 Visual Studio 程式開發伺服器可讓您輕鬆地在 Visual Studio 中，在開發期間進行測試，但它不能與 IIS 完全相同。 如此一來，很可能您測試在 Visual Studio 中，但失敗時的裝載環境中將它部署到 IIS 應用程式將會正確執行。

您可以更可靠地測試您的應用程式，以下列方式：

1. 使用 IIS Express 或完整的 IIS Visual Studio 程式開發伺服器時，代替您測試在 Visual Studio 在開發期間。 這個方法通常會模擬更精確地您的網站在 IIS 底下的執行方式。 不過，這個方法不會測試您的部署程序或驗證的部署程序的結果將會正確執行。
2. 使用相同的程序，您稍後會使用將其部署到生產環境部署到 IIS 應用程式，在開發電腦上。 這個方法會驗證您的應用程式會正確地在 IIS 下執行您部署程序除了驗證。
3. 部署應用程式是盡可能接近實際執行環境的測試環境。 由於這些教學課程的生產環境是協力廠商主機服務提供者，理想的測試環境會是具有主機服務提供者的第二個帳戶。 您會使用這個第二個帳戶僅供測試，但它會做為實際帳戶的相同方式設定。

本教學課程示範選項 2 的步驟。 在結尾處提供的選項 3 的指導方針[部署到生產環境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教學課程中，而且在本教學課程結尾處有資源的選項 1 的連結。

提示： 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必檢查[疑難排解頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="configuring-the-application-to-run-in-medium-trust"></a>在中度信任中設定要執行的應用程式

安裝 IIS 和之前部署至它，您要使站台執行更像在一般的共用裝載環境中會變更 Web.config 檔案設定。

主機服務提供者通常執行您的網站*中度信任*，這表示不允許執行幾個步驟。 例如，應用程式程式碼無法存取 Windows 登錄，無法讀取或寫入應用程式的資料夾階層之外的檔案。 根據預設應用程式執行的*高信任度*您本機電腦上，這表示應用程式可能可以執行一些作業，當您部署到生產環境時將會失敗。 因此，若要讓測試環境更準確地反映實際執行環境，您會設定要在中度信任中執行的應用程式。

在應用程式 Web.config 檔案中，加入**信任**中的項目**system.web**項目，如這個範例所示。

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

在 IIS 中的中度信任現在將會執行應用程式即使在本機電腦上。 這項設定可讓您在生產環境中將會失敗的應用程式程式碼以做到盡早攔截的任何嘗試行為。

> [!NOTE]
> 如果您使用 Entity Framework Code First 移轉，請確定您有 5.0 版或更新的版本。 在 Entity Framework 4.3 版中，移轉需要完全信任，才能更新資料庫結構描述。


## <a name="installing-iis-and-web-deploy"></a>安裝 IIS 和 Web Deploy

若要在開發電腦部署到 IIS，您必須擁有 IIS 和 Web Deploy 安裝。 預設的 Windows 7 組態不包含這些。 如果您已安裝 IIS 和 Web Deploy，請跳至下一節。

使用[Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)是慣用的方法來安裝 IIS 和 Web Deploy，因為 Web Platform Installer 安裝 iis 建議的組態，而且它會自動安裝 IIS 和 Web 的必要條件如果需要，部署。

若要執行 Web Platform Installer 安裝 IIS 和 Web Deploy，請使用下列連結。 如果您已安裝 IIS、 Web Deploy 或任何必要的元件，會安裝 Web Platform Installer，只會遺失。

- [安裝 IIS 和 Web Deploy 使用 WebPI，](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>將預設應用程式集區設定為.NET 4

安裝 IIS 之後, 執行**IIS 管理員**，確定.NET Framework 第 4 版預設應用程式集區指派。

從 Windows**啟動**功能表上，選取**執行**，輸入 「 inetmgr"，然後按一下**確定**。 (如果**執行**命令不會在您**啟動**功能表上，您可以按 Windows 鍵和 R，以開啟它。 或以滑鼠右鍵按一下工作列，請按一下**屬性**，選取**開始功能表**索引標籤上，按一下 **自訂**，然後選取**執行命令**。)

在**連線** 窗格中，展開伺服器節點，然後選取**應用程式集區**。 在**應用程式集區** 窗格中，如果**DefaultAppPool**是指派給.NET framework 第 4 版圖所示，請跳至下一節。

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

如果您看到只有兩個應用程式集區，而這兩種都設定為.NET Framework 2.0，您必須安裝在 IIS 中的 ASP.NET 4:

- 開啟命令提示字元視窗，以滑鼠右鍵按一下**命令提示字元**windows**啟動**功能表，然後選取**系統管理員身分執行**。 然後執行[aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx)在 IIS 中，使用下列命令安裝 ASP.NET 4。 （在 64 位元系統中，取代"架構"與"Framework64"）。

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    此命令會建立新的應用程式集區的.NET Framework 4，但預設應用程式集區將仍會設定為 2.0。 您將部署應用程式目標為.NET 4 至該應用程式集區，因此您需要將應用程式集區變更為.NET 4。

如果您關閉**IIS 管理員**、 執行一次、 展開伺服器節點，然後按一下**應用程式集區**顯示**應用程式集區** 窗格。

在**應用程式集區**] 窗格中，按一下 [ **DefaultAppPool**，然後在**動作**窗格中，按一下**基本設定**。

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

在**編輯應用程式集區**對話方塊中，變更 **.NET Framework 版本**至 **.NET Framework v4.0.30319**按一下 **[確定]**。

[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

現在您已經準備好要發行至 IIS。

## <a name="publishing-to-iis"></a>發行至 IIS

有數種方式，您可以使用 Visual Studio 2010 和 Web Deploy 來部署：

- 使用單鍵發行 Visual Studio。
- 建立*部署套件*及使用 IIS 管理員 UI 進行安裝。 部署套件組成 *.zip*檔案，其中包含所有檔案和在 IIS 中安裝站台所需的中繼資料。
- 建立部署套件，並使用命令列進行安裝。

您已在上一個教學課程中設定 Visual Studio，來自動化部署工作套用到所有這三個方法的程序。 在這些教學課程中，您將使用這些方法的第一個。 使用部署套件的相關資訊，請參閱[ASP.NET 部署內容地圖](https://msdn.microsoft.com/library/bb386521.aspx)。

在發行之前，請確定您系統管理員模式中執行 Visual Studio。 (在 Windows 7**啟動**功能表上，以滑鼠右鍵按一下您要使用的 Visual Studio 版本的圖示，然後選取**系統管理員身分執行**。)需要發行只有當您發行至 IIS 在本機電腦上的系統管理員模式。

在**方案總管 中**，以滑鼠右鍵按一下 ContosoUniversity 專案 （而非 ContosoUniversity.DAL 專案），然後選取**發行**。

**發行 Web**精靈 隨即出現。

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

在下拉式清單中，選取**&lt;新增...&gt;**.

在**新設定檔**對話方塊中，輸入 「 測試 」，，然後按一下**確定**。

![New_Profile_dialog_box](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

這個名稱是 Web.Test.config 中間節點相同轉換您稍早建立的檔案。 此對應是什麼情況會讓您使用此設定檔來發佈時要套用的 Web.Test.config 轉換。

精靈會自動移到**連接** 索引標籤。

在**服務 URL**方塊中，輸入*localhost*。

在**網站/應用程式**方塊中，輸入*預設網站/ContosoUniversity*。

在**目的地 URL**方塊中，輸入`http://localhost/ContosoUniversity`。

**目的地 URL**設定不是必要的。 當 Visual Studio 完成部署應用程式時，它會自動開啟預設的瀏覽器對這個 URL。 如果您不想要在部署後會自動開啟瀏覽器，將此方塊保留空白。

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

按一下**驗證連線**確認設定正確無誤，而且您可以在本機電腦上連接到 IIS。

綠色的核取記號會驗證是成功的連線。

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

按一下**下一步**前進到**設定** 索引標籤。

**組態**下拉式清單方塊指定要部署的組建組態。 預設值是版本中，這是您想要。

保留**移除目的端的其他檔案**清除核取方塊。 由於這是您第一次部署時，將不會有任何檔案的目的地資料夾中尚未。

在**資料庫**區段中，輸入下列值中的連接字串方塊**SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

部署程序將會置於這個連接字串已部署 Web.config 檔案，因為**在執行階段使用此連接字串**已選取。

也在**SchoolContext**，選取**套用 Code First 移轉**。 這個選項會讓部署程序來設定已部署的 Web.config 檔案來指定`MigrateDatabaseToLatestVersion`初始設定式。 應用程式部署後第一次存取資料庫時，此初始設定式會將資料庫自動更新為最新版本。

在連接字串 方塊的**DefaultConnection**，輸入下列值：

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

保留**更新資料庫**清除。 將部署成員資格資料庫，藉由複製應用程式中的.sdf 檔案\_資料，且您不想執行任何動作與這個資料庫的部署程序。

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

按一下**下一步**前進到**預覽** 索引標籤。

在**預覽**索引標籤上，按一下 **啟動預覽**若要查看將複製的檔案清單。

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

按一下 [發行] 。

如果 Visual Studio 不在系統管理員模式中，您可能會收到錯誤訊息，指出權限錯誤。 在此情況下，關閉 Visual Studio，以系統管理員模式開啟，並再次嘗試發行。

如果 Visual Studio 是以系統管理員模式**輸出**視窗中報告成功建置並發行。

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

瀏覽器會自動開啟 Contoso 大學首頁，即可在本機電腦上執行於 IIS。

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>在測試環境中測試

請注意，環境標記會顯示 「 （測試） 」 而不是 「 （開發） 」，其中顯示*Web.config*環境指標的轉換是否成功。

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

執行**學生**頁面，確認已部署的資料庫已經沒有學生。 當您選取此頁面時可能需要幾分鐘才能載入，因為第一個程式碼會建立資料庫，然後執行`Seed`方法。 （它未執行作業，當應用程式沒有存取資料庫尚未嘗試過，因為已在首頁上時）。

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

執行**講師**頁面，即可確認 Code First 植入講師資料的資料庫：

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

選取**新增學生**從**學生**功能表上，新增一位學生，，然後再檢視中新的學生**學生**頁面，即可確認您可以成功地寫入至資料庫:

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

從**課程**功能表上，選取**更新信用額度**。 **更新信用額度**頁面需要系統管理員權限，所以**登入**頁面隨即顯示。 輸入您建立更早 （"admin"、"Pa$ w0rd"） 的系統管理員帳戶認證。 **更新信用額度**頁面隨即顯示，以確認您在上一個教學課程中建立的系統管理員帳戶已正確地部署到測試環境。

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

確認*Elmah*資料夾有只在它的預留位置檔案。

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>檢閱自動 Web.config 變更的程式碼 Code First 移轉

開啟*Web.config*中部署的應用程式在檔案*C:\inetpub\wwwroot\ContosoUniversity* ，您可以查看其中部署程序 Code First 移轉來自動設定更新資料庫的最新版本。

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

部署程序也會建立新的 Code First 移轉，專門用來更新資料庫結構描述的連接字串：

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

這個額外的連接字串可讓您指定一個使用者帳戶的資料庫結構描述更新，以及不同的使用者帳戶存取應用程式資料。 例如，您可能會指派 db\_Code First 移轉，和資料庫的擁有者角色\_datareader 和 db\_datawriter 應用程式的角色。 這是常見的深度防禦模式會防止應用程式無法變更資料庫結構描述中的潛在惡意程式碼。 （例如，這可能會發生在成功的 SQL 資料隱碼攻擊。）此模式不會使用這些教學課程。 它不適用於 SQL Server Compact，並不會套用在稍後的教學課程中，在這一系列將移轉至 SQL Server 時。 Cytanium 網站提供一個使用者帳戶來存取您在 Cytanium 建立 SQL Server 資料庫。 如果您能夠在您的案例中實作此模式，您可以執行下列步驟來執行它：

1. 在**設定** 索引標籤**發行 Web**精靈 中，輸入完整的資料庫結構描述更新權限與指定的使用者的連接字串，並清除**使用此連接字串在執行階段**核取方塊。 在已部署 Web.config 檔案中，這會變成`DatabasePublish`連接字串。
2. 建立的連接字串，您想要在執行階段使用的應用程式的 Web.config 檔案轉換。

您現在已在開發電腦部署到 IIS 應用程式，而且那里進行測試。 這會確認部署程序複製到正確的位置 （不含您不想要部署的檔案），應用程式的內容，也該 Web Deploy IIS 正確設定在部署期間。 在下一個教學課程中，您將執行一項會尋找尚未完成的部署工作的多個測試： 設定資料夾的權限*Elmah*資料夾。

## <a name="more-information"></a>更多資訊

如需 Visual Studio 中執行 IIS 或 IIS Express，請參閱下列資源：

- [IIS Express 概觀](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview)IIS.net 網站上。
- [導入 IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) Scott Guthrie 的部落格上。
- [如何： 在 Visual Studio 中的 Web 專案指定的 Web 伺服器](https://msdn.microsoft.com/library/ms178108.aspx)。
- [核心 IIS 之間的差異和 ASP.NET 程式開發伺服器](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md)ASP.NET 網站上。
- [在 30 秒內測試 ASP.NET MVC 或 Web Form 應用程式在 IIS 7 上](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx)Rick Anderson 部落格上。 此項目提供的範例使用 Visual Studio 程式開發伺服器 (Cassini) 來測試為何不可靠，做為測試在 IIS Express 中，以及在 IIS Express 測試為何無法做為測試在 IIS 中的可靠性。

在中度信任執行的應用程式時，可能會引發哪些問題的相關資訊，請參閱[裝載在中度信任 ASP.NET 應用程式](http://www.4guysfromrolla.com/articles/100307-1.aspx)4 Guy 從 Rolla 站台上。

> [!div class="step-by-step"]
> [上一頁](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
> [下一頁](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
