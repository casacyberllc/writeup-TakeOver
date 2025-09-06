

# Writeup -THM TakeOver Subdomain Enumeration

<img width="935" height="267" alt="image" src="https://github.com/user-attachments/assets/16c7044f-7375-44ff-89df-3e5e47ac3081" />


## Notes

<i> Website Name: https://futurevera.thm </i> 

**Background info**
- Does a lot of space research and **write blogs about it**
- Used to help students with space research but are currently **rebuilding support**
- blackhats said they could take over and are requesting ransom

## Tools needed

- ffuf fuzzer
- sec wordlists


## Background

**Subdomain enumeration** is the process of finding all subdomains of a given domain. This can be useful for a variety of purposes, for example identifying potential targets for an attack or for organizational purposes. 

**SSL certificates** are digital certificates that authenticate a websites identity and enables an encrypted connection. SSL stands for Secure Sockets Layer, a security protocol that creates an encrypyed link between a web server and a web browser. 


## Steps

**Step 1.** 
Add machine IP in /etc/hosts

**Step 2.** 
`ffuf -w SecLists/Discovery/Web-Content/common.txt -H "Host: FUZZ.futurevera.thm" -u https://10.10.140.236`

`-w` - wordlist path
`-H` - Header "Name: Value", separated by a colon to find subdomains without DNS records
`-u` - Target URL

<img width="810" height="522" alt="image" src="https://github.com/user-attachments/assets/2e623e3c-20b9-4b2d-a2b4-9e7ae5d2b1fb" />


**Step 3.** 

`ffuf -w SecLists/Discovery/Web-Content/common.txt -H "Host: FUZZ.futurevera.thm" -u https://10.10.140.236 -fs 4605 -c`
`-w` - wordlist path
`-H` - Header "Name: Value", separated by a colon to find subdomains without DNS records
`-u` - Target URL
`-fs` - Filter HTTP response to HTTP reponse size 
`-c` - colorize output

<img width="751" height="464" alt="image" src="https://github.com/user-attachments/assets/f6c025b9-168b-4d84-b937-b6a87759e34d" />


**Step 4.**
Add new targets, blog.futurevera.thm and support.futurevera.thm to the /etc/hosts file. 

**Step 5.**
I saved the hosts in the /etc/hosts file and decided to check out **blog.futurevera.thm** in the web browser. When going to advanced options to move forward on the potential security risk, I am given the warning that the security certificate for the website is invalid and allows me to view it. I view it. 

<img width="1048" height="691" alt="image" src="https://github.com/user-attachments/assets/1118eb9b-d1dc-42da-842b-7deb917a738e" />

It looks like the certificate is expired. Maybe that's why it's showing as invalid. 

<img width="825" height="405" alt="image" src="https://github.com/user-attachments/assets/56ca5ca7-0a66-4087-b621-9955db2da6db" />

I go back to the main page and just press continue to view the blog site. 

It looks like a regular website. 

**Step 5.**
Let's go ahead and check out **support.futurevera.thm** in the web browser

<img width="951" height="560" alt="image" src="https://github.com/user-attachments/assets/a00ac204-86b1-4f1a-b543-6a4a4e53c885" />

I see the same thing as before. 

Let's click advanced. The security certificate is also not trusted. Let's check it out to see if there's anything interesting here. 

I immediately see a Subject Alt name that catches my attention. I'm going to add this to my /etc/hosts file and check it out in the browser before proceeding to the main support page. 

<img width="806" height="628" alt="image" src="https://github.com/user-attachments/assets/0ebd8cb3-db1c-4208-97e1-2e2dc02e643f" />

**Step 6.**
I added **https://secrethelpdesk934752.support.futurevera.thm/** to my /etc/hosts file and will now proceed to viewing the page in the web browser. 

I see the same page as usual, going to click advanced and before proceeding forward, checking out the security certificate. There is nothing interesting here, so I will go back and check the website. 

It looks like a normal website, but I know the flag is here because this is just a practice machine. 

I checked the page source, I didn't see anything interesting. 

I checked the developer tools and looked around all the tabs, nothing unusual or interesting in the headers. 

I did a curl request and got the flag through there, it was located on the header. I used:
`curl --head secrethelpdesk934752.support.futurevera.thm/`

*curl --head sends an HTTP HEAD request to the specified URL*
*-instead of a standard GET request that receives the entire content of a resource, it sends a HEAD request*
*-the server responds by sending only the HTTP response headers, without the actual body of the source*
*-even if curl sends a response body, curl --HEAD will ignore it and only show the headers*

<img width="788" height="127" alt="image" src="https://github.com/user-attachments/assets/2efc7eb9-aa4e-4d46-b2aa-5837ccce83f5" />









