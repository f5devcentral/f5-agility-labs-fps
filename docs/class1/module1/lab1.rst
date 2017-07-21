Lab 1: Examine the Dangers of Malware and Phishing
--------------------------------------------------

In this lab, you will see how malware can manipulate web pages using the
Document Object Model (DOM), and then you will see how easy it is to
create a phishing web site:

Task 1 - Connect to Ravello and Use Chrome to Manipulate a Web Page
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Use a browser to access **http://IP\_address** with the IP address
   supplied by your instructor, and log in using the username and
   password supplied by your instructor.

#. For **WebSafe Training Blueprint** click **View**.

#. Copy the IP address of the **Windows 7 External** VM, and then use
   RDP to access the IP address.

#. Log into the Windows workstation as **external\_user** /
   **password**.

#. Update the Windows time:

   a. Select the clock and click **Change date and time settings…**

   b. Select the **Internet Time** tab, and then click **Change
      settings…**

   c. Select **time.windows.com**, and then click **Update now**.

#. Open Chrome and press the **F12** key, and then click the **Bank**
   bookmark.

#. Examine the **Elements** tab.

   |image1|

   The **<html>** element is the top-level of the document object model
   tree. This element contains two child nodes, **<head>** and **<body>**,
   and the **<body>** node contains two **<div class=…>** child nodes.

#. Expand the second **<div>** node, and then expand its child **<div>**
   node.

#. Mouse-over the second child **<div>** node and examine the web page.

   This element represents the Demo Bank heading and the text below it.

#. Expand the second child **<div>** node, then mouse over the **<h2>**
   element and the **<p>** element and examine the web page.

#. Expand the **<h2>** node, then right-click on **“ – Secure Online**,
   and then select **Edit text**.

   |image2|

#. Edit the element from **– Secure Online** to **– Very Insecure
   Online**, then press the **Enter** key.

#. Examine the change to the web page.

   You’ve just made a simple change to the web page within the browser
   after it was sent from the web server.

#. Copy the following text:

   .. code-block:: html
      :linenos:

      <form method="POST"> 
      <div class="form-group">
        Username: <input type="text" placeholder="" name="username" class="form-control">
      </div>
       
      <div class="form-group">
        Password: <input type="password" placeholder="" name="password" class="form-control">
      </div>
       
      <div class="form-group">
        ATM Pin: <input type="text" placeholder="" name="pin" class="form-control">
      </div>
       
      <input type="submit" class="btn btn-success" style="float:right" value="Login">
      </form>

#. In the web page, right click inside the **Username** field and select
   **Inspect**.

#. Right-click the **<form method="POST">** line, and then select **Edit
   as HTML**.

   |image3|

#. Select and delete all the text between the **<form>** opening tag and
   the **</form>** closing tag, then paste the text that copied to your
   clipboard earlier, then click outside of the **<form>** editing area
   and examine the web page.

#. Enter the following credentials but do not click **Login**.
   **Username**: your first name
   **Password**: P@ssw0rd!
   **PIN**: your last name

#. In the inspection window open the **Console** tab, and in the
   console, one at a time type (or copy and paste) each of the following
   and press **Enter**:

   .. code-block:: javascript

      document.forms[0].username.value
      
      document.forms[0].password.value
      
      document.forms[0].pin.value

   These values haven’t yet been submitted and are therefore available in
   cleartext for form grabbing.

#. In the console, one at a time type (or copy and paste) each of the
   following and press **Enter**:

   .. code-block:: javascript

      document.forms[0].username.value = "bob"

      document.forms[0].pin.value = "smith"

#. Examine the web page form.

   Malware can manipulate the parameter values before they are submitted.

#. Click the **Bank** bookmark, then click the **Demo Tools** bookmark, and 
   from the Demo Tools click **Start Keylogger**, and then click on the
   **Password** field.

#. For **Password** type **P@ssw0rd1** and examine the top of the Demo
   Tools window.

   |image4|

   A keylogging program can capture the characters of the user’s password
   as they’re typed.

Task 2 - Create a Phishing Web Site
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Open the **Start** menu, then right-click on **Notepad** and select
   **Run as administrator**, and then click **Yes**.

   |image5|

#. Go to **File > Open**, from the file types list select **All Files**,
   and then open the **hosts** file.

#. At the end of the hosts file list, add a new entry for the following,
   and then save and close the **hosts** file.

   ``10.1.10.16 bank.vlab.f5demos.com``

#. In the banking page click the **Bank** bookmark.

#. Right-click inside the page and select **Save as**.

#. Navigate to the desktop and open the **Phishing** directory.

#. Name the file **login.html**, ensure that **Webpage, Complete** is
   selected and click **Save**, and then close the banking page.

#. Open **WinSCP**.

#. Change the **File protocol** to **SCP**, for **Host name** type
   **10.1.1.252**, and log in as **root** / **default**.

   This is a web server that’s been high jacked by a phishing hacker.

#. In the left panel for the Windows workstation, navigate to the
   desktop and open the **Phishing** directory.

#. In the right panel for the high-jacked web server, navigate to
   **var/www/dvwa**.

#. Select both **login.htm**\ l and **login\_files** and copy them into
   the **dvwa** directory, and then close **WinSCP**.

#. Open an incognito window and access
   **http://bank.vlab.f5demos.com/login.html**.

   |image6|

#. Enter the following credentials and click **Login**.
   **Username**: your first name
   **Password**: P@ssw0rd!

   .. NOTE:: Your login fails, however you have just submitted your username
      and password on the hacker’s phishing site.

#. Close Chrome.

Task 3 - Configure BIG-IQ for Logging
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Open Chrome and click the **BIGIQ\_Mgmt** bookmark, and then log into
   the BIG-IQ system as **admin** / **admin**.

#. On the **BIG-IQ Logging > Logging Nodes** page click **Add Node**.

#. Use the following information, and then click **Add**.

   +---------------------+---------------+
   | Form field          | Value         |
   +=====================+===============+
   | IP Address          | 10.1.20.248   |
   +---------------------+---------------+
   | User name           | admin         |
   +---------------------+---------------+
   | Password            | admin         |
   +---------------------+---------------+
   | Transport Address   | 10.1.20.248   |
   +---------------------+---------------+
   | Transport Port      | 9300          |
   +---------------------+---------------+

   It takes a couple of minutes to discover the logging node.

#. Once the logging node has been discovered, click
   **bigipqlogging.f5demo.com**, and then open the **Services** page.

#. For **Fraud Protection Service**, click **Activate**.

#. Once the activation is complete, open a new tab and click the
   **BIGIP\_A** bookmark, and then log into the BIG-IP system as
   **admin** / **admin**.

#. Open the **Pool List** page and ensure that the
   **bigiq\_logging\_pool** displays as online.

.. |image1| image:: /_static/class1/image3.png
   :width: 5.03366in
   :height: 1.52512in
.. |image2| image:: /_static/class1/image4.png
   :width: 1.85289in
   :height: 0.89394in
.. |image3| image:: /_static/class1/image5.png
   :width: 4.98791in
   :height: 0.74167in
.. |image4| image:: /_static/class1/image6.png
   :width: 1.72576in
   :height: 0.34676in
.. |image5| image:: /_static/class1/image7.png
   :width: 2.57989in
   :height: 1.62164in
.. |image6| image:: /_static/class1/image8.png
   :width: 1.76774in
   :height: 1.24397in
