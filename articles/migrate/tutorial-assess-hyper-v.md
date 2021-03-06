---
title: Avaliar VMs do Hyper-V para migração para o Azure usando as Migrações para Azure | Microsoft Docs
description: Descreve como avaliar as VMs locais do Hyper-V para migração para o Azure usando a Avaliação de Servidor de Migrações para Azure.
ms.topic: tutorial
ms.date: 06/03/2020
ms.custom: mvc
ms.openlocfilehash: 57d91f14b8f3a9f58373cbd43561a03a8546fd8f
ms.sourcegitcommit: 7f62a228b1eeab399d5a300ddb5305f09b80ee14
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89514485"
---
# <a name="assess-hyper-v-vms-with-azure-migrate-server-assessment"></a>Avaliar VMs do Hyper-V usando a avaliação de servidor das Migrações para Azure

Este artigo mostra como avaliar as VMs locais do Hyper-V usando a ferramenta [Migrações para Azure: Avaliação de Servidor](migrate-services-overview.md#azure-migrate-server-assessment-tool).


Este tutorial é o segundo de uma série que demonstra como avaliar e migrar VMs do Hyper-V para o Azure. Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Configurar um projeto das Migrações para Azure.
> * Configurar e registrar um dispositivo das Migrações para Azure.
> * Iniciar a descoberta contínua de VMs locais.
> * Agrupar as VMs descobertas e avaliar o grupo.
> * Examinar a avaliação.

> [!NOTE]
> Os tutoriais mostram o caminho de implantação mais simples para um cenário para que você possa configurar rapidamente uma prova de conceito. Os tutoriais usam opções padrão quando possível e não mostram todas as configurações e todos os caminhos possíveis. Para obter instruções detalhadas, examine os artigos de instruções.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de começar.


## <a name="prerequisites"></a>Pré-requisitos

- [Conclua](tutorial-prepare-hyper-v.md) o primeiro tutorial desta série. Caso contrário, as instruções deste tutorial não funcionarão.
- Veja o que você deve ter feito no primeiro tutorial:
    - [Preparar o Azure](tutorial-prepare-hyper-v.md#prepare-azure) para trabalhar com as Migrações para Azure.
    - [Preparar a avaliação de hosts](tutorial-prepare-hyper-v.md#prepare-for-assessment) e VMs do Hyper-V.
    - [Verificar](tutorial-prepare-hyper-v.md#prepare-for-appliance-deployment) o que você precisa para implantar o dispositivo de Migrações para Azure para avaliação do Hyper-V.

## <a name="set-up-an-azure-migrate-project"></a>Configurar um projeto das Migrações para Azure

1. No portal do Azure > **Todos os serviços**, pesquise **Migrações para Azure**.
2. Nos resultados da pesquisa, selecione **Migrações para Azure**.
3. Em **Visão Geral**, em **Descobrir, avaliar e migrar servidores**, clique em **Avaliar e migrar servidores**.

    ![Descobrir e avaliar servidores](./media/tutorial-assess-hyper-v/assess-migrate.png)

4. Em **Introdução**, clique em **Adicionar ferramentas**.
5. Na guia **Migrar projeto**, selecione sua assinatura do Azure e crie um grupo de recursos, caso não tenha um.
6. Em **Detalhes do Projeto**, especifique o nome do projeto e a região em que deseja criar o projeto. Examine as geografias compatíveis para [nuvens públicas](migrate-support-matrix.md#supported-geographies-public-cloud) e [governamentais](migrate-support-matrix.md#supported-geographies-azure-government).

    - A região do projeto é usada apenas para armazenar os metadados coletados das VMs locais.
    - Você pode selecionar uma região de destino do Azure diferente ao migrar as VMs. Todas as regiões do Azure têm suporte como destino de migração.

    ![Criar um projeto de Migrações para Azure](./media/tutorial-assess-hyper-v/migrate-project.png)

7. Clique em **Próximo**.
8. Em **Selecionar ferramenta de avaliação**, selecione **Migrações para Azure: Avaliação de Servidor** > **Avançar**.

    ![Criar um projeto de Migrações para Azure](./media/tutorial-assess-hyper-v/assessment-tool.png)

9. Em **Selecionar ferramenta de migração**, selecione **Ignorar a adição de uma ferramenta de migração por enquanto** > **Avançar**.
10. Em **Examinar + adicionar ferramentas**, examine as configurações e clique em **Adicionar ferramentas**.
11. Aguarde alguns minutos até que o projeto das Migrações para Azure seja implantado. Você será levado para a página do projeto. Caso não veja o projeto, acesse-o em **Servidores** no painel das Migrações para Azure.

## <a name="set-up-the-azure-migrate-appliance"></a>Configurar o dispositivo das Migrações para Azure


O recurso Migrações para Azure: Avaliação do Servidor usa um dispositivo leve de Migrações para Azure. O dispositivo executa a descoberta de VM e envia os metadados de VM e os dados de desempenho para as Migrações para Azure. O dispositivo pode ser configurado de várias maneiras.

- Configure-o em uma VM do Hyper-V usando um VHD do Hyper-V baixado. Esse é o método usado neste tutorial.
- Configurar em uma VM do Hyper-V ou computador físico com um script do instalador do PowerShell. [Esse método](deploy-appliance-script.md) deverá ser usado se você não puder configurar uma VM usando o VHD ou se você estiver no Azure Government.

Depois de criar o dispositivo, você verifica se é possível conectá-lo ao Migrações para Azure: Avaliação do Servidor, configurá-lo pela primeira vez e registrá-lo com o projeto de Migrações para Azure.

### <a name="generate-the-azure-migrate-project-key"></a>Gerar a chave do projeto das Migrações para Azure

1. Em **Metas de Migração** > **Servidores** > **Migrações para Azure: Avaliação de Servidor**, selecione **Descobrir**.
2. Em **Descobrir computadores** > **Os computadores estão virtualizados?** , selecione **Sim, com o Hyper-V**.
3. Em **1: Gerar chave de projeto das Migrações para Azure**, forneça um nome para o dispositivo das Migrações para Azure que você vai configurar para a descoberta das VMs do Hyper-V. O nome deverá conter até 14 caracteres alfanuméricos.
1. Clique em **Gerar chave** para iniciar a criação dos recursos do Azure necessários. Não feche a página Descobrir computadores durante a criação de recursos.
1. Após a criação bem-sucedida dos recursos do Azure, uma **chave de projeto das Migrações para Azure** é gerada.
1. Copie a chave, pois você precisará dela para concluir o registro do dispositivo durante a configuração dele.

### <a name="download-the-vhd"></a>Baixar o VHD

Em **2: Baixar o dispositivo das Migrações para Azure**, selecione o arquivo .VHD e clique em **Baixar**. 

   ![Seleções para Descobrir computadores](./media/tutorial-assess-hyper-v/servers-discover.png)


   ![Seleções para Gerar Chave](./media/tutorial-assess-hyper-v/generate-key-hyperv.png)


### <a name="verify-security"></a>Verificar a segurança

Verifique se o arquivo compactado é seguro antes de implantá-lo.

1. No computador no qual você baixou o arquivo, abra uma janela de comando do administrador.

2. Execute o comando do PowerShell a seguir para gerar o hash para o arquivo zip
    - ```C:\>Get-FileHash -Path <file_location> -Algorithm [Hashing Algorithm]```
    - Exemplo de uso: ```C:\>Get-FileHash -Path ./AzureMigrateAppliance_v1.19.06.27.zip -Algorithm SHA256```

3.  Verifique as versões mais recentes do dispositivo e os valores de hash:

    - Para a nuvem pública do Azure:

        **Cenário** | **Download** | **SHA256**
        --- | --- | ---
        Hyper-V (10,4 GB) | [Última versão](https://go.microsoft.com/fwlink/?linkid=2140422) |  79c151588de049cc102f61b910d6136e02324dc8d8a14f47772da351b46d9127

    - Para o Azure Government:

        **Cenário*** | **Download** | **SHA256**
        --- | --- | ---
        Hyper-V (85 MB) | [Última versão](https://go.microsoft.com/fwlink/?linkid=2140424) |  0769c5f8df1e8c1ce4f685296f9ee18e1ca63e4a111d9aa4e6982e069df430d7


### <a name="create-the-appliance-vm"></a>Criar a VM do dispositivo

Importe o arquivo baixado e crie a VM.

1. Depois de baixar o arquivo VHD compactado para o host Hyper-V no qual a VM do dispositivo será colocada, extraia o arquivo compactado.
    - No local extraído, o arquivo é descompactado em uma pasta chamada **AzureMigrateAppliance_NúmeroDaVersão**.
    - Essa pasta contém uma subpasta, também chamada de **AzureMigrateAppliance_NúmeroDaVersão**.
    - Essa subpasta contém três subpastas adicionais – **Instantâneos**, **Discos Rígidos Virtuais** e **Máquinas Virtuais**.

2. Abra o Gerenciador do Hyper-V. Em **Ações**, clique em **Importar Máquina Virtual**.

    ![Implantar o VHD](./media/tutorial-assess-hyper-v/deploy-vhd.png)

2. No Assistente para Importar Máquina Virtual > **Antes de começar**, clique em **Próximo**.
3. Em **Localizar Pasta**, selecione a pasta **Máquinas Virtuais**. Em seguida, clique em **Próximo**.
1. Em **Selecionar Máquina Virtual**, clique em **Próximo**.
2. Em **Escolher Tipo de Importação**, clique em **Copiar a máquina virtual (criar uma nova ID exclusiva)** . Em seguida, clique em **Próximo**.
3. Em **Escolher Destino**, mantenha a configuração padrão. Clique em **Próximo**.
4. Em **Pastas de Armazenamento**, mantenha a configuração padrão. Clique em **Próximo**.
5. Em **Escolher Rede**, especifique o comutador virtual que será usado pela VM. O comutador precisa de conectividade com a Internet para enviar dados ao Azure. [Saiba mais](/windows-server/virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines) sobre como criar um comutador virtual.
6. Em **Resumo**, examine as configurações. Em seguida, clique em **Concluir**.
7. No Gerenciador do Hyper-V, > **Máquinas Virtuais**, inicie a máquina virtual.


## <a name="verify-appliance-access-to-azure"></a>Verificar o acesso do dispositivo ao Azure

Verifique se a VM do dispositivo pode se conectar às URLs do Azure para as nuvens [pública](migrate-appliance.md#public-cloud-urls) e [governamental](migrate-appliance.md#government-cloud-urls).

### <a name="configure-the-appliance"></a>Configurar o dispositivo

Configure o dispositivo pela primeira vez.

> [!NOTE]
> Se você configurar o dispositivo usando um [script do PowerShell](deploy-appliance-script.md), em vez do VHD baixado, as duas primeiras etapas neste procedimento não serão relevantes.

1. No Gerenciador do Hyper-V > **Máquinas Virtuais**, clique com o botão direito do mouse na VM > **Conectar**.
2. Forneça o idioma, o fuso horário e a senha do dispositivo.
3. Abra um navegador em qualquer computador que possa se conectar à VM e abra a URL do aplicativo Web do dispositivo: **https://*nome do dispositivo ou endereço IP*: 44368**.

   Como alternativa, você pode abrir o aplicativo na área de trabalho do dispositivo clicando no atalho do aplicativo.
1. Aceite os **termos de licença** e leia as informações de terceiros.
1. No aplicativo Web > **Configurar os pré-requisitos**, faça o seguinte:
    - **Conectividade**: o aplicativo verifica se a VM tem acesso à Internet. Se a VM usar um proxy:
      - Clique em **Configurar proxy** e especifique o endereço de proxy (na forma http://ProxyIPAddress ou http://ProxyFQDN) e na porta de escuta.
      - Especifique as credenciais caso o proxy exija autenticação.
      - Há suporte apenas para o proxy HTTP.
      - Se você tiver adicionado detalhes de proxy ou desabilitado o proxy e/ou a autenticação, clique em **Salvar** para disparar a verificação de conectividade novamente.
    - **Sincronização do horário**: o horário é verificado. o horário no dispositivo deve ser sincronizado com o horário na Internet para que a descoberta da VM funcione corretamente.
    - **Instalar as atualizações**: A avaliação do servidor das Migrações para Azure verifica se o dispositivo tem as últimas atualizações instaladas. Depois que a verificação for concluída, você poderá clicar em **Exibir serviços de dispositivo** para ver o status e as versões dos componentes em execução no dispositivo.

### <a name="register-the-appliance-with-azure-migrate"></a>Registrar o dispositivo nas Migrações para Azure

1. Cole a **chave do projeto das Migrações para Azure** copiada do portal. Se você não tiver a chave, acesse **Avaliação do Servidor> Descobrir> Gerenciar dispositivos existentes**, selecione o nome do dispositivo fornecido no momento da geração da chave e copie a chave correspondente.
1. Clique em **Fazer logon**. Será aberto um prompt de logon do Azure em uma nova guia do navegador. Se essa opção não for exibida, verifique se você desabilitou o bloqueador de pop-ups no navegador.
1. Na nova guia, entre usando seu nome de usuário e senha do Azure.
   
   Não há suporte para a entrada com um PIN.
3. Depois de fazer logon com êxito, volte para o aplicativo Web. 
4. Se a conta de usuário do Azure usada para o registro em log tiver as [permissões](tutorial-prepare-hyper-v.md#prepare-azure) corretas nos recursos do Azure criados durante a geração de chave, o registro do dispositivo será iniciado.
1. Depois que o dispositivo for registrado com êxito, você poderá ver os detalhes do registro clicando em **Exibir detalhes**.



### <a name="delegate-credentials-for-smb-vhds"></a>Delegar credenciais para VHDs de SMB

Se estiver executando VHDs em SMBs, você precisará habilitar a delegação de credenciais do dispositivo para os hosts do Hyper-V. Para fazer isso, permita que cada host funcione como um representante do dispositivo. Se tiver seguido os tutoriais na ordem, você fez isso no tutorial anterior, quando preparou o Hyper-V para avaliação e migração. Você deve ter configurado o CredSSP para os hosts [manualmente](tutorial-prepare-hyper-v.md#enable-credssp-to-delegate-credentials) ou [executando um script](tutorial-prepare-hyper-v.md#run-the-script) que faz isso.

Habilite no dispositivo da seguinte maneira:

#### <a name="option-1"></a>Opção 1

Na VM do dispositivo, execute este comando. HyperVHost1/HyperVHost2 são nomes de host de exemplo.

```
Enable-WSManCredSSP -Role Client -DelegateComputer HyperVHost1.contoso.com, HyperVHost2.contoso.com, HyperVHost1, HyperVHost2 -Force
```

Exemplo: ` Enable-WSManCredSSP -Role Client -DelegateComputer HyperVHost1.contoso.com HyperVHost2.contoso.com -Force `

#### <a name="option-2"></a>Opção 2

Como alternativa, faça isso no Editor de Política de Grupo Local no dispositivo:

1. Em **Política do Computador local** > **Configuração do Computador**, clique em **Modelos Administrativos** > **Sistemas** > **Delegação de Credenciais**.
2. Clique duas vezes em **Permitir delegação de novas credenciais** e selecione **Habilitado**.
3. Em **Opções**, clique em **Mostrar** e adicione cada host Hyper-V que você deseja descobrir à lista, com o prefixo **wsman/** .
4. Em seguida, em **Delegação de Credenciais**, clique duas vezes em **Permitir delegação de novas credenciais com autenticação de servidor somente NTLM**. Mais uma vez, adicione cada host Hyper-V que você deseja descobrir à lista, com o prefixo **wsman/** .

## <a name="start-continuous-discovery"></a>Iniciar a descoberta contínua

Conecte-se do dispositivo a clusters ou hosts do Hyper-V e inicie a descoberta de VM.

1. Na **Etapa 1: Fornecer as credenciais do host Hyper-V**, clique em **Adicionar credenciais** para especificar um nome amigável para as credenciais, adicione o **Nome de usuário** e a **Senha** para um host/cluster Hyper-V que o dispositivo usará para descobrir VMs. Clique em **Save**.
1. Se desejar adicionar várias credenciais ao mesmo tempo, clique em **Adicionar mais** para salvar e adicionar mais credenciais. Há suporte a várias credenciais para a descoberta de VMs Hyper-V.
1. Na **Etapa 2: Fornecer detalhes do host/cluster Hyper-V**, clique em **Adicionar origem da descoberta** para especificar o **endereço IP/FQDN** do host/cluster Hyper-V e o nome amigável para as credenciais se conectarem ao host/cluster.
1. Você pode **Adicionar um item** de cada vez ou **Adicionar vários itens** em um só lugar. Também há uma opção de fornecer detalhes do host/cluster Hyper-V por meio de **Importar CSV**.

    ![Seleções para adicionar a origem da descoberta](./media/tutorial-assess-hyper-v/add-discovery-source-hyperv.png)

    - Se você escolher **Adicionar um item**, precisará especificar o nome amigável para as credenciais e o **endereço IP/FQDN** do host/cluster Hyper-V e clicar em **Salvar**.
    - Se você escolher **Adicionar vários itens** _(selecionado por padrão)_ , poderá adicionar vários registros de uma vez especificando o **endereço IP/FQDN** do host/cluster Hyper-V com o nome amigável para as credenciais na caixa de texto. **Verifique** os registros adicionados e clique em **Salvar**.
    - Se você escolher **Importar CSV**, poderá baixar um arquivo de modelo CSV, preenchê-lo com o **endereço IP/FQDN** do host/cluster Hyper-V e o nome amigável para as credenciais. Em seguida, importe o arquivo para o dispositivo, **verifique** os registros no arquivo e clique em **Salvar**.

1. Quando você clicar em Salvar, o dispositivo tentará validar a conexão com os hosts/clusters Hyper-V adicionados e mostrará o **Status de validação** na tabela em cada host/cluster.
    - Para ter hosts/clusters validados com êxito, você pode ver mais detalhes clicando no endereço IP/FQDN deles.
    - Se a validação falhar para um host, examine o erro clicando em **Falha na validação** na coluna Status da tabela. Corrija o problema e valide novamente.
    - Para remover hosts ou clusters, clique em **Excluir**.
    - Não é possível remover um host específico de um cluster. Você só pode remover o cluster inteiro.
    - Você pode adicionar um cluster, mesmo que haja problemas com hosts específicos nele.
1. Você pode **revalidar** a conectividade com os hosts/clusters a qualquer momento antes de iniciar a descoberta.
1. Clique em **Iniciar descoberta** para iniciar a descoberta da VM dos hosts/clusters validados com êxito. Depois que a descoberta for iniciada com êxito, você poderá verificar o status da descoberta em cada host/cluster na tabela.

Isso iniciará a descoberta. São necessários aproximadamente 2 minutos por host para que os metadados dos servidores descobertos sejam exibidos no portal do Azure.

### <a name="verify-vms-in-the-portal"></a>Verifique as VMs no portal

Após a descoberta terminar, você poderá verificar se as VMs são exibidas no portal.

1. Abra o painel das Migrações para Azure.
2. Na página **Migrações para Azure – Servidores** > **Migrações para Azure: Avaliação de Servidor**, clique no ícone que exibe a contagem de **Servidores descobertos**.

## <a name="set-up-an-assessment"></a>Configurar uma avaliação

Há dois tipos de avaliações que podem ser executadas usando a Avaliação de Servidor das Migrações para Azure.

**Avaliação** | **Detalhes** | **Dados**
--- | --- | ---
**Com base no desempenho** | Avaliações com base nos dados de desempenho coletados | **Tamanho de VM recomendado**: com base nos dados de utilização da CPU e de memória.<br/><br/> **Tipo de disco recomendado (disco gerenciado Standard ou Premium)** : com base na IOPS e na taxa de transferência dos discos locais.
**Como local** | Avaliações com base no dimensionamento local. | **Tamanho de VM recomendado**: com base no tamanho da VM local<br/><br> **Tipo de disco recomendado**: com base na configuração de tipo de armazenamento selecionada para a avaliação.



### <a name="run-an-assessment"></a>Ler uma avaliação

Execute uma avaliação da seguinte maneira:

1. Examine as [melhores práticas](best-practices-assessment.md) para a criação de avaliações.
2. Em **Servidores** > **Migrações para Azure: Avaliação de Servidor**, clique em **Avaliar**.

    ![Avaliar](./media/tutorial-assess-hyper-v/assess.png)

3. Em **Avaliar Servidores**, especifique um nome para a avaliação.
4. Clique em **Exibir tudo** para examinar as propriedades da avaliação.

    ![Propriedades de avaliação](./media/tutorial-assess-hyper-v/assessment-properties.png)

3. Em **Selecionar ou criar um grupo**, selecione **Criar** e especifique um nome de grupo. Um grupo reúne uma ou mais VMs para avaliação.
4. Em **Adicionar computadores ao grupo**, selecione as VMs a serem adicionadas ao grupo.
5. Clique em **Criar Avaliação** para criar o grupo e execute a avaliação.

    ![Criar uma avaliação](./media/tutorial-assess-hyper-v/assessment-create.png)

6. Após a criação da avaliação, veja-a em **Servidores** > **Migrações para Azure: Avaliação de Servidor**.
7. Clique em **Exportar avaliação**, para baixá-la como um arquivo do Excel.


## <a name="review-an-assessment"></a>Examinar uma avaliação

Uma avaliação descreve:

- **Preparação para o Azure**: indica se as VMs são adequadas para a migração para o Azure.
- **Estimativa de custo mensal**: os custos mensais estimados de computação e armazenamento para execução das VMs no Azure.
- **Estimativa de custo de armazenamento mensal**: custos estimados para o armazenamento em disco após a migração.


### <a name="view-an-assessment"></a>Exibir uma avaliação

1. Em **Metas de migração** >  **Servidores** > **Migrações para Azure: Avaliação de Servidor**, clique em **Avaliações**.
2. Em **Avaliações**, clique em uma avaliação para abri-la.

    ![Resumo da avaliação](./media/tutorial-assess-hyper-v/assessment-summary.png)


### <a name="review-azure-readiness"></a>Examinar a Preparação para o Azure

1. Em **Preparação para o Azure**, verifique se as VMs estão prontas para a migração para o Azure.
2. Examine o status da VM:
    - **Pronto para o Azure**: as Migrações para Azure recomendam um tamanho de VM e estimativas de custo para as VMs na avaliação.
    - **Pronto com condições**: mostra os problemas e a correção sugerida.
    - **Não está pronto para o Azure**: mostra os problemas e a correção sugerida.
    - **Preparação desconhecida**: usado quando as Migrações para Azure não podem avaliar a preparação, devido a problemas de disponibilidade de dados.

2. Clique em um status de **Preparação para o Azure**. Você pode exibir os detalhes de preparação da VM e fazer uma busca detalhada para ver os detalhes da VM, incluindo as configurações de computação, armazenamento e rede.

### <a name="review-cost-details"></a>Examinar os detalhes de custo

Essa exibição mostra o custo estimado de computação e armazenamento da execução das VMs no Azure.

1. Examine os custos mensais de computação e armazenamento. Os custos são agregados para todas as VMs no grupo avaliado.

    - As estimativas de custo são baseadas nas recomendações de tamanho para um computador e em seus discos e propriedades.
    - Os custos mensais estimados de computação e armazenamento são mostrados.
    - A estimativa de custo refere-se à execução das VMs locais como VMs de IaaS. A Avaliação de Servidor das Migrações para Azure não considera os custos de PaaS ou SaaS.

2. Você pode examinar as estimativas de custo mensal de armazenamento. Essa exibição mostra os custos agregados de armazenamento para o grupo avaliado, divididos em diferentes tipos de discos de armazenamento.
3. Você pode fazer uma busca detalhada para ver os detalhes de VMs específicas.


### <a name="review-confidence-rating"></a>Revisar classificação de confiança

Quando você executa avaliações com base no desempenho, uma classificação de confiança é atribuída à avaliação.

![Classificação de confiança](./media/tutorial-assess-hyper-v/confidence-rating.png)

- Uma classificação de 1 estrela (mais baixa) a 5 estrelas (mais alta) é fornecida.
- A classificação de confiança ajuda você a estimar a confiabilidade das recomendações de tamanho fornecidas pela avaliação.
- A classificação de confiança é baseada na disponibilidade dos pontos de dados necessários para calcular a avaliação.

As classificações de confiança para uma avaliação são indicadas a seguir.

**Disponibilidade do ponto de dados** | **Classificação de confiança**
--- | ---
0%-20% | 1 estrela
21%-40% | 2 estrelas
41%-60% | 3 estrelas
61%-80% | 4 estrelas
81%-100% | 5 estrelas

[Saiba mais](best-practices-assessment.md#best-practices-for-confidence-ratings) sobre as melhores práticas de classificações de confiança.





## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você:

> [!div class="checklist"]
> * Configurou um dispositivo das Migrações para Azure
> * Criou e examinou uma avaliação

Continue e acesse o terceiro tutorial da série para saber como migrar VMs do Hyper-V para o Azure com a Migração de Servidor das Migrações para Azure.

> [!div class="nextstepaction"]
> [Migrar VMs do Hyper-V](./tutorial-migrate-hyper-v.md)
