<properties 
    pageTitle="Szolgáltatás Bus AMQP áttekintése |} Microsoft Azure" 
    description="Tudjon meg többet a speciális üzenet Queuing Protocol (AMQP) 1.0 Azure-ban." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="09/29/2016" 
    ms.author="sethm"/>



# <a name="amqp-10-support-in-service-bus"></a>A szolgáltatás Bus AMQP 1.0 támogatás

Az Azure Service Bus felhőbeli szolgáltatástól és a helyszíni [Szolgáltatás Bus Windows Server (Service Bus 1.1)](https://msdn.microsoft.com/library/dn282144.aspx) támogatja a speciális üzenet várólista Protocol (AMQP) 1.0. AMQP összeállítása platformok, a hibrid alkalmazások egy megnyitott szabványos protokoll használatával teszi lehetővé. A különféle nyelvek és keretek kialakításának, és a különféle operációs rendszeren futó összetevők használó alkalmazásai állíthat össze. Az összes szolgáltatás Bus, és zökkenőmentesen csatlakoztatása az összetevők üzenetváltásra strukturált üzleti hatékonyan és teljes romlását eredményezhetik.

## <a name="introduction-what-is-amqp-10-and-why-is-it-important"></a>Bevezetés: Mi az, hogy AMQP 1.0, és miért fontos?

Régebben üzenet készült termékekre használtak saját protokoll ügyfélalkalmazások és kereskedők közötti kommunikációt. Ez azt jelenti, hogy miután megadta, hogy egy adott szállító üzenetben ügynök, kell használnia, hogy szállító tárak az adott ügynök ügyfélalkalmazásai csatlakozni. Ennek eredményeképpen ezt a szállítót függőségét fokú óta a csatlakoztatott alkalmazások kód változtatást az alkalmazás más termékhez portolása igényel. 

Ezenkívül üzenetben kereskedők kapcsolódás más gyártóktól származó körlevelekben. Ehhez a szokásos alkalmazásszintű üzenetek áthelyezése a rendszer egy másik, és ha a saját üzenetformátumok közötti összekötő. Ez a közös követelménynek; például ha, kell egy új egységesített felületet biztosít a való régebbi elemezve rendszerek, vagy egyesülés követő informatikai rendszerek integráció.

A szoftver iparágban a gyorsan változó üzleti; új programnyelv és az alkalmazás keretek egy néha bewildering tempójában jelent meg. Hasonlóképpen az IT-rendszerek követelményeinek verzióinformációk, és a fejlesztők szeretné az új platform szolgáltatások előnyeit. Jó helyen jár néha a kijelölt üzenetben szállító nem támogatja a következő platformokon. Mivel az üzenetben protokollok védett, nincs lehetőség mások számára ezekre a platformokra a tárak szükséges. Ezért például az átjárók vagy hidakat, melyekkel továbbra is szeretné használni, termék üzenetben az alábbi módszerek kell használnia.

Ezeket a problémákat, a speciális üzenet Queuing Protocol (AMQP) 1.0 fejlesztését ösztönözte. A JP Péter Chase, akik, a pénzügyi szolgáltatások cégek, például köztes üzenet lépések nehéz felhasználói származik. A cél egyszerű volt: egy szabványos Megnyitás üzenetben protokoll árán elért számára készült különböző nyelvű, keretek és operációs rendszerek, összetevőket üzeneten alapuló alkalmazások létrehozásához nyomja le a legjobb a fajta a összetevőinek a szállítók cellatartományban.

## <a name="amqp-10-technical-features"></a>AMQP 1.0 technikai funkciók

AMQP 1.0 nem használható össze robusztus, platformok, üzenetküldés alkalmazások hatékony, megbízható, vezetékes szintű üzenetben protokoll. A protokoll van egy egyszerű cél: a idejéről biztonságos, a megbízható és a hatékony üzenetek között két fél meghatározásához. Az üzenetek maguk van kódolva, használja a hordozható adatok ábrázolása, amely lehetővé teszi, hogy eltérő küldő és a teljes funkcióvesztést strukturált üzleti-üzeneteket váltani. Az alábbiakban a legfontosabb funkciók összefoglalása:

*    **Oldalablakot**: AMQP 1.0 kapcsolat alapú protokollt, hogy a bináris kódolásának protokoll utasításokat, és az üzleti üzeneteket átvitt használ. A hálózat és a csatlakoztatott összetevők hasznosítása maximalizálása összetett folyamatvezérlés rendszerek magában foglal. Említett, a protokoll készült optimális megoldást találni hatékonyság, rugalmasság és együttműködési között.
*    **Megbízható**: A AMQP 1.0 protokoll lehetővé teszi, hogy az üzenetek küldését megbízhatóság garanciákkal fire – és – elfelejti megbízható, hogy a tartományba pontosan-egyszer nyugtázza kézbesítési.
*    **Rugalmas**: AMQP 1.0 egy rugalmas protokollt támogató különböző topológiák használható. Ugyanaz a protokoll ügyfél-ügyfél, ügyfél-ügynök és ügynök-ügynök kommunikáció használható.
*    **Ügynök-modell független**: A AMQP 1.0 specifikációja nem teszi követelményeket ügynök által alkalmazott üzenetben modell. Ez azt jelenti, hogy könnyen AMQP 1.0 támogatási hozzáadása meglévő üzenetben kereskedők lehetséges.

## <a name="amqp-10-is-a-standard-with-a-capital-s"></a>AMQP 1.0 szabványos (az egy nagy ")

AMQP 1.0 egy nemzetközi szabvány ISO és az ISO/IEC 19464:2014 mint IEC által jóváhagyott.

Több, mint 20-cégek, core csoportja által 2008 óta fejlesztési AMQP 1.0 már technológia a szállítók és végfelhasználói cégek is. Adott idő alatt felhasználói cégek járult hozzá a való életben üzleti követelmények és technológia szállítók lett hatékonyabb e követelményeknek Protocol (protokoll). A folyamat során szállítók részt vesznek munkaértekezletek közvetlenül a megvalósítás együttműködését érvényesítéséhez.

Az október 2011 a előrelépés az strukturált adatokat szabványok (OASIS) és a OASIS AMQP 1.0 szabványos a szervezeten belül technikai bizottság-re áttelepített fejlesztési munka október 2012 jelent meg. Az alábbi vállalkozások részt a Technikai Bizottság a standard fejlesztésekor:

*    **Technológia szállítók**: Axway szoftver, Huawei technológiák, IIT szoftver, INETCO rendszerek, Kaazing, Microsoft, Mitre Corporation, Primeton technológiák, előrehaladását szoftver, vörös kalap, SITA, szoftver AG, Solace rendszerek, VMware, WSO2, Zenika.
*    **Felhasználói cégek**: amerikai Bank hitelképesség Suisse, német Boerse, Goldman Sachs, JPMorgan Chase.

A megnyitott szabványokat gyakran jegyzett előnyei közé tartoznak:

*    Kisebb valószínűséggel szállító zárolása a
*    Együttműködés
*    Tárak és szerszámok széles elérhetősége
*    Elavulása elleni védelem
*    Hozzáértő oktatói elérhetősége
*    Alsó és kezelhető kockázat

## <a name="amqp-10-and-service-bus"></a>AMQP 1,0 és szolgáltatási Bus

Az Azure Service Bus AMQP 1.0 támogatási azt jelenti, hogy, amelyet most kihasználhatja a szolgáltatás Bus queuing és közzététel/előfizetés brokered üzenetküldési szolgáltatások egy cellatartományból platformokon egy hatékony bináris protokoll használatával. Ezenkívül készíthet alkalmazások verzióban lévő összetevők beépített kombinálja a keretek, operációs rendszeren és nyelvek használata.

Az alábbi ábra egy példa környezetben, amelyben Linux rendszerhez, a normál Java üzenet szolgáltatás (JMS) API-val és a Windows rendszeren futó .NET-ügyfelek írt rendszeren futó Java-ügyfelek üzenetváltásra AMQP 1.0 használ szolgáltatási Bus keresztül.

![][0]

**Ábra 1: Például telepítési forgatókönyv megjelenítő platformok közötti szolgáltatás Bus és AMQP 1.0 használata**

Ekkor a következő ügyfél-tárak tudjuk, hogy a szolgáltatás Bus használata:

| Nyelvi | Dokumentumtár                                                                       |
|----------|-------------------------------------------------------------------------------|
| Java     | Ügyfél Apache Qpid Java üzenet szolgáltatás (JMS)<br/>IIT szoftver SwiftMQ Java-ügyfél |
| C        | Apache Qpid Proton-C                                                          |
| PHP      | Apache Qpid Proton-PHP                                                        |
| Python   | Apache Qpid Proton-Python                                                     |
| C#       | AMQP .net Lite                                                                |

**Szám 2: Ügyfél-tárak AMQP 1.0 táblázata**

## <a name="summary"></a>Összefoglalás

*    AMQP 1.0 az egy megnyitott, a megbízható üzenetben protokoll platformok, a hibrid alkalmazások készítéséhez használható. AMQP 1.0 egy OASIS szabvány.
*    AMQP 1.0 támogatás érhető el Azure Service Bus, valamint a szolgáltatás Bus Windows Server (Service Bus 1.1). Árak ugyanúgy történik, mint a már meglévő protokollok.

## <a name="next-steps"></a>Következő lépések

Készen áll a további információt? Látogasson el az alábbi hivatkozások:

- [A .NET szolgáltatás Bus használata AMQP]
- [Java a szolgáltatás Bus használata AMQP]
- [A Python szolgáltatás Bus használata AMQP]
- [A PHP szolgáltatás Bus használata AMQP]
- [Az Azure Linux virtuális Apache Qpid Proton-C telepítése]
- [A Windows Server szolgáltatás Bus AMQP]

[0]: ./media/service-bus-amqp-overview/service-bus-amqp-1.png
[A .NET szolgáltatás Bus használata AMQP]: service-bus-amqp-dotnet.md
[Java a szolgáltatás Bus használata AMQP]: service-bus-amqp-java.md
[A Python szolgáltatás Bus használata AMQP]: service-bus-amqp-python.md
[A PHP szolgáltatás Bus használata AMQP]: service-bus-amqp-php.md
[Egy Linux Azure virtuális Apache Qpid Proton-C telepítése]: service-bus-amqp-apache.md
[A Windows Server szolgáltatás Bus AMQP]: https://msdn.microsoft.com/library/dn574799.aspx