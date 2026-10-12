# Runbook

## Purpose
This prototype's purpose is to retrieve issues from GitHub's webhook forwarding feature, process the data, and store it in a database.
Evidence for this can be seen in both the README.md file and the test_evidence.md file within this repo. Look for Tests 1 through 4.

## Normal Operation
Under normal operation, any created issues will be received, be normalized, be classified, and then will be inserted or updated within the database.
Evidence for this can be seen in both the README.md file and the test_evidence.md file within this repo. Look for Tests 1 through 4.

## Duplicate Delivery
In case of a duplicate delivery, a second issue should not be processed and inserted into the database. A test attempt was made for this scenario by forcing a redelivery within GitHub's webhook settings within this repo. A 401 error occurred preventing the data from ever reaching n8n for processing. This was repeatable and is currently considered a feature of either GitHub's webhook forwarding system or n8n's webhook node, acting as a gate to prevent duplicate deliveries.

## Route-Log Failure
In case of a failed route-log write, the route_log status should be updated as "failed" within the ledger table, and no route-log record should be created.
Evidence for this can be seen in both the README.md file and the test_evidence.md file within this repo.  Look for Test 5.

## Recovery Action
Within this prototype, there are few instances where things can go wrong in a catastrophic way. In most instances, if an issue fails to be processed, the user should first check what data made it into the database. If there are records, but data is corrupted or missing, view the execution history of the workflow and work through the execution, monitoring inputs and outputs to find the problematic data or node. If there are no records, view where the execution failed. If the failure is due to the Opened action? node, check that there is not a duplicate issue within the database. If there was no execution, check that GitHub's webhook forwarding is functioning, and that there is a delivery attempt within the Webhook delivery history within GitHub. Finally, ensure the Docker container is running and healthy.

## Safe Shutdown
For a safe shutdown, perform the following steps:
1. Unpublish the n8n workflow.
2. Close or terminate the webhook forwarding within the command line by hitting Ctrl+C.
3. Ensure the webhook is no longer present within the GitHub repo.
4. For this localhosted container, ensure everything within n8n is saved, and then enter "docker compose stop" within the terminal running the services.
5. Close Docker.

## What this prototype does not test
This prototype does not test durable error handling, or the creation of a stack-trace for robust error handling. This prototype also lacks a properly designed relational database for the storage of these issues, as well as the means to handle assignments, issue status updates/changes, or any other required functions expected in a ticket tracking tool. And while this workflow functions really well, it really needs a better way to present the data stored within the database to users, which will likely require the creation of an internal tool dashboard through another software, with n8n utilizing additional workflow automation to send the contents of its database as a .csv or other format for processing into a larger database.
