No Amazon Kinesis Data Streams, é possível criar consumidores que usem um recurso chamado _distribuição avançada_. Esse atributo permite que consumidores recebam registros de um fluxo com throughput de até 2 MB de dados por segundo por fragmento. Esse throughput é dedicada, o que significa que consumidores que usam a divisão avançada não precisam lidar com outros consumidores que estejam recebendo dados do fluxo. O Kinesis Data Streams envia registros de dados do fluxo para consumidores que usam a distribuição avançada. Portanto, esses consumidores não precisam sondar dados.
![[Pasted image 20260422211603.png]]
![[Pasted image 20260422211755.png]]


O diagrama a seguir ilustra a arquitetura de alto nível do Kinesis Data Streams. Os _produtores_ enviam dados por push continuamente ao Kinesis Data Streams, que os _consumidores_ processam em tempo real. Os consumidores (como um aplicativo personalizado executado no Amazon EC2 ou em um stream de entrega do Amazon Data Firehose) podem armazenar seus resultados usando um AWS serviço como Amazon DynamoDB, Amazon Redshift ou Amazon S3.

![[Pasted image 20260422212018.png]]### 

### Shard

A _shard_ is a uniquely identified sequence of data records in a stream. A stream is composed of one or more shards, each of which provides a fixed unit of capacity. Each shard can support up to 5 transactions per second for reads, up to a maximum total data read rate of 2 MB per second and up to 1,000 records per second for writes, up to a maximum total data write rate of 1 MB per second (including partition keys). The data capacity of your stream is a function of the number of shards that you specify for the stream. The total capacity of the stream is the sum of the capacities of its shards.

If your data rate increases, you can increase or decrease the number of shards allocated to your stream. For more information, see [Reshard a stream](https://docs.aws.amazon.com/streams/latest/dev/kinesis-using-sdk-java-resharding.html).

### Partition Key

A _partition key_ is used to group data by shard within a stream. Kinesis Data Streams segregates the data records belonging to a stream into multiple shards. It uses the partition key that is associated with each data record to determine which shard a given data record belongs to. Partition keys are Unicode strings, with a maximum length limit of 256 characters for each key. An MD5 hash function is used to map partition keys to 128-bit integer values and to map associated data records to shards using the hash key ranges of the shards. When an application puts data into a stream, it must specify a partition key.

> A _chave de partição_ é usada para agrupar os dados por fragmento dentro de um fluxo. O Kinesis Data Streams segrega os registros de dados pertencentes a um fluxo em vários fragmentos. Ele usa a chave de partição associada a cada registro de dados para determinar a qual fragmento um determinado registro de dados pertence. As chaves de partição são strings Unicode, com um limite de tamanho máximo de 256 caracteres para cada chave. Uma função MD5 hash é usada para mapear chaves de partição para valores inteiros de 128 bits e para mapear registros de dados associados a fragmentos usando os intervalos de chaves de hash dos fragmentos. Quando um aplicativo insere dados em um fluxo, ele deve especificar uma chave de partição.

### Sequence Number

Each data record has a _sequence number_ that is unique per partition-key within its shard. Kinesis Data Streams assigns the sequence number after you write to the stream with `client.putRecords` or `client.putRecord`. Sequence numbers for the same partition key generally increase over time. The longer the time period between write requests, the larger the sequence numbers become.

> Cada registro de dados tem um _número de sequência_ exclusivo por chave de partição dentro do fragmento. O Kinesis Data Streams atribuirá o número de sequência depois que um registro é gravado no fluxo com `client.putRecords` ou `client.putRecord`. Geralmente, os números de sequência da mesma chave de partição aumentam ao longo do tempo. Quanto maior for o período entre as solicitações de gravação, maiores serão os números de sequência.

###### nota

Os números de sequência não podem ser usados como índices para conjuntos de dados dentro do mesmo fluxo. Para separar logicamente conjuntos de dados, use chaves de partição ou crie um fluxo separado para cada conjunto de dados.