# バージョン 4.0 ユーザ向けのガイダンス

標準のバージョン 4.0 の既存ユーザーにとっては、バージョン 5.0 の内容におけるいくつかの主な変更点と、バージョン 5.0 の開発の中で行われたスコープと理念のいくつかの変更点を確認するのに役立つと思われます。

## 5.0 の新規情報

以下のセクションでは最も主要な変更点について説明します。

### 完全な番号再割り当てと再配置

ほぼすべての要件が何らかの形で変更されており、変更や移動されていないものであっても、再配置や他の要件の移動の結果として番号を変更しています。

提供されているマッピングはバージョン 4.0 の要件がバージョン 5.0 に取り込まれているかどうか、どこに取り込まれているかを追跡するのに役立つはずです。

### NIST Digital Identity Guidelines との少ない結びつき

NIST の [Digital Identity Guidelines (SP 800-63)](https://pages.nist.gov/800-63-3/) は、認証と認可に関する主要なコントロールのための優れたエビデンス主導の標準であり、今後もそうあり続けるでしょう。バージョン 4.0 の特定の章は、構造や用語など、これらのガイドラインと密接に結びついています。

これらのガイドラインとその後の改善は重要なリファレンスと多くの要件の基礎であり続けていますが、厳密な結びつきは課題を引き起こし、このアプローチから脱却する決断に至りました。これらの課題には、あまり広く認識されていない用語、わずかに異なる状況での非常に類似した要件の重複、ASVS に関連があると認識された事項に基づくマッピングが不完全であるという事実などがありました。

### 共通脆弱性タイプ一覧 (Common Weakness Enumeration, CWE) からの脱却

Mitre の [Common Weakness Enumeration (CWE)](https://cwe.mitre.org/) はソフトウェアのさまざまなセキュリティ脆弱性をマップする便利な方法です。特定の CWE はカテゴリのみでありマッピングに使用すべきではないなど使用が困難であったり、特定の既存の要件を一つの CWE にマップするのは困難であったり、ASVS のバージョン 4.0 では緩いマッピングや不正確なマッピングがあったという事実もあります。

解決策は CWE (およびその他のマッピング) を削除し、代わりに ASVA を他のさまざまな OWASP プロジェクトや外部標準にマップする OWASP Common Requirement Enumeration (CRE) プロジェクトにマップすることを目指しました。

### メカニズムではなく目標に焦点を当てる

バージョン 4.0 では、達成すべきセキュリティ目標に焦点を当てるのではなく、特定のメカニズムに焦点を当てた要件が多くありました。バージョン 5.0 では、目標を達成するための現実的なメカニズムが一つしかない場合を除き、要件はセキュリティ目標に焦点を当て、特定のメカニズムは例として含めるか、他のガイダンスにリンクします。

### セキュリティに関する決定事項の文書化

特定の要件については、実装が複雑になり、アプリケーションのニーズに非常に特有となります。よくある例としては、パーミッション、入力バリデーション、さまざまなレベルの機密データに関する保護コントロールなどがあります。これを考慮するために、「すべてのデータは暗号化しなければならない」などの極論的な文言や、要件として考えられるすべてのユースケースをカバーしようとするのではなく、アプリケーション開発者のこの種のコントロールに対するアプローチと構成を文書化することを義務付ける特定の要件があります。これにより適切かどうかをレビューし、実際の実装をドキュメントと比較して、実装が期待と合致しているかどうかを評価できます。

<!--

### TODO: add more items

We set out to ensure that the ASVS 4.0 Level 1 is a comprehensive superset of PCI DSS 3.2.1 Sections 6.5, for application design, coding, testing, secure code reviews, and penetration tests. This necessitated covering buffer overflow and unsafe memory operations in V5, and unsafe memory-related compilation flags in V14, in addition to existing industry-leading application and web service verification requirements.

We have completed the shift of the ASVS from monolithic server-side-only controls, to providing security controls for all modern applications and APIs. In the days of functional programming, server-less API, mobile, cloud, containers, CI/CD and DevSecOps, federation and more, we cannot continue to ignore modern application architecture. Modern applications are designed very differently from those built when the original ASVS was released in 2009. The ASVS must always look far into the future so that we provide sound advice for our primary audience - developers. We have clarified or dropped any requirement that assumes that applications are executed on systems owned by a single organization.

Due to the size of the ASVS 4.0, as well as our desire to become the baseline ASVS for all other ASVS efforts, we have retired the mobile chapter, in favor of the Mobile Application Security Verification Standard (MASVS). We have also retired the Internet of Things appendix, in favor of the IoT Security Verification Standard (ISVS). We thank both the OWASP Mobile Team and OWASP IoT Project Team for their support of the ASVS, and look forward to working with them in the future to provide complementary standards.

Lastly, we have de-duped and retired less impactful controls. Over time, the ASVS started being a comprehensive set of controls, but not all controls equally contribute to producing secure software. This effort to eliminate low-impact items could go further. In a future edition of the ASVS, the Common Weakness Scoring System (CWSS) will help prioritize further those controls that are truly important and those that should be retired.

As of version 4.0, the ASVS will focus solely on being the leading web apps and service standard, covering traditional and modern application architecture, agile security practices and DevSecOps culture.
-->

## レベルアプローチを変更する追加の理論的根拠

ASVS バージョン 4.0 では、レベルを L1 「最小 (Minimum)」、L2 「標準 (Standard)」、L3 「上級 (Advanced)」 と説明しており、機密データを処理するすべてのアプリケーションは少なくとも L2 であるべきということを示しています。

私たちはこのアプローチにいくつかの課題を見つけました。バージョン 4.0 のユーザは前の章の理論的根拠に加えて、レベルアプローチへの変更に関する以下の背景が参考になるかもしれません。

### 参入障壁の高さ

バージョン 4.0 の L1 には 100 を超える要件があり、L2 も同様であり、L3 にはわずかな要件しか残っていませんでした。これは、L1 を達成するだけでも多大な労力が必要であり、その時点でこれが「最小 (Minimum)」レベルであり、「標準 (Standard)」レベルのセキュリティを達成するには、さらに 100 の要件が必要であると知らされることを意味します。ASVS ユーザとコミュニティからのフィードバックに基づいて、これが阻害要因となり、アプリケーションが ASVS を採用し始めることを困難にしていることが明らかになりました。

### 試験性の誤り

バージョン 4.0 の L1 にコントロールを入れた主な動機は、L1 が最小限のセキュリティコントロールであるというコンセプトとは完全には沿わない、「ブラックボックス」スタイルのテストを使用してチェックできるかどうかということでした。一方では、ASVS ユーザは L1 では安全なアプリケーションには不十分だと言い、他方では ASVS はテストが難しすぎると不満を漏らしています。

さらに、試験性は相対的なものであり、誤解を招く場合もあります。テスト可能だからといって、それが自動化された方法や簡単な方法でテスト可能であるとは限りません。最後に、最もテスト可能な要件は、必ずしもセキュリティに最も重要な影響を与えるものでも、最も簡単に実装できるものでもありません。

### リスクだけではない

特定のアプリケーションが特定のレベルに達しなければならないというような、規範的なリスクベースのレベルの使用は、振り返って考えてみると過度に独断的であるように思われます。実際には、セキュリティ対策を実施する順序は、リスク低減と実施の労力の両方を含む要因に依存します。
