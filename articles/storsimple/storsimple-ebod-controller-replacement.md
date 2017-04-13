<properties 
   pageTitle="Egy StorSimple EBOD vezérlő cseréje |} Microsoft Azure"
   description="Megtudhatja, hogyan cserélje ki az egyik vagy mindkét EBOD vezérlők StorSimple 8600 készüléken."
   services="storsimple"
   documentationCenter=""
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="08/17/2016"
   ms.author="alkohli" />

# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a>Egy EBOD vezérlő StorSimple eszközén cseréje

## <a name="overview"></a>– Áttekintés

Ebből az oktatóanyagból megtudhatja, hogyan cserélje ki a hibás EBOD vezérlő modult a Microsoft Azure StorSimple készülékén. Egy vezérlő EBOD modul cseréjéhez kell:

- A hibás EBOD vezérlő eltávolítása
- Egy új EBOD vezérlő telepítése

Mielőtt elkezdené, vegye figyelembe a következőket:

- Üres EBOD modulokat az összes használaton kívüli helyek kell szúrnia. A ház nem hűtsük megfelelően, ha egy tárolóhely megnyitott marad.

- A EBOD vezérlő szerepel melegvíz-cserélhető és eltávolítható vagy helyett. Nem távolítja el a sikertelen modul műveletet az összes elhelyezni egy másikat. Ha a csere megkezdéséhez 10 percen belül kell végzett.

>[AZURE.IMPORTANT] Mielőtt megnyitná eltávolítása vagy áthelyezése minden StorSimple összetevő, győződjön meg arról, hogy tekintse át a [biztonsági ikon szabályokat](storsimple-safety.md#safety-icon-conventions) és más [biztonságra](storsimple-safety.md).

## <a name="remove-an-ebod-controller"></a>Egy EBOD vezérlő eltávolítása

StorSimple eszköze nem sikerült EBOD vezérlő moduljának cseréje, előtt győződjön meg róla, hogy a többi EBOD vezérlő modul aktív és futnia. A következő eljárás, illetve eltávolíthatja a EBOD vezérlő modult ismertetik.

#### <a name="to-remove-an-ebod-module"></a>Ha el szeretne távolítani egy EBOD modul

1. Nyissa meg az Azure klasszikus portált.

2. Nyissa meg az **eszközök** > **karbantartási** > **Hardveres állapotát**, és ellenőrizze, hogy az aktív EBOD vezérlő modulhoz a LED állapota zöld, valamint a LED a sikertelen EBOD vezérlő modulhoz piros.

3. Keresse meg a sikertelen EBOD vezérlő modul hátulján az eszközön található.

4. Távolítsa el a kábelek, hogy a vezérlő előtt véve a EBOD modul ki a rendszer a EBOD vezérlő modul csatlakozzon.

5. Jegyezze fel a pontos Társítások port a EBOD vezérlő modul, amely a vezérlő volt kapcsolva. Számára kötelező, a rendszer visszaállításához ebben a konfigurációban a EBOD modul cseréje után. 

    >[AZURE.NOTE] A Port, amelyek az alábbi ábrán **a Host** felirata általában a ez lesz.

    ![A EBOD hátlap vezérlő](./media/storsimple-ebod-controller-replacement/IC741049.png)

     **Ábra 1** EBOD modul oldalán

  	|Címke|Leírás|
  	|:----|:----------|
  	|1|Hibafa LED|
  	|2|A Power LED|
  	|3|Társítások összekötők|
  	|4|Társítások LED|
  	|5|Csak a gyári használatra a soros portok|
  	|6|A port lévő (állomás)|
  	|7|B port (állomás meg)|
  	|8|Port C (csak gyári használata esetén)|

## <a name="install-a-new-ebod-controller"></a>Egy új EBOD vezérlő telepítése

Az alábbi eljárás és a táblázat egy EBOD vezérlő modul telepítése StorSimple eszközbe ismertetik.

#### <a name="to-install-an-ebod-controller"></a>Egy EBOD vezérlő telepítése

1. Jelölje be a sérült, különösen a felület-összekötő EBOD eszközt. Az új EBOD vezérlő nem telepíthető, ha minden elgörbülve.

2. A megnyitott pozícióban ajtózárak, a dia a modul be, a ház, mindaddig, amíg a ajtózárak vesznek.

    ![EBOD vezérlő telepítése](./media/storsimple-ebod-controller-replacement/IC741050.png)

    **Ábra 2**  A EBOD vezérlő modul telepítése

3. Zárja be a együtt. Egy kattintással kell hallható, mint a zár folytat.

    ![EBOD együtt feloldása](./media/storsimple-ebod-controller-replacement/IC741047.png)

    **Ábra 3**  A EBOD modul zár bezárása

4. Újracsatlakozás a kábelek. Használja a pontos beállításokat, amelyek a jelen, a csere előtt volt. Lásd az alábbi ábra és a táblázat csatlakoztassa a kábel olvashat.

    ![Kábel power 4U eszköze](./media/storsimple-ebod-controller-replacement/IC770723.png)

    **Ábra 4**. Újracsatlakozás a kábelek

  	|Címke|Leírás|
  	|:----|:----------|
  	|1|Elsődleges ház|
  	|2|PCM 0|
  	|3|PCM 1|
  	|4|Vezérlő 0|
  	|5|1 vezérlő|
  	|6|0 EBOD vezérlő|
  	|7|1 EBOD vezérlő|
  	|8|EBOD ház|
  	|9|A Power terjesztési mennyiség|

## <a name="next-steps"></a>Következő lépések

További tudnivalók a [StorSimple hardver összetevő feltölteni](storsimple-hardware-component-replacement.md).
