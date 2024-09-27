# Problem Details for HTTP APIs
This is a base for applying the Problem Details for HTTP APIs in your API. It was original developed by Bump.sh as part of their Train Travel API template, which we have also published as a base, but this base is meant to just showcase Problem Details for HTTP APIs all by itself, encouraging reuse across APIs.

## Links
Here are two reference links to help provide more background in this base.

- [Problem Details for HTTP APIs](https://datatracker.ietf.org/doc/html/rfc7807) -  This document defines a "problem detail" as a way to carry machine-readable details of errors in a HTTP response to avoid the need to define new error response formats for HTTP APIs.
- [Train Travel API: A Modern OpenAPI PetStore Replacement](https://bump.sh/blog/modern-openapi-petstore-replacement) - Everyone working with OpenAPI (formerly Swagger) will have come across the PetStore at some point. It's a sample OpenAPI description for an imaginary Pet Store with an API, but the OpenAPI is old, and the API it describes is pretty far from best practices. We thought it was time for a refresh, so we're bringing you the Train Travel API, a new sample OpenAPI you can use for your tooling and testing.

## OpenAPI
Here is the [OpenAPI base](https://github.com/api-commons/problem-details-for-http-apis/blob/main/openapi.yml), which you are welcome to fork as part of this repo.

```
openapi: 3.1.0
info:
  title: Problem Details for HTTP APIs
  description: |
    This is a base OpenAPI for the Problem Details for HTTP APIs, as a way to carry machine-readable details of errors in a HTTP response to avoid the need to define new error response formats for HTTP APIs. This was originally developed by Bump.sh as part of their [Train Travel API template](https://bump.sh/bump-examples/doc/train-travel-api), but reduced here to provide a base just for showcasing Problem Details for HTTP APIs.
  version: 1.0.0
  contact:
    name: API Evangelist
    url: https://apievangelist.com
    email: info@apievangelist.com
  license: 
    name: Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International
    url: https://creativecommons.org/licenses/by-nc-sa/4.0/deed.en

externalDocs:
  description: This document defines a "problem detail" as a way to carry machine-readable details of errors in a HTTP response to avoid the need to define new error response formats for HTTP APIs.
  url: https://datatracker.ietf.org/doc/html/rfc7807

tags:
  - name: Errors
    description: | 
      Managing errors encountered within APIs in a standardized way.

paths:
  /example:
    get:
      summary: Get Example
      description: This is just an example to demonstrate errors.
      operationId: getExample
      tags:
        - Errors
      responses:
        '200':
          description: Just an empty response.

        '400':
          $ref: '#/components/responses/BadRequest'
        '401':
          $ref: '#/components/responses/Unauthorized'
        '403':
          $ref: '#/components/responses/Forbidden'
        '429':
          $ref: '#/components/responses/TooManyRequests'
        '500':
          $ref: '#/components/responses/InternalServerError'

components:
  schemas:
    Problem:
      xml:
        name: problem
        namespace: urn:ietf:rfc:7807
      properties:
        type:
          type: string
          description: A URI reference that identifies the problem type
          example: https://example.com/probs/out-of-credit
        title:
          type: string
          description: A short, human-readable summary of the problem type
          example: You do not have enough credit.
        detail:
          type: string
          description: A human-readable explanation specific to this occurrence of the problem
          example: Your current balance is 30, but that costs 50.
        instance:
          type: string
          description: A URI reference that identifies the specific occurrence of the problem
          example: /account/12345/msgs/abc
        status:
          type: integer
          description: The HTTP status code
          example: 400
  headers:
    RateLimit:
      description: |
        The RateLimit header communicates quota policies. It contains a `limit` to
        convey the expiring limit, `remaining` to convey the remaining quota units,
        and `reset` to convey the time window reset time.
      schema:
        type: string
        example:
          - limit=10, remaining=0, reset=10

    Retry-After:
      description: | 
        The Retry-After header indicates how long the user agent should wait before making a follow-up request. 
        The value is in seconds and can be an integer or a date in the future. 
        If the value is an integer, it indicates the number of seconds to wait. 
        If the value is a date, it indicates the time at which the user agent should make a follow-up request. 
      schema:
        type: string
      examples:
        integer:
          value: '120'
          summary: Retry after 120 seconds
        date:
          value: 'Fri, 31 Dec 2021 23:59:59 GMT'
          summary: Retry after the specified date

  responses:
  
    BadRequest:
      description: Bad Request
      headers:
        RateLimit:
          $ref: '#/components/headers/RateLimit'
      content:
        application/problem+json:
          schema:
            $ref: '#/components/schemas/Problem'
          example:
            type: https://example.com/errors/bad-request
            title: Bad Request
            status: 400
            detail: The request is invalid or missing required parameters.
        application/problem+xml:
          schema:
            $ref: '#/components/schemas/Problem'
          example:
            type: https://example.com/errors/bad-request
            title: Bad Request
            status: 400
            detail: The request is invalid or missing required parameters.

    Conflict:
      description: Conflict
      headers:
        RateLimit:
          $ref: '#/components/headers/RateLimit'
      content:
        application/problem+json:
          schema:
            $ref: '#/components/schemas/Problem'
          example:
            type: https://example.com/errors/conflict
            title: Conflict
            status: 409
            detail: There is a conflict with an existing resource.
        application/problem+xml:
          schema:
            $ref: '#/components/schemas/Problem'
          example:
            type: https://example.com/errors/conflict
            title: Conflict
            status: 409
            detail: There is a conflict with an existing resource.

    Forbidden:
      description: Forbidden
      headers:
        RateLimit:
          $ref: '#/components/headers/RateLimit'
      content:
        application/problem+json:
          schema:
            $ref: '#/components/schemas/Problem'
          example:
            type: https://example.com/errors/forbidden
            title: Forbidden
            status: 403
            detail: Access is forbidden with the provided credentials.
        application/problem+xml:
          schema:
            $ref: '#/components/schemas/Problem'
          example:
            type: https://example.com/errors/forbidden
            title: Forbidden
            status: 403
            detail: Access is forbidden with the provided credentials.

    InternalServerError:
      description: Internal Server Error
      headers:
        RateLimit:
          $ref: '#/components/headers/RateLimit'
      content:
        application/problem+json:
          schema:
            $ref: '#/components/schemas/Problem'
          example:
            type: https://example.com/errors/internal-server-error
            title: Internal Server Error
            status: 500
            detail: An unexpected error occurred.
        application/problem+xml:
          schema:
            $ref: '#/components/schemas/Problem'
          example:
            type: https://example.com/errors/internal-server-error
            title: Internal Server Error
            status: 500
            detail: An unexpected error occurred.

    NotFound:
      description: Not Found
      headers:
        RateLimit:
          $ref: '#/components/headers/RateLimit'
      content:
        application/problem+json:
          schema:
            $ref: '#/components/schemas/Problem'
          example:
            type: https://example.com/errors/not-found
            title: Not Found
            status: 404
            detail: The requested resource was not found.
        application/problem+xml:
          schema:
            $ref: '#/components/schemas/Problem'
          example:
            type: https://example.com/errors/not-found
            title: Not Found
            status: 404
            detail: The requested resource was not found.

    TooManyRequests:
      description: Too Many Requests
      headers:
        RateLimit:
          $ref: '#/components/headers/RateLimit'
        Retry-After:
          $ref: '#/components/headers/Retry-After'
      content:
        application/problem+json:
          schema:
            $ref: '#/components/schemas/Problem'
          example:
            type: https://example.com/errors/too-many-requests
            title: Too Many Requests
            status: 429
            detail: You have exceeded the rate limit.
        application/problem+xml:
          schema:
            $ref: '#/components/schemas/Problem'
          example:
            type: https://example.com/errors/too-many-requests
            title: Too Many Requests
            status: 429
            detail: You have exceeded the rate limit.

    Unauthorized:
      description: Unauthorized
      headers:
        RateLimit:
          $ref: '#/components/headers/RateLimit'
      content:
        application/problem+json:
          schema:
            $ref: '#/components/schemas/Problem'
          example:
            type: https://example.com/errors/unauthorized
            title: Unauthorized
            status: 401
            detail: You do not have the necessary permissions.
        application/problem+xml:
          schema:
            $ref: '#/components/schemas/Problem'
          example:
            type: https://example.com/errors/unauthorized
            title: Unauthorized
            status: 401
            detail: You do not have the necessary permissions.
```

## Support
If you have any questions feel free to [submit an issue on this repository](https://github.com/api-commons/problem-details-for-http-apis/issues/new), engage [directly with the IETF RFC](https://datatracker.ietf.org/doc/html/rfc7807), or contact [Bump.sh](https://bump.sh/) about their Train Travel API and services.
