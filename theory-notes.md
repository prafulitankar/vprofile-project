### What is Ansible?
bash
- Ansible is an opensource IT configuration management tool. It enables DevOps teams to define their infrastructure as a code in a
  simple and way.
- Most of peoples compare Ansible to similar tool like Puppet and Chef, they all use for automate and provision infrastructure,
  but due to some features Ansible are more preferable as compare to other.
- Ansible can also be considered for administrators and operations systems to ensure control over multiple servers.

### How Ansible Works?
![image](https://github.com/user-attachments/assets/43a421ef-859d-44df-a6a0-301ba7301838)

### Benefits of Ansible:
Benefits Of Ansible
- Simple – It is very simple to use and is supported by YAML.
- Agentless – There are no agents or software deployed on the clients/servers to work with Ansible. The connection can be done through the SSH or using the Python.
- Efficient – There are no servers, daemons, or databases required for Ansible to work.
- Performance- The Ansible’s performance is excellent and has very little latency.
- Secure and consistent – Since the Ansible uses SSH and Python it is very secure, and the operations are flawless.
- Reliable – The Ansible Playbook can be used to write programs or the modules and can be used to manage the IT without any downside.

### Master node requirements 
For your master node (the machine that runs Ansible), you can use any machine with Python newer version installed. This includes Red Hat, Debian, CentOS, macOS and so on.

### Managed node requirements 
Although you do not need a daemon on your managed nodes, you do need a way for Ansible to communicate with them. For most managed nodes, Ansible makes a connection over SSH and transfers modules using SFTP. For any machine or device that can run Python.

### Components Of Ansible 
1. Inventory
   - An inventory file is also sometimes called a “host-file”. Your inventory can information like IP address for each managed node.
   - Ansible works against multiple managed nodes or "hosts" in your infrastructure at the same time, using a list or group of lists           known as Inventory.
   - Inventory file is a Collection of hosts(nodes) which are managed by Ansible master node.
   - Hosts information can be defined in following ways.
      1. Default Location: /etc/ansible/hosts	
      2. Use -i option my_hosts.
   - Add private IP of nodes in inventory file.

 2. Ad-hoc command
   - An Ansible ad hoc command uses the /usr/bin/ansible command-line tool to automate a single task on one or more managed nodes.
     ad-hoc commands are quick and easy, but they are not reusable. 
   - Ad hoc commands are great for tasks you repeat rarely. For example, if you want to power off all the machines in your lab for
      Diwali vacation, you could execute a quick one-liner in Ansible without writing a playbook.
 3. Playbook:
   - A playbook is a text file written in YAML (YAML Ain’t Markup Language) format and is normally saved in “. yml” or “.yaml”.
   - The playbook begins with a line consisting of three dashes (---) as a start of document marker.
   - An item in a YAML list starts with a single dash (-) followed by a space.
   - Hosts & Tasks are mandatory items in a playbook.
   - The playbook primarily uses Indentation with Space character to indicate the structure of its data.
   - Modules are used to perform tasks.
   - Comment start with # symbol.

  4. Modules
   - Modules (also referred to as "task plugins" or "library plugins") are discrete units of code that can be used from the command
     line or in a playbook task.
   - Ansible executes each module, usually on the remote managed node, and collects return values.
   - Each module supports taking arguments. Nearly all modules take key=value arguments, space delimited. Some modules
     take no arguments, and the command/shell modules simply take the string of the command you want to run.
   - In short, we can say that 
       • Ansible module is reusable, standalone script that ansible run on your behalf, either locally or remotely.
       • Module interact with your local machine, API, or remote system to perform specific task like.
         - Creating user.
         - Installing packages.
         - Updating configuration, etc.
  - List of modules : https://docs.ansible.com/ansible/2.9/modules/list_of_all_modules.html 

  5. What is Gathering facts?

   In Ansible, gathering facts means collecting system information (a.k.a. "facts") about the remote host(s) before running any tasks.
   These facts include things like:

- OS type and version
- IP address and hostname
- Available memory and CPU info
- Network interfaces
- Disk space
- Environment variables
- Default shell, Python version, etc.



