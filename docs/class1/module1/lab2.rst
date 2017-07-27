Lab 2: Use Malware Detection
----------------------------

In this lab, you will see how to create and configure a BIG-IP WebSafe
anti-fraud profile and see how malicious activity sends alerts to the
BIG-IQ logging server for viewing and analysis.

Task 1 - Create a WebSafe Anti-Fraud Profile
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. In the BIG-IP Configuration Utility, open the **Security > Fraud
   Protection Service > Anti-Fraud Profiles** page and click **Create**.

#. Use the following information, and then click **Create**.

   +--------------------+-----------------------------------------------------------------------+
   | Form field         | Value                                                                 |
   +====================+=======================================================================+
   | Profile Name       | banking\_fraud\_profile                                               |
   +--------------------+-----------------------------------------------------------------------+
   | Alert Identifier   | D1 (you need to first click the checkbox to the right of the field)   |
   +--------------------+-----------------------------------------------------------------------+
   | Alert Pool         | bigiq\_logging\_pool (same note as above)                             |
   +--------------------+-----------------------------------------------------------------------+
   | Log Publisher      | bigiq\_logging\_publisher (same note as above)                        |
   +--------------------+-----------------------------------------------------------------------+

#. Open the **Virtual Server List** page and click **bank\_virtual**,
   and then open the **Security > Policies** page.

   |image7|

#. From the **Anti-Fraud Profile** list select **Enabled**.

#. From the **Profile** list select **banking\_fraud\_profile**, and
   then click **Update**.

#. Open a new tab and press the **F12** key, and then click the **Bank**
   bookmark.

#. In the inspection window examine the files on the **Network** tab.

   There are five files returned from the web server to build this web page.

#. In the BIG-IP Configuration Utility, open the **Security > Fraud
   Protection Service > Anti-Fraud Profiles** page and click
   **banking\_fraud\_profile**.

#. Expand the left panel and click **URL List**, and then click **Add**.

   |image8|

#. For **URL Path** leave **Explicit** selected, and type
   **/login.php**.

#. Expand the left panel and open the **Malware Detection** page.

   Note that nearly all malware detection options are enabled by default.

#. Click **Create**.

#. In the banking tab click the **Bank** bookmark and examine the
   **Network** tab.

There is now a **script** file and additional files that were added by
BIG-IP WebSafe.

Task 2 - View WebSafe Alerts
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. In the BIG-IQ Configuration Utility, from the main BIG-IQ menu select
   **Fraud Protection Service**.

   |image9|

#. On on the left panel open the **Malware Alerts** section.

   |image10|

   No alerts have been generated yet.

#. In the banking tab click the **Demo Tools** bookmark and then click
   **Insert Malicious Script**.

#. For the **Malicious domain** field, copy and paste
   **http://www.hackingsite.com/inject.js**, and then click **OK**.

#. Log in as **bobsmith** / **P@ssw0rd1**, and then click **Logout**.

#. In the BIG-IQ Configuration Utility reload the page, then open the
   **Malware Alerts > External Scripts** page, and then expand the
   **hackingsite.com** alert.

   |image11|

   An external script has been reported with the alert type of **External
   Sources**.  There are also additional alerts caused by the **Demo Tools**
   bookmark, which also makes calls to scripts from external sources.

#. Examine the **User Name** column.

   The user name is presently **Unknown**.

#. Expand the alert section for **ajax.googleapis.com** and select the
   alert checkbox, and then click **Remove** and then **Delete
   Selected**.

#. Repeat the step above for the alert for
   **s3-eu-west-1.amazonaws.com**.

#. In the banking tab, to discover the parameter name that needs to be
   sent to the alert server, right-click inside the **Username** field
   and select **Inspect**.

   |image12|

   The parameter name is ``username``.

#. In the BIG-IP Configuration Utility, for the ``banking_fraud_profile``, 
   click the ``Anti-Fraud Profile`` link (see below).

   |image13|

#. From the left panel select the global **Malware Detection** option.

   |image14|

#. For **Allow URLs from these external domains**, add both
   **ajax.googleapis.com** and **s3-eu-west-1.amazonaws.com**, and then
   click **Save**.

   |image15|

#. From the left panel select **URL List**, and then click
   **/login.php**.

#. From the left panel select **Login Page Properties**, and then
   select the **URL is Login Page** checkbox.

#. For **Expected HTTP response status code**, in the **Specify** field
   enter **302**.

   |image16|

#. From the left panel select **Parameters**.

#. Create a new parameter named **username**, and then click **Add**.

#. Select the **Identify as Username** and **Send in Alerts**
   checkboxes, and then click **Save**.

   |image17|

#. In the banking tab click the **Bank** bookmark, then click the **Demo
   Tools** bookmark and click **Insert Malicious Script**.

#. For the **Malicious domain** field, copy and paste
   **http://www.worsesite.com/malware.js**, and then click **OK**.

#. Log in as **bobsmith** / **P@ssw0rd1**, and then click **Logout**.

#. In the BIG-IQ Configuration Utility reload the page, and then in the
   **Malware Alerts > External Scripts** section expand the
   **worsesite.com** alert.

   The user name information (**bobsmith**) is now being sent to the
   alert server. In addition, the scripts used for the Demo Tools are no
   longer triggering alerts.
   (NOTE: If the **User Name** is still displaying as **Unknown**, wait
   about 30 seconds and reload the page again.)

Task 3 - Check for Malware JavaScript Signatures
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. In the BIG-IQ Configuration Utility open the **Malware Alerts > Alert
   Transform Rules** page and click **tatang.Trojan**.

   This signature, looking for **tatangakatanga**, was added before the
   exercise. Notice at the bottom of the page the alert severity is
   configured at **90**.

#. In the BIG-IP Configuration Utility, click the
   ``banking_fraud_profile`` **Anti-Fraud Profile** link, and then
   from the left panel open the global **Malware Detection** page.

#. For the **Search for malicious words in the HTML or JavaScript code**
   field, add both **tatangakatanga** and **trojan** as two separate
   entries to the global forbidden list, and then click **Save**.

#. Select **URL List** and click **/login.php**.

#. From the left panel select **Malware Detection** and scroll down to
   the **Malware JavaScript Signatures** option.

   By default, words added to the global list are configured for all URLs.
   Notice you could ignore a globally defined JavaScript signature for a
   specific URL.

#. In the banking tab click the **Bank** bookmark, then click the **Demo
   Tools** bookmark and then click **Imitate Trojan**.

   |image18|

   This imitates a trojan for **tatangakatanga**.

#. Log in as **bobsmith** / **P@ssw0rd1**, and then click **Logout**.

#. In the BIG-IQ Configuration Utility reload the page, and then open
   the **Malware Alerts > Targeted Malware** page and expand the
   **bobsmith** grouping.

   A **tatang.Trojan** alert was issued. Notice the severity level of
   **90**. In addition,
   a **Symbols Found** alert was issued, due to the word **trojan** that
   occurred when you clicked **Imitate Trojan**.

Optional Task - Require Mandatory Words
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. In the BIG-IP Configuration Utility, open the **URL List** page and
   click **/login.php**.

#. From the left panel select **Malware Detection**.

#. In the **Mandatory Words** section, add both **Secured** and **We
   will never ask you** as two separate entries to the mandatory words
   list, and then click **Save**.

#. In the banking tab click the **Bank** bookmark, then log in as
   **bobsmith** / **P@ssw0rd1**, and then click **Logout**.

#. In the BIG-IQ Configuration Utility reload the page, and then open
   the **Validation Errors > Missing Components** page, and then expand
   the alert.

   A **String(s) Are Not Visible** alert was issued.

#. Click **String(s) Are Not Visible** and view the **Alert Details**.

   The alert was issued due to the missing words **Secured**.

#. Click **Remove** and then **OK**, and then click **Refresh**.

#. In the banking tab, examine the Demo Bank page.

   |image19|

   The word that appears at the top of the page is actually **Secure**, not
   **Secured**.

#. In the BIG-IP Configuration Utility, select the **Secured** entry and
   click **Delete**, and then add a new entry for **Secure**, and then
   click **Save**.

#. In the banking tab click the **Bank** bookmark.

#. In the BIG-IQ Configuration Utility reload the page, and then open
   the **Validation Errors > Missing Components** page.

No new alerts were generated.

.. |image7| image:: /_static/class1/image9.png
   :width: 3.77083in
   :height: 0.70980in
.. |image8| image:: /_static/class1/image10.png
   :width: 2.93700in
   :height: 0.91935in
.. |image9| image:: /_static/class1/image11.png
   :width: 3.17400in
   :height: 1.09677in
.. |image10| image:: /_static/class1/image12.png
   :width: 1.66667in
   :height: 1.19394in
.. |image11| image:: /_static/class1/image13.png
   :width: 4.71772in
   :height: 0.74306in
.. |image12| image:: /_static/class1/image14.png
   :width: 5.17500in
   :height: 0.30326in
.. |image13| image:: /_static/class1/image15.png
   :width: 4.48930in
   :height: 0.99167in
.. |image14| image:: /_static/class1/image16.png
   :width: 3.64968in
   :height: 1.76427in
.. |image15| image:: /_static/class1/image17.png
   :width: 4.59884in
   :height: 0.58822in
.. |image16| image:: /_static/class1/image18.png
   :width: 3.53030in
   :height: 0.37498in
.. |image17| image:: /_static/class1/image19.png
   :width: 5.53562in
   :height: 0.45000in
.. |image18| image:: /_static/class1/image20.png
   :width: 2.14673in
   :height: 0.85606in
.. |image19| image:: /_static/class1/image21.png
   :width: 3.51684in
   :height: 0.47581in
