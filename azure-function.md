## **Azure Functions: Serverless Compute Service**

Azure Functions is a **serverless computing service** that allows you to run event-driven code without managing infrastructure. It automatically scales based on demand and charges only for the execution time used.

---

## **🔹 Key Features of Azure Functions**

### **1. Event-Driven Execution**

- Triggers execution based on **events** (HTTP requests, messages in queues, database changes, etc.).
- Supports a wide range of triggers (e.g., HTTP, Timer, Blob Storage, Event Hub).

### **2. Fully Managed & Serverless**

- No need to manage servers—Azure automatically scales based on demand.
- Ideal for background tasks, APIs, and automation scripts.

### **3. Pay-Per-Use Pricing**

- **Consumption Plan**: You only pay for the execution time and resources used.
- **Premium Plan**: Provides better performance and supports VNET integration.
- **Dedicated Plan**: Runs on App Service with reserved capacity.

### **4. Multiple Language Support**

- Supports **C#, Python, Java, JavaScript, PowerShell, TypeScript, and Go**.

### **5. Integration with Azure Services**

- Easily integrates with **Azure Storage, Cosmos DB, Event Hubs, Service Bus, and more**.
- Supports CI/CD using **GitHub Actions, Azure DevOps, and Terraform**.

---

## **🔹 How Azure Functions Work**

1️⃣ **Trigger an event** (e.g., HTTP request, file upload, message in a queue).  
2️⃣ **Function executes** automatically and processes the event.  
3️⃣ **Function interacts** with other services (database, storage, APIs).  
4️⃣ **Function returns output** or performs an action.

---

## **🔹 Example: Create an HTTP-Triggered Azure Function**

### **1️⃣ Create a Function App in Azure**

1. Go to **Azure Portal** → Create a **Function App**.
2. Select a **Runtime Stack** (e.g., Python, C#, Node.js).
3. Choose a **Hosting Plan** (Consumption, Premium, or Dedicated).

### **2️⃣ Example: HTTP-Triggered Function (Python)**

```python
import azure.functions as func

def main(req: func.HttpRequest) -> func.HttpResponse:
    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
            name = req_body.get('name')
        except ValueError:
            pass

    if name:
        return func.HttpResponse(f"Hello, {name}!")
    else:
        return func.HttpResponse("Please pass a name in the request.", status_code=400)
```

**Deployment Options:**  
✅ Azure CLI (`az functionapp deploy`)  
✅ Visual Studio Code with Azure Extensions  
✅ GitHub Actions / Azure DevOps Pipelines

---

## **🔹 Common Use Cases for Azure Functions**

✅ **REST APIs & Microservices** – Build lightweight serverless APIs.  
✅ **Automated Workflows** – Run scheduled tasks (e.g., send reports, clean databases).  
✅ **Data Processing** – Process messages from queues or event streams.  
✅ **IoT & Real-time Processing** – Handle IoT sensor data in real time.  
✅ **File Processing** – Resize images, analyze logs, or transform files in storage.

Would you like help setting up an Azure Function? 🚀

Got it! Below are examples of **Timer Trigger**, **Blob Trigger**, and **Service Bus Topic Trigger** in **.NET Core (C#)** for Azure Functions.

---

## **1️⃣ Timer Trigger (Scheduled Execution)**

A **Timer Trigger** runs a function at a scheduled time using a CRON expression.

### **Example: Run Every 5 Minutes**

```csharp
using System;
using Microsoft.Azure.WebJobs;
using Microsoft.Extensions.Logging;

public static class TimerTriggerFunction
{
    [FunctionName("TimerTriggerFunction")]
    public static void Run(
        [TimerTrigger("0 */5 * * * *")] TimerInfo myTimer,
        ILogger log)
    {
        log.LogInformation($"Timer triggered at: {DateTime.UtcNow}");

        if (myTimer.IsPastDue)
        {
            log.LogWarning("The timer is running late!");
        }
    }
}
```

🔹 **Schedule Format (CRON Expression)**:

- `"0 */5 * * * *"` → Every 5 minutes
- `"0 30 9 * * *"` → At 9:30 AM daily
- `"0 0 0 * * 1"` → Every Monday at midnight

---

## **2️⃣ Blob Storage Trigger (File Upload Processing)**

A **Blob Trigger** executes when a file is added or updated in Azure Blob Storage.

### **Example: Process a File Uploaded to Blob Storage**

```csharp
using System;
using System.IO;
using Microsoft.Azure.WebJobs;
using Microsoft.Extensions.Logging;

public static class BlobTriggerFunction
{
    [FunctionName("BlobTriggerFunction")]
    public static void Run(
        [BlobTrigger("samples-container/{name}", Connection = "AzureWebJobsStorage")] Stream myBlob,
        string name,
        ILogger log)
    {
        log.LogInformation($"Processing blob\n Name: {name} \n Size: {myBlob.Length} bytes");
    }
}
```

🔹 **How it Works?**

- **Triggers when a file is uploaded/modified** in `samples-container`.
- **`name`** → The name of the uploaded file.
- **`AzureWebJobsStorage`** → Connection string in `local.settings.json`.

---

## **3️⃣ Service Bus Topic Trigger (Message Processing)**

A **Service Bus Topic Trigger** runs when a message is published to an Azure Service Bus **topic**.

### **Example: Process Messages from a Service Bus Topic**

```csharp
using Microsoft.Azure.WebJobs;
using Microsoft.Extensions.Logging;

public static class ServiceBusTopicTriggerFunction
{
    [FunctionName("ServiceBusTopicTriggerFunction")]
    public static void Run(
        [ServiceBusTrigger("my-topic", "my-subscription", Connection = "ServiceBusConnection")] string message,
        ILogger log)
    {
        log.LogInformation($"Received message: {message}");
    }
}
```

🔹 **How it Works?**

- Listens to messages on **`my-topic`** for **`my-subscription`**.
- **`ServiceBusConnection`** → Defined in `local.settings.json`.

---

## **🔹 local.settings.json (Connection Strings Example)**

When running locally, update your `local.settings.json` file:

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "UseDevelopmentStorage=true",
    "ServiceBusConnection": "<Your-Service-Bus-Connection-String>"
  }
}
```

---

## **🔹 Summary of Triggers**

| **Trigger**                   | **Usage**                                                 |
| ----------------------------- | --------------------------------------------------------- |
| **Timer Trigger**             | Runs at scheduled intervals (e.g., every 5 minutes).      |
| **Blob Trigger**              | Runs when a file is uploaded or modified in Blob Storage. |
| **Service Bus Topic Trigger** | Runs when a message is received in a Service Bus Topic.   |

Would you like help with deployment or testing? 🚀
