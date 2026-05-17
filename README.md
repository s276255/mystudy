# 予後予測画像生成プロジェクト (Masked-ControlNet / Pix2Pix)

本リポジトリは、卒業論文およびICPR 2026等の国際会議に向けた、医用画像（MRI等）の予後予測画像生成に関する研究コードを管理しています。
Masked-ControlNetアーキテクチャおよびベースラインであるPix2Pixを用いた学習・推論・評価パイプラインを含みます。

## 動作環境 (Environment)
- OS: Ubuntu / Linux
- GPU: NVIDIA TITAN X (Pascal)
- CUDA Version: 12.2
- 主要ライブラリ: PyTorch, Diffusers, nnUNet

## ディレクトリ構成とファイル概要

### 1. データ準備・前処理 (`00` - `01`)
- `00_initial_setup.ipynb`
  - 環境構築、必要なライブラリのインポートテスト、ディレクトリの初期化。
- `01_data_preparation.ipynb`
  - MRIデータ（T1pre, T1post, FLAIRなど）の前処理パイプライン。
  - Skullstrip処理、病変部（Metastases）の差分画像作成、nnUNet用のデータ形式変換など。

### 2. モデルの学習 (`02_model_training_*.ipynb`)
ベースラインモデルと、提案手法であるMasked-Loss（$\mathcal{L}_{mask}$）を導入したモデルの比較実験コード群です。

- **ベースライン (Pix2Pix)**
  - `02_model_training_Pix2Pix.ipynb`
- **標準ControlNet / LDM**
  - `02_model_training_l+c_50.ipynb` (50エポック)
  - `02_model_training_l+c_150.ipynb` (150エポック)
- **提案手法 (Masked-ControlNet)**
  - `02_model_training_l+Lmask_150.ipynb`
  - `02_model_training_l+c+Lmask_150_λ5.ipynb` ($\lambda=5$)
  - `02_model_training_l+c+Lmask_150_λ10.ipynb` ($\lambda=10$)
  - `02_model_training_l+c+Lmask_150_λ15.ipynb` ($\lambda=15$)
  - `02_model_training_l+c+Lmask_200.ipynb` (200エポック)

### 3. 推論・評価・可視化 (`03_inference_*.ipynb`)
学習済みモデルを用いて推論を行い、評価指標の算出および論文用の図表（EPS形式など）を出力します。

- `03_inference_and_evaluation_Pix2Pix.ipynb`
- `03_inference_and_visualization.ipynb` (統合可視化用)
- 各条件の推論用ノートブック (`_l+c_50`, `_l+c_150`, `_l+c+Lmask_150_λ10` など)
  - ※一部の `test` が付与されたノートブックは動作確認・デバッグ用。

## データセットと重みファイルについて（Git管理外）
ファイルサイズ制限のため、以下のデータや学習済みモデルはリポジトリ（GitHub）には含まれていません。ローカル環境で適切なディレクトリに配置して実行してください。

- **データセット**: `my_yale_data/`, `UCSF-BMSR-benchmarks/`, 各種ZIPアーカイブ (`Metastases_*.zip`, `skullstrip_FLAIR.zip` 等)
- **モデルの重み**: `trained_models_*/` ディレクトリに各条件ごとの `.pt` / `.pth` ファイルが保存されます。
- **生成物**: `nnUNet_results/`, `generated_images_pix2pix/`, 推論出力のEPSファイル等。論文用の清書された図表の一部は `paper_figures/` で管理しています。
