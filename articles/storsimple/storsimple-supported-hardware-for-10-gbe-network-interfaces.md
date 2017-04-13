<properties 
   pageTitle="StorSimple 10 GbE hardver felületek |} Microsoft Azure"
   description="A támogatott kis helyigény beépíthető (SFP) adó, a kábelek és a kapcsolók a 10 GbE hálózati kapcsolat esetén StorSimple eszközén mutatja be."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="09/21/2016"
   ms.author="alkohli" />

# <a name="supported-hardware-for-the-10-gbe-network-interfaces-on-your-storsimple-device"></a>Hardveres támogatott StorSimple eszközén 10 GbE hálózati kapcsolat esetén

## <a name="overview"></a>– Áttekintés

Ez a cikk a Microsoft Azure StorSimple eszközzel működő kiegészítő hardver információt tartalmaz.

## <a name="list-of-devices-tested-by-microsoft"></a>A Microsoft által vizsgált eszközök listája

A következő kis helyigény beépíthető (SFP) adó kábelek és annak érdekében, hogy működjenek optimálisan eszközökkel kapcsolók a Microsoft tesztelte. (Az alábbi táblázatokban frissülnek, új hardver vizsgálata.)

### <a name="sfp-transceivers"></a>SFP + adó

|Helyezése|Modell|
|---|---|
|Cisco|SFP-10G-SR|

### <a name="cables"></a>Kábelek

|S nem. |Helyezése|Modell|
|---|---|---|
| 1.|Cisco|SFP-H10GB-CU1M|
| 2.|Cisco|SFP-H10GB-CU2M|
| 3.|Cisco|SFP-H10GB-CU3M|
| 4.|Tripp Lite|N820 - 05M (OM3)|

### <a name="switches"></a>Kapcsolók

|S nem.|Helyezése|Modell|
|---|---|---|
| 1. |Cisco|N3K-C3172PQ-10GE|
| 2. |Cisco|N3K-C3048-ZM-F|
| 3. |Cisco|N5K-C5596UP-BE|

## <a name="list-of-devices-tested-in-the-field"></a>A mező vizsgált eszközök listája

Ebben a szakaszban már telepítette a mező StorSimple vevők eszközök listáját tartalmazza. Ezek a Microsoft nem teszteltük, de valószínűleg az StorSimple eszköz használata.
 
| Paraméter                         | Érték                                    |
|-----------------------------------|------------------------------------------|
| Váltás a helyezése                     | Boróka                                  |
| Váltás a modell                    | ex4550-32F                               |
| Váltás az operációs rendszer verziója | JunOS 12.3R9.4                           |
| A lap modell                     | Portok beépített (PIC 0)                    |
| Adó helyezése                  | Boróka                                  |
| Adó modell               | Cikkszám 740-021308 <br></br> Cikkszám 740-030658                   |
| Adó belső vezérlőprogram verziója    | 01 verzióját (jelentése) 0.0 Rev            |
| Kábel modell                     | Kétoldalas átkötés LC/LC 50/125µ, OM3, LSZH |
| StorSimple modell                | 8600                                     |
| StorSimple szoftver verziószáma     | 6.3.9600.17491                           |


## <a name="list-of-devices-tested-by-oem-provider-mellanox"></a>(Mellanox) OEM szolgáltató által vizsgált eszközök listája  

Mellanox tesztelte a következő kis helyigény beépíthető (SFP) adó kábelek és annak érdekében, hogy működjenek optimálisan Mellanox hálózati kapcsolatok, például a 10 GbE hálózati kapcsolatok StorSimple eszközén a kapcsolók.

### <a name="cables-and-modules-supported-by-mellanox"></a>Kábelek és Mellanox által támogatott modulok 

Az alábbi táblázat a kábelek és a modulokat Mellanox által támogatott. Ezek a Microsoft nem teszteltük, de valószínűleg az StorSimple eszköz használata.

| S nem. | Sebessége | Modell                 | Leírás                                            | Ellenőrizze                |
|--------|-------|-----------------------|--------------------------------------------------------|-----------------------|
| 1.     | 10 GbE| CAB-SFP-SFP - 1 MILLIÓ        | passive réz kábel SFP + 10 Gb/s-1 millió                   | Arista                |
| 2.     | 10 GbE| CAB-SFP-SFP - 2M        | passive réz kábel SFP + 10 Gb/s 2m                   | Arista                |
| 3.     | 10 GbE| CAB-SFP-SFP - 3M        | passive réz kábel SFP + 10 Gb/s 3m                   | Arista                |
| 4.     | 10 GbE| CAB-SFP-SFP - 5M        | passive réz kábel SFP + 10 Gb/s 5m                   | Arista                |
| 5.     | 10 GbE| Cisco SFP-H10GBCU1M   | Cisco SFP + kábel                                       | Cisco                 |
| 6.     | 10 GbE| Cisco SFP-H10GBCU3M   | Cisco SFP + kábel                                       | Cisco                 |
| 7.     |10 GbE | Cisco SFP-H10GBCU5M   | Cisco SFP + kábel                                       | Cisco                 |
| 8.     | 10 GbE| J9281B HP X242 10 G    | SFP + SFP + 1 millió réz kábel közvetlen csatolása             | HP                    |
| 9.     | 10 GbE| 455883-B21 HP BLc     | 10 GB-os SR SFP + csatlakozás                                       | HP                    |
| 10.    | 10 GbE| 455886-B21 HP BLc     | 10 GB-os LR SFP + csatlakozás                                       | HP                    |
| 11.    |10 GbE | 487649-B21 HP BLc     | SFP + m 0,5 10GbE réz kábel                           | HP                    |
| 12.    |10 GbE | 487652-B21 HP BLc     | SFP + 1 millió 10GbE réz kábel                             | HP                    |
| 13-mal.    |10 GbE | 487655-B21 HP BLc     | 3m 10GbE SFP + réz kábel                             | HP                    |
| 14.    |10 GbE | 487658-B21 HP BLc     | SFP + 7m 10GbE réz kábel                             | HP                    |
| 15.    |10 GbE | 537963-B21 HP BLc     | SFP + 5m 10GbE réz kábel                             | HP                    |
| 16.    |10 GbE | AP784A HP             | 3m C-sorozat passzív réz SFP + kábel                  | HP                    |
| 17.    |10 GbE | AP785A HP             | 5m C-sorozat passzív réz SFP + kábel                  | HP                    |
| a 18.    |10 GbE | AP818A HP             | 1 millió B sorozathoz aktív réz SFP + kábel                   | HP                    |
| 19.    |10 GbE | AP819A HP             | 3m B sorozathoz aktív réz SFP + kábel                   | HP                    |
| 20.    |10 GbE | J9150A HP             | X132 10 G SFP + LC SR adó                        | HP                    |
| 21.    |10 GbE | J9151A HP             | X132 10 G SFP + LC LR adó                        | HP                    |
| 22.    |10 GbE | J9283B HP             | X242 10 G SFP + SFP + 3 m DAC kábel                        | HP                    |
| 23.    |10 GbE | J9285B HP             | X242 10 G SFP + SFP + 7 m DAC kábel                        | HP                    |
| 24.    | 10 GbE| JD095B HP             | X240 10 G SFP + SFP + m 0,65 DAC kábel                     | HP                    |
| 25.    | 10 GbE| JD096B HP             | X240 10 G SFP + SFP + m 1.2-es DAC kábel                      | HP                    |
| 26.    | 10 GbE| JD097B HP             | X240 10 G SFP + SFP + 3 m apa kábel                        | HP                    |
| 27.    | 10 GbE| MAM1Q00A-QSA Mellanox | SFP + kártyához QSFP                                   | Mellanox technológiák |
| 28.    | 10 GbE| MC2309124-006 Mt      | Passive réz kábel 1 x SFP+ való QSFP 10 Gb/s 24awg 7 m   | Mellanox technológiák |
| 29.    | 10 GbE| MC2309124-007 Mt      | Passive réz kábel 1 x SFP+ való QSFP 10 Gb/s 24awg 7 m   | Mellanox technológiák |
| 30.    | 10 GbE| MC2309130-003 Mt      | Passive réz kábel 1 x SFP+ való QSFP 10 Gb/s 30awg 3 m   | Mellanox technológiák |
| 31.    | 10 GbE| MC2309130-00A Mt      | Passive réz kábel 1 x SFP+ QSFP 10 Gb/s 30awg a 0,5 m | Mellanox technológiák |
| 32.    | 10 GbE| MC3309124-005 Mt      | Passive kiemelés kábelmodemek 1 x SFP+ 10 Gb/s 24awg 5 m           | Mellanox technológiák |
| 33.    | 10 GbE| MC3309124-007 Mt      | Passive kiemelés kábelmodemek 1 x SFP+ 10 Gb/s 24awg 7 m           | Mellanox technológiák |
| 34.    | 10 GbE| MC3309130-003 Mt      | Passive kiemelés kábelmodemek 1 x SFP+ 10 Gb/s 30awg 3 m           | Mellanox technológiák |
| 35.    | 10 GbE| MC3309130-00A Mt      | Passive kiemelés kábelmodemek 1 x SFP+ 10 Gb/s 30awg 0,5 m         | Mellanox technológiák |

### <a name="switches-supported-by-mellanox"></a>Mellanox által támogatott kapcsolók 

Az alábbi táblázat a kapcsolók Mellanox által támogatott. Ezek a Microsoft nem teszteltük, de valószínűleg az StorSimple eszköz használata.

| S nem. | Sebessége | Modell | Leírás                                                         | Helyezése              |
|--------|-------|-------------|---------------------------------------------------------------------|-------------|
| 1.     | 10GbE | 516733-B21  | HP ProCurve 6120XG 10GbE Ethernet lap kapcsoló                      | HP    |
| 2.     | 10GbE | 538113-B21  | HP 10GbE átadó modul (PTM)                                  | HP    |
| 3.     | 10GbE | EN4093      | IBM PureFlex rendszer háló EN4093 10 Gigabit méretezhető kapcsoló modul | IBM   |
| 4.     | 1GbE  | 3020        | Cisco katalizátort 3020 1GbE kapcsoló lap                               | Cisco |
| 5.     | 1GbE  | 3020 X       | Váltás a Cisco katalizátort 3020 X 1GbE lap                              | Cisco |
| 6.     | 1GbE  | 438030-B21  | HP 1GbE kapcsoló modul - GbE2c réteg 2 és 3 Ethernet lap Váltás       | HP    |
| 7.     | 1GbE  | 6120G       | HP ProCurve 6120G/XG 1GbE kapcsoló lap                              | HP    |

## <a name="next-steps"></a>Következő lépések

[További tudnivalók a hardver-összetevő StorSimple és állapotát](storsimple-monitor-hardware-status.md).
