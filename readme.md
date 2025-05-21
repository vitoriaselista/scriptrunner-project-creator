# Automation to Create and Configure Jira Projects

This Groovy script is designed to **automatically create Jira projects from issues**, and is meant to be used **as a listener**, triggered by an event of your choice or **as a post function** in a workflow transition.

## 🔧 What It Does

- Reads user input from an issue (template, project name, key...).
- Maps that template to predefined project configurations (schemes).
- Automatically creates the project via Jira REST API.
- Saves the created project as a link back to the original issue for tracking.

## 🛠️ Required Custom Fields to Use the Script

| Custom Field Name       | Field Type                                 | Purpose                                                                                                                                           |
| ----------------------- | ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Project Template**    | Select List (single choice)                | To let the user select the type of project to be created (`kanban` or `scrum`). The value is used to decide which configuration set will be used, assuming your instance have different standard schemes for kanban and scrum projects. |
| **Project Key Input**   | Short Text Field                           | To input the desired key (project abbreviation) for the new project. This value is passed to the API when creating the project.                   |
| **Created Project Key** | Project picker | To store the link of the project created by the script. Useful for tracking and validation later.                                                  |
