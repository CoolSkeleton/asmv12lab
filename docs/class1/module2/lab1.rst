Understanding Staging
=====================

If you look at the OWASP ZAP session you opened earlier you will see the
ZAP detected a potential SQL Injection vulnerability.

.. image:: /_static/image24.png

What this indicates is the username parameter will process the
metacharacters that can be used to craft a successful attack. You are
going to plug that hole.

**Perform a SQL Injection**

- Close your last Firefox browser to the Hackazon site and open **new** FireFox browser to http://hackazon.f5demo.com and select **Sign In** in the upper right corner of the page.

- Enter **ZAP' AND '1'='1'** in the **Username** section and anything you want for the password.

.. image:: /_static/image25.png

- You will get a 503 Service Temporarily Unavailable response. This means your SQL injection got through and web server returned an error trying to process the request. This tells a hacker that if he can correctly craft the attack he will probably get through.

- In the Configuration Utility, open the **Security > Event Logs > Application > Requests**. Click on the entry that contains ***[HTTP] /user/login**. (It will be the request in the list with a violation rating)

.. image:: /_static/image26.png

You should see the **Attack signature detected** violation. Click on the violation and you will notice this attack has been identified as a SQL Injection because multiple signatures match this request and we can see these attack signatures are currently in staging.

.. image:: /_static/image27.png

Perform a Cross Site Scripting attack
=====================================

Again, if you look at the OWASP ZAP Alerts under **Cross Site Scripting (Reflected)** from the scanning section you will see there was a successful Cross Site Scripting attack against the search page. Using the following URL::

    http://hackazon.f5demo.com/search?id=%22%3E%3Cscript%3Ealert%281%29%3B%3C%2Fscript%3E&searchString=ZAP

.. NOTE::

   The attack uses URL encoding in the XSS attack script as an evasion technique.
..

- Open the Firefox browser and access **http://hackazon.f5demo.com** virtual server.

.. NOTE:: 

   In private/incognito mode is recommended for testing.

- Paste in the URL above or copy and paste the URL from the OWASP ZAP alert. You should get a pop-up. This indicates the attack was successful.

- In the Configuration Utility, open the **Security > Event Logs > Application > Requests.** Click on the entry that contains **[HTTP] /search**

.. image:: /_static/image28.png
 
You should see the Attack signature detected violation. Click on the violation and you will notice several signatures will detected.

.. image:: /_static/image29.png

Signature Staging
=================

- In the Configuration Utility, open the **Security > Application Security > Policy Building > Learning and Blocking Settings.** Under Signatures** section\ **.** Uncheck **Enable Signature Staging** and click **Save** and **Apply Policy**.

.. image:: /_static/image30.png

- Open a new browser window to the auction site and repeat the SQL Injection attack (**ZAP' AND '1'='1'**) in the Username field of the login form). You will notice that the attack does not get blocked, however; the attack signature is still detected in the Event Log. The reason for this is that the username parameter is also in staging.

- Open **Security > Application Security > Parameters > Parameters List**. You will see that the username parameter is in staging and also has learning suggestions:

.. image:: /_static/image31.png

- Click on **username** and uncheck the box for **Perform Staging** and click **Update**, then **Apply Policy**.

.. image:: /_static/image32.png

- Repeat the SQL injection you and you should now see the blocking page.

.. image:: /_static/image33.png

- Copy the support ID from the ASM Blocking Page. In the Configuration Utility, open the **Security > Event Logs > Application > Requests** and click the **Show Filter Details**. Scroll down to the **Support ID** section and paste the **Support ID** in the empty field then click **Go**. This should bring up the log entry for the most recent SQL injection that was just blocked. Review the entry and clear the filter.

.. image:: /_static/image34.png

- Repeat the forceful browsing attack. What are the results? Think about why further policy changes weren't required as they were for a SQL Injection

.. NOTE::

   This completes the ASM 101 ASM Fundamentals Lab Section