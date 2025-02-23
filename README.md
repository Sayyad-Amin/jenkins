### 1. What is Jenkins, and why is it used?
**Answer:**
Jenkins is an open-source automation server used for **Continuous Integration (CI) and Continuous Deployment (CD)**. It automates building, testing, and deploying applications, integrating with tools like Git, Docker, Kubernetes, and Terraform.

### 2. How does Jenkins work?
**Answer:**
Jenkins follows a **Master-Agent** architecture:
- **Master:** Schedules jobs, manages agents, and provides UI.
- **Agent:** Executes the build steps assigned by the master.

### 3. What are the key features of Jenkins?
**Answer:**
- **Pipeline as Code** using Jenkinsfile.
- **Integration with Git, Docker, Kubernetes, and Ansible.**
- **Extensive plugin support** (1,800+ plugins available).
- **Distributed builds** using multiple agents.
- **Automated testing and deployment.**

### 4. How do you install Jenkins?
**Answer:**
1. Install Java (`JDK 11` recommended).
2. Download Jenkins `.war` file and run:
   ```sh
   java -jar jenkins.war
   ```
3. Access Jenkins at `http://localhost:8080`.
4. For production:
   - Ubuntu: `sudo apt update && sudo apt install jenkins`
   - CentOS: `sudo yum install jenkins`

### 5. What is the difference between Freestyle and Pipeline jobs?
| Feature | Freestyle Job | Pipeline Job |
|---------|--------------|--------------|
| **Flexibility** | Simple UI-based | Script-based (Groovy) |
| **Reusability** | Less reusable | Highly reusable |
| **Version Control** | Not versioned | Stored as Jenkinsfile |
| **Complex Workflows** | Limited | Supports advanced logic |

### 6. How do you integrate Jenkins with GitHub?
**Answer:**
1. Install **Git Plugin** in Jenkins.
2. Add GitHub repository URL in job configuration.
3. Set up **GitHub Webhooks** to trigger builds automatically.

### 7. What is a Jenkinsfile, and why is it important?
**Answer:**
A **Jenkinsfile** is a text file that defines a Jenkins pipeline using **Declarative** or **Scripted** syntax. It enables **Pipeline as Code** and version control.

### 8. What is the difference between Declarative and Scripted Pipelines?
| Feature | Declarative Pipeline | Scripted Pipeline |
|---------|---------------------|------------------|
| **Syntax** | Simple, structured | Uses Groovy scripting |
| **Readability** | Easier to read | More complex |
| **Flexibility** | Limited | Highly flexible |

**Example Declarative Pipeline:**
```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
            }
        }
    }
}
```

### 9. How do you secure Jenkins?
**Answer:**
- Enable **authentication** and **role-based access control (RBAC)**.
- Disable **anonymous access**.
- Keep Jenkins **plugins updated**.
- Use **SSL and reverse proxy (Nginx/Apache)**.

### 10. How do you handle credentials securely in Jenkins?
**Answer:**
- Use **Jenkins Credentials Store**.
- Secure credentials using `withCredentials` block.
- Use **Vault (HashiCorp) or AWS Secrets Manager**.

Example:
```groovy
withCredentials([usernamePassword(credentialsId: 'my-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
    sh 'echo $USER'
}

### 11. How do you integrate Jenkins with Docker?
**Answer:**
- Install the **Docker Plugin**.
- Use the `docker` directive in a pipeline.

Example:
```groovy
pipeline {
    agent {
        docker { image 'python:3.8' }
    }
    stages {
        stage('Run') {
            steps {
                sh 'python --version'
            }
        }
    }
}
```

### 12. How do you integrate Jenkins with Kubernetes?
**Answer:**
- Install **Kubernetes Plugin**.
- Use Kubernetes agent in the pipeline.

Example:
```groovy
pipeline {
    agent {
        kubernetes {
            yaml '''
            apiVersion: v1
            kind: Pod
            spec:
              containers:
              - name: build
                image: maven:3.8.1
                command: ["cat"]
                tty: true
            '''
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
    }
}
```

### 13. How do you set up Jenkins for Terraform automation?
**Answer:**
- Install the **Terraform Plugin**.
- Use Terraform commands in a pipeline.

Example:
```groovy
pipeline {
    agent any
    stages {
        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }
        stage('Terraform Apply') {
            steps {
                sh 'terraform apply -auto-approve'
            }
        }
    }
}
```

### 14. How do you implement Blue-Green Deployment in Jenkins?
**Answer:**
1. Deploy the **new version (Blue)** alongside **current version (Green)**.
2. Run **tests** on Blue.
3. If successful, switch traffic to Blue and remove Green.

Example:
```groovy
pipeline {
    agent any
    stages {
        stage('Deploy Blue') {
            steps {
                sh 'kubectl apply -f blue-deployment.yaml'
            }
        }
        stage('Switch Traffic') {
            steps {
                sh 'kubectl set service my-service --selector app=blue'
            }
        }
        stage('Cleanup Green') {
            steps {
                sh 'kubectl delete -f green-deployment.yaml'
            }
        }
    }
}
```

