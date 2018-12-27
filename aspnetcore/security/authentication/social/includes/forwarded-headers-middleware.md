## <a name="forward-request-information-with-a-proxy-or-load-balancer"></a>轉寄要求使用 proxy 的資訊或負載平衡器

如果應用程式部署 proxy 伺服器或負載平衡器後方時，原始的要求資訊的一些可能會轉送給要求標頭中的應用程式。 這項資訊通常包括安全的要求配置 (`https`)，主機和用戶端 IP 位址。 應用程式不會自動讀取這些探索及使用原始的要求資訊的要求標頭。

配置用於連結產生會影響使用外部提供者的驗證流程。 遺失的安全的配置 (`https`) 會導致產生不正確不安全的重新導向 Url 的應用程式。

將原始的要求資訊提供給要求處理的應用程式中使用轉送標頭中介軟體。

如需詳細資訊，請參閱<xref:host-and-deploy/proxy-load-balancer>。
