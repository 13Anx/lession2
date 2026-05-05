Написать ansible-playbook
---
- name: Install and configure Nginx
  hosts: all
  become: true
  tasks:
    - name: Update all system packages
      # Использование модуля apt для Debian/Ubuntu систем
      ansible.builtin.apt:
        update_cache: true
        upgrade: dist
        cache_valid_time: 3600

    - name: Install Nginx package
      ansible.builtin.apt:
        name: nginx
        state: present

    - name: Ensure Nginx service is started and enabled
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: true

    - name: Change default Nginx index page
      ansible.builtin.copy:
        content: "<h1>Welcome to the Custom Nginx Page</h1>\n"
        dest: /var/www/html/index.nginx-debian.html
        owner: www-data
        group: www-data
        mode: '0644'
