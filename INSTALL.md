
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

## CollectD

Instalando os requisitos:

```bash
sudo yum install libcurl libcurl-devel rrdtool rrdtool-devel rrdtool-prel libgcrypt-devel gcc make gcc-c++
```

Instalando os pacotes para os diversos plugins.

```bash
sudo yum install libatasmart-devel libcurl-devel libdbi-devel libesmtp-devel ganglia-devel libgcrypt-devel hal-devel hiredis-devel iptables-devel java-1.8.0-openjdk-devel openldap-devel lvm2-devel libmemcached-devel libmnl-devel libmodbus-devel mysql-devel net-snmp-devel libnotify-devel OpenIPMI-devel liboping-devel libpcap-devel perl-devel, perl-ExtUtils-Embed postgresql-devel librabbitmq-devel rrdtool-devel lm_sensors-devel libstatgrab-devel libudev-devel nut-devel varnish-libs-devel libvirt-devel libxml2-devel yajl-devel protobuf-c-devel python-devel librabbitmq-devel libcurl-devel libcurl-devel, libxml2-devel yajl-devel libcurl-devel libcurl-devel, yajl-devel libcurl-devel, libxml2-devel libdbi-devel libpcap-devel ganglia-devel OpenIPMI-devel iptables-devel java-1.8.0-openjdk-devel yajl-devel lvm2-devel libmemcached-devel mysql-devel libmnl-devel libcurl-devel libnotify-devel libesmtp-devel nut-devel openldap-devel perl-devel, perl-ExtUtils-Embed liboping-devel postgresql-devel python-devel hiredis-devel rrdtool-devel libatasmart-devel, libudev-devel net-snmp-devel varnish-libs-devel libvirt-devel, libxml2-devel libcurl-devel hiredis-devel 
```

Download da versão 5.6.1 collectd.

```bash
wget https://storage.googleapis.com/collectd-tarballs/collectd-5.6.1.tar.bz2
cd collectd-5.6.1/
./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var --libdir=/usr/lib --mandir=/usr/share/man 
make
sudo make install
```

Criar script para iniciar o collectd:

```bash
cp contrib/redhat/init.d-collectd /etc/init.d/collectd
sudo cp contrib/redhat/init.d-collectd /etc/init.d/collectd
sudo +x /etc/init.d/collectd 
sudo chmod +x /etc/init.d/collectd 
service collectd status
```

## Graphite

```bash


https://support.ciscozeus.io/support/solutions/articles/9000035008-how-to-compile-and-install-collectd-5-5-on-centos-6
















