## <a name="grpc-not-supported-on-azure-app-service"></a>Azure 應用程式服務不支援 gRPC

> [!WARNING]
> ASP.NET Azure 應用服務或 IIS 當前不支援[核心 gRPC。](xref:grpc/index) Http.Sys 的 HTTP/2 實現不支援 gRPC 所依賴的 HTTP 回應尾隨標頭。 如需詳細資訊，請參閱[這個 GitHub 問題](https://github.com/dotnet/AspNetCore/issues/9020) \(英文\)。
