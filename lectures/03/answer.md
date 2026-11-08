# 第3回 解答

## 演習

- 第1回（素のLLM）とエージェントの回答の違い： agent with tools show more consistent & transparent behaviors
- `--trace` で見えたツール呼び出し（ツール名と引数）：[tool call]   make_quiz({'lecture': 2})
- 1つの質問での LLM 呼び出し回数（`requests`）：dependends on the question
- 追加したツールと、docstring に書いた説明：make_quiz. 
    INSTRUCTIONS = """\
    あなたはAIエンジニアリング講座（AIE2026）の学習支援エージェントです。
    - 用語や講座に固有のことを答える前に、必ず search_glossary で用語集を確認する。
    - 回の内容を聞かれたら get_lecture_summary を使う。
    - 復習問題を求められたら make_quiz を使う。
    - 用語集に見つからないことは、推測で断定せず「用語集にはない」と伝える。
    - 回答の最後に、関係する回（第N回）を示す。
    - 宿題の答えそのものは書かず、考え方のヒントで導く。
    """

## 宿題

- 取り組んだレベル：基礎
- やったこと：implemeted make_quiz tool & registered the tool in agent.py. Asking the same question to see how the output of the agent will change.
- 結果： 

        """agent output"""
            (aie2026-exercises) @tomonkm ➜ /workspaces/lecture-ai-engineering-2026 (lec03) $ uv run python -m app.agent --trace "give me two quizes. One from lec1 & another from lec2"
        [model] gemini-3.5-flash-lite
        第1回と第2回からの復習問題です！

        ---

        ### 第1回の復習問題
        **Q:** 「利用者の質問とは別に、モデルの振る舞い（役割、口調、守るべきルールなど）を指定する入力。Pydantic AIでは Agent の instructions 引数で与える。」を表す用語は何ですか？
        *(答えは考えてみてくださいね！)*

        ---

        ### 第2回の復習問題
        **Q:** 「LoRAで使う低ランク行列の大きさを決めるハイパーパラメータ。大きいほど表現力が上がるが、学習するパラメータとメモリも増える。」を表す用語は何ですか？
        *(こちらも考えてみてくださいね！)*

        ---
        （関連する回：第1回、第2回）

        --- trace ---
        [tool call]   make_quiz({'lecture': 1})
        [tool call]   make_quiz({'lecture': 2})
        [tool return] make_quiz -> {"question": "「利用者の質問とは別に、モデルの振る舞い（役割、口調、守るべきルールなど）を指定する入力。Pydantic AIでは Agent の instructions 引数で与える。」を表す用語は何ですか？", "ans…
        [tool return] make_quiz -> {"question": "「LoRAで使う低ランク行列の大きさを決めるハイパーパラメータ。大きいほど表現力が上がるが、学習するパラメータとメモリも増える。」を表す用語は何ですか？", "answer": "ランク（LoRAのr）"}…
        [usage] RunUsage(cost=Decimal('0.0007797'), details={'text_prompt_tokens': 999}, input_text_tokens=999, input_tokens=999, output_tokens=192, requests=2, tool_calls=2)
        (aie2026-exercises) @tomonkm ➜ /workspaces/lecture-ai-engineering-2026 (lec03) $ uv run python -m app.agent --trace "give me two quizes. One from lec1 & another from lec2"
        [model] gemini-3.5-flash-lite
        第1回と第2回からの復習問題です！

        ---

        ### 第1回の復習問題
        **【問題】**  
        「AIE2026の宿題の構成。「基礎（講義内容の確認）」「実装（自分でコードを書く）」「発展（調査や試行）」の3段階があり、どれか1つに取り組めばその回の宿題は完了。原則として採点はしない。」を表す用語は何ですか？

        *(ヒント: 宿題の進め方に関するレベル分けを表す言葉です)*

        **【答え】**  
        宿題の3レベル

        *(関連: 第1回)*

        ---

        ### 第2回の復習問題
        **【問題】**  
        「学習データにだけ過度に適合し、未知の入力に対する性能が落ちる状態。学習に使わない検証データで性能を測って気づく。」を表す用語は何ですか？

        *(ヒント: 「オーバーフィッティング」とも呼ばれます)*

        **【答え】**  
        過学習

        *(関連: 第2回)*

        --- trace ---
        [tool call]   make_quiz({'lecture': 1})
        [tool call]   make_quiz({'lecture': 2})
        [tool return] make_quiz -> {"question": "「AIE2026の宿題の構成。「基礎（講義内容の確認）」「実装（自分でコードを書く）」「発展（調査や試行）」の3段階があり、どれか1つに取り組めばその回の宿題は完了。原則として採点はしない。」を表す用語は何ですか…
        [tool return] make_quiz -> {"question": "「学習データにだけ過度に適合し、未知の入力に対する性能が落ちる状態。学習に使わない検証データで性能を測って気づく。」を表す用語は何ですか？", "answer": "過学習"}…
        [usage] RunUsage(cost=Decimal('0.0009467'), details={'text_prompt_tokens': 1014}, input_text_tokens=1014, input_tokens=1014, output_tokens=257, requests=2, tool_calls=2)
        """agent output"""

- 気づき・疑問： The appropriate tool was correctly called. The user query required the agent to create two different questions from different lectures & the agent was able to correctly call the tool twice. However, even with the same query, the return (especially format) significantly differs btw 1st qeury & 2nd query (such as the hit/tips included only on the 2nd query response). These behavioral deviations shold be controlled with the harness layer. 
