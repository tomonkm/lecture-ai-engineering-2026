# 第1回 解答

## 演習

- 講座に固有の質問（宿題の3レベル）へのモデルの回答（要約でよい）：
- 用語集の正しい意味と比べて気づいたこと：
- 表示できたトークン数（入力 / 出力）：

## 宿題

- 取り組んだレベル：基礎
- やったこと：ask the same question to 3 different models
- 結果：
    gemini-3.5-flash-lite token= 775
    gemini-3.6-flash token = 897
    gemini-omni-1.1-flash token = 859

    Ask the question "what is brain foundation model?".
    Compared with 3.5, 3.6 has generated the answer with bit more token consumption. The content-wise, the composition is slightly different where 3.5 focuses on what it is (input, output, process) where 3.6 provides information on use cases & challenges. 3.5  has the best latency while 3.6 seems the slowest. omni 1.1 has the similar content structure as 3.6 but including additional contents like ethical concerns.
- 気づき・疑問：
    I wonder why omni 1.1 shows the similar response structure as 3.6. Possibility is that we have more input for pretraining data focusing on topics around use cases & pragmatic concerns compared to the time when 3.5 was pretrained.
