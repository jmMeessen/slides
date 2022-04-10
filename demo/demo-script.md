# Gitpod demo script

- Open browser in separate window (`<cmd>n`)
	- check size for screen, turn off the sound, and notifications
- Open quickTime, just in case
- Load https://github.com/jmMeessen/jenkins.io

---

- Click the "Gitpod" button
    - Show what is the URL behind
- loading workspace and pre-build
- show softwares
    - `git --version`
    - `git remote -v`
    - `mvn -version`
    - `go version`
    -  `brew install gh`  (in case of missing software)
- show and briefly comment the `gitpod.yml`
- System configuration
    - `uname` (linux)
    - `lscpu`  (show cpu)
    - `free -h`  (show memory)
    -  `df -h` (show disk)
    - examiné la config de l'environement en consultant les CGROUP (v2)
        - `/sys/fs/cgroup`

---
- show the two consoles (config and interactive)
- shot the remotes (`git remote -v`)
---
- ==create branch==
- open the preview port
- Navigate to `community/outreach`
- open `content/sigs/advocacy-and-outreach/index`
    -create a section `devoxxFR`

````
=== DevoxxFR 2022

Faire un atelier "Jenkins Pipeline" ou "Jenkins et la gestion des secrets".`
````

- Faire un commit du fichier, choisir `origin`
- Le plugin Github propose de créer une pull request
---
- Pas nécessaire de faire un clone (fork oui)
- Pas de choses encombrante

## Roulez jeunesse !
