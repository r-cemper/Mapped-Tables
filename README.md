# Semi-Pesistent Tables or Classes
This means a different approach to global storage.  
While by default you have generated hard coded storage globals.     
This example uses local variables with indirection to define   
the globals where your data are homed.  
### Description
The base class (User.People) is kind of a common template that  
you apply for USER, CLERKS, CUSTOMERS, ....  that are strictly  
separated from each other.
I admit that using inheritance may offer similar behaviour. 
As the original description dates from 2020 it is all designed
for the traditional storage model. 
Sharding, Columnar store, .... is not addressed by the eexample
### Prerequisites
Make sure you have [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) and [Docker desktop](https://www.docker.com/products/docker-desktop) installed.
### Installation
Clone/git pull the repo into any local directory
```
$ git clone https://github.com/r-cemper/Semi-Persistent-Tables.git
```
To build and start the container run:
```
$ docker compose up -d && docker compose logs -f
```
To open IRIS console Terminal do:
```
$ docker-compose exec iris iris session iris
USER>
```
or using **iterm**
```
http://localhost:52773/iterm/
```
To access IRIS System Management Portal
```
http://localhost:52773/csp/sys/UtilHome.csp
```
## Testing
Enter iris console or iterm
or work from SMP > Explorer > SQL


