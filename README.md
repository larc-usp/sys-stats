# sys-stats

Sys-stats é conjunto de ferramentas para realizar o monitoramento do uso de máquinas Linux e dispositivos com suporte a SNMP.

As ferramentas utilizadas são:

* Collectd
* InfluxDB
* Grafana

Arquitetura base para a coleta das métricas nos equipamentos que serão monitorados.

![Arch Sys Stats](sys-stats.png)



Cada cliente irá enviar os dados para o servidor que irá armazenar os dados no InfluxBD que posteriormente será utilizado pelo Grafana para visualizar os resultados.

[TOC]

## Plugins

Existem diversos Plugins para serem utilizados no Collectd, clique [plugins](plugins.md) serão listados apenas os utilizados nas configurações.

Uma lista completa pode ser encontrada na página oficinal [](https://collectd.org/wiki/index.php/Table_of_Plugins)



## Instalação e Configuração

Para realizar a instação e configuração tanto do cliente como do servidor, acesse: [Install.md](Install.md)

## Métricas

O collectd irá utilizar os plugins selecionados e irá coletar as informações de acordo com a configuração de cada um deles. Após isso, o collectd irá enviar os dados via rede para o banco de dados InfluxDB, que por sua vez, irá salvar os dados no banco de dados.

Como estamos utilizando o InfluxDB para salvar os dados, temos que criar queries especificas para cada uma das métricas que queremos avaliar.

Desta forma, podemos plugar o InfluxDB no Grafana para visualizar os gráficos e acompanhar durante o tempo as variações dessa métrica.

Acesse o arquivo [Metrics.md](Metrics.md) para visualizar as consultas SQLs para cada um dos plugins.



## Referências

https://grafana.net/dashboards/203

https://gist.github.com/uffsalot/2edca3a47c600b2341f45cb845af0308

https://marc.info/?l=collectd&m=141779559616998&w=4

https://collectd.org/wiki/index.php/Plugin:SNMP

https://github.com/collectd/collectd/issues/50

http://www.alexlinux.com/collectd-example-snmp-hp-1910-switch/