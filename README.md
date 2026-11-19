# Future Interns | Cyber Security Task 3

## API Security Risk Analysis

**Candidate:** QUSAY ALHEMIRY  
**Internship:** Future Interns - Cyber Security (November 2026)  
**Assessment date:** 18 November 2026  
**Target:** [JSONPlaceholder](https://jsonplaceholder.typicode.com/)  
**Deliverable:** [Six-page API Security Risk Analysis Report](API_Security_Risk_Analysis_Report.pdf)

## Objective and scope
Assess a documented, public REST API using read-only requests and evaluate authentication, response data, object-level access, tokens, rate limiting, input handling and security configuration. The API serves synthetic public data and does not require registration or an API key. No private data exposure or confirmed high-severity vulnerability was established.

## Tools
- Postman Desktop: manual GET requests, No Auth setting, response bodies and headers, one Origin header inspection.
- Web browser: official JSONPlaceholder documentation.
- OWASP API Security Top 10 (2023): reference categories.
- PDF reporting and original screenshots: evidence and recommendations.

## Methodology
1. Read the official API documentation and establish expected public access.
2. Send manual GET requests to the five documented or deliberately missing public resource paths below.
3. Inspect response status, JSON fields, and request/response headers; compare one illustrative Origin request.
4. Review configuration, rate-limit metadata and error handling without exploitation or load testing.
5. Distinguish observed behavior, conditional production risks and controls that were not assessable.
6. Document business impact, remediation and evidence.

## Observed requests
| Method | Path | Status | Response time (captured) |
|---|---|---|---|
| GET | `/posts/1` | 200 | 581 ms (baseline) |
| GET | `/users/1` | 200 | 517 ms |
| GET | `/users/2` | 200 | 511 ms |
| GET | `/users/abc` | 404 | 498 ms |
| GET | `/users/999` | 404 | 482 ms |

The additional Origin request to `/users/1` returned 200 and `Access-Control-Allow-Origin: https://example.com`, with `Access-Control-Allow-Credentials: true`. These are observed headers, not proof of an exploitable credentialed CORS weakness.

## Risk summary
| ID | Area | Rating / qualification |
|---|---|---|
| API-01 | Unauthenticated access | High in a private-data scenario; expected on the demo |
| API-02 | Excessive data exposure | High if unnecessary, sensitive fields are exposed; demo fields are synthetic |
| API-05 | Object-level authorization | High if private cross-account access occurs; not established |
| API-04 | CORS | High if sensitive authenticated responses are exposed; one origin reflected |
| API-07 | Rate limiting | Medium conditional risk; enforcement and window unverified |
| API-03 | Technology disclosure | Low; Express framework name observed, no version |
| API-08 | HSTS | Low hardening observation; not visible in sampled responses |
| API-06 | Input validation / errors | No issue observed in two 404 bodies |
| API-09 | Authentication tokens | Not assessed; public API needs no token |

High/Medium scenario ratings are qualitative and conditional, not CVSS scores or confirmed findings against JSONPlaceholder. Recommendations include authentication and object/property authorization for private endpoints, data minimization, appropriate quotas, restrictive credentialed CORS, HSTS when HTTPS-ready, and generic error handling.

## Evidence index
Original screenshots were captured during the training session. Selected figures in the PDF are cropped solely for legibility. No new requests were sent to prepare this report.

- [E01_API_Public_Scope.png](Evidence/E01_API_Public_Scope.png) - Official homepage: fake API, public access.
- [E02_API_Documentation.png](Evidence/E02_API_Documentation.png) - Guide: public resources and GET response.
- [E03_Posts_Baseline.png](Evidence/E03_Posts_Baseline.png) - Baseline GET /posts/1 with response.
- [E04_Posts_No_Auth.png](Evidence/E04_Posts_No_Auth.png) - No Auth and successful /posts/1 response.
- [E05_User1_Profile.png](Evidence/E05_User1_Profile.png) - No Auth /users/1, profile fields.
- [E06_User1_Profile_Continued.png](Evidence/E06_User1_Profile_Continued.png) - Remaining user 1 response fields.
- [E07_Response_Headers_Cache.png](Evidence/E07_Response_Headers_Cache.png) - Response headers: content type, cache and credentials.
- [E08_Response_Headers_Rate_Limit.png](Evidence/E08_Response_Headers_Rate_Limit.png) - Response headers: Express, nosniff and rate-limit metadata.
- [E09_Response_Headers_Server.png](Evidence/E09_Response_Headers_Server.png) - Response headers: Cloudflare and cache HIT.
- [E10_CORS_Origin_Response.png](Evidence/E10_CORS_Origin_Response.png) - CORS response reflecting example.com.
- [E11_CORS_Response_Headers.png](Evidence/E11_CORS_Response_Headers.png) - Additional CORS response headers.
- [E12_CORS_Response_Server.png](Evidence/E12_CORS_Response_Server.png) - Remaining CORS response headers.
- [E13_User2_Profile.png](Evidence/E13_User2_Profile.png) - Returned public user 2 object and 200 status.
- [E14_User2_Profile_Continued.png](Evidence/E14_User2_Profile_Continued.png) - Remaining user 2 fields.
- [E15_Invalid_Text_ID.png](Evidence/E15_Invalid_Text_ID.png) - Non-numeric ID, 404 and empty body.
- [E16_Nonexistent_Numeric_ID.png](Evidence/E16_Nonexistent_Numeric_ID.png) - Missing numeric ID, 404 and empty body.
- [E17_Error_Response_Headers.png](Evidence/E17_Error_Response_Headers.png) - 404 response headers, cache HIT and rate-limit metadata.
- [E18_Request_Headers.png](Evidence/E18_Request_Headers.png) - Request header configuration with auto-generated headers.

## Limitations and ethics
- No exploitation, credential bypass, private endpoint testing, write operations, flooding or load tests.
- Public record access does not establish BOLA; authenticated ownership and role boundaries were unavailable.
- Only one Origin value was inspected; no authenticated browser session was tested.
- Rate-limit headers were observed in cached responses; no rate-limit enforcement or reset interval was verified.
- No actual authentication token lifecycle was assessable. Postman-Token is not an API authentication credential. The last captured header row had no key; it does not establish an active Origin header.
- The 404 bodies did not disclose sensitive internals in the two tested cases; comprehensive input validation was not established. The Postman test script was not reviewed.
- HSTS was not visible in the sampled responses; no site-wide claim is made.

## References
- Future Interns task: https://futureinterns.com/cyber-security-task-3-2026/
- JSONPlaceholder: https://jsonplaceholder.typicode.com/
- JSONPlaceholder Guide: https://jsonplaceholder.typicode.com/guide/
- OWASP API Security Top 10 (2023): https://api-security.owasp.org/editions/2023/en/0x11-t10/
- MDN CORS configuration: https://developer.mozilla.org/en-US/docs/Web/Security/Practical_implementation_guides/CORS
- MDN HSTS: https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Strict-Transport-Security

This is an educational assessment, not an independent penetration test or a certification of the API's security.
