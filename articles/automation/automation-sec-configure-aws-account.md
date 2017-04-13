<properties
   pageTitle="Konfigurálása az Amazon webszolgáltatásokhoz |} Microsoft Azure"
   description="Ez a cikk ismerteti, hogyan hozhat létre, és az Azure automatizálás AWS erőforrások kezelésére runbooks egy AWS hitelesítő adatainak ellenőrzése."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="hitelesítés AWS aws konfigurálása"/>
<tags
   ms.service="automation"
   ms.workload="tbd"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.date="09/12/2016"
   ms.author="magoedte"/>

# <a name="authenticate-runbooks-with-amazon-web-services"></a>Hitelesítő Runbooks Amazon webszolgáltatással
Az erőforrások Amazon Web Services (AWS) gyakori feladatok automatizálása elvégezhető az automatizálási runbooks Azure-ban.  Automatizálható AWS automatizálási runbooks ugyanúgy, mint az Azure-ban erőforrások is használ a sok feladatot.  Minden szükséges két dolgot:

* Egy AWS előfizetést és a hitelesítő adatait.  Konkrétan a AWS hívóbetű és a titkos kulcs.  További tudnivalókért olvassa el a cikk [Használatával AWS hitelesítő adatait](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html).
* Az Azure előfizetés és automatizálási fiókot.  Azure automatizálási fiók beállításával kapcsolatos további tudnivalókért olvassa el a cikk [Azure futtatása mint fiók konfigurálása](../automation/automation-sec-configure-azure-runas-account.md).  

AWS hitelesítése, meg kell adnia egy sor olyan AWS hitelesítő adataival az Azure automatizálási futó runbooks. Ha már létrehozott automatizálási fiókkal rendelkezik, és használni, amely AWS hitelesíteni kívánt, kövesse a következő szakaszban szereplő lépéseket.  Ha szeretne runbooks adatforráselemhez AWS erőforrások fiók kitűzött célja, először hozzon létre egy új [fiók automatizálási Futtatás mint](../automation/automation-sec-configure-azure-runas-account.md) (a vezérlőt, amellyel hozzon létre egy egyszerű kihagyja), és kövesse az alábbi lépéseket.

## <a name="configure-automation-account"></a>Automatizálási fiókjának beállítása
Az Azure automatizálási AWS kommunikálni először a AWS hitelesítő adatok beolvasásához, és az Azure automatizálás eszközként tárolja őket.  A következő lépésekkel dokumentált [Hívóbetűk kezelése AWS fiókjának](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) hívóbetű létrehozásához, és másolja a **Hozzáférési kulcs Azonosítóját** és a **Titkos hívóbetű** AWS dokumentumban (tetszés szerint töltse le a fő fájl biztonságos helyen tárolja).

Miután létrehozta, és másolja a AWS biztonsági kulcsok, szüksége lesz Azure automatizálást fiókkal biztonságosan tárolja őket, és a hivatkozás őket a runbooks a hitelesítő adatok eszköz létrehozása.  Kövesse a lépéseket az szakasz **Hozzon létre egy új hitelesítő adatok** című témakörben [található automatizálás Azure hitelesítőadat-eszközök](../automation/automation-certificates.md/###To create a new credential with the Azure portal) , és írja be az alábbi adatokat:

1. A **név** mezőbe írja be a **AWScred** vagy a elnevezési szabályai követő megfelelő értéket.  
2. A **felhasználónév** mezőbe írja be a **Hozzáférés-azonosító** és a **Titkos hívóbetű** a **jelszó** és a **jelszó megerősítése** mezőbe.   

## <a name="next-steps"></a>Következő lépések

- A cikk a megoldás [egy virtuális az Amazon Web Services automatizálása telepítésének](../automation/automation-scenario-aws-deployment.md) megtudhatja, hogy miként hozhat létre, amellyel műveletek automatizálhatók a AWS runbooks Reivew.
