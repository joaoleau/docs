É possível usar o Amazon Kinesis Data Streams para coletar e processar grandes [fluxos](https://aws.amazon.com/streaming-data/) de registros de dados em tempo real. É possível criar aplicações de processamento de dados, conhecidas como _aplicações do Kinesis Data Streams_. Uma aplicação típica do Kinesis Data Streams lê dados de um _fluxo de dados_ como registros de dados. Esses aplicativos podem usar a **Kinesis Client Library** e podem ser executados em instâncias da Amazon EC2. É possível enviar os registros processados para painéis, usá-los para gerar alertas, alterar dinamicamente as estratégias de preços e publicidade ou enviar dados para vários outros serviços da AWS .
- O tipo de dados usado pode incluir dados de log de infraestrutura de TI, logs de aplicativo, mídias sociais, feeds de dados de mercado e dados de sequência de cliques da web. Como o tempo de resposta para a entrada e o processamento de dados é em tempo real, o processamento geralmente é leve.


Embora seja possível usar o Kinesis Data Streams para resolver vários problemas de dados em fluxo, um uso comum é agregar dados em tempo real e carregar os dados agregados em um data warehouse ou cluster de redução de mapa.

Os dados são colocados em fluxos de dados do Kinesis, o que garante durabilidade e elasticidade. **O atraso entre o momento em que um registro é colocado no stream e o tempo em que ele pode ser recuperado (put-to-get atraso) geralmente é inferior a 1 segundo.** Em outras palavras, uma aplicação do Kinesis Data Streams pode começar a consumir os dados do fluxo quase que imediatamente após sua adição. Como é um serviço gerenciado, o Kinesis Data Streams cuida da carga operacional de criar e executar um pipeline de entrada de dados. É possível criar aplicações de redução de mapa de fluxo. A elasticidade do Kinesis Data Streams permite escalar o fluxo. Assim, registros de dados nunca são perdidos antes que exirem.

> Como várias aplicações do Kinesis Data Streams podem consumir dados de um fluxo, múltiplas ações como arquivamento e processamento podem ocorrer de maneira simultânea e independente. Por exemplo, dois aplicativos podem ler dados do mesmo fluxo. A primeira aplicação calcula agregados em execução e atualiza uma tabela do Amazon DynamoDB, e a segunda compacta e arquiva dados em um datastore, como o Amazon Simple Storage Service (Amazon S3). A tabela do DynamoDB com agregados em execução é então lida por um painel para relatórios. up-to-the-minute.

V**ocê pode criar consumidores para o Kinesis Data Streams usando a Kinesis Client Library (KCL) ou AWS SDK para Java**. Você também pode desenvolver consumidores usando outros AWS serviços AWS Lambda, como o Amazon Managed Service for Apache Flink e o Amazon Data Firehose. O Kinesis Data Streams oferece suporte a integrações AWS com outros serviços, como Amazon EMR, Amazon e Amazon Redshift. Ele também oferece suporte a integrações de terceiros EventBridge AWS Glue, incluindo Apache Flink, Adobe Experience Platform, Apache Druid, Apache Spark, Databricks, Confluent Platform, Kinesumer e Talend.

---

 #### **Kinesis Agent**
 O **Kinesis Agent** é um aplicativo Java independente que oferece uma maneira simples e eficiente de coletar e enviar dados para o Kinesis Data Streams. Ele monitora continuamente um conjunto de arquivos, enviando novos dados para o stream, gerenciando rotação de arquivos, checkpoints e tentativas em caso de falhas. Além disso, emite métricas no Amazon CloudWatch para monitoramento e solução de problemas, tornando-o ideal para ambientes Linux como servidores web, de logs e bancos de dados.

---
### Diferenças Kinesis Video Streams, Data Streams, Data Firehose e Data Analytics.

**Kinesis Video Streams**
![[Pasted image 20260425124352.png]]

O Kinesis Video Streams é um serviço projetado para a ingestão, processamento e armazenamento de fluxos de vídeo em tempo real. Ele permite a captura de vídeo de dispositivos, como câmeras, e a transmissão contínua desses dados para a AWS.
- **Fornece recursos avançados, como indexação automática, análise de vídeo em tempo real e a capacidade de reproduzir vídeos gravados.**
	O Kinesis Video Streams é ideal para aplicativos que requerem streaming de vídeo, como vigilância inteligente, análise de vídeo e transmissão ao vivo.

**Kinesis Data Streams**
![[Pasted image 20260425124520.png]]
O Kinesis Data Streams é um serviço de streaming de dados em tempo real que permite a captura e o processamento de grandes volumes de dados em tempo real.
- Ele funciona com base no **conceito de shards**, que são unidades de capacidade de streaming.
- Os dados enviados para um s**tream são divididos em shards e podem ser processados por consumidores em paralelo.**
	O Kinesis Data Streams é altamente escalável e durável, permitindo que os aplicativos processem e analisem dados em tempo real, como dados de logs, eventos de IoT e métricas de aplicativos. **(pode armazenar até 365 dias)**

**Kinesis Data Firehose**
![[Pasted image 20260425124723.png]]
O Kinesis Data Firehose é um serviço que permite a entrega direta de dados de streaming para serviços de armazenamento da AWS, como o Amazon S3, Amazon Redshift e Amazon Elasticsearch. 
- Ele s**implifica o processo de ingestão e armazenamento de dados em escala, eliminando a necessidade de configurar e gerenciar infraestrutura adicional.**
- **O Kinesis Data Firehose pode transformar, comprimir e criptografar dados antes de entregá-los aos destinos de armazenamento.**
- É **ideal para cenários em que os dados de streaming precisam ser armazenados em um local persistente para análise futura**, como registros de aplicativos e dados de sensor IoT.

**Kinesis Data Analytics**
![[Pasted image 20260425125159.png]]
O Kinesis Data Analytics é um serviço que permite o processamento em tempo real e a **análise de dados de streaming usando consultas SQL padrão.**
- Ele facilita a escrita de consultas para transformar, filtrar e agregar dados de streaming em tempo real.
-  O Kinesis Data Analytics suporta várias fontes de dados, como Kinesis Data Streams, Kinesis Data Firehose e até mesmo streams personalizados. Ele fornece recursos avançados, como janelas de tempo, agregações e suporte para funções personalizadas, permitindo análises em tempo real de dados de streaming em larga escala.

	Cada serviço Kinesis aborda um aspecto específico da arquitetura de streaming de dados. Ao escolher o serviço adequado, é importante considerar os requisitos do aplicativo e os casos de uso específicos. **O Kinesis Video Streams é ideal para streaming de vídeo em tempo real**, enquanto o **Kinesis Data Streams é mais adequado para captura e processamento de dados em tempo real em grande escala.** **O Kinesis Data Firehose é a escolha certa quando o objetivo é entregar dados de streaming diretamente para serviços de armazenamento**, enquanto o **Kinesis Data Analytics é indicado para realizar análises em tempo real sobre os dados de streaming**.