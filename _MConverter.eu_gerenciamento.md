# GERENCIAMENTO DE PROCESSOS E ESCALONAMENTO

Este projeto cumpre os requisitos do trabalho prático de Sistemas
Operacionais, implementando um simulador de escalonamento (Round-Robin e
Prioridade Preemptiva) e demonstrações de concorrência utilizando
Python.

## 1. Estrutura do Projeto {#estrutura-do-projeto}

- **Arquivos:** main.py
- **Conteúdo:** Código-fonte principal. Contém a lógica de escalonamento, simulação de threads e demonstrações de concorrência.
- **Parte do Trabalho:** A,B,C

- **Arquivos:** processos.csv
- **Conteúdo:** Arquivo de entrada com PID, tempo de chegada, burst time e prioridade dos processos.
- **Parte do Trabalho:** Entrada (A)

- **Arquivos:** README.md
- **Conteúdo:** Este manual de instruções.
- **Parte do Trabalho:** Entregável

- **Arquivos:** Relatorio.md
- **Conteúdo:** Relatório técnico detalhado sobre a implementação, métricas e análise.
- **Parte do Trabalho:** Entregável (D)

- **Arquivos:** output_rr.txt
- **Conteúdo:** Log de Saída (Gerado na execução da Parte A).
- **Parte do Trabalho:** Saída de Teste

- **Arquivos:** output_priority.txt
- **Conteúdo:** Log de Saída (Gerado na execução da Parte A).
- **Parte do Trabalho:** Saída de Teste

## 2. Pré-requisitos {#pré-requisitos}

O código foi desenvolvido em Python e requer algumas bibliotecas não
nativas para a geração dos gráficos de análise (Parte A).

1.  **Python 3.6+**

2.  **Bibliotecas:** Instale as dependências usando pip:
    > pip install pandas matplotlib

## 3. Instruções de Execução {#instruções-de-execução}

O script principal utiliza argumentos de linha de comando para
selecionar a parte do trabalho a ser executada. O arquivo processos.csv
deve estar no mesmo diretório.

### Modos de Execução

Use o seguinte comando no terminal (CMD, PowerShell ou Bash no
Linux/WSL), substituindo \<MODO\> por A, B, C ou ALL.

main.py \<MODO\>

#### 3.1. Modo A: Simulador de Escalonamento Lógico {#modo-a-simulador-de-escalonamento-lógico}

Executa os algoritmos Round-Robin (Q=100ms) e Prioridade Preemptiva no
ambiente lógico.

main.py A

**Saída:**

- Exibição das métricas globais e individuais no terminal.

- Geração de um **gráfico de barras comparativo** do Turnaround Time.

- Criação dos arquivos de log output_rr.txt e output_priority.txt.

#### 3.2. Modo B: Escalonador Round-Robin com Threads Reais {#modo-b-escalonador-round-robin-com-threads-reais}

Demonstra a simulação do Round-Robin usando threads reais do sistema
(threading).

main.py B

**Saída:**

- Log detalhado no console, mostrando a **Troca de Contexto** e o
  > **Progresso** de cada thread (T1, T2, T3, T4) em tempo real,
  > comprovando o controle de execução.

#### 3.3. Modo C: Sincronização e Concorrência {#modo-c-sincronização-e-concorrência}

Executa as demonstrações de problemas clássicos de concorrência.

main.py C

**Saída:**

- **C.1 Race Condition:** Exibe a perda de dados sem lock e o resultado

  > correto com Mutex.

- **C.2 Produtor-Consumidor:** Demonstra o uso de Semáforos (Semaphore e

  > Lock) para coordenar o acesso ao buffer limitado.

- **C.3 Deadlock:** Demonstra o cenário de bloqueio mútuo (DEADLOCK

  > DETECTADO!) e a falha do sistema.

- **C.4 Inversão de Prioridade:** Explicação conceitual e das soluções
  > (Herança de Prioridade).

#### 3.4. Modo ALL {#modo-all}

Executa as três partes em sequência.

main.py ALL
