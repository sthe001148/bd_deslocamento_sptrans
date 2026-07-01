<p align="center">
  <img src="https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white" alt="MySQL" />
  <img src="https://img.shields.io/badge/SQL-CC292B?style=for-the-badge&logo=sqlite&logoColor=white" alt="SQL" />
  <img src="https://img.shields.io/badge/Database-Relational-blue?style=for-the-badge" alt="Database Type" />
</p>

<br />
<p align="center">
  <h1 align="center">Otimização de Mobilidade Urbana: bd_deslocamento_sptrans</h1>
  <p align="center">
    Análise Estatística de Capilaridade e Tempo de Espera do Sistema de Transporte Coletivo (SPTrans) nas Zonas Norte e Leste de São Paulo.
    <br />
    <a href="#-como-executar"><strong>Explorar Documentação »</strong></a>
  </p>
</p>

<details>
  <summary>📌 Sumário Executivo</summary>
  <ol>
    <li><a href="#-sobre-o-projeto">Sobre o Projeto</a></li>
    <li><a href="#-cenário-e-necessidade-de-negócio">Cenário e Necessidade de Negócio</a></li>
    <li><a href="#-solução-desenvolvida">Solução Desenvolvida</a></li>
    <li><a href="#-modelagem-e-arquitetura-de-dados">Modelagem e Arquitetura de Dados</a></li>
    <li><a href="#-estrutura-do-repositório">Estrutura do Repositório</a></li>
    <li><a href="#-como-executar">Como Executar</a></li>
    <li><a href="#-equipe-de-desenvolvimento">Equipe de Desenvolvimento</a></li>
  </ol>
</details>

---

## 📖 Sobre o Projeto

O **bd_deslocamento_sptrans** é um banco de dados relacional desenvolvido como entrega consolidada da qualificação de **Assistente de gestão de dados**. O projeto visa centralizar, estruturar e auditar dados operacionais de transporte público da SPTrans, mapeando gargalos logísticos estruturais de deslocamento na capital paulista.

---

## 🎯 Cenário e Necessidade de Negócio

Nas macro regiões da **Zona Norte e Zona Leste de São Paulo**, caracterizadas por alta densidade demográfica, o tempo despendido pela população em deslocamentos diários é crítico. Embora a malha logística da SPTrans cubra geograficamente o município, a frequência de frotas e a capilaridade das linhas apresentam alta variabilidade entre distritos.

* **O Gargalo:** Havia uma escassez de dados estruturados e centralizados que servissem como evidências estatísticas do impacto do tempo de espera na rotina dos cidadãos.
* **Impacto:** Sem uma base de dados normalizada, a tomada de decisão por parte da **Prefeitura de São Paulo** e dos **gestores da SPTrans** para otimização de frotas tornava-se puramente reativa e carente de precisão técnica.

---

## 💡 Solução Desenvolvida

Foi implementado um ecossistema de banco de dados baseado em **Engenharia de Dados Relacional**. O sistema consome dados brutos de logística urbana, processa indicadores de tempo e popula tabelas normalizadas para cruzamento analítico. 

A inteligência do negócio reside na capacidade de responder queries complexas, tais como:
> *"Quais territórios específicos registram os maiores tempos de espera e possuem, proporcionalmente, a menor oferta de linhas ativas?"*

---

## 🗄️ Modelagem e Arquitetura de Dados

O banco foi projetado utilizando o modelo relacional (SQL) devido à forte consistência e natureza estruturada das entidades envolvidas.

### 📐 Diagrama Relacional Textual (MER)
```text
  [Distrito] 1 --------- N [Ponto_parada]
  [Distrito] 1 --------- N [Deslocamento_estimado] (Origem)
  [Distrito] 1 --------- N [Deslocamento_estimado] (Destino)
  [linha]    1 --------- N [Deslocamento_estimado]
```

### 📖 Dicionário de Dados

#### 1. Tabela: `Distrito`
| Atributo | Tipo de Dado | Restrição | Descrição |
| :--- | :--- | :--- | :--- |
| `id_distrito` | INT | PK, AI | Identificador único do distrito. |
| `nome_distrito` | VARCHAR(100) | NOT NULL | Nome oficial do distrito de SP. |
| `zona` | VARCHAR(20) | NOT NULL | Região geográfica (Ex: Zona Leste). |
| `populacao_estimada` | INT | NOT NULL | População total estimada da região. |

#### 2. Tabela: `linha`
| Atributo | Tipo de Dado | Restrição | Descrição |
| :--- | :--- | :--- | :--- |
| `id_linha` | INT | PK, AI | Identificador interno da linha. |
| `codigo_linha` | VARCHAR(20) | NOT NULL, UNIQUE | Código operacional da SPTrans (Ex: 2590-10). |
| `nome_linha` | VARCHAR(150) | NOT NULL | Descrição do itinerário da linha. |
| `tipo_dia` | VARCHAR(30) | NOT NULL | Escala de operação (Dia útil, Sábado, Domingo). |


#### 3. Tabela: `Ponto_parada`
| Atributo | Tipo de Dado | Restrição | Descrição |
| :--- | :--- | :--- | :--- |
| `id_ponto` | INT | PK, AI | Código de identificação física do ponto. |
| `endereco_ponto` | VARCHAR(255) | NOT NULL | Endereço ou localização aproximada. |
| `id_distrito` | INT | FK | Vínculo com a tabela de Distritos. |


#### 4. Tabela: `Deslocamento_estimado`
| Atributo | Tipo de Dado | Restrição | Descrição |
| :--- | :--- | :--- | :--- |
| `id_deslocamento` | INT | PK, AI | Chave primária do registro de métrica. |
| `id_distrito_origem` | INT | FK | Distrito inicial da rota de viagem. |
| `id_distrito_destino` | INT | FK | Distrito final de destino da viagem. |
| `id_linha_utilizada` | INT | FK | Linha de ônibus monitorada. |
| `tempo_medio_espera_minutos`| DECIMAL(5,2) | NOT NULL | Tempo médio de espera calculado no ponto. |


---

## 📂 Estrutura do Repositório

O ambiente de produção foi modularizado para garantir a separação de conceitos (*Separation of Concerns*):

```text
├── data/                            # Camada de Persistência Física (Massa de Dados)
│   ├── distritos.csv                # Carga de dados demográficos compilados
│   ├── linhas.csv                   # Cadastro de frotas e itinerários
│   ├── pontos_parada.csv            # Coordenadas urbanas de embarque
│   └── deslocamentos_estimados.csv  # Matriz de tempos gerados por trajetos
│
├── sql/                             # Camada de Engenharia e Scripts SQL
│   ├── 1-schema.sql                 # Data Definition Language (DDL) - Criação do Banco
│   ├── 2-seed.sql                   # Scripts de carga automatizada via CSV
│   └── 3-queries_consumo.sql        # Data Manipulation Language (DML) - Relatórios de Negócio
└── README.md

🚀 Como Executar
Pré-requisitos
Possuir o SGBD MySQL Server 8.0+ instalado localmente.

Passos para Instalação
Clone o repositório institucional:

Bash
git clone [https://github.com/sthe001148/bd_deslocamento_sptrans.git]
Inicialize o servidor MySQL e execute o script de criação da arquitetura relacional

SQL
SOURCE sql/1-schema.sql;
Execute o script de semeadura (seed) para realizar o preenchimento das tabelas instanciadas através dos arquivos .csv

SQL
SOURCE sql/2-seed.sql;
Execute as queries analíticas de consumo para auditar os resultados obtidos de mobilidade

SQL
SOURCE sql/3-queries_consumo.sql;


👥 Equipe de Desenvolvimento

A execução deste ecossistema foi dividida em etapas com os seguintes responsáveis técnicos
Engenharia de Requisitos & Escopo: Desenvolvimento conjunto por todos os integrantes.
(Coleta de Dados Públicos): brunaluiza18
(Tratamento e Filtragem de Dados): nicolee-kats
(Modelagem e DBA MySQL): miguel676726
(Documentação e Regras de Negócio): Sthe001148
