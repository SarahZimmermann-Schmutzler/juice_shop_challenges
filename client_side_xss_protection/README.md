# Client-side XSS Protection

[Watch the video to this challenge](https://go.screenpal.com/watch/cTVj2fneAOQ)

## Table of Contents

1. [Challenge Overview](#challenge-overview)
1. [What do you need to solve the challenge?](#what-do-you-need-to-solve-the-challenge) 
1. [Solution](#solution)
   * [What is XSS?](#step-0-what-is-xss)
   * [Login with admin status and examine the admin section](#step-1-login-with-admin-status-and-examine-the-admin-section)
   * [It has to be placed at the Customer Feedback!](#hypothesis-it-has-to-be-placed-at-the-customer-feedback)
   * [Placing the payload at the Registered Users via the Registration Form](#step-2-placing-the-payload-at-the-registered-users-via-the-registration-form)<a href=""></a>
   * [Check the success](#step-3-check-the-success)
1. [Summary](#summary)

## Challenge Overview

#### Difficulty

* 3-Star-Challenge

#### Category

* XSS (Cross-site Scripting)

#### Description

* Perform a persisted XSS attack with ```<iframe src=\"javascript:alert(`xss`)\">``` bypassing a client-side security mechanism.

#### Hints from OWASP Juice Shop itself

* Only some input fields validate their input. Even less of these are persisted in a way where their content is shown on another screen.

#### Hints from the Developer Akademie

* Using the payload ```<iframe src=\"javascript:alert(`xss`)\">```, ensure that the administrator receives the following message in the *administration section*:

    ![alert](https://raw.githubusercontent.com/SarahZimmermann-Schmutzler/juice_shop_challenges/main/client_side_xss_protection/alert.png)

* Select a suitable source and sink. To do this, take a close look at the administration section and think about what could have triggered the message.

## What do you need to solve the challenge?

* **OWASP Juice Shop**
  * Access to an account with admin status
  * Administration page `http://127.0.0.1:3000/#/administration`

* **Burpsuite** - [Click here for more information](https://portswigger.net/burp)
  * Proxy Server
  * Intercept mode
  * HTTP history

## Solution

### Step 0: What is XSS?

* This task is about **persistent XSS (Cross-Site Scripting)**:
  * The attacker permanently stores malicious code on the server, e.g. in a database.
  * When users visit the infected site, the malicious code is automatically executed.
  * <ins>Example</ins>: An attacker posts a crafted script on a forum that runs every time another user reads the post.

### Step 1: Login with admin status and examine the admin section

* First of all the administration section needs to be examined. **Login** at the **OWASP Jucie Shop** with the admin account or an user account that has admin status to unlock the administration page:

    ```bash
    http://127.0.0.1:3000/#/login
    ```

  * In the task `Admin Registration` a user was registered with admin status, so this account can be used:
    * <ins>Email</ins>: `kazuto@kirigaia.saop`
    * <ins>Password</ins>: `abc123`
  * If this task isn't done yet, use the admin account. In the two-star-challenge `Password Strength` the password was cracked via SQL Injection so it is known:
    * <ins>Email</ins>: `admin@juice-sh.op`
    * <ins>Password</ins>: `admin123`

* Switch to the **administration page** that is now unlocked:  

    ```bash
    http://127.0.0.1:3000/#/administration
    ```

  * <ins>What do you find here?</ins> The page shows the `Registered Users` and the `Customer Feedback`.

### Hypothesis: It has to be placed at the **Customer Feedback**!

* Looking at **Step 0** the `Customer Feedback` feels like the perfect source to put the payload.
* <ins>But it turns out, no</ins>. The JavaScript code could be placed in the feedback form but the server-side validation works as it should and filters out the code.

    ![feedback_form](https://raw.githubusercontent.com/SarahZimmermann-Schmutzler/juice_shop_challenges/main/client_side_xss_protection/feedback_form.png)

    ![feedback_request_and_response](https://raw.githubusercontent.com/SarahZimmermann-Schmutzler/juice_shop_challenges/main/client_side_xss_protection/feedback_response.png)

### Step 2: Placing the payload at the Registered Users via the **Registration Form**

* First it looks like the wrong path too: Because of the safety precautions taken, placing the payload diretcly in the **Registration Form** is not possible.
* Choose the indirect route to place the payload in the request:
  * Open **Burpsuite** and the **Proxy Browser**
  * Browse to the **OWASP Juice Shop's register page**:

    ```bash
    http://127.0.0.1:3000/#/register
    ```

    * Fill in some dummy-data. Before clicking the *Register Button*, switch on the **Intercept mode** in the Burpsuite window to intercept the data that the form will send.
    * Find the request payload and change the parameter `"email": "your_dummy@data"` to ```"email": "<iframe src=\"javascript:alert(`xss`)\">"```  

    ![payload](https://raw.githubusercontent.com/SarahZimmermann-Schmutzler/juice_shop_challenges/main/client_side_xss_protection/payload.png)

    * Forward the request until it is done and confetti appears.
    * Change to the **HTTP history** tab and have a look at the response. It adopted the entered payload as the email address:

    ![payload_response](https://raw.githubusercontent.com/SarahZimmermann-Schmutzler/juice_shop_challenges/main/client_side_xss_protection/payload_response.png)

### Step 3: Check the success

* Go back to the administration page (not proxy server) and refresh it. The JavaScript code runs correctly - an **alert window** appears and its message says: `xss`.

    ![popup](https://raw.githubusercontent.com/SarahZimmermann-Schmutzler/juice_shop_challenges/main/client_side_xss_protection/popup.png)

* The table of the Registered User looks like this:

    ![table_registered_user](https://raw.githubusercontent.com/SarahZimmermann-Schmutzler/juice_shop_challenges/main/client_side_xss_protection/table.png)

* And the the details of the xss-User like that:

    ![details_xss_user](https://raw.githubusercontent.com/SarahZimmermann-Schmutzler/juice_shop_challenges/main/client_side_xss_protection/details.png)

## Summary

In this challenge there is given a scenario of **XSS - Cross-Site Scripting**. A JavaScript code should be placed in a proper source causing the popup of an alert window every time the administration page is opened.  
  
The **registration form** turned out to be a suitable source. Although the client-side validation works (you cannot submit the form with the malicious code), there is no server-side validation, which is why the payload that was injected into the request using Burpsuite is processed by the server. What do we learn from this?

* **Client-side validation alone is not secure**. It can be easily bypassed because the client (browser) is under the attacker's control.
* **The server is the final authority of security**. Every input must be checked and processed on the server side.
* **Layers of protection measures are ideal**: client-side validation for ease of use, server-side validation for security, and additional mechanisms such as output encoding and CSP for comprehensive protection.
  
An `<iframe>` is normally used to embed external content (such as another web page or a resource) into a web page. As seen here, the javascript schema can also be used so that inline JavaScript code is executed directly in the `src attribute of the <iframe>-tag`. That means in a real attack, `alert('xss')` could be replaced with more malicious code, e.g. to steal cookies or redirect to a phishing site.  
**As a user, you can protect yourself** against attacks such as XSS and similar security issues by observing certain behavioral and technical protection measures. Here are some tips:

* Only enter personal information on trusted sites.
* Avoid clicking on links in suspicious emails or on dubious websites.
* Modern browsers such as Chrome, Firefox or Edge implement CSP rules on websites. Keep your browser up to date so that these protective mechanisms are effective.
* Install trusted extensions like uBlock Origin or NoScript that can block scripts and filter out unwanted content.
* Use a VPN, especially on public networks, to increase the security of your data.
 Delete cookies regularly or use the browser in private mode to reduce the risk of cookie theft.
