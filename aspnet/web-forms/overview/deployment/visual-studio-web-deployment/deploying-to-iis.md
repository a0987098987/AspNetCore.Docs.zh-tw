---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 使用 Visual Studio 的 ASP.NET Web 部署： 部署到測試 |Microsoft Docs
author: tdykstra
description: 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: 8c5034dd4948d96c5722e2dcc960cc0349241a1a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365681"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>使用 Visual Studio 的 ASP.NET Web 部署： 部署至測試
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。 這個系列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。


## <a name="overview"></a>總覽

本教學課程會示範如何部署 ASP.NET web 應用程式至 IIS 在本機電腦上。

在開發應用程式時，您通常會測試 Visual Studio 中執行。 根據預設，Visual Studio 2012 中的 web 應用程式專案使用 IIS Express 做為開發 web 伺服器。 IIS Express 的行為更類似於 Visual Studio 程式開發伺服器 (也稱為 Cassini)，依預設採用 Visual Studio 2010 的完整 IIS。 但是，開發 web 伺服器的運作方式完全類似 IIS。 如此一來，就可以在 Visual Studio 中進行測試，但失敗時將它部署至 IIS 應用程式將會正確執行。

您可以更可靠地測試您的應用程式，以下列方式：

1. 使用相同的程序，您稍後將用來將它部署到生產環境部署至 IIS 應用程式，在您的開發電腦上。 您可以設定 Visual Studio 使用 IIS，當您執行 web 專案，但這麼做，就不會測試您的部署程序。 這個方法會驗證您的應用程式會正確地在 IIS 下執行您部署程序除了驗證。
2. 部署應用程式幾乎等同於生產環境的測試環境。 由於這些教學課程的實際執行環境是 Azure App Service 中的 Web 應用程式，最理想的測試環境是 Azure App Service 中建立的其他 web 應用程式。 您會使用此第二個 web 應用程式僅供測試，但它會設定為生產 web 應用程式相同的方式。

選項 2 最可靠的方式，若要測試，而且如果您這麼做，您不一定要執行的選項 1。 不過，如果您要部署的協力廠商裝載提供者選項 2 可能不可行，或可能耗費資源，因此本教學課程系列會顯示這兩種方法。 選項 2 的指導方針中提供[部署到生產環境](deploying-to-production.md)教學課程。

如需在 Visual Studio 中使用 web 伺服器的詳細資訊，請參閱[ASP.NET Web 專案的 Visual Studio 中的 Web 伺服器](https://msdn.microsoft.com/library/58wxa9w5.aspx)。

提醒： 如果您收到錯誤訊息，或當您瀏覽本教學課程，項目無法運作，請務必檢查[疑難排解頁面](troubleshooting.md)。

## <a name="install-iis"></a>安裝 IIS

若要部署至 IIS 的開發電腦上，您必須有 IIS 和 Web Deploy 安裝。 使用 Visual Studio 中，預設會安裝 web Deploy，但 IIS 不會包含在預設的 Windows 8 或 Windows 7 中設定。 如果您已安裝 IIS 和預設應用程式集區已設為.NET 4，請跳至[下一節](#sqlexpress)。

1. 使用[Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)是慣用的方法來安裝 IIS 和 Web Deploy，Web Platform Installer 會安裝 IIS 的建議的設定，因此它會自動安裝 IIS 和 Web 的必要條件如有必要部署。

    若要執行 Web Platform Installer 來安裝 IIS 和 Web Deploy，請使用下列連結。 如果您已安裝 IIS、 Web Deploy 或任何及其必要元件，會安裝 Web Platform Installer，只會遺失。

   - [安裝 IIS 和 Web Deploy 使用 WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

     您會看到訊息指出將會安裝 IIS 7。 適用於 Windows 8 中的 IIS 8，但適用於 Windows 8，連結運作確定安裝 ASP.NET 4.5 時，會藉由執行下列步驟：

   - 開啟**控制台中**，**程式和功能**，**開啟或關閉開啟的 Windows 功能**。
   - 依序展開**Internet Information Services**， **World Wide Web 服務**，並**應用程式開發功能**。
   - 請確定**ASP.NET 4.5**已選取。

      ![選取 ASP.NET 4.5](deploying-to-iis/_static/image1.png)

安裝 IIS 之後, 執行**IIS 管理員**先確定.NET Framework 第 4 版，都會指派給預設應用程式集區。

1. 按 WINDOWS + R 來開啟**執行** 對話方塊。

    (或 Windows 8 中輸入 「 執行 」**開始**頁面上，或在 Windows 7 中選取**執行**從**啟動**功能表。 如果**執行**不在**開始**功能表上，以滑鼠右鍵按一下工作列，請按一下**屬性**，選取**開始 功能表**索引標籤上，按一下**自訂**，然後選取**執行命令**。)
2. 輸入"inetmgr"，然後再按一下**確定**。
3. 在 **連線**窗格中，展開伺服器節點，然後選取**應用程式集區**。 在 **應用程式集區**窗格中，如果**DefaultAppPool**是指派給.NET framework 第 4 版如如下圖所示，請跳至下一節。

    [![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image3.png)](deploying-to-iis/_static/image2.png)
4. 如果您看到只有兩個應用程式集區，這些都設為.NET Framework 2.0，您必須安裝在 IIS 中的 ASP.NET 4。

    適用於 Windows 8，請參閱指示，在上一個區段，並確定該 ASP.NET 4.5 已安裝，或請參閱[這篇知識庫文章](https://support.microsoft.com/kb/2736284)。 適用於 Windows 7，請以滑鼠右鍵按一下開啟命令提示字元視窗**命令提示字元**在 Windows 中**開始**功能表，然後選取**系統管理員身分執行**。 然後執行[aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx)在 IIS 中，使用下列命令安裝 ASP.NET 4。 （在 32 位元系統中，取代"Framework64 」 與 「 架構 」）。

    [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

    此命令為.NET Framework 4 中，建立新的應用程式集區，但預設應用程式集區仍會設為 2.0。 您將部署應用程式.NET 4 為目標的應用程式集區中，所以您不必將應用程式集區變更為.NET 4。
5. 如果您關閉**IIS 管理員**，再執行一次，展開伺服器節點，然後按一下**應用程式集區**以顯示**應用程式集區**窗格一次。
6. 在 **應用程式集區**窗格中，按一下**DefaultAppPool**，然後在**動作**窗格中，按一下**基本設定**。

    [![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image5.png)](deploying-to-iis/_static/image4.png)
7. 在 **編輯應用程式集區** 對話方塊中，變更 **.NET Framework 版本**來 **.NET Framework v4.0.30319** ，按一下  **確定**。

    [![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image7.png)](deploying-to-iis/_static/image6.png)

IIS 現在已準備好要發行 web 應用程式，但可以這麼做之前您必須在測試環境中建立您將使用的資料庫。

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>安裝 SQL Server Express

LocalDB 的目的不是用於在 IIS 中，因此您必須先安裝 SQL Server Express 為您的測試環境。 如果您使用 Visual 的 Studio 2010 SQL Server Express 預設已安裝。 如果您使用 Visual Studio 2012，您必須安裝它。

若要安裝 SQL Server Express，將它從安裝[Download Center: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062)依序按一下[ENU\x64\SQLEXPR\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLEXPR_x64_ENU.exe)或[ENU\x86\SQLEXPR\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLEXPR_x86_ENU.exe)。 如果您選擇了錯誤，它會為您的系統無法安裝，而且您可以嘗試另一個。

在 SQL Server 安裝中心 的第一個頁面上，按一下**新的 SQL Server 獨立安裝或將功能加入到現有安裝**，並遵循指示接受預設選項。 在 [安裝精靈] 中，接受預設設定。 如需安裝選項的詳細資訊，請參閱[從安裝精靈 （安裝程式） 安裝 SQL Server 2012](https://msdn.microsoft.com/library/ms143219.aspx)。

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>建立測試環境的 SQL Server Express 資料庫

Contoso 大學應用程式有兩個資料庫： 成員資格資料庫和應用程式資料庫。 兩個不同的資料庫或單一資料庫，您可以部署這些資料庫。 您可以將它們結合為簡化您的應用程式資料庫和成員資格資料庫之間的資料庫聯結。 如果您要部署到協力廠商主機服務提供者，您的主控方案可能也會提供將它們結合的原因。 例如，主機服務提供者可能會因為多個資料庫的多個或甚至不可能會允許多個資料庫。

在本教學課程中，您會將部署兩個資料庫，在測試環境中，並在預備與生產環境中的一個資料庫。

從**檢視**功能表，在 Visual Studio 中，選取**伺服器總管**(**資料庫總管**在 Visual Web Developer)，然後以滑鼠右鍵按一下**資料連接** ，然後選取**建立新的 SQL Server 資料庫**。

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

在**建立新的 SQL Server 資料庫**對話方塊方塊中，輸入 「。 \SQLExpress 」 中**伺服器名稱**] 方塊中，"aspnet 集 ContosoUniversity"中**新的資料庫名稱**方塊，然後按一下 [**確定**。

![建立 aspnet ContosoUniversity](deploying-to-iis/_static/image9.png)

請遵循相同的程序，建立名為"ContosoUniversity"的新 SQL Server Express School 資料庫。

**伺服器總管**現在會顯示兩個新的資料庫。

![在 [伺服器總管] 中的新資料庫](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>建立新的資料庫授與指令碼

當您開發電腦上，在 IIS 中執行應用程式時，應用程式會使用預設應用程式集區的認證存取資料庫。 不過，根據預設，應用程式集區身分識別沒有權限來開啟資料庫。 因此，您必須執行的指令碼，以授與該權限。 在本節中，您會建立指令碼，您將會執行更新版本，以確定應用程式在執行於 IIS 時，可以開啟資料庫。

以滑鼠右鍵按一下方案 （不是其中一個專案），然後按一下 **加入新項目**，然後再建立 新**SQL 檔案**名為*Grant.sql*。 將下列 SQL 命令複製到檔案，然後儲存並關閉檔案：

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

> [!NOTE]
> 此指令碼可配合搭配 SQL Server Express 2012 與 Windows 8 或 Windows 7 中的 IIS 設定，因為它們會指定在本教學課程。 如果您使用不同版本的 SQL Server 或 Windows，或如果您將 IIS 設定您的電腦上以不同的方式，可能必須使用這個指令碼的變更。 如需有關 SQL Server 指令碼的詳細資訊，請參閱 < [SQL Server 線上叢書 》](https://go.microsoft.com/fwlink/?LinkId=132511)。


> [!NOTE] 
> 
> **安全性注意事項**此指令碼會為 db\_使用者在執行階段，也就是您會有在生產環境中存取資料庫的擁有者權限。 在某些情況下，您可能想要指定具有完整的資料庫結構描述更新的權限只部署，並指定執行階段不同的使用者具有權限才能讀取和寫入資料的使用者。 如需詳細資訊，請參閱 <<c0> [ 檢閱 Code First 移轉自動 Web.config 變更](#reviewingmigrations)稍後在本教學課程。


<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>執行應用程式資料庫中的 授與指令碼

您可以設定執行指令碼授與成員資格資料庫中，在部署期間因為該資料庫的部署使用的 dbDacFx 提供者的發行設定檔。 您無法執行指令碼，Code First 移轉在部署期間，也就是您要部署的應用程式資料庫的方式。 因此，您必須以手動方式執行應用程式資料庫中的 部署前的指令碼。

1. 在 Visual Studio 中開啟*Grant.sql*您稍早建立的檔案。
2. 按一下 **[Connect]**(連線)。 

    ![[連線] 按鈕](deploying-to-iis/_static/image11.png)
3. 在**連接到伺服器**對話方塊方塊中，輸入 *。 \SQLExpress*作為**伺服器名稱**，然後按一下**Connect**。
4. 在 [資料庫] 下拉式清單中選取**ContosoUniversity**，然後按一下**Execute**。 

    ![](deploying-to-iis/_static/image12.png)

預設應用程式集區識別現在具有足夠的權限，在 Code First 移轉來建立資料庫資料表，當應用程式的應用程式資料庫中。

## <a name="publish-to-iis"></a>發行至 IIS

有數種方式，您可以部署到使用 Visual Studio 和 Web Deploy 的 IIS:

- 使用單鍵發行的 Visual Studio。
- 從命令列發佈。
- 建立*部署套件*並使用 IIS 管理員 UI 安裝它。 部署套件組成 *.zip*檔案，其中包含的所有檔案和在 IIS 中安裝站台所需的中繼資料。
- 建立部署套件，然後使用命令列進行安裝。

您已執行在先前的教學課程，以設定 Visual Studio 來自動化部署工作適用於所有這些方法的程序。 在這些教學課程中，您將使用這些方法的前兩個。 使用部署套件的相關資訊，請參閱[部署 web 應用程式建立及安裝 web 部署套件](https://go.microsoft.com/fwlink/p/?LinkId=282413#package)Visual Studio 及 ASP.NET 的 Web 部署內容對應中。

在發行之前，請確定您系統管理員模式中執行 Visual Studio。 如果您沒有看到 **（系統管理員）** 在標題列中，關閉 Visual Studio。 在 Windows 8**開始**網頁或 Windows 7**開始**功能表上，以滑鼠右鍵按一下您要使用的 Visual Studio 版本的圖示，然後選取**系統管理員身分執行**。 需要發行只當您要發行至 IIS 在本機電腦上的系統管理員模式。

### <a name="create-the-publish-profile"></a>建立發行設定檔

1. 在 **方案總管**，以滑鼠右鍵按一下 ContosoUniversity 專案 （而非 ContosoUniversity.DAL 專案），然後選取**發佈**。

    **發佈 Web**  精靈隨即出現。

    ![發佈 Web 精靈設定檔 索引標籤](deploying-to-iis/_static/image13.png)
2. 在下拉式清單中，選取**&lt;新增...&gt;**. (最新 Visual Studio 安裝更新之後，任何下拉式清單中，且  按鈕，按一下 若要從頭開始建立新的設定檔**自訂**。)
3. 在 [**新的設定檔**] 對話方塊中，輸入"Test"，然後按一下**確定**。

    精靈會自動移到**連線** 索引標籤。
4. 在 **服務 URL**方塊中，輸入*localhost*。
5. 在 **網站/應用程式**方塊中，輸入*Default Web Site/ContosoUniversity*
6. 在 **目的地 URL**方塊中，輸入 `http://localhost/ContosoUniversity`

    **目的地 URL**設定不需要。 完成 Visual Studio 部署應用程式，它會自動開啟預設瀏覽器對這個 URL。 如果您不想要在部署後自動開啟瀏覽器，將此方塊保留空白。
7. 按一下 **驗證連線**確認設定是否正確，而且您可以在本機電腦上連接到 IIS。

    綠色的核取記號驗證連線成功。

    ![發佈 Web 精靈連線 索引標籤](deploying-to-iis/_static/image14.png)
8. 按一下 [**下一步**前往**設定**] 索引標籤。
9. **組態**下拉式清單方塊中指定的組建組態來部署。 將它設為發行版本的預設值。 您不會在本教學課程部署偵錯組建。
10. 依序展開**檔案發佈選項**，然後選取**從應用程式中排除檔案\_Data 資料夾**。

    在測試環境中應用程式會存取您在本機的 SQL Server Express 執行個體中建立的資料庫中的檔案不是.mdf*應用程式\_資料*資料夾。
11. 離開**發行期間預先編譯**並**移除目的地上的其他檔案**清除核取方塊。

    ![在 設定 索引標籤的 檔案發行選項](deploying-to-iis/_static/image15.png)

    先行編譯是主要用於非常大型的網站; 此選項它可以減少頁面要求頁面時發行網站之後的第一次的啟動時間。

    您不需要移除其他檔案，因為這是您第一次部署，而且不會有任何檔案的目的地資料夾中尚未。

    > [!NOTE] 
    > 
    > [!CAUTION]
    > 如果您選取**移除其他檔案**後續部署到相同的站台，請確定您使用的是預覽功能，以便您看到事先將刪除的檔案，然後再部署。 預期的行為是，Web Deploy 將會刪除您已刪除您的專案中的目的地伺服器上的檔案。 不過，比較來源和目的地資料夾下的整個資料夾結構，而且在某些情況下在 Web 部署可能會刪除您不想要刪除的檔案。
    > 
    > 例如，如果您部署專案的根資料夾時，您可以有在伺服器上的子資料夾中的 web 應用程式，將刪除子資料夾。 您可能必須在 contoso.com 的主要站台的一個專案和 contoso.com/blog 的部落格的另一個專案。 部落格應用程式是在子資料夾。 如果您選取移除目的地上的其他檔案，當您部署的主要站台時，將會刪除部落格應用程式。
    > 
    > 如需其他範例，您的應用程式\_資料資料夾可能會意外刪除。 特定的資料庫，例如 SQL Server Compact 資料庫檔案儲存在應用程式\_Data 資料夾。 初始部署之後，您不想讓後續的部署中複製資料庫檔案，讓您選取排除的應用程式\_封裝/發行 Web 索引標籤上的資料。您這麼做，如果您沒有移除其他檔案，在 選取目的地，您的資料庫檔案和應用程式之後\_資料資料夾本身將會刪除您所發行下一次。

### <a name="configure-deployment-for-the-membership-database"></a>設定成員資格資料庫的部署

下列步驟適用於**DefaultConnection**資料庫中**資料庫**對話方塊的區段。

1. 在 **遠端的連接字串**方塊中，輸入下列連接字串指向新的 SQL Server Express 成員資格資料庫。

    [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

    部署程序會將此連接字串置於已部署的 Web.config 檔案因為**在執行階段使用此連接字串**已選取。

    您也可以取得的連接字串**伺服器總管**。 在 [**伺服器總管**，展開**資料連接**，然後選取 **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity**資料庫，然後從**屬性**] 視窗複製**連接字串**值。 連接字串會有一個額外的設定，您可以將其刪除： `Pooling=False`。
2. 選取 **更新資料庫**。

    這會導致在部署期間，目的地資料庫中建立的資料庫結構描述。 您可以在下列步驟中指定您需要執行其他指令碼： 一個用來授與資料庫存取權的預設應用程式集區，另一個用於部署的資料。
3. 按一下 **設定資料庫更新**。
4. 在 [**設定資料庫更新**] 對話方塊中，按一下**新增 SQL 指令碼**然後瀏覽至*Grant.sql*您稍早儲存的方案資料夾中的指令碼。
5. 重複此程序加入*aspnet-資料-dev.sql*指令碼。

    ![設定成員資格資料庫的資料庫更新](deploying-to-iis/_static/image16.png)
6. 按一下 [ **關閉**]。

### <a name="configure-deployment-for-the-application-database"></a>設定應用程式資料庫的部署

當 Visual Studio 偵測到 Entity Framework`DbContext`類別，它會建立中的項目**資料庫**區段，其中含有**Execute Code First Migrations**核取方塊，而不是**更新資料庫**核取方塊。 在本教學課程中，您將使用該核取方塊來指定 Code First 移轉部署。

在某些情況下，您可能會使用`DbContext`資料庫，但您想要的 dbDacFx 提供者而非移轉，來部署資料庫。 在此情況下，請參閱 <<c0> [ 如何部署沒有移轉的 Code First 資料庫？](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations)在 MSDN 上 「 ASP.NET Web 部署常見問題集。

下列步驟適用於**SchoolContext**資料庫中**資料庫**對話方塊的區段。

1. 在 **遠端的連接字串**方塊中，輸入下列連接字串指向新的 SQL Server Express 應用程式資料庫。

    [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

    部署程序會將此連接字串置於已部署的 Web.config 檔案因為**在執行階段使用此連接字串**已選取。

    您也可以取得的應用程式資料庫連接字串**伺服器總管**相同的方式取得成員資格資料庫連接字串。
2. 選取  **Execute Code First Migrations （在應用程式啟動時執行）**。

    這個選項會讓部署程序來設定已部署的 Web.config 檔來指定`MigrateDatabaseToLatestVersion`初始設定式。 應用程式部署之後第一次存取資料庫時，此初始設定式會將資料庫自動更新為最新版本。

### <a name="configure-publish-profile-transforms"></a>設定發行設定檔轉換

1. 按一下 **關閉**，然後按一下**是**當詢問您是否要儲存變更。
2. 在 **方案總管**，展開**屬性**，展開**PublishProfiles**。
3. Rright 單鍵*Test.pubxml，* ，然後按一下**新增組態轉換**。

    ![新增組態轉換功能表](deploying-to-iis/_static/image17.png)

    Visual Studio 會建立*Web.Test.config*轉換檔案並開啟它。
4. 在  *Web.Test.config*轉換檔中，開啟之後，立即插入下列程式碼組態標記。

    [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    當您使用測試發行設定檔時，這項轉換會設定"Test"的環境標記。 在已部署之網站中，您會看到 」 (Test)"之後的"Contoso University"H1 標題。
5. 儲存並關閉檔案。
6. 以滑鼠右鍵按一下*Web.Test.config*檔案，然後按一下**預覽轉換**藉此確定您編寫的轉換會產生預期的變更。

    **Web.config 預覽** 視窗會顯示結果的同時套用*Web.Release.config*轉換並*Web.Test.config*轉換。

### <a name="preview-the-deployment-updates"></a>預覽部署更新

1. 開啟**發佈 Web**精靈一次 (以滑鼠右鍵按一下 ContosoUniversity 專案，然後按一下**發佈**)。
2. 在 **預覽**索引標籤上，請確定**測試**設定檔是仍選取，然後再按一下**開始預覽**若要查看將複製的檔案清單。

    ![[預覽] 按鈕](deploying-to-iis/_static/image18.png)

    ![發行預覽](deploying-to-iis/_static/image19.png)

    您也可以按一下**預覽資料庫**連結來查看將在成員資格資料庫中執行的指令碼。 （任何指令碼會不執行 Code First 移轉部署，因此沒有任何項目預覽應用程式資料庫）。
3. 按一下 [發行] 。

    如果 Visual Studio 不在系統管理員模式中，您可能會收到錯誤訊息，指出權限錯誤。 在此情況下，關閉 Visual Studio，在系統管理員模式中開啟它並再次嘗試發行。

    如果 Visual Studio 系統管理員模式**輸出**視窗報告成功建置並發行。

    ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

    如果您在輸入中的 URL**目的地 URL**  方塊中的發行設定檔**連線**索引標籤上，瀏覽器會自動開啟至 Contoso 大學首頁本機電腦上 IIS 中執行。

## <a name="test-in-the-test-environment"></a>在測試環境中測試

請注意，環境指示器會顯示 [（測試）] 而不是"(Dev)"，其中會顯示*Web.config*環境指標的轉換是否成功。

執行**講師**頁面，確認第一個程式碼植入講師資料的資料庫。 當您選取此頁面時，可能需要幾分鐘的時間載入，因為第一個程式碼，則會建立資料庫，然後再執行`Seed`方法。 （它並沒有進行，因為尚未存取資料庫沒有嘗試過的應用程式已在首頁上時。）

按一下 [**學生**] 索引標籤，確認已部署的資料庫有任何學生。

選取 **新增的學生**從**學生**功能表上，新增為學生，，然後檢視 在新的學生**學生**頁面，確認您可以成功地寫入至資料庫.

從**課程**功能表上，選取**更新信用額度**。 **更新信用額度**頁面會要求系統管理員權限，所以**登入**頁面隨即顯示。 輸入您建立舊版 （"admin"和"devpwd 」） 的系統管理員帳戶認證。 **更新信用額度**顯示網頁時，會驗證您在上一個教學課程中建立的系統管理員帳戶已正確部署到測試環境。

確認*Elmah*資料夾存在於*c:\inetpub\wwwroot\ContosoUniversity*資料夾，並只在其中的預留位置檔案。

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>檢閱自動 Web.config 變更的 Code First 移轉

開啟*Web.config*在已部署的應用程式中的檔案*C:\inetpub\wwwroot\ContosoUniversity*和您所見，部署程序設定 Code First 移轉，自動更新為最新版本的資料庫。

![](deploying-to-iis/_static/image21.png)

部署程序也會建立新的連接字串，針對專門用來更新資料庫結構描述的 Code First 移轉：

![Database_Publish 連接字串](deploying-to-iis/_static/image22.png)

此額外的連接字串可讓您指定的資料庫結構描述更新，一個使用者帳戶和不同的使用者帳戶存取應用程式資料。 比方說，您可以指派**db\_擁有者**角色，以 Code First 移轉，和**db\_datareader**並**db\_資料寫入元**應用程式的角色。 這是常見的深度防禦模式，以防止潛在的惡意程式碼變更資料庫結構描述應用程式。 （例如，這可能是在 SQL 資料隱碼攻擊成功。）這些教學課程不會使用此模式。 如果您想要實作此模式在您的案例中，您可以執行下列步驟來執行：

1. 在 [**設定**索引標籤**發佈 Web**精靈] 中，輸入連接字串，指定具有完整的資料庫結構描述更新權限的使用者，並清除**使用此連接字串在執行階段**核取方塊。 在已部署的 Web.config 檔案中，這會成為`DatabasePublish`連接字串。
2. 建立 Web.config 檔案轉換為您想要在執行階段使用的應用程式的連接字串。

## <a name="summary"></a>總結

您現在已部署至 IIS 的應用程式的開發電腦上，而且進行了測試那里。

![在測試中的 [首頁] 頁面](deploying-to-iis/_static/image23.png)

這會確認部署程序將應用程式的內容複製到正確的位置 （不包括您不想要部署的檔案），也該 Web Deploy IIS 正確地在部署期間設定。 在下一個教學課程中，您將執行一個會尋找尚未完成的部署工作的多個測試： 設定資料夾權限*Elmah*資料夾。

## <a name="more-information"></a>詳細資訊

如需在 Visual Studio 中執行 IIS 或 IIS Express 的資訊，請參閱下列資源：

- [IIS Express 概觀](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview)IIS.net 網站上。
- [簡介 IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) Scott Guthrie 的部落格上。
- [Web 伺服器，在 Visual Studio 中的，針對 ASP.NET Web 專案](https://msdn.microsoft.com/library/58wxa9w5.aspx)。
- [核心 IIS 之間的差異和 ASP.NET Development Server](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) ASP.NET 網站上。

如在中度信任執行的應用程式時，就可能發生哪些問題的相關資訊，請參閱 <<c0> [ 裝載在中度信任 ASP.NET 應用程式](http://www.4guysfromrolla.com/articles/100307-1.aspx)Rolla 站台的 4 個夥伴上。

> [!div class="step-by-step"]
> [上一頁](project-properties.md)
> [下一頁](setting-folder-permissions.md)
