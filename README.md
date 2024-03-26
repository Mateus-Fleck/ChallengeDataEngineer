# ChallengeDataEngineer
Desafio de código para Desenvolvedor de Software com foco em projetos de dados.

## **ATG - Analyzing To Grow**

Desafio de código para Desenvolvedor de Software com foco em projetos de dados.

### **Contexto**

Na ATG Datta Solutions temos muitos projetos onde desenvolvemos todo o pipeline de dados do nosso cliente, desde a extração de dados de diversas fontes de dados até o carregamento desses dados em seu destino final, sendo que esse destino final varia desde um data warehouse para uma ferramenta de Business Intelligence até uma API para integração com sistemas de terceiros.

Como desenvolvedor de software com foco em projetos de dados, sua missão é planejar, desenvolver, implantar e manter um pipeline de dados.

### **Desafio**

Forneceremos 2 fontes de dados, um banco de dados PostgreSQL e um arquivo CSV.

O arquivo CSV representa detalhes de pedidos de um sistema de comércio eletrônico.

O banco de dados fornecido é um banco de dados de amostra fornecido pela Microsoft para fins educacionais, chamado Northwind. A única diferença é que a tabela **order_detail** não existe neste banco de dados que você está recebendo. Esta tabela order_details é representada pelo arquivo CSV que fornecemos.

Esquema do banco de dados Northwind original:

![Diagrama Northwind](https://raw.githubusercontent.com/Mateus-Fleck/ChallengeDataEngineer/7d4892c84bea4a35099c37a19e15b8f9be31b7e9/Challenge/doc/db_northwind.png)


Seu desafio é construir um pipeline que extraia os dados todos os dias de ambas as fontes e grave os dados primeiro no disco local e depois em um banco de dados PostgreSQL. Para este desafio, o arquivo CSV e o banco de dados serão estáticos, mas em qualquer projeto do mundo real, ambas as fontes de dados mudariam constantemente.

É importante que todas as etapas de gravação (gravação de dados de entradas no sistema de arquivos local e gravação de dados do sistema de arquivos local no banco de dados PostgreSQL) sejam isoladas umas das outras, você deve ser capaz de executar qualquer etapa sem executar as outras.

Para a primeira etapa, onde você grava dados no disco local, você deve gravar um arquivo para cada tabela. Este pipeline será executado todos os dias, portanto deve haver uma separação nos caminhos dos arquivos que você criará para cada combinação de origem (CSV ou Postgres), tabela e dia de execução, por exemplo:

```
/data/postgres/{table}/2024-01-01/file.format
/data/postgres/{table}/2024-01-02/file.format
/data/csv/2024-01-02/file.format

```

Você é livre para escolher o nome e o formato do arquivo que vai salvar.

Na etapa 2, você deve carregar os dados do sistema de arquivos local que você criou para o banco de dados final.

O objetivo final é poder executar uma consulta que mostre os pedidos e seus detalhes. Os pedidos são colocados em uma tabela chamada **pedidos** no banco de dados postgres Northwind. Os detalhes são colocados no arquivo csv fornecido, e cada linha tem um campo **order_id** apontando para a tabela **orders** .

**Diagrama de Solução**

Como a ATG Datta Solutions utiliza algumas ferramentas padrão, o desafio foi pensado para ser realizado utilizando algumas dessas ferramentas.

As seguintes ferramentas devem ser usadas para resolver este desafio.

Agendador:

- [Airflow](https://airflow.apache.org/docs/apache-airflow/stable/installation/index.html)

Carregador de dados:

- [Embulk](https://www.embulk.org/) (baseado em Java) **OU**
- [Meltano](https://docs.meltano.com/?_gl=1*1nu14zf*_gcl_au*MTg2OTE2NDQ4Mi4xNzA2MDM5OTAz) (baseado em Python)

Base de dados:

- [PostgreSQL](https://www.postgresql.org/docs/15/index.html)

A solução deve ser baseada nos diagramas abaixo:

![Diagrama Embulk Meltano](https://raw.githubusercontent.com/Mateus-Fleck/ChallengeDataEngineer/db623ecbcac7fbbf969bb0994b156698f1b296a1/Challenge/doc/diagrama_embulk_meltano.jpg)

**Requisitos**

- Você **deve** usar as ferramentas descritas acima para completar o desafio.
- Todas as tarefas devem ser idempotentes, você deve poder executar o pipeline todos os dias e, neste caso onde os dados são estáticos, a saída deve ser a mesma.
- A etapa 2 depende de ambas as tarefas da etapa 1, portanto você não poderá executar a etapa 2 por um dia se as tarefas da etapa 1 não forem bem-sucedidas.
- Você deve extrair todas as tabelas do banco de dados de origem, não importa que você não utilizará a maioria delas para a etapa final.
- Você deve ser capaz de dizer claramente onde o pipeline falhou, para saber em qual etapa deve executar novamente o pipeline.
- Você deve fornecer instruções claras sobre como executar todo o pipeline. Quanto mais fácil, melhor.
- Você deve fornecer evidências de que o processo foi concluído com sucesso, ou seja, você deve fornecer um csv ou json com o resultado da consulta descrita acima.
- Você deve assumir que funcionará em dias diferentes, todos os dias.
- Seu pipeline deve estar preparado para ser executado nos dias anteriores, o que significa que você poderá passar um argumento para o pipeline com um dia anterior e ele deverá reprocessar os dados desse dia. Como os dados deste desafio são estáticos, a única diferença para cada dia de execução serão os caminhos de saída.

**Coisas que importam**

- Código limpo e organizado.
- Boas decisões em qual etapa (qual banco de dados, qual formato de arquivo...) e bons argumentos para apoiar essas decisões.
- O objetivo do desafio não é apenas avaliar o conhecimento técnico na área, mas também a capacidade de buscar informações e utilizá-las para resolver problemas com ferramentas que não necessariamente são do conhecimento do candidato.
- Ferramentas de apontar e clicar não são permitidas.

Obrigado por participar do Desafio!
