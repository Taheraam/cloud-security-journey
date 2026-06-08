1. IAM User vs. IAM Role (In My Own Words)
IAM User: Think of this as a permanent physical passport belonging to a specific person or long-running application. It has long-term credentials (a password or a static pair of Access Keys) that stay valid until someone manually deletes them. It represents a single, persistent identity.

IAM Role: Think of this as a temporary, specialized visitor badge. It doesn’t belong to anyone permanently. Instead, a human user, an EC2 instance, or a Lambda function can dynamically "put it on" (assume the role) to grab temporary security credentials that automatically expire in a few hours. It’s an identity meant to be adopted on the fly.

2. What Surprised Me About Policy Evaluation Logic
What caught me off guard at first was how silent and aggressive the Default Deny rule is.

I didn't realize that if you don't explicitly say Allow, AWS treats it exactly like a Deny. It forces you to completely flip your default mindset. You can't just build architectures and fix permissions as mistakes happen; you have to know exactly what API calls your application makes before deployment, or everything will flawlessly block you from the start.

The fact that an Explicit Deny anywhere instantly overrides a million Allows across completely separate policies is incredibly powerful, but it means a single rogue line of JSON can break an entire system's connectivity.

3. Resource-Based vs. Identity-Based Policies
Identity-Based Policies: I use these when I want to answer: "What can Taher do?" You attach these directly to the user or role to dictate their personal permissions across the cloud.

Resource-Based Policies: I use these when I want to answer: "Who is allowed to touch this specific object?"

The Best Use Case: Cross-account access or public sharing. If I have an S3 bucket or an encryption key in Account A, and a Lambda function in Account B needs to read it, it's infinitely easier to write a resource-based policy directly on the bucket saying "Allow Account B to read me" rather than trying to choreograph complex role-assuming structures across account boundaries.

4. One Real-World Mistake I Will Avoid From Now On
Using Wildcards (*) out of sheer convenience.

Before this week, if a script needed to read an S3 bucket, it was tempting to just drop Action: "s3:*" and Resource: "*" into the policy to get the code working quickly. Now I see that as a critical security vulnerability. If an attacker exfiltrates those access keys, they have a master key to wipe or steal everything in my entire S3 footprint. From now on, I'm sticking strictly to scoping down the exact actions (e.g., s3:GetObject) and naming the exact bucket ARN.