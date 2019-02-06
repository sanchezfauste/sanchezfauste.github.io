# Maneres de definir una variable
Anem a veure les diferents maneres de definir una variable en JS.

## Variables constants
La definim amb:
```js
const foo = 'bar';
```

No es pot reasignar el valor de la variable foo. És a dir, que no podem fer:
```js
foo = 'new_value';
```

Però sí que podem fer:
```js
const foo = {
  prop1: 'hey',
  prop2: 'woho'
};

foo.prop1 = 'hey que tal';
```

## Utilitzant `var`
Si definim una variable de la següent forma:
```js
var foo = 'bar';
```

Aquesta variable passa a ser global dins del bloc de la funció en que es troba.

## Utilitzant `let`
Si definim una variable utilitzant la paraula reservada `let`, aquesta variable
nomès estarà disponible dins del scope actual. És per això que és preferible
utilitzar `let` en comptes de `var`, si la variable només ha de ser accedida
dins del scope en que es declarada.

[Diferència entre var i let](https://stackoverflow.com/questions/762011/whats-the-difference-between-using-let-and-var-to-declare-a-variable-in-jav/11444416#11444416)


# Com desestructurar un objecte
Si tenim:
```js
const obj = {
  prop1: 'ha',
  prop2: 'he',
  prop3: 'hi',
  prop4: 'ho',
  prop5: 'hu'
};
```

Podem fer el següent:
```js
const { prop1, prop2, prop4 } = obj;
```

I tindrem disponibles les variables `prop1`, `prop2` i `prop4` amb el valor que
tenen dins de l'objecte `obj`.

```js
console.log(prop1);
```

## Valor per defecte en una desestructuració
Podem definir un valor per defecte en una desestructuració, de manera
que si aquella propietat és `undefined`, se li asigni un valor per defecte.

Si tenim:
```js
const obj = {
  prop1: 'ha',
  prop2: 'he',
  prop3: 'hi',
  prop4: 'ho',
  prop5: 'hu'
};

const { prop1, prop7 = 'pa' } = obj;
```

En aquest cas, com que no existeix la propietat `prop7` dins de l'objecte `obj`,
la constant `prop7` tindrà el valor per defecte. En aquest exemple `pa`.

## Obtenir la clau d'un objecte a partir del valor d'una variable
Si per exemple fem:

```js
const foo = 'field1';

const obj = {
    [foo]: 'bar',
    field2: 'fuu'
}
```

L'objecte `obj` quedaria de la següent manera:
```js
{
    field1: 'bar',
    field2: 'fuu'
}
```

