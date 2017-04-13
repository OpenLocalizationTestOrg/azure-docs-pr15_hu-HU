<properties 
    pageTitle="Azure API-kezelés hívások nyomon követheti a API megtekintő használatáról" 
    description="Megtudhatja, hogy miként nyomon követheti az API felügyelő Azure API-kezelés hívások." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-use-the-api-inspector-to-trace-calls-in-azure-api-management"></a>Azure API-kezelés hívások nyomon követheti az API megtekintő használatáról

API-kezelés itt egy API felügyelő eszköz szeretné hibakeresése során, és az API-khoz hibaelhárítási segítséget. Az API felügyelő programozás útján is használható, és közvetlenül a developer Portal segítségével is használható. 

Nemcsak a nyomkövetés műveletek API felügyelő is [házirend kifejezés](https://msdn.microsoft.com/library/azure/dn910913.aspx) értékelést követi nyomon. Egy a bemutató témakörben [felhő terjed ki rész 177: további API-felügyeleti funkciókat](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) és -21:00 előretekerés.

Ez az útmutató egy segédlet API felügyelő vonatkoznak.

>[AZURE.NOTE] API felügyelő halad csak generált és elérhetővé tett tartalmazó előfizetés kulcsok a [rendszergazdai](api-management-howto-create-groups.md) fiókhoz tartozó kérelmek.

## <a name="trace-call"></a> Használata API felügyelő hívás nyomon követése

Felügyelő API-t, vegye fel egy **ocp-apim-nyomkövetés: igaz** kérése a művelet hívásba élőfej, töltse le és vizsgálja meg a követés használata a **ocp-apim-nyomkövetés-helye** válasz fejléc jelölt URL-CÍMÉT. Programozottan végezhetők, és közvetlenül a developer Portal segítségével is elvégezhető.

Ebből az oktatóanyagból megtudhatja, hogy hogyan a API felügyelő nyomkövetési műveletek a egyszerű számológép API, amely az [kezelése az első API](api-management-get-started.md) keresztüli lépések oktatóprogram van beállítva. Ha még nem adott oktatóprogram elvégezte csak az egyszerű számológép API importálása néhány percet vesz igénybe, vagy használhatja a kiválasztásának a visszhang API például egy másik API. Minden egyes API Management szolgáltatás példányának megtalálható előre beállított egy használható is kipróbálhat és az API-kezelésével kapcsolatos további tudnivalók a visszhang API-val. A visszhang API-t ad eredményül vissza bármely bevitel a rendszer elküldi azt. Ahhoz, hogy használhassa, bármilyen HTTP igei hívhatja, és a visszaadott érték egyszerűen lesz, az Ön által küldött. 



Első lépésként kattintson az Azure klasszikus portálra a API-kezelési szolgáltatáshoz **developer Portal segítségével** . Műveletek közvetlenül a developer Portal segítségével, amely magában foglalja a megtekintése, és a műveleteket egy API teszteléséhez kényelmesen hívható.

>Ha, még nem hozott létre API Management szolgáltatás példány, olvassa el a [API Management szolgáltatás példány létrehozása][] az [Azure API-kezelés – első lépések][] oktatóprogram során.

![API-kezelés developer Portal segítségével][api-management-developer-portal-menu]

**API** a felső parancsot, és kattintson a **Egyszerű számológép**.

![A visszhang API][api-management-api]

Kattintson a **próbálja ki** próbálkozzon **hozzáadása két egészérték részt veszi figyelembe** .

![További információk][api-management-open-console]

Hagyja meg az alapértelmezett paraméterértékeket, és válassza ki az **Előfizetés-kulcs** legördülő használni kívánt termék előfizetés kulcsot.

Alapértelmezés szerint a developer Portal segítségével a **Ocp-Apim-nyomkövetés** a fejléc már értéke **Igaz**. Ezt a fejlécet állítja be, attól függetlenül, hogy a nyomkövetési jön létre.

![Küldés][api-management-http-get]

Kattintson a **Küldés** meghívni a művelet gombra.

![Küldés][api-management-send-results]

A válasz a fejlécek az **ocp-apim-nyomkövetés-helyet** a következőhöz hasonló érték lesz.

    ocp-apim-trace-location : https://contosoltdxw7zagdfsprykd.blob.core.windows.net/apiinspectorcontainer/ZW3e23NsW4wQyS-SHjS0Og2-2?sv=2013-08-15&sr=b&sig=Mgx7cMHsLmVDv%2B%2BSzvg3JR8qGTHoOyIAV7xDsZbF7%2Bk%3D&se=2014-05-04T21%3A00%3A13Z&sp=r&verify_guid=a56a17d83de04fcb8b9766df38514742

A követés is töltődnek le a megadott helyen, és felül, ahogyan az a következő lépéssel.

## <a name="inspect-trace"> </a>a követés vizsgálata

Meg szeretné tekinteni az értékeket a naplóban, töltse le a nyomkövetési fájl **ocp-apim-nyomkövetés-hely** URL-CÍMÉT. Egy olyan szövegfájl JSON formátumban, és a következőhöz hasonló bejegyzéseket tartalmaz.

    {
        "traceId": "abcd8ea63d134c1fabe6371566c7cbea",
        "traceEntries": {
            "inbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0725926",
                    "data": {
                        "request": {
                            "method": "GET",
                            "url": "https://contoso5.azure-api.net/calc/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "Connection",
                                    "value": "Keep-Alive"
                                },
                                {
                                    "name": "Host",
                                    "value": "contoso5.azure-api.net"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "mapper",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0726213",
                    "data": {
                        "configuration": {
                            "api": {
                                "from": "/calc",
                                "to": {
                                    "scheme": "http",
                                    "host": "calcapi.cloudapp.net",
                                    "port": 80,
                                    "path": "/api",
                                    "queryString": "",
                                    "query": {},
                                    "isDefaultPort": true
                                }
                            },
                            "operation": {
                                "method": "GET",
                                "uriTemplate": "/add?a={a}&b={b}"
                            },
                            "user": {
                                "id": 1,
                                "groups": [
                                    "Administrators",
                                    "Developers"
                                ]
                            },
                            "product": {
                                "id": 1
                            }
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.2998610Z",
                    "elapsed": "00:00:00.0727522",
                    "data": {
                        "message": "Request is being forwarded to the backend service.",
                        "request": {
                            "method": "GET",
                            "url": "http://calcapi.cloudapp.net/api/add?a=51&b=49",
                            "headers": [
                                {
                                    "name": "Ocp-Apim-Subscription-Key",
                                    "value": "5d7c41af64a44a68a2ea46580d271a59"
                                },
                                {
                                    "name": "X-Forwarded-For",
                                    "value": "33.52.215.35"
                                }
                            ]
                        }
                    }
                }
            ],
            "outbound": [
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1960601",
                    "data": {
                        "response": {
                            "status": {
                                "code": 200,
                                "reason": "OK"
                            },
                            "headers": [
                                {
                                    "name": "Pragma",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Length",
                                    "value": "124"
                                },
                                {
                                    "name": "Cache-Control",
                                    "value": "no-cache"
                                },
                                {
                                    "name": "Content-Type",
                                    "value": "application/xml; charset=utf-8"
                                },
                                {
                                    "name": "Date",
                                    "value": "Tue, 23 Jun 2015 19:51:35 GMT"
                                },
                                {
                                    "name": "Expires",
                                    "value": "-1"
                                },
                                {
                                    "name": "Server",
                                    "value": "Microsoft-IIS/8.5"
                                },
                                {
                                    "name": "X-AspNet-Version",
                                    "value": "4.0.30319"
                                },
                                {
                                    "name": "X-Powered-By",
                                    "value": "ASP.NET"
                                }
                            ]
                        }
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1961112",
                    "data": {
                        "message": "Response headers have been sent to the caller. Starting to stream the response body."
                    }
                },
                {
                    "source": "handler",
                    "timestamp": "2015-06-23T19:51:35.4256650Z",
                    "elapsed": "00:00:00.1963155",
                    "data": {
                        "message": "Response body streaming to the caller is complete."
                    }
                }
            ]
        }
    }

## <a name="next-steps"> </a>Következő lépések

-   A házirend kifejezések a nyomkövetés a bemutatóból [felhő terjed ki rész 177: további API-felügyeleti funkciókat](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). Előretekerése-21:00 a bemutatása.

>[AZURE.VIDEO episode-177-more-api-management-features-with-vlad-vinogradsky]

[Use API Inspector to trace a call]: #trace-call
[Inspect the trace]: #inspect-trace
[Next steps]: #next-steps

[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md

[Azure API-kezelés – első lépések]: api-management-get-started.md
[Hozza létre az API Management szolgáltatás]: api-management-get-started.md#create-service-instance
[Azure Classic Portal]: https://manage.windowsazure.com/


[api-management-developer-portal-menu]: ./media/api-management-howto-api-inspector/api-management-developer-portal-menu.png
[api-management-api]: ./media/api-management-howto-api-inspector/api-management-api.png
[api-management-echo-api-get]: ./media/api-management-howto-api-inspector/api-management-echo-api-get.png
[api-management-developer-key]: ./media/api-management-howto-api-inspector/api-management-developer-key.png
[api-management-open-console]: ./media/api-management-howto-api-inspector/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-api-inspector/api-management-http-get.png
[api-management-send-results]: ./media/api-management-howto-api-inspector/api-management-send-results.png




 