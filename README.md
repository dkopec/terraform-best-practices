# My Understanding of Terraform Best Practices
A on going collection of best practices for working with Terraform as collected and implemented by myself.

Feel free to keep a copy of these documents in your own project

By following these best practices, you can enhance the readability, maintainability, and consistency of your Terraform codebase, making it easier for both you and your team to manage infrastructure as code.

## References
* [Hashicorp - Recommended Practices](https://developer.hashicorp.com/terraform/cloud-docs/recommended-practices)
* [Google Cloud - Best Practices for Terraform](https://cloud.google.com/docs/terraform/best-practices-for-terraform)
* [Anton Babenko - Terraform Best Practices](https://github.com/antonbabenko/terraform-best-practices)

## Universal / Provider Agnostic Practices


## Workflow and Automation:

- **Automate Deployments:**
  - Implement automated workflows for deploying infrastructure changes across environments. This could include using CI/CD pipelines to apply Terraform configurations automatically.

- **Manual Approval Process:**
  - Incorporate manual approval steps in your deployment process for critical environments like production. This adds an extra layer of control and helps prevent unintentional changes.

- **Tools**
* [tflint](https://github.com/terraform-linters/tflint)
* [tfsec](https://github.com/aquasecurity/tfsec)
* [terraform-docs](https://terraform-docs.io/)
* [git](https://git-scm.com/)

- **Development Process**

1. Create new git branch from main.
1. Make code changes
1. Run `terraform fmt`
1. Run `tflint` and fix anything listed
1. Run `tfsec` and fix anything listed
1. Run `terraform-docs markdown table --output-file README.md --output-mode inject --sort-by required .`
1. Make git commit.
1. Create Pull Request to main and have code reviewed by someone.
1. Make sure the Pipeline Checks run correctly.
1. Merge code into main after approval and delete branch.
1. Make sure the at the deployment pipeline runs correctly.
1. Checkout main locally, `git fetch origin --prune` to sync and clean, `git branch -D <branchName>`

### Documentation:

- **Environment Documentation:**
  - Maintain documentation that clearly outlines the specific configurations and considerations for each environment. This includes variables, state file locations, and any environment-specific nuances.

- **Change Management:**
  - Document and follow a change management process to track and communicate changes across environments. This helps in understanding the history of changes and their impact.

* [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/)

### Locals Best Practices:

1. **Use Locals for Repeated Expressions:**
   - Locals are useful for defining repeated expressions to avoid duplication and make the code more readable.

2. **Use Descriptive Names:**
   - Give descriptive names to locals that reflect their purpose. This helps in understanding the logic and intent behind the expressions.

3. **Avoid Overuse:**
   - While locals can improve code readability, avoid creating too many locals for every small expression. Use them judiciously for complex or repeated logic.

4. **Keep Locals Close to Their Usage:**
   - Define locals close to where they are used. This enhances code organization and makes it easier for others to understand the relationships between expressions and resources.

### Resource Names Best Practices:

1. **Be Descriptive:**
   - Use descriptive names for resources that convey their purpose. This is crucial for anyone reviewing the code to quickly understand the role of each resource.

2. **Consistent Naming Conventions:**
   - Adopt consistent naming conventions across your infrastructure code. This includes naming conventions for resources, variables, and other elements. Consistency helps in maintaining a standardized and predictable codebase.

3. **Avoid Hardcoding Names:**
   - Avoid hardcoding resource names whenever possible. Use variables and dynamic expressions to allow for flexibility and reusability.

4. **Include Environment or Purpose in Names:**
   - Include information about the environment (e.g., dev, prod) or the purpose of the resource in its name. This helps differentiate resources across different environments and clarifies their roles.

5. **Use Terraform Variables:**
   - Utilize Terraform variables to parameterize resource names. This enables you to manage and customize resource names based on specific requirements or environments.

6. **Consider Dependency Relationships:**
   - When naming resources, consider their dependency relationships. A resource's name might include information about its dependencies or connections to other resources.

7. **Document Naming Conventions:**
   - Document your naming conventions in a README or inline comments within the code. This ensures that team members are aware of the standards and can adhere to them consistently.

8. **Avoid Special Characters and Spaces:**
   - Stick to alphanumeric characters and underscores in resource names to ensure compatibility with various providers and to avoid potential issues.

### Module Design Best Practices:

1. **Modularize for Reusability:**
   - Design modules to be reusable across different projects and environments. Create modules with clear interfaces and inputs, allowing them to be easily adapted to various use cases.

2. **Abstract Complexity:**
   - Use modules to abstract complex configurations and resource definitions. This simplifies the main configuration files and makes them more maintainable.

3. **Clear Module Purpose:**
   - Ensure each module has a clear purpose and encapsulates related functionality. This improves code organization and makes it easier for users to understand the module's role.

4. **Parameterize Inputs:**
   - Parameterize module inputs using variables. This allows users to customize the behavior of the module without modifying its source code.

5. **Output Relevant Information:**
   - Define outputs in modules to provide relevant information to the calling configuration. Outputs can be used for chaining modules together or providing information to higher-level configurations.

6. **Version Modules:**
   - Use versioning for modules to ensure stability and prevent unintended changes from affecting multiple projects. This can be done using version control systems or Terraform Module Registry.

### Module Usage Best Practices:

1. **Consistent Naming Conventions:**
   - Adopt consistent naming conventions for module variables, making it easier for users to understand and provide inputs. Consistency in variable names enhances the usability of modules.

2. **Documentation:**
   - Document module usage, including expected inputs, outputs, and any specific considerations or constraints. Good documentation helps users understand how to leverage the module correctly.

3. **Environment-Agnostic Modules:**
   - Design modules to be environment-agnostic when possible. This allows users to reuse the same modules across different environments by providing appropriate input variables.

4. **Test Modules Independently:**
   - Test modules independently before integrating them into larger configurations. This ensures that modules work as expected and reduces the risk of issues when used in different contexts.

5. **Use Conditional Logic Sparingly:**
   - Minimize the use of complex conditional logic within modules. While some conditionals may be necessary, excessive complexity can make modules harder to understand and maintain.

6. **Version Pinning:**
   - Pin module versions explicitly in the calling configuration. This helps maintain reproducibility and prevents unexpected changes when updating modules.

7. **Consider Remote Module Sources:**
   - Host modules in a version-controlled repository or use Terraform Module Registry. This allows for better collaboration, versioning, and sharing of modules across teams.

8. **Understand Module Dependencies:**
   - Be aware of any dependencies that modules may have. Modules should be designed to be independent where possible, but dependencies should be documented and understood by users.

## Directory Structure:

- **Separate Environments:**
  - Organize your Terraform configurations by separating environments into distinct directories. This helps avoid confusion and keeps configurations for different environments isolated.

- **Use a Root Module for Each Environment:**
  - Create a root module for each environment, containing the main configuration files and possibly shared modules. This provides a clear structure and makes it easier to manage configurations for each environment.

- **Use Shared Modules:**
  - Factor out common configurations into shared modules that can be reused across environments. This promotes code reuse and ensures consistency.

## Variable Management:

- **Parameterize Configuration:**
  - Parameterize configurations using variables to make them adaptable to different environments. Use variables for things like resource names, sizes, and other environment-specific settings.

- **Environment-Specific Variable Files:**
  - Use separate variable files for each environment to store environment-specific values. This allows for easy customization without modifying the main configuration files.

- **Leverage Terraform Workspaces:**
  - Use Terraform workspaces to manage environment-specific state files and variables. This can simplify the management of multiple environments within a single Terraform configuration.

## State Management:

- **Separate State Files:**
  - Keep separate state files for each environment. This ensures that changes in one environment do not impact others, and it facilitates a cleaner approach to managing infrastructure state.

- **Remote State Storage:**
  - Store Terraform state remotely using a backend like AWS S3 or Azure Storage. This provides a centralized and secure location for storing state files, making it easier to collaborate and manage state.

## Environment-Specific Configurations:

- **Conditional Logic:**
  - Use conditional logic within your configurations to handle environment-specific settings. For example, use `count` or `for_each` with conditional statements based on the environment.

- **Environment Tagging:**
  - Tag resources with environment-specific labels to easily identify and manage resources belonging to different environments. This is particularly useful for cost tracking and resource management.
