# Vulnerability Assessment Report – Live Website

![HTTP Login Page](screenshots/http_login_page.png)

## Future Interns Cyber Security Internship

**Prepared by:** Keerit Kapoor
**Task:** Task 1 – Vulnerability Assessment Report
**Assessment date:** 20 June 2026
**Target:** `http://testaspnet.vulnweb.com/login.aspx`

---

## Objective

Conduct a passive vulnerability assessment of a public security-training web application and document observable security risks, their potential impact, and recommended remediation steps.

> **Ethical Note:** This assessment was limited to passive observation using the browser and Developer Tools. No exploitation, credential testing, SQL injection, brute forcing, or other intrusive testing was performed.

---

## Scope

| Item            | Details                                                                                      |
| --------------- | -------------------------------------------------------------------------------------------- |
| Target URL      | `http://testaspnet.vulnweb.com/login.aspx`                                                   |
| Assessment Type | Passive vulnerability assessment                                                             |
| In Scope        | Login page, HTTP transport, visible response headers                                         |
| Out of Scope    | Exploitation, authentication testing, SQL injection, XSS, brute force, and data modification |

---

## Tools Used

* **Browser Developer Tools** – inspected network requests and HTTP response headers
* **Manual Browser Review** – reviewed the login page and transport security indicator

---

## Methodology

1. Opened the target login page in a browser.
2. Confirmed whether the page used HTTP or HTTPS.
3. Used Browser Developer Tools → Network to inspect the document request.
4. Reviewed visible response headers for unnecessary technology disclosure.
5. Classified risks based on potential confidentiality, integrity, and user-security impact.

---

## Executive Summary

The assessment identified two observable security issues:

* The login page is available over unencrypted HTTP.
* The server response exposes web-server and framework version information.

The most significant issue is the use of HTTP on a page containing username and password fields. If real users entered credentials on an untrusted network, the information could potentially be exposed or modified in transit.

---

## Findings Summary

| ID   | Finding                                           | Severity | Status |
| ---- | ------------------------------------------------- | -------: | ------ |
| F-01 | Sensitive Login Page Served Over Unencrypted HTTP |     High | Open   |
| F-02 | Server and Framework Information Disclosure       |      Low | Open   |

---

# F-01: Sensitive Login Page Served Over Unencrypted HTTP

**Severity:** High
**Affected URL:** `http://testaspnet.vulnweb.com/login.aspx`

## Description

The target login page is served using HTTP rather than HTTPS. The browser displays a **“Not secure”** indicator while the page contains fields for a username and password.

HTTP does not encrypt traffic between the user’s browser and the server.

## Evidence

![HTTP Login Page Evidence](screenshots/http_login_page.png)

## Potential Impact

* Usernames and passwords may be exposed on untrusted networks.
* An attacker positioned on the network may be able to alter traffic in transit.
* Users may lose trust in the website due to the browser security warning.

## Recommendation

* Enforce HTTPS for the entire application, especially all authentication-related pages.
* Redirect all HTTP requests to HTTPS.
* Configure HTTP Strict Transport Security (HSTS).
* Mark all session cookies as `Secure`.

---

# F-02: Server and Framework Information Disclosure

**Severity:** Low
**Affected URL:** `http://testaspnet.vulnweb.com/login.aspx`

## Description

The HTTP response headers reveal technology and version details, including:

* `Server: Microsoft-IIS/8.5`
* `X-AspNet-Version: 2.0.50727`
* `X-Powered-By: ASP.NET`

This information can help an attacker identify the technologies used by the application during reconnaissance.

## Evidence

![Response Header Evidence](screenshots/response_headers.png)

## Potential Impact

* Makes technology fingerprinting easier.
* May help attackers research publicly known weaknesses related to disclosed software versions.
* Provides unnecessary detail that does not need to be exposed to users.

## Recommendation

* Remove or suppress unnecessary response headers.
* Avoid exposing detailed server and framework versions.
* Keep the web server and application framework updated.
* Review server configuration regularly for information leakage.

---

## Risk Rating Guide

| Severity | Meaning                                                                           |
| -------- | --------------------------------------------------------------------------------- |
| High     | A weakness that could significantly affect user security or sensitive information |
| Medium   | A weakness that may create meaningful risk under certain conditions               |
| Low      | A weakness that provides limited exposure but should still be addressed           |

---

## Conclusion

This passive assessment identified an important transport-security issue on the login page and a low-severity information-disclosure issue in the response headers.

The highest priority should be migrating the login page and all sensitive application traffic to HTTPS. Removing unnecessary technology headers would further reduce the amount of information exposed to potential attackers.

---

## Repository Contents

```text
FUTURE_CS_01/
│
├── README.md
├── Vulnerability_Assessment_Report_Keerit_Kapoor.pdf
│
└── screenshots/
    ├── http_login_page.png
    └── response_headers.png
```

---

## Skills Demonstrated

* Passive web security assessment
* HTTP response-header analysis
* Risk classification
* Vulnerability reporting
* Security remediation planning
* GitHub portfolio documentation

---

## Disclaimer

This repository is created solely for educational and internship-portfolio purposes. The target used is a public security-testing demonstration environment. No intrusive testing or exploitation was performed.
