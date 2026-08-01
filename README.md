## 💡 Por que escolhemos o Banco de Dados Relacional?

A escolha de um **Banco de Dados Relacional (SGBD MySQL)** para este projeto justifica-se pelos seguintes motivos técnicos e acadêmicos:

1. **Estrutura de Dados Rígida e Padronizada:** Os dados de mobilidade urbana (distritos, linhas, pontos de parada e tempos de deslocamento) possuem atributos fixos e tabulares bem definidos, perfeitos para a estrutura em tabelas, colunas e tipos de dados nativos do modelo relacional.
2. **Integridade Referencial (Chaves Estrangeiras):** O projeto exige forte consistência entre as entidades (ex: um ponto de parada precisa estar associado a um distrito existente; uma estimativa de deslocamento vincula distrito de origem, distrito de destino e linha utilizada). As *Foreign Keys* garantem que não existam dados órfãos ou inconsistentes.
3. **Capacidade de Consultas Complexas via SQL:** A pergunta principal do projeto exige cruzar dados de diferentes origens (agrupar tempos médios de espera por distrito, contar linhas disponíveis por zona, etc.). O SQL permite realizar **JOINs**, agrupamentos (`GROUP BY`) e agregações (`AVG`, `COUNT`, `MAX`) de forma rápida, eficiente e expressiva.
4. **Padronização e Ampla Adoção:** O modelo relacional é o padrão de mercado para dados estruturados de infraestrutura e administração pública, além de estar totalmente alinhado aos objetivos da unidade curricular (UC03).

---

## 🗄️ Estrutura do Banco de Dados

O banco de dados foi modelado para armazenar dados regionais, linhas de ônibus, pontos de parada e estimativas de tempo de deslocamento:

| Tabela / Entidade | Atributos | Finalidade / Importância |
| :--- | :--- | :--- |
| **`Distrito`** | `id_distrito`, `nome_distrito`, `zona`, `populacao_estimada` | Mapear onde a população está localizada e suas necessidades de transporte. |
| **`Linha`** | `id_linha`, `codigo_linha`, `nome_linha`, `tipo_dia` | Identificar rotas e dias de operação dos ônibus. |
| **`Ponto_parada`** | `id_ponto`, `endereco_ponto`, `id_distrito` | Registrar a localização física dos pontos de embarque. |
| **`Deslocamento_estimado`** | `id_deslocamento`, `id_distrito_origem`, `id_distrito_destino`, `id_linha_utilizada`, `tempo_medio_espera_minutos` | Medir o tempo médio de espera e eficiência do transporte entre origem e destino. |

---

## 📂 Como Executar o Banco de Dados (Local)

1. **Clone este repositório:**
   ```bash
   git clone https://github.com/seu-usuario/nome-do-repositorio.git
   ```
2. **Abra seu gerenciador MySQL (ex: MySQL Workbench):**
3. **Execute o script de criação do banco e das tabelas:**
   ```sql
   CREATE DATABASE db_sptrans_analise;
   USE db_sptrans_analise;
   -- Execute o arquivo .sql de criação de tabelas e inserção de dados
   ```
4. **Rode as queries de consulta** para visualizar os dados consolidados sobre os tempos de espera por distrito.

---

## 📅 Divisão de Tarefas do Grupo

- **Ajuste do Escopo e Problema:** Todos os integrantes **
- **Coleta de Dados:** Bruna *
- **Filtragem e Tratamento das Informações:** Nicole
- **Montagem e Estruturação do Banco de Dados:** Miguel *
