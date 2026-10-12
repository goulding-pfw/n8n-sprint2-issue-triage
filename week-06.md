Student Name: JT Goulding 

Week: 7 

Date Range: 10/5 to 10/11 

List Course Work and/or Project: Sprint 2 Assignment 

1. Weekly Focus and Goals [1 point] 

State the current sprint, lab, or project objective. Then list up to three specific goals you planned to complete this week. 

The only goal this week is the Sprint 2 Assignment 
* Goal 1: Complete the n8n workflow 
* Goal 2: Test the webhook forwarding and gather evidence 
* Goal 3: Write the documentation for submission 

2. Completed Work and Evidence [3 points] 

For each completed item, state what you changed or built and attach or link the evidence. Evidence may be a screenshot, repository commit, Figma file, Baserow base, n8n workflow export, app link, test output, or another inspectable artifact. 

| Completed work | Evidence | Why it matters to this week’s objective |
| ------- | ------- | ------- |
| N8n Workflow | Submitted workflow json file as  a Sprint 2 deliverable | If this workflow is not created, the rest of the work can’t happen. |
| Test the webhook forwarding and workflow | Tests 1-5 documented within the test_evidence.md and README.md submissions for Sprint 2 | These tests are crucial for submission as a grade, as well as necessary for the Own vs Rent report. |
| Write the documentation for submission | All Sprint 2 deliverables | These documents are what are required for a grade. |

3. Work in Progress and Next Step [1 point] 

Name the work that is not complete yet. For each item, state the smallest next action you will take. 

	At time of writing, all work has been completed for this week. 

4. What You Learned [2 points] 

Describe the result of this week’s work. Include one decision, observation, or result that changed your understanding of the problem, user flow, data model, workflow, or platform. 

Then name the prototype dimension you tested most directly this week, role, look and feel, implementation, data shape, or process, and one important question your work did not answer. 

	This sprint had us familiarize ourselves with n8n and webhook forwarding. We created the workflows and tested the functionality, and it all was extremely quick and easy. It demonstrated to me the benefits of utilizing tools like n8n to quickly design automatic workflows that can handle a lot of work that would otherwise need to be coded by hand. Specifically, the ability to map incoming data to be ready to use and insert into database tables in a visual way. That was extremely easy and intuitive, especially coming from my background in having to parse all of that manually with test runs and printing the raw data from input to better understand the data objects being passed to my chunk of the program. The visual mapping is so much easier, although it is likely limiting in some ways based on what n8n allows and is able to handle. I have not figured out the limits of what n8n can do, and I believe that is a very important question to answer before I can properly endorse this in any business setting. 

5. Challenge and Diagnosis [2 points] 

Describe one obstacle honestly and specifically. 

	The primary challenge that occurred was resolving errors in my workflow design that prevented the tests from running successfully. The most annoying one was a failure on my part in handling the visual mapping, as I absentmindedly added an empty character in the front of the JSON for the value of one of the fields. This was causing errors in my workflow execution, and I could not figure out why due to n8n’s visual design making it difficult to tell that there was an empty character within the text box. I had to manually check every text field for any mistakes, which is how I found the empty character. This sort of thing is much easier to see in text editors that have a clear alignment and special characters to denote end-of-line. 

6. Response Plan [1 point] 

State what you anticipate to address next week. If you need instructor help, please send an email! 

	The primary goal for next week is fully completing my Own vs. Rent report! 

7. Extension Prompt 

Reflect on one 
capability you could add next without replacing the workflow’s core design 

The main thing I want to add is the ability to handle issues being assigned to people to work on.  It likely will just need an additional mapping that will create a new column for assignee.  When issues are updated, additional nodes and routing will need to be created to check that the assignee is properly updated to the record as well, but the core design as it currently stands should be able to remain the same. The nodes that will need changing are the Normalize node, as well as the Opened action? Node.  The Opened action? Node will especially need updating, as it currently only checks if an issue already exists.  in order for assignees to be updated, this node must allow for the data to pass through if some major values have changed between the record as it is coming from the webhook and the record as it exists within the database. 
