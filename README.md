This project, **Deploy-AKS-Cluster-In-Azure-With-Terraform-Custom-Modules**, automates the deployment of a production-ready Azure Kubernetes Service (AKS) environment using a modular Terraform approach. It integrates identity management, secure secret storage, and container orchestration into a single automated workflow.

---

**Project Overview**

The architecture follows a "Security First" approach by leveraging **Azure Entra ID (formerly Azure AD)** and **Azure Key Vault** to manage the lifecycle and credentials of the AKS cluster.

  Key Components

* **Infrastructure as Code (IaC):** Built with Terraform ().

* **Identity Management:** Creates a dedicated **Service Principal** within Entra ID to manage Azure resources.

* **Role-Based Access Control (RBAC):** Automatically assigns the **Contributor** role to the Service Principal at the subscription level.

* **Secret Management:** Stores the Service Principal's `client_id` and `client_secret` securely in **Azure Key Vault**.

* **Orchestration:** Deploys a fully functional **AKS Cluster** that utilizes the created Service Principal for its cloud provider integration.

---

**Architecture & Logic Flow**

The deployment follows a strict dependency graph to ensure resources are created in the correct order:

1. **Resource Group:** Initializes the logical container in a specified location (default: `canadacentral`).

2. **Service Principal:** Generates the identity required for the AKS cluster to interact with Azure APIs.

3. **Permissions:** Grants the Service Principal "Contributor" rights via `azurerm_role_assignment`.

4. **Key Vault:** Provisions a Key Vault and immediately stores the Service Principal credentials as secrets.

5. **AKS Cluster:** Deploys the Kubernetes cluster using the previously created identity.

6. **Kubeconfig:** Automatically generates a local `kubeconfig` file to allow immediate `kubectl` access to the cluster.



---

**File Structure**

* `provider.tf`: Configures the `azurerm` and `azuread` providers.

* `main.tf`: The root module that orchestrates the `ServicePrincipal`, `keyvault`, and `aks` custom modules .

* `variables.tf`: Defines input variables such as `rgname`, `location`, and `SUB_ID` for environment flexibility.

* `output.tf`: Exports critical information like the Resource Group name and Service Principal details (marked as sensitive) .

---

Getting Started Prerequisites

* Azure CLI installed and authenticated.
* Terraform CLI ().
* An active Azure Subscription ID.

  **Deployment**

1. **Initialize:** `terraform init` (Sets up the backend and downloads providers ).
2. **Plan:** `terraform plan` (Review the infrastructure changes).
3. **Apply:** `terraform apply` (Deploy the stack to Azure).


<img width="1024" height="550" alt="image" src="https://github.com/user-attachments/assets/040b1367-0875-442d-b787-b25294fd972c" />
