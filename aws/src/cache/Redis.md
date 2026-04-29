
**Visão Geral**

- **Serviço**: ElastiCache for Redis — Redis gerenciado pela AWS, cache in‑memory com persistência opcional, replicação e recursos de dados ricos.
- **Uso comum**: caches, sessões, filas leves, leaderboards, pub/sub, operações atômicas e dados com estrutura (listas, sets, hashes, sorted sets).

**Modos de implantação**

- **Cluster Mode Disabled**: um primário (writer) com 0..N réplicas (read replicas). Escala leitura adicionando réplicas; failover automático para réplica se habilitado.
- **Cluster Mode Enabled (sharding)**: dados particionados em múltiplos shards (node groups). Cada shard tem um primário e 0..N réplicas. Permite partição horizontal (mais capacidade e memória agregada) e redimensionamento de shards online.

**Nodes e endpoints**

- **Primário (primary/master)**: aceita gravações.
- **Réplica (replica/read-only)**: atende leituras, replica dados do primário.
- **Endpoints**:
    - **Primary endpoint** (cluster disabled): endpoint do primário para escrita.
    - **Reader endpoints** (quando aplicável): para distribuir leituras entre réplicas.
    - **Configuration/Cluster endpoints** (cluster enabled): resolve para shards/primários corretos; também há endpoints por shard (shard endpoints).
- **Tipos de instância**: escolha conforme memória/CPU; para produção use tipos de instância com memória suficiente e I/O adequado.

**Persistência e sincronização**

- **Snapshots (RDB)**: ElastiCache permite snapshots pontuais e backups automáticos (RDB) armazenados no S3 para restauração.
- **Sincronização primário→réplicas**: réplicas fazem replicação assíncrona/semisíncrona do primário; durante failover réplicas podem ser promovidas a primário.
- **Recuperação**: ao adicionar réplica nova ou após falha, é feito resync usando snapshot e/o sincronização incremental.

**Alta disponibilidade — Cross‑AZ / Multi‑AZ**

- **Multi‑AZ com Auto‑Failover**: quando habilitado, pelo menos uma réplica deve existir em outra AZ; se primário falhar, AWS promove réplica em outra AZ automaticamente (minimiza downtime).
- **Cross‑AZ**: réplicas distribuídas via Subnet Groups em múltiplas AZs para tolerância a falhas locais.
- **Cross‑Region (Global Datastore)**: ElastiCache Global Datastore permite replicação entre regiões para recuperação de desastres e leituras globais (clusters secundários read‑only em outra região). Não é replicação multi‑mestre — escrita apenas no primário.

**Escalabilidade**

- **Vertical**: troca de tipo de instância (scale-up/down) — geralmente envolve reinicialização.
- **Horizontal**:
    - **Cluster Mode Enabled**: adicionar/remover shards (resharding) para escalar capacidade agregada.
    - **Cluster Mode Disabled**: escala de leitura via réplicas; não há shard automático.

**Segurança e operações**

- **Autenticação e criptografia**: suporte a Redis AUTH; TLS para criptografia in‑transit; encryption at‑rest via KMS.
- **Network**: implantado em VPC; controle com Security Groups e Subnet Groups.
- **Parâmetros**: Parameter Groups para ajustar config do Redis.
- **Monitoramento**: CloudWatch métricas, logs, eventos; alertas para latência, evictions, CPU, uso de memória.
- **Backups/restore e manutenção**: snapshots programáveis; janelas de manutenção e atualizações de engine aplicadas pela AWS.

**Diferenças principais entre Redis (ElastiCache) e Memcached**

- **Persistência**: Redis suporta persistência (snapshots/AOF); Memcached é volátil (sem persistência).
- **Estruturas de dados**: Redis oferece strings, lists, sets, sorted sets, hashes, bitmaps, hyperloglog; Memcached é apenas key→value simples.
- **Replicação e HA**: Redis suporta réplicas e failover; Memcached não tem réplica nativa nem failover automático.
- **Sharding**: Memcached escala horizontalmente adicionando nós (cliente faz hashing); Redis Cluster fornece sharding nativo com gerenciamento do cluster.
- **Multi‑threading**: Memcached é multi‑threaded; Redis é single‑threaded por shard (alto desempenho por núcleo).
- **Casos de uso**: escolha Memcached para caches simples, alta taxa de throughput e latência muito baixa; escolha Redis para funcionalidades avançadas, consistência, persistência e operações atômicas.

**Limitações/considerações para prova**

- **Auto‑failover requer réplicas e Multi‑AZ habilitado.**
- **Cluster Mode Enabled é necessário para sharding nativo (particionamento de chaves).**
- **Global Datastore = cross‑region read‑replicas / DR, não escrita multi‑master.**
- **Backups são snapshots; recuperação depende da restauração de snapshot/replica.**
- **Segurança: use VPC + Security Groups + TLS + AUTH + KMS para produção.**
- **Escolha entre Redis x Memcached baseado em requisitos de dados (persistência/estruturas/replicação) e escalabilidade.**

![[Pasted image 20260425131140.png]]
![[Pasted image 20260425131205.png]]

![[Pasted image 20260425131223.png]]
