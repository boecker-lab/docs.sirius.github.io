---
permalink: /account-and-license/
title: "Account and License"
---

Since SIRIUS 5, a user account and a license is required to use the webservice-based
features of SIRIUS.

**Non-webservice-based features** include molecular formula annotation using fragmentation trees and isotope pattern analysis -
this is performed on your local computer and no license is needed.

**Webservice-based features** are essentially advanced structure elucidation features including structure database search 
with CSI:FingerID and CANOPUS compound class prediction.  
No worries, these and upcoming features **will stay free for academic/non-commercial use**
and will be provided/hosted by the FSU Jena. On the other hand the [Bright Giant GmbH](https://bright-giant.com/) 
offers SIRIUS web service hostings for commercial users. 

### Academic users
Non-commercial licenses will be granted (automatically) based on the domain of your 
**institutional email address**. You need to use your institutional email address for SIRIUS your account
to benefit from free academic licenses.

The FSU Jena maintains an allow-list of academic/non-profit intuitions/organizations. However, such a 
list will never be complete. If your Institution does not have access but you think it should be, please 
[email](mailto:sirius@uni-jena.de) us with some information about your institution (e.g. official website). 
We will perform manual validation and grant access to your institution if it meets the requirements for the 
academic/non-commercial license. 

### Commercial users
Please contact [Bright Giant](https://bright-giant.com/) via [email](mailto:info@bright-giant.com) for offerings and pricing.  

### Account creation
User accounts can be created directly via the SIRIUS GUI.
Open `Webservice -> Account`. Click on `Create Account` and enter you **institutional** (not private) email address
and specify a password for your account. Verify your email address with the link that will be sent to your inbox.
Finally, click on `Log in` and log in with your account credentials.

To login from the CLI use the following command:
```
sirius login -u <email> -p
```
See `sirius login --help` for details.

**NOTE:** When logging in, SIRIUS will retrieve a long-lived `refresh_token` that will be stored until it is invalidated 
by logging out. Your username and password will **never** be stored locally.
