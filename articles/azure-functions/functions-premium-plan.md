---
title: Azure Functions plano Premium
description: Detalhes e opções de configuração (VNet, sem início frio, duração de execução ilimitada) para o plano Azure Functions Premium.
author: jeffhollan
ms.topic: conceptual
ms.date: 08/28/2020
ms.author: jehollan
ms.custom: references_regions
ms.openlocfilehash: 4f6e2008cad66ce7cd68016d3873ecbc18b1961c
ms.sourcegitcommit: d7352c07708180a9293e8a0e7020b9dd3dd153ce
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/30/2020
ms.locfileid: "89145735"
---
# <a name="azure-functions-premium-plan"></a>Azure Functions plano Premium

O plano Azure Functions Premium (às vezes chamado de plano Premium elástico) é uma opção de hospedagem para aplicativos de funções. O plano Premium fornece recursos como conectividade VNet, sem início frio e hardware Premium.  Vários aplicativos de funções podem ser implantados no mesmo plano Premium e o plano permite que você configure o tamanho da instância de computação, o tamanho do plano base e o tamanho máximo do plano.  Para obter uma comparação do plano Premium e outros tipos de plano e hospedagem, consulte [Opções de escala e Hospedagem de função](functions-scale.md).

## <a name="create-a-premium-plan"></a>Criar um plano Premium

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]

Você também pode criar um plano Premium usando [AZ functionapp Plan Create](/cli/azure/functionapp/plan#az-functionapp-plan-create) na CLI do Azure. O exemplo a seguir cria um plano de camada _Premium 1 elástico_ :

```azurecli-interactive
az functionapp plan create --resource-group <RESOURCE_GROUP> --name <PLAN_NAME> \
--location <REGION> --sku EP1
```

Neste exemplo, substitua `<RESOURCE_GROUP>` pelo seu grupo de recursos e `<PLAN_NAME>` por um nome para seu plano que seja exclusivo no grupo de recursos. Especifique um [suporte `<REGION>` ](https://azure.microsoft.com/global-infrastructure/services/?products=functions). Para criar um plano Premium que ofereça suporte ao Linux, inclua a `--is-linux` opção.

Com o plano criado, você pode usar [AZ functionapp Create](/cli/azure/functionapp#az-functionapp-create) para criar seu aplicativo de funções. No portal, o plano e o aplicativo são criados ao mesmo tempo. Para obter um exemplo de um script de CLI do Azure completo, consulte [criar um aplicativo de funções em um plano Premium](scripts/functions-cli-create-premium-plan.md).

## <a name="features"></a>Recursos

Os recursos a seguir estão disponíveis para aplicativos de funções implantados em um plano Premium.

### <a name="always-ready-instances"></a>Instâncias sempre prontas

Se nenhum evento e execução ocorrer hoje no plano de consumo, seu aplicativo poderá ser dimensionado para uma instância zero. Quando novos eventos chegam, uma nova instância precisa ser especializada em seu aplicativo em execução.  A especialização de novas instâncias pode levar algum tempo, dependendo do aplicativo.  Essa latência adicional na primeira chamada geralmente é chamada de inicialização a frio do aplicativo.

No plano Premium, você pode ter seu aplicativo sempre pronto em um número especificado de instâncias.  O número máximo de instâncias sempre prontas é 20.  Quando os eventos começam a disparar o aplicativo, eles são roteados para as instâncias do Always Ready primeiro.  À medida que a função se torna ativa, as instâncias adicionais serão quentes como um buffer.  Esse buffer impede a inicialização a frio para novas instâncias necessárias durante a escala.  Essas instâncias em buffer são chamadas de [instâncias pré-configuradas](#pre-warmed-instances).  Com a combinação das instâncias Always Ready e um buffer pré-configurado, seu aplicativo pode efetivamente eliminar a inicialização a frio.

> [!NOTE]
> Todo plano Premium terá pelo menos uma instância ativa e cobrada em todos os momentos.

Você pode configurar o número de instâncias do Always Ready no portal do Azure selecionando sua **aplicativo de funções**, acessando a guia **recursos da plataforma** e selecionando as opções de **scale out** . Na janela Editar do aplicativo de funções, as instâncias sempre prontas são específicas para esse aplicativo.

![Configurações de escala elástica](./media/functions-premium-plan/scale-out.png)

Você também pode configurar instâncias do Always Ready para um aplicativo com o CLI do Azure.

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.minimumElasticInstanceCount=<desired_always_ready_count> --resource-type Microsoft.Web/sites 
```

#### <a name="pre-warmed-instances"></a>Instâncias pré-passivas

Instâncias pré-configuradas são o número de instâncias que são ativadas como um buffer durante os eventos de escala e ativação.  As instâncias pré-configuradas continuam no buffer até que o limite máximo de expansão seja atingido.  A contagem de instâncias pré-passiva padrão é 1 e, para a maioria dos cenários, deve permanecer como 1.  Se um aplicativo tiver um longo tempo de aquecimento (como uma imagem de contêiner personalizada), talvez você queira aumentar esse buffer.  Uma instância pré-configurada se tornará ativa somente depois que todas as instâncias ativas estiverem suficientemente utilizadas.

Considere este exemplo de como as instâncias do Always Ready e as instâncias pré-configuradas funcionam juntas.  Um aplicativo de funções Premium tem cinco instâncias sempre prontas configuradas e o padrão de uma instância pré-configurada.  Quando o aplicativo estiver ocioso e nenhum evento estiver sendo disparado, o aplicativo será provisionado e executado em cinco instâncias.  

Assim que o primeiro gatilho chega, as cinco instâncias sempre prontas se tornam ativas e uma instância pré-configurada adicional é alocada.  Agora, o aplicativo está em execução com seis instâncias provisionadas: as cinco instâncias do Always Ready estão ativas e o sexto buffer pré-configurado e inativo.  Se a taxa de execuções continuar aumentando, as cinco instâncias ativas serão utilizadas eventualmente.  Quando a plataforma decidir Dimensionar para além de cinco instâncias, ela será dimensionada para a instância pré-configurada.  Quando isso acontecer, agora haverá seis instâncias ativas, e uma sétima instância será instantaneamente provisionada e preencherá o buffer pré-configurado.  Essa sequência de dimensionamento e pré-aquecimento continuará até que a contagem máxima de instâncias do aplicativo seja atingida.  Nenhuma instância será pré-configurada ou ativada além do máximo.

Você pode modificar o número de instâncias pré-configuradas para um aplicativo usando o CLI do Azure.

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.preWarmedInstanceCount=<desired_prewarmed_count> --resource-type Microsoft.Web/sites 
```

#### <a name="maximum-instances-for-an-app"></a>Máximo de instâncias para um aplicativo

Além da [contagem máxima de instâncias do plano](#plan-and-sku-settings), você pode configurar um máximo por aplicativo.  O máximo do aplicativo pode ser configurado usando o [limite de escala do aplicativo](./functions-scale.md#limit-scale-out).

### <a name="private-network-connectivity"></a>Conectividade de rede privada

Azure Functions implantadas em um plano Premium aproveita a [nova integração de VNet para aplicativos Web](../app-service/web-sites-integrate-with-vnet.md).  Quando configurado, seu aplicativo pode se comunicar com recursos em sua VNet ou protegidos por meio de pontos de extremidade de serviço.  As restrições de IP também estão disponíveis no aplicativo para restringir o tráfego de entrada.

Ao atribuir uma sub-rede ao seu aplicativo de funções em um plano Premium, você precisa de uma sub-rede com endereços IP suficientes para cada instância em potencial. Exigimos um bloco de IP com pelo menos 100 endereços disponíveis.

Para obter mais informações, consulte [integrar seu aplicativo de funções a uma VNet](functions-create-vnet.md).

### <a name="rapid-elastic-scale"></a>Escala elástica rápida

Instâncias de computação adicionais são adicionadas automaticamente para seu aplicativo usando a mesma lógica de dimensionamento rápido que o plano de consumo. Os aplicativos no mesmo plano do serviço de aplicativo são dimensionados independentemente um do outro com base nas necessidades de um aplicativo individual. No entanto, os aplicativos do Functions no mesmo plano do serviço de aplicativo compartilham recursos da VM para ajudar a reduzir os custos, quando possível. O número de aplicativos associados a uma VM depende da superfície de cada aplicativo e do tamanho da VM.

Para saber mais sobre como o dimensionamento funciona, consulte [escala de funções e hospedagem](./functions-scale.md#how-the-consumption-and-premium-plans-work).

### <a name="longer-run-duration"></a>Duração de execução mais longa

Azure Functions em um plano de consumo são limitados a 10 minutos para uma única execução.  No plano Premium, o padrão de duração da execução é de 30 minutos para evitar execuções de fuga. No entanto, você pode [Modificar o host.jsna configuração](./functions-host-json.md#functiontimeout) para tornar a duração desassociada para aplicativos de plano Premium (garantido 60 minutos).

## <a name="plan-and-sku-settings"></a>Configurações de plano e SKU

Quando você cria o plano, há duas configurações de tamanho de plano: o número mínimo de instâncias (ou o tamanho do plano) e o limite máximo de intermitência.

Se seu aplicativo exigir instâncias além das instâncias do Always Ready, ele poderá continuar a escalar horizontalmente até que o número de instâncias atinja o limite máximo de intermitência.  Você será cobrado por instâncias além do tamanho do plano somente enquanto eles estiverem em execução e alugados para você.  Faremos um melhor esforço em dimensionar seu aplicativo para o limite máximo definido.

Você pode configurar o tamanho do plano e os máximos no portal do Azure selecionando as opções de **scale out** no plano ou um aplicativo de funções implantado nesse plano (em **recursos da plataforma**).

Você também pode aumentar o limite máximo de intermitência do CLI do Azure:

```azurecli-interactive
az resource update -g <resource_group> -n <premium_plan_name> --set properties.maximumElasticWorkerCount=<desired_max_burst> --resource-type Microsoft.Web/serverfarms 
```

O mínimo para cada plano será pelo menos uma instância.  O número mínimo real de instâncias será configurado para você com base nas instâncias do Always Ready solicitadas por aplicativos no plano.  Por exemplo, se o aplicativo A solicitar cinco instâncias sempre prontas e o aplicativo B solicitar duas instâncias do Always Ready no mesmo plano, o tamanho mínimo do plano será calculado como cinco.  O aplicativo A será executado em todos os 5, e o aplicativo B só será executado em 2.

> [!IMPORTANT]
> Você é cobrado por cada instância alocada na contagem mínima de instâncias, independentemente de as funções serem executadas ou não.

Na maioria das circunstâncias, o mínimo autocalculado deve ser suficiente.  No entanto, o dimensionamento além do mínimo ocorre com um melhor esforço.  É possível, embora improvável, que, em uma escala horizontal de tempo horizontal, possa ser atrasada se instâncias adicionais não estiverem disponíveis.  Ao definir um mínimo maior que o mínimo autocalculado, você reserva instâncias com antecedência da escala horizontal.

O aumento do mínimo calculado para um plano pode ser feito usando o CLI do Azure.

```azurecli-interactive
az resource update -g <resource_group> -n <premium_plan_name> --set sku.capacity=<desired_min_instances> --resource-type Microsoft.Web/serverfarms 
```

### <a name="available-instance-skus"></a>SKUs da instância disponível

Ao criar ou dimensionar seu plano, você pode escolher entre três tamanhos de instância.  Você será cobrado pelo número total de núcleos e memória consumida por segundo.  Seu aplicativo pode ser dimensionado automaticamente para várias instâncias, conforme necessário.  

|SKU|Núcleos|Memória|Armazenamento|
|--|--|--|--|
|EP1|1|3,5 GB|250GB|
|EP2|2|7 GB|250GB|
|EP3|4|14 GB|250GB|

### <a name="memory-utilization-considerations"></a>Considerações sobre utilização de memória
A execução em um computador com mais memória nem sempre significa que seu aplicativo de funções usará toda a memória disponível.

Por exemplo, um aplicativo de funções JavaScript é restrito pelo limite de memória padrão em Node.js. Para aumentar esse limite de memória fixa, adicione a configuração `languageWorkers:node:arguments` do aplicativo com um valor de `--max-old-space-size=<max memory in MB>` .

## <a name="region-max-scale-out"></a>Scale Out máxima da região

Abaixo estão os valores máximos de expansão com suporte no momento para um único plano em cada região e configuração de so. Para solicitar um aumento, abra um tíquete de suporte.

Veja a disponibilidade regional completa das funções aqui: [Azure.com](https://azure.microsoft.com/global-infrastructure/services/?products=functions)

|Região| Windows | Linux |
|--| -- | -- |
|Austrália Central| 20 | Não disponível |
|Austrália Central 2| 20 | Não disponível |
|Leste da Austrália| 100 | 20 |
|Australia Southeast | 100 | 20 |
|Brazil South| 60 | 20 |
|Canadá Central| 100 | 20 |
|Centro dos EUA| 100 | 20 |
|Leste da Ásia| 100 | 20 |
|Leste dos EUA | 100 | 20 |
|Leste dos EUA 2| 100 | 20 |
|França Central| 100 | 20 |
|Centro-Oeste da Alemanha| 100 | Não disponível |
|Japan East| 100 | 20 |
|Oeste do Japão| 100 | 20 |
|Coreia Central| 100 | 20 |
|Centro-Norte dos EUA| 100 | 20 |
|Norte da Europa| 100 | 20 |
|Leste da Noruega| 20 | 20 |
|Centro-Sul dos Estados Unidos| 100 | 20 |
|Sul da Índia | 100 | Não disponível |
|Sudeste Asiático| 100 | 20 |
|Sul do Reino Unido| 100 | 20 |
|Oeste do Reino Unido| 100 | 20 |
|Europa Ocidental| 100 | 20 |
|Oeste da Índia| 100 | 20 |
|Centro-Oeste dos EUA| 20 | 20 |
|Oeste dos EUA| 100 | 20 |
|Oeste dos EUA 2| 100 | 20 |

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Entender Azure Functions escala e opções de hospedagem](functions-scale.md)
