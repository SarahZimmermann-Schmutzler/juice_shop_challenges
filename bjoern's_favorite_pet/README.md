# Bjoern's Favorite Pet

## Table of Contents
1. <a href="#profile">Profile</a>  
2. <a href="#what-do-you-need-to-solve-the-challenge">What do you need to solve the challenge?</a>  
3. <a href="#solution">Solution</a>  
4. <a href="#summary">Summary</a> 

## Profile
#### Difficulty
- 3-Start-Challenge

#### Category:
- Broken Authentication

#### Description
- Reset the password of Bjoern's OWASP account via the Forgot Password mechanism with the original answer to his security question.

#### Hints from OWASP Juice Shop itself
- He might have trumpeted it on at least one occasion where a camera was running. Maybe elsewhere as well.

#### Hints from the Developer Akademie
- Reset the password for the user `bjoern@owasp.org`.
    - As security question when registering he used the option: `Name of your favorite pet?`.
- Tip:
    - Use the **Burp Intruder**.

## What do you need to solve the challenge?
- **OWASP Juice Shop**
    - All Products page: `http://127.0.0.1:3000/#/`
    - Photo Wall: `http://127.0.0.1:3000/#/photo-wall`
- **Google** image search  

OR
- If you have a proper wordlist:
    - **Burpsuite** - <a href="https://portswigger.net/burp">Click here for more information</a>
        - Proxy Server
        - Burp Intruder

## Solution
### Step 1: Find a Foto or Video that shows a pet
- Would like to know more about Björn. Maybe he left a review. Search for "bjoern" on the product page using the developer tool's Inspect Mode.
    - Result: His full name is `Björn Kimminich`.
- Now the clue says Björn's pet can be seen on a photo or in a video.
    - Among the one-star tasks, there is a task called "Missing Encoding", which involves making a cat photo visible in the photo gallery.
        - Have a look at the photo wall:
            ```
            http://127.0.0.1:3000/#/photo-wall
            ```
            <img alt="Zatschi" src="https://github.com/SarahZimmermann-Schmutzler/juice_shop_challenges/blob/main/bjoern's_favorite_pet/zatschi.png"></img>
        - The three legged cat that is shown there is called **Zatschi**.
            - >i: This is a dead end here. It's the right cat but Zatschi is her pet name, not the pet's name. Haha, how ironic.
    - If all else fails, ask Daddy Google and his image search option:
        - Search query: `björn kimminich owasp`
        - Result: An **Instagram** post from @bkimminich that shows Zatschi alias `Zaya`.
        <img alt="Zaya" src="https://github.com/SarahZimmermann-Schmutzler/juice_shop_challenges/blob/main/bjoern's_favorite_pet/zaya.png"></img>

### Step 2: Try to reset Bjoern's password
- Open the login page and select `Forgot your password?`` or go directly to the page:
    ```
    http://127.0.0.1:3000/#/forgot-password
    ```
- Fill out the form:
    - Email: `bjoern@owasp.org`
    - Security Question: `Zaya`
    - New Password: `abc123`
    - Repeat Password: `abc123`
    <img alt="Forgotten Password Form" src="https://github.com/SarahZimmermann-Schmutzler/juice_shop_challenges/blob/main/bjoern's_favorite_pet/forgotten-pwd.png"></img>
- You will be rewarded with confetti and a message:
    - You successfully solved a challenge: Bjoern's Favorite Pet (Reset the password of Bjoern's OWASP account via the Forgot Password mechanism with the original answer to his security question.)


### Step 3: Shouldn't the task have been solved with the **Burp Intruder**?
- What ist the **Burp Intruder?
    - The Burp Intruder is a powerful tool within the **Burp Suite** used to carry out automated attacks on web applications. For example it is used for Brute-Force-Attacks.
    - You can bruteforce the answer to the security question, but you need a suitable word list. I haven't found any that contain the name Zaya. Neither a first name list nor a list with pet names.

## Summary
In this challenge there is given a scenario of **Improper Input Validation** leading to a so-called **Privilege Escalation**.   
The user's entries in the registration form are not checked before they are further processed. Using Burpsuite, it was possible to intercept the HTTP request and manipulate it - adding the key-value pair `"role":"admin"` to the payload - so that the user gets administrative priviliges. Actually, this parameter is not entered via the input mask. By default, each user is set a `"custumer"` status.