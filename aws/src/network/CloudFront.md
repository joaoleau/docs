**É possível configurar o CloudFront para exigir que os visualizadores usem HTTPS para as suas conexões sejam criptografadas durante a comunicação do CloudFront com os visualizadores. Também é possível configurar o CloudFront para usar HTTPS com sua origem a fim de que as conexões sejam criptografadas quando o CloudFront se comunicar com os usuários.**
## Personalizar na borda com o CloudFront Functions
Com o CloudFront Functions, você pode escrever funções leves em JavaScript para personalizações de CDN de alta escala e sensíveis à latência. Suas funções podem manipular as solicitações e respostas que fluem pelo CloudFront, executar autenticação e autorização básicas, gerar respostas HTTP na borda e muito mais. O ambiente de tempo de execução do CloudFront Functions oferece tempos de startup de submilissegundos, é dimensionado imediatamente para lidar com milhões de solicitações por segundo e é altamente seguro. O CloudFront Functions é um recurso nativo do CloudFront, o que significa que você pode criar, testar e implantar seu código inteiramente no CloudFront.

Quando você associa uma função do CloudFront a uma distribuição do Lambda, o CloudFront intercepta solicitações e respostas nos locais da borda do CloudFront e os passa à sua função. Você pode invocar CloudFront Functions quando ocorrerem os seguintes eventos:

- Quando o CloudFront receber uma solicitação de um visualizador (solicitação de visualizador).
- Antes de o CloudFront exibir a resposta para o visualizador (resposta ao visualizador).
- Durante o estabelecimento da conexão TLS (solicitação de conexão): no momento, disponível para conexão TLS mútua (mTLS).

**Diferenças entre o CloudFront Functions e o Lambda@Edge**
O CloudFront Functions é ideal para funções leves e curtas para os seguintes casos de uso:
- **Normalização da chave de cache**: transforme atributos de solicitação HTTP (cabeçalhos, strings de consulta, cookies e até mesmo o caminho do URL) para criar uma [chave de cache](https://docs.aws.amazon.com/pt_br/AmazonCloudFront/latest/DeveloperGuide/understanding-the-cache-key.html) ideal, que pode melhorar a taxa de acertos do cache.
- **Manipulação de cabeçalhos**: insira, modifique ou exclua cabeçalhos HTTP na solicitação ou na resposta. Por exemplo, você pode adicionar um cabeçalho `True-Client-IP` a cada solicitação.
- **Redirecionamento ou regravações de URL**: redirecione os visualizadores para outras páginas com base nas informações da solicitação ou reescreva todas as solicitações de um caminho para outro.
- **Solicitar autorização**: valide tokens de autorização com hash, como tokens Web JSON (JWT), por meio da inspeção dos cabeçalhos de autorização ou outros metadados de solicitação.


O **Lambda@Edge** é ideal para os seguintes casos de uso:
- Funções que levam alguns milissegundos ou mais para serem concluídas.
- Funções que exigem CPU ou memória ajustável.
- Funções que dependem de bibliotecas de terceiros (incluindo o SDK da AWS para integração com outros Serviços da AWS).
- Funções que exigem acesso à rede para usar serviços externos para processamento.
- Funções que exigem acesso ao sistema de arquivos ou acesso ao corpo de solicitações HTTP.
![[Pasted image 20260424205628.png]]
## Usar URLs assinados

### Decidir usar políticas predefinidas ou personalizadas para URLs assinados

![[Pasted image 20260424210144.png]]

### Como signed URLs funcionam
A seguir, uma visão geral de como configurar o CloudFront e o Amazon S3 para signed URLs e como o CloudFront responde quando um usuário usa um signed URL para solicitar um arquivo.

1. Na sua distribuição do CloudFront, especifique **um ou mais grupos de chaves confiáveis, que contenham as chaves públicas que o CloudFront pode usar para verificar a assinatura do URL. Use as chaves privadas correspondentes para assinar os URLs.**
    O CloudFront permite URLs assinados com assinaturas de chave RSA 2048 e ECDSA 256.
    Para obter mais informações, consulte [Especificar os assinantes que podem criar URLs e cookies assinados](https://docs.aws.amazon.com/pt_br/AmazonCloudFront/latest/DeveloperGuide/private-content-trusted-signers.html).
2. Desenvolva sua aplicação para determinar se um usuário deve ter acesso a seu conteúdo e criar signed URLs para os arquivos ou partes da aplicação às quais você deseja restringir o acesso. Para obter mais informações, consulte os tópicos a seguir:
    - [Criar um URL assinado usando uma política predefinida](https://docs.aws.amazon.com/pt_br/AmazonCloudFront/latest/DeveloperGuide/private-content-creating-signed-url-canned-policy.html)
    - [Criar um URL assinado usando uma política personalizada](https://docs.aws.amazon.com/pt_br/AmazonCloudFront/latest/DeveloperGuide/private-content-creating-signed-url-custom-policy.html)
3. Um usuário solicita um arquivo para o qual você deseja exigir signed URLs.
4. Seu aplicativo verifica se o usuário está autorizado a acessar o arquivo: ele fez login, pagou para acessar o conteúdo ou atendeu a outro requisito de acesso.
5. O aplicativo cria e retorna um signed URL para o usuário.
6. O signed URL permite que o usuário faça download ou transmita o conteúdo.
    Essa etapa é automática; o usuário geralmente não precisa fazer nada a mais para acessar o conteúdo. Por exemplo, se um usuário estiver acessando seu conteúdo em um navegador da Web, a aplicação retornará o signed URL para o navegador. O navegador imediatamente usa o signed URL para acessar o arquivo no ponto de presença de caches do CloudFront sem intervenção do usuário.
    
7. O CloudFront usa a chave pública para validar a assinatura e confirmar se o URL não foi adulterado. Se a assinatura for inválida, a solicitação será rejeitada.
    Se a assinatura for válida, o CloudFront analisará a declaração de política no URL (ou criará uma se você estiver usando uma política padrão) para confirmar se a solicitação continua válida. Por exemplo, se você especificou uma data e hora de início e término para o URL, o CloudFront confirmará se o usuário está tentando acessar o conteúdo durante o período de acesso permitido.
    Se a solicitação cumprir os requisitos da declaração de política, o CloudFront executará as operações padrão: determinar se o arquivo já está no ponto de presença de caches, encaminhar a solicitação para a origem, se necessário, e retornar o arquivo para o usuário.


> Se um URL não assinado contiver parâmetros de string de consulta, certifique-se de incluí-los na parte do URL que você assinar. Se você adicionar uma string de consulta a um signed URL depois de assiná-lo, o URL retornará um status HTTP 403.

