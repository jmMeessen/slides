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

Often overlooked, solid configuration management is needed to support in a sound way power user's freedom and experimentation. 
It allows a quick and easy way to get back to solid ground.
But don't forget to add these experimentations to your automated configuration corpus once they get main-stream. 

But configuration automation has an important drawback: if not used very regularly, it becomes stale and trust in the system is lost. All the benefits are then lost.

To keep their configuration scripts "fresh" but also to avoid any malicious modification (ex: turning a CI infrastructure into a Bitcoin mining engine), some shops rebuild their CI/CD infrastructure from scratch every 6 weeks. 



## The configuration management philosophies

During time, Configuration management evolved from the Golden Image philosophy to scripts. 
Golden Image, in the early days, very often ended up being a lot of work to maintain, messy and "one size fits nobody". 
Scripts solved a lot of these problems and added readability and versioning.
First hand crafted, then they evolved to special tools like Chef, Puppet or Ansible.

With the advent of Docker, the Golden Image approach gained more momentum because of the very short start time and the way images are built. 
This is particularly true with the quick grow of Cloud solutions.

But this doesn't solve all the problems. 
The reality lies in between generalization (general purpose images) and the need for fine grained customizations to adapt to the local constrains.

This discussion will dive into the verious means to automate that fine grained configuration. 
While trying to stay tool agnostic, it will use Ansible for illustrations.


## Jenkins configuration vectors

Jenkins and CloudBees Core can be configured through various vectors, each one having its pros and cons.

They come in x main category:
* manipulating the file system
* using command line interfaces (like REST API or the Jenkin CLI)
* executing groovy scripts (automatic execution or via a command)
* Jenkins Configuration as Code (JCasC)

### Modifying the file system directly

This a classical way to configure a system. 
Jenkins is configured by copying binary files and updating configuration files directly on the file system. 
This is done mainly with the `copy`, `template`, `lineinfile`, or `xml` Ansible modules.

**Pro**

* This is very easy to perform with tools like Ansible.

**Cons**

* This method requires a very good knowledge of the Jenkins internals. This knowledge is not readily available and published.
* The stability of these undocumented configuration is not guaranteed. This is especially impactful with plugin configuration.
* It is not easy to proceed that way if non file-system resources need to be updated (ex: database settings)

### Remote Access API (aka REST API)

Jenkins offers a REST API. This type of application program interface (API) uses HTTP requests to GET, PUT, POST and DELETE data.

Using the Jenkins REST API is programmatically quite simple: it just requires to POST a well formatted URL.  For a more complete discussion, see https://wiki.jenkins.io/display/JENKINS/Remote+access+API and https://wiki.jenkins.io/display/JENKINS/Authenticating+scripted+clients.

Rest API example (delete a job)
```
curl --silent --show-error http://<username>:<password>@<jenkins-server>/job/<job-name>/doDelete
```

**Pro**
* relatively simple to use and rich set of API
* Steps can easily be tested stand alone (especially if using the shell module)
* Can be used to execute Groovy scripts.

**Con**
* API Token and CSRF Crumb management can get tricky to be done in Ansible, especially in a secure way.
* The calls are non-blocking (meaning they return immediately, thus "fire and forget"). If steps need to be sequenced, polling on the result has to be implemented.
* Parameter checking is not as good as for the Command Line Interface (CLI)
* The required password/API token management is not adapted for automation and less elegant than with the Command Line Interface (CLI).
* Managing/parsing the results is also a challenge. See comments above.


#### Security consideration
The examples above imply the use of the password. 
Using the actual user password is a very bad practice. 
Jenkins offers the alternative API token mechanism. 
It can be invalidated and rotated independently from the user's credential. 

The API token is available in your personal configuration page. 
Click your name on the top right corner on every page, then click "Configure" to see your API token. (The URL $root/me/configure is a good shortcut.) You can also change your API token from here.

When the Jenkins server enables "Prevent Cross Site Request Forgery exploits" security option (which it should), the calls must supply a "crumb" that is obtained at the start of the session. See an example below.

Providing the CRSF protection crumb example
```
SERVER=http://localhost:8080
CRUMB=$(curl --user $USER:$APITOKEN \
    $SERVER/crumbIssuer/api/xml?xpath=concat\(//crumbRequestField,%22:%22,//crumb\))
 
curl --user $USER:$APITOKEN -H "$CRUMB" -d "script=$GROOVYSCRIPT" $SERVER/script
```

### Using the Command Line Interface
The Command Line Interface (CLI) has been explicitly designed to interact with Jenkins. The parameters are better checked than with the REST API. The Interface is also blocking, meaning that the call will wait until completion or timeout.

Using the `help` command will display a list of available commands available (some are added with plugins).

The CLI can be accessed over SSH or with the Jenkins CLI client `.jar` (distributed with Jenkins). See the Jenkins CLI article (https://jenkins.io/doc/book/managing/cli/) for more details. 

While using the SSH flavour makes authentication with keypairs more easier (especially when bootstrapping a system), having to configure the dedicated SSH port 

## Some common problems

