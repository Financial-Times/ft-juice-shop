# Open Redirect
An open redirect happens when a web server has been designed so that it is possible to take an untrusted user input in the form of a URL, such as google.com so that when a customer visits a page crated by the hacker they are redirected to another site.

## Helpful to have knowledge of:
* HTTP
* HTML


## Commonly found in

This is often used in authentication mechanisms, but there can sometimes be a well intentioned purpose in most web apps

## How would this attack be discovered?

With this one you could guess. You could also use a tool, such as [OpenRedireX](https://github.com/devanshbatham/OpenRedireX), to scan for these. You could also look through each page, looking for interesting things like links that look like redirects.

## To find yourself

In this case because it's a _modern_ web app, meaning everything is in JavaScript, you can use the Developer Mode of your browser and search for redirect.

### To use in Fubble Tea

You can exploit it by following this link:

https://ft-juice-shop-b80cdbae3ec6.herokuapp.com/redirect?to=https://www.google.com

### What happens?

You can see that despite the link referencing Fubble Tea you actually end up at Google

## So What?

This can be used as part of a phishing campaign.

If a hacker found an open redirect on ft.com they could craft their own page that looks like the ft.com login page that when you enter your username and password stores that somewhere the hacker can access.

They could then send you an e-mail saying "Please see this article" that has a genuine ft.com link but redirects to their fake login page. It looks genuine because it looks like ft.com and you may not notice that the URL has changed to the fake login page.

If you entered your details in there then the hacker would have access to your account.

## How to fix it?

If you do still need a redirecting service they don't need to redirect to the whole internet, and can be limited to just ft.com and maybe a few others.

## How common is it?

We get a couple of these a month on HackerOne

# Cross Site Scripting (XSS)

Cross Site Scripting occurs when a web service has been designed so that it is possible to take untrusted user input and put that on the page.

## Helpful to have knowledge of
* HTML
* JavaScript

## Commonly found in
In comments sections, or search queries.

## How would this be discovered?

Someone could manually use the special text below entering it in any search field or comment field to see if it did anything. There are tools that also help automate this such as [NucleiFuzzer](https://github.com/0xKayala/NucleiFuzzer).

```html
<iframe src="javascript:alert(`xss`)">
```

### To use in Fubble Tea

Copy that value in to the search field and press enter

### What happens?

The search page is loaded and a popup appears saying XSS

## So What?

With sufficient knowledge of JavaScript this can be used to get access to user cookies, such as the FT session cookie, and send them off to another server where a hacker could then use that for themselves and access whatever the user had access to. Their personal details, their subscription information.

This case is known as a Reflected Cross Site Scripting attack. You can see in the URL on the search page, contains the iframe text that was copied in.

This means an attacker could send that link to someone else and if they convinced them to click it, could take their session cookies for themselves.

## How to fix it?

Fortunately most modern web frameworks already account for this by default and you have to put effort in to make it happen.

Usually it's not complex fix to. You must validate all user input and [replace all `<` and `>` with HTML encoded characters in the user submitted information](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html#output-encoding-for-html-contexts).

## How common is it?
Number 1 in the list of reports FT got from HackerOne in 2023.

## Notes

* [Technical Details](https://owasp.org/www-community/attacks/xss/)
* There are [mulitple kinds of Cross Site Scripting](https://owasp.org/www-community/Types_of_Cross-Site_Scripting)

# Cross Site Request Forgery (CSRF)

Cross Site Request Forgery occurs when a web service doesn't verify data sent to it. On most web services you will have a form which allows you to submit data, such as a name, or e-mail address. But this form can be submitted from anywhere, not just the site you are on. Combined with some JavaScript it is possible to craft a page that changes your data just by visiting it.

## Helpful to have knowledge of
* HTML
* HTTP

## Commonly found in
It's not very common to see this these days.

## How would this be discovered?

This is closely related to how you prevent it from happening.

By looking at the HTML of a page you can find `<form` tags. If they don't contain a CSRF field with a unique code, it will likely be liable to CSRF.

### To use in Fubble Tea

By submitting this form you can change your own username if you are logged in.

<form action="https://ft-juice-shop-b80cdbae3ec6.herokuapp.com/profile" method="POST" target="_blank">
  <input placeholder="username" name="username" value="CSRF"/>
  <button type="submit">Submit</button>
</form>

## So What?

The real attack scenario is to share a page containing this that automatically submits new values, such as an hacker e-mail address, then convincing the target to go to that page. When their e-mail address has changed the hacker can then take-over the account.

## How to fix it?

Fortunately most modern web frameworks add CSRF protection to a form by default and you would have to put in work to stop that from happening. If it doesn't then there's likely a library that will add it for you.

## How common is it?

As mentioned above this is handled automatically by most modern web frameworks and is a lot less common than it used to be. But it's still worth being aware of what is possible.

# Server Side Request Forgery (SSRF)
Server Side Request Forgery is similar to Cross Site Scripting above, but instead of the alert appearing on the page, it makes the server do something instead. Because of this it's usually more impactful than XSS and CSRF.

# RCE


# Sub-domain Take-over