- php artisan migrate
- composer install laravel/ui
- composer require laravel/ui
- php artisan ui bootstrap --auth
- npm install

# Build dev assets
npm run dev

# Or use a watcher to automatically update changes
npm run watch


- Schema::create('links', function (Blueprint $table) {
-           $table->id();
-           $table->string('title');
-           $table->string('url')->unique();
-           $table->text('description');
-           $table->timestamps();
        });

- php artisan migrate
- php artisan make:model --factory Link
* 
* 

- links\database\factories\LinkFactory.php
- $factory->define(Link::class, function (Faker $faker) {
-    return [
-       'title' => substr($faker->sentence(2), 0, -1),
-       'url' => $faker->url,
-       'description' => $faker->paragraph,
-   ];
- });
* 
* 
- php artisan make:seeder LinksTableSeeder
- links\database\seeds\LinksTableSeeder.php
-  public function run()
-   {
-       factory(App\Link::class, 5)->create();
-   }
- php artisan migrate:fresh --seed
- 
- 
- php artisan tinker
>>> \App\Link::first();
=> App\Link {#3060
     id: 1,
     title: "Rerum doloremque",
     url: "http://russel.info/suscipit-et-iste-debitis-beatae-repudiandae-eveniet.html",
     description: "Dolorem voluptas voluptatum voluptatem consequuntur amet dolore odit. Asperiores ullam alias vel soluta ut in. Facere quia et sit laudantium culpa ea possimus.",
     created_at: "2020-04-05 00:44:33",
     updated_at: "2020-04-05 00:44:33",
   }
>>>
- 
- 
- Route::get('/', function () {
-    $links = \App\Link::all();
-   return view('welcome', ['link' => $links]);
- });
- 
- 
- welcome.blade.php
- @foreach ($links as $link)
-    <a href="{{ $link->url }}">{{ $link->title }}</a>
- @endforeach
* 
* 
* 
* tests\Feature\SubmitLinksTest.php
* php artisan make:test SubmitLinksTest
# These tests should give you a high-level overview of what we are going to test:

1. Verify that valid links get saved in the database
2. When validation fails, links are not in the database
3. Invalid URLs are not allowed
4. Validation should fail when the fields are longer than the max:255 validation rule
5. Validation should succeed when the fields are long enough according to max:255.

- use RefreshDatabase;
- 
-    /** @test */
-   function guest_can_submit_a_new_link()
-   {
-      $response = $this->post('/submit', [
-           'title' => 'Example Title',
-           'url' => 'http://example.com',
-           'description' => 'Example description.',
-       ]);
- 
-       $this->assertDatabaseHas('links', [
-           'title' => 'Example Title'
-       ]);
- 
-       $response
-           ->assertStatus(302)
-           ->assertHeader('Location', url('/'));
- 
-       $this
-           ->get('/')
-           ->assertSee('Example Title');
-   }
- 
- 
- 
- 
- 
