# nginx-installation-using-ansible-playbook
here you will able to install nginx and create your own HTML page and deploy locally on port 80

**Goal:**
----------
✅ Install Nginx
✅ Create a custom HTML page
✅ Serve it on port 80

**Folder Structure:**

nginx-setup/
├── web_inv      # inventory file
|___ansible.cfg
├── nginx-main.yml
└── files/
    └── index.html

**Creatre Inventory file**
**web_inv**
[web-server]
node2 ansible_host=192.168.30.129 ansible_user=ansible-admin

**--------------------------------------------------------------**

**Create ansible.cfg**
[defaults]
inventory = web_inv

**--------------------------------------------------------------**

**Custom HTML Page**
**files/index.html**

<!DOCTTYPE html>
<html>
    <head>
        <title> Welcome to Nginx Webserver 1 </title>
    </head>
<body>
    <h1> Nginx installation via Ansible Playbook </h1>
    <p> running on CentOS 9v </p>
    <p> This is Webserver 1</p>
    <p> on Port no: 80 </p>
</body>
</html>

**--------------------------------------------------------------**

**Ansible Playbook:**
nginx-main.yml
---
- name: My First Webserver Instalation nginx
  hosts: web-server
  gather_facts: false
  become: yes # root access

  tasks: 
    - name: Install EPL Repostory
      ansible.builtin.yum:
        name: epel-release
        state: present
    
    # install nginx 
    - name: Install NGINX PKG
      ansible.builtin.yum:
        name: nginx
        state: present
      
    - name: Copy the index file 
      ansible.builtin.copy:
        src: files/index.html
        dest: /usr/share/nginx/html/index.html
        owner: root
        mode: '0777'

    - name: Enable the nginx and start the sevice
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: yes

    # FIREWALL - ALLOW PORT 80 to acces web-page
    - name: Allow http port 80 on FIREWALL
      ansible.builtin.firewalld:
        service: http
        permanent: yes
        state: enabled
        immediate: yes
      when: ansible_facts['os_family'] == 'RedHat'

# code on github account
  ## https://github.com/ansiblesikho/nginx-installation-using-ansible-playbook/edit/main/README.md
...

**--------------------------------------------------------------**
 **Run the Playbook**
ansible-playbook nginx-main.yml

 **--------------------------------------------------------------**
 **Check Webpage**
 Enter Target Node IP address
 http://192.168.30.129:80





