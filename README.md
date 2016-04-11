README
======

Vulnerability/Exploit 1
---------------
Vulnerability: Unescaped HTML in user input fields.

Exploit:  XSS can be used to place JavaScript in user input fields, which can then get executed in other user's browsers upon loading this malicious code.  This code can then steal sensitive information from the user such as their cookie.  The cookie can enable the attacker to impersonate the user to the system.  The attacker can also make fraudulent requests from the victim's browser to the current website.  Lastly, the attacker can manipulate the website'scontent.

Fix:  To fix this vulnerability, I changed the swig template system's autoescape parameter from false to true.  This allows the website to escape malicious HTML code submitted by the user. 

Vulnerability/Exploit 2
-----------------------
Vulnerability: Static file serving without access control

Exploit: A user without admin permisions can view and edit the benefits
of any employee by navigating to http://cs132security-group42.herokuapp.com/benefits.  There is no access control on the benefits page so any user that is logged into the system can view and edit employee benefits.

Fix:  
<a href="https://heroku.com/deploy">
  <img src="https://www.herokucdn.com/deploy/button.svg" alt="Deploy">
</a>
