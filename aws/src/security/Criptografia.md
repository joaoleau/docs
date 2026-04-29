
## AWS CodeCommit:
- Armazene seu código com segurança. CodeCommit os repositórios são criptografados tanto em repouso quanto em trânsito. Não precisa configurar KMS manualmente.
## S3

**SSE-C (Server-Side Encryption with Customer-Provided Keys):** método de criptografia do **Amazon S3** em que o **cliente fornece sua própria chave de criptografia**. O S3 usa essa chave temporariamente para criptografar e descriptografar os dados, sem armazená-la. É obrigatório o uso de **HTTPS** para proteger a chave em trânsito.  

**Client-Side Encryption:** abordagem em que os **dados são criptografados antes do envio ao S3**, permanecendo criptografados durante todo o armazenamento. O servidor S3 nunca vê a chave nem os dados em texto puro.  

**SSE-KMS (Server-Side Encryption with AWS KMS Keys):** criptografia gerenciada pelo **AWS Key Management Service**, que controla, audita e protege as chaves de forma centralizada, integrando-se com políticas de IAM e logs de auditoria.  

**SSE-S3 (Server-Side Encryption with Amazon S3-Managed Keys):** criptografia gerenciada automaticamente pelo **Amazon S3**, em que as chaves são criadas e armazenadas pelo próprio serviço, sem necessidade de configuração adicional.

---

Como parte das normas internas, você deve garantir que todas as comunicações com o Amazon S3 sejam criptografadas. Para qual dos seguintes mecanismos de criptografia uma requisição será rejeitada caso a conexão não utilize HTTPS?

- (Incorreta) **SSE-KMS (Server-Side Encryption with AWS KMS Keys)**. 
	A criptografia do lado servidor usando chaves gerenciadas pelo AWS KMS não exige que a conexão seja via HTTPS para que a requisição seja aceita. Embora o uso de HTTPS seja uma boa prática, o Amazon S3 não rejeita requisições HTTP com SSE-KMS.

- (Correta) **SSE-C (Server-Side Encryption with Customer-Provided Keys)**. :
	A criptografia do lado servidor com chaves fornecidas pelo cliente (SSE-C) exige que a chave de criptografia seja enviada junto com a requisição. O Amazon S3 rejeita qualquer requisição feita via HTTP (não segura) para proteger a chave, pois o envio da chave por HTTP pode comprometer sua segurança. Portanto, o uso de HTTPS é obrigatório para SSE-C.

- (Incorreta) **SSE-S3 (Server-Side Encryption with Amazon S3-Managed Keys):**
	 A criptografia do lado servidor com chaves gerenciadas pelo próprio Amazon S3 (SSE-S3) não exige que a conexão seja via HTTPS para aceitar a requisição. O Amazon S3 criptografa os dados em repouso, mas não rejeita requisições HTTP.

---

| Modelo                 |                                                                                                                                                                     Cabeçalhos principais | Quem gerencia a chave |    S3 armazena a chave? |                      HTTPS obrigatório | Auditoria / logs                        | Observação / CRR                                                                             |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | --------------------- | ----------------------: | -------------------------------------: | --------------------------------------- | -------------------------------------------------------------------------------------------- |
| SSE‑S3                 |                                                                                                                                                    `x-amz-server-side-encryption: AES256` | Amazon S3             |                     Sim |                            Recomendado | Sem logs KMS detalhados                 | Simples, sem custo KMS; funciona com CRR direto                                              |
| SSE‑KMS                |                                                                            `x-amz-server-side-encryption: aws:kms`<br/>`x-amz-server-side-encryption-aws-kms-key-id: <key-id>` (opcional) | AWS KMS (CMK)         |               Sim (KMS) |                            Recomendado | Auditável via CloudTrail (chamadas KMS) | Requer configurar CMKs nas regiões destino para CRR; há custo por chamada KMS                |
| SSE‑C                  | `x-amz-server-side-encryption-customer-algorithm: AES256`<br/>`x-amz-server-side-encryption-customer-key: <Base64-key>`<br/>`x-amz-server-side-encryption-customer-key-MD5: <Base64-MD5>` | Cliente (você)        |                     Não |                            Obrigatório | Sem logs KMS (cliente responsável)      | Chave deve ser enviada em cada requisição; complica CRR/replicação                           |
| Client‑Side Encryption |                                                   Nenhum cabeçalho S3 padronizado (dados já cifrados); SDKs gravam `x-amz-meta-*` (ex.: `x-amz-meta-x-amz-key-v2`, `x-amz-meta-x-amz-iv`) | Cliente (você)        | Não (apenas ciphertext) | Recomendado (para metadados sensíveis) | Sem logs KMS (cliente responsável)      | S3 nunca vê plaintext; CRR replica ciphertext — decriptação só onde chave estiver disponível |
