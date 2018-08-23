---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: 使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署至 IIS 作為測試環境-5，12 個 |Microsoft Docs
author: tdykstra
description: 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫，使用 Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: ba939012e8fb11a50992eeaef70e8ebf61cea851
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825812"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署至 IIS 作為測試環境-5，12 個
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 包含 SQL Server Compact 資料庫。 如果您安裝 Web Publish Update，您也可以使用 Visual Studio 2010。 在數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 顯示 Visual Studio 2012 RC 版本之後引入的部署功能，示範如何部署 SQL Server Compact，以外的 SQL Server 版本，並示範如何部署至 Azure App Service Web Apps 的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>總覽

本教學課程會示範如何部署 ASP.NET web 應用程式至 IIS 在本機電腦上。

在開發應用程式時，您通常會測試 Visual Studio 中執行。 根據預設，這表示您使用 Visual Studio 程式開發伺服器 (也稱為 Cassini)。 Visual Studio 程式開發伺服器可讓您輕鬆地在 Visual Studio 的開發期間測試，但沒有作用完全相同 IIS。 如此一來，就可以在 Visual Studio 中進行測試，但失敗時的裝載環境中將它部署至 IIS 應用程式將會正確執行。

您可以更可靠地測試您的應用程式，以下列方式：

1. 而 Visual Studio 程式開發伺服器不是使用 IIS Express 或完整的 IIS，當您在 Visual Studio 中開發期間測試。 這個方法通常會模擬更精確地說您的網站在 IIS 下的執行方式。 不過，這個方法不會測試您的部署程序或驗證部署程序的結果將會正確執行。
2. 使用相同的程序，您稍後將用來將它部署到生產環境部署至 IIS 應用程式，在您的開發電腦上。 這個方法會驗證您的應用程式會正確地在 IIS 下執行您部署程序除了驗證。
3. 部署至測試環境盡可能接近到生產環境的應用程式。 由於這些教學課程的實際執行環境是協力廠商主機服務提供者，理想的測試環境會使用主機服務提供者的第二個帳戶。 您會使用此第二個帳戶僅供測試，但它會設定與實際執行帳戶相同的方式。

本教學課程會示範選項 2 的步驟。 在結尾處提供指引，選項 3[部署到生產環境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教學課程中，並在本教學課程結尾處有資源的選項 1 的連結。

提醒： 如果您收到錯誤訊息，或當您瀏覽本教學課程，項目無法運作，請務必檢查[疑難排解頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="configuring-the-application-to-run-in-medium-trust"></a>在中度信任設定要執行的應用程式

安裝 IIS 和之前進行部署，您要變更 Web.config 檔案設定才能讓網站執行更像是會在一般的共用裝載環境中。

通常在中執行您的網站的主機服務提供者*中度信任*，這表示不允許做幾件事。 例如，應用程式程式碼無法存取 Windows 登錄和無法讀取或寫入您的應用程式資料夾階層之外的檔案。 應用程式執行的預設*更高的信任*您本機電腦上，這表示應用程式可能無法執行一些作業，當您將它部署到生產環境時將會失敗。 因此，若要讓測試環境更準確地反映實際執行環境，您將設定要在中度信任中執行的應用程式。

在應用程式 Web.config 檔案中，新增**信任**中的項目**system.web**項目，在此範例中所示。

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

現在應用程式會在 IIS 中的中度信任上執行，即使在您的本機電腦上。 此設定可讓您攔截在生產環境中將會失敗的應用程式程式碼進行的任何嘗試盡早。

> [!NOTE]
> 如果您使用 Entity Framework Code First 移轉，請確定您有 5.0 版或更新的版本。 在 Entity Framework 4.3 版中，移轉會需要完全信任，才能更新資料庫結構描述。


## <a name="installing-iis-and-web-deploy"></a>安裝 IIS 和 Web 部署

若要部署至 IIS 的開發電腦上，您必須有 IIS 和 Web Deploy 安裝。 這些都不會包含在預設的 Windows 7 設定。 如果您已安裝 IIS 和 Web Deploy，請跳至下一節。

使用[Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)是慣用的方法來安裝 IIS 和 Web Deploy，Web Platform Installer 會安裝 IIS 的建議的設定，因此它會自動安裝 IIS 和 Web 的必要條件如有必要部署。

若要執行 Web Platform Installer 來安裝 IIS 和 Web Deploy，請使用下列連結。 如果您已安裝 IIS、 Web Deploy 或任何及其必要元件，會安裝 Web Platform Installer，只會遺失。

- [安裝 IIS 和 Web Deploy 使用 WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>將預設應用程式集區設定為.NET 4

安裝 IIS 之後, 執行**IIS 管理員**先確定.NET Framework 第 4 版，都會指派給預設應用程式集區。

從 Windows**開始**功能表上，選取**執行**，輸入"inetmgr"，然後再按一下**確定**。 (如果**執行**命令不會在您**開始** 功能表中，您可以按下 Windows 鍵和 R，以開啟它。 或以滑鼠右鍵按一下工作列中，按一下**屬性**，選取**開始] 功能表**索引標籤上，按一下 [**自訂**，然後選取**執行命令**。)

在 **連線**窗格中，展開伺服器節點，然後選取**應用程式集區**。 在 **應用程式集區**窗格中，如果**DefaultAppPool**是指派給.NET framework 第 4 版如如下圖所示，請跳至下一節。

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

如果您看到只有兩個應用程式集區，這些都設為.NET Framework 2.0，您必須安裝在 IIS 中的 ASP.NET 4:

- 開啟命令提示字元視窗，以滑鼠右鍵按一下**命令提示字元**在 Windows 中**開始**功能表，然後選取**系統管理員身分執行**。 然後執行[aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx)在 IIS 中，使用下列命令安裝 ASP.NET 4。 （在 64 位元系統中，取代 「 架構 」 與 「 Framework64"）。

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    此命令為.NET Framework 4 中，建立新的應用程式集區，但預設應用程式集區仍會設為 2.0。 您將部署應用程式.NET 4 為目標的應用程式集區中，所以您不必將應用程式集區變更為.NET 4。

如果您關閉**IIS 管理員**，再執行一次，展開伺服器節點，然後按一下**應用程式集區**以顯示**應用程式集區**窗格一次。

在 **應用程式集區**窗格中，按一下**DefaultAppPool**，然後在**動作**窗格中，按一下**基本設定**。

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

在 **編輯應用程式集區** 對話方塊中，變更 **.NET Framework 版本**來 **.NET Framework v4.0.30319** ，按一下  **確定**。

[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

您現在已準備好發行至 IIS。

## <a name="publishing-to-iis"></a>發行至 IIS

有數種方式，您可以使用 Visual Studio 2010 和 Web Deploy 來部署：

- 使用單鍵發行的 Visual Studio。
- 建立*部署套件*並使用 IIS 管理員 UI 安裝它。 部署套件組成 *.zip*檔案，其中包含的所有檔案和在 IIS 中安裝站台所需的中繼資料。
- 建立部署套件，然後使用命令列進行安裝。

您已執行在先前的教學課程，以設定 Visual Studio 來自動化部署工作適用於所有這三種方法的程序。 在這些教學課程中，您將使用這些方法的第一個。 使用部署套件的相關資訊，請參閱[ASP.NET 部署內容對應](https://msdn.microsoft.com/library/bb386521.aspx)。

在發行之前，請確定您系統管理員模式中執行 Visual Studio。 (在 Windows 7**開始** 功能表中，以滑鼠右鍵按一下您要使用的 Visual Studio 版本的圖示，然後選取**系統管理員身分執行**。)需要發行只當您要發行至 IIS 在本機電腦上的系統管理員模式。

在 **方案總管**，以滑鼠右鍵按一下 ContosoUniversity 專案 （而非 ContosoUniversity.DAL 專案），然後選取**發佈**。

**發佈 Web**  精靈隨即出現。

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

在下拉式清單中，選取**&lt;新增...&gt;**.

在 [**新的設定檔**] 對話方塊中，輸入"Test"，然後按一下**確定**。

![New_Profile_dialog_box](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

這個名稱會是 [中間] 節點的 Web.Test.config 相同轉換您稍早建立的檔案。 這種對應關係是什麼情況導致 Web.Test.config 轉換，以使用此設定檔進行發行時套用。

精靈會自動移到**連線** 索引標籤。

在 **服務 URL**方塊中，輸入*localhost*。

在 **網站/應用程式**方塊中，輸入*Default Web Site/ContosoUniversity*。

在 **目的地 URL**方塊中，輸入 `http://localhost/ContosoUniversity` 。

**目的地 URL**設定不需要。 完成 Visual Studio 部署應用程式，它會自動開啟預設瀏覽器對這個 URL。 如果您不想要在部署後自動開啟瀏覽器，將此方塊保留空白。

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

按一下 **驗證連線**確認設定是否正確，而且您可以在本機電腦上連接到 IIS。

綠色的核取記號驗證連線成功。

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

按一下 [**下一步**前往**設定**] 索引標籤。

**組態**下拉式清單方塊中指定的組建組態來部署。 預設值是版本中，這是您想要。

離開**移除目的地上的其他檔案**清除核取方塊。 由於這是您第一次部署時，不會有任何檔案的目的地資料夾中尚未。

在 **資料庫**區段中，針對 連接字串 方塊中輸入下列值**SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

部署程序會將此連接字串置於已部署的 Web.config 檔案因為**在執行階段使用此連接字串**已選取。

之下，而且**SchoolContext**，選取**套用 Code First 移轉**。 這個選項會讓部署程序來設定已部署的 Web.config 檔來指定`MigrateDatabaseToLatestVersion`初始設定式。 應用程式部署之後第一次存取資料庫時，此初始設定式會將資料庫自動更新為最新版本。

在的連接字串中**DefaultConnection**，輸入下列值：

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

離開**更新資料庫**清除。 複製應用程式中的.sdf 檔案也會部署成員資格資料庫\_資料，而且您不想採取任何動作與這個資料庫的部署程序。

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

按一下 [**下一步**前往**預覽**] 索引標籤。

在  **Preview**索引標籤上，按一下**開始預覽**若要查看將複製的檔案清單。

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

按一下 [發行] 。

如果 Visual Studio 不在系統管理員模式中，您可能會收到錯誤訊息，指出權限錯誤。 在此情況下，關閉 Visual Studio，在系統管理員模式中開啟它並再次嘗試發行。

如果 Visual Studio 系統管理員模式**輸出**視窗報告成功建置並發行。

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

IIS 中執行，本機電腦上的 Contoso 大學首頁上會自動開啟瀏覽器。

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>在測試環境中測試

請注意，環境指示器會顯示 [（測試）] 而不是"(Dev)"，其中會顯示*Web.config*環境指標的轉換是否成功。

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

執行**學生**頁面，確認已部署的資料庫有任何學生。 當您選取此頁面時可能需要幾分鐘的時間載入，因為第一個程式碼，則會建立資料庫，然後再執行`Seed`方法。 （它並沒有進行，因為尚未存取資料庫沒有嘗試過的應用程式已在首頁上時。）

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

執行**講師**頁面，確認第一個程式碼植入資料庫與講師資料：

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

選取 **新增的學生**從**學生**功能表上，新增為學生，，然後檢視 在新的學生**學生**頁面，確認您可以成功地寫入至資料庫:

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

從**課程**功能表上，選取**更新信用額度**。 **更新信用額度**頁面會要求系統管理員權限，所以**登入**頁面隨即顯示。 輸入您建立舊版 （"admin"和"$w0rd Pa"） 的系統管理員帳戶認證。 **更新信用額度**顯示網頁時，會驗證您在上一個教學課程中建立的系統管理員帳戶已正確部署到測試環境。

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

確認*Elmah*資料夾有只在其中的預留位置檔案。

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>檢閱自動 Web.config 變更的 Code First 移轉

開啟*Web.config*在已部署的應用程式中的檔案*C:\inetpub\wwwroot\ContosoUniversity*和您所見，部署程序設定 Code First 移轉，自動更新為最新版本的資料庫。

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

部署程序也會建立新的連接字串，針對專門用來更新資料庫結構描述的 Code First 移轉：

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

此額外的連接字串可讓您指定的資料庫結構描述更新，一個使用者帳戶和不同的使用者帳戶存取應用程式資料。 例如，您可以將資料庫指派\_Code First 移轉，和資料庫的擁有者角色\_datareader 和 db\_應用程式的資料寫入元角色。 這是常見的深度防禦模式，以防止潛在的惡意程式碼變更資料庫結構描述應用程式。 （例如，這可能是在 SQL 資料隱碼攻擊成功。）這些教學課程不會使用此模式。 它不適用於 SQL Server Compact，並不適用於當您在稍後的教學課程中，在這一系列移轉至 SQL Server。 Cytanium 網站提供一個使用者帳戶來存取您在 Cytanium 建立 SQL Server 資料庫。 如果您可以實作此模式在您的案例中，您可以執行下列步驟來執行：

1. 在 [**設定**索引標籤**發佈 Web**精靈] 中，輸入連接字串，指定具有完整的資料庫結構描述更新權限的使用者，並清除**使用此連接字串在執行階段**核取方塊。 在已部署的 Web.config 檔案中，這會成為`DatabasePublish`連接字串。
2. 建立 Web.config 檔案轉換為您想要在執行階段使用的應用程式的連接字串。

您現在已部署至 IIS 的應用程式的開發電腦上，而且進行了測試那里。 這會確認部署程序將應用程式的內容複製到正確的位置 （不包括您不想要部署的檔案），也該 Web Deploy IIS 正確地在部署期間設定。 在下一個教學課程中，您將執行一個會尋找尚未完成的部署工作的多個測試： 設定資料夾權限*Elmah*資料夾。

## <a name="more-information"></a>更多資訊

如需在 Visual Studio 中執行 IIS 或 IIS Express 的資訊，請參閱下列資源：

- [IIS Express 概觀](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview)IIS.net 網站上。
- [簡介 IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) Scott Guthrie 的部落格上。
- [如何： 在 Visual Studio 中的 Web 專案中指定的 Web 伺服器](https://msdn.microsoft.com/library/ms178108.aspx)。
- [核心 IIS 之間的差異和 ASP.NET Development Server](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) ASP.NET 網站上。
- [在 30 秒內測試您的 ASP.NET MVC 或 Web Forms 應用程式在 IIS 7 上](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx)Rick Anderson 部落格上。 此項目會提供使用 Visual Studio 程式開發伺服器 (Cassini) 測試為何不可靠，做為測試在 IIS Express 中，和在 IIS Express 測試為何不可靠，做為測試在 IIS 中的範例。

如在中度信任執行的應用程式時，就可能發生哪些問題的相關資訊，請參閱 <<c0> [ 裝載在中度信任 ASP.NET 應用程式](http://www.4guysfromrolla.com/articles/100307-1.aspx)Rolla 站台的 4 個夥伴上。

> [!div class="step-by-step"]
> [上一頁](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
> [下一頁](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
