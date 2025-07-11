# Alpha Miners - スクリプトドキュメント

このドキュメントは、アルファ戦略の管理とフロントエンドデータ生成のための主要なスクリプトについて説明します。

## メインスクリプト

### 1. `generate_source_code.py`
**目的**: すべてのアルファ戦略クラスからソースコードを自動的に抽出し、フロントエンド用の`source_code.json`を生成します。

**使用方法**:
```bash
python generate_source_code.py
```

**機能**:
- `alpha/`ディレクトリ内のすべてのPythonファイルをスキャン
- AST解析を使用して各アルファ戦略クラスのソースコードを抽出
- 以下の内容を含む`vercel-frontend/public/data/source_code.json`を生成:
  - クラス名
  - ソースファイル
  - 完全なソースコード
  - 言語識別子（Python）

**出力**:
- `vercel-frontend/public/data/source_code.json` (~190KB)
- ファイルごとに抽出されたクラスを示すコンソールサマリー

### 2. `generate_vercel_data.py`
**目的**: Vercelフロントエンドデプロイメント用の包括的なデータ生成。

**使用方法**:
```bash
python generate_vercel_data.py
```

**機能**:
1. **`generate_source_code.py`を呼び出し**てソースコードデータを作成
2. **アルファリーダーボードのコピー**を`alpha_leaderboard.json`から実行
3. **戦略数とタイムスタンプを含むメタデータを生成**
4. **トップパフォーマーを含むサマリー統計を作成**
5. **実際のパフォーマンスデータを使用して累積リターンを生成**
6. **フロントエンド用のAPIスタイルデータを作成**

**生成ファイル**:
- `source_code.json` - すべてのアルファソースコード (185.6 KB)
- `alpha_leaderboard.json` - パフォーマンスランキング (61.9 KB)
- `summary.json` - サマリー統計 (1.7 KB)
- `cumulative_returns.json` - 時系列データ (199.3 KB)
- `metadata.json` - メタデータ情報 (0.3 KB)
- `api.json` - 統合APIデータ (252.6 KB)

### 3. `run_alphas.py`
**目的**: すべてのアルファ戦略を実行し、パフォーマンス結果を生成します。

**使用方法**:
```bash
python run_alphas.py
```

**機能**:
- 48業種ポートフォリオデータをロード
- 登録されたすべてのアルファ戦略を実行（119クラス、300以上のインスタンス）
- パフォーマンスメトリクスを計算（シャープレシオ、リターン、ドローダウンなど）
- 分析用の出力ファイルを生成

**生成ファイル**:
- `alpha_results.json` - 各アルファの詳細結果
- `alpha_performance.csv` - CSV形式のパフォーマンスメトリクス
- `alpha_leaderboard.json` - ランク付けされたパフォーマンスデータ

## ワークフロー

### 完全なアルファ開発ワークフロー:

1. **`alpha/`ディレクトリで新しいアルファを開発**
2. **`alpha/__init__.py`と`run_alphas.py`でアルファを登録**
3. **バックテストを実行**: `python run_alphas.py`
4. **フロントエンドデータを生成**: `python generate_vercel_data.py`
5. **Vercelにデプロイ**: `cd vercel-frontend && npm run build && vercel --prod`

### クイックフロントエンド更新:

ソースコードのみを更新する場合（例：新しいアルファを追加した後）:
```bash
python generate_source_code.py
```

完全なデータリフレッシュが必要な場合:
```bash
python generate_vercel_data.py
```

## アルファ戦略の構造

### ファイル構成:
```
alpha/
├── __init__.py              # すべてのアルファをインポート
├── base_alpha.py           # BaseAlphaクラスとAlphaEngine
├── momentum_alphas.py      # モメンタムベースの戦略
├── statistical_alphas.py   # 統計的アプローチ
├── enhanced_alphas.py      # 強化技術
├── simple_alphas.py        # シンプルな取引戦略
├── advanced_alphas.py      # 洗練された手法
├── new_alphas.py          # 最近のイノベーション
├── innovative_alphas.py    # 創造的なアプローチ
├── next_gen_alphas.py     # 最先端の戦略
├── quantum_alphas.py      # 量子インスパイアの手法
└── professional_alphas.py  # プロフェッショナルグレードの戦略
```

### アルファカテゴリ:
- **11ファイル全体で119の総クラス数**
- **異なるパラメータを持つ300以上の戦略インスタンス**
- カテゴリ: モメンタム、平均回帰、ボラティリティ、クロスセクショナル、レジームベースなど

### 最近の追加（プロフェッショナルアルファ）:
1. `MarketStructureArbitrage` - 市場マイクロ構造の非効率性を利用
2. `VolatilityClusteringPredictor` - ボラティリティクラスタリングパターンを予測
3. `CrossSectionalMeanReversionSpeed` - 平均回帰速度分析
4. `AdaptiveRiskParityAlpha` - 相関調整付きダイナミックリスクパリティ
5. `MomentumQualityScore` - 品質フィルタリングされたモメンタム戦略
6. `VolatilitySpilloverAlpha` - クロスアセットボラティリティスピルオーバー効果
7. `RegimeAwareReversal` - レジーム依存リバーサル戦略
8. `LiquidityDrivenMomentum` - 流動性フィルタリングされたモメンタム
9. `MacroSentimentAlpha` - 経済センチメント駆動シグナル
10. `VolatilityTermStructureAlpha` - ボラティリティ期間構造の活用

## データフロー

```
アルファ開発 → run_alphas.py → generate_vercel_data.py → Vercelフロントエンド
                            → generate_source_code.py ↗
```

## フロントエンド統合

生成されたJSONファイルは`vercel-frontend/`のNext.jsフロントエンドで使用されます:

- **ソースコードビューア**: `source_code.json`を使用
- **パフォーマンスチャート**: `cumulative_returns.json`を使用
- **リーダーボード**: `alpha_leaderboard.json`を使用
- **サマリー統計**: `summary.json`を使用
- **APIエンドポイント**: `api.json`を使用

## エラーハンドリング

- スクリプトには包括的なエラーハンドリングが含まれています
- ファイルが見つからない場合は警告を生成しますが、実行は停止しません
- ソースコード抽出は解析エラーを適切に処理します
- 実際のデータが利用できない場合はシミュレーションデータにフォールバック

## パフォーマンスノート

- ソースコード抽出: 119クラスで約1-2秒
- フルデータ生成: データサイズに応じて約5-10秒
- 生成ファイル合計: 約900KBの圧縮データ
- 本番デプロイメント準備完了

## メンテナンス

新しいアルファ戦略を追加するには:
1. 適切な`alpha/*.py`ファイルに新しいクラスを作成
2. `alpha/__init__.py`にインポートを追加
3. `run_alphas.py`のcreate_all_alphas()関数に登録
4. 新しいカテゴリを追加する場合はこのREADMEを更新

スクリプトは変更なしで新しい戦略を自動的に検出して含めます。