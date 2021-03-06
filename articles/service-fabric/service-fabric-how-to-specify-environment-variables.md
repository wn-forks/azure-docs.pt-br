---
title: Especificar variáveis de ambiente para serviços
description: Mostra como usar variáveis de ambiente para aplicativos no Service Fabric
author: mikkelhegn
ms.topic: conceptual
ms.date: 12/06/2017
ms.author: mikhegn
ms.openlocfilehash: f4c4f2a1c140e3d0f181c4fd55482056f9f91b62
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75614308"
---
# <a name="how-to-specify-environment-variables-for-services-in-service-fabric"></a>Como especificar variáveis de ambiente para serviços no Service Fabric

Este artigo mostra como especificar variáveis de ambiente para um serviço ou contêiner no Service Fabric.

## <a name="procedure-for-specifying-environment-variables-for-services"></a>Procedimento para especificar variáveis de ambiente para serviços

Neste exemplo, você pode definir uma variável de ambiente para um contêiner. O artigo supõe que você já tem um manifesto de aplicativo e serviço.

1. Abra o arquivo ServiceManifest.xml.
2. No elemento `CodePackage`, adicione um novo elemento `EnvironmentVariables` e um elemento `EnvironmentVariable` para cada variável de ambiente.

    ```xml
    <CodePackage Name="MyCode" Version="CodeVersion1">
            ...
            <EnvironmentVariables>
                  <EnvironmentVariable Name="MyEnvVariable" Value="DefaultValue"/>
                  <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentVariables>
    </CodePackage>
    ```

   As variáveis de ambiente podem ser substituídas no manifesto do aplicativo.

3. Para substituir essas variáveis de ambiente no manifesto do aplicativo, use o elemento `EnvironmentOverrides`.

    ```xml
      <ServiceManifestImport>
        <ServiceManifestVersion="1.0.0" />
        <EnvironmentOverrides CodePackageRef="MyCode">
          <EnvironmentVariable Name="MyEnvVariable" Value="OverrideValue"/>
        </EnvironmentOverrides>
      </ServiceManifestImport>
    ```

## <a name="specifying-environment-variables-dynamically-using-docker-compose"></a>Especificando variáveis de ambiente dinamicamente usando Docker Compose

Service Fabric dá suporte à capacidade de [usar Docker Compose para implantação](service-fabric-docker-compose.md#supported-compose-directives). Os arquivos de composição podem ser variáveis de ambiente de origem do Shell. Esse comportamento pode ser usado para substituir dinamicamente os valores de ambiente desejados:

```yml
environment:
  - "hostname:${hostname}"
```

## <a name="next-steps"></a>Próximas etapas
Para saber mais sobre alguns dos principais conceitos que são discutidos neste artigo, veja os artigos sobre [Gerenciar aplicativos de vários ambientes](service-fabric-manage-multiple-environment-app-configuration.md).

Para obter informações sobre outras funcionalidades de gerenciamento de aplicativo disponíveis no Visual Studio, confira [Gerenciar seus aplicativos do Service Fabric no Visual Studio](service-fabric-manage-application-in-visual-studio.md).
