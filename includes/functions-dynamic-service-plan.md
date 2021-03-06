## <a name="dynamic-service-plan"></a>Plano de serviço Dinâmico
No Plano de serviço Dinâmico, seus aplicativos de função serão atribuídos a uma instância do aplicativo de função. Se necessário, mais instâncias serão adicionadas dinamicamente.
Essas instâncias podem ser distribuídas por vários recursos de computação, obtendo o máximo da infraestrutura do Azure disponível. Além disso, as funções serão executadas em paralelo, minimizando o tempo total necessário para processar solicitações. O tempo de execução para cada função é adicionado, em segundos, e agregado pelo aplicativo de função que a contém. Com o custo orientado pelo número de instâncias, o tamanho da memória e o tempo de execução total, medido em gigabyte/segundos. Essa é uma excelente opção se suas necessidades de computação são intermitentes ou os horários de trabalho tendem a ser muito curtos, pois permite que você pague pelos recursos de computação apenas quando estão realmente em uso.   

### <a name="memory-tier"></a>Camada de memória
Dependendo das necessidades de sua função, você pode selecionar a quantidade de memória necessária para executá-la no aplicativo de função (contêiner de funções).
As opções de tamanho de memória variam de **128 MB a 1536 MB**. O tamanho da memória selecionado corresponde ao conjunto de trabalho necessário para todas as funções que fazem parte do seu aplicativo de função. Se seu código exigir mais memória do que o tamanho selecionado, a instância do aplicativo de função será encerrada devido à falta de memória disponível.

### <a name="scaling"></a>Dimensionamento
A plataforma Azure Functions avaliará as necessidades de tráfego, com base nos gatilhos configurados, para decidir quando escalar verticalmente ou reduzir verticalmente. A granularidade da escala é o aplicativo de função. Escalar verticalmente nesse caso significa adicionar mais instâncias de um aplicativo de função. Inversamente conforme o tráfego diminui, as instâncias do aplicativo de função são desabilitadas, eventualmente reduzindo verticalmente a zero quando não houver nenhuma em execução.  

### <a name="resource-consumption-and-billing"></a>Consumo de recursos e cobrança
No modo Dinâmico, a alocação de recurso é feita de forma diferente do que no plano de Serviço de Aplicativo standard, portanto, o modelo de consumo também é diferente, permitindo um modelo de “pagamento por uso”. O consumo será relatado por aplicativo de função, somente para o tempo em que o código está sendo executado.  
Ele é computado multiplicando o tamanho de memória (em GB) pela quantidade total de tempo de execução (em segundos) para todas as funções em execução dentro do aplicativo de função. A unidade de consumo será **GB-s (Gigabyte/segundos)**.



<!--HONumber=Jan17_HO3-->


