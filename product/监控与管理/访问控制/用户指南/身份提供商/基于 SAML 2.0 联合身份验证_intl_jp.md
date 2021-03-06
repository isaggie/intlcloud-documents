Tencent Cloudは、SAML 2.0（Security Assertion Markup Language 2.0）に基づく統合身元検証をサポートしています。SAML 2.0は、多くのIDプロバイダー（Identity Provider、IdP）によって使用されているオープン標準です。IDプロバイダーを使用して、統合シングルサインオン（Federated Single Sign-on、SSO）が実現できます。ユーザーは、企業または組織の各メンバーに対してCAMサブユーザーを作成することなく、統合身元検証に合格したユーザーにTencent Cloud管理コンソールへのログインまたはTencent Cloud API操作の呼び出し権限を付与することができます。同時に、SAML 2.0は一般的なオープンプロトコルなので、カスタム身元プロキシコードを書く必要はありません。SAMLを使用して、Tencent Cloudの統合身元検証のプロセスを直接簡素化できます。

## SAML IDプロバイダー

IDプロバイダーは、アクセス管理（CAM）の1つのエンティティです。外部のクレジットアカウントの集まりと見なすことができます。SAML 2.0統合身元検証に基づくIDプロバイダー（IdP）は、SAML 2.0（Security Assertion Markup Language 2.0）標準をサポートするIDプロバイダー（IdP）サービスについて説明しています。企業または組織のメンバーがTencent Cloudリソースにアクセスできるように、SAML 2.0プロトコル互換のIDプロバイダー（Microsoft Active Directory統合身元検証サービスなど）とTencent Cloudの間で信頼関係を確立する場合は、SAML IDプロバイダーを作成する必要があります。SAML IDプロバイダーの作成については、[IDプロバイダーの作成](https://cloud.tencent.com/document/product/598/30290)を参照してください。

## IDプロバイダーロール

SAMLプロバイダーを作成した後、SAML IDプロバイダーをロールキャリアとする1つ以上のIDプロバイダーロールを作成する必要があります。ロールは一連の権限を持つ仮想身元であり、リソースにアクセスするときは一時的セキュリティ証明書が使用されます。SAML 2.0アサーションのコンテキストでは、IDプロバイダー（IdP）によって検証された統合身元ユーザーにロールを割り当てることができます。このロールにより、IDプロバイダーはTencent Cloudリソースアクセス用の一時的セキュリティ証明書を申請できます。このロールに関連付けられているポリシーによって、統合身元ユーザーのTencent Cloudリソースへのアクセス範囲が決まります。SAML 2.0統合身元検証に基づいてIDプロバイダーロールを作成することについては、[ロールの作成](https://cloud.tencent.com/document/product/598/19381)を参照してください。
![](https://main.qcloudimg.com/raw/f85ada7eb563f1132aabfd6d398cca21.png)

## SAML 2.0統合身元検証を使用したTencent Cloud APIへのアクセス

![1](https://main.qcloudimg.com/raw/65eb02712b75d7bfcbba509b8f10be7c.png)
1.	企業または組織のユーザーがクライアントを使用して、企業のIDプロバイダーに身元検証を申請します。
2.	IDプロバイダーは、企業の身元検証システムによって検証されています。
3.	ユーザー検証の結果を返します。
4.	IDプロバイダーは検証結果に基づいて、標準のSAML 2.0アサーションドキュメントを生成してクライアントに返します。
5.	クライアントは、SAML 2.0アサーション、IDプロバイダーのリソース説明、および使用されているIDプロバイダーロールのリソース説明に基づいて、sts:AssumeRoleWithSAMLに一時的セキュリティ暗号鍵を申請します。
6.	STSはSAML 2.0アサーション情報を検証します。
7.	検証結果を返します。
8.	返された結果に基づいて一時的証明書を申請し、クライアントに返します。

## SAML 2.0統合身元検証に基づいた統合シングルサインオン（SSO）の使用
![](https://main.qcloudimg.com/raw/10d90eb5e5f91927e873cec8dc0e5823.png)
1.	企業または組織ユーザーは、ブラウザを使用してTencent Cloudサービスにアクセスします。
2.	Tencent Cloudサービスは認証リクエストをブラウザに返します。
3.	ブラウザは、企業組織のIDプロバイダー（IdP）にリダイレクトされます。
4.	企業はユーザーの身元を検証します。
5.	企業ユーザーの身元検証が成功して、ユーザー情報がIDプロバイダー（IdP）に返されます。
6.	IDプロバイダー（IdP）は、標準のSAML2.0アサーションを生成してブラウザに返します。
7.	ブラウザはSAML 2.0アサーションをTencent Cloudにリダイレクトします。
8.	Tencent Cloud SSOログインサービスを開始し、cAuthを申請してユーザーの身元を検証します。
9.	Tencent Cloudの検証結果を返します。
10.	検証は成功して、ログインセッションが返されます。
11.	Tencent Cloudコンソールサービスにリダイレクトします。
