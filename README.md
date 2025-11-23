**🧠 Gerenciamento de Processos e Escalonamento**

Simulador de algoritmos de escalonamento e demonstrações de concorrência em Python.
Este projeto implementa um simulador completo de escalonamento de processos (Round-Robin e Prioridade Preemptiva) e demonstra, por meio de threads, conceitos fundamentais de concorrência e sincronização.

**📋 Pré-requisitos:**
- Sistema Operacional: Windows, Linux ou macOS
- Linguagem: Python 3.6+

Dependências externas: Nenhuma (somente biblioteca padrão)

📂 Estrutura do Projeto
- processos.csv                 ( Lista de processos: PID, tempo, prioridade etc.)
- process_scheduler_simulator.py ( Simulação RR e Prioridade Preemptiva )
- concurrency_demos.py          ( Demonstrações de concorrência e escalonamento cooperativo )
- output_rr.txt                 ( Log gerado da simulação Round-Robin )
- output_priority.txt           ( Log gerado da simulação por Prioridade )
- relatorio.md                  ( Relatório técnico do trabalho )
-  README.md                    ( Esse aqui )
 
▶️ Como Executar
1. Simulação de Escalonamento (Parte A)
Execute o simulador principal:
python process_scheduler_simulator.py

O script irá:
- Ler os dados de processos.csv
- Executar Round-Robin (Quantum = 50)
→ salvar log em output_rr.txt

- Executar Prioridade Preemptiva
→ salvar log em output_priority.txt

**Exibir no console as métricas:**
- Tempo médio de espera
- Tempo de turnaround
- Utilização da CPU

**2. Demonstrações de Concorrência e Threads (Partes B e C)**

**Execute:**
python concurrency_demos.py

**O script demonstra:**
- 🌀 Escalonamento Cooperativo (RR Simulado)
- Threads cedendo controle voluntariamente.

**⚠️ Condição de Corrida**
Exemplo com resultado incorreto e correção usando Mutex.

**📦 Produtor–Consumidor**
Implementado com queue.Queue (sincronização via semáforos internos).

**⛓️ Inversão de Prioridade**
Simulação conceitual do problema.

**💀 Deadlock**
O exemplo real está comentado para evitar travamentos, mas o código ilustra a solução com ordenamento de locks.

**🧩 Dependências**
Apenas biblioteca padrão do Python:
- csv
- threading
- time
- queue
- sys



📘 Autor / Observações

Este projeto foi desenvolvido com fins acadêmicos e demonstração de conceitos de sistemas operacionais, escalonamento e sincronização.
