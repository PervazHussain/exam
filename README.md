# Smart Contract-Based Exams System

## Overview

The **Smart Contract-Based Exams System** is a decentralized application built on the Ethereum blockchain that allows for secure and transparent management of exams. This system supports exam creation, question management, student registration, answer submission, and grading. It leverages blockchain technology to ensure immutability and security of academic assessments.

## Features

- **Exam Creation**: Create new exams with multiple-choice and true/false questions.
- **Question Management**: Add and manage exam questions.
- **Student Registration**: Register students for exams.
- **Answer Submission**: Submit answers for registered exams.
- **Grading**: Grade exams and compute scores based on student responses.
- **Data Retrieval**: Fetch details about exams, questions, and student scores.

## Contract Functions

### User Management

- `registerUser(string _name, UserType _userType)`: Register a user as a Student or Instructor.
  - **Parameters**:
    - `_name`: Name of the user.
    - `_userType`: Type of the user (Student or Instructor).

### Question Management

- `addQuestion(string _questionText, string[] _options, uint256 _correctOptionIndex, QuestionType _questionType)`: Add a new question to the system.
  - **Parameters**:
    - `_questionText`: Text of the question.
    - `_options`: Array of options (for multiple-choice questions).
    - `_correctOptionIndex`: Index of the correct option (for multiple-choice) or 0/1 (for true/false).
    - `_questionType`: Type of the question (MultipleChoice or TrueFalse).

### Exam Management

- `createExam(string _examName, uint256[] _questionIds)`: Create a new exam with a list of question IDs.
  - **Parameters**:
    - `_examName`: Name of the exam.
    - `_questionIds`: Array of question IDs included in the exam.

- `endExam(uint256 _examId)`: End an active exam.
  - **Parameters**:
    - `_examId`: ID of the exam to be ended.

### Student Interaction

- `submitAnswer(uint256 _examId, uint256 _questionId, uint256 _selectedOptionIndex, bool _answer)`: Submit an answer for a specific question in an exam.
  - **Parameters**:
    - `_examId`: ID of the exam.
    - `_questionId`: ID of the question.
    - `_selectedOptionIndex`: Selected option index (for multiple-choice) or not used.
    - `_answer`: True/False answer (for true/false questions).

### Grading

- `gradeExam(address _student, uint256 _examId)`: Grade an exam for a student.
  - **Parameters**:
    - `_student`: Address of the student.
    - `_examId`: ID of the exam to be graded.

### Data Retrieval

- `getExam(uint256 _examId)`: Get details of an exam.
  - **Parameters**:
    - `_examId`: ID of the exam.
  - **Returns**:
    - Name of the exam and active status.

- `getQuestion(uint256 _questionId)`: Get details of a question.
  - **Parameters**:
    - `_questionId`: ID of the question.
  - **Returns**:
    - Text of the question, options, correct option index, and question type.

- `getStudentScore(address _student, uint256 _examId)`: Get a student's score for an exam.
  - **Parameters**:
    - `_student`: Address of the student.
    - `_examId`: ID of the exam.
  - **Returns**:
    - Score of the student.

## Deployment

1. **Compile the Contract**: Use a Solidity compiler such as Remix IDE, Hardhat, or Truffle to compile the smart contract.
2. **Deploy the Contract**: Deploy the compiled contract to an Ethereum testnet (e.g., Rinkeby, Goerli) or the Ethereum mainnet using tools like Remix, Hardhat, or Truffle.
3. **Interact with the Contract**: Use Ethereum libraries like Ethers.js or Web3.js to interact with the deployed contract. You can call functions, register users, add questions, create exams, and more.

## Setup

1. **Install Dependencies**: 
   - Make sure you have Node.js and npm installed.
   - Install required libraries using npm:
     ```bash
     npm install ethers web3
     ```

2. **Configure Environment**:
   - Set up your Ethereum wallet and provider (Infura, Alchemy, or local node).
   - Create a `.env` file to store sensitive information such as private keys and API keys.

3. **Run Tests**: Before deploying, test the contract on a local blockchain or testnet:
   ```bash
   npx hardhat test
