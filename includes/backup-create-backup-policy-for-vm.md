## <a name="defining-a-backup-policy"></a>A biztonsági másolat házirend megadása

Biztonsági házirend határozza meg, hogy mikor kell venni, az adatok pillanatképet, és mennyi ideig őrződnek meg ezeket a pillanatképek mátrixok. A virtuális mentésével házirend megadásakor válthat a biztonsági mentési feladat *egyszer*. Amikor létrehoz egy új házirendet, érvényesül a tárolóból elemre. A biztonsági másolat házirend felület néz ki:

![Biztonsági másolat házirend](./media/backup-create-policy-for-vms/backup-policy.png)

A házirend létrehozása:

1. Írja be egy nevet a **házirend nevére**.

2. Az adatok is pillanatképek napi vagy heti időközönként. A **Biztonsági másolat gyakoriság** legördülő menü használatával válassza ki, hogy adatokat pillanatképek tett napi vagy heti.

    - Ha úgy dönt, hogy a napi intervallumban, a kijelölt vezérlőelem segítségével válassza ki a pillanatkép napja időpontját. Ha módosítani szeretné az órát, kijelölésének az órát, és jelölje ki az új órát.

    ![Napi biztonsági házirend](./media/backup-create-policy-for-vms/backup-policy-daily.png) <br/>

    - Ha úgy dönt, hogy a heti intervallumban, a kiemelt vezérlők segítségével jelölje ki a, a hét napja és a pillanatkép a időpontját. A napi menüben jelölje ki egy vagy több napig. Az óra menüjében válassza a egy órával. Ha módosítani szeretné az órát, kijelölésének törlése a kijelölt óra, és jelölje ki az új órát.

    ![Heti biztonsági házirend](./media/backup-create-policy-for-vms/backup-policy-weekly.png)

3. Alapértelmezés szerint minden **Adatmegőrzés tartományát** lehetőség van kijelölve. Törölje a jelet az adatmegőrzési tartomány korlátozva használni nem kívánt. Ezután adja meg a interval(s) használni.

    Havi, és évente adatmegőrzési tartományok lehetővé teszi, hogy adja meg a pillanatképek a napi, heti vagy növelése alapján.

    >[AZURE.NOTE] Ha egy virtuális védheti, egy biztonsági mentési feladat fut. egyszer Ha a biztonsági mentés futtatásakor megegyezik a minden adatmegőrzési tartományra.

4. A házirend összes beállításokról után a lap tetején kattintson a **Mentés**gombra.

    Az új házirendet a program azonnal alkalmazza a tárolóból elemre.
