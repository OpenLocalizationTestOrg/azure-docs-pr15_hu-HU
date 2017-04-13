<properties
    pageTitle="Mi a teendő az Azure szolgáltatási zavarok Azure virtuális hálózatok érintő megelőzve |} Microsoft Azure"
    description="Ismerje meg, mi a teendő az Azure szolgáltatási zavarok Azure virtuális hálózatok érintő megelőzve."
    services="virtual-network"
    documentationCenter=""
    authors="NarayanAnnamalai"
    manager="jefco"
    editor=""/>

<tags
    ms.service="virtual-network"
    ms.workload="virtual-network"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="narayan;aglick"/>

#<a name="virtual-network--business-continuity"></a>Virtuális hálózati – üzemkészségének

##<a name="overview"></a>– Áttekintés

A virtuális hálózati (VNet) érték egy logikai megfelelője, a hálózat a felhőben. Lehetővé teszi a saját személyes IP-címterület meghatározása, és a hálózat szakaszokhoz alhálózat be. VNets tárolni a számítási erőforrások, például az Azure virtuális gépeken futó és Cloud Services (webes/dolgozó szerepkörök) adatvédelmi oszlopazonosító jobboldali szolgál. Egy VNet lehetővé teszi, hogy közvetlenül a magánjellegű IP kommunikációját az erőforrások, akkor is. A virtuális hálózati egy helyszíni hálózaton keresztül, a hibrid beállításokat, például a virtuális Magánhálózati átjáró vagy készült ExpressRoute közül is csatolható.
 
Egy VNet létrejön egy régió hatálya alá. Két különböző régiókban azonos címterület VNets hozhat létre (azaz US keleti és US nyugati, de nem tud csatlakozni őket egy másik közvetlenül). 

##<a name="business-continuity"></a>Üzleti folytonosságot

Lehet, hogy az alkalmazás sikerült modem többféle módszert. Egy adott régió sikerült kell teljesen levágni természeti katasztrófa vagy több eszközök és szolgáltatások meghibásodása miatt részleges katasztrófa miatt. A VNet szolgáltatás hatással más a következő helyzetekben.

**K: Mit kell tennie megelőzve egy üzemszünetek egy teljes terület? tehát, ha egy tartomány az teljesen maximális természeti katasztrófa miatt? Mi történik a tartományban lévő is a virtuális hálózatok?**

Megoldás: a virtuális hálózati és az erőforrások a szóban forgó régióban marad, nem érhető el a szolgáltatási zavarok ideje alatt.

![Virtuális Egyszerű hálózatdiagram](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**Kérdés: milyen is lehet újbóli létrehozása egy másik régióbeli virtuális hálózatához?**

A: virtuális hálózati (VNet) meglehetősen egyszerű erőforrás. Egy VNet létrehozását egy másik régióbeli az azonos címterület Azure API-khoz is hivatkozhat. Hozza létre újból a ugyanazt a szóban forgó régióban található környezetet, meg kell API-hívásokhoz újra bevezetését tervezi a Cloud Services (webes/dolgozó szerepkörök) és a virtuális gépeken futó volt. Akkor is be a VPN Használatát az átjárók léptetőnyíl vezérlőelem, és a helyszíni hálózaton csatlakozni, ha a helyszíni kapcsolódási (például a hibrid telepítés).

Az utasításokat hozhat létre egy VNet találhatók [Itt](./virtual-networks-create-vnet-arm-pportal.md). 

**K: egy VNet egy adott tartomány replikáját hozható létre ismét időszakokra egy másik tartományban lévő?**

Válasz: Igen, az azonos magánjellegű IP-címterület és a források használata időszakokra két különböző régiók két VNets hozhat létre. Egy ügyfél internetes szolgáltatások a VNet lett szolgáltatónál, ha azokat is állította be forgalom Manager geo útvonal forgalom az régiót, amelynek az aktív. Azonban egy ügyfél nem tud csatlakozni két VNets a azonos helyet címét a helyszíni hálózaton, útválasztási problémákat okoz. Katasztrófa időpontját és egy VNet egy régióban elvesztése egy ügyfél léphet kapcsolatba az a rendelkezésre álló tartományban lévő többi VNet egyező címterület, hogy a helyszíni hálózaton.

Az utasításokat hozhat létre egy VNet találhatók [Itt](./virtual-networks-create-vnet-arm-pportal.md).
