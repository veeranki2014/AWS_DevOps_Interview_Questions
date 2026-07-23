# Questions
### 1. Can you give a quick introduction about yourself?
### 2. Do you have experience with landing zone or infrastructure level setup?
### 3. When you mention connectivity in terms of VNets, what exactly did you establish between accounts? Did you automate any account vending process or similar tasks?

### 4.
💬 **Question**: What exactly was your role in the landing zone process—specifically, did you handle new account creation (in a new or sub-OU), provisioning, and handoff to end users?

---

⭐️ **Answer**:

Yes, I was directly involved in the end-to-end process of landing zone automation, including new account provisioning and setup within the organizational structure (OU/sub-OU). Here’s how my role fit into this process:

- **Account Creation & Organizational Placement**:
  - I automated the creation of new accounts (or subscriptions, in Azure) within the appropriate Organizational Unit (OU) or sub-OU, depending on the business or workload requirements.
  - This included setting up the account structure to align with governance and compliance needs, ensuring each new account was properly nested for policy inheritance and management.

- **Automated Provisioning of Landing Zone Resources**:
  - Using Terraform and Azure DevOps (or Jenkins), I provisioned all foundational resources for each new account:
    - **Networking**: VNets, subnets, route tables, and connectivity (including VNet peering or hub-and-spoke models).
    - **Security**: IAM roles, policies, security groups, and baseline guardrails.
    - **Monitoring & Logging**: Integrated Azure Monitor, Log Analytics, and alerting as part of the baseline.
    - **Resource Policies**: Applied Azure Policies or AWS SCPs for compliance and operational consistency.

- **Account Vending Automation**:
  - I built reusable Terraform modules and CI/CD pipelines to automate the entire account vending process, ensuring that every new account was provisioned with the required resources, security, and connectivity from day one.
  - This reduced manual effort, improved consistency, and accelerated onboarding for end users or application teams.

- **Handoff & Support**:
  - Once the landing zone and account were provisioned, I handed over the environment to the end user or application team, providing documentation and support for onboarding.
  - I also provided ongoing support for troubleshooting, scaling, or enhancing the landing zone as requirements evolved.

**In summary:**  
My role covered the full lifecycle—from automating new account creation within the correct OU/sub-OU, to provisioning all necessary landing zone resources, to handing off a ready-to-use, secure, and compliant environment to end users. This ensured rapid, consistent, and governed cloud adoption across the organization.
