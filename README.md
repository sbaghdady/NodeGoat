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

Fix: I added an isAdmin function in sessions.js which checks if the user is an admin.  Moreover, I call this function in the routes/index.js file when handling the case in which the user requests the benefits page.  In this case, it will first check to see if the user is logged in.  If not, the user is not logged in so it will redirect to the login page.  Otherwise, the user is logged in so it will then check if the user has admin access.  If not, the user is logged in but is not an admin so it will redirect the user to the dashboard.  Otherwise, the is logged in and has admin access so it will display the benefits page.
<a href="https://heroku.com/deploy">
  <img src="https://www.herokucdn.com/deploy/button.svg" alt="Deploy">
</a>
