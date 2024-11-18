# Relationships

This section covers one of the more complex topics of your Budibase web app, the ability to create relationships between different rows. The relationship is bi-directional, in that when it is created, each row will have a relation to the other one. A row is also allowed to relate to many rows, meaning that many to many relationships can be created in your web app.

### Creating a relationship column

1. Select two tables that the relationship should be formed between.
2.  Make sure that in both tables there is a column defined that can be used for display purposes. When creating this column make sure to set it with the _Display column_ toggle.\


    <figure><img src="https://files.readme.io/43d1a74-small-Screenshot_2023-05-15_at_12.46.57.png" alt=""><figcaption></figcaption></figure>
3.  Click the _Create new column_ button in either of the two tables and select the _Relationship_ data type from the dropdown.\


    <figure><img src="https://files.readme.io/5bc1f6c-small-Screenshot_2023-05-15_at_12.49.37.png" alt=""><figcaption></figcaption></figure>
4.  Here you will need to select two options, the tables to which this column relates, and the name of the relationship column in the other table as well.\


    <figure><img src="https://files.readme.io/be8bbca-Screenshot_2024-01-17_at_08.09.48.png" alt=""><figcaption></figcaption></figure>
5.  At this point, you will now have a relationship formed between the two tables. If you create a row in one of the tables, then go to the other table and create a row you should see the first row that was created as an option in the relationship multi-select.\


    <figure><img src="https://files.readme.io/0dbf6a3-showcasing_relationships.gif" alt=""><figcaption></figcaption></figure>

\


### Example

In this example, we will be creating a web app for tracking students, teachers, parking spots for teachers and classes in a school - specifically how they relate to each other.

Specifically, we are going to break these down into the three relationship types that can be modelled with Budibase. Below we will map out the relationships that we are going to create, first describing them then walking through how to build them.

#### One-to-many

<figure><img src="https://files.readme.io/28d7cbb-one-to-many.png" alt=""><figcaption></figcaption></figure>

The second relationship we can model in Budibase is one of the most common, linking a single row to many other rows. Here we can see that a class can only be taught by a single teacher however a teacher is capable of teaching many different classes.

#### Many-to-many

<figure><img src="https://files.readme.io/57e6b3d-many-to-many.png" alt=""><figcaption></figcaption></figure>

The last relationship we can model is arguably one of the most complexes but again very common in real-life systems. Here we can see that students are able to take many classes and each class can have many different students in it.

#### Building it in Budibase

To create this in Budibase all we need to do is:

1. Create the four tables, students, classes, teachers, and parking spots. In each of these, we will initially create a display text column and mark these as the _Display column -_ for example, N_ame_ or _Subject_.
2. First, we will create the teacher to parking spot relationship, starting in the teacher table we create a relationship type column, call it `ParkingSpot` and set the table to parking spots - then name the column in the other table `Teacher`.
3. Second, we create the teacher to classes relationship, hopefully, at this point, it should be clear how this is done. Simply create a relationship from the teacher table to the class table, name the column `Classes` and in the other table call the column `Teacher`.
4. Lastly, we create the students to classes relationship, the same as before but set the relationship table to classes, call the column in the students table `Classes` and call the column in the other table `Students`.

As you can see by creating three relationship columns we were able to make a fairly complex web of relationships, which can now be easily managed when creating/updating rows in any of the tables.

#### Video Demonstration

Based on the example above here is a video showing how easy it is to create in the Budibase Builder!

### Considerations

The Budibase DB in a NoSQL database. As such, there are implicit limitations when it comes to handling relational data for larger more complex applications.\
Watch the video below for more information on deciding which database type is best to use.
