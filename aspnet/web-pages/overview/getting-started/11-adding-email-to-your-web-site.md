---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: 傳送電子郵件從 ASP.NET Web Pages (Razor) 網站 |Microsoft Docs
author: tfitzmac
description: 本章說明如何從網站傳送自動化電子郵件訊息。
ms.author: aspnetcontent
ms.date: 02/20/2014
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: e94cba91da43101ef1c058b49be746821bb0fe16
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841467"
---
<a name="sending-email-from-an-aspnet-web-pages-razor-site"></a>從 ASP.NET Web Pages (Razor) 網站傳送電子郵件
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 這篇文章說明如何從網站傳送電子郵件訊息，當您使用 ASP.NET Web Pages (Razor)。
> 
> 您將學到什麼：
> 
> - 如何從您的網站傳送電子郵件訊息。
> - 如何將檔案附加到電子郵件。
> 
> 這是發行項中導入的 ASP.NET 功能：
> 
> - `WebMail`協助程式。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> 本教學課程也適用於 ASP.NET Web Pages 2。


<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a>從您的網站傳送電子郵件訊息

有各種您可能需要從您的網站傳送電子郵件的原因。 您可能會傳送確認訊息給使用者，或您可能會傳送通知給自己 （例如，註冊新的使用者。）`WebMail`協助程式可讓您輕鬆地傳送電子郵件。

若要使用`WebMail`協助程式，您必須擁有 SMTP 伺服器的存取權。 (代表 SMTP *Simple Mail Transfer Protocol*。)SMTP 伺服器是只將訊息轉送到收件者伺服器的電子郵件伺服器&#8212;是電子郵件的輸出端。 如果您使用主機服務提供者為您的網站時，它們可能為您設定電子郵件，他們可以告訴您您的 SMTP 伺服器名稱為何。 如果您使用公司網路內部時，系統管理員或您的 IT 部門可通常讓您的 SMTP 伺服器，您可以使用的相關資訊。 如果您在家工作，您甚至可以測試使用一般的電子郵件提供者，能告訴您其 SMTP 伺服器的名稱。 您通常需要：

- SMTP 伺服器的名稱。
- 連接埠號碼。 這幾乎都是 25。 不過，您的 ISP 可能會要求您使用連接埠 587。 如果您使用安全通訊端層 (SSL) 的電子郵件，您可能需要不同的連接埠。 請洽詢您的電子郵件提供者。
- 認證 （使用者名稱、 密碼）。

在此程序中，您會建立兩個頁面。 第一頁會有一個表單，讓使用者輸入的描述，如同它們所填入的技術支援表單。 第一頁會提交其第二個頁面的資訊。 在第二個頁面中，程式碼會擷取使用者的資訊，並傳送電子郵件訊息。 它也會顯示訊息，確認已收到問題報告。

![[影像]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> 為了簡化此範例中，程式碼會初始化`WebMail`在頁面中，您使用的協助程式權限。 不過，實際的網站，就像這樣的初始化程式碼置於通用的檔案，是更好的辦法，讓您初始化`WebMail`協助程式，您的網站中的所有檔案。 如需詳細資訊，請參閱 <<c0> [ 自訂全網站行為適用於 ASP.NET 網頁](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers)。


1. 建立新的網站。
2. 新增名為頁面*EmailRequest.cshtml*並新增下列標記： 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    請注意，`action`表單項目的屬性已設定為*ProcessRequest.cshtml*。 這表示會在該頁面，而不是到目前的頁面後送出表單。
3. 新增名為頁面*ProcessRequest.cshtml*網站，並新增下列程式碼和標記：   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    在程式碼中，您可以取得已提交至網頁表單欄位的值。 然後呼叫`WebMail`協助程式的`Send`方法來建立並傳送電子郵件訊息。 在此情況下，要使用的值是組成您提交表單中的值串連的文字。

    此頁面的程式碼位於`try/catch`區塊。 如果由於任何原因嘗試傳送電子郵件將無法運作 （例如，設定不正確，） 中的程式碼`catch`區塊執行，並設定`errorMessage`變數設為已發生的錯誤。 (如需詳細資訊`try/catch`區塊或`<text>`標記，請參閱[ASP.NET Web Pages 程式設計使用 Razor 語法簡介](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors)。)

    本文的頁面上，如果`errorMessage`變數是空的 （預設值），使用者會看到已傳送電子郵件訊息的訊息。 如果`errorMessage`變數設為 true，使用者所看到，訊息已傳送訊息的問題。

    請注意，在顯示錯誤訊息的頁面部分，有額外的測試： `if(debuggingFlag)`。 這是您可以設定為 true，如果您遇到問題，傳送電子郵件的變數。 當`debuggingFlag`為 true，而且如果沒有問題，傳送電子郵件，一則錯誤訊息會顯示顯示嘗試傳送電子郵件訊息時，ASP.NET 任何已回報。 公平警告，不過： 可以是泛型的 ASP.NET 報告時它無法傳送電子郵件訊息的錯誤訊息。 例如，如果 ASP.NET 無法連絡 SMTP 伺服器 （例如，因為您在 伺服器名稱做錯誤），錯誤為`Failure sending mail`。

    > [!NOTE] 
    > 
    > **重要**當您收到錯誤訊息從例外狀況物件 (`ex`程式碼中)，請勿*不*定期將透過該訊息傳遞給使用者。 例外狀況物件通常會包含的資訊的使用者應該不會看到和，甚至還可以有安全性弱點。 這就是為什麼這段程式碼包含變數`debuggingFlag`作為交換器，用來顯示錯誤訊息，以及為什麼預設變數設為 false。 您應該設定為 true （並因此顯示錯誤訊息） 該變數*只*如果您遇到的問題傳送電子郵件，而且您需要偵錯。 一旦您已修正任何問題，設定`debuggingFlag`回為 false。

    修改下列電子郵件的程式碼中的相關的設定：

   - 設定`your-SMTP-host`您具有存取權的 SMTP 伺服器的名稱。
   - 設定`your-user-name-here`SMTP 伺服器帳戶的使用者名稱。
   - 設定`your-account-password`SMTP 伺服器帳戶的密碼。
   - 設定`your-email-address-here`您自己的電子郵件地址。 這是從訊息傳送的電子郵件地址。 (某些電子郵件提供者不讓您指定不同`From`位址，並使用您的使用者名稱，做為`From`地址。)

     > [!TIP] 
     > 
     > <a id="configuring_email_settings"></a>
     > ### <a name="configuring-email-settings"></a>設定電子郵件設定
     > 
     > 它可以是一項挑戰，有時候是為了確定您已正確設定 SMTP 伺服器、 連接埠號碼和等等。 下列提供幾個秘訣：
     > 
     > - SMTP 伺服器名稱通常會像下面`smtp.provider.com`或`smtp.provider.net`。 不過，如果您將您的站台發佈到主機服務提供者時，SMTP 伺服器名稱此時可能`localhost`。 這是因為您已發行，並提供者的伺服器上執行您的網站之後，電子郵件伺服器可能已本機從您的應用程式的觀點來看。 在 伺服器名稱中的這項變更可能表示您必須變更 SMTP 伺服器名稱做為您的發行程序的一部分。
     > - 連接埠號碼通常為 25。 不過，某些提供者需要您使用的連接埠 587 或某些其他連接埠。
     > - 請確定您使用正確的認證。 如果您已發行您的站台至主控提供者，使用提供者特別指出適用於電子郵件的認證。 這些可能是不同於您使用發行認證。
     > - 有時候您不需要認證。 如果您要傳送電子郵件使用您個人的 ISP，您的電子郵件提供者可能已經知道您的認證。 發行之後，您可能需要使用不同的認證比當您測試本機電腦上。
     > - 如果您的電子郵件提供者會使用加密，您必須設定`WebMail.EnableSsl`至`true`。
4. 執行*EmailRequest.cshtml*瀏覽器中的頁面。 (請確定中選取頁面**檔案**才能執行這個工作區。)
5. 輸入您的名稱和問題描述，然後再按一下**送出** 按鈕。 若要將您重新導向*ProcessRequest.cshtml*  頁面上，以確認您的訊息，並將您傳送電子郵件訊息。 

    ![[影像]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a>使用電子郵件的檔案傳送

您也可以傳送已附加至電子郵件訊息的檔案。 在此程序中，您會建立文字檔案和兩個 HTML 網頁。 您將使用的文字檔案，作為電子郵件附件。

1. 在網站中，加入新的文字檔並命名*MyFile.txt*。
2. 複製下列文字，並將它貼在檔案中： 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. 建立名為的頁面*SendFile.cshtml*並新增下列標記： 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. 建立名為的頁面*ProcessFile.cshtml*並新增下列標記： 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. 修改下列電子郵件的範例程式碼中的相關的設定：

    - 設定`your-SMTP-host`您具有存取權的 SMTP 伺服器的名稱。
    - 設定`your-user-name-here`SMTP 伺服器帳戶的使用者名稱。
    - 設定`your-email-address-here`您自己的電子郵件地址。 這是從訊息傳送的電子郵件地址。
    - 設定`your-account-password`SMTP 伺服器帳戶的密碼。
    - 設定`target-email-address-here`您自己的電子郵件地址。 （如之前，您通常會傳送一封電子郵件給其他人，但為了測試，您可以傳送給您自己）。
6. 執行*SendFile.cshtml*瀏覽器中的頁面。
7. 輸入您的名稱、 [主旨] 行中和要附加的文字檔案的名稱 (*MyFile.txt*)。
8. 按一下 `Submit` 按鈕。 因為您之前，會被重新導向至*ProcessFile.cshtml*  頁面上，以確認您的訊息，其會將傳送您的電子郵件訊息使用附加的檔案。

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源


- [ASP.NET Web Pages (Razor) 疑難排解指南](https://go.microsoft.com/fwlink/?LinkId=253001)
- [簡易郵件傳輸通訊協定](https://msdn.microsoft.com/library/aa480435.aspx)
- [ASP.NET Web 網頁的自訂全網站行為](https://go.microsoft.com/fwlink/?LinkId=202906)
