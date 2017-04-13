<properties
   pageTitle="Alkalmazás átjáró WebSocket támogatása |} Microsoft Azure"
   description="Ezen az oldalon az alkalmazás átjáró WebSocket támogatás áttekintése."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/16/2016"
   ms.author="amsriva"/>

# <a name="application-gateway-websocket-support"></a>Alkalmazás átjáró WebSocket támogatása

Alkalmazás átjáró támogatja natív WebSocket az összes átjárót méretű keresztül. Szelektív engedélyezése vagy letiltása a WebSocket támogatási nincs felhasználó konfigurálható beállítás nem. Egy szabványos HTTPListener is a port 80 és 443-as WebSocket forgalmat fogadni. WebSocket forgalom van a megfelelő kódmentes készlet használó átjárók szabályokban meghatározott WebSocket engedélyezett kódmentes kiszolgálóhoz irányítja. A [RFC6455](https://tools.ietf.org/html/rfc6455) szabványos WebSocket protokoll lehetővé teszi, hogy egy teljes kétirányú kommunikációt között a kiszolgáló és az ügyfél időigényes TCP-kapcsolaton keresztül. Ez a szolgáltatás lehetővé teszi, hogy a több interaktív kommunikációhoz webkiszolgáló és az ügyfél, amely nincs szükség szerint szükséges a HTTP-alapú megvalósítás lekérdezési kétirányú lehet között.  WebSocket van alacsony felsőbb HTTP eltérően, és így újból felhasználhatja több kérés/válaszok egy erőforrás-hatékonyabb hasznosítása így ugyanazt a TCP kapcsolatot. WebSocket protokollok készült 80 és 443-as portjához hagyományos HTTP használatos.

A kódmentes kiszolgáló [állapot vizsgálati áttekintés](application-gateway-probe-overview.md) szakaszban ismertetett alkalmazás átjáró szondákat kell reagálni. Alkalmazás átjáró állapota szondákat HTTP-/ HTTPS csak, ez azt jelenti, hogy minden kódmentes kiszolgáló válaszolnia kell HTTP szondákat alkalmazás átjáró WebSocket forgalmat a kiszolgálóra.

## <a name="listener-configuration-element"></a>Figyelő konfigurációs elem

Meglévő HTTPListener WebSocket támogatásához használható. Az alábbiakban a minta sablonfájlból HttpListeners elem kódtöredékének. HTTP és a HTTPS hallgatók WebSocket támogatása, és a biztonságos WebSocket forgalom lenne szüksége. Hasonlóképpen használhatja a [portálon](application-gateway-create-gateway-portal.md) vagy a [PowerShell](application-gateway-create-gateway-arm.md) -alkalmazás átjáró létrehozása a hallgatók a port 80 és 443-as WebSocket forgalom támogatására.


    "httpListeners": [
                {
                    "name": "appGatewayHttpsListener",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/DefaultFrontendPublicIP"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort443'"
                        },
                        "Protocol": "Https",
                        "SslCertificate": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/sslCertificates/appGatewaySslCert1'"
                        },
                    }
                },
                {
                    "name": "appGatewayHttpListener",
                    "properties": {
                        "FrontendIPConfiguration": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendIPConfigurations/appGatewayFrontendIP'"
                        },
                        "FrontendPort": {
                            "Id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/frontendPorts/appGatewayFrontendPort80'"
                        },
                        "Protocol": "Http",
                    }
                }
            ],

## <a name="backendaddresspool-backendhttpsetting-and-routing-rule-configuration"></a>BackendAddressPool BackendHttpSetting és vonalvezetés szabály beállítása

BackendAddressPool használandó meghatározásához engedélyezett WebSocket kiszolgálókkal kódmentes erőforráskészlethez tartozik. BackendHttpSetting kell definiálhatók kódmentes 80 és 443-as csak port. Cookie-alapú kapcsolatok és a requestTimeouts tulajdonságainak sem fontos WebSocket forgalmat. Nem kötelező a szabály módosítása nem. A szabály egyszerű"továbbra is használhatók a megfelelő figyelő a megfelelő kódmentes cím kvótáját kötik. 

    "requestRoutingRules": [{
        "name": "<ruleName1>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpsListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }
        }

    }, {
        "name": "<ruleName2>",
        "properties": {
            "RuleType": "Basic",
            "httpListener": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/httpListeners/appGatewayHttpListener')]"
            },
            "backendAddressPool": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/ContosoServerPool')]"
            },
            "backendHttpSettings": {
                "id": "/subscriptions/<subid>/resourceGroups/<rgName>/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings')]"
            }

        }
    }]

## <a name="websocket-enabled-backend"></a>Engedélyezett WebSocket kódmentes

A kódmentes rendelkeznie kell a beállított futó HTTP-/ HTTPS webkiszolgálóra (általában 80 és 443-as) port: WebSocket a munkát. Ez a követelmény oka az, hogy WebSocket protokoll van szükség a kezdeti kézfogás verziófrissítéssel HTTP kell WebSocket jegyzőkönyv élőfej mezőként.

    GET /chat HTTP/1.1
    Host: server.example.com
    Upgrade: websocket
    Connection: Upgrade
    Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
    Origin: http://example.com
    Sec-WebSocket-Protocol: chat, superchat
    Sec-WebSocket-Version: 13

Egy másik ennek oka, hogy alkalmazás átjáró kódmentes állapot vizsgálati csak a HTTP-/ HTTPS protokollt támogató. Ha a kódmentes kiszolgáló nem válaszol a HTTP-/ HTTPS szondákat, kódmentes készlet és nincs kérelmeket, beleértve WebSocket kérések ki szeretné tenni, szeretné elérni a kódmentes.

## <a name="next-steps"></a>Következő lépések

Után tanulási WebSocket támogatásával kapcsolatos, nyissa meg [az alkalmazás átjáró létrehozása](application-gateway-create-gateway.md) engedélyezett WebSocket webalkalmazás – első lépések.
