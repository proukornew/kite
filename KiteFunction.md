# Introduction #

All code of the KiTE engine is contained inside the `kite()` function.
There is an optional collection of formatting functions that is available at kite.formatters (JS object - map of name/function pairs)

# The kite() function #

The kite() function can be invoked in two different ways:

  1. `kite( template:string, data:object|array]): string`
> compiles and executes the template. Returns generated text (string)

  1. `kite( template:string): function`
> compiles the template. Returns function - compiled version of the template for later invocation with data.

Where:
  * _template_, string, is a source of the template or if it starts from '#' is treated as an id of `<script type="text/x-kite" id=...>` element that contains template source.
  * _data_, object or array, that contains data that will be used for template "instantiation".

Examples of calls:

```
var text = kite("#sample", data); // load and execute the template
```

```
var template = kite("#sample"); // compile the template
var text = template(data);      // execute the template - produce 
                                // text from the data.
```