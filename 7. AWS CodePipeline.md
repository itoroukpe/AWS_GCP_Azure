**Deploying a Maven Web Application on AWS CodePipeline**

## **Objective**
This guide provides step-by-step instructions on setting up **AWS CodePipeline** to automatically deploy a Maven-based web application from the GitHub repository: **https://github.com/itoroukpe/maven-web-application.git**.

---

## **Step 1: Setting Up IAM Permissions**
1. Navigate to **AWS Console > IAM**.
2. Create a new IAM role for CodePipeline:
   - Select **AWS service** as the trusted entity.
   - Choose **CodePipeline** as the use case.
   - Attach policies:
     - `AWSCodePipelineFullAccess`
     - `AWSCodeBuildAdminAccess`
     - `AmazonEC2ContainerRegistryFullAccess`
     - `AmazonS3FullAccess`
     - `IAMFullAccess`
   - Click **Create Role**.

---

## **Step 2: Creating an S3 Bucket for Artifact Storage**
1. Navigate to **AWS Console > S3**.
2. Click **Create Bucket**, name it `maven-web-app-artifacts`.
3. Enable **versioning** and **server-side encryption**.
4. Note the **bucket name** for later use.

---

## **Step 3: Creating a Build Specification File**
1. Clone the repository:
   ```bash
   git clone https://github.com/itoroukpe/maven-web-application.git
   cd maven-web-application
   ```
2. Create a file named **buildspec.yml** inside the repository root:
   ```yaml
   version: 0.2
   phases:
     install:
       runtime-versions:
         java: openjdk17
       commands:
         - echo Installing dependencies...
         - mvn clean install
     build:
       commands:
         - echo Building application...
         - mvn package
         - cp target/*.war app.war
     post_build:
       commands:
         - echo Uploading artifacts to S3...
         - aws s3 cp app.war s3://maven-web-app-artifacts/app.war
   artifacts:
     files:
       - app.war
   ```
3. Commit and push the changes:
   ```bash
   git add buildspec.yml
   git commit -m "Added buildspec.yml for CodeBuild"
   git push origin main
   ```

---

## **Step 4: Setting Up AWS CodeBuild**
1. Navigate to **AWS Console > CodeBuild > Create Build Project**.
2. Name the project **MavenWebAppBuild**.
3. Choose **Managed Image > Ubuntu > Standard Runtime**.
4. Select **AWS CodeCommit** or **GitHub** as the source.
5. Set the repository to **https://github.com/itoroukpe/maven-web-application.git**.
6. Specify the **buildspec.yml** file created earlier.
7. Choose **S3 as the artifact store**, selecting `maven-web-app-artifacts`.
8. Click **Create Build Project** and run a test build.

---

## **Step 5: Setting Up AWS CodeDeploy**
1. Navigate to **AWS Console > CodeDeploy > Create Application**.
2. Name it `MavenWebAppDeploy`.
3. Choose **EC2/On-Premises** deployment type.
4. Create a **Deployment Group**:
   - Select an **EC2 instance** with **Apache Tomcat** installed.
   - Attach the **AWSCodeDeployRole** IAM role.
   - Enable **Load Balancer** if needed.

---

## **Step 6: Creating AWS CodePipeline**
1. Navigate to **AWS Console > CodePipeline > Create Pipeline**.
2. Name the pipeline `MavenWebAppPipeline`.
3. Select **GitHub** as the source provider and connect the repository.
4. Choose the branch (`main`) for the pipeline.
5. Select **AWS CodeBuild** as the build provider.
6. Choose the **MavenWebAppBuild** project.
7. Select **AWS CodeDeploy** as the deployment provider.
8. Choose the `MavenWebAppDeploy` deployment group.
9. Click **Create Pipeline** and trigger a manual deployment.

---

## **Step 7: Testing the Deployment**
1. Wait for the **CodePipeline execution** to complete.
2. Open the **EC2 instance public IP** in a browser:
   ```
   http://<EC2-PUBLIC-IP>:8080/app.war
   ```
3. Verify that the web application is running.

---

## **Conclusion**
By completing this workshop, participants will gain hands-on experience in:
✅ **Setting up AWS CodePipeline for CI/CD automation**  
✅ **Building and deploying Maven applications with AWS CodeBuild**  
✅ **Configuring AWS CodeDeploy for EC2 deployments**  
✅ **Ensuring seamless application updates using GitHub and AWS services**  

This hands-on experience helps students **automate software releases on AWS**, preparing them for real-world DevOps implementations!

