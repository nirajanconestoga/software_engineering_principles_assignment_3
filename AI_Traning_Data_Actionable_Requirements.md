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