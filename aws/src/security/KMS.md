# 1) Conceitos-chave

## KMS

O Amazon Web Services KMS é um serviço de **gerenciamento de chaves**, não de criptografia em massa.

---

## CMK (Customer Managed Key)

- Chave principal armazenada no KMS
- Nunca sai do KMS
- Usada para **criptografar outras chaves**, não grandes volumes de dados

---

## Data Key

- Chave simétrica (geralmente **AES-256**)
- Usada para criptografar os dados de fato
- Pode existir como:
    - plaintext (temporário)
    - ciphertext (armazenado)

---

## Envelope Encryption

Modelo padrão do KMS:

- Dados são criptografados com uma **data key**
- A data key é criptografada com a **CMK**

Isso resolve:

- performance (KMS não vira gargalo)
- segurança (CMK nunca sai do serviço)

---

# 2) Fluxo padrão (o mais importante)

## Cenário: aplicação criptografando dados

### Passo 1 — Gerar data key

GenerateDataKey

Retorna:

- plaintext data key
- data key criptografada (com CMK)

---

### Passo 2 — Criptografia local

- algoritmo típico: AES-256
- uso: arquivos, JSON, payloads

---

### Passo 3 — Descartar plaintext

- não persistir
- reduzir exposição

---

### Passo 4 — Armazenar

- dado criptografado
- data key criptografada

---

## Descriptografia

1. Envia data key criptografada → `Decrypt`
2. KMS retorna plaintext
3. Aplicação descriptografa os dados

---

# 3) Fluxo sem plaintext

## Operação:

GenerateDataKeyWithoutPlaintext

Retorna:

- apenas a data key criptografada

---

## Quando usar

### Cenário 1 — Segurança máxima

- chave não pode existir em memória
- você só chama `Decrypt` no momento exato de uso

---

### Cenário 2 — Criptografia tardia

- você ainda não precisa criptografar o dado
- armazena a chave criptografada para uso futuro

---

### Cenário 3 — Serviços gerenciados

Ex:

- Amazon S3
- Amazon EBS

Você não vê a chave — o serviço usa KMS internamente.

---

# 4) Operações importantes (cai em prova)

- `Encrypt` → pequenos dados (não comum em escala)
- `Decrypt` → recuperar plaintext
- `GenerateDataKey` → padrão para dados grandes
- `GenerateDataKeyWithoutPlaintext` → reduzir exposição
- `ReEncrypt` → trocar CMK sem descriptografar localmente

---

# 5) Diferença essencial

|Elemento|Função|
|---|---|
|CMK|protege a data key|
|Data key (plaintext)|criptografa dados|
|Data key (ciphertext)|armazenada com segurança|

---

# 6) Cenários clássicos de prova

## Cenário A

“Aplicação precisa criptografar grandes volumes com alta performance”

Resposta:

- Envelope Encryption
- `GenerateDataKey`
- AES local

---

## Cenário B

“Minimizar exposição de chave sensível”

Resposta:

- `GenerateDataKeyWithoutPlaintext`
- Decrypt sob demanda

---

## Cenário C

“Rotacionar chave sem reprocessar dados”

Resposta:

- `ReEncrypt`

---

## Cenário D

“Controle de acesso à chave”

Resposta:

- Key Policy + IAM (ambos necessários)

---

# 7) Erros comuns

- achar que KMS criptografa arquivos grandes diretamente
- esquecer de descartar plaintext
- confundir CMK com data key
- ignorar Key Policy

---

# 8) Resumo mental (o que você precisa lembrar)

1. KMS protege chaves, não dados grandes
2. Dados são criptografados com AES usando data key
3. Data key é protegida pela CMK (envelope encryption)
4. Ciphertext da key é o que você armazena
5. Plaintext só existe temporariamente