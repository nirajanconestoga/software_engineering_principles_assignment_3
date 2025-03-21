# AI Training Data - Actionable Requirements

## Project Overview
Our client, an AI developer, is using web scraping to generate training data for their models. They have two key challenges:
- **Categorization of training questions and answers separately.**
- **Ensuring the dataset is balanced and free from biases.**

This document outlines actionable requirements, along with prioritization for structured implementation.

---
## Priority Levels
🔴 **High Priority** → Critical for system functionality, must be implemented first.  
🟠 **Medium Priority** → Important but not blocking major progress.  
🟢 **Low Priority** → Enhancements or optional features.  

---
## Actionable Requirements

### 1. Automated Categorization of Training Data
**Title:** As an AI developer, I want training questions to be automatically categorized by topic using metadata tagging so that I can easily organize and retrieve relevant data for model training.  
**Priority:** 🔴 High  
**GitHub Issue:** #1  

#### Purpose
After discussions with the development team, we determined that **manual categorization of training questions** is **time-consuming and inconsistent**.
To address this, we will implement an **automated categorization system** that assigns **metadata tags** (e.g., topic, difficulty) to each training question.

#### Implementation Strategy
- **Set up a database** to store categorized questions and metadata.
- **Develop an AI-based categorization model** to classify questions based on their content.
- **Implement a backend API** to provide categorized results to other systems.
- **Build a user interface** to allow developers to review and edit categorizations before finalizing.

##### Sub-Issues

**Sub-Issue 1:** We assume that automated categorization will reduce manual effort and improve consistency.  
**Priority:** 🔴 High  
- Validate this assumption by testing categorization results on three sample datasets (each with 10,000 questions), ensuring the model achieves an **F1-score of at least 85%**.
- Review results with subject matter experts and analyze misclassifications to improve model accuracy and reliability.

**Sub-Issue 2:** Design and Set Up Database for Categorization  
**Priority:** 🟠 Medium  
- **Goal:** Create a **structured database** to store categorized questions, ensuring efficient retrieval and metadata management.
- **Approach:** Use **PostgreSQL** with **indexes for fast retrieval**.
- **Tasks:**
  - Define **metadata categories** (topic, difficulty, length).
  - Create a **PostgreSQL table** to store categorized questions.
  - Add **indexes** to optimize search queries.
  - Write an **SQL script** to initialize the database schema.
  - Develop a **function to insert categorized questions** into the database.

**SQL Implementation Suggestion:**
```sql
CREATE TABLE categorized_questions (
    id SERIAL PRIMARY KEY,
    question_text TEXT NOT NULL,
    category VARCHAR(255),
    difficulty VARCHAR(50),
    metadata JSONB
);
CREATE INDEX idx_category ON categorized_questions(category);
```

**Sub-Issue 3:** Implement AI-Based Categorization Model  
**Priority:** 🔴 High  
- **Goal:** Develop an NLP model that analyzes questions and assigns metadata based on their content.
- **Approach:** Train an AI-based model (e.g., using **spaCy** or **BERT**) to classify questions into predefined categories.
- **Tasks:**
  - Preprocess training data (tokenization, stopword removal).
  - Train the model using labeled questions.
  - Evaluate model accuracy (**F1-score ≥ 85%**).
  - Develop a function that takes a question as input and returns metadata predictions.
  - Optimize model performance to reduce categorization time.

**Python Implementation Suggestion:**
```python
def categorize_question(question_text: str) -> dict:
    """
    Categorizes a question using an NLP model.
    Parameters:
        question_text (str): The input question.
    Returns:
        dict: A dictionary containing predicted category and difficulty.
    """
```

**Sub-Issue 4:** Develop Backend API for Categorization  
**Priority:** 🔴 High  
- **Goal:** Create a REST API that allows external systems to request categorized questions.
- **Approach:** Use **Flask/Django** to connect to the AI model and database.
- **Tasks:**
  - Develop a `POST /categorize` API endpoint to accept a question and return metadata.
  - Connect the API to the AI-based categorization model.
  - Implement `GET /categorized_questions` to retrieve categorized questions from the database.
  - Implement `PUT /update_categorized_question/{id}` to allow category updates.
  - Handle error cases (e.g., missing data, invalid inputs).

**Python Implementation Suggestion:**
```python
@app.route('/categorized_questions', methods=['GET'])
def get_categorized_questions():
    """
    Fetches categorized questions from the database.
    Returns:
        JSON list of categorized questions with metadata.
    """
```

**Sub-Issue 5:** Implement User Interface for Categorization Review  
**Priority:** 🟠 Medium  
- **Goal:** Develop a web interface that allows users to review, edit, and approve categorized questions.
- **Purpose:** Enable AI developers to verify and refine the automated categorization results before finalizing them.
- **Tasks:**
  - **Develop UI Layout**
    - Design a web page that displays categorized questions in a table format.
    - Ensure a responsive design for different screen sizes.
  - **Create Input and Display Fields**
    - Display the question text in a read-only field.
    - Add an editable dropdown for category selection.
    - Add an editable dropdown for difficulty level selection.
    - Include a metadata section where users can add additional tags.
  - **Implement User Actions**
    - Add an **"Edit"** button that allows users to modify category tags.
    - Add a **"Save"** button to confirm category changes.
    - Add a **"Reset"** button to undo any changes before saving.
  - **Implement Search and Filter Functionalities**
    - Add a search bar for searching questions.
    - Implement filter options for category and difficulty level.
  - **Connect UI with Backend API**
    - Fetch categorized data from the API (**GET /categorized_questions**).
    - Implement real-time updates when a user modifies a category.
    - Send the updated data to the API (**PUT /update_categorized_question/{id}**).
  - **Handle Errors and Notifications**
    - Show a **confirmation message** after saving changes.
    - Display an **error message** if saving fails.

---

### 2. Dataset Upload and Structured Storage
**Title:** As an AI developer, I want to upload a dataset and have the system automatically structure and store categorized training questions so that I can efficiently retrieve and manage large volumes of data.  
**Priority:** 🔴 High  
**GitHub Issue:** #2  

#### **Purpose**
After discussions with AI developers, we identified that **manually managing large datasets** is inefficient and prone to errors.  
To address this, we will build a system that allows **automatic dataset uploads, structured storage, and fast retrieval** of categorized training data.

#### **Implementation Strategy**
- **Design a database schema** optimized for storing and retrieving structured datasets.
- **Develop an API** that enables dataset uploads via a web interface or programmatically.
- **Implement automatic indexing** for fast data retrieval.
- **Ensure scalability** to handle large datasets efficiently (up to **10 million entries**).

##### **Sub-Issues**

**Sub-Issue 1:** Assumption Validation - Storage & Indexing Performance  
**Priority:** 🔴 High  
- Validate the assumption that structured storage with appropriate indexing will enhance data retrieval speeds.
- Test dataset uploads up to **10 million entries**, ensuring retrieval times remain under **200ms**, even during concurrent operations.

**Sub-Issue 2:** Design and Set Up Database for Dataset Storage  
**Priority:** 🟠 Medium  
- **Goal:** Create a **scalable and structured database** to store uploaded datasets and categorized training questions efficiently.
- **Approach:** Use **PostgreSQL** as the primary database and implement **indexing for optimized retrieval speeds**.
- **Tasks:**
  - Design **a relational database schema** for storing datasets.
  - Implement **tables for training questions, metadata, and indexing**.
  - Add **indexes** to improve query performance.
  - Write an **SQL script** to create the database schema.
  - Develop a **function to insert structured datasets** into the database.

  **SQL Implementation Suggestion:**
```sql
CREATE TABLE uploaded_datasets (
    id SERIAL PRIMARY KEY,
    dataset_name VARCHAR(255) NOT NULL,
    upload_timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    metadata JSONB
);

CREATE TABLE training_questions (
    id SERIAL PRIMARY KEY,
    dataset_id INT REFERENCES uploaded_datasets(id),
    question_text TEXT NOT NULL,
    category VARCHAR(255),
    difficulty VARCHAR(50),
    metadata JSONB
);

CREATE INDEX idx_dataset_name ON uploaded_datasets(dataset_name);
CREATE INDEX idx_question_text ON training_questions(question_text);
```
**Sub-Issue 3:** Implement API for Dataset Upload  
**Priority:** 🔴 High  
- **Goal:** Develop a **REST API** that allows users to upload datasets via a web interface or programmatically.
- **Approach:** Use **Flask/Django**, supporting **CSV and JSON** uploads.
- **Tasks:**
  - Develop a `POST /upload_dataset` API endpoint to accept dataset files.
  - Validate file format (support **only CSV/JSON**).
  - Store metadata about the uploaded dataset in the database.
  - Process and insert training questions into the structured database.
  - Handle error cases (e.g., invalid format, corrupted files).

**Python Implementation Suggestion:**
```python
@app.route('/upload_dataset', methods=['POST'])
def upload_dataset():
    """
    Handles dataset uploads via API.
    Returns:
        JSON response confirming the upload or an error message.
    """
```