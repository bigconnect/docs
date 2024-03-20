# Using a relational database

The setup for the relational persistence layers is a 3-step process. The first two steps are database specific, so please refer to the relevant subsections below.

1. **Database setup**: You set up the database and create a schema with a few tables. This is typically be done by a DBA.
2. **Application server setup**: You configure your application server to use the database.
3. **Advanced Forms setup**: You configure Advanced Forms to use the relevant persistence layer.

### Database setup <a href="#database-setup" id="database-setup"></a>

#### Oracle database setup <a href="#oracle-database-setup" id="oracle-database-setup"></a>

1. Make sure that Oracle's Database Character Set is set to `AL32UTF8`, also as [recommended by Oracle](http://docs.oracle.com/cd/B19306\_01/appdev.102/b14259/xdb03usg.htm#sthref263). You can see you database parameters by running the following query: `select * from nls_database_parameters`, and the Database Character Set is identified by `nls_characterset`.
2.  Create a user/schema in Oracle, for instance with the commands below. In this example "all privileges" are granted to the newly created user/schema, which is not strictly required. You might want to fine-tune permissions on your system as appropriate. If you had already created this schema and that the definition changed, or that you want to restart from scratch for some other reason, you can first delete the schema with all the data it contains with `drop user forms cascade`.

    Copy

    ```
    > sqlplus sys/password as sysdba
    SQL> CREATE USER FORMS IDENTIFIED BY password ;
    SQL> GRANT ALL PRIVILEGES TO forms ;
    SQL> GRANT UNLIMITED TABLESPACE TO forms ;
    ```
3. Run the DDL to create or update your Advanced Forms database, and note that if upgrading to 2016.2, you need to reindex your Advanced Forms database.

**Oracle binary XML storage**

With Oracle 11.2, `XMLType` values are stored by default using the binary XML storage. The binary XML storage has numerous benefits over the basic file storage. In many respect, it is the "proper" way to store XML. However, we found that Oracle fails to properly save some documents when the binary XML storage is used. In particular, when documents have attributes with long values (several thousands of characters), when retrieving the document, the value of some attributes is missing. For this reason, until this issue is solved by Oracle, we recommend you store `XMLType` values as "basic file", per the above DDL.

#### MySQL database setup <a href="#mysql-database-setup" id="mysql-database-setup"></a>

**MySQL 5.7 and MySQL 8**: Since version 2018.2, Advanced Forms uses the `utf8mb4` character set instead of the `utf8` character set. The reason being that MySQL's `utf8` character set can only store UTF-8-encoded symbols that consist of 1 to 3 bytes, that is characters in the Unicode [Basic Multilingual Plane](https://tinyurl.com/85a8weh), which means that none of the characters in the [Supplementary Multilingual Plane](https://tinyurl.com/yatmk5et), which include Emojis, could stored. However, the switch to the `utf8mb4` character set will prevent you from creating some indexes on MySQL 5.6 where default key prefix limit is 767 bytes. Hence we recommend you use MySQL 5.7, which raised the index key prefix length limit to 3072 bytes for InnoDB tables. If in your situation upgrading to MySQL 5.7 isn't an option, you can explore [enabling the `innodb_large_prefix` configuration option on your MySQL 5.6](https://dev.mysql.com/doc/refman/5.6/en/innodb-restrictions.html), or changing the DDL that ships with Advanced Forms to use `utf8` instead of `utf8mb4`.

**Unicode support**

By default, the MySQL JDBC driver [uses](https://dev.mysql.com/doc/connector-j/5.1/en/connector-j-reference-charsets.html) the _character encoding_ and _character collation_ set on the server. [Up to MySQL 5.7](https://dev.mysql.com/doc/refman/5.7/en/charset-server.html) (included), the default character encoding is `latin1` and the default collation `latin1_swedish_ci`. [Since MySQL 8](https://dev.mysql.com/doc/refman/8.0/en/charset-server.html), the default character encoding is `utf8mb4` and the default collation `utf8mb4_0900_ai_ci`. So, if you're using MySQL 5.7 or earlier, you must specify the following 2 parameters when starting MySQL:

```
--character-set-server=utf8mb4
--collation-server=utf8mb4_0900_ai_ci
```

**Setting up users and schema**

1.  Create a new user `forms`. Advanced Forms will connect to MySQL as that user.



    ```
    mysql -u root
    mysql> CREATE USER 'forms'@'%' IDENTIFIED BY 'forms';
    ```
2.  Create a new schema `forms`. This schema will contains the tables used to store your forms definitions and form data.



    ```
    mysql> CREATE schema forms;
    ```
3.  If needed, grant permissions, for example:



    ```
    mysql> GRANT ALL PRIVILEGES ON *.* TO 'forms'@'%' WITH GRANT OPTION;
    ```
4. Run the DDL to create or update your Forms database

#### SQL Server database setup <a href="#sql-server-database-setup" id="sql-server-database-setup"></a>

Advanced Forms relies on SQL Server's full-text search, which is included out-of-the-box in all SQL Server editions, except the Express and Express with Tools. If you're using one of those two editions of SQL Server, you might want to look into getting Express with Advanced Services.

Run the DDL to create or update your Fomrs database.

#### PostgreSQL database setup <a href="#postgresql-database-setup" id="postgresql-database-setup"></a>

Run the DDL to create or update your Forms database.

#### DB2 database setup <a href="#db2-database-setup" id="db2-database-setup"></a>

Run the  DDL to create or update your Forms database.

### Advanced Forms setup <a href="#orbeon-forms-setup" id="orbeon-forms-setup"></a>

#### With a single schema <a href="#with-a-single-schema" id="with-a-single-schema"></a>

In your `properties-local.xml`, you map an app / form / form type to the implementation of the persistence API you're using with the `oxf.fr.persistence.provider.*.*.*` wildcard property. For instance, if using Oracle, set the property to:

```
<property
    as="xs:string"
    name="oxf.fr.persistence.provider.*.*.*"
    value="oracle"/>
```

#### With multiple schemas <a href="#with-multiple-schemas" id="with-multiple-schemas"></a>

The single schema configuration described in the previous section uses the predefined `oracle` and `mysql` providers. To use multiple schemas you need to define you own provider names. For instance, assume that you have two apps, `hr` and `finance`, and would like both the form definition and data for those apps to be stored in two separate schemas:

1. In your application server configuration, you setup two data sources ; let's call them `hr-datasource` and `finance-datasource`.
2.  In `properties-local.xml`, you use the following properties to define two providers `hr` and `finance` that you configure to use the desired persistence layer implementation (Oracle in this example) and data source:

    Copy

    ```
    <!-- HR provider -->
    <property
        as="xs:anyURI"
        name="oxf.fr.persistence.hr.uri"
        value="/fr/service/oracle"/>
    <property
        as="xs:string"
        name="oxf.fr.persistence.hr.datasource"
        value="hr-datasource"/>

    <!-- Finance provider -->
    <property
        as="xs:anyURI"
        name="oxf.fr.persistence.finance.uri"
        value="/fr/service/oracle"/>
    <property
        as="xs:string"
        name="oxf.fr.persistence.finance.datasource"
        value="finance-datasource"/>
    ```
3.  Still in `properties-local.xml`, you map the `hr` and `finance` app to the respective provider:

    Copy

    ```
    <property
        as="xs:string"
        name="oxf.fr.persistence.provider.hr.*.*"
        value="hr"/>
    <property
        as="xs:string"
        name="oxf.fr.persistence.provider.finance.*.*"
        value="finance"/>
    ```

### Flat view or table <a href="#flat-view-or-table" id="flat-view-or-table"></a>

Advanced Forms stores form data as XML in relational databases, which gives it a lot of flexibility. However, it might be harder for other tools to access this XML data. For this reason, you might want to provide other tools a way to access the XML data through another "flat" table or view that has one column per form field.

#### Manual relational table setup with MySQL <a href="#manual-relational-table-setup-with-mysql" id="manual-relational-table-setup-with-mysql"></a>

Advanced Forms doesn't provide a way to have a table or view automatically created for a form upon publishing in MySQL. However, you can do this manually. For instance, assume you want to create a "flat" `bookshelf` table for the sample bookshelf form. You want that table to have 3 columns:

* `title` corresponds to the title form field;
* `author` corresponds to the author form field;
* `document_id` corresponds to the column with the same name in `forms_form_data`.

Start by creating the `bookshelf` table:

```
create table bookshelf (
    document_id varchar(255),
    title  text,
    author text
);
```

Choose an appropriate type for your columns, depending on the maximum length for the fields. Then create a trigger, which will update your `bookshelf` table when form data is saved in `forms_form_data`:

```
delimiter |
create trigger bookshelf_trigger before insert on forms_form_data for each row begin
    if new.app = 'forms' and new.form = 'bookshelf' then
        delete from bookshelf where document_id = new.document_id;
        if new.deleted = 'N' and new.draft = 'N' then
            insert into bookshelf set document_id = new.document_id,
                title = extractValue(new.xml, '/form/details/title'),
                author = extractValue(new.xml, '/form/details/author');
        end if;
    end if;
end;
|
```

Since you are interested in data for Bookshelf form, which is in the app `forms` form `bookshelf`, the trigger only does something if `new.app = 'forms' and new.form = 'bookshelf'`. To enable auditing, the MySQL persistence layer never deletes or updates data; it only inserts new row. So your trigger only needs to be concerned about updates. On insert, you want to make sure you are not creating duplicates in your `bookshelf` table, hence the `delete` statement. When a newly inserted row has `delete = 'N'`, this indicates that a user deleted that document, in which case you don't want to insert a row in your `bookshelf` table, hence the `if` test.
