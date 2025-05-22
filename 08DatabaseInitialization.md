## Database Initialization
- An SQL database can be initialized in different ways depending on what your stack is. Of course, you can also do it manually, provided the database is a separate process.

### Initialize a Database Using Hibernate
- You can set `spring.jpa.hibernate.ddl-auto` to control `Hibernate’s database initialization`. Supported values are `none`, `validate`, `update`, `create`, and `create-drop`.
 
