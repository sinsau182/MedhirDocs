# Medhir Backend API Documentation

## Overview
This document provides a comprehensive list of all APIs available in the Medhir backend system. Each API is categorized by its functional area and includes detailed information about endpoints, request/response formats, and authentication requirements.

## Table of Contents
- [Authentication APIs](#authentication-apis)
- [Employee Management APIs](#employee-management-apis)
- [Attendance Management APIs](#attendance-management-apis)
- [Leave Management APIs](#leave-management-apis)
- [Payroll Management APIs](#payroll-management-apis)
- [Company Management APIs](#company-management-apis)
- [Department Management APIs](#department-management-apis)
- [Designation Management APIs](#designation-management-apis)
- [Expense Management APIs](#expense-management-apis)
- [Income Management APIs](#income-management-apis)
- [Module Management APIs](#module-management-apis)
- [Public Holiday Management APIs](#public-holiday-management-apis)
- [Request Management APIs](#request-management-apis)

## Authentication APIs
Used by: `authSlice`

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| POST | `/auth/register` | Register a new user | ❌ No |
| POST | `/auth/login` | Login and get JWT token | ❌ No |

### Request/Response Examples

#### Register User
```http
POST /auth/register
Content-Type: application/json

{
    "email": "string",
    "password": "string",
    "firstName": "string",
    "lastName": "string",
    "role": "string"
}
```

Response:
```json
{
    "message": "User registered successfully",
    "userId": "string"
}
```

#### Login
```http
POST /auth/login
Content-Type: application/json

{
    "email": "string",
    "password": "string"
}
```

Response:
```json
{
    "token": "string",
    "refreshToken": "string",
    "user": {
        "id": "string",
        "email": "string",
        "role": "string"
    }
}
```

## Employee Management APIs
Used by: `employeeSlice`

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/hradmin/employees` | Fetch all employees | ✅ Yes |
| GET | `/hradmin/companies/{companyId}/employees` | Fetch employees of a company | ✅ Yes |
| POST | `/hradmin/employees` | Create new employee | ✅ Yes |
| PUT | `/hradmin/employees/{id}` | Update employee details | ✅ Yes |
| DELETE | `/hradmin/employees/{id}` | Delete an employee | ✅ Yes |

### Request/Response Examples

#### Create Employee
```http
POST /hradmin/employees
Content-Type: multipart/form-data

{
    "firstName": "string",
    "lastName": "string",
    "email": "string",
    "role": "string",
    "companyId": "string",
    "documents": [
        {
            "type": "string",
            "file": "file"
        }
    ]
}
```

Response:
```json
{
    "id": "string",
    "employeeId": "string",
    "message": "Employee created successfully"
}
```

## Attendance Management APIs
Used by: `attendanceSlice`

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/attendance/month/{month}/year/{year}` | Fetch attendance for all employees | ✅ Yes |
| GET | `/api/attendance/employee/{employeeId}/month/{month}/year/{year}` | Fetch attendance for specific employee | ✅ Yes |

### Request/Response Examples

#### Get Monthly Attendance
```http
GET /api/attendance/month/3/2024
Authorization: Bearer <token>
```

Response:
```json
[
    {
        "employeeId": "string",
        "date": "date",
        "checkIn": "time",
        "checkOut": "time",
        "status": "string"
    }
]
```

## Leave Management APIs
Used by: `leaveSlice`, `leaveBalanceSlice`, `leavePolicySlice`, `leaveTypeSlice`

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/leave-balance/current/{employeeId}` | Get current leave balance | ✅ Yes |
| PUT | `/api/leave-balance/{employeeId}` | Update leave balance | ✅ Yes |
| GET | `/api/leave-policies/company/{companyId}` | Get company leave policies | ✅ Yes |
| POST | `/api/leave-policies` | Create leave policy | ✅ Yes |
| PUT | `/api/leave-policies/{id}` | Update leave policy | ✅ Yes |
| DELETE | `/api/leave-policies/{id}` | Delete leave policy | ✅ Yes |
| GET | `/leave/employee/{employeeId}` | Get employee leaves | ✅ Yes |
| POST | `/leave/apply` | Apply for leave | ✅ Yes |
| PUT | `/leave/update-status` | Update leave status | ✅ Yes |

### Request/Response Examples

#### Apply Leave
```http
POST /leave/apply
Content-Type: application/json
Authorization: Bearer <token>

{
    "employeeId": "string",
    "leaveType": "string",
    "startDate": "date",
    "endDate": "date",
    "reason": "string"
}
```

Response:
```json
{
    "leaveId": "string",
    "message": "Leave application submitted successfully"
}
```

## Payroll Management APIs
Used by: `payrollSettingSlice`, `payslipSlice`

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/payslip/generate/{employeeId}/{month}/{year}` | Generate payslip | ✅ Yes |
| GET | `/tds-settings/company/{companyId}` | Get TDS settings | ✅ Yes |
| POST | `/tds-settings` | Create TDS settings | ✅ Yes |
| PUT | `/tds-settings/company/{companyId}` | Update TDS settings | ✅ Yes |
| GET | `/professional-tax-settings/company/{companyId}` | Get PTAX settings | ✅ Yes |
| POST | `/professional-tax-settings` | Create PTAX settings | ✅ Yes |
| PUT | `/professional-tax-settings/company/{companyId}` | Update PTAX settings | ✅ Yes |

### Request/Response Examples

#### Generate Payslip
```http
GET /payslip/generate/123/3/2024
Authorization: Bearer <token>
```

Response:
```json
{
    "employeeId": "string",
    "month": "string",
    "year": number,
    "basicSalary": number,
    "allowances": number,
    "deductions": number,
    "netSalary": number
}
```

## Company Management APIs
Used by: `companySlice`

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/superadmin/companies` | Get all companies | ✅ Yes |
| POST | `/superadmin/companies` | Create company | ✅ Yes |
| PUT | `/superadmin/companies/:id` | Update company | ✅ Yes |
| DELETE | `/superadmin/companies/:id` | Delete company | ✅ Yes |

### Request/Response Examples

#### Create Company
```http
POST /superadmin/companies
Content-Type: application/json
Authorization: Bearer <token>

{
    "name": "string",
    "address": "string",
    "contact": "string"
}
```

Response:
```json
{
    "id": "string",
    "message": "Company created successfully"
}
```

## Department Management APIs
Used by: `departmentSlice`

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/departments/company/:companyId` | Get company departments | ✅ Yes |
| POST | `/departments` | Create department | ✅ Yes |
| PUT | `/departments/:id` | Update department | ✅ Yes |
| DELETE | `/departments/:id` | Delete department | ✅ Yes |

### Request/Response Examples

#### Create Department
```http
POST /departments
Content-Type: application/json
Authorization: Bearer <token>

{
    "name": "string",
    "companyId": "string",
    "description": "string"
}
```

Response:
```json
{
    "id": "string",
    "message": "Department created successfully"
}
```

## Designation Management APIs
Used by: `designationSlice`

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/designations/company/{companyId}` | Get company designations | ✅ Yes |
| POST | `/api/designations` | Create designation | ✅ Yes |
| PUT | `/api/designations/{id}` | Update designation | ✅ Yes |
| DELETE | `/api/designations/{id}` | Delete designation | ✅ Yes |

### Request/Response Examples

#### Create Designation
```http
POST /api/designations
Content-Type: application/json
Authorization: Bearer <token>

{
    "title": "string",
    "companyId": "string",
    "description": "string"
}
```

Response:
```json
{
    "id": "string",
    "message": "Designation created successfully"
}
```

## Expense Management APIs
Used by: `expenseSlice`

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/expenses/employee/{employeeId}` | Get employee expenses | ✅ Yes |
| POST | `/expenses` | Create expense | ✅ Yes |
| PUT | `/expenses/{id}` | Update expense | ✅ Yes |

### Request/Response Examples

#### Create Expense
```http
POST /expenses
Content-Type: multipart/form-data
Authorization: Bearer <token>

{
    "employeeId": "string",
    "category": "string",
    "amount": number,
    "description": "string",
    "receipt": "file"
}
```

Response:
```json
{
    "expenseId": "string",
    "message": "Expense created successfully"
}
```

## Income Management APIs
Used by: `incomeSlice`

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/income/employee/{employeeId}` | Get employee income | ✅ Yes |
| POST | `/income` | Create income | ✅ Yes |
| PUT | `/income/{incomeId}` | Update income | ✅ Yes |

### Request/Response Examples

#### Create Income
```http
POST /income
Content-Type: application/json
Authorization: Bearer <token>

{
    "employeeId": "string",
    "category": "string",
    "amount": number,
    "description": "string"
}
```

Response:
```json
{
    "incomeId": "string",
    "message": "Income record created successfully"
}
```

## Module Management APIs
Used by: `modulesSlice`

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/superadmin/modules` | Get all modules | ✅ Yes |
| POST | `/superadmin/modules` | Create module | ✅ Yes |
| PUT | `/superadmin/modules/{moduleId}` | Update module | ✅ Yes |
| DELETE | `/superadmin/modules/{moduleId}` | Delete module | ✅ Yes |
| GET | `/employees/minimal` | Get minimal employee details | ✅ Yes |

### Request/Response Examples

#### Create Module
```http
POST /superadmin/modules
Content-Type: application/json
Authorization: Bearer <token>

{
    "name": "string",
    "description": "string",
    "permissions": ["string"]
}
```

Response:
```json
{
    "id": "string",
    "message": "Module created successfully"
}
```

## Public Holiday Management APIs
Used by: `publicHolidaySlice`

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/public-holidays/company/{companyId}` | Get company holidays | ✅ Yes |
| POST | `/public-holidays` | Create holiday | ✅ Yes |
| PUT | `/public-holidays/{id}` | Update holiday | ✅ Yes |
| DELETE | `/public-holidays/{id}` | Delete holiday | ✅ Yes |

### Request/Response Examples

#### Create Holiday
```http
POST /public-holidays
Content-Type: application/json
Authorization: Bearer <token>

{
    "name": "string",
    "date": "date",
    "companyId": "string",
    "description": "string"
}
```

Response:
```json
{
    "id": "string",
    "message": "Holiday created successfully"
}
```

## Request Management APIs
Used by: `sessionStorageSlice`

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/manager/leave/status/Pending/{employeeId}` | Get pending leave requests | ✅ Yes |
| GET | `/leave/status/{company}/Pending` | Get pending leave requests | ✅ Yes |
| GET | `/manager/{employeeId}/members/update-requests` | Get profile update requests | ✅ Yes |
| GET | `/hradmin/company/{company}/update-requests` | Get profile update requests | ✅ Yes |
| PUT | `/leave/update-status` | Update leave status | ✅ Yes |
| PUT | `/manager/{managerId}/members/{employeeId}/update-requests` | Update profile request | ✅ Yes |
| PUT | `/hradmin/update-requests/{employeeId}` | Update profile request | ✅ Yes |

### Request/Response Examples

#### Update Leave Status
```http
PUT /leave/update-status
Content-Type: application/json
Authorization: Bearer <token>

{
    "leaveId": "string",
    "status": "string",
    "comments": "string"
}
```

Response:
```json
{
    "message": "Leave status updated successfully"
}
```

## Common Response Formats

### Success Response
```json
{
    "status": "success",
    "data": {
        // Response data
    },
    "message": "string"
}
```

### Error Response
```json
{
    "status": "error",
    "error": {
        "code": "string",
        "message": "string",
        "details": [
            // Error details
        ]
    }
}
```

## Authentication Requirements

- All APIs except `/auth/register` and `/auth/login` require JWT authentication
- Include the JWT token in the Authorization header:
  ```
  Authorization: Bearer <token>
  ```

## Rate Limiting

- Standard rate limit: 100 requests per minute per IP
- Authentication endpoints: 10 requests per minute per IP

## Error Codes

- 400: Bad Request
- 401: Unauthorized
- 403: Forbidden
- 404: Not Found
- 409: Conflict
- 422: Unprocessable Entity
- 500: Internal Server Error

## Versioning

- Current API version: v1
- Version is included in the URL path: `/api/v1/...`

## Best Practices

1. Always include proper error handling
2. Use appropriate HTTP methods
3. Implement proper validation
4. Follow RESTful conventions
5. Use proper status codes
6. Implement proper security measures
7. Handle file uploads securely
8. Implement proper logging
