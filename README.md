# 瞳孔評価 - Pupil Assessment App

## Functional Neuro Training (FNT)

カメラで眼を撮影し、AIが左右の瞳孔サイズを自動測定。動眼神経の機能評価を行うPWAアプリケーションです。

---

## 🧠 神経科学的背景

### 瞳孔不同（Anisocoria）とは

瞳孔不同は、左右の瞳孔サイズに差がある状態を指します。瞳孔の大きさは、以下の2つの神経系のバランスによって制御されています：

1. **副交感神経系**（動眼神経経由）: 瞳孔を収縮させる（縮瞳）
2. **交感神経系**: 瞳孔を拡大させる（散瞳）

### 動眼神経（第III脳神経）の役割

動眼神経は、瞳孔括約筋を支配し、瞳孔の収縮を制御します。この神経経路には以下が含まれます：

- **中脳**: Edinger-Westphal核（副交感神経核）
- **毛様体神経節**: 眼窩内のシナプス中継点
- **瞳孔括約筋**: 虹彩の環状筋

### 臨床的意義

瞳孔不同は以下の神経学的状態を示唆する可能性があります：

| 状態 | 特徴 |
|------|------|
| **動眼神経麻痺** | 患側の散瞳、眼瞼下垂、外斜視 |
| **Horner症候群** | 患側の縮瞳、眼瞼下垂、無汗症 |
| **Adie瞳孔** | 一側性散瞳、対光反射遅延 |
| **外傷性脳損傷** | 急性散瞳（脳ヘルニアの警告徴候） |
| **脳卒中・脳腫瘍** | 一側性瞳孔変化 |

### 評価基準

| 左右差 | 評価 | 解釈 |
|--------|------|------|
| ≤0.5mm | 正常範囲 | 生理的瞳孔不同の範囲内 |
| 0.5〜1.0mm | 軽度不同 | 経過観察推奨 |
| >1.0mm | 臨床的に有意 | 専門医への相談推奨 |

> **注意**: 健常者の約20%に0.4mm程度の生理的瞳孔不同が見られます。

---

## 🔬 技術的実装

### AI顔検出

本アプリは**TensorFlow.js**と**MediaPipe Face Mesh**モデルを使用して、リアルタイムで顔のランドマークを検出します。

- **478個のランドマークポイント**を検出
- **虹彩中心および外周ポイント**を特定（インデックス468-477）
- フレームレート: 約15-30 FPS（デバイス依存）

### 瞳孔サイズ推定アルゴリズム

1. **虹彩直径の測定**: 虹彩の中心点と外周点の距離から直径を計算
2. **顔幅によるキャリブレーション**: 顔幅（約14cm）を基準に実寸への変換係数を算出
3. **瞳孔/虹彩比の適用**: 瞳孔は虹彩の約30-40%として推定
4. **スムージング処理**: 指数移動平均でノイズを低減

### 精度の限界

- カメラ解像度、照明条件、顔の角度に依存
- 医療グレードの精度ではない（参考値として使用）
- 複数回測定の平均値を推奨

---

## 📱 使用方法

### 準備

1. 明るすぎず暗すぎない環境を確保
2. カメラから約30〜50cm離れる
3. フロントカメラを使用（ミラー表示）

### 測定手順

1. **アプリ起動**: AIモデルの読み込みを待つ
2. **顔を合わせる**: ガイドフレーム内に顔を配置
3. **測定開始**: 「測定開始」ボタンをタップ
4. **静止**: 顔を動かさずに数秒待つ
5. **記録**: 「撮影・記録」で測定値を保存

### 結果の解釈

- **L（Left）**: 左瞳孔サイズ（mm）
- **R（Right）**: 右瞳孔サイズ（mm）
- **Δ（差分）**: 左右差（mm）

---

## ⚠️ 注意事項

### 医学的免責事項

**本アプリは医療診断ツールではありません。**

- スクリーニング・参考用途のみ
- 異常値が継続する場合は医療専門家に相談
- 急性の神経症状（頭痛、視力変化、意識障害など）がある場合は直ちに受診

### 測定精度に影響する要因

- 照明条件（暗すぎると虹彩境界が不明瞭）
- カメラ品質・解像度
- 顔の角度・距離
- 眼鏡・コンタクトレンズ
- 瞬きのタイミング

---

## 🛠️ 技術仕様

### PWA機能

- ✅ オフライン対応（Service Worker）
- ✅ ホーム画面インストール
- ✅ フルスクリーン表示
- ✅ クロスプラットフォーム（iOS/Android/PC）

### 依存関係

- TensorFlow.js 4.10.0
- MediaPipe Face Landmarks Detection 1.0.5

### ブラウザ要件

- Chrome 80+
- Safari 14.1+
- Firefox 90+
- Edge 80+

---

## 📚 参考文献

1. Hall CA, Chilcott RP. Eyeing up the Future of the Pupillary Light Reflex in Neurodiagnostics. *Diagnostics (Basel)*. 2018;8(1):19.

2. Lam BL, Thompson HS, Corbett JJ. The prevalence of simple anisocoria. *Am J Ophthalmol*. 1987;104(1):69-73.

3. Cartridge NEF. Pupil disorders: clinical aspects and pharmacological applications. *J Neurol Neurosurg Psychiatry*. 2006;77(1):2-3.

4. Bazilinskyy P, de Winter J. Eye movements as a proxy measure for human driving performance using computer vision techniques. *PLoS ONE*. 2023;18(1):e0280607.

5. MediaPipe Face Mesh. Google AI. https://google.github.io/mediapipe/solutions/face_mesh.html

---

## 📁 ファイル構成

```
pupil-assessment/
├── index.html          # メインアプリケーション
├── manifest.json       # PWAマニフェスト
├── sw.js              # Service Worker
├── FNT512.png         # ロゴ（白背景）
├── FNT512-transparent.png  # ロゴ（透明背景）
└── README.md          # このファイル
```

---

## 🚀 デプロイ方法

### GitHub Pages

1. リポジトリを作成
2. 全ファイルをアップロード
3. Settings → Pages → Source: main branch
4. `https://[username].github.io/[repository]/` でアクセス

### ロゴファイルの追加

- `FNT512.png`: ホーム画面アイコン用（白背景）
- `FNT512-transparent.png`: アプリ内ヘッダー用（透明背景）

---

© 2025 Functional Neuro Training
