# mk-script
Script-Config-Padrão-MK
# ATUALIZAR PARA ULTIMA VERSÃO DO 6.49 (Primeiro Sistema depois o FIRMWARE)
# TROCAR O NOME DAS INTERFACES DE INTERNET PARA LINK-1 E LINK-2
# CRIAR A LISTA WAN-LIST
# ASSOCIAR OS LINK1 E LINKS2 NA WANLIST
# CRIAR A LISTA LAN-LIST
# ATIVAR A WAN-LIST E LAN-LIST


:do {
/ppp profile add name=LINK on-up=":local gw \$\"remote-address\"\r\
    \n:local nomeInterface [/interface get \$interface name]\r\
    \n:local gatewayRecursivo\r\
    \n:if (\$nomeInterface= \"LINK-1\") do={\r\
    \n\t:set gatewayRecursivo \"8.8.8.8\"\r\
    \n} else {\r\
    \n\t:if (\$nomeInterface= \"LINK-2\") do={\r\
    \n\t\t:set gatewayRecursivo \"208.67.222.222\"\r\
    \n\t}\r\
    \n}\r\
    \n:local comentarioRotaRecursiva (\"\$nomeInterface -GATEWAY\")\r\
    \n:local distanciaRotaRecursiva 1\r\
    \n:local scopeRotaRecursiva 10\r\
    \n\r\
    \n:if ([:len \$gw] > 0) do={\r\
    \n\t#TRATAR ROTA RECURSIVA\r\
    \n\t:local verificaRotaRecursiva [/ip route find where comment=\"\$comentari\
    oRotaRecursiva\"]\r\
    \n\r\
    \n\tif ([:len \$verificaRotaRecursiva ] = 0) do={\r\
    \n\t\t:log info message=\"ADICIONANDO ROTA RECURSIVA \$link: INTERFACE: \$no\
    meInterface  | GATEWAY: \$gw\"\r\
    \n\t\t/ip route add dst-address=\"\$gatewayRecursivo\" comment=\"\$comentari\
    oRotaRecursiva\" distance=\$distanciaRotaRecursiva gateway=\$gw scope=\$scop\
    eRotaRecursiva check-gateway=ping \r\
    \n\t} else {\r\
    \n\t\tif ([:len \$verificaRotaRecursiva ] = 1) do={\r\
    \n\t\t\t:local verificaRotaRecursivaGateway [/ip route get \$verificaRotaRec\
    ursiva gateway]\r\
    \n\t\t\t:if (\$gw != \$verificaRotaRecursivaGateway) do={\r\
    \n\t\t\t\t:log info message=\"ATUALIZANDO ROTA RECURSIVA \$link: INTERFACE: \
    \$nomeInterface  | GATEWAY: \$gw\"\r\
    \n\t\t\t\t/ip route set \$verificaRotaRecursiva dst-address=\"\$gatewayRecur\
    sivo\" distance=\$distanciaRotaRecursiva gateway=\$gw scope=\$scopeRotaRecur\
    siva check-gateway=ping \r\
    \n\t\t\t}\r\
    \n\t\t\t\r\
    \n\t\t} else {\r\
    \n\t\t\t:log error message=\"ERRO DE DUPLICACAO DE ROTA \$link\"\r\
    \n\t\t\t:log info message=\"APAGANDO DUPLICACAO DE ROTAS \$link\"\r\
    \n\t\t\t/ip route remove [find comment=\"\$comentarioRotaRecursiva\"]\r\
    \n\t\t\t:log info message=\"ADICIONANDO ROTA RECURSIVA \$link: INTERFACE: \$\
    nomeInterface  | GATEWAY: \$gw\"\r\
    \n\t\t\t/ip route add dst-address=\"\$gatewayRecursivo\" comment=\"\$comenta\
    rioRotaRecursiva\" distance=\$distanciaRotaRecursiva gateway=\$gw scope=\$sc\
    opeRotaRecursiva check-gateway=ping \r\
    \n\t\t}\r\
    \n\r\
    \n\t}\r\
    \n}"
    } on-error={
    	/ppp profile set [find name=LINK] on-up=":local gw \$\"remote-address\"\r\
    \n:local nomeInterface [/interface get \$interface name]\r\
    \n:local gatewayRecursivo\r\
    \n:if (\$nomeInterface= \"LINK-1\") do={\r\
    \n\t:set gatewayRecursivo \"8.8.8.8\"\r\
    \n} else {\r\
    \n\t:if (\$nomeInterface= \"LINK-2\") do={\r\
    \n\t\t:set gatewayRecursivo \"208.67.222.222\"\r\
    \n\t}\r\
    \n}\r\
    \n:local comentarioRotaRecursiva (\"\$nomeInterface -GATEWAY\")\r\
    \n:local distanciaRotaRecursiva 1\r\
    \n:local scopeRotaRecursiva 10\r\
    \n\r\
    \n:if ([:len \$gw] > 0) do={\r\
    \n\t#TRATAR ROTA RECURSIVA\r\
    \n\t:local verificaRotaRecursiva [/ip route find where comment=\"\$comentari\
    oRotaRecursiva\"]\r\
    \n\r\
    \n\tif ([:len \$verificaRotaRecursiva ] = 0) do={\r\
    \n\t\t:log info message=\"ADICIONANDO ROTA RECURSIVA \$link: INTERFACE: \$no\
    meInterface  | GATEWAY: \$gw\"\r\
    \n\t\t/ip route add dst-address=\"\$gatewayRecursivo\" comment=\"\$comentari\
    oRotaRecursiva\" distance=\$distanciaRotaRecursiva gateway=\$gw scope=\$scop\
    eRotaRecursiva check-gateway=ping \r\
    \n\t} else {\r\
    \n\t\tif ([:len \$verificaRotaRecursiva ] = 1) do={\r\
    \n\t\t\t:local verificaRotaRecursivaGateway [/ip route get \$verificaRotaRec\
    ursiva gateway]\r\
    \n\t\t\t:if (\$gw != \$verificaRotaRecursivaGateway) do={\r\
    \n\t\t\t\t:log info message=\"ATUALIZANDO ROTA RECURSIVA \$link: INTERFACE: \
    \$nomeInterface  | GATEWAY: \$gw\"\r\
    \n\t\t\t\t/ip route set \$verificaRotaRecursiva dst-address=\"\$gatewayRecur\
    sivo\" distance=\$distanciaRotaRecursiva gateway=\$gw scope=\$scopeRotaRecur\
    siva check-gateway=ping \r\
    \n\t\t\t}\r\
    \n\t\t\t\r\
    \n\t\t} else {\r\
    \n\t\t\t:log error message=\"ERRO DE DUPLICACAO DE ROTA \$link\"\r\
    \n\t\t\t:log info message=\"APAGANDO DUPLICACAO DE ROTAS \$link\"\r\
    \n\t\t\t/ip route remove [find comment=\"\$comentarioRotaRecursiva\"]\r\
    \n\t\t\t:log info message=\"ADICIONANDO ROTA RECURSIVA \$link: INTERFACE: \$\
    nomeInterface  | GATEWAY: \$gw\"\r\
    \n\t\t\t/ip route add dst-address=\"\$gatewayRecursivo\" comment=\"\$comenta\
    rioRotaRecursiva\" distance=\$distanciaRotaRecursiva gateway=\$gw scope=\$sc\
    opeRotaRecursiva check-gateway=ping \r\
    \n\t\t}\r\
    \n\r\
    \n\t}\r\
    \n}"
}

:do {/ip dhcp-client
add add-default-route=no disabled=no interface=LINK-1 script=":local link \"LINK\
    -1\"\r\
    \n:local nomeInterface \$interface\r\
    \n:local gw \$\"gateway-address\"\r\
    \n:local gatewayRecursivo \"8.8.8.8\"\r\
    \n:local comentarioRotaRecursiva (\"\$link-GATEWAY\")\r\
    \n:local distanciaRotaRecursiva 1\r\
    \n:local scopeRotaRecursiva 10\r\
    \n:delay 1\r\
    \n:if ([:len \$gw] > 0) do={\r\
    \n\t#TRATAR ROTA RECURSIVA\r\
    \n\t:local verificaRotaRecursiva [/ip route find where comment=\"\$comentari\
    oRotaRecursiva\"]\r\
    \n\r\
    \n\tif ([:len \$verificaRotaRecursiva ] = 0) do={\r\
    \n\t\t:log info message=\"ADICIONANDO ROTA RECURSIVA \$link: INTERFACE: \$no\
    meInterface  | GATEWAY: \$gw\"\r\
    \n\t\t/ip route add dst-address=\"\$gatewayRecursivo\" comment=\"\$comentari\
    oRotaRecursiva\" distance=\$distanciaRotaRecursiva gateway=\$gw scope=\$scop\
    eRotaRecursiva check-gateway=ping \r\
    \n\t} else {\r\
    \n\t\tif ([:len \$verificaRotaRecursiva ] = 1) do={\r\
    \n\t\t\t:local verificaRotaRecursivaGateway [/ip route get \$verificaRotaRec\
    ursiva gateway]\r\
    \n\t\t\t:if (\$gw != \$verificaRotaRecursivaGateway) do={\r\
    \n\t\t\t\t:log info message=\"ATUALIZANDO ROTA RECURSIVA \$link: INTERFACE: \
    \$nomeInterface  | GATEWAY: \$gw\"\r\
    \n\t\t\t\t/ip route set \$verificaRotaRecursiva dst-address=\"\$gatewayRecur\
    sivo\" distance=\$distanciaRotaRecursiva gateway=\$gw scope=\$scopeRotaRecur\
    siva check-gateway=ping \r\
    \n\t\t\t}\r\
    \n\t\t\t\r\
    \n\t\t} else {\r\
    \n\t\t\t:log error message=\"ERRO DE DUPLICACAO DE ROTA \$link\"\r\
    \n\t\t\t:log info message=\"APAGANDO DUPLICACAO DE ROTAS \$link\"\r\
    \n\t\t\t/ip route remove [find comment=\"\$comentarioRotaRecursiva\"]\r\
    \n\t\t\t:log info message=\"ADICIONANDO ROTA RECURSIVA \$link: INTERFACE: \$\
    nomeInterface  | GATEWAY: \$gw\"\r\
    \n\t\t\t/ip route add dst-address=\"\$gatewayRecursivo\" comment=\"\$comenta\
    rioRotaRecursiva\" distance=\$distanciaRotaRecursiva gateway=\$gw scope=\$sc\
    opeRotaRecursiva check-gateway=ping \r\
    \n\t\t}\r\
    \n\r\
    \n\t}\r\
    \n}" use-peer-dns=no use-peer-ntp=no} on-error={/ip dhcp-client
set [ find interface=LINK-1] add-default-route=no disabled=no interface=LINK-1 script=":local link \"LINK\
    -1\"\r\
    \n:local nomeInterface \$interface\r\
    \n:local gw \$\"gateway-address\"\r\
    \n:local gatewayRecursivo \"8.8.8.8\"\r\
    \n:local comentarioRotaRecursiva (\"\$link-GATEWAY\")\r\
    \n:local distanciaRotaRecursiva 1\r\
    \n:local scopeRotaRecursiva 10\r\
    \n:delay 1\r\
    \n:if ([:len \$gw] > 0) do={\r\
    \n\t#TRATAR ROTA RECURSIVA\r\
    \n\t:local verificaRotaRecursiva [/ip route find where comment=\"\$comentari\
    oRotaRecursiva\"]\r\
    \n\r\
    \n\tif ([:len \$verificaRotaRecursiva ] = 0) do={\r\
    \n\t\t:log info message=\"ADICIONANDO ROTA RECURSIVA \$link: INTERFACE: \$no\
    meInterface  | GATEWAY: \$gw\"\r\
    \n\t\t/ip route add dst-address=\"\$gatewayRecursivo\" comment=\"\$comentari\
    oRotaRecursiva\" distance=\$distanciaRotaRecursiva gateway=\$gw scope=\$scop\
    eRotaRecursiva check-gateway=ping \r\
    \n\t} else {\r\
    \n\t\tif ([:len \$verificaRotaRecursiva ] = 1) do={\r\
    \n\t\t\t:local verificaRotaRecursivaGateway [/ip route get \$verificaRotaRec\
    ursiva gateway]\r\
    \n\t\t\t:if (\$gw != \$verificaRotaRecursivaGateway) do={\r\
    \n\t\t\t\t:log info message=\"ATUALIZANDO ROTA RECURSIVA \$link: INTERFACE: \
    \$nomeInterface  | GATEWAY: \$gw\"\r\
    \n\t\t\t\t/ip route set \$verificaRotaRecursiva dst-address=\"\$gatewayRecur\
    sivo\" distance=\$distanciaRotaRecursiva gateway=\$gw scope=\$scopeRotaRecur\
    siva check-gateway=ping \r\
    \n\t\t\t}\r\
    \n\t\t\t\r\
    \n\t\t} else {\r\
    \n\t\t\t:log error message=\"ERRO DE DUPLICACAO DE ROTA \$link\"\r\
    \n\t\t\t:log info message=\"APAGANDO DUPLICACAO DE ROTAS \$link\"\r\
    \n\t\t\t/ip route remove [find comment=\"\$comentarioRotaRecursiva\"]\r\
    \n\t\t\t:log info message=\"ADICIONANDO ROTA RECURSIVA \$link: INTERFACE: \$\
    nomeInterface  | GATEWAY: \$gw\"\r\
    \n\t\t\t/ip route add dst-address=\"\$gatewayRecursivo\" comment=\"\$comenta\
    rioRotaRecursiva\" distance=\$distanciaRotaRecursiva gateway=\$gw scope=\$sc\
    opeRotaRecursiva check-gateway=ping \r\
    \n\t\t}\r\
    \n\r\
    \n\t}\r\
    \n}" use-peer-dns=no use-peer-ntp=no}



:do {/ip dhcp-client
add add-default-route=no disabled=no interface=LINK-2 script=":local link \"LINK\
    -2\"\r\
    \n:local nomeInterface \$interface\r\
    \n:local gw \$\"gateway-address\"\r\
    \n:local gatewayRecursivo \"208.67.222.222\"\r\
    \n:local comentarioRotaRecursiva (\"\$link-GATEWAY\")\r\
    \n:local distanciaRotaRecursiva 1\r\
    \n:local scopeRotaRecursiva 10\r\
    \n:delay 1\r\
    \n:if ([:len \$gw] > 0) do={\r\
    \n\t#TRATAR ROTA RECURSIVA\r\
    \n\t:local verificaRotaRecursiva [/ip route find where comment=\"\$comentari\
    oRotaRecursiva\"]\r\
    \n\r\
    \n\tif ([:len \$verificaRotaRecursiva ] = 0) do={\r\
    \n\t\t:log info message=\"ADICIONANDO ROTA RECURSIVA \$link: INTERFACE: \$no\
    meInterface  | GATEWAY: \$gw\"\r\
    \n\t\t/ip route add dst-address=\"\$gatewayRecursivo\" comment=\"\$comentari\
    oRotaRecursiva\" distance=\$distanciaRotaRecursiva gateway=\$gw scope=\$scop\
    eRotaRecursiva check-gateway=ping \r\
    \n\t} else {\r\
    \n\t\tif ([:len \$verificaRotaRecursiva ] = 1) do={\r\
    \n\t\t\t:local verificaRotaRecursivaGateway [/ip route get \$verificaRotaRec\
    ursiva gateway]\r\
    \n\t\t\t:if (\$gw != \$verificaRotaRecursivaGateway) do={\r\
    \n\t\t\t\t:log info message=\"ATUALIZANDO ROTA RECURSIVA \$link: INTERFACE: \
    \$nomeInterface  | GATEWAY: \$gw\"\r\
    \n\t\t\t\t/ip route set \$verificaRotaRecursiva dst-address=\"\$gatewayRecur\
    sivo\" distance=\$distanciaRotaRecursiva gateway=\$gw scope=\$scopeRotaRecur\
    siva check-gateway=ping \r\
    \n\t\t\t}\r\
    \n\t\t\t\r\
    \n\t\t} else {\r\
    \n\t\t\t:log error message=\"ERRO DE DUPLICACAO DE ROTA \$link\"\r\
    \n\t\t\t:log info message=\"APAGANDO DUPLICACAO DE ROTAS \$link\"\r\
    \n\t\t\t/ip route remove [find comment=\"\$comentarioRotaRecursiva\"]\r\
    \n\t\t\t:log info message=\"ADICIONANDO ROTA RECURSIVA \$link: INTERFACE: \$\
    nomeInterface  | GATEWAY: \$gw\"\r\
    \n\t\t\t/ip route add dst-address=\"\$gatewayRecursivo\" comment=\"\$comenta\
    rioRotaRecursiva\" distance=\$distanciaRotaRecursiva gateway=\$gw scope=\$sc\
    opeRotaRecursiva check-gateway=ping \r\
    \n\t\t}\r\
    \n\r\
    \n\t}\r\
    \n}" use-peer-dns=no use-peer-ntp=no} on-error={/ip dhcp-client
set [ find interface=LINK-2] add-default-route=no disabled=no interface=LINK-2 script=":local link \"LINK\
    -2\"\r\
    \n:local nomeInterface \$interface\r\
    \n:local gw \$\"gateway-address\"\r\
    \n:local gatewayRecursivo \"208.67.222.222\"\r\
    \n:local comentarioRotaRecursiva (\"\$link-GATEWAY\")\r\
    \n:local distanciaRotaRecursiva 1\r\
    \n:local scopeRotaRecursiva 10\r\
    \n:delay 1\r\
    \n:if ([:len \$gw] > 0) do={\r\
    \n\t#TRATAR ROTA RECURSIVA\r\
    \n\t:local verificaRotaRecursiva [/ip route find where comment=\"\$comentari\
    oRotaRecursiva\"]\r\
    \n\r\
    \n\tif ([:len \$verificaRotaRecursiva ] = 0) do={\r\
    \n\t\t:log info message=\"ADICIONANDO ROTA RECURSIVA \$link: INTERFACE: \$no\
    meInterface  | GATEWAY: \$gw\"\r\
    \n\t\t/ip route add dst-address=\"\$gatewayRecursivo\" comment=\"\$comentari\
    oRotaRecursiva\" distance=\$distanciaRotaRecursiva gateway=\$gw scope=\$scop\
    eRotaRecursiva check-gateway=ping \r\
    \n\t} else {\r\
    \n\t\tif ([:len \$verificaRotaRecursiva ] = 1) do={\r\
    \n\t\t\t:local verificaRotaRecursivaGateway [/ip route get \$verificaRotaRec\
    ursiva gateway]\r\
    \n\t\t\t:if (\$gw != \$verificaRotaRecursivaGateway) do={\r\
    \n\t\t\t\t:log info message=\"ATUALIZANDO ROTA RECURSIVA \$link: INTERFACE: \
    \$nomeInterface  | GATEWAY: \$gw\"\r\
    \n\t\t\t\t/ip route set \$verificaRotaRecursiva dst-address=\"\$gatewayRecur\
    sivo\" distance=\$distanciaRotaRecursiva gateway=\$gw scope=\$scopeRotaRecur\
    siva check-gateway=ping \r\
    \n\t\t\t}\r\
    \n\t\t\t\r\
    \n\t\t} else {\r\
    \n\t\t\t:log error message=\"ERRO DE DUPLICACAO DE ROTA \$link\"\r\
    \n\t\t\t:log info message=\"APAGANDO DUPLICACAO DE ROTAS \$link\"\r\
    \n\t\t\t/ip route remove [find comment=\"\$comentarioRotaRecursiva\"]\r\
    \n\t\t\t:log info message=\"ADICIONANDO ROTA RECURSIVA \$link: INTERFACE: \$\
    nomeInterface  | GATEWAY: \$gw\"\r\
    \n\t\t\t/ip route add dst-address=\"\$gatewayRecursivo\" comment=\"\$comenta\
    rioRotaRecursiva\" distance=\$distanciaRotaRecursiva gateway=\$gw scope=\$sc\
    opeRotaRecursiva check-gateway=ping \r\
    \n\t\t}\r\
    \n\r\
    \n\t}\r\
    \n}" use-peer-dns=no use-peer-ntp=no}


/ip firewall filter remove [find comment!="apagarPorScripts"]
/ip firewall mangle remove [find comment!="apagarPorScripts"]
/ip firewall raw remove [find comment!="apagarPorScripts"]
/ip firewall address-list remove [find comment!="apagarPorScripts"]
/ip firewall filter
add action=accept chain=input comment="LIBERA LISTA DE INTERFACE LAN-LIST" in-interface-list=LAN-LIST
add action=accept chain=input comment="LIBERA LISTA DE ENDERE\C7OS LISTA_BRANCA" src-address-list=LISTA_BRANCA
add action=accept chain=input comment="LIBERA CONEX\D5ES ESTABILIZADAS E RELACIONAS PARA O MIKROTIK" connection-state=established,related
add action=accept chain=forward comment="LIBERA CONEX\D5ES ESTABILIZADAS E RELACIONAS PASSANDO PELO MIKROTIK" connection-state=established,related
add action=add-src-to-address-list address-list=LISTA_BRANCA address-list-timeout=1h chain=input comment="ADICIONA IP DE ORIGEM NA LISTA BRANCA" dst-port=3578 protocol=\
    tcp
add action=add-src-to-address-list address-list=LISTA_NEGRA address-list-timeout=1w chain=input comment="ADICIONA IP DE ORIGEM NA LISTA NEGRA " dst-port=\
    20-25,3389,80,443,8080,8291 in-interface-list=WAN-LIST protocol=tcp
add action=add-src-to-address-list address-list=LISTA_NEGRA address-list-timeout=1w chain=input comment="ADICIONA IP DE ORIGEM NA LISTA NEGRA [PORTSCAN - TCP] " \
    in-interface-list=WAN-LIST protocol=tcp psd=21,3s,3,1
add action=add-src-to-address-list address-list=LISTA_NEGRA address-list-timeout=1w chain=input comment="ADICIONA IP DE ORIGEM NA LISTA NEGRA [PORTSCAN - UDP] " \
    in-interface-list=WAN-LIST protocol=udp psd=21,3s,3,1
add action=drop chain=input comment="BLOQUEIO GERAL" in-interface-list=WAN-LIST
add action=accept chain=output comment="TESTA GATEWAY LINK-1" dst-address=8.8.8.8 out-interface=LINK-1 protocol=icmp
add action=drop chain=output comment="TESTA GATEWAY LINK-1" dst-address=8.8.8.8 protocol=icmp
add action=accept chain=output comment="TESTA GATEWAY LINK-2" dst-address=208.67.222.222 out-interface=LINK-2 protocol=icmp
add action=drop chain=output comment="TESTA GATEWAY LINK-2" dst-address=208.67.222.222 protocol=icmp


/ip firewall raw
add action=drop chain=prerouting comment="BLOQUEIO DA LISTA NEGRA" src-address-list=LISTA_NEGRA

/ip firewall address-list


/tool netwatch
add down-script=":log error message=\"LINK-1 COM PROBLEMAS\"\r\
    \n:local link [/ip route find gateway=\$host]\r\
    \n:local distancia [/ip route get \$link distance]\r\
    \n\r\
    \n:if (\$distancia=201) do={\r\
    \n\t:log error message=\"TROCANDO DISTANCIA DO LINK-1 PARA 202 | PADR\C3O LI\
    NK-2\"\r\
    \n\t/ip route set [find distance=201] distance=203\r\
    \n\t/ip route set [find distance=202] distance=201\r\
    \n\t/ip route set [find distance=203] distance=202\r\
    \n\t/ip firewall connection remove [find protocol!=\"apagarPorTeste\"]\r\
    \n\t/ip cloud force-update \r\
    \n}\r\
    \n" host=8.8.8.8 interval=30s timeout=500ms up-script=\
    ":log info message=\"LINK-1 REESTABILIZADO\""
add down-script=":log error message=\"LINK-2 COM PROBLEMAS\"\r\
    \n:local link [/ip route find gateway=\$host]\r\
    \n:local distancia [/ip route get \$link distance]\r\
    \n\r\
    \n:if (\$distancia=201) do={\r\
    \n\t:log error message=\"TROCANDO DISTANCIA DO LINK-2 PARA 202 | PADR\C3O LI\
    NK-1\"\r\
    \n\t/ip route set [find distance=201] distance=203\r\
    \n\t/ip route set [find distance=202] distance=201\r\
    \n\t/ip route set [find distance=203] distance=202\r\
    \n\t/ip firewall connection remove [find protocol!=\"apagarPorTeste\"]\r\
    \n\t/ip cloud force-update\r\
    \n}\r\
    \n" host=208.67.222.222 interval=30s timeout=500ms up-script=\
    ":log info message=\"LINK-2 REESTABILIZADO\""






/tool netwatch 
remove [find host=8.8.8.8]
remove [find host=208.67.222.222]
add down-script=":log error message=\"LINK-1 COM PROBLEMAS\"\r\
    \n:local link [/ip route find gateway=\$host]\r\
    \n:local distancia [/ip route get \$link distance]\r\
    \n/system scheduler set [find name=\"LINK-1\"] disabled=no\r\
    \n:if (\$distancia=201) do={\r\
    \n\t:log error message=\"TROCANDO DISTANCIA DO LINK-1 PARA 202 | PADR\C3O LI\
    NK-2\"\r\
    \n\t/ip route set [find distance=201] distance=203\r\
    \n\t/ip route set [find distance=202] distance=201\r\
    \n\t/ip route set [find distance=203] distance=202\r\
    \n\t/ip firewall connection remove [find protocol!=\"apagarPorTeste\"]\r\
    \n\t/ip cloud force-update \r\
    \n}\r\
    \n" host=8.8.8.8 interval=30s timeout=1000ms up-script=":log info message=\"L\
    INK-1 REESTABILIZADO\"\r\
    \n/system scheduler set [find name=\"LINK-1\"] disabled=yes"
add down-script=":log error message=\"LINK-2 COM PROBLEMAS\"\r\
    \n:local link [/ip route find gateway=\$host]\r\
    \n:local distancia [/ip route get \$link distance]\r\
    \n/system scheduler set [find name=\"LINK-2\"] disabled=no\r\
    \n\r\
    \n:if (\$distancia=201) do={\r\
    \n\t:log error message=\"TROCANDO DISTANCIA DO LINK-2 PARA 202 | PADR\C3O LI\
    NK-1\"\r\
    \n\t/ip route set [find distance=201] distance=203\r\
    \n\t/ip route set [find distance=202] distance=201\r\
    \n\t/ip route set [find distance=203] distance=202\r\
    \n\t/ip firewall connection remove [find protocol!=\"apagarPorTeste\"]\r\
    \n\t/ip cloud force-update\r\
    \n}\r\
    \n" host=208.67.222.222 interval=30s timeout=1000ms up-script=":log info message=\"LINK-2 R\
    EESTABILIZADO\"\r\
    \n/system scheduler set [find name=\"LINK-2\"] disabled=yes\r\
    \n"

/system scheduler
add interval=1m name=LINK-1 on-event=\
    "/tool netwatch set [find host=\"8.8.8.8\" and status=down] disabled=yes\r\
    \n:delay 1\r\
    \n/tool netwatch set [find host=\"8.8.8.8\"] disabled=no\r\
    \n\r\
    \n" policy=ftp,reboot,read,write,policy,test,password,sniff,sensitive,romon start-time=startup
add disabled=yes interval=1m name=LINK-2 on-event=\
    "/tool netwatch set [find host=\"208.67.222.222\" and status=down] disabled=yes\r\
    \n:delay 1\r\
    \n/tool netwatch set [find host=\"208.67.222.222\"] disabled=no\r\
    \n\r\
    \n" policy=ftp,reboot,read,write,policy,test,password,sniff,sensitive,romon start-time=startup



/ip cloud set ddns-enabled=yes

/system script remove [find name=trocaLink]
/system script 
add dont-require-permissions=yes name=trocaLink owner=admin policy=ftp,reboot,read,write,policy,test,password,sniff,sensitive,romon source="/ip route set [find distance=20\
    1] distance=203\r\
    \n/ip route set [find distance=202] distance=201\r\
    \n/ip route set [find distance=203] distance=202\r\
    \n/ip firewall connection remove [find protocol!=\"apagarPorTeste\"]\r\
    \n/ip cloud force-update \r\
    \n"


:local gw $"remote-address"
:if ([:len $gw] > 0) do={
	:local LINK [/interface get $interface name]
	# Tratar rota default
	:local INTERFACE [/ip route find where comment="$LINK-GATEWAY"]
	if ([:len $INTERFACE] = 0) do={
		:log info message="Criando rota default: interface $LINK - gateway: $gw"
		/ip route add dst-address="8.8.8.8" comment="$LINK-GATEWAY" distance=10 gateway=$gw scope=10
	} else {
		:log info message="Atualizando rota default: interface $LINK - gateway: $gw" 
		/ip route set $INTERFACE  dst-address="8.8.8.8" comment="$LINK-GATEWAY" distance=10 gateway=$gw scope=10
	}
}

/ip route 
add distance=201 gateway=8.8.8.8 comment="apagarPorScripts"
add distance=202 gateway=208.67.222.222 comment="apagarPorScripts"
#/ip route remove [find comment!="apagarPorScripts"]
/ip dhcp-client release [find interface=LINK-1]
/ip dhcp-client release [find interface=LINK-2]
/ip route set [find comment="apagarPorScripts"] comment=""

