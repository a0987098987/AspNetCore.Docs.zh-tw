## <a name="grpc-not-supported-on-azure-app-service"></a>Azure App Service 不支援 gRPC

> [!WARNING]
> Azure App Service 或 IIS 目前不支援[ASP.NET Core gRPC](xref:grpc/index) 。 Http.sys 的 HTTP/2 執行不支援 gRPC 所依賴的 HTTP 回應尾端標頭。 如需詳細資訊，請參閱 <<c0> [ 此 GitHub 問題](https://github.com/aspnet/AspNetCore/issues/9020)。
