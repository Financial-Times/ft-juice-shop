# Open Redirect

## Helpful to have knowledge of:
* HTTP
* HTML

## Description
An open redirect happens when a web server has been designed so that it is possible to take an untrusted user input in the form of a URL, such as google.com so that when a customer visits a page crated by the hacker they are redirected to another site.

## Commonly found

This is often used in authentication mechanisms

## How would this attack be discovered?

With this one you could guess. You could also use a tool, such as [OpenRedireX](https://github.com/devanshbatham/OpenRedireX), to scan for these. You could also look through each page, looking for interesting things like links that look like redirects.

## To find yourself

In this case because it's a _modern_ web app, meaning everything is in JavaScript, you can use the Developer Mode of your browser and search for redirect.

You can exploit it by following this link:

https://ft-juice-shop-b80cdbae3ec6.herokuapp.com/redirect?to=https://www.google.com

You can see at the end that the site will redirect you to Google.

# Cross Site Scripting (XSS)

## Helpful to have knowledge of
* HTML
* JavaScript

## Description
Cross Site Scripting occurs when a web service has been designed so that it is possible to take untrusted user input and put that on the page.

## Commonly found
In comments sections, or search queries.

## How would this be discovered?

Someone could manually use the special text below entering it in any search field or comment field to see if it did anything. There are tools that also help automate this such as [NucleiFuzzer](https://github.com/0xKayala/NucleiFuzzer).

## To find yourself

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

# CSRF
# SSRF
# Sub-domain Take-over