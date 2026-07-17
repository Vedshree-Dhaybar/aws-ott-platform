# 🎌 AWS Anime OTT Streaming Platform

This platform provides fans with a seamless anime streaming experience, offering adaptive bitrate video playback, an interactive content catalog, and user profile tracking. By leveraging AWS specialized media workflows and serverless compute, the platform handles massive spikes during new episode drops without needing dedicated server maintenance.

---

## 🛠️ AWS Services Used

**Amazon S3:** Serves a dual purpose—hosts the static frontend web application files and acts as the storage repository for all anime video files and thumbnail images.
*   **Amazon API Gateway:** Exposes secure RESTful HTTP endpoints that act as the single entry-point for routing all user requests from the frontend client.
*   **AWS Lambda:** Executes backend business logic (fetching catalog data, managing watchlists, handling search operations) as lightweight, event-driven functions.
*   **Amazon DynamoDB:** A high-speed, fully managed NoSQL database that stores anime metadata (titles, episodes, categories, descriptions) and user activity records.
---

## ⚙️ How It Was Implemented
### 1. Static Web Hosting & Asset Storage
The frontend client interface code is uploaded to a public **Amazon S3** bucket configured for static web hosting. A separate, dedicated directory/bucket within S3 holds the raw web-optimized video files (`.mp4`) and visual assets, serving them directly to the client's built-in web video player.

### 2. Request Routing & API Layer
When a user searches for an anime or interacts with the user interface dashboard, the frontend issues an asynchronous HTTP request. This request is captured by **Amazon API Gateway**, which validates the request routing parameters and forwards the event package to the designated backend service handler.

### 3. Serverless Backend Compute
**AWS Lambda** microservices process the request payloads triggered by the API Gateway. Because these functions are completely stateless and serverless, they spin up instantly in response to concurrent spikes in user activity and execute script operations within milliseconds.

### 4. Database Querying & Storage
The Lambda functions communicate with **Amazon DynamoDB** to perform CRUD operations. The database stores flexible schema objects representing the anime catalog. When fetching the video feed dashboard, Lambda performs queries on DynamoDB indexes to pull the metadata and matching S3 file source URLs, packaging it all back into a clean JSON response for the frontend player.
