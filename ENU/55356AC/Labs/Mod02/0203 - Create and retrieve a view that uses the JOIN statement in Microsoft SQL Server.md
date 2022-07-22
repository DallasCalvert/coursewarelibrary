# Lab 3: Create and retrieve a view that uses the JOIN statement in Microsoft SQL Server

## Scenario

You work at a publishing house managing information about books and publishers. In your customer-facing role, you're often asked for the titles of books and the names of their publishers. However, these two pieces of information are stored in different tables in the database. You'll retrieve a synthesis of this information using a JOIN statement. After you've done so, you'll create a view that runs this statement.

## Objectives

By completing this lab, you’ll achieve the knowledge and skills to:

- Connect to the database.
- Create a view that retrieves information using a JOIN statement.


Estimated time: 10 minutes

## Exercise 1: Connect to the database

### Scenario

You'll begin by using Microsoft SQL Server Management Studio to connect to the publishing house's database, named **pubs**. This will allow you to retrieve information from the database.

The main tasks for this exercise are as follows:

1. Prepare the lab environment.
2. Connect to the database.

#### Task 1: Prepare the lab environment.

1. Sign in to MIA-SQL as **Administrator** with the password of **Pa55w.rd**.

2. On the taskbar, select **Microsoft SQL Server Management Studio 18**. 

   > The Microsoft SQL Server Management Studio opens.

####  Task 2: Connect to the database

1. From Microsoft SQL Server Management Studio, in the **Connect to Server** dialog box, verify the following selections, and then select **Connect**.

   - Server type: **Database Engine**

   - Server name: **MIA-SQL**

   - Authentication: **Windows Authentication**

   >The **Object Explorer** opens.

2. From the **Object Explorer**, expand the **Databases** node.

3. Right-click or access the context menu for the **pubs** database, and then select **New Query**.

   > The **SQLQuery1.sql** window opens. In this query window you can run SQL code against the **pubs** database.

### Results

You've connected to the **pubs** database and have opened a new query window.

## Exercise 2: Create a view that retrieves information using a JOIN statement

### Scenario

Now that you've successfully connected to the **pubs** database, you'll run a `SELECT` statement to retrieve information from the **titles** and **publishers** tables. Then you'll use a `JOIN` statement to combine select information from each table to retrieve only the titles of books and the names of their publishers. Finally, you'll create and run a view from this `JOIN` statement.

The main tasks for this exercise are as follows:

1. Retrieve information using the `SELECT` and `JOIN` statements.
1. Create a view and use it to retrieve information from the database.

#### Task 1: Retrieve information using the SELECT and JOIN statements

1. In a new query to the **pubs** database, enter the following `SELECT` statements in the **SQLQuery1.sql** window:

   ```sql
   SELECT * FROM titles;
   SELECT * FROM publishers;
   ```

2. Select the **Execute** button, or select **`F5`** on the keyboard to run the statements. 

   > You should receive two tables.
   >
   > The first table, **titles**, contains information about books. It should have 10 columns and 18 rows.
   >
   > The second table, **publishers**, contains information about publishers. It should have 5 columns and 8 rows.
   
3. Delete the previous query from the **SQLQuery1.sql** window and then enter the following `JOIN` statement:

   ```sql
   SELECT title, pub_name FROM titles
   INNER JOIN publishers
   ON titles.pub_id = publishers.pub_id;
   ```

4. Select the **Execute** button, or select **`F5`** on the keyboard to run the statement.

   > You should receive a table that matches the titles of books to the names of their publishers.
   >
   > The table should have 2 columns and 18 rows.
   

#### Task 2: Create a view and use it to retrieve information from the database

1. Modify the final statement from **Task 1** or enter the following `CREATE VIEW` statement in a new query to the **pubs** database:

   ```sql
   CREATE VIEW title_and_publisher_name
   AS
   SELECT title, pub_name FROM titles
   INNER JOIN publishers
   ON titles.pub_id = publishers.pub_id;
   ```

2. Select the **Execute** button, or select **`F5`** on the keyboard to run the statement. 

   > The view is created.

3. Delete the previous query from the **SQLQuery1.sql** window. Enter the following `SELECT` statement:

   ```sql
   SELECT * FROM title_and_publisher_name;
   ```

4. Select the **Execute** button, or select **`F5`** on the keyboard to run the statement.

   > You should receive an identical table to the one you received from the final `SELECT` statement in **Task 1**, that matches the titles of books to the names of their publishers. 
   >
   > The table should have 2 columns and 18 rows.

5. Close Microsoft SQL Server Management Studio without saving changes.

### Results

You can now use Microsoft SQL Server Management Studio to run a `JOIN` statement, and then save and retrieve it as a view.
