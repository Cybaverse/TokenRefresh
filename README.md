# Bearer Token Refresh

 
Clean, simple and easy to use tool to manage short validity authorisation tokens.

![image](https://user-images.githubusercontent.com/110976090/188850493-57bcff7b-87c0-4e0e-a573-087d3bfc49ca.png)

The extension checks the response for an ‘expired token’ message and if present in the response will reach out to an endpoint and request a new token. All subsequent requests will have the new token automatically used instead of the old expired token. Populate the required fields (shown below) and click on ‘Start Token Refresher’ – that’s it!

# Settings

![image](https://user-images.githubusercontent.com/110976090/188850663-65f4f1f7-c2f8-49c3-a89f-a74fb323483d.png)

These settings are by default set up for OAuth, however they can be tweaked for any header based authorisation schema i.e. X-AUTH-USER : or ACCESS-TOKEN : 

- Access Token Regex - regex to extract the token from the authentication endpoint response
- Bearer Token Error - regex to identify in a response the error message when the token is expired
- Authorisation Endpoint  - endpoint you access to get a new token 
- Authorisation Header - header that is part of the request that contains the current token
- Replacement Header - header to replace Authorisation Header with


![image](https://user-images.githubusercontent.com/110976090/188850742-662d23f4-afe4-40c1-844f-da93660cc7ba.png)


- User Agent - user agent to be sent - useful if testing mobile endpoints
- Content Type - allows you to set the content type if required (some auth endpoints require JSON for example)
- Auth Data - data that you submit to get a token – normally a username and password or another form of credentials
- Is B64 - legacy. However, if you have an issue with odd characters that you can’t copy and paste in the Auth Data box for some reason, you can base64 encode your request and ticking this box will decode it prior to sending it


![image](https://user-images.githubusercontent.com/110976090/188850888-f4063f22-7a6a-4360-aa20-efc31ae38cdd.png)


- Enable Debugging  - gives you a much more verbose output so you can troubleshoot regex queries or error messages connecting :
- Clear logs - clear the logs! 

![image](https://user-images.githubusercontent.com/110976090/188851018-66795705-8bbe-4bd8-8725-c743547738f4.png)

- Start Token Refresher – Start the extension
- In Scope Only : off - restrict actions to only in scope URL
- Refresh Token – Manually obtain a new token


#Walkthrough using DVWS

![image](https://user-images.githubusercontent.com/110976090/188860063-656f6e37-3179-4678-b0c0-8412b3e69fcf.png)

We log on using our valid credentials.

![image](https://user-images.githubusercontent.com/110976090/188860161-5e6f100e-d5c4-4e04-b6ba-38fc47e460a5.png)

Looking in Burp, this gives us most of the information that we require to set the tool up.

![image](https://user-images.githubusercontent.com/110976090/188860962-afbf7297-8612-4875-92f9-3ba0d5ece2d8.png)

We have the Authorisation Endpoint and data we need to send. and we can see that the token is returned to us as a token : "data". So we can set this up in the tool.

![image](https://user-images.githubusercontent.com/110976090/188861397-3be668bb-f9e5-4156-87a1-dea326f0fea8.png)

We can test that this works by enabling the extension and manually refreshing the token. 

![image](https://user-images.githubusercontent.com/110976090/188862108-4c34b9e7-6b17-4767-9839-1c6c3c47aff6.png)

As you can see, the new current token is displayed. In the event this doesn't work, you can enable debugging to try and troubleshoot the issue.

![image](https://user-images.githubusercontent.com/110976090/188862492-0b932000-81e1-41c5-9f39-9126e44a1f35.png)

Server errors will always be displayed regardless of debugging status (invalid username or password for DVWS generates a 401 error).

![image](https://user-images.githubusercontent.com/110976090/188862724-7c8a95f7-68ec-41e7-84f2-5254847c1baa.png)

Subsequent requests made show the correct format for the token when making requests.

![image](https://user-images.githubusercontent.com/110976090/188863667-1f155798-e738-4cd1-9d9b-97a0a7cc2d53.png)

In this instance, we don't need to make any changes in the tool but if the bearer token was sent using a different header (X-AUTH-USER for example), this is where we would change them - red denoted the header and green denotes the token. The tool requires the [TOKEN] to be present so it knows where to insert the new token.

![image](https://user-images.githubusercontent.com/110976090/188864169-84eb94c4-7292-41f0-8997-a671924c6039.png)

To simulate the token expiring, we are just going to remove it from our request and note the generated error.

![image](https://user-images.githubusercontent.com/110976090/188864517-c937ca0d-7ffe-4b4e-8fca-50722e89f639.png)

In this instance, we shall pick JsonWebTokenError as a nice unique value to search for (using regex) in the response to indicate that the token is no longer valid (it would be a different error message if it had expired, but this is for demonstration purposes). So we update that field in the tool, and we are good to go.

![image](https://user-images.githubusercontent.com/110976090/188865548-f9f9d09b-cf7d-4b02-a6fa-30a459c78a80.png)

Now we run the repeater request a second time, and now it is fully configured, the tool will automatically detect that the token is invalid and collect a new one for you. The below output is from the debugging window.
```Loading Bearer Token Refresh Tool by @bb_hacks
Based on https://github.com/t3hbb/OAuthRenew
Check for an expired bearer token and replace if required.

Remember to update any necessary details in the options above!
URL Requested :http://127.0.0.1:80/api/v2/notes
URL is in scope 
Processing Message 
URL Requested :http://127.0.0.1:80/api/v2/notes
URL is in scope 
Processing Message 
Response received
Bearer token expired - obtaining new one
Attempting to reach http://127.0.0.1/api/v2/login to re-authenticate

URL: http://127.0.0.1/api/v2/login
Headers sent: {'User-agent': u'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36'}
Body sent: username=user&password=user

Content received : {"token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoidXNlciIsInBlcm1pc3Npb25zIjpbInVzZXI6cmVhZCIsInVzZXI6d3JpdGUiXSwiaWF0IjoxNjYyNTQ5NzI1LCJleHAiOjE2NjI3MjI1MjUsImlzcyI6Imh0dHBzOi8vZ2l0aHViLmNvbS9zbm9vcHlzZWN1cml0eSJ9.CKtD0IFd67sgCNJ6yvbri566p8vH4oKqJWxS7OkIhC8","status":200,"result":{"admin":false,"_id":"63087ac4fc9d100020943f35","username":"user","password":"$2b$10$t7exJ4FimBZEqRTO1ECpzO7ZvAQmRK7ZV3H4f4HQzmxtSczrFq5i.","__v":0}}
Discovered response : token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoidXNlciIsInBlcm1pc3Npb25zIjpbInVzZXI6cmVhZCIsInVzZXI6d3JpdGUiXSwiaWF0IjoxNjYyNTQ5NzI1LCJleHAiOjE2NjI3MjI1MjUsImlzcyI6Imh0dHBzOi8vZ2l0aHViLmNvbS9zbm9vcHlzZWN1cml0eSJ9.CKtD0IFd67sgCNJ6yvbri566p8vH4oKqJWxS7OkIhC8"
Extracted Token : eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoidXNlciIsInBlcm1pc3Npb25zIjpbInVzZXI6cmVhZCIsInVzZXI6d3JpdGUiXSwiaWF0IjoxNjYyNTQ5NzI1LCJleHAiOjE2NjI3MjI1MjUsImlzcyI6Imh0dHBzOi8vZ2l0aHViLmNvbS9zbm9vcHlzZWN1cml0eSJ9.CKtD0IFd67sgCNJ6yvbri566p8vH4oKqJWxS7OkIhC8
New Bearer Token Acquired : ...eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoidXNlciIsInBlcm1pc3Npb25zIjpbInVzZXI6cmVhZCIsInVzZXI6d3JpdGUiXSwiaWF0IjoxNjYyNTQ5NzI1LCJleHAiOjE2NjI3MjI1MjUsImlzcyI6Imh0dHBzOi8vZ2l0aHViLmNvbS9zbm9vcHlzZWN1cml0eSJ9.CKtD0IFd67sgCNJ6yvbri566p8vH4oKqJWxS7OkIhC8...
URL Requested :http://127.0.0.1:80/api/v2/notes
URL is in scope 
Processing Message 
Replacing Bearer Token with latest obtained
New headers to be transmitted : GET /api/v2/notes HTTP/1.1
Host: 127.0.0.1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:105.0) Gecko/20100101 Firefox/105.0
Accept: application/json, text/plain, */*
Accept-Language: en-GB,en-US;q=0.7,en;q=0.3
Accept-Encoding: gzip, deflate
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyIjoidXNlciIsInBlcm1pc3Npb25zIjpbInVzZXI6cmVhZCIsInVzZXI6d3JpdGUiXSwiaWF0IjoxNjYyNTQ5NzI1LCJleHAiOjE2NjI3MjI1MjUsImlzcyI6Imh0dHBzOi8vZ2l0aHViLmNvbS9zbm9vcHlzZWN1cml0eSJ9.CKtD0IFd67sgCNJ6yvbri566p8vH4oKqJWxS7OkIhC8
DNT: 1
Connection: close
Referer: http://127.0.0.1/notes.html
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: same-origin

URL Requested :http://127.0.0.1:80/api/v2/notes
URL is in scope 
Processing Message 
Response received
Bearer token is valid
```
Now it has a new valid token, when the request is made, it is automatically updated and is successful.

![image](https://user-images.githubusercontent.com/110976090/188866593-3332c5db-f7ac-45f2-809f-e7f28904728a.png)







