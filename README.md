# montecarlo-ros2
ロボットシステム学の`課題２`用のROS2パッケージを保存するリポジトリです。
***

## パッケージの概要
- モンテカルロ法を用いて円周率の近似値を求めます。
- 円周率はfloat32にて計算されます。

### モンテカルロ法について
モンテカルロ法は、確率論を利用した数値計算手法です。このプロジェクトでは、単位正方形内にランダムな点を生成し、その中で単位円に含まれる点の割合を用いて円周率を近似します。

****

## ノードについて
このリポジトリには、モンテカルロ法を用いて円周率（π）を近似するための2つのROS 2ノードが含まれています。

### 構成

1.`monte_carlo_publisher.py`:
- `0.3秒`ごとに、1回ずつランダムな点を生成し、モンテカルロ法を使用して円周率(π)を計算してその結果をトピック`random_pi_estimator`に`渡すパブリッシャーノード。

2.`result.py`:
- トピック`random_pi_estimator`を購読し、試行回数と近似したπの値をログ出力する。
- パブリッシャーノード`monte_carlo_publisher.py`の起動確認・テスト用の簡易的なサブスクライバーノード。

***
## ファイル構成

- `mypkg/monte_carlo_publisher.py`: モンテカルロ法を使用して円周率を近似し、結果を`random_pi_estimator`トピックにパブリッシュします。
- `mypkg/result.py`: `random_pi_estimator`トピックを購読し、円周率の近似値と試行回数を表示します。
- `launch/monte_carlo_publisher-result.launch.py`: 両方のノードを起動するためのROS 2 Launchファイルです。

## 動作の仕組み

1.`monte_carlo_publisher.py`:
- 範囲`[-1, 1]`内でランダムな`(x, y)`座標を生成。
- 点が単位円内`x^2 + y^2 <= 1`に含まれるかを判定。
- 単位円内の点の割合を基に、円周率を近似計算。
- 近似した円周率(π)をトピック`random_pi_estimator`に渡す。

2.`result.py`:
- トピック`random_pi_estimator`から円周率(π)を受取り、`試行回数:16 円周率: 3.5`のような形で出力する。
- 試行回数と近似した円周率をログに記録する。

***
## 使い方

### 前提条件
- ROS2がインストールされていること。
- ROS 2のワークスペースが適切にセットアップされていること。

### ビルドと実行

1.リポジトリをクローン:
```
git clone https://github.com/ChumaNaoki/montecarlo-ros2.git
```

2.ワークスペースをビルド：
```
cd ~/ros2_ws
colcon build
```

3.ワークスペースをソース：
```
source ~/ros2_ws/install/setup.bash
source ~/ros2_ws/install/local_setup.bash
```

4.以下のコマンドでローンチファイルを使用して両方のノードを実行：
```
ros2 launch mypkg monte_carlo_publisher-result.launch.py
```

***
# 注意事項
- このパッケージにおけるノード`monte_carlo_publisher.py`のテストでは、結果がランダムで出力されるため出力結果のテストが行われていません。launchファイルを使用し実行した結果エラーが出力されないことまでしか確認出来ていませんがご了承ください。(手持ちの環境では正しく動作しました。)
- このパッケージにおけるノード`monte_carlo_publisher.py`はあくまで、モンテカルロ法を使用して円周率(π)を計算し、その結果をトピック`random_pi_estimator`に渡しているだけです。launchファイルを使用し実行した結果の`試行回数:16 円周率: 3.5`における試行回数の数値は、ノード`result.py`にて計算している為、ノードを個別に実行した場合は試行回数の数値が`result.py`が起動された回数になります。

***
## テスト環境
- OS: Ubuntu 20.04 LTS
- ROS 2 バージョン: Foxy Fitzroy
- Python バージョン: Python 3.8.10

# ライセンス
- このソフトウェアパッケージは３条BSDライセンスの下、再頒布および使用が許可されます。
- このパッケージのtest.ymlにて, [こちらのコンテナ](https://hub.docker.com/r/ryuichiueda/ubuntu22.04-ros2/tags)（by Ryuichi Ueda）が使用されています.
- © 2025 Chuma Naoki
