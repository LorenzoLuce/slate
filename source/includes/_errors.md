# Error Codes

The BigProfiles API uses the following codes:


Code | Meaning
---------- | -------
200 | OK
329 | Forbidden -- Exceeded API rate limit.
400 | Bad Request -- Missing required field.
401 | Unauthorized -- Please supply a key with header's field x-api-key.
403 | Forbidden -- Exceeded API limit.
404 | Not Found -- Not an API Endpoint.
504 | Timeout -- Please retry.