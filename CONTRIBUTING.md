# <a name="contribute-to-the-aspnet-documentation"></a>參與 ASP.NET 文件

本文件涵蓋參與 [ASP.NET 文件網站](https://docs.microsoft.com/aspnet/)所裝載文章和程式碼範例的程序。 歡迎您加入修正錯字和新增文章的行列。

## <a name="how-to-make-a-simple-correction-or-suggestion"></a>如何提出簡單的修正或建議

文章會以 Markdown 檔案形式儲存在存放庫中。 您可以在瀏覽器中，選取瀏覽器視窗右上角的 [編輯] 連結，對 Markdown 檔案內容進行簡易變更。 (如果瀏覽器視窗很窄，展開 [選項] 列即可看到 [編輯] 連結)。請依照指示來建立提取要求 (PR)。 我們將檢閱 PR 並接受它，或建議變更。

## <a name="how-to-make-a-more-complex-submission"></a>如何進行更複雜的提交

您需要對 [Git 和 GitHub.com](https://guides.github.com/activities/hello-world/) 有基本了解。

* 建立一則[議題](https://github.com/aspnet/Docs/issues/new)描述您欲執行的動作，例如變更現有的文章或建立新的文章。 我們通常會要求新建議主題的大綱。 在您投入更多時間之前，請等候小組的核准。
* 派生 [aspnet/Docs](https://github.com/aspnet/Docs/) 存放庫，並針對您的變更建立分支。
* 將內含變更的 PR 提交給管理員。
* 如果您的 PR 被分派到 'cla-required' 標籤，請[完成貢獻授權合約 (CLA)](https://cla.dotnetfoundation.org/)。
* 回應 PR 意見。

如需此程序進展至發行新文章的範例，請參閱 .NET Docs 存放庫中的[議題&num;67](https://github.com/dotnet/docs/issues/67) 和[提取要求&num;798](https://github.com/dotnet/docs/pull/798)。 新文章為[記錄您的程式碼](https://docs.microsoft.com/dotnet/articles/csharp/codedoc)。

## <a name="docs-authoring-pack-extension-in-visual-studio-code"></a>Visual Studio Code 中的 Docs Authoring Pack 延伸模組 

如果您使用 Visual Studio Code 來參與 ASP.NET 文件，您即可藉由安裝 [Docs Authoring Pack](https://marketplace.visualstudio.com/items?itemName=docsmsft.docs-authoring-pack) 延伸模組來提高生產力。 該延伸模組會提供各種不同的工具，協助 Markdown linting、程式碼拼字檢查和文章範本。

## <a name="markdown-syntax"></a>Markdown 語法

文章是以 [DocFX Flavored Markdown](https://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html) 撰寫，也就是 [GitHub Flavored Markdown (GFM)](https://guides.github.com/features/mastering-markdown/) 的超集。 如需 ASP.NET 文件中常用 UI 功能的 DFM 語法範例，請參閱 .NET Docs 存放庫樣式指南中的 [Metadata and Markdown Template](https://github.com/dotnet/docs/blob/master/styleguide/template.md) (中繼資料和 Markdown 範本)。 

## <a name="folder-structure-conventions"></a>資料夾結構慣例

在每個 Markdown 檔案中，可能存在一個影像資料夾和一個範例程式碼資料夾。 如果文章是 [fundamentals/configuration/index.md](https://github.com/aspnet/Docs/blob/master/aspnetcore/fundamentals/configuration/index.md)，影像會位於 [fundamentals/configuration/index/\_static](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/_static) 中，而範例應用程式專案檔會位於 [fundamentals/configuration/index/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) 中。 *fundamentals/configuration/index.md* 檔案中的影像會由下列 Markdown 轉譯：

```
![description of image for alt attribute](configuration/index/_static/imagename.png)
```

所有影像都應有[替代 (alt) 文字](https://wikipedia.org/wiki/Alt_attribute)。 如需指定 alt 文字的建議，請參閱線上資源，例如 [WebAIM: Alternative Text](https://webaim.org/techniques/alttext/) (WebAIM：替代文字)。

針對 Markdown 檔案名稱和影像檔案名稱，請使用小寫。

## <a name="internal-links"></a>內部連結

內部連結應使用目標文章的 `uid` 並搭配 xref 連結 (設為連結內容標題的連結文字)：

```
<xref:uid_of_the_topic>
```

如果文章的標題不適用於連結文字 (例如連結文字是句子中的單字或片語)，請使用下列命令來指定 xref 連結和連結文字：

```
[link text](xref:uid_of_the_topic)
```

如需詳細資訊，請參閱 [DocFX Cross Reference](https://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#cross-reference) (DocFX 交互參照)。

## <a name="images-and-screenshots"></a>影像和螢幕擷取畫面

不包含影像與發行項，但下列項目除外：

* 在基本上線 (初學者) 教學課程中。
* 需要影像予以釐清時。

這些限制會減少存放庫的大小。

還有一個選擇性步驟，那就是確認文件中使用的所有影像和螢幕擷取畫面均已壓縮，這有助於檔案大小和頁面載入效能。 一些熱門的工具包括 TinyPNG (使用 [TinyPNG 網站](https://tinypng.com/)或 [TinyPNG API](https://tinypng.com/developers)) 或 [Image Optimizer](https://marketplace.visualstudio.com/items?itemName=MadsKristensen.ImageOptimizer) Visual Studio 延伸模組。 

## <a name="code-snippets"></a>程式碼片段

文章通常會包含程式碼片段，作重點說明之用。 DFM 可讓您將程式碼複製到 Markdown 檔案中，或參考不同的程式碼檔案。 建議盡可能使用不同的程式碼檔案，將程式碼中的錯誤機率降到最低。 程式碼檔案會儲存在存放庫 (使用於先前範例專案所述的資料夾結構) 中。 

下列範例說明 *configuration/index.md* 檔案中所使用的 [DFM 程式碼片段語法](https://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#code-snippet)。

若要將完整程式碼檔案轉譯為程式碼片段：

```
[!code-csharp[](configuration/index/sample/Program.cs)]
```

若要使用行號，將一部分檔案轉譯為程式碼片段：

```
[!code-csharp[](configuration/index/sample/Program.cs?range=1-10,20,30,40-50]
[!code-html[](configuration/index/sample/Views/Home/Index.cshtml?range=1-10,20,30,40-50]
```

針對 C# 程式碼片段，請參考 [C# 區域](https://docs.microsoft.com/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-region)。 請盡可能使用區域而不是行號，因為程式碼檔案中的行號通常會變更，而變得與 Markdown 中的行號參考不同步。 C# 區域可以是巢狀的。 如果參考外部區域，則不會轉譯程式碼片段中的內部 `#region` 和 `#endregion` 指示詞。 

若要轉譯名為 "snippet_Example" 的 C# 區域：

```
[!code-csharp[](configuration/index/sample/Program.cs?name=snippet_Example)]
```

若要醒目提示轉譯程式碼片段中所選取的行 (通常會以黃色背景色彩呈現)：

```
[!code-csharp[](configuration/index/sample/Program.cs?name=snippet_Example&highlight=1-3,10,20-25)]
[!code-csharp[](configuration/index/sample/Program.cs?range=10-20&highlight=1-3]
[!code-html[](configuration/index/sample/Views/Home/Index.cshtml?range=10-20&highlight=1-3]
[!code-javascript[](configuration/index/sample/UsingOptionsSample.csproj?range=10-20&highlight=1-3]
```

## <a name="test-changes-with-docfx"></a>使用 DocFX 測試變更

使用 [DocFX 命令列工具](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool)測試您的變更，這會建立本機裝載的網站版本。 DocFX 不會轉譯 docs.microsoft.com 建立的樣式和網站延伸模組。

DocFX 需要：

* Windows 上的 .NET Framework。
* 適用於 Linux 或 macOS 的 Mono。 

### <a name="windows-instructions"></a>Windows 指示

* 從 [DocFX 版本](https://github.com/dotnet/docfx/releases)下載並解壓縮 *docfx.zip*。
* 將 DocFX 新增至您的 PATH。
* 在命令列視窗中，巡覽至包含 *docfx.json* 檔案的適當資料夾 (若是 ASP.NET 內容則為 *aspnet*；若是 ASP.NET Core 內容則為 *aspnetcore*)，然後執行下列命令：

  ```
  docfx --serve
  ```
    
* 在瀏覽器中，巡覽至 `http://localhost:8080`。

### <a name="mono-instructions"></a>Mono 指示

* 透過 Homebrew 安裝 Mono：`brew install mono`。
* 下載[最新版本的 DocFX](https://github.com/dotnet/docfx/releases)。
* 解壓縮至 `\bin\docfx`。
* 建立 **docfx** 的別名：

  ```
  function docfx {
    mono $HOME/bin/docfx/docfx.exe
  }
    
  function docfx-serve {
    mono $HOME/bin/docfx/docfx.exe serve _site
  }
  ```

* 執行 *Docs\aspnet* 或 *Docs\aspnetcore* 目錄中的 `docfx` 來建置網站。 執行 `docfx-serve` 以檢視位於 `http://localhost:8080` 的網站。

## <a name="voice-and-tone"></a>語態和語氣

我們撰寫文件的目標是盡可能讓越多使用者輕鬆了解越好。 為此，我們制定了書寫樣式方針，並要求參與者務必遵循。 如需詳細資訊，請參閱 .NET 存放庫中的 [Voice and tone guidelines](https://github.com/dotnet/docs/blob/master/styleguide/voice-tone.md) (語態和語氣方針)。

## <a name="microsoft-writing-style-guide"></a>Microsoft 書寫樣式指南

[Microsoft Writing Style Guide](https://docs.microsoft.com/style-guide/welcome/) (Microsoft 書寫樣式指南) 針對所有形式的技術通訊提供書寫樣式和術語指引，包括 ASP.NET Core 文件。

## <a name="redirects"></a>重新導向

如果您刪除文章、變更其檔案名稱，或把文章移至其他資料夾，請建立重新導向，確保將文章加入書籤的人員不會收到 *404 找不到*錯誤。 將重新導向新增至[主要重新導向檔案](https://github.com/aspnet/Docs/blob/master/.openpublishing.redirection.json)。
