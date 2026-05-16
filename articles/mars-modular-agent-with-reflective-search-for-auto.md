---
title: "MARS: Modular Agent with Reflective Search for Automated AI Research"
source_url: "https://research.google/pubs/mars-modular-agent-with-reflective-search-for-automated-ai-research/"
source_type: "publication"
published_at: "2026-05-16"
processed_at: "2026-05-16"
confidence: "medium"
tags: ["research", "ml", "publication"]
---
# 要約（3行)
- Google Researchが、AI研究の自動化に特化したエージェント基盤「MARS（Modular Agent with Reflective Search）」を提案。
- コスト制約付きMCTSによる予算意識の探索、Design-Decompose-Implementの分割開発、比較型リフレクティブメモリでの知見抽出を組み合わせて動作。
- MLE-Benchでオープンソース系SOTAを達成し、探索分岐間の知識転移（63%）により汎化的な改善が生じる点が重要。

## 1. 何が新しいか
- 計算コストを明示的に扱うcost-constrained Monte Carlo Tree Search（MCTS）を研究探索に適用した点。
- 単一スクリプト生成ではなく、Design-Decompose-Implementという段階分割でリポジトリ全体を構築する枠組み。
- 解の差分比較に基づく「Comparative Reflective Memory」によるクレジット割当の改善。
- 探索分岐間での知識転移（cross-branch transfer）を定量化し、63%がそこから来ると報告。
- MLE-Benchという評価環境で、既存オープンソースフレームワークを上回る性能を示した点。

## 2. 技術ポイント（エンジニア向け）
- MARSは3本柱：Budget-Aware Planning、Modular Construction、Comparative Reflective Memoryで構成。
- Budget-Aware Planningでは、コスト制約付きMCTSを用いて性能向上と実行コスト（例：モデル学習）を同時最適化。
- 探索ノードの評価にコストを組み込むことで、高価な試行を抑制しつつ有望領域を探索。
- Modular Constructionは「Design → Decompose → Implement」のパイプラインで、複雑な研究コードベースを段階的に生成・更新。
- Comparative Reflective Memoryは、複数解の差分を解析し、どの変更が性能に寄与したかを推定（クレジット割当問題への対処）。
- 探索中に得た知見をメモリ化し、別分岐へ転用することで探索効率を向上。
- 評価はMLE-Bench上で実施され、同等条件でオープンソース系の中でSOTAを達成（詳細設定は本文外のため不確実）。
- 「Aha! moments」として、63%の有効な学習が分岐間転移から生じたと報告。
- 従来のLLMエージェントが生成しがちなモノリシックなスクリプトを回避する設計。

## 3. 実装・運用への示唆
- 高コストな実験（学習・評価）が支配的な環境では、MCTSにコスト制約を組み込む設計が実運用の計算予算管理に直結する。
- Design-Decompose-Implementの分割は、大規模リポジトリ生成時のレビューや差分管理（CI/CDやコード所有境界）と相性がよい。
- 差分比較ベースのメモリは、実験管理ツール（例：実験トラッキング）と連携しないと再現性や因果推定が弱くなる可能性がある。
- 分岐間の知識転移を前提にするため、探索履歴の保存・再利用（ストレージ設計と検索戦略）がボトルネックになりうる。
- MCTSの評価関数に何を含めるか（精度・コスト・時間・安定性）が結果を大きく左右するため、ドメインごとのチューニングが必要。
- モノリシック生成を避ける設計は、既存の自動コード生成パイプラインを段階化するリファクタリングを伴う。

## 4. 注意点・限界
- MLE-Benchの詳細設定や比較対象の条件が本文要約からは不明で、汎用的優位性は断定できない。
- 63%という知識転移の割合はタスク依存の可能性があり、他ドメインで再現されるかは不確実。
- コスト制約付きMCTSの具体的な報酬設計・ハイパーパラメータが明示されていないため、実装難易度の見積もりが難しい。
- Comparative Reflective Memoryの差分分析が誤った因果推定をするリスク（相関と因果の混同）。
- ブログ要約ベースの情報であり、論文本文の詳細アルゴリズムやアブレーションが未確認。

## 5. こんなチームに有効
- 自動化されたML研究基盤やAutoMLを開発するチーム
- 大規模実験基盤（GPUクラスタ）を運用するMLOps/Platformチーム
- エージェントベースのコード生成・研究支援ツールを開発するR&Dチーム

## 6. 5分で話せる説明
- 背景として、AI研究の自動化は評価コストが高く、どの変更が効いたか分かりにくい点が難しい。
- MARSはそのために、コストを意識した探索（MCTS）、段階的なコード構築、差分からの学習という3つを組み合わせている。
- まずMCTSで、精度向上と計算コストのバランスを取りながら実験候補を探索する。
- 次にDesign-Decompose-Implementで、大きな研究タスクを分割しながらコードを生成・更新する。
- 並行して、異なる解の差分を比較して有効な変更をメモリ化し、他の探索分岐に再利用する。
- その結果、MLE-Benchで高い性能を示し、特に分岐間の知識転移が探索効率に寄与していると報告されている。

## 7. 私の視点
- コスト制約を探索に組み込んだ点は、実運用の制約に直結しており実務的価値が高い。
- 差分ベースのリフレクションは有効だが、因果推定の誤りをどう抑えるかが実用化の鍵になりそう。
- モジュール分割はソフトウェア工学的には妥当で、既存の開発フローとの統合も現実的に見える。
- 一方で、MCTS＋メモリ＋分割生成の組み合わせはシステムが重くなりやすく、小規模環境では過剰設計になる可能性もある。

## 8. どこまで鵜呑みにしてよいか
- Google Research発であり基礎的な信頼性は高いが、査読状況は不明。
- MLE-BenchでのSOTA主張は条件依存のため、同一設定での再現確認が必要。
- 63%の知識転移という数値は魅力的だが、他タスクでの再現性を検証すべき。
- アルゴリズム詳細（MCTSの設計、メモリ更新規則）が未確認のため、実装時は論文本文の精読が前提。

## 9. 関連して見るとよい論点
- AutoMLやNeural Architecture Searchにおける探索戦略との比較（進化計算 vs MCTS）
- LLMエージェントの長期メモリ設計（vector DB vs 構造化差分メモリ）
- 実験管理ツール（Weights & Biases等）との統合可能性
- プログラム合成におけるモジュール分割手法との共通点
- 計算予算制約下での最適化（multi-objective optimization）の既存研究

## 原文リンク
- https://research.google/pubs/mars-modular-agent-with-reflective-search-for-automated-ai-research/
