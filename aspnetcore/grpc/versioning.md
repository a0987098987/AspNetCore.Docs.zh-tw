---
title: Versioning gRPC 服務
author: jamesnk
description: 瞭解如何對 gRPC 服務進行版本控制。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 01/09/2020
uid: grpc/versioning
ms.openlocfilehash: 9bd76009ba28a1abef25a98686afea6753d4a8f4
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78664112"
---
# <a name="versioning-grpc-services"></a>Versioning gRPC 服務

由[詹姆斯·牛頓-金](https://twitter.com/jamesnk)

添加到應用的新功能可能需要向用戶端提供的 gRPC 服務進行更改,有時以意外和中斷的方式更改。 當 gRPC 服務變更時:

* 應考慮更改如何影響客戶。
* 應實施支援更改的版本控制策略。

## <a name="backwards-compatibility"></a>回溯相容性

gRPC 協定旨在支援隨時間變化的服務。 通常,gRPC 服務和方法的添加是非突破的。 非重大更改允許現有用戶端在不進行更改的情況下繼續工作。 更改或刪除 gRPC 服務正在中斷更改。 當 gRPC 服務發生重大更改時,必須更新和重新部署使用該服務的用戶端。

對服務進行不重大變更具有許多好處:

* 現有客戶端繼續運行。
* 避免將更改通知用戶端並更新它們所涉及的工作。
* 只需要記錄和維護一個版本的服務。

### <a name="non-breaking-changes"></a>非中斷性變更

這些更改在 gRPC 協定等級和 .NET 二進位級上是非突破的。

* **新增服務**
* **新增新方法**
* **將欄位加入到請求訊息**- 新增到請求訊息的欄位在未設定時使用伺服器上的[預設值](https://developers.google.com/protocol-buffers/docs/proto3#default)進行反序列化。 要進行非重大更改,當舊用戶端未設置新欄位時,服務必須成功。
* **傳回回應訊息加入欄位**- 加入到回應訊息的欄位將反序列化到用戶端上訊息的[未知欄位](https://developers.google.com/protocol-buffers/docs/proto3#unknowns)集合中。
* **向枚舉**- 枚舉添加值將序列化為數值。 新的枚舉值在用戶端上反序列化到沒有枚舉名稱的枚舉值。 要進行非重大更改,較舊的用戶端在接收新的枚舉值時必須正確運行。

### <a name="binary-breaking-changes"></a>二進位中斷變更

以下更改在 gRPC 協定等級不會中斷,但如果用戶端升級到最新的 *.proto*協定或用戶端 .NET 程式集,則需要更新用戶端。 如果您計劃將 gRPC 庫發佈到 NuGet,則二進位兼容性非常重要。

* **從刪除的欄位中移除欄位**值將反序列化為訊息的[未知欄位](https://developers.google.com/protocol-buffers/docs/proto3#unknowns)。 這不是 gRPC 協定中斷更改,但如果用戶端升級到最新協定,則需要更新用戶端。 將來不會意外重用已刪除的欄位編號,這一點很重要。 為確保這種情況不會發生,請使用 Protobuf 的[保留](https://developers.google.com/protocol-buffers/docs/proto3#reserved)關鍵字在郵件上指定已刪除的欄位號和名稱。
* **重新命名訊息**- 訊息名稱通常不在網路上發送,因此這不是 gRPC 協定中斷更改。 如果用戶端升級到最新合同,則需要更新該用戶端。 在網路上**發送消息名稱**的一種方式是使用[Any](https://developers.google.com/protocol-buffers/docs/proto3#any)欄位,當消息名稱用於識別訊息類型時。
* **變更csharp_namespace** `csharp_namespace`- 更改將更改產生的 .NET 類型的命名空間。 這不是 gRPC 協定中斷更改,但如果用戶端升級到最新協定,則需要更新用戶端。

### <a name="protocol-breaking-changes"></a>協定中斷變更

以下項目是協定與二進制變更:

* **重命名字段**- 使用 Protobuf 內容時,欄位名稱僅在生成的代碼中使用。 欄位編號用於標識網路上的欄位。 重命名字段不是 Protobuf 的中斷協定更改。 但是,如果伺服器正在使用 JSON 內容,則重命名字段是一個重大更改。
* **變更欄位資料類型**- 將欄位的資料類型更改為[不相容的類型](https://developers.google.com/protocol-buffers/docs/proto3#updating)將導致消息序列化時出錯。 即使新數據類型相容,如果用戶端升級到最新協定,它也可能需要更新以支援新類型。
* **變更欄位編號**- 使用 Protobuf 有效負載時,欄位編號用於識別網路上的欄位。
* **重新命名套件、服務或方法**- gRPC 使用套件名稱、服務名稱和方法名稱來生成 URL。 用戶端從伺服器獲取 *「未執行」* 狀態。
* **刪除服務或方法**- 用戶端在呼叫已刪除的方法時從伺服器獲取*UN實現*狀態。

### <a name="behavior-breaking-changes"></a>行為中斷變更

進行非重大更改時,還必須考慮較舊的用戶端是否可以繼續使用新的服務行為。 例如,向要求訊息加入新欄位:

* 不是協定崩潰的更改嗎?
* 如果未設置新欄位,則在伺服器上返回錯誤狀態,將使其成為舊用戶端的重大變化。

行為相容性由特定於應用的代碼決定。

## <a name="version-number-services"></a>版本號碼

服務應努力與舊用戶端保持向後相容。 最終,對應用的更改可能需要重大更改。 中斷舊用戶端並強制它們隨服務一起更新並不是一個好的用戶體驗。 在進行重大更改時保持向後相容性的一種方法是發佈服務的多個版本。

gRPC 支援可選[的包](https://developers.google.com/protocol-buffers/docs/proto3#packages)指定器,它的功能很像 .NET 命名空間。 事實上,如果未`option csharp_namespace`在`package` *.proto*檔中設置 ,則將用作生成的 .NET 類型的 .NET 命名空間。 該套件可用於為服務及其訊息指定版本號:

[!code-protobuf[](versioning/sample/greet.v1.proto?highlight=3)]

包名稱與服務名稱相結合以標識服務位址。 服務位址允許並行託管服務的多個版本:

* `greet.v1.Greeter`
* `greet.v2.Greeter`

版本化服務的實現在*Startup.cs*註冊:

```csharp
app.UseEndpoints(endpoints =>
{
    // Implements greet.v1.Greeter
    endpoints.MapGrpcService<GreeterServiceV1>();

    // Implements greet.v2.Greeter
    endpoints.MapGrpcService<GreeterServiceV2>();
});
```

在包名稱中包含版本號後,您有機會發佈具有重大更改的服務的*v2*版本,同時繼續支援調用*v1*版本的舊用戶端。 用戶端更新後使用*v2*服務後,您可以選擇刪除舊版本。 排程發佈服務的多個版本時:

* 如果合理,避免進行重大更改。
* 除非進行重大更改,否則不要更新版本號。
* 進行重大更改時,請更新版本號。

發佈服務的多個版本會複製它。 為了減少重複,請考慮將業務邏輯從服務實現移動到可由新舊實現重用的集中位置:

[!code-csharp[](versioning/sample/GreeterServiceV1.cs?highlight=10,19)]

使用不同套件名稱產生的服務和訊息是不同的 **.NET 型態**。 將業務邏輯移動到集中位置需要將消息映射到常見類型。
