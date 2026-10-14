# Employee Attendance & Analytics API

A backend API built using **Python, FastAPI and MongoDB** for managing employee attendance and generating attendance-related analytics.

## Features

- Employee creation and employee listing
- Punch-in and punch-out with timestamp validation
- Automatic calculation of late minutes, work hours, overtime and half-day status
- Attendance listing with filtering and pagination
- Attendance regularization with reason and audit history
- Monthly employee attendance summary
- Department-wise attendance summary
- Late arrival leaderboard
- Department attendance trend with 7-day moving average
- MongoDB query explain endpoint for checking query performance

## Tech Stack

- Python
- FastAPI
- MongoDB
- PyMongo
- Pydantic
- Uvicorn

## Setup

Create and activate a virtual environment, then install the dependencies:

```bash
pip install -r requirements.txt
```

Create a local `.env` file with the MongoDB connection details:

```env
MONGO_URI=your_mongodb_connection_string
MONGO_DB=hrone_attendance
```

Run the application:

```bash
uvicorn app.main:app --reload
```

The API documentation is available at:

```text
http://127.0.0.1:8000/docs
```

## Notes

Attendance dates are calculated using **IST**, while timestamps are stored as BSON datetime values in MongoDB. Attendance calculations such as late minutes and work hours are derived from punch timestamps.

The application also creates the required MongoDB indexes at startup to support uniqueness and commonly used queries.