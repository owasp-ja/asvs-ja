# ASVS の使い方

アプリケーションセキュリティ検証標準 (ASVS) は、現代の Web アプリケーションとサービスに対するセキュリティ要件を定義し、アプリケーション開発者が制御できる側面に焦点を当てています。

ASVS は、安全なアプリケーションの開発と保守や、アプリケーションのセキュリティ評価を目的とするすべての人に役立ちます。この章では、優先度ベースのレベルや標準のさまざまな使用例など、ASVS を使用する際の重要な側面について説明します。

## アプリケーションセキュリティ検証レベル

ASVS では三つのセキュリティ検証レベルを定義しており、レベルごとに深さと複雑さを増していきます。ASVS の各レベルは、そのレベルを達成するために必要なセキュリティ要件を示します (その他のレベルは推奨事項のままです)。一般的な目標は、組織が最低レベルから始めて、そこからより高いレベルへ上げていくことです。

### レベルの評価基準

バージョン 5.0 のレベル定義へのアプローチは、ASVS ユーザからのフィードバックに基づき、前章で説明したバージョン 5.0 の原則を指針として、ワーキンググループ内での議論を重ねて決定されました。バージョン 5.0 では、セキュリティ要件の実装経験に基づいて、各要件を優先度に基づいて評価します。このアプローチでは、各要件は以下の基準を使用して評価されました。

#### リスク低減

その要件はアプリケーション内のセキュリティ要件をどの程度低減しますか？これは、従来の機密性、完全性、可用性の影響要因を考慮するだけでなく、これが防御の第一層であるか、多層防御とみなされるかを考慮します。

一般的に、リスクを最も大きく低減する要件はレベル 1 またはレベル 2 にあり、それでもなお価値があるものの、多層防御であるか、よりニッチな領域や問題に関連する要件はレベル 3 にあります。

#### 実装の労力

ASVS は検証標準と名付けされていますが、要件がアプリケーションに実装されていない限り、検証するものは何もありません。実装や保守が他の要件よりもはるかに複雑な要件があることは明らかであり、要件の相対的な優先順位を決定する際には、この点を考慮することが重要です。

十分に大きなリスク低減を提供するレベル 1 にも実装が困難なコントロールがいくつかありますが、より複雑な要件はレベル 2 またはレベル 3 になります。

#### 参入障壁の低さ

業界における過去の ASVS バージョンの使用 (または不使用) に関するフィードバックから、特定された唯一最大の問題は、レベル 1 には多数の要件 (およそ120) を有すると同時に、ほとんどのアプリケーションにとって十分ではない「最小」レベルとみなされているという諸刃の剣であることです。これにより、組織は始める前に諦めるか、レベル 1 を実際に達成することなく要件のサブセットを実装しようとして、達成感や進歩感が薄れてしまうようです。

このため、レベル 1 では優先度の高い要件を最大 70 程度とすることが決定されました。これは、達成可能であることと、強固なレベルのセキュリティを提供することとの間の適切な妥協点であるように思われ、他の要件はレベル 2 やレベル 3 に押し込まれることになります。これを実現するために、レベル 1 に含めるものと含めないものについて、いくつかの厳しい決断が下されました。目標は、達成できない完璧なレベル 1 ではなく、達成できる優れたレベル 1 を実現することでした。

#### レベルバランスの改善

バージョン 4.0 では、レベル 1 とレベル 2 の両方におよそ 120 の要件があり、レベル 3 にはおよそ 30 の要件がありました。バージョン 5.0 では、レベル間で要件のバランスを均等にして、レベル 2 とレベル 3 の要件をより均等に配分しようとしています。繰り返しますが、レベル 2 をより達成できる現実的なものにする一方で、レベル 3 は最高レベルのセキュリティを実証したいアプリケーションに残すことが目的です。

### レベルの定義

上記の基準に基づいて、バージョン 5.0 の要件は 3 つのレベルのいずれかに割り当てられました。上記の要因に基づく比較分析は、割り当てに判断の要素があったことを意味しますが、基準とレベル分けの決定の両方に関する厳密な議論の結果、すべての状況に 100% 適合するわけではないことを受け入れつつ、大半のケースに当てはまるように割り当てが行われました。

つまり、特定のケースにおいて、組織はその組織固有のリスク検討に基づいて、早い段階からより高いレベルの要件を優先することを望む可能性があることを意味します。

各レベルの要件の種類は以下のように特徴付けることができます。

#### レベル 1 要件

これらは一般的にクリティカルまたは基本的な、一般的な攻撃を防ぐための第一層の防御要件であり、他の脆弱性や前提条件を必要としません。その要件は実装が比較的簡単であるか、労力に値するほど重要です。

レベル 1 は必ずしも人間によるペネトレーションテストが可能ではありませんが、要件の数が少ないため、検証は比較的容易でしょう。

#### レベル 2 要件

これらは一般的に、あまり一般的ではない攻撃、または一般的な攻撃に対するより複雑な保護のいずれかに関連します。これらは依然として第一層の防御となることもあれば、攻撃が成功するには特定の前提条件を必要とすることもあります。

#### レベル 3 要件

このセクションの要件は一般的に多層防御メカニズムか、その他の有用だが実装が困難なコントロールのいずれかです。また、よりニッチな攻撃や、特定の状況にのみ関連する攻撃に関連するものもあります。

### どのレベルを達成するか

各要件を優先度に基づく評価に移行することで、レベルは組織とアプリケーションのアプリケーションセキュリティ成熟度をより反映したものになります。ASVS は、アプリケーションがどのレベルにあるべきかを規範的に示すのではなく、アプリケーションの機密性と、そしてもちろんアプリケーションのユーザの期待に応じて、組織がどのレベルにあるべきかを判断すべきです。

たとえば、限られた機密データしか収集していない早期段階の新興企業はレベル 1 で十分と判断するかもしれませんが、銀行はオンラインバンキングアプリケーションについて、レベル 3 未満であることを顧客に正当化することは難しいかもしれません。

## ASVS 要件の見方

各要件には `<chapter>.<section>.<requirement>` という形式の識別子があります。各要素は数値です。例: `1.11.3`

* `<chapter>` の値は要件の属する章に対応します。例: `1.#.#` の要件はすべて `アーキテクチャ` の章のものです。
* `<section>` の値は要件が現れる章内のセクションに対応します。例: `1.11.#` の要件はすべて `アーキテクチャ` の章の `ビジネスロジックアーキテクチャ要件` セクションにあります。
* `<requirement>` の値は章およびセクション内の特定の要件を識別します。例: この標準のバージョン 4.0.2 での `1.11.3` は以下のとおりです。

> 認証、セッション管理、アクセス制御などを含む重要度の高いすべてのビジネスロジックフローがスレッドセーフであり、チェック時間と使用時間（TOCTOU）の競合状態に対して耐性がある。

識別子は標準のバージョン間で変更となる可能性があるため、他のドキュメント、レポート、ツールでは `v<version>-<chapter>.<section>.<requirement>` という形式を使用することを推奨します。ここでは 'version' は ASVS バージョンタグです。例: `v4.0.2-1.11.3` はバージョン 4.0.2 の 'アーキテクチャ' の章の 'ビジネスロジックアーキテクチャ要件' セクションの 3 番目にある要件を意味すると理解できます。 (これは `v<version>-<requirement_identifier>` と要約できます。)

注: この形式でバージョン番号の前にある `v` は常に小文字にする必要があります。

`v<version>` 要素を含めずに識別子を使用する場合には、最新のアプリケーションセキュリティ検証標準コンテンツを参照していると想定すべきです。標準の進化や変更に伴いこれが問題になるため、執筆者や開発者はバージョン要素を含める必要があります。

ASVS 要件リストは CSV、JSON、および参照またはプログラムでの使用に役立つ可能性があるその他の形式で提供されています。

## ASVS のフォーク

組織は ASVS を採用することでメリットがあります。三つのレベルのいずれかを選択するか、ドメイン固有のフォークを作成して、アプリケーションリスクレベルごとに要件を調整します。トレーサビリティが維持されている限りこのようなフォークを推奨します。要件 4.1.1 に合格することがすべてのバージョンで同じことを意味するようにします。

理想的には、各組織が独自に調整した ASVS を作成し、関係のないセクション (未使用の場合、GraphQL、Websockets、SOAP など) を省略すべきです。フォークは ASVS レベル 1 をベースラインとして始めて、アプリケーションのリスクに基づいてレベル 2 または 3 に進めるべきです。

## ASVS の用途

ASVS はアプリケーションのセキュリティを評価するために使用できます。これについては次の章で詳しく説明しますが、ASVS (もしくはフォーク版) のその他の潜在的な用途がいくつか考えられています。

### 詳細なセキュリティアーキテクチャガイダンスとして

アプリケーションセキュリティ検証標準のより一般的な用途の一つはセキュリティアーキテクトのためのリソースです。Sherwood Applied Business Security Architecture (SABSA) にはアプリケーションセキュリティアーキテクチャの十分なレビューを完了するために必要な多くの情報が欠けています。ASVS を使用して、セキュリティアーキテクトがデータ保護パターンや入力バリデーション戦略などの一般的な問題に対してより適切な管理策を選択できるようにすることで、これらのギャップを埋めることができます。

### 専用のセキュアコーディングチェックリストとして

ASVS はセキュアアプリケーション開発のためのセキュアコーディングチェックリストとして使用でき、開発者がソフトウェアを構築する際にセキュリティを考慮するよう支援します。セキュアコーディングチェックリストは統一され、明確で、すべての開発チームに適用可能であるべきです。理想的にはセキュリティエンジニアやセキュリティアーキテクトの指導に基づいて作成されるべきです。

### 自動ユニットテストおよび自動統合テストのガイドとして

ASVS は、アーキテクチャ要件およびドキュメント要件を除いて、高度にテスト可能なように設計されています。特定の関連する悪用のケースをテストするユニットテストや統合テストやファジングを構築することにより、各ビルドで実装されたコントロールを検証しやすくなります。例えば、ログインコントローラのテストスイートとして、一般的なデフォルトユーザ名、アカウント列挙、総当たり攻撃、LDAP と SQL インジェクション、XSS のユーザ名パラメータをテストする追加のテストを作成することができます。同様に、パスワードパラメータのテストには一般的なパスワード、パスワード長、null バイトインジェクション、パラメータの削除、XSS などを含める必要があります。

### セキュア開発トレーニングのために

ASVS はセキュアソフトウェアの特性を定義するためにも使用できます。多くの「セキュアコーディング」コースはコーディングのヒントがわずかにあるだけの単なるエシカルハッキングコースです。これは開発者がよりセキュアなコードを書くのに必ずしも役に立つとは限りません。代わりに、セキュア開発コースでは、してはいけないことの Top 10 ネガティブ項目ではなく、ASVS にある予防的管理策に重点を置いて ASVS を使用できます。

### アジャイルアプリケーションセキュリティの牽引役として

ASVS は、セキュアな製品を開発するためにチームが実装する必要がある特定のタスクを定義するためのフレームワークとして、アジャイル開発プロセスで使用できます。一つのアプローチとして、レベル 1 から始めて、指定されたレベルの ASVS 要件に従い特定のアプリケーションやシステムを検証し、どの管理策が欠けているかを見つけ、バックログに特定のチケットやタスクを上げます。これは特定のタスクの優先度付け (または調整) に役立ち、アジャイルプロセスでセキュリティを可視化します。このアプローチは組織内の監査タスクおよびレビュータスクの優先度付けにも使用できます。特定の ASVS 要件は特定のチームメンバーのレビュー、リファクタリング、監査を牽引し、最終的に対処しなければならないバックログ内の「負債」として可視化します。

### セキュアなソフトウェアの調達をガイドするためのフレームワークとして

ASVS は、セキュアなソフトウェアの調達やカスタム開発サービスの調達を支援する優れたフレームワークです。調達者は単に入手したいソフトウェアを ASVS レベル X で開発しなければならないという要件を設定し、そのソフトウェアが ASVS レベル X を満たすことを販売者に証明するよう要求できます。これは OWASP Secure Software Contract Annex と組み合わせると効果的です。

## 実際に ASVS を適用する

脅威が異なれば動機も異なります。一部の業界では独自の情報資産と技術資産があり、ドメイン固有の規制順守要件があります。

組織はそのビジネスの性質に基づく独自のリスク特性を詳細に検討し、そのリスクとビジネス要件に基づいて適切な ASVS レベルを決定することを強く推奨します。
