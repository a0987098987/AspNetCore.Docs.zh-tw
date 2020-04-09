---
title: 使用 dotnet-grpc 管理 Protobuf 參考
author: juntaoluo
description: 瞭解如何使用 dotnet-grpc 全域工具添加、更新、刪除和列出 Protobuf 引用。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 10/17/2019
uid: grpc/dotnet-grpc
ms.openlocfilehash: 994597c854a95bb33de1686ab025cb3744cf6845
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78667332"
---
# <a name="manage-protobuf-references-with-dotnet-grpc"></a><span data-ttu-id="c44d3-103">使用 dotnet-grpc 管理 Protobuf 參考</span><span class="sxs-lookup"><span data-stu-id="c44d3-103">Manage Protobuf references with dotnet-grpc</span></span>

<span data-ttu-id="c44d3-104">作者：[John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="c44d3-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="c44d3-105">`dotnet-grpc`是 .NET 核心全域工具,用於管理 .NET gRPC 專案中[的 Protobuf *(.proto*)](xref:grpc/basics#proto-file)引用。</span><span class="sxs-lookup"><span data-stu-id="c44d3-105">`dotnet-grpc` is a .NET Core Global Tool for managing [Protobuf (*.proto*)](xref:grpc/basics#proto-file) references within a .NET gRPC project.</span></span> <span data-ttu-id="c44d3-106">該工具可用於添加、刷新、刪除和列出 Protobuf 引用。</span><span class="sxs-lookup"><span data-stu-id="c44d3-106">The tool can be used to add, refresh, remove, and list Protobuf references.</span></span>

## <a name="installation"></a><span data-ttu-id="c44d3-107">安裝</span><span class="sxs-lookup"><span data-stu-id="c44d3-107">Installation</span></span>

<span data-ttu-id="c44d3-108">要安裝`dotnet-grpc` [.NET 核心全域工具](/dotnet/core/tools/global-tools),請執行以下指令:</span><span class="sxs-lookup"><span data-stu-id="c44d3-108">To install the `dotnet-grpc` [.NET Core Global Tool](/dotnet/core/tools/global-tools), run the following command:</span></span>

```dotnetcli
dotnet tool install -g dotnet-grpc
```

## <a name="add-references"></a><span data-ttu-id="c44d3-109">新增參考</span><span class="sxs-lookup"><span data-stu-id="c44d3-109">Add references</span></span>

<span data-ttu-id="c44d3-110">`dotnet-grpc`可用於將 Protobuf`<Protobuf />`引用為項目新增*到 .csproj*檔:</span><span class="sxs-lookup"><span data-stu-id="c44d3-110">`dotnet-grpc` can be used to add Protobuf references as `<Protobuf />` items to the *.csproj* file:</span></span>

```xml
<Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
```

<span data-ttu-id="c44d3-111">Protobuf 引用用於生成 C# 用戶端和/或伺服器資產。</span><span class="sxs-lookup"><span data-stu-id="c44d3-111">The Protobuf references are used to generate the C# client and/or server assets.</span></span> <span data-ttu-id="c44d3-112">該工具`dotnet-grpc`可以:</span><span class="sxs-lookup"><span data-stu-id="c44d3-112">The `dotnet-grpc` tool can:</span></span>

* <span data-ttu-id="c44d3-113">從磁碟上的本地檔創建 Protobuf 引用。</span><span class="sxs-lookup"><span data-stu-id="c44d3-113">Create a Protobuf reference from local files on disk.</span></span>
* <span data-ttu-id="c44d3-114">從 URL 指定的遠端檔案中創建 Protobuf 引用。</span><span class="sxs-lookup"><span data-stu-id="c44d3-114">Create a Protobuf reference from a remote file specified by a URL.</span></span>
* <span data-ttu-id="c44d3-115">確保向專案添加了正確的 gRPC 包依賴項。</span><span class="sxs-lookup"><span data-stu-id="c44d3-115">Ensure the correct gRPC package dependencies are added to the project.</span></span>

<span data-ttu-id="c44d3-116">例如,包`Grpc.AspNetCore`將添加到 Web 應用。</span><span class="sxs-lookup"><span data-stu-id="c44d3-116">For example, the `Grpc.AspNetCore` package is added to a web app.</span></span> <span data-ttu-id="c44d3-117">`Grpc.AspNetCore`包含 gRPC 伺服器和用戶端庫以及工具支援。</span><span class="sxs-lookup"><span data-stu-id="c44d3-117">`Grpc.AspNetCore` contains gRPC server and client libraries and tooling support.</span></span> <span data-ttu-id="c44d3-118">或者,`Grpc.Net.Client`僅`Grpc.Tools``Google.Protobuf`包含 gRPC 用戶端庫和工具支援的 的和 包將添加到主控台應用。</span><span class="sxs-lookup"><span data-stu-id="c44d3-118">Alternatively, the `Grpc.Net.Client`, `Grpc.Tools` and `Google.Protobuf` packages, which contain only the gRPC client libraries and tooling support, are added to a Console app.</span></span>

### <a name="add-file"></a><span data-ttu-id="c44d3-119">新增檔案</span><span class="sxs-lookup"><span data-stu-id="c44d3-119">Add file</span></span>

<span data-ttu-id="c44d3-120">該`add-file`命令用於在磁碟上添加本地檔作為 Protobuf 引用。</span><span class="sxs-lookup"><span data-stu-id="c44d3-120">The `add-file` command is used to add local files on disk as Protobuf references.</span></span> <span data-ttu-id="c44d3-121">提供的檔案路徑:</span><span class="sxs-lookup"><span data-stu-id="c44d3-121">The file paths provided:</span></span>

* <span data-ttu-id="c44d3-122">可以相對於當前目錄或絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="c44d3-122">Can be relative to the current directory or absolute paths.</span></span>
* <span data-ttu-id="c44d3-123">可能包含基於模式的檔案[globing](https://wikipedia.org/wiki/Glob_(programming))的通配符。</span><span class="sxs-lookup"><span data-stu-id="c44d3-123">May contain wild cards for pattern-based file [globbing](https://wikipedia.org/wiki/Glob_(programming)).</span></span>

<span data-ttu-id="c44d3-124">如果專案目錄之外有任何檔,則添加一`Link`個元素以在 Visual Studio 中的`Protos`資料夾下顯示該檔。</span><span class="sxs-lookup"><span data-stu-id="c44d3-124">If any files are outside the project directory, a `Link` element is added to display the file under the folder `Protos` in Visual Studio.</span></span>

### <a name="usage"></a><span data-ttu-id="c44d3-125">使用量</span><span class="sxs-lookup"><span data-stu-id="c44d3-125">Usage</span></span>

```dotnetcli
dotnet grpc add-file [options] <files>...
```

#### <a name="arguments"></a><span data-ttu-id="c44d3-126">引數</span><span class="sxs-lookup"><span data-stu-id="c44d3-126">Arguments</span></span>

| <span data-ttu-id="c44d3-127">引數</span><span class="sxs-lookup"><span data-stu-id="c44d3-127">Argument</span></span> | <span data-ttu-id="c44d3-128">描述</span><span class="sxs-lookup"><span data-stu-id="c44d3-128">Description</span></span> |
|-|-|
| <span data-ttu-id="c44d3-129">files</span><span class="sxs-lookup"><span data-stu-id="c44d3-129">files</span></span> | <span data-ttu-id="c44d3-130">原檔引用。</span><span class="sxs-lookup"><span data-stu-id="c44d3-130">The protobuf file references.</span></span> <span data-ttu-id="c44d3-131">這些可以是本地原體檔 glob 的路徑。</span><span class="sxs-lookup"><span data-stu-id="c44d3-131">These can be a path to glob for local protobuf files.</span></span> |

#### <a name="options"></a><span data-ttu-id="c44d3-132">選項。</span><span class="sxs-lookup"><span data-stu-id="c44d3-132">Options</span></span>

| <span data-ttu-id="c44d3-133">短選項</span><span class="sxs-lookup"><span data-stu-id="c44d3-133">Short option</span></span> | <span data-ttu-id="c44d3-134">長選項</span><span class="sxs-lookup"><span data-stu-id="c44d3-134">Long option</span></span> | <span data-ttu-id="c44d3-135">描述</span><span class="sxs-lookup"><span data-stu-id="c44d3-135">Description</span></span> |
|-|-|-|
| <span data-ttu-id="c44d3-136">-p</span><span class="sxs-lookup"><span data-stu-id="c44d3-136">-p</span></span> | <span data-ttu-id="c44d3-137">--專案</span><span class="sxs-lookup"><span data-stu-id="c44d3-137">--project</span></span> | <span data-ttu-id="c44d3-138">要操作的專案檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="c44d3-138">The path to the project file to operate on.</span></span> <span data-ttu-id="c44d3-139">如果未指定檔,該命令將搜索當前目錄。</span><span class="sxs-lookup"><span data-stu-id="c44d3-139">If a file is not specified, the command searches the current directory for one.</span></span>
| <span data-ttu-id="c44d3-140">-S</span><span class="sxs-lookup"><span data-stu-id="c44d3-140">-s</span></span> | <span data-ttu-id="c44d3-141">--服務</span><span class="sxs-lookup"><span data-stu-id="c44d3-141">--services</span></span> | <span data-ttu-id="c44d3-142">應生成的 gRPC 服務的類型。</span><span class="sxs-lookup"><span data-stu-id="c44d3-142">The type of gRPC services that should be generated.</span></span> <span data-ttu-id="c44d3-143">如果`Default`指定,`Both`則用於 Web`Client`專案, 用於非 Web 專案。</span><span class="sxs-lookup"><span data-stu-id="c44d3-143">If `Default` is specified, `Both` is used for Web projects and `Client` is used for non-Web projects.</span></span> <span data-ttu-id="c44d3-144">接受的值是`Both``Client` `Default` `None`, `Server`, , , , , , ,</span><span class="sxs-lookup"><span data-stu-id="c44d3-144">Accepted values are `Both`, `Client`, `Default`, `None`, `Server`.</span></span>
| <span data-ttu-id="c44d3-145">-i</span><span class="sxs-lookup"><span data-stu-id="c44d3-145">-i</span></span> | <span data-ttu-id="c44d3-146">--附加導入-迪爾</span><span class="sxs-lookup"><span data-stu-id="c44d3-146">--additional-import-dirs</span></span> | <span data-ttu-id="c44d3-147">解析原文件導入時要使用的其他目錄。</span><span class="sxs-lookup"><span data-stu-id="c44d3-147">Additional directories to be used when resolving imports for the protobuf files.</span></span> <span data-ttu-id="c44d3-148">這是路徑的分號分隔清單。</span><span class="sxs-lookup"><span data-stu-id="c44d3-148">This is a semicolon separated list of paths.</span></span>
| | <span data-ttu-id="c44d3-149">--訪問</span><span class="sxs-lookup"><span data-stu-id="c44d3-149">--access</span></span> | <span data-ttu-id="c44d3-150">用於生成的 C# 類別的訪問修改器。</span><span class="sxs-lookup"><span data-stu-id="c44d3-150">The access modifier to use for the generated C# classes.</span></span> <span data-ttu-id="c44d3-151">預設值是 `Public`。</span><span class="sxs-lookup"><span data-stu-id="c44d3-151">The default value is `Public`.</span></span> <span data-ttu-id="c44d3-152">接受的值為 `Internal` 和 `Public`。</span><span class="sxs-lookup"><span data-stu-id="c44d3-152">Accepted values are `Internal` and `Public`.</span></span>

### <a name="add-url"></a><span data-ttu-id="c44d3-153">新增 URL</span><span class="sxs-lookup"><span data-stu-id="c44d3-153">Add URL</span></span>

<span data-ttu-id="c44d3-154">該`add-url`指令用於添加源網址指定的遠端檔案作為 Protobuf 引用。</span><span class="sxs-lookup"><span data-stu-id="c44d3-154">The `add-url` command is used to add a remote file specified by an source URL as Protobuf reference.</span></span> <span data-ttu-id="c44d3-155">必須提供檔案路徑以指定下載遠端檔案的位置。</span><span class="sxs-lookup"><span data-stu-id="c44d3-155">A file path must be provided to specify where to download the remote file.</span></span> <span data-ttu-id="c44d3-156">檔路徑可以是相對於當前目錄或絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="c44d3-156">The file path can be relative to the current directory or an absolute path.</span></span> <span data-ttu-id="c44d3-157">如果檔案路徑位於項目目錄之外,則添加一`Link`個元素以在 Visual Studio`Protos`中的虛擬 資料夾下顯示該檔。</span><span class="sxs-lookup"><span data-stu-id="c44d3-157">If the file path is outside the project directory, a `Link` element is added to display the file under the virtual folder `Protos` in Visual Studio.</span></span>

### <a name="usage"></a><span data-ttu-id="c44d3-158">使用量</span><span class="sxs-lookup"><span data-stu-id="c44d3-158">Usage</span></span>

```dotnetcli
dotnet-grpc add-url [options] <url>
```

#### <a name="arguments"></a><span data-ttu-id="c44d3-159">引數</span><span class="sxs-lookup"><span data-stu-id="c44d3-159">Arguments</span></span>

| <span data-ttu-id="c44d3-160">引數</span><span class="sxs-lookup"><span data-stu-id="c44d3-160">Argument</span></span> | <span data-ttu-id="c44d3-161">描述</span><span class="sxs-lookup"><span data-stu-id="c44d3-161">Description</span></span> |
|-|-|
| <span data-ttu-id="c44d3-162">url</span><span class="sxs-lookup"><span data-stu-id="c44d3-162">url</span></span> | <span data-ttu-id="c44d3-163">遠端原檔案 URL。</span><span class="sxs-lookup"><span data-stu-id="c44d3-163">The URL to a remote protobuf file.</span></span> |

#### <a name="options"></a><span data-ttu-id="c44d3-164">選項。</span><span class="sxs-lookup"><span data-stu-id="c44d3-164">Options</span></span>

| <span data-ttu-id="c44d3-165">短選項</span><span class="sxs-lookup"><span data-stu-id="c44d3-165">Short option</span></span> | <span data-ttu-id="c44d3-166">長選項</span><span class="sxs-lookup"><span data-stu-id="c44d3-166">Long option</span></span> | <span data-ttu-id="c44d3-167">描述</span><span class="sxs-lookup"><span data-stu-id="c44d3-167">Description</span></span> |
|-|-|-|
| <span data-ttu-id="c44d3-168">-o</span><span class="sxs-lookup"><span data-stu-id="c44d3-168">-o</span></span> | <span data-ttu-id="c44d3-169">--output</span><span class="sxs-lookup"><span data-stu-id="c44d3-169">--output</span></span> | <span data-ttu-id="c44d3-170">指定遠端原檔案的下載路徑。</span><span class="sxs-lookup"><span data-stu-id="c44d3-170">Specifies the download path for the remote protobuf file.</span></span> <span data-ttu-id="c44d3-171">這是必要選項。</span><span class="sxs-lookup"><span data-stu-id="c44d3-171">This is a required option.</span></span>
| <span data-ttu-id="c44d3-172">-p</span><span class="sxs-lookup"><span data-stu-id="c44d3-172">-p</span></span> | <span data-ttu-id="c44d3-173">--專案</span><span class="sxs-lookup"><span data-stu-id="c44d3-173">--project</span></span> | <span data-ttu-id="c44d3-174">要操作的專案檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="c44d3-174">The path to the project file to operate on.</span></span> <span data-ttu-id="c44d3-175">如果未指定檔,該命令將搜索當前目錄。</span><span class="sxs-lookup"><span data-stu-id="c44d3-175">If a file is not specified, the command searches the current directory for one.</span></span>
| <span data-ttu-id="c44d3-176">-S</span><span class="sxs-lookup"><span data-stu-id="c44d3-176">-s</span></span> | <span data-ttu-id="c44d3-177">--服務</span><span class="sxs-lookup"><span data-stu-id="c44d3-177">--services</span></span> | <span data-ttu-id="c44d3-178">應生成的 gRPC 服務的類型。</span><span class="sxs-lookup"><span data-stu-id="c44d3-178">The type of gRPC services that should be generated.</span></span> <span data-ttu-id="c44d3-179">如果`Default`指定,`Both`則用於 Web`Client`專案, 用於非 Web 專案。</span><span class="sxs-lookup"><span data-stu-id="c44d3-179">If `Default` is specified, `Both` is used for Web projects and `Client` is used for non-Web projects.</span></span> <span data-ttu-id="c44d3-180">接受的值是`Both``Client` `Default` `None`, `Server`, , , , , , ,</span><span class="sxs-lookup"><span data-stu-id="c44d3-180">Accepted values are `Both`, `Client`, `Default`, `None`, `Server`.</span></span>
| <span data-ttu-id="c44d3-181">-i</span><span class="sxs-lookup"><span data-stu-id="c44d3-181">-i</span></span> | <span data-ttu-id="c44d3-182">--附加導入-迪爾</span><span class="sxs-lookup"><span data-stu-id="c44d3-182">--additional-import-dirs</span></span> | <span data-ttu-id="c44d3-183">解析原文件導入時要使用的其他目錄。</span><span class="sxs-lookup"><span data-stu-id="c44d3-183">Additional directories to be used when resolving imports for the protobuf files.</span></span> <span data-ttu-id="c44d3-184">這是路徑的分號分隔清單。</span><span class="sxs-lookup"><span data-stu-id="c44d3-184">This is a semicolon separated list of paths.</span></span>
| | <span data-ttu-id="c44d3-185">--訪問</span><span class="sxs-lookup"><span data-stu-id="c44d3-185">--access</span></span> | <span data-ttu-id="c44d3-186">用於生成的 C# 類別的訪問修改器。</span><span class="sxs-lookup"><span data-stu-id="c44d3-186">The access modifier to use for the generated C# classes.</span></span> <span data-ttu-id="c44d3-187">預設值為 `Public`。</span><span class="sxs-lookup"><span data-stu-id="c44d3-187">Default value is `Public`.</span></span> <span data-ttu-id="c44d3-188">接受的值為 `Internal` 和 `Public`。</span><span class="sxs-lookup"><span data-stu-id="c44d3-188">Accepted values are `Internal` and `Public`.</span></span>

## <a name="remove"></a><span data-ttu-id="c44d3-189">移除</span><span class="sxs-lookup"><span data-stu-id="c44d3-189">Remove</span></span>

<span data-ttu-id="c44d3-190">該`remove`命令用於從 *.csproj*檔中刪除 Protobuf 引用。</span><span class="sxs-lookup"><span data-stu-id="c44d3-190">The `remove` command is used to remove Protobuf references from the *.csproj* file.</span></span> <span data-ttu-id="c44d3-191">該命令接受路徑參數和源 URL 作為參數。</span><span class="sxs-lookup"><span data-stu-id="c44d3-191">The command accepts path arguments and source URLs as arguments.</span></span> <span data-ttu-id="c44d3-192">該工具:</span><span class="sxs-lookup"><span data-stu-id="c44d3-192">The tool:</span></span>

* <span data-ttu-id="c44d3-193">僅刪除 Protobuf 引用。</span><span class="sxs-lookup"><span data-stu-id="c44d3-193">Only removes the Protobuf reference.</span></span>
* <span data-ttu-id="c44d3-194">不會刪除 *.proto*檔,即使它最初是從遠端 URL 下載的。</span><span class="sxs-lookup"><span data-stu-id="c44d3-194">Does not delete the *.proto* file, even if it was originally downloaded from a remote URL.</span></span>

### <a name="usage"></a><span data-ttu-id="c44d3-195">使用量</span><span class="sxs-lookup"><span data-stu-id="c44d3-195">Usage</span></span>

```dotnetcli
dotnet-grpc remove [options] <references>...
```

### <a name="arguments"></a><span data-ttu-id="c44d3-196">引數</span><span class="sxs-lookup"><span data-stu-id="c44d3-196">Arguments</span></span>

| <span data-ttu-id="c44d3-197">引數</span><span class="sxs-lookup"><span data-stu-id="c44d3-197">Argument</span></span> | <span data-ttu-id="c44d3-198">描述</span><span class="sxs-lookup"><span data-stu-id="c44d3-198">Description</span></span> |
|-|-|
| <span data-ttu-id="c44d3-199">參考</span><span class="sxs-lookup"><span data-stu-id="c44d3-199">references</span></span> | <span data-ttu-id="c44d3-200">要刪除的原語引用的 URL 或檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="c44d3-200">The URLs or file paths of the protobuf references to remove.</span></span> |

### <a name="options"></a><span data-ttu-id="c44d3-201">選項。</span><span class="sxs-lookup"><span data-stu-id="c44d3-201">Options</span></span>

| <span data-ttu-id="c44d3-202">短選項</span><span class="sxs-lookup"><span data-stu-id="c44d3-202">Short option</span></span> | <span data-ttu-id="c44d3-203">長選項</span><span class="sxs-lookup"><span data-stu-id="c44d3-203">Long option</span></span> | <span data-ttu-id="c44d3-204">描述</span><span class="sxs-lookup"><span data-stu-id="c44d3-204">Description</span></span> |
|-|-|-|
| <span data-ttu-id="c44d3-205">-p</span><span class="sxs-lookup"><span data-stu-id="c44d3-205">-p</span></span> | <span data-ttu-id="c44d3-206">--專案</span><span class="sxs-lookup"><span data-stu-id="c44d3-206">--project</span></span> | <span data-ttu-id="c44d3-207">要操作的專案檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="c44d3-207">The path to the project file to operate on.</span></span> <span data-ttu-id="c44d3-208">如果未指定檔,該命令將搜索當前目錄。</span><span class="sxs-lookup"><span data-stu-id="c44d3-208">If a file is not specified, the command searches the current directory for one.</span></span>

## <a name="refresh"></a><span data-ttu-id="c44d3-209">Refresh</span><span class="sxs-lookup"><span data-stu-id="c44d3-209">Refresh</span></span>

<span data-ttu-id="c44d3-210">該`refresh`指令用於使用源 URL 中的最新內容更新遠端引用。</span><span class="sxs-lookup"><span data-stu-id="c44d3-210">The `refresh` command is used to update a remote reference with the latest content from the source URL.</span></span> <span data-ttu-id="c44d3-211">下載檔案路徑和源 URL 都可用於指定要更新的引用。</span><span class="sxs-lookup"><span data-stu-id="c44d3-211">Both the download file path and the source URL can be used to specify the reference to be updated.</span></span> <span data-ttu-id="c44d3-212">注意:</span><span class="sxs-lookup"><span data-stu-id="c44d3-212">Note:</span></span>

* <span data-ttu-id="c44d3-213">比較文件內容的哈希,以確定是否應更新本地檔案。</span><span class="sxs-lookup"><span data-stu-id="c44d3-213">The hashes of the file contents are compared to determine whether the local file should be updated.</span></span>
* <span data-ttu-id="c44d3-214">不比較時間戳資訊。</span><span class="sxs-lookup"><span data-stu-id="c44d3-214">No timestamp information is compared.</span></span>

<span data-ttu-id="c44d3-215">如果需要更新,該工具始終將本地檔替換為遠端檔。</span><span class="sxs-lookup"><span data-stu-id="c44d3-215">The tool always replaces the local file with the remote file if an update is needed.</span></span>

### <a name="usage"></a><span data-ttu-id="c44d3-216">使用量</span><span class="sxs-lookup"><span data-stu-id="c44d3-216">Usage</span></span>

```dotnetcli
dotnet-grpc refresh [options] [<references>...]
```

### <a name="arguments"></a><span data-ttu-id="c44d3-217">引數</span><span class="sxs-lookup"><span data-stu-id="c44d3-217">Arguments</span></span>

| <span data-ttu-id="c44d3-218">引數</span><span class="sxs-lookup"><span data-stu-id="c44d3-218">Argument</span></span> | <span data-ttu-id="c44d3-219">描述</span><span class="sxs-lookup"><span data-stu-id="c44d3-219">Description</span></span> |
|-|-|
| <span data-ttu-id="c44d3-220">參考</span><span class="sxs-lookup"><span data-stu-id="c44d3-220">references</span></span> | <span data-ttu-id="c44d3-221">應更新的 URL 或檔案路徑到遠端原代引用引用。</span><span class="sxs-lookup"><span data-stu-id="c44d3-221">The URLs or file paths to remote protobuf references that should be updated.</span></span> <span data-ttu-id="c44d3-222">將此參數留空以刷新所有遠端引用。</span><span class="sxs-lookup"><span data-stu-id="c44d3-222">Leave this argument empty to refresh all remote references.</span></span> |

### <a name="options"></a><span data-ttu-id="c44d3-223">選項。</span><span class="sxs-lookup"><span data-stu-id="c44d3-223">Options</span></span>

| <span data-ttu-id="c44d3-224">短選項</span><span class="sxs-lookup"><span data-stu-id="c44d3-224">Short option</span></span> | <span data-ttu-id="c44d3-225">長選項</span><span class="sxs-lookup"><span data-stu-id="c44d3-225">Long option</span></span> | <span data-ttu-id="c44d3-226">描述</span><span class="sxs-lookup"><span data-stu-id="c44d3-226">Description</span></span> |
|-|-|-|
| <span data-ttu-id="c44d3-227">-p</span><span class="sxs-lookup"><span data-stu-id="c44d3-227">-p</span></span> | <span data-ttu-id="c44d3-228">--專案</span><span class="sxs-lookup"><span data-stu-id="c44d3-228">--project</span></span> | <span data-ttu-id="c44d3-229">要操作的專案檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="c44d3-229">The path to the project file to operate on.</span></span> <span data-ttu-id="c44d3-230">如果未指定檔,該命令將搜索當前目錄。</span><span class="sxs-lookup"><span data-stu-id="c44d3-230">If a file is not specified, the command searches the current directory for one.</span></span>
| | <span data-ttu-id="c44d3-231">--幹跑</span><span class="sxs-lookup"><span data-stu-id="c44d3-231">--dry-run</span></span> | <span data-ttu-id="c44d3-232">輸出將在不下載任何新內容的情況下更新的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="c44d3-232">Outputs a list of files that would be updated without downloading any new content.</span></span>

## <a name="list"></a><span data-ttu-id="c44d3-233">清單</span><span class="sxs-lookup"><span data-stu-id="c44d3-233">List</span></span>

<span data-ttu-id="c44d3-234">該`list`命令用於在專案檔中顯示所有 Protobuf 引用。</span><span class="sxs-lookup"><span data-stu-id="c44d3-234">The `list` command is used to display all the Protobuf references in the project file.</span></span> <span data-ttu-id="c44d3-235">如果列的所有值都是預設值,則可以省略該列。</span><span class="sxs-lookup"><span data-stu-id="c44d3-235">If all values of a column are default values, the column may be omitted.</span></span>

### <a name="usage"></a><span data-ttu-id="c44d3-236">使用量</span><span class="sxs-lookup"><span data-stu-id="c44d3-236">Usage</span></span>

```dotnetcli
dotnet-grpc list [options]
```

### <a name="options"></a><span data-ttu-id="c44d3-237">選項。</span><span class="sxs-lookup"><span data-stu-id="c44d3-237">Options</span></span>

| <span data-ttu-id="c44d3-238">短選項</span><span class="sxs-lookup"><span data-stu-id="c44d3-238">Short option</span></span> | <span data-ttu-id="c44d3-239">長選項</span><span class="sxs-lookup"><span data-stu-id="c44d3-239">Long option</span></span> | <span data-ttu-id="c44d3-240">描述</span><span class="sxs-lookup"><span data-stu-id="c44d3-240">Description</span></span> |
|-|-|-|
| <span data-ttu-id="c44d3-241">-p</span><span class="sxs-lookup"><span data-stu-id="c44d3-241">-p</span></span> | <span data-ttu-id="c44d3-242">--專案</span><span class="sxs-lookup"><span data-stu-id="c44d3-242">--project</span></span> | <span data-ttu-id="c44d3-243">要操作的專案檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="c44d3-243">The path to the project file to operate on.</span></span> <span data-ttu-id="c44d3-244">如果未指定檔,該命令將搜索當前目錄。</span><span class="sxs-lookup"><span data-stu-id="c44d3-244">If a file is not specified, the command searches the current directory for one.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c44d3-245">其他資源</span><span class="sxs-lookup"><span data-stu-id="c44d3-245">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
