# Odpowiedzi

## Polecenie SELECT 👀

Wyświetl wszystkie dane wszystkich klientów firmy.
Wyświetl tylko nazwisko i adres
Z tabeli produktów wyświetl nazwę produktu wraz z ceną.

```sql
SELECT * from customer;
SELECT first_name, last_name from customer;
SELECT product_name, price from product;
```

Wyświetl roczne pensje wszystkich pracowników.

```sql
SELECT
	employee_id, first_name, last_name, salary * 12 AS salary_per_year
FROM employee;
```

## Filtrowanie WHERE 🔍

Sprawdź, ilu pracowników o nazwisku Smith pracuje w tej firmie, a Jones?

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
Znajdź pracowników, których pensja nie została podana

```sql
SELECT first_name, last_name, salary
FROM employee 
WHERE salary IS NULL;

-- wynik: 2
```