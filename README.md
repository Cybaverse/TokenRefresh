# Bearer Token Refresh

 
Clean simple and easy to use tool to manage short validity authorisation tokens
![image](https://user-images.githubusercontent.com/110976090/188850493-57bcff7b-87c0-4e0e-a573-087d3bfc49ca.png)

The extension checks the response for an ‘expired token’ message and if present in the response will reach out to an endpoint and request a new token. All subsequent requests made will have the new token automatically used instead of the old expired token. Populate the required fields (shown below) and click on ‘Start Token Refresher’ – that’s it!

# Settings

![image](https://user-images.githubusercontent.com/110976090/188850663-65f4f1f7-c2f8-49c3-a89f-a74fb323483d.png)

These setting are by default set up for OAuth, however can be tweaked for any header based authorisation schema i.e. X-AUTH-USER : or ACCESS-TOKEN : 

- Access Token Regex – regex to extract the token from the authentication endpoint response
- Bearer Token Error – regex to identify in a response the error message when the token is expired
- Authorisation Endpoint  - endpoint you access to get a new token 
- Authorisation Header : header that is part of the request that contains the current token
- Replacement Header : header to replace Authorisation Header with


![image](https://user-images.githubusercontent.com/110976090/188850742-662d23f4-afe4-40c1-844f-da93660cc7ba.png)


- User Agent – user agent to be sent - useful if testing mobile endpoints
- Content Type – allows you to set the content type if required (some auth endpoints require JSON for example)
- Auth Data – data that you submit to get a token – normally a username and password or other form of credentials
- Is B64 – legacy. However if you have an issue with odd characters that you can’t copy and paste in the Auth Data box for some reason you can base64 encode your request and ticking this box will decode it prior to sending it.


![image](https://user-images.githubusercontent.com/110976090/188850888-f4063f22-7a6a-4360-aa20-efc31ae38cdd.png)


- Enable Debugging  - gives you a much more verbose output so you can troubleshoot regex queries or error messages connecting :
- Clear log – clear the logs! 

![image](https://user-images.githubusercontent.com/110976090/188851018-66795705-8bbe-4bd8-8725-c743547738f4.png)

- Start Token Refresher – Start the extension
- In Scope Only : restrict actions to only in scope URL
- Refresh Token – Manually obtain a new token



