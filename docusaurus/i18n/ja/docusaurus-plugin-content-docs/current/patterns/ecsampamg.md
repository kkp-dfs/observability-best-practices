# ECS ワークロードのモニタリング
<!--with ADOT, AWS X-Ray, and Amazon Managed Service for Prometheus-->




## はじめに

コンテナ化されたアプリケーションの世界では、信頼性とパフォーマンスを維持するために効果的な監視が不可欠です。
このドキュメントでは、AWS Distro for OpenTelemetry (ADOT)、AWS X-Ray、Amazon Managed Service for Prometheus を活用した Amazon Elastic Container Service (ECS) ワークロードの高度な監視ソリューションについて説明します。



## アーキテクチャの概要

監視アーキテクチャの中心は、アプリケーションと ADOT コレクターの両方をホストする ECS タスクです。このセットアップにより、アプリケーション環境から直接、包括的なデータ収集が可能になります。

![ECS AMP](./images/ecs.png)
*図 1：ECS から AMP と X-Ray にメトリクスを送信する*



## 主要コンポーネント




### ECS タスク
ECS タスクは基本的な単位として機能し、アプリケーションとモニタリングコンポーネントをカプセル化します。




### サンプルアプリケーション
コンテナ化されたアプリケーションが ECS タスク内で実行され、監視対象のワークロードを表しています。




### AWS Distro for OpenTelemetry (ADOT) Collector
ADOT コレクターはアプリケーションと共にデプロイされ、テレメトリデータの中央集約ポイントとして機能します。アプリケーションからメトリクスとトレースの両方を収集します。




### AWS X-Ray
X-Ray は ADOT コレクターからトレースデータを受け取り、リクエストのフローとサービスの依存関係に関する詳細な洞察を提供します。




### Amazon Managed Service for Prometheus
このサービスは、ADOT コレクターによって収集されたメトリクスを保存および管理し、メトリクスの保存とクエリのためのスケーラブルなソリューションを提供します。




## データフロー

1. サンプルアプリケーションは、動作中にテレメトリデータを生成します。
2. 同じ ECS タスクで実行されている ADOT コレクターが、アプリケーションからこのデータを収集します。
3. トレースデータは、分散トレース分析のために AWS X-Ray に転送されます。
4. メトリクスは、保存と後の分析のために Amazon Managed Service for Prometheus に送信されます。




## 利点

- **包括的なモニタリング**: メトリクスとトレースの両方を収集し、アプリケーションのパフォーマンスを総合的に把握できます。
- **スケーラビリティ**: マネージドサービスを活用して、大量のテレメトリデータを処理します。
- **統合**: ECS や他の AWS サービスとシームレスに連携します。
- **運用オーバーヘッドの削減**: マネージドサービスを利用することで、インフラストラクチャ管理の必要性を最小限に抑えます。




## 実装時の考慮事項

- ECS タスクが X-Ray と Prometheus にデータを送信できるよう、適切な IAM ロールと権限を設定する必要があります。
- ECS タスク内のリソース割り当ては、アプリケーションと ADOT コレクターの両方を考慮する必要があります。
- 完全なオブザーバビリティソリューションを実現するために、メトリクスとトレースに加えてログ収集の実装を検討してください。




## まとめ

このアーキテクチャは、OpenTelemetry の機能と AWS マネージドサービスを組み合わせることで、ECS ワークロードに対する堅牢な監視ソリューションを提供します。
アプリケーションのパフォーマンスと動作に関する深い洞察を可能にし、コンテナ化された環境での迅速な問題解決と情報に基づいた意思決定を促進します。
