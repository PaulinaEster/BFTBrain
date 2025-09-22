# Explicação da configuração `config.framework.yaml`

Este arquivo define os parâmetros de um ambiente de simulação de protocolos BFT (Byzantine Fault Tolerant).  
Abaixo, uma explicação seção por seção e o papel de cada campo no geral.

---

## network
- **server**: endereço e porta do servidor principal de controle/coordenação.  
- **units**: lista de nós (réplicas) que participam do protocolo BFT, com seus endereços/portas.  

Define a topologia da rede da simulação.

---

## general
- **f**: número máximo de falhas bizantinas toleradas (regra geral: precisa de 3f+1 réplicas).  
- **max-active-requests**: limite de requisições concorrentes que o sistema pode processar.  
- **verbosity**: nível de detalhe dos logs (`v` = verbose).  
- **logfile**: se os logs vão para arquivo (`True`) ou só console.  
- **learning**: se a simulação coleta dados de aprendizado (estatísticas, métricas).  
- **report-sequence**: intervalo de requisições após o qual o sistema gera relatórios de progresso.  
- **exchange-sequence**: frequência de troca de mensagens de sincronização entre nós.  

Configura comportamento global da simulação e como ela gera métricas.

---

## benchmark
- **block-size**: tamanho de bloco (quantas requisições agrupar antes de replicar).  
- **checkpoint-size**: quantas operações até criar um checkpoint para recuperação.  
- **catch-up-k**: número de checkpoints usados na recuperação de um nó atrasado.  
- **request-interval-micros**: intervalo em microssegundos entre requisições de clientes.  
- **benchmark-interval-ms**: frequência de geração de estatísticas de benchmark.  
- **timeout**: política de timeout (aqui é `fixed`).  
- **timeout-trigger-interval-ms**: intervalo de checagem de timeout.  
- **client**: tipo de cliente usado (ex: `basic`).  
- **closed-loop**: modelo de carga:  
  - **enable**: se usa clientes em loop fechado (cada cliente envia nova requisição só após receber resposta).  
  - **num-client**: quantidade de clientes simulados.  
  - **delay-ms**: atraso artificial entre requisições.  
- **leader-rotate-interval**: frequência de rotação de líder (em protocolos que suportam).  
- **aggregation-delay-ms**: atraso antes de agrupar requisições em blocos.  

Controla como os benchmarks são executados e como a carga de trabalho é simulada.

---

## workload
- **contention-level**: nível de concorrência/acesso simultâneo a dados.  
- **dataset-size**: tamanho do conjunto de dados simulado.  
- **payload**:  
  - **request-size**: tamanho da requisição em bytes.  
  - **reply-size**: tamanho da resposta em bytes.  
- **compute-factor**: custo de computação artificial por requisição (simula operações pesadas).  
- **distribution**: distribuição de acesso (ex: [1,0,0,0] = todos acessam um único item).  
- **read-only-ratio**: proporção de operações somente leitura.  

Define o tipo de carga de trabalho que será simulada.

---

## fault
Modela falhas bizantinas ou problemas de desempenho.  
- **in-dark**: nó fica “no escuro” (não responde) em certos intervalos.  
  - **affected-entities**: IDs dos nós afetados.  
  - **schedule**: define quando os nós entram e saem da falha.  
- **timeout**: injeta falhas de timeout em determinados nós.  
- **slow-proposal**: líder ou nó propositor fica mais lento de propósito.  

Permite testar a resiliência do protocolo em cenários de falha.

---

## switching
- **protocol-pool**: lista de protocolos BFT que podem ser ativados (PBFT, Zyzzyva, HotStuff, etc.).  

Permite comparar diferentes protocolos no mesmo ambiente.

---

## demo
- **enabled**: se o modo demonstração está ativo.  
- **update_interval_ms**: frequência de envio de atualizações para visualização.  
- **update_server** / **update_port**: endereço e porta do servidor que recebe dados da simulação.  

Serve para visualização em tempo real ou integração com ferramentas externas.

---

## Resumo
Este YAML configura a rede (nós e servidor), parâmetros gerais do sistema, execução de benchmarks, tipo de carga de trabalho, injeção de falhas, protocolos disponíveis e até um modo demo para visualização.  
Ele é bem flexível para simular diferentes cenários e medir como cada protocolo BFT se comporta.