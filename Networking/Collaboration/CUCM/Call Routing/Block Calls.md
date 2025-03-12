## Diagram

![[diagram.png]]


## Create

[+] 2 Partitions:

```sql
run sql insert into routepartition (name,description) values ("GW_PRT","Needs to add to GW_CSS")

run sql insert into routepartition (name,description) values ("Filter_PRT","Needs to add to ToFilterCSS")
```

[+] 2 Calling Search Space:

```sql
run sql insert into callingsearchspace (name,description) values ("GW_CSS","We Add the GW_PRT only")

run sql insert into callingsearchspace (name,description) values ("ToFilterCSS","We Add the Filter_PRT only")
```

[+] We associate the CSS with the paritions

```sql
run sql insert into callingsearchspacemember (fkcallingsearchspace,fkroutepartition,sortorder) values ((select pkid from callingsearchspace where name = "GW_CSS"),(select pkid from routepartition where name = "GW_PRT"),1)

run sql insert into callingsearchspacemember (fkcallingsearchspace,fkroutepartition,sortorder) values ((select pkid from callingsearchspace where name = "ToFilterCSS"),(select pkid from routepartition where name = "Filter_PRT"),1)
```

[+] Create Translation Pattern for filtering calls

```sql
run sql insert into numplan (fkroutepartition,dnorpattern,tkpatternusage,routenexthopbycgpn,description,fkcallingsearchspace_translation,patternurgency) values ((select pkid from routepartition where name = "GW_PRT"),"!",3,"t","TP for filtering BlockingCalls",(select pkid from callingsearchspace where name like "ToFilterCSS"),"t")
```

[+] Create translation pattern for route all other calls, "**dCloud_CSS**" is the CSS that you normally use on your Trunk for inbound calls:

```sql
run sql insert into numplan (fkroutepartition,dnorpattern,tkpatternusage,description,fkcallingsearchspace_translation) values ((select pkid from routepartition where name = "Filter_PRT"),"!",3,"TP for route no block calls",(select pkid from callingsearchspace where name like "dCloud_CSS"))
```

[+] On the trunk will be configured as this:

![[inboundcallsetting.png]]

[+] you will create the translation pattern for route the calls that we are not blocking:
```sql
run sql insert into numplan (fkroutepartition,dnorpattern,tkpatternusage,description) values ((select pkid from routepartition where name = "Filter_PRT"),"!",3,"TP for route no block calls")
```

You need to add here the Calling Search space on the translation pattern by yourself here:
![[cssTP.png]]

[+] Then you create the translation pattern that will be blocking the calls, I will be blocking all calls from **3144762348** you can change here for your preference number, and add multiple blocking numbers by using this command multiple times:

```sql
run sql insert into numplan (fkroutepartition,dnorpattern,tkpatternusage,description,blockenable) values ((select pkid from routepartition where name = "Filter_PRT"),"3144762348",3,"TP for Block a Number","t")
```

[+] Finally assign the CSS to the trunk

I have this

![[inboundcallsetting.png]]

And I will change for this

![[inboundcallsetting2.png]]

[+] I will make an example calling from 3144762348 > 1000

![[dna-blockCall.png]]