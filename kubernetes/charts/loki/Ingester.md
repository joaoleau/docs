
##### Shipper
Mecanismo de sincronização de index, através dele o index local (/var/loki/tsdb_index ou /var/loki/index), claro onde declarado em "active_index" envia ao determinado mecanismo de store.

Por exemplo, quando configurado o AWS S3 como destino de chunks de logs, também são encaminhados indexes, que podem ser gerados por um determinado mecanismo (apartir da versão 3 recomenda-se o TSDB, porém em antigas temos a presença do boltbd). Esses index a princípio são salvos localmente no path de "active_index" e depois são enviados ao S3, pelo Shipper.

Para boltdb temos polling de envio de index, ou seja, há mais escritas no S3, já quanto ao TSDB isso só ocorre após fechamento de bloco TSDB, que geralmente dura 24h (aparentemente configurado no index -> period no schema config)

##### Object Store

##### Schema Config

