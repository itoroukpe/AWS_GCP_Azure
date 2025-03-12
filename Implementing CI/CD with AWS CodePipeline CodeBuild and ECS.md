**AWS Hands-On Workshop: Implementing CI/CD with AWS CodePipeline, CodeBuild, and ECS**

## **Objective**
This hands-on workshop will guide participants through setting up a **Continuous Integration and Continuous Deployment (CI/CD) pipeline** using **AWS CodePipeline, CodeBuild, and Amazon ECS**. By the end of this workshop, students will have a fully automated deployment pipeline for a containerized application.

---

## **Scenario: Automating Deployments for a Web Application**
### **Business Use Case:**
A technology company wants to **automate deployments** for their **containerized web application** to ensure faster releases, minimize downtime, and increase operational efficiency. This workshop will cover setting up **version control, building containerized applications, running tests, and deploying to AWS ECS using CI/CD tools**.

---

## **Step 1: Setting Up AWS CodeCommit for Source Control**
### **Create a CodeCommit Repository**
1. Navigate to **AWS Console > CodeCommit > Create Repository**.
2. Name it `webapp-repo` and add a description.
3. Clone the repository to your local machine:
   ```bash
   git clone https://git-codecommit.<region>.amazonaws.com/v1/repos/webapp-repo
   cd webapp-repo
   ```
4. Add sample application files (`index.html`, `Dockerfile`):
   ```bash
   echo "<h1>Welcome to CI/CD on AWS!</h1>" > index.html
   ```
5. Push the changes:
   ```bash
   git add .
   git commit -m "Initial commit"
   git push origin main
   ```

---

## **Step 2: Creating a Dockerized Web Application**
### **Dockerfile for Containerized Deployment**
1. In the `webapp-repo` directory, create a **Dockerfile**:
   ```dockerfile
   FROM nginx:latest
   COPY index.html /usr/share/nginx/html/index.html
   EXPOSE 80
   ```
2. Build the Docker image:
   ```bash
   docker build -t webapp-image .
   ```
3. Test locally:
   ```bash
   docker run -d -p 8080:80 webapp-image
   ```
4. Verify by opening `http://localhost:8080` in a browser.

---

## **Step 3: Setting Up Amazon ECR (Elastic Container Registry)**
1. Navigate to **AWS Console > ECR > Create Repository**.
2. Name it `webapp-repo`.
3. Authenticate Docker to AWS ECR:
   ```bash
   aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com
   ```
4. Tag and push the Docker image:
   ```bash
   docker tag webapp-image:latest <aws_account_id>.dkr.ecr.<region>.amazonaws.com/webapp-repo:latest
   docker push <aws_account_id>.dkr.ecr.<region>.amazonaws.com/webapp-repo:latest
   ```

---

## **Step 4: Deploying with AWS ECS (Elastic Container Service)**
### **Create an ECS Cluster**
1. Navigate to **AWS Console > ECS > Create Cluster**.
2. Choose **Fargate** for a serverless container experience.
3. Create a **Task Definition**:
   - Select **Fargate**.
   - Use the **ECR image URL** for the container.
   - Assign **512MB Memory, 256 CPU**.
   - Set container port mapping to `80`.
4. Create an **ECS Service**:
   - Choose **Launch Type: Fargate**.
   - Select the previously created **Task Definition**.
   - Set the **desired count** to `2` for high availability.

---

## **Step 5: Configuring AWS CodeBuild**
### **Create a BuildSpec File**
1. In `webapp-repo`, create a file called `buildspec.yml`:
   ```yaml
   version: 0.2
   phases:
     pre_build:
       commands:
         - echo Logging in to Amazon ECR...
         - aws ecr get-login-password --region $AWS_REGION | docker login --username AWS --password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com
     build:
       commands:
         - echo Building the Docker image...
         - docker build -t webapp-image .
         - docker tag webapp-image:latest $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/webapp-repo:latest
     post_build:
       commands:
         - echo Pushing the Docker image...
         - docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_REGION.amazonaws.com/webapp-repo:latest
   ```
2. Navigate to **AWS Console > CodeBuild > Create Build Project**.
3. Select **Source: AWS CodeCommit** (`webapp-repo`).
4. Choose **Managed Image: Ubuntu** and attach the IAM role.
5. Configure **buildspec.yml** and save.

---

## **Step 6: Creating an AWS CodePipeline for Deployment**
### **Configure the Deployment Pipeline**
1. Navigate to **AWS Console > CodePipeline > Create Pipeline**.
2. Choose **Source: CodeCommit (`webapp-repo`)**.
3. Choose **Build Provider: CodeBuild**.
4. Choose **Deploy Provider: Amazon ECS** and select the cluster and service.
5. Save and start the pipeline.

---

## **Final Testing & Validation**
1. Push a change to `index.html`:
   ```bash
   echo "<h1>Welcome to AWS CI/CD Pipeline!</h1>" > index.html
   git add .
   git commit -m "Updated UI"
   git push origin main
   ```
2. Watch **CodePipeline** automatically build and deploy the changes.
3. Retrieve the **ECS Service Load Balancer URL** from the AWS Console and test the web application.

---

## **Conclusion**
By completing this hands-on workshop, participants will gain experience in:
✅ **Setting up CI/CD pipelines with AWS CodePipeline**  
✅ **Building and deploying containerized applications with AWS CodeBuild & ECS**  
✅ **Managing source control with AWS CodeCommit**  
✅ **Automating deployments to AWS infrastructure**  
✅ **Ensuring high availability with AWS Fargate & ECS**  

This workshop prepares students for **real-world DevOps automation** using AWS CI/CD best practices.

