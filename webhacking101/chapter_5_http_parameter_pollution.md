## HTTP Parameter Pollution
There are two types of HTTP Parameter Pollution. Backend and Frontend.

### Backend HTTP Parameter Pollution 
Imagine an application that does wire transfer via a POST request with the following params.

```
fromAccount=<accNumber>&toAccount=<accNumber>&amount=<amount>
```

The backend itself makes a POST call to a third party that actually does the transfer with the same parameters.

Now, if a site only accepts the last parameters in case of duplicates, someone can alter the `toAccount` attribute to transfer money into their account.

```
amount=<amount>&fromAccount=<accNumber>&toAccount=<accNumber>&toAccount=<hackers acc number>
```

### Frontend HTTP Parameter Pollution
The way that duplicate parameters are handled by the application is highly dependant on the language and the framework the site is implemented in.

Imagine a code snippet that generates a link on the frontend.

```html
<a href="/page?action=view&id=${id}">View Page</a>
```

A hacker can send in the value of `id` as `1&action=edit`.

If the application is vulnerable to HPP attacks, it'll render a "edit" view.


### Examples

#### 1. Hackerone Social Media Sharing
Hackerone allows user to share stuff on various different social media websites. When sharing, it sets up a few parameters by default for the user.

A user was able to tack on duplicate parameters and take over the message.

```bash
https://www.facebook.com/sharer.php?u=https://hackerone.com/blog/introducing-signal?&u=https://vk.com/durov
```

Here the hacker is able to add another `u` parameter, which takes presedence.

Takeaways
* Be on the lookout for such social share functionality.

#### 2. Twitter Unsubscribe
A hacker was able to find the `UID` parameter being passed in the Unsubscribe link to twitter. He tried changing it, twitter returned an error. He tried adding another UID param and with another user's UID and was able to successfully unsubscribe them.

Takeaways
* Look for UID or ID params in the URL

Another thing like this was in embeded tweets or functionality that twitter offers where you can embed an element (tweet, user profilet etc.) on any page and have an authenticated user interact with it.

Again this was vulnerable to HPP as well and if you created a `Profile` element with a duplicate parameter with another user's UID, folks would essentially follow the other guy.

*_The point is that this reflects a systematic issue where devs aren't checking for HPPs. Once you've identified it, look for more of such vulernabilities_*

### Summary
Impact of HPP is depends on what the application does with the passed parameters. Finding this require a fair bit of trial and error, especially the backend HPP, since backend is often a complete black box to the hacker.

Links are a good place to look for these.
