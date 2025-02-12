<!--
%\VignetteEngine{knitr::knitr}
%\VignetteIndexEntry{Introduction to WikidataR}
-->

# WikidataR: the API client library for Wikidata
Wikidata is a wonderful and irreplaceable resource for linked data, containing information on pretty much any subject. If there's a Wikipedia article on it, there's almost certainly a Wikidata item for it.

<code>WikidataR</code> - following the naming scheme of [WikipediR](https://github.com/Ironholds/WikipediR#thanks-and-misc) - is an API client library for Wikidata, written in and accessible from R.

## Items and properties
The two basic component pieces of Wikidata are "items" and "properties". An "item" is a thing - a concept, object or
topic that exists in the real world, such as "Rush". These items each have statements associated with them - for
example, "Rush is an instance of: Rock Band". In that statement, "Rock Band" is a property: a class or trait
that items can hold. Wikidata items are organised as descriptors of the item, in various languages, and references to the properties that that item holds.

## Retrieving specific items or properties
Items and properties are both identified by numeric IDs, prefaced with "Q" in the case of items,
and "P" in the case of properties. WikipediR can be used to retrieve items or properties with specific
ID numbers, using the <code>get\_item</code> and <code>get\_property</code> functions:


```r
#Retrieve an item 
item <- get_item(id = 1)

#Get information about the property of the first claim it has.
first_claim <- get_property(id = names(item$claims)[1])
#Do we succeed? Dewey!
```

These functions are capable of accepting various forms for the ID, including (as examples), "Q100" or "100"
for items, and "Property:P100", "P100" or "100" for properties. They're also vectorised - pass them as many IDs as you want!

## Retrieving randomly-selected items or properties
As well as retrieving specific items or properties, Wikidata's API also allows for the retrieval of *random*
elements. With WikidataR, this can be achieved through:


```r
#Retrieve a random item
rand_item <- get_random_item()

#Retrieve a random property
rand_prop <- get_random_property()
```

These also allow you to retrieve *sets* of random elements - not just one at a time, but say, 50 at a time - by including the "limit" argument:


```r
#Retrieve 42 random items
rand_item <- get_random_item(limit = 42)

#Retrieve 42 random properties
rand_prop <- get_random_property(limit = 42)
```

## Search
Wikidata's search functionality can also be used, either to find items or to find properties. All you need is
a search string (which is run over the names and descriptions of items or properties) and a language code
(since Wikidata's descriptions can be in many languages):


```r
#Find item - find defaults to "en" as a language.
aarons <- find_item("Aaron Halfaker")

#Find a property - also defaults to "en"
first_names <- find_property("first name")
```

The resulting search entries have the ID as a key, making it trivial to then retrieve the full corresponding
items or properties:


```r
#Find item.
all_aarons <- find_item("Aaron Halfaker")

#Grab the ID code for the first entry and retrieve the associated item data.
first_aaron <- get_item(all_aarons[[1]]$id)
```

## Other and future functionality
If you have ideas for other types of useful Wikidata access, the best approach
is to either [request it](https://github.com/TS404/WikidataR/issues) or [add it](https://github.com/TS404/WikidataR/pulls)!
