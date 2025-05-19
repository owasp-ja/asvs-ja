# 付録 V: 暗号化

「暗号化」の章は単にベストプラクティスを定義するだけではありません。暗号の原則に対する理解を深め、より耐性のある最新のセキュリティ手法の採用を促進することを目的としています。この付録では、「暗号化」の章で概説されている包括的な標準を保管するために、各要件に関する詳細な技術情報を提供します。

この付録ではさまざまな暗号メカニズムの承認レベルを定義します。

* **承認済み (Approved)** (A) メカニズムはアプリケーションで使用できます。
* **レガシー (Legacy)** メカニズム (L) はアプリケーションで使用すべきではありませんが、既存のレガシーアプリケーションやコードとの互換性のためだけに依然として使用されるかもしれません。このようなメカニズムの使用自体は現在のところ脆弱性とはみなされませんが、できるだけ早く、より安全で将来性のあるメカニズムに置き換えるべきです。
* **不許可 (Disallowed)** メカニズム (D) は、現時点で破壊されているとみなされているか、十分なセキュリティを提供していないため、使用してはいけません。

このリストは、以下のようなさまざまな理由により、特定のアプリケーションのコンテキストにおいて上書きされる可能性があります。

* 暗号分野における新たな進化
* 規制への準拠

## 暗号インベントリとドキュメント (V11.1)

アルゴリズム、鍵、証明書などの暗号資産を定期的に発見し、インベントリ化し、評価することが重要です。レベル 3 では、これにはアプリケーションでの暗号の使用を発見するための静的スキャンおよび動的スキャンの使用を含むべきです。SAST や DAST などのツールがこれに役立つかもしれませんが、より包括的なカバレッジを得るには専用のツールが必要になるかもしれません。フリーウェアのツールの例には以下のものがあります。

* [CryptoMon - Network Cryptography Monitor - using eBPF, written in python](https://github.com/Santandersecurityresearch/CryptoMon)
* [Cryptobom Forge Tool: Generating Comprehensive CBOMs from CodeQL Outputs](https://github.com/Santandersecurityresearch/cryptobom-forge)

## アルゴリズム (V11.2)

## 暗号パラメータの等価な強度

さまざまな暗号システムの相対的なセキュリティ強度は以下の表のとおりです ([NIST SP 800-57 Part 1](https://csrc.nist.gov/pubs/sp/800/57/pt1/r5/final), p.71 より):

| セキュリティ強度 | 対称鍵アルゴリズム | 有限体 | 整数因数分解 | 楕円曲線 |
|--|--|--|--|--|
| <= 80 | 2TDEA | L = 1024 <br> N = 160 | k = 1024 | f = 160-223 |
| 112 | 3TDEA   | L = 2048 <br> N = 224 | k = 2048 | f = 224-255 |
| 128 | AES-128 | L = 3072 <br> N = 256 | k = 3072 | f = 256-383 |
| 192 | AES-192 | L = 7680 <br> N = 384 | k = 7680 | f = 384-511 |
| 256 | AES-256 | L = 15360 <br> N = 512 | k = 15360 | f = 512+ |

適用例:

* 有限体暗号: DSA, FFDH, MQV
* 整数因数分解暗号: RSA
* 楕円曲線暗号: ECDSA, EdDSA, ECDH, MQV

注: このセクションでは量子コンピュータが存在しないことを前提としています。そのようなコンピュータが存在する場合、最後の 3 列の推定値はもはや有効ではなくなります。

## 乱数値 (V11.5)

| 名称 | バージョン/リファレンス | 備考 | ステータス |
|:-:|:-:|:-:|:-:|
| `/dev/random` | Linux 4.8 以降 [(2016 年 10 月)](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=818e607b57c94ade9824dad63a96c2ea6b21baf3)、iOS、Android、その他の Linux ベースの POSIX オペレーティングシステムにもあります。[RFC7539](https://datatracker.ietf.org/doc/html/rfc7539) に基づいています。 | ChaCha20 ストリームを利用します。iOS [`SecRandomCopyBytes`](https://developer.apple.com/documentation/security/secrandomcopybytes(_:_:_:)?language=objc) および Android [`Secure Random`](https://developer.android.com/reference/java/security/SecureRandom) にあり、それぞれに正しい設定が提供されています。 | A |
| `/dev/urandom` | ランダムデータを提供するための Linux カーネルの特殊ファイルです。 | ハードウェアのランダム性から高性能のエントロピーソースを提供します。 | A |
| `AES-CTR-DRBG` | [NIST SP800-90A](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-90Ar1.pdf) | [`BCRYPT_RNG_ALGORITHM`](https://learn.microsoft.com/en-us/windows/win32/seccng/cng-algorithm-identifiers) で設定された [Windows CNG API `BCryptGenRandom`](https://learn.microsoft.com/en-us/windows/win32/api/bcrypt/nf-bcrypt-bcryptgenrandom) などの一般的な実装で使用されます。 | A |
| `HMAC-DRBG` | [NIST SP800-90A](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-90Ar1.pdf) | | A |
| `Hash-DRBG` | [NIST SP800-90A](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-90Ar1.pdf) | | A |
| `getentropy()` | [OpenBSD](https://man.openbsd.org/getentropy.2)、[Linux glibc 2.25 以降](https://man7.org/linux/man-pages/man3/getentropy.3.html)、[macOS 10.12 以降](https://support.apple.com/en-gb/guide/security/seca0c73a75b/web) で利用可能です。 | シンプルで最小限の API で、カーネルのエントロピーソースから安全なランダムバイトを直接提供します。より現代的であり、古い API に関連する落とし穴を回避します。 | A |

HMAC-DRBG または Hash-DRBG で使用される基礎となるハッシュ関数はこの用途に対して承認されていなければなりません。

## 暗号アルゴリズム (V11.3)

承認済み暗号アルゴリズムを優先順に並べています。

| 対称鍵アルゴリズム | リファレンス | ステータス |
|--|--|--|
| AES-256 | [FIPS 197](https://csrc.nist.gov/pubs/fips/197/final) | A |
| Salsa20 | [Salsa 20 specification](https://cr.yp.to/snuffle/spec.pdf) | A |
| XChaCha20 | [XChaCha20 Draft](https://datatracker.ietf.org/doc/html/draft-irtf-cfrg-xchacha-03) | A |
| XSalsa20 | [Extending the Salsa20 nonce](https://cr.yp.to/snuffle/xsalsa-20110204.pdf) | A |
| ChaCha20 | [RFC 8439](https://www.rfc-editor.org/info/rfc8439) | A |
| AES-192 | [FIPS 197](https://csrc.nist.gov/pubs/fips/197/final) | A |
| AES-128 | [FIPS 197](https://csrc.nist.gov/pubs/fips/197/final) | L |
| 2TDEA | | D |
| TDEA (3DES/3DEA) | | D |
| IDEA | | D |
| RC4 | | D |
| Blowfish| | D |
| ARC4 | | D |
| DES | | D |

### AES 暗号モード

現代の暗号はさまざまな目的でさまざまなモード、特に AES を使用しています。ここでは AES 暗号モードの要件について説明します。一部の暗号モードはディスクレベルのブロック暗号にのみ承認されています。

| モード | 認証済み | リファレンス | ステータス | 制限 |
|--|--|--|--|--|
| GCM | Yes | [NIST SP 800-38D](https://csrc.nist.gov/pubs/sp/800/38/d/final) | A | |
| CCM | Yes | [NIST SP 800-38C](https://csrc.nist.gov/pubs/sp/800/38/c/upd1/final) | A | |
| CBC | No | [NIST SP 800-38A](https://csrc.nist.gov/pubs/sp/800/38/a/final) | A | |
| XTS | No | [NIST SP 800-38E](https://csrc.nist.gov/pubs/sp/800/38/e/final) | A | ディスクレベルのブロック暗号用のみ。 |
| XEX | No | [Rogaway 2004](https://doi.org/10.1007/978-3-540-30539-2_2) | A | ディスクレベルのブロック暗号用のみ。 |
| LRW | No | [Liskov, Rivest, and Wagner 2005](https://doi.org/10.1007/s00145-010-9073-y) | A | ディスクレベルのブロック暗号用のみ。 |
| ECB | No | | D | |
| CFB | No | | D | |
| OFB | No | | D | |
| CTR | No | | D | |
| CCM-8 | Yes | | D | |

承認済みモードを優先順に並べています。

注:

* すべての暗号化されたメッセージは認証されなければなりません。このため、CBC モードを使用する場合は必ず、メッセージを検証するためにハッシュ関数または MAC が関連付けられていなければなりません (MUST)。一般的に、これは Encrypt-Then-Hash 方式で適用されなければなりません (MUST) (ただし、TLS 1.2 では代わりに Hash-Then-Encrypt を使用します)。これが保証できない場合、CBC を使用してはいけません (MUST NOT)。
* CCM-8 を使用する場合、MAC タグは 64 ビットのセキュリティしか持ちません。これは、少なくとも 128 ビットのセキュリティを必要とする要件 6.2.9 に準拠していません。

### 鍵ラッピング

暗号鍵ラップ (および対応する鍵アンラップ) は、追加の暗号メカニズムを使用して既存の鍵をカプセル化 (つまりラップ) し、転送中などに元の鍵が明らかに露出しないようにすることで、既存の鍵を保護する方法です。元の鍵を保護するために使用されるこの追加の鍵はラップ鍵と呼ばれます。

この操作は、信頼できないとみなされる場所で鍵を保護したい場合、あるいは信頼できないネットワーク上やアプリケーション内で機密鍵を送信したい場合に実行できます。
ただし、ラップ/アンラップ手順を実行する前に、元の鍵の性質 (アイデンティティや目的など) を理解することを真剣に検討すべきです。これは、セキュリティと、特に鍵の機能 (署名など) の監査証跡や適切な鍵の保存を含むコンプライアンスの点で、ソースとターゲットの両方のシステム/アプリケーションに影響を及ぼす可能性があります。

特に、鍵ラッピングには、[NIST SP 800-38F](https://csrc.nist.gov/pubs/sp/800/38/f/final) に従い、量子脅威に対する将来を見据えた対策を考慮して、AES-256 を使用しなければなりません (MUST)。AES を使用する暗号モードは優先順に以下のとおりです:

| 鍵ラッピング | リファレンス | ステータス |
|--|--|--|
| KW | [NIST SP 800-38F](https://csrc.nist.gov/pubs/sp/800/38/f/final) | A |
| KWP | [NIST SP 800-38F](https://csrc.nist.gov/pubs/sp/800/38/f/final) | A |

AES-192 と AES-128 はユースケースで必要な場合に使用できます (MAY) が、その理由はエンティティの暗号インベントリに文書化されなければなりません (MUST)。

### 認証された暗号

ディスク暗号化を除いて、暗号化されたデータは何かしらの形で認証された暗号 (AE) スキーム、通常は関連データ付き認証暗号 (AEAD) スキームを使用して、認可されていない変更から保護されなければなりません。

アプリケーションは承認済みの AEAD スキームを使用することをお勧めします。代わりに、承認済みの暗号スキームと承認済みの MAC アルゴリズムを Encrypt-then-MAC 構造で組み合わせることもできます。

MAC-then-encrypt はレガシーアプリケーションとの互換性のために依然として許可されています。これは古い暗号スイートを使用する TLS v1.2 で使用されます。

| AEAD メカニズム | リファレンス | ステータス |
|-----------------|--------------|------------|
| AES-GCM | [SP 800-38D](https://csrc.nist.gov/pubs/sp/800/38/d/final) | A |
| AES-CCM | [SP 800-38C](https://csrc.nist.gov/pubs/sp/800/38/c/upd1/final) | A |
| ChaCha-Poly1305 | [RFC 7539](https://datatracker.ietf.org/doc/html/rfc7539) | A |
| Encrypt-then-MAC | | A |
| MAC-then-encrypt | | L |

## ハッシュ関数 (V11.4)

### 一般的なユースケースでのハッシュ関数

以下の表は、デジタル署名などの一般的な暗号ユースケースで承認されているハッシュ関数を示しています。

* 承認済みハッシュ関数は強力な衝突耐性を備えており、高セキュリティのアプリケーションに適しています。
* これらのアルゴリズムの中には、適切な暗号鍵管理と併用すると攻撃に対する強力な耐性を発揮するものがあるため、HMAC、KDF、RBG 機能についても追加的に承認されています。
* 出力が 254 ビット未満のハッシュ関数は衝突耐性が不十分であるため、デジタル署名や衝突耐性を必要とするその他の用途には使用してはいけません。その他の用途では、レガシーシステムとの互換性と検証にのみ使用できます (ONLY) が、新しい設計には使用してはいけません。

| ハッシュ関数 | リファレンス | ステータス | 制限 |
| ------------ | ------------ | ---------- | ---- |
| SHA3-512 | [FIPS 202](https://csrc.nist.gov/pubs/fips/202/final) | A | |
| SHA-512 | [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | A | |
| SHA3-384 | [FIPS 202](https://csrc.nist.gov/pubs/fips/202/final) | A | |
| SHA-384 | [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | A | |
| SHA3-256 | [FIPS 202](https://csrc.nist.gov/pubs/fips/202/final) | A | |
| SHA-512/256 | [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | A | |
| SHA-256 | [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | A | |
| SHAKE256 | [FIPS 202](https://csrc.nist.gov/pubs/fips/202/final) | A | |
| BLAKE2s | [BLAKE2: simpler, smaller, fast as MD5](https://eprint.iacr.org/2013/322) | A | |
| BLAKE2b | [BLAKE2: simpler, smaller, fast as MD5](https://eprint.iacr.org/2013/322) | A | |
| BLAKE3 | [BLAKE3 one function, fast everywhere](https://github.com/BLAKE3-team/BLAKE3-specs/raw/master/blake3.pdf) | A | |
| SHA-224 | [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | L | Not suitable for HMAC, KDF, RBG, digital signatures |
| SHA-512/224 | [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | L | Not suitable for HMAC, KDF, RBG, digital signatures |
| SHA3-224 | [FIPS 202](https://csrc.nist.gov/pubs/fips/202/final) | L | Not suitable for HMAC, KDF, RBG, digital signatures |
| SHA-1 | [RFC 3174](https://www.rfc-editor.org/info/rfc3174) & [RFC 6194](https://www.rfc-editor.org/info/rfc6194) | L | Not suitable for HMAC, KDF, RBG, digital signatures |
| CRC (any length) |  | D |  |
| MD4 | [RFC 1320](https://www.rfc-editor.org/info/rfc1320) | D | |
| MD5 | [RFC 1321](https://www.rfc-editor.org/info/rfc1321) | D | |

### パスワード保存のためのハッシュ関数

安全なパスワードハッシュには、専用のハッシュ関数を使用しなければなりません。これらの低速ハッシュアルゴリズムは、パスワードクラッキングの計算難易度を上げることで、ブルートフォース攻撃や辞書攻撃を軽減します。

| 鍵導出関数 | リファレンス | 必要なパラメータ | ステータス |
| ---------- | ------------ | ---------------- | ---------- |
| argon2id | [RFC 9106](https://www.rfc-editor.org/info/rfc9106) | t = 1: m ≥ 47104 (46 MiB), p = 1<br>t = 2: m ≥ 19456 (19 MiB), p = 1<br>t ≥ 3: m ≥ 12288 (12 MiB), p = 1 | A |
| scrypt | [RFC 7914](https://www.rfc-editor.org/info/rfc7914) | p = 1: N ≥ 2^17 (128 MiB), r = 8<br>p = 2: N ≥ 2^16 (64 MiB), r = 8<br>p ≥ 3: N ≥ 2^15 (32 MiB), r = 8 | A |
| bcrypt | [A Future-Adaptable Password Scheme](https://www.researchgate.net/publication/2519476_A_Future-Adaptable_Password_Scheme) | cost ≥ 10 | A |
| PBKDF2-HMAC-SHA-512 | [NIST SP 800-132](https://csrc.nist.gov/pubs/sp/800/132/final), [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | iterations ≥ 210,000 | A |
| PBKDF2-HMAC-SHA-256 | [NIST SP 800-132](https://csrc.nist.gov/pubs/sp/800/132/final), [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | iterations ≥ 600,000 | A |
| PBKDF2-HMAC-SHA-1 | [NIST SP 800-132](https://csrc.nist.gov/pubs/sp/800/132/final), [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | iterations ≥ 1,300,000 | L |

承認済みのパスワードベースの鍵導出関数はパスワードストレージとして使用できます。

## 鍵導出関数 (KDF)

### 汎用の鍵導出関数

| 鍵導出関数       | リファレンス                                                                                  | ステータス |
| ---------------- | --------------------------------------------------------------------------------------------- | ---------- |
| HKDF             | [RFC 5869](https://www.rfc-editor.org/info/rfc5869)                                           | A          |
| TLS 1.2 PRF      | [RFC 5248](https://www.rfc-editor.org/info/rfc5248)                                           | L          |
| MD5-based KDFs   | [RFC 1321](https://www.rfc-editor.org/info/rfc1321)                                           | D          |
| SHA-1-based KDFs | [RFC 3174](https://www.rfc-editor.org/info/rfc3174) & [RFC 6194](https://www.rfc-editor.org/info/rfc6194) | D          |

### パスワードベースの鍵導出関数

| 鍵導出関数 | リファレンス | 必要なパラメータ | ステータス |
| ---------- | ------------ | ---------------- | ---------- |
| argon2id | [RFC 9106](https://www.rfc-editor.org/info/rfc9106) | t = 1: m ≥ 47104 (46 MiB), p = 1<br>t = 2: m ≥ 19456 (19 MiB), p = 1<br>t ≥ 3: m ≥ 12288 (12 MiB), p = 1 | A |
| scrypt | [RFC 7914](https://www.rfc-editor.org/info/rfc7914) | p = 1: N ≥ 2^17 (128 MiB), r = 8<br>p = 2: N ≥ 2^16 (64 MiB), r = 8<br>p ≥ 3: N ≥ 2^15 (32 MiB), r = 8 | A |
| PBKDF2-HMAC-SHA-512 | [NIST SP 800-132](https://csrc.nist.gov/pubs/sp/800/132/final), [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | iterations ≥ 210,000 | A |
| PBKDF2-HMAC-SHA-256 | [NIST SP 800-132](https://csrc.nist.gov/pubs/sp/800/132/final), [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | iterations ≥ 600,000 | A |
| PBKDF2-HMAC-SHA-1 | [NIST SP 800-132](https://csrc.nist.gov/pubs/sp/800/132/final), [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | iterations ≥ 1,300,000 | L |

## 鍵交換メカニズム (V11.6)

### KEX スキーム

すべての鍵交換スキームに対して 112 ビット以上のセキュリティ強度が確保されていなければならず (MUST)、その実装は以下の表のパラメータ選択に従わなければなりません (MUST)。

| スキーム | ドメインパラメータ | 前方秘匿性 | ステータス |
|--|--|--|--|
| Finite Field Diffie-Hellman (FFDH) | L >= 3072 & N >= 256 | Yes | A |
| Elliptic Curve Diffie-Hellman (ECDH) | f >= 256-383 | Yes | A |
| Encrypted key transport with RSA-PKCS#1 v1.5 | k >= 3072 | No | L |

ここでのパラメータは以下のとおりです:

* k は RSA 鍵の鍵サイズです。
* L は有限体暗号の公開鍵 (public key) のサイズであり、N は秘密鍵 (private key) のサイズです。
* f は ECC の鍵サイズの範囲です。

新しい実装では [NIST SP 800-56A](https://csrc.nist.gov/pubs/sp/800/56/a/r3/final) & [B](https://csrc.nist.gov/pubs/sp/800/56/b/r2/final) および [NIST SP 800-77](https://csrc.nist.gov/pubs/sp/800/77/r1/final) に準拠していないスキームを使用してはいけません (MUST NOT)。特に IKEv1 は本番環境で使用してはいけません (MUST NOT)。

### Diffie-Hellman グループ

以下のグループが承認されており、Diffie-Hellman KEX の実装に使用しなければなりません (MUST)。IKEv2 グループはリファレンスとして提供されています ([NIST SP 800-77](https://csrc.nist.gov/pubs/sp/800/77/r1/final))。同等のグループが他のプロトコルで使用されるかもしれません。このリストは最も強力なものから最も弱いものの順に並べられています。セキュリティ強度は [NIST SP 800-56A](https://csrc.nist.gov/pubs/sp/800/56/a/r3/final), Appendix D および [NIST SP 800-57 Part 1 Rev.5](https://csrc.nist.gov/pubs/sp/800/57/pt1/r5/final) に記載されています。

| グループ | スキーム | パラメータ | セキュリティビット | ステータス |
|--|--|--|--|--|
| 21 | ECC | 521-bit random ECP group | 260 | A |
| 32 | ECC | Curve448 | 224 | A |
| 18 | MODP | 8192-bit MODP Group | 192 < 200 | A |
| 20 | ECC | 384-bit random ECP group | 192 | A |
| 17 | MODP | 6144-bit MODP Group | 128 < 176 | A |
| 16 | MODP | 4096-bit MODP Group | 128 < 152 | A |
| 31 | ECC | Curve25519 | 128 | A |
| 19 | ECC | 256-bit random ECP group | 128 | A |
| 15 | MODP | 3072-bit MODP Group | 128 | A |
| 14 | MODP | 2048-bit MODP Group | 112 | A |

## メッセージ認証コード (MAC)

メッセージ認証コード (MAC) は、メッセージの完全性と真正性を検証するために使用される暗号構造です。MAC は、メッセージと共有鍵 (secret key) を入力として受け取り、固定サイズのタグ (MAC 値) を生成します。MAC は安全な通信プロトコル (TLS/SSL など) で広く使用され、パーティ間で交換されるメッセージが本物で無傷であることを確保します。

| MAC アルゴリズム  | リファレンス | ステータス | 制限 |
| ----------------- | ------------ | ---------- | ---- |
| HMAC-SHA-256  | [RFC 2104](https://www.rfc-editor.org/info/rfc2104) & [FIPS 198-1](https://csrc.nist.gov/pubs/fips/198-1/final) | A | |
| HMAC-SHA-384  | [RFC 2104](https://www.rfc-editor.org/info/rfc2104) & [FIPS 198-1](https://csrc.nist.gov/pubs/fips/198-1/final) | A | |
| HMAC-SHA-512  | [RFC 2104](https://www.rfc-editor.org/info/rfc2104) & [FIPS 198-1](https://csrc.nist.gov/pubs/fips/198-1/final) | A | |
| KMAC128       | [NIST SP 800-185](https://csrc.nist.gov/pubs/sp/800/185/final)                             | A | |
| KMAC256       | [NIST SP 800-185](https://csrc.nist.gov/pubs/sp/800/185/final)                             | A | |
| BLAKE3        | [BLAKE3 one function, fast everywhere](https://github.com/BLAKE3-team/BLAKE3-specs/raw/master/blake3.pdf)  | A | |
| AES-CMAC      | [RFC 4493](https://datatracker.ietf.org/doc/html/rfc4493) & [NIST SP 800-38B](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-38b.pdf) | A | |
| AES-GMAC      | [NIST SP 800-38D](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-38d.pdf)            | A | |
| Poly1305-AES  | [The Poly1305-AES message-authentication code](https://cr.yp.to/mac/poly1305-20050329.pdf)                  | A | |
| HMAC-SHA-1    | [RFC 2104](https://www.rfc-editor.org/info/rfc2104) & [FIPS 198-1](https://csrc.nist.gov/pubs/fips/198-1/final) | L | |
| HMAC-MD5      | [RFC 1321](https://www.rfc-editor.org/info/rfc1321)                                | D      | |

## デジタル署名

署名スキームは [NIST SP 800-57 Part 1](https://csrc.nist.gov/pubs/sp/800/57/pt1/r5/final) に従って承認された鍵サイズとパラメータを使用しなければなりません (MUST):

| 署名アルゴリズム               | リファレンス                                               | ステータス |
| ------------------------------ | ---------------------------------------------------------- | ---------- |
| EdDSA (Ed25519, Ed448)         | [RFC 8032](https://www.rfc-editor.org/info/rfc8032)        | A      |
| XEdDSA (Curve25519, Curve448)  | [XEdDSA](https://signal.org/docs/specifications/xeddsa/)   | A      |
| ECDSA (P-256, P-384, P-521)    | [FIPS 186-4](https://csrc.nist.gov/pubs/fips/186-5/final)  | A      |
| RSA-RSSA-PSS                   | [RFC 8017](https://www.rfc-editor.org/info/rfc8017)        | A      |
| RSA-SSA-PKCS#1 v1.5            | [RFC 8017](https://www.rfc-editor.org/info/rfc8017)        | D      |
| DSA (any key size)             | [FIPS 186-4](https://csrc.nist.gov/pubs/fips/186-4/final)  | D      |

## ポスト量子暗号標準

PQC 実装は、堅牢化されたコードや実装リファレンスはまだ最低限しか存在しないため、[FIPS-203](https://csrc.nist.gov/pubs/fips/203/ipd)/[204](https://csrc.nist.gov/pubs/fips/204/ipd)/[205](https://csrc.nist.gov/pubs/fips/205/ipd) に準拠していなければなりません。 https://www.nist.gov/news-events/news/2024/08/nist-releases-first-3-finalized-post-quantum-encryption-standards

提案されているハイブリッド TLS 鍵交換グループは、[draft-tls-westerbaan-xyber768x00-03](https://www.ietf.org/archive/id/draft-tls-westerbaan-xyber768d00-03.txt) で規定され、[Firefox リリース 132](https://www.ietf.org/archive/id/draft-tls-westerbaan-xyber768d00-03.txt) や [Chrome リリース 131](https://security.googleblog.com/2024/09/a-new-path-for-kyber-on-web.html) などの主要なブラウザでサポートされており、暗号テスト環境や業界や政府が承認したライブラリ内で利用可能な場合に使用できます (MAY)。
