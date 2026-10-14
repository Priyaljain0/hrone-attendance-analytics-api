# Design Decisions

## 1. MongoDB indexes

I added indexes for fields that are frequently used for searching and filtering attendance data. A unique compound index on `emp_code` and `date` ensures that an employee cannot have two attendance records for the same day.

## 2. Handling duplicate punch-ins

The application checks for an existing attendance record before creating a new one, but the unique MongoDB index is the final protection. This also handles the case where two requests arrive at almost the same time.

## 3. Attendance date and time

Attendance dates are based on **IST**, because the attendance day should follow the business timezone. Timestamps are converted to UTC for storage and converted back when calculating the attendance date.

## 4. Calculations

Late minutes, work hours, overtime and half-day status are calculated by the backend instead of accepting these values directly from the client. This keeps the stored attendance data consistent.

## 5. Regularization history

When attendance is manually changed, the previous values and the changes are stored in a history array along with the reason and person who made the correction. This provides an audit trail without creating a separate collection.

## 6. Analytics

For monthly summaries, department summaries and trends, MongoDB aggregation pipelines are used instead of fetching all records into Python. This keeps filtering, grouping and calculations closer to the database.

## 7. Headcount calculation

Department headcount is based on employees who had already joined during the selected period, not only on employees who have attendance records. This prevents employees with zero attendance records from being missed.

## 8. Leaderboard ranking

The late-arrival leaderboard uses MongoDB ranking so employees with the same total late minutes receive the same rank.

## 9. Pagination

Attendance listing uses database-side sorting and pagination. The page size is also limited to avoid unnecessarily large responses.

## 10. Query performance

An admin explain endpoint was added to inspect MongoDB query execution statistics. This helps verify whether the expected indexes are being used instead of relying only on application-level testing.