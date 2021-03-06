---
title: "Receber alertas do log de atividades nas notificações de serviço | Microsoft Docs"
description: "Seja notificado por SMS, email ou webhook quando um serviço do Azure for executado."
author: anirudhcavale
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 57e136c1027cfa7c789803409cef354f3d52d222
ms.lasthandoff: 03/31/2017


---
# <a name="create-activity-log-alerts-on-service-notifications"></a>Criar alertas do log de atividades em notificações de serviço
## <a name="overview"></a>Visão geral
Este artigo mostra como configurar alertas do log de atividades para notificações de integridade do serviço usando o portal do Azure.  

Você pode receber um alerta de acordo com as notificações de integridade do serviço de sua assinatura do Azure.  
Você pode configurar o alerta de acordo com:
- A classe de notificação de integridade do serviço (Incidente, Manutenção, Informações, etc.)
- O status da notificação (Ativo vs. Resolvido)
- O nível das notificações (Informativo, Aviso ou Erro)

Também é possível configurar para quem o alerta deve ser enviado:
- Selecionar um Grupo de Ação existente
- Criar um novo Grupo de Ação (que pode ser usado posteriormente para futuros alertas)

Saiba mais sobre Grupos de Ação [aqui](monitoring-action-groups.md)

Você pode configurar e obter informações sobre alertas de notificação de integridade do serviço usando
- [Portal do Azure](monitoring-activity-log-alerts-on-service-notifications.md)
- [Modelos do Gerenciador de Recursos](monitoring-create-activity-log-alerts-with-resource-manager-template.md)

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-with-the-azure-portal"></a>Criar um alerta em uma notificação de integridade do serviço para um novo grupo de ação com o Portal do Azure
1.    No [portal](https://portal.azure.com), navegue até o serviço **Monitor**

    ![Monitoramento](./media/monitoring-activity-log-alerts-on-service-notifications/home-monitor.png)

2.    Clique na opção **Monitor** para abrir a folha Monitor. Ela abre primeiro na seção **Log de atividades** .

3.    Agora, clique na seção **Alertas**.

    ![Alertas](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades.png)

4.    Selecione o comando **Adicionar alerta do log de atividades** e preencha os campos

    ![Add-Alert](./media/monitoring-activity-log-alerts-on-service-notifications/add-activity-log-alert.png)

5.    **Dê um nome** ao alerta do log de atividades e escolha uma **Descrição**. Ela será exibida nas notificações enviadas quando esse alerta for acionado.

    ![Add-Alert-New-Action-Group](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-service-notification-new-action-group.png)

6.    A **Assinatura** deve ser preenchida automaticamente para a assinatura que você está usando no momento.

7.    Escolha o **Grupo de Recursos** para esse alerta.

8.    Em **Categoria de Evento**, selecione a opção “Integridade do Serviço”. Escolha sobre qual **Tipo, Status** e **Nível** de Notificações de Integridade do Serviço você deseja ser notificado.

9.    Crie um **Novo** Grupo de Ação dando a ele um **Nome** e um **Nome Curto**; o Nome Curto será referenciado nas notificações enviadas quando esse alerta for acionado.

10.    Em seguida, defina uma lista de destinatários fornecendo os seguintes itens do destinatário

    a. **Nome:** nome do destinatário, alias ou identificador.

    b. **Tipo de Ação:** opte por contatar o destinatário por SMS, email ou webhook

    c. **Detalhes:** de acordo com o tipo de ação escolhido, forneça um número de telefone, endereço de email ou URI de webhook.

11.    Selecione **OK** ao concluir a criação do alerta.

Em alguns minutos, o alerta estará ativo e disparará conforme descrito anteriormente.

Para obter detalhes sobre o esquema de webhook para os alertas do log de atividades, [clique aqui](monitoring-activity-log-alerts-webhook.md)

>[!NOTE]
>O grupo de ações definido nessas etapas será reutilizável, como um grupo de ação existente, para todas as definições de alerta futuras.
>
>

## <a name="create-an-alert-on-a-service-health-notification-for-an-existing-action-group-with-the-azure-portal"></a>Criar um alerta em uma notificação de integridade do serviço para um grupo de ação existente com o Portal do Azure
1.    No [portal](https://portal.azure.com), navegue até o serviço **Monitor**

    ![Monitoramento](./media/monitoring-activity-log-alerts-on-service-notifications/home-monitor.png)
2.    Clique na opção **Monitor** para abrir a folha Monitor. Ela abre primeiro na seção **Log de atividades** .

3.    Agora, clique na seção **Alertas**.

    ![Alertas](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades.png)
4.    Selecione o comando **Adicionar alerta do log de atividades** e preencha os campos

    ![Add-Alert](./media/monitoring-activity-log-alerts-on-service-notifications/add-activity-log-alert.png)
5.    **Dê um nome** ao alerta do log de atividades e escolha uma **Descrição**. Ela será exibida nas notificações enviadas quando esse alerta for acionado.

    ![Add-Alert-Existing-Action-Group](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-service-notification-existing-action-group.png)
6.    A **Assinatura** deve ser preenchida automaticamente para a assinatura que você está usando no momento.

7.    Escolha o **Grupo de Recursos** para esse alerta.

8.    Em **Categoria de Evento**, selecione a opção “Integridade do Serviço”. Escolha sobre qual **Tipo, Status** e **Nível** de Notificações de Integridade do Serviço você deseja ser notificado.

9.    Escolha **Notificar Por** um **Grupo de ação existente**. Selecione o respectivo grupo de ação.

10.    Selecione **OK** ao concluir a criação do alerta.

Em alguns minutos, o alerta estará ativo e disparará conforme descrito anteriormente.

## <a name="managing-your-alerts"></a>Gerenciar seus alertas

Depois de criar um alerta, ele ficará visível na seção Alertas do serviço Monitor. Selecione o alerta que você deseja gerenciar para poder:
* **Editá-lo**.
* **Excluí-lo**.
* **Desabilitá-lo** ou **Habilitá-lo** se desejar interromper temporariamente ou continuar recebendo notificações do alerta.

## <a name="next-steps"></a>Próximas etapas:
Saiba mais sobre as [Notificações de integridade do serviço](monitoring-service-notifications.md)  
Examine o [esquema do webhook dos alertas do log de atividades](monitoring-activity-log-alerts-webhook.md) Obtenha uma [visão geral dos alertas do log de atividades](monitoring-overview-alerts.md) e saiba como receber alertas  
Saiba mais sobre [grupos de ação](monitoring-action-groups.md)

