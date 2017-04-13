## <a name="create-an-event-hub"></a>Hozzon létre egy esemény hubhoz

1. Jelentkezzen be az [Azure-portálra][], és a képernyő tetején kattintson az **Új** balra a képernyő.

2. Kattintson az **adatok + Analytics**, majd kattintson az **Esemény hubok**.

    ![](./media/event-hubs-create-event-hub/create-event-hub9.png)

3. Írja be a **Létrehozás névtér** lap névtér nevét. A rendszer azonnal ellenőrzi, hogy ha érhető el a nevét.

    ![](./media/event-hubs-create-event-hub/create-event-hub1.png)

4. Miután gondoskodhat arról, hogy a névtér neve áll rendelkezésre, válassza a árak réteg (Basic vagy Standard). Válassza ki, az Azure előfizetés erőforráscsoport és helyet, ahová az erőforrás létrehozásához. 

2. Kattintson a **Create** a névtér létrehozásához.

6. Az esemény hubok névtér listájában válassza az újonnan létrehozott névtér.      

    ![](./media/event-hubs-create-event-hub/create-event-hub2.png)

7. Kattintson a névtér lap **Esemény hubok**.

    ![](./media/event-hubs-create-event-hub/create-event-hub3.png)

8. A lap tetején kattintson az **Esemény központi hozzáadása**gombra.

    ![](./media/event-hubs-create-event-hub/create-event-hub4.png)

3. Írja be az esemény központi nevét, majd kattintson a **Létrehozás**gombra.

    ![](./media/event-hubs-create-event-hub/create-event-hub5.png)

4. Az esemény hubok, kattintson az újonnan létrehozott esemény központi nevét. 

    ![](./media/event-hubs-create-event-hub/create-event-hub6.png)

5. Vissza a névtér lap (nem az adott esemény központi lap) kattintson **a megosztott access-házirendek**, és válassza a **RootManageSharedAccessKey**.

    ![](./media/event-hubs-create-event-hub/create-event-hub7.png)

5. A Másolás gombra kattintva másolja a vágólapra a **RootManageSharedAccessKey** kapcsolati karakterláncot. Mentse a kapcsolati karakterláncot az oktatóprogram használata.

    ![](./media/event-hubs-create-event-hub/create-event-hub8.png)

Az esemény központi most hoz létre, és a kapcsolat karakterláncok küldéséhez és fogadásához események van szüksége van.

[Azure portál]: https://portal.azure.com/