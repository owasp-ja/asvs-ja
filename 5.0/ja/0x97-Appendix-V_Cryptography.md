# 付録 V: 暗号化

「暗号化」の章は単にベストプラクティスを定義するだけではありません。暗号の原則に対する理解を深め、より耐性のある最新のセキュリティ手法の採用を促進することを目的としています。この付録では、「暗号化」の章で概説されている包括的な標準を保管するために、各要件に関する詳細な技術情報を提供します。

## アルゴリズム (V11.2)

### 暗号パラメータの等価な強度

さまざまな暗号システムの相対的なセキュリティ強度は以下の表のとおりです ([NIST SP 800-57 Part 1](https://csrc.nist.gov/pubs/sp/800/57/pt1/r5/final), p.71 より):

| セキュリティ強度 | 対称鍵アルゴリズム | 有限体暗号 (DSA, DH, MQV) | 整数因数分解暗号 (RSA) | 楕円曲線暗号 (ECDSA, EdDSA, DH, MQV) |
|--|--|--|--|--|
| <= 80 | 2TDEA | L = 1024 <br> N = 160 | k = 1024 | f = 160-223 |
| 112 | 3TDEA   | L = 2048 <br> N = 224 | k = 2048 | f = 224-255 |
| 128 | AES-128 | L = 3072 <br> N = 256 | k = 3072 | f = 256-383 |
| 192 | AES-192 | L = 7680 <br> N = 384 | k = 7680 | f = 384-511 |
| 256 | AES-256 | L = 15360 <br> N = 512 | k = 15360 | f = 512+ |

注: このセクションでは量子コンピュータが存在しないことを前提としています。そのようなコンピュータが存在する場合、最後の 3 列の推定値はもはや有効ではなくなります。

## 乱数値 (V11.5)

### 承認された RNG 方式とアルゴリズム

| 名称 | バージョン/リファレンス | 備考 | L1 | L2 | L3 |
|:-:|:-:|:-:|:-:|:-:|:-:|
| `/dev/random` | Linux 4.8 以降 [(2016 年 10 月)](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=818e607b57c94ade9824dad63a96c2ea6b21baf3)、iOS、Android、その他の Linux ベースの POSIX オペレーティングシステムにもあります。[RFC7539](https://datatracker.ietf.org/doc/html/rfc7539) に基づいています。 | ChaCha20 ストリームを利用します。iOS [`SecRandomCopyBytes`](https://developer.apple.com/documentation/security/secrandomcopybytes(_:_:_:)?language=objc) および Android [`Secure Random`](https://developer.android.com/reference/java/security/SecureRandom) にあり、それぞれに正しい設定が提供されています。 | ✓ | ✓ | ✓ |
| `/dev/urandom` | ランダムデータを提供するための Linux カーネルの特殊ファイルです。 | ハードウェアのランダム性から高性能のエントロピーソースを提供します。 | ✓ | ✓ | ✓ |
| `AES-CTR-DRBG` | [NIST SP800-90A](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-90Ar1.pdf) | [`BCRYPT_RNG_ALGORITHM`](https://learn.microsoft.com/en-us/windows/win32/seccng/cng-algorithm-identifiers) で設定された [Windows CNG API `BCryptGenRandom`](https://learn.microsoft.com/en-us/windows/win32/api/bcrypt/nf-bcrypt-bcryptgenrandom) などの一般的な実装で使用されます。 | ✓ | ✓ | ✓ |
| `HMAC-DRBG` | [NIST SP800-90A](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-90Ar1.pdf) | | ✓ | ✓ | ✓ |
| `Hash-DRBG` | [NIST SP800-90A](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-90Ar1.pdf) | | ✓ | ✓ | ✓ |
| `getentropy()` | [OpenBSD](https://man.openbsd.org/getentropy.2)、[Linux glibc 2.25 以降](https://man7.org/linux/man-pages/man3/getentropy.3.html)、[macOS 10.12 以降](https://support.apple.com/en-gb/guide/security/seca0c73a75b/web) で利用可能です。 | シンプルで最小限の API で、カーネルのエントロピーソースから安全なランダムバイトを直接提供します。より現代的であり、古い API に関連する落とし穴を回避します。 | ✓ | ✓ | ✓ |

### RBG で許可されていないハッシュ

以下は RBG に使用すべきではありません (SHOULD NOT) ([NIST SP-800-57 Part 1](https://csrc.nist.gov/pubs/sp/800/57/pt1/r5/final) による):

| ハッシュ関数 | リファレンス |
|--|--|
| SHA3-224 | [FIPS 202](https://csrc.nist.gov/pubs/fips/202/final) |
| SHA-512/224 | [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) |
| SHA-224 | [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) |
| KMAC128 | [NIST SP 800-185](https://csrc.nist.gov/pubs/sp/800/185/final) |

## 暗号アルゴリズム (V11.3)

### 承認された暗号

以下の暗号が承認されています:

| 対称鍵アルゴリズム | リファレンス | L1 | L2 | L3 |
|--|--|--|--|--|
| AES-256 | [FIPS 197](https://csrc.nist.gov/pubs/fips/197/final) | | ✓ | ✓ |
| Salsa20 | [Salsa 20 specification](https://cr.yp.to/snuffle/spec.pdf) | | ✓ | ✓ |
| XChaCha20 | [XChaCha20 Draft](https://datatracker.ietf.org/doc/html/draft-irtf-cfrg-xchacha-03) | ✓ | ✓ | ✓ |
| XSalsa20 | [Extending the Salsa20 nonce](https://cr.yp.to/snuffle/xsalsa-20110204.pdf) | ✓ | ✓ | ✓ |
| ChaCha20 | [RFC 8439](https://www.rfc-editor.org/info/rfc8439) | ✓ | ✓ | ✓ |
| AES-192 | [FIPS 197](https://csrc.nist.gov/pubs/fips/197/final) | ✓ | ✓ | ✓ |
| AES-128 | [FIPS 197](https://csrc.nist.gov/pubs/fips/197/final) | ✓ | ✓ | ✓ |

### 許可されていない暗号

以下は、_明示的に許可されていない_ 暗号のリストです。上記のリストにないものは、文書化された理由とともに承認されなければなりませんが、以下は明示的に禁止されており、使用してはなりません (MUST NOT):

| 対称鍵アルゴリズム |
|--|
| 2TDEA |
| TDEA (3DES/3DEA) |
| IDEA |
| RC4 |
| Blowfish|
| ARC4 |
| DES |

### AES 暗号モード

現代の暗号はさまざまな目的でさまざまなモード、特に AES を使用しています。ここでは AES 暗号モードの要件について説明します。

#### 一般的なユースケースで承認されている暗号モード

以下のモードは、その機能が暗号化されたデータ保存 (次のサブセクションを参照) である場合を除いて、承認されています:

| AES 暗号モード | 認証済み | リファレンス | L1 | L2 | L3 |
|--|--|--|--|--|--|
| GCM | Yes | [NIST SP 800-38D](https://csrc.nist.gov/pubs/sp/800/38/d/final) | ✓ | ✓ | ✓ |
| CCM | Yes | [NIST SP 800-38C](https://csrc.nist.gov/pubs/sp/800/38/c/upd1/final) | ✓ | ✓ | ✓ |
| CBC* | No | [NIST SP 800-38A](https://csrc.nist.gov/pubs/sp/800/38/a/final) | ✓ | ✓ | ✓ |

\* すべての暗号化されたメッセージは認証されなければなりません。このため、CBC モードを使用する場合は必ず、メッセージを検証するためにハッシュ関数または MAC が関連付けられていなければなりません (MUST)。一般的に、これは Encrypt-Then-Hash 方式で適用されなければなりません (MUST) (ただし、TLS 1.2 では代わりに Hash-Then-Encrypt を使用します)。これが保証できない場合、CBC を使用してはいけません (MUST NOT)。

#### 一般的なユースケースで承認されている推奨暗号モード

指定の承認されたブロックモードのうち、実装ではこのリストの暗号を優先順に使用すべきです (SHOULD):

| AES 暗号モード | リファレンス | L1 | L2 | L3 |
|--|--|--|--|--|
| GCM | [NIST SP 800-38D](https://csrc.nist.gov/pubs/sp/800/38/d/final) | ✓ | ✓ | ✓ |
| CCM | [NIST SP 800-38C](https://csrc.nist.gov/pubs/sp/800/38/c/upd1/final) | ✓ | ✓ | ✓ |

#### データ保存 (ディスク上のブロック暗号) でのみ承認されている暗号モード

以下のディスクレベルブロック暗号モードが承認されており、優先順にリストされています:

| AES ディスク暗号モード | リファレンス | L1 | L2 | L3 |
|--|--|--|--|--|
| XTS | [NIST SP 800-38E](https://csrc.nist.gov/pubs/sp/800/38/e/final) | ✓ | ✓ | ✓ |
| XEX | [Rogaway 2004](https://doi.org/10.1007/978-3-540-30539-2_2) | ✓ | ✓ | ✓ |
| LRW | [Liskov, Rivest, and Wagner 2005](https://doi.org/10.1007/s00145-010-9073-y) | ✓ | ✓ | ✓ |

#### 許可されていない暗号モード

以下の暗号モードはいかなるユースケースでも使用してはいけません (MUST NOT):

| 暗号モード |
|--|
| ECB |
| CFB |
| OFB |
| CTR |
| CCM-8** |

\** CCM-8 を使用する場合、MAC タグは 64 ビットのセキュリティしか持ちません。
これは、少なくとも 128 ビットのセキュリティを必要とする要件 6.2.9 に準拠していません。

### 鍵ラッピング

暗号鍵ラップ (および対応する鍵アンラップ) は、追加の暗号メカニズムを使用して既存の鍵をカプセル化 (つまりラップ) し、転送中などに元の鍵が明らかに露出しないようにすることで、既存の鍵を保護する方法です。元の鍵を保護するために使用されるこの追加の鍵はラップ鍵と呼ばれます。

この操作は、信頼できないとみなされる場所で鍵を保護したい場合、あるいは信頼できないネットワーク上やアプリケーション内で機密鍵を送信したい場合に実行できます。
ただし、ラップ/アンラップ手順を実行する前に、元の鍵の性質 (アイデンティティや目的など) を理解することを真剣に検討すべきです。これは、セキュリティと、特に鍵の機能 (署名など) の監査証跡や適切な鍵の保存を含むコンプライアンスの点で、ソースとターゲットの両方のシステム/アプリケーションに影響を及ぼす可能性があります。

特に、鍵ラッピングには、[NIST SP 800-38F](https://csrc.nist.gov/pubs/sp/800/38/f/final) に従い、量子脅威に対する将来を見据えた対策を考慮して、AES-256 を使用しなければなりません (MUST)。AES を使用する暗号モードは優先順に以下のとおりです:

| 鍵ラッピング | リファレンス | L1 | L2 | L3 |
|--|--|--|--|--|
| KW | [NIST SP 800-38F](https://csrc.nist.gov/pubs/sp/800/38/f/final) | ✓ | ✓ | ✓ |
| KWP | [NIST SP 800-38F](https://csrc.nist.gov/pubs/sp/800/38/f/final) | ✓ | ✓ | ✓ |

AES-192 と AES-128 はユースケースで必要な場合に使用できます (MAY) が、その理由はエンティティの暗号インベントリに文書化されなければなりません (MUST)。

### 認証された暗号

ディスク暗号化を除いて、暗号化されたデータは何かしらの形で認証された暗号 (AE) スキーム、通常は関連データ付き認証暗号 (AEAD) スキームを使用して、認可されていない変更から保護されなければなりません。

アプリケーションは承認済みの AEAD スキームを使用することをお勧めします。代わりに、承認済みの暗号スキームと承認済みの MAC アルゴリズムを Encrypt-then-MAC 構造で組み合わせることもできます。

MAC-then-encrypt はレガシーアプリケーションとの互換性のために依然として許可されています。これは古い暗号スイートを使用する TLS v1.2 で使用されます。

| AEAD メカニズム | リファレンス | ステータス |
|-----------------|--------------|------------|
| AES-GCM | [SP 800-38D](https://csrc.nist.gov/pubs/sp/800/38/d/final) | 承認済み |
| AES-CCM | [SP 800-38C](https://csrc.nist.gov/pubs/sp/800/38/c/upd1/final) | 承認済み |
| ChaCha-Poly1305 | [RFC 7539](https://datatracker.ietf.org/doc/html/rfc7539) | 承認済み |
| Encrypt-then-MAC | | 承認済み |
| MAC-then-encrypt | | レガシー |

## ハッシュ関数 (V11.4)

### 一般的なユースケースで承認されているハッシュ関数

以下のハッシュ関数は、デジタル署名、HMAC、鍵導出関数 (KDF)、ランダムビット生成 (RBG) などの一般的な暗号ユースケースでの使用が承認されています。これらの関数は強力な衝突耐性を提供し、高セキュリティアプリケーションに適しています。これらのアルゴリズムの一部は、適切な暗号鍵管理で使用すると攻撃に対する強力な耐性を提供するため、HMAC、KDF、RBG 機能でも承認されています。

| ハッシュ関数 | HMAC/KDF/RBG に適しているか？ | リファレンス | L1 | L2 | L3 |
| -------------- | ----------------------------- |-------------------------------------------------------------- |--|--|--|
| SHA3-512 | Y |[FIPS 202](https://csrc.nist.gov/pubs/fips/202/final) | | ✓ | ✓ |
| SHA-512 | Y |[FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | | ✓ | ✓ |
| SHA3-384 | Y |[FIPS 202](https://csrc.nist.gov/pubs/fips/202/final) | | ✓ | ✓ |
| SHA-384 | Y |[FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | | ✓ | ✓ |
| SHA3-256 | Y |[FIPS 202](https://csrc.nist.gov/pubs/fips/202/final) | | ✓ | ✓ |
| SHA-512/256 | Y |[FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | | ✓ | ✓ |
| SHA-256 | Y |[FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | ✓ | ✓ | ✓ |
| KMAC256 | N |[NIST SP 800-185](https://csrc.nist.gov/pubs/sp/800/185/final) | ✓ | ✓ | ✓ |
| KMAC128 | N |[NIST SP 800-185](https://csrc.nist.gov/pubs/sp/800/185/final) | ✓ | ✓ | ✓ |
| SHAKE256 | Y |[FIPS 202](https://csrc.nist.gov/pubs/fips/202/final) | ✓ | ✓ | ✓ |
| BLAKE2s | Y | [BLAKE2: simpler, smaller, fast as MD5](https://eprint.iacr.org/2013/322) | ✓ | ✓ | ✓ |
| BLAKE2b | Y | [BLAKE2: simpler, smaller, fast as MD5](https://eprint.iacr.org/2013/322) | ✓ | ✓ | ✓ |
| BLAKE3 | Y | [BLAKE3 one function, fast everywhere](https://github.com/BLAKE3-team/BLAKE3-specs/raw/master/blake3.pdf) | ✓ | ✓ | ✓ |

### パスワード保存のために承認されているハッシュ関数

以下のハッシュ関数は安全なパスワード保存に特に推奨されています。これらの低速ハッシュアルゴリズムは、パスワードクラッキングの計算難易度を上げることで、ブルートフォース攻撃や辞書攻撃を軽減します。

| ハッシュ関数 | リファレンス | 必要なパラメータセット | L1 | L2 | L3 |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------- | --|--|--|
| argon2 | RFC 9106 | Argon2ID: Memory Cost 19MB, Time Cost 2, Parallelism 1 | | ✓ | ✓ |
| scrypt | RFC 7914 | 2^15 r = 8 p = 1 | | ✓ | ✓ |
| bcrypt |[A Future-Adaptable Password Scheme](https://www.usenix.org/legacy/events/usenix99/provos/provos.pdf) | At least 10 rounds. | | ✓ | ✓ |
| PBKDF2_SHA512 | [NIST SP 800-132](https://csrc.nist.gov/pubs/sp/800/132/final), [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | 210,000 iterations | ✓ | ✓ | ✓ |
| PBKDF2_SHA256 | [NIST SP 800-132](https://csrc.nist.gov/pubs/sp/800/132/final), [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) | 600,000 iterations | ✓ | ✓ | ✓ |

### 許可されていないハッシュ関数

以下のハッシュ関数は、既知の弱点や脆弱性のため、新しいマテリアルを生成する暗号操作に使用してはいけません (MUST NOT)。既存のマテリアルの検証にのみ使用できます (MAY)。

| ハッシュ関数 | リファレンス |
| ---------------- | --------------------------------------------------------------------------------------------------------- |
| CRC (any length) | -- |
| MD4 | [RFC 1320](https://www.rfc-editor.org/info/rfc1320) |
| MD5 | [RFC 1321](https://www.rfc-editor.org/info/rfc1321) |
| SHA-1 | [RFC 3174](https://www.rfc-editor.org/info/rfc3174) & [RFC 6194](https://www.rfc-editor.org/info/rfc6194) |

### デジタル署名のために許可されていないハッシュ関数

衝突耐性が不十分なため、以下のハッシュ関数はデジタル署名や、衝突耐性を必要とするその他のアプリケーションには使用してはいけません (MUST NOT)。他の用途では、レガシーシステムとの互換性と検証にのみ使用できますが、新しい設計には使用してはいけません。

| ハッシュ関数 | リファレンス |
| -------------- | -------------------------------------------------------------- |
| SHA-224 | [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) |
| SHA-512/224 | [FIPS 180-4](https://csrc.nist.gov/pubs/fips/180-4/upd1/final) |
| SHA3-224 | [FIPS 202](https://csrc.nist.gov/pubs/fips/202/final) |

## 鍵交換メカニズム (V11.6)

### 承認されている KEX スキーム

すべての鍵交換スキームに対して 128 ビット以上のセキュリティ強度が確保されていなければならず (MUST)、その実装は以下の表のパラメータ選択に従わなければなりません (MUST)。

| スキーム | ドメインパラメータ |
|--|--|
| RSA | k >= 3072 |
| Diffie-Hellman (DH) | L >= 3072 & N >= 256 |
| Elliptic Curve Diffie-Hellman (ECDH) | f >= 256-383 |

ここでのパラメータは以下のとおりです:

* k は RSA 鍵の鍵サイズです。
* L は有限体暗号の公開鍵 (public key) のサイズであり、N は秘密鍵 (private key) のサイズです。
* f は ECC の鍵サイズの範囲です。

### 承認されている DH グループ

以下のグループが承認されており、Diffie-Hellman KEX の実装に使用しなければなりません (MUST)。IKEv2 グループはリファレンスとして提供されています ([NIST SP 800-77](https://csrc.nist.gov/pubs/sp/800/77/r1/final))。同等のグループが他のプロトコルで使用されるかもしれません。このリストは最も強力なものから最も弱いものの順に並べられています。セキュリティ強度は [NIST SP 800-56A](https://csrc.nist.gov/pubs/sp/800/56/a/r3/final), Appendix D および [NIST SP 800-57 Part 1 Rev.5](https://csrc.nist.gov/pubs/sp/800/57/pt1/r5/final) に記載されています。

| グループ | スキーム | パラメータ | セキュリティビット | L1 | L2 | L3 |
|--|--|--|--|--|--|--|
| 21 | ECC | 521-bit random ECP group | 260 | | | ✓ |
| 32 | ECC | Curve448 | 224 | | | ✓ |
| 18 | MODP | 8192-bit MODP Group | 192 < 200 | | ✓ | ✓ |
| 20 | ECC | 384-bit random ECP group | 192 | | ✓ | ✓ |
| 17 | MODP | 6144-bit MODP Group | 128 < 176 | ✓ | ✓ | ✓ |
| 16 | MODP | 4096-bit MODP Group | 128 < 152 | ✓ | ✓ | ✓ |
| 31 | ECC | Curve25519 | 128 | ✓ | ✓ | ✓ |
| 19 | ECC | 256-bit random ECP group | 128 | ✓ | ✓ | ✓ |
| 15 | MODP | 3072-bit MODP Group | 128 | ✓ | ✓ | ✓ |
| 14 | MODP | 2048-bit MODP Group | 112 | ✓ | ✓ | ✓ |

### 許可されていない KEX スキーム

新しい実装では [NIST SP 800-56A](https://csrc.nist.gov/pubs/sp/800/56/a/r3/final) & [B](https://csrc.nist.gov/pubs/sp/800/56/b/r2/final) および [NIST SP 800-77](https://csrc.nist.gov/pubs/sp/800/77/r1/final) に準拠していないスキームを使用してはいけません (MUST NOT)。

特に IKEv1 は本番環境で使用してはいけません (MUST NOT)。

## メッセージ認証コード (MAC)

メッセージ認証コード (MAC) は、メッセージの完全性と真正性を検証するために使用される暗号構造です。MAC は、メッセージと共有鍵 (secret key) を入力として受け取り、固定サイズのタグ (MAC 値) を生成します。MAC は安全な通信プロトコル (TLS/SSL など) で広く使用され、パーティ間で交換されるメッセージが本物で無傷であることを確保します。

### 承認されている MAC アルゴリズム

以下の MAC アルゴリズムは、完全性と真正性の保証を提供することで、メッセージ保護に使用することが承認されています。実装では、メッセージのセキュリティを確保するために、認証された暗号モードまたは別途適用された HMAC のみを使用しなければなりません (MUST)。

| MAC アルゴリズム  | リファレンス                                                                              | 一般的な使用に適しているか？ | L1 | L2 | L3 |
| ----------------- | ----------------------------------------------------------------------------------------- | ------------------------- |----|----|----|
| HMAC-SHA-256      | [RFC 2104](https://www.rfc-editor.org/info/rfc2104) & [FIPS 198-1](https://csrc.nist.gov/pubs/fips/198-1/final) | ✓                       | ✓  | ✓  | ✓  |
| HMAC-SHA-384      | [RFC 2104](https://www.rfc-editor.org/info/rfc2104) & [FIPS 198-1](https://csrc.nist.gov/pubs/fips/198-1/final) | ✓                       |    | ✓  | ✓  |
| HMAC-SHA-512      | [RFC 2104](https://www.rfc-editor.org/info/rfc2104) & [FIPS 198-1](https://csrc.nist.gov/pubs/fips/198-1/final) | ✓                       |    | ✓  | ✓  |
| KMAC128           | [NIST SP 800-185](https://csrc.nist.gov/pubs/sp/800/185/final)                             | ✓                       | ✓  | ✓  | ✓  |
| KMAC256           | [NIST SP 800-185](https://csrc.nist.gov/pubs/sp/800/185/final)                             | ✓                       | ✓  | ✓  | ✓  |
| BLAKE3            |  [BLAKE3 one function, fast everywhere](https://github.com/BLAKE3-team/BLAKE3-specs/raw/master/blake3.pdf)  | ✓                       | ✓  | ✓  | ✓  |
| AES-CMAC          | [RFC 4493](https://datatracker.ietf.org/doc/html/rfc4493) & [NIST SP 800-38B](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-38b.pdf) | ✓ | ✓  | ✓  | ✓  |
| AES-GMAC          | [NIST SP 800-38D](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-38d.pdf)            | ✓ | ✓  | ✓  | ✓  |
| Poly1305-AES      | [The Poly1305-AES message-authentication code](https://cr.yp.to/mac/poly1305-20050329.pdf)                  | ✓ | ✓  | ✓  | ✓  |

### 許可されていない MAC アルゴリズム

以下のアルゴリズムは、既知の脆弱性や不十分なセキュリティ強度のため、明示的に禁止されており、使用してはいけません (MUST NOT):

| MAC アルゴリズム | リファレンス                                                                       |
| ---------------- | ---------------------------------------------------------------------------------- |
| HMAC-MD5         | [RFC 1321](https://www.rfc-editor.org/info/rfc1321)                                |

## デジタル署名

### 承認されているデジタル署名アルゴリズム

以下のデジタル署名アルゴリズムは、データの真正性と完全性を確保するために使用することが承認されています。署名スキームは [NIST SP 800-57 Part 1](https://csrc.nist.gov/pubs/sp/800/57/pt1/r5/final) に従って承認された鍵サイズとパラメータを使用しなければなりません (MUST):

| 署名アルゴリズム               | リファレンス                                               | 一般的な使用に適しているか？ | L1 | L2 | L3 |
| ------------------------------ | ---------------------------------------------------------- | ------------------------- |----|----|----|
| EdDSA (Ed25519, Ed448)         | [RFC 8032](https://www.rfc-editor.org/info/rfc8032)        | ✓                         | ✓  | ✓  | ✓  |
| XEdDSA (Curve25519, Curve448)  | [XEdDSA](https://signal.org/docs/specifications/xeddsa/)   | ✓                         | ✓  | ✓  | ✓  |
| ECDSA (P-256, P-384, P-521)    | [FIPS 186-4](https://csrc.nist.gov/pubs/fips/186-5/final)  | ✓                         | ✓  | ✓  | ✓  |
| RSA-RSSA-PSS                   | [RFC 8017](https://www.rfc-editor.org/info/rfc8017)        | ✓                         | ✓  | ✓  | ✓  |

### 許可されていないデジタル署名アルゴリズム

以下のデジタル署名アルゴリズムは、既知の弱点や不十分なセキュリティ強度のため、使用してはいけません (MUST NOT):

| 署名アルゴリズム    | リファレンス                                                                       |
| ------------------- | ---------------------------------------------------------------------------------- |
| RSA-SSA-PKCS#1 v1.5 | [RFC 8017](https://www.rfc-editor.org/info/rfc8017)                                |
| DSA (any key size)  | [FIPS 186-4](https://csrc.nist.gov/pubs/fips/186-4/final)                          |

## 鍵導出関数 (KDF)

### 承認されている KDF

鍵導出関数は鍵マテリアルを特定の暗号操作に適した鍵に変換します。以下の KDF が承認されており、アプリケーションのニーズとセキュリティコンテキストに基づいて使用されなければなりません (MUST):

| KDF         | リファレンス                                                                                  | 一般的な使用に適しているか？ | L1 | L2 | L3 |
| ----------- | --------------------------------------------------------------------------------------------- | ------------------------- |----|----|----|
| argon2id    | [RFC 9106](https://www.rfc-editor.org/info/rfc9106)                                          | ✓                       |    | ✓  | ✓  |
| scrypt      | [RFC 7914](https://www.rfc-editor.org/info/rfc7914)                                          | ✓                       |    | ✓  | ✓  |
| PBKDF2      | [RFC 8018](https://www.rfc-editor.org/info/rfc8018) & [NIST SP 800-132](https://csrc.nist.gov/pubs/sp/800/132/final) | ✓                       | ✓  | ✓  | ✓  |
| HKDF        | [RFC 5869](https://www.rfc-editor.org/info/rfc5869)                                          | ✓                       | ✓  | ✓  | ✓  |

### 許可されていない KDF

以下の KDF は、不十分なセキュリティプロパティや既知の弱点のため、明示的に禁止されており、使用してはいけません (MUST NOT):

| KDF            | リファレンス                                                                       |
| -------------- | ---------------------------------------------------------------------------------- |
| MD5-based KDFs | [RFC 1321](https://www.rfc-editor.org/info/rfc1321)                                |
| SHA-1-based KDFs | [RFC 3174](https://www.rfc-editor.org/info/rfc3174) & [RFC 6194](https://www.rfc-editor.org/info/rfc6194) |

### ポスト量子暗号標準

PQC 実装は、堅牢化されたコードや実装リファレンスはまだ最低限しか存在しないため、[FIPS-203](https://csrc.nist.gov/pubs/fips/203/ipd)/[204](https://csrc.nist.gov/pubs/fips/204/ipd)/[205](https://csrc.nist.gov/pubs/fips/205/ipd) に準拠していなければなりません。 https://www.nist.gov/news-events/news/2024/08/nist-releases-first-3-finalized-post-quantum-encryption-standards

提案されているハイブリッド TLS 鍵交換グループは、[draft-tls-westerbaan-xyber768x00-03](https://www.ietf.org/archive/id/draft-tls-westerbaan-xyber768d00-03.txt) で規定され、[Firefox リリース 132](https://www.ietf.org/archive/id/draft-tls-westerbaan-xyber768d00-03.txt) や [Chrome リリース 131](https://security.googleblog.com/2024/09/a-new-path-for-kyber-on-web.html) などの主要なブラウザでサポートされており、暗号テスト環境や業界や政府が承認したライブラリ内で利用可能な場合に使用できます (MAY)。
