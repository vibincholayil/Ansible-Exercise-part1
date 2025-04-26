# Ansible URL Availability Monitoring

This repository demonstrates the creation and execution of two Ansible roles designed to monitor the availability of specific URLs. The first role, check/tests the availability of a single URL, while the second, multiurl, checks the availability of multiple URLs.

## Table of Contents
- Exercise 1: Single URL Availability Check
- Exercise 2: Multiple URL Availability Check

## Exercise 1: Single URL Availability Check

Create an Ansible role named check_vibin_url to test the availability of the URL: https://www.stackoverflow.com/.

### Setup  
**Ansible Environment Configuration:** Set up an Ansible environment with one control server and two managed nodes.

Navigate to the project directory:  
```
ansible@server:~$ cd dev
ansible@server:~/dev$ pwd
/home/ansible/dev
```
SSH into the managed nodes:
```
ansible@server:~/dev$ ssh node1
ansible@server:~/dev$ ssh node2
```
Simulate Negative Test Case: On node2, update the inbound rules to block traffic on ports 80 and 443, simulating an unreachable URL scenario.
![image](https://github.com/user-attachments/assets/6faab780-02b1-4f65-85fe-99de4d500123)

**Create Ansible Role:**  
Create the singleurl role to test the availability of the specified URL.  
![image](https://github.com/user-attachments/assets/512f2e8b-1c9f-458d-9836-c8fc3609c00c)  
My JSON script now outputs a difference in the "status": 200 field. Based on this outcome, I will move forward with creating a complete YAML file and keep this result as reference.  
![image](https://github.com/user-attachments/assets/b19dd7d9-8b21-4c28-8d12-c403f102a9f5)  
Output  
![image](https://github.com/user-attachments/assets/138461da-84c3-43ba-ab5b-45dc105056d4)  

### Conclusion
This exercise demonstrates how to create an Ansible role to check the availability of a single URL, handle different scenarios, and output the results accordingly.

## Exercise 2: Multiple URL Availability Check  
Extend the functionality to check the availability of multiple URLs.

### Setup
Create multiurl Role:  
Create a new role named multiurl to check the availability of multiple URLs.

First, I created a single URL check script inside a roles directory named singleurl with the help ansible-playbook galaxy. 
Define URLs in defaults/main.yml:  
![image](https://github.com/user-attachments/assets/a27ee69d-69b8-4854-a11e-e5787a8a16c4)  

Add a YAML task in tasks/main.yml using the uri module to check if the URL is reachable.
```
---
- name: check url is reachable or not
  uri:
   url: "{{ url }}"
- name: print the {{ url }} is working
  debug:
   msg: "{{ url }} is working and reachble"
```

Then, I created a single_url_monitoring.yml YAML file outside the role directory.  
```
vim /home/ansible/dev/singleurltesting.yml
```

And added the following commands:  
```
---
- name: test the url reachablity
  hosts: all
  roles:
   - singleurl
```

Now, when I run singleurltesting.yml, it displays a message confirming that Google is reachable from both nodes.  
![image](https://github.com/user-attachments/assets/ea451f70-3690-4672-9748-b114fbff7033)  

Now, I need to proceed to the second step. I have created another directory inside roles called multiurl using the ansible-galaxy command. Next, I navigated to the meta directory and opened main.yml. In this file, I added a list of dependencies, linking the role to singleurl.  
![image](https://github.com/user-attachments/assets/5c22fd4d-a394-4497-916f-5314e2ced1d6)  
![image](https://github.com/user-attachments/assets/cff8859a-89b5-4849-875b-4c2bee1dad6c)  

Inside the tasks directory, I wrote the following command:
```
---
- name: all urls has been tested and working fine.
  debug:
   msg: "url working fine"
```

I exited the role directory, wrote the code for multiurl, and then ran it.  
```
---
- name: test the url reachablity
  hosts: all
  ignore_errors: true
  roles:
   - multiurl
```
```
ansible-playbook muiltiurltesting.yml
```
![image](https://github.com/user-attachments/assets/6290af56-e8c3-48a7-9c42-e100c323f3d3)  

Yeah! It has now successfully checked all the URLs I tested!

## Conclusion
This exercise demonstrates how to extend the functionality to check the availability of multiple URLs using Ansible roles and handle different scenarios accordingly.  
By completing these exercises, I have learned how to create Ansible roles to check the availability of URLs. Handle different scenarios, including reachable and unreachable URLs.Output the results of the URL reachability tests. Feel free to customize these roles and playbooks to suit your specific requirements. Learn together!
