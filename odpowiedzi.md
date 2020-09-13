# Odpowiedzi

## Polecenie SELECT 👀

- Wyświetl wszystkie dane wszystkich klientów firmy.
- Wyświetl tylko nazwisko i adres
-Z tabeli produktów wyświetl nazwę produktu wraz z ceną.

```sql
SELECT * from customer;
SELECT first_name, last_name from customer;
SELECT product_name, price from product;
```

- Wyświetl roczne pensje wszystkich pracowników.

```sql
SELECT
	employee_id, first_name, last_name, salary * 12 AS salary_per_year
FROM employee;
```

## Filtrowanie WHERE 🔍

- Sprawdź, ilu pracowników o nazwisku Smith pracuje w tej firmie, a Jones?

```sql
SELECT employee_id, first_name, last_name
FROM employee
WHERE last_name = 'Smith';
-- wynik 0

SELECT employee_id, first_name, last_name
FROM employee
WHERE last_name = 'Jones';
-- wynik 2
```

#### Operatory

1. Wyszukaj wszystkich pracowników, których id jest większe niż 10
2. Wyszukaj praconików, którzy mają na nazwisko Berry
3. Wyszukaj wszystkich pracowników o nazwisku różnym od Jones
4. Wyszukaj zarabiających więcej niż 50000
5. Znajdź rekordy pracowników o nazwiskach zaczynających się od _Bu_
6. Znajdź pracowników, których nazwisko zawiera w środku literę t (nie na końcu, nie na początku).
7. Znajdź zarówno pracownice o imieniu Anna, jak i Anne.

```sql
SELECT employee_id, first_name, last_name
FROM employee
WHERE employee_id > 10;

SELECT employee_id, first_name, last_name
FROM employee
WHERE last_name = 'Berry';

SELECT employee_id, first_name, last_name
FROM employee
WHERE last_name <> 'Jones';

SELECT employee_id, first_name, last_name, salary
FROM employee
WHERE salary > 50000;

SELECT employee_id, first_name, last_name
FROM employee
WHERE last_name <> 'Jones';

SELECT employee_id, first_name, last_name
FROM employee
WHERE last_name LIKE 'Bu%';

SELECT employee_id, first_name, last_name
FROM employee
WHERE last_name LIKE '%t%';

SELECT employee_id, first_name, last_name
FROM employee
WHERE first_name LIKE 'Ann_';
```

### Łączenie filtrów

1. Zmodyfikuj to zapytanie tak by znaleźć pracowników, których nazwisko zawiera dwie litery r w środku obok siebie np. Berry, Perry, Garrett, etc.
2. Znajdź pracowników o pensji wyższej lub równej 60 tys a niższej niż 90 tys.
3. Znajdź pracowników o imieniu Justin lub o nazwisku Little
4. Znajdź pracowników, których pensja jest wyższa niż 6500 i imię to Barbara lub nazwisko zawiera literę o

```sql
SELECT first_name, last_name, salary
FROM employee 
WHERE last_name LIKE '%rr%';

-- rozwiązanie 2 AND
SELECT first_name, last_name, salary
FROM employee 
WHERE salary >= 60000 AND salary < 90000;

-- rozwiązanie 2 BETWEEN !
SELECT first_name, last_name, salary
FROM employee 
WHERE salary BETWEEN 60000 AND 89999;

SELECT first_name, last_name, salary
FROM employee 
WHERE first_name = 'Justin' OR last_name = 'Little';

SELECT first_name, last_name, salary
FROM employee 
WHERE salary > 65000 AND (first_name = 'Barbara' OR last_name LIKE '%o%');
```

### NULL
- Znajdź pracowników, których pensja nie została podana

```sql
SELECT first_name, last_name, salary
FROM employee 
WHERE salary IS NULL;

-- wynik: 2
```

## Polecenie DISTINCT 🌠

1. Policz ile departamentów pojawia się w tabeli pracowników. Wyświetl tylko unikalne wartości departamentów.
2. Sprawdź w jakich nazwiskach przedostatnia litera to litera a.

```sql
SELECT COUNT(DISTINCT department_id)
FROM employee;

SELECT DISTINCT last_name
FROM employee
WHERE last_name LIKE '%a';
```

## Sortowanie ORDER BY 🎈

- Posortuj tabelę pracowników wg. pensji rosnąco oraz malejąco.
- Ogranicz swoje zapytanie. Znajdź pracowników, którzy pracują w departamentach o id wyższym niż 4, i którzy zarabiają ponad 70 tys a mniej niż 100 tys.
- Wyświetl numer departamentu i jego nazwę z tabeli zawierającej działy w firmie, nazwy wyświetl w kolejności alfabetycznej.

```sql
SELECT first_name, last_name, salary
FROM employee
ORDER BY salary DESC;

SELECT first_name, last_name, salary, department_id
FROM employee
WHERE department_id > 4 AND salary > 70000 AND salary < 100000
ORDER BY salary DESC;

SELECT department_id, department_name
FROM department
WHERE department_id > 4
ORDER BY department_name;
```


-  ⭐ Posortuj dane pracowników po dwóch kolumnach, najpierw alfabetycznie wg nazwisk i malejąco wg pensji.
-  ⭐ Sprawdź jak działają metoody UPPER/LOWER/REPLACE dla PostgreSQL. Wyświetl nazwy departamentów drukowanymi literami w kolejności alfabetycznej. Odfiltruj tak, by mieć widoczne tylko departamenty o nazwach dwu członowych.

```sql
SELECT first_name, last_name, salary
FROM employee
ORDER BY last_name, salary DESC

SELECT department_id, UPPER(department_name)
FROM department
WHERE department_name LIKE '% %'
ORDER BY department_name;
```

## Dodawanie i usuwanie rekordów ✒️

### Aktualizowanie danych 🍩

- Dowolnemu pracownikowi zmień nazwisko. Podejrzyj zmianę
- Podnieś wszystkim pracownikom działu o id 4 pensję o 10 tys. Wyświetl zmianę.
- Podnieś pensję pracownikom, których manager ma id większe niż 190 i których pensja była niższa niż 70 tys. Wynik przed i po wyświetl.

```sql
UPDATE employee
SET last_name = 'Smith'
WHERE employee_id = 1;

SELECT * FROM employee 
WHERE employee_id = 1


UPDATE employee
SET salary = salary + 10000
WHERE department_id = 4;

UPDATE employee
SET salary = salary + 10000
WHERE manager_id > 190 AND salary < 70000;
```

### Usuwanie danych 🗑️

- Usuń pracowników, których pensja nie jest znana
- Znajdź pracownika, który nie posiada managera. Usuń go z tabeli.
- Znajdź pracowników działu 5 i usuń wszystkich ❌

```sql
DELETE FROM employee
where salary IS NULL;

DELETE FROM employee
where manager_id IS NULL;

DELETE FROM employee
where department_id = 5;
```

## Funkcje 💫
- Znajdź maksymalną pensję w dziale nr 7
- Znajdź minimalną pensję pracownika tej firmy
- Znajdź średnią pensję pracowników firmy
- Znajdź średnią pensję pracowników tej firmy, którzy należą do działów 4, 5, 6, 7 lub ich nazwisko zawiera literę a wewnątrz nazwiska (nie na początku i nie na końcu).

```sql
SELECT MAX(salary)
FROM employee
WHERE department_id = 7;

SELECT MIN(salary)
FROM employee;

SELECT AVG(salary)
FROM employee;

SELECT AVG(salary)
FROM employee
WHERE department_id IN (4, 5, 6, 7) OR last_name LIKE '%a%';
```

## 9. Podzapytania 🤔

1. Sprawdź jaka pensja jest minimalna w tej firmie. Znajdź pracowników, którzy zarabiają pensję do 10 tys większą niż pensja minimalna. Policz ich.
2. Znajdź pracowników, którzy zarabiają pensję do 10 tys mniejszą niż pensja maksymalna. Policz ich.
3. Znajdź pracowników, którzy zarabiają od o 20 tys więcej niż pensja minimalna do o 20 tys mniej niż pensja maksymalna.
4. Znajdź pracowników, którzy zarabiają ponad połowę pensji pracownika o najwyższej pensji, ich nazwisko zawiera literę _r_.  Pogrupuj wg. otrzymywanej pensji oraz posortuj malejąco liczbą pracowników o takiej samej pensji.


```sql
SELECT COUNT(*) FROM employee
WHERE salary < (
    SELECT MIN(salary) + 10000
    FROM employee
);

-- wynik: 23

SELECT COUNT(*) FROM employee
WHERE salary > (
    SELECT MAX(salary) - 10000
    FROM employee
);

--- wynik: 18


SELECT COUNT(*) FROM employee
WHERE salary > (
    SELECT MIN(salary) + 20000
    FROM employee
)
AND salary < (
    SELECT MAX(salary) - 20000
    FROM employee
);

SELECT salary, COUNT(*) FROM employee
WHERE salary > (
    SELECT MAX(salary) / 2
    FROM employee
)
AND last_name LIKE '%r%'
GROUP BY salary
ORDER BY count DESC;
```

