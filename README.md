README
======
Link to URL: http://cs132security-group42.herokuapp.com
Vulnerability/Exploit 1
---------------
Vulnerability: Unescaped HTML in user input fields.

Exploit:  XSS can be used to place JavaScript in user input fields, which can then get executed in other user's browsers upon loading this malicious code.  This code can then steal sensitive information from the user such as their cookie.  The cookie can enable the attacker to impersonate the user to the system.  The attacker can also make fraudulent requests from the victim's browser to the current website.  Lastly, the attacker can manipulate the website'scontent.

An example of an XSS attack can be for an attacker to change his or her name to a malicious script that executes malicious code such as stealing a user's cookie and sending it to the attattacker's own site, manipulating the contents of the user's webpage, or sending requests to site as the current user.  These attacks can be executed by placing this script as the new name of the user in the profile edit section.  Because HTML isn't escaped, this code will get executed anytime a user visits a page in which the website display's teh attacker's name.  In this case, the admins have access to a benefits page, which displays the benefits of each user along with their names.  If the admin views benefits, then the attacker's name, which is now malicious script code, will get executed on the admin's browser.  Thus, the admin's cookie can be stolen along with other sensitive data and sent to the attacker.

Fix:  To fix this vulnerability, I changed the swig template system's autoescape parameter from false to true.  This allows the website to escape malicious HTML code submitted by the user. 

Vulnerability/Exploit 2
-----------------------
Vulnerability: Static file serving without access control

Exploit: A user without admin permisions can view and edit the benefits
of any employee by navigating to http://cs132security-group42.herokuapp.com/benefits.  There is no access control on the benefits page so any user that is logged into the system can view and edit employee benefits.  Can also view anyone's allocations without access control.

Fix: I added an isAdmin function in sessions.js which checks if the user is an admin.  Moreover, I call this function in the routes/index.js file when handling the case in which the user requests the benefits page.  In this case, it will first check to see if the user is logged in.  If not, the user is not logged in so it will redirect to the login page.  Otherwise, the user is logged in so it will then check if the user has admin access.  If not, the user is logged in but is not an admin so it will redirect the user to the dashboard.  Otherwise, the is logged in and has admin access so it will display the benefits page.  I also edited the routes/allocations.js file to ensure that only the allocations belonging to the requesting user displayed.

Vulnerability/Exploit 3
-----------------------
Vulnerability: Remote origin access

Exploit:  An attacker can create a website that sends a request to http://cs123security-group42.herokuapp.com/profile and other parts of the site on behalf of the user.  In particular, the attacker can get the user to click on a form that submits input and alters the contents of the users profile on the heroku app.  This works if the user is currently logged into heroku.  Because the user has an active session with heroku, the user can inadvertently submit a form which alters the state of the server on the heroku app.  

Example #1:
This creates a form, which can be placed on the attackers site.  If the user is tricked into clicking on this form, then it will submit
the contents of the form to the /contributions page on the heroku app and alter the user's contributions data.

<form method="POST" action="http://cs132security-group42.herokuapp.com/contributions">
    <table class="table table-bordered table-hover tablesorter">
        <thead>
            <tr>
                <th>Contribution Type</th>
                <th>Payroll Contribution Percent
                    <br>(per pay period)
                    <br>
                </th>
                <th>New Payroll Contribution Percent
                    <br>(per pay period)
                    <br>
                </th>
            </tr>
        </thead>
        <tbody>

            <tr>
                <td>Employee Pre-Tax</td>
                <td>0 %</td>
                <td>
                    <input type="text" name="preTax" value="0"></input>
                    <span>%</span>
                </td>
            </tr>
            <tr>
                <td>Roth Contribution</td>
                <td>0 %</td>
                <td>
                    <input type="text" name="roth" value="0"></input>
                    <span>%</span>
                </td>
            </tr>
            <tr>
                <td>Employee After Tax</td>
                <td>0 %</td>
                <td>
                    <input type="text" name="afterTax" value="0"></input>
                    <span>%</span>
                </td>
            </tr>
        </tbody>
    </table>
    <!-- Fix for A8 - CSRF -->
    <!--
    <input type="hidden" name="_csrf" value=""></input>
    -->
    <button type="submit" class="btn btn-default">Submit</button>
</form>

Example 2:

Submitting this form on the user's behalf will alter the contents of the user's profile information.  
This will ideally be hidden, with just the submit button showing, so the user will not be able to realize
the attack and thus will submit the form and alter their info without knowing.

<form id='test' role="form" method="post" action="http://cs132security-group42.herokuapp.com/profile">
    <div class="form-group">
        <label for="firstName">First Name</label>
        <input type="text" class="form-control" id="firstName" name="firstName" value="HACKED" placeholder="Enter first name">
    </div>
    <div class="form-group">
        <label for="lastName">Last Name</label>
        <input type="text" class="form-control" id="lastName" name="lastName" value="HACKED" placeholder="Enter last name">
    </div>
    <div class="form-group">
        <label for="ssn">SSN</label>
        <input type="text" class="form-control" id="ssn" name="ssn" value="HACKED" placeholder="Enter SSN">
    </div>
    <div class="form-group">
        <label for="dob">Date of Birth</label>
        <input type="date" class="form-control" id="dob" name="dob" value="" placeholder="Enter date of birth">
    </div>
    <div class="form-group">
        <label for="bankAcc">Bank Account #</label>
        <input type="text" class="form-control" id="bankAcc" name="bankAcc" value="HACKED" placeholder="Enter bank account number">
    </div>
    <div class="form-group">
        <label for="bankRouting">Bank Routing #</label>
        <input type="text" class="form-control" id="bankRouting" name="bankRouting" value="HACKED" placeholder="Enter bank routing number">
    </div>
    <div class="form-group">
        <label for="address">Address</label>
        <input type="text" class="form-control" id="address" name="address" value="HACKED" placeholder="Enter address">
    </div>
    <input type="hidden" name="_csrf" value="" />
    <button type="submit" class="btn btn-default" name="submit">Submit</button>
</form> 

Fix: Adding CSRF tokens in forms can enable the website to prevent attackers from sending requests to the site on the user's behalf.  Could not get this to work.

Vulnerability/Exploit 4
-----------------------
Vulnerability:
The security flaw that I found on the server is using eval statements on the server side in order to process input.  In particular, the server's contributions.js route has a function that handles the event where a user submits new contribution data.  However, in order to process this data, the server evaluates the data as live JavaScript by calling the eval(data) function around the data.  This is bad because it enables an attacker to submit any malicious JavaScript code as data to the benefits.js route, which will get executed as live JavaScript on the server by using the eval() statement with the data as an argument.  This will then run and execute the javascript code on the serverside that the user submitted, which can compromise the server.

Exploit:

The attacker can commit a DOS attack in which service is denied to the server.  This occurs when the attacker sumits as data to the contributions.js page an input consisting of JavaScript code that enters an infinite loop such as while(1){} or for(;;){}.  When submitted, the server will execute this input data as JavaScript code using the eval() statement, which will call wihle(1){} on the server and will enter the infite loop.  The server will be forever stuck in this infinite loop and will never finish handling the user's requqest and will be unable to handle and serve any other client's requests.

Fix:

Remove the eval statements to prevent the server from executing input data as live JavaScript and instead treating input data as static text input that can't be executed.  Thus, the attacker willl be unable to run and execute any malicious JavaScript code on the server because it will be treated as static input data. 



<a href="https://heroku.com/deploy">
  <img src="https://www.herokucdn.com/deploy/button.svg" alt="Deploy">
</a>
