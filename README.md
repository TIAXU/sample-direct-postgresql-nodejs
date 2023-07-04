## Create the PostgreSQL service instance.
Perform the following command:
```
cf create-service postgresql v9.4-dev sapcpcfhw-db
```
For more information about this command, see the [documentation](https://docs.cloudfoundry.org/devguide/services/managing-services.html).


## Push to Cloud and run the service.
In the folder you cloned into, execute the `npm install` command.  
Then perform the following command:
```
cf push --random-route
```
For more information about this command, see the [documentation](http://docs.cloudfoundry.org/devguide/deploy-apps/deploy-app.html).  
Check the output of this command, and write down the URL created for the application.  

## Check in Browser
As a result you should be able to browse `https://<URL for your app>/users`.  
Import the new version of the `Postman` collection, and replace `<host>` with the allocated `<URL for your app>` in the URL.

## Check in Database
Terminal 1: 
```
cf enable-ssh YOUR-HOST-APP
cf restart YOUR-HOST-APP
cf create-service-key MY-DB EXTERNAL-ACCESS-KEY
cf service-key MY-DB EXTERNAL-ACCESS-KEY
cf ssh -L 63306:<hostname>:<port> YOUR-HOST-APP
```
Terminal 2:
```
brew install PostgreSQL
psql -d <dbname> -U <username> -p 63306 -h localhost
```
SQL: 
```
\d
SELECT * FROM <TABLE>;
pg_dump -p 63306 -U <username> <dbname> > /c/dataexport/mydata.sql
psql -p 63306 -U <username> -d <dbname> -c "COPY <tablename> TO stdout DELIMITER ',' CSV HEADER;" > /c/dataexport/<tablename>.csv
```
For more information: 
https://help.sap.com/viewer/6be7ed96ddeb4e158c2107c434142545/Cloud/en-US/7547876937594510aa13cfaf693d07b1.html
