# Modules

## Create new module

### Scaffold
```bash
$ php artisan asgard:module:scaffold
```

You will be asked the following questions:
* What is the module name ?

   This is in the following format `jjsoft-ar/yourmodulename` all lowercase. Do not use dashes in the module name

* Do you wish to use Doctrine or Eloquent ?

  This is to know what kind of Entities to generate. Use **`Eloquent`**

* Enter your desired entities

  You can enter as many as you like, when you're done leave empty and the next question will come up.

* Enter you desired value objects

  Again, you can enter as many as you like, when you're done leave empty.

Before seeing the newly created module on the **_admin dashboard_** you will have to give you access to the new module. Go the the **User > Roles** view and edit the Admin role to give you permission to the new module.

### GIT
The last step required is to add you module in the `Modules/.gitignore` file as following:

```git
!YourModuleName
```

This will tell git to ignore everything in the Modules folder except for your modules.

## ENTITIES (MODELS)

### Create Migration:

```bash
$ php artisan module:make-migration <Model> <YourModuleName>
```

Going to `Modules/<:YourModuleName>/Database/Migrations` and edit de migrations files

### Create Model (Entitie):

```bash
$ php artisan module:make-model <Model> <YourModuleName>
```
#### Secondary DB definition
To use secondary database on model open de model class `Modules/<YourModuleName>/Entities/<NewEntitie>.php`, and add this:

```php
class <NewEntitie> extends Model
{
    protected $connection = 'bs_siges';
    public $timestamps = false;
```

#### Fillable definition

Complete `$fillable` array with all fields that you want to fill on a form:

```php
  protected $fillable = [
      '<field1>',
      ...
      '<fieldN'>
  ];
```
#### Validation definition
To are functionality of form validation add this array:
```php
  /**
   * Validation rules
   *
   * @var array
   */
  public static $rules = [
      'id_banco' => 'required',
      'nombre' => 'required'
  ];
```
#### Cast definition

You cast attributes in Eloquent by adding a protected `$casts` array to your model.
```php
  /**
   * The attributes that should be casted to native types.
   *
   * @var array
   */
  protected $casts = [
      'id_banco' => 'integer',
      'nombre' => 'string',
      'is_admin' => 'boolean',
  ];
``` 
## CONTROLLERS

### Create Controller:
If you need to create a controller without scaffold
```bash
$ php artisan module:make-controller <Model>Controller <YourModuleName>
```

If you use secundary database `bs_siges` then to work propietly you need edit 

`/Modules/Core/Repositories/Eloquent/EloquentBaseRepository.php`

Search each occurrence of `->orderBy('created_at', 'DESC')` and delete, to work fine. Because this database doesn't work to timestamp.

### Create Request
To add functionalytie at form to fields validation, you need to create two Request for each Model you created:

`/Modules/<YourModuleName>/Http/Requests/Create<YourModelName>Request.php`
`/Modules/<YourModuleName>/Http/Requests/Update<YourModelName>Request.php`

And their code, for example `Banco` Entitie:
```php
<?php namespace Modules\Sistema\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;
use Modules\Sistema\Entities\Banco;

class CreateBancoRequest extends FormRequest {

	/**
	 * Determine if the user is authorized to make this request.
	 *
	 * @return bool
	 */
	public function authorize()
	{
		return true;
	}

	/**
	 * Get the validation rules that apply to the request.
	 *
	 * @return array
	 */
	public function rules()
	{
		return Banco::$rules;
	}

}
```

Then you need edit the Controller class on `store` and `update` functions.
```php
...
/**
     * Store a newly created resource in storage.
     *
     * @param  CreateBancoRequest $request
     * @return Response
     */
    public function store(CreateBancoRequest $request)

...

/**
     * Update the specified resource in storage.
     *
     * @param  Banco $banco
     * @param  UpdateBancoRequest $request
     * @return Response
     */
    public function update(Banco $banco, UpdateBancoRequest $request)
```