---
title: "Elemento de interface do usuário Section de aplicativo gerenciado do Azure | Microsoft Docs"
description: "Descreve o elemento Microsoft.Common.Section da interface do usuário para aplicativos gerenciados do Azure"
services: azure-resource-manager
documentationcenter: na
author: tabrezm
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/12/2017
ms.author: tabrezm;tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: afa23b1395b8275e72048bd47fffcf38f9dcd334
ms.openlocfilehash: 34c0f2f88e6af5a0f822ec116e7e2334e4e29e8d
ms.contentlocale: pt-br
ms.lasthandoff: 05/12/2017


---
# <a name="microsoftcommonsection-ui-element"></a>Elemento de interface do usuário Microsoft.Common.Section
Um controle que agrupa um ou mais elementos em um título. Use esse elemento ao [criar um Aplicativo Gerenciado do Azure](managed-application-publishing.md).

## <a name="ui-sample"></a>Exemplo de interface do usuário
![Microsoft.Common.Section](./media/managed-application-elements/microsoft.common.section.png)

## <a name="schema"></a>Esquema
```json
{
  "name": "section1",
  "type": "Microsoft.Common.Section",
  "label": "Some section",
  "elements": [
    {
      "name": "element1",
      "type": "Microsoft.Common.TextBox",
      "label": "Some text box 1"
    },
    {
      "name": "element2",
      "type": "Microsoft.Common.TextBox",
      "label": "Some text box 2"
    }
  ],
  "visible": true
}
```

## <a name="remarks"></a>Comentários
- `elements` deve conter pelo menos um elemento e pode conter todos os tipos de elemento exceto `Microsoft.Common.Section`.
- Esse elemento não dá suporte à propriedade `toolTip`.

## <a name="sample-output"></a>Saída de exemplo
Para acessar os valores de saída de elementos em `elements`, use as funções [basics()](managed-application-createuidefinition-functions.md#basics) ou [steps()](managed-application-createuidefinition-functions.md#steps) e a notação de ponto:

```json
basics('section1').element1
```

Elementos do tipo `Microsoft.Common.Section` não têm nenhum valor de saída próprio.

## <a name="next-steps"></a>Próximas etapas
* Para obter uma introdução aos aplicativos gerenciados, consulte [Visão geral de aplicativos gerenciados do Azure](managed-application-overview.md).
* Para obter uma introdução à criação de definições de interface do usuário, consulte [Introdução ao CreateUiDefinition](managed-application-createuidefinition-overview.md).
* Para obter uma descrição das propriedades comuns em elementos de interface do usuário, consulte [Elementos de CreateUiDefinition](managed-application-createuidefinition-elements.md).

