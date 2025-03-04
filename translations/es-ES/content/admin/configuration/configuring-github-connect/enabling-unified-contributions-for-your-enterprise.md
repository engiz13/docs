---
title: Habilitar las contribuciones unificadas para tu empresa
shortTitle: Unified contributions
intro: 'Puedes permitir que los usuarios incluyan conteos de contribuciones anonimizados para su trabajo de {% data variables.product.product_location %} en su gráfica de contribuciones de {% data variables.product.prodname_dotcom_the_website %}.'
redirect_from:
  - /enterprise/admin/guides/developer-workflow/enabling-unified-contributions-between-github-enterprise-and-github-com
  - /enterprise/admin/guides/developer-workflow/enabling-unified-contributions-between-github-enterprise-server-and-github-com
  - /enterprise/admin/developer-workflow/enabling-unified-contributions-between-github-enterprise-server-and-githubcom
  - /enterprise/admin/installation/enabling-unified-contributions-between-github-enterprise-server-and-githubcom
  - /enterprise/admin/configuration/enabling-unified-contributions-between-github-enterprise-server-and-githubcom
  - /admin/configuration/enabling-unified-contributions-between-github-enterprise-server-and-githubcom
  - /admin/configuration/managing-connections-between-github-enterprise-server-and-github-enterprise-cloud/enabling-unified-contributions-between-github-enterprise-server-and-githubcom
  - /admin/configuration/managing-connections-between-your-enterprise-accounts/enabling-unified-contributions-between-your-enterprise-account-and-githubcom
permissions: 'Enterprise owners can enable unified contributions between {% data variables.product.product_location %} and {% data variables.product.prodname_dotcom_the_website %}.'
versions:
  ghes: '*'
  ghae: '*'
type: how_to
topics:
  - Enterprise
  - GitHub Connect
ms.openlocfilehash: af07f30a8f164f6bec3d3c0f44c77181f1e8db7b
ms.sourcegitcommit: 47bd0e48c7dba1dde49baff60bc1eddc91ab10c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2022
ms.locfileid: '145120809'
---
{% data reusables.github-connect.beta %}

## Acerca de las contribuciones unificadas

Como propietario de empresa, puedes permitir que los usuarios finales envíen cuentas de contribuciones anonimizadas para su trabajo desde {% data variables.product.product_location %} hacia su gráfica de contribuciones de {% data variables.product.prodname_dotcom_the_website %}.

Después de habilitar las {% data variables.product.prodname_unified_contributions %}, para que un usuario pueda enviar recuentos de contribuciones de {% data variables.product.product_location %} a {% data variables.product.prodname_dotcom_the_website %}, deberá conectar también su cuenta de usuario de {% data variables.product.product_name %} con una cuenta personal de {% data variables.product.prodname_dotcom_the_website %}. Para obtener más información, consulta "[Enviar contribuciones empresariales a tu perfil de {% data variables.product.prodname_dotcom_the_website %}](/account-and-profile/setting-up-and-managing-your-github-profile/managing-contribution-graphs-on-your-profile/sending-enterprise-contributions-to-your-githubcom-profile)".

{% data reusables.github-connect.sync-frequency %}

Si el propietario de la empresa inhabilita la funcionalidad o si los usuarios individuales deciden no participar en la conexión, el conteo de contribuciones de {% data variables.product.product_name %} se borrará en {% data variables.product.prodname_dotcom_the_website %}. Si el usuario reconecta sus perfiles después de haberlos inhabilitado, el conteo de contribuciones de los 90 días anteriores se restablecerá.

{% data variables.product.product_name %} **solo** envía el número de contribuciones y el origen ({% data variables.product.product_name %}) de los usuarios conectados. No envía ningún tipo de información sobre la contribución o cómo se realizó.

## Habilitar las contribuciones unificadas

Antes de habilitar las {% data variables.product.prodname_unified_contributions %} en {% data variables.product.product_location %}, primero debes habilitar {% data variables.product.prodname_github_connect %}. Para obtener más información, consulta "[Administración de {% data variables.product.prodname_github_connect %}](/admin/configuration/configuring-github-connect/managing-github-connect)".

{% ifversion ghes %} {% data reusables.github-connect.access-dotcom-and-enterprise %} {% data reusables.enterprise_site_admin_settings.access-settings %} {% data reusables.enterprise_site_admin_settings.business %} {% data reusables.enterprise-accounts.github-connect-tab %}{% else %}
1. Inicia sesión en {% data variables.product.product_location %} y {% data variables.product.prodname_dotcom_the_website %}.
{% data reusables.enterprise-accounts.access-enterprise %}{% data reusables.enterprise-accounts.github-connect-tab %}{% endif %}
1. En "Los usuarios pueden compartir recuentos de contribuciones en {% data variables.product.prodname_dotcom_the_website %}", haz clic en **Solicitar acceso**.
  ![Opción para solicitar acceso a contribuciones unificadas](/assets/images/enterprise/site-admin-settings/dotcom-ghe-connection-request-access.png){% ifversion ghes %}
2. [Inicia sesión](https://enterprise.github.com/login) en el sitio de {% data variables.product.prodname_ghe_server %} para recibir instrucciones adicionales.

Cuando solicitas acceso, podríamos redireccionarte al sitio de {% data variables.product.prodname_ghe_server %} para verificar tus condiciones de servicio actuales.
{% endif %}
