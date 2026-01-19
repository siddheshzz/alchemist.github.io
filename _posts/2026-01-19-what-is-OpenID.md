---
layout: default
title: "What is openId and how does is differ from oauth"
description: "We all would have seen sing in with Google or apple id but how do they work in background?"
---

## What is openId and how does is differ from oauth

TL;DR

What is openId and how does is differ from oauth? But before that let us find out what these two are.

#### OpenId
It is a standard that lets you use an existing account such as github, google, apple to sign in to other website.
Some kind of digital passport.
So there are two things here LIVE(identity provider like google) and VISIT(Relying party or website you wish to visit).
    The Request: You go to a new website (e.g., a news site) and click "Sign in with Google."

    The Handshake: The news site sends you to Google’s login page.

    The Verification: You enter your password only on Google’s site. Google confirms it’s you.

    The Proof: Google sends a digital "token" (a secure message) back to the news site saying, "Yes, this is John Doe, and his email is john@example.com."

    Access: The news site trusts Google and lets you in.

