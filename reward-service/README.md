# Rewards Program API

A Spring Boot REST API that calculates customer reward points based on transactions made during the last 3 months.

The application calculates monthly and total reward points for customers based on a configurable rewards program. It demonstrates REST API development, Spring Data JPA integration, exception handling, and comprehensive testing using JUnit and Mockito.

---

## Features

* Calculate reward points for customer transactions
* Monthly reward aggregation
* Total reward points calculation
* RESTful API design
* Global exception handling
* H2 in-memory database
* Layered architecture (Controller, Service, Repository)
* Unit, Slice, and Integration testing
* Sample transaction data loaded automatically

---

## Tech Stack

| Technology      | Version           |
| --------------- | ----------------- |
| Java            | 17                |
| Spring Boot     | 3.2.5             |
| Spring Data JPA | Latest            |
| H2 Database     | In-Memory         |
| Maven           | Build Tool        |
| JUnit 5         | Testing           |
| Mockito         | Mocking Framework |

---

## Reward Calculation Logic

The rewards program follows these rules:

* 2 points for every dollar spent above $100
* 1 point for every dollar spent between $51 and $100
* 0 points for spending $50 or less

### Examples

| Amount Spent | Reward Points |
| ------------ | ------------- |
| $25          | 0             |
| $75          | 25            |
| $100         | 50            |
| $120         | 90            |
| $200         | 250           |

### Formula

For a transaction amount:

```text
Amount > 100:
    Points = 50 + ((Amount - 100) * 2)

Amount between 51 and 100:
    Points = Amount - 50

Amount <= 50:
    Points = 0
```

---

## Project Structure

```text
src
├── main
│   ├── java
│   │   └── com.example.rewards
│   │       ├── controller
│   │       ├── service
│   │       ├── repository
│   │       ├── entity
│   │       ├── dto
│   │       ├── exception
│   │       └── util
│   └── resources
│       ├── application.properties
│       └── data.sql
│
└── test
    ├── controller
    ├── service
    ├── repository
    └── integration
```

---

## Sample Data

The application loads sample transactions automatically during startup.

```sql
INSERT INTO transactions VALUES
(1, 101, 'Sakthi', 120.00, DATEADD('MONTH', -2, CURRENT_DATE)),
(2, 101, 'Sakthi', 75.00, DATEADD('MONTH', -1, CURRENT_DATE)),
(3, 101, 'Sakthi', 200.00, CURRENT_DATE),

(4, 102, 'Preethi', 130.00, DATEADD('MONTH', -2, CURRENT_DATE)),
(5, 102, 'Preethi', 90.00, DATEADD('MONTH', -1, CURRENT_DATE)),
(6, 102, 'Preethi', 180.00, CURRENT_DATE),

(7, 103, 'Sakthi', 45.00, DATEADD('MONTH', -2, CURRENT_DATE)),
(8, 103, 'Sakthi', 60.00, DATEADD('MONTH', -1, CURRENT_DATE)),
(9, 103, 'Sakthi', 140.00, CURRENT_DATE),

(10, 104, 'Preethi', 49.00, CURRENT_DATE),
(11, 104, 'Preethi', 51.00, CURRENT_DATE),
(12, 104, 'Preethi', 99.00, CURRENT_DATE),
(13, 104, 'Preethi', 101.00, CURRENT_DATE),

(14, 105, 'Sakthi', 250.00, CURRENT_DATE),
(15, 105, 'Sakthi', 300.00, DATEADD('MONTH', -1, CURRENT_DATE),

(16, 106, 'OldCustomer', 500.00, DATEADD('MONTH', -6, CURRENT_DATE));
```

**Note:** Customer 106 has transactions older than 3 months and will not appear in API responses.

---

## Running the Application

### Clone the Repository

```bash
git clone https://github.com/your-username/rewards-program-api.git
```

### Navigate to Project

```bash
cd rewards-program-api
```

### Run Application

```bash
mvn spring-boot:run
```

Application starts on:

```text
http://localhost:8080
```

---

## API Endpoints

### Get Rewards for a Specific Customer

```http
GET /api/rewards/{customerId}
```

Example:

```http
GET http://localhost:8080/api/rewards/101
```

Response:

```json
{
  "customerId": 101,
  "customerName": "Sakthi",
  "monthlyRewards": [
    {
      "month": "MARCH",
      "points": 90
    },
    {
      "month": "APRIL",
      "points": 25
    },
    {
      "month": "MAY",
      "points": 250
    }
  ],
  "totalPoints": 365
}
```

---

### Get Rewards for All Customers

```http
GET /api/rewards/allCustomers
```

Example:

```http
GET http://localhost:8080/api/rewards/allCustomers
```

Response:

```json
[
  {
    "customerId": 101,
    "customerName": "Sakthi",
    "totalPoints": 365
  },
  {
    "customerId": 102,
    "customerName": "Preethi",
    "totalPoints": 310
  }
]
```

---

## Error Handling

### Customer Not Found

Request:

```http
GET /api/rewards/9999
```

Response:

```json
{
  "timestamp": "2026-05-29T10:30:00.123",
  "status": 404,
  "error": "Not Found",
  "message": "Customer not found with id: 9999"
}
```

---

## H2 Database Console

URL:

```text
http://localhost:8080/h2-console
```

Configuration:

| Property | Value                 |
| -------- | --------------------- |
| JDBC URL | jdbc:h2:mem:rewardsdb |
| Username | sa                    |
| Password | (leave blank)         |

---

## Running Tests

Run all tests:

```bash
mvn test
```

### Test Coverage

| Test Class                | Type        | Tests |
| ------------------------- | ----------- | ----- |
| RewardCalculatorTest      | Unit        | 12    |
| RewardControllerTest      | Unit        | 4     |
| RewardServiceImplTest     | Unit        | 8     |
| TransactionRepositoryTest | Slice       | 5     |
| RewardIntegrationTest     | Integration | 7     |
| Total                     |             | 36    |

---

## Sample Reward Calculations

### Transaction = $120

```text
50 points for amount between $51-$100
+
20 × 2 points for amount above $100

= 90 points
```

### Transaction = $200

```text
50 points for amount between $51-$100
+
100 × 2 points for amount above $100

= 250 points
```

---

## Future Enhancements

* Swagger/OpenAPI documentation
* PostgreSQL integration
* Docker support
* JWT Authentication
* Flyway database migration
* CI/CD pipeline with GitHub Actions

---

## Author

Sakthi Preethi

