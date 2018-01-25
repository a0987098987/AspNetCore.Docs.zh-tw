---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: "使用 Visual Studio 的 ASP.NET Web 部署： 部署至測試 |Microsoft 文件"
author: tdykstra
description: "此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: 01f72e0240e84944f8ffece9a2dbc5802be4646b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>使用 Visual Studio 的 ASP.NET Web 部署： 部署至測試
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。 數列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。


## <a name="overview"></a>總覽

本教學課程會示範如何在 ASP.NET web 應用程式部署至 IIS 在本機電腦上。

當您開發應用程式時，您通常會測試執行 Visual Studio 中。 根據預設，Visual Studio 2012 中的 web 應用程式專案使用 IIS Express 做為程式開發 web 伺服器。 IIS Express 的行為更類似於 Visual Studio 程式開發伺服器 (也稱為 Cassini)，依預設採用 Visual Studio 2010 的完整 IIS。 但是，開發 web 伺服器的運作方式完全一樣 IIS。 如此一來，很可能您測試在 Visual Studio 中，但失敗時，它會部署到 IIS 應用程式將會正確執行。

您可以更可靠地測試您的應用程式，以下列方式：

1. 使用相同的程序，您稍後會使用將其部署到生產環境部署到 IIS 應用程式，在開發電腦上。 您可以設定 Visual Studio 來使用 IIS，當您執行 web 專案中，但這麼做可不會測試您的部署程序。 這個方法會驗證您的應用程式會正確地在 IIS 下執行您部署程序除了驗證。
2. 部署至生產環境幾乎完全相同的測試環境。 由於這些教學課程的生產環境是 Azure App Service 中的 Web 應用程式時，理想的測試環境是 Azure App Service 中建立的其他 web 應用程式。 您會使用此第二個 web 應用程式僅供測試，但它會設定為生產環境 web 應用程式的相同方式。

選項 2 最可靠的方式，來測試，而如果您這樣做，您不一定需要執行選項 1。 不過，如果您要部署到協力廠商裝載提供者選項 2 可能不可行，或可能高度耗費資源，因此這個教學課程 數列會顯示這兩種方法。 選項 2 的指南中提供[部署到生產環境](deploying-to-production.md)教學課程。

如需有關使用 Visual Studio 中的網頁伺服器的詳細資訊，請參閱[ASP.NET Web 專案的 Visual Studio 中的 Web 伺服器](https://msdn.microsoft.com/library/58wxa9w5.aspx)。

提示： 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必檢查[疑難排解頁面](troubleshooting.md)。

## <a name="install-iis"></a>安裝 IIS

若要在開發電腦部署到 IIS，您必須擁有 IIS 和 Web Deploy 安裝。 使用 Visual Studio 預設會安裝 web Deploy，但 IIS 未包含在預設的 Windows 8 或 Windows 7 中設定。 如果您已安裝 IIS，而且預設應用程式集區已設為.NET 4，請跳至[下一節](#sqlexpress)。

1. 使用[Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)是慣用的方法來安裝 IIS 和 Web Deploy，因為 Web Platform Installer 安裝 iis 建議的組態，而且它會自動安裝 IIS 和 Web 的必要條件如果需要，部署。

    若要執行 Web Platform Installer 安裝 IIS 和 Web Deploy，請使用下列連結。 如果您已安裝 IIS、 Web Deploy 或任何必要的元件，會安裝 Web Platform Installer，只會遺失。

    - [安裝 IIS 和 Web Deploy 使用 WebPI，](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

    您會看到訊息，指出將會安裝 IIS 7。 IIS 8 中在 Windows 8 中，但適用於 Windows 8，連結運作確定執行下列步驟來確認已安裝 ASP.NET 4.5:

    1. 開啟**控制台**，**程式和功能**，**開啟或關閉 Windows 功能**。
    2. 展開**Internet Information Services**， **World Wide Web 服務**，和**應用程式開發功能**。
    3. 請確定**ASP.NET 4.5**已選取。

        ![選取 ASP.NET 4.5](deploying-to-iis/_static/image1.png)

安裝 IIS 之後, 執行**IIS 管理員**，確定.NET Framework 第 4 版預設應用程式集區指派。

1. 按 WINDOWS + R，以開啟**執行** 對話方塊。

    (或 Windows 8 中輸入 「 執行 」**啟動**頁面上，或在 Windows 7 中選取**執行**從**啟動**功能表。 如果**執行**不在**啟動**功能表上，以滑鼠右鍵按一下工作列中，按一下**屬性**，選取**開始功能表**索引標籤上，按一下**自訂**，然後選取**執行命令**。)
2. 輸入 「 inetmgr"，然後按一下**確定**。
3. 在**連線** 窗格中，展開伺服器節點，然後選取**應用程式集區**。 在**應用程式集區** 窗格中，如果**DefaultAppPool**是指派給.NET framework 第 4 版圖所示，請跳至下一節。

    [![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image3.png)](deploying-to-iis/_static/image2.png)
4. 如果您看到只有兩個應用程式集區，而這兩種都設定為.NET Framework 2.0，您必須安裝在 IIS 中的 ASP.NET 4。

    適用於 Windows 8，請參閱指示，在舊版區段中針對已安裝並確定該 ASP.NET 4.5，或請參閱[這篇知識庫文章](https://support.microsoft.com/kb/2736284)。 Windows 7 中，開啟命令提示字元視窗以滑鼠右鍵按一下**命令提示字元**windows**啟動**功能表，然後選取**系統管理員身分執行**。 然後執行[aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx)在 IIS 中，使用下列命令安裝 ASP.NET 4。 （在 32 位元系統中，取代"Framework64"與"架構"）。

    [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

    此命令會建立新的應用程式集區的.NET Framework 4，但預設應用程式集區將仍會設定為 2.0。 您將部署應用程式目標為.NET 4 至該應用程式集區，因此您需要將應用程式集區變更為.NET 4。
5. 如果您關閉**IIS 管理員**、 執行一次、 展開伺服器節點，然後按一下**應用程式集區**顯示**應用程式集區** 窗格。
6. 在**應用程式集區**] 窗格中，按一下 [ **DefaultAppPool**，然後在**動作**窗格中，按一下**基本設定**。

    [![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image5.png)](deploying-to-iis/_static/image4.png)
7. 在**編輯應用程式集區**對話方塊中，變更**.NET Framework 版本**至**.NET Framework v4.0.30319**按一下**[確定]**。

    [![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image7.png)](deploying-to-iis/_static/image6.png)

IIS 現在準備好要發行的 web 應用程式，但可以這樣做之前您必須建立將使用之資料庫的測試環境中。

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>安裝 SQL Server Express

LocalDB 不被為了在 IIS 中運作，因此您需要擁有 SQL Server Express 安裝您的測試環境。 如果您使用 Visual Studio 2010 SQL Server Express 中預設已安裝。 如果您使用 Visual Studio 2012，您必須安裝它。

若要安裝 SQL Server Express，將它從安裝[Download Center: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062)按一下[ENU\x64\SQLEXPR\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLEXPR_x64_ENU.exe)或[ENU\x86\SQLEXPR\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLEXPR_x86_ENU.exe)。 如果您選擇錯誤的其中一個用於您的系統，它將無法安裝，而且您可以嘗試另一個。

在 SQL Server 安裝中心] 的第一個頁面上，按一下 [**新的 SQL Server 獨立安裝或將功能加入現有安裝**，並遵循指示，接受預設選項。 安裝精靈 中接受預設設定。 如需有關安裝選項的詳細資訊，請參閱[從安裝精靈 （安裝程式） 安裝 SQL Server 2012](https://msdn.microsoft.com/library/ms143219.aspx)。

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>建立測試環境的 SQL Server Express 資料庫

Contoso 大學應用程式有兩個資料庫： 成員資格資料庫和應用程式資料庫。 兩個不同的資料庫或單一資料庫，您可以部署這些資料庫。 您可以將它們結合為簡化資料庫應用程式資料庫和成員資格資料庫之間的聯結。 如果您要部署到協力廠商主機服務提供者，主控方案可能也會提供將它們結合的原因。 例如，主機服務提供者可能會因為多個資料庫的多個或甚至不可能會允許多個資料庫。

在本教學課程中，您將部署在測試環境中，兩個資料庫，並在預備與生產環境中的一個資料庫。

從**檢視**在 Visual Studio 中選取的功能表**伺服器總管**(**資料庫總管**在 Visual Web Developer)，然後以滑鼠右鍵按一下**資料連接**選取**建立新的 SQL Server 資料庫**。

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

在**建立新的 SQL Server 資料庫**對話方塊方塊中，輸入 「。 \SQLExpress 」 中**伺服器名稱**方塊和"aspnet-ContosoUniversity 」 中**新的資料庫名稱**方塊，然後按一下**確定**。

![建立 aspnet ContosoUniversity](deploying-to-iis/_static/image9.png)

請遵循相同的程序來建立新的 SQL Server Express School 資料庫名為"ContosoUniversity"。

**伺服器總管**現在會顯示兩個新的資料庫。

![在伺服器總管 中的新資料庫](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>建立新資料庫的授與指令碼

當您開發電腦上，在 IIS 中執行應用程式時，應用程式會使用預設應用程式集區認證存取資料庫。 不過，根據預設，應用程式集區識別沒有權限來開啟資料庫。 因此，您必須執行指令碼授與該權限。 本節中，您會建立指令碼，您將執行更新版本，才能確定應用程式在執行於 IIS 時，可以開啟資料庫。

以滑鼠右鍵按一下方案 （不是其中一個專案），然後按一下**加入新項目**，然後再建立新**SQL 檔案**名為*Grant.sql*。 將下列 SQL 命令複製到檔案，然後儲存並關閉檔案：

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

> [!NOTE]
> 此指令碼被設計用於 SQL Server Express 2012 與 Windows 8 或 Windows 7 中的 IIS 設定為它們指定本教學課程中。 如果您使用不同版本的 SQL Server 或 Windows，或如果您設定 IIS 在您的電腦上以不同的方式，可能必須使用這個指令碼的變更。 如需有關 SQL Server 指令碼的詳細資訊，請參閱[SQL Server 線上叢書 》](https://go.microsoft.com/fwlink/?LinkId=132511)。


> [!NOTE] 
> 
> **安全性注意事項**此指令碼會為 db\_使用者在執行階段，您將必須在生產環境中存取資料庫的擁有者權限。 在某些情況下，您可以指定具有完整的資料庫結構描述更新權限只部署，而且指定的執行階段不同的使用者具有權限僅讀取和寫入資料的使用者。 如需詳細資訊，請參閱[檢閱自動 Web.config 變更的程式碼 Code First 移轉](#reviewingmigrations)稍後在本教學課程。


<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>執行應用程式資料庫中授與指令碼

您可以設定執行授與指令碼的成員資格資料庫中，在部署期間因為該資料庫的部署使用 dbDacFx 提供者的發行設定檔。 您無法執行指令碼，Code First 移轉在部署期間，也就是您要部署應用程式資料庫的方式。 因此，您必須手動執行部署前指令碼應用程式資料庫中。

1. 在 Visual Studio 中開啟*Grant.sql*您稍早建立的檔案。
2. 按一下 **[Connect]**(連線)。 

    ![連接按鈕](deploying-to-iis/_static/image11.png)
3. 在**連接到伺服器**對話方塊方塊中，輸入*。 \SQLExpress*為**伺服器名稱**，然後按一下 **連接**。
4. 在 資料庫 下拉式清單中選取**ContosoUniversity**，然後按一下  **Execute**。 

    ![](deploying-to-iis/_static/image12.png)

預設應用程式集區識別現在會在 Code First 移轉應用程式執行時建立資料庫資料表的應用程式資料庫有足夠的權限。

## <a name="publish-to-iis"></a>發行至 IIS

有數種方式，您可以部署到使用 Visual Studio 和 Web Deploy 的 IIS:

- 使用單鍵發行 Visual Studio。
- 從命令列發行。
- 建立*部署套件*及使用 IIS 管理員 UI 進行安裝。 部署套件組成*.zip*檔案，其中包含所有檔案和在 IIS 中安裝站台所需的中繼資料。
- 建立部署套件，並使用命令列進行安裝。

您已在上一個教學課程中設定 Visual Studio，來自動化部署工作套用到所有這些方法的程序。 在這些教學課程中，您將使用這些方法的前兩個。 使用部署套件的相關資訊，請參閱[部署 web 應用程式建立及安裝 web 部署套件](https://go.microsoft.com/fwlink/p/?LinkId=282413#package)Visual Studio 和 ASP.NET 的 Web 部署內容對應中。

在發行之前，請確定您系統管理員模式中執行 Visual Studio。 如果您沒有看到**（系統管理員）**的標題列中，關閉 Visual Studio。 在 Windows 8**啟動**頁面或 Windows 7**啟動**功能表上，以滑鼠右鍵按一下您要使用的 Visual Studio 版本的圖示，然後選取**系統管理員身分執行**。 需要發行只有當您發行至 IIS 在本機電腦上的系統管理員模式。

### <a name="create-the-publish-profile"></a>建立發行設定檔

1. 在**方案總管 中**，以滑鼠右鍵按一下 ContosoUniversity 專案 （而非 ContosoUniversity.DAL 專案），然後選取**發行**。

    **發行 Web**精靈 隨即出現。

    ![發行網站精靈設定檔 索引標籤](deploying-to-iis/_static/image13.png)
2. 在下拉式清單中，選取**&lt;新增...&gt;**. (最新版 Visual Studio 安裝更新之後，沒有下拉式清單，並按一下 以從頭開始建立新的設定檔的按鈕**自訂**。)
3. 在**新設定檔**對話方塊中，輸入 「 測試 」，，然後按一下**確定**。

    精靈會自動移到**連接** 索引標籤。
4. 在**服務 URL**方塊中，輸入*localhost*。
5. 在**網站/應用程式**方塊中，輸入*預設網站/ContosoUniversity*
6. 在**目的地 URL**方塊中，輸入`http://localhost/ContosoUniversity`

    **目的地 URL**設定不是必要的。 當 Visual Studio 完成部署應用程式時，它會自動開啟預設的瀏覽器對這個 URL。 如果您不想要在部署後會自動開啟瀏覽器，將此方塊保留空白。
7. 按一下**驗證連線**確認設定正確無誤，而且您可以在本機電腦上連接到 IIS。

    綠色的核取記號會驗證是成功的連線。

    ![發行網站精靈連線 索引標籤](deploying-to-iis/_static/image14.png)
8. 按一下**下一步**前進到**設定** 索引標籤。
9. **組態**下拉式清單方塊指定要部署的組建組態。 將它設為發行版本的預設值。 您將不會在本教學課程部署偵錯組建。
10. 展開**檔案發行選項**，然後選取**從應用程式中排除檔案\_資料資料夾**。

    在測試環境中應用程式會存取您本機的 SQL Server Express 執行個體中建立的資料庫，不是.mdf 檔案中*應用程式\_資料*資料夾。
11. 保留**發行期間 Precompile**和**移除目的端的其他檔案**清除核取方塊。

    ![在 [設定] 索引標籤中的檔案發行選項](deploying-to-iis/_static/image15.png)

    先行編譯是選項，主要是用於非常大型的網站。它可減少第一次發行網站之後，要求頁面的頁面啟動時間。

    您不需要移除其他檔案，因為這是您第一次部署，而且不會有任何檔案的目的地資料夾中尚未。

    > [!NOTE] 
    > 
    > [!CAUTION]
    > 如果您選取**移除其他檔案**後續部署相同的站台，請確定您使用預覽功能，讓您查看事先在部署之前，將會刪除的檔案。 預期的行為是，Web Deploy 將會刪除您已刪除專案中的目的地伺服器上的檔案。 不過，比較來源和目的地資料夾底下的整個資料夾結構，而且在某些情況下在 Web Deploy 可能會刪除您不想要刪除的檔案。
    > 
    > 例如，如果您部署專案的根資料夾時，您可以有伺服器上的子資料夾中的 web 應用程式，將刪除子資料夾。 您可能必須在 contoso.com 的主要站台的單一專案並且在 contoso.com/blog 部落格內容的另一個專案。 部落格應用程式是在子資料夾。 如果您選取移除目的地的其他檔案，當您部署的主要站台時，將會刪除部落格應用程式。
    > 
    > 如需其他範例，您的應用程式\_資料資料夾可能會意外刪除。 某些資料庫，例如 SQL Server Compact 資料庫檔案儲存在應用程式\_Data 資料夾。 初始部署之後，您不想保留在後續的部署中複製資料庫檔案，讓您選取排除的應用程式\_封裝/發行 Web 索引標籤上的資料。如果您有要移除其他檔案，在 選取目的地，將資料庫檔案和應用程式之後\_資料資料夾本身將會刪除您所發佈在下一次。

### <a name="configure-deployment-for-the-membership-database"></a>設定成員資格資料庫的部署

下列步驟適用於**DefaultConnection**資料庫**資料庫**對話方塊的區段。

1. 在**遠端的連接字串**方塊中，輸入下列連接字串指向新的 SQL Server Express 的成員資格資料庫。

    [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

    部署程序將會置於這個連接字串已部署 Web.config 檔案，因為**在執行階段使用此連接字串**已選取。

    您也可以取得的連接字串**伺服器總管**。 在**伺服器總管**，依序展開**資料連接**選取 **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity**資料庫，然後從**屬性**視窗複製**連接字串**值。 連接字串將會有一個額外的設定，您可以將其刪除： `Pooling=False`。
2. 選取**更新資料庫**。

    這會導致在部署期間，目的地資料庫中建立的資料庫結構描述。 您可以在下列步驟中指定您需要執行其他指令碼： 一個，以便授與資料庫存取權的預設應用程式集區以及一到部署的資料。
3. 按一下**設定資料庫更新**。
4. 在**設定資料庫更新**對話方塊中，按一下 **加入 SQL 指令碼**然後瀏覽至*Grant.sql*您稍早儲存的方案資料夾中的指令碼。
5. 重複此程序加入*aspnet-資料-dev.sql*指令碼。

    ![設定資料庫更新成員資格資料庫](deploying-to-iis/_static/image16.png)
6. 按一下 [ **關閉**]。

### <a name="configure-deployment-for-the-application-database"></a>設定應用程式資料庫的部署

當 Visual Studio 偵測到 Entity Framework`DbContext`類別，它會建立中的項目**資料庫**區段**執行 Code First 移轉**核取方塊，而不是**更新資料庫**核取方塊。 在此教學課程中，您將使用該核取方塊來指定 Code First 移轉部署。

在某些情況下，您也可以使用`DbContext`資料庫中，但是想要將資料庫部署，而不是移轉使用 dbDacFx 提供者。 在此情況下，請參閱[如何部署 Code First 移轉沒有資料庫？](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations)在 ASP.NET Web 部署常見問題集 MSDN 上。

下列步驟適用於**SchoolContext**資料庫**資料庫**對話方塊的區段。

1. 在**遠端的連接字串**方塊中，輸入下列連接字串指向新的 SQL Server Express 應用程式資料庫。

    [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

    部署程序將會置於這個連接字串已部署 Web.config 檔案，因為**在執行階段使用此連接字串**已選取。

    您也可以取得應用程式資料庫的連接字串從**伺服器總管**相同的方式取得成員資格資料庫連接字串。
2. 選取**執行 Code First 移轉 （在應用程式啟動時執行）**。

    這個選項會讓部署程序來設定已部署的 Web.config 檔案來指定`MigrateDatabaseToLatestVersion`初始設定式。 應用程式部署後第一次存取資料庫時，此初始設定式會將資料庫自動更新為最新版本。

### <a name="configure-publish-profile-transforms"></a>設定發行設定檔的轉換

1. 按一下**關閉**，然後按一下 **是**當詢問您是否要儲存變更。
2. 在**方案總管 中**，依序展開**屬性**，依序展開**PublishProfiles**。
3. Rright 按一下*Test.pubxml，* ，然後按一下 **新增 Config 轉換**。

    ![新增 Config 轉換功能表](deploying-to-iis/_static/image17.png)

    Visual Studio 會建立*Web.Test.config*轉換檔案並開啟它。
4. 在*Web.Test.config*轉換檔案，請插入下列程式碼開頭之後立即組態標記。

    [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    當您使用測試發行設定檔時，此轉換設定"Test"的環境標記。 已部署的站台中您會看到 「 （測試） 」 之後的 「 Contoso 大學"H1 標題。
5. 儲存並關閉檔案。
6. 以滑鼠右鍵按一下*Web.Test.config*檔案，然後按一下 **預覽轉換**先確定您已編碼的轉換會產生預期的變更。

    **Web.config 預覽** 視窗會顯示結果的同時套用*Web.Release.config*轉換和*Web.Test.config*轉換。

### <a name="preview-the-deployment-updates"></a>預覽部署更新

1. 開啟**發行 Web**精靈一次 (ContosoUniversity 專案上按一下滑鼠右鍵，然後按一下**發行**)。
2. 在**預覽**索引標籤上，請確定**測試**設定檔為仍然選取，然後按一下**啟動預覽**若要查看將複製的檔案清單。

    ![[預覽] 按鈕](deploying-to-iis/_static/image18.png)

    ![發行預覽](deploying-to-iis/_static/image19.png)

    您也可以按一下**預覽資料庫**連結，查看將在成員資格資料庫中執行的指令碼。 （任何指令碼會不執行 Code First 移轉部署，因此沒有要預覽的應用程式資料庫）。
3. 按一下 [發行] 。

    如果 Visual Studio 不在系統管理員模式中，您可能會收到錯誤訊息，指出權限錯誤。 在此情況下，關閉 Visual Studio，以系統管理員模式開啟，並再次嘗試發行。

    如果 Visual Studio 是以系統管理員模式**輸出**視窗中報告成功建置並發行。

    ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

    如果您在輸入中的 URL**目的地 URL**發行設定檔 方塊**連接**索引標籤上，瀏覽器會自動開啟 Contoso 大學首頁，即可在本機電腦上執行於 IIS。

## <a name="test-in-the-test-environment"></a>在測試環境中測試

請注意，環境標記會顯示 「 （測試） 」 而不是 「 （開發） 」，其中顯示*Web.config*環境指標的轉換是否成功。

執行**講師**頁面，即可確認 Code First 植入講師資料的資料庫。 當您選取此頁面時，可能需要幾分鐘才能載入，因為第一個程式碼會建立資料庫，然後執行`Seed`方法。 （它未執行作業，當應用程式沒有存取資料庫尚未嘗試過，因為已在首頁上時）。

按一下**學生** 索引標籤，確認已部署的資料庫具有不學生。

選取**新增學生**從**學生**功能表上，新增一位學生，，然後再檢視中新的學生**學生**頁面，即可確認您可以成功地寫入至資料庫.

從**課程**功能表上，選取**更新信用額度**。 **更新信用額度**頁面需要系統管理員權限，所以**登入**頁面隨即顯示。 輸入您建立更早 （"admin"、"devpwd"） 的系統管理員帳戶認證。 **更新信用額度**頁面隨即顯示，以確認您在上一個教學課程中建立的系統管理員帳戶已正確地部署到測試環境。

確認*Elmah*資料夾存在於*c:\inetpub\wwwroot\ContosoUniversity*資料夾，並只在它的預留位置檔案。

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>檢閱自動 Web.config 變更的 Code First 移轉

開啟*Web.config*中部署的應用程式在檔案*C:\inetpub\wwwroot\ContosoUniversity* ，您可以查看其中部署程序 Code First 移轉來自動設定更新資料庫的最新版本。

![](deploying-to-iis/_static/image21.png)

部署程序也會建立新的 Code First 移轉，專門用來更新資料庫結構描述的連接字串：

![Database_Publish 連接字串](deploying-to-iis/_static/image22.png)

這個額外的連接字串可讓您指定一個使用者帳戶的資料庫結構描述更新，以及不同的使用者帳戶存取應用程式資料。 例如，您可能會指派**db\_擁有者**Code First 移轉，角色和**db\_datareader**和**db\_datawriter**應用程式的角色。 這是常見的深度防禦模式會防止應用程式無法變更資料庫結構描述中的潛在惡意程式碼。 （例如，這可能會發生在成功的 SQL 資料隱碼攻擊。）此模式不會使用這些教學課程。 如果您想要實作此模式在您的案例中，您可以執行下列步驟來執行它：

1. 在**設定** 索引標籤**發行 Web**精靈 中，輸入完整的資料庫結構描述更新權限與指定的使用者的連接字串，並清除**使用此連接字串在執行階段**核取方塊。 在已部署 Web.config 檔案中，這會變成`DatabasePublish`連接字串。
2. 建立的連接字串，您想要在執行階段使用的應用程式的 Web.config 檔案轉換。

## <a name="summary"></a>總結

您現在已在開發電腦部署到 IIS 應用程式，而且那里進行測試。

![在測試中的 [首頁] 頁面](deploying-to-iis/_static/image23.png)

這會確認部署程序複製到正確的位置 （不含您不想要部署的檔案），應用程式的內容，也該 Web Deploy IIS 正確設定在部署期間。 在下一個教學課程中，您將執行一項會尋找尚未完成的部署工作的多個測試： 設定資料夾的權限*Elmah*資料夾。

## <a name="more-information"></a>詳細資訊

如需 Visual Studio 中執行 IIS 或 IIS Express，請參閱下列資源：

- [IIS Express 概觀](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview)IIS.net 網站上。
- [導入 IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) Scott Guthrie 的部落格上。
- [Web 伺服器，在 Visual Studio 中的，ASP.NET Web 專案](https://msdn.microsoft.com/library/58wxa9w5.aspx)。
- [核心 IIS 之間的差異和 ASP.NET 程式開發伺服器](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md)ASP.NET 網站上。

在中度信任執行的應用程式時，可能會引發哪些問題的相關資訊，請參閱[裝載在中度信任 ASP.NET 應用程式](http://www.4guysfromrolla.com/articles/100307-1.aspx)4 Guy 從 Rolla 站台上。

>[!div class="step-by-step"]
[上一頁](project-properties.md)
[下一頁](setting-folder-permissions.md)
