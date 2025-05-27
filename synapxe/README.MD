[root@sgctcrhclient02 ~]# yum install ansible-core
Updating Subscription Management repositories.
Last metadata expiration check: 2:40:06 ago on Tue 06 May 2025 07:38:38 PM EDT.
Dependencies resolved.
=============================================================================================================================
 Package                          Architecture    Version                    Repository                                 Size
=============================================================================================================================
Installing:
 ansible-core                     x86_64          2.16.3-2.el8               rhel-8-for-x86_64-appstream-rpms          3.6 M
Installing dependencies:
 python3.12                       x86_64          3.12.8-1.el8_10            rhel-8-for-x86_64-appstream-rpms           31 k
 python3.12-cffi                  x86_64          1.16.0-2.el8               rhel-8-for-x86_64-appstream-rpms          299 k
 python3.12-cryptography          x86_64          41.0.7-1.el8               rhel-8-for-x86_64-appstream-rpms          1.2 M
 python3.12-libs                  x86_64          3.12.8-1.el8_10            rhel-8-for-x86_64-appstream-rpms           10 M
 python3.12-pip-wheel             noarch          23.2.1-4.el8               rhel-8-for-x86_64-appstream-rpms          1.5 M
 python3.12-ply                   noarch          3.11-2.el8                 rhel-8-for-x86_64-appstream-rpms          134 k
 python3.12-pycparser             noarch          2.20-2.el8                 rhel-8-for-x86_64-appstream-rpms          145 k
 python3.12-pyyaml                x86_64          6.0.1-2.el8                rhel-8-for-x86_64-appstream-rpms          203 k
 sshpass                          x86_64          1.09-4.el8                 rhel-8-for-x86_64-appstream-rpms           30 k

Transaction Summary
=============================================================================================================================
Install  10 Packages



[root@sgctcrhclient02 ~]# ansible-galaxy collection install f5networks.f5_modules
Starting galaxy collection install process
Process install dependency map
Starting collection install process
Downloading https://galaxy.ansible.com/api/v3/plugin/ansible/content/published/collections/artifacts/f5networks-f5_modules-1.35.0.tar.gz to /root/.ansible/tmp/ansible-local-2743232088eq81x/tmpvwckdncq/f5networks-f5_modules-1.35.0-82yu9nu3
Installing 'f5networks.f5_modules:1.35.0' to '/root/.ansible/collections/ansible_collections/f5networks/f5_modules'
Downloading https://galaxy.ansible.com/api/v3/plugin/ansible/content/published/collections/artifacts/ansible-netcommon-8.0.0.tar.gz to /root/.ansible/tmp/ansible-local-2743232088eq81x/tmpvwckdncq/ansible-netcommon-8.0.0-fimaegrr
f5networks.f5_modules:1.35.0 was installed successfully
Installing 'ansible.netcommon:8.0.0' to '/root/.ansible/collections/ansible_collections/ansible/netcommon'
Downloading https://galaxy.ansible.com/api/v3/plugin/ansible/content/published/collections/artifacts/ansible-utils-6.0.0.tar.gz to /root/.ansible/tmp/ansible-local-2743232088eq81x/tmpvwckdncq/ansible-utils-6.0.0-y138ck1w
ansible.netcommon:8.0.0 was installed successfully
Installing 'ansible.utils:6.0.0' to '/root/.ansible/collections/ansible_collections/ansible/utils'
ansible.utils:6.0.0 was installed successfully
[root@sgctcrhclient02 ~]#
[root@sgctcrhclient02 ~]#




- name: Collect and Email F5 BIG-IP Device Info
  hosts: f5
  gather_facts: no
  collections:
    - f5networks.f5_modules
    - community.general
  vars:
    f5_user: admin
    f5_password: "{{ vault_f5_password }}"
    f5_server: "{{ inventory_hostname }}"
    gmail_user: your_email@gmail.com
    gmail_app_password: "{{ vault_gmail_password }}"

  tasks:
    - name: Collect BIG-IP device info
      bigip_device_info:
        server: "{{ f5_server }}"
        user: "{{ f5_user }}"
        password: "{{ f5_password }}"
        validate_certs: no
      register: f5_device_facts

    - name: Write HTML file from BIG-IP info
      template:
        src: templates/f5_device_info_template.j2
        dest: "/tmp/f5_device_info_{{ inventory_hostname }}.html"
      vars:
        f5_info: "{{ f5_device_facts.ansible_facts }}"

    - name: Email BIG-IP info HTML report
      community.general.mail:
        host: smtp.gmail.com
        port: 587
        username: "{{ gmail_user }}"
        password: "{{ gmail_app_password }}"
        to: "{{ gmail_user }}"
        subject: "BIG-IP Device Info - {{ inventory_hostname }}"
        subtype: html
        body: "{{ lookup('file', '/tmp/f5_device_info_' + inventory_hostname + '.html') }}"
      delegate_to: localhost


<!DOCTYPE html>
<html>
<head>
    <title>F5 BIG-IP Device Info</title>
    <style>
        body { font-family: Arial, sans-serif; }
        h2 { color: #2c3e50; }
        pre {
            background-color: #f4f4f4;
            padding: 10px;
            border-radius: 5px;
            overflow-x: auto;
        }
    </style>
</head>
<body>
    <h1>BIG-IP Device Info - {{ inventory_hostname }}</h1>
    {% for key, value in f5_info.items() %}
        <h2>{{ key }}</h2>
        <pre>{{ value | to_nice_json }}</pre>
    {% endfor %}
</body>
</html>


Jinja2 Template: templates/f5_device_info_template.j2



