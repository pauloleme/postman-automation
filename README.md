# ServeRest API Tests

Automated tests for the ServeRest API (https://serverest.dev/) using Postman.

## What's included

- `ServeRest-Collection.postman_collection.json` - Collection with all tests
- `ServeRest-Environment.postman_environment.json` - Environment variables

## How to run

### In Postman

1. Open Postman and click **Import**
2. Import both JSON files
3. Select "ServeRest Environment" from the dropdown in the top right corner
4. Right-click the collection > **Run collection**
5. Set delay to 600ms and click Run

### Via terminal (Newman)

```bash
npm install -g newman

newman run ServeRest-Collection.postman_collection.json \
    -e ServeRest-Environment.postman_environment.json \
    --delay-request 600
```

## Tests covered (16 total)

**1. Authentication** (1 test)
- 1.1 Create Admin User

**2. Users - Create** (2 tests)
- 2.1 Create Regular User
- 2.2 Create Admin User

**3. Users - List** (3 tests)
- 3.1 List All Users
- 3.2 Filter by Admin Status
- 3.3 Filter by Name

**4. Users - Get by ID** (1 test)
- 4.1 Get User by ID

**5. Users - Update** (2 tests)
- 5.1 Update User
- 5.2 Upsert User (create via PUT)

**6. Users - Delete** (5 tests)
- 6.1 Delete User
- 6.2 Delete Upsert User
- 6.3 Delete Admin User
- 6.4 Delete Test Admin User
- 6.5 Cleanup Variables

**7. Health Check** (1 test)
- 7.1 API Health

**8. Products** (1 test)
- 8.1 List Products

## Notes

- The API has a 100 requests per minute limit, hence the 600ms delay
- Tests create and delete data automatically (self-cleaning)
- All tests use `pm.globals` for variable persistence across requests
