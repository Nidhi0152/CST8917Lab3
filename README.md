# CST8917 – Lab 3: Teams Chat Content Moderation Service

## Objective

This lab demonstrates the use of **Azure Logic Apps** to build a real-time **Microsoft Teams chat moderation system**. The Logic App monitors new Teams messages, detects inappropriate content, and sends an **email alert** when a policy violation is found. The solution is fully serverless and built without custom code or AI models.

---

##  Technologies Used

- **Azure Logic Apps (Consumption Plan)**
- **Microsoft Teams connector**
- **Office 365 Outlook connector**
- (No Azure Functions or Cognitive Services were used)

---

## Steps Performed

### 1. Create Logic App

- Created a **Logic App (Consumption)** in the Azure Portal.

---

### 2. Add Teams Trigger

- Trigger used: `When a new message is added to a channel`
- Signed in with my Microsoft Teams account.
- Selected the appropriate **Team** and **Channel** for monitoring.

---

### 3. Add Content Moderation Logic

- Added a **Condition** to inspect message content.
- Expression used in “Choose a value” field:
  ```text
  @triggerBody()?['body']?['content'] ```
  
- Operator: contains
- Comparison value (banned word): stupid

---

### 4. Add Email Notification (if violation detected)

Under the **"Yes"** path of the condition, added:

📨 **Send an email (V2)** using **Office 365 Outlook** connector.

Configured as follows:

- **To:** My admin email  
- **Subject:** `Policy Violation Detected`  
- **Body (HTML):**

```html
A policy violation was detected.<br><br>
<b>Message:</b> @{triggerBody()?['body']?['content']}<br>
<b>User:</b> @{triggerBody()?['from']?['user']?['displayName']}<br>
<b>Timestamp:</b> @{triggerBody()?['createdDateTime']}
```

----

### 5. Testing

- **Message Sent:** `You are stupid`
- **Expected Behavior:** Email alert should be sent.
- **Result:**  
   Email alert successfully triggered.

**Email Content Included:**
- Message: `You are stupid`
- User: Display name of sender
- Violation Reason: Inappropriate content detected via moderation.
----

###  Challenges Faced

- Could not perform multiple word detection in the Logic App condition (e.g., checking for both "stupid" and "hate" together).
- Plan to explore advanced expressions or integrate Azure Cognitive Services for more robust content analysis in the future.
----
###  Demo Video
 [https://youtu.be/Ny05i80tQY8](https://youtu.be/Ny05i80tQY8)
