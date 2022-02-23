# Réalisation d'un API avec Laravel
<img src="http://atomrace.com/blog/wp-content/uploads/2018/04/03-tutorial-laravel-API-REST-restful-app-web-Laravel-5.5.png" width=100%/>

#  1-creation du projet backend

```php
 composer create-project --prefer-dist laravel/laravel Backend
 
```

# 2-Allez dans le fichier .env
```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=backend
DB_USERNAME=root
DB_PASSWORD=

```
# Crrer la base de donnée backend sur phpmyadmin
<img src="https://github.com/EpsilonCoder/Boite-A-idees/blob/main/image-demo/database.PNG"   width=100% /> 

# Creation du model et du controller
Vous pouvez le faire de deux manieres:<br>
La maniere Lumpeul


```php
php artisan make:controller create_idees_tables 

```

```php
php artisan make:model idees
```
Alors y a une maniere Epsilon de faire la meme chose de facon plus efficaces😁😁😁
cette commande te permet de générer le model et le controller en meme temps
```php
php artisan make:model idee -mc
```
# Creations des champs de notre tables idees

<img src="https://github.com/EpsilonCoder/Boite-A-idees/blob/main/image-demo/champs.PNG"   width=100% />
Creer les champs dont vous avez besoin pour votre base de donnée pour notre cas on besoin de(libelle,description,message)

```php
public function up()
    {
        Schema::create('idees', function (Blueprint $table) {
            $table->id();
            $table->string('libelle');
            $table->string('description');
            $table->string('message');
            $table->timestamps();
        });
    }
```
# Migrations des champs sur phpmyadmin

```php
php artisan migrate
```
<img src="https://github.com/EpsilonCoder/Boite-A-idees/blob/main/image-demo/db1.PNG"   width=100% /> 
<img src="https://github.com/EpsilonCoder/Boite-A-idees/blob/main/image-demo/db2.PNG"   width=100% /> 
# ouvrez le fichier IdeeController.php 

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Models\idee;

class IdeeController extends Controller
{
    public function index()
    {
        return idee::All();
    }
}
```

# Parametrer les routes d' acces a l ' API
```php
use App\Http\Controllers\IdeeController;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;

Route::get('/idee', [IdeeController::class, 'index']);

```

# Testons si notre api marche a merveille
Tout d abord ajoutons des entrées dans notre base de données

<img src="https://github.com/EpsilonCoder/Boite-A-idees/blob/main/image-demo/test1db.PNG"   width=100% /> 

# Testons si les urls de notre Api marche  >>> http://127.0.0.1:8000/api/idee
<img src="https://github.com/EpsilonCoder/Boite-A-idees/blob/main/image-demo/verificationapi.PNG"   width=100% /> 

LA PARTIE  BACKEND EST TERMINEE  VOUS POUVEZ VOUS FELICITEZ 

<img src="https://c.tenor.com/-jvrI-Uvb6YAAAAM/noblainer-napoleon-dynamite.gif"   width=100% />








