**AWS Hands-On Workshop: Building a Serverless Application with AWS Lambda, API Gateway, DynamoDB, and S3**

## **Objective**
This hands-on workshop will guide participants through building a **serverless web application** using **AWS Lambda, API Gateway, DynamoDB, and S3**. By the end of the workshop, students will understand how to deploy a **scalable, cost-efficient, and fully managed application** without provisioning servers.

---

## **Scenario: Creating a Serverless Task Management App**
### **Business Use Case:**
A startup needs a **low-cost, scalable task management application** that allows users to create, update, and delete tasks via a web interface. The backend will be powered by AWS Lambda and DynamoDB, while the frontend will be hosted on Amazon S3.

---

## **Step 1: Setting Up an Amazon S3 Static Website**
### **Create an S3 Bucket for Frontend Hosting**
1. Navigate to **AWS Console > S3 > Create Bucket**.
2. Name the bucket `task-manager-frontend`.
3. Uncheck **Block all public access** (Enable public access).
4. Under **Properties**, enable **Static Website Hosting**.
5. Upload **HTML, CSS, and JavaScript files**.
6. Copy the **S3 Static Website Endpoint** for testing.

---

## **Step 2: Configuring DynamoDB for Backend Storage**
### **Create a DynamoDB Table**
1. Navigate to **AWS Console > DynamoDB > Create Table**.
2. Name the table `Tasks`.
3. Set **Primary Key** as `taskId` (String).
4. Enable **On-Demand Capacity Mode**.
5. Add sample data:
   ```json
   {
     "taskId": "1",
     "title": "Complete AWS Workshop",
     "status": "In Progress"
   }
   ```

---

## **Step 3: Creating AWS Lambda Functions for API Backend**
### **Create a Lambda Function for CRUD Operations**
1. Navigate to **AWS Console > Lambda > Create Function**.
2. Choose **Author from Scratch**.
3. Name the function `taskManagerFunction`.
4. Select **Runtime: Python 3.9**.
5. Assign **DynamoDB Read/Write permissions** using IAM Role.

### **Deploy Python Code for Task Management**
1. Open **AWS Lambda > taskManagerFunction**.
2. Replace the default code with:
   ```python
   import json
   import boto3
   
   dynamodb = boto3.resource('dynamodb')
   table = dynamodb.Table('Tasks')
   
   def lambda_handler(event, context):
       method = event['httpMethod']
       
       if method == 'POST':
           body = json.loads(event['body'])
           table.put_item(Item=body)
           return {'statusCode': 200, 'body': json.dumps('Task Created!')}
       
       elif method == 'GET':
           response = table.scan()
           return {'statusCode': 200, 'body': json.dumps(response['Items'])}
   ```
3. Click **Deploy**.

---

## **Step 4: Setting Up API Gateway to Expose Lambda Function**
### **Create a REST API**
1. Navigate to **AWS Console > API Gateway > Create API**.
2. Choose **REST API**.
3. Name it `TaskManagerAPI`.

### **Configure API Endpoints**
1. Create a **new resource** `/tasks`.
2. Create **methods**:
   - `POST` → Integrate with **taskManagerFunction**.
   - `GET` → Integrate with **taskManagerFunction**.
3. Deploy the API and note the **Invoke URL**.

---

## **Step 5: Connecting Frontend with API Gateway**
### **Modify JavaScript Code in S3**
1. Update `script.js` to connect with API Gateway:
   ```javascript
   async function fetchTasks() {
       const response = await fetch("<API_GATEWAY_URL>/tasks");
       const tasks = await response.json();
       console.log(tasks);
   }
   ```
2. Upload the updated **script.js** to **S3 Bucket**.
3. Test the application by loading the **S3 Website Endpoint**.

---

## **Step 6: Monitoring with Amazon CloudWatch**
1. Navigate to **AWS CloudWatch > Logs**.
2. Select **Log Group for Lambda Function**.
3. Analyze logs for errors and performance metrics.

---

## **Final Testing & Validation**
1. Open the **S3 Static Website URL**.
2. Add a new task using the frontend.
3. Check **DynamoDB Table** for new entries.
4. Monitor **Lambda logs** for execution success.

---

## **Conclusion**
By completing this hands-on workshop, participants will gain experience in:
✅ **Deploying serverless applications with AWS Lambda and API Gateway**  
✅ **Using DynamoDB for scalable database storage**  
✅ **Hosting static websites on Amazon S3**  
✅ **Integrating frontend applications with a backend API**  
✅ **Monitoring serverless applications using CloudWatch**  

This workshop prepares students for **real-world serverless deployments** using AWS best practices.

