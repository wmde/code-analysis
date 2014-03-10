Progress between the [ECR in April 2013](http://meta.wikimedia.org/wiki/Wikidata/Development/Code_Review)
and now, March 2014.

Preliminary notes for creation of presentation. Does not include the JS.

2014-03-10. WB: eb334f65ffc691d3a2c33aa2cba28df15aa2c390



Also include https://www.mediawiki.org/wiki/User:Jeroen_De_Dauw/Wikibase.git_cleanup




## Wikibase phploc diff

minus DM

* NCLOC: 58k -> 95k
* Classes: 417 -> 602
* Files: 466 -> 654
* Abstract classes: 10% -> 6%
* Class length: 100 -> 112 NCLOC

* Static: 11.3% -> 8.7%
* Public: 74% -> 68%
* Method NCLOC: 16 -> 17
* Complexity/Methods: 2.12 -> 2.02



## Tools

* TravisCI
    * PHP versions
    * Databases

* Composer

* Packagist

* ScrutinizerCI

* Coverage tracking



## Testing

* Now for "all" code

* Mock API was not used, now is. Includes some non-optimal usage

* High complexity in test classes, unchanged

* Test quality not high - should be kept clean and refactored

* Education on basic testing principles advised




## Frequent problems

### static scoping

Hooks, misplaced code

Slowly decreasing, topic understood

### lack of lifecycle control

API, SpecialPage, Action, Job, ContentHandler, Maintenance etc

Some new introductions, some partial cleanup.
Estimate technical debt remained similar.
Suggest more education and dicipline. Also DI contained in WB.git apps

### SRP violation

More awareness, new issues still being introduced. More education would be good.

### inheritance code reuse

API, SpecialPage, Entity, test cases

Complexity, LSP

Encouraged by lack of lifecycle control



## Further issues

### Singleton

* Few, deprecated
* No new ones added, some removed
* Only a few to go

### Namespace usage

* PSR-0 was suggested
* PSR-0/4 now used in all code except WB.git and part of Diff
* Suggested migration strategy: fix as boundaries are drawn



## Ball of mud

* No boundaries
* State in Lib



## DI

* Libraries use DI (+factories), no remaining issues there

* Repo and Client factories
    * Abused for non-construction
    * Splits and composition did not occur
    * Bound to from many places in codebase, no consistent effort in fixing this

* DI approach from ECR implemented in WB Query
    * Examples for API, SpecialPage and Hook in place



## Serialization

* ECR identified recursion problem in DVs, used suggested solution in Ask
* Decoupling of serialization code from value objects was suggested
* Now migrating serialization code to new format also used in Ask to reduce technical debt
* Makes in-demand reuse possible



## Action items

* Implement DI -> partial
* Refactor EditEntity (API) using DI -> fail
* Refactor WB hooks using DI -> fail



## Concrete PHP

### DispatchChanges

ECR

* bad function level code (4 levels nest)
* very high complexity
* dispatched to many non-public methods -> hard to test

Now

* No changes
* Clearly does to much
* Is its own app, should be moved out of Lib

### ChangeHandler

ECR

* High complexity
* Should be split
* Singleton
* Doing stuff in constructor
    * Null object pattern suggested

Now

* No changes
* Does not belong in Lib, move to Repo

### Wikibase\Api\EditEntity::modifyEntity

NPath 17,336,096, project max

Many function level issues.

Definitely better now. Still not clean, though not critical (class is though).

### GeoCoordinateParser

ECR

* Possible SRP violation
* Suggest splitting and dispatching

Done now

### MapDiffer

* Highest complexity
* No changes recommended, just good tests

* Complexity was reduced via method extraction

### ListDiffer

* Suggested using strategy for different approaches
* Strategy implemented

