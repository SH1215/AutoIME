# AutoIME — Public Alpha

**日本語入力の「IME切替ミス」を、入力後でも自動で救済する実験的ツールです。**

日本語を書くつもりなのに英数モードのまま `nihongo` と打ってしまい、消して、IMEを切り替えて、もう一度打ち直す。  
AutoIMEは、この小さな反復を減らすことを目指します。

> 人間が常に正しいIMEモードを意識するのではなく、入力内容から「日本語ローマ字らしい」と判断できる場合に、PC側が切替ミスを救済する。

## なぜ作るのか

IME切替ミスは1回なら数秒です。しかし毎日・長期間・多数の日本語ユーザーで積み重なると、無視できない時間ロスになる可能性があります。

現時点で社会全体の損失時間を示す十分な実測データはありません。そこでAutoIMEでは、便利さだけでなく、将来は**救済回数や再入力削減量をローカルで測定し、この仮説を検証すること**も目標にします。

詳しくは [IME切替ミスの時間ロスをどう検証するか](docs/IMPACT_MEASUREMENT.md) を参照してください。

## 現在の対応

### Windows 11
- AutoHotkey v2 + Microsoft IME
- 英数モード（A）のまま `nihongo` と入力してSpace
- 日本語入力へ救済して変換
- `Ctrl + Alt + J` でAutoIME ON/OFF

### Ubuntu 20.04
- X11 + IBus + Mozc 2.23 で動作確認
- AT-SPIで「実際に文書へ入力されたテキスト」を監視
- Spaceキー、Ctrl、Shift、矢印キーをシステム全体でgrabしない
- 上部バーからAutoIMEのON/OFF状態を確認・切替可能

## 重要：まだPublic Alphaです

現時点では、**個人環境で動作確認した研究・実験段階のコード**です。一般利用向けの完成品ではありません。

特に次を広く検証する必要があります。

- Chrome / Firefox / Edge
- Microsoft Word / LibreOffice
- Gmail / Webメール
- ChatGPTなどのWeb入力欄
- 日本語と英語が混在する文章
- パスワード・認証入力欄
- 誤判定する英単語
- 長時間常駐時の安定性

重要な文章、認証情報、業務システムでは、十分な検証が済むまで使用しないでください。

## Windows版の試し方

1. [AutoHotkey v2](https://www.autohotkey.com/) をインストール
2. `windows/AutoIME_Windows_v0.5_ALPHA.ahk` を実行
3. Microsoft IMEを英数モード（A）にする
4. `nihongo` と入力してSpace

停止・再開は `Ctrl + Alt + J` です。

詳細は [windows/README.md](windows/README.md) を参照してください。

## Ubuntu版の試し方

動作確認環境は Ubuntu 20.04 / X11 / IBus / Mozc 2.23 です。

```bash
cd ubuntu
chmod +x install.sh uninstall.sh
./install.sh
```

必要なパッケージのインストール後、AutoIME本体と状態表示を起動し、次回ログイン時の自動起動も登録します。

停止・自動起動解除:

```bash
./uninstall.sh
```

詳細は [ubuntu/README.md](ubuntu/README.md) を参照してください。

## プライバシーと安全性

AutoIMEはキーボード入力を扱うため、安全性を最優先にします。

- 入力内容を外部サーバーへ送信しないことを基本方針とします
- 誤変換が危険なアプリでは動作対象から外します
- 削除・再入力の前に対象文字列を確認します
- Ubuntu版ではAT-SPI上でパスワード欄と判定できる入力欄を除外します
- 自動処理は簡単にOFFにできるようにします
- 将来、統計送信を追加する場合も明示的なオプトイン方式にします

詳細は [PRIVACY_AND_SAFETY.md](PRIVACY_AND_SAFETY.md) を参照してください。

## Alphaテスターへ

誤判定、未反応、キー操作への干渉などを見つけた場合はIssueで報告してください。

**パスワード、住所、氏名、メール本文などの個人情報をIssueへ投稿しないでください。**

テスト項目は [docs/TEST_PLAN.md](docs/TEST_PLAN.md) にあります。

## 今後の目標

1. 少人数のAlphaテスターで検証
2. 誤判定例を集める
3. 対応アプリを増やす
4. パスワード欄などでの安全性を強化
5. ローカルの「救済回数・推定節約時間」カウンターを実装
6. 十分に安定した段階で v1.0

## License

MIT License
