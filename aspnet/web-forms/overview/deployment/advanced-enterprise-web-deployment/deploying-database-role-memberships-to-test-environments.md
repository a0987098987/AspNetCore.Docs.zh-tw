---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: "將資料庫角色成員資格部署到測試環境 |Microsoft 文件"
author: jrjlee
description: "本主題描述如何將使用者帳戶新增至資料庫角色做為方案部署到測試環境的一部分。 當您部署方案，包含..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: 226c28622f76e866fba1fc33cf9b9b7a01e5295b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="deploying-database-role-memberships-to-test-environments"></a>將資料庫角色成員資格部署到測試環境
====================
由[Jason Lee](https://github.com/jrjlee)

[下載 PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> 本主題描述如何將使用者帳戶新增至資料庫角色做為方案部署到測試環境的一部分。
> 
> 當您部署包含要預備或生產環境的資料庫專案的方案時，您通常不想自動加入至資料庫角色的使用者帳戶的開發人員。 在大部分情況下，開發人員不知道要新增至哪一個資料庫角色、 哪些使用者帳戶，這些需求可能會隨時變更。 不過，當您部署方案，包含資料庫專案開發或測試環境，這種情況通常是而不同：
> 
> - 開發人員通常重新部署上定期執行，解決方案通常數次一天。
> - 通常每個部署，這表示必須建立資料庫使用者，而且每次部署之後加入至角色上重新建立資料庫。
> - 開發人員通常有目標開發或測試環境的完整控制權。
> 
> 在此案例中，通常很有幫助自動建立資料庫使用者，並將資料庫角色成員資格指派做為部署程序的一部分。
> 
> 關鍵因素是，這項作業必須是有條件根據目標環境。 如果您要部署至預備或生產環境中，您會想要跳過此作業。 如果您要部署的開發人員或測試環境，您會想要部署不需要其他介入的角色成員資格。 本主題說明您可以使用來解決這個問題的其中一個方法。


本主題根據名為 Fabrikam，Inc.的虛構公司的企業部署需求的教學課程系列的一部分此教學課程使用範例方案 & #x 2014;[連絡人管理員解決方案](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)（& s) 來代表實際的層級的複雜性，包括 ASP.NET MVC 3 應用程式時，Windows 與 web 應用程式的 #x 2014;Communication Foundation (WCF) 服務，與資料庫專案。

這些教學課程的核心的部署方法為基礎所說明的分割專案檔案方法[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，這在建置程序由兩個專案中檔案 & #x 2014; 一個包含建置適用於每個目的地環境中和包含特定環境的建置和部署設定的指示。 在建置時，環境特定專案檔就會合併環境無從驗證專案檔來形成一組完整組建指示。

## <a name="task-overview"></a>工作概觀

本主題假設：

- 中所述，使用方案部署的分割專案檔案方法[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)。
- 中所述，從專案檔來部署資料庫專案中，呼叫 VSDBCMD[瞭解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。

若要建立資料庫使用者，並指派角色成員資格，您將資料庫專案部署到測試環境時，您將需要：

- 建立會進行必要的資料庫變更的 Transact 結構化查詢語言 (TRANSACT-SQL) 指令碼。
- 建立會使用 sqlcmd.exe 公用程式執行的 SQL 指令碼的 Microsoft Build Engine (MSBuild) 目標。
- 設定您的專案檔案，來叫用的目標，當您要將方案部署至測試環境。

本主題將說明如何執行這種程序。

## <a name="scripting-the-database-role-memberships"></a>指令碼的資料庫角色成員資格

您可以在許多不同的方式，來建立 TRANSACT-SQL 指令碼，而且在您選擇的任何位置。 最簡單的方法是在 Visual Studio 2010 中建立您的方案內的指令碼。

**若要建立 SQL 指令碼**

1. 在**方案總管 中**視窗中，展開資料庫專案節點。
2. 以滑鼠右鍵按一下**指令碼**資料夾，指向**新增**，然後按一下 **新資料夾**。
3. 型別**測試**做為資料夾名稱，然後按 Enter。
4. 以滑鼠右鍵按一下**測試**資料夾，指向**新增**，然後按一下 **指令碼**。
5. 在**加入新項目**對話方塊方塊中，為您的指令碼提供有意義的名稱 (例如， **AddRoleMemberships.sql**)，然後按一下 **新增**。

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. 在*AddRoleMemberships.sql*檔案中，將 TRANSACT-SQL 陳述式加入的：

    1. 建立資料庫使用者，將會存取您的資料庫之 SQL Server 登入。
    2. 將資料庫使用者加入至任何必要的資料庫角色。
7. 檔案應與以下相似：

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. 儲存檔案。

## <a name="executing-the-script-on-the-target-database"></a>在目標資料庫上執行指令碼

在理想情況下，您會執行任何必要的 TRANSACT-SQL 指令碼做為部署後指令碼的一部分，當您部署資料庫專案。 不過，部署後指令碼不允許您執行條件式方案組態或組建屬性上的邏輯。 替代方法是直接從 MSBuild 專案檔中，執行您的 SQL 指令碼，藉由建立**目標**執行 sqlcmd.exe 命令的項目。 您可以使用此命令在目標資料庫上執行您的指令碼：


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> 如需有關 sqlcmd 命令列選項的詳細資訊，請參閱[sqlcmd 公用程式](https://msdn.microsoft.com/library/ms162773.aspx)。


此命令嵌入 MSBuild 目標之前，您需要考慮在哪些情況下您想要執行的指令碼：

- 目標資料庫必須存在才能變更其角色成員資格。 因此，您需要執行此指令碼*之後*資料庫部署。
- 您必須包含一個條件，以便在測試環境，才會執行指令碼。
- 如果您執行 「 假設 」 部署 & #x 2014; 換句話說，如果您正在產生部署指令碼，但實際上不會執行它們 & #x 2014; 您不應該執行的 SQL 指令碼。

如果您使用分割專案檔案描述的方法中[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)，所示連絡人管理員範例方案，您可以將組建指示分割 SQL 指令碼如下：

- 任何所需的環境特有的屬性，以及決定是否要將權限，部署的屬性應該放在特定環境的專案檔 (例如， *Env Dev.proj*)。
- MSBuild 目標本身，以及目的地環境之間不會變更任何屬性都應在通用專案檔 (例如， *Publish.proj*)。

在特定環境的專案檔中，您需要定義的資料庫伺服器名稱、 目標資料庫名稱，以及讓使用者可以指定是否要部署角色成員資格的布林值屬性。


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


在通用專案檔中，您需要提供 sqlcmd 可執行檔的位置以及您想要執行的 SQL 指令碼的位置。 這些屬性會維持不變，不論目的環境。 您也需要建立 MSBuild 目標執行 sqlcmd 命令。


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


請注意 sqlcmd 可執行檔的位置加入為靜態屬性，這可能是其他目標很有用。 相反地，您定義 SQL 指令碼的位置和 sqlcmd 命令的語法為動態屬性內目標，因為它們不會需要執行目標之前。 在此情況下， **DeployTestDBPermissions**如果符合這些條件，則只會執行目標：

- **DeployTestDBRoleMemberships**屬性設定為**true**。
- 使用者尚未指定**WhatIf = true**旗標。

最後，別忘了要叫用的目標。 在*Publish.proj*檔案中，您可以將目標加入至預設值的相依性清單**FullPublish**目標。 您必須確保**DeployTestDBPermissions**目標不會執行，直到**PublishDbPackages**在執行目標。


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a>結論

本主題描述可以在其中您加入資料庫使用者和角色成員資格做為部署後動作當您部署資料庫專案的一種方法。 當您定期重新建立資料庫，以在測試環境中，但它應該通常避免您將資料庫部署到預備環境或生產環境時，這是通常很有用。 因此，您應該確定您使用的是必要的條件式邏輯，使資料庫使用者和角色成員資格，才會建立適合這樣做的時機。

## <a name="further-reading"></a>進一步閱讀

如需有關使用 VSDBCMD 部署資料庫專案的詳細資訊，請參閱[部署資料庫專案](../web-deployment-in-the-enterprise/deploying-database-projects.md)。 如需自訂不同的目標環境的資料庫部署的指引，請參閱[自訂資料庫部署多個環境](customizing-database-deployments-for-multiple-environments.md)。 如需使用自訂的 MSBuild 專案檔來控制部署程序的詳細資訊，請參閱[了解專案檔](../web-deployment-in-the-enterprise/understanding-the-project-file.md)和[瞭解建置程序](../web-deployment-in-the-enterprise/understanding-the-build-process.md)。 如需有關 sqlcmd 命令列選項的詳細資訊，請參閱[sqlcmd 公用程式](https://msdn.microsoft.com/library/ms162773.aspx)。

>[!div class="step-by-step"]
[上一頁](customizing-database-deployments-for-multiple-environments.md)
[下一頁](deploying-membership-databases-to-enterprise-environments.md)
