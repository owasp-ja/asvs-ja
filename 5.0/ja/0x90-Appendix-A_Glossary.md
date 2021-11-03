# Appendix A: 用語集

- **アドレス空間配置のランダム化 (Address Space Layout Randomization)** (ASLR) – メモリ破損のバグ悪用をより困難にする手法
- **許可リスト (Allow list)** – アプリケーションにおいて許可されているデータまたは操作のリスト(例：入力バリデーションで許可されている文字のリストなど)
- **アプリケーションセキュリティ (Application Security)** – OS やネットワークではなく，OSI 参照モデルのアプリケーション層を構成するコンポーネントの分析に重点をおく、アプリケーションレベルのセキュリティ
- **アプリケーションセキュリティ検証 (Application Security Verification)** – OWASP ASVS に沿って実施するアプリケーションの技術的評価
- **アプリケーションセキュリティ検証報告書 (Application Security Verification Report)** – 対象となるアプリケーションの分析と検証結果をまとめた報告書
- **認証 (Authentication)** – アプリケーションのユーザが正当であることの検証
- **自動検証 (Automated Verification)** – シグネチャを使って脆弱性を見つける自動ツール(動的解析ツール、静的解析ツール、その両方)を用いた検査
- **ブラックボックステスト (Black box testing)** – アプリケーションの内部構造や仕組みを調べずにアプリケーションの機能を調べるソフトウェアテストの方法
- **コンポーネント (Component)** – 独立したコード単位。ディスクや他のコンポーネントと通信するネットワークインターフェイスとの関連をもちます
- **クロスサイトスクリプティング (Cross-Site Scripting (XSS))** – クライアントからコンテンツへのスクリプトの注入を可能にする、Web アプリケーションに典型的なセキュリティ上の脆弱性
- **暗号モジュール (Cryptographic module)** – 暗号アルゴリズムを実装し、暗号鍵を生成する、ハードウェアやソフトウェア、ファームウェア
- **Common Weakness Enumeration** (CWE) - コミュニティが開発した一般的なソフトウェアセキュリティの弱点一覧。共通言語、ソフトウェアセキュリティツールの測定基準、および脆弱性の特定・軽減・防止の取り組みのベースラインとして機能します
- **設計検証 (Design Verification)** – アプリケーションのセキュリティアーキテクチャに関する技術的評価
- **動的アプリケーションセキュリティテスト (Dynamic Application Security Testing)** (DAST) - 技術は、実行状態にあるアプリケーションのセキュリティの脆弱性を示す状況を検出するように設計されています
- **動的検証 (Dynamic Verification)** – アプリケーションの実行中に脆弱性のシグネチャを用いて問題点を検出する自動化ツールを使用すること
- **ファイド (Fast IDentity Online)** (FIDO) - バイオメトリクス、トラステッドプラットフォームモジュール (Trusted Platform Module, TPM)、USB セキュリティトークンなど、さまざまな認証方式を使用できるようにする一連の認証標準
- **グローバル一意識別子（GUID）** – ソフトウェアで識別子として使用される一意の照会番号
- **HTTP** – 分散、コラボレーション、ハイパーメディア情報システム用のアプリケーションプロトコル．World Wide Web におけるデータ通信の基盤
- **ハードコードされた鍵 (Hardcoded keys)** – コード、コメント、ファイルなど、ファイルシステムに格納されている暗号化鍵
- **ハードウェアセキュリティモジュール (Hardware Security Module)** (HSM) - 暗号化鍵とその他のシークレットを保護された方法で保存できるハードウェアコンポーネント
- **Hibernate クエリ言語 (Hibernate Query Language)** (HQL) - Hibernate ORM ライブラリで使用される SQL と似た外観のクエリ言語 
- **入力バリデーション (Input Validation)** – 信頼できないユーザ入力を正規化およびバリデーションします
- **悪性コード (Malicious Code)** – アプリケーションの開発時にアプリケーションのオーナに気付かれることなく導入されるコードであり，アプリケーションのセキュリティポリシーを回避します。ウイルスやワームなどのマルウェアとは異なります！
- **マルウェア (Malware)** – アプリケーションの実行時に、ユーザや管理者に気付かれることなくアプリケーションに侵入する実行コード
- **Open Web Application Security Project (OWASP)** – The Open Web Application Security Project (OWASP) は、アプリケーションソフトウェアのセキュリティ向上に注力する、自由でオープンなコミュニティ。OWASP のミッションは、アプリケーションのセキュリティを"見える化" することで、人や組織が、アプリケーションセキュリティのリスクについて十分な情報に基づいた決断を下すことができることです。http://www.owasp.org/ を参照
- **ワンタイムパスワード (One-time Password)** (OTP) - 一度だけ使用するために一意に生成されるパスワード
- **オブジェクトリレーショナルマッピング (Object-relational Mapping)** (ORM) - アプリケーション互換オブジェクトモデルを使用して、アプリケーションプログラム内でリレーショナル/テーブルベースのデータベースを参照および照会できるようにするために使用されるシステム
- **パスワードベース鍵導出関数2 (Password-Based Key Derivation Function 2)** (PBKDF2) - 入力テキスト (パスワードなど) および追加のランダムソルト値から強力な暗号鍵を作成するために使用される特殊な一方向アルゴリズム。元のパスワードの代わりに結果の値が保存されている場合には、パスワードをオフラインで解読することが困難になります
- **個人を特定できる情報 Personally Identifiable Information (PII)** - - 単独または他の情報と共に使用して、1 人の人物を識別したり、連絡したり、居場所を特定したり、または状況に応じて個人を識別したりできる情報
- **位置独立実行形式 (Position-independent executable)** (PIE) - 1 次メモリのどこかに配置されて、絶対アドレスに関係なく適切に実行される機械語
- **公開鍵暗号基盤 (Public Key Infrastructure)** (PKI) - 公開鍵をエンティティのそれぞれの ID にバインドする取り決めです。バインディングは、認証局（CA）での証明書の登録および発行のプロセスを通じて確立
- **公衆交換電話網 (Public Switched Telephone Network)** (PSTN) - 固定電話と携帯電話の両方を含む従来の電話網
- **依拠当事者 (Relying Party)** (RP) - 一般的には別の認証プロバイダに対して認証されたユーザに依存するアプリケーション。アプリケーションは認証プロバイダが提供するある種のトークンまたは一連の署名済みアサーションに依存して、ユーザが本人であることを信頼します
- **静的アプリケーションセキュリティテスト (Static application security testing)** (SAST) - アプリケーションのソースコード、バイトコード、セキュリティの脆弱性を示すコーディングおよびバイナリを分析するために設計された一連の技術。SAST ソリューションは、実行されていない状態の「インサイドアウト」からアプリケーションを分析
- **ソフトウェア開発ライフサイクル (Software development lifecycle)** (SDLC) - 初期要件からデプロイメントおよび保守に至るまでのソフトウェア開発における段階的なプロセス
- **セキュリティアーキテクチャ (Security Architecture)** – アプリケーションの設計を抽象化したもの。どこでどのようにセキュリティ管理策を使用しているか、また、ユーザデータとアプリケーションデータを保持する場所とデータの機密性について記述します
- **セキュリティ設定 (Security Configuration)** – アプリケーションにおけるセキュリティの管理を左右するランタイム設定
- **セキュリティ管理 (Security Control)** – – セキュリティチェック(例:アクセス制御の検査など)を実行する、あるいは呼出し結果がセキュリティに影響を与えうる(例：監査レコードの生成など)、機能や構成要素
- **サーバサイドリクエストフォージェリ (Server-side Request Forgery)** (SSRF) - サーバで実行されるコードがデータを読み取りまたは送信する URL を提供または変更することにより、さーばーの機能を悪用して内部リソースを読み取りまたは更新する攻撃
- **シングルサインオン認証 (Single Sign-on Authentication)** (SSO) - ユーザが 1 つのアプリケーションにログインした後に、再認証を必要とせずに自動的に他のアプリケーションにログインする。例えば、Google にログインすると、YouTube、Google Docs、Gmail などの他のGoogle サービスにアクセスすると、自動的にログインします
- **SQL インジェクション (SQL Injection)** (SQLi) – データ駆動型アプリケーションに対する攻撃に使用されるコードインジェクション技法の 1 つ。悪性 SQL 文がデータの入力箇所に挿入されます
- **SVG** - Scalable Vector Graphics
- **タイムベース OTP (Time-based OTP)** - OTP を生成する手法で、現在の時刻がパスワードを生成するアルゴリズムの一部として機能します
- **脅威モデリング (Threat Modelling)** - 脅威の主体、セキュリティゾーン、セキュリティ管理、重要な技術資産やビジネス資産を明らかにするための、精緻なセキュリティアーキテクチャの構築に基づく手法
- **Transport Layer Security** (TLS) – ネットワークの通信セキュリティを提供する暗号プロトコル
- **Trusted Platform Module** (TPM) - 通常はマザーボードなどのより大きなハードウェアコンポーネントに接続され、そのシステムの「信頼の基点 (Root of Trust)」として機能する HSM の一種
- **二要素認証 (Two-factor authentication)** (2FA) - アカウントログインに第二レベルの認証を追加します
- **Universal 2nd Factor** (U2F) - OUSB または NFC セキュリティキーを二番目の認証要素として使用できるようにするために FIDO により作成された標準の一つ
- **URI/URL/URL フラグメント (URI/URL/URL fragments)** – Uniform Resource Identifier (URI) は、名前または Web リソースを識別するために使用される文字列。多くの場合リソースへの参照として使用されます
- **検証者 (Verifier)** – OWASP ASVS の要件に基づいてアプリケーションをレビューする個人またはグループ
- **What You See Is What You Get** (WYSIWYG) - レンダリングを制御するために使用されるコーディングを表示するのではなく、レンダリング時にコンテンツが実際にどのように見えるかを示すリッチコンテンツエディタのタイプ
- **X.509 証明書 (X.509 Certificate)** – X.509 証明書は、広く受け入れられている国際的な X.509 公開鍵暗号基盤（PKI）標準を使用して、公開鍵が証明書に含まれるユーザやコンピュータまたはサービス ID に属していることを確認するデジタル証明書
- **XML 外部エンティティ (XML eXternal Entity)** (XXE) - 宣言されたシステム識別子を介してローカルまたはリモートコンテンツにアクセスできる XML エンティティのタイプ。これはさまざまなインジェクション攻撃につながる可能性があります