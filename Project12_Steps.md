
#### ANSIBLE REFACTORING AND STATIC ASSIGNMENTS (IMPORTS AND ROLES)
So far in Project 9 10
now every new change in the codes creates a separate directory which is not very convenient when we want to run some commands from one place so we use plug in [Copy Artifact](https://plugins.jenkins.io/copyartifact/)   


Installing **Copy Artifact**  
**Manage Jenkins** >  **Manage Plugins**  

![Markdown Logo](https://raw.githubusercontent.com/hectorproko/ANSIBLE-REFACTORING-ASSIGNMENTS-IMPORTS/main/images/copyArtifact.png)


Creating directory in Jenkins instance to store all artifacts after each build
``` bash
sudo mkdir /home/ubuntu/ansible-config-artifact
```

``` bash
ubuntu@ip-172-31-94-159:~$ sudo chmod -R 0777 /home/ubuntu/ansible-config-artifact	
ubuntu@ip-172-31-94-159:~$ ls -l
total 24
-rw-r--r-- 1 root   root    333 Apr 15 23:09 2plays.yml
drwxrwxrwx 2 root   root   4096 Apr 17 15:53 ansible-config-artifact
-rw-r--r-- 1 ubuntu ubuntu  572 Apr 17 15:16 common.yml
-rw------- 1 ubuntu ubuntu 1799 Apr  2 14:45 daro.io.pem
-rw-rw-r-- 1 ubuntu ubuntu    4 Apr  2 15:41 hello
drwxrwxr-x 3 ubuntu ubuntu 4096 Apr  2 14:44 key
ubuntu@ip-172-31-94-159:~$
```


Creating Jenkins Job **save_artifacts**

![Markdown Logo](https://raw.githubusercontent.com/hectorproko/ANSIBLE-REFACTORING-ASSIGNMENTS-IMPORTS/main/images/itemName.png)


This project/job will be triggered by completion of our existing **ansible** project.   
![Markdown Logo](https://raw.githubusercontent.com/hectorproko/ANSIBLE-REFACTORING-ASSIGNMENTS-IMPORTS/main/images/copy_artifact_trigger.png)


Now we configure the job to actually save the artifact to the directory we just created we created  
_/home/ubuntu/ansible-config-artifact_  

![Markdown Logo](https://raw.githubusercontent.com/hectorproko/ANSIBLE-REFACTORING-ASSIGNMENTS-IMPORTS/main/images/build1.png)

![Markdown Logo](https://raw.githubusercontent.com/hectorproko/ANSIBLE-REFACTORING-ASSIGNMENTS-IMPORTS/main/images/build2.png)

I will test the job by making a change in repo **ansible-config-mgt** specifically the readme file to trigger the project through the webhook

**Console Log** of **ansible** job.  
Shows it was triggered by a **push** (a commit with message "Update README.md") and it triggered job **save_artifacts**
``` bash
Job name  ansible

Started by GitHub push by hectorproko #<<<<

Running as SYSTEM
Building in workspace /var/lib/jenkins/workspace/ansible
The recommended git tool is: NONE
using credential 4339e2c2-cc4c-4596-b5a3-b06d893f6339
 > git rev-parse --resolve-git-dir /var/lib/jenkins/workspace/ansible/.git # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url https://github.com/hectorproko/ansible-config-mgt.git # timeout=10
Fetching upstream changes from https://github.com/hectorproko/ansible-config-mgt.git
 > git --version # timeout=10
 > git --version # 'git version 2.25.1'
using GIT_ASKPASS to set credentials 
 > git fetch --tags --force --progress -- https://github.com/hectorproko/ansible-config-mgt.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/main^{commit} # timeout=10
Checking out Revision 5db5aa77bd6c8b9cb3563c81f359026ccee275ce (refs/remotes/origin/main)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 5db5aa77bd6c8b9cb3563c81f359026ccee275ce # timeout=10

Commit message: "Update README.md" #<<<<
 
 > git rev-list --no-walk 4a520c83d57729a09264bec6bf5831d470a7e728 # timeout=10
Archiving artifacts

Triggering a new build of save_artifacts #<<<<

Finished: SUCCESS
```

Looking at **save_artifacts**'s **Console Output** we see it was triggered by upstream project **ansible**

![Markdown Logo](https://raw.githubusercontent.com/hectorproko/ANSIBLE-REFACTORING-ASSIGNMENTS-IMPORTS/main/images/consoleOutput.png)

``` bash
Started by upstream project "ansible" build number 10	
originally caused by:
 Started by GitHub push by hectorproko
Running as SYSTEM
Building in workspace /var/lib/jenkins/workspace/save_artifacts
Copied 7 artifacts from "ansible" build number 10
Finished: SUCCESS
```

Now the directory we created is populated 
``` bash
ubuntu@ip-172-31-94-159:~/ansible-config-artifact$ sudo tree
.
├── 2plays.yml
├── README.md
├── inventory [error opening dir] #tree didnt work
└── playbooks [error opening dir]
2 directories, 2 files
```

Just to see my pipeline worked I created TestFile in the repo **ansible-config-mng** to trigger the ansible job automatically and I can see the file in the directory **ansible-config-artifact**

``` bash
ubuntu@ip-172-31-94-159:~/ansible-config-artifact$ sudo tree
.
├── 2plays.yml
├── README.md
├── TestFile #<<<
├── inventory [error opening dir]
└── playbooks [error opening dir]

2 directories, 3 files
```

Now that I'm adding new stuff to the repo and changing the structure around I can see it copied to **ansible-config-artifact**



#### REFACTOR ANSIBLE CODE BY IMPORTING OTHER PLAYBOOKS INTO SITE.YML  


#### CONFIGURE UAT WEBSERVERS WITH A ROLE ‘WEBSERVER’  


#### REFERENCE WEBSERVER ROLE  

