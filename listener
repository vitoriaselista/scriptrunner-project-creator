import groovy.json.JsonOutput

// ===== STATIC CONFIGURATION =====
// Predefined configuration templates for different project types

def kanbanType = [
    issueTypeScheme: 0,            // Replace with your Issue Type Scheme ID
    issueTypeScreenScheme: 0,      // Replace with your Issue Type Screen Scheme ID
    notificationScheme: 0,         // Replace with your Notification Scheme ID
    permissionScheme: 0,           // Replace with your Permission Scheme ID
    workflowScheme: 0,             // Replace with your Workflow Scheme ID
]

def scrumType = [
    issueTypeScheme: 0,            // Replace with your Issue Type Scheme ID
    issueTypeScreenScheme: 0,      // Replace with your Issue Type Screen Scheme ID
    notificationScheme: 0,         // Replace with your Notification Scheme ID
    permissionScheme: 0,           // Replace with your Permission Scheme ID
    workflowScheme: 0,             // Replace with your Workflow Scheme ID
]

// ===== DYNAMIC INITIALIZATION =====
// Variable to store selected configuration
def data = null

// Retrieve the selected template from a custom select list field
def inputTemplate = issue.fields.customfield_0.value.toLowerCase()  // Replace with your custom field ID

// ===== TEMPLATE SWITCH =====
// Assign the correct project configuration based on the selected template
switch (inputTemplate) {
    case 'kanban':
        data = kanbanType
        break
    case 'scrum':
        data = scrumType
        break
}

// ===== ADD PROJECT METADATA =====
// Add dynamic project attributes to the data map
data['name'] = issue.fields.summary                            // Project name from issue summary
data['description'] = issue.fields.description                 // Project description from issue description
data['key'] = issue.fields.customfield_0                       // Replace with the custom field used for the project key
data['leadAccountId'] = issue.fields.assignee.accountId        // Project lead
data['projectTypeKey'] = 'software'                            // Type of project 

// ===== LOG SELECTED TEMPLATE AND PAYLOAD =====
logger.info('Selected template: ' + inputTemplate)
logger.info("Project creation payload: " + JsonOutput.toJson(data))

// ===== ATTEMPT TO CREATE PROJECT =====
while (true) {

    // Send POST request to create a new project
    def projectCreationResult = post('/rest/api/2/project')
        .header('Content-Type', 'application/json')
        // .body(JsonOutput.toJson(data)) // Optional: Convert to JSON string if required
        .body(data)
        .asString()

    // If project creation is successful (HTTP 201 Created)
    if (projectCreationResult.status == 201) {

        logger.info(projectCreationResult.toString())

        // Update current issue with the created project key
        put("/rest/api/2/issue/" + issue.key)
            .header('Content-Type', 'application/json')
            .body([
                fields: [
                    customfield_0: [key: data['key']]  // Replace with your custom field for saving project key
                ]
            ])
            .asString()

        logger.info('Breaking the loop')
        break

    } else {
        // If project key is already in use, append "X" and retry
        logger.info('Project key in use, generating new key = ' + data['key'] + "X")
        data["key"] = data['key'] + "X"
    }
}
