# Salesforce Bulk Data Remediation: Renewal Opportunity Cleanup

## Situation
A data integrity issue surfaced affecting ~700 renewal opportunities, which had 
been created with incorrect product line items due to an upstream automation 
flaw. The opportunities were live in the CRM and feeding into pipeline and 
forecasting reports, so the fix needed to preserve historical integrity while 
correcting the underlying data.

## Investigation
Used SOQL queries via direct Salesforce API access to identify the full scope 
of affected records, batching the analysis into two waves (264 and 385 record 
pairs respectively) to manage query limits and validation complexity. Each 
affected opportunity was paired with its correct line-item configuration, 
and a subset of records didn't cleanly match the expected pattern — flagged 
separately for manual review rather than risking an incorrect bulk fix.

## Resolution
Rather than attempting in-place updates (higher risk of partial failures at 
this volume), generated paired DELETE/INSERT CSV files for use with Salesforce 
Data Loader — a safer pattern for bulk correction that allows validation before 
commit and a clean rollback path if needed.

## Outcome
Corrected several hundred opportunity records across two batched waves with 
zero data loss on the unmatched edge cases, which were resolved separately 
through manual review.

## Skills demonstrated
SOQL query design, bulk data operations, risk-aware remediation strategy 
(batch processing over live edits), Salesforce Data Loader workflows.
