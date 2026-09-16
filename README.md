## Mapped Tables and Classes
The previous example [Semi-Persistent-Tables](https://openexchange.intersystems.com/package/Semi-Persistent-Tables) was oriented to the   
traditional storage model and used dynamic change of globals.  
This has limits and risks that are avoided here.   
### Description
The base class (User.Common) is a template that you apply for   
USER, CLERKS, CUSTOMERS, ....  that are separated from each other  
as before.
The difference: Class User.Common uses parameter [NoExtent]   
so it doesn't include a storage definition.  
The storage to use is then assigned in the individual classes  
and it is now a static value that defines the Globals to use.   
And is a really simple definition. e.g.:  
```
Class User.USR Extends User.Common [ Not NoExtent ]
{
Parameter DEFAULTGLOBAL As STRING = "^USR";
}
```
All the rest is done by the compiler.   
Side effect: POPULATE is specific to every Class/Table    
### Prerequisites
Make sure you have [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) and [Docker desktop](https://www.docker.com/products/docker-desktop) installed.
### Installation
Clone/git pull the repo into any local directory  
```
$ git clone https://github.com/r-cemper/Mapped-Tables.git
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
[SQL]USER>>select * from USR where USR_POP(5)>0
2.      select * from USR where USR_POP(5)>0

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
[SQL]USER>>cos ZW ^USRD
^USRD=5
^USRD(1)=$lb("","Ahmed,Dmitry M.","782-47-5617",50613,"Bensonhurst")
^USRD(2)=$lb("","Vivaldi,Xavier X.","802-93-5957",31727,"Washington")
^USRD(3)=$lb("","Quine,Hannah N.","948-78-9049",56042,"Newton")
^USRD(4)=$lb("","LaRocca,Nellie L.","744-55-4737",56219,"Oak Creek")
^USRD(5)=$lb("","Gallant,Kim Z.","555-47-4869",49917,"Miami")

[SQL]USER>>cos ZW ^USERI
^USRI("NameDob"," AHMED,DMITRY M.",50613,1)=""
^USRI("NameDob"," GALLANT,KIM Z.",49917,5)=""
^USRI("NameDob"," LAROCCA,NELLIE L.",56219,4)=""
^USRI("NameDob"," QUINE,HANNAH N.",56042,3)=""
^USRI("NameDob"," VIVALDI,XAVIER X.",31727,2)=""
^USRI("NameIDX"," AHMED,DMITRY M.",1)=$lb("","782-47-5617")
^USRI("NameIDX"," GALLANT,KIM Z.",5)=$lb("","555-47-4869")
^USRI("NameIDX"," LAROCCA,NELLIE L.",4)=$lb("","744-55-4737")
^USRI("NameIDX"," QUINE,HANNAH N.",3)=$lb("","948-78-9049")
^USRI("NameIDX"," VIVALDI,XAVIER X.",2)=$lb("","802-93-5957")
^USRI("SSNKey"," 555-47-4869",5)=$lb("","Gallant,Kim Z.")
^USRI("SSNKey"," 744-55-4737",4)=$lb("","LaRocca,Nellie L.")
^USRI("SSNKey"," 782-47-5617",1)=$lb("","Ahmed,Dmitry M.")
^USRI("SSNKey"," 802-93-5957",2)=$lb("","Vivaldi,Xavier X.")
^USRI("SSNKey"," 948-78-9049",3)=$lb("","Quine,Hannah N.")
```
For temporary use on PPG this looks like this   
```
[SQL]USER>>select * from PPG where PPG_POP(3)>0
8.      select * from PPG where PPG_POP(3)>0

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
Any normal SELECT just requires the Table name. Nothing else. No tricks
```
[SQL]USER>>SELECT name,city,id from PPG order by city
9.      SELECT name,city,id from PPG order by city

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
[actual Article](https://community.intersystems.com/post/mapped-tables-and-classes) . . 
[original Article](https://community.intersystems.com/post/semi-persistent-classes-and-tables)

