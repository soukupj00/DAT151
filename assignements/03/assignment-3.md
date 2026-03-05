# DAT151: Assignment 3 Report

**Group:** 2

**Group Members:** Soukup Jan, Fabienne Feilke

**Date:** March ..., 2026

---

## Task 1: SELinux

*Objective: Work with SELinux contexts, users, domains, and policies.* 

### 1a: SELinux Status

*

**Task:** Check whether SELinux is currently enabled and make sure it is in targeted and enforcing mode.

* **Commands Used:**
* `[Insert command here]`


* **Terminal Output / Printout:**

```bash
[Paste output here]

```

* **Explanation:**
* [Explain what the output means]

### 1b: Working with SELinux Users

* **1b.1 Mapping Linux users to SELinux users:**
*

**Task:** List the mapping between Linux users and SELinux confined users on your computer.

* **Commands & Output:**

```bash
[Paste output here]

```




* **1b.2 Available SELinux Users:**
* 
**Task:** List all available SELinux users on your computer using `seinfo`.


* **Commands & Output:**
```bash
[Paste output here]

```




* **1b.3 Restricting `su` and `sudo`:**
* 
**Task:** Create a new Linux user and use SELinux to prevent this user from using the `su` and `sudo` tools.


* **Commands & Output:**
```bash
[Paste output here]

```


* **Explanation of Method:**
* [Explain how SELinux capabilities were modified]





### 1c. 

Domain Transitions (systemd to Apache) 

* 
**Task:** Verify that an Apache web server started by systemd has read access to the content of `/var/www/html`. Note: For each step below, you must use the `sesearch` command and document the applicable policy rules.


* 
**1c.1 Is systemd allowed to start the Apache web server?** 


* 
*SELinux type of systemd:* [Insert type, e.g., init_t] 


* 
*SELinux type of Apache executable:* [Insert type] 


* 
*Is systemd allowed to run it?:* 


* 
**Commands & Policy Output (`sesearch`):** 


```bash
[Paste rule output here]

```




* 
**1c.2 Can the Apache web server run in domain `httpd_t`?** 


* 
**Commands & Policy Output (`sesearch`):** 


```bash
[Paste rule output here]

```




* 
**1c.3 Is systemd allowed a transition to `httpd_t`?** 


* 
**Commands & Policy Output (`sesearch`):** 


```bash
[Paste rule output here]

```




* 
**1c.4 Has domain `httpd_t` access to open and read files in `/var/www/html`?** 


* 
**Commands & Policy Output (`sesearch`):** 


```bash
[Paste rule output here]

```





### 1d. 

SELinux Boolean for `public_html` 

* 
**Task:** Use a SELinux boolean to allow Apache to read web content in a `public_html` directory in the home directory of users.


* **Commands & Output:**
```bash
[Paste output here]

```



### 1e. 

Custom Web Directory `/www` 

* 
**Task:** Create a directory `/www` and configure SELinux to allow Apache to read web content in this directory.


* **Commands & Output:**
```bash
[Paste output here]

```



---

## Task 2: Printing 

* 
**Task:** Install CUPS and all necessary dependencies, start the cups service, set it to run on startup, and add the HVL printing system.


* **Commands & Output (Installation & Setup):**
```bash
[Paste output here]

```


* 
**2.1 Print job from LibreOffice:** 


* *Screenshot or Result:* [Insert Image / Description]


* 
**2.2 Print job from command line (`lpr`):** 


* **Commands & Output:**
```bash
[Paste output here]

```





---

## Task 3: Open Ports and Processes 

* 
**Task:** Find all ports open for tcp and udp connections on your computer, and list the processes and services that use these ports.


* **Commands & Output:**
```bash
[Paste output here]

```


* **Explanation of Findings:**
* [Briefly summarize the open services]



---

## Task 4: Two-Factor Authentication (2FA) 

> 
> **Important:** Do not modify any of the files below `/etc/ssh/`; create new files with the modifications.
> 
> 

* 
**Task:** Set up 2FA for SSH login on your computer, using an SSH key and time-based one-time passwords. Set this up on all lab computers of the group.


* **Commands & Output (Setup):**
```bash
[Paste output here]

```


* 
**SELinux Context Verification:** * *Note:* The `google-authenticator` secret must be stored below `~/.ssh/` to get the correct SELinux context.


* **Command showing correct context:**
```bash
[Paste output here]

```




* 
**Verification of Login:** * *Task:* Verify that you can log in from one computer to the other using SSH, and that 2FA is used for authentication.


* 
**Terminal Output (Showing `keyboard-interactive` prompt):** 


```bash
[Paste successful login flow here]

```





---

## Task 5: Secure your computer 

* 
**Task:** Use Fail2Ban to secure your computer. Set PAM requirements for password qualities, password history, and account locking if there are many consecutive authentication failures.



> 
> **Requirement:** Do not modify directly the PAM files; configure the PAM system using `authselect`.
> 
> 

* **Commands & Output (Fail2Ban Setup):**
```bash
[Paste output here]

```


* **Commands & Output (`authselect` PAM configuration):**
```bash
[Paste output here]

```



---

## Task 6: SSH 

* 
**Setup:** Create an SSH key pair as a normal user on your own computer and start an OpenSSH server on your lab computer.



6.1 Direct Login Attempt 

* 
**Task:** Try to log in to your lab computer from your own computer.


* **Commands & Output:**
```bash
[Paste output here]

```


* **Result & Explanation:**
* [Explain the result and why this happens].





6.2 Jump Host Login 

* 
**Task:** From your own computer, log into your lab computer with a jump through `eple.hvl.no`.


* **Commands & Output:**
```bash
[Paste output here]

```



6.3 MariaDB SSH Tunnel 

* 
**Task:** Install the MariaDB client tool on your own computer, set up an SSH tunnel through `eple.hvl.no` to the MariaDB server on your lab computer, and connect to it.


* **Commands & Output (Establishing Tunnel):**
```bash
[Paste output here]

```


* **Commands & Output (Database Connection):**
```bash
[Paste output here]

```
