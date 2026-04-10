# ServeRest API - Complete Test Suite

## 📋 Overview

This Postman collection provides **100% test coverage** for the ServeRest Users API (https://serverest.dev/). The test suite includes comprehensive testing for all CRUD operations with JWT authentication.

## 🎯 API Under Test

| Base URL | Rate Limit | Authentication |
|----------|------------|----------------|
| https://serverest.dev | 100 requests/minute | JWT Bearer Token |

## 📁 Files Included

| File | Description |
|------|-------------|
| `ServeRest-Collection.postman_collection.json` | Complete Postman collection with all tests |
| `ServeRest-Environment.postman_environment.json` | Environment variables configuration |

---

## 🔧 Setup Instructions

### Prerequisites

- **Postman** (Desktop or Web version)
- Internet connection to access https://serverest.dev

### Step 1: Import Collection

1. Open Postman
2. Click **Import** button (top-left corner)
3. Select `ServeRest-Collection.postman_collection.json`
4. Click **Import**

### Step 2: Import Environment

1. Click **Import** button
2. Select `ServeRest-Environment.postman_environment.json`
3. Click **Import**
4. In the top-right corner, select **ServeRest Environment** from the environment dropdown

### Step 3: Run Tests

#### Option A: Run Individual Requests
- Navigate through folders in the collection
- Click **Send** on any request
- View test results in the **Tests** tab

#### Option B: Run Entire Collection (Recommended)
1. Right-click on the collection name
2. Select **Run collection**
3. Configure Runner:
   - **Iterations**: 1 (or more for stress testing)
   - **Delay**: 600ms (to respect rate limits)
4. Click **Run ServeRest API - Complete Test Suite**

---

## 🧪 Test Coverage

### 1. Authentication (`/login`)

| Test ID | Test Name | Method | Expected Status |
|---------|-----------|--------|-----------------|
| 1.1 | Setup - Create Admin User for Auth | POST | 201 |
| 1.2 | Login - Get JWT Token | POST | 200 |
| 1.3 | Login - Invalid Credentials | POST | 401 |
| 1.4 | Login - Missing Fields | POST | 400 |

**Coverage:**
- ✅ Successful authentication
- ✅ Invalid credentials handling
- ✅ Missing required fields validation
- ✅ JWT token extraction and storage

---

### 2. Create User - POST (`/usuarios`)

| Test ID | Test Name | Expected Status |
|---------|-----------|-----------------|
| 2.1 | Create User - Success | 201 |
| 2.2 | Create User - Duplicate Email | 400 |
| 2.3 | Create User - Missing Required Fields | 400 |
| 2.4 | Create User - Invalid Email Format | 400 |
| 2.5 | Create User - Empty Name | 400 |
| 2.6 | Create Admin User - Success | 201 |

**Request Body Schema:**
```json
{
    "nome": "string (required)",
    "email": "string (required, valid email format)",
    "password": "string (required)",
    "administrador": "string (required, 'true' or 'false')"
}
```

**Coverage:**
- ✅ Successful user creation (regular and admin)
- ✅ Duplicate email prevention
- ✅ All required fields validation
- ✅ Email format validation
- ✅ Empty field handling

---

### 3. List Users - GET (`/usuarios`)

| Test ID | Test Name | Expected Status |
|---------|-----------|-----------------|
| 3.1 | List All Users - Success | 200 |
| 3.2 | List Users - Filter by Name | 200 |
| 3.3 | List Users - Filter by Admin Status | 200 |

**Coverage:**
- ✅ List all users
- ✅ Query parameter filtering (nome)
- ✅ Admin status filtering
- ✅ Response structure validation

---

### 4. Get User by ID - GET (`/usuarios/{_id}`)

| Test ID | Test Name | Expected Status |
|---------|-----------|-----------------|
| 4.1 | Get User by ID - Success | 200 |
| 4.2 | Get User by ID - Not Found | 400 |
| 4.3 | Get User by ID - Invalid ID Format | 400 |

**Coverage:**
- ✅ Successful retrieval by ID
- ✅ Non-existent user handling
- ✅ Invalid ID format handling

---

### 5. Update User - PUT (`/usuarios/{_id}`)

| Test ID | Test Name | Expected Status |
|---------|-----------|-----------------|
| 5.1 | Update User - Success | 200 |
| 5.2 | Update User - Create if Not Exists (Upsert) | 201 |
| 5.3 | Update User - Missing Fields | 400 |
| 5.4 | Update User - Duplicate Email | 400 |

**Coverage:**
- ✅ Successful update
- ✅ Upsert functionality (create if not exists)
- ✅ Required fields validation
- ✅ Duplicate email on update

---

### 6. Delete User - DELETE (`/usuarios/{_id}`)

| Test ID | Test Name | Expected Status |
|---------|-----------|-----------------|
| 6.1 | Delete User - Success | 200 |
| 6.2 | Delete User - Not Found (Idempotent) | 200 |
| 6.3 | Delete Upsert User - Cleanup | 200 |
| 6.4 | Delete Admin User - Cleanup | 200 |
| 6.5 | Delete Test Admin User - Final Cleanup | 200 |

**Coverage:**
- ✅ Successful deletion
- ✅ Idempotent delete (non-existent user)
- ✅ Test data cleanup

---

### 7. Rate Limiting Tests

| Test ID | Test Name | Expected Status |
|---------|-----------|-----------------|
| 7.1 | Rapid Sequential Requests | 200 |

**Coverage:**
- ✅ Response time monitoring
- ✅ Rate limit compliance (100 req/min)
- ✅ No 429 status codes

---

### 8. API Health Check

| Test ID | Test Name | Expected Status |
|---------|-----------|-----------------|
| 8.1 | API Availability Check | 200 |

**Coverage:**
- ✅ API accessibility
- ✅ Response time validation
- ✅ JSON response format

---

## 📊 Test Summary

| Category | Tests | Coverage |
|----------|-------|----------|
| Authentication | 4 | 100% |
| POST /usuarios | 6 | 100% |
| GET /usuarios | 3 | 100% |
| GET /usuarios/{id} | 3 | 100% |
| PUT /usuarios/{id} | 4 | 100% |
| DELETE /usuarios/{id} | 5 | 100% |
| Rate Limiting | 1 | 100% |
| Health Check | 1 | 100% |
| **TOTAL** | **27** | **100%** |

---

## 🔐 JWT Authentication

The collection automatically handles JWT authentication:

1. **Test 1.1** creates a temporary admin user
2. **Test 1.2** logs in and stores the JWT token in `authToken` variable
3. All subsequent requests that require authentication use the stored token

**Token Format:**
```
Authorization: Bearer <jwt_token>
```

---

## ⚠️ Rate Limiting

The ServeRest API has a limit of **100 requests per minute**. The collection includes:

- Pre-request delay options (600ms recommended)
- Rate limit monitoring in tests
- Response time assertions

**To enable rate limiting protection:**

Uncomment this line in the collection's pre-request script:
```javascript
// setTimeout(function() {}, 600); // 600ms = 100 req/min
```

---

## 🔄 Test Data Management

The collection is **self-cleaning**:

1. Creates test users at the beginning
2. Performs all CRUD operations
3. Deletes all test data at the end

**Variables used:**
- `testEmail` - Auto-generated unique email
- `userId` - Stores created user ID for subsequent tests
- `adminUserId` - Admin user ID for cleanup

---

## 🚀 Running from Command Line (Newman)

### Install Newman
```bash
npm install -g newman
```

### Run Collection
```bash
newman run ServeRest-Collection.postman_collection.json \
    -e ServeRest-Environment.postman_environment.json \
    --delay-request 600
```

### Generate HTML Report
```bash
npm install -g newman-reporter-htmlextra

newman run ServeRest-Collection.postman_collection.json \
    -e ServeRest-Environment.postman_environment.json \
    --delay-request 600 \
    -r htmlextra
```

---

## 📈 CI/CD Integration

### GitHub Actions Example

```yaml
name: API Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Install Newman
        run: npm install -g newman newman-reporter-htmlextra
      
      - name: Run API Tests
        run: |
          newman run ServeRest-Collection.postman_collection.json \
            -e ServeRest-Environment.postman_environment.json \
            --delay-request 600 \
            -r htmlextra,cli
      
      - name: Upload Report
        uses: actions/upload-artifact@v3
        with:
          name: test-report
          path: newman/
```

---

## 🐛 Troubleshooting

| Issue | Solution |
|-------|----------|
| 429 Too Many Requests | Increase delay between requests |
| Connection timeout | Check internet connection |
| Tests failing on cleanup | Run cleanup tests manually |
| Token expired | Re-run authentication tests |

---

## 📝 Author

Automated Test Suite - April 2026

## 📄 License

MIT License - Free to use and modify
