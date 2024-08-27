// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ExamSystem {
    enum UserType { None, Student, Instructor }
    enum QuestionType { MultipleChoice, TrueFalse }

    struct User {
        UserType userType;
        string name;
    }

    struct Question {
        uint256 id;
        string questionText;
        string[] options; // Multiple choice options
        uint256 correctOptionIndex;
        QuestionType questionType;
    }

    struct Exam {
        uint256 id;
        string examName;
        uint256[] questionIds;
        bool isActive;
    }

    struct Answer {
        uint256 questionId;
        uint256 selectedOptionIndex; // For multiple-choice questions
        bool answer; // For true/false questions
    }

    mapping(address => User) public users;
    mapping(uint256 => Question) public questions;
    mapping(uint256 => Exam) public exams;
    mapping(address => mapping(uint256 => Answer)) public studentAnswers;
    mapping(address => mapping(uint256 => uint256)) public studentScores;

    uint256 public nextQuestionId;
    uint256 public nextExamId;
    address public owner;

    event UserRegistered(address indexed userAddress, UserType userType, string name);
    event ExamCreated(uint256 indexed examId, string examName);
    event QuestionAdded(uint256 indexed questionId, string questionText);
    event AnswerSubmitted(address indexed student, uint256 indexed examId, uint256 questionId, uint256 selectedOptionIndex, bool answer);
    event ExamGraded(address indexed student, uint256 indexed examId, uint256 score);

    constructor() {
        owner = msg.sender;
    }

    modifier onlyInstructor() {
        require(users[msg.sender].userType == UserType.Instructor, "Only instructors can perform this action.");
        _;
    }

    modifier onlyStudent() {
        require(users[msg.sender].userType == UserType.Student, "Only students can perform this action.");
        _;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can perform this action.");
        _;
    }

    // Register a user as a Student or Instructor
    function registerUser(string memory _name, UserType _userType) public {
        require(users[msg.sender].userType == UserType.None, "User already registered");
        require(_userType == UserType.Student || _userType == UserType.Instructor, "Invalid user type");

        users[msg.sender] = User({
            userType: _userType,
            name: _name
        });

        emit UserRegistered(msg.sender, _userType, _name);
    }

    // Add a new question
    function addQuestion(
        string memory _questionText,
        string[] memory _options,
        uint256 _correctOptionIndex,
        QuestionType _questionType
    ) public onlyInstructor {
        questions[nextQuestionId] = Question({
            id: nextQuestionId,
            questionText: _questionText,
            options: _options,
            correctOptionIndex: _correctOptionIndex,
            questionType: _questionType
        });

        emit QuestionAdded(nextQuestionId, _questionText);
        nextQuestionId++;
    }

    // Create a new exam
    function createExam(string memory _examName, uint256[] memory _questionIds) public onlyInstructor {
        exams[nextExamId] = Exam({
            id: nextExamId,
            examName: _examName,
            questionIds: _questionIds,
            isActive: true
        });

        emit ExamCreated(nextExamId, _examName);
        nextExamId++;
    }

    // Submit an answer for a question in an exam
    function submitAnswer(uint256 _examId, uint256 _questionId, uint256 _selectedOptionIndex, bool _answer) public onlyStudent {
        require(exams[_examId].isActive, "Exam is not active");
        require(questions[_questionId].id == _questionId, "Question does not exist");

        studentAnswers[msg.sender][_questionId] = Answer({
            questionId: _questionId,
            selectedOptionIndex: _selectedOptionIndex,
            answer: _answer
        });

        emit AnswerSubmitted(msg.sender, _examId, _questionId, _selectedOptionIndex, _answer);
    }

    // Grade an exam for a student
    function gradeExam(address _student, uint256 _examId) public onlyInstructor {
        require(exams[_examId].isActive, "Exam is not active");
        uint256 score = 0;

        Exam memory exam = exams[_examId];
        for (uint256 i = 0; i < exam.questionIds.length; i++) {
            Question memory question = questions[exam.questionIds[i]];
            Answer memory answer = studentAnswers[_student][exam.questionIds[i]];

            if (question.questionType == QuestionType.MultipleChoice) {
                if (answer.selectedOptionIndex == question.correctOptionIndex) {
                    score++;
                }
            } else if (question.questionType == QuestionType.TrueFalse) {
                if (answer.answer == (question.correctOptionIndex == 1)) { // Assuming correctOptionIndex 1 represents 'True'
                    score++;
                }
            }
        }

        studentScores[_student][_examId] = score;
        emit ExamGraded(_student, _examId, score);
    }

    // Get exam details
    function getExamDetails(uint256 _examId) public view returns (string memory, uint256[] memory, bool) {
        Exam memory exam = exams[_examId];
        return (exam.examName, exam.questionIds, exam.isActive);
    }

    // Get question details
    function getQuestionDetails(uint256 _questionId) public view returns (string memory, string[] memory, uint256, QuestionType) {
        Question memory question = questions[_questionId];
        return (question.questionText, question.options, question.correctOptionIndex, question.questionType);
    }

    // Get a student's score for an exam
    function getStudentScore(address _student, uint256 _examId) public view returns (uint256) {
        return studentScores[_student][_examId];
    }
}

