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


# Essayons de Creer notre Fontend avec le framework React afin de recuperer les donnees de notre API

```js
npx create-react-app EpsilonFront
cd EpsilonFront
npm start
```
De nombreux projets sur le web doivent s’interfacer avec une API REST à un certain stade de leur développement. Axios est un client HTTP léger basé sur le service $http et est similaire à l’API native JavaScript Fetch.
Axios est basé sur Promise, ce qui vous permet de profiter des avantages d’async de JavaScript et await pour un code asynchrone plus lisible.

# installons axios
```
npm install axios
```
Ouvrez le ficher App.js 
<img src="https://github.com/EpsilonCoder/Boite-A-idees/blob/main/image-demo/react1.PNG"   width=100% /> 

# Recuperer les données de notre API

```js
import './App.css';
import { useState, useEffect } from "react";
import axios from "axios";
function App() {
  const [idees, setIdees,] = useState([])

  useEffect(() => {
    async function Ajouter() {
      try {
        const epsilon = await axios.get("http://127.0.0.1:8000/api/idee")
        console.log(epsilon.data)
        setIdees(epsilon.data)
      } catch (error) {
        console.log(error)
      }

    }
    Ajouter()

  }, [])
  return (
    <div className="App">
      <h1>Je suis sur react afin de recuperer les données de mon API</h1>
      {
        idees.map((idee, i) => {
          return (
            <h4 key={i}>{idee.message}</h4>
          )
        })
      }
    </div>
  );
}

export default App;


```

# Verifions si on parvient a recuperer les messages a partir de notre API Laravel

<img src="https://github.com/EpsilonCoder/Boite-A-idees/blob/main/image-demo/react2.PNG"   width=100% /> 

# 😒 Etes vous d'accord que pour l instant vous pouvez que faire l affichage des données de l api.
# Ne serait t'il pas interessant de pouvoir ajouter des informations , modifier ou meme de supprimer des informations.

# 🤦‍♀️🤦‍♀️🤦‍♀️ On fait comment ?
Pas de panique c est tres simple faut tout juste revenir sur ton application laravel 
ensuite specifier des routes et des methodes afin de permettre de permettre a l api de
d accepter des requettes post,put,delete.

Ouvrez le model => idee.php

<img src="https://github.com/EpsilonCoder/Boite-A-idees/blob/main/image-demo/post1.PNG"   width=100% /> 
ajouter => protected $fillable = [
        "libelle",
        "description",
        "message",
    ];
    
C'est pour spécifier les champs de notre base de donnée.
```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class idee extends Model
{
    use HasFactory;
    protected $fillable = [
        "libelle",
        "description",
        "message",
    ];
}

```
Rendez vous dans votre controlleur pour ajouter une methode post a votre controlleur.
<img src="https://github.com/EpsilonCoder/Boite-A-idees/blob/main/image-demo/post2.PNG"   width=100% />
La methode d'ajout
```php
 public function store(Request $request)
    {
        return idee::create($request->all());
    }
```

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

    public function store(Request $request)
    {
        return idee::create($request->all());
    }
}

```





















