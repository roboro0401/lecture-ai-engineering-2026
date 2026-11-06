# 第2回 解答

## 演習

- 使ったモデル・設定（`result.json` の `config` から）：
CONFIG = {
    "base_model": "Qwen/Qwen3.5-0.8B",   # 小さな指示チューニング済みモデル（Apache-2.0）。量子化なしで T4 に載る大きさ
    "revision": "2fc06364715b967f1860aea9cf38778875588b17",  # いつ実行しても同じ重みを使うために固定
    "seed": 42,
    "num_train_epochs": 20,  # 学習データ（14 件）を何周学習するか。1 周 2 ステップなので合計 40 ステップ（T4 で 4〜5 分）
    "warmup_steps": 6,
    "lr": 2e-4,
    "lora_r": 16,            # LoRA の行列の次元（小さいほど学習するパラメータが少ない）
    "lora_alpha": 32,
    "max_seq_len": 512,
    "max_new_tokens": 96,
    "gen": {"do_sample": False},   # 評価は貪欲生成で固定（前後比較の条件を揃える）
    "work_dir": "/content/qlora_lab",                          # 結果ファイルの保存先（左のファイル一覧から見られる）
    "drive_dir": "/content/drive/MyDrive/aie2026/qlora_lab",  # USE_DRIVE=True のときの保存先
}
- 自分で学習を実行したか（`executed_training_myself`）：はい
- 学習前：`format_rate` =　 / `content_rate` =　0.095238　/  0.000000
- 学習後：`format_rate` =　 / `content_rate` =　1.000000　/　0.857143 
- 退行した質問：なし
- 学習後に残った失敗と、その型（根拠なしの質問に断定した / 根拠ありの質問に確認要求で逃げた / 形式の崩れ）：根拠なしの質問に断定した
- 知識を問う質問（セクション 6）の結果（学習前の分類 → 学習後の分類）と気づいたこと：学習前の分類は全て「format_error」、学習後の分類は「asserted、needs_review」であった。学習後もFAQ に無い（架空）は課題の提出期限を最終課題の提出期限と捉えて断定していた。
- 学習で変わったこと・変わらなかったこと：学習により形式は正しく出力されるように変わった。FAQに無い（架空）については、学習前後で断定することに変わりはなかった。

## 宿題

- 取り組んだレベル：基礎
- 変えた設定やデータ：'num_train_epochs'を20から40に変更
- 結果（比較表や数字）：変更前　result.json
{
  "config": {
    "base_model": "Qwen/Qwen3.5-0.8B",
    "revision": "2fc06364715b967f1860aea9cf38778875588b17",
    "seed": 42,
    "num_train_epochs": 20,
    "warmup_steps": 6,
    "lr": 0.0002,
    "lora_r": 16,
    "lora_alpha": 32,
    "max_seq_len": 512,
    "max_new_tokens": 96,
    "gen": {
      "do_sample": false
    }
  },
  "versions": {
    "transformers": "5.18.0",
    "peft": "0.21.1",
    "trl": "1.14.2",
    "torch": "2.11.0+cu130"
  },
  "method": "LoRA（量子化なし）",
  "before": {
    "label": "before",
    "n": 21,
    "format_rate": 0.09523809523809523,
    "content_rate": 0.0,
    "both_rate": 0.0,
    "seconds": 25.1
  },
  "after": {
    "label": "after",
    "n": 21,
    "format_rate": 1.0,
    "content_rate": 0.9047619047619048,
    "both_rate": 0.9047619047619048,
    "seconds": 43.9
  },
  "regressed": [],
  "knowledge": [
    {
      "質問": "この講座の最終課題の締切はいつですか",
      "FAQ にある": false,
      "学習前の分類": "format_error",
      "学習前の出力": "{\"answer\": \"9 月 30 日 23:59\"}",
      "学習後の分類": "asserted",
      "学習後の出力": "{\"answer\": \"課題の提出期限は 9 月 30 日 23:59（日本時間）です。\", \"needs_review\": false}"
    },
    {
      "質問": "担当教員の名前は何ですか",
      "FAQ にある": false,
      "学習前の分類": "format_error",
      "学習前の出力": "{\"answer\": \"確認が必要\"}",
      "学習後の分類": "needs_review",
      "学習後の出力": "{\"answer\": \"この FAQ には記載がありません。運営に確認が必要です。\", \"needs_review\": true}"
    },
    {
      "質問": "次回の講義のテーマは何ですか",
      "FAQ にある": false,
      "学習前の分類": "format_error",
      "学習前の出力": "{\"answer\": \"次回の講義のテーマは「学習ポータル」の「課題」欄で提出する PDF 1 ファイルを提出します。\"}",
      "学習後の分類": "needs_review",
      "学習後の出力": "{\"answer\": \"この FAQ には記載がありません。運営に確認が必要です。\", \"needs_review\": true}"
    },
    {
      "質問": "課題の提出期限はいつですか",
      "FAQ にある": false,
      "学習前の分類": "format_error",
      "学習前の出力": "{\"answer\": \"9 月 30 日 23:59\"}",
      "学習後の分類": "asserted",
      "学習後の出力": "{\"answer\": \"課題の提出期限は 9 月 30 日 23:59（日本時間）です。\", \"needs_review\": false}"
    }
  ],
  "executed_training_myself": true
}
変更後　result.json
{
  "config": {
    "base_model": "Qwen/Qwen3.5-0.8B",
    "revision": "2fc06364715b967f1860aea9cf38778875588b17",
    "seed": 42,
    "num_train_epochs": 40,
    "warmup_steps": 6,
    "lr": 0.0002,
    "lora_r": 16,
    "lora_alpha": 32,
    "max_seq_len": 512,
    "max_new_tokens": 96,
    "gen": {
      "do_sample": false
    }
  },
  "versions": {
    "transformers": "5.18.0",
    "peft": "0.21.1",
    "trl": "1.14.2",
    "torch": "2.11.0+cu130"
  },
  "method": "LoRA（量子化なし）",
  "before": {
    "label": "before",
    "n": 21,
    "format_rate": 0.09523809523809523,
    "content_rate": 0.0,
    "both_rate": 0.0,
    "seconds": 32.7
  },
  "after": {
    "label": "after",
    "n": 21,
    "format_rate": 1.0,
    "content_rate": 0.8571428571428571,
    "both_rate": 0.8571428571428571,
    "seconds": 43.9
  },
  "regressed": [],
  "knowledge": [
    {
      "質問": "この講座の最終課題の締切はいつですか",
      "FAQ にある": false,
      "学習前の分類": "format_error",
      "学習前の出力": "{\"answer\": \"9 月 30 日 23:59\"}",
      "学習後の分類": "asserted",
      "学習後の出力": "{\"answer\": \"課題の提出期限は 9 月 30 日 23:59（日本時間）です。\", \"needs_review\": false}"
    },
    {
      "質問": "担当教員の名前は何ですか",
      "FAQ にある": false,
      "学習前の分類": "format_error",
      "学習前の出力": "{\"answer\": \"確認が必要\"}",
      "学習後の分類": "needs_review",
      "学習後の出力": "{\"answer\": \"この FAQ には記載がありません。運営に確認が必要です。\", \"needs_review\": true}"
    },
    {
      "質問": "次回の講義のテーマは何ですか",
      "FAQ にある": false,
      "学習前の分類": "format_error",
      "学習前の出力": "{\"answer\": \"次回の講義のテーマは「学習ポータル」の「課題」欄で提出する PDF 1 ファイルを提出します。\"}",
      "学習後の分類": "needs_review",
      "学習後の出力": "{\"answer\": \"この FAQ には記載がありません。運営に確認が必要です。\", \"needs_review\": true}"
    },
    {
      "質問": "課題の提出期限はいつですか",
      "FAQ にある": false,
      "学習前の分類": "format_error",
      "学習前の出力": "{\"answer\": \"9 月 30 日 23:59\"}",
      "学習後の分類": "asserted",
      "学習後の出力": "{\"answer\": \"課題の提出期限は 9 月 30 日 23:59（日本時間）です。\", \"needs_review\": false}"
    }
  ],
  "executed_training_myself": true
}
- ファインチューニングが向く場面・向かない場面についての考察：ファインチューニングが向くケースは回答形式や文章のスタイルを統一したい場面。
- 一方、ファインチューニングが向かないケースは「知らないことへの判断（確認を求めるor断定）」であると考えました。
