# V17 WebRTC

## 管理目標

Web リアルタイム通信 (Web Real-Time Communication, WebRTC) は現代のアプリケーションにおいてリアルタイムの音声、動画、データ交換を可能にします。普及が進むにつれて、WebRTC インフラストラクチャのセキュリティ確保が重要になります。このセクションでは、WebRTC システムを開発、ホスト、統合する関係者向けのセキュリティ要件を提供します。

WebRTC 市場は三つのセグメントに大別できます。

1. 製品開発者: WebRTC 製品やソリューションを作成および提供するプロプライエタリおよびオープンソースのベンダーです。他者が使用できる堅牢で安全な WebRTC テクノロジの開発に重点を置いています。

2. サービスとしての通信プラットフォーム (Communication Platforms as a Service, CPaaS): WebRTC の機能を実現するために必要な API、SDK、インフラストラクチャやプラットフォームを提供します。CPaaS プロバイダは第一カテゴリの製品を使用することも、独自の WebRTC ソフトウェアを開発して、これらのサービスを提供することもできます。

3. サービスプロバイダ: 製品開発者や CPaaS プロバイダの製品を活用したり、独自の WebRTC ソリューションを開発する組織です。オンライン会議、ヘルスケア、e ラーニングなど、リアルタイム通信が重要な領域に対してアプリケーションを作成および実装します。

ここで説明するセキュリティ要件は主に以下のような製品開発者、CPaaS、サービスプロバイダを対象としています。

* オープンソースソリューションを利用して WebRTC アプリケーションを構築する。
* インフラストラクチャの一部として商用 WebRTC 製品を使用する。
* 内部で開発した WebRTC ソリューションを使用するか、さまざまなコンポーネントを統合して、まとまりのあるサービスを提供する。

これらのセキュリティ要件は CPaaS ベンダーが提供する SDK や API だけを使用する開発者には適用しないことに注意することが重要です。そのような開発者としては、一般的に CPaaS プロバイダがプラットフォーム内の基本的なセキュリティの懸念のほとんどに責任があり、ASVS のような汎用的なセキュリティ標準では開発者のニーズに完全に対応できないかもしれません。

## V17.1 TURN サーバ

このセクションでは、独自の TURN (Traversal Using Relays around NAT) サーバを運用するシステムのセキュリティ要件を定義します。TURN サーバは制限の厳しいネットワーク環境のメディア中継を支援しますが、設定を誤るとリスクをもたらす可能性があります。これらのコントロールは安全なアドレスフィルタリングと、リソース枯渇に対する保護に重点を置いています。

| # | 説明 | レベル | #v5.0.be |
| :---: | :--- | :---: | :---: |
| **17.1.1** | Traversal Using Relays around NAT (TURN) サービスは特別な目的 (内部ネットワーク、ブロードキャスト、ループバックなど) のために予約されていない IP アドレスへのアクセスのみを許可している。これは IPv4 と IPv6 の両方のアドレスに適用することに注意する。 | 2 | v5.0.be-53.1.1 |
| **17.1.2** | 正規のユーザが TURN サーバ上で多数のポートを開こうとした際に、Traversal Using Relays around NAT (TURN) サービスはリソース枯渇の影響を受けない。 | 3 | v5.0.be-53.1.2 |

## V17.2 メディア

これらの要件は、選択転送ユニット (Selective Forwarding Unit, SFU)、多地点制御ユニット (Multipoint Control Unit, MCU)、レコーディングサーバ、ゲートウェイサーバなど、独自の WebRTC メディアサーバをホストするシステムにのみ適用します。メディアサーバはメディアストリームを処理して配信するため、ピア間の通信を保護するためにセキュリティが重要になります。WebRTC アプリケーションでは、ユーザのプライバシーと通信品質を損なう可能性のある、盗聴、改竄、サービス拒否攻撃を防ぐために、メディアストリームの保護が非常に重要です。

特に、レート制限、タイムスタンプの検証、同期クロックを使用したリアルタイムインターバルの一致、オーバーフローを防いで適切なタイミングを維持するためのバッファ管理など、フラッド攻撃に対する保護を実装する必要があります。特定のメディアセッションのパケットがあまりにも早く到着する場合、過剰なパケットをドロップすべきです。入力バリデーションを実装し、整数オーバーフローを安全に処理し、バッファオーバーフローを防止し、その他の堅牢なエラー処理技法を採用することで、不正なパケットからシステムを保護することも重要です。

中間メディアサーバを介さずに、Web ブラウザ間のピアツーピアメディア通信のみに依存するシステムは、これらの特定のメディア関連セキュリティ要件から除外されます。

このセクションでは、WebRTC のコンテキストにおけるデータグラムトランスポート層セキュリティ (Datagram Transport Layer Security, DTLS) の使用について説明します。暗号鍵の管理に対する文書化されたポリシーを持つことに関する要件は「暗号化」の章に記載されています。承認済み暗号方式に関する情報は、ASVS の暗号化の付録や、NIST SP 800‑52 Rev. 2 や BSI TR‑02102‑2 (Version 2025‑01) などのドキュメントに記載されています。

| # | 説明 | レベル | #v5.0.be |
| :---: | :--- | :---: | :---: |
| **17.2.1** | データグラムトランスポート層セキュリティ (Datagram Transport Layer Security, DTLS) 証明書の鍵は、暗号鍵の管理に関する文書化されたポリシーに基づいて管理および保護されている。 | 2 | v5.0.be-53.2.1 |
| **17.2.2** | メディアサーバは、承認されたデータグラムトランスポート層セキュリティ (Datagram Transport Layer Security, DTLS) 暗号スイートと、セキュアリアルタイムトランスポートプロトコル (Secure Real-time Transport Protocol) の鍵を確立するための DTLS 拡張 (DTLS-SRTP) 用の安全な保護プロファイルを使用およびサポートするように構成されている。 | 2 | v5.0.be-53.2.2 |
| **17.2.3** | セキュアリアルタイムトランスポートプロトコル (Secure Real-time Transport Protocol, SRTP) 認証はメディアサーバで確認されており、リアルタイムトランスポートプロトコル (Real-time Transport Protocol, RTP) インジェクション攻撃によるサービス拒否状態や、メディアストリームへの音声または映像メディアの挿入を防止している。 | 2 | v5.0.be-53.2.4 |
| **17.2.4** | 不正なセキュアリアルタイムトランスポートプロトコル (Secure Real-time Transport Protocol, SRTP) パケットに遭遇した場合でも、メディアサーバは受信メディアトラフィックの処理を継続できる。 | 2 | v5.0.be-53.2.7 |
| **17.2.5** | 正規のユーザからセキュアリアルタイムトランスポートプロトコル (Secure Real-time Transport Protocol, SRTP) パケットが大量送信されている間も、メディアサーバは受信メディアトラフィックの処理を継続できる。 | 3 | v5.0.be-53.2.5 |
| **17.2.6** | メディアサーバはデータグラムトランスポート層セキュリティ (Datagram Transport Layer Security, DTLS) での "ClientHello" Race Condition 脆弱性の影響を受けておらず、メディアサーバが脆弱性を持つことを公表しているかどうかを確認し、競合状態テストを実行している。 | 3 | v5.0.be-53.2.3 |
| **17.2.7** | 正規のユーザからセキュアリアルタイムトランスポートプロトコル (Secure Real-time Transport Protocol, SRTP) パケットが大量送信されている間も、メディアサーバに関連付けられた音声や映像のレコーディングメカニズムは受信メディアトラフィックの処理を継続できる。 | 3 | v5.0.be-53.2.6 |
| **17.2.8** | データグラムトランスポート層セキュリティ (Datagram Transport Layer Security, DTLS) 証明書はセキュアリアルタイムトランスポートプロトコル (Secure Real-time Transport Protocol, SRTP) フィンガープリント属性と照合され、チェックに失敗した場合はメディアストリームを終了して、メディアストリームの真正性を確保している。 | 3 | v5.0.be-53.2.8 |

## V17.3 シグナリング

このセクションでは、独自の WebRTC シグナリングサーバを運用するシステムの要件を定義します。シグナリングはピアツーピア通信を調整するものであり、セッションの確立や制御を妨害する可能性のある攻撃に対して耐性を持つ必要があります。

安全なシグナリングを確保するには、システムは不正な入力を適切に処理して、負荷がかかっても利用可能であり続ける必要があります。

| # | 説明 | レベル | #v5.0.be |
| :---: | :--- | :---: | :---: |
| **17.3.1** | フラッド攻撃中でも、シグナリングサーバは正当な受信シグナリングメッセージの処理を継続できる。これはシグナリングレベルでレート制限を実装することで実現できる。 | 2 | v5.0.be-53.3.1 |
| **17.3.2** | サービス拒否状態を引き起こす可能性のある不正なシグナリングメッセージに遭遇した場合でも、シグナリングサーバは正当なシグナリングメッセージの処理を継続できる。これには、入力バリデーションの実装、整数オーバーフローの安全な処理、バッファオーバーフローの防止、その他の堅牢なエラー処理技法の採用などを含む。 | 2 | v5.0.be-53.3.2 |

## 参考情報

詳しくは以下の情報を参照してください。

* WebRTC DTLS ClientHello DoS については [セキュリティ専門家を対象とした Enable Security のブログ投稿](https://www.enablesecurity.com/blog/novel-dos-vulnerability-affecting-webrtc-media-servers/) と、関連する [WebRTC 開発者を対象としたホワイトペーパー](https://www.enablesecurity.com/blog/webrtc-hello-race-conditions-paper/) に詳しく記載されています。
* [RFC 3550 - RTP: A Transport Protocol for Real-Time Applications](https://www.rfc-editor.org/rfc/rfc3550)
* [RFC 3711 - The Secure Real-time Transport Protocol (SRTP)](https://datatracker.ietf.org/doc/html/rfc3711)
* [RFC 5764 - Datagram Transport Layer Security (DTLS) Extension to Establish Keys for the Secure Real-time Transport Protocol (SRTP))](https://datatracker.ietf.org/doc/html/rfc5764)
* [RFC 8825 - Overview: Real-Time Protocols for Browser-Based Applications](https://www.rfc-editor.org/info/rfc8825)
* [RFC 8826 - Security Considerations for WebRTC](https://www.rfc-editor.org/info/rfc8826)
* [RFC 8827 - WebRTC Security Architecture](https://www.rfc-editor.org/info/rfc8827)
* [DTLS-SRTP Protection Profiles](https://www.iana.org/assignments/srtp-protection/srtp-protection.xhtml)
