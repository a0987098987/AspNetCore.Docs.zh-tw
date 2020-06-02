---
title: 使用 dotnet-grpc 管理 Protobuf 參考
author: juntaoluo
description: 瞭解如何使用 dotnet-grpc 通用工具新增、更新、移除和列出 Protobuf 參考。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 10/17/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: grpc/dotnet-grpc
ms.openlocfilehash: 0990013947be2cee5045deac92efc3c6bcf12e03
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82768831"
---
# <a name="manage-protobuf-references-with-dotnet-grpc"></a><span data-ttu-id="ecedb-103">使用 dotnet-grpc 管理 Protobuf 參考</span><span class="sxs-lookup"><span data-stu-id="ecedb-103">Manage Protobuf references with dotnet-grpc</span></span>

<span data-ttu-id="ecedb-104">作者：[John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="ecedb-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="ecedb-105">`dotnet-grpc`是 .NET Core 通用工具，可用於管理 .NET gRPC 專案內的[Protobuf （*. proto*）](xref:grpc/basics#proto-file)參考。</span><span class="sxs-lookup"><span data-stu-id="ecedb-105">`dotnet-grpc` is a .NET Core Global Tool for managing [Protobuf (*.proto*)](xref:grpc/basics#proto-file) references within a .NET gRPC project.</span></span> <span data-ttu-id="ecedb-106">此工具可以用來新增、重新整理、移除和列出 Protobuf 的參考。</span><span class="sxs-lookup"><span data-stu-id="ecedb-106">The tool can be used to add, refresh, remove, and list Protobuf references.</span></span>

## <a name="installation"></a><span data-ttu-id="ecedb-107">安裝</span><span class="sxs-lookup"><span data-stu-id="ecedb-107">Installation</span></span>

<span data-ttu-id="ecedb-108">若要安裝 `dotnet-grpc` [.Net Core 通用工具](/dotnet/core/tools/global-tools)，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="ecedb-108">To install the `dotnet-grpc` [.NET Core Global Tool](/dotnet/core/tools/global-tools), run the following command:</span></span>

```dotnetcli
dotnet tool install -g dotnet-grpc
```

## <a name="add-references"></a><span data-ttu-id="ecedb-109">新增參考</span><span class="sxs-lookup"><span data-stu-id="ecedb-109">Add references</span></span>

<span data-ttu-id="ecedb-110">`dotnet-grpc`可以用來將 Protobuf 參考當做 `<Protobuf />` 專案新增至 *.csproj*檔案：</span><span class="sxs-lookup"><span data-stu-id="ecedb-110">`dotnet-grpc` can be used to add Protobuf references as `<Protobuf />` items to the *.csproj* file:</span></span>

```xml
<Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
```

<span data-ttu-id="ecedb-111">Protobuf 參考是用來產生 c # 用戶端和/或伺服器資產。</span><span class="sxs-lookup"><span data-stu-id="ecedb-111">The Protobuf references are used to generate the C# client and/or server assets.</span></span> <span data-ttu-id="ecedb-112">此 `dotnet-grpc` 工具可以：</span><span class="sxs-lookup"><span data-stu-id="ecedb-112">The `dotnet-grpc` tool can:</span></span>

* <span data-ttu-id="ecedb-113">從磁片上的本機檔案建立 Protobuf 參考。</span><span class="sxs-lookup"><span data-stu-id="ecedb-113">Create a Protobuf reference from local files on disk.</span></span>
* <span data-ttu-id="ecedb-114">從 URL 所指定的遠端檔案建立 Protobuf 參考。</span><span class="sxs-lookup"><span data-stu-id="ecedb-114">Create a Protobuf reference from a remote file specified by a URL.</span></span>
* <span data-ttu-id="ecedb-115">請確定已將正確的 gRPC 套件相依性新增至專案。</span><span class="sxs-lookup"><span data-stu-id="ecedb-115">Ensure the correct gRPC package dependencies are added to the project.</span></span>

<span data-ttu-id="ecedb-116">例如， `Grpc.AspNetCore` 封裝會新增至 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="ecedb-116">For example, the `Grpc.AspNetCore` package is added to a web app.</span></span> <span data-ttu-id="ecedb-117">`Grpc.AspNetCore`包含 gRPC 伺服器和用戶端程式庫和工具支援。</span><span class="sxs-lookup"><span data-stu-id="ecedb-117">`Grpc.AspNetCore` contains gRPC server and client libraries and tooling support.</span></span> <span data-ttu-id="ecedb-118">或者， `Grpc.Net.Client` `Grpc.Tools` `Google.Protobuf` 只包含 gRPC 用戶端程式庫和工具支援的、和套件會新增至主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="ecedb-118">Alternatively, the `Grpc.Net.Client`, `Grpc.Tools` and `Google.Protobuf` packages, which contain only the gRPC client libraries and tooling support, are added to a Console app.</span></span>

### <a name="add-file"></a><span data-ttu-id="ecedb-119">新增檔案</span><span class="sxs-lookup"><span data-stu-id="ecedb-119">Add file</span></span>

<span data-ttu-id="ecedb-120">`add-file`命令是用來將磁片上的本機檔案新增為 Protobuf 參考。</span><span class="sxs-lookup"><span data-stu-id="ecedb-120">The `add-file` command is used to add local files on disk as Protobuf references.</span></span> <span data-ttu-id="ecedb-121">提供的檔案路徑：</span><span class="sxs-lookup"><span data-stu-id="ecedb-121">The file paths provided:</span></span>

* <span data-ttu-id="ecedb-122">可以相對於目前的目錄或絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="ecedb-122">Can be relative to the current directory or absolute paths.</span></span>
* <span data-ttu-id="ecedb-123">可能包含以模式為基礎之檔案[通配](https://wikipedia.org/wiki/Glob_(programming))的萬用字元。</span><span class="sxs-lookup"><span data-stu-id="ecedb-123">May contain wild cards for pattern-based file [globbing](https://wikipedia.org/wiki/Glob_(programming)).</span></span>

<span data-ttu-id="ecedb-124">如果有任何檔案位於專案目錄外， `Link` 則會加入元素，以顯示 Visual Studio 資料夾下的檔案 `Protos` 。</span><span class="sxs-lookup"><span data-stu-id="ecedb-124">If any files are outside the project directory, a `Link` element is added to display the file under the folder `Protos` in Visual Studio.</span></span>

### <a name="usage"></a><span data-ttu-id="ecedb-125">使用方式</span><span class="sxs-lookup"><span data-stu-id="ecedb-125">Usage</span></span>

```dotnetcli
dotnet grpc add-file [options] <files>...
```

#### <a name="arguments"></a><span data-ttu-id="ecedb-126">引數</span><span class="sxs-lookup"><span data-stu-id="ecedb-126">Arguments</span></span>

| <span data-ttu-id="ecedb-127">引數</span><span class="sxs-lookup"><span data-stu-id="ecedb-127">Argument</span></span> | <span data-ttu-id="ecedb-128">說明</span><span class="sxs-lookup"><span data-stu-id="ecedb-128">Description</span></span> |
|-|-|
| <span data-ttu-id="ecedb-129">files</span><span class="sxs-lookup"><span data-stu-id="ecedb-129">files</span></span> | <span data-ttu-id="ecedb-130">Protobuf 檔案會參考。</span><span class="sxs-lookup"><span data-stu-id="ecedb-130">The protobuf file references.</span></span> <span data-ttu-id="ecedb-131">這些可以是本機 protobuf 檔案的 glob 路徑。</span><span class="sxs-lookup"><span data-stu-id="ecedb-131">These can be a path to glob for local protobuf files.</span></span> |

#### <a name="options"></a><span data-ttu-id="ecedb-132">選項</span><span class="sxs-lookup"><span data-stu-id="ecedb-132">Options</span></span>

| <span data-ttu-id="ecedb-133">Short 選項</span><span class="sxs-lookup"><span data-stu-id="ecedb-133">Short option</span></span> | <span data-ttu-id="ecedb-134">Long 選項</span><span class="sxs-lookup"><span data-stu-id="ecedb-134">Long option</span></span> | <span data-ttu-id="ecedb-135">說明</span><span class="sxs-lookup"><span data-stu-id="ecedb-135">Description</span></span> |
|-|-|-|
| <span data-ttu-id="ecedb-136">-p</span><span class="sxs-lookup"><span data-stu-id="ecedb-136">-p</span></span> | <span data-ttu-id="ecedb-137">--project</span><span class="sxs-lookup"><span data-stu-id="ecedb-137">--project</span></span> | <span data-ttu-id="ecedb-138">要操作之專案檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="ecedb-138">The path to the project file to operate on.</span></span> <span data-ttu-id="ecedb-139">如果未指定檔案，此命令會在目前的目錄中搜尋一個檔案。</span><span class="sxs-lookup"><span data-stu-id="ecedb-139">If a file is not specified, the command searches the current directory for one.</span></span>
| <span data-ttu-id="ecedb-140">-S</span><span class="sxs-lookup"><span data-stu-id="ecedb-140">-s</span></span> | <span data-ttu-id="ecedb-141">--服務</span><span class="sxs-lookup"><span data-stu-id="ecedb-141">--services</span></span> | <span data-ttu-id="ecedb-142">應產生的 gRPC 服務類型。</span><span class="sxs-lookup"><span data-stu-id="ecedb-142">The type of gRPC services that should be generated.</span></span> <span data-ttu-id="ecedb-143">如果 `Default` 指定， `Both` 則會用於 Web 專案，並 `Client` 用於非 Web 專案。</span><span class="sxs-lookup"><span data-stu-id="ecedb-143">If `Default` is specified, `Both` is used for Web projects and `Client` is used for non-Web projects.</span></span> <span data-ttu-id="ecedb-144">接受的值為 `Both` 、 `Client` 、 `Default` 、 `None` 、 `Server` 。</span><span class="sxs-lookup"><span data-stu-id="ecedb-144">Accepted values are `Both`, `Client`, `Default`, `None`, `Server`.</span></span>
| <span data-ttu-id="ecedb-145">-i</span><span class="sxs-lookup"><span data-stu-id="ecedb-145">-i</span></span> | <span data-ttu-id="ecedb-146">--其他-匯入-目錄</span><span class="sxs-lookup"><span data-stu-id="ecedb-146">--additional-import-dirs</span></span> | <span data-ttu-id="ecedb-147">解析 protobuf 檔案的匯入時，所要使用的其他目錄。</span><span class="sxs-lookup"><span data-stu-id="ecedb-147">Additional directories to be used when resolving imports for the protobuf files.</span></span> <span data-ttu-id="ecedb-148">這是以分號分隔的路徑清單。</span><span class="sxs-lookup"><span data-stu-id="ecedb-148">This is a semicolon separated list of paths.</span></span>
| | <span data-ttu-id="ecedb-149">--access</span><span class="sxs-lookup"><span data-stu-id="ecedb-149">--access</span></span> | <span data-ttu-id="ecedb-150">要用於產生之 c # 類別的存取修飾詞。</span><span class="sxs-lookup"><span data-stu-id="ecedb-150">The access modifier to use for the generated C# classes.</span></span> <span data-ttu-id="ecedb-151">預設值為 `Public`。</span><span class="sxs-lookup"><span data-stu-id="ecedb-151">The default value is `Public`.</span></span> <span data-ttu-id="ecedb-152">接受的值為 `Internal` 和 `Public`。</span><span class="sxs-lookup"><span data-stu-id="ecedb-152">Accepted values are `Internal` and `Public`.</span></span>

### <a name="add-url"></a><span data-ttu-id="ecedb-153">新增 URL</span><span class="sxs-lookup"><span data-stu-id="ecedb-153">Add URL</span></span>

<span data-ttu-id="ecedb-154">`add-url`命令是用來將來源 URL 所指定的遠端檔案新增為 Protobuf 參考。</span><span class="sxs-lookup"><span data-stu-id="ecedb-154">The `add-url` command is used to add a remote file specified by an source URL as Protobuf reference.</span></span> <span data-ttu-id="ecedb-155">必須提供檔案路徑，以指定要下載遠端檔案的位置。</span><span class="sxs-lookup"><span data-stu-id="ecedb-155">A file path must be provided to specify where to download the remote file.</span></span> <span data-ttu-id="ecedb-156">檔案路徑可以相對於目前的目錄或絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="ecedb-156">The file path can be relative to the current directory or an absolute path.</span></span> <span data-ttu-id="ecedb-157">如果檔案路徑位於專案目錄外， `Link` 則會加入元素，以在 Visual Studio 中的虛擬資料夾底下顯示檔案 `Protos` 。</span><span class="sxs-lookup"><span data-stu-id="ecedb-157">If the file path is outside the project directory, a `Link` element is added to display the file under the virtual folder `Protos` in Visual Studio.</span></span>

### <a name="usage"></a><span data-ttu-id="ecedb-158">使用方式</span><span class="sxs-lookup"><span data-stu-id="ecedb-158">Usage</span></span>

```dotnetcli
dotnet-grpc add-url [options] <url>
```

#### <a name="arguments"></a><span data-ttu-id="ecedb-159">引數</span><span class="sxs-lookup"><span data-stu-id="ecedb-159">Arguments</span></span>

| <span data-ttu-id="ecedb-160">引數</span><span class="sxs-lookup"><span data-stu-id="ecedb-160">Argument</span></span> | <span data-ttu-id="ecedb-161">說明</span><span class="sxs-lookup"><span data-stu-id="ecedb-161">Description</span></span> |
|-|-|
| <span data-ttu-id="ecedb-162">URL</span><span class="sxs-lookup"><span data-stu-id="ecedb-162">url</span></span> | <span data-ttu-id="ecedb-163">遠端 protobuf 檔的 URL。</span><span class="sxs-lookup"><span data-stu-id="ecedb-163">The URL to a remote protobuf file.</span></span> |

#### <a name="options"></a><span data-ttu-id="ecedb-164">選項</span><span class="sxs-lookup"><span data-stu-id="ecedb-164">Options</span></span>

| <span data-ttu-id="ecedb-165">Short 選項</span><span class="sxs-lookup"><span data-stu-id="ecedb-165">Short option</span></span> | <span data-ttu-id="ecedb-166">Long 選項</span><span class="sxs-lookup"><span data-stu-id="ecedb-166">Long option</span></span> | <span data-ttu-id="ecedb-167">說明</span><span class="sxs-lookup"><span data-stu-id="ecedb-167">Description</span></span> |
|-|-|-|
| <span data-ttu-id="ecedb-168">-o</span><span class="sxs-lookup"><span data-stu-id="ecedb-168">-o</span></span> | <span data-ttu-id="ecedb-169">--output</span><span class="sxs-lookup"><span data-stu-id="ecedb-169">--output</span></span> | <span data-ttu-id="ecedb-170">指定遠端 protobuf 檔的下載路徑。</span><span class="sxs-lookup"><span data-stu-id="ecedb-170">Specifies the download path for the remote protobuf file.</span></span> <span data-ttu-id="ecedb-171">這是必要選項。</span><span class="sxs-lookup"><span data-stu-id="ecedb-171">This is a required option.</span></span>
| <span data-ttu-id="ecedb-172">-p</span><span class="sxs-lookup"><span data-stu-id="ecedb-172">-p</span></span> | <span data-ttu-id="ecedb-173">--project</span><span class="sxs-lookup"><span data-stu-id="ecedb-173">--project</span></span> | <span data-ttu-id="ecedb-174">要操作之專案檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="ecedb-174">The path to the project file to operate on.</span></span> <span data-ttu-id="ecedb-175">如果未指定檔案，此命令會在目前的目錄中搜尋一個檔案。</span><span class="sxs-lookup"><span data-stu-id="ecedb-175">If a file is not specified, the command searches the current directory for one.</span></span>
| <span data-ttu-id="ecedb-176">-S</span><span class="sxs-lookup"><span data-stu-id="ecedb-176">-s</span></span> | <span data-ttu-id="ecedb-177">--服務</span><span class="sxs-lookup"><span data-stu-id="ecedb-177">--services</span></span> | <span data-ttu-id="ecedb-178">應產生的 gRPC 服務類型。</span><span class="sxs-lookup"><span data-stu-id="ecedb-178">The type of gRPC services that should be generated.</span></span> <span data-ttu-id="ecedb-179">如果 `Default` 指定， `Both` 則會用於 Web 專案，並 `Client` 用於非 Web 專案。</span><span class="sxs-lookup"><span data-stu-id="ecedb-179">If `Default` is specified, `Both` is used for Web projects and `Client` is used for non-Web projects.</span></span> <span data-ttu-id="ecedb-180">接受的值為 `Both` 、 `Client` 、 `Default` 、 `None` 、 `Server` 。</span><span class="sxs-lookup"><span data-stu-id="ecedb-180">Accepted values are `Both`, `Client`, `Default`, `None`, `Server`.</span></span>
| <span data-ttu-id="ecedb-181">-i</span><span class="sxs-lookup"><span data-stu-id="ecedb-181">-i</span></span> | <span data-ttu-id="ecedb-182">--其他-匯入-目錄</span><span class="sxs-lookup"><span data-stu-id="ecedb-182">--additional-import-dirs</span></span> | <span data-ttu-id="ecedb-183">解析 protobuf 檔案的匯入時，所要使用的其他目錄。</span><span class="sxs-lookup"><span data-stu-id="ecedb-183">Additional directories to be used when resolving imports for the protobuf files.</span></span> <span data-ttu-id="ecedb-184">這是以分號分隔的路徑清單。</span><span class="sxs-lookup"><span data-stu-id="ecedb-184">This is a semicolon separated list of paths.</span></span>
| | <span data-ttu-id="ecedb-185">--access</span><span class="sxs-lookup"><span data-stu-id="ecedb-185">--access</span></span> | <span data-ttu-id="ecedb-186">要用於產生之 c # 類別的存取修飾詞。</span><span class="sxs-lookup"><span data-stu-id="ecedb-186">The access modifier to use for the generated C# classes.</span></span> <span data-ttu-id="ecedb-187">預設值為 `Public`。</span><span class="sxs-lookup"><span data-stu-id="ecedb-187">Default value is `Public`.</span></span> <span data-ttu-id="ecedb-188">接受的值為 `Internal` 和 `Public`。</span><span class="sxs-lookup"><span data-stu-id="ecedb-188">Accepted values are `Internal` and `Public`.</span></span>

## <a name="remove"></a><span data-ttu-id="ecedb-189">移除</span><span class="sxs-lookup"><span data-stu-id="ecedb-189">Remove</span></span>

<span data-ttu-id="ecedb-190">`remove`命令是用來從 *.csproj*檔案中移除 Protobuf 參考。</span><span class="sxs-lookup"><span data-stu-id="ecedb-190">The `remove` command is used to remove Protobuf references from the *.csproj* file.</span></span> <span data-ttu-id="ecedb-191">命令接受路徑引數和來源 Url 做為引數。</span><span class="sxs-lookup"><span data-stu-id="ecedb-191">The command accepts path arguments and source URLs as arguments.</span></span> <span data-ttu-id="ecedb-192">工具：</span><span class="sxs-lookup"><span data-stu-id="ecedb-192">The tool:</span></span>

* <span data-ttu-id="ecedb-193">只會移除 Protobuf 參考。</span><span class="sxs-lookup"><span data-stu-id="ecedb-193">Only removes the Protobuf reference.</span></span>
* <span data-ttu-id="ecedb-194">不會刪除此*proto*檔案，即使該檔案原本是從遠端 URL 下載也一樣。</span><span class="sxs-lookup"><span data-stu-id="ecedb-194">Does not delete the *.proto* file, even if it was originally downloaded from a remote URL.</span></span>

### <a name="usage"></a><span data-ttu-id="ecedb-195">使用方式</span><span class="sxs-lookup"><span data-stu-id="ecedb-195">Usage</span></span>

```dotnetcli
dotnet-grpc remove [options] <references>...
```

### <a name="arguments"></a><span data-ttu-id="ecedb-196">引數</span><span class="sxs-lookup"><span data-stu-id="ecedb-196">Arguments</span></span>

| <span data-ttu-id="ecedb-197">引數</span><span class="sxs-lookup"><span data-stu-id="ecedb-197">Argument</span></span> | <span data-ttu-id="ecedb-198">說明</span><span class="sxs-lookup"><span data-stu-id="ecedb-198">Description</span></span> |
|-|-|
| <span data-ttu-id="ecedb-199">參考</span><span class="sxs-lookup"><span data-stu-id="ecedb-199">references</span></span> | <span data-ttu-id="ecedb-200">要移除之 protobuf 參考的 Url 或檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="ecedb-200">The URLs or file paths of the protobuf references to remove.</span></span> |

### <a name="options"></a><span data-ttu-id="ecedb-201">選項</span><span class="sxs-lookup"><span data-stu-id="ecedb-201">Options</span></span>

| <span data-ttu-id="ecedb-202">Short 選項</span><span class="sxs-lookup"><span data-stu-id="ecedb-202">Short option</span></span> | <span data-ttu-id="ecedb-203">Long 選項</span><span class="sxs-lookup"><span data-stu-id="ecedb-203">Long option</span></span> | <span data-ttu-id="ecedb-204">說明</span><span class="sxs-lookup"><span data-stu-id="ecedb-204">Description</span></span> |
|-|-|-|
| <span data-ttu-id="ecedb-205">-p</span><span class="sxs-lookup"><span data-stu-id="ecedb-205">-p</span></span> | <span data-ttu-id="ecedb-206">--project</span><span class="sxs-lookup"><span data-stu-id="ecedb-206">--project</span></span> | <span data-ttu-id="ecedb-207">要操作之專案檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="ecedb-207">The path to the project file to operate on.</span></span> <span data-ttu-id="ecedb-208">如果未指定檔案，此命令會在目前的目錄中搜尋一個檔案。</span><span class="sxs-lookup"><span data-stu-id="ecedb-208">If a file is not specified, the command searches the current directory for one.</span></span>

## <a name="refresh"></a><span data-ttu-id="ecedb-209">重新整理</span><span class="sxs-lookup"><span data-stu-id="ecedb-209">Refresh</span></span>

<span data-ttu-id="ecedb-210">`refresh`命令是用來以來源 URL 的最新內容來更新遠端參考。</span><span class="sxs-lookup"><span data-stu-id="ecedb-210">The `refresh` command is used to update a remote reference with the latest content from the source URL.</span></span> <span data-ttu-id="ecedb-211">下載檔案路徑和來源 URL 都可以用來指定要更新的參考。</span><span class="sxs-lookup"><span data-stu-id="ecedb-211">Both the download file path and the source URL can be used to specify the reference to be updated.</span></span> <span data-ttu-id="ecedb-212">注意:</span><span class="sxs-lookup"><span data-stu-id="ecedb-212">Note:</span></span>

* <span data-ttu-id="ecedb-213">檔案內容的雜湊會進行比較，以判斷是否應該更新本機檔案。</span><span class="sxs-lookup"><span data-stu-id="ecedb-213">The hashes of the file contents are compared to determine whether the local file should be updated.</span></span>
* <span data-ttu-id="ecedb-214">不會比較任何時間戳記資訊。</span><span class="sxs-lookup"><span data-stu-id="ecedb-214">No timestamp information is compared.</span></span>

<span data-ttu-id="ecedb-215">如果需要更新，此工具一律會將本機檔案取代為遠端檔案。</span><span class="sxs-lookup"><span data-stu-id="ecedb-215">The tool always replaces the local file with the remote file if an update is needed.</span></span>

### <a name="usage"></a><span data-ttu-id="ecedb-216">使用方式</span><span class="sxs-lookup"><span data-stu-id="ecedb-216">Usage</span></span>

```dotnetcli
dotnet-grpc refresh [options] [<references>...]
```

### <a name="arguments"></a><span data-ttu-id="ecedb-217">引數</span><span class="sxs-lookup"><span data-stu-id="ecedb-217">Arguments</span></span>

| <span data-ttu-id="ecedb-218">引數</span><span class="sxs-lookup"><span data-stu-id="ecedb-218">Argument</span></span> | <span data-ttu-id="ecedb-219">說明</span><span class="sxs-lookup"><span data-stu-id="ecedb-219">Description</span></span> |
|-|-|
| <span data-ttu-id="ecedb-220">參考</span><span class="sxs-lookup"><span data-stu-id="ecedb-220">references</span></span> | <span data-ttu-id="ecedb-221">應更新之遠端 protobuf 參考的 Url 或檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="ecedb-221">The URLs or file paths to remote protobuf references that should be updated.</span></span> <span data-ttu-id="ecedb-222">將此引數保留空白以重新整理所有遠端參考。</span><span class="sxs-lookup"><span data-stu-id="ecedb-222">Leave this argument empty to refresh all remote references.</span></span> |

### <a name="options"></a><span data-ttu-id="ecedb-223">選項</span><span class="sxs-lookup"><span data-stu-id="ecedb-223">Options</span></span>

| <span data-ttu-id="ecedb-224">Short 選項</span><span class="sxs-lookup"><span data-stu-id="ecedb-224">Short option</span></span> | <span data-ttu-id="ecedb-225">Long 選項</span><span class="sxs-lookup"><span data-stu-id="ecedb-225">Long option</span></span> | <span data-ttu-id="ecedb-226">說明</span><span class="sxs-lookup"><span data-stu-id="ecedb-226">Description</span></span> |
|-|-|-|
| <span data-ttu-id="ecedb-227">-p</span><span class="sxs-lookup"><span data-stu-id="ecedb-227">-p</span></span> | <span data-ttu-id="ecedb-228">--project</span><span class="sxs-lookup"><span data-stu-id="ecedb-228">--project</span></span> | <span data-ttu-id="ecedb-229">要操作之專案檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="ecedb-229">The path to the project file to operate on.</span></span> <span data-ttu-id="ecedb-230">如果未指定檔案，此命令會在目前的目錄中搜尋一個檔案。</span><span class="sxs-lookup"><span data-stu-id="ecedb-230">If a file is not specified, the command searches the current directory for one.</span></span>
| | <span data-ttu-id="ecedb-231">--試執行</span><span class="sxs-lookup"><span data-stu-id="ecedb-231">--dry-run</span></span> | <span data-ttu-id="ecedb-232">輸出會更新而不下載任何新內容的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="ecedb-232">Outputs a list of files that would be updated without downloading any new content.</span></span>

## <a name="list"></a><span data-ttu-id="ecedb-233">清單</span><span class="sxs-lookup"><span data-stu-id="ecedb-233">List</span></span>

<span data-ttu-id="ecedb-234">`list`命令是用來顯示專案檔中的所有 Protobuf 參考。</span><span class="sxs-lookup"><span data-stu-id="ecedb-234">The `list` command is used to display all the Protobuf references in the project file.</span></span> <span data-ttu-id="ecedb-235">如果資料行的所有值都是預設值，可能會省略資料行。</span><span class="sxs-lookup"><span data-stu-id="ecedb-235">If all values of a column are default values, the column may be omitted.</span></span>

### <a name="usage"></a><span data-ttu-id="ecedb-236">使用方式</span><span class="sxs-lookup"><span data-stu-id="ecedb-236">Usage</span></span>

```dotnetcli
dotnet-grpc list [options]
```

### <a name="options"></a><span data-ttu-id="ecedb-237">選項</span><span class="sxs-lookup"><span data-stu-id="ecedb-237">Options</span></span>

| <span data-ttu-id="ecedb-238">Short 選項</span><span class="sxs-lookup"><span data-stu-id="ecedb-238">Short option</span></span> | <span data-ttu-id="ecedb-239">Long 選項</span><span class="sxs-lookup"><span data-stu-id="ecedb-239">Long option</span></span> | <span data-ttu-id="ecedb-240">說明</span><span class="sxs-lookup"><span data-stu-id="ecedb-240">Description</span></span> |
|-|-|-|
| <span data-ttu-id="ecedb-241">-p</span><span class="sxs-lookup"><span data-stu-id="ecedb-241">-p</span></span> | <span data-ttu-id="ecedb-242">--project</span><span class="sxs-lookup"><span data-stu-id="ecedb-242">--project</span></span> | <span data-ttu-id="ecedb-243">要操作之專案檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="ecedb-243">The path to the project file to operate on.</span></span> <span data-ttu-id="ecedb-244">如果未指定檔案，此命令會在目前的目錄中搜尋一個檔案。</span><span class="sxs-lookup"><span data-stu-id="ecedb-244">If a file is not specified, the command searches the current directory for one.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ecedb-245">其他資源</span><span class="sxs-lookup"><span data-stu-id="ecedb-245">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
