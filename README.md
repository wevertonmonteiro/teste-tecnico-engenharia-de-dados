# Desafio Técnico - Engenharia de Dados

Este projeto contém a resolução dos desafios propostos utilizando SQL sobre os dados disponibilizados.
Neste arquivo, esclareço passo a passo como foram feitas as resoluçôes dos desafios.

As consultas foram desenvolvidas e executadas no notebook do projeto utilizando pysqldf( Usando o modelo de notebook disponibilizado por voces).

Para rodar o notebook:
1) acesse https://colab.research.google.com/,
2) clique na opçao Fazer upload de notebook
3) seleciona opçao Upload, e clique em Procurar, selecione o notebook na pasta do computador e clique em abrir.
4) no menu lateral esquerdo, seleciona Arquivos, e carregue os csv necessarios para executar os desafios.

## Desafio 1 - Métricas mensais de pedidos

Foi realizada uma análise mensal dos pedidos, considerando apenas os status completed e delivered.

Para cada mês foram calculados:

- faturamento total bruto;
- quantidade de pedidos;
- ticket médio.

Os dados foram agrupados por mês através da data de criação dos pedidos e ordenados do período mais recente para o mais antigo.

## Desafio 2 - Crescimento de GMV por seller

Para comparar o desempenho dos sellers entre dois trimestres, foram criadas duas CTEs:

- trimestre anterior: abril a junho de 2024;
- trimestre atual: julho a setembro de 2024.

Em cada período foi calculado o GMV e a quantidade de pedidos por seller.

Foram considerados apenas sellers com pelo menos 50 pedidos nos dois períodos.

Depois, os resultados foram relacionados com a tabela de sellers para obter nome e estado e foi calculado o percentual de crescimento:

(GMV atual - GMV anterior) / GMV anterior * 100

O resultado final apresenta os 10 sellers com maior crescimento.

Foi utilizado NULLIF no cálculo para evitar possíveis divisões por zero.

## Desafio 3 - Pedidos com descontos acima de 40%

Para essa análise, os itens foram agrupados por pedido.

O valor bruto de cada pedido foi calculado através de:

SUM(qty * unit_price)

O desconto total foi calculado através da soma dos descontos dos itens:

SUM(discount)

Foram excluídos os pedidos com status cancelled e mantidos apenas aqueles em que o desconto total representa mais de 40% do valor bruto.

Por fim, foi realizado o relacionamento com a tabela de sellers para identificar o seller responsável por cada pedido.

## Desafio 4 - Produtos com alto volume que nunca foram o item de maior valor

Primeiro foi calculado o total de unidades vendidas de cada produto através de SUM(qty).

Durante a validação foi identificado que todos os 800 produtos existentes no conjunto de dados possuem mais de 1.000 unidades vendidas.

Para identificar o item de maior valor unitário de cada pedido foi utilizada a função RANK(), particionando os dados por order_id e ordenando pelo unit_price de forma decrescente.

O uso de RANK() também permite considerar empates no maior valor unitário como primeira posição.

Em seguida, foi utilizado NOT EXISTS para buscar produtos que nunca apareceram na posição 1 de nenhum pedido.

A consulta final não retornou produtos.

Os 800 produtos distintos já foram rank_preco = 1 em pelo menos um pedido, fazendo com que nenhum produto atendesse simultaneamente às duas condições do desafio

## Observações

As consultas e etapas de validação estão disponíveis no notebook junto com os respectivos resultados.
