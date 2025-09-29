# peptaibols_clonostachys
# Análise e Agrupamento de Picos de Espectrometria de Massas de Peptaibois

Este repositório contém um script em R desenvolvido para processar, filtrar e agrupar íons de peptaibois putativos, identificados em espécies do fungo *Clonostachys*, a partir de dados pré-tratados de espectrometria de massas (EM) no software MZmine.

## Contexto

A análise de metabólitos secundários, como os peptaibois, por EM gera uma grande quantidade de dados, onde a mesma molécula pode ser detectada múltiplas vezes com pequenas variações de massa/carga (m/z). O objetivo deste script é automatizar o processo de agrupar esses picos semelhantes, facilitando a identificação de compostos únicos e a análise quantitativa subsequente.

## Arquivos no Repositório

* `peptaibols_unicos.xlsx`: O script principal em R que executa toda a análise.
* `pep_matriz_filled.xlsx`: Um arquivo de exemplo com o resultado final gerado pelo script.
* `heatmap_peptaibols.tiff`: Um arquivo de heatmap exemplo de como ficam os agrupamentos dentro de casa espécie de _Clonostachys_
* `peptaibols_featuregrouping.qmd`: Um arquivo qmd. de exemplo pronto para ser rodado no softwre R
* `README.md`: Este arquivo de documentação.

-   Etapa 1: Preparação e Filtragem dos Dados Iniciais
O processo começa com uma lista de picos curada, contendo os sinais mais intensos (ex: top 20%) de cada espécie.

Picos redundantes dentro de uma mesma espécie (com massa e tempo de retenção muito similares) são filtrados, mantendo-se apenas o sinal mais intenso como representante.

Etapa 2: Agrupamento Isotópico
O script identifica e agrupa picos que são isótopos uns dos outros (M, M+1, M+2), baseando-se nas diferenças de massa teóricas e na proximidade do tempo de retenção.

Etapa 3: Identificação de Séries Homólogas
Picos que pertencem a uma mesma família de moléculas, mas que diferem por unidades de metileno (-CH₂), são agrupados. Isso permite identificar peptaibols estruturalmente relacionados que variam, por exemplo, no comprimento de uma cadeia de aminoácidos.

Etapa 4: Agrupamento de Picos Compartilhados (Matching)
O algoritmo compara os picos entre as diferentes espécies para encontrar moléculas que são compartilhadas pelo grupo de organismos, utilizando tanto a massa representativa quanto o tempo de retenção para garantir a precisão do match.

Etapa 5: Criação de IDs Únicos (pep_id)
Um identificador final e legível é gerado para cada peptaibol. O formato do ID diferencia os picos que são compartilhados entre espécies daqueles que são únicos para uma espécie específica.

Etapa 6: Criação da Matriz de Abundância
A lista de dados em formato longo é transformada em uma matriz de abundância em formato largo, onde as linhas são os pep_id, as colunas são as specie e as células contêm os valores de log10(area).

Etapa 7: Preenchimento de Lacunas (Gap-Filling)
Esta é uma etapa avançada e crucial. O script utiliza uma segunda lista de dados, completa e não filtrada, proveniente do MZmine para buscar os picos que foram marcados com área zero na matriz inicial.

A busca é específica para cada lacuna (pep_id, specie) e utiliza critérios rigorosos de massa, tempo de retenção e um limiar mínimo de área para preencher os valores ausentes com dados de baixa intensidade, mas de alta confiança.

Etapa 8: Anotação de compostos putativos por meio de busca em banco de dados contendo uma lista das posssíveis moléculas presentes nos extratos de Clonostachys

Etapa 9: Geração de Resultados
Tabelas (.xlsx): O script exporta a matriz final preenchida e uma lista separada contendo apenas os peptaibols que foram identificados como exclusivos de uma única espécie.

Heatmaps (.tiff):

Um heatmap com normalização Z-score e clusterização por correlação, ideal para visualizar os padrões relativos de produção dos peptaibols compartilhados.


