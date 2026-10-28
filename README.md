# Nuclei workflow filtering issue

Hardcoded target for all templates is a http server running on localhost:8000.

To reproduce the issue:

```bash
git clone https://github.com/mmuslija/nuclei-workflow-filter-issue.git
cd nuclei-workflow-filter-issue
NUCLEI_TEMPLATES_DIR=$(pwd) nuclei -duc -debug -u ignored -w workflow.yaml -id L1
```

In the applied filter `-id L1`, `L1` is the ID of template [leaf_1.yaml].

Based on defined steps in [workflow.yaml], expected behavior is for nuclei to load and run templates:
- `branch_A.yaml` because it needs to match in order to run `leaf_1.yaml`.
- `leaf_1.yaml` because it satisfies the filter.

Instead, if you observe the output of the command above, you'll notice that nuclei runs:
- `branch_A.yaml`
- `leaf_1.yaml`
- `branch_B.yaml`
- `branch_C.yaml`
- `branch_C_A.yaml`


Nuclei output:

```
$ NUCLEI_TEMPLATES_DIR=$(pwd) ./nuclei-3.11.1 -duc -debug -u ignored -w workflow.yaml -id L1

                     __     _
   ____  __  _______/ /__  (_)
  / __ \/ / / / ___/ / _ \/ /
 / / / / /_/ / /__/ /  __/ /
/_/ /_/\__,_/\___/_/\___/_/   v3.11.1

                projectdiscovery.io

[url] got empty hostname for ignored skipping ip selection
[INF] Current nuclei version: v3.11.1 (unknown) - remove '-duc' flag to enable update checks
[INF] Current nuclei-templates version: v10.5.1 (unknown) - remove '-duc' flag to enable update checks
[WRN] Scan results upload to cloud is disabled.
[INF] Workflows loaded for current scan: 1
[WRN] Loading 5 unsigned templates for scan. Use with caution.
[INF] Targets loaded for current scan: 1
[INF] Running httpx on input host
[INF] Found 0 URL from httpx
[INF] [BA] Dumped HTTP request for http://localhost:8000/A

GET /A HTTP/1.1
Host: localhost:8000
User-Agent: Mozilla/5.0 (Fedora; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.0.0 Safari/537.36
Accept: */*
Accept-Language: en
Accept-Encoding: gzip

[DBG] [BA] Dumped HTTP response http://localhost:8000/A

HTTP/1.0 404 File not found
Content-Length: 460
Connection: close
Content-Type: text/html;charset=utf-8
Date: Wed, 28 Oct 2026 13:56:51 GMT
Server: SimpleHTTP/0.6 Python/3.14.7

<!DOCTYPE HTML>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <style type="text/css">
            :root {
                color-scheme: light dark;
            }
        </style>
        <title>Error response</title>
    </head>
    <body>
        <h1>Error response</h1>
        <p>Error code: 404</p>
        <p>Message: File not found.</p>
        <p>Error code explanation: 404 - Nothing matches the given URI.</p>
    </body>
</html>
[INF] [L1] Dumped HTTP request for http://localhost:8000/1

GET /1 HTTP/1.1
Host: localhost:8000
User-Agent: Mozilla/5.0 (SS; Linux i686) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/140.0.0.0 Safari/537.36
Accept: */*
Accept-Language: en
Accept-Encoding: gzip

[DBG] [L1] Dumped HTTP response http://localhost:8000/1

HTTP/1.0 404 File not found
Content-Length: 460
Connection: close
Content-Type: text/html;charset=utf-8
Date: Wed, 28 Oct 2026 13:56:51 GMT
Server: SimpleHTTP/0.6 Python/3.14.7

<!DOCTYPE HTML>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <style type="text/css">
            :root {
                color-scheme: light dark;
            }
        </style>
        <title>Error response</title>
    </head>
    <body>
        <h1>Error response</h1>
        <p>Error code: 404</p>
        <p>Message: File not found.</p>
        <p>Error code explanation: 404 - Nothing matches the given URI.</p>
    </body>
</html>
[L1:status-1] [http] [low] http://localhost:8000/1
[INF] [BB] Dumped HTTP request for http://localhost:8000/B

GET /B HTTP/1.1
Host: localhost:8000
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:1.9.7.20) Gecko/ Firefox/3.6.15
Accept: */*
Accept-Language: en
Accept-Encoding: gzip

[DBG] [BB] Dumped HTTP response http://localhost:8000/B

HTTP/1.0 404 File not found
Content-Length: 460
Connection: close
Content-Type: text/html;charset=utf-8
Date: Wed, 28 Oct 2026 13:56:51 GMT
Server: SimpleHTTP/0.6 Python/3.14.7

<!DOCTYPE HTML>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <style type="text/css">
            :root {
                color-scheme: light dark;
            }
        </style>
        <title>Error response</title>
    </head>
    <body>
        <h1>Error response</h1>
        <p>Error code: 404</p>
        <p>Message: File not found.</p>
        <p>Error code explanation: 404 - Nothing matches the given URI.</p>
    </body>
</html>
[INF] [BC] Dumped HTTP request for http://localhost:8000/C

GET /C HTTP/1.1
Host: localhost:8000
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/133.0.0.0 Safari/537.36
Accept: */*
Accept-Language: en
Accept-Encoding: gzip

[DBG] [BC] Dumped HTTP response http://localhost:8000/C

HTTP/1.0 404 File not found
Content-Length: 460
Connection: close
Content-Type: text/html;charset=utf-8
Date: Wed, 28 Oct 2026 13:56:51 GMT
Server: SimpleHTTP/0.6 Python/3.14.7

<!DOCTYPE HTML>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <style type="text/css">
            :root {
                color-scheme: light dark;
            }
        </style>
        <title>Error response</title>
    </head>
    <body>
        <h1>Error response</h1>
        <p>Error code: 404</p>
        <p>Message: File not found.</p>
        <p>Error code explanation: 404 - Nothing matches the given URI.</p>
    </body>
</html>
[INF] [BC_A] Dumped HTTP request for http://localhost:8000/C_A

GET /C_A HTTP/1.1
Host: localhost:8000
User-Agent: Mozilla/5.0 (Debian; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/141.0.0.0 Safari/537.36
Accept: */*
Accept-Language: en
Accept-Encoding: gzip

[DBG] [BC_A] Dumped HTTP response http://localhost:8000/C_A

HTTP/1.0 404 File not found
Content-Length: 460
Connection: close
Content-Type: text/html;charset=utf-8
Date: Wed, 28 Oct 2026 13:56:51 GMT
Server: SimpleHTTP/0.6 Python/3.14.7

<!DOCTYPE HTML>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <style type="text/css">
            :root {
                color-scheme: light dark;
            }
        </style>
        <title>Error response</title>
    </head>
    <body>
        <h1>Error response</h1>
        <p>Error code: 404</p>
        <p>Message: File not found.</p>
        <p>Error code explanation: 404 - Nothing matches the given URI.</p>
    </body>
</html>
[INF] Scan completed in 12.052906ms. 1 matches found.
[INF] HTTP connections: 5 total, 5 new, 0 reused (0.0%)
```

Python server logs:

```
$ python3 -m http.server 8000
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
127.0.0.1 - - [28/Oct/2026 14:56:51] code 404, message File not found
127.0.0.1 - - [28/Oct/2026 14:56:51] "GET /A HTTP/1.1" 404 -
127.0.0.1 - - [28/Oct/2026 14:56:51] code 404, message File not found
127.0.0.1 - - [28/Oct/2026 14:56:51] "GET /1 HTTP/1.1" 404 -
127.0.0.1 - - [28/Oct/2026 14:56:51] code 404, message File not found
127.0.0.1 - - [28/Oct/2026 14:56:51] "GET /B HTTP/1.1" 404 -
127.0.0.1 - - [28/Oct/2026 14:56:51] code 404, message File not found
127.0.0.1 - - [28/Oct/2026 14:56:51] "GET /C HTTP/1.1" 404 -
127.0.0.1 - - [28/Oct/2026 14:56:51] code 404, message File not found
127.0.0.1 - - [28/Oct/2026 14:56:51] "GET /C_A HTTP/1.1" 404 -
```

This is because nuclei automatically includes workflow templates containing subtemplates in the run, without checking whether any of the subtemplates match the applied filters.

