Lab 3: Use Phishing Detection
-----------------------------

In this lab, you will add phishing detection to the banking application,
then redo the task of adding the phishing site to the high-jacked
server, and then view alerts triggered when the phishing site is
accessed.

Task 1 - Enable Phishing Detection
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Open an incognito window and access the phishing web site at
   **http://bank.vlab.f5demos.com/login.html**.

#. Enter the following credentials and click **Login**.
   **Username**: your first name
   **Password**: P@ssw0rd!

#. Close the phishing page.

#. In the BIG-IQ Configuration Utility reload the page, then open the
   **Phishing Alerts > Phishing** page.

   There are no alerts because this page was copied before a WebSafe
   profile was added to the virtual server.

#. In the BIG-IP Configuration Utility, select the ``login.php`` from
   the **URL** page, and then from the left menu select **Phishing
   Detection**.

When you created the WebSafe profile, phishing detection was enabled by
default.

Task 2 - Detect Phishing of a Web Site
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. From the desktop open the **Phishing** directory and delete the two
   files you created earlier.

#. In the banking tab click the **Bank** bookmark, then right-click
   inside the page and select **Save as**.

#. Name the file **login.html**, ensure that **Webpage, Complete** is
   selected and that you’re saving into the **Phishing** directory and
   click **Save**, and then close the banking tab.

#. Open **WinSCP**.

#. Change the **File protocol** to **SCP**, for **Host name** type
   **10.1.1.252**, and log in as **root** / **default**.

#. In the left panel for the Windows workstation, navigate to the
   desktop and open the **Phishing** directory.

#. In the right panel for the web server, navigate to **var/www/dvwa**.

#. Delete the two files currently in the **dvwa** directory.

#. Select the new **login.html** and **login\_files** and copy them to
   the **dvwa** directory.

#. Open a new incognito window and access
   **http://bank.vlab.f5demos.com/login.html**.

#. Attempt to log in as **bobsmith** / **P@ssw0rd1**, and then close
   Chrome.

#. In the BIG-IQ Configuration Utility reload the page, then open the
   **Phishing Alerts > Phishing** page, and then expand the
   **bank.vlab.f5demos.com** alert.

   A **Copied Pages** alert was generated, and in addition a **Phishing
   Users** alert was generated for user **bobsmith**.

#. Click **Copied Pages** and view the **Domain** and the **Additional
   Info**.

The fake domain name is **bank.vlab.f5demos.com** and the original page
is **https://bank.vlab.f5demo.com/login.php**.

Task 3 - Use JavaScript Removal Detection
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. In WinSCP, in the **dvwa** directory, right-click **login.html** and
   select **Edit**.

#. Click on the find (binoculars) button and type **<script** and click
   **Find Next** several times to locate all scripts in the page.

   There are three script entries added by WebSafe.

#. Select and delete everything from the first **<script
   type="text/javascript" src=…>** tag to its closing **</script>** tag.

   |image20|

#. Select and delete everything from the next **<script
   type="text/javascript">** tag to its closing **</script>** tag (right
   before the **<style>** tag near the end of the same line).

   |image21|

#. Select and delete everything from the final **<script
   type="text/javascript">** tag to its closing **</script>** tag (right
   before the **<img home= >** tag).

#. When you’re done, your code should resemble the following:

   |image22|

#. Save and close the **login.html** file.

#. Open a new incognito window and access
   **http://bank.vlab.f5demos.com/login.html** and attempt to log in as
   **bobsmith** / **P@ssw0rd1**, and then close Chrome.

Notice the page still displays as expected.

#. In the BIG-IQ Configuration Utility reload the page, then open the
   **Phishing Alerts > Advanced Phishing** page, and then expand the
   **bank.vlab.f5demos.com** alert.

   Although the hacker removed the JavaScript, a **CSS Check** alert and
   an **Image Check** alert was issued.

#. Close **WinSCP**.

.. |image20| image:: /_static/class1/image22.png
   :width: 5.42690in
   :height: 0.78030in
.. |image21| image:: /_static/class1/image23.png
   :width: 5.53545in
   :height: 0.29456in
.. |image22| image:: /_static/class1/image24.png
   :width: 5.63620in
   :height: 0.82258in
