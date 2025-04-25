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

Create Ansible Role:

Create the check_vibin_url role to test the availability of the specified URL.

Define URL in defaults/main.yml:

yaml
Copy
Edit
---
url: https://www.stackoverflow.com/
Create Task in tasks/main.yml:

yaml
Copy
Edit
---
- name: Check if URL is reachable
  uri:
    url: "{{ url }}"
  register: url_status
  failed_when: url_status.status != 200

- name: Print URL reachability status
  debug:
    msg: "{{ url }} is reachable"
Create Playbook:

Create a playbook singleurltesting.yml to execute the role:

yaml
Copy
Edit
---
- name: Test URL reachability
  hosts: all
  roles:
    - check_vibin_url
Run the Playbook:

Execute the playbook to test the URL reachability:

bash
Copy
Edit
ansible-playbook singleurltesting.yml
Output
The playbook will output the status of the URL reachability. If the URL is reachable, it will display a message indicating the URL is working. If the URL is not reachable, it will fail the task.

Conclusion
This exercise demonstrates how to create an Ansible role to check the availability of a single URL, handle different scenarios, and output the results accordingly.

Exercise 2: Multiple URL Availability Check
Objective
Extend the functionality to check the availability of multiple URLs.

Setup
Create multiurl Role:

Create a new role named multiurl to check the availability of multiple URLs.

Define URLs in defaults/main.yml:

yaml
Copy
Edit
---
urls:
  - https://www.python.org
  - https://www.nodejs.org
  - https://www.djangoproject.com
  - https://www.github.com
  - https://www.gitlab.com
  - https://www.bitbucket.org
  - https://www.stackexchange.com
  - https://www.linkedin.com
  - https://www.salesforce.com
  - https://www.ibm.com
  - https://www.oracle.com
  - https://www.dropbox.com
  - https://www.adobe.com
  - https://www.slack.com
  - https://www.spotify.com
  - https://www.airbnb.com
  - https://www.booking.com
  - https://www.zomato.com
  - https://www.foodpanda.com
  - https://www.uber.com
Create Task in tasks/main.yml:

yaml
Copy
Edit
---
- name: Check if all URLs are reachable
  uri:
    url: "{{ item }}"
  register: url_status
  failed_when: url_status.status != 200
  with_items: "{{ urls }}"
  loop_control:
    loop_var: item

- name: Print URL reachability status
  debug:
    msg: "{{ item }} is reachable"
  with_items: "{{ urls }}"
Create Playbook:

Create a playbook multiurltesting.yml to execute the role:

yaml
Copy
Edit
---
- name: Test multiple URLs reachability
  hosts: all
  roles:
    - multiurl
Run the Playbook:

Execute the playbook to test the reachability of all URLs:

bash
Copy
Edit
ansible-playbook multiurltesting.yml
Output
The playbook will output the status of each URL's reachability. If a URL is reachable, it will display a message indicating the URL is working. If a URL is not reachable, it will fail the task for that specific URL.

Conclusion
This exercise demonstrates how to extend the functionality to check the availability of multiple URLs using Ansible roles and handle different scenarios accordingly.

Conclusion
By completing these exercises, you have learned how to:

Create Ansible roles to check the availability of URLs.

Handle different scenarios, including reachable and unreachable URLs.

Output the results of the URL reachability tests.

Feel free to customize these roles and playbooks to suit your specific requirements.

License
This project is licensed under the MIT License - see the LICENSE file for details.

This README.md template provides a comprehensive overview of your project, including objectives, setup instructions, and conclusions for each exercise. It follows best practices for GitHub documentation, ensuring clarity and ease of understanding for users and collaborators.


Sources


You said:
Create a first release README document for these files

ChatGPT said:
Certainly! Here's a polished and structured README.md tailored for the first release of your GitHub project, which demonstrates Ansible roles for URL availability monitoring:

