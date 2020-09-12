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

