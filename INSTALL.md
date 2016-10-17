
# Introdução

A ideia é basear a arquitetura nesse exemplo.

![](https://community.rackspace.com/cfs-file.ashx/__key/communityserver-discussions-components-files/25/2870.Screen-Shot-2015_2D00_12_2D00_28-at-3.19.58-PM.png) 

## CollectD

É um deamon que coleta informações estatísticas do desempenho do sistema e fornece mecanismos para salvar esses valores de diferentes formas. 

A ideia é enviar as métricas do collectd para o Graphite (carbon) utilizando o plugin do collectd chamado write_graphite. 

## Graphite

É um sistema de criação de gráficos em tempo real. Consiste de 3 componentes de software:

**carbon-cache**: um serviço de rede que escuta por métricas de entrada. Armazena as métricas temporariamente na memória (buffer-cache) durante um pequeno período antes de escrever no disco na formato do banco de dados Whisper (que tem a mesma similaridade do RRD (round-robin-database)).

**whisper**

Uma biblioteca de banco de dados para armazenar dados em tempo-real.

**graphite-web**

Um webapp do Django que renderiza os gráficos sob demanda utilizando o Cairo.

## Grafana

Substituiu o dashboard do Graphite com edição de gráficos e interface para criação de dashboards. Ele recuperar todos os dados necessários do Graphite.

## Seyren (opcional)

Dashboard Graphite para alertas. Suporte notificações via Email, Flowdock, HipChat, HTTP, Hubot, IRCcat, PagerDuty, Pushover, SLF4J, Slack, SNMP, Twilio.

# Instalação e Configuração

















