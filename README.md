# Coco
is a [CoffeeScript](http://coffeescript.org) dialect that aims to be more radical and practical.

### Additions

- `.=` / `[=`

        $ coco -bpe 'location.href.=replace /^http:/, "https:"'
        location.href = location.href.replace(/^http:/, "https:");
        $ coco -bpe 'a[=0]'
        a = a[0];


- `instanceof []`

        $ coco -bpe 'A instanceof [B, C]'
        A instanceof B || A instanceof C;


- `import` / `import all`

  An infix operator that copies properties from left to right and returns the right operand.

        $ coco -bpe 'x import y import all z'
        var __import = function(obj, src, own){
          if (own) own = Object.prototype.hasOwnProperty;
          for (var key in src) if (!own || own.call(src, key)) obj[key] = src[key];
          return obj;
        };
        __import(__import(x, y, true), z);


- `it`

  Represents the first argument of the current function (like in Groovy).
  Available only when the function omits argument declarations.

        $ coco -bpe 'I = -> it'
        var I;
        I = function(it){
          return it;
        };


- `switch`-`case`-`default`

  Same as switch-when-else in original, but requires less indentation.

        $ coco -bpe 'switch x
        case 1, 2 then 3
        case 4    then 5
        default 6'
        switch (x) {
        case 1:
        case 2:
          3;
          break;
        case 4:
          5;
          break;
        default:
          6;
        }


- `<[ quoted words ]>`

        $ coco -bpe '<[ quoted words ]>'
        ["quoted", "words"];


- `void`

  A literal that compiles to `void 0`.


### Incompatibilities

- `yes`/`no`/`on`/`off` are not reserved. Define your own or just use `true`/`false`.
- `undefined` is not reserved.
- switch-when-else has been replaced by switch-case-default.
