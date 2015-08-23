## Dependency Inversion principle
**Creds to Samuel Oloruntoba(@kayandrae07) from Scotch IO**

> Entities must depend on abstractions not on concretions. It states that the high level module must not depend on the low level module, but they should depend on abstractions.

This might sound bloated, but it is really easy to understand. This principle allows for decoupling, an example that seems like the best way to explain this principle:

```PHP
class PasswordReminder {
    private $dbConnection;

    public function __construct(MySQLConnection $dbConnection) {
        $this->dbConnection = $dbConnection;
    }
}
```

First the MySQLConnection is the low level module while the PasswordReminder is high level, but according to the definition of D in S.O.L.I.D. which states that Depend on Abstraction not on concretions, this snippet above violates this principle as the PasswordReminder class is being forced to depend on the MySQLConnection class.

Later if you were to change the database engine, you would also have to edit the PasswordReminder class and thus violates Open-close principle.

The PasswordReminder class should not care what database your application uses, to fix this again we “code to an interface”, since high level and low level modules should depend on abstraction, we can create an interface:

```PHP
interface DBConnectionInterface {
    public function connect();
}    
```

The interface has a connect method and the MySQLConnection class implements this interface, also instead of directly type-hinting MySQLConnection class in the constructor of the PasswordReminder, we instead type-hint the interface and no matter the type of database your application uses, the PasswordReminder class can easily connect to the database without any problems and OCP is not violated.

```PHP
class MySQLConnection implements DBConnectionInterface {
    public function connect() {
        return "Database connection";
    }
}

class PasswordReminder {
    private $dbConnection;

    public function __construct(DBConnectionInterface $dbConnection) {
        $this->dbConnection = $dbConnection;
    }
}  
```

According to the little snippet above, you can now see that both the high level and low level modules depend on abstraction.