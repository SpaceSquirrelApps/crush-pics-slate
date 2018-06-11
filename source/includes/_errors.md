# Errors

The Crush.pics API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid (Usually invalid JSON).
401 | Unauthorized -- Your API key is wrong.
403 | Forbidden -- When your account is suspended.
413 | Request Entity Too Large -- Sending a file larger than your plan allows.
415 | Unsupported Media Type -- Sending an unsupported image type
422 | Unprocessable Entity -- Sending invalid JSON properties
429 | Too Many Requests -- You're sending too many requests at the same time.
500 | Internal Server Error -- We had a problem with our server or any other unexpected server error. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
