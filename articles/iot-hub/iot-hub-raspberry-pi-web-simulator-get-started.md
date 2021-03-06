---
title: "Raspberry Pi simulado para nuvem (Node.js) – Conectar o simulador web Raspberry Pi ao Hub IoT do Azure | Microsoft Docs"
description: Conectar o simulador web Raspberry Pi ao Hub IoT do Azure para que o Raspberry Pi envie dados para a nuvem do Azure.
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: simulador raspberry pi, raspberry pi azure iot, hub iot raspberry pi, raspberry pi enviar dados para a nuvem, raspberry pi para nuvem
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 5/27/2017
ms.author: xshi
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: fcba6f37f4a71da78f0e8b1c83c7a60711bbed09
ms.contentlocale: pt-br
ms.lasthandoff: 06/28/2017


---

# <a name="connect-raspberry-pi-online-simulator-to-azure-iot-hub-nodejs"></a>Conectar o simulador online Raspberry Pi ao Hub IoT do Azure (Node.js)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

Neste tutorial, você começará aprendendo as noções básicas de como trabalhar com o simulador online Raspberry Pi. Em seguida, aprenderá a conectar o simulador Pi diretamente à nuvem usando o [Hub IoT do Azure](iot-hub-what-is-iot-hub.md). 

Se você tiver dispositivos físicos, visite [Conectar ao Raspberry Pi ao Azure IoT Hub](iot-hub-raspberry-pi-kit-node-get-started.md) para começar. 

## <a name="what-you-do"></a>O que fazer

* Conheça os fundamentos do simulador online Raspberry Pi.
* Crie um Hub IoT.
* Registre um dispositivo para o Pi em seu Hub IoT.
* Execute um aplicativo de exemplo no Pi para enviar os dados do sensor simulado para o seu Hub IoT.

Conecte o Raspberry Pi simulado a um Hub IoT criado por você. Em seguida, você deve executar um aplicativo de exemplo com o simulador para gerar dados de sensor. Por fim, você envia os dados do sensor para o Hub IoT.

## <a name="what-you-learn"></a>O que você aprenderá

* Como criar um Hub IoT do Azure e obter sua nova cadeia de conexão do dispositivo. Se não tiver uma conta do Azure, [crie uma conta de avaliação gratuita do Azure](https://azure.microsoft.com/free/) em apenas alguns minutos.
* Como trabalhar com o Simulador online Raspberry Pi.
* Como enviar dados de sensor ao Hub IoT.

## <a name="overview-of-raspberry-pi-web-simulator"></a>Visão geral do simulador de web do Raspberry Pi

Clique no botão para iniciar o simulador online Raspberry Pi.

> [!div class="button"]
[Iniciar o simulador do Raspberry Pi](https://azure-samples.github.io/raspberry-pi-web-simulator/)

Há três áreas no simulador da web.
* Área do assembly - O circuito padrão é que um Pi se conecta com um sensor BME280 e um LED. A área é bloqueada na versão prévia assim no momento você não pode fazer a personalização.
* Codificação área - Um editor de código online para você no código com Raspberry Pi. O aplicativo de exemplo padrão ajuda a coletar dados de sensor do sensor BME280 e envia para o Azure IoT Hub. O aplicativo é totalmente compatível com dispositivos de Pi reais. 
* Janela de console integrado - Mostra a saída do seu código. Na parte superior da janela, há três botões.
   * **Executar** - Executar o aplicativo na área de codificação.
   * **Redefinir** - Redefinir a área de codificação para o aplicativo de exemplo padrão.
   * **Dobrar/expandir** - No lado direito, há um botão para a dobrar/expandir a janela do console.

> [!NOTE] 
O simulador de web do Raspberry Pi agora está disponível na versão prévia. Gostaríamos de ouvir sua voz no [Gitter Chatroom](https://gitter.im/Microsoft/raspberry-pi-web-simulator). O código-fonte é público no [GitHub](https://github.com/Azure-Samples/raspberry-pi-web-simulator).

![Visão geral do simulador online de Pi](media/iot-hub-raspberry-pi-web-simulator/0_overview.png)

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]


## <a name="run-a-sample-application-on-pi-web-simulator"></a>Executar um aplicativo de exemplo no simulador de web Pi

1. Na área de codificação, verifique se você está trabalhando no aplicativo de exemplo padrão. Substitua o espaço reservado na Linha 15 com cadeia de conexão do dispositivo do Azure IoT hub.
   ![Substitua a cadeia de conexão do dispositivo](media/iot-hub-raspberry-pi-web-simulator/1_connectionstring.png)

2. Clique em **Executar** ou digite `npm start` para executar o aplicativo.


Você deverá ver a seguinte saída, mostrando os dados do sensor e as mensagens que são enviadas ao seu Hub IoT ![saída - dados de sensor enviados do Raspberry Pi para o seu Hub IoT](media/iot-hub-raspberry-pi-web-simulator/2_run_application.png)


## <a name="next-steps"></a>Próximas etapas

Você executou um aplicativo de exemplo para coletar dados de sensor e enviá-los ao seu Hub IoT.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

