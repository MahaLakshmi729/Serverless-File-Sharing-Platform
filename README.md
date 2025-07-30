# Serverless-File-Sharing-Platform

Project Description
The Serverless File Sharing Platform is a simple and secure way to upload and download files using an HTTP API. It is built using AWS services like Lambda (to run the code), API Gateway (to handle HTTP requests), and Amazon S3 (to store the files). This platform doesn’t require any server setup or maintenance. Users can send a POST request to upload files by specifying the file name and content, and a GET request to download them using a pre-signed link. It works well with tools like Postman or curl. This kind of platform is useful for sharing documents, distributing files like images, videos, or software, and enabling team collaboration by allowing people in different locations to access and share files securely.

Use Cases:

* File Sharing: Users can upload files to the platform using a POST request, specifying the file name and content. This can be useful for sharing documents, images, or any other digital assets securely.
* File Distribution: Content creators can distribute files (e.g., software updates, media files) to consumers by generating download links through the platform.
* Collaborative Work: Teams can share project resources, documents, and data securely, facilitating collaboration across different locations.


## ✅ Step-by-Step Guide to Build a Serverless File Sharing Platform

### 🔹 Step 1: Create an S3 Bucket

* Go to the **AWS Console** → Search for **S3**
* Click **“Create Bucket”**
* Give it a name (e.g., `file-sharing-bucket`)
* Click **Create**

---

### 🔹 Step 2: Create the Upload Lambda Function

* Go to **AWS Lambda** → **Create function**
* Choose **Author from scratch**
* Name it `UploadFunction`
* Choose runtime (Python, Node.js, etc.)
* Paste your code to upload a file to S3
* In the **Permissions**, ensure the role has `s3:PutObject` permission to your bucket

---

### 🔹 Step 3: Create the Download Lambda Function

* Repeat the same steps as above
* Name it `DownloadFunction`
* This function should generate a **pre-signed S3 URL** to download a file
* Give the role `s3:GetObject` permission

---

### 🔹 Step 4: Set Up API Gateway

* Go to **API Gateway** → Create a new **HTTP API**
* Add two routes:

  * `POST /files` → Integrate with `UploadFunction`
  * `GET /files?fileName=...` → Integrate with `DownloadFunction`
* Deploy the API

---

## ⚙️ API Gateway Configuration

### ✅ Configure **GET Method**

#### **Method Request**

* Click **Edit**
* Set **Request Validator** to:
  **✔️ Validate query string parameters and headers**
* Add **Query String Parameter**:
  `fileName` → **Required**

#### **Integration Request**

* Click **Edit** → **Mapping Templates**
* Add **Content-Type**: `application/json`
* Set the **Template Body**:

```json
{
  "queryStringParameters": {
    "fileName": "$input.params('fileName')"
  }
}
```

---

### ✅ Configure **POST Method**

#### **Method Request**

* Accepts `text/plain` request body and query param `fileName`
  (Optional: add validator if needed)

#### **Integration Request**

* Click **Edit** → **Mapping Templates**
* Add **Content-Type**: `text/plain`
* Set the **Template Body**:

```json
{
  "body" : "$input.body",
  "queryStringParameters" : {
    "fileName" : "$input.params('fileName')"
  }
}
```

### 🔹 Step 5: Test with Postman or curl

* To **upload** a file:

  ```bash
  curl --location 'https://your-api-url/dev/files?fileName=test.txt' \
  --header 'Content-Type: text/plain' \
  --data 'Hello World!'
  ```

* To **download** a file:

  ```bash
  curl 'https://your-api-url/dev/files?fileName=test.txt'
  ```

  It should return a pre-signed URL — open it to download the file.

---

### 🔹 Step 6: Secure the APIs 

* Add **API Key**, **IAM**, or **JWT** authentication
* Limit file types and sizes in Lambda code
* Add logging using **CloudWatch**

---




