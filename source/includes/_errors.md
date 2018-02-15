# Errors

Generally the only errors you will receive are 400, 404, 405 or 500. Because the API is overly-forgiving, requests that attempt to create a duplicate user or are otherwise unacceptable will yield 200 indicating it was well formatted but provide a message describing the internal failure.

Error Code | Meaning
---------- | -------
400 | Bad Request
401 | Unauthorized
403 | Forbidden
404 | Not Found
405 | Method Not Allowed
500 | Internal Server Error
503 | Service Unavailable