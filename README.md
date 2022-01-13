# Entregaveis-projeto-bi

Um breve de todos os entregáveis solicitados

## 1° Entregável
▼ Questionamentos que seriam feitos em uma reunião de entendimento

	 - Lucro negativo (Seria uma prática que a empresa utiliza)

	 - O desconto está em valor unitário, foi aplicado e esta associado ao lucro?

	 - O Sales, é o valor total das vendas ou valor unitário?

	 - People, são pessoas que compram ou que vendem?
▼ DQ

	→ Lucro Negativo

	→ Produto com mais de um código

	→ Cidade com mais de um código postal

	→ Cliente com mais de um segmento

	→ Clinete com mais de um código

	→ Pedido com mais de um cliente

	→ Sub-categoria com mais de uma categoria
  
▼ Sugestões

	→ Trocar o campo lucro por resultado e criar um outro campo lucro, transformando o negativo em 0
  
→ Existe um PBIX Para ilustrar o proposto (1-Entregavel)

----------------------------------------------------------------------------------------------------------------------------------------------

## 2° Entregável

Foi criado um notebook para consumir o json proposto e utilizado pandas

importado o pandas

```bash
import pandas as pd
```
Carregando o json

```bash
from pandas.io.json import json_normalize
```
Foi utilizado normalize , pegando os campos para utilizar no registro da tabela.
Utilizei também o parâmetro record_path, ele faz uma "Varredura" no caminho do objeto e coloca na lista, fazendo uma matriz de registros.
Deste jeito foi expandido todas as informações em um único df.

```bash
df2 = pd.json_normalize(data, meta=['CreateDate','EmissionDate','Discount','NFeNumber','NFeID'], errors='ignore',record_path='ItemList')
```
Para dividir em dois df e seguir um modelo transacional (NF / Itens NF)

Primeiro foi pego todos os campos da NF
```bash
nfs = df2[['NFeID','CreateDate','EmissionDate','Discount','NFeNumber']]
```
Depois os campos dos itens da NF
```bash
itens = df2[['NFeID','ProductName','Quantity','Value']]
```
Desta forma iremos utilizar o relacionamento pelo campo NFeID

----------------------------------------------------------------------------------------------------------------------------------------------

## 3° Entregável

Para solução deste entregável, coloquei dois exemplos de solução do problema proposto
O primeiro foi feito na parte do "Front" com Dax e o segundo no power query.

→ Existe um PBIX Para ilustrar o proposto (3-Entregavel)
 
 ----------------------------------------------------------------------------------------------------------------------------------------------

## 4° Entregável

Criado dois modelos de arquitetura na AWS
1ª Arquitetura:
Proposto um EC2 consumindo uma API Gatway para buscar os dados e colocar os arquivos em um S3, depois temos um Ec2 (Python) consumindo o json, criando um df e subindo para o Redshift para criar o DW, organizando e consolidando com outras fontes de dados necessárias para o negócio e por fim está pronto para "plugar" qualquer ferramenta da Data View criando as visões.

2ªArquitetura:
Proposto um EC2 consumindo uma API Gatway para buscar os dados e colocar os arquivos em um Kafka(MSK), depois temos um Ec2 (Python) consumindo o json, criando um df e subindo para o Redshift para criar o DW, organizando e consolidando com outras fontes de dados necessárias para o negócio e por fim está pronto para "plugar" qualquer ferramenta da Data View criando as visões.

A diferença proposta entre as arquiteturas foi pensado na questão de performance, onde o Kafka pode se sair melhor com grandes volumes de dados.
