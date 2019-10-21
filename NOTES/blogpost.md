# Automating Jenkins (re)Installation: Some Thoughts, Tips and Tricks

A CI/CD engine, like any other modern system, should be handled as cattle and not as a pet anymore.
Lovingly hand tuned via the Graphical User Interface, "pet" systems can't survive in our world.

Modern systems must be configured in an automated and versioned process. Proceeding that way achieves the following goals:
* the installation and configuraiton of the system is repeatable and yields always the same end-result.
* the configuration is stored in a Source Management system. It can be reviewed and audited.
* as the configuration is stored in SCM, reverting a change is more easy.

A CloudBees Core or Jenkins system is no exception.

Such an automation, besides freeing precious time from a boring and uninteresting tasks, will
* install in an efficient, repeatable and consistant way, a new CI/CD cluster or major component like a Master
* update the system (ex: change a setting, add a plugin)
* implement peer-review mechanism for configuration changes
* enable to manage easily very large CI/CD cluster
* properly document the system

But configuration automation has an important drawback: if not used very regularly, it becomes stale and trust in the system is lost. All the benefits are then lost.

To keep their configuration scripts "fresh" but also to avoid any malicious modification (ex: turning a CI infrastructure into a Bitcoin mining engine), some shops rebuild their CI/CD infrastructure from scratch every 6 weeks. 

An implicit characteristic of an automatic system is its anonymity. Particularly in secured environments (but also relevant in more relaxed ones), it is important that audit traces are kept of who triggered the automatic system. This will solve accountability issues but also detect abuse of the automatic process or credential used for it.

The automatic system must thus provide adequate audit trails and protect privileged accounts used for the automation.

### Two philosophies:
* golden image
* scripted configuration

## Jenkins configuration vectors?
* aa
* bb

## Some common problems

