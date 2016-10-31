# sys-stats - Plugins

[TOC]

### LoadPlugin contextswitch

Coleta o número de troca de contexto realizado pelo sistema operacional. Essa troca de contexto é o processo computacional de armazenar e restaurar o estado (contexto) de uma CPU de forma que múltiplos processos possam compartilhar uma única instância de CPU.

### LoadPlugin cpu

Este plugin coleta as métricas de uso da CPU. Por padrão, o uso da CPU é reportado como [Jiffies](http://www.makelinux.net/books/lkd2/ch10lev1sec3). É possível aplicar duas agregações:

1. Soma, por-estado, de todos os CPUs instalado no sistema; e
2. Soma, por-cpu, de todos os estados não-ociosos de uma CPU, criando um estado "ativo"

As duas agregações podem ser combinadas, levando o ```collectd``` a emitir apenas uma única métrica "ativa" para todo o sistema. Assim que uma dessas agragações (ou ambas) é ativada, o plugin irá reportar a porcentagem, ao invés do Jiffies. As seguintes configurações são disponíveis

- ```ReportByState true|false```
  - Quando definido como ```true``` as métricas serão por-estado (é o padrão). Por exemplo: "system", "user" e "idle". 
  - Quando definido como ```false```, agrega todos os estados não-ociosos em uma métrica ativa.
- ```ReportByCpu true|false```
  - Quando definido como ```true```, as métricas são reportadas por-cpu (é o padrão). 
  - Quando definido como ```false```, a soma global dos estados da CPU ser]a emitida.
- ```ValuesPercentage false|true``` - Essa opção só é considerada se ReportByCPU e ReportByState estiverem definidas como ```true```. Nesse caso, por padrão, as métricas são reportadas como Jiffies. Setando essa opção para ```true```, é possível requisitar valores em porcentagem no modo não agregado por-CPU e por-estado.

### LoadPlugin df

Este plugin coleta informações sobre o uso do sistema. Basicamente o quanto de espaço é utilizado e disponível em cada partição montada. Mais informações sobre este plugin pode ser visualizado em [](https://collectd.org/wiki/index.php/Plugin:DF)

### LoadPlugin disk

Coleta informações sobre o uso do disco físico, bem como das partições lógicas. Valores coletados são:

- O número de octetos escritos e lidos do disco ou partição. 
- O número de operações de leitura/escrita no disco.
- O tempo em que esses comandos foram executados.

Utilizando as duas opções a seguir, é possível ignorar algum disco ou configurar uma coleção de discos que serão utilizados.

- ``` Disk Name``` - Seleciona o disco *Name*. Os discos definidos aqui podem ser utilizados para coletar os dados ou não, isso irá depender das configurações da opção *IgnoreSelected*. Exemplo:

```bash
Disk "sdd"
Disk "/hda[34]/"
```

Note que no segundo caso, estamos aplicando uma expressão regular que começa e termina com / (barra). Nesse caso, serão realizadas as capturar do ```sdd, hda3, hda4```.

- ```IgnoreSelected true|false``` - Define os discos se selecionados. Ou seja, se IgnoreSelected for definido:
  - ```true``` então todos os discos que não foram definidos anteriormente serão coletados.
  - ```false``` então todos os discos que foram definidos serão coletados.

Caso nenhum disco seja definido anteriormente, todos os discos serão coletados.

### LoadPlugin entropy

Coleta a entropia de um sistema. A entropia é importante para gerar numeros aleatórios, os quais podem ser utilizados para criptografia, autorização e tarefas similares. Os valores são em bits.

### LoadPlugin interface

- ```Interface *Interface*``` - Seleciona a *Interface*. Por padrão as interfaces definidas aqui (pode ser mais que uma) serão coletadas. Mais detalhes em IgnoreSelected.


- ```IgnoreSelected true|false``` - Se nenhuma configuração for definida o plugin irá coletar dados de todas as interfaces. Isso pode não ser prático, especialmente para interfaces de loopback ou similares. Assim, é possível escolher as interfaces que está interessado ou então definir as interfaces que não está interessado. Portanto, se IgnoreSelected estiver como:
  - ```true``` - todas as interfaces não listadas anteriormente serão coletadas.
  - ```false``` - todas as interfaces listas anteriormente serão coletadas.

É possível utilizar expressões regulares nos nomes das interfaces. Se / (barra) for adicionado nas extremidades dos nomes será utilizado regexps. 

- ```Um exemplo```

```bash
 Interface "lo"
 Interface "/^veth/"
 Interface "/^tun[0-9]+/"
 IgnoreSelected "true"
```

 Isso irá ignorar a interface de loopback, bem como todas as interfaces que começarem com a palavra ```veth```, bem como todas as interfaces que começam com ```tun``` seguido por pelo menos um digito.

### LoadPlugin irq

IRQ é um sinal de hardware enviado ao processador que temporariamente para um programa em execução e permite que um programa especial, um manipulador de interrupções seja executado. Interrupções de hardware são utilizados para manipular eventos como recebimento de dados de um modem ou placa de rede, movimento do mouse etc.

Este plugin coleta o número de vezes que cada interrupção foi tratada pelo sistema operacional.

### LoadPlugin load

Coleta informações sobre a carga do sistema (System Load). Esse números são uma visão geral sobre a utilização de uma máquina. A carga do sistema é definida como a média dos números de tarefas esperando para ser executadas ou em execução nos últimos períodos de 1, 5 e 15 minutos.

- ```ReportRelative false|true``` - Quando habilitado, a carga do sistema é dividida pelo número de núcleos de CPU disponíveis é relatado para intervalos de 1, 5 e 15 minutos.

### LoadPlugin memory

O plugin de memória fornece as seguintes opções de configuração:

- ```ValuesAbsolute true|false``` - Habilita ou desabilita a coleta do uso da memória física em valores absolutos, por exemplo: bytes. Padrão é ```true```.
- ```ValuesPercentage false|true``` - Habilita ou desabilita a coleta do uso da memória física em porcentagem, por exemplo: a porcentagem da memória física utilizada. O padrão é falso.

### LoadPlugin network

Este plugin envia os dados para uma instancia remota do collectd. Recebe dados de uma instancia remota, ou as duas ao mesmo tempo.

Vamos utilizar esse plugin no momento da configuração e instalação.

Mais informações podem ser visualizadas em: https://collectd.org/documentation/manpages/collectd.conf.5.shtml#plugin_network

### LoadPlugin numa

Este plugin reporta as estatísticas do Acesso Não-Uniforme a Memória (NUMA - Non-Uniform Memory Access). 

As informações capturadas são:

- numa_hit - é a memória alocada com êxito neste nó como pretendido.
- numa_miss - é a memória alocada neste nó, mesmo o processo escolher algum nó diferente. Cada **numa_miss** tem uma **numa_foreign** em outro nó.
- numa_foreign - é a memória 
- local_node - Número de páginas alocadas desse nó enquanto o processo roda localmente.
- other_node - Número de páginas alocadas desse nó enqunato o processo estava rodando remotamente (em outro nó)
- interleave_hit - Número de página alocadas com sucesso com a estratégia **interleave**.

Refs: 

- http://blog.tsunanet.net/2011/06/clarifications-on-linuxs-numa-stats.html
- https://en.wikipedia.org/wiki/Non-uniform_memory_access
- http://man7.org/linux/man-pages/man8/numastat.8.html

### LoadPlugin processes

Coleta o número de processos nos estados de:

- running
- sleeping 
- paging
- stopped
- blocked - o processo está esperando algum evento, como um recurso se tornar disponível ou uma operação de I/O esteja completa.
- zombies - o processo foi finalizado com sucesso, porém a mudança do estado ainda não foi reconhecida pelo processo pai.

### LoadPlugin swap

Coleta informações sobre o espaço de troca (swap) disponível e utilizado. Opções disponíveis no Linux ou Solaris.

- ```ReportByDevice false|true``` - Configura como reportar os dispositivos físicos de swap. Se:
  - ```false``` - apenas o resumo de todos os dispositivos de swap. Por exemplo: o uso e disponibilidade global do espaço em todos os dispositivos.
  - ```true``` - uso e disponibilidade do espaço de todos os dispositivos separadamente.

Essa opção está disponível apenas se o plugin possa ler /proc/swaps (no Linux) ou utilizar o mecanismos swapctl(2) (no Solaris).

- ```ReportBytes false|true``` - Quando habilidade, o I/O do swap é reportado em bytes. Quando disabilidade, o swap I/O é reportado em páginas. Opção apenas para Linux.
- ```ValuesAbsolute true|false``` - Habilidade ou desabilita relatórios de métricas de swap absolutos, isto é, número de bytes disponíveis e utilizados. O padrão é verdadeiro.
- ```ValuesPercentage false|true``` - Habilita ou desabilita relatórios de métricas de swap relativos, isto é, porcentagem disponível e livre. O padrão é ```true```.

### LoadPlugin tcpconns

Este plugin conta o número de conexões TCPs para ou de uma porta específica. Tipicamente as conexões onde especifica-se a porta local são conexões de entrada, enquanto as conexões onde especifica-se as portas remotas são as conexões de saída.

### LoadPlugin uptime

Este plugin mantém o controle do tempo de funcionamento do sistema, fornecendo informações tais como o tempo de execução médio ou o tempo de atividade máxima atingida durante um certo período de tempo.

Iremos utilizar esse plugin para listar todos os hosts que queremos visualizar no dashboard.

### LoadPlugin users

Mantém a contagem de quantos usuários atualmente estão logados no sistema.

### LoadPlugin snmp

Este plugin snmp, posibilita utilizar o Simple Network Management Protocol (SNMP) e traduz o valor recebido para o collectd.

Consultar um dispositivo via SNMP pode causar timeout, desta forma, multiplas threads são utilizadas para a consulta em paralelo. Dependendo o número de dispositivos será utilizada um ou dez threads.

As configurações via SNMP serão utilizadas para recuperar métricas do Switch.