---
- name: Install and configure Nginx web server
  hosts: all
  become: true
  tasks:
    - name: Update all system packages to the latest version
      ansible.builtin.apt:
        update_cache: true
        upgrade: dist
        cache_valid_time: 3600

    - name: Ensure Nginx is installed
      ansible.builtin.apt:
        name: nginx
        state: present

    - name: Create a custom index.html page
      ansible.builtin.copy:
        content: |
          <!DOCTYPE html>
          <html>
          <head>
              <title>Custom Nginx Page</title>
          </head>
          <body>
              <h1>Nginx has been configured via Ansible</h1>
          </body>
          </html>
        dest: /var/www/html/index.html
        owner: www-data
        group: www-data
        mode: '0644'

    - name: Ensure Nginx service is running and enabled
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: true
