[[Cookie and Session drawing]]
Same Site prevents CSRF vulnerability

A cross-site request forgery (CSRF) attack occurs when an attacker can make a target’s browser send an HTTP request to another website. That website then performs an action as though the request were valid and sent by the target. 

Such an attack typically relies on the target being previously authenticated on the vulnerable website where the action is submitted and occurs without the target’s knowledge. 

When a CSRF attack is successful, the attacker is able to modify server-side information and might even take over a user’s account.
## Basic CSRF Example

1. Bob logs into his banking website to check his balance.
2. When he’s finished, Bob checks his email account on a different domain.
3. Bob has an email with a link to an unfamiliar website and clicks the link to see where it leads.
4. When loaded, the unfamiliar site instructs Bob’s browser to make an HTTP request to Bob’s banking website, requesting a money transfer from his account to the attacker’s.
5. Bob’s banking website receives the HTTP request initiated from the unfamiliar (and malicious) website. But because the banking website doesn’t have any CSRF protections, it processes the transfer.

---
## CSRF with GET Requests

Done using `<img>` tag and specifying a URL in `src` attribute:
`<img src="https://www.bank.com/transfer?from=bob&to=joe&amount=500">`

To avoid this vulnerability, developers should never use HTTP `GET` requests to perform any backend data-modifying requests, such as transferring money. But any request that is read-only should be safe. 

Many common web frameworks used to build websites, such as Ruby on Rails, Django, and so on, will expect developers to follow this principle, and so they’ll automatically add CSRF protections to `POST` requests but not `GET` requests.

---
## CSRF with POST Requests

`<img>` tag can't invoke a `POST` request

In this situation, it’s possible for a malicious site to create a hidden HTML form and submit it silently to the vulnerable site without the target’s knowledge.

---
## Related links

Interesting [video](https://youtu.be/CKHai0OW6BY) about CSRF by mdisec