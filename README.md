GitLab CI/CD with Self-Hosted Runners on AWS EC2
📌 Project Overview

This project demonstrates how to build a custom CI/CD infrastructure using GitLab Self-Hosted Runners deployed on an AWS EC2 instance.

The objective was to create a scalable runner environment capable of executing multiple CI/CD jobs using different executors (Docker and Shell) while implementing custom runner tags for intelligent job scheduling.

This architecture allows organizations to control build environments, improve security, and efficiently run multiple CI/CD jobs on a single machine.

🏗 Architecture
                 Developer
                     │
                     │ Push Code
                     ▼
              GitLab Repository
                     │
                     │ Triggers
                     ▼
              GitLab CI/CD Pipeline
                     │
                     │ Job Scheduling
                     ▼
        ┌─────────────────────────────────┐
        │        AWS EC2 Instance         │
        │                                 │
        │   GitLab Runner Installed       │
        │                                 │
        │   ┌───────────────┐             │
        │   │ Docker Runner │             │
        │   │ Executor      │             │
        │   │ Tag: docker   │             │
        │   └───────────────┘             │
        │                                 │
        │   ┌───────────────┐             │
        │   │ Shell Runner  │             │
        │   │ Executor      │             │
        │   │ Tag: myshell  │             │
        │   └───────────────┘             │
        │                                 │
        └─────────────────────────────────┘
                     │
                     ▼
              Job Execution
                     │
                     ▼
                Pipeline Logs

                
⚙️ Technologies Used
Technology	Purpose
GitLab	CI/CD platform
GitLab Runner	Execute CI/CD jobs
AWS EC2	Self-hosted runner server
Docker	Containerized job execution
Linux	Runner host environment
Git	Version control
📂 Project Structure
.
├── .gitlab-ci.yml
├── README.md
🔧 Implementation Steps
1️⃣ Launch AWS EC2 Instance

Create an EC2 instance that will host the GitLab Runner.

Example configuration:

Instance Type: t2.micro / t3.micro
OS: Ubuntu / Amazon Linux
Storage: 8GB+
Security Group: Allow SSH (22)

Connect to the instance:

ssh ec2-user@your-ec2-ip
2️⃣ Install GitLab Runner

Download and install GitLab Runner.

curl -L --output gitlab-runner \
https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64

Make it executable:

chmod +x gitlab-runner

Move to system path:

sudo mv gitlab-runner /usr/local/bin/

Verify installation:

gitlab-runner --version
3️⃣ Install Docker (For Docker Executor)
sudo yum update -y
sudo yum install docker -y
sudo systemctl start docker
sudo systemctl enable docker

Verify:

docker --version
4️⃣ Create GitLab Runner in Project

Go to:

GitLab → Project → Settings → CI/CD → Runners

Click:

Create Project Runner

Copy the registration token.

5️⃣ Register Runner on EC2

Register the runner using:

gitlab-runner register

Enter:

GitLab URL:
https://gitlab.com

Token:
<Project Runner Token>

Description:
aws-runner

Executor:
docker

For Docker executor:

Default Docker Image:
python:3.11

Tags example:

docker
aws
6️⃣ Register Shell Runner

Register another runner using shell executor.

gitlab-runner register

Select:

Executor: shell

Tags example:

myshell
shell
7️⃣ Verify Runners

Check runners:

gitlab-runner list

Example output:

docker executor
shell executor
8️⃣ Start Runner Service

Run runner:

gitlab-runner run

Runner will start polling GitLab for jobs.

📜 CI/CD Pipeline Configuration

File:

.gitlab-ci.yml

Example pipeline configuration:

job1:
  script:
    - echo "CR7 job1 started.."
    - sleep 30
    - hostname
  tags:
    - aws

job2:
  script:
    - echo "job2 started.."
    - sleep 30
    - hostname
  tags:
    - myshell
🏷 Custom Runner Tags

Tags help GitLab decide which runner should execute the job.

Example tags used in this project:

docker
shell
aws
myshell
Tag Matching
Job Tag	Runner Tag	Executor
aws	aws	Docker
myshell	myshell	Shell
🔄 CI/CD Execution Flow

1️⃣ Developer pushes code to GitLab repository.

2️⃣ GitLab detects change and triggers pipeline.

3️⃣ Pipeline reads .gitlab-ci.yml.

4️⃣ GitLab scheduler searches for matching runner.

5️⃣ Runner with matching tag picks the job.

6️⃣ Job executes using configured executor.

7️⃣ Logs are sent back to GitLab.

8️⃣ Pipeline marked Passed / Failed.

📊 Runner Configuration Example

Inside runner config file:

/etc/gitlab-runner/config.toml

Example:

[[runners]]
  name = "docker-runner"
  executor = "docker"
  tags = ["docker", "aws"]

[[runners]]
  name = "shell-runner"
  executor = "shell"
  tags = ["myshell", "shell"]
🚀 Key Features Implemented

✔ Self-hosted GitLab Runner on AWS
✔ Multiple executors (Docker + Shell)
✔ Custom runner tags
✔ Multi-job execution
✔ CI/CD automation
✔ Pipeline monitoring

🧠 DevOps Concepts Demonstrated

Continuous Integration

Continuous Delivery

Self-hosted CI/CD infrastructure

Runner orchestration

Containerized job execution

Infrastructure on cloud

Pipeline automation

💡 Real World Use Cases

This architecture is commonly used for:

• Automated software builds
• Test automation
• Docker image building
• Infrastructure provisioning
• Security scanning pipelines
• Machine learning pipelines

📸 Project Screenshots

Example screenshots include:

GitLab Runner configuration

EC2 runner setup

CI/CD job execution

Pipeline logs

Runner status dashboard

📈 Future Improvements

Possible enhancements:

• Kubernetes runners
• Auto-scaling runners
• Docker-in-Docker builds
• GitLab container registry integration
• Multi-node runner cluster<img width="1919" height="946" alt="Screenshot 2026-03-10 230120" src="https://github.com/user-attachments/assets/e655ee87-d2ee-4d05-a2b4-7ff5f9de5c24" />
<img width="1919" height="933" alt="Screenshot 2026-03-10 230138" src="https://github.com/user-attachments/assets/2b7fc675-cb08-4aa3-975a-e4e881c2b4ad" />
<img width="1919" height="943" alt="Screenshot 2026-03-10 230208" src="https://github.com/user-attachments/assets/d7372b89-d980-4f81-8e62-be6d54157720" />
<img width="1919" height="941" alt="Screenshot 2026-03-10 230217" src="https://github.com/user-attachments/assets/136cebd8-8227-44e3-878e-1ca1590f79eb" />
<img width="1919" height="936" alt="Screenshot 2026-03-10 230259" src="https://github.com/user-attachments/assets/a58cd788-2bc0-4368-9240-6f469ae67d2d" />
<img width="1919" height="939" alt="Screenshot 2026-03-10 230312" src="https://github.com/user-attachments/assets/caf8520d-e5db-4ad9-98da-7bbc1caea2e0" />
<img width="1919" height="934" alt="Screenshot 2026-03-10 230332" src="https://github.com/user-attachments/assets/02b5c243-65fb-4dbb-b5d9-ee38d8d4ec85" />
<img width="1919" height="941" alt="Screenshot 2026-03-10 230349" src="https://github.com/user-attachments/assets/0ebdee35-2435-4982-a864-9c1065961112" />
<img width="1915" height="938" alt="Screenshot 2026-03-10 230749" src="https://github.com/user-attachments/assets/966924b6-9947-4780-aee1-b03126b0a739" />
<img width="1919" height="936" alt="Screenshot 2026-03-10 230801" src="https://github.com/user-attachments/assets/13917a92-558b-4609-b1f2-3164e99b6a6f" />
<img width="1917" height="941" alt="Screenshot 2026-03-10 230842" src="https://github.com/user-attachments/assets/41b85279-8b2d-46f1-8f85-c339fa9c9c52" />
<img width="916" height="904" alt="fig1arc-1" src="https://github.com/user-attachments/assets/7e8db08b-aefb-47d4-98e6-d667c25e9980" />
<img width="1241" height="675" alt="fig1" src="https://github.com/user-attachments/assets/c81224b3-32a2-475d-8dad-6ff75b1b6c32" />
