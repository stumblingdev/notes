## HTML Injection
Allowing the attacker to insert arbitrary HTML values and rendering them to the page as is. Mainly it changes how the page actually looks as this is what the purpose of the HTML usually is on the web page.

### Examples
#### 1. Coinbase
An attacker was able to send a URI Encoded string which coinbase parsed and rendered the corresponding letters.

However if the attacker entered `<h1>hello world</h1>`, this would be rendered as plain text.

Takeaways
* Check how the website looks for encoded and plain text.
* Check to see if any encoded values are rendered as their original form. eg `%2F becomes /`


#### 2. Within Security Content Spoofing
On the login page of within security, now merged with hackerone, on authentication error you'd see `access_denied` error which was rendered from URL parameter `error`. If you simply entered anything else there, it'd be rendered as is.

A bounty of $250 was paid for this, however this is a fairly small issue and most likely require some social engineering to actually exploit in the real world.

Takeaways
* Lookout for content rendered directly from the URL

### Summary
HTML injection is generally used to trick users into submit data or click links that they shouldn't.

You should lookout for how the content is rendered back to you, not just plain text but HTML, Encoded URI, HTML Encoded etc.
