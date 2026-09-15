# Database Optimization Challenge

## Background

You are working on an event management system with over 2 million events. The following query takes 8-12 seconds and is causing timeout issues in production.

## Schema

```sql
-- events table (~2M rows)
events (
  id INT PRIMARY KEY,
  venue_id INT,
  date DATE,
  min_price DECIMAL(10,2),
  available_tickets INT,
  created_at TIMESTAMP
)

-- venues table (~50K rows)
venues (
  id INT PRIMARY KEY,
  name VARCHAR(255),
  city VARCHAR(100),
  capacity INT
)

-- event_categories table (~8M rows)
event_categories (
  event_id INT,
  category_name VARCHAR(50),
  PRIMARY KEY (event_id, category_name)
)
```

Assume a mainstream relational database (PostgreSQL is fine). You may propose additional columns or tables if needed.

## Problem query

```sql
SELECT e.*, v.name as venue_name, v.city
FROM events e
JOIN venues v ON e.venue_id = v.id
JOIN event_categories ec ON e.id = ec.event_id
WHERE v.city = 'Vancouver'
  AND e.date >= '2026-06-01'
  AND e.date <= '2026-12-31'
  AND ec.category_name IN ('sports', 'concerts')
  AND e.available_tickets > 0
ORDER BY e.date, e.min_price;
```



## Questions

1. **How would you optimize this query?** Walk through your approach step by step.
2. **What indexes would you create and why?**
3. **How would you verify the optimization worked?**
4. **Any alternative approaches** to restructure the query or data model?
5. **Correctness:** does this query return one row per event you intend to show? If not, what breaks, and how do you fix it?

