### HumanGov: Deployment of HumanGov SaaS Application on AWS Elastic Kubernetes Service (EKS) Using a Route 53 Domain, ALB Ingress, and SSL Endpoint Powered by AWS Certificate Manager.
<p align="center">
<img src="https://i.imgur.com/E49oRFy.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

## Project Description
The "Multistate SaaS Application Deployment" project provides a comprehensive hands-on experience in deploying and managing a multi-state Software as a Service (SaaS) application. Leveraging Kubernetes for container orchestration and AWS services like Elastic Kubernetes Service (EKS), Route 53, DynamoDB, and S3, participants gain practical insights into deploying scalable, isolated, and state-specific applications.

### Project Objectives:
1. **Deploy a Multi-Tenant Application:**
   * Create a scalable and secure environment for deploying the HumanGov application for multiple states.

2. **Utilize AWS Services:**
   * Implement EKS, ALB, Route 53, and other AWS services for efficient traffic management and application performance.

3. **Enable SSL Encryption:**
   * Use AWS Certificate Manager to secure application traffic.

4. **Test Application Functionality:**
   * Ensure the application works correctly for adding and managing employee records across different deployments.

## Tools and Technologies
* **Amazon Elastic Kubernetes Service (EKS):** For orchestrating and managing containerized applications.
* **Amazon Route 53:** For DNS management and routing traffic to the Application Load Balancer.
* **Amazon Application Load Balancer (ALB):** To distribute incoming traffic and manage SSL termination.
* **AWS Certificate Manager:** For provisioning and managing SSL certificates.
* **Docker:** For containerizing the HumanGov application.
* **Terraform:** For infrastructure as code (IaC) to provision and manage AWS resources.
* **Amazon DynamoDB:** For storing employee records in a scalable NoSQL database.
* **Amazon S3:** For storing application assets and backups.
* **Kubernetes:** For managing deployment and scaling of containerized applications.

## PROJECT SOLUTION

### I. Application Deployment and Testing:
1. **Florida State Deployment:**
   * Expanded Terraform infrastructure by adding variables for new states (e.g., Florida).
   * Executed Terraform to create a new DynamoDB table and S3 bucket for the Florida application.

2. **Kubernetes Deployment File for Florida:**
   * Duplicated the California deployment file, adjusting it for Florida.
   * Utilized a find-and-replace approach to tailor the file for the new state, updating the S3 bucket name accordingly.

3. **Kubernetes Deployment:**
   * Applied the Florida configuration using kubectl apply, deploying pods, services, and config maps.
   * Verified the deployment with kubectl get pods and kubectl get services.

4. **Ingress Update:**
   * Updated the Ingress file to include a new rule for the Florida domain, ensuring proper traffic routing.
   * Directed requests for "Florida.humangov.click" to the Florida service.

5. **DNS Registration:**
   * Registered a new Route 53 record for "Florida.humangov.click," pointing to the shared application load balancer.

6. **Verification:**
   * Confirmed successful deployments by accessing both "Florida.humangov.click" and "California.humangov.click."

7. **Testing the Florida Application:**
   * Demonstrated the independence of the Florida application, highlighting separate databases and S3 buckets.

### II. Environment Inspection:
1. **Kubernetes Cluster:**
   * Explored the Kubernetes cluster to understand its structure, checking pods, deployments, services, and Ingress.

2. **Application Load Balancer:**
   * Inspected the unified AWS Application Load Balancer, showcasing how Ingress manages multiple applications with a single load balancer.

3. **Route 53:**
   * AWS DNS service for domain registration and routing.

## Step-by-Step Directions

### 1. AWS CLI Installation:
* Confirm the installation of AWS CLI version 2 or later on your Cloud9 instance by running AWS --version.

<p align="center">
<img src="https://i.imgur.com/8nOodgC.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

* Upgrade the AWS CLI if necessary, following the steps provided in the documentation.

### 2. Install EKS CTL:
* Install the EKS CTL CLI tool by running the provided commands.
* Download the EKS CTL binary.
* Copy the binary to the bin directory using sudo cp.
* Verify the installation with eksctl version.

<p align="center">
<img src="https://i.imgur.com/oa3Ufyk.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 3. Install kubectl CLI:
* Install the kubectl CLI tool using the provided commands.
* Download kubectl.
* Grant execution permissions.
* Copy to the bin directory.
* Verify the installation with kubectl version.

<p align="center">
<img src="https://i.imgur.com/kdkZG40.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 4. Install Helm:
* Install Helm, the Kubernetes package management tool, using the provided commands.
* Download Helm using curl.
* Grant execution permissions.
* Install Helm.
* Verify the installation with helm version.

<p align="center">
<img src="https://i.imgur.com/IFxnRIl.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 5. IAM User Creation:
* Navigate to the AWS IAM console and create a new user named "EKS user" without console access.

<p align="center">
<img src="https://i.imgur.com/WQrG5db.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

* Attach the "AdministratorAccess" policy to the user (Note: In production, assign the least privilege required).

### 6. Access Key Creation:
* Inside the newly created IAM user, generate an access key under the "Security credentials" tab.
* Make note of the access key ID and secret access key.

<p align="center">
<img src="https://i.imgur.com/RlQEsut.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 7. AWS CLI Configuration:
* Disable AWS managed temporary credentials in Cloud9 settings.

<p align="center">
<img src="https://i.imgur.com/1Ujs3U3.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

* Export the IAM user's access key and secret access key as environment variables in the Cloud9 terminal.
* export AWS_ACCESS_KEY_ID=XXXXXXXXXXXXXXXX
* export AWS_SECRET_ACCESS_KEY=XXXXXXXXXXXXXXXXXXXXXXXXXXX

### 8. Terraform Infrastructure Provisioning the S3 & DynamoDB Table for HumanGov Application:
* Navigate to the HumanGov infrastructure folder and then to Terraform.

<p align="center">
<img src="https://i.imgur.com/f2TDV0k.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

* Run terraform show to verify no resources exist.

<p align="center">
<img src="https://i.imgur.com/AyLRmUz.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

* Execute terraform plan to view the resources Terraform will create.

<p align="center">
<img src="https://i.imgur.com/JN9o1Bu.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

* Deploy the infrastructure using terraform apply and make note of the S3 bucket and DynamoDB table names.

<p align="center">
<img src="https://i.imgur.com/iqXyjz5.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 9. EKS Cluster Creation:
* Run the EKS CTL command to create a Kubernetes cluster.
* Use the recommended instance size (t3.medium) for worker nodes.
* Deploy only one node to minimize charges.
* Wait for the cluster creation process to complete (approximately 5-15 minutes).

<p align="center">
<img src="https://i.imgur.com/SNlidlX.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 10. Verify Cluster and Connectivity:
* Confirm the Kubernetes cluster's creation by checking the AWS EKS console.

<p align="center">
<img src="https://i.imgur.com/nKWaxOr.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

* Update the local kubeconfig file using the following AWS EKS command.

<p align="center">
<img src="https://i.imgur.com/vr2M0cb.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

* Validate connectivity to the cluster with kubectl get svc and kubectl get nodes.

<p align="center">
<img src="https://i.imgur.com/wd5LsU8.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

## Part 2: Kubernetes Cluster Configuration

### 1. IAM Policy Creation:
* Download an IAM policy for the AWS Load Balancer Controller that allows it to make calls to AWS APIs on your behalf.

<p align="center">
<img src="https://i.imgur.com/aM7oTEe.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

* Create an IAM policy using the policy downloaded in the previous step.

<p align="center">
<img src="https://i.imgur.com/6BUT2Zs.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

* The IAM policy, stored in a JSON file, grants the necessary permissions for the AWS Load Balancer Controller to interact with AWS services.
* The policy includes permissions such as creating and managing load balancers. It is based on the EKS documentation.
* This IAM policy provides the required permissions for the AWS Load Balancer Controller to make API calls to AWS services on behalf of the Kubernetes cluster.

### 3. Create an IAM OIDC identity provider for your cluster with eksctl:
* OpenID Connect (OIDC) facilitates authentication between the Kubernetes cluster and AWS services.
* The eksctl utils associate-iam-oidc-provider command is used to create the OIDC identity provider for the cluster. This acts as a bridge between the Kubernetes cluster and AWS services.

<p align="center">
<img src="https://i.imgur.com/ZMWxFUn.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 4. IAM Role and Service Account Creation:
* An IAM role and Kubernetes service account named "AWS load balancer controller" are created. The service account is annotated with the IAM role's name.
* This step ensures that the Kubernetes cluster has the necessary permissions to communicate with AWS services, especially for the creation of an Elastic Load Balancer.

<p align="center">
<img src="https://i.imgur.com/KqacA2D.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 5. AWS Load Balancer Controller Installation Using [Helm V3]:
* Helm, a Kubernetes package manager, is utilized to install the AWS Load Balancer Controller on the Kubernetes cluster.
* Add the `eks-charts` repository.

<p align="center">
<img src="https://i.imgur.com/lcRaDfZ.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

* Update your local repo to make sure that you have the most recent charts.

<p align="center">
<img src="https://i.imgur.com/Mhife4u.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 6. Verification of Installation:
* The successful installation of the AWS Load Balancer Controller is confirmed by checking the deployment status using the kubectl get deployment command.

<p align="center">
<img src="https://i.imgur.com/4F7pX7n.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

* Verify that the controller is installed.

<p align="center">
<img src="https://i.imgur.com/8eFmN0P.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

## Part 3: Deploying the HumanGov Application

### 1. IAM Service Account for Application Pods:
* In this step, we create another IAM service account named "human-gov-pod-execution-role." This service account is assigned to pods within the Kubernetes cluster, enabling them to access DynamoDB and S3 buckets securely.

<p align="center">
<img src="https://i.imgur.com/Bs2gJRI.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 2. Containerization of HumanGov Application:
* Switch to the application directory

<p align="center">
<img src="https://i.imgur.com/H47Qoon.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

* An Amazon Elastic Container Registry (ECR) repository is created to host the Docker image of the HumanGov application.
* The Docker image is built, tagged, and pushed to the ECR repository using the AWS CLI and Docker commands.

<p align="center">
<img src="https://i.imgur.com/w8zl0wq.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 3. Deploy the Application for Each State:
* Create the file `humangov-california.yaml` under the `human-gov-application/src` directory.

<p align="center">
<img src="https://i.imgur.com/AEorvxa.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 4. Deployment Verification:
* The deployment is verified by checking the status of pods, services, and deployments using kubectl commands.

<p align="center">
<img src="https://i.imgur.com/MpmFazy.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

* Pod logs can be checked in case of deployment issues.

<p align="center">
<img src="https://i.imgur.com/acOnvfa.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

<p align="center">
<img src="https://i.imgur.com/TiQ41To.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

<p align="center">
<img src="https://i.imgur.com/v2Yb0XU.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

## Part 4: Registering your domain with Amazon Route 53

### 1. Navigate to the AWS Management Console:
* Search for Route 53 
* Click on Registered Domains and Select Domain of choice.

<p align="center">
<img src="https://i.imgur.com/aDnWXV1.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

* Confirm your email address associated with your domain to complete registration.

### 2. Review Hosted Zones:
* In the Route 53 dashboard, you will see the "Hosted zones" on the left-hand side. Click on "Hosted zones" to view existing hosted zones or create a new one.

<p align="center">
<img src="https://i.imgur.com/m4SU5h1.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 3. Access AWS Certificate Manager (ACM):
* In the AWS Management Console, locate and click on the "Services" dropdown in the top left corner. Under the "Security, Identity, & Compliance" section, select "Certificate Manager."

### 4. Request a Certificate:
* Click on the "Request a certificate" button

<p align="center">
<img src="https://i.imgur.com/lYlf5Z8.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 5. Enter Domain Information:
* In the "Add domain names" section, enter your domain name (e.g., yourdomain.com).

<p align="center">
<img src="https://i.imgur.com/DFIDwAb.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 6. Select Validation Method:
* Choose the validation method for your domain. AWS provides options like email validation or DNS validation. DNS validation is recommended for ease of automation.

### 7. Review and Confirm:
* Review your configuration and click "Confirm and request" to proceed.

### 8. Deploy the Ingress Rules:
* Create an Ingress YAML file (humangov-ingress-all.yaml) that includes rules for each state

<p align="center">
<img src="https://i.imgur.com/qFLwnYK.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 9. Saving Ingress Configuration:
* Modify the file to include rules for redirecting requests to the appropriate services for different states (e.g., California).
* Save the file to preserve the updated Ingress configuration.

### 10. Applying Ingress Configuration:
* Apply the updated Ingress configuration using the following command

<p align="center">
<img src="https://i.imgur.com/JAM1NGC.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

* Confirm that the Ingress resources have been successfully created.

<p align="center">
<img src="https://i.imgur.com/PduyiiH.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 11. Verification of Ingress and Load Balancer:
**Verification in AWS Console:**
* Access the AWS Management Console and navigate to the EC2 service.
* Locate the Load Balancers section to view the provisioning stage of the Application Load Balancer (ALB) created by the Ingress object.

<p align="center">
<img src="https://i.imgur.com/XpqCPk4.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 12. Update Route 53:
* Access the AWS Route 53 console.
* Add a new record for the California subdomain, pointing to the same Application Load Balancer as the California subdomain.

<p align="center">
<img src="https://i.imgur.com/rjVXH76.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

<p align="center">
<img src="https://i.imgur.com/vEuB5n2.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 13. Update Terraform to Add Florida to config file:
* Open Cloud9 text editor navigate to /human-gov-infrastructure/terraform/variables.tf add Florida 

<p align="center">
<img src="https://i.imgur.com/YjxZmrH.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 14. Update Terraform:
* Run Terraform Apply to create Dynamodb table, S3 bucket for Florida

<p align="center">
<img src="https://i.imgur.com/DRYmraA.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 15. Create Florida Kubernetes Deployment:
* Duplicate the existing Kubernetes deployment file for California and create a new one for Florida (e.g., human-gov-Florida.yaml).
* Replace all occurrences of "California" with "Florida" using the Cloud9 text editor.
* Change the S3 bucket name to the newly created Florida bucket.

<p align="center">
<img src="https://i.imgur.com/II0QYi3.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 16. Apply Florida Kubernetes Configuration:
* Use kubectl apply to deploy the Florida Kubernetes configuration (kubectl apply -f human-gov-Florida.yaml).

<p align="center">
<img src="https://i.imgur.com/YY0p9nc.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

* Verify that the pods, services, and config maps are created for the Florida deployment (kubectl get pods, kubectl get services)

<p align="center">
<img src="https://i.imgur.com/Xdnghrd.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

<p align="center">
<img src="https://i.imgur.com/mmGF9Pj.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 17. Ingress Update:
* Updated the Ingress file to include a new rule for the Florida domain, ensuring proper traffic routing.

<p align="center">
<img src="https://i.imgur.com/Y6Inakt.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

* Directed requests for "Florida.humangov.click" to the Florida service.

<p align="center">
<img src="https://i.imgur.com/VPaSsPd.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 18. DNS Registration:
* Registered a new Route 53 record for "Florida.humangov.click," pointing to the shared application load balancer.

<p align="center">
<img src="https://i.imgur.com/wTsx9d3.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 19. Verification:
* Confirmed successful deployments by accessing both "Florida.humangov.click" and "California.humangov.click."

<p align="center">
<img src="https://i.imgur.com/ud5wxIZ.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

<p align="center">
<img src="https://i.imgur.com/yDVmW75.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

### 20. Testing the Florida Application:
* Demonstrated the independence of the Florida application (https://)

<p align="center">
<img src="https://i.imgur.com/oNFndZj.png" height="80%" width="80%" alt="Pic"/>
<br />
<br />

## Project Conclusion
The deployment of the HumanGov application was successful, achieving the project's objectives of scalability, security, and multi-tenancy. The application is now capable of handling employee records for multiple states, with data isolation and SSL encryption in place.

### Challenges Encountered
* **Resource Management:** Managing multiple deployments within a single Kubernetes cluster required careful configuration to ensure proper resource allocation and traffic routing.
* **Configuration Errors:** Initial misconfigurations in the ingress rules led to traffic being routed incorrectly, necessitating multiple iterations to resolve.

### Lessons Learned
* **Thorough Testing:** It is crucial to thoroughly test each configuration change, especially in a multi-tenant architecture, to ensure that traffic is routed correctly and securely.
* **Documentation:** Keeping detailed documentation of each step in the deployment process helped streamline troubleshooting and configuration management.

### Future Improvements
* **Automated Deployment:** Implement CI/CD pipelines to automate the deployment process for future updates and additional state deployments.
* **Enhanced Monitoring:** Integrate more comprehensive monitoring and logging solutions, such as AWS CloudWatch and Prometheus, to better track application performance and health.
* **User Feedback Loop:** Establish a mechanism for gathering user feedback to continuously improve the application's features and user experience.
