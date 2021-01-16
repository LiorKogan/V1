## V1 Patterns - JSON-based Representation

## V1 pattern element types

| JSON name  | V1 pattern element type | visual representation
|------------|-------------------------|----------------------
| Start      | [Pattern Start](#pattern-start-type--start) | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element01.png)
| Concrete   | [Concrete Entity](#concrete-entity-type--concrete) | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element02.png)
| Typed      | [Typed Entity](#typed-entity-type--typed) | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element03.png)
| Untyped    | [Untyped Entity](#untyped-entity-type--untyped) | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element04.png)
| Rel        | [Relationship](#relationship-type--rel) | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element07.png)
| EExpr      | [Entity's Expression](#entitys-expression-type--eexpr)       | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element08.png)
| RExpr      | [Relationship's Expression](#relationships-expression-type--rexpr) | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element09.png)
| Quant      | [Quantifier](#quantifier-type--quant) | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element10.png)
| Comb       | [Combiner](#combiner-type--comb) | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element13.png)
| Path       | [Path](#path-type--path) | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element14.png)
| PathSeg    | [Path Segment](#path-segment-type--pathseg) | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element15.png)
| HQuant     | [Horizontal Quantifier](#horizontal-quantifier-type--hquant) | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element16.png)
| HComb      | [Horizontal Combiner](#horizontal-combiner-type--hcomb) | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element17.png)
| A1         | [A1 Aggregator](#a1-aggregator-type--a1) | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element31.png)
| A2         | [A2 Aggregator](#a2-aggregator-type--a2) | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element32.png)
| A3         | [A3 Aggregator](#a3-aggregator-type--a3) | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element33.png)
| M1         | [M1 Aggregator](#m1-aggregator-type--m1) | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element41.png)
| M2         | [M2 Aggregator](#m2-aggregator-type--m2) | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element42.png)
| M3         | [M3 Aggregator](#m3-aggregator-type--m3) | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element43.png)
| R1         | [R1 Aggregator](#r1-aggregator-type--r1) | ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/Element45.png)

## Wrappers (for Rel / Path / Quant)

**X** - Negator                   ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/WrapperX.png)
**N** - relationship/path negator ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/WrapperN.png)
**O** - Optional                  ![V1](https://raw.githubusercontent.com/LiorKogan/V1/master/Elements/WrapperO.png)

## JSON structure

|Mandatory| Name         | Type   | Description
|---------|--------------|--------| ------
| +       | schema       | string | Schema name
| +       | name         | string | Pattern name
| +       | elements     | [...]  | Elements composing the pattern
|         | nonidentical | [...]  | nonidenticality constraints between entity tags
|         | order        | [...]  | order constraints between entity tags

**JSON fields: for each pattern element:**

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | elNum     | int    | Element number. Distinct value for each pattern element
| +       | type      | string | Element type (e.g. "Start", "Concrete", "Quant")

**Additional JSON fields for pattern elements:**

## Pattern Start (type = "Start")

There must be a single element with type "Start". Its elNum must be 0.

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | next      | int    | elNum of next element. <br> Valid element types: Concrete, Typed, Untyped, Quant
|         | chained   | int    | elNum of chained element. <br> Valid element types: A1, A3, M1, M3

## Concrete Entity (type = "Concrete")

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | eTag      | string | entity tag (e.g. "A")
| +       | eID       | string | Technical ID of the entity
| +       | eType     | int    | Entity type (e.g. of 'Person') <br> According to the schema (eType)
| +       | eName     | string | Cached display name of the entity (e.g. "Brandon Stark"). This was the display name when the pattern was stored
|         | expLatent | bool   | true if explicit latent (default: false)
|         | next      | int    | elNum of next element. <br> Valid element types: Rel, EExpr, Quant, Path

## Typed Entity (type = "Typed")

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | eTag      | string | entity tag (e.g. "A")
| +       | eType     | int    | Entity type (e.g. of 'Person') <br> According to the schema (eType) / 0 (null entity type)
|         | expLatent | bool   | true if explicit latent (default: false)
|         | next      | int    | elNum of next element.  <br> Valid element types: Rel, EExpr, Quant, Path

## Untyped Entity (type = "Untyped")

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | eTag      | string | entity tag (e.g. "A")
|         | ett       | int    | _entity type tag_ to assign (unique positive integer)
|         | valid     | bool   | true if etts and eTypes are lists of valid types, false if lists of invalid types (default: true)
|         | etts      | [int]  | non-empty list of entity type tags
|         | eTypes    | [int]  | non-empty list of entity types. According to the schema (eType) / 0 (null entity type)
|         | expLatent | bool   | true if explicit latent (default: false)
|         | next      | int    | elNum of next element. <br> Valid element types: Rel, EExpr, Quant, Path

## Relationship (type = "Rel")

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
|         | rType     | int    | Relationship type (e.g. of 'own') <br> According to the schema (rType) <br> **either rType or [rtt] [valid] [rtts] [rTypes]**
|         | rtt       | int    | _relationship type tag_ to assign (unique positive integer)
| +       | dir       | string | "-": non-directional, "O": Outgoing to next element, "I": Incoming from next element
|         | valid     | bool   | true if rtts and rTypes are lists of valid types, false if lists of invalid types (default: true)
|         | rtts      | [int]  | non-empty list of relationship type tags
|         | rTypes    | [int]  | non-empty list of relationship types. According to the schema (rType)
|         | wrapper   | string | valid wrappers: X,N,XN,O,ON
| +       | next      | int    | elNum of next element. <br> Valid element types: Concrete, Typed, Untyped, Comb
|         | chained   | int    | elNum of chained element. <br> Valid element types: <ul><li>RExpr</li> <li>HQuant</li> <li>A1 (Rel wrappers: O,N,ON)</li> <li>A2 (Rel wrappers: O)</li> <li>A3 (Rel wrappers: O,N,ON)</li> <li>M1 (Rel wrappers: O,N,ON)</li> <li>M2 (Rel wrappers: O)</li> <li>M3 (Rel wrappers: O,N,ON)</li> <li>R1 (Rel wrappers: O)</li> </ul> 

## Entity's Expression (type = "EExpr") 

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | EAtag     | int    | EA tag to assign (unique positive integer)
| +       | expr      | string | Entity's expression. <br> Property types are according to the schema (pType) <br> for untyped entity: property types should be valid for all valid (implicitly and explicitly) entity types
|         | con       | {...}  | Constraint <br> op: string - Constraint operator ("=" / "≠" / "∈" / "∉" / ... / "is null" / "not null") <br> When op ≠ "is null" and op ≠ "not null": <br> - expr: string - Constraint expression <br> - null: bool - how to evaluate the constraint when the expression is evaluated to null (default: false - the constraint is not satisfied)

## Relationship's Expression (type = "RExpr")

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | EAtag     | int    | EA tag to assign (unique positive integer)
| +       | expr      | string | Relationship's Expression. <br> Property types are according to the schema (pType) (e.g. "$(1).$(.2)" for "tf.till" of "member of") <br>
|         | con       | {...}  | Constraint <br> op: string - Constraint operator ("=" / "≠" / "∈" / "∉" / ... / "is null" / "not null") <br> When op ≠ "is null" and op ≠ "not null": <br> - expr: string - Constraint expression <br> - null: bool - how to evaluate the constraint when the expression is evaluated to null (default: false - the constraint is not satisfied)
|         | chained   | int    | elNum of chained element. <br> Valid element types: <ul><li>RExpr</li> <li>HQuant</li> <li>HComb</li> <li>A1 (Rel wrappers: O,N,ON) (Not in an HQuant's branch)</li> <li>A2 (Rel wrappers: O) (Not in an HQuant's branch)</li> <li>A3 (Rel wrappers: O,N,ON) (Not in an HQuant's branch)</li> <li>M1 (Rel wrappers: O,N,ON) (Not in an HQuant's branch)</li> <li>M2 (Rel wrappers: O) (Not in an HQuant's branch)</li> <li>M3 (Rel wrappers: O,N,ON) (Not in an HQuant's branch)</li> <li>R1 (Rel wrappers: O) (Not in an HQuant's branch)</li> </ul> 

## Quantifier (type = "Quant")

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | qType     | string | "all"/"some"/"gt"/"ge"/"notall"/"none"/"eq"/"ne"/"lt"/"le"/"range"/"notrange"
|         | qVal      | int    | mandatory if qType = "gt"/"ge"/"eq"/"ne"/"lt"/"le"
|         | qVal      | [int]  | mandatory if qType = "range"/"notrange": array [int] of two elements
|         | wrapper   | string | valid wrappers: O
| +       | next      | [int]  | elNum of first element in each branch (0 or more branches). <br> Next to a sequence of Quants that is directly next to Start - <br> Valid element types: Concrete, Typed, Untyped, Quant <br> Otherwise - <br> Valid element types: Rel, Path, EExpr, Quant
|         | chained   | int    | elNum of chained element (chained to the quantifier's input) <br> Valid element types: <ul><li> A1 </li> <li> A2 </li> <li> A3 </li> <li> M1 </li> <li> M2 </li> <li> M3 </li> </ul>

## Combiner (type = "Comb")

|Mandatory| Name      | Type   | Description
|---------|-----------|--------| ------
| +       | next      | int    | elNum of next element. <br> Valid element types: Concrete, Typed, Untyped, Comb

## Path (type = "Path") 

|Mandatory| Name      | Type       | Description
|---------|-----------|------------| ------
|         | rTypes    | [{...}]    | Relationship types and constraints <br> For each: <br> _mandatory_ **rType**: int - Relationship type. According to the schema (rType) <br> _not mandatory_ **dir** - string - direction ("O"/"I") (see "Rel") <br> _not mandatory_ **con**: {...} - op: string, expr: string - constraint over a non-negative integer expression (number of relationships of this type [and direction] along the path)
|         | con       | {...}      | Constraint for path length (positive integer) <br> op: string - Constraint operator  ("=" / "<" / "≤" / "∈" only) <br>  expr: string - Constraint expression
|         | shortest  | bool       | Shortest paths only. <br> There must either be a con field or "shortest" is true
|         | eTypes    | [{...}]    | Entity types and constraints <br> For each: <br> _mandatory_ **eType**: int - entity type. According to the schema (eType) <br> _not mandatory_ **con**: {...} - op: string, expr: string - constraint over a non-negative integer expression (number of entities of this type along the path)
|         | wrapper   | string     | valid wrappers: X,N,XN,O,ON
| +       | next      | int        | elNum of next element. <br> Valid element types: Concrete, Typed, Untyped, Comb
|         | chained   | int        | elNum of chained element. <br> Valid element types: <ul> <li>A1 (valid wrappers: O,N,ON)</li> <li>A2 (valid wrappers: O)</li> <li>A3 (valid wrappers: O,N,ON)</li> <li>M1 (valid wrappers: O,N,ON)</li> <li>M2 (valid wrappers: O)</li> <li>M3(valid wrappers: O,N,ON)</li></ul> 

## Path Segment (type = "PathSeg")

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
|todo     |           |          |

## Horizontal Quantifier (type = "HQuant")

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
| +       | qType     | string   | "some"/"gt"/"ge"/"notall"/"none"/"eq"/"ne"/"lt"/"le"/"range"/"notrange"
|         | qVal      | int      | mandatory if qType = "gt"/"ge"/"eq"/"ne"/"lt"/"le"
|         | qVal      | [int]    | mandatory if qType = "range"/"notrange": array [int] of two elements
| +       | chained   | [int]    | elNum of first element in each branch (0 or more branches). <br> Valid element types: RExpr, HQuant

## Horizontal Combiner (type = "HComb")

|Mandatory| Name      | Type     | Description
|---------|-----------|----------| ------
| +       | chained   | int      | elNum of chained element. <br> Valid element types: <ul><li>RExpr</li> <li>HQuant</li> <li>HComb</li> <li>A1 (Rel wrappers: O,N,ON)</li> <li>A2 (Rel wrappers: O)</li> <li>A3 (Rel wrappers: O,N,ON)</li> <li>M1 (Rel wrappers: O,N,ON)</li> <li>M2 (Rel wrappers: O)</li> <li>M3 (Rel wrappers: O,N,ON)</li> <li>R1 (Rel wrappers: O)</li> </ul> 

## A1 Aggregator (type = "A1")

|Mandatory| Name      | Type       | Description
|---------|-----------|------------| ------
| +       | EAtag     | int        | EA tag to assign (unique positive integer)
|         | per       | {...}      | elements composing the _per_ clause: <br> eTags: [string] - one or more entity tags (e.g. "A", "B") or a single string: "<", ">" or "<>" (for ←,→,↔) <br> exprs: [string] expressions (e.g. "$(1).$(.1).date"). Property types are according to the schema (pType) <br> etts: [int] - entity type tags (e.g. 1 for "⟨1⟩") <br> rtts: [int] - relationship type tags (e.g. 1 for "⟪1⟫")
| +       | eTags     | [[string]] | entity tags in the "et × et × ... ∪ ..." clause (union of Cartesian products), or a single clause of a single string: "<", ">" or "<>" (for ←,→,↔)
|         | con       | {...}      | Constraint over non-negative integer expression <br> op: string - Constraint operator ("=" / "≠" / "∈" / "∉" / ...) <br> expr: string - Constraint expression
|         | chained   | int        | elNum of chained element. <br> Valid element types: <ul><li>A1 (Rel wrappers: O,N,ON)</li> <li>A2 (Rel wrappers: O)</li> <li>A3 (Rel wrappers: O,N,ON)</li> <li>M1 (Rel wrappers: O,N,ON)</li> <li>M2 (Rel wrappers: O)</li> <li>M3 (Rel wrappers: O,N,ON)</li> <li>R1 (Rel wrappers: O)</li> </ul> 

## A2 Aggregator (type = "A2")

|Mandatory| Name      | Type       | Description
|---------|-----------|------------| ------
| +       | EAtag     | int        | EA tag to assign (unique positive integer)
|         | per       | {...}      | elements composing the _per_ clause: <br> eTags: [string] - one or more entity tags (e.g. "A", "B") or a single string: "<", ">" or "<>" (for ←,→,↔) <br> exprs: [string] expressions (e.g. "$(1).$(.1).date"). Property types are according to the schema (pType) <br> etts: [int] - entity type tags (e.g. 1 for "⟨1⟩") <br> rtts: [int] - relationship type tags (e.g. 1 for "⟪1⟫")
|         | con       | {...}      | Constraint over non-negative integer expression <br> op: string - Constraint operator ("=" / "≠" / "∈" / "∉" / ...) <br> expr: string - Constraint expression
|         | chained   | int        | elNum of chained element. <br> Valid element types: <ul><li>A1 (Rel wrappers: O,N,ON)</li> <li>A2 (Rel wrappers: O)</li> <li>A3 (Rel wrappers: O,N,ON)</li> <li>M1 (Rel wrappers: O,N,ON)</li> <li>M2 (Rel wrappers: O)</li> <li>M3 (Rel wrappers: O,N,ON)</li> <li>R1 (Rel wrappers: O)</li> </ul> 

## A3 Aggregator (type = "A3")

|Mandatory| Name      | Type       | Description
|---------|-----------|------------| ------
| +       | EAtag     | int        | EA tag to assign (unique positive integer)
|         | per       | {...}      | elements composing the _per_ clause: <br> eTags: [string] - one or more entity tags (e.g. "A", "B") or a single string: "<", ">" or "<>" (for ←,→,↔) <br> exprs: [string] expressions (e.g. "$(1).$(.1).date"). . Property types are according to the schema (pType) <br> etts: [int] - entity type tags (e.g. 1 for "⟨1⟩") <br> rtts: [int] - relationship type tags (e.g. 1 for "⟪1⟫")
| +       | aggOp     | string     | aggregation operator: min/max/avg/sum/distinct/set/bag/union/intersection
| +       | expr      | string     | expression to aggregate. <br> Property types are according to the schema (pType) (e.g. "$(1).$(.2)" for "tf.till" of "member of") <br>
|         | con       | {...}      | Constraint <br> op: string - Constraint operator ("=" / "≠" / "∈" / "∉" / ... / "is null" / "not null") <br> When op ≠ "is null" and op ≠ "not null": <br> - expr: string - Constraint expression <br> - null: bool - how to evaluate the constraint when the expression is evaluated to null (default: false - the constraint is not satisfied)
|         | chained   | int        | elNum of chained element. <br> Valid element types: <ul><li>A1 (Rel wrappers: O,N,ON)</li> <li>A2 (Rel wrappers: O)</li> <li>A3 (Rel wrappers: O,N,ON)</li> <li>M1 (Rel wrappers: O,N,ON)</li> <li>M2 (Rel wrappers: O)</li> <li>M3 (Rel wrappers: O,N,ON)</li> <li>R1 (Rel wrappers: O)</li> </ul> 

## M1 Aggregator (type = "M1")

|Mandatory| Name      | Type       | Description
|---------|-----------|------------| ------
|         | per       | {...}      | elements composing the _per_ clause: <br> eTags: [string] - one or more entity tags (e.g. "A", "B") or a single string: "<", ">" or "<>" (for ←,→,↔) <br> exprs: [string] expressions (e.g. "$(1).$(.1).date") <br> etts: [int] - entity type tags (e.g. 1 for "⟨1⟩") <br> rtts: [int] - relationship type tags (e.g. 1 for "⟪1⟫")
| +       | k         | int        | number of min/max entities. negative value means "all but ..."
| +       | els       | {...}      | elements composing the '[all but] k ...' clause: at least one of the following: <br> eTags: [string] - one or more entity tags (e.g. "A", "B") or a single string: "<", ">" or "<>" (for ←,→,↔) <br> exprs: [string] - one or more expressions (e.g. "$(1).$(.1).date") <br> etts: [int] - on or more entity type tags (e.g. 1 for "⟨1⟩") <br> rtts: [int] - on or more relationship type tags (e.g. 1 for "⟪1⟫")
| +       | op        | string     | min/max
| +       | eTags     | [[string]] | entity tags in the 'with min/max et × et × ... ∪ ..." clause (union of Cartesian products), or a single clause of a single string: "<", ">" or "<>" (for ←,→,↔)
|         | chained   | int        | elNum of chained element. <br> Valid element types: <ul><li>A1 (Rel wrappers: O,N,ON)</li> <li>A2 (Rel wrappers: O)</li> <li>A3 (Rel wrappers: O,N,ON)</li> <li>M1 (Rel wrappers: O,N,ON)</li> <li>M2 (Rel wrappers: O)</li> <li>M3 (Rel wrappers: O,N,ON)</li> <li>R1 (Rel wrappers: O)</li> </ul> 

## M2 Aggregator (type = "M2")

|Mandatory| Name      | Type       | Description
|---------|-----------|------------| ------
|         | per       | {...}      | elements composing the _per_ clause: <br> eTags: [string] - one or more entity tags (e.g. "A", "B") or a single string: "<", ">" or "<>" (for ←,→,↔) <br> exprs: [string] expressions (e.g. "$(1).$(.1).date") <br> etts: [int] - entity type tags (e.g. 1 for "⟨1⟩") <br> rtts: [int] - relationship type tags (e.g. 1 for "⟪1⟫")
| +       | k         | int        | number of min/max entities. negative value means "all but ..."
| +       | els       | {...}      | elements composing the '[all but] k ...' clause: at least one of the following: <br> eTags: [string] - one or more entity tags (e.g. "A", "B") or a single string: "<", ">" or "<>" (for ←,→,↔) <br> exprs: [string] - one or more expressions (e.g. "$(1).$(.1).date") <br> etts: [int] - on or more entity type tags (e.g. 1 for "⟨1⟩") <br> rtts: [int] - on or more relationship type tags (e.g. 1 for "⟪1⟫")
| +       | op        | string     | min/max
|         | chained   | int        | elNum of chained element. <br> Valid element types: <ul><li>A1 (Rel wrappers: O,N,ON)</li> <li>A2 (Rel wrappers: O)</li> <li>A3 (Rel wrappers: O,N,ON)</li> <li>M1 (Rel wrappers: O,N,ON)</li> <li>M2 (Rel wrappers: O)</li> <li>M3 (Rel wrappers: O,N,ON)</li> <li>R1 (Rel wrappers: O)</li> </ul> 

## M3 Aggregator (type = "M3")

|Mandatory| Name      | Type       | Description
|---------|-----------|------------| ------
|         | per       | {...}      | elements composing the _per_ clause: <br> eTags: [string] - one or more entity tags (e.g. "A", "B") or a single string: "<", ">" or "<>" (for ←,→,↔) <br> exprs: [string] expressions (e.g. "$(1).$(.1).date") <br> etts: [int] - entity type tags (e.g. 1 for "⟨1⟩") <br> rtts: [int] - relationship type tags (e.g. 1 for "⟪1⟫")
| +       | k         | int        | number of min/max entities. negative value means "all but ..."
| +       | els       | {...}      | elements composing the '[all but] k ...' clause: at least one of the following: <br> eTags: [string] - one or more entity tags (e.g. "A", "B") or a single string: "<", ">" or "<>" (for ←,→,↔) <br> exprs: [string] - one or more expressions (e.g. "$(1).$(.1).date") <br> etts: [int] - on or more entity type tags (e.g. 1 for "⟨1⟩") <br> rtts: [int] - on or more relationship type tags (e.g. 1 for "⟪1⟫")
| +       | op        | string     | min/max
| +       | expr      | string     | expression to aggregate. <br> Property types are according to the schema (pType) (e.g. "$(1).$(.2)" for "tf.till" of "member of") <br>
|         | chained   | int        | elNum of chained element. <br> Valid element types: <ul><li>A1 (Rel wrappers: O,N,ON)</li> <li>A2 (Rel wrappers: O)</li> <li>A3 (Rel wrappers: O,N,ON)</li> <li>M1 (Rel wrappers: O,N,ON)</li> <li>M2 (Rel wrappers: O)</li> <li>M3 (Rel wrappers: O,N,ON)</li> <li>R1 (Rel wrappers: O)</li> </ul> 

## R1 Aggregator (type = "R1")

|Mandatory| Name      | Type       | Description
|---------|-----------|------------| ------
|         | per       | {...}      | elements composing the _per_ clause: <br> eTags: [string] - one or more entity tags (e.g. "A", "B") or a single string: "<", ">" or "<>" (for ←,→,↔) <br> exprs: [string] expressions (e.g. "$(1).$(.1).date") <br> etts: [int] - entity type tags (e.g. 1 for "⟨1⟩") <br> rtts: [int] - relationship type tags (e.g. 1 for "⟪1⟫")
| +       | k         | int        | number of min/max entities. negative value means "all but ..."
| +       | op        | string     | min/max
| +       | expr      | string     | Relationship's Expression. <br> Property types are according to the schema (pType) (e.g. "$(1).$(.2)" for "tf.till" of "member of")
|         | chained   | int        | elNum of chained element. <br> Valid element types: <ul><li>A1 (Rel wrappers: O,N,ON)</li> <li>A2 (Rel wrappers: O)</li> <li>A3 (Rel wrappers: O,N,ON)</li> <li>M1 (Rel wrappers: O,N,ON)</li> <li>M2 (Rel wrappers: O)</li> <li>M3 (Rel wrappers: O,N,ON)</li> <li>R1 (Rel wrappers: O)</li> </ul> 

## Nonidenticality Constraints between Entity Tags

The following snippet demonstrates nonidenticality constraints between entity tags "C≠E" and "C≠F":

  "nonidentical": [ 
    ["C", "E"]
    ["C", "F"]
  ]

## Order Constraints between Entity Tags

The following snippet demonstrates order constraints between entity tags "C<E" and "C<F":

  "order": [ 
    ["C", "E"]
    ["C", "F"]
  ]

## References in JSON Strings

|References         | Examples
|-------------------| --------
|EA tag             | ${n} represents EA tag _n_. To differentiate from "{n}" - a set of one element
|(sub)property      | $(n) represents property _n_ of the attached entity/relationship. <br> $(.n) represents subproperty _n_ of the last referred [sub]property. <br> $(1).$(.2) - subproperty 2 of property 1 of the attached entity/relationship <br> ${1}.$(.2) - subproperty 2 of the expression referenced by EA tag 1 <br> $(1).at(5).$(.2) - subproperty 2 of index 5 of list property 1
|categorical value  | #color(2) - second value of the categorical type 'color'
