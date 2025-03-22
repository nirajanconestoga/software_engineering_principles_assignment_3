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

**Sub-Issue 1:** We assume that structured storage with appropriate indexing will enhance data retrieval speeds and system efficiency.   
**Priority:** 🔴 High  
- Validate this assumption by testing uploads with datasets up to 10 million entries, and let the client know that the benefit of this is to ensure the system can scale efficiently while maintaining fast retrieval times under 200ms, even during concurrent upload and query operations.

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
**Sub-Issue 4:** Implement Automatic Indexing for Fast Retrieval  
**Priority:** 🟠 Medium  
- **Goal:** Ensure fast search and retrieval of uploaded datasets using indexing and optimized queries.
- **Approach:** Integrate **Elasticsearch** to enable real-time indexing and sub-200ms retrieval times.
- **Tasks:**
  - Install and configure **Elasticsearch** for efficient search indexing.
  - Develop a background process that **automatically indexes datasets** upon upload.
  - Implement a function to **search categorized questions using Elasticsearch**.
  - Optimize query execution times to ensure results are retrieved **within 200ms**.

**Python Implementation Suggestion:**
```python
def index_dataset(dataset_id: int):
    """
    Indexes dataset into Elasticsearch for fast retrieval.
    Parameters:
        dataset_id (int): The ID of the dataset to index.
    Returns:
        Success message or error response.
    """
```
**Sub-Issue 5:** Develop User Interface for Dataset Upload and Management
**Priority:** 🟠 Medium
- **Goal:** Build a user-friendly web interface that allows users to upload, manage, and search datasets easily.
- **Purpose:** Enable AI developers to upload structured datasets, view dataset details, and search for categorized training data. Build a user-friendly web interface that allows users to upload, manage, and search datasets easily.
- **Tasks:**
    - **Develop UI Layout**
        - Design a dataset upload page with drag-and-drop file support.
        - Implement a table view to display uploaded datasets.
        - Ensure a responsive design for desktop and mobile.

    - **Create Input and Display Fields**
        - Create a file upload button for CSV/JSON dataset uploads.
        - Display dataset name, upload timestamp, and metadata.
        - Add a search bar to find datasets by name.

    - **Implement User Actions**
        - Allow users to browse and select a file for upload.
        - Display a progress bar during dataset upload.
        - Show a confirmation message upon successful upload.

    - **Implement Search and Filter Functionalities**
        - Allow users to search training questions by keyword, category, and difficulty.
        - Display search results in real-time using indexed queries.

    - **Connect UI with Backend API**
        - Send dataset files to the API (`POST /upload_dataset`).
        - Fetch dataset details from the API (`GET /datasets`).
        - Implement real-time updates when a dataset is uploaded.

    - **Handle Errors and Notifications**
        - Show a notification message when an upload fails.
        - Display error details (e.g., unsupported file format, upload failure).

---

### 3. Logical Separation of Questions and Answers
**Title:** As an AI developer, I want training questions to be stored separately from answers so that I can ensure unbiased model training and independent validation of AI performance.  
**Priority:** 🔴 High  
**GitHub Issue:** #3  

#### **Purpose**
After discussions with AI developers, we identified that **storing questions and answers together** can lead to **bias in model training** and **inconsistent validation results**.  
To address this, we will implement **a system that ensures complete separation** between training questions and their corresponding answers.

#### **Implementation Strategy**
- **Design a database schema** that stores questions and answers in separate tables.  
- **Develop an API** that allows retrieval of questions and answers independently.  
- **Ensure validation mechanisms** to prevent cross-contamination of question and answer data.  
- **Implement a UI** that allows users to retrieve and manage questions/answers separately. 

##### **Sub-Issues**

**Sub-Issue 1:** We assume that separating questions and answers will reduce bias in model training.
**Priority:** 🔴 High  
- Validate this assumption by inspecting database structures to confirm separation and verifying that questions and answers can be accessed independently.

**Sub-Issue 2:** Design and Set Up Database for Separate Storage  
**Priority:** 🟠 Medium  
- **Goal:** Create a **structured database** where **questions and answers** are stored **in separate tables** to ensure unbiased training.
- **Approach:** Use **PostgreSQL** and store **questions and answers in distinct schemas** to enforce separation.
- **Tasks:**
  - Create a **table for questions** with metadata fields.
  - Create a **table for answers**, linking each answer to a question **via a foreign key**.
  - Implement **indexing** for fast lookup of questions and answers.
  - Write an **SQL script** to initialize the database.
  - Develop a **function to insert questions and answers** separately.

**SQL Implementation Suggestion:**
```sql
CREATE TABLE questions (
    id SERIAL PRIMARY KEY,
    question_text TEXT NOT NULL,
    category VARCHAR(255),
    difficulty VARCHAR(50),
    metadata JSONB
);

CREATE TABLE answers (
    id SERIAL PRIMARY KEY,
    question_id INT REFERENCES questions(id),
    answer_text TEXT NOT NULL,
    metadata JSONB
);

CREATE INDEX idx_question_id ON answers(question_id);
```

**Sub-Issue 3:** Implement API for Independent Question and Answer Retrieval  
**Priority:** 🔴 High  
- **Goal:** Develop a REST API that allows retrieval of questions and answers independently.
- **Approach:** Use **Flask/Django** to enforce separate access to questions and answers.
- **Tasks:**
  - Develop a `GET /questions` API endpoint to retrieve only questions.
  - Develop a `GET /answers/{question_id}` API endpoint to fetch answers separately.
  - Implement a validation mechanism to prevent storing answers in the questions table.
  - Handle error cases (e.g., missing question ID, invalid requests).

**Python Implementation Suggestion:**
```python
@app.route('/questions', methods=['GET'])
def get_questions():
    """
    Fetches all questions from the database.
    Returns:
        JSON list of questions with metadata.
    """
```

**Sub-Issue 4:** Implement Data Validation to Prevent Cross-Contamination  
**Priority:** 🟠 Medium  
- **Goal:** Ensure that questions and answers remain stored separately in the system at all times.
- **Approach:** Implement data validation rules that prevent incorrect data storage or retrieval.
- **Tasks:**
  - Implement a data pipeline check to verify uploads follow the separate schema structure.
  - Develop a validation function that prevents answers from being stored in the questions table.
  - Log violations or incorrect data uploads for monitoring.
  - Implement unit tests to verify that questions and answers remain separate in all operations.

**Python Implementation Suggestion:**
```python
def validate_data_separation(question_data, answer_data):
    """
    Ensures that question data and answer data are stored separately.
    Returns:
        Boolean indicating if the separation is enforced.
    """
```
**Sub-Issue 5:** Develop User Interface for Managing Questions and Answers  
**Priority:** 🟠 Medium  
- **Goal:** Develop a web interface that allows users to retrieve and manage questions and answers independently.
- **Purpose:** Enable AI developers to view, edit, and validate questions and answers separately before using them for training.
- **Tasks:**
  
    - **Develop UI Layout**  
        - Design a two-tab layout:  
            - **Tab 1:** Displays only questions.  
            - **Tab 2:** Displays only answers (linked to a question).  
        - Ensure a responsive design for various screen sizes.

    - **Create Input and Display Fields**  
        - Display the question text in a table format.  
        - Add a separate section for answers linked to a selected question.  
        - Include metadata fields for both questions and answers.

    - **Implement User Actions**  
        - Add a "View Answers" button next to each question.  
        - Allow users to edit questions and answers separately.  
        - Add a "Delete" button that confirms deletion of only questions or only answers.

    - **Implement Search and Filter Functionalities**  
        - Allow users to search questions by keyword or category.  
        - Add a filter for difficulty level to refine questions displayed.  
        - Implement pagination for better navigation of large datasets.

    - **Connect UI with Backend API**  
        - Fetch questions from `GET /questions`.  
        - Fetch answers from `GET /answers/{question_id}` when a question is selected.  
        - Ensure updates to questions do not affect answer storage.

    - **Handle Errors and Notifications**  
        - Show an error message if a user tries to edit an answer in the question panel.  
        - Display a notification when a question or answer is successfully updated.

---

### 4. Bias Analysis and Reporting System
**Title:** As an AI developer, I want to upload a dataset and receive a bias analysis report so that I can assess fairness before using the data for training.  
**Priority:** 🔴 High  
**GitHub Issue:** #4  

#### **Purpose**
After discussions with AI developers, we identified that **biased training datasets** can result in **unfair AI models** that **discriminate against certain groups**.  
To address this, we will build a system that **analyzes datasets for bias**, presents insights through **interactive dashboards**, and provides **exportable reports** for further review.

#### **Implementation Strategy**
- **Develop an AI-based bias detection system** that identifies **demographic and contextual biases**.  
- **Build an API** that allows users to upload datasets and receive **bias analysis reports**.  
- **Design an interactive dashboard** for visualizing bias insights.  
- **Ensure reports can be exported** in **PDF and JSON formats**. 

##### **Sub-Issues**

**Sub-Issue 1:** We assume that bias detection will identify demographic and contextual data biases in uploaded datasets. 
**Priority:** 🔴 High  
- Validate this assumption by comparing bias detection reports with **industry benchmarks** and conducting **user feedback sessions** to confirm the reports’ usefulness.

**Sub-Issue 2:** Define Bias Metrics and Fairness Criteria  
**Priority:** 🟠 Medium  
- **Goal:** Establish **bias detection standards** and define **fairness evaluation metrics**.
- **Approach:** Identify **key bias metrics** using industry standards such as **demographic parity, equalized odds, and statistical parity difference**.
- **Tasks:**
  - **Interview AI developers** to determine necessary bias metrics.
  - **Review historical datasets** to identify common bias patterns.
  - **Define fairness metrics**, including:
    - **Demographic parity**  
    - **Equalized odds**  
    - **Statistical parity difference**  
  - **Research industry best practices** for fairness analysis (e.g., IBM’s AIF360, Microsoft’s Fairlearn).
  - **Document bias detection criteria** for system implementation.

**Sub-Issue 3:** Implement Bias Detection Model  
**Priority:** 🔴 High  
- **Goal:** Develop a **bias detection model** that analyzes training datasets for demographic and contextual biases.
- **Approach:** Use **open-source fairness toolkits** such as **AIF360 and Fairlearn**.
- **Tasks:**
  - **Integrate AIF360 and Fairlearn** for bias detection.
  - **Preprocess training data** to extract relevant features.
  - **Run bias analysis algorithms** to detect **disparities in representation**.
  - **Generate a bias report** summarizing **key findings and recommendations**.
  - **Optimize detection algorithms** to ensure accuracy.

**Python Implementation Suggestion:**
```python
from aif360.datasets import BinaryLabelDataset
from aif360.metrics import BinaryLabelDatasetMetric

def analyze_bias(dataset: BinaryLabelDataset):
    """
    Analyzes dataset for demographic bias using AIF360.
    Returns:
        dict: A dictionary containing bias metrics and fairness insights.
    """
```

**Sub-Issue 4:** Develop API for Bias Detection and Report Generation  
**Priority:** 🔴 High  
- **Goal:** Create a REST API that allows users to upload datasets and receive bias reports.
- **Approach:** Use **Flask/Django** to connect to the bias detection model.
- **Tasks:**
  - Develop a `POST /upload_dataset` API endpoint to accept dataset files.
  - Validate file format (supporting only CSV/JSON).
  - Run bias analysis on the uploaded dataset.
  - Generate a downloadable report in PDF and JSON formats.
  - Handle error cases (e.g., unsupported format, missing data).

**Python Implementation Suggestion:**
```python
@app.route('/analyze_bias', methods=['POST'])
def analyze_bias():
    """
    API endpoint to upload a dataset and receive a bias analysis report.
    Returns:
        JSON response with bias metrics or an error message.
    """
```

**Sub-Issue 5:** Implement Interactive Dashboard for Bias Analysis  
**Priority:** 🟠 Medium  
- **Goal:** Develop a web interface that allows users to visualize bias insights interactively.
- **Purpose:** Enable AI developers to explore bias metrics visually and download reports.
- **Tasks:**
    - **Develop UI Layout**  
        - Design a dashboard layout displaying key bias metrics and insights.
        - Ensure a responsive design for desktop and mobile.

    - **Create Data Visualization Components**  
        - Implement bar charts and heatmaps to display bias distributions.
        - Add dropdown filters to analyze bias across different demographic groups.
        - Show summary statistics on dataset fairness.

    - **Implement User Actions**  
        - Add a "Run Analysis" button to trigger bias detection.
        - Provide an "Export Report" button to download bias reports in PDF/JSON.

    - **Connect UI with Backend API**  
        - Send uploaded datasets to the API (`POST /upload_dataset`).
        - Fetch bias analysis results from `GET /bias_report/{dataset_id}`.
        - Update visualizations in real-time as new reports are generated.

    - **Handle Errors and Notifications**  
        - Show an error message if a dataset upload fails.
        - Display a notification when bias analysis completes successfully.

---