# 2024-11-11 SQL Repetition och CRUD

<details open>
<summary>Innehåll</summary>

- [Repetition](#repetition)
  - [SELECT and FROM](#select-and-from)
  - [WHERE & AND & OR](#where--and--or)
  - [ORDER BY & DESC & ASC](#order-by--desc--asc)
  - [LIKE](#like)
- [JOIN](#join-sätta-ihop-flera-tabeller-till-ett-sökresultat)
- [CRUD](#crud)

</details>

## Repetition

### SELECT and FROM

Båda två är resererad nyckelord inom SQL. SELECT avser någon form av hämtning av data. Den ska se lite olika ut beroende på vilken data det är vi ska plocka fram. FROM betyder helt enkelt vilken tabell det är vi hämtar data ifrån. Båda nyckelordern kan skrivas med stora eller små bokstäver. Men stora bokstäver är en konvention som används.

Exmpel på detta:

```sql
SELECT * FROM users
```

Denna query hämtar allt ifrån tabellen "users". Alla rader inklusive alla kolumner. "*" betyder alltså **ALLT**. Skulle tabellen innehåla miljontals rader så är denna query inte jättesmidigt. Försök då limitera hämtningen med en LIMIT kanske _( max antal rader som ingår i resultatet )_. Vill man endast ha specifika kolumner i resultatet så får man ange det.

```sql
SELECT id, name, address FROM users
LIMIT 100
```

Nu inkluderas endast tre kolumner i resultatet och endast de 100 första raderna.

- [Tillbaks till toppen](#2024-11-11-sql-repetition-och-crud)

### WHERE & AND & OR

Dessa är villkor som ska uppfyllas av de rader som ska inkluderas i resultatet. WHERE kan används för att ange alla rader med ett specifikt värde på en kolumn, namn till exempel. Eller vill man endast ha en rad i resultatet så kan man köra en WHERE på det unika id istället. AND och OR används för att hämta rader som uppfyller 1 eller flera villkor, antingen att alla villkor ska uppfyllas för att inkludera den specifika raden, eller att en av alla villkoren ska var uppfyllt för att inkludera raden. 

```sql
SELECT * FROM users
WHERE name = "Niklas" OR address = "Drottninggatan 10"
```

I detta fall, kommer alla users med namnet "Niklas" eller adressen "Drottninggatan 10" att inkluderas i resultatet.

Man kan kombinera flera "AND" med flera "OR" men i det fallet så kanske "LIKE" passar bättre. Men allt beror situationen givitvis.

- [Tillbaks till toppen](#2024-11-11-sql-repetition-och-crud)

### ORDER BY & DESC & ASC

ORDER BY är ett sorteringsnyckelord som används för att sortera resultatet efter en specifik kolumn. Både DESC _( descending )_ och ASC _( acending )_ kan användas. Fallande och stigande med andra ord.

Tänk på att sortering på db-nivå är mycket effektivere än att hämta en större mängd data för att sen sortera den i frontend. 

- [Tillbaks till toppen](#2024-11-11-sql-repetition-och-crud)

### LIKE

LIKE används tillsammans med WHERE på samma sätt som ovan men våra villkor kan vara mycket mer "öppna" eller dynamiska kanske man kan uttrycka sig. Till exempel om vi är ute efter alla users som har ett namn som börjar på "L" så kan vi använda LIKE för att ta reda på det.

För att använda LIKE så behöver vi två stycken "specialtecken" till, och de är "%" och "_". "%" fungerar som ett wildcard som ersätter antingen 0 eller flera tecken. Har vi detta till exempel:

```sql
SELECT * FROM users
WHERE name LIKE "N%"
```

Så betyder det att vi letar efter alla namn som börjar specifikt på "N". Stor eller liten bokstav när det kommer till LIKE. 

Andra kombinationer av "%":

- "%son%" - Letar efter en matchning där "son" ingår, antingen i början, mitten eller slutet av värdet. Exempel Håkansson, Son Goku, son-son-son.
 osv.

- "%holm" - matchar allt som slutar på "holm". Ekholm, Djursholm, Stockholm osv.

- "N%" - Niklas, Nils, Nokia, Neverland osv.

Det andra specialtecknet vi kan använda var "_". Understräck används för specifik en okänd matchning i värdet. Så tillexempel skriver vi någon sånt här:

- "m_ta" - mata och meta båda varit en matchningen.

- "m__a" - mata, meta, mala, måla osv. Allt är matchingar du vi specifkt anger två okända tecken. 

- [Tillbaks till toppen](#2024-11-11-sql-repetition-och-crud)

## JOIN, Sätta ihop flera tabeller till ett sökresultat.

- [Tillbaks till toppen](#2024-11-11-sql-repetition-och-crud)

## CRUD
