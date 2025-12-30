---
title: "OpenLane2 セットアップ進捗メモ（2025-12-30）"
date: 2025-12-30
tags: [OpenLane2, Yosys, Docker, Setup, Status]
---

# OpenLane2 セットアップ進捗メモ

本書は、OpenLane2 環境構築における **2025-12-30 時点の到達点整理** と  
**次回作業の明確化** を目的とするステータスドキュメントである。

---

## 1. 目的

- OpenLane2 を用いた RTL → GDS フローを **gcd デザイン**で実行
- ローカル環境（WSL2 / Ubuntu 20.04）での動作確認

---

## 2. ここまでの到達点（結論）

```
① Yosys 単体ビルド        ✅ 完了
② OpenLane2 実行環境      ⚠️ 途中（Docker/GHCR 認証で停止）
③ OpenLane フロー実行     ❌ 未到達
```

---

## 3. Yosys 状態

### 3.1 ビルド結果

- 自前ビルド成功
- 正常にインストール済み

```bash
$ yosys -V
Yosys 0.38 (git sha1 543faed9c, clang 10.0.0-4ubuntu1)
```

### 3.2 評価

- Yosys 自体には **問題なし**
- OpenLane からの呼び出し対象としては要件を満たしている

---

## 4. OpenLane2 状態

### 4.1 実行結果

- `poetry install` 成功
- OpenLane2 (2.3.10) インストール完了

### 4.2 発生しているエラー

```text
Generate JSON Header: subprocess ('yosys -y ...') failed
OpenLane will now quit.
```

### 4.3 本質的原因

- OpenLane2 は **Docker 実行前提の設計**
- 現在はローカル環境でフローを直接回そうとしている状態
- OpenROAD / Magic / Netgen など **Docker内ツール群が存在しない**

👉 JSON Header ステージ（Stage 5）で必ず失敗するのは仕様通り

---

## 5. Docker 状態

### 5.1 Docker 自体

```bash
$ docker --version
Docker version 26.1.3
```

- Docker エンジンは正常

### 5.2 問題点

以下のイメージ取得がすべて失敗：

```text
ghcr.io/efabless/openlane:*
ghcr.io/chipfoundry/openlane2:*
```

エラー内容：

```text
failed to resolve reference
not found
```

### 5.3 原因

- **GHCR (GitHub Container Registry) に未ログイン**
- イメージが存在しないのではなく、認証されていない

---

## 6. 現在地の整理（重要）

```
[完了]   Yosys ビルド
[完了]   OpenLane2 ソース取得
[停止]   Docker (GHCR 未認証)
[未到]   OpenLane 公式コンテナ起動
[未到]   gcd フロー完走
```

---

## 7. 次回やること（最小ステップ）

### 7.1 必須作業（これだけ）

```bash
docker login ghcr.io
```

- GitHub ユーザー名
- GitHub Personal Access Token
  - 権限: `read:packages`

### 7.2 その後の想定コマンド

```bash
docker pull ghcr.io/efabless/openlane:latest
```

または OpenLane2 指定バージョン。

---

## 8. 補足メモ

- 現在の状態は「迷走」ではない
- OpenLane2 初期導入で **最も多い停止点（GHCR 認証）**
- 技術的なミスではなく、運用前提差分によるもの

---

## 9. 次回再開ポイント

1. GHCR ログイン成功ログ確認
2. OpenLane Docker イメージ取得確認
3. gcd デザインで `openlane run` 再実行

---

（EOF）
