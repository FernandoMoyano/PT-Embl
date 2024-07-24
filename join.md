![join](https://miro.medium.com/v2/resize:fit:1200/1*av8Om3HpG1MC7YTLKvyftg.png)

### Problema 1

En este desafío de MySQL, tu consulta debe devolver la información del empleado con el tercer salario más alto. Escribe una consulta que encuentre a este empleado y devuelva esa fila, pero luego reemplaza la columna DivisionID con el correspondiente DivisionName de la tabla cb_companydivisions. También debes reemplazar la columna ManagerID con el ManagerName si el ID existe en la tabla y no es NULL. Tu salida debería verse como ID=222 Name=Mark Red DivisionName=Sales ManagerName=Susan Mall Salary=86000.

```sql

SELECT
    maintable_M3LJZ.ID,
    maintable_M3LJZ.Name,
    cb_companydivisions.DivisionName AS DivisionName,
    t.Name AS ManagerName,
    maintable_M3LJZ.Salary
FROM maintable_M3LJZ
JOIN cb_companydivisions ON cb_companydivisions.id = maintable_M3LJZ.DivisionID
JOIN (
    SELECT
        Name,
        ID
    FROM
        maintable_M3LJZ
) AS t ON t.ID = maintable_M3LJZ.ManagerID
WHERE Salary = (
    SELECT
        MIN(Salary)
    FROM (
        SELECT
            *
        FROM
            maintable_M3LJZ
        ORDER BY salary DESC
        LIMIT 3
    ) AS x
);

```
