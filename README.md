# AligneAI Python API library

The AligneI Python library provides convenient access to the AligneAI REST API from any Python 3.7+
application. The library includes type definitions for all request params and response fields,
and offers only synchronous (asynchronous upcoming) client powered by [requests](https://github.com/psf/requests).

## Documentation

The REST API direct access and its documentation is still being worked upon.

## Installation

> [!IMPORTANT]
> The Aligne AI Python SDK is not public and is only available for internal aligne use or our customers (On request) at the moment.

```sh
pip install aligneai
```

## Usage

```python
import json
from aligneai import AligneAI

def get_embeddings():
    # Initialise AligneAI client
    client = AligneAI()

    # Generate embeddings using AligneAI
    response = client.embeddings.create(
        inputs=["The new coffee blend from BeanWorks is robust and flavorful", 
                "Looking for a budget-friendly beach vacation spot in July with great food and snorkeling options."])

    # Log the response into your stdout
    print("Embeddings created sccessfully:\n\n", json.dumps(response))

if __name__ == "__main__":
    get_embeddings()
```

While you can provide an `api_key` keyword argument,
we recommend using [python-dotenv](https://pypi.org/project/python-dotenv/)
to add `ALIGNEAI_API_KEY="My API Key"` to your `.env` file
so that your API Key is not stored in source control.

## Async usage

In Progress ...

Functionality between the synchronous and asynchronous clients is otherwise identical.

## Streaming Responses

We provide support for streaming responses using Server Side Events (SSE). (In Progress)

## Module-level client

We recommend that you always instantiate a client (e.g., with `client = AligneAI()`) in application code.

## Handling errors

When the library is unable to connect to the API (for example, due to network connection problems or a timeout), a subclass of `aligneai.APIConnectionError` will be raised.

When the API returns a non-success status code (that is, 4xx or 5xx
response), a subclass of `aligneai.APIStatusError` is raised, containing `status_code` and `response` properties.

All errors inherit from `aligneai.APIError`.

Error codes are as followed:

| Status Code | Error Type                 |
| ----------- | -------------------------- |
| 400         | `BadRequestError`          |
| 401         | `AuthenticationError`      |
| 403         | `PermissionDeniedError`    |
| 404         | `NotFoundError`            |
| 422         | `UnprocessableEntityError` |
| 429         | `RateLimitError`           |
| >=500       | `InternalServerError`      |
| N/A         | `APIConnectionError`       |

### Retries

Certain errors are automatically retried 2 times by default, with a short exponential backoff.
Connection errors (for example, due to a network connectivity problem), 408 Request Timeout, 409 Conflict,
429 Rate Limit, and >=500 Internal errors are all retried by default.

You can use the `max_retries` option to configure or disable retry settings. (In Progress)

## Versioning

This package generally follows [SemVer](https://semver.org/spec/v2.0.0.html) conventions, though certain backwards-incompatible changes may be released as minor versions:

1. Changes that only affect static types, without breaking runtime behavior.
2. Changes to library internals which are technically public but not intended or documented for external use. _(Please open a GitHub issue to let us know if you are relying on such internals)_.
3. Changes that we do not expect to impact the vast majority of users in practice.

We take backwards-compatibility seriously and work hard to ensure you can rely on a smooth upgrade experience.

We are keen for your feedback; please open an [issue](https://www.github.com/aligne/aligneai-python/issues) with questions, bugs, or suggestions.