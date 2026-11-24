# Lab Reproduction and Validation Checklist

This checklist helps another learner reproduce the documented OWASP Juice Shop security-header assessment in an isolated local lab.

## Scope and safety

- [ ] Use only a Juice Shop instance that you own or are explicitly authorized to test.
- [ ] Keep the target bound to localhost (the example below uses `127.0.0.1:3000`).
- [ ] Do not substitute a public website, third-party IP, or production service as the target.
- [ ] Record the date, tool versions, target URL, and any configuration changes.

## 1. Start the training application

Start OWASP Juice Shop using the official Docker image:

```bash
docker run --rm -d --name juice-shop -p 127.0.0.1:3000:3000 bkimminich/juice-shop
```

Verify that the container is running:

```bash
docker ps --filter name=juice-shop
curl -I http://127.0.0.1:3000
```

Expected result: the container is listed as running and the HTTP request receives a response. The exact status and headers can vary by application version.

## 2. Confirm the local service

Run a narrowly scoped scan against the local lab only:

```bash
nmap -sV -p 3000 127.0.0.1
```

Record the detected port state and service information. Do not broaden the target range.

## 3. Inspect HTTP response headers

Capture the response headers:

```bash
curl -sS -D response-headers.txt -o /dev/null http://127.0.0.1:3000
cat response-headers.txt
```

Check whether the response includes relevant defensive headers, such as:

- `Content-Security-Policy`
- `X-Content-Type-Options`
- `X-Frame-Options` or an appropriate `frame-ancestors` CSP directive

A missing header is a configuration observation, not proof by itself that an exploitable vulnerability exists.

## 4. Run a passive ZAP assessment

1. Open OWASP ZAP.
2. Set the target to `http://127.0.0.1:3000`.
3. Browse the application through ZAP or use its passive scanning workflow.
4. Review the Alerts panel and retain only findings relevant to the assessment scope.
5. Save the report and supporting evidence.

Avoid active attacks or fuzzing unless they are separately authorized and necessary for the lab exercise.

## 5. Validate and document each finding

For every reported issue, complete the following:

- [ ] Finding title and affected response/endpoint
- [ ] Evidence (sanitized request/response excerpt or screenshot)
- [ ] Risk and confidence rating, with a short justification
- [ ] Potential impact and realistic limitations
- [ ] Recommended mitigation
- [ ] Retest result after applying a fix, if a fix was tested

Do not claim a mitigation is verified unless you actually changed the configuration and repeated the relevant check.

## 6. Close the lab

Stop and remove the temporary container:

```bash
docker stop juice-shop
```

## Interpretation notes

- Security-header recommendations must be adapted to the application's real requirements. For example, a restrictive Content Security Policy can break legitimate scripts, styles, or integrations if deployed without testing.
- Browser protections and framework defaults may change between Juice Shop releases, so findings should be rechecked against the exact version used.
- Keep evidence limited to the authorized lab and remove secrets, session cookies, tokens, and personal data before publishing.

## References

- [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/)
- [OWASP ZAP Documentation](https://www.zaproxy.org/docs/)
- [Nmap Reference Guide](https://nmap.org/book/man.html)
