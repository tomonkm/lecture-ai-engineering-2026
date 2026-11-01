# 第2回 解答

## 演習

- 使ったモデル・設定（`result.json` の `config` から）：
  {'base_model': 'Qwen/Qwen3.5-0.8B',
 'revision': '2fc06364715b967f1860aea9cf38778875588b17',
 'seed': 42,
 'num_train_epochs': 20,
 'warmup_steps': 6,
 'lr': 0.0002,
 'lora_r': 16,
 'lora_alpha': 32,
 'max_seq_len': 512,
 'max_new_tokens': 96,
 'gen': {'do_sample': False}}

- 自分で学習を実行したか（`executed_training_myself`）：はい / いいえ（実行できた範囲：　）
- 学習前：`format_rate` = 0.09523809523809523　 / `content_rate` =　0.0
- 学習後：`format_rate` = 1.00　 / `content_rate` =　0.9047619047619048
- 退行した質問：none
- 学習後に残った失敗と、その型（根拠なしの質問に断定した / 根拠ありの質問に確認要求で逃げた / 形式の崩れ）：
  Q15 & 20. Content failures where these two return false instead of the needs_review = True, which is indeed the correct answer. 根拠なしの質問に断定した
- 知識を問う質問（セクション 6）の結果（学習前の分類 → 学習後の分類）と気づいたこと：slightly improved; however, fine-tuned model still struggles with ambgiuous questions where the Qs involve the word like "final submission" where in the asnwer, only the sentence with "submission" is available.
- 学習で変わったこと・変わらなかったこと： Content remain problematic. Especially the fine-tuned models seem to provide the answer from   the partially matched FAQ list. Thus, it looks that the fine-tuned model focuses not on     the details of sentence but semantic similarity of the overall sentence 

## 宿題

- 取り組んだレベル：基礎
- 変えた設定やデータ：change lr to 2e-2 or 2e-6
- 結果（比較表や数字）：v1 2e-2 => deteriorated. all incorrect, v2 2e-6 => "format_rate": 0.9047619047619048, "content_rate": 0.7142857142857143
- ファインチューニングが向く場面・向かない場面についての考察：fine-tuning could be effective for changing the format if it is configured with optimal hyperparameters. However, it still struggles with the content & there is clear tendency that the fine-tuning forces the model to pay attention to the semantic similarity 
