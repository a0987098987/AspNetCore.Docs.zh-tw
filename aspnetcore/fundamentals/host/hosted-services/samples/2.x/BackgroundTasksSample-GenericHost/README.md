# <a name="aspnet-background-tasks-sample-generic-host"></a>ASP.NET 背景工作範例 (泛型主機)

此範例說明如何使用 [IHostedService](https://docs.microsoft.com/dotnet/api/microsoft.extensions.hosting.ihostedservice)。 這個範例會示範[在 ASP.NET Core 中使用託管服務的背景工作](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services)主題中所述的功能。

在 [Visual Studio Code](https://code.visualstudio.com/) 中執行範例時，請將 *.vscode/launch.json* 中主控台設定的 **console** 值設定成 `externalTerminal` 或 `integratedTerminal`。 使用 `internalConsole` 會不相容於應用程式用於將背景工作項目加入佇列的按鍵輸入。
