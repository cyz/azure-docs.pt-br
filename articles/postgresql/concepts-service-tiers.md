---
title: "Tipo de preço no Banco de Dados do Azure para PostgreSQL"
description: "Tipo de preço no Banco de Dados do Azure para PostgreSQL"
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.custom: mvc
ms.service: postgresql-database
ms.topic: article
ms.date: 05/31/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: 2bd54b85f4c9f9ff13b8975eee15649f607a9194
ms.contentlocale: pt-br
ms.lasthandoff: 06/16/2017

---
# <a name="azure-database-for-postgresql-options-and-performance-understand-whats-available-in-each-pricing-tier"></a>Opções e desempenho do Banco de Dados do Azure para PostgreSQL: Compreenda o que está disponível em cada tipo de serviço
Quando você cria um banco de dados do Azure para o servidor PostgreSQL, você decide três principais opções para configurar os recursos alocados para o servidor. Essas opções afetam o desempenho e dimensionamento do servidor.
- Camada de preços
- Unidades de computação
- Armazenamento (GB)

Cada camada de preços tem um intervalo de níveis de desempenho (Unidades de Computação) à sua escolha, dependendo dos requisitos de cargas de trabalho. Os níveis de desempenho mais elevados fornecem recursos adicionais, projetados para oferecer uma taxa de transferência crescente. Você pode alterar o nível de desempenho do servidor dentro de uma camada de preços praticamente sem tempo de inatividade do aplicativo.

> [!IMPORTANT]
> Enquanto o serviço estiver em visualização pública, não há um contrato de nível de serviço (SLA) garantido.

Dentro de um banco de dados do Azure para o servidor MySQL, você pode ter um ou mais bancos de dados. Você pode optar por criar um banco de dados por servidor para utilizar todos os recursos ou criar vários bancos de dados para compartilhar os recursos. 

## <a name="choose-a-pricing-tier"></a>Escolher um tipo de preço
Enquanto estiver no modo visualização, o Banco de Dados do Azure para PostgreSQL oferece apenas as faixas de preço Básico e Padrão. Faixa Premium não está disponível, mas estará disponível em breve. 

A tabela a seguir fornece exemplos das faixas de preço mais adequadas para cargas de trabalho de aplicativos diferentes.

| Camada de preços | Cargas de trabalho de destino |
| :----------- | :----------------|
| Basic | Mais adequada para cargas de trabalho pequenas que exigem computação e armazenamento escalonáveis sem garantia de IOPS. Os exemplos incluem servidores usados para desenvolvimento ou teste ou aplicativos de pequena escala usados com pouca frequência. |
| Standard | A opção para aplicativos de nuvem que precisam de IOPS com alta taxa de transferência. Entre os exemplos incluem aplicativos analíticos ou da Web. |
| Premium | Ideal para cargas de trabalho que precisam de baixa latência para transações e IO. Fornece o melhor suporte para muitos usuários simultâneos. Aplicável a bancos de dados que oferecem suporte a aplicativos de missão crítica.<br />A faixa de preço Premium não está disponível no modo visualização. |

Para decidir sobre uma faixa de preço, comece determinando se sua carga de trabalho precisa de garantia de IOPS. Nesse caso, use a faixa de preços Padrão.

| **Recursos das faixas de preços** | **Básico** | **Standard** |
| :------------------------ | :-------- | :----------- |
| Unidades de computação máxima | 100 | 800 | 
| Armazenamento total máximo | 1 TB | 1 TB | 
| Garantia IOPS de armazenamento | N/D  | Sim | 
| IOPS de armazenamento máximo | N/D  | 3.000 | 
| Período de retenção do backup de banco de dados | 7 dias | 35 dias | 

Durante o período de visualização, você não pode alterar o tipo de preço depois que o servidor for criado. No futuro, será possível atualizar ou fazer downgrade de um tipo de preço para outro.

## <a name="choose-a-performance-level-compute-units"></a>Escolher um nível de desempenho (Unidades de Computação)
Depois de determinar a faixa de preços para seu banco de dados do Azure para o servidor PostgreSQL, você está pronto para determinar o nível de desempenho, selecionando o número de unidades de computação necessário. As Unidades de Computação 200 e 400 costumam ser um bom ponto de partida para aplicativos que exigem maior simultaneidade de usuários para suas cargas de trabalho de análise ou da Web e podem ser ajustadas incrementalmente conforme a necessidade. 

Unidades de computação são uma medida de taxa de transferência de processamento da CPU que possuem disponibilidade garantida para um único Banco de Dados do Azure para o servidor PostgreSQL. Uma unidade de computação é uma medida combinada de recursos de CPU e memória.  Para obter mais informações, consulte [Explicando Unidades de Computação](concepts-compute-unit-and-storage.md)

### <a name="basic-pricing-tier-performance-levels"></a>Níveis de desempenho da faixa de preço Básico:

| **Nível de desempenho** | **50** | **100** |
| :-------------------- | :----- | :------ |
| Unidades de computação máxima | 50 | 100 |
| Tamanho do armazenamento incluído | 50 GB | 50 GB |
| Tamanho máximo de armazenamento do servidor\* | 1 TB | 1 TB |

### <a name="standard-pricing-tier-performance-levels"></a>Níveis de desempenho da faixa de preço Padrão:

| **Nível de desempenho** | **100** | **200** | **400** | **800** |
| :-------------------- | :------ | :------ | :------ | :------ |
| Unidades de computação máxima | 100 | 200 | 400 | 800 |
| Tamanho de armazenamento incluído e IOPS provisionado | 125 GB,<br/> 375 IOPS | 125 GB,<br/> 375 IOPS | 125 GB,<br/> 375 IOPS | 125 GB,<br/> 375 IOPS |
| Tamanho máximo de armazenamento do servidor\* | 1 TB | 1 TB | 1 TB | 1 TB |
| IOPS máximo provisionado do servidor | 3.000 IOPS | 3.000 IOPS | 3.000 IOPS | 3.000 IOPS |
| IOPS máximo provisionado do servidor por GB | 3 IOPS fixos por GB | 3 IOPS fixos por GB | 3 IOPS fixos por GB | 3 IOPS fixos por GB |

\* Tamanho máximo de armazenamento do servidor refere-se ao tamanho máximo de armazenamento provisionado para o servidor.

## <a name="storage"></a>Armazenamento 
A configuração de armazenamento define a quantidade de capacidade de armazenamento disponível para um banco de dados do Azure para o servidor PostgreSQL. O armazenamento usado pelo serviço inclui os arquivos de banco de dados, logs de transação e os logs do servidor PostgreSQL. Considere o tamanho de armazenamento necessário para hospedar os bancos de dados e os requisitos de desempenho (IOPS) ao selecionar a configuração de armazenamento.

Algumas capacidades de armazenamento estão incluídas no mínimo com cada tipo de preço, indicadas na tabela anterior como "Tamanho dos armazenamentos incluídos." Capacidade de armazenamento adicional pode ser adicionada quando o servidor é criado, em incrementos de 125 GB até o máximo permitido de armazenamento. A capacidade de armazenamento adicional pode ser configurada independentemente da configuração de Unidades de Computação. O preço altera com base na quantidade de armazenamento selecionado.

A configuração de IOPS em cada nível de desempenho está relacionada à faixa de preços e o tamanho de armazenamento escolhido. A faixa Básico não oferece garantia de IOPS. Na faixa de preços Padrão, o IOPS dimensiona proporcionalmente ao tamanho máximo de armazenamento em uma taxa fixa de 3:1. O armazenamento incluído de 125 GB garante 375 IOPS provisionados, cada um com um tamanho de E/S de 256 KB. Você pode escolher armazenamento adicional máximo de 1 TB, para garantir 3.000 IOPS provisionados.

Monitorar o gráfico de métricas no portal do Azure ou gravar comandos de CLI do Azure para medir o consumo de armazenamento e IOPS. Métricas relevantes para monitorar são o Limite de armazenamento, Porcentagem de armazenamento, Armazenamento usado e porcentagem de IO.

>[!IMPORTANT]
> Enquanto estiver no modo de visualização, escolha a quantidade de armazenamento no momento em que o servidor é criado. Ainda não há suporte para a alteração do tamanho de armazenamento em um servidor existente. 

## <a name="scaling-a-server-up-or-down"></a>Aumentar ou diminuir um único servidor
Inicialmente, você escolher o nível de desempenho e a faixa de preços quando você cria o banco de dados do Azure para PostgreSQL. Posteriormente, você pode dimensionar as Unidades de Computação para cima ou para baixo dinamicamente, dentro do intervalo da mesma faixa de preços. No portal do Azure, deslize as Unidades de Computação na folha de faixa de preços do servidor ou script, como no exemplo a seguir: [Monitorar e escalar um único servidor PostgreSQL usando a CLI do Azure](scripts/sample-scale-server-up-or-down.md)

Dimensionamento das Unidades de Computação é feito independentemente do tamanho máximo de armazenamento que você escolheu.

A alteração da faixa de serviço e/ou nível de desempenho de um banco de dados cria uma réplica do banco de dados original com o novo nível de desempenho e então faz a transição das conexões para réplica. Nenhum dado será perdido durante esse processo. Nenhum dado é perdido durante esse processo, mas durante o breve momento em que realizamos a transição para a réplica, conexões com o banco de dados são desabilitadas, então algumas transações em andamento podem ser revertidas. Essa janela varia, mas é em média inferior a quatro segundos e, em mais de 99% dos casos, de menos de 30 segundos. Se houver grandes números de transações em andamento no momento em que as conexões estiverem desabilitadas, esta janela poderá ser maior.

A duração de todo o processo de expansão depende da faixa de preço e tamanho do servidor antes e após a alteração. Por exemplo, um servidor que está mudando as Unidades de Computação, de, para ou dentro de uma camada de serviço Padrão, deverá concluir dentro de alguns minutos. As novas propriedades do servidor não serão aplicadas até que as alterações sejam concluídas.

## <a name="next-steps"></a>Próximas etapas
- Para obter mais informações, consulte [Explicando Unidades de Computação](concepts-compute-unit-and-storage.md)
- Aprenda a [Monitorar e escalar um único servidor PostgreSQL usando a CLI do Azure](scripts/sample-scale-server-up-or-down.md)

