O CloudWatch fornece duas categorias de monitoramento: _monitoramento básico_ e _monitoramento detalhado_.

Muitos serviços da AWS oferecem monitoramento básico ao publicar um conjunto padrão de métricas para o CloudWatch sem cobrança para os clientes. Quando você começa a usar um desses Serviços da AWS, o monitoramento básico é habilitado automaticamente por padrão. Para obter uma lista dos serviços que oferecem monitoramento básico, consulte [Produtos da AWS que publicam métricas do CloudWatch](https://docs.aws.amazon.com/pt_br/AmazonCloudWatch/latest/monitoring/aws-services-cloudwatch-metrics.html).

O monitoramento detalhado é oferecido apenas por alguns serviços. Essa modalidade também gera cobranças. Para usá-lo em um serviço da AWS, você deve optar por ativá-lo. Para obter mais informações sobre a definição de preço, consulte [Definição de preço do Amazon CloudWatch](https://aws.amazon.com/cloudwatch/pricing).

As opções do **monitoramento detalhado** diferem com base nos serviços que o oferecem. Por exemplo, o monitoramento detalhado do Amazon EC2 fornece métricas mais frequentes, publicadas em intervalos de **1 minuto**, em vez dos intervalos de 5 minutos usados no monitoramento básico do Amazon EC2. O monitoramento detalhado para o Amazon S3 e o Amazon Managed Streaming for Apache Kafka significa métricas mais refinadas.

Em diferentes serviços da AWS, o monitoramento detalhado também tem nomes diferentes. Por exemplo, no Amazon EC2, ele é chamado de monitoramento detalhado, no AWS Elastic Beanstalk ele é chamado de monitoramento aprimorado. No Amazon S3, ele é chamado de métricas de solicitação.

O uso de monitoramento detalhado para o Amazon EC2 ajuda a gerenciar melhor seus recursos do Amazon EC2, permitindo que você encontre tendências e atue com mais rapidez. Para o Amazon S3, as métricas de solicitação estão disponíveis em intervalos de 1 minuto a fim de ajudar você a identificar e agir rapidamente em problemas operacionais. No Amazon MSK, ao habilitar o monitoramento de nível `PER_BROKER`, `PER_TOPIC_PER_BROKER` ou `PER_TOPIC_PER_PARTITION`, você obtém métricas adicionais que fornecem maior visibilidade.

A tabela a seguir lista os serviços que oferecem monitoramento detalhado. Ela também inclui links para a documentação dos respectivos serviços, explicando mais sobre o monitoramento detalhado e fornecendo instruções sobre como ativá-lo.

## Publicar métricas usando a API do CloudWatch
##  Métricas de alta resolução

Cada métrica é um dos seguintes:

- Resolução padrão, com dados de granularidade de um minuto
    
- Resolução alta, com dados de granularidade de um segundo
    

Por padrão, as métricas produzidas por serviços da AWS têm resolução padrão. Quando você publica uma métrica personalizada, pode defini-la com resolução padrão ou alta. Quando você publica uma métrica de alta resolução, o CloudWatch a armazena com uma resolução de 1 segundo. Você pode ler e recuperar essa métrica no período de 1 segundo, 5 segundos, 10 segundos, 30 segundos ou em qualquer múltiplo de 60 segundos.

As métricas de alta resolução podem também dar a você insight mais imediato da atividade de subminuto da seu aplicativo. Lembre-se de que cada chamada `PutMetricData` de uma métrica personalizada é cobrada. Portanto, chamar `PutMetricData` com mais frequência em uma métrica de alta resolução pode resultar em tarifas mais altas. Para obter mais informações sobre os preços do CloudWatch, consulte [Preço do Amazon CloudWatch](https://aws.amazon.com/cloudwatch/pricing/).

Se você definir um alarme em uma métrica de alta resolução, pode especificar um alarme de alta resolução com um período de 10 ou 30 segundos ou pode definir um alarme regular com um período de qualquer múltiplo de 60 segundos. Há uma tarifa maior para alarmes de alta resolução com um período de 10 ou 30 segundos.