
## Installation
```
composer require zamshed/crud-package-laravel-v10
```
## Features

* Controller (with all the code already written)
* Model
* Migration
* Requests
* Routes

## Configuration
Publish the configuration file

This step is required

```
php artisan vendor:publish --provider="zamshed\CrudGenerator\CrudGeneratorServiceProvider"
```

## Usage

After publishing the configuration file just run the below command

```
php artisan crud:generate ModelName
```

Just it, Now all of your `Model Controller, Migration, routes` and `Request` will be created automatically with all the code required for basic crud operations

## Example

```angular2
php artisan crud:generate Post
```
#### PostController.php
```angular2
<?php

namespace App\Http\Controllers;

use App\Http\Requests\PostRequest;
use App\Post;

class PostController extends Controller
{
    public function index()
    {
        $posts = Post::latest()->get();

        return response()->json($posts, 201);
    }

    public function store(PostRequest $request)
    {
        $post = Post::create($request->all());

        return response()->json($post, 201);
    }

    public function show($id)
    {
        $post = Post::findOrFail($id);

        return response()->json($post);
    }

    public function update(PostRequest $request, $id)
    {
        $post = Post::findOrFail($id);
        $post->update($request->all());

        return response()->json($Post, 200);
    }

    public function destroy($id)
    {
        Post::destroy($id);

        return response()->json(null, 204);
    }
}
```

#### Post.php
```angular2
<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    protected $guarded = ['id'];
}
```

#### PostRequest.php
```angular2
<?php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class PostRequest extends FormRequest
{
    public function authorize()
    {
        return true;
    }

    public function rules()
    {
        return [];
    }
}
```

#### posts table migration
```angular2
<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreatepostsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('posts', function (Blueprint $table) {
            $table->bigIncrements('id');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('posts');
    }
}
``` 

#### Routes/api.php
```angular2
Route::apiResource('posts', 'PostController'); 
```
