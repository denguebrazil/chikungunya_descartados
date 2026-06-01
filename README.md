#### Algoritmo para análise e validação de critérios de descarte de casos notificados de Febre do Chikungunya

O projeto automatiza a auditoria e a validação dos critérios de encerramento de casos notificados de Chikungunya. O objetivo principal é apoiar a vigilância na identificação de inconsistências nos registros do SINAN, separando os casos que foram descartados corretamente daqueles que possuem sintomas clínicos evidentes e exames ausentes ou fora do prazo oportuno.

Com esta ferramenta, equipes de saúde pública conseguem auditar grandes volumes de dados rapidamente, garantindo que nenhum caso suspeito real seja descartado de maneira inadequada,

Um caso de suspeito só deve ser descartado se houver uma justificativa clínica ou laboratorial sólida. No entanto, erros de preenchimento e na investigação podem fazer com que casos com sintomas clássicos e sem exames laboratoriais acabem recebendo o status de descartado.

O script lê a base de dados bruta de notificações (chikon.dbf) e categoriza os registros em três frentes:

1. Critério Clínico-Epidemiológico: Verifica se o paciente tinha febre e dores nas articulações, mas o exame não foi realizado.

2. Critério Laboratorial: Avalia se o exame de PCR foi coletado no tempo correto (até 8 dias do início dos sintomas) e se o resultado justifica o descarte.

3. Casos em Investigação: Analisa os registros que ainda estão abertos para identificar quais já reúnem critérios para avaliação imediata.

Ao final, o programa gera um relatório consolidado em Excel indicando se o descarte de cada caso foi Adequado ou Não Adequado (inconsistente).

#### Pipeline Análise

[Dados Brutos: chikon.xlsx]
          │
          ▼
 [Limpeza do Banco de Dados] ──> Remove dados pessoais/irrelevantes
          │
          ▼
 ┌────────┴──────────────────────────┐
 │ Classificação por Grupos (Regras) │
 └────────┬──────────────────────────┘
          ├─> 1. Clínico-Epidemiológico (Filtra Critério 2 + Descartados)
          ├─> 2. Laboratorial (Filtra Critério 1 + Descartados)
          └─> 3. Em Investigação (Filtra os casos sem encerramento)
          │
          ▼
[Geração do Relatório Final: chik_descartados_total.xlsx]

Importante!
O arquivo Chikon, baixado no formato [dbf], deve ser convertido em Pasta de Trabalho Excel [.xlsx] com o nome chikon_[siglaestado].xlsx

#### Preparação do ambiente
1 - Instalar o Python 3
2 - Instalar bibliotecas [pip install pandas numpy openpyxl notebook]
3 - Excel
4 - Editor de código (VS Code, Anaconda, Google Colab)

#### Execução do Projeto
Organize seus arquivos: Certifique-se de que o arquivo bruto (extensão .xlsx) esteja na mesma pasta que o arquivo do código (chik_descartados.ipynb).

#### Arquivos Gerados
Após o término da execução, o programa salvará automaticamente os seguintes arquivos na sua pasta:

chikon_notificados.xlsx: Uma versão limpa da base original, mantendo apenas as colunas essenciais para a epidemiologia.

chik_descartados_total.xlsx: O relatório final contém 4 abas (Planilhas):

clinico_epidemiologico: Casos encerrados por este critério, com a coluna DESCARTE_CRITERIO indicando se foi adequado ou não.

laboratorial: Casos encerrados por laboratório, detalhando a diferença de dias entre os sintomas e a coleta (DIF_DIAS) e se a coleta foi oportuna (DIAG_OPORTUNO).

investigacao: Casos que ainda não foram fechados no sistema, sinalizando quais já precisam de avaliação.

notificados: A base consolidada de suporte.

#### Regra aplicada a adequabilidade do descarte
1 - Caso Suspeito Padrão: Paciente que apresenta Febre (FEBRE == 1) E manifestação articular, que pode ser Artralgia (ARTRALGIA == 1) OU Artrite (ARTRITE == 1).

2 - Descarte Não Adequado (Inconsistente): Se o paciente se encaixa na definição de caso suspeito acima, mas o exame de PCR consta como Não Realizado ou está em branco, o sistema classifica como n_adequado, pois faltam subsídios para descartar o caso.

Período Oportuno (Laboratorial): O cálculo da diferença de dias entre a Data de Início dos Sintomas (DT_SIN_PRI) e a Data da Coleta do PCR (DT_PCR) deve ser de 0 a 8 dias. Se um caso foi descartado com exame negativo colhido fora desse prazo (ex: no 12º dia), o sistema acusará como n_adequado, exigindo revisão da vigilância.

Algoritmo desenvolvido por Dengue Brazil
Pode ser utilizado livremente, desde que citada a fonte.
