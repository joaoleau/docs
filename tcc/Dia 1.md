01/03/2026

Decidi o Nome: Floslo
Ideia: Despertador

Código Terraform:
```
resource "aws_s3vectors_vector_bucket" "floslo-vector" {

  vector_bucket_name = var.vector_bucket_name

}

  

resource "aws_s3vectors_index" "vector-index" {

  index_name         = var.vector_index

  vector_bucket_name = aws_s3vectors_vector_bucket.floslo-vector.vector_bucket_name

  

  data_type       = "float32"

  dimension       = 1024

  distance_metric = var.distance_metric

  

  metadata_configuration {

    non_filterable_metadata_keys = ["AMAZON_BEDROCK_TEXT", "AMAZON_BEDROCK_METADATA"]

  }

}

  

resource "aws_bedrockagent_knowledge_base" "floslo-kb" {

  name     = "floslo-kb"

  role_arn = aws_iam_role.floslo-kb-role.arn

  

  knowledge_base_configuration {

    vector_knowledge_base_configuration {

      embedding_model_arn = var.embedding_model_arn

      embedding_model_configuration {

        bedrock_embedding_model_configuration {

          dimensions          = 1024

          embedding_data_type = "FLOAT32"

        }

      }

    }

    type = "VECTOR"

  }

  

  storage_configuration {

    type = "S3_VECTORS"

    s3_vectors_configuration {

      index_arn = aws_s3vectors_index.vector-index.index_arn

    }

  }

}

  

resource "aws_bedrockagent_data_source" "floslo-ds" {

  knowledge_base_id = aws_bedrockagent_knowledge_base.floslo-kb.id

  name              = "floslo-ds"

  

  data_source_configuration {

    type = "S3"

    s3_configuration {

      bucket_arn         = data.aws_s3_bucket.floslo-bucket.arn

      inclusion_prefixes = [var.s3_context_prefix]

    }

  }

  

  vector_ingestion_configuration {

    chunking_configuration {

      chunking_strategy = "FIXED_SIZE"

      fixed_size_chunking_configuration {

        max_tokens         = var.chunk_max_tokens

        overlap_percentage = var.chunk_overlap_percentage

      }

    }

  }

}
```

```
resource "aws_iam_role" "floslo-kb-role" {

  name = "floslo-kb"

  

  assume_role_policy = jsonencode({

    Version = "2012-10-17"

    Statement = [

      {

        Action = "sts:AssumeRole"

        Effect = "Allow"

        Sid    = ""

        Principal = {

          Service = "bedrock.amazonaws.com"

        }

      },

    ]

  })

}

  

resource "aws_iam_role_policy" "s3-floslo-bucket-ro-policy" {

  name = "s3-floslo-bucket-ro-policy"

  role = aws_iam_role.floslo-kb-role.id

  

  policy = jsonencode({

    Version = "2012-10-17"

    Statement = [

      {

        Sid    = "S3BucketReadPermission"

        Effect = "Allow"

        Action = [

          "s3:GetObject",

          "s3:ListBucket"

        ]

        Resource = [

          data.aws_s3_bucket.floslo-bucket.arn,

          "${data.aws_s3_bucket.floslo-bucket.arn}/*"

        ]

      }

    ]

  })

}

  

resource "aws_iam_role_policy" "s3-floslo-vector-bucket-rw-policy" {

  name = "s3-floslo-vector-bucket-rw-policy"

  role = aws_iam_role.floslo-kb-role.id

  

  policy = jsonencode({

    Version = "2012-10-17"

    Statement = [

      {

        Sid = "S3VectorBucketReadAndWritePermission",

        Action = [

          "s3vectors:PutVectors",

          "s3vectors:GetVectors",

          "s3vectors:DeleteVectors",

          "s3vectors:QueryVectors",

          "s3vectors:GetIndex"

        ]

        Effect   = "Allow"

        Resource = aws_s3vectors_index.vector-index.index_arn

      },

    ]

  })

}

  

resource "aws_iam_role_policy" "bedrock-model-invoke" {

  name = "bedrock-model-invoke"

  role = aws_iam_role.floslo-kb-role.id

  

  policy = jsonencode({

    Version = "2012-10-17"

    Statement = [

      {

        Effect = "Allow",

        Action = [

          "bedrock:InvokeModel",

          "bedrock:Retrieve",

          "bedrock:RetrieveAndGenerate"

        ],

        Resource = var.embedding_model_arn

      }

    ]

  })

}
```

![[Pasted image 20260301225106.png]]

![[Pasted image 20260301225124.png]]

![[Pasted image 20260301225235.png]]

```
User: Gere 1 questão de múltipla escolha baseada exclusivamente no CONTEXTO fornecido.

Regras:
- 5 alternativas
- Apenas 1 correta
- Explicação da alternativa correta
- Responda somente em JSON
- Não escreva nada fora do JSON
- Se o contexto não for suficiente, responda:
{"error":"informacao insuficiente"}

Formato obrigatório:

{
  "pergunta": "",
  "alternativas": [
    {"id": "A", "texto": ""},
    {"id": "B", "texto": ""},
    {"id": "C", "texto": ""},
    {"id": "D", "texto": ""},
    {"id": "E", "texto": ""}
  ],
  "resposta_correta": "",
  "explicacao": ""
}

Ai: {
  "pergunta": "Qual é a função principal do módulo de DMA no contexto da transferência de dados entre a memória e um dispositivo de E/S?",
  "alternativas": [
    {"id": "A", "texto": "Executar instruções de processamento"},
    {"id": "B", "texto": "Transferir dados diretamente entre a memória e o dispositivo de E/S sem envolver o processador"},
    {"id": "C", "texto": "Gerenciar a interrupção do processador"},
    {"id": "D", "texto": "Realizar cálculos matemáticos complexos"},
    {"id": "E", "texto": "Controlar o barramento do sistema"}
  ],
  "resposta_correta": "B",
  "explicacao": "O módulo de DMA é responsável por transferir dados diretamente entre a memória e o dispositivo de E/S sem envolver o processador, o que é descrito na passagem como uma técnica mais eficiente para mover grandes volumes de dados
```

![[Pasted image 20260301225423.png]]![[Pasted image 20260301225534.png]]
![[Pasted image 20260301225729.png]]