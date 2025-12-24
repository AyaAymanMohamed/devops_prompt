# Prompt: Docker Build & Push Pipeline



## **Role**  
Act as a **DevOps Engineer** with expertise in **CI/CD, Jenkins, Docker, and container registry management**, focusing on **secure and production-ready Docker image build and push workflows**.



## **Context**  
I am building a **Jenkins Declarative Pipeline** to build and push Docker images to DockerHub.  
The pipeline should:  
- Support **parameterized Docker image name and tag**.  
- Ensure **secure handling of DockerHub credentials**.  
- Include notifications on success or failure.  
- Follow **best practices** for retries, logging, and clean workspace.  



## **Task**  
Generate a **Jenkins Declarative Pipeline (Jenkinsfile)** with the following specifications.



### **Pipeline_Type**  
- Declarative  



### **Trigger**  
- Type: GitHub Webhook  
- Frequency: On push to target branch  



### **Source_Control**  
- Provider: GitHub  
- Repository_URL: `https://github.com/your-org/your-repo.git`  
- Credential_IDs: `dockerhub-credentials`  
- Shared_Library: No  



### **Parameters**  
- **docker_image**  
  - Type: string  
  - Default: `myapp`  
  - Description: Docker image name  

- **docker_tag**  
  - Type: string  
  - Default: `latest`  
  - Description: Docker image tag  



### **Agent**  
- Label: `docker-agent`  
- Requirements: Docker CLI installed, network access to DockerHub  



### **Tools**  
- Docker CLI  
- Optional: BuildKit enabled for faster builds  



### **Stages**  

1. **Checkout**
   - Pull application source code from GitHub.  
   - Ensure clean workspace for reproducible builds.  

2. **Docker Build**
   - Build Docker image using `Dockerfile`.   
   - Tag image using parameters (`docker_image` and `docker_tag`).  
   - Include retries for transient build failures.  
   - Log build output with timestamps.  

3. **Push**
   - Authenticate to DockerHub using credentials.  
   - Push the Docker image to DockerHub.  
   - Retry push on network errors.  
   - Logout after push to maintain security.  



### **Notifications**  
- Type: Slack  
- Channel: `#ci-cd`  
- Trigger: On Success  
- Content:  
  - Docker image name and tag  
  - Build Number  
  - Jenkins Job Name  
  - Build URL  



### **Post_Actions**
- Optional: Clean workspace to remove intermediate Docker images or build artifacts.  
- Optional: Trigger downstream deployments or tests.  



### **File_Structure**
- `Dockerfile`  
- `/app`  
- Jenkinsfile  



## **Style_Guide**
- **Syntax** : The pipeline must be written exclusively in Groovy within the declarative syntax. Use of scripted pipeline elements is strictly prohibited to maintain consistency and readability.
- **Readability**: Ensure clear, self-documenting stage and step names (e.g., BuildAndTagImage, AuthenticateAndPush). Use proper indentation (4 spaces) and whitespace to create a visually clean and easy-to-read structure.
- **Comments**: Include brief, high-level comments for each stage, explaining its purpose. Avoid over-commenting every line, as the code should be largely self-explanatory.
- **Reproducibility** : The pipeline should be designed to be completely reproducible. This means using cleanWs() to ensure a fresh build environment and referencing build parameters to tag images consistently.



## **Specific_Requirements**
- **Credential Management**: The pipeline must use a withCredentials block for authenticating with DockerHub. The dockerhub-credentials ID is mandatory. Credentials should be masked and never exposed in logs.
- **Error Handling & Robustness**: The Docker Build and Push stages must include a retry block to handle transient network issues or build failures. The pipeline should also have a timeout set to prevent runaway jobs.
- **Parameterized Operations** : The pipeline must dynamically reference the docker_image and docker_tag parameters to build and push the final image, making the pipeline highly flexible and reusable.



## **Output_Format**
- The final output is a complete, production-ready Jenkinsfile.
- The entire pipeline code must be enclosed within a single markdown code block (```groovy).
- No additional conversational text, descriptions, or explanations should be included outside of this single block.
- The code must be fully functional and ready to be copied and pasted into a Jenkins project.

## **Tone** 

- **Voice**: The voice is that of a knowledgeable DevOps Engineer delivering a robust, automated containerization solution.
- **Directness**: The language is clear and to the point, leaving no room for ambiguity.
- **Security-Focused**: The pipeline's structure and features (e.g., credential masking, cleanup, retries) should emphasize its security and reliability, demonstrating a commitment to building a trusted CI/CD workflow.
