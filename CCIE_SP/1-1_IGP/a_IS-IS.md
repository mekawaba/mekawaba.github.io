# 1.1.a IS-IS

Reference: IS-IS Network Design Solutions by Abe Martey, Scott Sturgess

- [1. IPルーティング概要](#IPルーティング概要)
- [2. IS-ISルーティングプロトコルの概要](#IS-ISルーティングプロトコルの概要)

## 1. IPルーティング概要
- IPルーティングはコントロールプレーンで実行
- IPルーティングプロトコルはネットワーク内の宛先への候補パスをRouting Information Base(RIB)へ保存
- RIB内の宛先ごとのベストパスがルーティングテーブルに載る
- IPフォワーディングでは、IPパケットのヘッダ情報を処理して、パケットを目的の宛先まで送る方法を決定
- これは、Forwarding Database(FIB)で出力インタフェースを検索、IP time-to-live (TTL)を減らす、IPチェックサム値の計算（伝送中のビット反転や誤りの検出）、出力インタフェースでのパケットのキューイング、最終的にパケットをネクストホップルータへのリンクに送るという処理が含まれる。

- 分散転送機能を備えたルータでは、各インタフェースプロセッサまたはラインカードが独立したForwarding Engingを持ち、ルータ上の送信元インタフェースから宛先インタフェースへのパケット処理とスイッチングを高速化するように最適されている
- IPルーティングの機能は、ルート計算用のRouting Engineを備えたRoute Processorで実行される
- Route Processorは他のルータと対話し、ルーティング情報を収集・処理し、ルーティングテーブルを構築するためのルーティングプロトコルを実行、インタフェースプロセッサと情報共有する
- 全てのパケットをパケットプロセッサに誘導し、転送を行う集中型アーキテクチャのルータもあり、そのハイブリットもある

- ドメイン間ルーティングでBGPが採用される理由
  - グローバルインターネットルーティングテーブルのような多数の経路を扱うことが可能
  - ドメイン間のルーティング情報を制御するためのより複雑なポリシー構成（ルートフィルタリングやタグ付けなど）をサポート
  - ネットワークの変化に適度に速く反応（更新の送受信及び代替ルートの選択）

- パケットスイッチングメカニズム
  - パケット転送は、専用のハードウェアベースのForwarding Engineによって行われ、ASICに実装されている
  - 高い転送速度を実現するには、ASICベースのForwarding Engineが必要
  - シスコは、比較的低速なCPUベースのRoute Processorにおける検索プロセスの高速化のためにあらゆる手法を開発してきた
    1. Process Switching
       - パケットはRoute Processorにキューイングされ、CPUにより順番に処理される
       - 低速でCPUに多くの負荷をかける
    2. Fast Switching
       - キャッシュを作成し、そのキャッシュに存在しない場合のみProcess Switchingを実行
       - 特定の宛先の最初のパケットは必ずProcess Switchingされる
    3. CEF (Cisco Express Forwarding)
       - ルーティングテーブル内のすべてのエントリについて、事前にパケット転送に必要な情報を作成しておく
       - 2つのデータベースを作成し、すべてハードウェア転送を行うため、高速。
         - FIB (Forwarding Information Base): ルルーティングテーブルから作成。ネクストホップIPを管理。
         - Adjacency Database: ARPにより取得。L2ネクストホップ情報(MACアドレスやインタフェース)を管理。

        
## 2. IS-ISルーティングプロトコルの概要
- 国際標準化機構（ISO）(現在は国際電気通信連合(ITU))が、７層のOSI参照モデルを定義
- 異なるメーカの通信機器間の相互接続性と相互運用性を可能にするためのオープンスタンダードを開発するためのアーキテクチャフレームワーク
- OSI参照モデルでは２つのネットワークサービスが定義されている
  1. CONS (Connection-Oriented Network Services)
     - エンドツーエンドのパスを事前定義して設定
  2. CLNS (Connectionless Network Services)
     - 送信元と宛先間の最適パスに従ってルータによって個別に転送
     - 下記のISOプロトコルによってサポート
       - CLNP (Connectionless Network Protocol)
       - ES-IS (End System-to-Intermediate System)
       - IS-IS (Intermediate System-to-Intermediate System)
    - これらはOSI参照モデルの第三層(Network Layer)に共存し、Initial Protocol Identifer(IPI)フィールドの値によって区別される
    - 

        

