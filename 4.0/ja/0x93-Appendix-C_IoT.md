# Appendix C: IoT の検証要件

この章は元々メインブランチにありましたが、OWASP IoT チームが行った作業によって、このテーマに関する 2 つの異なる基準を維持することは意味がありません。4.0 リリースでは、これを付録に移動し、これを必要とするすべての人に、メインの [OWASP IoT プロジェクト](https://owasp.org/www-project-internet-of-things/) 使用することを推奨します。

## 管理目標

組み込み/IoT 機器は、

* 信頼できる環境でセキュリティ管理を実施することによって、サーバ内と同じレベルのセキュリティ管理をデバイス内でも行う
* デバイスに保存されている機密データは、セキュアエレメントなどのハードウェアに保護されたストレージを使用して、安全な方法で実行する必要があります
* デバイスから送信されるすべての機密データは、TLS を利用する必要があります

## セキュリティ検証要件

| # | 説明 | L1 | L2 | L3 | Since |
| --- | --- | --- | --- | -- | -- |
| **C.1** | USB、UART、そして他のシリアルバリアントのような、アプリケーション層のデバッグインターフェイスが無効になっているか、複雑なパスワードによって保護されている。 | ✓ | ✓ | ✓ | 4.0 |
| **C.2** | 暗号鍵と証明書が各デバイスに固有となっている。 | ✓ | ✓ | ✓ | 4.0 |
| **C.3** | ASLR や DEP などのメモリ保護制御が、組み込み/ IoT オペレーティングシステムによって有効になっている（該当する場合）。 | ✓ | ✓ | ✓ | 4.0 |
| **C.4** | JTAG や SWD などのオンチップデバッグインターフェイスが無効になっている、または使用可能な保護メカニズムが有効になっていて適切に構成されている。 | ✓ | ✓ | ✓ | 4.0 |
| **C.5** | SoC デバイスまたは CPU で利用可能な場合、Trusted Execution が実装および有効になっている。 | ✓ | ✓ | ✓ | 4.0 |
| **C.6** | 機密データ、秘密鍵、そして証明書が、セキュアエレメント、TPM、TEE（Trusted Execution Environment）に安全に保存されていること、または強力な暗号化を使用して保護されている。 | ✓ | ✓ | ✓ | 4.0 |
| **C.7** | ファームウェアアプリが Transport Layer Security（トランスポートレイヤセキュリティ）を使用して伝送中のデータを保護している。 | ✓ | ✓ | ✓ | 4.0 |
| **C.8** | ファームウェアアプリがサーバ接続のデジタル署名を検証する。 | ✓ | ✓ | ✓ | 4.0 |
| **C.9** | ワイヤレス通信が相互に認証されている。 | ✓ | ✓ | ✓ | 4.0 |
| **C.10** | ワイヤレス通信が暗号化されたチャネルを介して送信される。 | ✓ | ✓ | ✓ | 4.0 |
| **C.11** | 禁止された C 関数の使用が適切な安全な同等の関数に置き換えられている。 | ✓ | ✓ | ✓ | 4.0 |
| **C.12** | 各ファームウェアがサードパーティのコンポーネント、バージョン、および公開された脆弱性をカタログ化するソフトウェア部品表を維持している。 | ✓ | ✓ | ✓ | 4.0 |
| **C.13** | サードパーティのバイナリ、ライブラリ、フレームワークを含むすべてのコードがハードコードされたクレデンシャル（バックドア）についてレビューされている。 | ✓ | ✓ | ✓ | 4.0 |
| **C.14** | シェルコマンドラッパー、スクリプトを呼び出して、アプリケーションおよびファームウェアコンポーネントが OS コマンドインジェクションの影響を受けないこと、またはセキュリティ制御により OS コマンドインジェクションが阻止されている。 | ✓ | ✓ | ✓ | 4.0 |
| **C.15** | ファームウェアのアプリが信頼できるサーバを固定化している。 |  | ✓ | ✓ | 4.0 |
| **C.16** | 耐タンパ性または、改ざん検知機能が存在する。 |  | ✓ | ✓ | 4.0 |
| **C.17** | チップ製造者による、有用な知的財産権保護技術が有効化されている。 |  | ✓ | ✓ | 4.0 |
| **C.18** | ファームウェアのリバースエンジニアリングを防止するためのセキュリティ管理策（デバッグ情報の除去など）が実施されている。 |  | ✓ | ✓ | 4.0 |
| **C.19** | デバイスが、ブートイメージのロード前に署名の検証を行う。 |  | ✓ | ✓ | 4.0 |
| **C.20** | ファームウェアの更新処理が TOCTOU 攻撃に対して脆弱でない。 |  | ✓ | ✓ | 4.0 |
| **C.21** | デバイスがコード署名を利用し、インストール前にファームウェアのアップグレードファイルの検証を行う。 |  | ✓ | ✓ | 4.0 |
| **C.22** | デバイスが、古いバージョンのファームウェアにダウングレードしない。 |  | ✓ | ✓ | 4.0 |
| **C.23** | 暗号論的に安全な擬似乱数生成器が、組込み機器上で使用されていること（例えば、チップが提供する乱数生成器を使用している等）。 |  | ✓ | ✓ | 4.0 |
| **C.24** | ファームウェアが事前設定のスケジュールに従って、ファームウェアの自動更新を実行する。 |  | ✓ | ✓ | 4.0 |
| **C.25** | 改ざんを検知、不正なメッセージを受信した際に、デバイスがファームウェアおよび機密データをワイプする。 |  |  | ✓ | 4.0 |
| **C.26** | デバッグ用インターフェース（JTAG/SWD など）を無効化できるマイクロコントローラだけが使用されている。 |  |  | ✓ | 4.0 |
| **C.27** | デキャップやサイドチャネル攻撃から防御できるマイクロコントローラを使用する。 |  |  | ✓ | 4.0 |
| **C.28** | 機微な痕跡がプリント基板の外部レイヤに漏えいしない。 |  |  | ✓ | 4.0 |
| **C.29** | チップ間の通信（メインボードからドーターボードへの通信など）を暗号化している。 |  |  | ✓ | 4.0 |
| **C.30** | デバイスがコード署名を使用し、実行前にコードの妥当性の検証を行う。 |  |  | ✓ | 4.0 |
| **C.31** | メモリ内に保持される機密な情報が、不要になったら直ちにゼロで上書きされる。 |  |  | ✓ | 4.0 |
| **C.32** | ファームウェアアプリがアプリ分離のため、カーネルコンテナを利用している。 |  |  | ✓ | 4.0 |
| **C.33** | ファームウェアが -fPIE、-fstack-protector-all、-Wl、-z、noexecstack、-Wl、-z、noexecheap などの安全なコンパイルフラグでビルドされている。 |  |  | ✓ | 4.0 |
| **C.34** | （該当する場合）マイクロコントローラが、コードプロテクションを利用している。 |  |  | ✓ | 4.0 |

## 参考情報

詳しくは以下の情報を参照してください。

* [OWASP Internet of Things Top 10](https://owasp.org/www-pdf-archive/OWASP-IoT-Top-10-2018-final.pdf)
* [OWASP Embedded Application Security Project](https://owasp.org/www-project-embedded-application-security/)
* [OWASP Internet of Things Project](https://owasp.org/www-project-internet-of-things/)
* [Trudy TCP Proxy Tool](https://github.com/praetorian-inc/trudy)
