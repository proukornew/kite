# KiTE template defintion language #

Here is an example of KiTE template:
```
<ul>
  {{#contacts}}
    <li><b>{{firstName}}</b> <i>{{lastName}}</i></li>
  {{/contacts}}
</ul>
```

When given by the following JS data:
```
{ 
  contacts:[
    { firstName: "Ernest", lastName:"Hemingway", role:"writer" },
    { firstName: "Scott", lastName:"Fitzgerald", role:"writer" },
    { firstName: "Charlie",lastName:"Chaplin", role:"actor" }
  ]
}
```

the template will be instantiated as this list:
  * **Ernest** _Hemingway_
  * **Scott** _Fitzgerald_
  * **Charlie** _Chaplin_
## KiTE directives (tags) ##

KiTE uses three types of directives:
  * Variables with optional formatting
  * Block sections
  * Conditional sections

### Variables ###

Variable placeholder is indicated by enclosing variable name in '{{' and '}}' brackets. There are two forms of variable declarations:
  * `{{varname}}`
> simple form, value of the variable will be inserted as it is and without any kind of escapment;

  * `{{varname|formatname}}`
> value insertion with the name of formatting function separated by the pipe '`|`' symbol . Formatting function will receive variable value and shall produce string - formatted representation of the value for the output.

While instantiating the template the KiTE will try to find the _varname_ in current context (current JavaScript object). If the name is not found (undefined) then no result is produced.

### Block sections ###

Blocks designate sections that will repeat enclosed content one or more times.  There are two forms of block declarations:
  * `{{#varname}}` ... section text ... `{{/varname}}`
> simple block.

  * `{{#varname}}` ... section text ... `{{^varname}}` ... alternative text ... `{{/varname}}`
> block with alternative section that will be rendered if the _varname_ resolves to an empty array or to _false_.

If the _varname_ resolves to an array then the block gets repeated in output for each element of the array. The context of the block will be set to the current item in each iteration. If the value of _varname_ is an object then it will be set as a current context and content will be rendered once for the object.

### Conditional sections ###

Conditional sections get rendered if their expression evaluates to _true_ in runtime. There are three forms of conditional sections:
  * `{{? expr }}` ... section text ... `{{/?}}`
> simple conditional, _expr_ here is a valid JavaScript expression. If it evaluates to _true_ the section will be rendered. If result of the expression is _false_ then the section will be skipped.

  * `{{? expr }}` ... section text ... `{{^?}}` ... "else" section `{{/?}}`
> conditional section with "else" part that gets rendered if the _expr_ evaluates to _false_.

  * `{{? expr1 }}`  section1 `{{? expr2}}`  section2 ...  `{{^?}}` ... "else" section `{{/?}}`
> that are "chained" conditional sections. They follow statndard if-elseif-elseif-else-fi logic used in almost all programming languages.

There are two predefined variables `at` and `set` that you can use in conditional expressions:

  * `set` - reference to current array of records.
  * `at` - current index in that array.

These two variables allow to express conditions like
"at last record", "at first record", "at even/odd record".
Conditions like "if previous record has that" and "if next record has that" are also feasible with the change.
