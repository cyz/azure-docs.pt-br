---
title: "Solução de contêineres no Azure Log Analytics | Microsoft Docs"
description: "A solução de contêineres no Log Analytics ajuda a exibir e gerenciar os hosts de contêiner do Docker e do Windows em uma única localização."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: e1e4b52b-92d5-4bfa-8a09-ff8c6b5a9f78
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: banders
ms.translationtype: HT
ms.sourcegitcommit: 79bebd10784ec74b4800e19576cbec253acf1be7
ms.openlocfilehash: 524c63d358ce22c10b7a23e5bcf0b33e9f2e5f26
ms.contentlocale: pt-br
ms.lasthandoff: 08/03/2017

---
# <a name="containers-preview-solution-in-log-analytics"></a>Solução Contêineres (Versão prévia) no Log Analytics

![Símbolo dos Contêineres](./media/log-analytics-containers/containers-symbol.png)

Este artigo descreve como configurar e usar a solução de contêineres no Log Analytics, que ajuda você a exibir e gerenciar os hosts de contêiner do Docker e do Windows em uma única localização. O Docker é um sistema de virtualização de software usado para criar contêineres que automatizam a implantação de software para infraestrutura de TI.

Com a solução, você pode ver quais contêineres estão em execução em seus hosts de contêiner e quais imagens estão em execução nos contêineres. Você pode exibir informações detalhadas de auditoria, mostrando os comandos usados com contêineres. E você pode solucionar os problemas de contêineres exibindo e pesquisando logs centralizados sem precisar exibir remotamente os hosts do Docker ou do Windows. Você pode encontrar contêineres que podem estar com ruídos e consumindo recursos em excesso em um host. E você pode exibir o uso de CPU, memória, armazenamento e rede e informações de desempenho centralizadas para contêineres. Nos computadores que executam o Windows, você pode centralizar e comparar os logs do Windows Server, do Hyper-V e dos contêineres do Docker.

O diagrama a seguir mostra as relações entre os vários hosts e agentes de contêiner com o OMS.

![Diagrama de contêineres](./media/log-analytics-containers/containers-diagram.png)

## <a name="system-requirements"></a>Requisitos do sistema
Antes de começar, examine os detalhes a seguir para verificar se você atende aos pré-requisitos.

### <a name="container-monitoring-solution-support-for-docker-orchestrator-and-os-platform"></a>Suporte de solução de monitoramento de contêiner para Docker Orchestrator e plataforma do SO 
A tabela a seguir descreve a orquestração do Docker e o suporte de monitoramento do sistema operacional do inventário de contêiner, desempenho e registros com o Log Analytics.   

| | ACS | Linux | Windows | Contêiner<br>Inventário | Imagem<br>Inventário | Nó<br>Inventário | Contêiner<br>Desempenho | Contêiner<br>Evento | Evento<br>Registro | Contêiner<br>Registro | 
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| kubernetes | Sim | Sim | | Sim | Sim | Sim | Sim | Sim | Sim | Sim | 
| Mesosphere<br>DC/OS | Sim | Sim | | Sim | Sim | Sim | Sim| Sim | Sim | Sim | 
| Docker<br>Swarm | Sim | Sim | Sim | Sim | Sim | Sim | Sim | Sim | | Sim |
| Descrição do<br>Fabric | | | Sim | Sim | Sim | Sim | Sim | Sim | Sim | Sim | 
| Red Hat Open<br>Shift | | Sim | | Sim | Sim| Sim | Sim | Sim | | Sim | 
| Windows Server<br>(autônomo) | | | Sim | Sim | Sim | Sim | Sim | Sim | | Sim |
| Linux Server<br>(autônomo) | | Sim | | Sim | Sim | Sim | Sim | Sim | | Sim |


### <a name="supported-linux-operating-system"></a>Sistema operacional Linux com suporte

- Docker 1.11 a 1.13
- Docker CE e EE v17.03

As seguintes distribuições de Linux x64 são suportadas como hosts de contêiner:

- Ubuntu 14.04 LTS, 16.04 LTS
- CoreOS(stable)
- Amazon Linux 2016.09.0
- openSUSE 13.2
- openSUSE LEAP 42.2
- CentOS 7.2, 7.3
- SLES 12
- RHEL 7.2, 7.3

### <a name="supported-windows-operating-system"></a>Sistema operacional Windows com suporte

- Windows Server 2016
- Edição de aniversário do Windows 10 (Professional ou Enterprise)

### <a name="docker-versions-supported-on-windows"></a>Versões do Docker do Windows com suporte

- Docker 1.12 – 1.13
- Docker 17.03.0 

## <a name="installing-and-configuring-the-solution"></a>Instalando e configurando a solução
Use as informações a seguir para instalar e configurar a solução.

Adicione a solução Contêineres do seu espaço de trabalho do OMS do [marketplace do Azure](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) ou usando o processo descrito em [Adicionar soluções do Log Analytics por meio da Galeria de Soluções](log-analytics-add-solutions.md).

Há algumas maneiras de instalar e usar o Docker com o OMS:

* Em sistemas operacionais Linux com suporte, instale e execute o Docker e, em seguida, instale e configure o Agente do OMS para Linux.
* No CoreOS, você não pode executar o Agente do OMS para Linux. Em vez disso, você deve executar uma versão em contêiner do Agente do OMS para Linux.
* No Windows Server 2016 e no Windows 10, instale o Mecanismo do Docker e, então, o cliente se conectará a um agente para coletar informações e enviá-las para o Log Analytics.


Você pode examinar as versões de sistema operacional Linux e Docker com suporte para seu host do contêiner no [GitHub](https://github.com/Microsoft/OMS-docker).

### <a name="container-services"></a>Serviços de Contêiner

- Se você tiver um cluster Kubernetes usando o Serviço de Contêiner do Azure, saiba mais em [Monitorar um cluster do Serviço de Contêiner do Azure com o Microsoft Operations Management Suite (OMS)](../container-service/kubernetes/container-service-kubernetes-oms.md).
- Se você tiver um cluster de DC/SO do Serviço de Contêiner do Azure, saiba mais em [Monitorar um cluster DC/OS do Serviço de Contêiner do Azure com o Operations Management Suite](../container-service/dcos-swarm/container-service-monitoring-oms.md).
- Se você tiver um ambiente no modo Docker Swarm, saiba mais em [Configurar um agente do OMS para o Docker Swarm](#configure-an-oms-agent-for-docker-swarm).
- Se você usa contêineres com o Service Fabric, saiba mais em [Visão geral do Azure Service Fabric](../service-fabric/service-fabric-overview.md).
- Examine o artigo [Mecanismo do Docker no Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon) para obter informações adicionais sobre como instalar e configurar seus Mecanismos do Docker em computadores que executam o Windows.

> [!IMPORTANT]
> O Docker deve estar em execução **antes** de instalar o [Agente do OMS para Linux](log-analytics-agent-linux.md) em seus hosts de contêiner. Se você já tiver instalado o agente antes de instalar o Docker, precisará reinstalar o Agente do OMS para Linux. Para obter mais informações sobre o Docker, consulte o [site do Docker](https://www.docker.com).
>
>

Você precisa das seguintes configurações definidas em seus hosts de contêiner antes de poder monitorar os contêineres.

## <a name="linux-container-hosts"></a>Hosts de contêiner do Linux

Depois de instalar o Docker, use as seguintes definições para o host do contêiner para configurar o agente para uso com o Docker. Você precisará da sua [chave e ID do espaço de trabalho do OMS](log-analytics-agent-linux.md).


### <a name="for-all-linux-container-hosts-except-coreos"></a>Para todos os hosts de contêiner do Linux, exceto CoreOS

- Para obter mais informações e etapas sobre como instalar o Agente do OMS para Linux, consulte  [Conectar computadores Linux ao OMS (Operations Management Suite)](log-analytics-agent-linux.md).

### <a name="for-all-linux-container-hosts-including-coreos"></a>Para todos os hosts de contêiner do Linux, incluindo o CoreOS

Inicie o contêiner do OMS que você deseja monitorar. Modifique e use o exemplo a seguir.

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25225:25225 --name="omsagent" --restart=always microsoft/oms
```

### <a name="for-all-azure-government-linux-container-hosts-including-coreos"></a>Para todos os hosts de contêiner do Linux no Azure Governamental, incluindo CoreOS

Inicie o contêiner do OMS que você deseja monitorar. Modifique e use o exemplo a seguir.

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v /var/log:/var/log -e WSID="your workspace id" -e KEY="your key" -e DOMAIN="opinsights.azure.us" -p 127.0.0.1:25225:25225 -p 127.0.0.1:25224:25224/udp --name="omsagent" -h=`hostname` --restart=always microsoft/oms
```


### <a name="switching-from-using-an-installed-linux-agent-to-one-in-a-container"></a>Alternância de uso de um agente do Linux instalado para outro em um contêiner
Se anteriormente você utilizou o agente instalado diretamente e, em vez disso, deseja usar um agente em execução em um contêiner, primeiro você deverá remover o Agente do OMS para Linux. Consulte [Desinstalar o Agente do OMS para Linux](log-analytics-agent-linux.md#uninstalling-the-oms-agent-for-linux).

### <a name="configure-an-oms-agent-for-docker-swarm"></a>Configurar um agente do OMS para o Docker Swarm

Execute o Agente do OMS como um serviço global no Docker Swarm. Use as informações a seguir para criar um serviço do Agente do OMS. Você precisa inserir a ID do Espaço de Trabalho e a Chave Primária.

- Execute o seguinte no nó mestre.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock  -e WSID="<WORKSPACE ID>" -e KEY="<PRIMARY KEY>" -p 25225:25225 -p 25224:25224/udp  --restart-condition=on-failure microsoft/oms
    ```

### <a name="secure-your-secret-information-for-container-services"></a>Proteger suas informações secretas de serviços de contêiner

Proteja sua ID do Espaço de Trabalho do OMS e as Chaves Primárias secretas para o Docker Swarm e o Kubernetes.

#### <a name="secure-secrets-for-docker-swarm"></a>Proteger segredos do Docker Swarm

Para o Docker Swarm, depois que o segredo da ID do Espaço de Trabalho e a Chave Primária for criado, você poderá executar e criar o serviço do Docker para o OMSagent. Use as informações a seguir para criar suas informações secretas.

1. Execute o seguinte no nó mestre.

    ```
    echo "WSID" | docker secret create WSID -
    echo "KEY" | docker secret create KEY -
    ```

2. Verifique se os segredos foram criados corretamente.

    ```
    keiko@swarmm-master-13957614-0:/run# sudo docker secret ls
    ```

    ```
    ID                          NAME                CREATED             UPDATED
    j2fj153zxy91j8zbcitnjxjiv   WSID                43 minutes ago      43 minutes ago
    l9rh3n987g9c45zffuxdxetd9   KEY                 38 minutes ago      38 minutes ago
    ```

3. Execute o comando a seguir para montar os segredos no Agente do OMS em contêineres.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock --secret source=WSID,target=WSID --secret source=KEY,target=KEY  -p 25225:25225 -p 25224:25224/udp --restart-condition=on-failure microsoft/oms
    ```

#### <a name="secure-secrets-for-kubernetes-with-yaml-files"></a>Proteger segredos do Kubernetes com arquivos yaml

Para o Kubernetes, use um script para gerar o arquivo .yaml de segredos para a ID do Espaço de Trabalho e a Chave Primária. Na página [GitHub do OMS Docker Kubernetes](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes), existem arquivos que você pode usar com ou sem as informações secretas.

- O DaemonSet do Agente do OMS Padrão que não tem informações secretas (omsagent.yaml)
- O arquivo yaml do DaemonSet do Agente do OMS que usa informações secretas (omsagent-ds-secrets.yaml) com scripts de geração de segredo que geram o arquivo yaml de segredos (omsagentsecret.yaml).

Você pode optar por criar DaemonSets do omsagent com ou sem segredos.

##### <a name="default-omsagent-daemonset-yaml-file-without-secrets"></a>Arquivo yaml do DaemonSet do OMSagent Padrão sem segredos

- Para o arquivo yaml do DaemonSet do Agente do OMS padrão, substitua `<WSID>` e `<KEY>` pela WSID e KEY. Copie o arquivo para o nó mestre e execute o seguinte:

    ```
    sudo kubectl create -f omsagent.yaml
    ```

##### <a name="default-omsagent-daemonset-yaml-file-with-secrets"></a>Arquivo yaml do DaemonSet do OMSagent Padrão com segredos

1. Para usar o DaemonSet do Agente do OMS usando informações secretas, crie os segredos primeiro.
    1. Copie o script e o arquivo de modelo de segredo e verifique se eles estão no mesmo diretório.
        - Script de geração de segredo – secret-gen.sh
        - modelo de segredo – secret-template.yaml
    2. Execute o script, como no exemplo a seguir. O script solicitará a ID do Espaço de Trabalho do OMS e a Chave Primária e, depois que você inseri-los, o script criará um arquivo .yaml secreto para que você possa executá-lo.   

        ```
        #> sudo bash ./secret-gen.sh
        ```

    3. Crie o pod de segredos executando o seguinte:
        ```
        sudo kubectl create -f omsagentsecret.yaml
        ```

    4. Para verificar, execute o seguinte:

        ```
        keiko@ubuntu16-13db:~# sudo kubectl get secrets
        ```

        O resultado deve ser semelhante a este:

        ```
        NAME                  TYPE                                  DATA      AGE
        default-token-gvl91   kubernetes.io/service-account-token   3         50d
        omsagent-secret       Opaque                                2         1d
        ```

        ```
        keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
        ```

        O resultado deve ser semelhante a este:

        ```
        Name:           omsagent-secret
        Namespace:      default
        Labels:         <none>
        Annotations:    <none>

        Type:   Opaque

        Data
        ====
        WSID:   36 bytes
        KEY:    88 bytes
        ```

    5. Criar o omsagent daemon-set executando ``` sudo kubectl create -f omsagent-ds-secrets.yaml ```

2. Verifique se o DaemonSet do Agente do OMS está em execução, de forma semelhante à seguinte:

    ```
    keiko@ubuntu16-13db:~# sudo kubectl get ds omsagent
    ```

    ```
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   3         3         <none>          1h
    ```


Para o Kubernetes, use um script para gerar o arquivo yaml de segredos para a ID do Espaço de Trabalho e a Chave Primária. Use as informações de exemplo a seguir com o [arquivo yaml do omsagent](https://github.com/Microsoft/OMS-docker/blob/master/Kubernetes/omsagent.yaml) para proteger suas informações secretas.

```
keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
Name:           omsagent-secret
Namespace:      default
Labels:         <none>
Annotations:    <none>

Type:   Opaque

Data
====
WSID:   36 bytes
KEY:    88 bytes
```


## <a name="windows-container-hosts"></a>Hosts de contêiner do Windows

### <a name="preparation-before-installing-windows-agents"></a>Preparação antes de instalar os agentes do Windows

Antes de instalar os agentes em computadores que executam o Windows, você precisa configurar o serviço Docker. A configuração permite que o agente do Windows ou a extensão da máquina virtual do Log Analytics use o soquete Docker TCP para que os agentes possam acessar o daemon do Docker remotamente e capturar os dados de monitoramento.

#### <a name="to-start-docker-and-verify-its-configuration"></a>Iniciar o Docker e verificar sua configuração

Há etapas necessárias para configurar o pipe nomeado por TCP para o Windows Server:

1. No Windows PowerShell, habilite o pipe TCP e o pipe nomeado.

    ```
    Stop-Service docker
    dockerd --unregister-service
    dockerd --register-service -H npipe:// -H 0.0.0.0:2375  
    Start-Service docker
    ```

2. Configure o Docker com o arquivo de configuração para o pipe TCP e o pipe nomeado. O arquivo de configuração está localizado em C:\ProgramData\docker\config\daemon.json.

    No arquivo daemon.json, você precisará do seguinte:

    ```
    {
    "hosts": ["tcp://0.0.0.0:2375", "npipe://"]
    }
    ```

Para obter mais informações sobre a configuração do daemon do Docker usada com os Contêineres do Windows, consulte [Mecanismo do Docker no Windows](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).


### <a name="install-windows-agents"></a>Instalar agentes do Windows

Para habilitar o monitoramento do contêiner do Windows e do Hyper-V, instale os agentes em computadores com Windows que sejam hosts do contêiner. Para computadores que executam o Windows no seu ambiente local, consulte [Conectar computadores Windows ao Log Analytics](log-analytics-windows-agents.md). Para máquinas virtuais em execução no Azure, conecte-as ao Log Analytics usando a [extensão da máquina virtual](log-analytics-azure-vm-extension.md).

Você pode monitorar os contêineres do Windows em execução no Service Fabric. No entanto, apenas [máquinas virtuais em execução no Azure](log-analytics-azure-vm-extension.md) e [computadores executando o Windows no seu ambiente local](log-analytics-windows-agents.md) têm suporte atualmente para o Service Fabric.

Para verificar se a solução de contêineres está definida corretamente:

- Verifique se o pacote de gerenciamento foi baixado corretamente, procure *ContainerManagement.xxx*.
    - Os arquivos devem estar na pasta C:\Arquivos de Programas\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs.
- Verifique se a ID do Espaço de Trabalho do OMS está correta indo para **Painel de Controle** > **Sistema e Segurança**.
    - Abra o **Microsoft Monitoring Agent** e verifique se as informações do espaço de trabalho estão corretas.


## <a name="containers-data-collection-details"></a>Detalhes da coleta de dados dos contêineres
A solução de contêineres coleta vários dados de log e métricas de desempenho dos hosts de contêiner e dos contêineres que usam os agentes que você habilitou.

A tabela a seguir mostra os métodos de coleta de dados e outros detalhes sobre como os dados são coletados para os contêineres.

| plataforma | [Agente do OMS para Linux](log-analytics-linux-agents.md) | Agente SCOM | Armazenamento do Azure | SCOM necessário? | Os dados do agente SCOM enviados por meio do grupo de gerenciamento | frequência de coleta |
| --- | --- | --- | --- | --- | --- | --- |
| Linux |![Sim](./media/log-analytics-containers/oms-bullet-green.png) |![Não](./media/log-analytics-containers/oms-bullet-red.png) |![Não](./media/log-analytics-containers/oms-bullet-red.png) |![Não](./media/log-analytics-containers/oms-bullet-red.png) |![Não](./media/log-analytics-containers/oms-bullet-red.png) |a cada 3 minutos |

| plataforma | [Agente do Windows](log-analytics-windows-agents.md) | Agente SCOM | Armazenamento do Azure | SCOM necessário? | Os dados do agente SCOM enviados por meio do grupo de gerenciamento | frequência de coleta |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |![Sim](./media/log-analytics-containers/oms-bullet-green.png) |![Não](./media/log-analytics-containers/oms-bullet-red.png) |![Não](./media/log-analytics-containers/oms-bullet-red.png) |![Não](./media/log-analytics-containers/oms-bullet-red.png) |![Não](./media/log-analytics-containers/oms-bullet-red.png) |a cada 3 minutos |

| plataforma | [Extensão de VM do Log Analytics](log-analytics-azure-vm-extension.md) | Agente SCOM | Armazenamento do Azure | SCOM necessário? | Os dados do agente SCOM enviados por meio do grupo de gerenciamento | frequência de coleta |
| --- | --- | --- | --- | --- | --- | --- |
| As tabelas |![Sim](./media/log-analytics-containers/oms-bullet-green.png) |![Não](./media/log-analytics-containers/oms-bullet-red.png) |![Não](./media/log-analytics-containers/oms-bullet-red.png) |![Não](./media/log-analytics-containers/oms-bullet-red.png) |![Não](./media/log-analytics-containers/oms-bullet-red.png) |a cada 3 minutos |

A tabela a seguir mostra exemplos de tipos de dados coletados pela solução de contêineres e os tipos de dados que são usados nas Pesquisas de Log e nos resultados.

| Tipo de dados | Tipo de dados na Pesquisa de Log | Campos |
| --- | --- | --- |
| Desempenho de hosts e contêineres | `Type=Perf` | Computer, ObjectName, CounterName &#40;%Processor Time, Disk Reads MB, Disk Writes MB, Memory Usage MB, Network Receive Bytes, Network Send Bytes, Processor Usage sec, Network&#41;, CounterValue,TimeGenerated, CounterPath, SourceSystem |
| Inventário de contêiner | `Type=ContainerInventory` | TimeGenerated, Computer, container name, ContainerHostname, Image, ImageTag, ContinerState, ExitCode, EnvironmentVar, Command, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Inventário de imagem de contêiner | `Type=ContainerImageInventory` | TimeGenerated, Computer, Image, ImageTag, ImageSize, VirtualSize, Running, Paused, Stopped, Failed, SourceSystem, ImageID, TotalContainer |
| Log do contêiner | `Type=ContainerLog` | TimeGenerated, Computer, image ID, container name, LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Log do serviço de contêiner | `Type=ContainerServiceLog`  | TimeGenerated, Computer, TimeOfCommand, Image, Command, SourceSystem, ContainerID |

## <a name="monitor-containers"></a>Monitorar contêineres
Depois de habilitar a solução no portal do OMS, você verá o bloco **Contêineres** mostrando informações resumidas sobre seus hosts de contêiner e os contêineres em execução nos hosts.

![Bloco Contêineres](./media/log-analytics-containers/containers-title.png)

O bloco mostra uma visão geral de quantos contêineres existem no ambiente e se eles estão com falha, em execução ou parados.

### <a name="using-the-containers-dashboard"></a>Usando o painel de Contêineres
Clique no bloco **Contêineres**. A partir daí, você verá exibições organizadas por:

* Eventos de contêiner
* Erros
* Status dos contêineres
* Inventário de imagem de contêiner
* Desempenho de CPU e memória

Cada painel no painel é uma representação visual de uma pesquisa que é executada nos dados coletados.

![Painel de Contêineres](./media/log-analytics-containers/containers-dash01.png)

![Painel de Contêineres](./media/log-analytics-containers/containers-dash02.png)

Na folha **Status do Contêiner**, clique na área superior, como mostrado abaixo.

![Status dos contêineres](./media/log-analytics-containers/containers-status.png)

A Pesquisa de Log é aberta, mostrando informações sobre os hosts e os contêineres em execução neles.

![Pesquisa de Log para contêineres](./media/log-analytics-containers/containers-log-search.png)

A partir daqui, você pode editar a consulta de pesquisa para modificá-la para localizar as informações específicas nas quais está interessado. Para obter mais informações sobre as Pesquisas de Log, consulte [Pesquisas de log no Log Analytics](log-analytics-log-searches.md).

Por exemplo, você pode modificar a consulta de pesquisa para que ela mostre todos os contêineres parados em vez dos contêineres em execução alterando **Em Execução** para **Parado** na consulta de pesquisa.

## <a name="troubleshoot-by-finding-a-failed-container"></a>Solucionar problemas localizando um contêiner com falha
O OMS marca um contêiner como **Com Falha** se ele tiver sido encerrado com um código de saída diferente de zero. Você pode conferir uma visão geral dos erros e falhas no ambiente na folha **Contêineres com Falha**.

### <a name="to-find-failed-containers"></a>Para localizar contêineres com falha
1. Clique na folha **Eventos de Contêiner**.  
   ![eventos de contêineres](./media/log-analytics-containers/containers-events.png)
2. A Pesquisa de Log é aberta, mostrando o status dos contêineres, semelhante ao seguinte.  
   ![estado dos contêineres](./media/log-analytics-containers/containers-container-state.png)
3. Em seguida, clique no valor com falha para exibir informações adicionais, como o tamanho da imagem e o número de imagens paradas e com falha. Expanda **mostrar mais** para exibir a ID da imagem.  
   ![contêineres com falha](./media/log-analytics-containers/containers-state-failed.png)
4. Em seguida, localize o contêiner que está executando esta imagem. Digite o seguinte na consulta de pesquisa.
   `Type=ContainerInventory <ImageID>` Isso exibe os logs. Você pode rolar para ver o contêiner com falha.  
   ![contêineres com falha](./media/log-analytics-containers/containers-failed04.png)

## <a name="search-logs-for-container-data"></a>Pesquisar nos logs por dados do contêiner
Quando você estiver solucionando um erro específico, pode ajudar ver onde ele está ocorrendo em seu ambiente. Os tipos de log a seguir ajudarão você a criar consultas para retornar as informações desejadas.

* **ContainerInventory** – use este tipo quando desejar obter informações sobre a localização do contêiner, quais são seus nomes e quais imagens eles estão executando.
* **ContainerImageInventory** – use este tipo quando estiver tentando localizar informações organizadas por imagem e para exibir informações da imagem como os tamanhos ou IDs da imagem.
* **ContainerLog** – use este tipo quando desejar localizar entradas e informações de log de erro específicas.
* **ContainerServiceLog** – use este tipo quando estiver tentando localizar informações de trilha de auditoria para o daemon do Docker, como os comandos start, stop, delete ou pull.

### <a name="to-search-logs-for-container-data"></a>Para pesquisar nos logs por dados do contêiner
* Escolha uma imagem que você saiba que falhou recentemente e encontre os logs de erros dela. Comece localizando um nome de contêiner que está executando a imagem com uma pesquisa **ContainerInventory**. Por exemplo, pesquise por `Type=ContainerInventory ubuntu Failed`  
    ![Pesquisar por contêineres do Ubuntu](./media/log-analytics-containers/search-ubuntu.png)

  Anote o nome do contêiner a lado de **Nome** e pesquise por esses logs. Neste exemplo, é `Type=ContainerLog adoring_meitner`.

**Exibir informações de desempenho**

Quando você começar a construir consultas, pode ajudar ver o que é possível primeiro. Por exemplo, para ver todos os dados de desempenho, tente uma consulta ampla digitando a seguinte consulta de pesquisa.

```
Type=Perf
```

![desempenho de contêineres](./media/log-analytics-containers/containers-perf01.png)

Você pode ver isso em um formato mais gráfico quando clicar na palavra **Métricas** nos resultados.

![desempenho de contêineres](./media/log-analytics-containers/containers-perf02.png)

Você pode definir o escopo dos dados de desempenho que está vendo para um contêiner específico digitando o nome dele à direita da sua consulta.

```
Type=Perf <containerName>
```

Isso mostra a lista de métricas de desempenho que são coletadas para um contêiner individual.

![desempenho de contêineres](./media/log-analytics-containers/containers-perf03.png)

## <a name="example-log-search-queries"></a>Exemplo de consultas de pesquisa de log
Costuma ser útil criar consultas começando com um ou dois exemplos e, em seguida, modificá-los de acordo com seu ambiente. Como ponto de partida, você pode experimentar com a folha **Consultas Notáveis** para ajudá-lo a criar consultas mais avançadas.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Consultas de contêineres](./media/log-analytics-containers/containers-queries.png)


## <a name="saving-log-search-queries"></a>Salvando as consultas de pesquisa de log
Salvar consultas é um recurso padrão do Log Analytics. Ao salvá-las, você terá aquelas que considerou úteis acessíveis para uso futuro.

Depois de criar uma consulta que considerar útil, salve-a clicando em **Favoritos** na parte superior da página Pesquisa de Log. Depois, você pode acessá-la facilmente pela página **Meu Painel**.

## <a name="next-steps"></a>Próximas etapas
* [Pesquise nos logs](log-analytics-log-searches.md) para exibir registros de dados de contêiner detalhados.

