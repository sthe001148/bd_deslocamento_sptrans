<!-- ÍCONES E BADGES DE TECNOLOGIAS -->
<p align="center">
  <img src="https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white" alt="MySQL Badges" />
  <img src="https://img.shields.io/badge/SQL-CC292B?style=for-the-badge&logo=sqlite&logoColor=white" alt="SQL Badges" />
  <img src="https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub Badges" />
</p>

<!-- TÍTULO DO PROJETO -->
<br />
<p align="center">
  <h3 align="center">PROJETO INTEGRADOR I</h3>
  <p align="center">
    <strong>bd_deslocamento_sptrans</strong>
    <br />
    Análise de Eficiência e Mobilidade Urbana nas Zonas Norte e Leste de São Paulo (SPTrans)
    <br />
    ·
    <br />
    <strong>Integrantes do Grupo:</strong><br />
    Nicole · Miguel · Bruna · Stefany
  </p>
</p>

<!-- ÍNDICE / TABLE OF CONTENTS -->
<details>
  <summary>Sumário</summary>
  <ol>
    <li>
      <a href="#sobre-o-projeto">Sobre o Projeto</a>
      <ul>
        <li><a href="#a-necessidade">A Necessidade</a></li>
        <li><a href="#a-solução-encontrada">A Solução Encontrada</a></li>
      </ul>
    </li>
    <li><a href="#tecnologias-utilizadas">Tecnologias Utilizadas</a></li>
    <li><a href="#modelo-do-banco-de-dados">Modelo do Banco de Dados</a></li>
    <li>
      <a href="#estrutura-de-arquivos-do-repositório">Estrutura de Arquivos</a>
      <ul>
        <li><a href="#arquivos-csv-das-entidades">Arquivos CSV (Carga de Dados)</a></li>
        <li><a href="#arquivos-sql-de-consumo">Arquivos SQL (Estrutura e Consumo)</a></li>
      </ul>
    </li>
    <li><a href="#como-executar">Como Executar o Projeto</a></li>
  </ol>
</details>

---

## Sobre o Projeto

Este repositório contém o desenvolvimento do banco de dados relacional construído com base nas diretrizes do documento de planejamento `20260603-desenho-projeto-integrador.pdf`, atendendo aos requisitos da disciplina **UC03 (Modelagem de Dados e SQL)**. O sistema foi projetado para analisar a capilaridade da rede de ônibus da SPTrans na cidade de São Paulo.

### A Necessidade
Nas regiões periféricas e densamente povoadas de São Paulo, como a **Zona Norte e a Zona Leste**, os cidadãos enfrentam tempos de deslocamento diários excessivos. Embora a malha rodoviária cubra o município, a frequência e a oferta de linhas variam de forma desigual entre os distritos. 

**O problema central:** Faltavam evidências centralizadas e dados estruturados que provassem o impacto real desse tempo de espera na rotina da população. Sem ferramentas analíticas organizadas, os gestores públicos encontram barreiras para tomar decisões eficientes.

### A Solução Encontrada
A solução desenvolvida consiste em um **Banco de Dados Relacional** local que armazena, de forma estruturada, registros numéricos e textuais de mobilidade urbana. O banco organiza informações sobre distritos, rotas de ônibus, pontos físicos e tempos de trânsito.

Por meio de consultas SQL analíticas, o sistema gera relatórios consolidados que servem como evidências para a **Prefeitura de São Paulo** e para os **gestores da SPTrans** identificarem os territórios críticos com menor oferta de linhas e os maiores tempos de espera.

---

## Tecnologias Utilizadas

* **SGBD:** MySQL (Ambiente Local)
* **Linguagem:** SQL (DDL para criação de tabelas e DML para extração de dados)

---

## Modelo do Banco de Dados

O modelo de dados foi estruturado de forma relacional focado nas seguintes entidades:

### Entidades e Atributos:

* **`Distrito`**: `id_distrito`, `nome_distrito`, `zona`, `populacao_estimada`.
  * *Importância:* Identifica a concentração demográfica e mapeia quem mais necessita do serviço de transporte.
* **`linha`**: `id_linha`, `codigo_linha`, `nome_linha`, `tipo_dia`.
  * *Importância:* Registra o planejamento das frotas, códigos de linhas e a operação de acordo com os dias (úteis ou fins de semana).
* **`Ponto_parada`**: `id_ponto`, `endereco_ponto`, `id_distrito`.
  * *Importância:* Mapeia geograficamente onde os locais de embarque e desembarque acontecem na prática.
* **`Deslocamento_estimado`**: `id_deslocamento`, `id_distrito_origem`, `id_distrito_destino`, `id_linha_utilizada`, `tempo_medio_espera_minutos`.
  * *Importância:* Tabela de métricas que permite medir o tempo médio de espera entre a origem e o destino para avaliar a eficiência do sistema.

---

## Estrutura de Arquivos do Repositório

O projeto está organizado com diretórios específicos para os dados e para os scripts do banco:

### 📊 Arquivos CSV (Das Entidades)
Disponíveis na pasta `/data`, contêm a carga de dados públicos tratados para o preenchimento das tabelas:
* `distritos.csv`: Listagem de distritos mapeados com dados demográficos.
* `linhas.csv`: Dados de identificação das rotas operantes da SPTrans.
* `pontos_parada.csv`: Localização urbana dos pontos de parada.
* `deslocamentos_estimados.csv`: Amostra de tempos de espera registrados e calculados por minutos.

### 💻 Arquivos SQL (De Consumo)
Disponíveis na pasta `/sql`, gerenciam a infraestrutura do SGBD e a geração de evidências:
* `1-schema.sql`: Instruções estruturais para a criação automatizada do banco de dados e suas respectivas chaves relacionais.
* `2-seed.sql`: Script responsável por executar a inserção automatizada dos dados limpos dos arquivos CSV para o MySQL.
* `3-queries_consumo.sql`: Consultas prontas criadas para extrair relatórios estratégicos de tempos de espera superiores ao aceitável e gargalos operacionais por distrito.

---

## Como Executar

1. Certifique-se de ter o **MySQL Server** ativo localmente na sua máquina.
2. Clone este repositório:
   ```bash
   git clone [https://github.com/SEU-USUARIO/bd_deslocamento_sptrans.git](https://github.com/SEU-USUARIO/bd_deslocamento_sptrans.git)
Acesse o terminal do seu SGBD (ou ferramenta visual de queries) e monte o esquema estrutural do banco:

SQL
SOURCE sql/1-schema.sql;
Execute o arquivo de população para alimentar as tabelas com a massa de dados coletada:

SQL
SOURCE sql/2-seed.sql;
Utilize o script de consultas analíticas para visualizar as métricas finais geradas sobre a SPTrans:

SQL
SOURCE sql/3-queries_consumo.sql;
