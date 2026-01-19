---
layout: default
title: "What is openId and how does is differ from oauth"
description: "We all would have seen sing in with Google or apple id but how do they work in background?"
---

## What is openId and how does is differ from oauth

TL;DR
The "Login with Google" button is just a pretty wrapper for the OpenID protocol.
The easiest way to think about it is that OAuth 2.0 is the car, and OpenID Connect (OIDC) is the driver’s license.

What is openId and how does is differ from oauth? But before that let us find out what these two are.

#### OpenId
It is a standard that lets you use an existing account such as github, google, apple to sign in to other website.
Some kind of digital passport.
So there are two things here LIVE(identity provider like google) and VISIT(Relying party or website you wish to visit).
    
    The Request: You go to a new website (e.g., a news site) and click "Sign in with Google."

                                        |
                                        v

    The Handshake: The news site sends you to Google’s login page.

                                        |
                                        v


    The Verification: You enter your password only on Google’s site. Google confirms it’s you.

                                        |
                                        v


    The Proof: Google sends a digital "token" (a secure message) back to the news site saying, "Yes, this is John Doe, and his email is john@example.com."

                                        |
                                        v


    Access: The news site trusts Google and lets you in.

#### Why use it?
For You (The User)
- No "Password Fatigue": You don't have to remember 50 different passwords.
- Security: You only trust your password to a few major, highly secure companies rather than dozens of small, potentially insecure websites.
- Speed: You can "sign up" for a new service in two clicks.

For Developers
- Lower Risk: They don't have to store your password, so if their database is hacked, your password isn't stolen.
- Easier Onboarding: Users are more likely to sign up if they don't have to fill out a long registration form.



OAuth 2.0 handles the "handshake" between the two websites.

OpenID Connect is the specific layer that sends over your name, email, and profile picture.

If you are just logging in, you are technically using OpenID Connect (which is powered by OAuth 2.0). If you are granting an app permission to access your data, you are using OAuth 2.0



