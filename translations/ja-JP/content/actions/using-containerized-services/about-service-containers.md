---
title: サービスコンテナについて
intro: サービスコンテナを使って、データベース、Webサービス、メモリキャッシュ、あるいはその他のツールをワークフローに接続できます。
redirect_from:
  - /actions/automating-your-workflow-with-github-actions/about-service-containers
  - /actions/configuring-and-managing-workflows/about-service-containers
  - /actions/guides/about-service-containers
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
type: overview
topics:
  - Containers
  - Docker
ms.openlocfilehash: 67bfb403bb18f7364e000170ce71f9387d4ada69
ms.sourcegitcommit: 47bd0e48c7dba1dde49baff60bc1eddc91ab10c5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/05/2022
ms.locfileid: '145121125'
---
{% data reusables.actions.enterprise-beta %} {% data reusables.actions.enterprise-github-hosted-runners %}

## サービスコンテナについて

サービスコンテナは、ワークフロー中でアプリケーションをテストもしくは運用するのに必要になるかもしれないサービスをホストするための、シンプルでポータブルな方法を提供するDockerコンテナです。 たとえば、ワークフローでデータベースやメモリキャッシュへのアクセスを必要とする結合テストを実行する必要があるかもしれません。

サービスコンテナは、ワークフロー中のそれぞれのジョブに対して設定できます。 {% data variables.product.prodname_dotcom %}は新しいDockerコンテナをワークフロー中で設定された各サービスに対して作成し、ジョブが完了したときにそのサービスコンテナを破棄します。 ジョブ中のステップは、同じジョブの一部であるすべてのサービスコンテナと通信できます。 ただし、複合アクション内でサービスコンテナーを作成して使用することはできません。 

{% data reusables.actions.docker-container-os-support %}

## サービスコンテナとの通信

ワークフロー中のジョブは、直接ランナーマシン上で実行するようにも、Dockerコンテナ中で実行するようにも設定できます。 ジョブと、ジョブのサービスコンテナとの通信は、ジョブがランナーマシン上で直接実行されているか、コンテナ内で実行されているかによって異なります。

### コンテナ内でのジョブの実行

コンテナ内でジョブを実行する場合、{% data variables.product.prodname_dotcom %}はDockerのユーザー定義ブリッジネットワークを使ってサービスコンテナをジョブに接続します。 詳しくは、Docker ドキュメントの「[ブリッジ ネットワークを使用する](https://docs.docker.com/network/bridge/)」を参照してください。

コンテナ内でジョブとサービスを実行すれば、ネットワークアクセスはシンプルになります。 サービスコンテナへは、ワークフロー中で設定したラベルを使ってアクセスできます。 サービスコンテナのホスト名は、自動的にラベル名にマップされます。 たとえば、`redis` というラベルでサービスコンテナを作成したなら、そのサービスコンテナのホスト名は `redis` になります。

サービスコンテナでポートを設定する必要はありません。 デフォルトで、すべてのコンテナは同じDockerネットワークの一部となってお互いにすべてのポートを公開し合い、Dockerネットワークの外部へはポートは公開されません。

### ランナーマシン上でのジョブの実行

ランナーマシン上でジョブを直接実行する場合、`localhost:<port>` か `127.0.0.1:<port>` を使ってサービスコンテナにアクセスできます。 {% data variables.product.prodname_dotcom %}は、サービスコンテナからDockerホストへの通信を可能にするよう、コンテナネットワークを設定します。

ジョブがランナーマシン上で直接実行されている場合、Dockerコンテナ内で実行されているサービスは、ランナー上で実行しているジョブに対してデフォルトではポートを公開しません。 サービスコンテナ上のポートは、Dockerホストに対してマップする必要があります。 詳しくは、「[Docker ホストとサービスコンテナポートのマッピング](/actions/automating-your-workflow-with-github-actions/about-service-containers#mapping-docker-host-and-service-container-ports)」を参照してください。

## サービスコンテナの作成

`services` キーワードを使って、ワークフロー内のジョブの一部であるサービスコンテナを作成できます。 詳細については、「[`jobs.<job_id>.services`](/actions/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#jobsjob_idservices)」を参照してください。

この例では、`container-job` という名前のジョブで `redis` という名前のサービスが作成されます。 この例の Docker ホストは `node:16-bullseye` コンテナです。

{% raw %}
```yaml{:copy}
name: Redis container example
on: push

jobs:
  # Label of the container job
  container-job:
    # Containers must run in Linux based operating systems
    runs-on: ubuntu-latest
    # Docker Hub image that `container-job` executes in
    container: node:16-bullseye

    # Service containers to run with `container-job`
    services:
      # Label used to access the service container
      redis:
        # Docker Hub image
        image: redis
```
{% endraw %}

## Dockerホストとサービスコンテナのポートのマッピング

ジョブがDockerコンテナ内で実行されるなら、ポートをホストあるいはサービスコンテナにマップする必要はありません。 ジョブがランナーマシン上で直接実行されるなら、必要なサービスコンテナのポートはホストランナーマシンのポートにマップしなければなりません。

サービスコンテナのポートは、`ports` キーワードを使って Docker ホストにマップできます。 詳細については、「[`jobs.<job_id>.services`](/actions/automating-your-workflow-with-github-actions/workflow-syntax-for-github-actions#jobsjob_idservices)」を参照してください。

| `ports` の値 |    説明 |
|------------------|--------------|
| `8080:80` |   コンテナのTCPのポート80をDockerホストのポート8080にマップします。 |
| `8080:80/udp` |   コンテナのUDPポート80をDockerホストのポート8080にマップします。 |
| `8080/udp`    | コンテナでランダムに選択したUDPポートをDockerホストのUDPポート8080にマップします。 |

`ports` キーワードを使ってポートをマップする場合、{% data variables.product.prodname_dotcom %} は `--publish` コマンドを使ってコンテナのポートを Docker ホストに公開します。 詳しくは、Docker ドキュメントの「[Docker コンテナネットワーク](https://docs.docker.com/config/containers/container-networking/)」を参照してください。

Dockerホストのポートを指定して、コンテナのポートを指定しなかった場合、コンテナのポートは空いているポートにランダムに割り当てられます。 {% data variables.product.prodname_dotcom %}は割り当てられたコンテナのポートをサービスコンテナのコンテキストに設定します。 たとえば `redis` サービスコンテナに対し、Docker ホストのポート 5432 を設定したなら、対応するコンテナのポートには `job.services.redis.ports[5432]` コンテキストを使ってアクセスできます。 詳細については、「[コンテキスト](/actions/learn-github-actions/contexts#job-context)」を参照してください。

### Redisのポートのマッピングの例

以下の例は、サービスコンテナ `redis` のポート 6379 を、Docker ホストのポート 6379 にマップします。

{% raw %}
```yaml{:copy}
name: Redis Service Example
on: push

jobs:
  # Label of the container job
  runner-job:
    # You must use a Linux environment when using service containers or container jobs
    runs-on: ubuntu-latest

    # Service containers to run with `runner-job`
    services:
      # Label used to access the service container
      redis:
        # Docker Hub image
        image: redis
        #
        ports:
          # Opens tcp port 6379 on the host and service container
          - 6379:6379
```
{% endraw %}

## 参考資料

- 「[Redis サービス コンテナの作成](/actions/automating-your-workflow-with-github-actions/creating-redis-service-containers)」
- 「[PostgreSQL サービスコンテナの作成](/actions/automating-your-workflow-with-github-actions/creating-postgresql-service-containers)」
