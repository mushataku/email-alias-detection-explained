# メールアドレスは無限に作れる。それでも検知は突破できない

独自ドメイン + catch-all（accept-all）でメールアドレスを無限に作る手口が、
なぜ検知側から見抜けるのかを、SMTPのプロトコル仕様と「最小律」という一般原理でつないだ解説記事です。

**📖 https://mushataku.github.io/email-alias-detection-explained/**

*A Japanese-language essay on why unlimited catch-all email aliases don't defeat detection — and what that implies about defending against them.*

## 中身

1. catch-all はアドレス数に関係なく年1,600円で手に入る
2. だが `RCPT TO` への応答だけで accept-all だと一発で見抜かれる
3. **プログラムで回避する案が、SMTPの応答順序（`RCPT TO` は `DATA` より前）で原理的に破綻する仕組み**
4. メール軸を完璧に直しても、ブラウザ指紋は1のまま変わらない実測（14件/50件・23軸）
5. リービッヒの最小律 —— 対策は一番弱い軸にしか効かない、という一般原理
6. 相手が複数の検知器だったとき、実装コストが軸の重みを変える話

## 出典と検証

技術的な主張は公開ドキュメントの範囲で誰でも確認できます。

- [Cloudflare Email Service — Limits](https://developers.cloudflare.com/email-service/platform/limits/)（ルーティングルール200/ドメイン・検証済み宛先200/アカウント）
- [Cloudflare Email Routing — Runtime API](https://developers.cloudflare.com/email-routing/email-workers/runtime-api/)（`setReject()` / `rawSize`）
- [The Ultimate Guide to Accept-all (Catch-all) Email Addresses](https://hunter.io/blog/ultimate-guide-accept-all-catch-all)
- [Liebig's law of the minimum — Wikipedia](https://en.wikipedia.org/wiki/Liebig%27s_law_of_the_minimum)

本文中の実験結果（プラスアドレス14件・一意な架空アドレス50件・23軸の指紋分類）は、
筆者が独自に構築した検証環境（自作の応募フォーム＋自作の検知器）での実測値です。
実データは非公開ですが、手法（SMTPプロービング・ブラウザ指紋の23軸分類）は公開情報の範囲で再現できます。

## 立ち位置

この記事は**検知する側**（フォームを作る人・不正対策を実装する人）の視点で書いています。
「こうすれば重複検知を回避できる」という手順のレシピではなく、「なぜ見抜けるのか」「どこを見れば安く見抜けるのか」を説明する内容です。

## ライセンス

MIT（[LICENSE](LICENSE)）。
