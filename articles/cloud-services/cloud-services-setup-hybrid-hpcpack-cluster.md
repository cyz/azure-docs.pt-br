---
title: "Configurar um cluster HPC híbrido com o Microsoft HPC Pack | Microsoft Docs"
description: "Aprenda a usar o Microsoft HPC Pack e o Azure para configurar um cluster de computação de alto desempenho (HPC) híbrido e pequeno"
services: cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,hpc-pack
ms.assetid: b11f3e1b-b9e1-4b0d-8455-6607b88814e9
ms.service: cloud-services
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: danlep
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 0fcfcc53641ebdf8a668b353db8eebb3bc64795d
ms.lasthandoff: 03/31/2017


---
# <a name="set-up-a-hybrid-high-performance-computing-hpc-cluster-with-microsoft-hpc-pack-and-on-demand-azure-compute-nodes"></a>Configurar um cluster HPC (computação de alto desempenho) híbrido com nós de computação do Azure sob demanda e o Microsoft HPC Pack
Use o Microsoft HPC Pack 2012 R2 e o Azure para configurar um cluster HPC (computação de alto desempenho) híbrido e pequeno. O cluster consistirá em um nó de cabeça no local (um computador que execute o sistema operacional Windows Server e o HPC Pack) e alguns nós de computação implantados sob demanda como instâncias de função de trabalho em um serviço de nuvem do Azure. É possível executar trabalhos de computação no cluster híbrido.

![Cluster híbrido de HPC][Overview] 

Este tutorial mostra uma abordagem, às vezes chamada de cluster "de estouro para a nuvem", para usar os recursos de computação dimensionáveis e sob demanda no Azure para executar aplicativos com uso intensivo de computação.

Este tutorial não pressupõe nenhuma experiência anterior com clusters de cálculo ou com o HPC Pack 2012 R2. Ele destina-se somente para ajudá-lo a implantar um cluster híbrido de cálculo de forma rápida para fins de demonstração. Para obter as considerações e as etapas para implantar um cluster híbrido do HPC Pack em uma escala maior em um ambiente de produção, ou para usar o HCP Pack 2016, confira as [diretrizes detalhadas](http://go.microsoft.com/fwlink/p/?LinkID=200493). Para ver outros cenários com o HPC Pack, incluindo a implantação de cluster automatizada nas máquinas virtuais do Azure, confira [Opções de cluster HPC com o Microsoft HPC Pack no Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="prerequisites"></a>Pré-requisitos
* **Assinatura do Azure** - Se você não tiver uma assinatura do Azure, poderá criar uma [conta gratuita](https://azure.microsoft.com/free/) em apenas alguns minutos.
* **Um computador local executando o Windows Server 2012 R2 ou o Windows Server 2012** - Esse computador será o nó de cabeçalho do cluster HPC. Se você ainda não estiver executando o Windows Server, poderá baixar e instalar uma [versão de avaliação](https://www.microsoft.com/evalcenter/evaluate-windows-server-2012-r2).
  
  * O computador deve estar ingressado a um domínio do Active Directory. Para um cenário de teste com uma instalação nova do Windows Server, você pode adicionar a função de servidor Serviços de Domínio do Active Directory e promover o computador nó principal como um controlador de domínio em uma nova floresta de domínio (consulte a documentação do Windows Server).
  * Para oferecer suporte ao HPC Pack, o sistema operacional deve ser instalado em um desses idiomas: inglês, japonês ou chinês (simplificado).
  * Verifique se as atualizações importantes e críticas foram instaladas.
* **HPC Pack 2012 R2** - [Baixe](http://go.microsoft.com/fwlink/p/?linkid=328024) o pacote de instalação para a versão mais recente gratuita e copie os arquivos para o computador do nó de cabeçalho ou um local de rede. Escolha os arquivos de instalação no mesmo idioma da sua instalação do Windows Server.

    >[!NOTE]
    > Se você quiser usar o HPC Pack 2016 em vez do HPC Pack 2012 R2, será necessária uma configuração adicional. Confira as [orientações detalhadas](http://go.microsoft.com/fwlink/p/?LinkID=200493).
    > 
* **Conta de domínio** - Esta conta deve ser configurada com as permissões de Administrador local no nó de cabeçalho para instalar o HPC Pack.
* **Conectividade TCP na porta 443** do nó de cabeçalho para o Azure.

## <a name="install-hpc-pack-on-the-head-node"></a>Instalar o HPC Pack no nó de cabeça
Primeiro instale o Microsoft HPC Pack em um computador local executando o Windows Server, que será o nó de cabeçalho do cluster.

1. Faça login no nó de cabeça usando uma conta de domínio que tenha permissões de Administrador local.
2. Inicie o Assistente de Instalação do HPC Pack executando Setup.exe nos arquivos de instalação do HPC Pack.
3. Na tela **Instalação do HPC Pack 2012 R2**, clique em **Nova instalação ou adicionar novos recursos a uma instalação existente**.
   
    ![Instalação do HPC Pack 2012][install_hpc1]
4. Na **página do Contrato de Usuário do Software da Microsoft**, clique em **Avançar**.
5. Na página **Selecionar Tipo de Instalação**, clique em **Criar um novo cluster de HPC, criando um nó de cabeça** e, em seguida, clique em **Avançar**.
   
    ![Selecione o Tipo de instalação][install_hpc2]
6. O assistente executa vários testes de pré-instalação. Clique em **Avançar** on the **Regras de instalação** se todos os testes forem aprovados. Caso contrário, examine as informações fornecidas e faça as alterações necessárias em seu ambiente. Execute os testes novamente ou se necessário inicie o Assistente de Instalação novamente.
   
    ![Regras de instalação][install_hpc3]
7. Na página **Configuração do BD de HPC**, certifique-se de que **Nó de cabeça** esteja selecionado para todos os bancos de dados de HPC e, em seguida, clique em **Avançar**.
   
    ![Configuração do BD][install_hpc4]
8. Aceite as seleções padrão nas páginas restantes do assistente. Na página **Instalar Componentes necessários**, clique em **Instalar**.
   
    ![Instalar][install_hpc6]
9. Após a conclusão da instalação, desmarque **Iniciar o Gerenciador de Cluster de HPC** e, em seguida, clique em **Concluir**. (Você iniciará o Gerenciador de Cluster HPC em uma etapa posterior.)
   
    ![Concluir][install_hpc7]

## <a name="prepare-the-azure-subscription"></a>Preparar a assinatura do Azure
Use o [portal clássico do Azure](https://manage.windowsazure.com) para realizar as etapas a seguir com sua assinatura do Azure. Elas são necessárias para implantar posteriormente os nós do Azure no nó de cabeça local. Confira os procedimentos detalhados nas próximas seções.

* Carregue um certificado de gerenciamento (necessário para proteger conexões entre o nó de cabeça e os serviços do Azure)
* Crie um serviço de nuvem do Azure no qual os nós do Azure (instâncias de função de trabalho) serão executados
* Criar uma conta de armazenamento do Azure
  
  > [!NOTE]
  > Anote também sua ID de assinatura do Azure, que será necessária posteriormente. Encontre-a no Portal Clássico clicando em **Configurações** > **Assinaturas**.
  > 
  > 

### <a name="upload-the-default-management-certificate"></a>Carregue o certificado de gerenciamento padrão
O HPC Pack instala um certificado autoassinado no nó de cabeça, chamado de Certificado de gerenciamento padrão de HPC no Azure da Microsoft, que pode ser carregado como um certificado de gerenciamento do Azure. Este certificado é fornecido para implantações de prova de conceito e fins de teste.

1. No computador do nó de cabeçalho, faça logon no [portal clássico do Azure](https://manage.windowsazure.com).
2. Clique em **Configurações** > **Certificados de Gerenciamento**.
3. Na barra de comandos, clique em **Nova**.
   
    ![Configurações de certificado][upload_cert1]
4. Procure no nó de cabeçalho pelo arquivo C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer. Em seguida, clique no botão **Verificar** .
   
    ![Carregar um certificado][install_hpc10]

Você verá **Gerenciamento padrão de HPC no Azure** na lista de certificados de gerenciamento.

### <a name="create-an-azure-cloud-service"></a>Criar um serviço de nuvem do Azure
> [!NOTE]
> Para melhor desempenho, crie o serviço de nuvem e a conta de armazenamento (em uma etapa posterior) na mesma região geográfica.
> 
> 

1. No portal clássico, na barra de comandos, clique em **Novo**.
2. Clique em **Computação** > **Serviço de Nuvem** > **Criação Rápida**.
3. Digite uma URL para o serviço de nuvem e, em seguida, clique em **Criar Serviço de Nuvem**.
   
    ![Criar Serviço][createservice1]

### <a name="create-an-azure-storage-account"></a>Criar uma conta de armazenamento do Azure
1. No portal clássico, na barra de comandos, clique em **Novo**.
2. Clique em **Serviços de Dados** > **Armazenamento** > **Criação Rápida**.
3. Digite uma URL para a conta e, em seguida, clique em **Criar Conta de armazenamento**.
   
    ![Criar Armazenamento][createstorage1]

## <a name="configure-the-head-node"></a>Configurar o nó de cabeça
Para usar o Gerenciador de Cluster de HPC para implantar nós do Azure e para enviar trabalhos, primeiro realize algumas etapas de configuração necessárias do cluster.

1. No nó de cabeça, inicie o Gerenciador de Cluster de HPC. Se a caixa de diálogo **Selecionar Nó de Cabeça** for exibida, clique em **Computador Local**. A **Lista de tarefas de implantação** é exibida.
2. Em **Tarefas de implantação necessárias**, clique em **Configurar sua rede**.
   
    ![Configurar Rede][config_hpc2]
3. No Assistente de Configuração de Rede, selecione **Todos os nós somente em uma rede corporativa** (Topologia 5).
   
    ![Topologia 5][config_hpc3]
   
   > [!NOTE]
   > Essa é a configuração mais simples para fins de demonstração, pois o nó de cabeçalho só precisa de um adaptador de rede para se conectar ao Active Directory e à Internet. Este tutorial não abrange cenários de cluster que exigem redes adicionais.
   > 
   > 
4. Clique em **Avançar** para aceitar os valores padrão nas páginas restantes do assistente. Em seguida, na guia **Examinar**, clique em **Configurar** para concluir a configuração de rede.
5. Na **Lista de tarefas de implantação**, clique em **Fornecer credenciais de instalação**.
6. Na caixa de diálogo **Credenciais de instalação** , digite as credenciais da conta de domínio usada para instalar o HPC Pack. Em seguida, clique em **OK**.
   
    ![Credenciais de instalação][config_hpc6]
   
   > [!NOTE]
   > Os serviços do HPC Pack só usam credenciais de instalação para implantar nós de computação que ingressaram em um domínio. Os nós do Azure que você adiciona neste tutorial não são ingressaram em um domínio.
   > 
   > 
7. Na **Lista de tarefas de implantação**, clique em **Configurar a nomeação dos novos nós**.
8. Na caixa de diálogo **Especificar Série de Nomeação de Nós**, aceite a série de nomeação padrão e clique em **OK**.
   
    ![Nomeação de nós][config_hpc8]
   
   > [!NOTE]
   > A série de nomeação apenas gera nomes para nós de computação que ingressaram em um domínio. Os nós de trabalho do Azure são nomeados automaticamente.
   > 
   > 
9. Na **Lista de tarefas de implantação**, clique em **Criar um modelo de nó**. Você usará o modelo de nó para adicionar nós do Azure ao cluster.
10. Em Criar Assistente do modelo de nó, faça o seguinte:
    
    a. Na página **Escolher Tipo de Modelo de Nó**, clique em **Modelo de nó do Azure** e, em seguida, clique em **Avançar**.
    
    ![Modelo do nó][config_hpc10]
    
    b. Clique em **Avançar** para aceitar o nome do modelo padrão.
    
    c. Na página **Fornecer Informações sobre Assinatura**, insira sua ID de Assinatura do Azure (disponível em suas informações de conta do Azure). Em seguida, em **Certificado de gerenciamento**, clique em **Procurar** e selecione **Gerenciamento de HPC no Azure Padrão** Em seguida, clique em **Próximo**.
    
    ![Modelo do nó][config_hpc12]
    
    d. Na página **Fornecer informações sobre serviço** , selecione o serviço de nuvem e a conta de armazenamento criados na etapa anterior. Em seguida, clique em **Próximo**.
    
    ![Modelo do nó][config_hpc13]
    
    e. Clique em **Avançar** para aceitar os valores padrão nas páginas restantes do assistente. Em seguida, na guia **Examinar**, clique em **Criar** para criar o modelo de nó.
    
    > [!NOTE]
    > Por padrão, o modelo de nó do Azure inclui configurações para iniciar (provisionar) e interromper os nós manualmente, usando o Gerenciador de Cluster HPC. Como opção, você também pode configurar uma agenda para iniciar e interromper os nós do Azure automaticamente.
    > 
    > 

## <a name="add-azure-nodes-to-the-cluster"></a>Adicionar nós do Azure ao cluster
Agora você usará o modelo do nó para adicionar nós do Azure ao cluster. Adicionar os nós ao cluster armazena suas informações de configuração para que você possa iniciá-los (provisioná-los) a qualquer momento como instâncias de função no serviço de nuvem. Sua assinatura somente é cobrada pelos nós do Azure depois que as instâncias de função estiverem sendo execucatas no serviço de nuvem.

Para este tutorial, você adicionará dois nós Pequenos.

1. No Gerenciador de Cluster HPC, em **Gerenciamento de Nós**, (chamado de **Gerenciamento de Recursos** em versões recentes do HPC Pack), no painel **Ações**, clique em **Adicionar Nó**.
   
    ![Adicionar Nó][add_node1]
2. No Assistente para Adicionar Nós, na página **Selecionar Método de Implantação**, clique em **Adicionar nós do Windows Azure** e em **Avançar**.
   
    ![Adicionar nó do Azure][add_node1_1]
3. Na página **Especificar Novos Nós**, selecione o modelo do nó do Azure criado anteriormente (chamado por padrão de **Modelo de Nós do Azure Padrão**). Em seguida, especifique **2** nós de tamanho **Pequeno** e clique em **Avançar**.
   
    ![Especificar nós][add_node2]
   
    Para obter detalhes sobre os tamanhos disponíveis, confira [Tamanhos para os Serviços de Nuvem](cloud-services-sizes-specs.md).
4. Na página de **Conclusão do Assistente de Adição de Nós**, clique em **Concluir**.
   
     Dois nós do Azure, chamados **AzureCN-0001** e **AzureCN-0002**, agora são exibidos no Gerenciador de Cluster de HPC. Ambos estão no estado **Não Implantado** .
   
    ![Nós adicionados][add_node3]

## <a name="start-the-azure-nodes"></a>Iniciar os nós do Azure
Quando desejar usar os recursos de cluster no Azure, use o Gerenciador de Cluster de HPC para iniciar (provisionar) os nós no Azure e colocá-los on-line.

1. No Gerenciador de Cluster HPC, em **Gerenciamento de Nós**, (chamado de **Gerenciamento de Recursos** em versões recentes do HPC Pack), clique em um ou nos dois nós e, em seguida, no painel **Ações**, clique em **Iniciar**.
   
   ![Iniciar nós][add_node4]
2. Na caixa de diálogo **Parar nós do Windows Azure**, clique em **Iniciar**.
   
    ![Iniciar nós][add_node5]
   
    Os nós farão a transição para o **Provisionamento** . Exiba o log de provisionamento para controlar o progresso do provisionamento.
   
    ![Provisionar nós][add_node6]
3. Depois de alguns minutos, os nós do Azure concluirão o provisionamento e estarão no estado **Off-line** . Nesse estado, as instâncias de função estarão sendo executadas, mas ainda não aceitarão trabalhos de cluster.
4. Para confirmar se as instâncias de função estão sendo executadas, no [Portal Clássico](https://manage.windowsazure.com), clique em **Serviços de Nuvem** > *nome_do_serviço_de_nuvem* > **Instâncias**.
   
    ![Executando instâncias][view_instances1]
   
    Você visualizará duas instâncias de função de trabalho sendo executadas no serviço. O HPC Pack implanta automaticamente duas instâncias **HpcProxy** (de tamanho Médio) para manipular as comunicações entre o nó de cabeça e o Azure.
5. Para colocar os nós do Azure on-line para executar trabalhos de cluster, selecione os nós, clique com o botão direito e clique em **Colocar on-line**.
   
    ![Nós off-line][add_node7]
   
    O Gerenciador de Cluster de HPC indica que os nós estão no estado **On-line** .

## <a name="run-a-command-across-the-cluster"></a>Executar um comando no cluster
Para verificar a instalação, use o comando **clusrun** do HPC Pack para executar um comando ou aplicativo em um ou mais nós do cluster. Como um exemplo simples, use **clusrun** para obter a configuração de IP dos nós do Azure.

1. No nó de cabeça, abra um prompt de comando.
2. Digite o seguinte comando:
   
    `clusrun /nodes:azurecn* ipconfig`
3. Você verá saídas semelhantes ao seguinte.
   
    ![clusrun][clusrun1]

## <a name="run-a-test-job"></a>Executar um trabalho de teste
Você pode enviar um trabalho de teste que é executado no cluster híbrido. Este exemplo é um trabalho muito simples de limpeza paramétrica (um tipo de computação intrinsecamente paralela). Este exemplo executa as subtarefas que adicionam um número inteiro a si mesmo usando o comando **set /a** . Todos os nós no cluster contribuem para concluir as subtarefas para números inteiros de 1 a 100.

1. No Gerenciador de Cluster de HPC, em **Gerenciamento de Trabalho**, no painel **Ações**, clique em **Novo Trabalho de Limpeza Paramétrica**.
   
    ![Novo trabalho][test_job1]
2. Na caixa de diálogo **Novo Trabalho de Limpeza Paramétrica** em **Linha de Comando**, digite `set /a *+*` (substituindo a linha de comando padrão que é exibida). Deixe os valores padrão para o restante das configurações e clique em **Enviar** para enviar o trabalho.
   
    ![Limpeza paramétrica][param_sweep1]
3. Quando o trabalho estiver concluído, clique duas vezes no trabalho **Minhas tarefas de limpeza** .
4. Clique em **Exibir tarefas**e em uma subtarefa para exibir a saída calculada dessa subtarefa.
   
    ![Resultados da tarefa][view_job361]
5. Para visualizar qual nó executou o cálculo para dessa subtarefa, clique em **Nós alocados**. (Seu cluster pode mostrar um nome de nó diferente).
   
    ![Resultados da tarefa][view_job362]

## <a name="stop-the-azure-nodes"></a>Parar os nós do Azure
Depois de testar o cluster, interrompa os nós do Azure para evitar cobranças desnecessárias em sua conta. Isso interromperá o serviço de nuvem e removerá as instâncias de função do Azure.

1. No Gerenciador de Cluster HPC, em **Gerenciamento de Nós**, (chamado de **Gerenciamento de Recursos** em versões recentes do HPC Pack), selecione ambos os nós do Azure. Em seguida, no painel **Ações**, clique **Parar**.
   
    ![Parar os nós][stop_node1]
2. Na caixa de diálogo **Parar nós do Windows Azure**, clique em **Parar**.
   
    ![Parar os nós][stop_node2]
3. Os nós farão a transição para a **Parada** . Depois de alguns minutos, o Gerenciador de Cluster de HPC mostra o estado dos nós como **Não implantados**.
   
    ![Nós não implantados][stop_node4]
4. Para confirmar que as instâncias de função não estão mais sendo executadas no Azure, no [Portal Clássico](https://manage.windowsazure.com), clique em **Serviços de Nuvem** > *nome_do_serviço_de_nuvem* > **Instâncias**. Nenhuma instância será implantada no ambiente de produção.
   
    ![Nenhuma instância][view_instances2]
   
    Assim, concluímos o tutorial.

## <a name="next-steps"></a>Próximas etapas
* Explore a documentação do [HPC Pack](https://technet.microsoft.com/library/cc514029).
* Para configurar uma implantação de cluster de HPC Pack hibrida em uma escala maior, consulte [Intermitência para Instâncias de Função de Trabalho do Azure com Microsoft HPC Pack](http://go.microsoft.com/fwlink/p/?LinkID=200493).
* Para outras maneiras de criar um cluster de HPC Pack no Azure, inclusive usando modelos do Azure Resource Manager, veja [Opções de cluster HPC com Microsoft HPC Pack no Azure](../virtual-machines/windows/hpcpack-cluster-options.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Consulte [Big Compute no Azure: Recursos Técnicos para HPC (Computação de Alto Desempenho) e Lote](../batch/big-compute-resources.md) para obter mais informações sobre o alcance de soluções de nuvem HPT e Big Compute no Azure.

[Overview]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/hybrid_cluster_overview.png
[install_hpc1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc1.png
[install_hpc2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc2.png
[install_hpc3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc3.png
[install_hpc4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc4.png
[install_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc6.png
[install_hpc7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc7.png
[install_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc10.png
[upload_cert1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/upload_cert1.png
[createstorage1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/createstorage1.png
[createservice1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/createservice1.png
[config_hpc2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc2.png
[config_hpc3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc3.png
[config_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc6.png
[config_hpc8]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc8.png
[config_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc10.png
[config_hpc12]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc12.png
[config_hpc13]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc13.png
[add_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1.png
[add_node1_1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1_1.png
[add_node2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node2.png
[add_node3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node3.png
[add_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node4.png
[add_node5]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node5.png
[add_node6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node6.png
[add_node7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node7.png
[view_instances1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances1.png
[clusrun1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/clusrun1.png
[test_job1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/test_job1.png
[param_sweep1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/param_sweep1.png
[view_job361]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job361.png
[view_job362]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job362.png
[stop_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node1.png
[stop_node2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node2.png
[stop_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node4.png
[view_instances2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances2.png

