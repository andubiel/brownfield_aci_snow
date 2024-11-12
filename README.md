

# Summary of Demo
brownfield_aci_snow


## Objective
Demonstrate building an ACI Fabric as Code within an existing ACI fabric using Ansible and AAP.This README includes two demos. 

## Overview
In this demo we will find a cure for the proverbial ACI GUI `Death by a thousand clicks`!!!

*  Brownfield ACI-as-Code with YAML files
This demo is for ACI operators who have an existing ACI fabric with many configurations. Using the cisco.aci collection and roles, we can query the existing managed objects and save as YAML configuration files. These variables are subsequently mapped the to the appropriate ansible modules to modify existing or deploy new ACI configurations. Optional, check for configuration drift.

* This demo uses Ansible and Service Now (SNOW) to open tickets when there are errors in the configuration.

## Devnet ACI Sandbox
The hosts.yml file in this demo can be modified to run in the Cisco Devnet ACI always on or reserved sandboxes. The latter requires a vpn connection.

### Always On:
apic_host: sandboxapicdc.cisco.com
apic_username: 'admin'
apic_password: '!v3G@!4@Y'
https://sandboxapicdc.cisco.com

## Other requirements
- git repo (one option is to fork this repo or install your own)
- service now (this is optional for errors)

# Demo Steps
Brownfield ACI-as-Code with YAML files

1. Launch the 0-ACI-as-Brownfield-Workflow to ingest the current ACI fabric configurations as code.
#### Sync your local VSCode repo 
In our demo we have installed gitea. Other git options will work as well.
~~~
git pull
~~~
2.  Review the defaults variables `(learned configs)` in each brownfield role
3.  Review the vars/main variables that are pre-loaded to provide an example of adding a new configuration to the ACI fabric
4. In VSCode modify an existing tenant configuration in the brownfield_tenant_deploy/defaults/tenants.yml vars. Adding descriptions to existing tenants is a recommended here.

#### Save and Push changes from vscode to gitea
~~~
git add all
git commit -m 'changed existing tenant'
git push
~~~ 
5.  Return to AAP JOBs to approve change control step in the 0-ACI-as-Brownfield-Workflow
6.  Review the job output for ACI-Brownfield-Deploy job-template (new configs) and ACI-Brownfield-Config job-template for (Existing Configs)
7.  Take a look at the ACI-Day2-Health-Check.  Take a look at what goes into the health score and why it is important.
8. Config Drift with Diff

#### step 1 - Rerun the 0-ACI-as-Brownfield-Workflow 
This time we will pull in the changes made in the afore mentioned `vars` because they become existing configurations in the default folders written by the playbooks in each role.

It's ok to stay `paused` at the approval stage of the workflow for this exercise.

#### sync your local repo
~~~
git pull
~~~

#### step 2 - Change directories to the `brownfield_tenant_deploy/defaults/main
Now you will be able to locate the tenants.yml diff the previous defaults/main/tenants.yml in the repo.

#### step 3 - Locate the files from the commit comments
 Locate the comments that state: "Updates made by ansible with play: ACI Brownfield to YAML Collection"
The way to see the comments is:
~~~
git log --pretty=full
~~~
~~~
[student@ansible-1 network-demos-repo]$ git log --pretty=full
commit c3b8faa20f8d5dc889b10c30a421324862519ef7 (HEAD -> master, origin/master)
Author: gitea <admin@example.com>
Commit: gitea <admin@example.com>

    Updates made by ansible with play: ACI Brownfield to YAML Collection

commit dfcbeba03ca4390a1caf7375bf9f10f28ab0df9c
Author: gitea <admin@example.com>
Commit: gitea <admin@example.com>

    Updates made by ansible with play: Validate ACI Health for New Tenants

commit a0b967f3df435534e1b66ed1b50b1aacb5cf4d87
Author: gitea <admin@example.com>
Commit: gitea <admin@example.com>

    Updates made by ansible with play: Brownfield Deployment (new configs)

commit 4820d4677b5916d2b2bb6e5871c5ef51f7cc8dae
Author: gitea <admin@example.com>
Commit: gitea <admin@example.com>

    changed

commit 1b6f83e6d489e7e3ec064e89865d7ed29d6adb5d
Author: gitea <admin@example.com>
Commit: gitea <admin@example.com>

    Updates made by ansible with play: ACI Brownfield to YAML Collection
#### step 4 - Run the git Diff from VSCode terminal
~~~
Diff Example:
change directory to cisco_aci/roles/brownfield_tenant_deploy/defaults/main/
~~~
cd cisco_aci/roles/brownfield_tenant_deploy/defaults/main/
~~~
Diff command:
In the following example you will observe the two tenants with modified descriptions (StuPedazzo, EQUI_UNTRUSTED_PROD_TNT ). Secondly, the new tenant `ansible` is appended to th bottom of the config. 
~~~
$ git diff c3b8faa20f8d5dc889b10c30a421324862519ef7 1b6f83e6d489e7e3ec064e89865d7ed29d6adb5d tenants.yml
diff --git a/cisco_aci/roles/brownfield_tenant_deploy/defaults/main/tenants.yml b/cisco_aci/roles/brownfield_tenant_deploy/defaults/main/tenants.yml
index 068d06e..1eee9ce 100644
--- a/cisco_aci/roles/brownfield_tenant_deploy/defaults/main/tenants.yml
+++ b/cisco_aci/roles/brownfield_tenant_deploy/defaults/main/tenants.yml
@@ -2,13 +2,13 @@ changed: false
 tenants:
 -   fvTenant:
         attributes:
-            annotation: orchestrator:ansible
+            annotation: ''
             childAction: ''
             descr: ''
             dn: uni/tn-infra
             extMngdBy: ''
             lcOwn: local
-            modTs: '2024-08-10T17:34:58.833+00:00'
+            modTs: '2024-08-10T10:08:02.959+00:00'
             monPolDn: uni/tn-common/monepg-default
             name: infra
             nameAlias: ''
@@ -19,13 +19,13 @@ tenants:
             userdom: all
 -   fvTenant:
         attributes:
-            annotation: orchestrator:ansible
+            annotation: ''
             childAction: ''
             descr: ''
             dn: uni/tn-common
             extMngdBy: ''
             lcOwn: local
-            modTs: '2024-08-10T17:35:00.425+00:00'
+            modTs: '2024-08-10T10:07:57.898+00:00'
             monPolDn: uni/tn-common/monepg-default
             name: common
             nameAlias: ''
@@ -36,13 +36,13 @@ tenants:
             userdom: all
 -   fvTenant:
         attributes:
-            annotation: orchestrator:ansible
+            annotation: ''
             childAction: ''
             descr: ''
             dn: uni/tn-mgmt
             extMngdBy: ''
             lcOwn: local
-            modTs: '2024-08-10T17:35:02.405+00:00'
+            modTs: '2024-08-10T10:08:05.056+00:00'
             monPolDn: uni/tn-common/monepg-default
             name: mgmt
             nameAlias: ''
@@ -53,13 +53,13 @@ tenants:
             userdom: all
 -   fvTenant:
         attributes:
-            annotation: orchestrator:ansible
+            annotation: ''
             childAction: ''
-            descr: changed by ansible 2.4
+            descr: ''
             dn: uni/tn-StuPedazzo
             extMngdBy: ''
             lcOwn: local
-            modTs: '2024-08-10T17:35:03.773+00:00'
+            modTs: '2024-08-10T15:33:05.812+00:00'
             monPolDn: uni/tn-common/monepg-default
             name: StuPedazzo
             nameAlias: ''
@@ -70,13 +70,13 @@ tenants:
             userdom: ':all:'
 -   fvTenant:
         attributes:
-            annotation: orchestrator:ansible
+            annotation: ''
             childAction: ''
-            descr: changed by ansible2.4
+            descr: ''
             dn: uni/tn-EQUI_UNTRUSTED_PROD_TNT
             extMngdBy: ''
             lcOwn: local
-            modTs: '2024-08-10T17:35:05.540+00:00'
+            modTs: '2024-08-10T16:52:56.195+00:00'
             monPolDn: uni/tn-common/monepg-default
             name: EQUI_UNTRUSTED_PROD_TNT
             nameAlias: ''
@@ -87,13 +87,13 @@ tenants:
             userdom: ':all:'
 -   fvTenant:
         attributes:
-            annotation: orchestrator:ansible
+            annotation: ''
             childAction: ''
             descr: Try ACI tool demo at https://oneaciapp.talapupa.com
             dn: uni/tn-oneaciapp
             extMngdBy: ''
             lcOwn: local
-            modTs: '2024-08-10T17:35:07.217+00:00'
+            modTs: '2024-08-10T16:58:56.507+00:00'
             monPolDn: uni/tn-common/monepg-default
             name: oneaciapp
             nameAlias: ''
@@ -104,13 +104,13 @@ tenants:
             userdom: ':all:'
 -   fvTenant:
         attributes:
-            annotation: orchestrator:ansible
+            annotation: ''
             childAction: ''
             descr: ''
             dn: uni/tn-TELE_UNTRUSTED_PROD_TNT
             extMngdBy: ''
             lcOwn: local
-            modTs: '2024-08-10T17:35:08.859+00:00'
+            modTs: '2024-08-10T17:27:57.997+00:00'
             monPolDn: uni/tn-common/monepg-default
             name: TELE_UNTRUSTED_PROD_TNT
             nameAlias: ''
@@ -119,21 +119,4 @@ tenants:
             status: ''
             uid: '15374'
             userdom: ':all:'
--   fvTenant:
-        attributes:
-            annotation: orchestrator:ansible
-            childAction: ''
-            descr: Created with AAP2.4
-            dn: uni/tn-Ansible
-            extMngdBy: ''
-            lcOwn: local
-            modTs: '2024-08-10T17:33:22.566+00:00'
-            monPolDn: uni/tn-common/monepg-default
-            name: Ansible
-            nameAlias: ''
-            ownerKey: ''
-            ownerTag: ''
-            status: ''
-            uid: '15374'
-            userdom: ':all:'
 failed: false
(END)
~~~

# Optional add Error
This will invoke a Service Now Ticket and Rollback


# Key Takeaways
* Easy migration from postman to Ansible with csv files
* No need to work with the APIC GUI or APIs directly
* Easy migration to IaC methodologies (gitops, webhook triggers etc)
* Easily convert existing ACI managed objects to YAML configurations
* Easily create health checks to verify the operation state of the ACI fabric
* Infra as Code anf GitOps allow for easy diff checks to manage configuration drift etc
* Easy integration between Ansible and Service Now

## Return to Demo Menu
 - [Menu of Demos](../README.md)


