## Algoritmo para análise e validação de critérios de descarte de casos notificados de Febre do Chikungunya

O projeto automatiza a auditoria e a validação dos critérios de encerramento de casos notificados de Chikungunya. O objetivo principal é apoiar a vigilância na identificação de inconsistências nos registros do SINAN, separando os casos que foram descartados corretamente daqueles que possuem sintomas clínicos evidentes e exames ausentes ou fora do prazo oportuno.

Com esta ferramenta, equipes de saúde pública conseguem auditar grandes volumes de dados rapidamente, garantindo que nenhum caso suspeito real seja descartado de maneira inadequada,

Um caso de suspeito só deve ser descartado se houver uma justificativa clínica ou laboratorial sólida. No entanto, erros de preenchimento e na investigação podem fazer com que casos com sintomas clássicos e sem exames laboratoriais acabem recebendo o status de descartado.

Para o correto descarte, é importante determinar a definição de caso suspeito e confirmado de Febre do Chikungunya. De acordo com o Ministério da Saúde (2026) 

Caso suspeito: "trata-se do indivíduo que apresenta febre de início súbito, acompanhada de artralgia ou artrite intensa (dor nas articulações) de início agudo, não explicado por outras condições, residente em (ou que tenha visitado) áreas com transmissão até duas semanas antes de começar os sintomas, ou que tenha vínculo epidemiológico com caso confirmado".

Caso Confirmado: É todo caso suspeito que foi confirmado por critério laboratorial, ou clínico-epidemiológico. O caso confirmado por critério laboratorial é aquele que obteve resultado laboratorial positivo, por isolamento viral, ou, detecção de RNA viral por RT-PCR (em amostra coletada até o 8º dia de início dos sintomas) ou detecção de anticorpos IgM em uma única amostra de soro durante a fase aguda (a partir do 6º dia de início dos sintomas), ou convalescente (15 dias após o início dos sintomas), demonstração de soro conversão entre as amostras na fase aguda (1ª amostra) e convalescente (2ª amostra) ou detecção de anticorpos IgG em amostras coletadas de pacientes na fase crônica da doença, com clínica sugestiva. O caso confirmado por critério clínico epidemiológico é aquele que atende a definição de caso suspeito, e que tenha vínculo familiar, ou espaço-temporal (vínculo epidemiológico) com caso confirmado laboratorialmente.

Assim, o script lê a base de dados bruta de notificações (chikon.dbf) e categoriza os registros em três frentes:

1. Investigação Clínico-Epidemiológico: Verifica se o paciente tinha febre e dores nas articulações, mas o exame não foi realizado.

2. Investigação Laboratorial: Avalia se o exame de PCR foi coletado no tempo correto (até 8 dias do início dos sintomas) e se o resultado justifica o descarte.

3. Casos em Investigação: Analisa os registros que ainda estão abertos para identificar quais já reúnem critérios para avaliação imediata.

Ao final, o programa gera um relatório consolidado em Excel indicando se o descarte de cada caso foi Adequado ou Não Adequado (inconsistente).

O script também faz uma análise dos casos comparando o número de casos confirmados e descartados por bairro (NM_BAIRRO), visando identificar zonas quentes de transmissão dentro do território e possíveis falhas no fluxo assistencial, de acordo com o padrão de descarte dos casos suspeitos.

#### Pipeline Análise

1. [Dados Brutos: chikon.xlsx] > 
2. [Limpeza do Banco de Dados] (Remove dados irrelevantes) > 
3. Classificação por Grupos (Regras): 
- Clínico-Epidemiológico (Filtra Critério 2 + Descartados)
- Laboratorial (Filtra Critério 1 + Descartados)
- Em Investigação (Filtra os casos sem encerramento) >
4. [Geração do Relatório Final: chik_descartados_total.xlsx]

Importante!
O arquivo Chikon, baixado no formato [dbf], deve ser convertido em Pasta de Trabalho Excel [.xlsx] com o nome chikon_[siglaestado].xlsx

#### Preparação do ambiente
- Instalar o Python 3;
- Instalar bibliotecas [pip install pandas numpy openpyxl notebook];
- Microsoft Excel;
- Editor de código (VS Code, Anaconda, Google Colab).

#### Execução do Projeto
Organize seus arquivos: Certifique-se de que o arquivo bruto (extensão .xlsx) esteja na mesma pasta que o arquivo do código (chik_descartados.ipynb).

#### Arquivos Gerados
Após o término da execução, o programa salvará automaticamente os seguintes arquivos na sua pasta:

chikon_notificados.xlsx: Uma versão limpa da base original, mantendo apenas as colunas essenciais para a epidemiologia.

chik_descartados_total.xlsx: O relatório final contém 4 abas (Planilhas):

- clinico_epidemiologico: Casos encerrados por este critério, com a coluna DESCARTE_CRITERIO indicando se foi adequado ou não.

- laboratorial: Casos encerrados por laboratório, detalhando a diferença de dias entre os sintomas e a coleta (DIF_DIAS) e se a coleta foi oportuna (DIAG_OPORTUNO).

- investigacao: Casos que ainda não foram fechados no sistema, sinalizando quais já precisam de avaliação.

- notificados: A base consolidada de suporte.

#### Regra aplicada a adequabilidade do descarte
1 - Caso Suspeito Padrão: Paciente que apresenta Febre (FEBRE == 1) E manifestação articular, que pode ser Artralgia (ARTRALGIA == 1) OU Artrite (ARTRITE == 1).

2 - Descarte Não Adequado (Inconsistente): Se o paciente se encaixa na definição de caso suspeito acima, mas o exame de PCR consta como Não Realizado ou está em branco, o sistema classifica como n_adequado, pois faltam subsídios para descartar o caso.

Período Oportuno (Laboratorial): O cálculo da diferença de dias entre a Data de Início dos Sintomas (DT_SIN_PRI) e a Data da Coleta do PCR (DT_PCR) deve ser de 0 a 8 dias. Se um caso foi descartado com exame negativo colhido fora desse prazo (ex: no 12º dia), o sistema acusará como n_adequado, exigindo revisão da vigilância.

Algoritmo desenvolvido em colaboração por: Pedro Araújo & Bárbara Araújo. Pode ser utilizado livremente, desde que citada a fonte.
