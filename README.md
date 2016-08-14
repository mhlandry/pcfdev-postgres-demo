# pcfdev-postgres-demo

Demo project for connecting to PCF Dev Postgres Service with a Spring Boot app

## Prerequisites

1. A running, configured PostgreSQL database.  For Mac OS, you can install one via homebrew.  [Postgres.app](Postgres.app) also looks to be very easy to use.  [Here](https://www.postgresql.org/download/macosx/) are some more options.
  *  Configure the database to listen for the PCF Dev VM.
    1.  Edited postgresql.conf, uncomment the listen_adresses line and set it to `listen_addresses = '192.168.11.1,localhost'`
      *  If using Postgres.app, you can find your postgresql.conf file in the Data Directory folder listed in the apps preferences
    2.  Edit pg_hba.conf, add a line with `host     all             all             192.168.11.1/24           trust`
2. [PCF Dev](https://docs.pivotal.io/pcf-dev/index.html) installed and running.

## Running the Spring Boot app, Creating the Postgres service and Connecting

1. Clone this repo.
2. Install the app (`mvn clean install`)
2. Run the app locally.
  1. `mvn clean spring-boot:run -Dspring.profiles.active=local`
  2. Navigate to the [Health](http://loalhost:8080/health) endpoint.
    1. Verify the app is running.
    2. Verify the database is `Postgres`.
    3. Verify the status of the database is `Up`.
3. Create a postgresql user provided sevice in PCF Dev similar to the following: `cf cups postgres -p "uri,username,password"`
  * for uri, enter `postgres://postgres:postgres@host.pcfdev.io:5432/postgres`
  * for username, enter `postgres`
  * for password, enter `postgres`
4. Using Postico, pgADmin or some other similar tool, connect to Postgres DB through the PVF Dev VM to ensure it's working.  Use hostname `host.cfdev.io`.
5. Push the app 
  1. `cf push`
  2. Navigate to the [Health](http://pcfdev-postgres-demo.local.pcfdev.io/health) endpoint
    1. Verify the app is running.
    2. Verify the database is `Postgres`.
    3. Verify the status of the database is `Up`.
 
## Troubleshooting

* Spring Boot 1.4 does not currently work in Cloud Foundry (as of 2016.08.14).  When pushing the app, the push will fail and you will get a class not found exception.  See one of the answers in this [Stack Overflow](http://stackoverflow.com/questions/35712435/spring-boot-java-lang-classnotfoundexception-org-springframework-context-appl) post

## Resources

The following resources were very helpful in figuring this out.
* [Stack Overflow Answer with the PCF Dev hostname of the db] (http://stackoverflow.com/questions/38232758/how-use-posgres-or-mongo-databases-in-pcf-devpivotal-cloudfoundry-dev)
* [Official PCF Dev Usage Docs](https://docs.pivotal.io/pcf-dev/usage.html)
* [Spring Cloud Connector Docs](http://cloud.spring.io/spring-cloud-connectors/spring-cloud-spring-service-connector.html)
* [Binging to Data Services with Spring Boot in Cloud Foundry](https://spring.io/blog/2015/04/27/binding-to-data-services-with-spring-boot-in-cloud-foundry)
* [Deploying Spring Boot DZone article](https://dzone.com/articles/deploying-spring-boot)
* [Stack Overflow question on connecting pgAdmin3 to a database on a VM](http://stackoverflow.com/questions/16665438/how-to-connect-pgadmin3-to-a-database-on-virtualbox-machine)
