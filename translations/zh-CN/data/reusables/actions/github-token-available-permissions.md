---
ms.openlocfilehash: 56ed7762c2325d0328bd52ca89fe7879b5ce4601
ms.sourcegitcommit: 47bd0e48c7dba1dde49baff60bc1eddc91ab10c5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2022
ms.locfileid: "145084941"
---
可用的作用域和访问权限值：

```yaml
permissions:
  actions: read|write|none
  checks: read|write|none
  contents: read|write|none
  deployments: read|write|none{% ifversion fpt or ghec %}
  id-token: read|write|none{% endif %}
  issues: read|write|none
  discussions: read|write|none
  packages: read|write|none
  pages: read|write|none
  pull-requests: read|write|none
  repository-projects: read|write|none
  security-events: read|write|none
  statuses: read|write|none
```

如果你指定其中任何作用域的访问权限，则所有未指定的作用域都被设置为 `none`。

您可以使用以下语法来定义所有可用作用域的读取或写入权限：

```yaml
permissions: read-all|write-all
```

可以使用以下语法禁用所有可用作用域的权限：

```yaml
permissions: {}
```
