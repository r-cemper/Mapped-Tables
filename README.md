## Semi-Pesistent Tables and Classes
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
Sharding, Columnar store, .... is not addressed by the example   
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
Enter iris console or [iterm](http://localhost:52773/iterm/)  {_SYSTEM/SYS}    
or work from [SMP > Explorer > SQL](http://localhost:52773/csp/sys/exp/%25CSP.UI.Portal.SQL.Home.zen?$NAMESPACE=USER)
```
USER>:sql
SQL Command Line Shell
[SQL]USER>>select * from people where MyPopulate('^USER',5)>0
2.      select * from people where MyPopulate('^USER',5)>0

| ID | City | DOB | Name | SSN |
| -- | -- | -- | -- | -- |
| 1 | Bensonhurst | 50613 | Ahmed,Dmitry M. | 782-47-5617 |
| 2 | Washington | 31727 | Vivaldi,Xavier X. | 802-93-5957 |
| 3 | Newton | 56042 | Quine,Hannah N. | 948-78-9049 |
| 4 | Oak Creek | 56219 | LaRocca,Nellie L. | 744-55-4737 |
| 5 | Miami | 49917 | Gallant,Kim Z. | 555-47-4869 |

5 Rows(s) Affected
statement prepare time(s)/globals/cmds/disk: 0.0528s/39,623/207,252/0ms
          execute time(s)/globals/cmds/disk: 0.0044s/116/3,631/0ms
                                query class: %sqlcq.USER.cls3
---------------------------------------------------------------------------
[SQL]USER>>cos ZW ^USERD
^USERD=5
^USERD(1)=$lb("","Ahmed,Dmitry M.","782-47-5617",50613,"Bensonhurst")
^USERD(2)=$lb("","Vivaldi,Xavier X.","802-93-5957",31727,"Washington")
^USERD(3)=$lb("","Quine,Hannah N.","948-78-9049",56042,"Newton")
^USERD(4)=$lb("","LaRocca,Nellie L.","744-55-4737",56219,"Oak Creek")
^USERD(5)=$lb("","Gallant,Kim Z.","555-47-4869",49917,"Miami")

[SQL]USER>>cos ZW ^USERI
^USERI("NameDob"," AHMED,DMITRY M.",50613,1)=""
^USERI("NameDob"," GALLANT,KIM Z.",49917,5)=""
^USERI("NameDob"," LAROCCA,NELLIE L.",56219,4)=""
^USERI("NameDob"," QUINE,HANNAH N.",56042,3)=""
^USERI("NameDob"," VIVALDI,XAVIER X.",31727,2)=""
^USERI("NameIDX"," AHMED,DMITRY M.",1)=$lb("","782-47-5617")
^USERI("NameIDX"," GALLANT,KIM Z.",5)=$lb("","555-47-4869")
^USERI("NameIDX"," LAROCCA,NELLIE L.",4)=$lb("","744-55-4737")
^USERI("NameIDX"," QUINE,HANNAH N.",3)=$lb("","948-78-9049")
^USERI("NameIDX"," VIVALDI,XAVIER X.",2)=$lb("","802-93-5957")
^USERI("SSNKey"," 555-47-4869",5)=$lb("","Gallant,Kim Z.")
^USERI("SSNKey"," 744-55-4737",4)=$lb("","LaRocca,Nellie L.")
^USERI("SSNKey"," 782-47-5617",1)=$lb("","Ahmed,Dmitry M.")
^USERI("SSNKey"," 802-93-5957",2)=$lb("","Vivaldi,Xavier X.")
^USERI("SSNKey"," 948-78-9049",3)=$lb("","Quine,Hannah N.")
```
For temporary use on PPG this looks like this   
```
[SQL]USER>>select * from people where MyPopulate('||PPG',3)>0
8.      select * from people where MyPopulate('||PPG',3)>0

| ID | City | DOB | Name | SSN |
| -- | -- | -- | -- | -- |
| 1 | Queensbury | 42086 | Nichols,Phyllis F. | 677-79-9564 |
| 2 | Elmhurst | 44148 | Vanzetti,Milhouse C. | 178-30-1660 |
| 3 | Larchmont | 55590 | Jenkins,Joshua V. | 586-48-2608 |

3 Rows(s) Affected
statement prepare time(s)/globals/cmds/disk: 0.0028s/37/4,700/0ms
          execute time(s)/globals/cmds/disk: 0.0044s/104/3,054/0ms
                                query class: %sqlcq.USER.cls3
---------------------------------------------------------------------------
[SQL]USER>>cos zw ^||PPGD
^||PPGD=3
^||PPGD(1)=$lb("","Nichols,Phyllis F.","677-79-9564",42086,"Queensbury")
^||PPGD(2)=$lb("","Vanzetti,Milhouse C.","178-30-1660",44148,"Elmhurst")
^||PPGD(3)=$lb("","Jenkins,Joshua V.","586-48-2608",55590,"Larchmont")

[SQL]USER>>cos zw ^||PPGI
^||PPGI("NameDob"," JENKINS,JOSHUA V.",55590,3)=""
^||PPGI("NameDob"," NICHOLS,PHYLLIS F.",42086,1)=""
^||PPGI("NameDob"," VANZETTI,MILHOUSE C.",44148,2)=""
^||PPGI("NameIDX"," JENKINS,JOSHUA V.",3)=$lb("","586-48-2608")
^||PPGI("NameIDX"," NICHOLS,PHYLLIS F.",1)=$lb("","677-79-9564")
^||PPGI("NameIDX"," VANZETTI,MILHOUSE C.",2)=$lb("","178-30-1660")
^||PPGI("SSNKey"," 178-30-1660",2)=$lb("","Vanzetti,Milhouse C.")
^||PPGI("SSNKey"," 586-48-2608",3)=$lb("","Jenkins,Joshua V.")
^||PPGI("SSNKey"," 677-79-9564",1)=$lb("","Nichols,Phyllis F.")
```
And any normal SLEETC just requires the STATIC condition to set the Globals 
```
[SQL]USER>>SELECT name,city,id from People where SetStorage('||PPG')>0 order by city
9.      SELECT name,city,id from People where SetStorage('||PPG')>0 order by city

| Name | City | ID |
| -- | -- | -- |
| Vanzetti,Milhouse C. | Elmhurst | 2 |
| Jenkins,Joshua V. | Larchmont | 3 |
| Nichols,Phyllis F. | Queensbury | 1 |

3 Rows(s) Affected
statement prepare time(s)/globals/cmds/disk: 0.0591s/39,452/216,468/0ms
          execute time(s)/globals/cmds/disk: 0.0003s/4/936/0ms
                                query class: %sqlcq.USER.cls6
---------------------------------------------------------------------------
```
[original Article](https://community.intersystems.com/post/semi-persistent-classes-and-tables)

