Laravel is a powerful PHP framework that makes it easy to manage your web application's database connections. In this tutorial, we'll show you how to set up multiple database connections in Laravel and how to switch between them on the fly.

##Replicating Your Database in Mariadb

The first step to connecting to multiple databases in Laravel is to replicate your database in Mariadb. This will allow you to have a read-only copy of your database that can be used to offload some of the read-heavy workload from your primary database server.

To set up database replication in Mariadb, follow these easy steps (taken from [mariadb-replication-easy-step](https://hevodata.com/learn/mariadb-replication-easy-steps/#Method1)):

1. Make sure that Mariadb is installed on your server.
2. Create a new database on your primary server and grant access to it for the replication user.
3. On the secondary server, create a new database and grant access to it for the replication user.
4. On the primary server, run the following command to set up the replication:
    ```sql
    CHANGE MASTER TO MASTER_HOST='secondary_server_ip', 
    MASTER_USER='replication_user', 
    MASTER_PASSWORD='replication_password', 
    MASTER_LOG_FILE='primary_server_bin_log_filename', 
    MASTER_LOG_POS=primary_server_bin_log_position;
    ```
5. Start the replication on the secondary server by running the following command:
    ```sql
    START SLAVE;
    ```
Your database should now be replicating from the primary server to the secondary server.

##Setting Up Laravel to Connect to Multiple Databases

Now that your database is replicated in Mariadb, it's time to set up Laravel to connect to multiple databases.

To do this, open the config/database.php file and add the following code (as described in the [Read & Write Connections](https://laravel.com/docs/5.6/database#read-and-write-connections) documentation):

```php
'mysql' => [
    'read' => [
        'host' => [ '192.168.1.1', '196.168.1.2' ],
    ],
    'write' => [
        'host' => ['196.168.1.2'],
    ],
    'sticky'    => true,
    ...
],
```

This code tells Laravel to use the `read` connection for read-only queries and the `write` connection for all other queries.

##Checking Which Database Server Laravel is Connected To

To check which database server Laravel is currently connected to, you can use the following code:

```php
Route::get('/mysql-test', function () {
  $places = App\Place::all();
  $results = DB::select( DB::raw("SHOW VARIABLES LIKE 'server_id'") );  
  return "Server ID: " . $results[0]->Value;
});
```

This will display the server ID of the database server that Laravel is currently connected to.

And that's it! You now know how to set up multiple database connections in Laravel and switch between them on the fly. 

Happy coding!