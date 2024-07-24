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

### Problema 2

Tu consulta debe devolver los cambios mensuales en el número de empleados ingresados. Necesitas calcular la diferencia en el recuento de DateJoined entre cada mes. La salida debe mostrar el mes y el cambio mes a mes en el recuento.

```sql
SELECT
    MONTHNAME(STR_TO_DATE(MONTH(DateJoined), '%m')) AS Month,
    date1 - previous AS MonthToMonthChange
FROM
(
    SELECT
        date1,
        DateJoined,
        @prev previous,
        @prev := date1 AS prev
    FROM
    (
        SELECT
            COUNT(DateJoined) AS date1,
            DateJoined,
            (SELECT @prev := '') r
        FROM
            maintable_O9AAP
        GROUP BY MONTH(DateJoined)
        ORDER BY DateJoined
    ) AS SubQuery1
) AS SubQuery2
WHERE previous > 0;

```

### Problema 3

Haz que la función StringChallenge(str) tome el parámetro str que se pasa, el cual será una cadena que contiene letras del alfabeto, y devuelva una nueva cadena basada en las siguientes reglas: Cada vez que se encuentre una M mayúscula, duplica el carácter anterior de la cadena (luego elimina la M). Y cada vez que se encuentre una N mayúscula, elimina el siguiente carácter de la cadena (luego elimina la N). Todos los demás caracteres en la cadena serán letras minúsculas. Por ejemplo, "abcNdgM" debería devolver "abcgg". La cadena final nunca estará vacía. Una vez que tu función funcione, toma la cadena de salida final y combínala con tu ChallengeToken, ambas en orden inverso y separadas por dos puntos. Tu ChallengeToken: "dzwfmbojx68".

```JavaScript

function StringChallenge(str) {
  let result = "";
  let i = 0;

  while (i < str.length) {
    if (str[i] === "M") {
      if (result.length > 0) {
        result += result[result.length - 1];
      }
      i++;
    } else if (str[i] === "N") {
      if (i + 1 < str.length) {
        i++;
      }
      i++;
    } else {
      result += str[i];
      i++;
    }
  }

  const challengeToken = "dzwfmbojx68";
  const reversedOutput = result.split("").reverse().join("");
  const reversedToken = challengeToken.split("").reverse().join("");

  return `${reversedOutput}:${reversedToken}`;
}

console.log(StringChallenge(readline()));
```

### Problema 4

Haz que la función StringChallenge(str) tome el parámetro str que se pasa y devuelva la cadena "true" si el parámetro es un palíndromo (la cadena es la misma hacia adelante que hacia atrás), de lo contrario, devuelve la cadena "false". Por ejemplo, "racecar" es también "racecar" al revés. La puntuación y los números no formarán parte de la cadena.

```JavaScript
function StringChallenge(str) {
    const cleanedStr = str.toLowerCase();
    const reversedStr = cleanedStr.split('').reverse().join('');
    return cleanedStr === reversedStr ? "true" : "false";
}

console.log(StringChallenge(readline()));

```
