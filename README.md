# Brute-Force-Using-Hydra-On-Login-Page

Bruteforce web based login with hydra
Hydra supports some bruteforcing service as i mentioned earlier, one of them is used to bruteforce web based logins such as, social media login form, user banking login form, your router web based login, etc. That “http[s]-{get|post}-form” which will handle this request. In this tutorial i am going to show you how to bruteforce vulnerable web logins. Before we fire up hydra we should know some needed arguments such below:

Target : http://testasp.vulnweb.com/Login.asp?RetURL=%2FDefault%2Easp%3F
Login username : admin (if you don’t sure, bruteforce this)
Password list : “The location of dictionary file list containing possible passwords.”
Form parameters : “for general, use tamper data or proxy to obtain form of request parameters. But here im using iceweasel, firefox based, network developer toolbar.”
Service module : http-post-form.

Obtaining post parameters using browser, iceweasel/firefox
In your firefox browser press keys ‘CTRL + SHIFT + Q‘. Then open the web login page http://testasp.vulnweb.com/Login.asp?RetURL=%2FDefault%2Easp%3F, you will notice some text appear on the network developer tab. It tells you what files are transfered to us. See the method all are GET, since we have not POST any data yet.

To obtain the post-form parameters, type whatever in the username and or password form. You will notice a new POST method on the network developer tab. Double click on that line, on the “Headers” tab click “Edit and Resend” button on right-side. On the Request Body copy the last line, such as “tfUName=asu&tfUPass=raimu”. the “tfUName” and “tfUPass” are parameters we need. 

Kali linux has bunch of wordlists, choose the appropriate wordlist or just use rockyou.txt place in /usr/share/wordlists/ as seen below:

Alright, now we got all arguments we need and ready to fire up hydra. Here is the command pattern:

hydra -l <username> -P <password list> <Target hostname> <service module> <post request parameters>[/code]
  
Finally, based on information we have gathered, our commands ahould look something like this:

<h1>
hydra -l admin -P /usr/share/wordlists/rockyou.txt testasp.vulnweb.com http-post-form "/Login.asp?RetURL=%2FDefault%2Easp%3F:tfUName=^USER^&tfUPass=^PASS^:S=logout" -vV -f </h1>

Let’s break down the commands:

l <username> : is a word containing username account, use -L <FILE> to refer list of possible user name in a file.
P <FILE> : is a file list of possible password, use -p <password> to literally use one word password instead of guess it.
testapp.vunlwebapp.com : is a hostname or target
http-post-form : is the service module we use
“/Login.asp?RetURL=%2FDefault%2Easp%3F:tfUName=^USER^&tfUPass=^PASS^:S=logout” = the 3 parameters needed, the syntax is :
{page URL}:{Request post body form parameters}:S={Find whatever in the page after succesfully logged in}
v = Verbose mode
V = show login:pass for each attempt
f = Terminate program if pair login:password is found
  
Now lets let hydra try to break the password for us, it needs time since it is a dictionary attack. Once you succeded finding a pair of login:password hydra will immediately terminate the job and show the valid credential.

There is so much that hydra could do, since in this tutorial we just learned how to bruteforce web based logon using hydra, we only learn one protocol, that is http-post-form protocol. We can also use hydra against another protocol such ssh, ftp, telnet, VNC, proxy, etc.

Note :- See attached files and Screemshots.
