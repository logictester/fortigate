.. SafeNet Trusted Access documentation master file, created by
   sphinx-quickstart on Wed Mar 24 16:12:19 2021.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

========================================================================
FortiGate VPN and FortiClient with SafeNet Trusted Access using SAML 2.0
========================================================================

.. toctree::
   :maxdepth: 3
   :hidden:

   index


Overview
========

This guide shows how to implement adaptive authentication with strong contextual security policies to FortiGate SSL VPN and the FortiClient using SAML federation with SafeNet Trusted Access.


Prerequisites
=============

  - FortiOS 6.4.4.1803 and above
  - FortiClient 6.4.2.1580 and above
  - A user with a SafeNet Trusted Access authenticator is enrolled
  - Users can authenticate using SafeNet Trusted Access


.. warning:: In evaluating this solution, it is advised to use a FortiGate firewall reserved exclusively for testing. Nevertheless, with limited resources, it's possible to create an SSL VPN portal on a dedicated port. Please review this document carefully, involve your FortiGate subject matter experts early in the cycle and as always proceed with caution.




Configuration Overview
======================

The configuration requires the following steps:

  **In SafeNet Trusted Access**

  - Create FortiGate Application in SafeNet Trusted Access
  - Adjust the Authentication Policies

  **In FortGate**

  - Add a SAML Identity Provider to FortiGate
  - Configure FortiClient VPN


SafeNet Trusted Access Configuration
====================================

Add FortiGate Application in SafeNet Trusted Access
***************************************************

.. note:: Open SafeNet Trusted Access Console (you can use the following direct links based on your availability zone, opens in a new tab)

          |US Zone|

            .. |US Zone| raw:: html

              <a href="https://sta.us.safenetid.com" target="_blank">US Zone SafeNet Trusted Access Console</a>

          |EU Zone|

            .. |EU Zone| raw:: html

              <a href="https://sta.eu.safenetid.com" target="_blank">EU Zone SafeNet Trusted Access Console</a>

          |Classic Zone|

            .. |Classic Zone| raw:: html

              <a href="https://sta.safenetid.com" target="_blank">Classic Zone SafeNet Trusted Access Console</a>

In the STA Console, add FortiGate application by following these steps:

1. In **Applications** tab, click on the :guilabel:`+` button and search for **Generic Template**.

.. thumbnail:: _images/applications.png

2. Name the application and choose **SAML** for the **Integration Protocol**.

.. thumbnail:: _images/application.png

3. *Optional* - Change the Application Logo by clicking on the default icon. You can download FortiGate logo icon :download:`here <_downloads/fg_logo.png>`

.. thumbnail:: _images/add_icon.png

.. thumbnail:: _images/icon.png

  - Browse for or drag the logo icon downloaded above and click :guilabel:`Select`

4. Clcik :guilabel:`Add` to add the FortiGate Application

5. Switch to **Manual Configuration**

.. thumbnail:: _images/manual.png

- Download STA Tenant Certificate by clicking :guilabel:`Download X.509 certificate`

.. thumbnail:: _images/certificate.png

- Note both STA Tenant **Issuer/Entity ID** and STA **Single Sign-On Service** URL

.. thumbnail:: _images/entity.png

6. Click :guilabel:`Next Step`

7. Switch to **Manual Configuration**

.. thumbnail:: _images/manual2.png

8. In **Account Details** provide the required details using the following values (replace **URL:PORT** with your own values depending on your FortGate SSL VPN configuration and port)

+--------------------------------+------------------------------------------+
| **Setting**                    | **Value**                                |
+--------------------------------+------------------------------------------+
| Entity ID                      | ::                                       |
|                                |                                          |
|                                |   https://URL:PORT/remote/saml/metadata/ |
+--------------------------------+------------------------------------------+
| Logout URL (Post Binding)      | ::                                       |
|                                |                                          |
|                                |   https://URL:PORT/remote/saml/logout/   |
+--------------------------------+------------------------------------------+
| Logout URL (Redirect Binding)  | ::                                       |
|                                |                                          |
|                                |   https://URL:PORT/remote/saml/logout/   |
+--------------------------------+------------------------------------------+
| Assertion Consumer Service URL | ::                                       |
|                                |                                          |
|                                |   https://URL:PORT/remote/saml/login     |
+--------------------------------+------------------------------------------+

.. thumbnail:: _images/account.png

9. In **User Login ID Mapping**, select **SAS User ID**

.. thumbnail:: _images/nameid.png

10. In **Return Attributes**, create an attribute by clicking :guilabel:`Add Atrribute` with *Return Attribute* **username** and *User Attribute* **SAS User ID**

.. thumbnail:: _images/attribute.png

11. In **User Portal Settings**, change to *Federation Mode* to **SP Initiated** and enter the URL of your FortiGate SSL VPN portal and port number using the colon :code:`:` delimiter in *Service Login URL*

.. thumbnail:: _images/user_portal.png
