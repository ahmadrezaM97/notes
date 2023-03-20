# CORS
Created: 2023-03-21 00:38
Tags: 
____

#### Cross origin resource sharing

It allows you to make requests from one website to another website in the browser, which is normally prohibited by another browser policy called the Same-Origin-Policy(SOP)

both CROS and SOP are developed in response to issues of browser policy.

__Same-Origin-Policy__ is meant to address __cross-site request forgery(CSRF)__

Before browsers implemented __SOP__, malicious website were able to exploit cookies. stored by your browser to make unauthorized request to other domains. Some of these unauthorized requests could do things like make purchases, delete user information, fetch sensitive data, etc.

#### Example

you might go to a banking website and provide some credentials to log into you account.
you username is stored in a secure browser cookie for a certain period of time so the bank can tell you are still logged in instead on having you login another time with each page you access.

Later on, you go to an innocent-seeming webpage listing some fact about bunnies. The webpage's HTML also has a little JS to make [[AJEX]] to the previous banking website to make a wire transaction to another acount.
Because your session is still authenticated with the cooki that was stored earlier, the banking web site thinks it's just you clicking on a lik on theri site to submit a wire transaction.


The easy fix was for browser to detect when a request is made from one website to another and prevent the response from being readable. 
This is __Same-Origin-policy__.
‌
BUT, what about loading images from other websites?
Unlike AJAX requests, browsers do allow embedding most content from other website, such as images, script tags, css, etc by default.
Some possible exceptions are __iframes__ depending on the sit's configuration, and fonts, depending on browser.

AND, this does leave an avenue of attack open for __CSRF__ attacks, since could use `<image src='...'/>` to make a malicious `GET` request to another website rather than loading an image.
Servers have to do some additional work on their end to implement security, by using [[CSRF TOKENS]] [[Transaction Tickets]] if GET requests can result in any unwanted side effects or revealing confidential information. 

If you are developing an API and you want to allow people to make AJAX or newer web APIs like Fetch requests to your API in the browser, Or more simply, you want to make sure anyone can access a font you are sharing from you website, regardless of their browser.

Here, We've arrived at __`CORS`__

__`CORS`__ allows servers to specify certain trusted `origin` they  are willing to permit request from.
Origins are defined as the combination of protocol (HTTP, HTTPS), host and port.

Browser which implement the `CORS` policy will include a HTTP header called `Origin` in request made with AJAX, including above information.

#### Simple requests

__GET, HEAD or POST without any special HTTP headers__

to instruct the browser to expose server responses to a HTTP requests from certain origin, the web server must respond to the request with an additional HTTP response header.`Access-Control-Allow-Origin: <origin>`.
Alternatively the web server my expose it's respones to all origins by specifying a value `Aceess-Control-Allow-Origin:**`

### Preflight request

__DELETE or PUT or (HEAD, GET or POST with special header)___
The browser will send "preflight" request to find out the `CORS` result prior to sending the actual request.

The preflight request is an __OPTIONS__ request made to the same HTTP path as the request, with a couple of HTTP headers.

* __Origin__ the 



```ad-tip
IS an HTTP- header based mechanism that allows a server to indicate any origins( domain, schema or port) other than its own from which a brwoser should permit loading resources.

```


_____
##### References
1.

