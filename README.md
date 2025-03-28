## Test Laravel Eloquent

There are PHPUnit tests in `tests/Feature/EloquentTest.php` file.

## Task 1. Model with Different Table Name.

In `app/Models/Morningnews.php` file, change it so that the model would work with "morning_news" table, as it is created in the migrations.

Test method `test_create_model_incorrect_table()`.

---

## Task 2. Get Data List.

In `app/Http/Controllers/UserController.php` file method `index()`, write Eloquent query to get 3 newest users with verified emails, ordered from newest to oldest. Transform this SQL query into Eloquent:

```
select * from users where email_verified_at is not null order by created_at desc limit 3
```

Test method `test_get_filtered_list()`.

---

## Task 3. Get a Single Record.

In `app/Http/Controllers/UserController.php` file method `show($userId)`, fill in the `$user` value with finding the user by `users.id = $userId`. If the user is not found, show default Laravel 404 page. 

Test method `test_find_user_or_show_404_page()`.

---

## Task 4. Get a Single Record or Create a New Record.

In `app/Http/Controllers/UserController.php` file method `check_create()`, find the user by name and email. If the user is not found, create it (with random password).

Test method `test_check_or_create_user()`.

---

## Task 5. Create a New Record.

In `app/Http/Controllers/ProjectController.php` file method `store()`, creating the project will fail. Fix the underlying issue, to make it work.

Test method `test_create_project()`.

---

## Task 6. Mass Update.

In `app/Http/Controllers/ProjectController.php` file method `mass_update()`, write the update SQL query as Eloquent statement.

```
update projects set name = $request->new_name where name = $request->old_name
```

Test method `test_mass_update_projects()`.

---

## Task 7. Update or New Record.

In `app/Http/Controllers/UserController.php` file method `check_update()`, find a user by $name and update it with $email. If not found, create a user with $name, $email and random password

Test method `test_check_or_update_user()`.

---

## Task 8. Mass Delete Users.

In `app/Http/Controllers/UserController.php` file method `destroy()`, delete all users by the array of `$request->users`

Test method `test_mass_delete_users()`.

---

## Task 9. Soft Deletes.

In `app/Http/Controllers/ProjectController.php` file method `destroy()`, change Eloquent statement to still return the soft-deleted records in the list of `$projects`

Test method `test_soft_delete_projects()`.

---

## Task 10. Scopes with Filters.

In `app/Http/Controllers/UserController.php` file method `only_active()`, make the main statement work and to filter records where email_verified_at is not null.

Test method `test_active_users()`.

---

## Task 11. Observers with New Record.

In `app/Http/Controllers/ProjectController.php` file method `store_with_stats()`, create a separate Observer file with an event to perform a +1 in the stats table.

Test method `test_insert_observer()`.

---


DB:
--

Copy the contents of your .env or .env.example file to create a file called .env.testing

Change the value of APP_ENV to 'testing'

remove all of the DB_ entries

Make sure your phpunit.xml has the following lines and uncomment them:

<env name="DB_CONNECTION" value="memory_testing"/>
<env name="DB_DATABASE" value=":memory:"/>

Add the following array to your connections in database.php:

'connections' => [

   'memory_testing' => [
     'driver' => 'sqlite',
     'database' => ':memory:',
     'prefix' => '',
   ],

   ...
Finally, run 
	php artisan optimize:clear
 to clear the caches.

Your unit and feature tests should now be using the in-memory SQLite database, 
while your local should continue using the database configured in .env file.



APP KEY:
--------

php artisan key:generate --env=testing



MySQL error key too long:
------------------------

Solution 1:

In file appServiceProvider.php in function boot() ->   Schema::defaultStringLength(191);

Solution 2:

Inside config/database.php, replace this line for mysql

'engine' => null',

with

'engine' => 'InnoDB ROW_FORMAT=DYNAMIC',


Then retry    php artisan migrate:fresh

