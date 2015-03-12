Templates get compiled into "instruction vector". That is `parts[]` array in the source.

Consider this template:

`ABC{{foo}}DEF`

After first compilation phase (tokenization) the `parts[]` vector looks like this:

```
parts[0] = "ABC";
parts[1] = "foo";
parts[2] = "DEF";
```
Items on even places in the array are always terminal stings (items: 0, 2). And odd items are "instructions" (item 1 here).

Last compilation phase replace instructions by their compiled versions so final template look like:

```
parts[0] = "ABC";
parts[1] = function() { return context["foo"]; };
parts[2] = "DEF";
```

And execution of the template (instantiation) is a matter of walking through items and outputting them into 'out' string:
  * if part is string - output it as it is.
  * if part is function - call it and output its result.

This is not that effective as a direct JS code generation (jQote, Resig Micro Templates, etc.) but in orders of magnitude better than current (v.0.3) {{mustache}} implementation.