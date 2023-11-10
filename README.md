Please, just give me a shitton of examples that I can
copy/paste...maybe later I'll read the manual.

Data for this is generated from here:
https://github.com/taylormonacelli/anythingflorida/blob/master/data.cypher

in repo:
https://github.com/taylormonacelli/anythingflorida


* [list nodes and relations](#list-nodes-and-relations)
* [list distinct node types](#list-distinct-node-types)
* [list products with the same name](#list-products-with-the-same-name)
* [list products](#list-products)
* [list all products and for each product list all if its urls](#list-all-products-and-for-each-product-list-all-if-its-urls)
* [list product names](#list-product-names)
* [list relations](#list-relations)
* [WRONG: list relations, not just CONTAINS and show relation properties](#wrong-list-relations-not-just-contains-and-show-relation-properties)
* [for each product, count the number of duplicates that exist for it](#for-each-product-count-the-number-of-duplicates-that-exist-for-it)
* [how many products have duplicates](#how-many-products-have-duplicates)
* [suppose I were to make Thai Curry, then what ingredients do I need?](#suppose-i-were-to-make-thai-curry-then-what-ingredients-do-i-need)
* [list products ordered by type](#list-products-ordered-by-type)
* [list products that I’ve not yet assiged a type to](#list-products-that-ive-not-yet-assiged-a-type-to)
* [something about urls](#something-about-urls)
* [list Product nodes and their properties](#list-product-nodes-and-their-properties)
* [WRONG: count the products that have a brand](#wrong-count-the-products-that-have-a-brand)
* [list products that don’t yet have a brand associated](#list-products-that-dont-yet-have-a-brand-associated)
* [list the brand of the product too](#list-the-brand-of-the-product-too)
* [list products whose names contain non-alphanum chars](#list-products-whose-names-contain-non-alphanum-chars)
* [fetch all urls for all products](#fetch-all-urls-for-all-products)
* [WRONG: fetch all urls for all products except empty urls](#wrong-fetch-all-urls-for-all-products-except-empty-urls)
* [fetch all products whose urls list is not empty](#fetch-all-products-whose-urls-list-is-not-empty)
* [list Product properties](#list-product-properties)
* [list properties assigned to the PURCHASE-AT relation](#list-properties-assigned-to-the-purchase-at-relation)
* [list properties across all entities sorted case insensitively](#list-properties-across-all-entities-sorted-case-insensitively)
* [WRONG: list properties across all entities](#wrong-list-properties-across-all-entities)
* [FIXED: list properties across all entities](#fixed-list-properties-across-all-entities)
* [list products that have at least one store associated with each](#list-products-that-have-at-least-one-store-associated-with-each)
* [list products that don’t have a store associated with them](#list-products-that-dont-have-a-store-associated-with-them)
* [list products that don’t have a store associated with them, but limit to 10](#list-products-that-dont-have-a-store-associated-with-them-but-limit-to-10)
* [list the entity type its assocted with](#list-the-entity-type-its-assocted-with)
* [list distinct entities](#list-distinct-entities)
* [list CONTAINS relations](#list-contains-relations)
* [list uniquely all CONTAINS relations](#list-uniquely-all-contains-relations)
* [FIXED: list products that have urls that are photos in google drive](#fixed-list-products-that-have-urls-that-are-photos-in-google-drive)
* [FIXED: list relations, not just CONTAINS and show relation properties](#fixed-list-relations-not-just-contains-and-show-relation-properties)
* [what store should I visit to make a recipe?](#what-store-should-i-visit-to-make-a-recipe)
* [where should I shop for ingredients for Chicken Teriyaki?](#where-should-i-shop-for-ingredients-for-chicken-teriyaki)
* [suppose I would like to make 2 recipes, then what stores do I need to visit?](#suppose-i-would-like-to-make-2-recipes-then-what-stores-do-i-need-to-visit)
* [I want to make a recipe and travel to the fewest number of stores](#i-want-to-make-a-recipe-and-travel-to-the-fewest-number-of-stores)
* [WRONG: some recipes point to the same product multiple times by mistake](#wrong-some-recipes-point-to-the-same-product-multiple-times-by-mistake)
* [find products whose type contains vegetable](#find-products-whose-type-contains-vegetable)
* [find products whose type contains peas](#find-products-whose-type-contains-peas)
# list nodes and relations

``` example
MATCH (n) RETURN n
;
```

Results:

``` example
{'n': {'name': 'Charity Ferreira'}}
{'n': {'urls': ['https://christieathome.com/'], 'name': 'christieathome'}}
{'n': {'urls': ['https://cleananddelicious.com/'], 'name': 'Dani Spies'}}
{'n': {'urls': ['https://www.simplyrecipes.com/recipes/tomatillo_salsa_verde/'], 'name': 'Elise Bauer'}}
{'n': {'urls': ['https://www.cookerru.com/about-me/'], 'name': 'Elle'}}
{'n': {'urls': ['https://www.youtube.com/c/HanaAsbrink', 'https://www.instagram.com/hanaasbrink/'], 'name': 'Hana Asbrink'}}
{'n': {'urls': [''], 'name': 'J. Kenji López-Alt'}}
{'n': {'urls': ['https://drivemehungry.com/zaru-soba-cold-soba-noodles/'], 'name': 'Jamie'}}
{'n': {'urls': ['https://www.youtube.com/@JoshuaWeissman'], 'name': 'Joshua Weissman'}}
{'n': {'ytb': 'https://www.youtube.com/@Marionskitchen', 'name': 'Marionskitchen'}}
# ...truncated to 10 for brevity
```

# list distinct node types

``` example
MATCH (n)
RETURN DISTINCT labels(n) AS objectType
ORDER BY objectType
;
```

Results:

``` example
{'objectType': []}
{'objectType': ['Person']}
{'objectType': ['Product']}
{'objectType': ['Recipe']}
{'objectType': ['Store']}
```

# list products with the same name

This reveals that I need to clean up duplicates.

``` example
MATCH (p:Product)
WITH p.name AS productName, COLLECT(p) AS products
WHERE SIZE(products) > 1
RETURN productName, products
;
```

Results:

``` example
{'productName': 'Apple Cider Vinegar', 'products': [{'name': 'Apple Cider Vinegar', 'type': 'Apple Cider Vinegar'}, {'name': 'Apple Cider Vinegar', 'type': 'Vinegar'}]}
{'productName': 'Red Curry Paste', 'products': [{'urls': ['https://www.google.com/search?sca_esv=579549787&sxsrf=AM9HkKlJ1akktSB6XfxzBxrRxM_VM-9vxA:1699158988679&q=aroy-d+red+curry+paste', 'https://www.youtube.com/watch?v=GC7ccNKatVU'], 'name': 'Red Curry Paste', 'type': 'Curry Paste', 'brand': 'Aroy D'}, {'urls': ['https://www.youtube.com/watch?v=d6YbVqqcR4w', 'https://www.google.com/search?sca_esv=579549787&sxsrf=AM9HkKn2yZQ4RvsQT3gvUhOknWGy59VJhQ:1699159240924&q=mae+ploy+red+curry+paste&tbm=isch&source=lnms&sa=X&sqi=2&ved=2ahUKEwiwjuO3hayCAxVdHzQIHSe3B3oQ0pQJegQICRAB&biw=1440&bih=758&dpr=2'], 'name': 'Red Curry Paste', 'type': 'Curry Paste', 'brand': 'Mae Ploy'}, {'name': 'Red Curry Paste', 'type': 'Curry Paste'}]}
{'productName': 'Avocado Oil', 'products': [{'name': 'Avocado Oil', 'type': 'Avocado Oil'}, {'name': 'Avocado Oil', 'type': 'Avocado Oil'}]}
{'productName': 'Cooking Oil', 'products': [{'name': 'Cooking Oil', 'type': 'Cooking Oil'}, {'name': 'Cooking Oil', 'type': 'Oil'}]}
{'productName': 'Heavy Cream', 'products': [{'name': 'Heavy Cream', 'type': 'Cream'}, {'name': 'Heavy Cream', 'type': 'Cream'}]}
{'productName': 'Dried Thai shrimp paste', 'products': [{'name': 'Dried Thai shrimp paste', 'type': 'Shrimp Paste'}, {'urls': ['https://www.eatingthaifood.com/thai-nam-prik-kapi-recipe/'], 'comments': ['thaiShrimpPasteComment1', 'thaiShrimpPasteComment2'], 'name': 'Dried Thai shrimp paste', 'google': ['shrimp paste kapi OR gabi OR gkabi'], 'type': 'Condiment'}]}
{'productName': 'Thai Eggplant', 'products': [{'name': 'Thai Eggplant', 'type': 'Thai Eggplant'}, {'name': 'Thai Eggplant', 'type': 'Thai Eggplant'}]}
{'productName': 'Extra Firm Tofu', 'products': [{'name': 'Extra Firm Tofu', 'type': 'Tofu'}, {'name': 'Extra Firm Tofu', 'type': 'Tofu'}]}
{'productName': 'Fish Sauce', 'products': [{'name': 'Fish Sauce', 'type': 'Fish Sauce', 'brand': 'Red Boat Premium'}, {'urls': ['https://www.google.com/search?client=emacs&sca_esv=579549787&sxsrf=AM9HkKm8epAD3ytpi0GWZEym4PGKNYwfHA:1699157904119&q=Squid+Fish+Sauce&tbm=isch&source=lnms&sa=X&ved=2ahUKEwiY96q6gKyCAxXiJzQIHVrbD78Q0pQJegQIChAB&biw=1440&bih=758&dpr=2'], 'name': 'Fish Sauce', 'type': 'Fish Sauce', 'brand': 'Squid'}]}
{'productName': 'Yellow Curry Paste', 'products': [{'urls': ['https://www.safeway.com/shop/product-details.960076294.html', 'https://youtu.be/GC7ccNKatVU?t=696'], 'name': 'Yellow Curry Paste', 'type': 'Curry Paste', 'brand': 'Mae Ploy'}, {'urls': ['https://www.safeway.com/shop/product-details.960076294.html'], 'name': 'Yellow Curry Paste', 'type': 'Curry Paste'}]}
{'productName': 'Maple Syrup', 'products': [{'name': 'Maple Syrup', 'type': 'Maple Syrup'}, {'name': 'Maple Syrup', 'type': 'Maple Syrup'}]}
{'productName': 'Red Onion', 'products': [{'name': 'Red Onion', 'type': 'Onion'}, {'name': 'Red Onion', 'type': 'Red Onion'}]}
{'productName': 'White Onion', 'products': [{'name': 'White Onion', 'type': 'Onion'}, {'name': 'White Onion', 'type': 'Onion'}]}
{'productName': 'Toasted Sesame Oil', 'products': [{'name': 'Toasted Sesame Oil', 'type': 'Sesame Oil'}, {'name': 'Toasted Sesame Oil', 'type': 'Sesame Oil'}]}
```

# list products

``` example
MATCH (p:Product)
RETURN p
;
```

Results:

``` example
{'p': {'name': 'A.1. Sauce', 'type': 'A.1. Sauce'}}
{'p': {'name': 'Allspice', 'type': 'Allspice'}}
{'p': {'name': 'Almond Milk', 'type': 'Almond Milk'}}
{'p': {'name': 'Almonds - bulk roasted or raw -- whichever is cheaper', 'type': 'Almonds'}}
{'p': {'name': 'Angkor Cambodian Food Paste Lemongrass', 'type': 'Food Paste'}}
# ...truncated to 5 for brevity
```

# list all products and for each product list all if its urls

``` example
MATCH (p:Product)
WITH p.name AS productName, p.urls AS productUrls
UNWIND productUrls AS url
RETURN productName, url
;
```

Results:

``` example
{'productName': 'Aroy-D Coconut Milk', 'url': 'https://www.google.com/search?sca_esv=581110607&sxsrf=AM9HkKlvxPZkhbmImtTjzpzoOo0bggx5gA:1699596383961&q=aroy-d+coconut+milk&tbm=isch&source=lnms&sa=X&sqi=2&ved=2ahUKEwjq0uj14biCAxW7GTQIHT6CDx0Q0pQJegQIDRAB&biw=1440&bih=754&dpr=2'}
{'productName': 'Aroy-D Coconut Milk', 'url': 'https://www.templeofthai.com/food/coconut-milk/aroy-d-large'}
{'productName': 'Aroy-D Coconut Milk', 'url': 'https://www.zhicayfoods.com/products/aroy-d-coconut-milk-original'}
{'productName': 'Red Curry Paste', 'url': 'https://www.google.com/search?sca_esv=579549787&sxsrf=AM9HkKlJ1akktSB6XfxzBxrRxM_VM-9vxA:1699158988679&q=aroy-d+red+curry+paste'}
{'productName': 'Red Curry Paste', 'url': 'https://www.youtube.com/watch?v=GC7ccNKatVU'}
# ...truncated to 5 for brevity
```

# list product names

``` example
MATCH (p:Product)
RETURN p.name
ORDER BY toLower(p.name)
;
```

Results:

``` example
{'p.name': 'A.1. Sauce'}
{'p.name': 'ACT Restoring Mouthwash'}
{'p.name': 'Adams Peanut Butter'}
{'p.name': 'Allspice'}
{'p.name': 'Almond Milk'}
# ...truncated to 5 for brevity
```

# list relations

``` example
MATCH ()-[r]-()
RETURN DISTINCT type(r) AS relationType
ORDER BY relationType
;
```

Results:

``` example
{'relationType': 'CONTAINS'}
{'relationType': 'CREATED'}
{'relationType': 'IS_THE_SAME_AS'}
{'relationType': 'PURCHASE_AT'}
{'relationType': 'RECOMMENDS'}
```

# WRONG: list relations, not just CONTAINS and show relation properties

Gotcha! This is wrong. Notice we're missing the is-the-same-as relation.

``` example
MATCH ()-[r]-()
UNWIND keys(r) AS propertyNames
RETURN DISTINCT type(r) AS type, propertyNames AS propertyName
ORDER BY type, propertyName
;
```

Results:

``` example
{'type': 'CONTAINS', 'propertyName': 'quantity'}
{'type': 'CONTAINS', 'propertyName': 'urls'}
{'type': 'PURCHASE_AT', 'propertyName': 'aisle'}
{'type': 'PURCHASE_AT', 'propertyName': 'note'}
{'type': 'PURCHASE_AT', 'propertyName': 'url'}
{'type': 'PURCHASE_AT', 'propertyName': 'urls'}
{'type': 'RECOMMENDS', 'propertyName': 'urls'}
```

# for each product, count the number of duplicates that exist for it

``` example
MATCH (p:Product)
WITH p.name AS productName, COLLECT(p) AS products
WHERE SIZE(products) > 1
RETURN productName, COUNT(products) AS duplicateCount
;
```

Results:

``` example
{'productName': 'Apple Cider Vinegar', 'duplicateCount': 1}
{'productName': 'Red Curry Paste', 'duplicateCount': 1}
{'productName': 'Avocado Oil', 'duplicateCount': 1}
{'productName': 'Cooking Oil', 'duplicateCount': 1}
{'productName': 'Heavy Cream', 'duplicateCount': 1}
{'productName': 'Dried Thai shrimp paste', 'duplicateCount': 1}
{'productName': 'Thai Eggplant', 'duplicateCount': 1}
{'productName': 'Extra Firm Tofu', 'duplicateCount': 1}
{'productName': 'Fish Sauce', 'duplicateCount': 1}
{'productName': 'Yellow Curry Paste', 'duplicateCount': 1}
{'productName': 'Maple Syrup', 'duplicateCount': 1}
{'productName': 'Red Onion', 'duplicateCount': 1}
{'productName': 'White Onion', 'duplicateCount': 1}
{'productName': 'Toasted Sesame Oil', 'duplicateCount': 1}
```

# how many products have duplicates

In other words how much work do I have to do to cleanup my data?

``` example
MATCH (p:Product)
WITH p.name AS productName, COUNT(p) AS productCount
WHERE productCount > 1
RETURN COUNT(productCount) AS totalDuplicateProducts
;
```

Results:

``` example
{'totalDuplicateProducts': 14}
```

# suppose I were to make Thai Curry, then what ingredients do I need?

``` example
MATCH (r:Recipe {name: 'Vegan Thai Red Curry'})-[:CONTAINS]->(p:Product)
MATCH (p)-[:PURCHASE_AT]->(s:Store)
RETURN s.name AS StoreName, COLLECT(DISTINCT p.name) AS Ingredients
;
```

Results:

``` example
{'StoreName': 'Madison Co-op', 'Ingredients': ['Cumin seeds', 'Coriander seeds', 'White Peppercorns']}
{'StoreName': 'Safeway', 'Ingredients': ['Shallots', 'Cilantro roots', 'Lemongrass']}
{'StoreName': "Trader Joe's", 'Ingredients': ['Garlic']}
{'StoreName': 'Uwajimaya', 'Ingredients': ['Galangal']}
```

# list products ordered by type

``` example
MATCH (p:Product)-[:PURCHASE_AT]->(s:Store)
RETURN p.name AS ProductName, s.name AS StoreName, p.type as Type
ORDER BY toLower(p.type)
;
```

Results:

``` example
{'ProductName': 'A.1. Sauce', 'StoreName': 'dummy place holder', 'Type': 'A.1. Sauce'}
{'ProductName': 'Allspice', 'StoreName': 'Central Co-op', 'Type': 'Allspice'}
{'ProductName': 'Almond Milk', 'StoreName': "Trader Joe's", 'Type': 'Almond Milk'}
{'ProductName': 'Almonds - bulk roasted or raw -- whichever is cheaper', 'StoreName': "Trader Joe's", 'Type': 'Almonds'}
{'ProductName': 'Artichoke Hearts', 'StoreName': 'Safeway', 'Type': 'Artichokes'}
{'ProductName': 'Asparagus', 'StoreName': 'dummy place holder', 'Type': 'Asparagus'}
{'ProductName': 'Asparagus', 'StoreName': "Trader Joe's", 'Type': 'Asparagus'}
{'ProductName': 'Avocado Oil', 'StoreName': 'Whole Foods', 'Type': 'Avocado Oil'}
{'ProductName': 'Avocados (not in bag stupid)', 'StoreName': "Trader Joe's", 'Type': 'Avocados'}
{'ProductName': 'Johnsons Creamy Baby Oil', 'StoreName': 'dummy place holder', 'Type': 'Baby Oil'}
# ...truncated to 10 for brevity
```

# list products that I've not yet assiged a type to

``` example
MATCH (p:Product)
WHERE p.type IS NULL
RETURN p.name
;
```

Results:

``` example
```

# something about urls

``` example
MATCH (r:Recipe)-[c:CONTAINS]->(p:Product)
WHERE id(p) IS NULL
RETURN r.name AS RecipeName, c.quantity AS Quantity, c.urls AS RecipeUrls
;
```

Results:

``` example
```

# list Product nodes and their properties

``` example
MATCH (n:Product) RETURN n
;
```

Results:

``` example
{'n': {'name': 'A.1. Sauce', 'type': 'A.1. Sauce'}}
{'n': {'name': 'Allspice', 'type': 'Allspice'}}
{'n': {'name': 'Almond Milk', 'type': 'Almond Milk'}}
{'n': {'name': 'Almonds - bulk roasted or raw -- whichever is cheaper', 'type': 'Almonds'}}
{'n': {'name': 'Angkor Cambodian Food Paste Lemongrass', 'type': 'Food Paste'}}
{'n': {'name': 'Apple Cider Vinegar', 'type': 'Apple Cider Vinegar'}}
{'n': {'name': 'Apples', 'type': 'Fruit'}}
{'n': {'urls': ['https://www.google.com/search?sca_esv=581110607&sxsrf=AM9HkKlvxPZkhbmImtTjzpzoOo0bggx5gA:1699596383961&q=aroy-d+coconut+milk&tbm=isch&source=lnms&sa=X&sqi=2&ved=2ahUKEwjq0uj14biCAxW7GTQIHT6CDx0Q0pQJegQIDRAB&biw=1440&bih=754&dpr=2', 'https://www.templeofthai.com/food/coconut-milk/aroy-d-large', 'https://www.zhicayfoods.com/products/aroy-d-coconut-milk-original'], 'name': 'Aroy-D Coconut Milk', 'type': 'Coconut Milk'}}
{'n': {'urls': ['https://www.google.com/search?sca_esv=579549787&sxsrf=AM9HkKlJ1akktSB6XfxzBxrRxM_VM-9vxA:1699158988679&q=aroy-d+red+curry+paste', 'https://www.youtube.com/watch?v=GC7ccNKatVU'], 'name': 'Red Curry Paste', 'type': 'Curry Paste', 'brand': 'Aroy D'}}
{'n': {'name': 'Artichoke Hearts', 'type': 'Artichokes'}}
# ...truncated to 10 for brevity
```

# WRONG: count the products that have a brand

I can't get this to do what I expect.

``` example
// MATCH (p:Product)
// OPTIONAL MATCH (p)-[:PURCHASE_AT]->(s:Store)
// WHERE p.brand = ''
// RETURN p.name AS ProductName, p.type AS Type, COALESCE(p.brand, '') AS Brand, COLLECT(DISTINCT s.name) AS AvailableAtStores
// ORDER BY toLower(Brand);

// MATCH (p:Product)
// OPTIONAL MATCH (p)-[:PURCHASE_AT]->(s:Store)
// WHERE p.brand IS NOT NULL AND p.brand <> ''
// RETURN p.name AS ProductName, p.type AS Type, COALESCE(p.brand, '') AS Brand, COLLECT(DISTINCT s.name) AS AvailableAtStores
// ORDER BY toLower(Brand);

// MATCH (p:Product)
// OPTIONAL MATCH (p)-[:PURCHASE_AT]->(s:Store)
// WHERE exists(p.brand) AND trim(p.brand) <> ''
// RETURN p.name AS ProductName, p.type AS Type, COALESCE(p.brand, '') AS Brand, COLLECT(DISTINCT s.name) AS AvailableAtStores
// ORDER BY toLower(Brand);

// Neo.ClientError.Statement.SyntaxError
// The property existence syntax `... exists(variable.property)` is no longer supported. Please use `variable.property IS NOT NULL` instead. (line 3, column 11 (offset: 77))
// "    WHERE exists(p.brand) AND trim(p.brand) <> ''"

// MATCH (p:Product)
// OPTIONAL MATCH (p)-[:PURCHASE_AT]->(s:Store)
// WHERE p.brand IS NOT NULL AND trim(p.brand) <> ''
// RETURN p.name AS ProductName, p.type AS Type, COALESCE(p.brand, '') AS Brand, COLLECT(DISTINCT s.name) AS AvailableAtStores
// ORDER BY toLower(Brand);

// MATCH (p:Product)
// OPTIONAL MATCH (p)-[:PURCHASE_AT]->(s:Store)
// WHERE p.brand IS NOT NULL AND TRIM(p.brand) <> ''
// RETURN p.name AS ProductName, p.type AS Type, COALESCE(p.brand, '') AS Brand, COLLECT(DISTINCT s.name) AS AvailableAtStores
// ORDER BY toLower(Brand);

// MATCH (p:Product)
// OPTIONAL MATCH (p)-[:PURCHASE_AT]->(s:Store)
// WHERE p.brand IS NOT NULL AND TRIM(p.brand) <> ''
// RETURN p.name AS ProductName, p.type AS Type, COALESCE(p.brand, '') AS Brand, COLLECT(DISTINCT s.name) AS AvailableAtStores
// ORDER BY toLower(p.brand);
//
// In a WITH/RETURN with DISTINCT or an aggregation, it is not possible to access variables declared before the WITH/RETURN: p (line 5, column 22 (offset: 270))
// "    ORDER BY toLower(p.brand);"

// MATCH (p:Product)
// OPTIONAL MATCH (p)-[:PURCHASE_AT]->(s:Store)
// WHERE p.brand IS NOT NULL AND TRIM(p.brand) <> ''
// WITH p, COLLECT(DISTINCT s.name) AS AvailableAtStores
// RETURN p.name AS ProductName, p.type AS Type, COALESCE(p.brand, '') AS Brand, AvailableAtStores
// ORDER BY toLower(p.brand);

// MATCH (p:Product)
// OPTIONAL MATCH (p)-[:PURCHASE_AT]->(s:Store)
// WHERE COALESCE(p.brand, '') <> ''
// WITH p, COLLECT(DISTINCT s.name) AS AvailableAtStores
// RETURN p.name AS ProductName, p.type AS Type, COALESCE(p.brand, '') AS Brand, AvailableAtStores
// ORDER BY toLower(p.brand);

// MATCH (p:Product)
// OPTIONAL MATCH (p)-[:PURCHASE_AT]->(s:Store)
// WHERE COALESCE(TRIM(p.brand), '') <> ''
// WITH p, COLLECT(DISTINCT s.name) AS AvailableAtStores
// RETURN p.name AS ProductName, p.type AS Type, COALESCE(p.brand, '') AS Brand, AvailableAtStores
// ORDER BY toLower(TRIM(p.brand));

// MATCH (p:Product)
// OPTIONAL MATCH (p)-[:PURCHASE_AT]->(s:Store)
// WHERE COALESCE(p.brand, '') <> '' AND TRIM(p.brand) <> ''
// WITH p, COLLECT(DISTINCT s.name) AS AvailableAtStores
// RETURN p.name AS ProductName, p.type AS Type, COALESCE(p.brand, '') AS Brand, AvailableAtStores
// ORDER BY toLower(TRIM(p.brand));

// MATCH (p:Product)
// OPTIONAL MATCH (p)-[:PURCHASE_AT]->(s:Store)
// WHERE NOT (p.brand IS NULL OR TRIM(p.brand) = '')
// WITH p, COLLECT(DISTINCT s.name) AS AvailableAtStores
// RETURN p.name AS ProductName, p.type AS Type, COALESCE(p.brand, '') AS Brand, AvailableAtStores
// ORDER BY toLower(TRIM(p.brand));

// cypher how to filter items whose properties are zero length string

// MATCH (n:Node)
// WHERE ALL(prop IN keys(n) WHERE length(n[prop]) = 0)
// RETURN n;

// MATCH (n:Product)
// WHERE ALL(prop IN keys(n) WHERE length(n[prop]) = 0)
// RETURN n;

MATCH (p:Product)
WHERE p.Brand IS NULL OR p.Brand = ""
RETURN COUNT(p) AS productCount
;
```

Results:

``` example
{'productCount': 553}
```

# list products that don't yet have a brand associated

``` example
MATCH (p:Product)
WITH count(p) AS TotalProducts,
     sum(CASE WHEN p.brand IS NOT NULL AND p.brand <> '' THEN 1 ELSE 0 END) AS ProductsWithBrand,
     sum(CASE WHEN p.brand IS NULL OR p.brand = '' THEN 1 ELSE 0 END) AS ProductsWithoutBrand
RETURN TotalProducts, ProductsWithBrand, ProductsWithoutBrand
;
```

Results:

``` example
{'TotalProducts': 553, 'ProductsWithBrand': 6, 'ProductsWithoutBrand': 547}
```

# list the brand of the product too

``` example
MATCH (p:Product)
OPTIONAL MATCH (p)-[:PURCHASE_AT]->(s:Store)
RETURN p.name AS ProductName, p.type AS Type, COALESCE(p.brand, '') AS Brand, COLLECT(DISTINCT s.name) AS AvailableAtStores
ORDER BY toLower(Brand)
;
```

Results:

``` example
{'ProductName': 'A.1. Sauce', 'Type': 'A.1. Sauce', 'Brand': '', 'AvailableAtStores': ['dummy place holder']}
{'ProductName': 'Allspice', 'Type': 'Allspice', 'Brand': '', 'AvailableAtStores': ['Central Co-op']}
{'ProductName': 'Almond Milk', 'Type': 'Almond Milk', 'Brand': '', 'AvailableAtStores': ["Trader Joe's"]}
{'ProductName': 'Almonds - bulk roasted or raw -- whichever is cheaper', 'Type': 'Almonds', 'Brand': '', 'AvailableAtStores': ["Trader Joe's"]}
{'ProductName': 'Angkor Cambodian Food Paste Lemongrass', 'Type': 'Food Paste', 'Brand': '', 'AvailableAtStores': ['QFC']}
{'ProductName': 'Apple Cider Vinegar', 'Type': 'Apple Cider Vinegar', 'Brand': '', 'AvailableAtStores': []}
{'ProductName': 'Apples', 'Type': 'Fruit', 'Brand': '', 'AvailableAtStores': ['Safeway']}
{'ProductName': 'Aroy-D Coconut Milk', 'Type': 'Coconut Milk', 'Brand': '', 'AvailableAtStores': []}
{'ProductName': 'Artichoke Hearts', 'Type': 'Artichokes', 'Brand': '', 'AvailableAtStores': ['Safeway']}
{'ProductName': 'Asparagus', 'Type': 'Asparagus', 'Brand': '', 'AvailableAtStores': ["Trader Joe's", 'dummy place holder']}
# ...truncated to 10 for brevity
```

# list products whose names contain non-alphanum chars

List products whose names contain non-alphanum sorted randomly to
prevent boredom while cleaning data.

``` example
MATCH (p:Product)
WHERE p.name =~ ".*[^a-zA-Z0-9 ].*"
RETURN p.name AS ProductName
ORDER BY RAND()
;
```

Results:

``` example
{'ProductName': 'Blueberries - frozen uponina bag'}
{'ProductName': 'Tortilla - Flour large diameter Don Pancho or Safeway brand'}
{'ProductName': 'Chili Pepper, Chipotle, Ground'}
{'ProductName': 'Ramen Noodles - Dry'}
{'ProductName': 'Chicken Broth - 32 Oz'}
{'ProductName': 'Grapes, grape shaped'}
{'ProductName': 'chicken - electric - rotisserie'}
{'ProductName': 'Pinot Grigio - Dry White Wine'}
{'ProductName': 'Tomato Sauce - 29 oz can'}
{'ProductName': 'Chicken Tenderloins / Rubber Duckies / 1 BAG ONLY'}
# ...truncated to 10 for brevity
```

# fetch all urls for all products

``` example
MATCH (p:Product)
RETURN p.name AS ProductName, p.urls AS URLs
;
```

Results:

``` example
{'ProductName': 'A.1. Sauce', 'URLs': None}
{'ProductName': 'Allspice', 'URLs': None}
{'ProductName': 'Almond Milk', 'URLs': None}
{'ProductName': 'Almonds - bulk roasted or raw -- whichever is cheaper', 'URLs': None}
{'ProductName': 'Angkor Cambodian Food Paste Lemongrass', 'URLs': None}
{'ProductName': 'Apple Cider Vinegar', 'URLs': None}
{'ProductName': 'Apples', 'URLs': None}
{'ProductName': 'Aroy-D Coconut Milk', 'URLs': ['https://www.google.com/search?sca_esv=581110607&sxsrf=AM9HkKlvxPZkhbmImtTjzpzoOo0bggx5gA:1699596383961&q=aroy-d+coconut+milk&tbm=isch&source=lnms&sa=X&sqi=2&ved=2ahUKEwjq0uj14biCAxW7GTQIHT6CDx0Q0pQJegQIDRAB&biw=1440&bih=754&dpr=2', 'https://www.templeofthai.com/food/coconut-milk/aroy-d-large', 'https://www.zhicayfoods.com/products/aroy-d-coconut-milk-original']}
{'ProductName': 'Red Curry Paste', 'URLs': ['https://www.google.com/search?sca_esv=579549787&sxsrf=AM9HkKlJ1akktSB6XfxzBxrRxM_VM-9vxA:1699158988679&q=aroy-d+red+curry+paste', 'https://www.youtube.com/watch?v=GC7ccNKatVU']}
{'ProductName': 'Artichoke Hearts', 'URLs': None}
# ...truncated to 10 for brevity
```

# WRONG: fetch all urls for all products except empty urls

This is not possible.

WRONG: fetch all urls for all products, but then don't show urls if
product doesn't have any

``` example
MATCH (p:Product)
RETURN p.name AS ProductName, p.urls AS URLs
;
```

Results:

``` example
{'ProductName': 'A.1. Sauce', 'URLs': None}
{'ProductName': 'Allspice', 'URLs': None}
{'ProductName': 'Almond Milk', 'URLs': None}
{'ProductName': 'Almonds - bulk roasted or raw -- whichever is cheaper', 'URLs': None}
{'ProductName': 'Angkor Cambodian Food Paste Lemongrass', 'URLs': None}
{'ProductName': 'Apple Cider Vinegar', 'URLs': None}
{'ProductName': 'Apples', 'URLs': None}
{'ProductName': 'Aroy-D Coconut Milk', 'URLs': ['https://www.google.com/search?sca_esv=581110607&sxsrf=AM9HkKlvxPZkhbmImtTjzpzoOo0bggx5gA:1699596383961&q=aroy-d+coconut+milk&tbm=isch&source=lnms&sa=X&sqi=2&ved=2ahUKEwjq0uj14biCAxW7GTQIHT6CDx0Q0pQJegQIDRAB&biw=1440&bih=754&dpr=2', 'https://www.templeofthai.com/food/coconut-milk/aroy-d-large', 'https://www.zhicayfoods.com/products/aroy-d-coconut-milk-original']}
{'ProductName': 'Red Curry Paste', 'URLs': ['https://www.google.com/search?sca_esv=579549787&sxsrf=AM9HkKlJ1akktSB6XfxzBxrRxM_VM-9vxA:1699158988679&q=aroy-d+red+curry+paste', 'https://www.youtube.com/watch?v=GC7ccNKatVU']}
{'ProductName': 'Artichoke Hearts', 'URLs': None}
# ...truncated to 10 for brevity
```

# fetch all products whose urls list is not empty

``` example
MATCH (p:Product)
WHERE p.urls IS NOT NULL AND SIZE(p.urls) > 0
RETURN p.name AS ProductName, p.urls AS URLs
;
```

Results:

``` example
{'ProductName': 'Aroy-D Coconut Milk', 'URLs': ['https://www.google.com/search?sca_esv=581110607&sxsrf=AM9HkKlvxPZkhbmImtTjzpzoOo0bggx5gA:1699596383961&q=aroy-d+coconut+milk&tbm=isch&source=lnms&sa=X&sqi=2&ved=2ahUKEwjq0uj14biCAxW7GTQIHT6CDx0Q0pQJegQIDRAB&biw=1440&bih=754&dpr=2', 'https://www.templeofthai.com/food/coconut-milk/aroy-d-large', 'https://www.zhicayfoods.com/products/aroy-d-coconut-milk-original']}
{'ProductName': 'Red Curry Paste', 'URLs': ['https://www.google.com/search?sca_esv=579549787&sxsrf=AM9HkKlJ1akktSB6XfxzBxrRxM_VM-9vxA:1699158988679&q=aroy-d+red+curry+paste', 'https://www.youtube.com/watch?v=GC7ccNKatVU']}
{'ProductName': 'Baked Tofu', 'URLs': ['https://www.google.com/search?sca_esv=579179295&sxsrf=AM9HkKnAjZCHvxR_pYrcL19p0l0Qjk1Zjg:1699032994034&q=Baked+Tofu&tbm=isch&source=lnms&sa=X&ved=2ahUKEwiwrsiQr6iCAxXHHjQIHVGWDjkQ0pQJegQIDRAB&biw=1440&bih=758&dpr=2']}
{'ProductName': 'Bonito Flakes', 'URLs': ['https://chefjacooks.com/en/wprm_print/7506', 'https://www.amazon.com/Kaneso-Tokuyou-Hanakatsuo-Bonito-Flakes/dp/B0052BGLMS', 'https://www.google.com/search?sca_esv=577907868&sxsrf=AM9HkKmChgo0Ktu9IlnGTSWuzmK5YqQsiQ:1698696041201&q=Bonito+Flakes&tbm=isch&source=lnms&sa=X&ved=2ahUKEwjy0Pfwx56CAxUBODQIHey0BwcQ0pQJegQIDhAB&biw=1440&bih=758&dpr=2']}
{'ProductName': 'brownie clif bar', 'URLs': ['https://shop.clifbar.com/collections/clif-bar']}
{'ProductName': 'Buckwheat Soba Nodles', 'URLs': ['https://www.amazon.com/gp/product/B00101YEBO', 'https://veggiekinsblog.com/2020/01/13/vegan-zaru-soba/']}
{'ProductName': 'Candlenuts', 'URLs': ['https://www.google.com/search?client=emacs&sca_esv=580758711&sxsrf=AM9HkKmwGL8OAnRZ8-PJqCLp_VU9-SlJfg:1699507479310&q=Candlenuts&tbm=isch&source=lnms&sa=X&ved=2ahUKEwiwsOPclraCAxVVETQIHabkCi0Q0pQJegQIDRAB&biw=1440&bih=754&dpr=2#imgrc=7uHbBToP7aPjSM']}
{'ProductName': 'Chili Sauce', 'URLs': ['https://thewoksoflife.com/wp-content/uploads/2020/07/chili-oil-recipe-18.jpg', 'https://www.amazon.com/%E8%80%81%E5%B9%B2%E5%A6%88%E9%A6%99%E8%BE%A3%E8%84%86%E6%B2%B9%E8%BE%A3%E6%A4%92-Spicy-Chili-Crisp-7-41/dp/B07VHKTTR3/ref=asc_df_B07VHKTTR3/?tag=hyprod-20&linkCode=df0&hvadid=642112947349&hvpos=&hvnetw=g&hvrand=12580253979732381700&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9061293&hvtargid=pla-1951193779579&psc=1', 'https://www.google.com/search?sca_esv=580857096&sxsrf=AM9HkKmLh9FDQ0x5jNY12kJCSSbwO6Q3FA:1699539552211&q=thai+and+true+hot+chili&tbm=isch&source=lnms&sa=X&ved=2ahUKEwiJ8KiajreCAxWqAjQIHaMBDKYQ0pQJegQIDBAB&biw=1440&bih=754&dpr=2#imgrc=KDhcVOHe9yNjkM', 'https://photos.google.com/photo/AF1QipMQPtIdU1_m3SkgBWs5RcE2QXFs2OnbbJAdaG9M']}
{'ProductName': 'Dashi', 'URLs': ['https://en.wikipedia.org/wiki/Dashi']}
{'ProductName': 'Eucerin Creme Daily Moisturizing Skin Calming', 'URLs': ['https://photos.google.com/photo/AF1QipM2_uDtc-2Uc7XriFP3k4H0L_DxcvxVeYvgUlpG', 'https://photos.google.com/photo/AF1QipM2_uDtc-2Uc7XriFP3k4H0L_DxcvxVeYvgUlpG']}
# ...truncated to 10 for brevity
```

# list Product properties

A product may or may not have any one of these properties.

``` example
MATCH (n:Product)
WITH DISTINCT keys(n) AS propertyNamesList
UNWIND propertyNamesList AS propertyName
RETURN DISTINCT propertyName
ORDER BY toLower(propertyName)
;
```

Results:

``` example
{'propertyName': 'bb_says'}
{'propertyName': 'brand'}
{'propertyName': 'comments'}
{'propertyName': 'detail'}
{'propertyName': 'google'}
{'propertyName': 'googleSearch'}
{'propertyName': 'manufacturer'}
{'propertyName': 'name'}
{'propertyName': 'note'}
{'propertyName': 'photos'}
{'propertyName': 'type'}
{'propertyName': 'urls'}
```

# list properties assigned to the PURCHASE-AT relation

``` example
MATCH ()-[r:PURCHASE_AT]->()
UNWIND keys(r) AS propertyNames
RETURN DISTINCT propertyNames
;
```

Results:

``` example
{'propertyNames': 'urls'}
{'propertyNames': 'aisle'}
{'propertyNames': 'url'}
{'propertyNames': 'note'}
```

# list properties across all entities sorted case insensitively

``` example
MATCH (n)
UNWIND keys(n) AS propertyName
RETURN DISTINCT propertyName
ORDER BY toLower(propertyName)
;
```

Results:

``` example
{'propertyName': 'bb_says'}
{'propertyName': 'brand'}
{'propertyName': 'comments'}
{'propertyName': 'detail'}
{'propertyName': 'google'}
{'propertyName': 'google_maps'}
{'propertyName': 'googleSearch'}
{'propertyName': 'manufacturer'}
{'propertyName': 'name'}
{'propertyName': 'note'}
{'propertyName': 'notes'}
{'propertyName': 'origin'}
{'propertyName': 'photos'}
{'propertyName': 'type'}
{'propertyName': 'urls'}
{'propertyName': 'ytb'}
```

# WRONG: list properties across all entities

Item 'list properties of all entities including relations' fixes this.

``` example
MATCH (n)
UNWIND keys(n) AS propertyName
RETURN DISTINCT propertyName
;
```

Results:

``` example
{'propertyName': 'name'}
{'propertyName': 'urls'}
{'propertyName': 'ytb'}
{'propertyName': 'origin'}
{'propertyName': 'notes'}
{'propertyName': 'google_maps'}
{'propertyName': 'type'}
{'propertyName': 'brand'}
{'propertyName': 'bb_says'}
{'propertyName': 'photos'}
{'propertyName': 'manufacturer'}
{'propertyName': 'note'}
{'propertyName': 'google'}
{'propertyName': 'comments'}
{'propertyName': 'googleSearch'}
{'propertyName': 'detail'}
```

# FIXED: list properties across all entities

Get properties of nodes and then get properties of relation entities and
then aggregate them into one list.

``` example
MATCH (n)
UNWIND keys(n) AS propertyName
RETURN DISTINCT 'Node' AS type, propertyName
ORDER BY type, propertyName

UNION

MATCH ()-[r]-()
UNWIND keys(r) AS propertyNames
RETURN DISTINCT type(r) AS type, propertyNames AS propertyName
ORDER BY type, propertyName
;
```

Results:

``` example
{'type': 'Node', 'propertyName': 'bb_says'}
{'type': 'Node', 'propertyName': 'brand'}
{'type': 'Node', 'propertyName': 'comments'}
{'type': 'Node', 'propertyName': 'detail'}
{'type': 'Node', 'propertyName': 'google'}
{'type': 'Node', 'propertyName': 'googleSearch'}
{'type': 'Node', 'propertyName': 'google_maps'}
{'type': 'Node', 'propertyName': 'manufacturer'}
{'type': 'Node', 'propertyName': 'name'}
{'type': 'Node', 'propertyName': 'note'}
{'type': 'Node', 'propertyName': 'notes'}
{'type': 'Node', 'propertyName': 'origin'}
{'type': 'Node', 'propertyName': 'photos'}
{'type': 'Node', 'propertyName': 'type'}
{'type': 'Node', 'propertyName': 'urls'}
{'type': 'Node', 'propertyName': 'ytb'}
{'type': 'CONTAINS', 'propertyName': 'quantity'}
{'type': 'CONTAINS', 'propertyName': 'urls'}
{'type': 'PURCHASE_AT', 'propertyName': 'aisle'}
{'type': 'PURCHASE_AT', 'propertyName': 'note'}
{'type': 'PURCHASE_AT', 'propertyName': 'url'}
{'type': 'PURCHASE_AT', 'propertyName': 'urls'}
{'type': 'RECOMMENDS', 'propertyName': 'urls'}
```

# list products that have at least one store associated with each

``` example
MATCH (p:Product)-[:PURCHASE_AT]->(s:Store)
RETURN p.name AS ProductName, s.name AS StoreName, p.type as Type
;
```

Results:

``` example
{'ProductName': 'Gochugaru', 'StoreName': 'Amazon', 'Type': 'Gochugaru'}
{'ProductName': 'Cleanser - Bon Ami', 'StoreName': 'Bartell', 'Type': 'Cleanser'}
{'ProductName': 'Crest', 'StoreName': 'Bartell', 'Type': 'Toothpaste'}
{'ProductName': 'Sonicare soft bristles', 'StoreName': 'Bartell', 'Type': 'Sonicare Bristles'}
{'ProductName': 'ACT Restoring Mouthwash', 'StoreName': 'Bartell', 'Type': 'Mouthwash'}
{'ProductName': 'Marketspice Tea Decaf - 2 Oz for Mommy', 'StoreName': 'Bartell', 'Type': 'Marketspice Tea'}
{'ProductName': 'Tumeric Powder', 'StoreName': 'Central Co-op', 'Type': 'Tumeric Powder'}
{'ProductName': 'Italian Seasoning', 'StoreName': 'Central Co-op', 'Type': 'Italian Seasoning'}
{'ProductName': 'Great Northern White Bean', 'StoreName': 'Central Co-op', 'Type': 'White Bean'}
{'ProductName': 'Boullion - Vegetable Broth Powdered', 'StoreName': 'Central Co-op', 'Type': 'Bouillon'}
# ...truncated to 10 for brevity
```

# list products that don't have a store associated with them

Where the hell do I buy this crap?

``` example
MATCH (p:Product)
WHERE NOT (p)-[:PURCHASE_AT]->(:Store)
RETURN p.name AS ProductName
ORDER BY toLower(ProductName)
;
```

Results:

``` example
{'ProductName': 'Apple Cider Vinegar'}
{'ProductName': 'Apple Cider Vinegar in Glass Bottle (Non-Organic)'}
{'ProductName': 'Aroy-D Coconut Milk'}
{'ProductName': 'Avocado Oil'}
{'ProductName': 'Beansprouts'}
{'ProductName': 'Candlenuts'}
{'ProductName': 'Coconut Aminos'}
{'ProductName': 'Coconut Oil'}
{'ProductName': 'Cooking Oil'}
{'ProductName': 'Cooking Oil'}
{'ProductName': 'Corn on cob'}
{'ProductName': 'Cornstarch'}
{'ProductName': 'Dashi'}
{'ProductName': 'Dried Thai Chilis'}
{'ProductName': 'Dried Thai shrimp paste'}
{'ProductName': 'Egg yolk'}
{'ProductName': 'Extra-virgin olive oil'}
{'ProductName': 'Fermented shrimp paste'}
{'ProductName': 'Feta Cheese'}
{'ProductName': 'Fish Sauce'}
{'ProductName': 'Fresh flat-leaf parsley'}
{'ProductName': 'Fresno chilies'}
{'ProductName': 'Fried shallots'}
{'ProductName': 'Grape Tomatoes'}
{'ProductName': 'Green Bell Pepper'}
{'ProductName': 'Green lettuce'}
{'ProductName': 'Ice-cold water'}
{'ProductName': 'Japanese Nori'}
{'ProductName': 'Kaffir Lime'}
{'ProductName': 'Kalamata Olives'}
{'ProductName': 'Korean Wild Sesame Oil'}
{'ProductName': 'Kosher Salt'}
{'ProductName': 'Laksa leaves'}
{'ProductName': 'Makrut lime zest'}
{'ProductName': 'Maple Syrup'}
{'ProductName': 'Mild dried red chilies'}
{'ProductName': 'Mirin'}
{'ProductName': 'Miso'}
{'ProductName': "Newman's Own Sesame Ginger Dressing"}
{'ProductName': 'Oil-packed sun-dried tomatoes'}
{'ProductName': 'Pressed Tofu'}
{'ProductName': 'Red Curry Paste'}
{'ProductName': 'Red Curry Paste'}
{'ProductName': 'Red Onion'}
{'ProductName': 'Rice vinegar'}
{'ProductName': 'Rosemary'}
{'ProductName': 'Round Rice Paper Sheets'}
{'ProductName': 'Russet potatoes'}
{'ProductName': 'Salted Turnip'}
{'ProductName': 'Sambal'}
{'ProductName': 'Sawtooth Coriander'}
{'ProductName': 'Sea Salt'}
{'ProductName': 'Shredded Carrot'}
{'ProductName': 'Shrimp Paste'}
{'ProductName': 'Spicy dried red chilies'}
{'ProductName': 'Straw Mushrooms'}
{'ProductName': 'Tamarind Paste'}
{'ProductName': 'Thai chili'}
{'ProductName': 'Three Crabs Fish Sauce'}
{'ProductName': 'Toasted sesame flakes'}
{'ProductName': 'Tofu puffs'}
{'ProductName': 'Turmeric'}
{'ProductName': 'Unsweetened Nut Butter'}
{'ProductName': 'Wasabi'}
{'ProductName': 'Yellow Bell Pepper'}
{'ProductName': 'Yellow Curry Paste'}
```

# list products that don't have a store associated with them, but limit to 10

Data cleanup is a pain in the ass and I want to take it in bite size
pieces, so randomize the list to keep me interested and return just 10
to keep me from being disheartended.

``` example
// fail:
// MATCH (product:Product)
// WHERE NOT (product)-[:PURCHASE_AT]->(:Store)
// WITH product
// ORDER BY RAND()
// RETURN product.name AS ProductName
// ORDER BY ProductName
// LIMIT 10;

// fail:
// MATCH (product:Product)
// WHERE NOT (product)-[:PURCHASE_AT]->(:Store)
// WITH product
// ORDER BY RAND()
// WITH COLLECT(product) AS randomProducts
// UNWIND randomProducts AS product
// RETURN product.name AS ProductName
// ORDER BY ProductName
// LIMIT 10;

// fail:
// MATCH (product:Product)
// WHERE NOT (product)-[:PURCHASE_AT]->(:Store)
// WITH product
// ORDER BY RAND()
// LIMIT 10
// RETURN product.name AS ProductName;

// fail:
// MATCH (product:Product)
// WHERE NOT (product)-[:PURCHASE_AT]->(:Store)
// WITH product
// ORDER BY RAND()
// LIMIT 10
// WITH COLLECT(product) AS randomProducts
// UNWIND randomProducts AS product
// ORDER BY product.name
// RETURN product.name AS ProductName;

// works:
MATCH (product:Product)
WHERE NOT (product)-[:PURCHASE_AT]->(:Store)
WITH product
ORDER BY RAND()
LIMIT 10
RETURN product.name AS ProductName
ORDER BY ProductName
;
```

Results:

``` example
{'ProductName': 'Coconut Oil'}
{'ProductName': 'Cooking Oil'}
{'ProductName': 'Fresh flat-leaf parsley'}
{'ProductName': 'Fresno chilies'}
{'ProductName': 'Grape Tomatoes'}
{'ProductName': 'Korean Wild Sesame Oil'}
{'ProductName': 'Red Curry Paste'}
{'ProductName': 'Red Curry Paste'}
{'ProductName': 'Spicy dried red chilies'}
{'ProductName': 'Wasabi'}
```

# list the entity type its assocted with

``` example
MATCH (n)
UNWIND labels(n) AS label
UNWIND keys(n) AS propertyName
RETURN label, propertyName
;
```

Results:

``` example
{'label': 'Person', 'propertyName': 'name'}
{'label': 'Person', 'propertyName': 'urls'}
{'label': 'Person', 'propertyName': 'name'}
{'label': 'Person', 'propertyName': 'urls'}
{'label': 'Person', 'propertyName': 'name'}
{'label': 'Person', 'propertyName': 'urls'}
{'label': 'Person', 'propertyName': 'name'}
{'label': 'Person', 'propertyName': 'urls'}
{'label': 'Person', 'propertyName': 'name'}
{'label': 'Person', 'propertyName': 'urls'}
# ...truncated to 10 for brevity
```

# list distinct entities

``` example
MATCH (n)
WITH DISTINCT labels(n) AS distinctLabels, keys(n) AS propertyNames
UNWIND distinctLabels AS label
UNWIND propertyNames AS propertyName
RETURN DISTINCT label, propertyName
;
```

Results:

``` example
{'label': 'Person', 'propertyName': 'name'}
{'label': 'Person', 'propertyName': 'urls'}
{'label': 'Person', 'propertyName': 'ytb'}
{'label': 'Recipe', 'propertyName': 'urls'}
{'label': 'Recipe', 'propertyName': 'name'}
{'label': 'Store', 'propertyName': 'name'}
{'label': 'Store', 'propertyName': 'urls'}
{'label': 'Store', 'propertyName': 'origin'}
{'label': 'Store', 'propertyName': 'notes'}
{'label': 'Store', 'propertyName': 'google_maps'}
{'label': 'Product', 'propertyName': 'type'}
{'label': 'Product', 'propertyName': 'name'}
{'label': 'Product', 'propertyName': 'urls'}
{'label': 'Product', 'propertyName': 'brand'}
{'label': 'Product', 'propertyName': 'bb_says'}
{'label': 'Product', 'propertyName': 'photos'}
{'label': 'Product', 'propertyName': 'manufacturer'}
{'label': 'Product', 'propertyName': 'note'}
{'label': 'Product', 'propertyName': 'google'}
{'label': 'Product', 'propertyName': 'comments'}
{'label': 'Product', 'propertyName': 'googleSearch'}
{'label': 'Product', 'propertyName': 'detail'}
```

# list CONTAINS relations

This doesn't help in the least bit…the properties are identical…find a
better way.

``` example
MATCH ()-[r:CONTAINS]-()
UNWIND keys(r) AS propertyNames
RETURN type(r) AS type, propertyNames AS propertyName
ORDER BY type, propertyName
;
```

Results:

``` example
{'type': 'CONTAINS', 'propertyName': 'quantity'}
{'type': 'CONTAINS', 'propertyName': 'quantity'}
{'type': 'CONTAINS', 'propertyName': 'quantity'}
{'type': 'CONTAINS', 'propertyName': 'quantity'}
{'type': 'CONTAINS', 'propertyName': 'quantity'}
# ...truncated to 5 for brevity
```

# list uniquely all CONTAINS relations

``` example
MATCH ()-[r:CONTAINS]-()
UNWIND keys(r) AS propertyNames
RETURN DISTINCT type(r) AS type, propertyNames AS propertyName
ORDER BY type, propertyName
;
```

Results:

``` example
{'type': 'CONTAINS', 'propertyName': 'quantity'}
{'type': 'CONTAINS', 'propertyName': 'urls'}
```

# FIXED: list products that have urls that are photos in google drive

This fails

``` example
MATCH (p:Product)
WHERE EXISTS(p.urls) AND ANY(url IN p.urls WHERE url CONTAINS 'google')
RETURN p.name AS ProductName, p.urls AS URLs;
```

with error

``` example
[mtm@Shane-s-Note:poorclaim(master)]$ cypher-shell -a neo4j://localhost:7687 --file /Users/mtm/pdev/taylormonacelli/anythingflorida/query.cypher
The property existence syntax `... exists(variable.property)` is no longer supported. Please use `variable.property IS NOT NULL` instead. (line 2, column 7 (offset: 24))
"WHERE EXISTS(p.urls) AND ANY(url IN p.urls WHERE url CONTAINS 'google')"
     ^
[mtm@Shane-s-Note:poorclaim(master)]$
```

``` example
// this works as expected:

MATCH (p:Product)
WHERE p.urls IS NOT NULL AND ANY(url IN p.urls WHERE url CONTAINS 'photos.google.com')
RETURN p.name AS ProductName, p.urls AS URLs
;
```

Results:

``` example
{'ProductName': 'Chili Sauce', 'URLs': ['https://thewoksoflife.com/wp-content/uploads/2020/07/chili-oil-recipe-18.jpg', 'https://www.amazon.com/%E8%80%81%E5%B9%B2%E5%A6%88%E9%A6%99%E8%BE%A3%E8%84%86%E6%B2%B9%E8%BE%A3%E6%A4%92-Spicy-Chili-Crisp-7-41/dp/B07VHKTTR3/ref=asc_df_B07VHKTTR3/?tag=hyprod-20&linkCode=df0&hvadid=642112947349&hvpos=&hvnetw=g&hvrand=12580253979732381700&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9061293&hvtargid=pla-1951193779579&psc=1', 'https://www.google.com/search?sca_esv=580857096&sxsrf=AM9HkKmLh9FDQ0x5jNY12kJCSSbwO6Q3FA:1699539552211&q=thai+and+true+hot+chili&tbm=isch&source=lnms&sa=X&ved=2ahUKEwiJ8KiajreCAxWqAjQIHaMBDKYQ0pQJegQIDBAB&biw=1440&bih=754&dpr=2#imgrc=KDhcVOHe9yNjkM', 'https://photos.google.com/photo/AF1QipMQPtIdU1_m3SkgBWs5RcE2QXFs2OnbbJAdaG9M']}
{'ProductName': 'Eucerin Creme Daily Moisturizing Skin Calming', 'URLs': ['https://photos.google.com/photo/AF1QipM2_uDtc-2Uc7XriFP3k4H0L_DxcvxVeYvgUlpG', 'https://photos.google.com/photo/AF1QipM2_uDtc-2Uc7XriFP3k4H0L_DxcvxVeYvgUlpG']}
{'ProductName': 'Jasmine Rice', 'URLs': ['https://photos.google.com/photo/AF1QipM0ragYoS8EjrRngQukQJH_U1hnen_ACdJyMqEV']}
{'ProductName': 'Kaffir lime leaves', 'URLs': ['https://www.wholefoodsmarket.com/product/kaffir-lime%20leaves-b07q8ldbvj', 'https://www.youtube.com/watch?v=4Qz5nC-DcKk', 'https://www.safeway.com/shop/marketplace/product-details.970537048.html', 'https://photos.google.com/photo/AF1QipPI_6_YxYIuCSAvP93sDoRcyFDjekCQjNSb3Ln0', 'https://photos.google.com/photo/AF1QipPd_yNuI9VcQAFOwMSuvBx40o_sl4gAmCgBYNIQ', 'https://www.youtube.com/watch?v=SB3AV7oHKiE']}
{'ProductName': 'Mint leaves', 'URLs': ['https://photos.google.com/photo/AF1QipNrbFzt7g3nCOVFOmFa6geW-HODg2hilRdq4xl0']}
{'ProductName': 'Perilla Oil', 'URLs': ['https://www.youtube.com/watch?v=VpAS3RarPi8', 'https://megakfood.com/products/8801045448503', 'https://photos.google.com/photo/AF1QipNe7d-KXSpC90FJ1uJNMnH1fMFZ6E8Qlzr_j3Q0', 'https://photos.google.com/photo/AF1QipOLrXnJ8Bj20xFh5lg5yhm71ApUoRlT1z6_ZqnB', 'https://photos.google.com/photo/AF1QipP8OZZvarZPkNnnaOOv3k_ng9doQzMeVZgONlxK']}
{'ProductName': 'Rice noodle sheets', 'URLs': ['https://www.google.com/search?sca_esv=579554252&sxsrf=AM9HkKlaWKZFra1JEJmQLagqVwu7lOpvPA:1699161392487&q=rice+paper&tbm=isch&source=lnms&sa=X&sqi=2&ved=2ahUKEwjyhdy5jayCAxWmADQIHTJBBhUQ0pQJegQIDxAB&biw=1440&bih=758&dpr=2', 'https://balancewithjess.com/hu-tieu-ap-chao/', 'https://www.google.com/search?q=hu+tieu+xao+rice+sheets&tbm=isch&ved=2ahUKEwjExZejjayCAxU_JjQIHf97ACQQ2-cCegQIABAA&oq=hu+tieu+xao+rice+sheets&gs_lcp=CgNpbWcQAzoECCMQJzoFCAAQgAQ6BwgAEIoFEEM6BwgAEBgQgARQvQRYpRdgxRpoAHAAeACAATmIAecEkgECMTOYAQCgAQGqAQtnd3Mtd2l6LWltZ8ABAQ&sclient=img&ei=ASVHZYTBDb_M0PEP__eBoAI&bih=758&biw=1440#imgrc=il_S9C1t9kGChM', 'https://www.foodsofjane.com/recipes/steamed-rice-rolls', 'https://www.google.com/search?client=emacs&sca_esv=579554252&sxsrf=AM9HkKkMHZcCbxpmpXqsj48WrwEW--xssw:1699161240321&q=Rice+noodle+sheets&tbm=isch&source=lnms&sa=X&ved=2ahUKEwiPypTxjKyCAxW_MDQIHVJjDeYQ0pQJegQIDBAB&biw=1440&bih=758&dpr=2#imgrc=Vw7_7S7XaN_v6M', 'https://photos.google.com/photo/AF1QipPM6Ts-zLh2dl10ono15alL7hCGwSCHhbOyav6v', 'https://phohoa.com/', 'https://www.google.com/search?q=pho+hoa+seattle&oq=pho+hoa+seatt&gs_lcrp=EgZjaHJvbWUqCggAEAAY4wIYgAQyCggAEAAY4wIYgAQyEAgBEC4YrwEYxwEYgAQYjgUyBggCEEUYOTIICAMQABgWGB4yCAgEEC4YFhgeMgoIBRAAGIYDGIoFMgYIBhBFGEDSAQg1Mjk1ajBqN6gCALACAA&sourceid=chrome&ie=UTF-8#lpg=cid:CgIgAQ%3D%3D,ik:CAoSLEFGMVFpcE40MXM4TXJDSzlDcFVRZWxBRHZPNUZXb1h5LWtIVFpaeHNnZm03', 'https://timeline.google.com/maps/timeline?pli=1&rapt=AEjHL4MhNWvrl4xjhvtinEYv8V8WTyxNYgSR-reE9VJgys6Ba7GccWm6B2Xi6Xa3uKxuR9rkftCXiinZ4f3LvAJGF9CnnqgrtUIGNdtCmaP1EhTNElp4eko&pb=!1m2!1m1!1s2023-11-04', 'https://www.google.com/search?client=emacs&sca_esv=579833118&sxsrf=AM9HkKmyvTZJVTjaoB4T2Is_emhNvlG1og:1699290431734&q=rice+paper&tbm=isch&source=lnms&sa=X&ved=2ahUKEwimz7aU7q-CAxVkFjQIHXrWCSgQ0pQJegQIDhAB&biw=1440&bih=758&dpr=2', 'https://i0.wp.com/www.wokandkin.com/wp-content/uploads/2021/04/Rice-Paper-saved-for-web-1200-px.png?w=1200&ssl=1']}
{'ProductName': 'Rice vermicelli', 'URLs': ['https://photos.google.com/photo/AF1QipPPETrmRSh8-h9guEbb90DRig4g_njAUvQ50Ol6', 'https://photos.google.com/photo/AF1QipMYLPcT9Oybki3TQGztAT1X5tIxpknKSJ0ZmdlP', 'https://www.amazon.com/Fresh-Stick-Vermicelli-SIMPLY-FOOD/dp/B08NXVTFTP/ref=asc_df_B08NXVTFTP/?tag=hyprod-20&linkCode=df0&hvadid=652498065761&hvpos=&hvnetw=g&hvrand=10598234170837115346&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9061293&hvtargid=pla-2065471401768&psc=1', 'https://www.amazon.com/Fresh-Stick-Vermicelli-SIMPLY-FOOD/dp/B08NXVTFTP/ref=asc_df_B08NXVTFTP/?tag=hyprod-20&linkCode=df0&hvadid=652498065761&hvpos=&hvnetw=g&hvrand=10598234170837115346&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9061293&hvtargid=pla-2065471401768&psc=1']}
{'ProductName': 'Signature Care Baby Lotion', 'URLs': ['https://www.google.com/search?client=emacs&sca_esv=580645679&sxsrf=AM9HkKmFAe6c5ttC3Glgq4OAYuHfy2tEjw:1699487253983&q=Signature+Care+baby+lotion&tbm=isch&source=lnms&sa=X&ved=2ahUKEwjopsuwy7WCAxWzFTQIHdjcCGIQ0pQJegQIDhAB&biw=1440&bih=754&dpr=2#imgrc=0Cnl_Uyq2nmiBM', 'https://photos.google.com/photo/AF1QipPtyZkpbFq-ZvHy5JD9WYAiDFBvmkPXB_pFNjPL']}
{'ProductName': 'Tamarind Liquid', 'URLs': ['https://photos.google.com/photo/AF1QipMTNoAmEBIUBgJiziw2Tl16y2KscVqpjfDGlS-q', 'https://photos.google.com/photo/AF1QipPd47xo0JnbBdfR9pbd6FgvPRvxghQoP_wmWxph']}
```

# FIXED: list relations, not just CONTAINS and show relation properties

This fixes the item in section: 'WRONG: list relations, not just
CONTAINS and show relation properties'

``` example
MATCH ()-[r]-()
RETURN DISTINCT type(r) AS type,
                CASE WHEN size(keys(r)) > 0 THEN keys(r) ELSE [] END AS propertyNames
ORDER BY type, propertyNames
;
```

Results:

``` example
{'type': 'CONTAINS', 'propertyNames': []}
{'type': 'CONTAINS', 'propertyNames': ['quantity']}
{'type': 'CONTAINS', 'propertyNames': ['quantity', 'urls']}
{'type': 'CREATED', 'propertyNames': []}
{'type': 'IS_THE_SAME_AS', 'propertyNames': []}
{'type': 'PURCHASE_AT', 'propertyNames': []}
{'type': 'PURCHASE_AT', 'propertyNames': ['note']}
{'type': 'PURCHASE_AT', 'propertyNames': ['url']}
{'type': 'PURCHASE_AT', 'propertyNames': ['urls']}
{'type': 'PURCHASE_AT', 'propertyNames': ['urls', 'aisle']}
{'type': 'RECOMMENDS', 'propertyNames': ['urls']}
```

# what store should I visit to make a recipe?

suppose I would like to make a particular recipe, then what stores do I
need to visit?

``` example
MATCH (r:Recipe)
WHERE r.name IN ['Vietnamese Spring Rolls (Gỏi Cuốn)']
WITH r
MATCH (r)-[:CONTAINS]->(p:Product)
OPTIONAL MATCH (p)-[:PURCHASE_AT]->(s:Store)
WITH p, COLLECT(DISTINCT s) AS stores
RETURN COLLECT(DISTINCT p.name) AS Ingredients,
       [store IN stores | CASE WHEN store IS NOT NULL THEN store.name ELSE 'Unknown' END] AS Stores
ORDER BY [store IN Stores | toLower(store)]
;
```

Results:

``` example
{'Ingredients': ['Green lettuce'], 'Stores': []}
{'Ingredients': ['Water'], 'Stores': ['dummy place holder']}
{'Ingredients': ['Shrimp'], 'Stores': ['Hau Hau Market']}
{'Ingredients': ['Rice vermicelli'], 'Stores': ["Lam's Seafood Asian Market"]}
{'Ingredients': ['Dry-Roasted Peanuts'], 'Stores': ['PCC']}
{'Ingredients': ['Lee Kum Kee Sauce Hoisin'], 'Stores': ['QFC']}
{'Ingredients': ['Ginger', 'Adams Peanut Butter', 'Vegetable Oil'], 'Stores': ['Safeway']}
{'Ingredients': ['Garlic'], 'Stores': ["Trader Joe's"]}
{'Ingredients': ['Rice paper', 'Mint leaves'], 'Stores': ['Uwajimaya']}
```

# where should I shop for ingredients for Chicken Teriyaki?

suppose I were to make Chicken Teriyaki, then what stores need I visit
to get products I'd need for it?

``` example
MATCH (r:Recipe {name: 'Tomatillo Salsa Verde'})-[:CONTAINS]->(p:Product)
MATCH (p)-[:PURCHASE_AT]->(s:Store)
RETURN s.name AS StoreName, COLLECT(DISTINCT p.name) AS Ingredients
;
```

Results:

``` example
{'StoreName': 'Safeway', 'Ingredients': ['Tomatillos', 'Cilantro', 'White Onion', 'Jalapeno Pepper']}
{'StoreName': "Trader Joe's", 'Ingredients': ['Garlic']}
{'StoreName': 'Whole Foods', 'Ingredients': ['Lime juice']}
{'StoreName': 'QFC', 'Ingredients': ['Salt']}
```

# suppose I would like to make 2 recipes, then what stores do I need to visit?

``` example
MATCH (r:Recipe)
WHERE r.name IN ['Vietnamese Spring Rolls (Gỏi Cuốn)','Tom Yum Goong']
WITH r
MATCH (r)-[:CONTAINS]->(p:Product)
OPTIONAL MATCH (p)-[:PURCHASE_AT]->(s:Store)
WITH p, COLLECT(DISTINCT s) AS stores
RETURN COLLECT(DISTINCT p.name) AS Ingredients,
       [store IN stores | CASE WHEN store IS NOT NULL THEN store.name ELSE 'Unknown' END] AS Stores
ORDER BY [store IN Stores | toLower(store)]
;
```

Results:

``` example
{'Ingredients': ['Sawtooth Coriander', 'Green lettuce'], 'Stores': []}
{'Ingredients': ['Water'], 'Stores': ['dummy place holder']}
{'Ingredients': ['Fish sauce', 'Shrimp'], 'Stores': ['Hau Hau Market']}
{'Ingredients': ['Kaffir lime leaves'], 'Stores': ['Hau Hau Market', 'Uwajimaya']}
{'Ingredients': ['Rice vermicelli'], 'Stores': ["Lam's Seafood Asian Market"]}
{'Ingredients': ['Jasmine Rice', 'Dry-Roasted Peanuts'], 'Stores': ['PCC']}
{'Ingredients': ['Lee Kum Kee Sauce Hoisin'], 'Stores': ['QFC']}
{'Ingredients': ['Evaporated Milk', 'Oyster Mushrooms', 'Lemongrass', 'Ginger', 'Adams Peanut Butter', 'Vegetable Oil'], 'Stores': ['Safeway']}
{'Ingredients': ['Garlic'], 'Stores': ["Trader Joe's"]}
{'Ingredients': ['Galangal', 'Mae Ploy Thai Chili Paste in Oil', 'Rice paper', 'Mint leaves'], 'Stores': ['Uwajimaya']}
{'Ingredients': ['Thai chilies'], 'Stores': ['Uwajimaya', "Lam's Seafood Asian Market"]}
{'Ingredients': ['Lime juice'], 'Stores': ['Whole Foods']}
```

# I want to make a recipe and travel to the fewest number of stores

If I would like to make a particular recipe, then what stores do I need
to visit and sort products by stores so I don't have to leave and return
because I didn't realize there were two products from the same store

Also, make sure that if a recipe has an item that is not assigned to a
store by the PURCAHSE<sub>AT</sub> relation, then the store field
appears empty as opposed to not seeing the product at all

``` example
MATCH (r:Recipe {name: 'Korean Sesame Noodles'})-[:CONTAINS]->(p:Product)
OPTIONAL MATCH (p)-[:PURCHASE_AT]->(s:Store)
WITH p, COLLECT(DISTINCT s) AS stores
RETURN COLLECT(DISTINCT p.name) AS Ingredients,
       [store IN stores | CASE WHEN store IS NOT NULL THEN store.name ELSE 'Unknown' END] AS Stores
ORDER BY [store IN Stores | toLower(store)]
;
```

Results:

``` example
{'Ingredients': ['Korean Wild Sesame Oil'], 'Stores': []}
{'Ingredients': ['Toasted Sesame Seeds'], 'Stores': ['Central Co-op']}
{'Ingredients': ['Chili Oil', 'Tsuyu', 'Soba Noodles', 'Toasted Seaweed'], 'Stores': ['M2M Mart']}
{'Ingredients': ['Sesame Seeds'], 'Stores': ['PCC', 'Naked Grocer']}
{'Ingredients': ['Green Onion', 'Red Chilli Peppers'], 'Stores': ['Safeway']}
```

# WRONG: some recipes point to the same product multiple times by mistake

``` example
MATCH (r:Recipe)-[:CONTAINS]->(p:Product)
WITH r, COLLECT(p) AS products
WHERE SIZE(products) > 1
RETURN r, products
;
```

Results:

``` example
{'r': {'urls': ['https://theflavoursofkitchen.com/wprm_print/104534'], 'name': 'Chicken Thai Red Curry'}, 'products': [{'name': 'Full fat coconut milk', 'type': 'Coconut Milk'}, {'name': 'Light Brown Sugar', 'type': 'Brown Sugar'}, {'name': 'Cooking Oil', 'type': 'Oil'}, {'name': 'Onion', 'type': 'Onion'}, {'name': 'Ginger', 'type': 'Ginger'}, {'name': 'Red Bell Pepper', 'type': 'Bell Pepper'}, {'name': 'Garlic', 'type': 'Garlic'}, {'urls': ['https://www.fredmeyer.com/p/simple-truth-organic-thai-basil/0001111001922'], 'name': 'Thai basil', 'type': 'Herb'}, {'name': 'Boneless Chicken Thighs', 'type': 'Chicken'}, {'name': 'Fish sauce', 'type': 'Fish Sauce'}, {'name': 'Chicken Stock or Water', 'type': 'Chicken Stock'}, {'name': 'Zucchini', 'type': 'Zucchini'}, {'name': 'Red Curry Paste', 'type': 'Curry Paste'}, {'name': 'Lemon Juice', 'type': 'Lemon Juice'}]}
{'r': {'urls': ['https://food52.com/recipes/print/86501', 'https://www.youtube.com/watch?v=VpAS3RarPi8'], 'name': 'Cold Soba With Periall Oil dresssing'}, 'products': [{'urls': ['https://www.amazon.com/gp/product/B00101YEBO', 'https://veggiekinsblog.com/2020/01/13/vegan-zaru-soba/'], 'name': 'Buckwheat Soba Nodles', 'type': 'Noodle'}, {'urls': ['https://www.google.com/search?client=emacs&sca_esv=577922779&sxsrf=AM9HkKkUxzT-KjHg9ziVgvqz5Zsqmn7xdw:1698703946500&q=Japanese+nori&tbm=isch&source=lnms&sa=X&ved=2ahUKEwi647yq5Z6CAxVxMjQIHRW8BBYQ0pQJegQIChAB&biw=1440&bih=758&dpr=2'], 'name': 'Japanese Nori', 'type': 'Nori'}, {'urls': ['https://www.youtube.com/watch?v=VpAS3RarPi8', 'https://megakfood.com/products/8801045448503', 'https://photos.google.com/photo/AF1QipNe7d-KXSpC90FJ1uJNMnH1fMFZ6E8Qlzr_j3Q0', 'https://photos.google.com/photo/AF1QipOLrXnJ8Bj20xFh5lg5yhm71ApUoRlT1z6_ZqnB', 'https://photos.google.com/photo/AF1QipP8OZZvarZPkNnnaOOv3k_ng9doQzMeVZgONlxK'], 'name': 'Perilla Oil', 'type': 'Oil'}]}
{'r': {'urls': ['https://cleananddelicious.com/wprm_print/26940'], 'name': 'Crispy Baked Tofu'}, 'products': [{'name': 'Extra Firm Tofu', 'type': 'Tofu'}, {'name': 'Avocado Oil', 'type': 'Avocado Oil'}, {'name': 'Kosher Salt', 'type': 'Kosher Salt'}, {'name': 'Black Pepper', 'type': 'Black Pepper'}, {'name': 'Tamari', 'type': 'Tamari'}, {'name': 'Garlic Powder', 'type': 'Garlic'}, {'name': 'Cornstarch', 'type': 'Cornstarch'}]}
{'r': {'urls': ['https://seonkyounglongest.com/drunken-noodles/'], 'name': 'The Best Drunken Noodles'}, 'products': [{'name': 'Palm Sugar', 'type': 'Sugar'}, {'name': 'White pepper', 'type': 'White pepper'}, {'name': 'Soy sauce', 'type': 'Soy sauce'}, {'name': 'Fish sauce', 'type': 'Fish Sauce'}, {'name': 'Pork', 'type': 'Pork'}, {'name': 'Red Chilli Peppers', 'type': 'Chilli Pepper'}, {'name': 'Chicken', 'type': 'Chicken'}, {'name': 'Basil', 'type': 'Basil'}, {'name': 'Thai-style Baked Tofu', 'type': 'Tofu'}, {'name': 'Fish sauce', 'type': 'Fish Sauce'}, {'name': 'Dark soy sauce', 'type': 'Soy Sauce'}, {'urls': ['https://www.google.com/search?sca_esv=579554252&sxsrf=AM9HkKlaWKZFra1JEJmQLagqVwu7lOpvPA:1699161392487&q=rice+paper&tbm=isch&source=lnms&sa=X&sqi=2&ved=2ahUKEwjyhdy5jayCAxWmADQIHTJBBhUQ0pQJegQIDxAB&biw=1440&bih=758&dpr=2', 'https://balancewithjess.com/hu-tieu-ap-chao/', 'https://www.google.com/search?q=hu+tieu+xao+rice+sheets&tbm=isch&ved=2ahUKEwjExZejjayCAxU_JjQIHf97ACQQ2-cCegQIABAA&oq=hu+tieu+xao+rice+sheets&gs_lcp=CgNpbWcQAzoECCMQJzoFCAAQgAQ6BwgAEIoFEEM6BwgAEBgQgARQvQRYpRdgxRpoAHAAeACAATmIAecEkgECMTOYAQCgAQGqAQtnd3Mtd2l6LWltZ8ABAQ&sclient=img&ei=ASVHZYTBDb_M0PEP__eBoAI&bih=758&biw=1440#imgrc=il_S9C1t9kGChM', 'https://www.foodsofjane.com/recipes/steamed-rice-rolls', 'https://www.google.com/search?client=emacs&sca_esv=579554252&sxsrf=AM9HkKkMHZcCbxpmpXqsj48WrwEW--xssw:1699161240321&q=Rice+noodle+sheets&tbm=isch&source=lnms&sa=X&ved=2ahUKEwiPypTxjKyCAxW_MDQIHVJjDeYQ0pQJegQIDBAB&biw=1440&bih=758&dpr=2#imgrc=Vw7_7S7XaN_v6M', 'https://photos.google.com/photo/AF1QipPM6Ts-zLh2dl10ono15alL7hCGwSCHhbOyav6v', 'https://phohoa.com/', 'https://www.google.com/search?q=pho+hoa+seattle&oq=pho+hoa+seatt&gs_lcrp=EgZjaHJvbWUqCggAEAAY4wIYgAQyCggAEAAY4wIYgAQyEAgBEC4YrwEYxwEYgAQYjgUyBggCEEUYOTIICAMQABgWGB4yCAgEEC4YFhgeMgoIBRAAGIYDGIoFMgYIBhBFGEDSAQg1Mjk1ajBqN6gCALACAA&sourceid=chrome&ie=UTF-8#lpg=cid:CgIgAQ%3D%3D,ik:CAoSLEFGMVFpcE40MXM4TXJDSzlDcFVRZWxBRHZPNUZXb1h5LWtIVFpaeHNnZm03', 'https://timeline.google.com/maps/timeline?pli=1&rapt=AEjHL4MhNWvrl4xjhvtinEYv8V8WTyxNYgSR-reE9VJgys6Ba7GccWm6B2Xi6Xa3uKxuR9rkftCXiinZ4f3LvAJGF9CnnqgrtUIGNdtCmaP1EhTNElp4eko&pb=!1m2!1m1!1s2023-11-04', 'https://www.google.com/search?client=emacs&sca_esv=579833118&sxsrf=AM9HkKmyvTZJVTjaoB4T2Is_emhNvlG1og:1699290431734&q=rice+paper&tbm=isch&source=lnms&sa=X&ved=2ahUKEwimz7aU7q-CAxVkFjQIHXrWCSgQ0pQJegQIDhAB&biw=1440&bih=758&dpr=2', 'https://i0.wp.com/www.wokandkin.com/wp-content/uploads/2021/04/Rice-Paper-saved-for-web-1200-px.png?w=1200&ssl=1'], 'name': 'Rice noodle sheets', 'google': 'Rice noodle sheets', 'type': 'Rice noodle sheets'}, {'name': 'Lime', 'type': 'Lime'}, {'name': 'Thai chili', 'type': 'Chilies'}, {'name': 'Cooking Oil', 'type': 'Cooking Oil'}, {'name': 'Chinese Broccoli', 'type': 'Broccoli'}, {'name': 'Shrimp', 'type': 'Shrimp'}, {'name': 'Garlic', 'type': 'Garlic'}, {'name': 'White pepper', 'type': 'White pepper'}, {'name': 'Oyster Sauce', 'type': 'Oyster Sauce'}]}
{'r': {'urls': ['https://www.williams-sonoma.com/recipe/farro-salad-with-artichoke-hearts.html?print=true'], 'name': 'Farro Salad with Artichoke Hearts'}, 'products': [{'name': 'Semi-pearled Farro', 'type': 'Farro'}, {'name': 'Oil-packed sun-dried tomatoes', 'type': 'Tomatoes'}, {'name': 'Fresh flat-leaf parsley', 'type': 'Herbs'}, {'name': 'Red Onion', 'type': 'Onion'}, {'name': 'Salt', 'type': 'Salt'}, {'name': 'Extra-virgin olive oil', 'type': 'Olive Oil'}, {'name': 'Pine Nuts', 'type': 'Pine nuts'}, {'name': 'Red wine vinegar', 'type': 'Vinegar'}, {'name': 'Black Pepper', 'type': 'Black Pepper'}, {'name': 'Artichoke Hearts', 'type': 'Artichokes'}]}
{'r': {'urls': ['https://www.meghanlivingstone.com/ginger-sesame-dressing/', 'https://www.meghanlivingstone.com/wprm_print/2060'], 'name': 'Ginger Sesame Dressing'}, 'products': [{'name': 'Apple Cider Vinegar', 'type': 'Apple Cider Vinegar'}, {'name': 'Ginger Powder', 'type': 'Ginger Powder'}, {'name': 'Toasted Sesame Oil', 'type': 'Sesame Oil'}, {'name': 'Maple Syrup', 'type': 'Maple Syrup'}, {'name': 'Unsweetened Nut Butter', 'type': 'Unsweetened Nut Butter'}, {'name': 'Coconut Aminos', 'type': 'Soy Sauce Alternative'}]}
{'r': {'urls': ['https://www.ambitiouskitchen.com/wprm_print/24776'], 'name': 'The Easiest Chickpea Greek Salad'}, 'products': [{'name': 'Salt', 'type': 'Salt'}, {'name': 'Feta Cheese', 'type': 'Cheese'}, {'name': 'Extra-virgin olive oil', 'type': 'Olive Oil'}, {'name': 'Lemon Juice', 'type': 'Lemon Juice'}, {'name': 'Grape Tomatoes', 'type': 'Tomatoes'}, {'name': 'Red Onion', 'type': 'Red Onion'}, {'name': 'Kalamata Olives', 'type': 'Olives'}, {'name': 'Yellow Bell Pepper', 'type': 'Bell Pepper'}, {'name': 'Red Bell Pepper', 'type': 'Bell Pepper'}, {'name': 'Green Bell Pepper', 'type': 'Bell Pepper'}, {'name': 'Garlic', 'type': 'Garlic'}, {'name': 'Cucumber', 'type': 'Cucumber'}, {'name': 'Oregano', 'type': 'Oregano'}, {'name': 'Chickpeas', 'type': 'Chickpeas'}]}
{'r': {'urls': ['https://seonkyounglongest.com/korean-sesame-noodles/print/46266/'], 'name': 'Korean Sesame Noodles'}, 'products': [{'name': 'Green Onion', 'type': 'Onion'}, {'name': 'Chili Oil', 'type': 'Chili Oil'}, {'urls': ['https://www.google.com/search?q=tsuyu+soup+seasoning+sauce&oq=tsuyu+soup+seasoning+sauce'], 'googleSearch': 'tsuyu, soup seasoning sauce', 'name': 'Tsuyu', 'type': 'Sauce'}, {'name': 'Red Chilli Peppers', 'type': 'Chilli Pepper'}, {'name': 'Soba Noodles', 'type': 'Soba Noodles'}, {'name': 'Sesame Seeds', 'type': 'Sesame Seeds'}, {'name': 'Green Onion', 'type': 'Onion'}, {'name': 'Toasted Sesame Seeds', 'type': 'Sesame Seeds'}, {'name': 'Toasted Seaweed', 'type': 'Seaweed'}, {'name': 'Korean Wild Sesame Oil', 'type': 'Sesame Oil'}]}
{'r': {'urls': ['https://hot-thai-kitchen.com/singaporean-laksa/print/7645/', 'https://hot-thai-kitchen.com/singaporean-laksa/', 'https://www.youtube.com/watch?v=cWtnFKFiB_0'], 'name': 'Laksa'}, 'products': [{'name': 'Lemongrass', 'type': 'Lemongrass'}, {'name': 'Shrimp', 'type': 'Shrimp'}, {'name': 'Thai chilies', 'type': 'Pepper'}, {'name': 'Mild dried red chilies', 'type': 'Dry Chilies'}, {'name': 'Galangal', 'type': 'Galangal'}, {'name': 'Tofu puffs', 'type': 'Tofu'}, {'name': 'Garlic', 'type': 'Garlic'}, {'name': 'Shallots', 'type': 'Shallots'}, {'urls': ['https://www.google.com/search?client=emacs&sca_esv=580758711&sxsrf=AM9HkKmwGL8OAnRZ8-PJqCLp_VU9-SlJfg:1699507479310&q=Candlenuts&tbm=isch&source=lnms&sa=X&ved=2ahUKEwiwsOPclraCAxVVETQIHabkCi0Q0pQJegQIDRAB&biw=1440&bih=754&dpr=2#imgrc=7uHbBToP7aPjSM'], 'name': 'Candlenuts', 'type': 'Candlenuts'}, {'urls': ['https://youtu.be/cWtnFKFiB_0?t=458'], 'name': 'Fish cakes', 'type': 'Seafood'}, {'name': 'Sambal', 'type': 'Condiment'}, {'name': 'Clams', 'type': 'Clams'}, {'name': 'Water', 'type': 'Water'}, {'urls': ['https://thewoksoflife.com/shrimp-paste-sauce/'], 'name': 'Fermented shrimp paste', 'type': 'Fermented shrimp paste'}, {'name': 'Fish sauce', 'type': 'Fish Sauce'}, {'name': 'Beansprouts', 'type': 'Vegetable'}, {'name': 'Full fat coconut milk', 'type': 'Coconut Milk'}, {'name': 'Turmeric', 'type': 'Turmeric'}, {'name': 'Granulated Sugar', 'type': 'Granulated Sugar'}, {'name': 'Dried Shrimp', 'type': 'Seafood', 'photos': ['https://photos.google.com/photo/AF1QipMJV_m1w-qezTjSZAmu6Vam_PKMR6GICW6TJ883', 'https://www.google.com/search?sca_esv=579651652&sxsrf=AM9HkKlBKUS5rDWtKoKSgxss4PSHC4u0jA:1699211859653&q=bdmp+dried+shrimp&tbm=isch&source=lnms&sa=X&sqi=2&ved=2ahUKEwiUtKu6ya2CAxVFIjQIHXeICOQQ0pQJegQIDRAB&biw=1440&bih=758&dpr=2#imgrc=_WqiWb3wPqLdYM', 'https://www.youtube.com/watch?v=dBSmCwUXZF0']}, {'name': 'Dry rice noodles', 'type': 'Rice Noodles'}, {'name': 'Laksa leaves', 'type': 'Herb'}]}
{'r': {'urls': ['https://www.foodandwine.com/pad-see-ew-7559639?print'], 'name': 'Pad See Ew'}, 'products': [{'urls': ['https://youtu.be/5odVRW9ldzU?t=323'], 'name': 'Wide rice noodles', 'type': 'Rice Noodles'}, {'name': 'Granulated Sugar', 'type': 'Granulated Sugar'}, {'name': 'Dark soy sauce', 'type': 'Soy Sauce'}, {'name': 'Eggs', 'type': 'Eggs'}, {'name': 'Distilled white vinegar', 'type': 'Vinegar'}, {'name': 'Chinese Broccoli', 'type': 'Broccoli'}, {'name': 'White pepper', 'type': 'White pepper'}, {'name': 'Cornstarch', 'type': 'Cornstarch'}, {'urls': ['https://en.wikipedia.org/wiki/Bird%27s_eye_chili', 'https://www.google.com/search?client=emacs&sca_esv=579702589&sxsrf=AM9HkKlqpOqf2K4ex4TTB1e3ix-WBqYAKQ:1699243036206&q=Thai+bird+chiles&tbm=isch&source=lnms&sa=X&ved=2ahUKEwjHnL3Mva6CAxVaCjQIHdJRCxEQ0pQJegQIDxAB&biw=1440&bih=758&dpr=2#imgrc=u6dinAhHDxTfaM'], 'name': 'Thai bird chiles', 'type': 'Chilies'}, {'name': 'Soy sauce', 'type': 'Soy sauce'}, {'name': 'Oyster Sauce', 'type': 'Oyster Sauce'}, {'name': 'Fish sauce', 'type': 'Fish Sauce'}, {'name': 'Garlic', 'type': 'Garlic'}, {'name': 'Skirt steak', 'type': 'Beef'}, {'name': 'Vegetable Oil', 'type': 'Vegetable Oil'}]}
{'r': {'urls': ['https://www.foodnetwork.com/recipes/pad-thai-7112938?soc=youtube'], 'name': 'Pad Thai'}, 'products': [{'name': 'Shrimp', 'type': 'Shrimp'}, {'name': 'Fish sauce', 'type': 'Fish Sauce'}, {'name': 'Dry-Roasted Peanuts', 'type': 'Peanuts'}, {'name': 'Garlic', 'type': 'Garlic'}, {'name': 'Granulated Sugar', 'type': 'Granulated Sugar'}, {'urls': ['https://www.amazon.com/8oz-Salted-Turnip-Pack/dp/B01578SHHW'], 'name': 'Salted Turnip', 'type': 'Salted Turnip'}, {'name': 'Lime', 'type': 'Lime'}, {'urls': ['https://www.youtube.com/watch?v=dBSmCwUXZF0'], 'name': 'Garlic Chives', 'type': 'Chives'}, {'name': 'Chicken', 'type': 'Chicken'}, {'name': 'Banana Leaf', 'type': 'Banana Leaf'}, {'name': 'Tamarind Paste', 'type': 'Tamarind Paste'}, {'name': 'Sweet Paprika', 'type': 'Paprika'}, {'name': 'Lime juice', 'type': 'Lime juice'}, {'name': 'Dried Shrimp', 'type': 'Seafood', 'photos': ['https://photos.google.com/photo/AF1QipMJV_m1w-qezTjSZAmu6Vam_PKMR6GICW6TJ883', 'https://www.google.com/search?sca_esv=579651652&sxsrf=AM9HkKlBKUS5rDWtKoKSgxss4PSHC4u0jA:1699211859653&q=bdmp+dried+shrimp&tbm=isch&source=lnms&sa=X&sqi=2&ved=2ahUKEwiUtKu6ya2CAxVFIjQIHXeICOQQ0pQJegQIDRAB&biw=1440&bih=758&dpr=2#imgrc=_WqiWb3wPqLdYM', 'https://www.youtube.com/watch?v=dBSmCwUXZF0']}, {'urls': ['https://thewoksoflife.com/wp-content/uploads/2020/07/chili-oil-recipe-18.jpg', 'https://www.amazon.com/%E8%80%81%E5%B9%B2%E5%A6%88%E9%A6%99%E8%BE%A3%E8%84%86%E6%B2%B9%E8%BE%A3%E6%A4%92-Spicy-Chili-Crisp-7-41/dp/B07VHKTTR3/ref=asc_df_B07VHKTTR3/?tag=hyprod-20&linkCode=df0&hvadid=642112947349&hvpos=&hvnetw=g&hvrand=12580253979732381700&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9061293&hvtargid=pla-1951193779579&psc=1', 'https://www.google.com/search?sca_esv=580857096&sxsrf=AM9HkKmLh9FDQ0x5jNY12kJCSSbwO6Q3FA:1699539552211&q=thai+and+true+hot+chili&tbm=isch&source=lnms&sa=X&ved=2ahUKEwiJ8KiajreCAxWqAjQIHaMBDKYQ0pQJegQIDBAB&biw=1440&bih=754&dpr=2#imgrc=KDhcVOHe9yNjkM', 'https://photos.google.com/photo/AF1QipMQPtIdU1_m3SkgBWs5RcE2QXFs2OnbbJAdaG9M'], 'name': 'Chili Sauce', 'type': 'Chili Sauce'}, {'name': 'Bean Sprouts', 'type': 'Bean Sprouts'}, {'name': 'Vegetable Oil', 'type': 'Vegetable Oil'}, {'name': 'Eggs', 'type': 'Eggs'}, {'urls': ['https://www.google.com/search?q=Rice%20Sticks'], 'name': 'Rice Sticks', 'type': 'Rice Noodles'}, {'name': 'Rice Wine Vinegar', 'type': 'Rice Wine Vinegar'}, {'name': 'Thai-style Baked Tofu', 'type': 'Tofu'}]}
{'r': {'urls': ['https://www.youtube.com/watch?v=9ANH-tkkBrg'], 'name': 'Pad Thai'}, 'products': [{'name': 'Shrimp', 'type': 'Shrimp'}, {'name': 'Lime', 'type': 'Lime'}, {'name': 'Grounded Roasted Peanuts', 'type': 'Peanuts'}, {'name': 'Dried Shrimp', 'type': 'Seafood', 'photos': ['https://photos.google.com/photo/AF1QipMJV_m1w-qezTjSZAmu6Vam_PKMR6GICW6TJ883', 'https://www.google.com/search?sca_esv=579651652&sxsrf=AM9HkKlBKUS5rDWtKoKSgxss4PSHC4u0jA:1699211859653&q=bdmp+dried+shrimp&tbm=isch&source=lnms&sa=X&sqi=2&ved=2ahUKEwiUtKu6ya2CAxVFIjQIHXeICOQQ0pQJegQIDRAB&biw=1440&bih=758&dpr=2#imgrc=_WqiWb3wPqLdYM', 'https://www.youtube.com/watch?v=dBSmCwUXZF0']}, {'name': 'Rice Stick Noodles', 'type': 'Rice Noodles'}, {'urls': ['https://www.youtube.com/watch?v=dBSmCwUXZF0'], 'name': 'Garlic Chives', 'type': 'Chives'}, {'name': 'Palm Sugar', 'type': 'Sugar'}, {'urls': ['https://photos.google.com/photo/AF1QipMTNoAmEBIUBgJiziw2Tl16y2KscVqpjfDGlS-q', 'https://photos.google.com/photo/AF1QipPd47xo0JnbBdfR9pbd6FgvPRvxghQoP_wmWxph'], 'name': 'Tamarind Liquid', 'type': 'Tamarind Liquid'}, {'name': 'Pressed Tofu', 'type': 'Tofu'}, {'name': 'Garlic', 'type': 'Garlic'}, {'name': 'Shallots', 'type': 'Shallots'}, {'name': 'Bean Sprouts', 'type': 'Bean Sprouts'}, {'urls': ['https://www.google.com/search?q=Sweetened+Radish&tbm=isch&chips=q:sweet+radish,g_1:pad+thai:jagT0YaAv9M%3D&client=emacs&hl=en&sa=X&ved=2ahUKEwj-mLvS56-CAxWKFjQIHTmHCrEQ4lYoAHoECAEQNQ&biw=1440&bih=758#imgrc=8T2ZeEeH0IL-QM'], 'name': 'Sweetened Radish', 'type': 'Sweetened Radish'}, {'name': 'Fish sauce', 'type': 'Fish Sauce'}, {'name': 'Eggs', 'type': 'Eggs'}, {'name': 'Roasted Chili Flakes', 'type': 'Chili Flakes'}]}
{'r': {'urls': ['https://www.evolvingtable.com/peanut-sauce/'], 'name': 'Peanut Sauce'}, 'products': [{'name': 'Soy sauce', 'type': 'Soy sauce'}, {'name': 'Rice vinegar', 'type': 'Vinegar'}, {'name': 'Brown Sugar', 'type': 'Sugar'}, {'name': 'Garlic', 'type': 'Garlic'}, {'name': 'Adams Peanut Butter', 'type': 'Peanut Butter'}, {'name': 'Water', 'type': 'Water'}, {'name': 'Sriracha', 'type': 'Sriracha'}]}
{'r': {'urls': ['https://hot-thai-kitchen.com/red-curry-paste/print/6752/'], 'name': 'Vegan Thai Red Curry'}, 'products': [{'name': 'Cumin seeds', 'type': 'Cumin '}, {'urls': ['https://www.google.com/search?client=emacs&sca_esv=579520937&sxsrf=AM9HkKlUrnbTZeiuHkGuxjA6wsla9_IkfQ:1699140927441&q=Makrut+Lime&tbm=isch&source=lnms&sa=X&ved=2ahUKEwir5pybwauCAxXfLTQIHYj1DqQQ0pQJegQICxAB&biw=1440&bih=758&dpr=2'], 'name': 'Makrut lime zest', 'type': 'Makrut Lime'}, {'name': 'Shallots', 'type': 'Shallots'}, {'name': 'Coriander seeds', 'type': 'Spice'}, {'name': 'Spicy dried red chilies', 'type': 'Dry Chilies'}, {'name': 'Cilantro roots', 'type': 'Cilantro'}, {'name': 'Garlic', 'type': 'Garlic'}, {'name': 'Mild dried red chilies', 'type': 'Dry Chilies'}, {'name': 'Galangal', 'type': 'Galangal'}, {'name': 'Lemongrass', 'type': 'Lemongrass'}, {'name': 'White Peppercorns', 'type': 'White Peppercorns'}, {'name': 'Shrimp Paste', 'type': 'Shrimp Paste'}]}
{'r': {'urls': ['https://www.seriouseats.com/the-best-roast-potatoes-ever-recipe'], 'name': 'The Best Crispy Roast Potatoes Ever'}, 'products': [{'name': 'Parsley', 'type': 'Parsley'}, {'name': 'Rosemary', 'type': 'Rosemary'}, {'name': 'Baking soda', 'type': 'Baking Soda'}, {'name': 'Extra-virgin olive oil', 'type': 'Olive Oil'}, {'name': 'Russet potatoes', 'type': 'Potatoes'}, {'name': 'Garlic', 'type': 'Garlic'}]}
{'r': {'urls': ['https://www.simplyrecipes.com/recipes/tomatillo_salsa_verde/?print'], 'name': 'Tomatillo Salsa Verde'}, 'products': [{'name': 'Tomatillos', 'type': 'Tomatillos'}, {'name': 'Garlic', 'type': 'Garlic'}, {'name': 'Lime juice', 'type': 'Lime juice'}, {'name': 'Cilantro', 'type': 'Cilantro'}, {'name': 'Salt', 'type': 'Salt'}, {'name': 'White Onion', 'type': 'Onion'}, {'name': 'Jalapeno Pepper', 'type': 'Pepper'}]}
{'r': {'urls': ['https://cookieandkate.com/sugar-snap-pea-and-carrot-soba-noodles/print/23556/'], 'name': 'Sugar Snap Pea and Carrot Soba Noodles'}, 'products': [{'name': 'Bell Pepper', 'type': 'Bell Pepper'}, {'name': 'Soba Noodles', 'type': 'Soba Noodles'}, {'name': 'Sweet White Miso', 'type': 'Miso'}, {'name': 'Tamari', 'type': 'Tamari'}, {'name': 'Carrots', 'type': 'Carrots'}, {'name': 'Sriracha', 'type': 'Sriracha'}, {'name': 'Honey', 'type': 'Honey'}, {'name': 'Bell Pepper', 'type': 'Bell Pepper'}, {'name': 'Soba Noodles', 'type': 'Soba Noodles'}, {'name': 'Lime', 'type': 'Lime'}, {'name': 'Cilantro', 'type': 'Cilantro'}, {'name': 'Toasted Sesame Oil', 'type': 'Sesame Oil'}, {'name': 'Tamari', 'type': 'Tamari'}, {'name': 'Peanut Oil', 'type': 'Oil'}, {'name': 'Ginger', 'type': 'Ginger'}, {'name': 'Cilantro', 'type': 'Cilantro'}, {'name': 'Tamari', 'type': 'Tamari'}, {'name': 'Tamari', 'type': 'Tamari'}, {'name': 'Edamame', 'type': 'Edamame'}, {'name': 'Edamame', 'type': 'Edamame'}, {'name': 'Soba Noodles', 'type': 'Soba Noodles'}, {'name': 'Sesame Seeds', 'type': 'Sesame Seeds'}, {'name': 'Sesame Seeds', 'type': 'Sesame Seeds'}, {'name': 'Soba Noodles', 'type': 'Soba Noodles'}, {'name': 'Lime', 'type': 'Lime'}, {'name': 'Sugar Snap Peas', 'type': 'Sugar Snap Peas'}, {'name': 'Sugar Snap Peas', 'type': 'Sugar Snap Peas'}]}
{'r': {'urls': ['https://youtu.be/HJPRPEJY2WM?t=265', 'https://natashaskitchen.com/fresh-spring-rolls/', 'https://natashaskitchen.com/wprm_print/72895'], 'name': 'Fresh Spring Rolls'}, 'products': [{'name': 'Dry rice noodles', 'type': 'Rice Noodles'}, {'name': 'Carrots', 'type': 'Carrots'}, {'name': 'Rice Wine Vinegar', 'type': 'Rice Wine Vinegar'}, {'urls': ['https://www.google.com/search?client=emacs&sca_esv=581269367&sxsrf=AM9HkKkz3fh-g6VKFw7SQLjSbKO7bO0n2g:1699640340645&q=Chili+Garlic+Sauce&tbm=isch&source=lnms&sa=X&ved=2ahUKEwjB3P_VhbqCAxW9FjQIHQ6rDewQ0pQJegQIDRAB&biw=1440&bih=754&dpr=2'], 'name': 'Huy Fong Chili Garlic Sauce', 'type': 'Chili Garlic Sauce'}, {'name': 'Lime juice', 'type': 'Lime juice'}, {'name': 'Round Rice Paper Sheets', 'type': 'Round Rice Paper Sheets'}, {'name': 'Granulated Sugar', 'type': 'Granulated Sugar'}, {'name': 'Three Crabs Fish Sauce', 'type': 'Three Crabs Fish Sauce'}, {'name': 'Water', 'type': 'Water'}, {'name': 'Green lettuce', 'type': 'Lettuce'}, {'name': 'Cucumber', 'type': 'Cucumber'}, {'name': 'Garlic', 'type': 'Garlic'}, {'name': 'Shredded Carrot', 'type': 'Shredded Carrot'}, {'name': 'Shrimp', 'type': 'Shrimp'}, {'name': 'Cilantro', 'type': 'Cilantro'}]}
{'r': {'urls': ['https://www.youtube.com/watch?v=t-Hj2pILMz4', 'https://prohomecooks.com/blogs/all/why-every-cook-should-master-chicken-teriyaki?_pos=1&_sid=7db443900&_ss=r'], 'name': 'Chicken Teriyaki Recipe'}, 'products': [{'name': 'Cornstarch', 'type': 'Cornstarch'}, {'name': 'Thai-style Baked Tofu', 'type': 'Tofu'}, {'name': 'Ginger', 'type': 'Ginger'}, {'name': 'Rice', 'type': 'Rice'}, {'name': 'Cooking Oil', 'type': 'Oil'}, {'name': 'Water', 'type': 'Water'}, {'name': 'Water', 'type': 'Water'}, {'name': 'Chicken Thighs', 'type': 'Chicken'}, {'name': 'Red Onion', 'type': 'Onion'}, {'name': 'Broccolini', 'type': 'Broccolini'}, {'name': 'Sesame Seeds', 'type': 'Sesame Seeds'}, {'name': 'Rice Wine Vinegar - Kikkoman Mirin', 'type': 'Vinegar'}, {'name': 'Garlic', 'type': 'Garlic'}, {'name': 'Red Pepper', 'type': 'Bell Pepper'}, {'name': 'Soy sauce', 'type': 'Soy sauce'}]}
{'r': {'urls': ['https://www.joshuaweissman.com/post/easy-authentic-thai-green-curry', 'https://photos.google.com/photo/AF1QipMJV_m1w-qezTjSZAmu6Vam_PKMR6GICW6TJ883'], 'name': 'The Best Green Curry'}, 'products': [{'name': 'Garlic cloves', 'type': 'Garlic'}, {'urls': ['https://www.fredmeyer.com/p/simple-truth-organic-thai-basil/0001111001922'], 'name': 'Thai basil', 'type': 'Herb'}, {'name': 'White Peppercorns', 'type': 'White Peppercorns'}, {'urls': ['https://www.wholefoodsmarket.com/product/kaffir-lime%20leaves-b07q8ldbvj', 'https://www.youtube.com/watch?v=4Qz5nC-DcKk', 'https://www.safeway.com/shop/marketplace/product-details.970537048.html', 'https://photos.google.com/photo/AF1QipPI_6_YxYIuCSAvP93sDoRcyFDjekCQjNSb3Ln0', 'https://photos.google.com/photo/AF1QipPd_yNuI9VcQAFOwMSuvBx40o_sl4gAmCgBYNIQ', 'https://www.youtube.com/watch?v=SB3AV7oHKiE'], 'name': 'Kaffir lime leaves', 'type': 'Kaffir Lime Leaves'}, {'name': 'Galangal', 'type': 'Galangal'}, {'name': 'Thai Eggplant', 'type': 'Thai Eggplant'}, {'name': 'Chicken Thighs', 'type': 'Chicken'}, {'name': 'Fried shallots', 'type': 'Condiment'}, {'name': 'Serranos', 'type': 'Serrano Peppers'}, {'name': 'Lime', 'type': 'Lime'}, {'urls': ['https://www.google.com/search?sca_esv=579007228&sxsrf=AM9HkKkqQcpTokvs8EUmjT-DnZNXV9I6Lw:1698970375605&q=kaffir+lime&tbm=isch&source=lnms&sa=X&ved=2ahUKEwiH6eLtxaaCAxVnMDQIHZ94DUYQ0pQJegQIDhAB&biw=1440&bih=758&dpr=2'], 'name': 'Kaffir Lime', 'type': 'Kaffir Lime'}, {'name': 'Lemongrass', 'type': 'Lemongrass'}, {'name': 'Shallots', 'type': 'Shallots'}, {'name': 'Cilantro', 'type': 'Cilantro'}, {'name': 'Palm Sugar', 'type': 'Sugar'}, {'name': 'Fish sauce', 'type': 'Fish Sauce'}, {'urls': ['https://www.eatingthaifood.com/thai-nam-prik-kapi-recipe/'], 'comments': ['thaiShrimpPasteComment1', 'thaiShrimpPasteComment2'], 'name': 'Dried Thai shrimp paste', 'google': ['shrimp paste kapi OR gabi OR gkabi'], 'type': 'Condiment'}, {'name': 'Cumin seeds', 'type': 'Cumin '}, {'name': 'Coriander seeds', 'type': 'Spice'}, {'name': 'Full fat coconut milk', 'type': 'Coconut Milk'}, {'name': 'Chicken stock', 'type': 'Stock'}, {'name': 'Snow peas', 'type': 'Snow Peas'}]}
{'r': {'urls': ['https://www.myfoodchannel.com/thai-eggplant-recipe/', 'https://www.youtube.com/watch?v=7a0IAC7pCgA'], 'name': 'Thai Eggplant Recipe'}, 'products': [{'name': 'Coriander powder', 'type': 'Spice'}, {'name': 'Red Bell Pepper', 'type': 'Bell Pepper'}, {'name': 'Lime juice', 'type': 'Lime juice'}, {'name': 'Salt', 'type': 'Salt'}, {'name': 'Ginger', 'type': 'Ginger'}, {'name': 'Lemongrass', 'type': 'Lemongrass'}, {'urls': ['https://www.fredmeyer.com/p/simple-truth-organic-thai-basil/0001111001922'], 'name': 'Thai basil', 'type': 'Herb'}, {'name': 'Onion', 'type': 'Onion'}, {'name': 'Garlic cloves', 'type': 'Garlic'}, {'name': 'Full fat coconut milk', 'type': 'Coconut Milk'}, {'name': 'Chili powder', 'type': 'Spice'}, {'name': 'Coconut Oil', 'type': 'Coconut Oil'}, {'name': 'Chicken stock', 'type': 'Stock'}, {'name': 'Thai Eggplant', 'type': 'Thai Eggplant'}, {'name': 'Thai chilies', 'type': 'Pepper'}, {'name': 'Turmeric', 'type': 'Turmeric'}]}
{'r': {'urls': ['https://hot-thai-kitchen.com/creamy-tom-yum/print/6203/', 'https://hot-thai-kitchen.com/creamy-tom-yum/', 'https://www.youtube.com/watch?v=hhcYNjeQ_XY&list=PLaS2Ffd8cyD7SL49uWtqbfuUBmLi9nVup'], 'name': 'Tom Yum Goong'}, 'products': [{'urls': ['https://photos.google.com/photo/AF1QipM0ragYoS8EjrRngQukQJH_U1hnen_ACdJyMqEV'], 'name': 'Jasmine Rice', 'type': 'Jasmine Rice'}, {'name': 'Thai chilies', 'type': 'Pepper'}, {'name': 'Evaporated Milk', 'type': 'Evaporated Milk'}, {'name': 'Oyster Mushrooms', 'type': 'Oyster Mushroom'}, {'name': 'Galangal', 'type': 'Galangal'}, {'name': 'Water', 'type': 'Water'}, {'urls': ['https://www.wholefoodsmarket.com/product/kaffir-lime%20leaves-b07q8ldbvj', 'https://www.youtube.com/watch?v=4Qz5nC-DcKk', 'https://www.safeway.com/shop/marketplace/product-details.970537048.html', 'https://photos.google.com/photo/AF1QipPI_6_YxYIuCSAvP93sDoRcyFDjekCQjNSb3Ln0', 'https://photos.google.com/photo/AF1QipPd_yNuI9VcQAFOwMSuvBx40o_sl4gAmCgBYNIQ', 'https://www.youtube.com/watch?v=SB3AV7oHKiE'], 'name': 'Kaffir lime leaves', 'type': 'Kaffir Lime Leaves'}, {'name': 'Fish sauce', 'type': 'Fish Sauce'}, {'name': 'Lime juice', 'type': 'Lime juice'}, {'urls': ['https://youtu.be/hhcYNjeQ_XY?list=PLaS2Ffd8cyD7SL49uWtqbfuUBmLi9nVup&t=433'], 'name': 'Mae Ploy Thai Chili Paste in Oil', 'type': 'Thai Chili Paste'}, {'urls': ['https://www.youtube.com/watch?v=hhcYNjeQ_XY&list=PLaS2Ffd8cyD7SL49uWtqbfuUBmLi9nVup'], 'name': 'Sawtooth Coriander', 'type': 'Sawtooth Coriander'}, {'name': 'Shrimp', 'type': 'Shrimp'}, {'name': 'Lemongrass', 'type': 'Lemongrass'}]}
{'r': {'urls': ['https://christieathome.com/wprm_print/3534'], 'name': 'Vietnamese Spring Rolls (Gỏi Cuốn)'}, 'products': [{'name': 'Dry-Roasted Peanuts', 'type': 'Peanuts'}, {'name': 'Rice paper', 'type': 'Rice Paper'}, {'name': 'Ginger', 'type': 'Ginger'}, {'name': 'Adams Peanut Butter', 'type': 'Peanut Butter'}, {'name': 'Green lettuce', 'type': 'Lettuce'}, {'name': 'Lee Kum Kee Sauce Hoisin', 'type': 'Lee Kum Kee Sauce Hoisin'}, {'name': 'Garlic', 'type': 'Garlic'}, {'name': 'Shrimp', 'type': 'Shrimp'}, {'name': 'Water', 'type': 'Water'}, {'urls': ['https://photos.google.com/photo/AF1QipPPETrmRSh8-h9guEbb90DRig4g_njAUvQ50Ol6', 'https://photos.google.com/photo/AF1QipMYLPcT9Oybki3TQGztAT1X5tIxpknKSJ0ZmdlP', 'https://www.amazon.com/Fresh-Stick-Vermicelli-SIMPLY-FOOD/dp/B08NXVTFTP/ref=asc_df_B08NXVTFTP/?tag=hyprod-20&linkCode=df0&hvadid=652498065761&hvpos=&hvnetw=g&hvrand=10598234170837115346&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9061293&hvtargid=pla-2065471401768&psc=1', 'https://www.amazon.com/Fresh-Stick-Vermicelli-SIMPLY-FOOD/dp/B08NXVTFTP/ref=asc_df_B08NXVTFTP/?tag=hyprod-20&linkCode=df0&hvadid=652498065761&hvpos=&hvnetw=g&hvrand=10598234170837115346&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9061293&hvtargid=pla-2065471401768&psc=1'], 'name': 'Rice vermicelli', 'type': 'Rice vermicelli'}, {'note': 'added to frezer Nov 6 2023', 'urls': ['https://photos.google.com/photo/AF1QipNrbFzt7g3nCOVFOmFa6geW-HODg2hilRdq4xl0'], 'name': 'Mint leaves', 'type': 'Mint'}, {'name': 'Vegetable Oil', 'type': 'Vegetable Oil'}]}
{'r': {'urls': ['https://lifemadesimplebakes.com/wprm_print/25731'], 'name': 'Yellow Coconut Curry Chicken'}, 'products': [{'urls': ['https://www.google.com/search?q=Yellow+Curry+Powder+near+me&tbm=isch&ved=2ahUKEwiVxLm7h6mCAxWIFjQIHTNwBKoQ2-cCegQIABAA&oq=Yellow+Curry+Powder+near+me&gs_lcp=CgNpbWcQAzIHCAAQGBCABDoECCMQJzoGCAAQBxAeOgYIABAIEB46BAgAEB46BggAEAUQHlDIBViIEGD3EWgAcAB4AIABS4gBkQSSAQE5mAEAoAEBqgELZ3dzLXdpei1pbWfAAQE&sclient=img&ei=QoxFZZWbEYit0PEPs-CR0Ao&bih=758&biw=1440&client=emacs'], 'name': 'Yellow Curry Powder', 'type': 'Spice'}, {'name': 'Carrots', 'type': 'Carrots'}, {'urls': ['https://www.wholefoodsmarket.com/product/maesri-red-curry-paste-b0013esw84', 'https://www.safeway.com/shop/product-details.970519982.html?cmpid=ps_swy_sea_ecom_goo_20200924_71700000073186042_58700007112018081_92700063963421736&r=https%3A%2F%2Fwww.google.com%2F'], 'name': 'Maesri Thai Red Curry Paste', 'type': 'Curry Paste'}, {'name': 'Russet Potatoes', 'type': 'Russet Potatoe'}, {'name': 'Garlic', 'type': 'Garlic'}, {'name': 'Yellow Onion', 'type': 'Onion'}, {'name': 'Chicken Breast', 'type': 'Chicken'}, {'name': 'Brown Sugar', 'type': 'Sugar'}, {'name': 'Full fat coconut milk', 'type': 'Coconut Milk'}, {'name': 'Rice', 'type': 'Rice'}, {'name': 'Coconut Oil', 'type': 'Coconut Oil'}, {'name': 'Fish sauce', 'type': 'Fish Sauce'}, {'name': 'Chicken Broth', 'type': 'Broth'}, {'name': 'Cilantro', 'type': 'Cilantro'}]}
{'r': {'urls': ['https://www.templeofthai.com/recipes/yellow_chicken_curry.php'], 'name': 'Yellow Curry with Chicken'}, 'products': [{'name': 'Curry Powder', 'type': 'Curry Powder'}, {'name': 'Chicken', 'type': 'Chicken'}, {'name': 'Shrimp Paste', 'type': 'Shrimp Paste'}, {'name': 'Shallots', 'type': 'Shallots'}, {'name': 'Fried shallots', 'type': 'Condiment'}, {'name': 'Cumin seeds', 'type': 'Cumin '}, {'name': 'Potatoes', 'type': 'Potatoe'}, {'name': 'Full fat coconut milk', 'type': 'Coconut Milk'}, {'name': 'Ginger', 'type': 'Ginger'}, {'name': 'Sea Salt', 'type': 'Seasoning'}, {'name': 'Lemongrass', 'type': 'Lemongrass'}, {'name': 'Fish sauce', 'type': 'Fish Sauce'}, {'name': 'Coriander seeds', 'type': 'Spice'}, {'name': 'Dried Thai Chilis', 'type': 'Thai Chilies'}, {'name': 'Garlic', 'type': 'Garlic'}, {'urls': ['https://www.safeway.com/shop/product-details.960076294.html'], 'name': 'Yellow Curry Paste', 'type': 'Curry Paste'}, {'name': 'Galangal', 'type': 'Galangal'}]}
{'r': {'urls': ['https://drivemehungry.com/wprm_print/13748'], 'name': '7-Minute Zaru Soba (Cold Soba Noodles)'}, 'products': [{'name': 'Soba Noodles', 'type': 'Soba Noodles'}, {'urls': ['https://www.amazon.com/Kikkoman-Japanese-Noodle-Soup-Tsuyu/dp/B002Z3F0IW', 'https://www.google.com/search?q=kikkoman+japanese+noodle+soup+base(hon+tsuyu)&oq=Kikkoman+Japanese+Noodle+Soup+Base(Hon+Tsuyu)&gs_lcrp=EgZjaHJvbWUqBwgAEAAYgAQyBwgAEAAYgAQyBwgBEAAYgAQyCggCEAAYhgMYigUyCggDEAAYhgMYigUyBggEEEUYPDIGCAUQRRg9MgYIBhBFGD3SAQc0NzBqMGo0qAIAsAIA&sourceid=chrome&ie=UTF-8', 'https://www.youtube.com/watch?v=61nPpDkz1AI'], 'name': 'Kikkoman Japanese Noodle Soup Base (Hon Tsuyu)', 'type': 'Sauce', 'manufacturer': 'Kikkoman'}, {'name': 'Wasabi', 'type': 'Wasabi'}, {'name': 'Ice-cold water', 'type': 'Water'}, {'name': 'Sesame Seeds', 'type': 'Sesame Seeds'}, {'name': 'SWEET preserved daikon radish', 'type': 'Radish'}, {'urls': ['https://www.google.com/search?client=emacs&sca_esv=577922779&sxsrf=AM9HkKkUxzT-KjHg9ziVgvqz5Zsqmn7xdw:1698703946500&q=Japanese+nori&tbm=isch&source=lnms&sa=X&ved=2ahUKEwi647yq5Z6CAxVxMjQIHRW8BBYQ0pQJegQIChAB&biw=1440&bih=758&dpr=2'], 'name': 'Japanese Nori', 'type': 'Nori'}]}
{'r': {'urls': ['https://www.cookerru.com/wprm_print/7756'], 'name': '10-Minute Zaru Soba (Cold Soba Noodles)'}, 'products': [{'name': 'Green Onion', 'type': 'Onion'}, {'name': 'Soy sauce', 'type': 'Soy sauce'}, {'name': 'Soba Noodles', 'type': 'Soba Noodles'}, {'name': 'SWEET preserved daikon radish', 'type': 'Radish'}, {'name': 'Mirin', 'type': 'Mirin'}, {'name': 'Egg yolk', 'type': 'Egg yolk'}, {'name': 'Toasted sesame flakes', 'type': 'Garnish'}, {'name': 'Toasted Seaweed', 'type': 'Seaweed'}, {'name': 'Wasabi', 'type': 'Wasabi'}, {'name': 'Granulated Sugar', 'type': 'Granulated Sugar'}]}
```

# find products whose type contains vegetable

``` example
MATCH (p:Product)
WHERE toLower(p.type) CONTAINS 'vegetable'
RETURN p.name AS ProductName, p.type AS Type
;
```

Results:

``` example
{'ProductName': 'Beansprouts', 'Type': 'Vegetable'}
{'ProductName': 'Vegetable Oil', 'Type': 'Vegetable Oil'}
```

# find products whose type contains peas

``` example
MATCH (p:Product)
WHERE toLower(p.type) CONTAINS 'peas'
RETURN p.name AS ProductName, p.type AS Type
;
```

Results:

``` example
{'ProductName': 'Chickpeas', 'Type': 'Chickpeas'}
{'ProductName': 'Frozen Peas', 'Type': 'Peas'}
{'ProductName': 'Snow peas', 'Type': 'Snow Peas'}
{'ProductName': 'Sugar Snap Peas', 'Type': 'Sugar Snap Peas'}
```