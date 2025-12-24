---
layout: default
title: "Why Local S3 Uploads are a "Triangle of Death" (And Why We Proxy)"
description: "The Localhost Docker S3 Upload Debacle"
---

## Why we proxy

TL;DR: Don't fight AWS SigV4 in Docker. Your localhost browser, your FastAPI localhost, and your MinIO minio are all different places. For local dev, just send files to your FastAPI backend, then let FastAPI send them to MinIO. It's simpler, stable, and saves your sanity.

We’ve all been there. You’re setting up MinIO in Docker to mimic AWS S3. You want that sleek, "Modern Web" flow:

```
                Frontend asks Backend for a URL.
                
                Backend signs a Presigned URL and hands it over.
                
                Frontend uploads the file directly to S3.

```


It sounds perfect. It’s A → B → C. But then, the console starts screaming: SignatureDoesNotMatch or Connection Refused.


<span style="color:red"><em>Welcome to the Triangle of Death.</em></span>
1. The "Localhost" Identity Crisis

The biggest lie in Docker development is that localhost means the same thing to everyone.
To your Browser: localhost is your Mac or PC.
To your FastAPI Container: localhost is itself. It can’t even see MinIO at localhost. It has to use http://minio:9000.

When your Backend signs a URL, it uses its own perspective. It says, "I'm signing this for my buddy at minio:9000." It hands that URL to the Browser. The Browser tries to use it, but the Browser doesn't know who "minio" is. So you, the dev, change the URL string to localhost:9000. BOOM. The signature breaks.
S3 signatures are cryptographic hashes. They include the Host header. If you sign for minio but send to localhost, the "contract" is void. 403 Forbidden.

2. Why the "Real World" is Easier
In production, this problem vanishes. Why? Because s3.amazonaws.com is a Global Identity.
Whether you are a Lambda function in the cloud or a user in a Starbucks, s3.amazonaws.com points to the same place. The signature your backend creates is valid for the browser because the hostname never changes.

3. The "Senior Dev" Move: The Proxy Pattern
When you realize you're fighting the laws of physics (or at least Docker networking), you stop trying to make A → B → C happen. You switch to the Proxy Pattern.
The Flow:
  ```
      Frontend sends the file to the Backend (POST /upload).
  
      Backend receives the file and pushes it to MinIO internally.

  
    Why this is the "Industry Secret":
  
  
      No Signatures: Your backend talks to MinIO over a private, trusted Docker network. No need for complex crypto-signing.
  
      No CORS: Your browser only ever talks to your API. You don't have to fight MinIO's CORS policies.
  
      Security: Your S3 bucket stays 100% private. The outside world never even sees it.
```

4. The Moral of the Story
Junior devs spend three days trying to "fix the signature." Senior devs spend 10 minutes realizing the network topology is the bottleneck and just move the data through the backend.
In production, go direct to S3. In local Docker? Save your sanity—use a proxy.

