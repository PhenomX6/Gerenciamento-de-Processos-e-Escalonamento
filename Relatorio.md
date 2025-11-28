# RELATÓRIO TÉCNICO: SIMULADOR DE ESCALONAMENTO E CONCORRÊNCIA

Alunos: Felipe Vieira e Samuel Roberto
Curso: Engenharia da Computação
Disciplina: Sistemas Operacionais

## 1. Introdução {#introdução}

Este projeto tem como objetivo a implementação prática de conceitos
fundamentais de sistemas operacionais, especificamente o gerenciamento
de processos (escalonamento de CPU) e o gerenciamento de threads
(concorrência e sincronização).

O trabalho foi desenvolvido em linguagem **Python**, escolhida pela sua
robustez na manipulação de estruturas de dados e suporte nativo a
threads. O projeto divide-se em três partes lógicas:

1.  **Simulação Algorítmica:** Implementação lógica de Round-Robin e

    > Prioridade Preemptiva.

2.  **Threads Reais:** Implementação de um escalonador cooperativo

    > utilizando threads do sistema operacional.

3.  **Concorrência:** Demonstração empírica de problemas clássicos como
    > Race Condition, Deadlock e Produtor-Consumidor.

Como extensão funcional (bônus), foi implementado um módulo de análise
de dados utilizando as bibliotecas pandas e matplotlib, permitindo a
visualização gráfica comparativa das métricas de desempenho.

## 2. Parte A: Simulador de Escalonamento Lógico {#parte-a-simulador-de-escalonamento-lógico}

Nesta etapa, o sistema operacional é simulado logicamente. Não há
execução real de código dos processos, apenas o cálculo de métricas
baseadas no tempo de simulação.

### 2.1. Estrutura de Dados (PCB) {#estrutura-de-dados-pcb}

Foi criada a classe Process que atua como o Bloco de Controle de
Processo (PCB), armazenando:

- PID: Identificador único.

- Burst Time: Tempo total de CPU necessário.

- Remaining Time: Tempo restante de execução (decrementado durante a

  > simulação).

- Priority: Nível de prioridade (menor valor = maior prioridade).

- Métricas: Tempos de Chegada, Início, Finalização e Espera.

### 2.2. Algoritmos Implementados {#algoritmos-implementados}

#### Round-Robin (Quantum = 100ms) {#round-robin-quantum-100ms}

Utiliza uma estrutura de fila (collections.deque) para gerenciar os
processos prontos.

- **Lógica:** O escalonador remove o processo da cabeça da fila e

  > concede a CPU por um tempo t = min(quantum, restante).

- **Preempção:** Se o processo não terminar após o tempo t, ele sofre
  > preempção e é reinserido no final da fila. Novos processos que
  > chegam durante a execução são inseridos na fila antes do processo
  > preemptado.

#### Prioridade Preemptiva

Utiliza uma lista dinâmica ordenada.

- **Lógica:** A cada ciclo de relógio ou chegada de novo processo, a

  > lista de prontos é reordenada baseada na tupla (prioridade,
  > tempo_chegada).

- **Preempção:** Se um processo chega com prioridade numérica menor do
  > que o processo em execução, ocorre a troca de contexto imediata. O
  > processo atual volta para a fila de prontos e o novo processo assume
  > a CPU.

### 2.3. Visualização de Dados (Extensão) {#visualização-de-dados-extensão}

Para enriquecer a análise, os dados brutos da simulação são convertidos
em _DataFrames_ do Pandas. Isso permite o cálculo automático de médias e
a geração de gráficos de barras via Matplotlib, facilitando a comparação
visual do _Turnaround Time_ entre os algoritmos.

## 3. Parte B: Simulação com Threads Reais {#parte-b-simulação-com-threads-reais}

Diferente da Parte A, aqui utilizamos a biblioteca threading para criar
fluxos de execução reais no sistema operacional hospedeiro.

### 3.1. Escalonador Cooperativo {#escalonador-cooperativo}

Como o Python (devido ao GIL - Global Interpreter Lock) e o SO gerenciam
threads nativamente, foi necessário forçar o comportamento de um
escalonador Round-Robin.

- **Mecanismo de Controle:** Cada \"processo\" é uma thread que possui

  > um objeto threading.Event exclusivo.

- **Execução:** A thread fica em estado de espera (event.wait()). O

  > escalonador principal \"acorda\" a thread (event.set()), aguarda o
  > tempo do quantum (simulado com time.sleep) e então \"congela\" a
  > thread novamente (event.clear()).

- **Troca de Contexto:** Este mecanismo simula o _overhead_ da troca de
  > contexto, permitindo medir a eficiência real da CPU e visualizar o
  > progresso intercalado das tarefas no console.

## 4. Parte C: Concorrência e Sincronização {#parte-c-concorrência-e-sincronização}

Esta seção aborda a coordenação de acesso a recursos compartilhados.

### 4.1. Race Condition (Condição de Corrida) {#race-condition-condição-de-corrida}

- **Cenário:** Duas threads incrementam uma variável global (counter)

  > 50.000 vezes cada.

- **Problema:** Sem sincronização, ocorre a leitura e escrita

  > simultânea, resultando em \"atualizações perdidas\" (ex: valor final
  > 70.000 ao invés de 100.000).

- **Solução:** Implementação de um threading.Lock() (Mutex). O bloco
  > with lock: garante que apenas uma thread entre na região crítica por
  > vez, assegurando a consistência (valor final 100.000), ao custo de
  > um maior tempo de execução.

### 4.2. Produtor-Consumidor {#produtor-consumidor}

- **Cenário:** Threads produtoras geram dados em um _Buffer_ limitado, e

  > consumidoras os removem.

- **Solução:** Utilização de Semáforos para controle de fluxo:

  1.  Semaphore(BUFFER_SIZE) (Empty): Garante que o produtor pare se o

      > buffer estiver cheio.

  2.  Semaphore(0) (Full): Garante que o consumidor espere se o buffer

      > estiver vazio.

  3.  Lock(): Garante exclusividade na manipulação da lista do buffer.

### 4.3. Deadlock (Impasse) {#deadlock-impasse}

- **Cenário:** Thread 1 segura Recurso A e pede B. Thread 2 segura B e

  > pede A.

- **Demonstração:** O código força essa situação de espera circular. Foi
  > implementado um mecanismo de detecção com _timeout_: se as threads
  > não concluem em X segundos, o sistema diagnostica o Deadlock e
  > encerra a simulação, prevenindo o congelamento total da aplicação.

## 5. Análise Comparativa e Resultados {#análise-comparativa-e-resultados}

Utilizando o arquivo de entrada processos.csv fornecido:

- **P1:** Chegada 0, Burst 250, Prio 3

- **P2:** Chegada 50, Burst 170, Prio 4

- **P3:** Chegada 130, Burst 75, **Prio 1 (Alta)**

- **P4:** Chegada 190, Burst 100, Prio 2

- **P5:** Chegada 210, Burst 130, **Prio 5 (Baixa)**

###

###

###

###

### 5.1. Comparação de Algoritmos {#comparação-de-algoritmos}

| **Métrica**           | **Round-Robin (RR)** | **Prioridade Preemptiva (PP)** | **Análise**                                                                                                                     |
| --------------------- | -------------------- | ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- |
| **Justiça**           | Alta                 | Baixa                          | O RR distribui CPU para todos. O PP ignora P5 enquanto houver processos mais importantes.                                       |
| **Turnaround Médio**  | Alto                 | Baixo                          | O PP finaliza rapidamente processos curtos e importantes (P3, P4), reduzindo a média geral.                                     |
| **Tempo de Resposta** | Rápido               | Variável                       | No RR, todos iniciam rapidamente. No PP, P5 sofreu grande atraso inicial.                                                       |
| **Starvation**        | Não ocorre           | Ocorre                         | **P5** (Prio 5) sofreu _starvation_ temporário no algoritmo de Prioridade, executando apenas após o término de todos os outros. |

### 5.2. Análise do Gráfico (Turnaround) {#análise-do-gráfico-turnaround}

O gráfico gerado pelo sistema demonstra claramente que processos de alta
prioridade (como **P3**) têm um _Turnaround Time_ drasticamente menor no
algoritmo de Prioridade em comparação ao Round-Robin. Em contrapartida,
processos longos e de baixa prioridade (como **P1** e **P5**) têm
desempenho similar ou pior no algoritmo de Prioridade, pois são
constantemente interrompidos.

## 6. Conclusão {#conclusão}

O projeto permitiu validar experimentalmente a teoria de sistemas
operacionais.

Conclui-se que:

1.  Não existe \"melhor\" algoritmo universal; o **Round-Robin** é ideal

    > para sistemas de tempo compartilhado (interatividade), enquanto
    > **Prioridade** é vital para sistemas de tempo real ou missão
    > crítica.

2.  A gestão de **Threads** impõe desafios significativos: a

    > sincronização (Locks/Semáforos) é obrigatória para evitar
    > inconsistência de dados, mas seu uso incorreto pode levar a
    > degradação de performance ou Deadlocks.

3.  A visualização gráfica implementada como extensão provou-se
    > essencial para identificar gargalos de desempenho e o impacto da
    > inanição em processos de baixa prioridade.
