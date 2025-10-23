# Multi_Minds_AI — Intelligent Multi-Agent System with CI/CD Pipeline

An advanced multi-agent AI application that orchestrates multiple LLM providers (Groq API, Tavily API) through a sophisticated backend system, featuring automated CI/CD deployment through Jenkins, Docker containerization, and AWS Fargate hosting — combining intelligent agent coordination, modern DevOps practices, and cloud-native deployment into one complete solution.

## Table of Contents
- [Features](#features)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Project Structure](#project-structure)
- [CI/CD Pipeline Setup](#cicd-pipeline-setup)
- [Technologies Used](#technologies-used)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)
- [License](#license)
- [Contact](#contact)
- [Future Enhancements](#future-enhancements)
- [Acknowledgments](#acknowledgments)

## Features

- **Multi-Agent Architecture**: Orchestrates multiple AI agents with specialized capabilities  
- **FastAPI Backend**: High-performance asynchronous API handling  
- **Streamlit Frontend**: Interactive and user-friendly interface  
- **LangChain Integration**: Core framework for agent coordination and logic  
- **Security-First**: SonarQube code quality scanning and security analysis  
- **Cloud-Native**: Deployed on AWS Fargate for serverless container management  
- **Containerized**: Docker-based deployment for consistency  
- **Automated CI/CD**: Jenkins pipeline with GitHub webhook integration  
- **Comprehensive Logging**: Built-in logging and exception handling  
- **Environment-Based Configuration**: Flexible model and variable selection

## Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                        PROJECT SETUP                                │
│  ┌──────────┐  ┌──────────────┐  ┌─────────────────┐                │
│  │ Groq API │  │  Tavily API  │  │    Virtual      │                │
│  │   Keys   │  │              │  │  Environment    │                │
│  └──────────┘  └──────────────┘  └─────────────────┘                │
│  ┌──────────┐  ┌──────────────┐  ┌─────────────────┐                │
│  │ Logging  │  │    Custom    │  │    Project      │                │
│  │          │  │  Exceptions  │  │   Structure     │                │
│  └──────────┘  └──────────────┘  └─────────────────┘                │
└─────────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────────┐
│                     MAIN APPLICATION                                │
│  ┌──────────────────────────────────────────────────────────────┐   │
│  │                    Configuration Layer                       │   │
│  │    (Environment Variables & Model Selection)                 │   │
│  └──────────────────────────────────────────────────────────────┘   │
│                              ↓                                      │
│  ┌─────────────┐         ┌─────────────┐                            │
│  │   FastAPI   │ ←────→  │   Backend   │                            │
│  │  (API Layer)│         │   (Core     │                            │
│  │             │         │  LangChain  │                            │
│  └─────────────┘         │  & Langraph)│                            │
│         ↕                └─────────────┘                            │
│  ┌─────────────┐                 ↓                                  │
│  │  Streamlit  │         ┌─────────────┐                            │
│  │  (Frontend) │ ←────→  │   Frontend  │                            │
│  │             │         │     UI      │                            │
│  └─────────────┘         └─────────────┘                            │
└─────────────────────────────────────────────────────────────────────┘
                              ↓
┌─────────────────────────────────────────────────────────────────────┐
│                      CI/CD PIPELINE                                 │
│                                                                     │
│  GitHub (SCM) → Jenkins → Docker Build → SonarQube Scan             │
│       ↓            ↓           ↓              ↓                     │
│  Webhook     Automated    Container     Code Quality               │
│  Trigger     Testing      Creation      Analysis                   │
│                                ↓                                    │
│                    Build & Push to ECR                              │
│                                ↓                                    │
│                      Deploy to AWS Fargate                          │
│                         (Production)                                │
└─────────────────────────────────────────────────────────────────────┘
```

## Prerequisites

### Required Software
- Python 3.8+
- Git
- Docker Desktop
- AWS Account

### Required Accounts & Credentials
- GitHub (for repository + Jenkins integration)
- AWS IAM user with ECR and Fargate access
- Groq API Key
- Tavily API Key
- SonarQube Server (for code quality analysis)

### Local Development
- Python virtual environment
- Code editor (VS Code recommended)

## Installation

### Step 1: Clone the Repository
```bash
git clone https://github.com/meenatchisundari/Multi_Minds_AI.git
cd Multi_Minds_AI
```

### Step 2: Create Virtual Environment

**Windows:**
```bash
python -m venv venv
venv\Scripts\activate
```

**macOS/Linux:**
```bash
python3 -m venv venv
source venv/bin/activate
```

### Step 3: Install Dependencies
```bash
pip install -r requirements.txt
```

### Step 4: Set Up Environment Variables
Create a `.env` file in the root directory:
```env
GROQ_API_KEY=your_groq_api_key
TAVILY_API_KEY=your_tavily_api_key
```

### Step 5: Run the Application

**Start Backend:**
```bash
uvicorn backend.main:app --reload
```

**Start Frontend:**
```bash
streamlit run frontend/app.py
```

## Project Structure

```
Multi_Minds_AI/
│
├── backend/           # FastAPI backend with LangChain core logic
│   ├── main.py
│   └── agents/       # Multi-agent orchestration
│
├── frontend/         # Streamlit UI
│   └── app.py
│
├── core/            # Core LangChain & Langraph logic
│   └── agents.py
│
├── config/          # Configuration management
│   └── settings.py
│
├── logs/            # Application logs
│
├── exceptions/      # Custom exception handlers
│
├── Dockerfile       # Container configuration
├── Jenkinsfile      # CI/CD pipeline stages
├── requirements.txt # Python dependencies
├── .gitignore       # Ignored files
└── README.md        # This file
```

## CI/CD Pipeline Setup

The Jenkins pipeline automates: **Code Checkout → Build → Code Quality Scan → Push to ECR → Deploy to AWS Fargate**

### 1. Jenkins Setup
- Build Jenkins Docker image with Docker-in-Docker (DIND)
- Install Python, Docker, and SonarQube Scanner
- Configure global credentials for GitHub, AWS, Docker, and SonarQube

### 2. GitHub Integration
- Generate Personal Access Token
- Add to Jenkins credentials as `github-token`
- Configure webhook for automatic pipeline triggering

### 3. SonarQube Code Quality Scanning
Jenkins performs code quality analysis:
```bash
sonar-scanner \
  -Dsonar.projectKey=multi-minds-ai \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://your-sonarqube-server \
  -Dsonar.login=your-sonarqube-token
```

### 4. Docker Build & Push
```bash
docker build -t multi-minds-ai:latest .
docker tag multi-minds-ai:latest <ECR_REPO_URI>:latest
docker push <ECR_REPO_URI>:latest
```

### 5. AWS Fargate Deployment
Jenkins triggers AWS Fargate to deploy the latest ECR image with automatic scaling and load balancing.

### 6. Webhook Configuration
Configure GitHub webhook to automatically trigger Jenkins pipeline on code push:
- **Payload URL**: `http://your-jenkins-url/github-webhook/`
- **Content type**: `application/json`
- **Events**: Push events

## Technologies Used

| Category | Tools |
|----------|-------|
| **AI & LLM** | LangChain • Langraph • Groq API • Tavily API |
| **Backend** | FastAPI • Python 3.8+ |
| **Frontend** | Streamlit |
| **DevOps** | Jenkins • Docker • SonarQube • AWS ECR • AWS Fargate |
| **Cloud** | AWS (ECR, Fargate, Load Balancer) |
| **Monitoring** | CloudWatch • Application Logs |
| **Version Control** | Git • GitHub |

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Jenkins not loading | Ensure Docker is running and port 8080 is available |
| SonarQube scan fails | Verify SonarQube server URL and authentication token |
| ECR push denied | Check IAM user policy: `AmazonEC2ContainerRegistryFullAccess` |
| Fargate deploy fails | Verify task definition, container port (8000 for FastAPI), and environment variables |
| API keys not working | Ensure `.env` file is properly configured and loaded |
| Frontend can't connect to backend | Check FastAPI is running on correct port and CORS is configured |

## Contributing

Contributions are welcome! To add features:

1. Fork the repository
2. Create your feature branch:
```bash
git checkout -b feature/amazing-feature
```
3. Commit your changes:
```bash
git commit -m "Add amazing feature"
```
4. Push to the branch:
```bash
git push origin feature/amazing-feature
```
5. Open a Pull Request

### Guidelines:
- Follow PEP 8 style guide
- Add unit tests for new features
- Ensure SonarQube quality gate passes
- Update documentation as needed

## License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.

## Contact

**Meenatchi Sundari**  
 LinkedIn: [Your LinkedIn Profile](https://linkedin.com/in/meenatchisundari)  

## Future Enhancements

- [ ] Implement multi-model comparison dashboard
- [ ] Add support for additional LLM providers (OpenAI, Anthropic)
- [ ] Integrate vector database for RAG capabilities
- [ ] Add authentication and user management
- [ ] Implement conversation history persistence
- [ ] Create comprehensive API documentation with Swagger
- [ ] Add unit and integration tests with pytest
- [ ] Implement Kubernetes deployment option
- [ ] Add Prometheus + Grafana monitoring
- [ ] Create agent performance analytics dashboard

## Acknowledgments

- **LangChain** for the powerful agent framework
- **FastAPI** for the modern, fast web framework
- **Streamlit** for the intuitive frontend framework
- **Jenkins** for robust CI/CD automation
- **AWS** for reliable cloud infrastructure
- **SonarQube** for code quality analysis
- **Open-Source Contributors** for making this project possible

---

 **If you found this project useful, give it a star!** 

---
