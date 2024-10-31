



# Ansible

Ansible is used to automate IT tasks. Multiple tasks can be combined into a single file called a playbook. The inventory file contains the destination IP addresses and can be referenced from the playbook file to group multiple IPs into domains.

## Running an Ansible Playbook

To run an Ansible playbook, use the following command (if the host file is not in the same directory):

ansible-playbook -i host file.yaml

Here is sample of ansible-playbook file

```bash

- name: playbook                      # Name of the playbook or the execution
  hosts: servers                      # Host file has details of IPs; make it [servers] and list the IPs below
  tasks:
    - name: Copying files from host to destination servers    # Name of the task
      copy:                                                   # for copy file/dir Use copy 
          src: /root/deploy.tar.gz
          dest: /opt/deploy.tar.gz

    - name: Creating a directory                             # Name of the task
      file:                                                  # for creating file/dir we use file 
          path: /opt/myapp
          state: directory

    - name: Unarchive file                                   # Name of the task
      unarchive:                                             # for unarchive file we use unarchive 
          src: /opt/deploy.tar.gz
          dest: /opt/myapp

    - name: Make the installer script executable             # Name of the task
      file:                                                  # Make permission changes in any file
          path: /opt/myapp/deploy/deployer.sh
          mode: '0755'                                       

     - name: Install apache2 on the web server                # Name of the task
       apt:                                                   # apt package manager
        pkg:                                                  
        - apache2                                             # Package name
        - php
        state: present                                        # State could be define installation/upgradation/uninstallation

      - name: Upgrade all packages to the latest version      # Name of the task
         apt:                                                 # apt package manager
          name: "*"                                           # all packages will be upgrade
          state: latest


    - name: Run script shell file                            # Name of the task
      shell: /opt/myapp/deploy/deployer.sh                   # Want to use shell in remote servers
      register: installer_output                             # assigning a variable
      become: true

    - name: Debug and show the variable installer_output     # Name of the task
      debug:                                                 # to get output of playbook result we use debug but we need to assign a variable before it use
          var: installer_output                              # To get the result
