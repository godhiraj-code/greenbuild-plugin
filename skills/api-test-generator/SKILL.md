---
name: api-test-generator
description: Generate API test cases and runnable tests (pytest+requests, REST Assured, Playwright request, Postman/Newman) from an OpenAPI spec or endpoint description, plus consumer-driven contract tests with Pact.
---
# API test generation
1. Get the spec (OpenAPI, Postman collection, or curl samples) and the team's framework from memory.
2. Cases per endpoint:
   - happy path
   - required vs optional fields
   - type, format and boundary checks
   - auth: missing, invalid, expired, wrong role
   - 404/409 responses
   - idempotency
   - pagination and filters
   - response schema/contract checks
   Prioritize by risk (writes, auth, money), and show the table before writing code.
3. pytest + requests:
```python
import os, uuid, requests
BASE = os.environ["BASE_URL"]
def test_create_user(auth_headers):
    body = {"email": f"qa+{uuid.uuid4().hex[:8]}@example.com"}
    r = requests.post(f"{BASE}/users", json=body, headers=auth_headers, timeout=10)
    assert r.status_code == 201
    assert r.json()["email"] == body["email"]
```
   Validate the schema with jsonschema or pydantic, and clean up the data you created in fixtures.
4. REST Assured (Java):
```java
given().baseUri(System.getenv("BASE_URL")).auth().oauth2(token)
  .contentType(ContentType.JSON).body(payload)
.when().post("/users")
.then().statusCode(201).body("email", equalTo(email))
  .body(matchesJsonSchemaInClasspath("user.json"));
```
5. Postman/Newman in CI: `npm i -g newman`, then `newman run collection.json -e staging.json --reporters cli,junit --reporter-junit-export results.xml`. Keep tests in the collection's Tests tab (`pm.test`, `pm.expect`). Keep env secrets out of git.
6. Contract testing with Pact (consumer-driven):
   - The consumer test defines its interactions against the Pact mock server (`@pact-foundation/pact` for JS, `pact-python` v3 for Python, `au.com.dius.pact` for JVM), which writes a pact JSON file.
   - Publish to a Pact Broker or PactFlow, and the provider verifies against it in its own CI.
   - Gate deploys with `pact-broker can-i-deploy --pacticipant <app> --version <sha> --to-environment production`.
   - Contracts test the shape and expectations the consumer depends on, not business logic.
7. Never put real tokens or production URLs in code. Give the run command, and state that the code was not executed unless you actually ran it.
