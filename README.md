# sample-node-app
Testing Jenkins Pipeline for backup 

**A.	Preliminaries — SSH from Windows**
1.	Open Git Bash/Git/CMD
2.	where your .pem key is
3.	connect SSH to EC2 host(ssh -i "Pem.file" ubuntu@ec2-IP.ap-south-1.compute.amazonaws.com)
In browser, log in to Jenkins.
next:

**B.In order to takeup the backup  we need to install backup plugin**
1.	Login to Jenkins as admin in browser: http://<IP>:8080
2.	Go to Manage Jenkins → Manage Plugins → Available.
3.	Search for ThinBackup (or “backup” if your plugin list not exact).
4.	Select ThinBackup → click Install without restart (or Download now and install after restart).
5.	Wait for installation; confirm under Installed the plugin appears.
6.	you should see Installation successful: thinBackup
 
Confirmed plugin loaded and Jenkins started
Found ThinBackup settings location:
Manage Jenkins → Configure System → (scroll down) → ThinBackup
We can see the two options  under thinBackup:

**C. Prepare backup directory on the host (must be a directory, not a file)**
Fixed steps you executed (must run as an account with sudo, e.g. ubuntu or ec2-user):
1.	SSH as a sudo user (ubuntu / ec2-user) and become root:then:
sudo -i
create a directory:
mkdir -p /var/lib/jenkins/backup
Verify::
ls -ld /var/lib/jenkins/backup**

**D. Configure ThinBackup settings in Jenkins UI**
1.	Open: Manage Jenkins → Configure System(THIS is important — ThinBackup settings are here, NOT on the ThinBackup page)
2.	Scroll down untill you find the ThinBackup block
3.	Configure these fields (example values you set / should set):
o	Backup directory: /var/lib/jenkins/backup
Cron schedules
•	ThinBackup expects 5 fields (minute, hour, day, month, day_of_week).
Full backup schedule (cron): choose one:
•	Jenkins-recommended (spread load): H 0 * * *
•	Exact daily at 02:00: 0 2 * * *
Differential backup schedule (cron, optional): example: every 6 hours: 0 */6 * * *

o	Backup files: (select what you need: jobs, builds, plugins, config)
o	Exclusions: workspaces (optional)
o	Number of backups to keep: e.g. 10
o	Restore directory (if used): leave default or set e.g. /var/lib/jenkins/backup/restore

Save the configuration, 

**E.then go to thinbacakup and click on Backup now option.**
Then verify ,see the below taken the backup under given backup directory, if you go and see backup directory all the files which are stored(full backup is created).
Below image one job is here, the job is successfully executed, I am going to delete this job
Job is deleted, already I have taken the backup .

**F.Restore procedure:**
Now I am going to restore the job from the backup.
Go to manage Jenkins Thinkup  restore option(click)
After restore, you may need to restart Jenkins: sudo systemctl restart jenkins (from the host). Or 
http://<url> /restart
Whatever job is deleted variable to get it from the Jenkins backup .



