---
title:  "Jualo User Bio XSS"
date:   2015-10-21 00:00:00
description: That Annoying Little Thing Called XSS
---

This is a documentation of an XSS vulnerability in Jualo, another Indonesian e-commerce startup.

### Timeline

__October 17, 2015:__ A report is made to Jualo regarding the vulnerability around noon.

__October 20, 2015:__ No reply from Jualo since the report was sent, so I kept checking if the bug is still around. When checking on the morning on October 20, the bug is already fixed.

### The Vulnerability

Since I was a bit bored on the weekend, I decided to go to Jualo and try checking for web vulnerabilities. I created an account and uploaded a product.

I tried putting HTML brackets in the product's name and description, and inspecting the element where the product name and description is rendered on the browser. But the HTML brackets are properly encoded, so it doesn't render any HTML elements.

Then, I moved on to the user profile page, and see the menu to edit the profile.

![Screenshot-01](/assets/images/posts/jualo-xss-full-01.png)

I played a bit with the fields available for editing the user information by inserting HTML tags and brackets, and see how it's rendered in the page. I found out that the brackets and HTML characters I inputted to the user bio field are rendered as they are in the page when inspecting the elements.

![Screenshot-02](/assets/images/posts/jualo-xss-inspect-01.png)

The problem is that whenever I put a full HTML tag (for example, `<script>` and `</script>`), it is filtered by Jualo. So I decided to put this string to override the closing `</div>` tag.

![Screenshot-03](/assets/images/posts/jualo-xss-full-02.png)

Let's zoom it in.

![Screenshot-04](/assets/images/posts/jualo-xss-zoom-01.png)

After I saved the new bio with that string, whenever I opened the user's profile (either logged in or not) I'll get this alert.

![Screenshot-05](/assets/images/posts/jualo-xss-full-03.png)

If we inspect the element, we can see that the closing `</div>` tag is overridden by the tag I put in the bio.

![Screenshot-06](/assets/images/posts/jualo-xss-inspect-02.png)

### Risks

XSS is a common vulnerability, and OWASP has a [pretty good overview](https://www.owasp.org/index.php/Cross-site_Scripting_(XSS)) regarding XSS and the risks. XSS enables attackers to modify a web page's display at will and run malicious scripts in the client's browser.
