---
title: Issue にラベルを追加する
intro: '{% data variables.product.prodname_actions %} を使用して、Issue に自動的にラベルを付けることができます。'
redirect_from:
  - /actions/guides/adding-labels-to-issues
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
type: tutorial
topics:
  - Workflows
  - Project management
ms.openlocfilehash: 8e80990a1a533ed303f47cbad8dafb95c890893d
ms.sourcegitcommit: 47bd0e48c7dba1dde49baff60bc1eddc91ab10c5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/05/2022
ms.locfileid: '147410436'
---
{% data reusables.actions.enterprise-beta %} {% data reusables.actions.enterprise-github-hosted-runners %}

## はじめに

このチュートリアルでは、ワークフローで [`andymckay/labeler` アクション](https://github.com/marketplace/actions/simple-issue-labeler)を使用して、新しくオープンまたは再オープンした Issue にラベルを付ける方法を示します。 たとえば、Issue をオープンまたは再オープンするたびに `triage` ラベルを追加できます。 次に、`triage` ラベルで Issue をフィルター処理して、トリアージする必要のあるすべての Issue を確認できます。

チュートリアルでは、[`andymckay/labeler` アクション](https://github.com/marketplace/actions/simple-issue-labeler)を使用するワークフロー ファイルをまず作成します。 次に、ニーズに合わせてワークフローをカスタマイズします。

## ワークフローの作成

1. {% data reusables.actions.choose-repo %}
2. {% data reusables.actions.make-workflow-file %}
3. 次の YAML コンテンツをワークフローファイルにコピーします。

    ```yaml{:copy}
{% indented_data_reference reusables.actions.actions-not-certified-by-github-comment spaces=4 %}

{% indented_data_reference reusables.actions.actions-use-sha-pinning-comment spaces=4 %}

    name: Label issues
    on:
      issues:
        types:
          - reopened
          - opened
    jobs:
      label_issues:
        runs-on: ubuntu-latest
        permissions:
          issues: write
        steps:
          - name: Label issues
            uses: andymckay/labeler@e6c4322d0397f3240f0e7e30a33b5c5df2d39e90
            with:
              add-labels: "triage"
              repo-token: {% raw %}${{ secrets.GITHUB_TOKEN }}{% endraw %}
    ```

4. ワークフローファイルのパラメータをカスタマイズします。
   - `add-labels` の値を、Issue に追加するラベルのリストに変更します。 複数のラベルはコンマで区切ります。 たとえば、「 `"help wanted, good first issue"` 」のように入力します。 ラベルの詳細については、「[ラベルを管理する](/github/managing-your-work-on-github/managing-labels#applying-labels-to-issues-and-pull-requests)」を参照してください。
5. {% data reusables.actions.commit-workflow %}

## ワークフローのテスト

リポジトリ内の Issue をオープンするか再オープンするたびに、このワークフローは指定したラベルを Issue に追加します。

リポジトリに Issue を作成して、ワークフローをテストします。

1. リポジトリで Issue を作成します。 詳細については、「[Issue の作成](/github/managing-your-work-on-github/creating-an-issue)」を参照してください。
2. Issue の作成によってトリガーされたワークフローの実行を確認するには、ワークフローの実行履歴を表示します。 詳細については、「[ワークフロー実行の履歴を表示する](/actions/managing-workflow-runs/viewing-workflow-run-history)」を参照してください。
3. ワークフローが完了すると、作成した Issue に指定されたラベルが追加されます。

## 次の手順

- ラベルの削除や、Issue が割り当てられているか特定のラベルがある場合にこのアクションをスキップするなど、`andymckay/labeler` アクションで実行できる追加の機能の詳細については、[`andymckay/labeler` アクションのドキュメント](https://github.com/marketplace/actions/simple-issue-labeler)を参照してください。
- ワークフローをトリガーできるさまざまなイベントの詳細については、「[ワークフローをトリガーするイベント](/actions/reference/events-that-trigger-workflows#issues)」を参照してください。 `andymckay/labeler` アクションは、`issues`、`pull_request`、`project_card` のいずれかのイベントでのみ機能します。
- このアクションを使用するワークフローの例については [GitHub を検索](https://github.com/search?q=%22uses:+andymckay/labeler%22&type=code)してください。
