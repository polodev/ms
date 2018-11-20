+++
type="post"
toc=true
title= "Basic Multi Auth or Role Based Authentication in Laravel"
date= 2018-11-19T02:06:09+06:00
draft= true
weight= 1
authors= ["Polo dev"]
categories= ['']
tags= ['']
language=['php']
software=['']
tutorial_tags= ['multi auth', 'laravel', 'role based authentication', 'authentication']
tutorialTypes=['tutorials']
keyword= "multi auth, laravel, role based authentication, authentication"
description= "In this video I have shown how to add multi auth / role based authentication in laravel"
skill_level=["beginner"]
available_skill_level=["beginner", "intermediate", "advanced"]
series=[]
series_weight=1
+++

# Installing laravel
I kept my app name is study-app
~~~bash
composer create-project  laravel/laravel study-app
~~~

# scaffolding laravel built in auth

~~~bash
php artisan make:auth
~~~

# create `Role` model and migration for  `roles` table

~~~bash
php artisan make:model Role -m
~~~
Here `-m` flag for making migration

# add name and slug field in migration file

{{< fd "database/migrations/2018_11_15_132208_create_roles_table.php" >}}

~~~php
Schema::create('roles', function (Blueprint $table) {
    $table->increments('id');
    $table->string('name');
    $table->string('slug')->unique();
    $table->timestamps();
});

~~~

# add `role_id` foreign key column in `users` table. I kept unique `user_name` field in my users table

{{< fd "database/migrations/2018_11_15_132208_create_users_table.php" >}}
~~~php
$table->integer('role_id')->default(2);
$table->string('user_name')->unique();
~~~

Here I gave default `role_id` 2. since 2 will be all general user.

# configure .env file for database connection

{{< fd ".env" >}}
~~~bash
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=studyapp
DB_USERNAME=root
DB_PASSWORD=
~~~

# now migrate our code

~~~bash
php artisan migrate
~~~


# Adding `spatie/laravel-sluggable` in our project
we have a slug field in `roles` and `user_name` in `users` table. to make it unique slug we will use
[spatie/laravel-sluggable](https://packagist.org/packages/spatie/laravel-sluggable) package.

~~~bash
composer require spatie/laravel-sluggable
~~~

now we will configure our `User` and `Role` model for  `spatie/laravel-sluggable` and one to many relationship

{{< fd "app/User.php" >}}

~~~bash
use Spatie\Sluggable\HasSlug;
use Spatie\Sluggable\SlugOptions;

class User extends Authenticatable
{
    use HasSlug;

    protected $fillable = [
        'name', 'email', 'password', 'user_name', 'role_id'
    ];

    public function getSlugOptions() : SlugOptions
    {
        return SlugOptions::create()
            ->generateSlugsFrom('name')
            ->saveSlugsTo('user_name');
    }
    public function role ()
    {
      return $this->belongsTo(Role::class);
    }

}
~~~

{{< fd "app/Role.php" >}}

~~~bash
use Spatie\Sluggable\HasSlug;
use Spatie\Sluggable\SlugOptions;

class Role extends Model
{
  use HasSlug;
  protected $guarded = [];

  public function getSlugOptions() : SlugOptions
  {
      return SlugOptions::create()
          ->generateSlugsFrom('name')
          ->saveSlugsTo('slug');
  }
  public function users()
  {
    return $this->hasMany(User::clss);
  }
}

~~~


# now create a seeder for our roles table and seeding roles to database

~~~bash
php artisan make:seeder RolesTableSeeder
~~~


roles table seeder go will following content in my case

{{< fd "database/seeds/RolesTableSeeder.php" >}}
~~~php
use Illuminate\Database\Seeder;
use App\Role;

class RolesTableSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
      Role::truncate();
      $roles = [
        [
          'name' => 'Admin',
          'id' => 1,
        ],
        [
          'name' => 'Student',
          'id' => 2,
        ]
      ];
      foreach ($roles as $role) {
        Role::create([
          'name' => $role['name']
        ]);
      }
    }
}

~~~

Since we use `spatie/laravel-sluggable` we don't need to pass `slug`  field. Before seeding, we truncating our role
table first since we will track role id. In my case admin will have id 1 and student will have id 2.

code `RolesTableSeeder` in our `DatabaseSeeder` `run` method

{{< fd "database/seeds/DatabaseSeeder.php" >}}
~~~php
public function run()
{
  $this->call(RolesTableSeeder::class);
}
~~~

now we can seed our database using artisan command

~~~bash
php artisan db:seed
~~~













