# Backup Policy Creation
We will now create a policy to backup MySQL to the previously configured object storage location.

From the main K10 dashbboard, click on the Policies card. There should be no policies visible at this point.

Click Create New Policy and:

- Give the policy the name: mysql-backup
- Select Snapshot for the Action
- Select Hourly for the Action Frequency
- Leave the Snapshot Retention selection as-is
- Select By Name for Select Applications and then, from the dropdown, select mysql
- Leave all other settings as-is and select Create Policy



Running the Policy
The above policies will only run at the scheduled time (by default, at the top of the hour). To run the policies manually for the first time, click on Run Once on the Policies page. Confirm by clicking Run Policy and then go back to the main dashboard to view the job in action. Verify that the job has successfully completed.
