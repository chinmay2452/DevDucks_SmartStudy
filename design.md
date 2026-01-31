# SmartStudy AI - Hackathon Prototype Design

## Architecture Overview

SmartStudy AI is a serverless AWS hackathon prototype demonstrating AI-powered codebase learning assistance. The system analyzes GitHub repositories and generates adaptive learning experiences through behavioral monitoring and intelligent explanation generation.

## High-Level Architecture

```
[Web UI] → [API Gateway] → [Lambda Services] → [AWS Bedrock]
                                ↓
                         [Amazon S3] ← → [DynamoDB]
```

### Component Responsibilities

- **Web UI**: Single-page application for repository input and learning interface
- **API Gateway**: REST endpoints for repository processing and learning interactions
- **Lambda Services**: Serverless compute for codebase analysis and AI orchestration
- **AWS Bedrock**: Foundation model services for code analysis and explanation generation
- **Amazon S3**: Temporary storage for cloned repository files
- **DynamoDB**: Session state and user interaction tracking

## Core Components

### 1. Repository Processing Service

**Lambda-based Repository Handler**
- Accepts GitHub repository URLs via API Gateway
- Clones repository to temporary S3 storage
- Performs basic file structure analysis
- Filters for Python/JavaScript files only
- Stores repository metadata in DynamoDB
- Triggers codebase analysis workflow

**S3 Storage**
- Temporary storage for cloned repositories
- Automatic cleanup for prototype demonstration
- Session-based organization structure

### 2. AI Analysis Engine

**Lambda-based Analysis Service**
- Invokes AWS Bedrock for repository overview generation
- Identifies entry points and key architectural patterns
- Generates learning path recommendations
- Creates file-level summaries for important components
- Stores analysis results in DynamoDB

**AWS Bedrock Integration**
- Lightweight foundation model for cost-effective analysis
- Prompts optimized for code structure understanding
- Context-aware explanation generation

### 3. Learning Path Generation

**Lambda-based Path Service**
- Analyzes codebase structure and dependencies
- Creates ordered learning sequence based on:
  - Entry point identification
  - Import/dependency relationships
  - File complexity assessment
  - Architectural importance
- Generates step-by-step navigation recommendations

### 4. Interactive Explanation System

**Lambda-based Explanation Service**
- Processes user requests for code explanations
- Maintains conversation context within session
- Adapts explanation complexity based on user behavior
- Integrates with confusion detection system

**Explanation Adaptation Logic**
- Default: Intermediate-level technical explanations
- Confusion detected: Simplified language, more examples
- Good comprehension: Concise, focused explanations

### 5. Confusion Detection System

**Lambda-based Behavior Monitor**
- Monitors user interaction patterns
- Processes behavioral indicators from DynamoDB
- Triggers explanation adaptation when confusion detected

**Detection Patterns**

*Navigation Behavior*
- Repeated visits to same files within short timeframes
- Erratic navigation without logical progression
- Frequent backtracking to previously visited files

*Interaction Timing*
- Extended periods on single code sections
- Rapid switching between multiple files
- Prolonged inactivity during learning sessions

*Communication Patterns*
- Repeated clarification requests on similar concepts
- Multiple questions about the same code areas
- Increasing frequency of help requests

### 6. Session Management

**DynamoDB Tables**
- User session tracking with interaction history
- Repository analysis caching
- Learning progress and behavioral indicators
- Explanation context and adaptation state

## AI Integration Strategy

### Codebase Overview Generation
1. Repository structure analysis via file tree traversal
2. Foundation model prompting for beginner-friendly overviews
3. Key component identification through import analysis
4. Architecture pattern recognition for common frameworks

### File-Level Explanations
1. Code content extraction and preprocessing
2. Context-aware prompting with surrounding file information
3. Explanation generation tailored to detected user experience level
4. Integration of confusion detection feedback for adaptive responses

### Learning Path Recommendation
1. Dependency graph construction from import statements
2. Complexity assessment based on file characteristics
3. Entry point prioritization for logical learning progression
4. Sequential path generation optimized for understanding

## Behavioral Monitoring Implementation

### Data Collection
- Frontend tracks user interactions and navigation patterns
- API calls logged with session context
- Behavioral indicators stored in DynamoDB
- Real-time analysis via Lambda triggers

### Confusion Detection Pipeline
1. **Data Ingestion**: User actions captured and stored
2. **Pattern Analysis**: Behavioral patterns processed for confusion indicators
3. **Threshold Evaluation**: Patterns evaluated against heuristic rules
4. **Adaptation Trigger**: Explanation system adjusted when confusion detected

### Adaptive Response System
- **Immediate**: Simplified explanations for current interaction
- **Session-wide**: Adjusted complexity for remaining learning path
- **Progressive**: Gradual complexity increase as understanding improves

## Prototype Constraints and Trade-offs

### Hackathon Scope
- **Repository Size**: Limited to small-medium repositories for demonstration
- **Language Support**: Python and JavaScript only for focused prototype scope
- **Session Duration**: Optimized for short demonstration sessions
- **Single User**: Individual learning sessions without multi-user complexity

### Technical Limitations
- **Serverless Architecture**: Cost-effective for prototype with potential cold start delays
- **Temporary Storage**: Session-based storage with automatic cleanup
- **Basic AI Integration**: Simplified confusion detection suitable for demonstration
- **Public Access**: No authentication required for hackathon presentation

### Prototype Trade-offs
- **Lambda Concurrency**: Limited concurrent executions for cost control
- **Storage Retention**: Temporary data storage with automatic purging
- **AI Model Usage**: Rate limiting to manage API costs during demonstration
- **Monitoring**: Basic logging and metrics for prototype validation

## Security and Data Handling

### Data Management
- Repository data stored temporarily for demonstration purposes
- No persistent user data collection beyond session scope
- Public repository access only
- Automatic data cleanup after sessions

### Access Control
- API Gateway with basic rate limiting for demonstration stability
- S3 access restricted to Lambda services
- DynamoDB access limited to application components
- No sensitive data processing requirements

## Deployment Strategy

### Infrastructure
- AWS CDK for resource provisioning and demonstration setup
- Environment-specific configurations for hackathon presentation
- Automated deployment suitable for rapid prototype iteration

### Monitoring
- CloudWatch for basic Lambda monitoring during demonstration
- API Gateway logging for request tracking
- Simple error handling and performance metrics