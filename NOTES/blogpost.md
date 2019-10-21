# Automating Jenkins (re)Installation: Some Thoughts, Tips and Tricks

A CI/CD engine, like any other modern system, should be handled as cattle and not as a pet anymore.
Lovingly hand tuned via the Graphical User Interface, "pet" systems can't survive in our world.

Modern systems must be configured in an automated and versioned process. Proceeding that way achieves the following goals:
* the installation and configuraiton of the system is repeatable and yields always the same end-result.
*  


### real life use cases

### Two philosophies:
* golden image
* scripted configuration

## Jenkins configuration vectors?
* aa
* bb

## Some common problems



=====

Installing the system
Deploy in a consistent and repeatable way a full CJP cluster, a Client Master or an agent.
Demonstrate that the Jenkins configuration is properly "documented" by code (Ansible Playbooks) by rebuilding it regularly.
Rebuilding the system regularly. It is a very good security practice to rebuild regularly systems from ground up. This makes sure that any rogue item or configuration introduced by a malicious entity is weeded out by this rebuild process.
Altering the system
Adding a new characteristic or feature to the system (install and configure a plugin, for example). This can be seen as an extension of the Installation category seen the idempotent characteristic of the system
Automate upgrade
(en-)forcing the upgrade of the Jenkins system in an automatic way in the same way as the underlying system must be patched.
Note about auditing.
An implicit characteristic of an automatic system is its anonymity. Particularly in secured environments (but also relevant in more relaxed ones), it is important that audit traces are kept of who triggered the automatic system. This will solve accountability issues but also detect abuse of the automatic process or credential used for it.

The automatic system must thus provide adequate audit trails and protect privileged accounts used for the automation.