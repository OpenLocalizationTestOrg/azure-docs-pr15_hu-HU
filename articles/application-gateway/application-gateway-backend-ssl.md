<properties
   pageTitle="SSL-házirend és az alkalmazás átjáró végpont SSL engedélyezése |} Microsoft Azure"
   description="Ezen az oldalon áttekintést nyújt az alkalmazás átjáró végpont SSL támogatja."
   documentationCenter="na"
   services="application-gateway"
   authors="amsriva"
   manager="rossort"
   editor="amsriva"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="amsriva"/>

# <a name="enabling-ssl-policy-and-end-to-end-ssl-on-application-gateway"></a>SSL-házirend és az alkalmazás átjáró végpont SSL engedélyezése

## <a name="overview"></a>– Áttekintés

Alkalmazás átjáró támogatja az SSL lemondási a az átjáró, után mely jellemzően forgalmat titkosítatlan kódmentes kiszolgálókhoz. Így kell a költséges titkosítás/visszafejtés terhelést unburdened webkiszolgálón. Azonban az egyes felhasználók a kódmentes kiszolgálókhoz titkosított kapcsolati lehetőség nem fogadhatók el. Ennek oka lehet biztonsági/megfelelőségi követelmények betartását, vagy az alkalmazás csak fogadja el a biztonságos kapcsolat. Az ilyen alkalmazásokhoz alkalmazás átjáró támogatja végpont SSL-titkosítás.

Záró végére SSL lehetővé teszi, hogy biztonságosan továbbítja az kódmentes titkosított továbbra is kihasználni a réteg 7 terheléselosztási szolgáltatások előnyei bizalmas adatokat melyik alkalmazás átjáró támogatja a, cookie-k affinitás, például az URL-alapú útválasztási, a webhelyek vagy az azt jelenti, hogy X behelyezése útválasztás alapján-továbbított-* fejléc.

Végpont SSL kommunikációs módot konfigurálásakor alkalmazás átjáró megszakítja felhasználó SSL-munkamenetek a az átjáró, és használatával visszafejti a felhasználó-forgalmat. Akkor jelölje be a forgalom átirányítása egy megfelelő kódmentes készlet példányt konfigurált szabályok lesz érvényes. Alkalmazás átjáró új SSL-kapcsolatot kódmentes kiszolgálóra kezdeményez, majd újra az adatokat a kódmentes kiszolgáló nyilvános kulcs a tanúsítvány használatával továbbítása a kódmentes kérelem előtt titkosítja. A Befejezés paranccsal befejezheti az SSL engedélyezte a Https, majd érvényesül kódmentes készletbe BackendHTTPSetting Protocol (protokoll) beállítás. Végpont SSL engedélyezett kódmentes készletben minden kódmentes kiszolgáló biztonságos kommunikáció engedélyezése a tanúsítvánnyal kell beállítania.

![végpont ssl eset][1]

Ebben a példában TLS1.2 használó kérések résztvevőkhöz kódmentes kiszolgálókon Pool1 végpont SSL-kapcsolaton keresztül.

## <a name="end-to-end-ssl-and-whitelisting-of-certificates"></a>A Befejezés paranccsal befejezheti az SSL-tanúsítványok whitelisting és

Alkalmazás átjáró csak kommunikál ismert kódmentes példányai, amelyek whitelisted az alkalmazás átjáró a tanúsítvány. Ahhoz, hogy milyen oklevelek whitelisting, a nyilvános kulcs kódmentes oklevelek kell feltölteni az alkalmazás átjáró (nem a legfelső szintű tanúsítvány). Ismert és whitelisted háttérkiszolgálókon csak kapcsolatok majd engedélyezett. A hátralévő háttérkiszolgálókon egy átjáró hibát eredményez. Önaláírt tanúsítványok vannak csak és nem ajánlott gyártási munkaterhelésekből tesztelése céljából. Ilyen tanúsítványokat kell whitelisted alkalmazás átjáró előtt is használhatók az előző lépéseket leírt módon is.

## <a name="application-gateway-ssl-policy"></a>Alkalmazás átjáró SSL-házirend

Alkalmazás átjáró felhasználói konfigurálható SSL egyeztetés házirendek, amelyek lehetővé teszik a vevők további beállítási lehetőségekre SSL-kapcsolatot a az alkalmazás átjáró támogatja.

1. Az SSL 2.0-s és az összes alkalmazás átjárók alapértelmezés szerint engedélyezve 3.0. Még nem konfigurálható egyáltalán.
2. Az SSL házirend definition ad meg lehetőséget, ha azt szeretné, az alábbi 3 protokollokat - TLSv1 letiltása\_0, TLSv1\_1, TLSv1\_2.
3. Ha meg van adva a nincs SSL házirend mindhárom (TLSv1\_0, TLSv1\_1, TLSv1_2) engedélyezett.

## <a name="next-steps"></a>Következő lépések

Után az SSL befejezése és az SSL-házirendet, nyissa meg a [végpont SSL alkalmazás átjáró a](application-gateway-end-to-end-ssl-powershell.md) befejezési megismerése-alkalmazás átjáró létrehozása az azt jelenti, hogy a forgalmat háttérkiszolgálókon küldése titkosított formában.

<!--Image references-->

[1]: ./media/application-gateway-backend-ssl/scenario.png