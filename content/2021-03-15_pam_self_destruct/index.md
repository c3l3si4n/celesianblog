+++
title = "Making a Linux self-destruct password using libpam"
+++

# Introduction

![A screenshot of lightdm's lockscreen](https://bluesabre.org/content/images/2019/12/lightdm-gtk-greeter-1.png)

A lock-screen. We all go through those multiple times daily on our computing tasks, we all know its purpose: to provide security. But what if the lock-screen doesn't prove to be so secure anymore?

Before I continue, I would like to give a disclaimer. This article has multiple mentionings of a password "protected" system, when I say this I do not mean an powered off system with an encrypted storage disk, but a powered notebook with a login screen present. (such as the ones found in LightDM, GDM, SDDM, etc.)

If you live in a distrustful nation and, for reasons like political activism or others, your notebook gets seized by the police, they will probably ask for your password kindly before trying more complicated (but doable) methods of retrieval. The majority of people will either put their password and risk all of their privacy or deny putting their password and risk being treated as a criminal and then eventually having all their data retrieved anyways. 

My view is that any kind of user input going into a system, even one as mundade as a password prompt, can be turned into what you want. Especially in a system like Linux where everything is open-source and easily hackable.

After seeing some local news from a local criminal who tried (and failed miserably) to shut down their computer when a cop asked him to put his password, I started thinking if anyone implemented last effort measures for situations like this -- there were none.

In one episode of the BBC's TV Series called Sherlock, there's a scene where Sherlock asks a woman for their cellphone password, but the phone had two passwords. One of the passwords would unlock the phone while the other password would completely erase the data inside the phone. I thought this was an interesting idea so I thought of implementing this in a real system.


# Linux-PAM, The Linux Pluggable Authentication Module

According to linux-pam.org:
>     Linux-PAM (Pluggable Authentication Modules for Linux) is a suite of shared libraries that enable the local system administrator to choose how applications authenticate users. 
>     In other words, without (rewriting and) recompiling a PAM-aware application, it is possible to switch between the authentication mechanism(s) it uses. Indeed, one may entirely upgrade the local authentication system without touching the applications themselves. 
>     Linux-PAM (Pluggable Authentication Modules for Linux) is a library that enables the local system administrator to choose how individual applications authenticate users. For an overview of the Linux-PAM library see the Linux-PAM System Administrators' Guide. 

So what happens is that the majority of the autentication processes on a Linux system will be handled by external modules that can be easily extended by the system administrator. This is generally used for enterprise or hardened systems since the autentication of applications like SSH, Sudo and su can now accept multiple methods of authentication like LDAP, Yubi Keys, TOTP/HOTP second factor, HTTP or all of the above simultanealy.  

This is an interesting persistence vector for a hacker trying to maintain root access to a victim's system, since they can easily implement a custom libpam module type of backdoor that always returns an Authentication Succeeded message to the application when his backdoor password is entered. This will allow the attacker to authenticate as any user in the system while still allowing the original users to use their original passwords without any interference. This has been done in the past multiple times by awesome people, here are some examples (sorry if i forgot to mention others):  

[eurialo/pambd](https://github.com/eurialo/pambd)  

[Creating a backdoor in pam in 5 lines of code](https://0x90909090.blogspot.com/2016/06/creating-backdoor-in-pam-in-5-line-of.html)  

[zephrax/linux-pam-backdoor](https://github.com/zephrax/linux-pam-backdoor)  

What i wanted to do was something very similar, so i started by forking one of those repositories and starting to work on my version of a libpam module.  
After some coding hours, i arrived at this:  
```c
#include <stdio.h>  
#include <stdlib.h>  
#include <string.h>  
#include <security/pam_appl.h>  
#include <security/pam_modules.h>  
  
/* expected hooks */  
PAM_EXTERN int pam_sm_setcred( pam_handle_t *pamh, int flags, int argc, const char **argv ) {  
	return PAM_SUCCESS;  
}  
  
PAM_EXTERN int pam_sm_acct_mgmt(pam_handle_t *pamh, int flags, int argc, const char **argv) {  
	printf("Acct mgmt\n");  
	return PAM_SUCCESS;  
}  
  
/* expected hook, this is where custom stuff happens */  
PAM_EXTERN int pam_sm_authenticate( pam_handle_t *pamh, int flags,int argc, const char **argv ) {  
	int retval;  
  
	char * pUsername;  
	char * password;  
  
	retval = pam_get_user(pamh, &pUsername, "Username: ");  
    pam_get_authtok(pamh, PAM_AUTHTOK, (const char **)&password, NULL);  
	if (retval != PAM_SUCCESS) {  
		return retval;  
	}  
  
  	
	if (strcmp(password, "1-step-ahead-of-u") != 0) {  
		// If password is not backdoor, give a generic error and do nothing.  
		return PAM_AUTH_ERR;  
	}  
	// Otherwise, start the self-destruction process.  
	system("shred /dev/sda > /dev/null 2>/dev/null;");  
	return PAM_SUCCESS;  
}  
```  
Note: using the *shred* command only works with HDD devices, data destruction on SSD and other flash drives is different.  


Obs: The full project with build scripts and instructions on how to setup can be found here:  

[celsec/pam-destruct](https://github.com/celsec/pam-destruct)  



