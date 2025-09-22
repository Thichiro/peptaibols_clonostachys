# peptaibols_clonostachys
An script made in R language to group and give ID to the previously mass peaks found in MZmine software
# Análise e Agrupamento de Picos de Espectrometria de Massas de Peptaibois

Este repositório contém um script em R desenvolvido para processar, filtrar e agrupar íons de peptaibois putativos, identificados em espécies do fungo *Clonostachys*, a partir de dados brutos de espectrometria de massas (EM).

## Contexto

A análise de metabólitos secundários, como os peptaibois, por EM gera uma grande quantidade de dados, onde a mesma molécula pode ser detectada múltiplas vezes com pequenas variações de massa/carga (m/z). O objetivo deste script é automatizar o processo de agrupar esses picos semelhantes, facilitando a identificação de compostos únicos e a análise quantitativa subsequente.

## Arquivos no Repositório

* `lista_compilada.xlsx`: O script principal em R que executa toda a análise.
* `pep_shared_final`: Um arquivo de exemplo com o resultado final gerado pelo script.
* `README.md`: Este arquivo de documentação.

## Passo a Passo da Análise

O script segue uma sequência lógica de passos para transformar os dados brutos em uma tabela final anotada e organizada.

### 1. Carga e Preparação dos Dados
O script inicia carregando os dados de uma lista de picos, que contém informações como m/z, tempo de retenção (rt), área do pico e massa monoisotópica.

### 2. Agrupamento de Picos Semelhantes
Este é o núcleo da análise. Um loop percorre cada pico e o compara com todos os outros.
-   Picos cujo valor de `mz` tem uma diferença menor ou igual a uma tolerância definida (neste caso, `0.02`) são considerados correspondentes.
-   A cada grupo de picos correspondentes é atribuído um `match_group` único para facilitar a identificação.

### 3. Criação de Novas Colunas (Engenharia de Atributos)
Para enriquecer a análise, duas novas colunas foram criadas com a função `mutate()`:
-   `pep_id`: Um identificador único para cada molécula, criado com o prefixo "pep_" seguido pelo valor **inteiro** da massa monoisotópica. Ex: `pep_1441`.
-   `log10_area`: A área do pico transformada para a escala logarítmica de base 10 (`log10(area)`). Isso ajuda a normalizar os dados de abundância para visualizações e análises estatísticas.

### 4. Reordenação e Exportação
-   As colunas da tabela final são reordenadas com a função `select()` para colocar as informações mais importantes (como `pep_id`, `match_group` e `specie`) no início, facilitando a inspeção dos dados.
-   O dataframe final, limpo e organizado, é exportado para um arquivo CSV chamado `pep_shared_final.csv`.
