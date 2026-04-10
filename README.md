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
npm install -g newman newman-reporter-htmlextra

newman run ServeRest-Collection.postman_collection.json \
    -e ServeRest-Environment.postman_environment.json \
    --delay-request 600
```

## Tests covered

The collection tests all user endpoints:

**Authentication**
- JWT login
- Invalid credentials
- Required fields validation

**POST /usuarios**
- Create user (regular and admin)
- Duplicate email
- Field validation

**GET /usuarios**
- List all users
- Filter by name and type

**GET /usuarios/{id}**
- Get by ID
- Non-existent ID

**PUT /usuarios/{id}**
- Update user
- Upsert (create if not exists)

**DELETE /usuarios/{id}**
- Delete user
- Idempotent deletion

Total: 27 tests covering positive and negative scenarios.

## Notes

- The API has a 100 requests per minute limit, hence the 600ms delay
- Tests create and delete data automatically (self-cleaning)
- JWT token is generated and stored automatically on the first run
