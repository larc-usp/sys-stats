# Instalação do Collectd + Collectd Web

Descrição

## Instalação e Configuração do Collectd Server

# Instalação e Configuração do Collectd Client

Instalando pacotes para os plugins:

```bash
sudo yum install libatasmart-devel libcurl-devel libdbi-devel libesmtp-devel ganglia-devel libgcrypt-devel hal-devel hiredis-devel iptables-devel java-1.8.0-openjdk-devel openldap-devel lvm2-devel libmemcached-devel libmnl-devel libmodbus-devel mysql-devel net-snmp-devel libnotify-devel OpenIPMI-devel liboping-devel libpcap-devel perl-devel perl-ExtUtils-Embed postgresql-devel librabbitmq-devel rrdtool-devel lm_sensors-devel libstatgrab-devel libudev-devel nut-devel varnish-libs-devel libvirt-devel libxml2-devel yajl-devel protobuf-c-devel python-devel librabbitmq-devel libcurl-devel libcurl-devel libxml2-devel yajl-devel libcurl-devel libcurl-devel yajl-devel libcurl-devel libxml2-devel libdbi-devel libpcap-devel ganglia-devel OpenIPMI-devel iptables-devel java-1.8.0-openjdk-devel yajl-devel lvm2-devel libmemcached-devel mysql-devel libmnl-devel libcurl-devel libnotify-devel libesmtp-devel nut-devel openldap-devel perl-devel perl-ExtUtils-Embed liboping-devel postgresql-devel python-devel hiredis-devel rrdtool-devel libatasmart-devel libudev-devel net-snmp-devel varnish-libs-devel libvirt-devel libxml2-devel libcurl-devel hiredis-devel 
```

Instalando pré-requisitos

```bash
sudo yum install make automake gcc gcc-c++ kernel-devel perl-develbzip2
```

Baixando a última versão do Collected

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

Inicie o serviço para verificar se está tudo bem!

```bash
sudo service collectd
```

Note que a saída será semelhante:

```bash
Starting collectd: not starting due to configuration error [FAILED]
```

Para iniciar o serviço corretamente precisamos habilitar alguns plugins dentro do arquivo /etc/collectd.conf. Abra o arquivo:

```bash
sudo vim /etc/collectd.conf 
```

E descomente e realize as seguintes alterações:

```bash
# Nome da máquina
Hostname	"ps-bw.sciencedmz.usp.br"

# Intervalo da captura
Interval	2

# Nível do log
LoadPlugin syslog

<Plugin syslog>
        LogLevel info
</Plugin>


# Habilitar os seguintes plugins para captura
LoadPlugin cpu
LoadPlugin disk
LoadPlugin entropy
LoadPlugin interface
LoadPlugin irq
LoadPlugin load
LoadPlugin memory
LoadPlugin numa
LoadPlugin processes
LoadPlugin swap
LoadPlugin network

# Procure pelo plugin network e realize as seguintes modificações:

<Plugin network>
#As insformações são enviadas para o ADM
        <Server "adm.sciencedmz.usp.br" "25826">
        </Server>
</Plugin>


```

Salve o arquivo. Reinicie o 


```bash
sudo service collectd start
```

A saída será:

```bash
Starting collectd:                                         [  OK  ]
```


Pronto! Tudo certo. Agora visualize em http://adm.sciencedmz.usp.br

https://support.ciscozeus.io/support/solutions/articles/9000035008-how-to-compile-and-install-collectd-5-5-on-centos-6
https://github.com/httpdss/collectd-web/blob/master/cgi-bin/collection.modified.cgi
https://raymii.org/s/tutorials/Collectd_server_setup_tutorial_with_web_frontend.html
https://collectd.org/wiki/index.php/Table_of_Plugins
http://www.tecmint.com/install-collectd-and-collectd-web-to-monitor-server-resources-in-linux/2/
https://raymii.org/s/tutorials/Collectd_client_setup_tutorial.html
https://raymii.org/s/tutorials/Collectd_server_setup_tutorial_with_web_frontend.html
https://collectd.org/wiki/index.php/Troubleshooting

