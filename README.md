# n8n-sprint2-issue-triage
Week 6 public test repository for an n8n GitHub Issue Triage workflow.

## Public repo
https://github.com/goulding-pfw/n8n-sprint2-issue-triage

## Purpose
This repository exists solely as a test bed for an n8n workflow that utilizes a GitHub webhook to receive all issues, normalize them, and insert/update them in a database.

## Services
Services used are GitHub CLI webhook forwarding, and a docker containerized local instance of n8n.

## Node sequence
Node sequence progresses as follows: Github issue webhook -> Opened action? (if true) -> Normalize issue (field mapping - see below) -> Classify title (4 branching switch node) -> Setting routes (four additional field mapping nodes based on switch node outcome) -> Check if issue already exists in database -> (if exists) Update record - (if not exists) insert record -> (failure check nodes) -> Update database tables with recorded status and log path (back to back datatable nodes)

## Field mapping

### Process GitHub Data
| Field | GitHub Data |
| -------- | ------- |
| repository_full_name | {{ $json.body.repository.full_name }} |
| issue_number | {{ $json.body.issue.number }} |
| issue_key | {{ $json.body.repository.full_name + "#" + $json.body.issue.number }} |
| issue_url | {{ $json.body.issue.html_url }} |
| title | {{ $json.body.issue.title }} |
| title_lower | {{ $json.body.issue.title.toLowerCase() }} |
| reporter | {{ $json.body.issue.user.login }} |
| opened_at | {{ $json.body.issue.created_at }} |
| last_seen_at | {{ $now.toISO() }} |

### Additional fields based on Switch node
| Field | Value Options |
| ------- | ------- |
| category | bug, feature, help, other |
| route_name | bug-triage, feature-requests, support, general-triage |

### Ledger Table Mapping
There are two values we set by hand here.  All new records start at a pending status with a blank error note.

| Table Column | Value |
| ------- | ------- |
| issue_key | {{ $json.issue_key }} |
| issue_url | {{ $json.issue_url }} |
| title | {{ $json.title }} |
| category | {{ $json.category }} |
| reporter | {{ $json.reporter }} |
| opened_at | {{ $json.opened_at }} |
| last_seen_at | {{ $json.last_seen_at }} |
| route_log_status | "pending" |
| route_name | {{ $json.route_name }} |
| error_note | "" |

### Route Log Table Mapping
| Table Column | Value |
| ------- | ------- |
| issue_key | {{ $json.issue_key }} |
| category | {{ $json.category }} |
| route_name | {{ $json.route_name }} |
| recorded_at | {{ $json.last_seen_at }} |

## Classification rules
For the switch node, there are three classification rules with one fallback output, which creates four outputs from the node.  The classification rules are as follows:
* If title_lower contains 'bug' or 'error', route it to the bug category and bug-triage route.
* If title_lower contains 'feature' or 'request', route it to the feature category and feature-requests route.
* If title_lower contains 'help' or 'question', route it to the help category and support route.
* All other incoming issues are routed to the other category and general-triage route.

## Setup notes
The only real notes I have to provide are to be mindful of the regex, as n8n only accepts the following: "\b(bug|error)\b" rather than what was assumed would work: "\\b(bug|error)\\b".

Otherwise, the setup should be fairly straightforward.  Connect with the GitHub repos' webhook forwarding function, retrieve it within n8n, and setup the workflow to receive, check current records, normalize the data, classify the issues, add additional data based on classification, and then either update or insert into the relevant datatables (all of which utilize default String or Text datatypes).

## Evidence screenshots
The following are screenshots from n8n, showing 5 test executions and the results in the tables.

### Test 1 - A Bug Issue is Created
<img width="1736" height="901" alt="Test1_Execution" src="https://github.com/user-attachments/assets/0ae5149e-82fe-4d07-8a5e-1b1ee98747e5" />
<img width="1750" height="204" alt="Test1_Ledger_RecordedStatus" src="https://github.com/user-attachments/assets/613997c6-bb53-489a-99d6-5a0ad8775b52" />
<img width="1742" height="185" alt="Test1_RouteLog" src="https://github.com/user-attachments/assets/b9e94d37-172d-4917-96a7-9ebd8ab126dd" />

### Test 2 - A Feature Request Issue is created
<img width="1728" height="909" alt="Test2_Execution" src="https://github.com/user-attachments/assets/96ae3985-c11f-4fce-8893-c878b80dfaca" />
<img width="1749" height="222" alt="Test2_Ledger_RecordedStatus" src="https://github.com/user-attachments/assets/e296f871-4a07-47bf-a7a4-4bc0e1c921c3" />
<img width="1749" height="213" alt="Test2_RouteLog" src="https://github.com/user-attachments/assets/e70c62e1-3d92-41bd-921b-e00751fd242e" />

### Test 3 - A Help Issue is created
<img width="1734" height="880" alt="Test3_Execution" src="https://github.com/user-attachments/assets/6423d81a-7042-49d1-bb15-062b9eb30719" />
<img width="1754" height="252" alt="Test3_Ledger_RecordedStatus" src="https://github.com/user-attachments/assets/c3843585-78b7-4b2b-b57e-f735316fe770" />
<img width="1744" height="253" alt="Test3_RouteLog" src="https://github.com/user-attachments/assets/32ff9f58-7fa2-4cdd-8b3e-3d7b232810a0" />

### Test 4 - A Non-Classifying Issue is created
<img width="1731" height="895" alt="Test4_Execution" src="https://github.com/user-attachments/assets/5c9ceb1b-fabc-4c7a-a789-1c37a2d4926f" />
<img width="1752" height="291" alt="Test4_Ledger_RecordedStatus" src="https://github.com/user-attachments/assets/e4dca809-b114-479e-b57d-77fd8f72c78e" />
<img width="1740" height="273" alt="Test4_RouteLog" src="https://github.com/user-attachments/assets/64771b92-87d4-4568-8f47-7fb7d3fc9cf0" />

### Test 5 - Testing an Intentional Failure Route
<img width="1732" height="917" alt="Test5_Execution" src="https://github.com/user-attachments/assets/65052a80-d0c3-4265-83e8-d5f430ad1583" />
<img width="1758" height="324" alt="Test5_Ledger_FailedStatus" src="https://github.com/user-attachments/assets/4086c965-6518-494f-b9c2-d20cda76c832" />
