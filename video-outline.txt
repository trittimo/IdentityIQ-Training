# VIDEO 1: IAM INTRODUCTION
  * Walk through the components of IAM:
    * Identity: A person at the organization
    * Access: An ability they have in an application (introduce "entitlements")
    * Management: This can include:
      * Lookup (Audit trails)
      * Verification (Certs)
      * Automation (Provisioning/Approvals)
  * Name major related software
    * IAM tools - IIQ
    * Directories - AD
    * HR systems - Workday
# VIDEO 2: IIQ ARCHITECTURE
  * Contained in an app server (e.g. Tomcat)
  * Links to a backing DB (e.g. SQL Server, MySQL)
  * Web/console interfaces
  * Exercise
    * Install AD on a VM
    * Install IdentityIQ on a separate VM
    * No customization needed, just base installs
# VIDEO 3: THE IIQ MODEL
  * Applications containing schema attributes, some of which can generate entitlements
  * Identities containing accounts and ID attributes
  * The two "sides" of IIQ (Certs/Provisioning)
# VIDEO 4: APPLICATIONS, AGGREGATION, AND ENTITLEMENTS
  * Types of applications
    * Direct connectors
      * App-specific
      * Generalized (e.g. JDBC)
    * Delimited files (read-only)
  * Account aggregation
  * Group aggregation
  * Entitlements (ManagedAttributes)
    * Anatomy of an entitlement
      * Application
      * Attribute
      * Value
    * Corresponds to account attributes marked as "managed" and "entitlement"
  * Uncorrelated accounts
    * Misnomer - These are actually ID cubes, but they all have one account each
    * Bring up significance of authoritative sources
  * Exercise
    * Create delimited-file application on IIQ as an authoritative source
    * Load up a CSV with sample accounts, simulating an HR system
    * Aggregate accounts, see new ID cubes populated as a result
  
    * Load AD with sample accounts
    * Create AD application on IIQ
    * Aggregate accounts, these will all show up as "uncorrelated accounts" - We'll fix that in the next exercise
# VIDEO 5: PROGRAMMING IN IIQ
  * XML wrappers for everything
  * Rules/Scripts
  * Beanshell (Including Javadoc references)
  * SSD/SSF
  * SSB
    * Directory structure
    * Build script / Targets
  * Deployment accelerator
  * Exercise
    * Write a correlation rule for the AD application that correlates AD accounts to ID cubes
    * Observe the full XML content of the rule in the debug page
    * SSB is not needed for this, just do it through the webapp
    * Aggregate both your HR and AD apps, and refresh all identities. You should see the AD accounts correlate to your existing cubes.
# VIDEO 6: ROLES
  * Types of roles
    * Business
    * IT
    * Organizational
  * Role assignment
    * Manual
    * Automatic (through assignment rules)
  * Role-based provisioning
  * Exercise
    * Create an IT role that matches users who have two specific AD entitlements
    * Create a business role that requires this IT role
    * Observe that IDs that have these entitlements have this role under their "detected roles"
# VIDEO 7: CERTIFICATIONS

# VIDEO 8: TRIGGERS AND WORKFLOWS
  * Triggers/Lifecycle events
    * Types (Rule/attribute change/create/etc)
    * Maps to a workflow
  * Workflows
    * Comprised of steps, which can be scripts, rules, specific actions (built-in methods), or even other workflows
    * Given certain input arguments depending on what's calling the workflow
    * Workflow variables can persist values through steps
    * Transitions control the flow of the workflow
    * Show graphical version, then XML version
  * Exercise
    * Create a joiner trigger that triggers on every new ID cube
    * Have the corresponding workflow print the new user's name to the console
# VIDEO 9: CUSTOM FORMS AND ON-DEMAND WORKFLOWS
  * Describe the use case - Letting users request that something happen, then doing that in a workflow
  * Components of the setup
    * Quicklink to launch the workflow
    * Form as the entry point (or as a mid-step as well, like an approval)
    * Workflow to resolve the request
  * How a form appears in a workflow step
    * The "Approval" tag
    * Returning values to Workflow variables
  * The form object
    * Sections
    * Fields
    * Buttons (Typically "Cancel"/"Enter")
  * Exercise
    * Create a custom form with a field for a message
    * In a corresponding on-demand workflow, print that message to the console
# VIDEO 10: PROVISIONING
  * Different types of objects involved
    * AccountRequest
    * AttributeRequest
    * ProvisioningPlan
    * ProvisioningProject
    * ProvisioningPolicy
  * The provisioning process
    * Create a ProvisioningPlan with AccountRequests that can contain AttributeRequests
    * Compile the ProvisioningPlan into a ProvisioningProject (This could potentially invoke a ProvisioningPolicy)
    * Provision the ProvisioningProject, at which point IIQ will automatically:
      * Split the ProvisioningProject into its individual ProvisioningPlans, each containing a single AccountRequest
      * For each ProvisioningPlan, figure out how to provision the plan (direct connect/custom integration/manual provisioning request)
      * Execute the provisioning action
  * Exercise
    * Create a provisioning policy for created users for AD (Use the default attributes in the default policy)
    * Modify the workflow from video 8's exercise to provision a new AD account to new ID cubes.
    * Add some new lines to the HR csv to create new cubes, then watch it work