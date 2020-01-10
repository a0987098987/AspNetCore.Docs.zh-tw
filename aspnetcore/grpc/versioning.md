---
title: 版本控制 gRPC 服務
author: jamesnk
description: 瞭解如何版本 gRPC 服務。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 01/09/2020
uid: grpc/versioning
ms.openlocfilehash: 9bd76009ba28a1abef25a98686afea6753d4a8f4
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828512"
---
# <a name="versioning-grpc-services"></a>版本控制 gRPC 服務

依[James 牛頓-王](https://twitter.com/jamesnk)

新增至應用程式的新功能可能需要提供給用戶端的 gRPC 服務變更，有時是以非預期和中斷的方式。 當 gRPC services 變更時：

* 應針對變更對用戶端的影響提供考慮。
* 應執行支援變更的版本控制策略。

## <a name="backwards-compatibility"></a>回溯相容性

GRPC 通訊協定是設計用來支援隨時間變更的服務。 一般來說，gRPC 服務和方法的新增功能不會中斷。 非重大變更可讓現有的用戶端繼續運作，而不需要變更。 變更或刪除 gRPC 服務是重大變更。 當 gRPC 服務有重大變更時，必須更新並重新部署使用該服務的用戶端。

對服務進行非重大變更有一些優點：

* 現有的用戶端會繼續執行。
* 避免與通知用戶端重大變更並加以更新所牽涉到的工作。
* 只有一個服務版本需要記載和維護。

### <a name="non-breaking-changes"></a>非中斷性變更

這些變更不會在 gRPC 通訊協定層級和 .NET 二進位層級中斷。

* **新增服務**
* **將新方法加入至服務**
* **將欄位加入至要求訊息**-新增至要求訊息的欄位會在未設定時，以伺服器上的[預設值](https://developers.google.com/protocol-buffers/docs/proto3#default)進行還原序列化。 若要做為非中斷變更，當舊的用戶端未設定新欄位時，服務必須成功。
* **將欄位加入至**回應訊息-新增至回應訊息的欄位會還原序列化至用戶端上的訊息[未知欄位](https://developers.google.com/protocol-buffers/docs/proto3#unknowns)集合中。
* **將值加入至 enum 列舉**會序列化為數值。 新的列舉值會在用戶端上還原序列化為不含列舉名稱的列舉值。 若要做為非中斷變更，較舊的用戶端必須在收到新的列舉值時正確執行。

### <a name="binary-breaking-changes"></a>二進位中斷性變更

下列變更在 gRPC 通訊協定層級不會中斷，但如果用戶端升級至最新的*proto*合約或用戶端 .net 元件，則需要更新。 如果您打算將 gRPC 程式庫發佈至 NuGet，二進位相容性就很重要。

* 從已移除的欄位**移除欄位**值，會還原序列化為訊息的[未知欄位](https://developers.google.com/protocol-buffers/docs/proto3#unknowns)。 這不是 gRPC 的通訊協定重大變更，但如果用戶端升級至最新的合約，則需要更新。 重要的是，在未來不小心重複使用移除的欄位號碼。 若要確保不會發生這種情況，請使用 Protobuf 的[reserved](https://developers.google.com/protocol-buffers/docs/proto3#reserved)關鍵字，在訊息上指定已刪除的欄位編號和名稱。
* 重新**命名訊息**：通常不會在網路上傳送訊息名稱，因此這不是 gRPC 的通訊協定中斷變更。 如果用戶端升級至最新的合約，則需要更新。 當訊息名稱用來識別訊息類型時，在網路**上傳送訊息**名稱的一種情況就是使用[任何](https://developers.google.com/protocol-buffers/docs/proto3#any)欄位。
* 變更**csharp_namespace**變更 `csharp_namespace` 會變更所產生之 .net 類型的命名空間。 這不是 gRPC 的通訊協定重大變更，但如果用戶端升級至最新的合約，則需要更新。

### <a name="protocol-breaking-changes"></a>通訊協定重大變更

下列專案是通訊協定和二進位中斷性變更：

* 重新**命名欄位**-使用 Protobuf 內容時，功能變數名稱只會用於產生的程式碼中。 欄位編號是用來識別網路上的欄位。 重新命名欄位不是 Protobuf 的通訊協定中斷變更。 不過，如果伺服器使用 JSON 內容，則重新命名欄位是一種中斷變更。
* **變更欄位資料**類型-將欄位的資料類型變更為[不相容的類型](https://developers.google.com/protocol-buffers/docs/proto3#updating)，會在還原序列化訊息時造成錯誤。 即使新的資料類型是相容的，如果用戶端升級至最新的合約，就可能需要更新它以支援新的類型。
* **變更欄位編號**-使用 Protobuf 裝載時，會使用欄位號碼來識別網路上的欄位。
* 重新**命名封裝、服務或方法**gRPC 會使用封裝名稱、服務名稱和方法名稱來建立 URL。 用戶端會從伺服器取得未*實現*的狀態。
* **移除服務或方法**-呼叫已移除的方法時，用戶端會從伺服器*取得未完成的狀態*。

### <a name="behavior-breaking-changes"></a>行為重大變更

進行非中斷性變更時，您也必須考慮舊的用戶端是否可以繼續使用新的服務行為。 例如，將新欄位新增至要求訊息：

* 不是通訊協定中斷變更。
* 如果未設定新欄位，則傳回伺服器上的錯誤狀態，使其成為舊用戶端的重大變更。

行為相容性取決於您的應用程式專屬程式碼。

## <a name="version-number-services"></a>版本號碼服務

服務應該致力於維持與舊用戶端的回溯相容性。 應用程式最終的變更可能需要重大變更。 中斷舊的用戶端，並強制它們與您的服務一起更新，並不是很好的使用者體驗。 在進行重大變更時，維護回溯相容性的方法是發佈服務的多個版本。

gRPC 支援選擇性的[封裝](https://developers.google.com/protocol-buffers/docs/proto3#packages)規範，其功能非常類似 .net 命名空間。 事實上，如果在*proto*檔案中未設定 `option csharp_namespace`，則 `package` 會當做產生之 .net 類型的 .net 命名空間使用。 封裝可以用來指定您的服務和其訊息的版本號碼：

[!code-protobuf[](versioning/sample/greet.v1.proto?highlight=3)]

封裝名稱會與服務名稱結合，以識別服務位址。 服務位址可讓多個版本的服務並存裝載：

* `greet.v1.Greeter`
* `greet.v2.Greeter`

版本服務的執行會在*Startup.cs*中註冊：

```csharp
app.UseEndpoints(endpoints =>
{
    // Implements greet.v1.Greeter
    endpoints.MapGrpcService<GreeterServiceV1>();

    // Implements greet.v2.Greeter
    endpoints.MapGrpcService<GreeterServiceV2>();
});
```

在封裝名稱中包含版本號碼，可讓您有機會發佈服務的*v2*版本與重大變更，同時繼續支援呼叫*v1*版本的舊版用戶端。 一旦用戶端已更新為使用*v2*服務，您可以選擇移除舊版本。 在規劃發行服務的多個版本時：

* 若合理，請避免中斷性變更。
* 除非進行重大變更，否則請勿更新版本號碼。
* 當您進行重大變更時，請更新版本號碼。

發行服務的多個版本會將它重複。 若要減少重複的情況，請考慮將商務邏輯從服務的部署移至可供舊的和新的執行重複使用的集中位置：

[!code-csharp[](versioning/sample/GreeterServiceV1.cs?highlight=10,19)]

以不同的套件名稱產生的服務和訊息是**不同的 .net 類型**。 將商務邏輯移至集中位置需要將訊息對應至一般類型。
