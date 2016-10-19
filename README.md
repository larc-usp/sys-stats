# sys-stats


## Plugins

### LoadPlugin cpu

Este plugin coleta as métricas de uso da CPU. Por padrão, o uso da CPU é reportado como [Jiffies](http://www.makelinux.net/books/lkd2/ch10lev1sec3). É possível aplicar duas agregações:

1. Soma, por-estado, de todos os CPUs instalado no sistema; e
2. Soma, por-cpu, de todos os estados não-ociosos de uma CPU, criando um estado "ativo"

As duas agregações podem ser combinadas, levando o ```collectd``` a emitir apenas uma única métrica "ativa" para todo o sistema. Assim que uma dessas agragações (ou ambas) é ativada, o plugin irá reportar a porcentagem, ao invés do Jiffies. As seguintes configurações são disponíveis

* ReportByState true|false
	* Quando definido como ```true```, o padrão, será relatada métricas por-estado. Por exemplo: "system", "user" e "idle". Quando definido como ```false```, aggregates (sums) all non-idle states into one "active" metric.

ReportByCpu true|false
When set to true, the default, reports per-CPU (per-core) metrics. When set to false, instead of reporting metrics for individual CPUs, only a global sum of CPU states is emitted.

ValuesPercentage false|true
This option is only considered when both, ReportByCpu and ReportByState are set to true. In this case, by default, metrics will be reported as Jiffies. By setting this option to true, you can request percentage values in the un-aggregated (per-CPU, per-state) mode as well.

### LoadPlugin load



### LoadPlugin numa

### LoadPlugin disk

### LoadPlugin swap

### LoadPlugin memory

### LoadPlugin entropy

### LoadPlugin interface

### LoadPlugin network

### LoadPlugin processes

### LoadPlugin snmp



## Métricas

## CPU Average (over all cores)
## Load
## Processes (Forks, State)
## Memory Usage
## Disc Usage
## Network Packets, Traffic e Errors




Network Interface Statistics

List NIC names: .1.3.6.1.2.1.2.2.1.2
Get Bytes IN: .1.3.6.1.2.1.2.2.1.10
Get Bytes IN for NIC 4: .1.3.6.1.2.1.2.2.1.10.4
Get Bytes OUT: .1.3.6.1.2.1.2.2.1.16
Get Bytes OUT for NIC 4: .1.3.6.1.2.1.2.2.1.16.4



System Uptime: .1.3.6.1.2.1.1.3.0





## Switch

* CPU
* Temperatura
* Buffer?
* Memoria


snSwIfStatsInDiscard - Shows the number of inbound packets that will be discarded even though they have no errors. These packets will be discarded to prevent them from being delivered to a higher-layer protocol. For example, packets may be discarded to free up buffer space.

brcdIp.1.1.3.3.5.1.42


snSwIfStatsOutDiscard - Shows the number of outbound packets that will be discarded even though they contain no errors. For example, packets may be discarded to free up buffer space.

brcdIp.1.1.3.3.5.1.43


