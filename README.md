# API Testing Repository

This repository contains a sample FastAPI application and a Postman-based API testing workspace for the Mergington High School Activities API.

## Repository structure

```text
.
├── postman/
│   ├── collections/
│   │   ├── activities/
│   │   │   └── activities-catalog.postman_collection.json
│   │   └── signups/
│   │       └── activity-signups.postman_collection.json
│   └── environments/
│       ├── dev.postman_environment.json
│       └── qa.postman_environment.json
├── src/
│   ├── app.py
│   └── README.md
├── requirements.txt
└── pytest.ini
```

## Postman organization

The Postman assets follow API testing best practices:

- Collections are organized by feature (`activities` and `signups`).
- Environments are separated by deployment target (`dev` and `qa`).
- Requests use reusable environment variables such as `baseUrl` and `maxResponseTime`.
- Test scripts stay close to the request they validate so they are easy to maintain.
- Environment files contain placeholders only and do not store secrets.

## Included test coverage

### Activities catalog collection

`postman/collections/activities/activities-catalog.postman_collection.json`

- Positive test for `GET /activities`
- Status code validation
- Response time validation
- Header validation
- JSON schema validation
- Response body validation
- Boundary validation to ensure participant counts stay within the configured limit

### Activity signups collection

`postman/collections/signups/activity-signups.postman_collection.json`

- Positive test for successful sign-up
- Negative test for unknown activities
- Boundary test with a long email alias
- Security tests using injection-style and XSS-style payloads

## Environment files

- `postman/environments/dev.postman_environment.json`
- `postman/environments/qa.postman_environment.json`

Update the `baseUrl` values as needed for your actual Dev and QA deployments.

## Run the API locally

Install dependencies and start the sample API:

```bash
python -m pip install -r requirements.txt
uvicorn src.app:app --reload
```

The API is then available at `http://localhost:8000`.

## Run the collections in Postman

1. Import both collection files from `postman/collections/`.
2. Import the desired environment file from `postman/environments/`.
3. Select the environment in Postman.
4. Update `baseUrl` if needed.
5. Run a collection with the Postman Collection Runner.

## Run the collections with Newman

You can also run the collections from the command line without changing the repository:

```bash
npx newman run postman/collections/activities/activities-catalog.postman_collection.json \
  -e postman/environments/dev.postman_environment.json

npx newman run postman/collections/signups/activity-signups.postman_collection.json \
  -e postman/environments/dev.postman_environment.json
```

## Notes

- The sample application documentation remains in `/src/README.md`.
- The security requests are designed to verify the API fails safely and avoids 5xx responses.
