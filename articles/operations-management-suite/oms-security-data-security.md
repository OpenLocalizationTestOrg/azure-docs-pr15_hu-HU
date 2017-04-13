<properties
   pageTitle="Műveletek Management csomagja biztonsági és naplózási megoldás adatok biztonsági |} Microsoft Azure"
   description="Ez a dokumentum adatainak felügyelt és a műveletek Management csomagja biztonsági és naplózási megoldás őrizni ismerteti."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="yurid"/>

# <a name="operations-management-suite-security-and-audit-solution-data-security"></a>Tevékenységek kezelése csomagja biztonság és a naplózási megoldás adatvédelem

Munkában megelőzésére, feltárására és veszélyek megválaszolása [Műveletek Management (MOBILE) csomagja biztonsági és naplózási megoldás](operations-management-suite-overview.md) gyűjti össze, és az erőforrások, adatait, beleértve a folyamatok:

- Biztonsági eseménynapló
- Esemény-nyomkövetés a Windows (esemény-nyomkövetés) események
- Az AppLocker naplózási események
- A Windows tűzfal napló
- Speciális veszélyt Analytics-események
- Eredeti felmérés eredményeinek
- Antimalware felmérés eredményeinek
- A javítás/felmérés eredményeinek
- A Agent explicit módon beállított Syslogs adatfolyamok

Erős kötelezettségek adatvédelmi és biztonsági az adatok védelme VÁLLALUNK. Microsoft nakhttp://go.microsoft.com/fwlink/?LinkId=311864 megfelelően igazodik szigorú megfelelőség és a biztonsági útmutató – a szolgáltatás működik-ból.
Ez a cikk ismerteti, adatainak felügyelt és a MOBILE és a naplózási megoldás védelmét.

## <a name="data-sources"></a>Adatforrások

MOBILE és a naplózási megoldás elemezheti az adatokat a virtuális gépeken futó és fizikai számítógépeket, amelyen a MOBILE ügynök telepítve van. MOBILE és a megoldás naplózási biztonsági eseményeket, például a Windows-eseménynaplózás, a naplókat, az IIS naplókról és a syslog üzenetek konfigurációs adatokat gyűjt. Ezek az adatok példák: operációs rendszer típusa és verziója, futó folyamatok, számítógépnév, IP-címek bejelentkezett felhasználó és a bérlői azonosítójával.  

## <a name="data-protection"></a>Adatok védelme

**Adatok feladatkörök**: a szolgáltatás teljes egyes összetevőre adatok logikailag külön legyen. Az összes adat egy szervezet címkézett. A címkézés továbbra is fennáll, az adatok életciklus során, és ez a szolgáltatás egyes rétegben vannak érvényben. 

**Adathozzáférés**: Adja meg a biztonsági javaslatok, és vizsgálja meg a potenciális biztonsági kockázatok, a Microsofton hozzáférés összegyűjtött vagy szolgáltatásai elemezheti. A [Microsoft Online Services kifejezéseket](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) és [Adatvédelmi nyilatkozata](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), mely állapot, hogy a Microsoft nem használja az ügyfél adatait, vagy adatait, hirdetési vagy hasonló kereskedelmi célú származhat mérvadónak. Adja meg a biztonsági javaslatok, és vizsgálja meg a potenciális biztonsági kockázatok, a Microsofton összegyűjtött vagy szolgáltatásai elemezni is hozzáférhet. Csak használjuk vevő adatait szükség szerint biztosítja az Azure szolgáltatásaival, beleértve azokat a szolgáltatásokat nyújtó kompatibilis céljából. Megőrzi az összes jogosultsággal, és a saját adatain.

**Adatok használata**: a Microsoft használja minták és több bérlők között látható veszélyt üzletiintelligencia tartalmasabbá teszi a megelőzése és a kimutatás funkciók; azt a műveletet az adatvédelmi nyilatkozatát el az [Adatvédelmi nyilatkozat](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx)leírtak szerint.

> [AZURE.NOTE] Munkaterület létrehozásakor a kezdeti MOBILE és a naplózás beállítási folyamat részét képező adatok helye a MOBILE munkaterület szinten van beállítva.

## <a name="see-also"></a>Lásd még:

A jelen dokumentum szüksége a tanultakhoz adatainak felügyelt és a MOBILE védelmét. További információ a MOBILE és a naplózási megoldás című témakörben talál:

- [Tevékenységek kezelése csomagja (MOBILE) – áttekintés](operations-management-suite-overview.md)
- [Figyelés és a biztonsági figyelmeztetések válaszol a műveletek Management csomagja biztonsági és naplózási megoldás](oms-security-responding-alerts.md)
- [Tevékenységek kezelése csomagja biztonsági és a naplózási megoldás erőforrások ellenőrzése](oms-security-monitoring-resources.md)

