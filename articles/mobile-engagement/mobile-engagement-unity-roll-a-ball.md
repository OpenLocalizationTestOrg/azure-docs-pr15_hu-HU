<properties
    pageTitle="Egységét Filmtekercs megtalálhatja a kos oktatóanyagok"
    description="A klasszikus egységessége létrehozásának lépései Vetítődnek egy megtalálhatja a kos mérkőzés szavakat, amely az összes Mobile tetszés szerint elmélyedhet egységét oktatóanyagok előzetesen szükséges"
    services="mobile-engagement"
    documentationCenter="mobile"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="piyushjo" />

#<a id="unity-roll-a-ball"></a>Hozzon létre egységét Vetítődnek megtalálhatja a kos játék

Ebben az oktatóanyagban is módosultak kissé [egységét Filmtekercs megtalálhatja a kos oktatóanyagot](http://unity3d.com/learn/tutorials/projects/roll-ball-tutorial)végigvezeti a fő lépéseit. Ez a példa játék áll, az alkalmazás felhasználó által ellenőrzött gömbös "player" objektum, és a játék célja, hogy "összegyűjtése" collectible objektumok collectible objektumokkal rendelkező player objektum ütközés szerint. Feltételezi, hogy a egyszerű ismerős egységét Szerkesztői környezetben. Ha problémák merülnek fel, akkor tekintse át a teljes oktatóanyag. 

### <a name="setting-up-the-game"></a>A játék beállítása
Az alábbi lépésekkel nem az [oktatóprogram egység](https://unity3d.com/learn/tutorials/projects/roll-a-ball/set-up?playlist=17141)

1. Nyissa meg a **Egységét szerkesztő** , és kattintson az **Új**gombra. 
    
    ![][51] 
    
2. Adja meg a **Projekt neve** & **helyét**, jelölje ki a **3D** , és kattintson a **Projekt létrehozása**.
    
    ![][52]

3. Mentse az imént létrehozott az új projekt részeként **MiniGame** nevét az új belül alapértelmezett jelenet ** \_jelenetek** **eszközök** mappában:
    
    ![][53]

4. Egy- **3D objektum -> sík** egyenlő mező létrehozása, és nevezze át az e **alapokról** sík objektumként

    ![][1]

5. Az átalakítás összetevő az **alapokról** objektum visszaállítása, hogy a származási helyen. 

    ![][3]

6. Törölje a jelet a **Rács megjelenítése** **Gizmos** menüből az **alapokról** objektum.

    ![][4]

7. A **Méretezés** -összetevő az **alapokról** objektum kell frissíteni [X = 2, Y = 1, Z = 2]. 

    ![][5]

8. Egy új- **-> kapcsolataikat 3D objektum** hozzáadása a projekthez, és nevezze át a **lejátszó**ezt kapcsolataikat az objektumot. 

    ![][6]

9. Jelölje ki a **lejátszó** objektumot, majd kattintson a **Átalakítás visszaállítása** hasonlít az sík objektumra. 

10. Frissítés **Átalakítás-pozíció > Y koordináta ->** a lejátszó Y 0,5-összetevője.  

    ![][7]

11. Hozzon létre egy új nevű **anyagok** a projekt hol hozunk létre az anyag a Windows Media player színes. 

12. Hozzon létre egy új **anyag** **háttér** nevű ebbe a mappába. 

    ![][8]

13. Frissítse az anyag színének, hogy a **Albedo** tulajdonság frissítésével. Választhat a [0,32,64] RGB-értékét. 

    ![][9]

14. Húzza az anyag szeretne színt alkalmazni az **alapokról** objektumra a jelenet nézetbe. 

    ![][10]

17. Végül frissítése a **Átalakítás -> Forgatás Y ->** 60 áttekinthetővé az irányított világos objektumon. 

    ![][12]

### <a name="moving-the-player"></a>A Windows Media player áthelyezése
Az alábbi lépésekkel nem az [oktatóprogram egység](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-player?playlist=17141)

1. **RigidBody** összetevő hozzáadása a **lejátszó** objektumra. 

    ![][13]

2. A projekt **parancsfájlok** nevű új mappa létrehozása. 

3. Kattintson a **összetevő hozzáadása új parancsfájl -> C# parancsprogram ->**. Nevezze el **PlayerController**, majd kattintson a **Létrehozás és a Hozzáadás gombra**. Ezzel létrehozása és parancsfájl csatolása a lejátszó objektumot.  

    ![][14]

5. Helyezze át a parancsfájl a **parancsfájlok** mappán a projektben. 

6. Nyissa meg szerkesztésre a kedvenc parancsfájl-szerkesztő a parancsfájlt, frissítse a parancsprogram-kódot a következő kódot, és mentse. 

        using UnityEngine;
        using System.Collections;
        
        public class PlayerController : MonoBehaviour 
        {
            public float speed;
            private Rigidbody rb;
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
                rb.AddForce (movement * speed);
            }
        }
    
8. Ne feledje, hogy a fenti parancsfájl használja-e a **sebesség** tulajdonságait. A egységét szerkesztőben frissítse a sebesség tulajdonság 10.  

    ![][15]

9. Találati **lejátszása** a egységét-szerkesztőben. Most már láthatja a megtalálhatja a kos, a billentyűzet használatával szabályozhatja, és kell forgatás, illetve mozgás. 

### <a name="moving-the-camera"></a>A kamera áthelyezése
Az alábbi lépéseket a [egységét oktatóprogram](https://unity3d.com/learn/tutorials/projects/roll-a-ball/moving-the-camera?playlist=17141) származik, és a **Fő kamera** a **lejátszó** objektum lelassul. 

1. Frissítse a **Transform.Position** kell X = 0, az Y = 10.5, Z-= 10.  
2. Frissítse a **Transform.Rotation** kell X = 45, Y = 0, Z = 0.  

    ![][16]

2. Vegye fel a **CameraController** , hogy a **MainCamera** nevű új parancsfájl, és helyezze át a parancsfájlok mappában találja. 

    ![][17]

3. Nyissa meg szerkesztésre a parancsfájlt, és a következő kód beszúrása:

        using UnityEngine;
        using System.Collections;
        
        public class CameraController : MonoBehaviour {
        
            public GameObject player;
        
            private Vector3 offset;
        
            void Start ()
            {
                offset = transform.position - player.transform.position;
            }
            
            void LateUpdate ()
            {
                transform.position = player.transform.position + offset;
            }
        }
    
5. Egységét környezet - húzni a lejátszó változó a lejátszó tárolóhely a fő kamera objektum, hogy a két egymással társítva. 

    ![][18]

6. Most ha Play találati egységét szerkesztőben, és a lejátszó megtalálhatja a kos objektum majd látni fogja a kamera mozgását követő azt.  

### <a name="setting-up-the-play-area"></a>A lejátszás terület beállítása
Az alábbi lépéseket a [egységét oktatóanyagból](https://unity3d.com/learn/tutorials/projects/roll-a-ball/setting-up-the-play-area?playlist=17141)van. A falak, hogy a lejátszó megtalálhatja a kos objektum ki annak az elmozdulásnak a play területén nem húzhatja a környező az alapokról hozunk létre. 

1. Kattintson a **létrehozása -> üres létrehozása játék objektum ->** , és nevezze el a **falakon**

    ![][19]

2. A falak objektum - alatt egy új- **-> kocka 3D-objektum** létrehozása, és "Nyugati fal" nevet. 

    ![][20]

3. Az **Átalakítás-pozíció >** és a **Méretezés-átalakítás >** frissítés a Microsoft nyugati fal szülőtől. 

    ![][21]

4. Ismétlődő a nyugati fal- **keleti fal** létrehozásához a frissített átalakítás elhelyezése és méretezése. 

    ![][22]

5. Ismétlődő **Észak-fal** létrehozásához a frissített átalakítás pozíció és skála keleti falhoz. 

    ![][23]

6. Ismétlődő Észak falra, és hozzon létre **dél Fal** a frissített átalakítás pozíció és méretezése. 

    ![][24]

### <a name="creating-collectible-objects"></a>Collectible objektumok létrehozása
Az [oktatóprogram egységét](https://unity3d.com/learn/tutorials/projects/roll-a-ball/creating-collectables?playlist=17141)van az alábbi lépéseket. Néhány vonzó megjelenésű objektumot, amely a lejátszó megtalálhatja a kos objektum kell "összegyűjtése" által velük ütközés collectible tárgyak készlete hozunk létre. 

1. Hozzon létre egy új **3D kocka objektumra** , és a felvétel nevet. 

2. Módosítsa a- **Átalakítás -> Forgatás** & felvételi objektum-**Átalakítás-skála >** . 

    ![][25]

3. Hozzon létre, és egy **Új C# parancsfájl** **Rotator** nevű felvételi objektum csatolása. Győződjön meg arról, hogy a parancsprogram helyezze a parancsfájlok mappában találja. 

    ![][26]

4. Nyissa meg szerkesztésre a parancsfájlt, és állítsa át a következő: 

        using UnityEngine;
        using System.Collections;
        
        public class Rotator : MonoBehaviour {
        
            void Update () 
            {
                transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
            }
        }

5. A tengelyen elforgatása most találati a a lejátszási módból a egységét szerkesztő és a felvétel objektum alatt.

6. **Prefabs** nevű új mappa létrehozása 

    ![][27]

7. Húzza a **Felvétel** objektumot, és a Prefabs mappába.

    ![][28]

8. Hozzon létre egy új **üres játék objektum** neve **Pickups**. Helyzetét visszaáll az origin, és húzza a a felvételi objektum játék objektum alatt.  

    ![][29]

9. A **Felvétel** objektum másolása és kiterjed az **alapokról** objektumra a **lejátszó** objektum körül a **Transform.Position tartozó X és Z** értékek megfelelően frissítésével. 

    ![][30]

10. Hozzon létre egy **új anyag** nevű **Felvétel** és frissítése, hogy legyen a piros színnel a **Albedo tulajdonság** hasonló mi azt did az alapokról objektum frissítésének frissítésével. 

    ![][31]

11. Az anyag alkalmazása 4 felvételi objektumait.

    ![][32]

### <a name="collecting-the-pickup-objects"></a>A felvételi objektumok gyűjtése
Az [oktatóprogram egységét](https://unity3d.com/learn/tutorials/projects/roll-a-ball/collecting-pick-up-objects?playlist=17141)van az alábbi lépéseket. A Windows Media Player úgy, hogy "összegyűjtése' a felvételi objektumok által velük ütközés tudja frissíteni fogjuk. 

1. Nyissa meg szerkesztésre a lejátszó objektumhoz **PlayerController** parancsfájl, és frissítheti a következőket:  

        using UnityEngine;
        using System.Collections;
        
        public class PlayerController : MonoBehaviour {
        
            public float speed;
        
            private Rigidbody rb;
        
            void Start ()
            {
                rb = GetComponent<Rigidbody>();
            }
        
            void FixedUpdate ()
            {
                float moveHorizontal = Input.GetAxis ("Horizontal");
                float moveVertical = Input.GetAxis ("Vertical");
        
                Vector3 movement = new Vector3 (moveHorizontal, 0.0f, moveVertical);
        
                rb.AddForce (movement * speed);
            }
        
            void OnTriggerEnter(Collider other) 
            {
                if (other.gameObject.CompareTag ("Pick Up"))
                {
                    other.gameObject.SetActive (false);
                }
            }
        }

2. **Válassza ki a felfelé** (annak egyeznie kell mit tartalmaz a parancsfájl) nevű új **címke** létrehozása  

    ![][33]
    
    ![][34]

3. **Címke** alkalmazása a Prefab felvétel objektumra. 

    ![][35]

4. Engedélyezze az Prefab objektum **IsTrigger** jelölőnégyzetet.

    ![][36]

5. Felvétel Prefab objektum adja hozzá a kemény törzsébe. A teljesítmény optimalizálásához azt a statikus collider használt frissül a dinamikus collider. 

    ![][37]
  
6. Végül jelölje be a **IsKinematic** tulajdonság az prefab objektum. 

    ![][38]

7. Találati **lejátszása** a egységét editor, és tudják játék ez **egy megtalálhatja a kos Vetítődnek** áthelyezésével, a lejátszó objektum, a billentyűk segítségével irányba adatbevitelt. 

### <a name="updating-the-game-for-mobile-play"></a>A mobil Play játék frissítése
A fenti szakaszok az alapvető oktatóprogram egységét kötött. Most már a mérkőzés szavakat, hogy legyen mobileszköz rövid módosítja azt. Figyelje meg, hogy azt használja a mérkőzés szavakat az amennyiben teszteléséhez billentyűzet beviteli. Most már azt módosítja azt, hogy a Windows Media player azt is szabályozhatja a mozgásvonalak a telefonszámot, vagyis használatával a bemeneti adataiként gyorsulásmérőt használata 

Nyissa meg szerkesztésre a **PlayerController** parancsfájl, és frissítse a **FixedUpdate** módszer a lejátszó objektum áthelyezése a mozgásvonalak a gyorsulásmérő a használatával. 

        void FixedUpdate()
        {
            //float moveHorizontal = Input.GetAxis("Horizontal");
            //float moveVertical = Input.GetAxis("Vertical");
            //Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
            rb.AddForce(Input.acceleration.x * Speed, 0, -Input.acceleration.z * Speed);
        }

Ebben az oktatóanyagban köt egy egyszerű játék létrehozása az egységnyi, és ez a játék lehetőség eszközön telepítheti. 

<!-- Images -->
[1]: ./media/mobile-engagement-unity-roll-a-ball/1.png  
[2]: ./media/mobile-engagement-unity-roll-a-ball/2.png
[3]: ./media/mobile-engagement-unity-roll-a-ball/3.png
[4]: ./media/mobile-engagement-unity-roll-a-ball/4.png
[5]: ./media/mobile-engagement-unity-roll-a-ball/5.png
[6]: ./media/mobile-engagement-unity-roll-a-ball/6.png
[7]: ./media/mobile-engagement-unity-roll-a-ball/7.png
[8]: ./media/mobile-engagement-unity-roll-a-ball/8.png
[9]: ./media/mobile-engagement-unity-roll-a-ball/9.png  
[10]: ./media/mobile-engagement-unity-roll-a-ball/10.png    
[11]: ./media/mobile-engagement-unity-roll-a-ball/11.png    
[12]: ./media/mobile-engagement-unity-roll-a-ball/12.png
[13]: ./media/mobile-engagement-unity-roll-a-ball/13.png
[14]: ./media/mobile-engagement-unity-roll-a-ball/14.png
[15]: ./media/mobile-engagement-unity-roll-a-ball/15.png
[16]: ./media/mobile-engagement-unity-roll-a-ball/16.png
[17]: ./media/mobile-engagement-unity-roll-a-ball/17.png
[18]: ./media/mobile-engagement-unity-roll-a-ball/18.png
[19]: ./media/mobile-engagement-unity-roll-a-ball/19.png    
[20]: ./media/mobile-engagement-unity-roll-a-ball/20.png    
[21]: ./media/mobile-engagement-unity-roll-a-ball/21.png    
[22]: ./media/mobile-engagement-unity-roll-a-ball/22.png    
[23]: ./media/mobile-engagement-unity-roll-a-ball/23.png    
[24]: ./media/mobile-engagement-unity-roll-a-ball/24.png    
[25]: ./media/mobile-engagement-unity-roll-a-ball/25.png    
[26]: ./media/mobile-engagement-unity-roll-a-ball/26.png    
[27]: ./media/mobile-engagement-unity-roll-a-ball/27.png    
[28]: ./media/mobile-engagement-unity-roll-a-ball/28.png    
[29]: ./media/mobile-engagement-unity-roll-a-ball/29.png    
[30]: ./media/mobile-engagement-unity-roll-a-ball/30.png    
[31]: ./media/mobile-engagement-unity-roll-a-ball/31.png    
[32]: ./media/mobile-engagement-unity-roll-a-ball/32.png    
[33]: ./media/mobile-engagement-unity-roll-a-ball/33.png    
[34]: ./media/mobile-engagement-unity-roll-a-ball/34.png    
[35]: ./media/mobile-engagement-unity-roll-a-ball/35.png    
[36]: ./media/mobile-engagement-unity-roll-a-ball/36.png    
[37]: ./media/mobile-engagement-unity-roll-a-ball/37.png    
[38]: ./media/mobile-engagement-unity-roll-a-ball/38.png    
[51]: ./media/mobile-engagement-unity-roll-a-ball/new-project.png
[52]: ./media/mobile-engagement-unity-roll-a-ball/new-project-properties.png
[53]: ./media/mobile-engagement-unity-roll-a-ball/save-scene.png

    
    
    
    
    
    
    
    
    
    
    
    
