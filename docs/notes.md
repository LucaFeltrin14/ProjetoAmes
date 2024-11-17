Take notes here.

O conjunto de dados contém informações do Ames Assessor's Office usadas no cálculo de valores avaliados para propriedades residenciais individuais vendidas em Ames, IA, de 2006 a 2010.


# Variáveis Categóricas

Categorias com pouca representação:

Podemos remover categorias com poucas ocorrências e anotar que o modelo não serve para casas nessas categorias.
Ou podemos ignorar totalmente essas colunas.
Outra opção é agrupar essas pequenas categorias em uma nova chamada "Outros", indicando que não as ignoramos, mas não temos dados suficientes para saber seu impacto no preço de venda.
Variáveis com muitos valores ausentes:

Pode ser melhor excluir essas colunas,
ou atribuir os valores ausentes a uma nova categoria "Desconhecido".

MS.Zoning: Identifica a classificação geral de zoneamento da venda.
- Remove vendas de imóveis não residenciais.
    * Industrial
    * Comercial
    * Agricultura
```python
selection = ~(data['MS.Zoning'].isin(['A (agr)', 'C (all)', 'I (all)']))
data = data[selection]
```

Sale Type e Sale Condition: Identificam o tipo e a condição de venda.
- Todos os tipos e condições de venda são válidos, mas algumas categorias têm baixa representatividade.
- Os tipos de venda WD(Warranty Deed) 'Cenventional', 'Cash' e 'VA Loan' são juntados em um tipo apenas de venda GroupedWD
- Outros tipos 'COD', 'ConLI', 'Con', 'ConLD', 'Oth' e 'ConLw' são alocados em uma categoria "Others" principalmente pela baixa representatividade

Street: Condição da rua do imóvel
- De todos os 2901 imóveis ainda presentes no dataset, 6 estão em uma ruas com cascalho e o restante em ruas pavimentadas
- Coluna removida porque esse dado provavelmente não é tão relevante pela proporção dos resultados 

Condition.1 e Condition.2: Se referem a proximidade em relação a lugares específicos como uma linha de trem.
- Problema de baixa representatividade também
- Algumas categorias foram juntadas em categorias mais genéricas devido sua alta similaridade
- Juntou Condition.1 e Condition.2 em uma única coluna categórica chamada Combined.Condition, agrupando condições similares para melhorar a análise e interpretação dos dados.
- Essas colunas foram removidas

Misc.Feature e Alley: Referem-se a recursos não cobertos em outras categorias como por exemplo uma segunda garagem (Misc.Feature) e tipo de acesso a propriedade como cascalho ou pavimentado (Alley)
- O principal problema dessas features é que tem muitos valores faltando.
- Ao invés de remover a coluna Misc.Feature (porque esse dado pode ser relevante) o nome da coluna foi alterado para Shed com valores True/False indicando se o imóvel possui algum dos tipos de recursos
- O mesmo é feito para HasAlley

Exterior1.st e Exterior2.nd: variáveis categóricas que representam os materiais de revestimento utilizados na parte externa da casa. Elas identificam o tipo de material aplicado, sendo que Exterior 1 refere-se ao material principal, enquanto Exterior 2 é utilizado quando há mais de um tipo de material.
- Muitas categorias distintas e várias delas com baixa representatividade
- Identificação de alguns erros de digitação nos nomes das categorias do Exterior.2nd
- Criação da coluna "Others" para agrupar categorias com baixa representatividade
- Remoção de dados de Exterior.2nd quando o material é o mesmo apresentado em Exterior.1st

Heating: Representa o tipo de aquecimento do imóvel.
- Não há valores faltando mas aproximadamente 98% dos imóveis tem aquecimento a 'GasA' (Gas forced warm air furnace)
- Por isso, não é tão relevante essa informação e removemos a coluna

Roof.Matl e Roof.Style: Referem-se ao tipo de material e ao estilo de teto respectivamente.
- Ao analisar os dados de Roof.Matl, devida a baixa representatividade causada por praticamente todos os tetos serem feitos do material 'CompShg', essa coluna foi removida
- Nas categorias do estilo do teto existem 2 estilos predominantes 'Gable' e 'Hip' e outras categorias com valores baixos. No entanto, devido a esses 2 estilos apresentarem uma divisão considerável, essa informação pode ser relevante e por isso vamos agrupar o resto dos dados em um grupo  'Others' e manter a coluna

Mas.Vnr.Type: refere ao tipo de revestimento de alvenaria da casa. Ex BrkCmn, BrkFace, ...
- As duas categorias com menos valores são agrupadas em uma categoria "Other"
- dados faltantes que no dataSet ficam como Nan, classificamos como "None" para melhorar a analise dos dados

MS.SubCLass: Identifica o tipo de habitação envolvido na venda. Cada código representa uma categoria específica de moradia.
- Apenas agrupa os dados menos relevântes no grupo "Others"

Foundation: Tipo de material utilizado. Por exemplo concreto, madeira , ...
- Feito a mesma coisa do anterior, as categorias com menos valores foram agrupados em uma categoria "Others"

Neighborhood: Bairros
- É cogitado a possibilidade de agrupar os bairros com menos valores em uma categoria "Others" mas no final decide remover esses bairros
- Por isso, não serão considerados imóveis dos bairros 'Blueste', 'Greens', 'GrnHill' 'Landmrk'
- As vantagens dessa decisão são: 
1. Mantém a precisão ao focar apenas nos bairros com representação significativa.
2. O modelo se torna mais robusto, pois se baseia em bairros que têm mais dados disponíveis
- As desvantagens são: 
1. Reduz a quantidade de dados, o que pode afetar a generalização do modelo (Overfit).
2. O modelo não será aplicável aos bairros excluídos, limitando a aplicabilidade do modelo em situações do mundo real.

Garage.Type: Indica a localização do estacionamento da casa e possui diversas categorias que descrevem as diferentes configurações de garagens.
- Adiciona uma categorias 'NoGarage' para os valores Nan


--------Fim Variáveis Categóricas


# Variáveis Ordinais

Categorias com Pouca Representação:
Podemos remover categorias com poucas ocorrências e anotar que o modelo não serve para casas nessas categorias. Ou podemos ignorar totalmente essas colunas. Outra opção é agrupar essas pequenas categorias em uma nova chamada "Outros", indicando que não as ignoramos, mas não temos dados suficientes para saber seu impacto no preço de venda.

Variáveis com Muitos Valores Ausentes:
Pode ser melhor excluir essas colunas, ou atribuir os valores ausentes a uma nova categoria "Desconhecido".

Utilities: Tipos de utilidades ques estão disponíveis como água, gás, eletricidade, etc.
- Novamente pelo motivo de baixa representatividade removemos a coluna

Pool.QC: Se refere a qualidade da piscina. 
- Das categorias existentes, existem apenas13 respostas dentre todos os imóveis
- Removemos a coluna por falta de dados

Fence: Nível de privacidade fornecido pelas cercas
- Pelas especificações valores Nan significam que não há cerca. Mas para melhorar a análise futura que será feita com cálculos, podemos criamos uam categoria NoFence