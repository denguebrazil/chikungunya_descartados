# Algoritmo de avaliação de casos descartados de dengue

Este projeto realiza o processamento e análise epidemiológica de dados de casos notificados de Dengue no Brasil, com foco específico na avaliação dos casos descartados e na qualidade da classificação final.

## Fonte de Dados

As bases de dados utilizadas neste projeto são provenientes do **SINAN (Sistema de Informação de Agravos de Notificação)**, abrangendo o período de 2020 a 2024. O sistema é a principal fonte de dados para vigilância epidemiológica no Brasil.

## Análises Realizadas

O código permite realizar as seguintes análises estruturadas:

### 1. Processamento e Consolidação de Dados
- **Concatenação de Bases Anuais**: Integração automática de múltiplos arquivos Excel (`den_2020.xlsx` a `den_2024.xlsx`) em um único DataFrame consolidado.
- **Limpeza de Dados**: Remoção automatizada de mais de 100 colunas administrativas e de variáveis de confundimento para otimizar o processamento e focar em variáveis epidemiológicas chave.

### 2. Análise Geral de Notificações
- **Contagem de Agravos**: Quantificação total de notificações por tipo de agravo.
- **Classificação Final**: Distribuição dos casos por classificação final (Confirmados vs. Descartados).
- **Série Temporal**: Evolução anual das notificações com visualização em gráficos de barras empilhadas para comparar proporções de classificação ao longo dos anos.

### 3. Avaliação de Critérios de Descarte (Qualidade da Vigilância)
O projeto implementa uma lógica rigorosa para avaliar se os casos foram descartados corretamente segundo os protocolos de vigilância:

- **Descarte Sem Critério**: Identificação de casos que foram descartados (`CLASSI_FIN == 5`), mas que apresentavam quadro clínico compatível (febre + pelo menos 2 outros sintomas) e não tiveram exames laboratoriais (Sorologia, NS1 ou PCR) realizados ou registrados.
- **Análise por Critério de Encerramento**: Comparação entre casos descartados por critério Laboratorial vs. Clínico-Epidemiológico.
- **Frequência de Sintomas**: Análise da prevalência de sintomas (Febre, Mialgia, Cefaleia, Exantema, Vômito, Náusea, Artralgia, Petéquias, Prova do Laço, Dor Retro-orbital) entre os casos suspeitos que foram posteriormente descartados.

### 4. Perfil Demográfico e Geográfico dos Casos Descartados
- **Análise por Sexo**: Distribuição de casos descartados entre homens e mulheres.
- **Zona de Residência**: Avaliação da ocorrência de descartes em áreas Urbanas vs. Rurais.
- **Ranking Municipal**: Identificação dos 10 municípios com maior número de casos descartados, segmentados por critério de encerramento (Laboratorial e Clínico-Epidemiológico).

### 5. Indicadores de Oportunidade
- **Tempo Médio de Encerramento**: Cálculo da diferença em dias entre a data de notificação (`DT_NOTIFIC`) e a data de encerramento (`DT_ENCERRA`), permitindo avaliar a agilidade do serviço de vigilância local.

## Tecnologias Utilizadas
- **Python 3.x**
- **Pandas**: Manipulação e tratamento de dados.
- **Matplotlib**: Geração de visualizações gráficas.
- **Openpyxl**: Manipulação de arquivos Excel com múltiplas abas.

## Como Executar
1. Certifique-se de que os arquivos `den_2020.xlsx` a `den_2024.xlsx` estejam no mesmo diretório do script.
2. Execute o notebook `den_descartados.ipynb`.
3. O script gerará arquivos de saída consolidados:
   - `den_2020_2024_total.xlsx`
   - `descartados_2020_2024_lab.xlsx`
   - `descartados_2020_2024_clepi.xlsx`
   - `den_descartados_criterios_avaliacao.xlsx`

---
*Este script foi desenvolvido para apoiar a análise técnica de vigilância epidemiológica, visando identificar potenciais falhas no descarte de casos suspeitos de arboviroses.*
