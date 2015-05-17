#The DIC Format
Firstly, let's define some terms:

Rant uses a plaintext, hierarchical file format called DIC for storing dictionary data in units called **tables**, which collectively form a **dictionary**. A table is a collection of **entries** of a specific category, such as nouns, verbs or first names. Table entries can be comprised of one or more **terms**, which make up the contents of that entry. An entry can also have metadata associated with it, such as **classes** that help describe what the entry means, as well as pronunciation, weight, and syllable data. More about these later.

The terms of an entry are defined using **subtypes**, which are an ordered collection of names that correspond to each term in an entry. Rantionary's `noun` table, for example, has two subtypes: `singular` and `plural`, and entries are written with the `singular` term first, and the `plural` term second (e.g. `horse/horses`).

##Structure of a table
There are two main parts to every DIC file: A **header** and a **corpus**.

###Header
The header defines the name, subtypes, types, and special class behaviors in the table. Headers are made up on **directives**, which are commands that start with a name prefixed by a `#` symbol, followed by a list of arguments.

Below is an example of a typical header found in a noun table:
```
#name noun
#subs singular plural
#hidden nsfw
```
This header says that the table is called `noun`, contains the subtypes `singular` and `plural`, and hides entries marked with the `nsfw` class from general query results. *(Note: The NSFW class is a special class that is hidden by default. It is only included here for the sake of example. You can leave it out and it will still be hidden.)*

All header directives are optional, except for `#name`.

###Corpus
The corpus contains the table's entries. All entries start with one of the following symbols:
* `>`: Verbose entry. All terms must be typed exactly as they are to appear.
* `>>`: Diff entry. All terms after the first term are [Diffmark](http://github.com/TheBerkin/Diffmark) patterns relative to the first term.

Each entry must contain as many terms as there are subtypes, with the terms in the same order as the subtypes are defined.

Examples of entries from various tables:
```
#name adverb

> quickly
> slowly
> silently
> noisily
```
```
#name noun
#subs singular plural

> dog/dogs
> cat/cats
> horse/horses
> elephant/elephants
```
```
#name verb
#subs simple ing ed s er pp nom

@ Verbose
> destroy/destroying/destroyed/destroys/destroyer/destroyed/destruction
@ Diff
>> destroy/ing/ed/s/er/ed/--uction
```

####Entry properties
Entries can have various kinds of metadata associated with them through properties.

Properties take on the following form:
```
| property-name property-value
```
A property affects the last entry that was defined before it. So, an entry with properties looks like this:
```
> apple/apples
  | class food red
```
As a general guideline, properties should be indented one unit further than their associated entry.

The above example demonstrates a usage of one of the most important properties, the `class` property. This property allows any number of arbitary strings to be associated with an entry. This is useful for categorizing table entries by purpose or meaning (such as having an `animal` class for nouns that describe animals), which can then be used to filter query results to specific classes in patterns.

####List of properties
The following properties are supported:
* `| class <names...>`: Assigns the specified classes to an entry.
* `| pron <p1>/...`: Defines pronunciation data for an entry, with each term's pronunciation separated by a forward slash.
* `| weight <weight>`: Assigns the specified weight to an entry. If omitted, the default weight is 1.

Additionally, any type name can be used as a property name to assign a class from a specific type to an entry.
For example, a `color` table could assign `primary` and `secondary` classes from a type named `kind` by doing this:
```
#name color
#type kind "primary secondary" ""

> red
  | kind primary
> green
  | kind primary
> blue
  | kind primary
> yellow
  | kind secondary
> magenta
  | kind secondary
> cyan
  | kind secondary
```

##Directives

Directive arguments are delimited by spaces. If an argument needs a space in it, surround the argument with double-quotes.

###Header directives
* `#name <table-name>`: Sets the table name.
* `#subs <sub1> [sub2...]`: Sets the subtypes used by the table. If omitted, a single subtype called `default` is used.
* `#hidden <class>`: Hides the specified class. 
* `#type <type-name> <type-classes> <required-on>`: Sets a type on entries to which the specified class filter applies. This means that every affected entry must contain one (and only one) of the specified type classes in order for Rant to load the table.
  * `<type-name>`: The name of the type.
  * `<type-classes>`: A space-separated list of classes associated with the type.
  * `<required-on>`: A list of classes that the type is required for. If an entry contains one of the specified classes, it must contain one of the type classes as well. This can be set to `*` to apply to all entries, or an empty string to apply to none.

###Corpus directives
* `class <add/remove> <classes...>`: Queues or dequeues one or more classes to be added to all following entries.

##Comments
A comment can be inserted anywhere in a DIC file by inserting the `@` symbol, which will turn the rest of the line into a comment.
```
@ This is a comment
>> run/ning/-an/s/ner/-an/ning @ Inline comment
```

#Standard subtype formats
##Verbs
English verbs currently have seven established subtypes: `simple` `ing` `ed` `s` `er` `pp`, and `nom`.

|Name|Description|Example|
|---|---|---|
|simple|Imperative form|forgive|
|ing|Gerund form|forgiving|
|ed|Simple past|forgave|
|s|Simple third-person present|forgives|
|er|Agent noun|forgiver|
|pp|Past participle|forgiven|
|nom|Nominalization|forgiveness|

#Standard classes
##Connotations
There are specific classes used to reflect the general connotation of a dictionary entry.

##Classes
|Class Name|Meaning|Example|
|:--------:|-------|-------|
|`_bad`|Most will be offended by any usage. Objectively negative.|shitty|
|`_taboo`|Generally regarded as negative, but not objectively negative.|vulgar|
|`_sensitive`|Only appropriate in very specific contexts.|moist|
|`_neutral`|Neither perceived as good nor bad. A regular word.|cylindrical|
|`_good`|Generally regarded as positive, cheerful, or constructive.|beautiful|
