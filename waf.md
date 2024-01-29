# AWS Web Application Firewall (WAF)

## TLDR
WAF is a service that lets you define a set of rules to filter traffic to your web applications. It can be used to block common attacks. The rules can be defined by you or in a set of predefined rules.

## OWASP Top 10
OWASP is a non-profit organization that provides a list of the most critical security risks to web applications. The list is updated every 3 years. The latest version is from 2021. The current list is available at [https://owasp.org/Top10/](https://owasp.org/Top10/).

## Open Source WAF
There are several open source WAFs available. One of he most popular is: [ModSecurity](https://modsecurity.org/) - it can be used as a standalone application or as a module for Apache, Nginx, and IIS.

## AWS WAF
[AWS WAF](https://docs.aws.amazon.com/waf/) can protect following resources:
- Amazon API Gateway REST API
- Application Load Balancer
- AWS AppSync GraphQL API
- Amazon Cognito user pool
- AWS App Runner service
- AWS Verified Access instance
- CloudFront distribution

## AWS WAF Rules
AWS WAF Rules are defined in [Web ACLs](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl.html). A Web ACL is a collection of rules that AWS WAF uses to inspect web requests. Each rule has a priority, and AWS WAF evaluates the rules in order, starting with the lowest priority. The first rule that matches the request determines the action that AWS WAF takes. AWS WAF Rules can have following actions:
- Allow
- Block
- Count

## AWS WAF Managed Rules
AWS WAF Managed Rules are predefined rules that you can use to protect your web applications.

## AWS WAF Pricing
AWS WAF is priced in few dimensions - see [AWS WAF Pricing](https://aws.amazon.com/waf/pricing/).

## AWS Shield and AWS Shield Advanced
AWS Shield is a managed DDoS protection service that safeguards applications running on AWS. AWS Shield Advanced is a paid service that provides additional protection against DDoS attacks.
See comparision here: [AWS Shield Standard and AWS Shield Advanced](https://aws.amazon.com/shield/faqs/).


## Tasks
1. Create WAF rules to protect against OWASP Top 10 attacks.
2. Create Cloud Front distribution and attach WAF to it.
3. Test the WAF rules using [nikto](https://github.com/sullo/nikto) tool.
```
git clone https://github.com/sullo/nikto
cd nikto
docker build -t sullo/nikto .
docker run --rm -v $(pwd):/tmp sullo/nikto \
    -useragent "UAM WMI Lab" -D V -Tuning 9 -ssl -h https://<distribution-id>.cloudfront.net/ \
    -o /tmp/report.txt
```
