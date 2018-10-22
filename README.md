#### METODOS PARA RECONECCION DE INTERFAZ A WIFI_ETECSA CON ROUTEROS

##### author: Tony Duran <yoet92@gmail.com>
##### git: https://github.com/yoet92/rosreconnect.git


#### (1)
```
:foreach k in={"WAN1"; "WAN2"; "WAN3"; "WAN4"; "WAN5"} do={
:if ( [/ping 10.180.0.30 routing-table=$k count=2 ] >0 ) do={
} else={
/ip dhcp-client release [find interface=$k];
:log info message="0==||====> Release $k";
}
}
```
#### (2)
```
:foreach r in={"WAN1"; "WAN2"; "WAN3"; "WAN4"; "WAN5"} do={
/interface wireless monitor $r once do={
:if ($"rx-rate" = "---") do={
/ip dhcp-client release [find interface=$r];
:log info message="0==||====> Release $r";
}
}
}
```
