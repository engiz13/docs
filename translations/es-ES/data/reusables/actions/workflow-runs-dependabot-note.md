---
ms.openlocfilehash: 6d101895af1ae0e202ebfb49119c83a14682de09
ms.sourcegitcommit: 47bd0e48c7dba1dde49baff60bc1eddc91ab10c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2022
ms.locfileid: "145093051"
---
{% ifversion fpt or ghec %} {% note %}

**Nota:** Las ejecuciones de flujos de trabajo que las solicitudes de incorporación de cambios de {% data variables.product.prodname_dependabot %} desencadenan se ejecutan como si fueran de un repositorio bifurcado y, por lo tanto, usan un `GITHUB_TOKEN` de solo lectura. Estas ejecuciones de flujo de trabajo no pueden acceder a ningún secreto. Consulta ["Mantenimiento de la seguridad de los flujos de trabajo y Acciones de GitHub: prevención de solicitudes pwn"](https://securitylab.github.com/research/github-actions-preventing-pwn-requests) para obtener estrategias destinadas a proteger estos flujos de trabajo.

{% endnote %} {% endif %}
