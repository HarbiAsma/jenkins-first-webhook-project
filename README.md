# Hands-on Jenkins-02 : Triggering Jenkins Jobs

Purpose of the this hands-on training is to learn how to trigger Jenkins jobs with different ways.

## Learning Outcomes

At the end of the this hands-on training, students will be able to;

- integrate your Jenkins server with Github

- trigger Jenkins jobs with webhook

- trigger Jenkins jobs with Poll SCM


## Outline

- Part 1 - Integrating Jenkins with GitHub using Webhook

- Part 2 - Integrating Jenkins Pipeline with GitHub Webhook

- Part 3 - Creating a Pipeline with Poll SCM


## Part 1 - Integrating Jenkins with GitHub using Webhook

- Create a public project repository `jenkins-first-webhook-project` on your GitHub account.

- Clone the `jenkins-first-webhook-project` repository on local computer.

```bash
git clone <your-repo-url>
```
- Go to your local repository.

```bash
cd jenkins-first-webhook-project
```

- Write a simple java code which prints `Hello World` and save it as `HelloWorld.java`.

```java
public class HelloWorld{

        public static void main (String[] args){

                System.out.println("Hello World");
        }
}
```

- Commit and push the local changes to update the remote repo on GitHub.

```bash
git add .
git commit -m 'added hello world'
git push
```

- Go back to Jenkins dashboard and click on `New Item` to create a new job item.

- Enter `first-job-triggered` then select `free style project` and click `OK`.

```yaml
- General:
- Description : My first job triggered from GitHub

- Source Code Management:
    Git:
      Repository URL: https://github.com/<your-github-account-name>/jenkins-first-webhook-project/

    Branches to build: It must be same branch name with your `jenkins-first-webhook-project` Github repository. If your repository's default branch name is "main", then change "master" to "main".

- Build Triggers:
    - GitHub hook trigger for GITScm polling

- Build Steps:
    Execute shell:
        javac HelloWorld.java
        java HelloWorld
```   

- Click `apply` and `save`.

- Go to the Jenkins project page and click `Build Now`. The job has to be executed manually one time in order for the push trigger and the git repo to be registered.

- Go to your Github `jenkins-first-webhook-project` repository page and click on `Settings`.

- Click on the `Webhooks` on the left hand menu, and then click on `Add webhook`.

- Copy the Jenkins URL from the AWS Management Console, paste it into `Payload URL` field, add `/github-webhook/` at the end of URL, and click on `Add webhook`.

```text
http://ec2-54-144-151-76.compute-1.amazonaws.com:8080/github-webhook/
```

- Change the java code on your local repository to print `Hello World by Jenkins Job` and save.

```java
public class HelloWorld{

        public static void main (String[] args){

                System.out.println("Hello World by Jenkins Job");
        }
}
```

- Commit and push the local changes to update the remote repo on GitHub.

```bash
git add .
git commit -m 'updated hello world'
git push
```

- Observe the new built under `Build History` on the Jenkins project page.

- Explain the details of the built on the Build page.

- Go back to the project page and explain the GitHub Hook log.

## Part 2 - Integrating Jenkins Pipeline with GitHub Webhook

- Go to your Github ``jenkinsfile-pipeline-project`` repository page and click on `Settings`.

- Click on the `Webhooks` on the left hand menu, and then click on `Add webhook`.

- Copy the Jenkins URL from the AWS Management Console, paste it into `Payload URL` field, add `/github-webhook/` at the end of URL, and click on `Add webhook`.

```text
http://ec2-54-144-151-76.compute-1.amazonaws.com:8080/github-webhook/
```

- Go to the Jenkins dashboard and click on `New Item` to create a pipeline.

- Enter `pipeline-with-jenkinsfile-and-webhook` then select `Pipeline` and click `OK`.

```yaml
- General:
- Description : Simple pipeline configured with Jenkinsfile and GitHub Webhook

- Build Triggers:
    - GitHub hook trigger for GITScm polling

- Pipeline:
    Definition:
        Pipeline script from SCM:
            Git:
                Repository URL:
                    - https://github.com/<your-github-account-name>/jenkinsfile-pipeline-project.git
                
                Branches to build: It must be same branch name with your `jenkins-first-webhook-project` Github repository. If your repository's default branch name is "main", then change "master" to "main".
```

- Click `apply` and `save`. Note that the script `Jenkinsfile` should be placed under root folder of repo.

- Go to the Jenkins project page and click `Build Now`.The job has to be executed manually one time in order for the push trigger and the git repo to be registered.

- Now, to trigger an automated build on Jenkins Server, we need to change any file it the repo, then commit and push the change into the GitHub repository. So, update the `Jenkinsfile`on your local repository with the following pipeline script.

```groovy
pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                echo 'Clarusway_Way to Reinvent Yourself'
                sh 'echo Integrating Jenkins Pipeline with GitHub Webhook using Jenkinsfile'
            }
        }
    }
}
```

- Commit and push the change to the remote repo on GitHub.

```bash
git add .
git commit -m 'updated Jenkinsfile'
git push
```

- Observe the new built triggered with `git push` command under `Build History` on the Jenkins project page.

- Explain the built results, and show the `Integrating Jenkins Pipeline with GitHub Webhook using Jenkinsfile` output from the shell.

- Explain the role of `Jenkinsfile` and GitHub Webhook in this automation.

## Part 3 - Creating a Pipeline with Poll SCM

- Go to the Jenkins dashboard and click on `New Item` to create a pipeline.

- Enter `jenkinsfile-pipeline-pollSCM` then select `Pipeline` and click `OK`.

```yaml
- General:
- Description : This is a pipeline project with pollSCM

- Build Triggers:
    - Poll SCM: * * * * *

- Pipeline:
    Definition:
        `Pipeline script from SCM:
            Git:
                Repository URL:
                    - https://github.com/<your-github-account-name>/jenkinsfile-pipeline-project.git
                
                Branches to build: It must be same branch name with your `jenkins-first-webhook-project` Github repository. If your repository's default branch name is "main", then change "master" to "main".
```

- `Save` the configuration.

- Go to the GitHub repo and modify some part in the `Jenkinsfile` and commit.

```bash
git add .
git commit -m 'updated jenkinsfile and added pipeline.py'
git push
```

- Observe the auto build action at Jenkins job.
