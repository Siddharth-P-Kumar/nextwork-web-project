<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Build a CI/CD Pipeline with AWS

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-devops-codepipeline-updated)

**Author:** siddharthpk nextwork  
**Email:** siddharthpknextwork@gmail.com

---

![Image](http://learn.nextwork.org/mischievous_red_fierce_pony/uploads/aws-devops-codepipeline-updated_fbdetger)

---

## Introducing Today's Project!

In this project, I will demonstrate how to build an automated CI/CD pipeline using AWS CodePipeline. I'm doing this project to learn how to connect my source code, build, and deployment stages for seamless, automated deployments, and to understand how to handle rollbacks.

### Key tools and concepts

Services I used were AWS CodePipeline, AWS CodeBuild, AWS CodeDeploy, GitHub, and Amazon EC2. Key concepts I learned include setting up a CI/CD pipeline, configuring Source, Build, and Deploy stages, and implementing automatic rollbacks for disaster recovery

### Project reflection

This project took me approximately 3 hours to complete. The most challenging part was troubleshooting the CodePipeline Deploy stage retry issue, as it required understanding the different execution modes and pipeline states. It was most rewarding to successfully retry the Deploy stage and then demonstrate disaster recovery skills by initiating a rollback in the Secret Mission.

---

## Starting a CI/CD Pipeline

AWS CodePipeline is a service that helps you automate your software release process. It orchestrates your entire workflow, giving you visibility into the process in one place. It can automatically detect code changes, trigger builds with CodeBuild, and start deployments with CodeDeploy.



CodePipeline offers different execution modes based on how it handles multiple runs of the same pipeline. I chose Superseded mode because it ensures that only the latest code changes are processed, with newer executions taking over and canceling older ones. Other options include Queued mode, where executions are processed one after another, and Parallel mode, which allows multiple executions to run simultaneousl

A service role gets created automatically during setup so CodePipeline has the necessary permissions to perform actions on your behalf, such as accessing S3 buckets for artifacts or CodeBuild for building your code.

![Image](http://learn.nextwork.org/mischievous_red_fierce_pony/uploads/aws-devops-codepipeline-updated_gdnhtm)

---

## CI/CD Stages

The three stages I've set up in my CI/CD pipeline are the Source stage, the Build stage, and the Deploy stage. While setting up each part, I learned about how the Source stage uses webhook events to automatically trigger the pipeline from GitHub, how the Build stage with AWS CodeBuild transforms source code into deployable artifacts, and how the Deploy stage with AWS CodeDeploy handles deploying the application to an EC2 instance and enables automatic rollbacks.

CodePipeline organizes the three stages into a visual pipeline diagram, showing the flow from source to build to deploy. In each stage, you can see more details on its status (like in progress, success, or failure) and by clicking on the Stage ID link in the Executions tab, you can view specific execution details for that stage.

![Image](http://learn.nextwork.org/mischievous_red_fierce_pony/uploads/aws-devops-codepipeline-updated_fbdetger)

---

## Source Stage

In the Source stage, the default branch tells CodePipeline to monitor this specific branch for changes and automatically trigger the pipeline whenever there's a new commit to it.

The source stage is also where you enable webhook events, which let CodePipeline automatically start your pipeline whenever code is pushed to your specified branch in GitHub. This makes your pipeline truly continuous, reacting to code changes in real-time.

![Image](http://learn.nextwork.org/mischievous_red_fierce_pony/uploads/aws-devops-codepipeline-updated_sergt)

---

## Build Stage

The Build stage sets up the process to transform our source code into a deployable artifact. I configured it to use AWS CodeBuild to compile and package my web application. The input artifact for the build stage is SourceArtifact because it's the ZIP file containing the source code that was outputted by the Source stage, making it available for the build process.

![Image](http://learn.nextwork.org/mischievous_red_fierce_pony/uploads/aws-devops-codepipeline-updated_j1k2l3m4)

---

## Deploy Stage

The Deploy stage is where the application artifacts are taken from the Build stage and deployed to the target environment, which is an EC2 instance. I configured it to use AWS CodeDeploy, specifying the application name and deployment group. I also enabled automatic rollback on stage failure to ensure stability.

![Image](http://learn.nextwork.org/mischievous_red_fierce_pony/uploads/aws-devops-codepipeline-updated_m4n5o6p7)

---

## Success!

Since my CI/CD pipeline gets triggered by pushing code changes to GitHub, I tested my pipeline by adding a new line to my index.jsp file and then committing and pushing those changes to the master branch of my GitHub repository.

The moment I pushed the code change, a new execution started automatically in CodePipeline. The commit message under each stage reflects the changes I made, and I could verify these changes by clicking the Commit ID link, which took me to the commit page in GitHub.

Once my pipeline executed successfully, I checked my web application in the browser and confirmed that the new line I added to index.jsp was visible, which confirmed that the latest changes were automatically deployed into production by CodePipeline.

![Image](http://learn.nextwork.org/mischievous_red_fierce_pony/uploads/aws-devops-codepipeline-updated_e1f2g3h4)

---

## Testing the Pipeline

In a project extension, I initiated a rollback on the CodePipeline Deploy stage as part of the Secret Mission. Automatic rollback is important for quickly restoring service and demonstrating disaster recovery capabilities in a production environment, allowing for rapid recovery from deployment failures.

During a rollback initiated in the Deploy stage, the Source and Build stages typically remain unaffected and do not re-execute. This is because the rollback action focuses on reverting the deployed application to a previous successful version, rather than re-processing the source code or rebuilding the application. I could verify this by observing the CodePipeline execution history, confirming that only the Deploy stage shows activity related to the rollback, while the Source and Build stages retain their last successful execution status.

After the rollback, the live web application reverted to its previous stable state, effectively undoing the changes introduced by the deployment that was rolled back. This confirms the successful execution of the rollback procedure.

![Image](http://learn.nextwork.org/mischievous_red_fierce_pony/uploads/aws-devops-codepipeline-updated_sdfgsdfgdf)

---

---
