---
title: Simular um dispositivo com Azure IoT Edge (Windows) | Microsoft Docs
description: Como usar o Azure IoT Edge no Windows para criar um dispositivo simulado que envie telemetria por meio de um gateway do Azure IoT Edge para um hub IoT.
services: iot-hub
documentationcenter: 
author: chipalost
manager: timlt
editor: 
ms.assetid: 6a2aeda0-696a-4732-90e1-595d2e2fadc6
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/09/2017
ms.author: andbuc
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: e7eb2931993daf3f0aecbd4a43d27ebd5adc10b0
ms.contentlocale: pt-br
ms.lasthandoff: 06/17/2017


---

# <a name="use-azure-iot-edge-to-send-device-to-cloud-messages-with-a-simulated-device-windows"></a>Use o Azure IoT Edge para enviar mensagens do dispositivo para a nuvem com um dispositivo simulado (Windows)

[!INCLUDE [iot-hub-iot-edge-simulated-selector](../../includes/iot-hub-iot-edge-simulated-selector.md)]

[!INCLUDE [iot-hub-iot-edge-install-build-windows](../../includes/iot-hub-iot-edge-install-build-windows.md)]

## <a name="how-to-run-the-sample"></a>Como executar a amostra

O script **build.cmd** gera sua saída na pasta **build** na cópia local do repositório **iot-edge**. Essa saída inclui os quatro módulos do IoT Edge usados neste exemplo.

O script da build coloca:

* **logger.dll** na pasta **build\\modules\\logger\\Debug**.
* **iothub.dll** na pasta **build\\modules\\iothub\\Debug**.
* **identity\_map.dll** na pasta **build\\modules\\identitymap\\Debug**.
* **simulated\_device.dll** na pasta **build\\modules\\simulated\_device\\Debug**.

Use esses caminhos para os valores de **caminho do módulo**, conforme mostrado no seguinte arquivo de configurações do JSON:

O processo de exemplo \_device\_cloud\_upload\_ leva o caminho até um arquivo de configuração JSON como um argumento na linha de comando. O arquivo JSON de exemplo a seguir é fornecido no repositório do SDK em **samples\\simulated\_device\_cloud\_upload\_sample\\src\\simulated\_device\_cloud\_upload\_sample\_win.json**. Este arquivo de configuração funciona da forma como está, a menos que você tenha modificado o script de build para colocar os módulos ou os executáveis de exemplo do IoT Edge em locais não padrão.

> [!NOTE]
> Os caminhos de módulo são relativos ao diretório em que se encontra o simulated\_device\_cloud\_upload\_sample.exe. O arquivo de configuração JSON de exemplo, por padrão, grava 'deviceCloudUploadGatewaylog.log' no diretório de trabalho atual.

Em um editor de texto, abra o arquivo **samples\\simulated\_device\_cloud\_upload\_sample\\src\\simulated\_device\_cloud\_upload\_win.json** em sua cópia local do repositório **iot-edge**. Este arquivo configura os módulos do IoT Edge no gateway de exemplo:

* O módulo **IoTHub** se conecta ao seu hub IoT. Você o configura para enviar dados ao Hub IoT. Especificamente, defina o valor de **IoTHubName** como o nome de seu hub IoT e defina o valor de **IoTHubSuffix** como **azure-devices.net**. Defina o valor de **Transporte** como um destes: **HTTP**, **AMQP** ou **MQTT**. Atualmente, apenas **HTTP** compartilha uma conexão TCP para todas as mensagens de dispositivo. Se você definir o valor como **AMQP** ou **MQTT**, o gateway manterá uma conexão TCP separada para o Hub IoT para cada dispositivo.
* O módulo **mapping** mapeia os endereços MAC dos dispositivos simulados para as IDs de dispositivo do Hub IoT. Verifique se os valores de **deviceId** correspondem às IDs dos dois dispositivos que você adicionou ao hub IoT e se os valores de **deviceKey** contêm as chaves dos dois dispositivos.
* Os módulos **BLE1** e **BLE2** são os dispositivos simulados. Observe como os endereços MAC do módulo correspondem aos endereços no módulo **mapping**.
* O módulo **Logger** registra a atividade de gateway em um arquivo.
* Os valores de **caminho de módulo** exibidos no exemplo a seguir são relativos ao diretório em que se encontra o simulated\_device\_cloud\_upload\_sample.exe.
* A matriz **links** na parte inferior do arquivo JSON conecta os módulos **BLE1** e **BLE2** ao módulo **mapping**, e o **mapping** ao módulo **IoTHub**. Ela também garante que todas as mensagens são registradas pelo módulo **Logger** .

```json
{
    "modules" :
    [
      {
        "name": "IotHub",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\iothub\\Debug\\iothub.dll"
          }
          },
          "args": {
            "IoTHubName": "<<insert here IoTHubName>>",
            "IoTHubSuffix": "<<insert here IoTHubSuffix>>",
            "Transport": "HTTP"
          }
        },
      {
        "name": "mapping",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\identitymap\\Debug\\identity_map.dll"
          }
          },
          "args": [
            {
              "macAddress": "01:01:01:01:01:01",
              "deviceId": "<<insert here deviceId>>",
              "deviceKey": "<<insert here deviceKey>>"
            },
            {
              "macAddress": "02:02:02:02:02:02",
              "deviceId": "<<insert here deviceId>>",
              "deviceKey": "<<insert here deviceKey>>"
            }
          ]
        },
      {
        "name": "BLE1",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "01:01:01:01:01:01"
          }
        },
      {
        "name": "BLE2",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "02:02:02:02:02:02"
          }
        },
      {
        "name": "Logger",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\logger\\Debug\\logger.dll"
          }
        },
        "args": {
          "filename": "deviceCloudUploadGatewaylog.log"
        }
      }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IotHub" }
    ]
}
```

Salve as alterações feitas no arquivo de configuração.

Para executar a amostra:

1. No prompt de comando, navegue até a pasta **build** em sua cópia local do repositório **iot-edge**.
2. Execute o comando a seguir:
   
    ```cmd
    samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe ..\samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```
3. É possível usar o [gerenciador de dispositivo][lnk-device-explorer] ou a ferramenta [iothub-explorer][lnk-iothub-explorer] para monitorar as mensagens que o Hub IoT recebe do gateway. Por exemplo, usando o iothub-explorer, você pode monitorar as mensagens de dispositivo para nuvem usando o seguinte comando:

    ```cmd
    iothub-explorer monitor-events --login "HostName={Your iot hub name}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={Your IoT Hub key}"
    ```

## <a name="next-steps"></a>Próximas etapas

Para compreender de maneira mais avançada o IoT Edge e experimentar alguns exemplos de código, acesse os seguintes recursos e tutoriais para desenvolvedores:

* [Enviar mensagens do dispositivo para a nuvem de um dispositivo físico com o IoT Edge][lnk-physical-device]
* [Azure IoT Edge][lnk-iot-edge]

Para explorar melhor as funcionalidades do Hub IoT, consulte:

* [Guia do desenvolvedor do Hub IoT][lnk-devguide]
* [Proteger sua solução de IoT desde o início][lnk-securing]

<!-- Links -->
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-physical-device]: iot-hub-iot-edge-physical-device.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
