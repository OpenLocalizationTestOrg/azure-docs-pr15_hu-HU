<properties
   pageTitle="Toll tesztelése |} Microsoft Azure"
   description="A cikk áttekintést nyújt a behatolási (pentest) tesztelésének, és hogyan végre pentest Azure infrastruktúra futó alkalmazás szemben."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="pen-testing"></a>Toll tesztelése

A kiváló dolgot használatáról a Microsoft Azure alkalmazásban tesztelje és telepítéséhez az, hogy nem kell összeállított egy helyszíni infrastruktúra fejlesztése, tesztelése és az alkalmazások terjesztése. Minden infrastruktúra van a Microsoft Azure platform szolgáltatások által készített kezeli. Nem kell aggódnia amiatt, hogy requisitioning, beszerzésekor, és "lefejtési és halmozási" saját helyszíni hardver.

Ez a remek –, de van szüksége, hogy a normál biztonsági elkészíti haladékot. Az dolgot kell tennie egyik behatolási tesztelje az alkalmazások telepítése Azure-ban.

Előfordulhat, hogy már tudja, hogy a Microsoft elvégzi a [behatolási vizsgálata az Azure környezetben](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e). Ez segít a platform fejlesztését, és a műveletek javítása, biztonsági funkciók, új biztonsági funkciók bemutatása és a biztonsági folyamatok javítására útmutatást.

Azt nem toll tesztelheti meg az alkalmazást, de azt szeretné, és végezze el a saját alkalmazások tesztelése toll kell fog megértéséhez. Ennek oka egy jó dolog, amikor az alkalmazás a biztonsági növelheti, hogy segítséget teljes Azure ökológiai biztonságosabbá.

Amikor, toll tesztelje az alkalmazásokat, akkor a következőhöz hasonló lehet támadások nekünk. Azt [folyamatosan figyelni](http://blogs.msdn.com/b/azuresecurity/archive/2015/07/05/best-practices-to-protect-your-azure-deployment-against-cloud-drive-by-attacks.aspx) szemben mintázatok és kezdeményez egy esemény válasz folyamat, ha szükség. Azt nem segít, és azt nem segítse a szolgáltatás, ha azt elindítani az esemény választ saját esedékes szorgalom toll tesztelése miatt.

Mi a teendő?

Ha készen áll a toll tesztelje az Azure által üzemeltetett alkalmazásokat, ossza meg velünk kell. Tudjuk, hogy fogjuk adott vizsgálatot hajt végre, azt nem véletlenül (például az IP-címet, használja a tesztelés blokkolása), leállíthatja, mindaddig, amíg a vizsgálatok felel meg az Azure toll tesztelés – feltételek és kikötések.
Szabványos vizsgálatok hajthatja végre a következők:

- A [megnyitott webhely alkalmazás biztonsági Project (OWASP) felső 10 biztonsági](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project) Kihúzás végpontok vizsgálata
- A végpontok [fuzz tesztelése](https://blogs.microsoft.com/cybertrust/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/)
- A [port végzett vizsgálatot](https://en.wikipedia.org/wiki/Port_scanner) a végpontok

Egy adott típusú tesztelje, hogy nem tudja elvégezni az [Szolgáltatás megtagadását (DoS)](https://en.wikipedia.org/wiki/Denial-of-service_attack) támadások bármilyen típusú. Ide tartoznak a kezdeményezése egy DoS szándékú magát, és előfordulhat, hogy határozza meg, bemutatják vagy bármilyen típusú DoS támadások szimulálhatja kapcsolódó tesztek végrehajtása.

Készen áll arra, toll – első lépések teszteli a Microsoft Azure-ban tárolt alkalmazások? Ha igen, majd a központi a fölé a [Behatolási teszt – áttekintés](https://security-forms.azure.com/penetration-testing/terms) oldalon (és az létrehozása kérése tesztelése gombra a lap alján kattintson. További tájékoztatást a feltételek és hogyan jelenthetik Azure vagy más Microsoft-szolgáltatás kapcsolatos biztonsági hibákat hasznos hivatkozások tesztelése toll is megtalálja.
