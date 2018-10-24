#### RECONECCION DE INTERFAZ A WIFI_ETECSA CON ROUTEROS


#### (1) POR PING (OBTENIENDO INTERFACES AUTOMATICAMENTE)
```
/system scheduler
add interval=20s name=RECONNECT1 on-event=":global a {\"\"}\r\
    \n:for i from=0 to=([/interface wireless print count-only]-2) do={\r\
    \nset (\$a->\$i) [/interface wireless get value-name=name number=\$i]\r\
    \n}\r\
    \n:foreach k in=\$a do={\r\
    \n:if ( [/ping 10.180.0.30 routing-table=\$k count=2 ] >0 ) do={\r\
    \n} else={\r\
    \n/ip dhcp-client release [find interface=\$k];\r\
    \n:log info message=\"0==||====> Release \$k\";\r\
    \n}\r\
    \n}" policy=ftp,reboot,read,write,policy,test,password,sniff,sensitive,romon start-time=startup
```
#### (2) POR RX-RATE (OBTENIENDO INTERFACES AUTOMATICAMENTE)
```
/system scheduler
add interval=10s name=RECONNECT2 on-event=":global a {\"\"}\r\
    \n:for i from=0 to=([/interface wireless print count-only]-2) do={\r\
    \nset (\$a->\$i) [/interface wireless get value-name=name number=\$i]\r\
    \n}\r\
    \n:foreach r in=\$a do={\r\
    \n/interface wireless monitor \$r once do={\r\
    \n:if (\$\"rx-rate\" = \"---\") do={\r\
    \n/ip dhcp-client release [find interface=\$r];\r\
    \n:log info message=\"0==||====> Release \$r\";\r\
    \n}\r\
    \n}\r\
    \n}" policy=ftp,reboot,read,write,policy,test,password,sniff,sensitive,romon start-time=startup
```
