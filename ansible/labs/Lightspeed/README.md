# Lab for Ansible LightSpeed

## 1. Configure VSCode with Git repository

git config --global user.email y*********@gmail.com
git config --global user.name Y*********

## 2. Enable Ansible Lightspeed

## 3. Create sample YAML file and push it in repository


---
- hosts: all
  name: Web application

  tasks:
    - name: Deploy HTTPD package
      ansible.builtin.dnf:
        name: httpd
        state: present

    - name: Enable firewall service
      ansible.posix.firewalld:
        service: http
        state: enabled
        immediate: true
        permanent: true

    - name : Deploy index.html template
      ansible.builtin.copy:
        src: index.html
        dest: /var/www/html/index.html

    - name: start HTTP service
      ansible.builtin.service:
        name: httpd
        state: started
        enabled: true

    - name: Curl localhost website and register
      ansible.builtin.uri:
        url: http://localhost
        return_content: true
      register: curl_result

    - name: display curl_result
      ansible.builtin.debug:
        var: curl_result

    - name: display host ip url
      ansible.builtin.debug:
        var: ansible_default_ipv4.address

## 4. Create new Git credential

## 5. Create new Git project

## 6. Create new job temple

## 7. Lunch create automation