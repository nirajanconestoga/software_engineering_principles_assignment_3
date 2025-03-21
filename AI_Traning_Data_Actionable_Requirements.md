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
