# sys-stats - Install

* [sys-stats - Install](#sys-stats---install)
     * [Collectd](#collectd)
        * [Servidor](#servidor)
        * [Cliente](#cliente)
     * [InfluxDB](#influxdb)
     * [Grafana](#grafana)
     * [Refs:](#refs)

## Collectd

Para instalar os pacotes necessários para carregar boa parte dos plugins do Collectd no Sistema Operacional CentOS 6 deve-se digitar:

```bash
sudo yum install libatasmart-devel libcurl-devel libdbi-devel libesmtp-devel ganglia-devel libgcrypt-devel hal-devel hiredis-devel iptables-devel java-1.8.0-openjdk-devel openldap-devel lvm2-devel libmemcached-devel libmnl-devel libmodbus-devel mysql-devel net-snmp-devel libnotify-devel OpenIPMI-devel liboping-devel libpcap-devel perl-devel perl-ExtUtils-Embed postgresql-devel librabbitmq-devel rrdtool-devel lm_sensors-devel libstatgrab-devel libudev-devel nut-devel varnish-libs-devel libvirt-devel libxml2-devel yajl-devel protobuf-c-devel python-devel librabbitmq-devel libcurl-devel libcurl-devel libxml2-devel yajl-devel libcurl-devel libcurl-devel yajl-devel libcurl-devel libxml2-devel libdbi-devel libpcap-devel ganglia-devel OpenIPMI-devel iptables-devel java-1.8.0-openjdk-devel yajl-devel lvm2-devel libmemcached-devel mysql-devel libmnl-devel libcurl-devel libnotify-devel libesmtp-devel nut-devel openldap-devel perl-devel perl-ExtUtils-Embed liboping-devel postgresql-devel python-devel hiredis-devel rrdtool-devel libatasmart-devel libudev-devel net-snmp-devel varnish-libs-devel libvirt-devel libxml2-devel libcurl-devel hiredis-devel 
```

Após isso, iremos instalar a última versão do Collectd 5.6.1. Para isso precisamos instalar os pacotes necessários para a compilação:

```bash
sudo yum install make automake gcc gcc-c++ kernel-devel perl-develbzip2
```

Baixar a última versão do Collectd:

```bash 
cd ~
curl -O https://collectd.org/files/collectd-5.6.1.tar.bz2
tar -jxf collectd-5.6.1.tar.bz2 
cd collectd-5.6.1
```

Pode acontecer de que o compilador apresente avisos (Warnings) como erro. Vamos ignora-los utilizando o comando:

```bash
export CFLAGS="-Wno-error"
```

Agora vamos configurar, compilar e instalar!

```bash
./configure
make 
sudo make install
```

Por fim, vamos criar um script para iniciar o collectd bem como criar os links simbólicos necessários.

```bash
sudo cp ~/collectd-5.6.1/contrib/redhat/init.d-collectd /etc/init.d/collectd

sudo chmod +x /etc/init.d/collectd

sudo ln -s /opt/collectd/etc/collectd.conf /etc/collectd.conf
sudo ln -s /opt/collectd/sbin/collectd /usr/sbin/collectd
sudo ln -s /opt/collectd/sbin/collectdmon /usr/sbin/collectdmon
```

Iniciando o serviço para verificar se tudo está ok:

```bash
sudo service collectd start
```

Note que a saída será semelhante:

```bash
Starting collectd: not starting due to configuration error [FAILED]
```

Para iniciar o serviço corretamente precisamos habilitar os plugins que desejamos e também definir alguns parâmetros de configuração no arquivo /etc/collectd.conf.

Em ambos os casos os parâmetros que servem tanto para o Servidor, quanto para o Cliente são descritos abaixo:

```bash
# Hostname definido pelo usuário
Hostname    "hostname_clienteX"
BaseDir     "/var/lib/collectd"
PIDFile     "/var/run/collectd.pid"
TypesDB     "/opt/collectd/share/collectd/types.db"

# Intervalo de captura
Interval     5

# Log
LoadPlugin syslog

<Plugin syslog>
        LogLevel info
</Plugin>

# Plugins habilitados
LoadPlugin contextswitch
LoadPlugin cpu
LoadPlugin df
LoadPlugin disk
LoadPlugin entropy
LoadPlugin interface
LoadPlugin irq
LoadPlugin load
LoadPlugin memory
LoadPlugin network
LoadPlugin numa
LoadPlugin processes
LoadPlugin swap
LoadPlugin tcpconns
LoadPlugin uptime
LoadPlugin users

# Configurações dos plugins
<Plugin cpu>
  ReportByCpu true
  ReportByState true
  ValuesPercentage true
</Plugin>

<Plugin df>
        ReportInodes true
        ValuesAbsolute true
        ValuesPercentage true
</Plugin>

<Plugin tcpconns>
        AllPortsSummary true 
</Plugin>

```



O que muda do servidor para o cliente é que o plugin de rede (network) deve ser configurado no servidor para receber os dados, enquanto que no cliente ele deve enviar os dados. Além disso, nesse caso o servidor irá coletar as informações via SNMP do Switch.

### Servidor

```bash
Hostname    "hostname_servidor"

# Plugin habilitado para salvar os logs em arquivo
LoadPlugin logfile

# Definição dos parâmetros do plugin logfile
<Plugin logfile>
        LogLevel notice
        File "/var/log/collectd.log"
        Timestamp true
</Plugin>



# Plugin Habilitado
LoadPlugin snmp

# Configuração do plugin de rede para receber os dados dos clientes
<Plugin network>
        <Server "127.0.0.1" "25826">
        </Server>
</Plugin>

# MIBs utilizadas para recuperar as informações do Switch
<Plugin snmp>
    <Data "trafego">
        Type "if_octets"
        Table true
        Instance "IF-MIB::ifDescr"
        Values "IF-MIB::ifInOctets" "IF-MIB::ifOutOctets"
        Scale 8
    </Data>
    <Data "temp_cpu_board">
        Type "temperature"
        Table false
        Instance "temp_cpu_board"
        Values ".1.3.6.1.4.1.1991.1.1.2.13.1.1.4.3.1"
        Scale 0.5
    </Data>

    <Data "temp_cpu_chip">
        Type "temperature"
        Table false
        Instance "temp_cpu_chip"
        Values ".1.3.6.1.4.1.1991.1.1.2.13.1.1.4.3.2"
        Scale 0.5
    </Data>

    <Data "uptime">
        Type "uptime"
        Table false
        Instance "uptime"
        Values ".1.3.6.1.2.1.1.3.0"
    </Data>

    <Host "switch">
        Address "143.107.6.17"
        Version 1
        Community "public"
        Collect "trafego" "temp_cpu_board" "temp_cpu_chip" "uptime"
        Interval 5
    </Host>
</Plugin>          
```



### Cliente

```bash
<Plugin network>
		# Nome ou IP do servidor
        <Server "nome_ou_ip_servidor" "25826">
        </Server>
</Plugin>
```



Uma vez que as configurações forem realizadas, basta reiniciar os serviços em ambos os servidores (cliente e servidor)

```bash
sudo service collectd start
```

A saída será:

```bash
Starting collectd:                                         [  OK  ]
```

Pronto! Coleta sendo realizada :)

Agora precisamos instalar o InfluxDB que irá armazenar os dados e depois instalar o Grafana para recuperar os dados e criar os Dashboards.

## InfluxDB

Para instalar o InfluxDB no CentOS 6, iremos utilizar um repositório próprio disponibilizado.

Crie o arquivo:

```bash
sudo vim /etc/yum.repos.d/influxdb.repo
```

E adicione o conteúdo:

```bash
[influxdb]
name = InfluxDB Repository - RHEL $releasever
baseurl = https://repos.influxdata.com/rhel/$releasever/$basearch/stable
enabled = 1
gpgcheck = 1
gpgkey = https://repos.influxdata.com/influxdb.key
```

Basta instalar:

```bash
sudo yum install influxdb
```

Abra o arquivo:

```bash
sudo vim /etc/influxdb/influxdb.conf
```

E procure pela a string collectd e deixe da seguinte forma:

```bash
[[collectd]]
  enabled = true
  bind-address = ":25826"
  database = "collectd"
  typesdb = "/opt/collectd/share/collectd/types.db"
```

Com isso já, todos os dados que forem enviados para a porta 25826 o InfluxDB irá salvar no banco de dados.

Feche e inicie o serviço:

```bash
sudo service influxdb start
```

Para verificar, acesse:

http://IP_SERVIDOR:8083 

Essa página permite realizar consultas SQLs nos dados que foram salvos. Note que é necessário selecionar a base de dados corretamente do lado superior direito.



**Observação**: É importante verificar que essa porta é aberta, talvez seja interessante definir algum tipo de autenticação. Para isso basta procurar a tag de controle [http] dentro do arquivo de configuração e definir as politicas que melhor se adequam as suas necessidades.

Agora que já estamos salvando os dados 



## Grafana

Para instalar o Grafana no CentOS 6, iremos utilizar um repositório próprio disponibilizado.

Crie o arquivo:

```bash
sudo vim /etc/yum.repos.d/grafana.repo
```

E adicione o conteúdo:

```bash
[grafana]
name=grafana
baseurl=https://packagecloud.io/grafana/stable/el/6/\$basearch
repo_gpgcheck=1
enabled=1
gpgcheck=1
gpgkey=https://packagecloud.io/gpg.key https://grafanarel.s3.amazonaws.com/RPM-GPG-KEY-grafana
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
```

Salve e digite:

```bash
sudo yum install grafana
```

Habilite a inicialização do grafana quando o sistema for reiniciado:

```bash
sudo chkconfig --add grafana-server
```

E inicialize o serviço:

```bash
sudo service grafana-server start
```

Acesse:

http://IP_SERVIDOR:3000/login

Por padrão a senha é admin/admin

Pronto agora é só criar os Dashboards iterativos através das consultas SQLs disponibilizadas pelo InfluxDB. Acesse [Grafana.md](Grafana.md).

## Refs:

- https://support.ciscozeus.io/support/solutions/articles/9000035008-how-to-compile-and-install-collectd-5-5-on-centos-6
- https://github.com/httpdss/collectd-web/blob/master/cgi-bin/collection.modified.cgi
- https://raymii.org/s/tutorials/Collectd_server_setup_tutorial_with_web_frontend.html
- https://collectd.org/wiki/index.php/Table_of_Plugins
- http://www.tecmint.com/install-collectd-and-collectd-web-to-monitor-server-resources-in-linux/2/
- https://raymii.org/s/tutorials/Collectd_client_setup_tutorial.html
- https://raymii.org/s/tutorials/Collectd_server_setup_tutorial_with_web_frontend.html
- https://collectd.org/wiki/index.php/Troubleshooting