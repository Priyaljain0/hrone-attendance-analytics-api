# DECISIONS.md

## 1. Indexes

I created a unique index on `employees.emp_code` because it is the business identifier and must never be duplicated. For attendance, I created a unique compound index on `(emp_code, date)` because there can be at most one attendance document for an employee on an attendance day. I also added indexes supporting employee department/joined-date queries and attendance date/employee sorting and filtering.

## 2. Punch-in race

I did not rely only on checking for an existing attendance record before inserting. Two concurrent requests could both pass such a check. The unique `(emp_code, date)` index provides the database-level guarantee. If MongoDB raises `DuplicateKeyError`, the API returns HTTP 409.

## 3. Ties

The late leaderboard uses MongoDB `$setWindowFields` with `$rank`. This gives equal late-minute totals the same rank and skips the following rank as required by standard competition ranking. The result limit is applied after ranking so ties at the cutoff are not incorrectly removed.

## 4. Headcount

Department summary and trend calculations derive headcount from the `employees` collection rather than only from attendance logs. This ensures employees who joined by the relevant date but have no attendance records are still counted.

## 5. Large datasets

The attendance listing performs filtering, sorting and pagination in MongoDB rather than loading the collection into Python. Analytics use MongoDB aggregation pipelines so that filtering, grouping, joins and window calculations are performed by the database. Supporting indexes were added for the main query patterns.