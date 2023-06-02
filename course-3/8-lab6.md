# Restore from Backup
Now that we have a MySQL backup, let's go simulate accidental data loss and then recover the system from that loss.

Causing Data Loss
For the purposes of this test drive, we have dropped the k10demo database we had created earlier.

Verify that the database has been deleted by running:

```
MYSQL_ROOT_PASSWORD=$(kubectl get secret --namespace mysql mysql -o jsonpath="{.data.mysql-root-password}" | base64 --decode; echo)

kubectl exec -it --namespace=mysql $(kubectl --namespace=mysql get pods -o jsonpath='{.items[0].metadata.name}') -- env MYSQL_PWD=$MYSQL_ROOT_PASSWORD mysql -u root -e "SHOW DATABASES LIKE 'k10demo'"
```

It's gone!

Recovering Data
To recover the databases, go to the K10 dashboard, click Applications, and then select Restore on the MySQL card.

Click on a recent restore point and then select the Exported restore point (this is stored in the object storage system instead of as a non-durable snapshot on the storage system). In this case, we will select the default Application Name option to restore in place (Restore as "mysql"). Leave all other selections as-is, click on Restore, and confirm the action.

Return to the main dashboard to view the Restore job and verify that it completes successfully.

To verify that our data was recovered, run the following command in the terminal to view the restored database:

```
kubectl exec -it --namespace=mysql $(kubectl --namespace=mysql get pods -o jsonpath='{.items[0].metadata.name}') -- env MYSQL_PWD=$MYSQL_ROOT_PASSWORD mysql -u root -e "SHOW DATABASES LIKE 'k10demo'"
```
