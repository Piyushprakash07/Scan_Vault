## 🛠️ Step-by-Step Implementation: Automating Receipt Processing Using AWS

This guide walks you through the full setup of a serverless receipt processing pipeline using AWS services.

---

## 1️⃣ Create the S3 Bucket (for uploading receipts)

### ✅ Steps:
1. Go to the **S3 Console** → Click **Create Bucket**
2. Name your bucket (e.g., `storage-receipt-omkarsharma2821`)
3. Choose a region (e.g., `ap-south-1`)
4. Click **Create bucket**
5. Create Organizational folder inside bucket.
6. Name it **incoming** inside this you will upload files.

![s3](https://github.com/Piyushprakash07/Scan_Vault/blob/main/Summer%20Training%20project/1.png?raw=true)


![s_3](https://github.com/Piyushprakash07/Scan_Vault/blob/main/Summer%20Training%20project/2.png?raw=true)


---

## 2️⃣ Create a DynamoDB Table (to store extracted data)

### ✅ Steps:
1. Go to the **DynamoDB Console** → Click **Create Table**
2. Table name: `Receipts-table`
3. Partition key: `receipt_id` (String)
4. Sort-key: `date` (String)
5. Click **Create**

![dynamo_db](https://github.com/Piyushprakash07/Scan_Vault/blob/main/Summer%20Training%20project/3.png?raw=true)


![dynamo_db2](https://github.com/Piyushprakash07/Scan_Vault/blob/main/Summer%20Training%20project/4.png?raw=true)


---

## 3️⃣ Set Up Amazon SES (to send emails)

### ✅ Steps:
1. Go to **Amazon SES Console**
2. Verify your sender email under **Verified Identities**
3. (Optional) Verify recipient email if your account is in sandbox mode
4. Note the region (e.g., `ap-south-1`) – you’ll need it in your Lambda


![SES](https://github.com/Piyushprakash07/Scan_Vault/blob/main/Summer%20Training%20project/5.png?raw=true)


![SES2](https://github.com/Piyushprakash07/Scan_Vault/blob/main/Summer%20Training%20project/6.png?raw=true)
![SES3](https://github.com/Piyushprakash07/Scan_Vault/blob/main/Summer%20Training%20project/7.png?raw=true)


---

## 4️⃣ Create IAM Role for Lambda Execution

### ✅ Steps:
1. Go to the **IAM Console** → Roles → Create Role
2. Choose **Lambda** as the use case
3. Attach the following policies:
  
```
   - `AmazonS3ReadOnlyAccess`
   - `AmazonTextractFullAccess`
   - `AmazonDynamoDBFullAccess`
   - `AmazonSESFullAccess`
   - `AWSLambdaBasicExecutionRole`
```
4. Name the role: `LambdaReceiptProcessingRole`


![IAM](https://github.com/Piyushprakash07/Scan_Vault/blob/main/Summer%20Training%20project/8.png?raw=true)

![IAM](https://github.com/Piyushprakash07/Scan_Vault/blob/main/Summer%20Training%20project/9.png?raw=true)


---

## 5️⃣ Create Lambda Function (processing engine)

### ✅ Steps:
1. Go to **AWS Lambda Console** → Click **Create Function**
2. Name: `ProcessReceiptFunction`
3. Runtime: **Python 3.9** or **Node.js**
4. Choose existing role → Select `LambdaReceiptProcessingRole`
5. Go to **configuration** tab inside **environment** varibales add this.
6. Go to the **Code** tab and add the pyhton code that I provide in **python.py** file and click **Deploy**.
7. Go to configuration tab > General configuration  > edit
8. Increase the timeout from 0.3 sec to 2 min for complex file.

![lambda](https://github.com/Piyushprakash07/Scan_Vault/blob/main/Summer%20Training%20project/10.png?raw=true)

![lam2](https://github.com/Piyushprakash07/Scan_Vault/blob/main/Summer%20Training%20project/11.png?raw=true)

![lam3](https://github.com/Piyushprakash07/Scan_Vault/blob/main/Summer%20Training%20project/12.png?raw=true)

![lam4](https://github.com/Piyushprakash07/Scan_Vault/blob/main/Summer%20Training%20project/13.png?raw=true)


## 6️⃣ Again go to S3 Bucket.
✅ Steps:
1. In the `Properties` Tab
2. Add the Event Notification 
3. Prefix : incoming/
4. Object creation : Select All object create events

![event](https://github.com/Piyushprakash07/Scan_Vault/blob/main/Summer%20Training%20project/14.png?raw=true)

![event2](https://github.com/Piyushprakash07/Scan_Vault/blob/main/Summer%20Training%20project/15.png?raw=true)

![event3](https://github.com/Piyushprakash07/Scan_Vault/blob/main/Summer%20Training%20project/16.png?raw=true)


> **_Wait for 30 sec and also check in spam folder for the mail....if you do not receive mail after 2 min go to the monitor tab in Lambda Function and check the log groups in cloudwatch. you can verify in dynamodb table as well below are the attached proofs_**


![check](https://github.com/Piyushprakash07/Scan_Vault/blob/main/Summer%20Training%20project/17.png?raw=true)

![check2](https://github.com/Piyushprakash07/Scan_Vault/blob/main/Summer%20Training%20project/18.png?raw=true)

![check3](https://github.com/Piyushprakash07/Scan_Vault/blob/main/Summer%20Training%20project/19.png?raw=true)\

![final_check](https://github.com/Piyushprakash07/Scan_Vault/blob/main/WhatsApp%20Image%202025-08-12%20at%2008.16.47_99ed84c5.jpg?raw=true)
