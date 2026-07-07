# 8.4. Insertar y actualizar con relaciones

## El método `save()`

_Eloquent_ ofrece métodos para añadir nuevos modelos a las relaciones. Por ejemplo, si queremos añadir un nuevo `ciclo` a una determinada `fasmilia_profesional`, en lugar de definir manualmente el atributo `familia_id` que le corresponde al modelo `Ciclo` podemos insertar ese `ciclo` utilizando el método `save()` de la relación:

```php 
use App\Models\Ciclo;
use App\Models\FamiliaProfesional;

$ciclo = new Ciclo(['nombre' => 'nuevo ciclo']);

$familiaProfesional = FamiliaProfesional::find(1);

$familiaProfesional->ciclos()->save($ciclo);
```

En este caso, no accedemos a la propiedad dinámica `ciclos` , sino que llamamos al método `ciclos()` para obtener una instancia de la relación. El método `save()` añadirá automáticamente el valor apropiado para el atributo `familia_id` de la nueva instancia del modelo `Ciclo`.

## El método `create()`

También podemos utilizar el método  `create()`, que acepta un array de atributos, para crear un modelo e insertarlo en la base de datos. La diferencia entre `save()` y `create()` es que `save()` acepta una instancia del modelo _Eloquent_ mientras que `create()` acepta un _array_ de PHP:

```php
use App\Models\FamiliaProfesional;

$familiaProfesional = FamiliaProfesional::find(1);

$ciclos = $familiaProfesional->ciclos()->create([
    'amount' => 1000,
]);
```

## Relaciones `belongsTo`

Si quisiéramos asignar un modelo hijo a un modelo padre diferente al que ya tuviera asignado, podríamos utilizar el método `associate()`.

En el siguiente ejemplo, el modelo `FamiliaProfesional` define una relación `belongsTo` con el modelo `User`. El método `associate()` asignará la clave ajena en el modelo hijo:

```php
        $ciclo = Ciclo::find(1);
        $familiaProfesional = FamiliaProfesional::find(1);
        $ciclo->familiaProfesional()->associate($familiaProfesional);
```

Para eliminar un modelo padre de un modelo hijo utilizaremos el método `dissociate()`, que asignará null a la clave ajena:

```php
$ciclo->familiaProfesional()->dissociate();

$ciclo->save();
```

## Relaciones Muchos-a-Muchos

_Eloquent_ también ofrece para trabajar convenientemente con las relaciones **muchos-a-muchos**. Por ejemplo, como hemos visto un `user` puede tener varias `competencias` y cada `competencia` puede estar asociada a varios `users`. Podemos utilizar el método `attach()` para asignar una `competencia` a un `user`, insertando un registro en la tabla intermedia:

```php
use App\Models\User;

$user = User::find(1);

$user->competencias()->attach($competencia_id);
```

Si queremos retirar la competencia de un determinado usuario, utilizaremos el método `detach()`:

```php
// Eliminar el rol de un usuario...
$user->competencias()->detach($competencia_id);

// Eliminar todos los roles de un usuario
$user->competencias()->detach();
```
