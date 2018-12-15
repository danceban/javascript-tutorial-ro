# Comparatii

Din matematica cunoastem mai multi operatori de comparatie:

- Mai mare/mai mic decat: <code>a &gt; b</code>, <code>a &lt; b</code>.
- Mai mare/mai mic sau egal decat: <code>a &gt;= b</code>, <code>a &lt;= b</code>.
- Verificarea egalitatii se scrie ca `a == b` (acordati atentie semnului dublu de egalitate `=`. Un singur semn de egalitate `a = b` ar insemna atribuire).
- Inegalitate. In matematica aceasta se noteaza ca <code>&ne;</code>, in JavaScript se noteaza ca atribuire cu un semn de exclamare inaintea acestuia: <code>a != b</code>.

## Rezultatul este Boolean

La fel ca ceilalti operatori, o comparatie returneaza o valoare. Valoarea returnata este de tip boolean.

- `true` -- inseamna "da", "corect" sau "adevarat".
- `false` -- inseamna "nu", "gresit" sau "fals".

De exemplu:

```js run
alert( 2 > 1 );  // true (corect)
alert( 2 == 1 ); // false (gresit)
alert( 2 != 1 ); // true (corect)
```

Rezultatul unei comparatii poate fi atribuit unei variabile, ca si oricare alta valoare:

```js run
let result = 5 > 4; // atribuie rezultatul comparatiei
alert( result ); // true
```

## Comparatiile pentru String

Pentru a vedea care string este mai mare decat celalalt, asa numita ordine dupa dictionar sau lexicografica este aplicata.

Cu alte cuvinte, string-urile sunt comparate litera cu litera.

De exemplu:

```js run
alert( 'Z' > 'A' ); // true
alert( 'Glow' > 'Glee' ); // true
alert( 'Bee' > 'Be' ); // true
```

Algoritmul compararii a doua string-uri este simplu:

1. Se compara primele caractere ale ambelor string-uri.
2. Daca primul este mai mare (sau mai mic), atunci primul string este mai mare (sau mai mic) decat celalalt.
3. In caz contrar, daca aceste doua caractere sunt egale, incepe compararea caracterelor secunde dupa aceeasi metoda.
4. Operatiunea se repeta pana la sfarsitul string-urilor.
5. Daca ambele string-uri se sfarsesc simultan, atunci acestea sunt egale. In caz contrar, string-ul mai lung este mai mare.

In exemplul de mai sus, comparatia `'Z' > 'A'` ofera rezultatul la primul pas al algoritmului. 

String-urile `"Glow"` si `"Glee"` sunt comparate caracter cu caracter:

1. `G` este la fel ca `G`.
2. `l` este la fel ca `l`.
3. `o` este mai mare ca `e`. Aici compararea sfarseste. Primul string este mai mare.

```smart header="Nu dupa ordinea dictionarului, ci dupa ordinea Unicode"
Algoritmul de comparare prezentat mai sus este aproape echivalent cu cel utilizat in dictionare si cartile telefonice. Dar nu este exact la fel.

De exemplu, litera majuscula `"A"` nu este egala cu litera minuscula `"a"`. Care este mai mare? De fapt, minuscula `"a"` este mai mare. Dece? Pentru ca acest caracter are un index mai mare in internal encoding table (Unicode). O sa ne intoarcem mai detaliat la acestea in capitolul <info:string>.
```

## Comparison of different types

When compared values belong to different types, they are converted to numbers.

For example:

```js run
alert( '2' > 1 ); // true, string '2' becomes a number 2
alert( '01' == 1 ); // true, string '01' becomes a number 1
```

For boolean values, `true` becomes `1` and `false` becomes `0`, that's why:

```js run
alert( true == 1 ); // true
alert( false == 0 ); // true
```

````smart header="A funny consequence"
It is possible that at the same time:

- Two values are equal.
- One of them is `true` as a boolean and the other one is `false` as a boolean.

For example:

```js run
let a = 0;
alert( Boolean(a) ); // false

let b = "0";
alert( Boolean(b) ); // true

alert(a == b); // true!
```

From JavaScript's standpoint that's quite normal. An equality check converts using the numeric conversion (hence `"0"` becomes `0`), while `Boolean` conversion uses another set of rules.
````

## Strict equality

A regular equality check `==` has a problem. It cannot differ `0` from `false`:

```js run
alert( 0 == false ); // true
```

The same thing with an empty string:

```js run
alert( '' == false ); // true
```

That's because operands of different types are converted to a number by the equality operator `==`. An empty string, just like `false`, becomes a zero.

What to do if we'd like to differentiate `0` from `false`?

**A strict equality operator `===` checks the equality without type conversion.**

In other words, if `a` and `b` are of different types, then `a === b` immediately returns `false` without an attempt to convert them.

Let's try it:

```js run
alert( 0 === false ); // false, because the types are different
```

There also exists a "strict non-equality" operator `!==`, as an analogy for `!=`.

The strict equality check operator is a bit longer to write, but makes it obvious what's going on and leaves less space for errors.

## Comparison with null and undefined

Let's see more edge cases.

There's a non-intuitive behavior when `null` or `undefined` are compared with other values.


For a strict equality check `===`
: These values are different, because each of them belongs to a separate type of its own.

    ```js run
    alert( null === undefined ); // false
    ```

For a non-strict check `==`
: There's a special rule. These two are a "sweet couple": they equal each other (in the sense of `==`), but not any other value.

    ```js run
    alert( null == undefined ); // true
    ```

For maths and other comparisons `< > <= >=`
: Values `null/undefined` are converted to a number: `null` becomes `0`, while `undefined` becomes `NaN`.

Now let's see funny things that happen when we apply those rules. And, what's more important, how to not fall into a trap with these features.

### Strange result: null vs 0

Let's compare `null` with a zero:

```js run
alert( null > 0 );  // (1) false
alert( null == 0 ); // (2) false
alert( null >= 0 ); // (3) *!*true*/!*
```

Yeah, mathematically that's strange. The last result states that "`null` is greater than or equal to zero". Then one of the comparisons above must be correct, but they are both false.

The reason is that an equality check `==` and comparisons `> < >= <=` work differently. Comparisons convert `null` to a number, hence treat it as `0`. That's why (3) `null >= 0` is true and (1) `null > 0` is false.

On the other hand, the equality check `==` for `undefined` and `null` is defined such that, without any conversions, they equal each other and don't equal anything else. That's why (2) `null == 0` is false.

### An incomparable undefined

The value `undefined` shouldn't participate in comparisons at all:

```js run
alert( undefined > 0 ); // false (1)
alert( undefined < 0 ); // false (2)
alert( undefined == 0 ); // false (3)
```

Why does it dislike a zero so much? Always false!

We've got these results because:

- Comparisons `(1)` and `(2)` return `false` because `undefined` gets converted to `NaN`. And `NaN` is a special numeric value which returns `false` for all comparisons.
- The equality check `(3)` returns `false`, because `undefined` only equals `null` and no other value.

### Evade problems

Why did we observe these examples? Should we remember these peculiarities all the time? Well, not really. Actually, these tricky things will gradually become familiar over time, but there's a solid way to evade any problems with them.

Just treat any comparison with `undefined/null` except the strict equality `===` with exceptional care.

Don't use comparisons `>= > < <=` with a variable which may be `null/undefined`, unless you are really sure what you're doing. If a variable can have such values, then check for them separately.

## Summary

- Comparison operators return a logical value.
- Strings are compared letter-by-letter in the "dictionary" order.
- When values of different types are compared, they get converted to numbers (with the exclusion of a strict equality check).
- Values `null` and `undefined` equal `==` each other and do not equal any other value.
- Be careful when using comparisons like `>` or `<` with variables that can occasionally be `null/undefined`. Making a separate check for `null/undefined` is a good idea.
