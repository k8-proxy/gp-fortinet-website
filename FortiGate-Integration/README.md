# FortiGate as a Reverse Proxy with ICAP

This guide show configuration FortiGate as a reverse proxy. Tested on
v6.4.3 build1778 (GA), but should work on any 6.x build.

Assumption that there is at least one Interface configured in FortiGate.

[Watch the Video](https://raw.githubusercontent.com/MariuszFerdyn/gp-fortinet-website/main/FortiGate-Integration/media/media/FortiGate_as_a_Reverse_Proxy.mp4)

## Enable ICAP feature

1.  Login to the Forigate.

2.  Go to **System** \> **Feature Visibility**.

3.  Enable **ICAP**.

4.  Click **Apply**.

![image](https://raw.githubusercontent.com/MariuszFerdyn/gp-fortinet-website/main/FortiGate-Integration/media/media/image1.png)

## Configure ICAP Security Profile

1.  Go to **Security Profiles** \> **ICAP**.

2.  Choose **Create NEW**.

3.  Provide **Name** of ICAP Security Profile.

4.  Enable **Response processing**.

5.  Provide **path** gw_rebuild.

6.  In **Server** choose **Create**

    a.  Provide **Name**.

    b.  ICAP **IP address** e.g. 3.139.22.215.

    c.  **Port** e.g. 1344.

    d.  Press **OK**.

7.  In **Server** tab choose created Server.

8.  Press **OK**.

![image](https://raw.githubusercontent.com/MariuszFerdyn/gp-fortinet-website/main/FortiGate-Integration/media/media/image2.png)

## Upload HTTPS certificate

You can use existing FortiGate Certificate, you can generate New
certificate from FortiGate internal CA or just upload your pfx standard
certificate.

If you will use internal FortiGate internal CA remember to download CA
certificate and put this certificate to the Trusted Root Certification
Authorities to your computer.

### Upload certificate .pfx certificate

1.  Go to **System** \> **Certificates**.

2.  Choose **Import** \> **Load Certificate**.

3.  Chose OKCS \#12 Certificate.

4.  Choose **Load** and choose .pfx file.

5.  Provide **Password**.

> ![image](https://raw.githubusercontent.com/MariuszFerdyn/gp-fortinet-website/main/FortiGate-Integration/media/media/image3.png)

### Import certificate to Trusted Root Certification Authorities (Windows OS)

Use this procedure only when you use existing FortiGate certificate or
certificate Generated from FortiGate Certyficate.

If you use FortiGate CA import CA cartyficate not website certificate.

1.  Run mmc.exe

2.  Choose **File** \> **Add/Remove Snap-In**.

3.  Choose **Certyficates** and clisk **Add**.

4.  Select **My User Account** and press Finish.

5.  Click **OK**.

6.  Expand **Certficates** \> **Current User**

7.  Expand **Trusted Root Certification Authorities**

8.  Right Click on **Certificates**

9.  Choose **All Task** \> **Import**.

10. Press **Next**

11. Browse filename of .cer file.

12. Click **Next** and then **Next** and **Finish**.

13. There could be a Waring message about Importing Certyficate so Click
    **Yes** or you should see message **Successful**.

14. Provide **Password**.

![image](https://raw.githubusercontent.com/MariuszFerdyn/gp-fortinet-website/main/FortiGate-Integration/media/media/image4.png)

## Create Virtual Interface on existing interface port

You can also use existing IP attached to interface port.

1.  Go to **Policy & Objects** \> **Virtual IPs**.

2.  Choose **Create New** \> **Virtual IP**.

3.  Provide **Name**.

4.  Choose **Interface**.

5.  Provide **External IP address/range** (from FortiGate range
    address).

6.  Provide **Mapped IP address/range** (this is IP of web server we are
    establish Reverse Proxy).

7.  Click **OK**.

![image](https://raw.githubusercontent.com/MariuszFerdyn/gp-fortinet-website/main/FortiGate-Integration/media/media/image5.png)

## Create Firewall Policy

1.  Go to **Policy & Objects** \> **Firewall Policy**.

2.  Choose **Create New**.

3.  Fill out

    a.  **Name**.

    b.  **Incoming Interface** (From Virtual IP).

    c.  **Outgoing Interface** (Interface that Web Server is Connected)
        .

    d.  **Source**: **all**.

    e.  **Destination** (choose Virtual IP created).

    f.  **Services** choose **HTTP** and **HTTPS**).

    g.  **Inspection Mode**: **Proxy based**.

    h.  Choose **ICAP** -- created before.

    i.  **SSL Inspection** - **Create New**

        i.  Fill Out **Name**.

        ii. Choose **Protecting SSL Server**.

        iii. **Server certificate**: Choose uploaded certificate.

        iv. Press **OK**.

> ![image](https://raw.githubusercontent.com/MariuszFerdyn/gp-fortinet-website/main/FortiGate-Integration/media/media/image6.png)

j.  Press **OK**.

![image](https://raw.githubusercontent.com/MariuszFerdyn/gp-fortinet-website/main/FortiGate-Integration/media/media/image7.png){width="6.5in" height="5.874305555555556in"}

## Configure DNS or hosts file to point to Virtual IP (Windows OS)

1.  Run **notepad C:\\Windows\\system32\\drivers\\etc\\hosts** as a administrator.

2.  Add line:
```bash
10.10.102.199 glass03.h.com.pl
```
> where 10.10.102.199 is **Virtual IP** and glass03.h.com.pl is Web Site FQDN name on Web Server.

3.  Browse site e.g. glass03.h.com.pl.
