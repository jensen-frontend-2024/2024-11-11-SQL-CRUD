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

Denna query hämtar allt ifrån tabellen "users". Alla rader inklusive alla kolumner. "\*" betyder alltså **ALLT**. Skulle tabellen innehåla miljontals rader så är denna query inte jättesmidigt. Försök då limitera hämtningen med en LIMIT kanske _( max antal rader som ingår i resultatet )_. Vill man endast ha specifika kolumner i resultatet så får man ange det.

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

För att använda LIKE så behöver vi två stycken "specialtecken" till, och de är "%" och "\_". "%" fungerar som ett wildcard som ersätter antingen 0 eller flera tecken. Har vi detta till exempel:

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

Det andra specialtecknet vi kan använda var "\_". Understräck används för specifik en okänd matchning i värdet. Så tillexempel skriver vi någon sånt här:

- "m_ta" - mata och meta båda varit en matchningen.

- "m\_\_a" - mata, meta, mala, måla osv. Allt är matchingar du vi specifkt anger två okända tecken.

- [Tillbaks till toppen](#2024-11-11-sql-repetition-och-crud)

## JOIN, Sätta ihop flera tabeller till ett sökresultat.

En join-operation används för att kombinera data från två eller flera olika tabeller baserat på gemensamma värdet i de olika tabellerna. De värden som menas här är då givitvis PK _( primary key )_ och FK _( foreign key )_ i de olika tabellerna. JOIN, låter oss koppla samman data för att få en djupare förståelse av relationerna mellan olika datamängder _( tabeller )_.

Det finns olika typer av join inom ramen av SQL, bland annat: inner join, left join, right join, cross join och outer join. De vi kommer att fokusera på är inner join _( även kallad join i sqlite )_ och left join.

### Inner Join _( join )_

JOIN är ett nyckelord som kopplar ihop två tabeller. Vi kan koppla ihop flera men då måste vi använda flera JOIN efter varandra, så vi startar med två helt enkelt. Hur de kopplas när man använder en join _( inner join )_ är att endast rader som har en matchning i båda tabellerna inkluderas. I exemplet "teachers" och "courses" så innebär det att endast "courses" som har en lärarer kopplat till sig kommer att inkluderas. Det skulle kunna finnas "courses" som inte har en lärar för tillfället, de kommer då att ignoreras i slutresultatet.

Syntax:

```sql
SELECT kolumn1, kolumn2, kolumnN..
FROM tabell1
JOIN tabell2 ON tabell1.kolumn = tabell2.kolumn
```

Så för att göra en exemepl på detta så använder vi chinook-databasen.

```sql
SELECT albums.Title AS AlbumTitle, artists.Name AS ArtistName
FROM albums
JOIN artists ON albums.ArtistId = artists.ArtistId
```

Queryn ovan hämtar albumtitlar och matchande artistnamn från tabellerna "albums" och "artists". Vi har även gjort en namnändring "on-the-go" på kolumnera så det blir lite mer lättläsligt i resultatet. Denna namnändring ändrar inget i databasen, bara i själva resultatsmängden som kommer tillbaka.

Ett till exempel. Låt oss hämta tracks kombinerat med deras album från chinook-databasen.

```sql
SELECT tracks.Name AS TrackName, albums.Title AS AlbumTitle
FROM tracks
JOIN albums ON tracks.AlbumId = albums.AlbumId
```

### Användning av COUNT tillsammans med en JOIN

Vad är count? Det är en aggregeringsfunktion som används för att räkna ut antalet rader som matchar ett givet villkor.

Exempel, hämta antalet spår för varje album.

```sql
SELECT albums.Title AS AlbumTitle, COUNT(tracks.TrackId) AS TrackCount
FROM albums
JOIN tracks ON albums.AlbumId = tracks.AlbumId
```

Denna query räknar antalet spår för varje album. En join i det här fallet _( inner join )_ säklerställer att endast album med spår inkluderad finns i resultatet.

### LEFT JOIN

LEFT JOIN retunerar alla rader från den vänstra _( första )_ tabellen och matchande rader från den högra tabellen. Om det inte finns någon matchning fylls kolumnera från den högra tabellen med NULL.

Syntax:

```sql
SELECT kolumn1, kolumn2 kolumnN..
FROM tabell1
LEFT JOIN tabell2 ON tabell1.kolumn = tabell2.kolumn
```

Exempel, lista alla artiser och deras album _( även om de inte har några album )_

```sql
SELECT artists.Name AS ArtistName, albums.Title AS AlbumTitle
FROM artists
LEFT JOIN albums ON artists.ArtistId = albums.ArtistId
```

Queryn hämtar alla artister och deras tillhärande albumtitlar. Om en artis in har något album, returneras NULL för albumtiteln.

Användninsområde för detta är helt enkelt när man vill inkludera alla rader från en tabell även om vissa inte har motsvarande data i den andra tabellen.

### Jämförelse mellan INNER JOIN och en LEFT JOIN

- INNER JOIN: retunerar endast rader med matchande data i båda tabellerna

- LEFT JOIN: Retunerar alla rader från den vänstra tabellen, även om det inte finns någon matchning i den högra tabellen. _( matchande data från de nhögra tabellen inkluderas om den finns, annars NULL )_

En JOIN som vi har gjort ovan har endast varit på tabeller som har någon form av 1-många-relation med varandra. Men JOIN funkar självklart på många-till-många-relationer också. Skillnade är då att vi har korstabell som vi måste inkludera i vår join-query. Så det blir det alltid en liten extra-join vi måste göra. 

Om vi går tillbaks till test-database vi gjorde innan så hade vi tabellerna "students", "courses" och "studentXcourses" som var en korstabell. En join för att ta fram vilka kurser en elev läste kan se ut såhär:

```sql
SELECT students.first_name, students.last_name, courses.name
FROM students
JOIN studentsXcourses ON students.student_id = studentsXcourses.student_id
JOIN courses ON studentsXcourses.course_id = courses.course_id
WHERE students.student_id = 2
```

Vi gör helt enkelt en extra join på korstabellen innan vi kan göra en join på "courses".

- [Tillbaks till toppen](#2024-11-11-sql-repetition-och-crud)

## CRUD


