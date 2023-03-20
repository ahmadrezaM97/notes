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
This is __Same-Origin-policy__




```ad-tip
IS an HTTP- header based mechanism that allows a server to indicate any origins( domain, schema or port) other than its own from which a brwoser should permit loading resources.

```


_____
##### References
1.

