## V1: A Visual Query Language for Property Graphs

Copyright © 2017-2023 [Lior Kogan](https://www.linkedin.com/in/liorkogan) (koganlior1 [at] gmail [dot] com)

The "V1" name is a trademark of Lior Kogan.

This work is licensed under a [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License](https://creativecommons.org/licenses/by-nc-sa/4.0/). Commercial licenses and software tools are also available from Lior Kogan.

![CC](https://upload.wikimedia.org/wikipedia/commons/1/12/Cc-by-nc-sa_icon.svg)

---

This page contains the description and the specifications of the V1 language. This content is periodically released in [arXiv](https://arxiv.org/abs/1710.04470).

V1 is named after the primary visual cortex in our brain, also known as visual area one (V1).

V1 is dedicated to my ancestors all the way back and their descendants all the way forth.

Feedback, questions, corrections, and suggestions are welcome.

---

## Table of Contents

- [Introduction](#introduction)
- [The Property Graph Mathematical Structure](#the-property-graph-mathematical-structure)
- [The Property Graph Data Model](#the-property-graph-data-model)
- [The Property Graph Schema](#the-property-graph-schema)
- [Patterns and Pattern Languages](#patterns-and-pattern-languages)
- [A Song of Ice and Fire](#a-song-of-ice-and-fire)
- [V1 Basics](#v1-basics)
- [Expressions and Expression Constraints](#expressions-and-expression-constraints)
- [Data Types, Operators, and Functions](#data-types-operators-and-functions)
- [Quantifiers](#quantifiers)
- [Entity-Tags](#entity-tags)
- [Negator](#negator)
- [Relationship/Path-Negator](#relationshippath-negator)
- [Combiner](#combiner)
- [Chains, Horizontal Quantifiers, and Horizontal Combiner](#chains-horizontal-quantifiers-and-horizontal-combiner)
- [Latent Pattern-Entities](#latent-pattern-entities)
- [Optional Components](#optional-components)
- [Untyped Entities](#untyped-entities)
- [Entity Type-Tags](#entity-type-tags)
- [Untyped Relationships](#untyped-relationships)
- [Relationship Type-Tags](#relationship-type-tags)
- [Null Entities](#null-entities)
- [Paths](#paths)
- [Shortest Paths](#shortest-paths)
- [Path Patterns](#path-patterns)
- [Referencing Expression-Tags](#referencing-expression-tags)
- [Aggregators](#aggregators)
  - [A1 Aggregator](#a1-aggregator)
  - [A2 Aggregator](#a2-aggregator)
  - [A3 Aggregator](#a3-aggregator)
- [Min/Max Aggregators](#minmax-aggregators)
  - [M1 Aggregator](#m1-aggregator)
  - [M2 Aggregator](#m2-aggregator)
  - [M3 Aggregator](#m3-aggregator)
  - [R1 Aggregator](#r1-aggregator)
- [Aggregator Chains](#aggregator-chains)
- [Aggregator Sequences](#aggregator-sequences)
- [Extended Aggregators](#extended-aggregators)
  - [Extended A1 Aggregator](#extended-a1-aggregator)
  - [Extended A2 Aggregator](#extended-a2-aggregator)
  - [Extended A3 Aggregator](#extended-a3-aggregator)
  - [Extended M1 Aggregator](#extended-m1-aggregator)
  - [Extended M2 Aggregator](#extended-m2-aggregator)
  - [Extended M3 Aggregator](#extended-m3-aggregator)
  - [Extended R1 Aggregator](#extended-r1-aggregator)
- [Multivalued Functions and Expressions](#multivalued-functions-and-expressions)
- [Application: Spatiotemporality](#application-spatiotemporality)

## Introduction

The _property graph_ is an increasingly popular data model. Pattern construction and pattern matching are important tasks when dealing with property graphs. Given a property graph schema 𝑆, a property graph 𝐺 conforming to 𝑆, and a query pattern 𝑃 conforming to 𝑆, all expressed in language 𝐿 = (𝐿<sub>𝑆</sub>, 𝐿<sub>𝐺</sub>, 𝐿<sub>𝑃</sub>, 𝐿<sub>𝑅</sub>), _pattern matching_ is the process of finding, transforming, merging, and annotating subgraphs of 𝐺 that match 𝑃. The syntaxes of sublanguages 𝐿<sub>𝑆</sub>_, 𝐿<sub>𝐺</sub>_, 𝐿<sub>𝑃</sub>_, and 𝐿<sub>𝑅</sub>_ define what and how symbols can be combined to form well-formed schemas, graphs, patterns, and query results, respectively. A semantics of 𝐿<sub>𝑃</sub> is a mapping (𝑆, 𝐺, 𝑃) → 𝑅: which subgraphs of 𝐺 match 𝑃 and how to transform, merge, and annotate them. Expressive pattern languages support topological constraints, property value constraints, negations, quantifications, aggregations, and path semantics. _Calculated properties_ may be defined for vertices, edges, and subgraphs, and constraints may be imposed on their evaluation result.

Many query posers are professionals (e.g., researchers, analysts, or investigators) who construct patterns as part of their daily work (e.g., investigative analytics). Such domain experts would like to construct patterns with minimal effort, minimal trial and error, and in a manner that is coherent with the way they think. The ability to express patterns in a way that is aligned with their mental processes is crucial to the flow of their work and to the quality of the insights they can draw. Many domain experts will not use textual property graph query languages (e.g., [Gremlin](https://arxiv.org/abs/1508.03843), [GSQL](https://arxiv.org/abs/1901.08248), [Cypher](https://dl.acm.org/citation.cfm?id=3190657), [PGQL](https://dl.acm.org/citation.cfm?id=2960421), [G-CORE](https://arxiv.org/abs/1712.01550), and the proposed [GQL](https://gql.today/)) either because it can be too hard for someone with little or no programming or scripting skills, or because it requires them to spend too much time on the technicalities and distracts them from their line of inquiry. As a result, they are forced to use only a predefined set of query templates or work in concert with technical experts. Both solutions are far from satisfying. 

Since the pattern perception capabilities of the human visual cortex are remarkable, it is a matter of course that query patterns were to be expressed visually. Indeed, five of the abovementioned languages use 'ASCII art syntax' for expressing topological constraints. Needless to say, this type of 'visualization' is quite limited. While the use of ASCII art declined during the 1990s in favor of graphical images, query languages began to adopt ASCII art only recently. Visual (graphical, diagrammatic) query languages have the potential to be much more 'user-friendly' than their textual counterparts in the sense that patterns may be constructed and understood much more quickly and with much less mental effort. Given a schema, interactive tools can allow query posers to construct valid patterns with minimal typing. A long-standing challenge is to design a visual query language that is generic, has rich expressive power, and is highly receptive and productive. V1 attempts to answer this challenge.

V1 is a declarative visual pattern query language for schema-based property graphs. V1 supports property graphs with mixed (both directed and undirected) edges, multivalued and composite properties, and _null_ property values. V1 supports temporal data types, operators, and functions and can be extended to support additional data types, operators, and functions (one spatiotemporal model is presented). V1 is generic, concise, has rich expressive power, and is highly receptive and productive.

---

The term _property graph_ refers to both a mathematical structure and a data model; both are described below.

## The Property Graph Mathematical Structure

A [_graph_](https://en.wikipedia.org/wiki/Graph_(discrete_mathematics)) is an ordered quintet 𝐺 = (𝑉, 𝐸, 𝐴, _ψₑ_, _ψₐ_) consisting of three pairwise disjoint sets and two functions. 𝑉 is a nonempty set whose elements are called [_vertices_](https://en.wikipedia.org/wiki/Vertex_(graph_theory)) (_nodes_, _dots_, _points_), 𝐸 is a set whose elements are called _undirected edges_ (_undirected links_, _undirected lines_), 𝐴 is a set whose elements are called _directed edges_ (_directed links_, _directed lines_, _arcs_, _arrows_), _ψₑ: E → { {u,v}: u,v ∈ V }_ is a total function mapping each undirected edge to an unordered pair of vertices, and _ψₐ: A → { (u,v): u,v ∈ V }_ is a total function mapping each directed edge to an ordered pair of vertices. An [_undirected graph_](https://en.wikipedia.org/wiki/Graph_(discrete_mathematics)#Graph) is a graph in which 𝐴 ≔ _∅_, a [_directed graph_](https://en.wikipedia.org/wiki/Directed_graph) (_digraph_, _oriented graph_) is a graph in which 𝐸 ≔ _∅_, and a [_mixed graph_](https://en.wikipedia.org/wiki/Mixed_graph) is a graph where both directed and undirected edges may exist.

Given undirected edge 𝑒: _ψₑ_(𝑒) = {𝑢,𝑣}, we say that 𝑒 is an edge _between_ 𝑢 and 𝑣, 𝑒 _connects_ (_joins_) 𝑢 and 𝑣, and 𝑢 and 𝑣 are _adjacent_. Likewise, given directed edge 𝑎: _ψₐ_(𝑎) = (𝑢,𝑣), we say that 𝑎 is an edge _from_ 𝑢 _to_ 𝑣, 𝑎 _connects_ (_joins_) 𝑢 to 𝑣, 𝑎 is an _outgoing edge_ of 𝑢, 𝑎 is an _incoming edge_ of 𝑣, 𝑣 is _adjacent from_ (_out-adjacent to_) 𝑢, 𝑢 is _adjacent to_ (_in-adjacent to_) 𝑣, 𝑢 is the _tail_ (_source vertex_, _initial vertex_) of 𝑎, and 𝑣 is the _head_ (_target vertex_, _terminal vertex_) of 𝑎. A [_loop_](https://en.wikipedia.org/wiki/Loop_(graph_theory)) (_self-edge_, _self-loop_, _buckle_) is an undirected edge 𝑒: _ψₑ_(𝑒) = {𝑢,𝑢} or a directed edge 𝑎: _ψₐ_(𝑎) = (𝑢,𝑢) connecting a vertex with itself. [_Multiple edges_](https://en.wikipedia.org/wiki/Multiple_edges) (_parallel edges_) are two or more undirected edges connecting the same unordered pair of vertices or directed edges connecting the same ordered pair of vertices. A _simple graph_ is a graph in which loops and multiple edges are not allowed. A [_pseudograph_](http://mathworld.wolfram.com/Pseudograph.html) is a graph in which loops and multiple edges are allowed.

An _attributed graph_ is a generic term referring to graphs in which an attribute (_single-attributed graph_) or a collection (e.g., a set, a bag, or a list) of attributes (_multi-attributed graph_) may be associated with each vertex (_vertex-attributed graph_), edge (_edge-attributed graph_), or the graph itself. An _attribute_ may be a nominal value, an ordinal value, a key-value pair, or any other annotation.

A _property graph_ (_PG_, _labeled property graph_, _LPG_) is a vertex-multi-attributed edge-multi-attributed pseudograph in which:

- Each vertex has an attribute called _label_ (_vertex-labeled graph_). Similarly, each edge has an attribute called _label_ (_edge-labeled graph_). The set of vertex labels, the set of undirected edge labels, and the set of directed edge labels are pairwise disjoint.

- Each vertex, as well as each edge, has a set of attributes called _properties_. Each property is an ordered pair (𝑛,𝑣) - the _property name_ and the _property value_. For each vertex, as well as for each edge, the property names are pairwise distinct.

Let 𝐿 denote the set of possible labels, 𝑃ₙ – the set of possible property names, and 𝑃ᵥ – the set of possible property values. 𝐿 and 𝑃ₙ are often defined as the set of all strings over a given alphabet, while 𝑃ᵥ is defined based on the supported value types (e.g., string, integer, date).

Extending the _graph_ definition, a _property graph_ is a septet 𝐺 = (𝑉, 𝐸, 𝐴, _ψₑ_, _ψₐ_, _λ_, _σ_) where _λ_: 𝑉 ∪ 𝐸 ∪ 𝐴 → 𝐿 is a total function mapping each node and edge to a label, and _σ_: 𝑉 ∪ 𝐸 ∪ 𝐴 → 2^(𝑃ₙ × 𝑃ᵥ) is a total function mapping each node and edge to a set of properties.

## The Property Graph Data Model

_Data_ is a representation of _information_. A _data element_ (_datum_) is an atomic unit of data, hence, an atomic unit of representation of information. A _[data model](https://en.wikipedia.org/wiki/Data_model)_ specifies the semantics and defines the structure of data elements and the relations between data elements. A data model consists of:

- An _[upper ontology](https://en.wikipedia.org/wiki/Upper_ontology)_ (_top-level ontology_, _foundation ontology_) is an [ontology](https://en.wikipedia.org/wiki/Ontology_(information_science)) that consists of general, domain-agnostic concepts describing data elements and their relations (e.g., _entity_, _relationship_, _type_, _feature_).

- A _structure_ (e.g., [mathematical](https://en.wikipedia.org/wiki/Mathematical_structure), [lexical](https://en.wikipedia.org/wiki/Lexical_grammar), [diagrammatic](https://en.wikipedia.org/wiki/Data_structure_diagram)) for organizing these data elements.

The _property graph data model_ comprises the following **concepts**:

-	An _entity_ represents information about a physical, conceptual, virtual, or fictional _particular_ (e.g., a certain person, guild, or dragon).

-	A _relationship_ (_binary relationship_) represents information about an _association_ or an _interaction_ between a pair of entities or between an entity and itself. Each relationship is either _directional_ (_unidirectional_, _asymmetric_) (e.g., an _owns_ relationship between a _Person_ entity and a _Horse_ entity, an _offspring_ relationship between two _Person_ entities) or bidirectional (_non-directional_, _symmetric_, _reciprocal_) (e.g., a _friend of_ relationship between two _Person_ entities).

-	An _action_ represents information about an _action of_ an entity (e.g., _erupts_ for a _Volcano_ entity) or an _action on_ an entity (e.g., _accused_ for a _Person_ entity), where no other [known or relevant] entities are concerned. An action may also represent a state of an entity (e.g., _sleeps_ action for a _Person_ entity) or a state-change (e.g., _falls asleep_ for a _Person_ entity). Like relationships, actions are either directional or bidirectional.

- Each entity, relationship, and action has a set of _features_ (_characteristics_). Each feature has an immutable name (e.g., _birth date_ for a _Person_ entity, _timeframe_ for an _owns_ association, _timeframe_ for a _sleeps_ action) and a value, for example, _weight_= 450. For each entity, relationship, or action, the feature names are pairwise distinct.

- Each entity, relationship, and action has a single, immutable _type_ (e.g., _Person_, _owns_, _erupts_).
 
  Types can be assigned based on many universals (qualities), e.g., _person_ entities, _red_ entities, _owner_ entities. Usually, entities of the same type are _semantically homogeneous_. The same is true also for relationships and actions. For property graphs, _semantic homogeneity_ means:

  - _Repetition of existence_: there are multiple entities of the same type, multiple relationships of the same type, and multiple actions of the same type.
  - _Repetition of features_: entities of the same type have features of the same types. The same is true also for relationships and actions.
  - _Repetition of actions_: entities of the same type 'have' actions of the same types.
  - _Repetition of relationships_: pairs of entities of the same pair of entity-types have relationships of the same types.

The property graph data model can hence represent _heterogeneous graphs_ - graphs that may contain multiple types of entities (_multi-modal graph_), of relationships (_multi-relational graph_), and of actions. In addition, each entity, relationship, or action  may have multiple features (_multifeatured graph_).

The property graph data model is a _metamodel_, as it does not specify types of entities, relationships, and actions, nor does it specify sets of features. It is domain-agnostic. Instead, domain-specific concepts may be specified and enforced using a _property graph schema_ (see next section).

The _property graph data model_ comprises the following **structure**:

  - All data elements are organized in a single property graph mathematical structure.
  - A _null vertex_ is a propertyless vertex with a null label. Each null vertex is connected to exactly one edge. An edge connecting two null vertices is not allowed.
  - Any vertex, except null vertices, represents an entity. The vertex's label is an integer or a nonempty string identifying the _entity's type_ (e.g., _Person_, _Guild_, _Dragon_).
  - An undirected edge {𝑢, 𝑣}, where both 𝑢 and 𝑣 are not null vertices, represents a bidirectional relationship between the entity represented by 𝑢 and the entity represented by 𝑣.
  - A directed edge (𝑢, 𝑣), where both 𝑢 and 𝑣 are not null vertices, represents a directional relationship between the entity represented by 𝑢 and the entity represented by 𝑣.
  - An edge, where one vertex is a null vertex, represents either

    - an entity's action (e.g., _sleeps_ action for a _Person_ entity), or
    - a relationship between an entity and a nonspecific entity. Sometimes, an entity is unknown or unimportant, but the existence of a relationship and the values of the relationship's properties - are important. For example, we may know that a given horse had different owners at different timeframes, but we do not know or care who owned it. Still, we want to be able to model such data.

  - The edge's label is an integer or a nonempty string identifying the _relationship's type_ (e.g., _owns_, _member of_) or the _action's type_ (e.g., _sleeps_).
  - A directed graph edge represents a directional relationship or action, while an undirected edge represents a bidirectional relationship or action.
  - Properties and subproperties represent features and subfeatures of entities (e.g., _name_ property and _first name_ subproperty for a _Person_ entity), relationships (e.g., _timeframe_ property for an _owns_ association), and actions (e.g., _timeframe_ for a _sleeps_ action). For each entity, relationship, and action, property names are pairwise distinct strings or integers, each identifying the feature's name, and each property value represents the feature's value.
  - Each feature value is represented using one of the _property data types_ supported by the model. There is, however, no standard definition of which data types the model should support. In this paper, we will use the following:

    - The model defines a set of _basic data types_ (e.g., _string_, _integer_, _float_).
    - A _multivalue_ is a set, a bag, or a list of values. All values are of the same basic data type (e.g., each value is a _string_), the same multivalue type (e.g., each value is a set(_string_)), or the same composite type (e.g., each value is a {_first_: _string_, _last_: _string_} composite).
    - A _multivalue type_ is defined by the collection type (set, bag, or list) and the data type of its elements. The definition must be nonrecursive.
    - A _map_ is a set of (name, value) pairs in which the names are pairwise distinct strings or integers identifying the subfeatures names, and the values are the respective subfeature values.
    - A _map type_ is defined by a set of (name, data type) pairs. The definition must be nonrecursive. A _composite_ is a map in which each value is of a basic data type, a multivalue type, or a composite type.

    A _basic property_ is a property whose value is of a basic data type. A _multivalued property_ is a property whose value is a multivalue (e.g., set, bag, or list), e.g., _titles_: _set_(_string_) = {"Her Majesty", "Her Royal Highness"}. A _composite property_ is a property whose value is composite, e.g., _name_: (_first_: _string_, _last_: _string_) = ("Brandon", "Stark"). Each member of a composite property is called a _subproperty_.

  - _null_ is a valid value for each _nullable_ property and subproperty, regardless of its data type. _Null-valued_ [sub]property indicates that a [sub]feature value is not specified.

    Several different interpretations can be associated with a _null_ value. Following the terminology introduced by [Codd](https://dl.acm.org/doi/10.1145/16301.16303) and adopted by many authors, a _null_ value is either
    - _Applicable missing_ – at present, a value is applicable (applies to the particular entity, relationship, or action) but unknown (whatever the reason, the graph does not have the value). E.g., the temperature 1000 years ago today; the phone number of a person who owns a phone, but the number is unknown; an answer to a question – when the questionee refused to answer.
    - _Inapplicable_ - at present, no value is applicable. E.g., the temperature tomorrow, previous citizenship when there is none, direct manager of the CEO, new hire's not-yet-assigned employee ID, a phone number of a person who does not own a phone, an answer to a question – when the question was not posed to the questionee.

    [Zaniolo](https://www.sciencedirect.com/science/article/pii/0022000084900801) proposed a third basic interpretation of _null_ values:

    - _No information_ – at present, the applicability of the unspecified value is unknown. E.g., a person's phone number – when it is unknown whether the person owns a phone; an answer to a question – when it is unknown if the question was posed to the questionee.
  
    Codd, Zaniolo, and many others proposed using two or more types of _null_ instead of a 'generic' _null_, but this approach remains mainly theoretical. In practice, _null_ values often have no consistent semantics. For a _birth date_ property, a _null_ value would likely represent an unknown birth date, but for a _death date_ property, a _null_ value may represent that the date on which the person died is unknown (_applicable missing_), that the person is still alive (_inapplicable_), or that it is unknown if the person is still alive (_no information_).

    Though the semantic of _null_ values is not always defined as part of the data model, nor as part of the _data schema_, it still must be well-defined for query languages' operators and functions. E.g., what is the result of (yesterday's date < person's death date) when the _death date_ is _null_? Often, _null_ values represent _applicable missing_ and _no information_, while _magic values_ (e.g., "9999-12-31" for dates) represent _inapplicable values_. In addition, a _sorting comparison operator_ is usually well-defined for _null_ values and may differ from the standard comparison operator (e.g., should _The five persons with the earliest birth date_ return persons with a _null_ birth date?)

- Should new information prove that two or more vertices represent the same entity, these vertices should be merged. Similarly, should new information prove that two or more edges represent the same relationship or action, these edges should be merged.

- Should new information prove that a vertex represents two or more entities, this vertex should be split. Similarly, should new information prove that an edge represents two or more relationships or actions, this edge should be split.

- Any pair of vertices, except null vertices, must be _distinguishable_, which means that vertices' _identifiers_ must be pairwise distinct, or there should be no pair of vertices with identical type, property values, and relationships. Similarly, any pair of edges must be _distinguishable_, which means that edges' _identifiers_ must be pairwise distinct, or there should be no pair of edges with identical type and property values that connect the same pair of vertices or the same vertex and a null vertex. An _identifier_ is a set of properties (often just an automatically generated index) that collectively uniquely identifies the element.

𝑛-ary relationships, where 𝑛 > 2, are not supported. However, this poses no expressivity limitation since any 𝑛-ary relationship, 𝑛 > 2, can be reframed as an entity and 𝑛 binary relationships. Consider, for example, a ternary relationship, where Person 𝐴 sells Horse 𝐻 to Person 𝐵. Instead, one can reframe this data as a _Sale_ entity 𝑆, a _seller_ relationship from 𝑆 to 𝐴, a _buyer_ relationship from 𝑆 to 𝐵, and an _asset_ relationship from 𝑆 to 𝐻.

The term _property graph_ was introduced by [Rodriguez](https://arxiv.org/abs/1006.2361) and [Neubauer](https://arxiv.org/abs/1004.1001), though other terms were used to describe similar data models. [Tsai and Fu's](https://ieeexplore.ieee.org/document/4310127) _attributed relational graph_ is a directed multigraph in which both nodes and edges have labels, and each label defines a set of numerical or logical attributes. [Shao et al.](https://ieeexplore.ieee.org/abstract/document/7953521) used the term _Heterogeneous graph_ for the same construct. [Gallagher](http://www.aaai.org/Papers/Symposia/Fall/2006/FS-06-02/FS06-02-007.pdf) used the term _data graph_ to refer to graphs in which vertices and/or edges may be typed and/or attributed. [Singh et al.](http://ieeexplore.ieee.org/abstract/document/4272051/) used the term _M*3_ (multi-modal, multi-relational, multifeatured) _network_ to refer to graphs with multiple entity-types, multiple relationship-types, and multiple descriptive features for nodes and edges. [Krause et al.](https://link.springer.com/chapter/10.1007/978-3-319-40530-8_10)  used the term _typed graph_ to refer to graphs with typed nodes, typed edges, and typed node properties.

Various extensions were proposed, including:
- Instead of a single label, each vertex has a (possibly empty) set of labels (_vertex multi-labeled graph_); entities are _multi-typed_
- Instead of a single label, each edge has a (possibly empty) set of labels (_edge multi-labeled graph_); relationships are _multi-typed_
- Directional relationship types naming: instead of a name for only one direction (e.g., _owns_), a unique name is defined for each direction (e.g., _owns_, _owned by_; _parent of_, _offspring of_)
- [_Property hypergraphs_](https://link.springer.com/chapter/10.1007%2F978-3-319-26148-5_21) (_hyperedges_ represent 𝑛-ary relationships)
- Schema-level and data-level _metaproperties_ (properties of properties – e.g., units of measure, accuracy, reliability)
- [EPGM – Extended Property Graph Model](https://dbs.uni-leipzig.de/file/EPGM.pdf), in which _logical graphs_ consist of subsets of a shared set of vertices and a shared set of edges. In addition, logical graphs have types and properties.
- Support of _derivation_ (_specialization_) of entity-types, relationship-types, and property types

## The Property Graph Schema

A _schema_ is a model for describing the structure of information in a certain domain using a certain data model. A _property graph schema_ defines the entity-types, the relationship-types, and the properties of thereof.

The property graph data model is _schema-optional_. Each property graph may be:
* _Schema-free_ (_schemaless_, _schema-independent_). A schema-free property graph neither defines nor enforces entity-types or relationships-types; each vertex and edge, regardless of its label, may have properties with any name and of any data type.
* _Schema-based_ (_schema-strict_, _schema-driven_, _schema-full_, _schema-dependent_). A schema-based property graph is a property graph conforming to a given schema.
* _Schema-mixed_ (_schema-hybrid_), where a schema is defined, but additional elements (e.g., additional properties) may be used.

It is much easier to define patterns when the information is presented consistently. For example, to match patterns such as _Any person who owns a white horse_, one would first:

* Define entity-types _Person_ and _Horse_ 
* Define a relationship-type _owns_ that holds from entities of type _Person_ to entities of type _Horse_
* For the _Horse_ entity-type, define a _color_ property with a nominal data type
* Ensure that all the information is structured accordingly

Though proposed property graph and property graph schema definitions have much in common (see [Angles](http://ceur-ws.org/Vol-2100/paper26.pdf), [Wu](https://arxiv.org/abs/1810.08755), [Hartig and Hidders](https://dl.acm.org/citation.cfm?id=3327964.3328495), and [Angles et al.](https://ieeexplore.ieee.org/document/9088985)), to date, there is neither a de jure nor a de facto standard definition (and hence, no standard property graph schema definition language). 

The following _property graph schema model_ is assumed in this paper:

A _property graph schema_ is defined by:
* A finite set of user-defined data types (on top of the built-in data types)
  * _Categorical_ (_nominal_ and _ordinal_) data types (e.g., _gender_: nominal _{male, female}_)
  * _Multivalued property data types_ (e.g., _nicknames_: set(_string_))
  * _Composite data types_ (e.g., _name_ {_first_: _string_, _last_: _string_}
  * _Interval data types_ (e.g., _span_: interval(_date_))
* A finite set of entity-types. For each entity-type: 
  * A unique name
  * A set of properties. For each property: 
    * A unique name
    * A data type
    * For properties and subproperties with numeric data types, intervals of numeric data types, or multivalued numeric data types: an optional schema-level _units_ metaproperty representing units of measure (e.g., Kg, cm, seconds)
* A finite set of relationship-types. For each relationship-type: 
  * The relationship-type's directionality: directional or bidirectional
  * A unique name
  * A set of pairs of entity-types for which the relationship-type is applicable (e.g., _owns_: {(_Person_, _Horse_), (_Person_, _Dragon_)}. When a pair is of the same type (e.g., (_Dragon_, _Dragon_)), loops can be allowed or disallowed
  * A set of properties - similar to entity-types' properties
  
A predefined property-less entity-type _Null_ serves two purposes:
* Realizing actions: an action-type can be realized as a relationship-type that is applicable between some entity-type and the _Null_ entity-type. For example, a _sleeps_: {(_Dragon_, _Null_)} relationship-type realizes a _sleeps_ action-type for the _Dragon_ entity-type.
* Realizing relationships to unknown or unimportant entities: sometimes, a real entity is unknown or unimportant, but the existence of a relationship and the values of the relationship's properties - are important. For example, we may know that a certain dragon was owned in a given timeframe, but we do not know or do not care who owned it. Still - we want to be able to store and query such information. _owns_: {(_Person_, _Dragon_), (_Guild_, _Dragon_), (_Null_, _Dragon_)} allows us to realize this.

Property graph schema definitions may vary in many aspects, including:

* Supported schema constraints (properties _uniqueness_ and _nullability_, _property value constraints_, _relationships cardinality constraints_, disallow loops for certain relationship-types, etc.)
* Supported ways to declare user-defined data types
* Properties may be either:
  * Defined globally and assigned to one or more entity/relationship-types, or
  * Defined per entity/relationship-type: different entity/relationship-types may have a property with the same name but with a different data type

V1 can be utilized with most definitions with minimal changes. 

## Patterns and Pattern Languages

A _pattern_ defines a set of topological and property value constraints on property graphs. Each property (sub)graph either _matches_ the pattern or not. For some patterns, a given (sub)graph may match a pattern in more than one way.

When a pattern is described in a natural language, it may be ambiguous or inaccurate. Nevertheless, all the patterns below are described in English. After all, to allow a reader to gain an intuitive understanding of a formal language, one has to use a natural language.

Here are two examples:

* _P1: Any person who owns at least five white horses_ (see Q101)

  _P1_ defines the set of (sub)graphs in which 

  - There is a vertex 𝑝 with a label _Person_
  - There are 𝑛 ≥ 5 vertices ℎ₁..ℎₙ, each with a label _Horse_
  - Each of ℎ₁..ℎₙ has a _color_ property, and its value is _white_
  - There are relationships from 𝑝 to ℎ₁..ℎₙ, each with a label _owns_

  Note that the pattern's description ignores temporal aspects. Maybe a person has owned a horse, owns it, or will own it. Assuming that the owns relationship has a timeframe property, a more accurate description would be _Any person who has 'owns' relationships with at least five white horses_. Maybe we are looking for _Any person who currently owns at least five white horses_ or for _Any person who at some timepoint owned at least five white horses_. If, for example, a horse's color may change over time, or if a horse may turn into a unicorn, we might want to rephrase the pattern.

* _P2: Any person whose date of birth is between January 1, 970 and January 1, 980, who owns a white Horse, who owns a dragon whose name starts with 'M' and over the last month froze at least three dragons belonging to members of the Masons Guild_

  _P2_ defines the set of (sub)graphs in which 

  - There is a vertex 𝑝 with a label _Person_
  - 𝑝 has a _birthDate_ property of type _date_, and its value is between January 1, 970 and January 1, 980
  - There is at least one vertex ℎ with a label _Horse_
  - There is a relationship from 𝑝 to ℎ with a label _owns_
  - ℎ has a _color_ property, and its value is _white_
  - There is at least one vertex 𝑑 with a label _Dragon_
  - There is a relationship from 𝑝 to 𝑑 with a label _owns_
  - 𝑑 has a _name_ property with a value that starts with 'M'
  - There are 𝑚 > 3 vertices, 𝑑₁..𝑑ₘ, each with a label _Dragon_
  - There are relationships from 𝑑 to any of 𝑑₁..𝑑ₘ, each with a label _freezes_
  - Each of these relationships has a _tf_ property (stands for "timeframe") with a _since_ subproperty whose value is in the range [_now_ - _months_(3) .. _now_]
  - There is a vertex 𝑔 with a label _Guild_
  - 𝑔 has a _name_ property, and its value is _Masons_
  - There are 𝑛 ≥ 1 vertices 𝑞₁..𝑞ₙ, each with a label _Person_
  - There are relationships from each of 𝑞₁..𝑞ₙ to 𝑔, each with a label _member of_
  - There are relationships from each of 𝑞₁..𝑞ₙ to one or more of 𝑑₁..𝑑ₘ, each with a label _owns_. Each of 𝑑₁..𝑑ₘ is connected by at least one of these relationships

The terms _entity_ and _relationship_ denote both pattern elements and graph elements. When the context may be ambiguous, we use the terms _pattern-entity_ and _pattern-relationship_ to refer to pattern elements and the terms _graph-entity_ and _graph-relationship_ to refer to graph elements.

Given a property graph schema 𝑆, a property graph 𝐺 conforming to 𝑆, and a query pattern 𝑃 conforming to 𝑆, all expressed in language 𝐿 = (𝐿<sub>𝑆</sub>, 𝐿<sub>𝐺</sub>, 𝐿<sub>𝑃</sub>, 𝐿<sub>𝑅</sub>), _pattern matching_ is the process of finding, transforming, merging, and annotating subgraphs of 𝐺 that match 𝑃. The syntaxes of sublanguages 𝐿<sub>𝑆</sub>, 𝐿<sub>𝐺</sub>, 𝐿<sub>𝑃</sub>, and 𝐿<sub>𝑅</sub> define what and how symbols may be combined to form well-formed schemas, graphs, patterns, and query results, respectively. A semantics of 𝐿<sub>_P_</sub> is a mapping (𝑆, 𝐺, 𝑃) → 𝑅: which subgraphs of 𝐺 match 𝑃 and how to transform, merge, and annotate them. 

Any valid subgraph that matches the pattern is called _an assignment_. We use _assignment to_ 𝑋 where 𝑋 is a pattern-entity, a pattern-relationship, or a set of thereof, to denote the graph-entity, the graph-relationship, or the set of thereof that matches 𝑋 as part of an assignment.

In the patterns given below, unless otherwise stated, each reported assignment should include the graph-entity assigned to each mentioned pattern-entity and the graph-relationship assigned to each mentioned pattern-relationship. Hence, any reported assignment to _P1_ should be composed of:
  
- A _Person_ graph-entity 
- Five or more _Horse_ graph-entities, each of which has a _color_ property, and its value is _white_
- The _owns_ graph-relationships between the _Person_ graph-entity to those _Horse_ graph-entities

Consider the following alternative patterns:

* _P1': Any person who owns at least five white horses. Report only the person_
* _P1'': Any person who owns at least five white horses. Report only the horses_
* _P1''': Any person who owns at least five white horses. Report the person and five of his horses_

A query may be:

* A decision query: does at least one assignment exist?
* A counting query: how many assignments exist?
* A counting-decision query: are there at least 𝑘 assignments?
* A reporting query:
  * Report [all / up to 𝑘] subgraphs of 𝐺, each is an assignment
  * Report subgraphs of 𝐺, each is a union of assignments, e.g., the union of all assignments with identical assignments to all entities (and different assignments to relationships)
  * Report a single subgraph of 𝐺, composed of the union of all assignments. This is sometimes preferred since it avoids combinatorial explosion for many queries (e.g., if a person owns ten white horses, any subset of five of the person's horses compose an assignment to _P1'''_). However, for some patterns, individual assignments cannot be deduced from their union.

Implementations may support one or more of the above.

V1 introduces the concept of _calculated properties_ - non-inherent properties of graph-entities, graph-relationships, and subgraphs, defined as part of a pattern. Each calculated property's evaluation result can be part of the reported query results, extending V1 capabilities beyond 'simple' pattern matching. For example, _The average number of horse ownerships per person_ - a calculated property of the set of all graph-entities of type _Person_ can be defined as part of a pattern. See Q356).

Pattern languages differ in many aspects, including:

* _Genericity_ - [general-purpose](https://en.wikipedia.org/wiki/General-purpose_language) (e.g., schema-driven) vs. [domain-specific](https://en.wikipedia.org/wiki/Domain-specific_language)
* _Pattern representation_ - _textual_ vs. _[visual](https://en.wikipedia.org/wiki/Visual_programming_language)_ (_graphical_, _diagrammatic_)
* _Receptivity_ and _Productivity_ (i.e., _readability_ and _writability_) - how intuitive and straightforward is it to understand existing patterns and construct new ones
* _Conciseness_ - the fewness of symbols and symbol types required for expressing patterns
* _Aesthetics_ - the quality of patterns being visually appealing
* _Declarative / Imperative_ - _[Declarative](https://en.wikipedia.org/wiki/Declarative_programming)_ languages describe patterns but do not specify how to match them. _[Imperative](https://en.wikipedia.org/wiki/Imperative_programming)_ languages describe patterns in terms of the steps required to match them on a given computational machine model (e.g., the [Gremlin Traversal Machine](https://arxiv.org/abs/1508.03843)). Languages may provide both declarative and imperative constructs.
* _[Expressive power](https://en.wikipedia.org/wiki/Expressive_power_(computer_science))_ - the breadth of patterns that can be expressed. 
  Unless a pattern language (declarative or imperative) is Turing-complete, there will always be computable patterns that cannot be expressed. 
  
There are always tradeoffs, especially between _receptivity and productivity_ and _expressive power_. Quoting Perlis' 54th and 55th [epigrams of programming](https://en.wikipedia.org/wiki/Epigrams_on_Programming): _"Beware of the Turing tar-pit in which everything is possible but nothing of interest is easy."_ and _"A LISP programmer knows the value of everything, but the cost of nothing."_ Perlis' 93rd and 26th epigrams are also worth quoting here: _"When someone says, 'I want a programming language in which I need only say what I wish done,' give him a lollipop."_ and _"There will always be things we wish to say in our programs that in all known languages can only be said poorly."_ Though these epigrams refer to programming languages, they are equally valid for property graph query languages.

## A Song of Ice and Fire

The following scenario, loosely based on [George R. R. Martin's _A Song of Ice and Fire_](http://www.georgerrmartin.com/bibliography/) will serve us to demonstrate the language's expressive power.

The subjects of [Sarnor](http://awoiaf.westeros.org/index.php/Kingdom_of_Sarnor), [Omber](http://awoiaf.westeros.org/index.php/Kingdom_of_Omber), and the other kingdoms of the [known world](http://awoiaf.westeros.org/index.php/Known_world) love their [horses](http://awoiaf.westeros.org/index.php/Horse). There is one thing they adore even more - that is their [dragons](http://awoiaf.westeros.org/index.php/Dragon). They own dragons of ice and fire. Like all well-behaved dragons, their dragons love to play. Dragons always play in couples. When playing, dragons often get furious, fire at each other (fire breath), and freeze one another (cold breath). Dragons usually freeze one another for periods of several minutes. However, on occasion, when they are furious, they can freeze one another for periods of several hours. The subjects enjoy watching their dragons play. Fascinated by these magnificent creatures, they have composed myriads of scrolls detailing each fire breath and cold breath over the last thousand years. The kings of Sarnor and Omber regularly pose queries about their history. More than often, it takes the royal historians and analysts several days to come up with answers, during which the kings tend to get pretty impatient. Lately, the [high king of Sarnor](http://awoiaf.westeros.org/index.php/High_King_of_Sarnor) posed a very complex query. After waiting for results for more than two moons, he ordered the chief analyst to be executed. He then summoned his chief mechanics and ordered them to develop an apparatus that he could use to pose queries and get results quickly. 

The engineers started by collecting all queries posed by their master over the last few years. Then they constructed a property graph schema over which these queries can be expressed.

The schema was composed of the following entity-types (and their properties):

* ***Person***: _name_ {_first_: _string_, _last_: _string_}, _gender_: _nominal {male, female}_, _birthDate_: _date_, _deathDate_: _date_, _height_: _int_ [cm]
* ***Dragon***: _name_: _string_, _color_: _nominal {black, white, ...}_
* ***Horse***: _name_: _string_, _color_: _nominal {black, white, ...}_, _weight_: _int_ [Kg]
* ***Guild***: _name_: _string_
* ***Kingdom***: _name_: _string_

the following directional relationship-types (and their properties):

* ***owns***: {(_Person_, _Horse_), (_Person_, _Dragon_), (_Guild_, _Horse_), (_Guild_, _Dragon_)} - _df_: _dateframe_

  When the person is still the owner and the ownership has no defined termination date (the value in _inapplicable_), df.till is 31/12/9999.

* ***fires at***: {(_Dragon_, _Dragon_)} - _time_: _datetime_; no loops allowed
* ***freezes***: {(_Dragon_, _Dragon_)} - _tf_: _datetimeframe_; no loops allowed

  tf is set only after the freeze has ended. tf.since and tf.till are _non-nullable_.

* ***offspring of***: {(_Person_, _Person_)}; no loops allowed
* ***member of***: {(_Person_, _Guild_)} - _df_: _dateframe_

  When the person is still a member and the membership has no defined termination date (the value in _inapplicable_), df.till is 31/12/9999.

* ***subject of***: {(_Person_, _Kingdom_)}

and of the following bidirectional relationship-type (and its properties):

* ***friend of***: {(_Person_, _Person_)} - _since_: _date_; no loops allowed

_Person_'s name is a composite property. The _date_, _datetime_, _dateframe_, and _datetimeframe_ data types are defined in [Data Types, Operators, and Functions](#data-types-operators-and-functions).

The engineers then represented the whole known history using a property graph conforming to this schema.

## V1 Basics

The following sections describe the syntax and the semantics of the V1 language. We start with the basics, adding more language elements as we go along.

**Note:** V1 has two equivalent syntaxes for expressing patterns: A _visual syntax_ - described below, and a _textual syntax_ (JSON-based) - summarized [here](https://github.com/LiorKogan/V1/blob/master/JSON%20pattern.md). There is a bijective mapping between patterns expressed in these two syntaxes. Sample textual patterns are available [here](https://github.com/LiorKogan/V1/tree/master/JSON%20Patterns). A V1 schema for _A Song of Ice and Fire_ is available [here](https://github.com/LiorKogan/V1/blob/master/Dragons%20Schema.json).

![V1](Pictures/BB01.png)

![V1](Pictures/BB04.png)

Patterns are generally read from left to right. Each pattern starts with **a small black diamond**, denoting the pattern start. The most straightforward patterns are structured as a sequence of rectangles, where consecutive rectangles are connected with an arrow or a line.

Yellow, blue, and red rectangles represent _concrete_, _typed_ and _untyped_ entities, respectively. The terms _concrete entity_, _typed entity_, and _untyped entity_ refer to pattern entities only (and not to graph entities).

**A yellow rectangle** represents a _concrete entity_: a specific person, a specific horse, etc. A concrete entity has a single assignment - a specific graph-entity. The text inside the rectangle denotes the entity-type and the value of a _visualization expression_ defined for this entity-type. For example, the visualization-expression for the _Person_ entity-type may be: _name.first_ ∥ ' ' ∥ _name.last_ and its value, for a specific graph-entity, would be 'Brandon Stark'.

**A blue rectangle** represents a _typed entity_. The text inside the rectangle denotes an entity-type. Only graph-entities of this type may be assigned to the pattern-entity. 

**A red rectangle** represents an _untyped entity_. Graph-entities of different types may be assigned to an untyped entity. An optional text inside the rectangle denotes an entity-type constraint (See [Untyped Entities](#untyped-entities)).

Two consecutive rectangles can be connected with:

* A horizontal **black arrow**, representing a _directional typed relationship_,
* A horizontal **black line**, representing either a _bidirectional typed relationship_ or a directional typed relationship for which either direction is acceptable,
* A horizontal **red arrow**, representing an _untyped directional relationship_ (see [Untyped Relationships](#untyped-relationships)),
* A horizontal **red line**, representing either an _untyped bidirectional relationship_ or an _untyped directional relationship_ where either direction is acceptable, or
* A horizontal **blue line**, representing a _pattern-path_ (see [Paths](#paths))

The terms _typed relationship_ and _untyped relationship_ refer only to pattern relationships. The term _path_ may refer to both _graph-path_ and _pattern-path_.

Each black arrow/line has a label on top. The label denotes a relationship-type. For arrows - the label is aligned to the arrow's origin. For lines - the label is centered. Only graph-relationships of this type can match the pattern-relationship.

A pattern matching engine would look in the property graph for assignments for every blue rectangle, red rectangle, black arrow, and black line. Graph entities are assigned to pattern entities. Graph relationships are assigned to pattern relationships. An assignment to the pattern is a set of graph-entities and graph-relationships that matches the whole pattern.

The relationship-type between any two entities must be valid with respect to the schema.

_**Q1:** Any dragon owned by Brandon Stark_ (two versions)

![V1](Pictures/Q001-1.png)

![V1](Pictures/Q001-2.png)

_**Q2:** Any dragon C that at least once had been frozen by a dragon owned by Brandon Stark_

![V1](Pictures/Q002.png)

_**Q184:** Any dragon C that at least once froze a dragon owned by Brandon Stark or was frozen by a dragon owned by Brandon Stark_

![V1](Pictures/Q184.png)

Both directions of the _freezes_ relationship are acceptable. Therefore - a line (instead of an arrow) is used in the pattern.

## Expressions and Expression Constraints

![V1](Pictures/BB02.png)

A **green rectangle** represents an expression. The rectangle contains:

- An _expression-tag_ ('{xt}') (see [Expression-Tags](#referencing-expression-tags))
- An expression ('_expr_')
- An optional _constraint_ on the result of the evaluation of the expression, composed of:
  - A constraint operator
  - A constraint expression (except when the constraint operator is _is null_ or _not null_)
- When units of measure are defined for the expression (based on the units of measures of the properties and the operators that compose the expression) - they are depicted as well (see Q117, Q304, Q265, Q95).

_expr_ is an entity's expression, a relationship's expression, or a Cartesian product's expression, or a global expression.

- An _entity's expression_

  The expression is composed of or depends on at least one property (inherent or calculated) of the connected entity and no properties of other entities/relationships.
  
  The green rectangle is connected to a pattern-entity (concrete, typed, or untyped) on its left.
  
  An expression-tag of an entity's expression is _a property of_ each unique assignment to the pattern-entity.

- A _relationship's expression_

  The expression is composed of or depends on at least one property (inherent or calculated) of the connected relationship and no properties of other entities/relationships.
  
  The green rectangle is connected to a pattern-relationship on its top.
  
  An expression-tag of a relationship's expression is _a property of_ each unique assignment to the pattern-relationship (Note that it is not a property of an assignment to the Cartesian product of the two related entities).

- A _Cartesian product's expression_

  The expression is composed of or depends on properties (inherent or calculated) of at least two entities (see {3} in Q207, {2}, {3} and {4} in Q340), at least two relationships (see {2} in Q267v2), or at least one entity and one relationship (see {1} in Q115v2, {2} and {4} in G3).
  
  The green rectangle is connected to one of the entities/relationships or located at the same level as the leftmost entities (when there is no single leftmost entity) (see Q207).
  
  An expression-tag of a Cartesian product's expression is _a property of_ each unique assignment to the Cartesian product.
  
  See also _extended Cartesian product's expression_ in [Extended Aggregators](#extended-aggregators).
  
- A _global expression_

  The expression is composed of and depends on no entity's expression, relationship's expression, nor Cartesian product's expression.
  
  The green rectangle is located at the same level as the leftmost entities (see Q375).
  
  A global expression is a _global property_.

Any expression-tag of an expression that is not a property name, a subproperty name, or a constant is called _a calculated property_.

An _expression_ is

- A literal (_string_, _integer_, or _float_),

  Note: _date_, _datetime_, and _duration_ literals are represented using the functions _date_(_string_), _datetime_(_string_) and _duration_(_string_), respectively. In visual syntax, these function names are omitted, and expressions are formatted according to the regional settings (see Q8),

- <_inherent property name_> (of a connected entity/relationship)

  (valid for a Cartesian product's expression only if it is connected to an entity/relationship),

- <_inherent property name_>.<_subproperty name_>&#91;.<_subproperty name_> ...&#93; (of a connected entity/relationship)

  (valid for a Cartesian product's expression only if it is connected to an entity/relationship),
  
- An _expression-tag_ or an _aggregation-tag_ (e.g., '{1}'),
- _op expr_, where _op_ is a unary operator (e.g., '- {1}'),
- _expr op expr_, where _op_ is a binary operator (e.g., '3 + {1}'),
- (_expr_),
-	'𝑓' where 𝑓 is a parameterless function (e.g., '_now_'. See G11),
-	'𝑓(e1, e2, ...)' where 𝑓 is a function with at least one parameter and e1, e2, ... are expressions (see Q353),
-	'e1.𝑓' - equivalent to 𝑓(e1), where 𝑓 is a function with one parameter and e1 is an expression,
-	'e1.𝑓(e2, e3, ...)' - equivalent to 𝑓(e1, e2, e3, ...), where 𝑓 is a function with more than one parameter and e1, e2, e3, ... are expressions,
- An _interval expression_ (see Q327),
- A _set expression_ (see Q318),
- A _bag expression_ (see Q315), or
- A _list expression_

An [_interval_](https://en.wikipedia.org/wiki/Interval_(mathematics)) can be explicitly constructed using the following syntaxes:

* (_expr1_ .. _expr2_)		- an open interval
* (_expr1_ .. _expr2_]		- a half-open interval
* [_expr1_ .. _expr2_)		- a half-open interval
* [_expr1_ .. _expr2_]		- a closed interval

  Both _expr1_ and _expr2_ are of the same ordinal data type.
  
  if _expr1_ > _expr2_ - the interval is an _empty interval_.
  
  if _expr1_ = _expr2_ - (_expr1_ .. _expr2_), (_expr1_ .. _expr2_], [_expr1_ .. _expr2_) are _empty intervals_.

  If _expr1_, _expr2_, or both are evaluated to _null_ - the interval is evaluated to _null_.

A _set_ is an unordered collection of zero or more _non-null_ values (called _elements_) of the same data type in which each element may occur only once. 

A set can be explicitly constructed using the following syntax:

* {_expr_, _expr_, ...} - zero or more expressions of the same data type. _null_ values are ignored. Duplicate values are merged.

  A comma after the last element is optional, except for a single-element set where it is mandatory.
  
A _bag_ (_multiset_) is an unordered collection of zero or more _non-null_ values (called _elements_) of the same data type in which elements may occur more than once.

A bag can be explicitly constructed using the following syntax:

* [_expr_, _expr_, ...] - zero or more expressions of the same data type. _null_ values are ignored.

  A comma after the last element is optional.

  Bag elements are unordered, and duplicates are allowed. A bag may not contain _null_ values.

A _list_ is an ordered collection of values (called _elements_) of the same data type in which elements may occur more than once.

A list can be explicitly constructed using the following syntax:

* (_expr_, _expr_, ...) - zero or more expressions of the same data type.

  A comma after the last element is optional, except for a single-element list where it is mandatory.

Expressions must match the data types defined for each operator and function.

A constraint filters assignments; an assignment is valid only if the result of the expression's evaluation _satisfies_ the constraint.

A constraint cannot be defined for a concrete entity's expression.

For untyped entities, expressions can be composed only of properties common to all valid entity-types. Valid entity-types for an untyped entity are defined implicitly (according to the types of the pattern-entities and pattern-relationships which are connected to the untyped entity) or explicitly (using entity-type constraints - see later) (see Q291).

A subproperty of a composite property is denoted as <_property name_>.<_subproperty name_> (e.g., _name.first_, _tf.since_).

_**Q3:** Any person whose first name is Brandon who owns a dragon_ (version 1)

![V1](Pictures/Q003-1.png)

{1} is a property of each unique assignment to B.

_**Q190:** Any person who became a dragon owner at 1011 or later_ (two versions)

![V1](Pictures/Q190-1.png)

{1} is a property of each unique assignment to the _owns_ relationship.

![V1](Pictures/Q190-2.png)

_year_ is a function (see next section).

All V1 constraint operators, except _is null_ and _not null_, are first evaluated using [Kleene's three-valued logic](https://www.jstor.org/stable/2267778) (3VL) to _true_, _false_, or _unknown_, and then mapped to a two-valued logic: the constraint is either _satisfied_ or _not satisfied_.

Each constraint operator, except _is null_ and _not null_, can be either blue or red.

![V1](Pictures/BB10-1.png)

- **A blue constraint operator**: the constraint is satisfied if and only if it is evaluated to _true_
- **A red constraint operator**: the constraint is satisfied if and only if it is evaluated to _true_ or to _unknown_

The following constraint operators can be only blue:

![V1](Pictures/BB10-2.png)

* A _is null_ constraint is satisfied if and only if the expression is evaluated to _null_ 
* A _not null_ constraint is satisfied if and only if the expression is not evaluated to _null_

## Data Types, Operators, and Functions

All V1's operators and functions must be well-defined when one or more of the operands or parameters are _null_ or evaluated to _null_. _Null-valued_ [sub]properties are interpreted as _applicable missing or no information_ (e.g., 1 + _null_ = _null_; max(5, _null_) = _null_).

Since V1 is schema-based, there is no need to define the behavior of each operator for any combination of operand types (and similarly, for each function for any combination of parameter types). When types do not match the definition (e.g., 5 > 'abc', _round_('abc')) – the query is invalid. Type mismatch should be detected during query analysis. In addition, interactive pattern-building tools should disallow the construction of such queries.

One design goal of V1 is to make it applicable to many property graph database management systems. Implementations may support different data types, operators, and functions than those presented here. 

To present V1, we use the following data types, operators, and functions:

**Built-in basic data types:**

|Type                         | Notes 
|-----------------------------|-----------------------------
| _int_                       | Integer
| _float_                     | Floating-point
| _date_                      | Date. For simplicity, we will not consider time zones here.
| _datetime_                  | Date and time. For simplicity, we will not consider time zones here.
| _duration_                  | Can be negative
| _string_                    | Unicode string

**Built-in composite data types:**

|Type                         | Notes 
|-----------------------------|-----------------------------
| _dateframe_                 | {_since_: _date_, _till_: _date_}
| _datetimeframe_             | {_since_: _datetime_, _till_: _datetime_}

**Literals:**

|Type                         | Examples 
|-----------------------------|-----------------------------
| _integer_                   | 12, -3
| _float_                     | 3., 3.12, -1.78e-6, NaN, -INF, +INF
| _string_                    | "", "abc", 'abc'


We will use _St_, _Bt_, and _Lt_, to denote a set, a bag, and a list of elements of type 𝑡, respectfully, and _It_ to denote an interval of ordinal type 𝑡.

**Operators:**

|Operator (_op_)                   | Operands and result type (result may be _null_ as well)
|----------------------------------|-----------------------------
| +, - (unary)                     | _op_ _int_ → _int_ <br> _op_ _float_ → _float_ <br> If the operand is NaN – the result is NaN. Otherwise, If it is _null_ – the result is _null_
| +, - (binary)                    | _int_ _op_ _int_ → _int_ <br> _float_ _op_ _float_ → _float_ <br> _duration_ _op_ _duration_ → _duration_ <br> _duration_ + _date_ → _date_ <br> _date_ _op_ _duration_ → _date_ <br> _duration_ + _datetime_ → _datetime_ <br> _datetime_ op _duration_ → _datetime_ <br> If one or both operands are NaN – the result is NaN. Otherwise, if one or both operands are _null_ – the result is _null_
| *                                | _int_ * _int_ → _int_ <br> _float_ * _float_ → _float_ <br> _float_ * _duration_ → _duration_ <br> _duration_ * _float_ → _duration_ <br> If one or both operands are _null_ - the result is _null_
| /                                | _int_ / _int_ → _int_ (truncated towards zero) <br> _float_ / _float_ → _float_ <br> _duration_ / _float_ → _duration_ <br> If one or both operands are _null_ - the result is _null_
| % (modulo)                       | _int_ % _int_ → _int_ (remainder has the same sign as the dividend) <br> If one or both operands are _null_ - the result is _null_
| ∪, ∩, -, △ <br> (union, intersection, difference, symmetric difference) | _St op St_ → _St_ (𝑡 is any type) <br> _Bt op Bt_ → _Bt_ (𝑡 is any type) (see Q377) <br> If one or both operands are _null_ - the result is _null_
| ∥ (concatenation)                | string ∥ string → string. 𝑠 ∥ _null_ = _null_ ∥ 𝑠 = _null_ <br> _Lt_ ∥ _Lt_ → _Lt_ (𝑡 is any type). _null_ ∥ 𝐿 = 𝐿 ∥ _null_ = _null_ <br> 𝑡 ∥ _Lt_ → _Lt_ (𝑡 is any type). _null_ ∥ 𝐿 = (_null_, ...). 𝐿. 𝑡 ∥ _null_ = _null_ <br> _Lt_ ∥ 𝑡 → _Lt_ (𝑡 is any type). 𝐿 ∥ _null_  = (..., _null_). _null_ ∥ 𝑡 = _null_

**Constraint Operators:**

|Operator (_op_)                        | Operands type (result is false / true / unknown)
|---------------------------------------|-----------------------------
| is null, not null (unary)             | any_type _op_ <br> An empty set / bag / list is not a _null_ value.
| =, ≠                                  | both operands of the same type (any type) <br> _unknown_ if at least one operand is _null_ <br> Exceptions for _float_: <br> (NaN = _null_) = _false_; (NaN ≠ _null_) = _true_
| <, >, ≤, ≥                            | both operands of the same ordinal type: <br> _int_ / _float_ / _date_ / _datetime_ / _duration_ / _string_ / other ordinal <br> _unknown_ if at least one operand is _null_. <br> Exceptions for _string_: <br> ("" ≤ _null_) = (_null_ ≥ "") = _true_ <br> (""  > _null_) = (_null_ < "") = _false_ <br> (""  > _null_) = (_null_ < "") = _false_ <br> Exceptions for _float_: <br> (_null_ ≤ NaN) = (_null_ ≥ NaN) = (_null_ < NaN) = (_null_ > NaN) = _false_ <br> (NaN ≤ _null_) = (NaN ≥ _null_) = (NaN < _null_) = (NaN > _null_) = _false_ <br> Exceptions for bounded types with no NaN value: <br> (_lb_ ≤ _null_) = (_null_ ≥ _lb_) = (_ub_ ≥ _null_) = (_null_ ≤ _ub_) = _true_ <br> (_ub_ < _null_) = (_null_ > _ub_) = (_lb_ > _null_) = (_null_ < _lb_) = _false_ <br> where _lb_ is the lower bound (e.g., INT_MIN for _integer_) and _hb_ is the upper bound (e.g., INT_MAX for _integer_)
| ∈, ∉ ([not] in)                       | left operand: any type 𝑡. right operand: _St_ / _Bt_ / _Lt_ <br> _unknown_ if at least one operand is _null_. Exceptions: <br> (_null_ ∈ {}/[]/()) = _false_; (_null_ ∉ {}/[]/()) = _true_ <br> <br> left operand: any ordinal type 𝑡. right operand : _It_ <br> _unknown_ if at least one operand is _null_. Exceptions:  <br> (_null_ ∈ _empty interval_) = false; (_null_ ∉ _empty interval_) = _true_ <br> 𝑡 is _int_: (_null_ ∈ [INT_MIN, INT_MAX]) = _true_; (_null_ ∉ [INT_MIN, INT_MAX]) = _false_
| ∋, ∌ ([not] contains)                 | right operand: any type 𝑡. left operand: _St_ / _Bt_ / _Lt_ <br> _unknown_ if at least one operand is _null_. Exceptions: <br> ({}/[]/() ∋ _null_) = _false_; ({}/[]/() ∌ _null_) = _true_ <br> <br> right operand: any ordinal type 𝑡. left operand : _It_ <br> _unknown_ if at least one operand is _null_. Exceptions: <br> (_empty interval_ ∋ _null_) = _false_; (_empty interval_ ∌ _null_) = _true_ <br> 𝑡 is _int_: ([INT_MIN, INT_MAX] ∋ _null_) = _true_; ([INT_MIN, INT_MAX] ∌ _null_) = _false_
| ⊆, ⊈ ([not] sub of) <br> ⊂, ⊄ ([not] proper sub of) | both operands: _string_ <br> _unknown_ if at least one operand is _null_. Exceptions: <br> (_null_ ⊂ "") = _false_; (_null_ ⊄ "") = _true_ <br> ("" ⊆ _null_) = _true_; ("" ⊈ _null_) = _false_ <br> <br> both operands of the same type: _St_ / _Bt_ / _Lt_ (t is any type) <br> _unknown_ if at least one operand is _null_. Exceptions: <br> (_null_ ⊂ {}/[]/()) = _false_; (_null_ ⊄ {}/[]/()) = _true_ <br> ({}/[]/() ⊆ _null_) = _true_; ({}/[]/() ⊈ _null_) = _false_ <br> <br> 𝑡 is ordinal, and both operands of the same type: _It_ <br> _unknown_ if at least one operand is _null_. Exceptions: <br> (_null_ ⊂ _empty interval_) = _false_; (_null_ ⊄ _empty interval_) = _true_ <br> (_empty interval_ ⊆ _null_) = _true_; (_empty interval_ ⊈ _null_) = _false_ <br> 𝑡 is _int_: (_null_ ⊂ [INT_MIN, INT_MAX]) = _true_; (_null_ ⊄ [INT_MIN, INT_MAX]) = _false_
| ⊇, ⊉ ([not] super of) <br> ⊃, ⊅ ([not] proper super of) | both operands: _string_ <br> _unknown_ if at least one operand is _null_. Exceptions: <br> (_null_ ⊇ "") = _true_; (_null_ ⊉ "") = _false_ <br> ("" ⊃ _null_) = _false_; ("" ⊅ _null_) = _true_ <br> <br> both operands of the same type: _St_ / _Bt_ / _Lt_ (t is any type) <br> _unknown_ if at least one operand is _null_. Exceptions: <br> (_null_ ⊇ {}/[]/()) = _true_; (_null_ ⊉ {}/[]/()) = _false_ <br> ({}/[]/() ⊃ _null_) = _false_; ({}/[]/() ⊅ _null_) = _true_ <br> <br> 𝑡 is ordinal, and both operands of the same type: _It_ <br> _unknown_ if at least one operand is _null_. Exceptions: <br> (_null_ ⊇ _empty interval_) = _true_; (_null_ ⊉ _empty interval_) = _false_ <br> (_empty interval_ ⊃ _null_) = _false_; (_empty interval_ ⊅ _null_) = _true_ <br> 𝑡 is _int_: ([INT_MIN, INT_MAX] ⊃ _null_) = _true_; ([INT_MIN, INT_MAX] ⊅ _null_) = _false_
| ⊳, ⋫ ([not] starts with)              | both operands: _string_ <br> _unknown_ if at least one operand is _null_. Exceptions: <br> (_null_ ⊳ "") = _true_; (_null_ ⋫ "") = _false_ <br> <br> left operand: _Lt_. right operand: 𝑡 (𝑡 is any type) <br> _unknown_ if at least one operand is _null_. Exceptions: <br> (() ⊳ _null_) = _false_; (() ⋫ _null_) = _true_ <br> <br> left operand: _Lt_. right operand: _Lt_ (𝑡 is any type) <br> _unknown_ if at least one operand is _null_. Exceptions: <br> (_null_ ⊳ ()) = _true_; (_null_ ⋫ ()) = _false_
| ⊲, ⋪ ([not] ends with)                | both operands: _string_ <br> _unknown_ if at least one operand is _null_. Exceptions: <br> (_null_ ⊲ "") = _true_; (_null_ ⋪ "") = _false_ <br> <br> left operand: _Lt_. right operand: 𝑡 (𝑡 is any type) <br> _unknown_ if at least one operand is _null_. Exceptions: <br> (() ⊲ _null_) = _false_; (() ⋪ _null_) = _true_ <br> <br> left operand: _Lt_. right operand: _Lt_ (𝑡 is any type) <br> _unknown_ if at least one operand is _null_. exceptions: <br> (_null_ ⊲ ()) = _true_; (_null_ ⋪ ()) = _false_
| ≍, ≭ ([not] match)                    | both operands: _string_ (right operand is a regex string) <br> unknown if at least one operand is null. Exceptions: <br> (_null_ ≍ "") = _true_; (_null_ ≭ "") = _false_

**Implicit Type Coercion**

|From type (_t1_)     | To type (_t2_)        | Examples
|---------------------|-----------------------| -----
|_int_                | _float_               | 5 + 3. → 8.
|_date_               | _datetime_            | _date_("2018-04-05") = _datetime_("2018-04-05T00:00:00") → true
|_dateframe_          | _datetimeframe_       | _dateframe_("2018-04-05", "2018-04-08") = _datetimeframe_("2018-04-05T00:00:00", "2018-04-08T23:59:59.999999999")
|_dateframe_          | interval(_date_)      | _df_._duration_, where _df_ is a _dateframe_ property
|_datetimeframe_      | interval(_datetime_)  | _tf_._duration_, where _tf_ is a _datetimeframe_ property
|interval(_date_)     | _dateframe_           | (_date_("2018-04-05") .. _date_("2018-05-05")).since → _date_("2018-04-05")
|interval(_datetime_) | _datetimefrmae_       | (_date_("2018-04-05") .. _datetime_("2018-05-05T00:00")).since → _datetime_("2018-04-05T00:00")

Also, based on any of these coercion rules:

|From type            | To type                     | Examples
|---------------------|-----------------------------| -----
|{} (empty set)       | set(_t2_) of any type _t2_  | {} ∪ {5} → {5}
|[] (empty bag)       | bag(_t2_) of any type _t2_  | [] ∪ [5] → [5] (see Q349)
|() (empty list)      | list(_t2_) of any type _t2_ | () ∥ (5,) → (5,)
|set(_t1_)            | set(_t2_)                   | {3, 5, 8} ∪ {3., 8.) → {3., 5., 8.}
|bag(_t1_)            | bag(_t2_)                   | [3, 5, 8] ∪ [3., 8.] → [3., 3., 5., 8., 8.]
|list(_t1_)           | list(_t2_)                  | (3, 5, 8) ∥ (3., 8.) → (3., 5., 8., 3., 8.)
|interval(_t1_)       | interval(_t2_)              | (3 .. 8) = (3. .. 8.) → true (3 .. 8) ⊃ (3. .. 5.) → true
|{_t1_, _t2_, ...}    | set(_t2_) *                 | {3, 5.) → {3., 5.)
|[_t1_, _t2_, ...]    | bag(_t2_) *                 | [3, 5.] → [3., 5.]
|(_t1_, _t2_, ...)    | list(_t2_) *                | (3, 5.) → (3., 5.)
|_t1_ .. _t2_         | interval(_t2_) *            | [3 .. 5.) → [3. .. 5.)

\* where there is implicit type coercion from t1 to t2

**Functions**

Functions over _float_ expressions:

|Function                          | Notes 
|----------------------------------|-----------------------------
| _trunc_(_float_) → _int_         | truncates toward zero
| _round_(_float_) → _int_         | rounds to the nearest integer (see G13)
| _mRound_(_float_, _int_) → _int_ | rounds to the nearest multiple of a given integer (see G9, G10)
| _seconds_(_float_) → _duration_  | (e.g., _seconds_(6) is a duration of 6 seconds)
| _minutes_(_float_) → _duration_  | See G10
| _hours_(_float_) → _duration_    |
| _days_(_float_) → _duration_     | See Q216, Q289
| _weeks_(_float_) → _duration_    | One week = 7 days
| _months_(_float_) → _duration_   | One month = 30.4367 days   (see Q110)
| _years_(_float_) → _duration_    | One year = 365.24 days   (see Q317)

Functions over _string_ expressions:

|Function                           | Notes 
|-----------------------------------|-----------------------------
| _length_(_string_) → _int_        | See Q255
| _toLower_(_string_) → _string_    | See Q308
| _date_(_string_) → _date_         | String format: YYYY-MM-DD (e.g., "2018-04-23") <br> In visual syntax the, function name is omitted, and the string is formatted according to the regional settings.
| _datetime_(_string_) → _datetime_ | String format: YYYY-MM-DDTHH:MM[:SS[.sss]] <br> (e.g., "2018-04-23T12:34:00") <br> In visual syntax, the function name is omitted, and the string is formatted according to the regional settings.
| _duration_(_string_) → _duration_ | String format adapted from Cypher: P[nY][nM][nW][nD][T[nH][nM][nS]] <br> (e.g., "P1Y2M10DT12H45M30.25S") <br> Y: years, M: months, W: weeks, D: days, H: hours, M: minutes, S: seconds <br> In visual syntax, the function name is omitted, and the string is formatted.

Functions over _datetime_ expressions:

|Function                                            | Notes 
|----------------------------------------------------|------
| _date_(_datetime_) → _date_                        | See Q158
| _year_(_datetime_) → _int_                         | See Q185
| _month_(_datetime_) → _int_                        | The month of the year (1-12)
| _day_(_datetime_) → _int_                          | The day of the month (1-31)
| _hour_(_datetime_) → _int_                         | 0-23 (see G4)
| _minute_(_datetime_) → _int_                       | 0-59 
| _sec_(_datetime_) → _int_                          | 0-59
| _yearsSinceEpoch_(_datetime_) → _float_            |
| _monthsSinceEpoch_(_datetime_) → _float_           |
| _weeksSinceEpoch_(_datetime_) → _float_            |
| _daysSinceEpoch_(_datetime_) → _float_             |
| _hoursSinceEpoch_(_datetime_) → _float_            |
| _minsSinceEpoch_(_datetime_) → _float_             |
| _span_(_datetime_, _datetime_) → _duration_        | Positive difference (see Q374)

Functions over _date_ expressions:

|Function                                            | Notes 
|----------------------------------------------------|------
| _year_(_date_) → _int_                             |
| _month_(_date_) → _int_                            | The month of the year (1-12)
| _day_(_date_) → _int_                              | The day of the month (1-31)
| _yearsSinceEpoch_(_date_) → _float_                |
| _monthsSinceEpoch_(_date_) → _float_               |
| _weeksSinceEpoch_(_date_) → _float_                |
| _daysSinceEpoch_(_date_) → _float_                 |

Functions over _dateframe_ expressions:

|Function                                            | Notes 
|----------------------------------------------------|------
| _duration_(_dateframe_) → _duration_               |
| _overlap_(_dateframe_, _dateframe_) → _duration_   | Always non-negative (see Q267v2)

Functions over _datetimeframe_ expressions:

|Function                                                  | Notes 
|----------------------------------------------------------|------
| _duration_(_datetimeframe_) → _duration_                 | See Q110
| _overlap_(_datetimeframe_, _datetimeframe_) → _duration_ | Always non-negative

Functions over _duration_ expressions:

|Function                                            | Notes 
|----------------------------------------------------|------
| _years_(_duration_) → _float_                      | One year = 365.24 days
| _months_(_duration_) → _float_                     | One month = 30.4367 days
| _weeks_(_duration_) → _float_                      | One week = 7 days
| _days_(_duration_) → _float_                       | See Q328
| _hours_(_duration_) → _float_                      |
| _minutes_(_duration_) → _float_                    |
| _seconds_(_duration_) → _float_                    |

Functions over _set_ expressions:

|Function                      | Notes 
|------------------------------|-----------------------------
| _count_(_St_) → _int_        | number of elements
| _bag_(_St_) → _Bt_           | set to bag
| _list_(_St_) → _Lt_          | set to list
| _el_(_St_) ⇉ 𝑡               | (see [Multivalued Functions and Expressions](#multivalued-functions-and-expressions))
| _subset_(_St_) ⇉ _St_        | (see [Multivalued Functions and Expressions](#multivalued-functions-and-expressions))
| _min_(_St_) → 𝑡 <br> _max_(_St_) → 𝑡 | 𝑡 is an ordinal type <br> _null_ when _St_ is _null_ or when it is empty
| _avg_(_St_) → 𝑡 or _float_   | 𝑡 is an ordinal type <br> if 𝑡 is _int_ - the result is _float_ <br> _null_ when _St_ is _null_ or when it is empty 
| _sum_(_St_) → 𝑡              | 𝑡 is _int_ / _float_ / _duration_ (not _date_ / _time_ / _datetime_) <br> _null_ when _St_ is _null_; zero when it is empty
| _min_(_St, n: int_) → _St_ <br> _max_(_St, n: int_) → _St_ | Set of (up to) _max_(0, 𝑛) smallest/largest values <br> 𝑡 is an ordinal type <br> _null_ when _St_ is _null_, {} when it is empty 
| _overlap_(_Sdateframe_) → _duration_ <br> _overlap_(_Sdatetimeframe_) → _duration_ | The duration of the overlap between all members of 𝑆 <br> Always non-negative (see Q371)
| _union_(_SSt_) → _St_ <br> _union_(_SBt_) → _Bt_	| The union of all members of a set of sets/bags (𝑡 is any type)
| intersection(_SSt_) → _St_ <br> intersection(_SBt_) → _Bt_| The intersection of all members of a set of sets/bags (𝑡 is any type)

Functions over _bag_ expressions:

|Function                           | Notes 
|-----------------------------------|-----------------------------
| _count_(_Bt_) → _int_             | number of elements
| _distinct_(_Bt_) → _int_          | number of distinct elements
| _multiplicity_(_Bt_, 𝑡) → _int_   | number of times 𝑡 occurs in _Bt_
| _set_(_Bt_) → _St_                | bag to set
| _list_(_Bt_) → _Lt_               | bag to list
| _el_(_Bt_) ⇉ 𝑡                    | (see [Multivalued Functions and Expressions](#multivalued-functions-and-expressions))
| _subbag_(_Bt_) ⇉ _Bt_             | (see [Multivalued Functions and Expressions](#multivalued-functions-and-expressions))
| _min_(_Bt_) → 𝑡 <br> _max_(_Bt_) → 𝑡 | 𝑡 is an ordinal type <br> _null_ when _Bt_ is _null_ or when it is empty
| _avg_(_Bt_) → 𝑡 or _float_         | 𝑡 is an ordinal type <br> if 𝑡 is _int_ - the result is _float_ <br> _null_ when _Bt_ is _null_ or when it is empty 
| _sum_(_Bt_) → 𝑡                    | 𝑡 is _int_ / _float_ / _duration_ (not _date_ / _time_ / _datetime_) <br> _null_ when _Bt_ is _null_; zero when it is empty
| _min_(_Bt, n: int_) → _Bt_ <br> _max_(_Bt, n: int_) → _Bt_ | Bag of (up to) _max_(0, 𝑛) smallest/largest values (see Q377) <br> 𝑡 is an ordinal type <br> _null_ when _Bt_ is _null_, [] when it is empty
| _overlap_(_Bdateframe_) → _duration_ <br> _overlap_(_Bdatetimeframe_) → _duration_ | The duration of the overlap between all members of 𝐵 <br> Always non-negative
| _union_(_BSt_) → _St_ <br> _union_(_BBt_) → _Bt_ | The union of all members of a bag of sets/bags (𝑡 is any type)
| intersection(_BSt_) → _St_ <br> intersection(_BBt_) → _Bt_ | The intersection of all members of a bag of sets/bags (𝑡 is any type)

Functions over _list_ expressions (𝑡):

|Function                           | Notes 
|-----------------------------------|-----------------------------
| _count_(_Lt_) → _int_             | number of elements
| _distinct_(_Lt_) → _int_          | number of distinct _non-null_ elements
| _multiplicity_(_Lt_, 𝑡) → _int_   | number of times 𝑡 occurs in _Lt_
| _at_(_Lt, n: int_) → t            | 𝑛'th element (1-based) <br> _null_ if 𝑛 is out of range
| _set_(_Lt_) → _St_                | list to set
| _bag_(_Lt_) → _Bt_                | list to bag
| _min_(_Lt_) → 𝑡 <br> _max_(_Lt_) → 𝑡 | 𝑡 is an ordinal type <br> _null_ values are ignored <br> _null_ when _Lt_ is _null_ or when it contains no _non-null_ elements
| _avg_(_Lt_) → 𝑡 or _float_ | 𝑡 is an ordinal type <br> if 𝑡 is _int_ - the result is _float_ <br> _null_ values are ignored <br> _null_ when _Lt_ is _null_ or when it contains no _non-null_ elements 
| _sum_(_Lt_) → 𝑡                 | 𝑡 is _int_ / _float_ / _duration_ (not _date_ / _time_ / _datetime_) <br> _null_ values are ignored <br> zero when _Lt_ is _null_ or when it contains no _non-null_ elements
| _min_(_Lt, n: int_) → _Lt_ <br> _max_(_Lt, n: int_) → _Lt_ | List of (up to) _max_(0, 𝑛) smallest/largest values <br> 𝑡 is an ordinal type <br> _null_ values are ignored <br> _null_ when _Lt_ is _null_, () when it contains no _non-null_ elements
| _sort_(_Lt_) → _Lt_               | Sorted list <br> 𝑡 is an ordinal type
| _invsort_(_Lt_) → _Lt_            | Inverse-sorted list <br> 𝑡 is an ordinal type

Functions over _interval_ expressions:

|Function                     | Notes 
|-----------------------------|-----------------------------
| _lb_(_It_) → 𝑡              | Lower bound <br> _null_ when _It_ is _null_
| _up_(_It_) → 𝑡              | Upper bound <br> _null_ when _It_ is _null_
| _set_(_It_) → _St_          | Interval to set <br> 𝑡 is discrete (_int_, _datetime_, or another ordinal type) <br> _null_ when _It_ is _null_
| _bag_(_It_) → _Bt_          | Interval to bag <br> 𝑡 is discrete (_int_, _datetime_, or another ordinal type) <br> _null_ when _It_ is _null_
| _list_(_It_) → _Lt_         | Interval to list <br> 𝑡 is discrete (_int_, _datetime_, or another ordinal type) <br> _null_ when _It_ is _null_

Other functions:

|Function                                            | Notes 
|----------------------------------------------------|------
| _now_ → _datetime_                                 | See Q8v2, G11
| _today_ → _date_                                   | See Q328
| _date_(year, month, day) → _date_                  | Construct date using three integers (see Q353) <br> _null_ when at least one value is _null_
| _min_(𝑡, 𝑡, ...) → 𝑡 <br> _max_(𝑡, 𝑡, ...) → 𝑡       | One or more values of the same ordinal type <br> _null_ when at least one value is _null_ <br> _min_({𝑡, 𝑡, …})and _max_({𝑡, 𝑡, …}) ignore _null_ values (see Q317)

Implementations may support _[opaque data types](https://en.wikipedia.org/wiki/Opaque_data_type)_ - data types for which the internal data representation is not exposed. For each opaque data type - a set of functions and operators may be defined (see _location_ data type in [Application: Spatiotemporality](#application-spatiotemporality)).

## Quantifiers

A **vertical purple rectangle** represents a _vertical quantifier_. The text inside the rectangle denotes the quantifier type.

Vertical quantifiers (or simply 'quantifiers') add much expressive power, including more complex topological constraints, more than one entity's expression constraint, and alternative subpatterns.

_**Q3:** Any person whose first name is Brandon who owns a dragon_ (version 2)

![V1](Pictures/Q003-2.png)

_**Q219:** Any person who owns a white horse weighing more than 200 Kg_

![V1](Pictures/Q219.png)

_**Q304:** Any person who owns a white horse and who owns a horse weighing more than 200 Kg_

![V1](Pictures/Q304.png)

The same graph-entity may match more than one pattern-entity. For example, Either the same horse or different horses may be assigned to B and C (this can be avoided: see *identicality, nonidenticality*, and *order constraints* later on). Similarly, the same graph-relationship may match more than one pattern-relationship.

A vertical quantifier has one connection on its left side and zero or more branches on its right side. On its left side is an entity, a quantifier, or the pattern's start. Except at the pattern's start, a quantifier may be wrapped with an 'O' (see Q147, Q360v2).

When a quantifier (or the rightmost quantifier in a sequence of quantifiers) is directly right of the pattern start, each branch may start with:

* An entity (see Q108),
* A Cartesian product's expression (see Q207)
* A global expression (see Q375), or
* A quantifier (see Q332v2)

When a quantifier (or the rightmost quantifier in a sequence of quantifiers) is directly right of an entity, each branch may start with:

* A relationship/path
  * optionally, with a relationship/path-negator (see Q358, Q359)
  * optionally, with a negator or with an 'O' (see Q358, Q359)
  * optionally, with relationship's expressions (see Q339)
  * optionally, with aggregators (see Q125),
* An entity's expression (see Q3v2),
* A Cartesian product's expression (see Q340), or
* A quantifier (see Q8)

The following branches do not affect the quantifier's evaluation:

* Any branch composed of an entity's expression with no constraint (see Q109)
* Any branch that starts with an 'O' (see Q148)

Each such branch is marked with a **white triangle**.

All other branches affect the quantifier's evaluation. Let 𝑏 denote the number of such branches.

We will name the left side of the quantifier _the left component_, and anything that follows a branch, up to the branch's end, _a right component_.

12 quantifier types are defined:

![V1](Pictures/BB03.png)

* **All** (denoted '&')

  If 𝑏 is zero, an assignment matches the pattern if and only if it matches the left component.
Otherwise - An assignment matches the pattern if and only if it matches the whole pattern. 

* **Some** (denoted '&#124;')

  If 𝑏 is zero, no assignment matches the pattern. Otherwise - An assignment matches pattern 𝑃 if and only if it matches pattern 𝑄 where
  
  * 𝑄's left component is identical to 𝑃's
  * 𝑄 has 𝑖 right components identical to 𝑃's, _1 ≤ i ≤ b_, and no other right components
  * The quantifier is replaced with an _All_ quantifier

* **Not all (but more than 0)** (denoted by an '&' with stroke)

  If 𝑏 is zero, no assignment matches the pattern. Otherwise - An assignment 𝐴 matches pattern 𝑃 if and only if it matches pattern 𝑄 where

  * 𝑄's left component is identical to 𝑃's
  * 𝑄 has 𝑖 right components identical to 𝑃's, _1 ≤ i < b_, and no other right components
  * The quantifier is replaced with an _All_ quantifier

  and there is no assignment 𝐵 with a similar left component as 𝐴's that matches pattern 𝑅 where

  * 𝑅's left component and all its right components are identical to 𝑃's
  * The quantifier is replaced with an _All_ quantifier

* **None** (denoted '0')

  If 𝑏 is zero, an assignment matches the pattern if and only if it matches the left component. Otherwise - An assignment 𝐴 matches pattern 𝑃 if and only if it matches pattern 𝑄 where

  * 𝑄's left component is identical to 𝑃's
  * The quantifier and the right components are removed

  and there is no assignment 𝐵 with a similar left component as 𝐴's that matches pattern 𝑅 where

  * 𝑅's left component is identical to 𝑃's
  * 𝑅 has 𝑖 right components identical to 𝑃's, _1 ≤ i ≤ b_, and no other right components
  * The quantifier is replaced with an _All_ quantifier
  
  The _None_ quantifier may not start a pattern.

* **= 𝑛**; 𝑏 ≥ 1, 𝑛 ∈ [1,𝑏]

  An assignment 𝐴 matches pattern 𝑃 if and only if it matches pattern 𝑄 where

  * 𝑄's left component is identical to 𝑃's
  * 𝑄 has 𝑛 right components identical to 𝑃's, and no other right components
  * The quantifier is replaced with an _All_ quantifier

  and, if _n ≠ b_, there is no assignment 𝐵 with a similar left component as 𝐴's that matches pattern 𝑅 where

  * 𝑅's left component is identical to 𝑃's
  * 𝑅 has 𝑖 right components identical to 𝑃's, 𝑖 > 𝑛, and no other right components
  * The quantifier is replaced with an _All_ quantifier

* **> 𝑛**; 𝑏 ≥ 2, 𝑛 ∈ [0, 𝑏-1]

  An assignment 𝐴 matches pattern 𝑃 if and only if it matches pattern 𝑄 where
  
  * 𝑄's left component is identical to 𝑃's
  * 𝑄 has 𝑖 right components identical to 𝑃's, 𝑛 < 𝑖 ≤ 𝑏, and no other right components
  * The quantifier is replaced with an _All_ quantifier

* **≥ 𝑛**; _b ≥ 2, n_ ∈ [1, 𝑏]

  An assignment 𝐴 matches pattern 𝑃 if and only if it matches pattern 𝑄 where
  
  * 𝑄's left component is identical to 𝑃's
  * 𝑄 has 𝑖 right components identical to 𝑃's, 𝑛 ≤ 𝑖 ≤ 𝑏, and no other right components
  * The quantifier is replaced with an _All_ quantifier

* **< 𝑛 (but more than 0)**; 𝑏 ≥ 2, 𝑛 ∈ [2, 𝑏]

  An assignment 𝐴 matches pattern 𝑃 if and only if it matches pattern 𝑄 where
  
  * 𝑄's left component is identical to 𝑃's
  * 𝑄 has 𝑖 right components identical to 𝑃's, 1 ≤ 𝑖 < 𝑛, and no other right components
  * The quantifier is replaced with an _All_ quantifier

  and there is no assignment 𝐵 with a similar left component as 𝐴's that matches pattern 𝑅 where

  * 𝑅's left component is identical to 𝑃's
  * 𝑅 has 𝑖 right components identical to 𝑃's, 𝑖 ≥ 𝑛, and no other right components
  * The quantifier is replaced with an _All_ quantifier

* **≤ 𝑛 (but more than 0)**; 𝑏 ≥ 2, 𝑛 ∈ [1, 𝑏]

  An assignment 𝐴 matches pattern 𝑃 if and only if it matches pattern 𝑄 where
   
  * 𝑄's left component is identical to 𝑃's
  * 𝑄 has 𝑖 right components identical to 𝑃's, 1 ≤ 𝑖 ≤ 𝑛, and no other right components
  * The quantifier is replaced with an _All_ quantifier

  and there is no assignment 𝐵 with a similar left component as 𝐴's that matches pattern 𝑅 where

  * 𝑅's left component is identical to 𝑃's
  * 𝑅 has 𝑖 right components identical to 𝑃's, 𝑖 > 𝑛, and no other right components
  * The quantifier is replaced with an _All_ quantifier

* **≠ 𝑛 (but more than 0)**; 𝑏 ≥ 2, 𝑛 ∈ [1, 𝑏]

  ≡ (_< n_) ∨ (_> n_)

* **𝑛1..𝑛2**; 𝑏 ≥ 2, 𝑛_1_ ∈ [1, 𝑏], 𝑛_2_ ∈ [2, 𝑏], 𝑛_1_ < 𝑛_2_

  An assignment 𝐴 matches pattern 𝑃 if and only if it matches pattern 𝑄 where
  
  * 𝑄's left component is identical to 𝑃's
  * 𝑄 has 𝑖 right components identical to 𝑃's, 𝑛_1_ ≤ 𝑖 ≤ 𝑛_2_, and no other right components
  * The quantifier is replaced with an _All_ quantifier

  and there is no assignment 𝐵 with a similar left component as 𝐴's that matches pattern 𝑅 where

  * 𝑅's left component is identical to 𝑃's
  * 𝑅 has 𝑖 right components identical to 𝑃's, 𝑖 > 𝑛_2_, and no other right components
  * The quantifier is replaced with an _All_ quantifier

* **∉ 𝑛1..𝑛2 (but more than 0)**; 𝑏 ≥ 4, _n1_ ∈ [2, _b-1_], _n2_ ∈ [3, 𝑏], 𝑛_1_ < 𝑛_2_

  ≡ (< 𝑛_1_) ∨ (> 𝑛_2_)

The order of the branches does not affect the evaluation result.

_**Q8:** Any person born before 970 and passed away or whose father was born not later than January 1, 950_ (two versions)

![V1](Pictures/Q008-1.png)

The person's death date is not _null_, nor is it _inapplicable_. When A's death date is _null_, 'deathDate ≠ 31/12/9999' is evaluated to _unknown_, and the constraint is not satisfied.

The following pattern also requires that A's death date is not a future date:


![V1](Pictures/Q008-2.png)

_**Q11:** Any current member of the Masons Guild who, on or after January 1, 1011, befriended someone who had left the Saddlers guild or the Blacksmiths guild in June 1010 or later_

![V1](Pictures/Q011.png)

The constraint 'member of df.till = 31/12/9999' means an _inapplicable_ membership end date – the person is currently a member of the Masons guild.

## Entity-Tags

The letter in the top-left corner of each pattern-entity rectangle (concrete, typed, or untyped) is called an _entity-tag_. Entity-tags are also included in query results: any graph-entity in a query result is annotated with the same tag as the pattern-entity to which it was assigned so that the query poser can understand why any given entity is part of the result. As part of the result, a graph-entity may be annotated with more than one entity-tag, as it may be assigned to several pattern-entities (in the same assignment or in different assignments - when assignments are merged).

Entity-tags may be referenced:

* in *identicality, nonidenticality*, and *order constraints*
* in an aggregator _per_ clause
* in an A1/M1/M2/M3 "_et × et × ..._" clause
* in an M1 aggregator "with min/max ..." clause (see Q196)

***Identicality constraints*** can be used when the same graph-entity should be assigned to:
- Several typed entities of the same type 
- Several untyped entities

_**Q4:** Any person A whose dragon was frozen by a dragon owned by (at least) one of A's parents_

![V1](Pictures/Q004.png)

Entity-tag 'B' is used to enforce identical assignment to two _Dragon_ pattern-entities. 

Entity-tags with identicality constraints are depicted in green.

_**Q9:** Any pair of dragons (A, B) where A froze B in both 980 and 984_

![V1](Pictures/Q009.png)

The same visual notation is also used when the same concrete entity appears more than once (see Q25v2, Q26v2)

***Nonidenticality constraint*** can be used when different graph-entities should be assigned to typed entities of the same type or to untyped entities.

_**Q5:** Any person A whose dragon was frozen by a dragon owned by two of A's parents_ (version 1)

![V1](Pictures/Q005-1.png)

Without the nonidenticality constraint, the same parent could be assigned to both D and E.

Nonidenticality constraints are depicted in red ('≠X'), where X is another entity-tag. Several nonidenticality constraints may be defined for the same pattern-entity, e.g., '≠A,≠C' (see Q57).

_**Q6:** Any person A whose dragon was frozen by two dragons - one owned by one of A's parents, the other owned by another parent (none, one, or both dragons may be owned by both parents)_

![V1](Pictures/Q006.png)

_**Q7:** Any person A whose dragon was either (i) frozen by a dragon owned by two of A's parents or (ii) frozen by two dragons - one owned by one of A's parents and the other owned by A's other parent_

![V1](Pictures/Q007.png)

_**Q24:** Any person A having (at least) two parents and owns a dragon that was frozen by a dragon neither of A's parents owns_

![V1](Pictures/Q024.png)

Q24 demonstrates the usage of both identicality and nonidenticality constraints for the same pattern-entity.

Consider Q5v1, Q6, Q7, and Q24. For any given assignment, there is another assignment where the two parents are switched (for example, in Q5v1, the assignments to D and E are switched). Such redundant assignments are usually undesired. Using _order constraints_, we can avoid such redundancies (see Q5v2).

Also, consider the following pattern: _Any three persons A, B, and C, who are pairwise friends_. If persons (A1, B1, C1) compose an assignment, so do (A1, C1, B1), (B1, A1, C1), and all other permutations. Such a factorial increase in the number of assignments is usually undesired. Using _order constraints_, we can express patterns such as _Any three persons A<B<C, who are pairwise friends_.

***Order constraints*** can be used when graph-entities should be orderly assigned to either typed entities of the same type or untyped entities.

_**Q31:** Any pair of dragons (A, B) where A froze B, A fired at B, B froze A, and B fired at A_ (version 1)

![V1](Pictures/Q031-1.png)

Without the order constraint, any reported pair of dragons would be reported twice: (D1, D2), (D2, D1).

The property graph data model requires that entities be _distinguishable_. To support order constraints between pattern-entities of the same type, V1 further requires each set of graph-entities of the same type to be _totally ordered_. To support order constraints between pattern-entities of possibly different types (See Q51), V1 requires the set of graph-entities to be _totally ordered_.

Order constraints are depicted in red ('<X' or '≤X'), where X is another entity-tag. Several nonidenticality or order constraints may be defined for the same pattern-entity, e.g., '<A,<B', '≠A,<C' (see Q83).

See also Q49, Q88, Q115v1, Q272, Q337, Q342, Q347, and Q350.

If, for some assignment, an identicality, nonidenticality, or order constraint is not relevant (e.g., identicality constraint between a pair of entities in two branches of a _Some_ quantifier, where the assignment includes only one branch) - the constraint is ignored. Otherwise - the assignment is valid only if the constraint is satisfied.

## Negator

![V1](Pictures/BB05.png)

Sometimes we need to express a pattern containing elements for which there is no assignment. For example, _any person whose first name is Brandon and who does not own a white horse_. Such patterns are composed of:

* A 'positive' component - _any person whose first name is Brandon_
* A negator - _does not_
* One or more 'negative' components - _own a white horse_

An assignment matches the pattern only if:

* It matches a pattern composed only of the positive component: there is an assignment to _a person whose first name is Brandon_.
* The assignment has no superset that matches a pattern composed of the positive component and the negative components: _the person whose first name is Brandon does not own a white horse_.

Such patterns can be composed using _negators_. The pattern starts with the 'positive' component, and negators separate it from the 'negative' components.

A negator is depicted by a narrow **pink 'X' rectangle**. The rectangle has one on its left side and one on its right side. 

On its left, there is either

* an entity
* a quantifier (see Q20v1)

On its right is a relationship or a path (see [Paths](#paths), Q84)

* optionally, with a relationship/path-negator (see [Relationship/Path-Negator](#relationshippath-negator), Q82v1)
* optionally, with relationship's expressions (see Q99v1)
* with no 'O' (see [Optional Components](#optional-components))
* with no aggregators (see [Aggregators](#aggregators))

Query results do not include assignments to entities/relationships/paths right of a negator. Any pattern entity right of a negator is depicted with a gray 'no report' icon on its top-right, indicating  that the query result does not include its assignment (see [Latent pattern-entities](#latent-pattern-entities)).

_**Q12:** Any person who does not own a horse_

![V1](Pictures/Q012.png)

_**Q13:** Any horse not owned by a person_

![V1](Pictures/Q013.png)

_**Q14:** Sweetfoot - if no person owns it_

![V1](Pictures/Q014.png)

_**Q15:** Brandon Stark - if he does not own a horse_

![V1](Pictures/Q015.png)

_**Q16:** Any person who does not own Sweetfoot_

![V1](Pictures/Q016.png)

_**Q17:** Any horse not owned by Brandon Stark_

![V1](Pictures/Q017.png)

_**Q18:** Brandon Stark - if he does not own Sweetfoot_

![V1](Pictures/Q018.png)

_**Q19:** Sweetfoot - if Brandon Stark does not own it_

![V1](Pictures/Q019.png)

_**Q256:** Any person who does not own a white horse_

![V1](Pictures/Q256.png)

_**Q257:** Any person who did not become a horse owner in or after 1011_

![V1](Pictures/Q257.png)

_**Q258:** Any person who did not become a white horse owner in or after 1011_

![V1](Pictures/Q258.png)

_**Q22:** Any horse not owned by a person who owns a dragon_

![V1](Pictures/Q022.png)

The positive component is _a horse_, while the negative component is _owned by a person who owns a dragon_. The right component is anything that follows the negator - up to the end of the branch.

Valid assignments:

* Any horse that is not owned
* Any horse that none of its owners is a person (e.g., a horse owned by a guild)
* Any horse that each person who owns it - does not own a dragon

_**Q333:** Any person who does not own a dragon that both fired at and froze the same dragon_

![V1](Pictures/Q333.png)

A negator may also appear left of a relationship that directly follows a quantifier's branch:

_**Q25:** Any dragon (C) not fired at by Balerion but fired at by a dragon that Balerion fired at_ (two versions)

![V1](Pictures/Q025-1.png)

![V1](Pictures/Q025-2.png)

_**Q26:** Any person who is a member of at least one guild that Brandon Stark is a member of and at least one guild that Brandon Stark is not a member of_ (four versions)

![V1](Pictures/Q026-1.png)

![V1](Pictures/Q026-2.png)

![V1](Pictures/Q026-3.png)

![V1](Pictures/Q026-4.png)

Negators may appear in more than one branch:

_**Q20:** Any horse neither owned by Rogar Bolton nor by Robin Arryn_ (two versions)

![V1](Pictures/Q020-1.png)

Note that there are two negative components. This query can also be represented using the _None_ quantifier:

![V1](Pictures/Q020-2.png)

_**Q21:** Any horse not owned by both Rogar Bolton and Robin Arryn_

![V1](Pictures/Q021.png)

If either Rogar Bolton or Robin Arryn owns the horse - the owner will not be part of the reported assignments. 

_**Q362:** Any horse owned either by Rogar Bolton or Robin Arryn but not by both_

![V1](Pictures/Q362.png)

If either Rogar Bolton or Robin Arryn owns the horse - the owner will be part of the reported assignments.

Negators may also appear in a sequence:

_**Q23:** Any horse not owned by a person who does not own a dragon_

![V1](Pictures/Q023.png)

The positive component is _a horse_, while the negative component is _owned by a person who does not own a dragon_. The right component is anything that follows the left negator - up to the end of the branch.

Valid assignments:

* Any horse that is not owned
* Any horse that none of its owners is a person (e.g., a horse owned by a guild)
* Any horse that each person who owns it - also owns a dragon

EA-tags, entity type-tags, and relationship type-tags defined right of a negator can only be referenced right of that negator.

Identicality, nonidenticality, and order constraints cannot be defined between pattern-entities in different negative components.

![V1](Pictures/Illegal-Tag07.png)

## Relationship/Path-Negator

![V1](Pictures/BB12.png)

The pattern _Any person whose first name is Brandon and who does not own a white horse_ has assignments in two cases:

* _There is a person whose first name is Brandon_, and _there are no white horses_
* _There is a person whose first name is Brandon_, and _there is at least one white horse_, but _there is no person whose first name is Brandon who owns a white horse_

Sometimes we want an assignment to match the pattern only in the second case - we want only assignments that match the whole pattern except for given relationships/paths. Hence, an assignment matches the pattern _only if_:

* It matches a pattern composed of the left component and the right component without the relationship/path
  * _There is a person whose first name is Brandon_
  * _There is a white horse_
* The assignment has no superset that matches a pattern composed of the left component, the right components, and the relationship/path
  * _There is no person whose first name is Brandon who owns a white horse_

A _relationship/path-negator_ is depicted with a **red '✕'** at the center of the relationship/path arrow/line. The arrow/line and the associated text are faded.

A component located right of a pattern relationship/path with a relationship/path-negator is required to have an assignment. Such assignments are included in the reported pattern assignment. The relationship/path is reported as a _negated relationship/path_, which may have a type and properties.

_**Q357:** Any dragon for which there is at least one dragon (besides itself) it did not freeze_

![V1](Pictures/Q357.png)

Each assignment comprises a dragon, a negated _freezes_ relationship, and a dragon it did not freeze.

_**Q83:** Any dragon for which there are at least two dragons (besides itself) it did not freeze_

![V1](Pictures/Q083.png)

_**Q204:** Any dragon for which there is a dragon not owned by a Sarnorian that did not freeze it_

![V1](Pictures/Q204.png)

_**Q205:** Any dragon A and Sarnorian subject C where there is a dragon not owned by C that did not freeze A_

![V1](Pictures/Q205.png)

Relationship/path-negators are often used with aggregators (see Q126, Q63-Q66, Q165, Q298, Q345).

A relationship/path with a relationship/path-negator may be wrapped with a negator.

_**Q82:** Any dragon that froze all other dragons. **Alternative phrasing:** Any dragon for which there is no dragon (besides itself) it did not freeze_ (version 1)

![V1](Pictures/Q082-1.png)

## Combiner

![V1](Pictures/BB15.png)

A _combiner_ combines two or more consecutive branches of the same quantifier.

A combiner is depicted by a narrow **purple '}' rectangle**. The rectangle has one connection on its left side and one on its right side.

On its left, there can be

- Types relationships, untyped relationships (see Q53), and paths
  - optionally, with a negator (see Q35) or with an 'O'
  - optionally, with a relationship/path-negator (see Q360v2)
  - optionally, with relationship's expressions (see Q185)
  - optionally, with aggregators (see Q244)
- a combiner (see Q100)

On its right – there is either

- an entity
- a combiner (see Q100)

The entity-type on a combiner's right side must match all the relationship-types and paths on its left side.

_**Q30:** Any pair of dragons (A, B) where A both froze B and fired at B_ (two versions)

![V1](Pictures/Q030-1.png)

A combiner is syntactic sugar, which, alternatively, can be implemented by duplicating anything right of it to each branch.

![V1](Pictures/Q030-2.png)

_**Q187:** Any dragon Balerion froze at least once on or after January 1, 1010, **and** at least once for less than ten minutes_

![V1](Pictures/Q187.png)

Note that the same graph-relationship may be assigned to both _freezes_ pattern-relationships. Compare with Q185.

_**Q189:** Any dragon Balerion froze at least once on or after January 1, 1010, **or** at least once for less than ten minutes_

![V1](Pictures/Q189.png)

_**Q29:** Any dragon that froze or fired at some dragon_ (versions 1-3)

![V1](Pictures/Q029-1.png)

Note that the implied identicality is redundant.

![V1](Pictures/Q029-2.png)

![V1](Pictures/Q029-3.png)

See also note under Q121v1 - combiner right of an A1 aggregator.

_**Q31:** Any pair of dragons (A, B) where A froze B, A fired at B, B froze A, and B fired at A_ (version 2)

![V1](Pictures/Q031-2.png)

Without the order constraint, any reported pair of dragons would be reported twice: (D1, D2), (D2, D1).

_**Q32:** Any pair of dragons (A, B) where A fired at both B and some dragon that fired at B_

![V1](Pictures/Q032.png)

_**Q5:** Any person A whose dragon was frozen by a dragon owned by two of A's parents_ (version 2)

![V1](Pictures/Q005-2.png)

_**Q170:** Any three dragons (A, B, D) where A fired at B, A fired at some dragon that fired at B, B froze D, and B froze some dragon that froze D_

![V1](Pictures/Q170.png)

_**Q33:** Any dragon A that froze some dragon B, froze some dragon that froze B, and fired at some dragon_

![V1](Pictures/Q033.png)

Different branches may be combined with different combiners.

_**Q34:** Any dragon A that froze some dragon B, froze some dragon that froze B, fired at some dragon D, and fired at some dragon that fired at D (B and D may be the same dragon or different dragons)_

![V1](Pictures/Q034.png)

_**Q35:** Any pair of dragons (A, B) where A froze B but did not fire at B_

![V1](Pictures/Q035.png)

At least one combined branch must be a non-negative component.

![V1](Pictures/Illegal-Tag08.png)

For the _None_ quantifier, all the branches are negative components.

![V1](Pictures/Illegal-Tag09.png)

## Chains, Horizontal Quantifiers, and Horizontal Combiner

Relationship's expressions can be _chained_. When chained, each expression with a constraint is a _filtering stage_. An assignment is valid only if all the chained constraints are satisfied, or in other words - if it passes all filtering stages.

_**Q188:** Any dragon Balerion froze at least once - on or after January 1, 1010, for less than ten minutes_

![V1](Pictures/Q188.png)

_**Q10:** Any person whose first name is Brandon, who owns some dragon B which froze a dragon C that (i) belongs to an offspring of Rogar Bolton, and (ii) froze a dragon that belongs to either Robin Arryn or Arrec Durrandon. B froze C at least once in or after 1010 - for longer than 100 seconds_

![V1](Pictures/Q010.png)

A **horizontal purple rectangle** represents a _horizontal quantifier_. The text inside the rectangle denotes the quantifier type.

Eleven horizontal quantifier types are defined (same as for vertical quantifiers, except the _All_ quantifier). Their semantics are like those of vertical quantifiers. _All_ can be implemented by chaining relationship's expressions or by using vertical quantifiers (see Q100).

On the top of a horizontal quantifier, there can be

- a relationship (see Q300)
- a relationship's expression (see Q301v1)
- a horizontal quantifier

On its bottom, there are zero or more branches. Each branch starts with either

- a relationship's expression (see Q267v3)
- a horizontal quantifier

A branch composed of relationship's expressions with no constraints does not affect the quantifier's evaluation.

A horizontal combiner may be used to combine two or more consecutive branches of the same horizontal quantifier. Each branch ends with either

- A relationship's expressions (see Q302)
- A horizontal combiner

Below a horizontal combiner, there can be either

- Another chained stage that starts with
  - a relationship's expression (see Q301v2)
  - an aggregator (see Q302)
  - a horizontal quantifier 
- A horizontal combiner

_**Q300:** Any pair of dragons (A, B) where A froze B, and at least two of the following conditions are satisfied: (i) the freeze duration was longer than ten minutes, (ii) the freeze started after January 1, 980, and (iii) the freeze ended before February 1, 980_

![V1](Pictures/Q300.png)

_**Q301:** Any pair of dragons (A, B) where A froze B for more than ten minutes, and either the freeze started after January 1, 980, or the freeze ended before February 1, 980_ (two versions)

![V1](Pictures/Q301-1.png)

![V1](Pictures/Q301-2.png)

See also Q267v3.

## Latent Pattern-Entities

![V1](Pictures/BB11.png)

Pattern entities may be annotated as implicit latent or explicit latent.

Assignments to _latent pattern-entities_ and assignments to pattern-relationships and pattern-paths connected to latent pattern-entities are not included when pattern assignment is reported.

_Implicit latent pattern-entities_ are pattern-entities that appear
- right of a negator (see Q12, Q22, Q288)
- right of a _None_ quantifier, except branches (or subbranch of a sequence of quantifiers that follows the _None_ quantifier) that starts with an 'O' (see Q359)

Such typed and untyped entities are required to have no assignments; hence, trivially, no assignments would be reported. Such concrete entities would not be reported as well (see Q20).

Implicit latent pattern-entities are depicted with a **gray 'no report' icon** on their top-right. These icons are automatically added by the interactive pattern-builder/visualizer tool and may not be added/removed by the pattern composer.

An entity right of a combiner is not depicted as implicit latent even if it is implicit latent according to one or more branches (see Q35).

_Explicit latent pattern-entities_ are non-implicit latent pattern-entities, which the pattern composer marks as latent. Though they may have assignments - those assignments are required not to be reported. Any non-implicit latent pattern-entity may be set as explicit latent.

Explicit latent pattern-entities are depicted with a **red 'no report' icon** on their top-right.

_**Q142:** Any person who owns a white or a black horse and has a parent who owns a dragon. The parent and its dragon are not to be included in the reported assignment_

![V1](Pictures/Q142.png)

Suppose person A1 has two parents, each with four _owns_ relationships to dragons. Without the latent annotations, there would be eight pattern assignments per person and _owns_ relationship to a horse. With the latent annotations, there would be only one assignment per person and _owns_ relationship to a horse.

_**Q143:** Any person who owns a horse that is neither white nor black and has a parent that does not own a horse. The parent is not to be included in the reported assignment_

![V1](Pictures/Q143.png)

When several pattern-entities have the same entity-tag, some may be latent (implicit or explicit) while others are not. See Q337, Q339.

Concrete, typed, and untyped entities may all be latent - implicit or explicit.

At least one pattern-entity should be non-latent.

## Optional Components

![V1](Pictures/BB07.png)

Anything right of an _optional_ is optional: if it has a valid assignment - it will be reported (unless it is latent). Otherwise - it will not.

An _optional_ (i.e., an 'O') is depicted with a **magenta 'O' rectangle**. The rectangle has one connection on its left side and one on its right side.

On its left, there is either

- an entity
- a quantifier (see Q144), except a quantifier at the start of the pattern

On its right, there is either 

- a relationship or a path (see [Paths](#paths))
  - with no negator
  - optionally, with a relationship/path-negator (see Q126)
  - optionally, with relationship's expressions (see Q339) 
  - optionally, with aggregators (see Q125)
- a quantifier 
  - optionally, with aggregators (see Q360v2)

Alternatively:

On its left is a quantifier at the start of the pattern, and on its right is an entity (see Q321v1)

_**Q147:** Any person A. If A owns both a horse and a dragon - they should be included in the assignment_

![V1](Pictures/Q147.png)

_**Q149:** Any person A. If A owns a horse - it should be included in the assignment. If A owns a dragon - it should be included in the assignment as well_

![V1](Pictures/Q149.png)

_**Q81:** Balerion. If there is a dragon it did not freeze - it should be included in the assignment_

![V1](Pictures/Q081.png)

Quantifier's branches that start with an 'O' do not affect the quantifier's evaluation.

Assignments to optional branches are reported regardless of the quantifier type, but only when there is an assignment to the pattern.

_**Q144:** Any person A who owns a white horse. If A has a parent who owns a horse - the parent and its horse should be included in the assignment_

![V1](Pictures/Q144.png)

_**Q145:** Any person A who owns a white horse. If A has a parent that does not own a horse - the parent should be included in the assignment_

![V1](Pictures/Q145.png)

_**Q146:** Any person A who owns a white horse. If A has a parent - the parent should be included in the assignment. If A's parent owns a horse - this horse should be included in the assignment as well_

![V1](Pictures/Q146.png)

_**Q148:** Any person A who owns both a horse and a dragon. If A has a parent - the parent should be included in the assignment_

![V1](Pictures/Q148.png)

_**Q150:** Any person A who owns a horse or a dragon. If A has a parent - the parent should be included in the assignment_

![V1](Pictures/Q150.png)

_**Q203:** Any person A. If A owns both a horse and a dragon - they should be included in the assignment. If A owns both a horse and a dragon and also has a parent - the parent should be included in the assignment as well_

![V1](Pictures/Q203.png)

_**Q359:** Any dragon A where (i) there is no black dragon A froze, (ii) there is no white dragon A did not freeze, (iii) there is at least one gold dragon A froze, and (iv) there is at least one silver dragon A did not freeze. Also, report any red dragon that A froze and any blue dragon that A did not freeze_

![V1](Pictures/Q359.png)

When tags right of an 'O' have no assignments:

- An expression-tag defined right of an 'O' is evaluated to _null_ (see Q140, Q141)
- An A1/A2 aggregation-tag defined right of an 'O' is evaluated to zero (see Q125, Q347, Q259v2)
- An A3 aggregation-tag defined right of an 'O' is 
  - evaluated to _null_ when _aggop_ is _min_/_max_/_avg_ (see Q317)
  - evaluated to zero when _aggop_ is _sum_/_distinct_ (see Q320v2, Q321v2)
  - evaluated to {} / [] when _aggop_ is _set_/_bag_/_union_/_intersection_ (see Q318)

Entity-tags defined right of an 'O' cannot be used in identicality, nonidenticality, and order constraints defined left of the 'O'.

Entity type-tags, defined right of an 'O' cannot be used in entity-type constraints defined left of the 'O'.

Relationship type-tags defined right of an 'O' cannot be used in relationship-type constraints defined left of the 'O'.

Entity-tags, entity type-tags, and relationship type-tags defined right of an 'O' can be used in aggregators defined left of the 'O':

- A/M/R aggregators: __𝑇__ may contain tags defined right of an 'O'
- A1/A3/M aggregators: __𝐵__ may contain tags defined right of an 'O' (see Q295, Q320, Q321v1)
- M1 aggregator: __𝑀__ may contain tags defined right of an 'O'

## Untyped Entities

A relationship-type may hold between different pairs of entity-types (e.g., owns: {(_Person_, _Dragon_), (_Guild_, _Dragon_), (_Null_, _Dragon_)}). _Untyped entities_ can be used to express patterns such as _Any dragon and its owners_, where the owner can be either a person, a guild, or _Null_.
 
 **A red rectangle** represents an _untyped entity_. Graph-entities of different types may be assigned to an untyped entity.
 
 A red rectangle containing only an entity-tag represents an entity with no explicit entity-type constraint.

_**Q36:** Any person who owns something_

![V1](Pictures/Q036.png)

B assignments are _implicitly type-constrained_ to things that a person can own.

_Implicit entity-type constraints_ are inferred from the pattern, including from identicality, nonidenticality and order constraints.

_**Q49:** Any three dragons with a cyclic freeze pattern and their owners (if any)_

![V1](Pictures/Q049.png)

The entity-types of all owners are implicitly constrained to things that can own a dragon.

Entities are implicitly type-constrained also when untyped entities are on either side of a negator or a relationship-negator:

_**Q39:** Any entity of a type that can own a dragon but does not_

![V1](Pictures/Q039.png)

_**Q40:** Any dragon that is not owned_

![V1](Pictures/Q040.png)

_**Q41:** Any entity of a type that can be owned but is not owned_

![V1](Pictures/Q041.png)

_**Q42:** Any entity of a type that can own something but owns nothing_

![V1](Pictures/Q042.png)

In addition to the implicit entity-type constraints, *explicit entity-type constraints* can be enforced using:

* a type equality constraint ('= ⟨ _ett_ ⟩') (see Q50),
* a type inequality constraint ('≠ _entity-type_' (see Q43) or '≠ ⟨ _ett_ ⟩' (see Q51))
* a set of at least two allowed entity-types and/or type-tags ('∈ {...}') (see Q37), or
* a set of at least two disallowed entity-types and/or type-tags ('∉ {...}') (see Q52)

_**Q43:** Any dragon that all of its owners (if any) are people_

![V1](Pictures/Q043.png)

_**Q37:** Any person who owns a horse or a dragon_

![V1](Pictures/Q037.png)

_**Q38:** Any person who owns something which is neither a horse nor a dragon_

![V1](Pictures/Q038.png)

For each untyped entity, implicit and explicit type constraints must not nullify the list of valid entity-types. Explicit type constraints must not contradict implicit type constraints. This can be asserted during query analysis.

Since both _horse_ and _dragon_ entity-types have a _name_ property of the same data type (string) - the following pattern is valid:

_**Q291:** Any person who owns a horse or a dragon whose name starts with an 'M'_

![V1](Pictures/Q291.png)

## Entity Type-Tags

![V1](Pictures/BB08.png)

A red rectangle (denoting an untyped entity) may contain an *entity type-tag*, depicted by a **numeric index wrapped in angle brackets**. An entity type-tag represents the entity-type of the entity in a given assignment.

Entity type-tags may be referenced:

* in a type constraint of an untyped entity (see Q50, Q52)
* in an extended aggregator _per_ clause (see Q218v2, Q350)
* in an A3 aggregator "aggop ..." clause (see Q167v2)
* in an extended M1/M2/M3 aggregator "[all but] 𝑘 ..." clause (see Q209, Q210)

_**Q50:** Any person who owns (at least) two things of the same type_

![V1](Pictures/Q050.png)

_**Q51:** Any person who owns (at least) two things of different types_

![V1](Pictures/Q051.png)

_**Q52:** Any person who owns (at least) two things of different types, neither is a horse_

![V1](Pictures/Q052.png)

_**Q167:** Any person whose owned entities are all of the same type_ (version 1)

![V1](Pictures/Q167-1.png)

For any quantifier except _All_ - an entity type-tag defined in a branch can only be referenced in that branch.

![V1](Pictures/Illegal-Tag04.png)

An entity type-tag defined right of a negator can only be referenced right of that negator.

![V1](Pictures/Illegal-Tag03.png)

An entity type-tag defined right of an 'O' can only be referenced right of that 'O', except when used in an aggregator's __𝑇__ (any aggregator) or __𝑀__ (A3, M1, M2, M3 aggregators) definition.

## Untyped Relationships

Multiple relationship-types may hold between a pair of entity-types (e.g., _freezes_: {(Dragon, Dragon)}, _fires at_: {(Dragon, Dragon)}. Untyped relationships can be used to express patterns such as _Any pair of dragons with at least one relationship_, where the relationship-type can be either _freezes_ or _fires at_.

A red arrow/line with no type label or with type constraints represents an _untyped relationship_. Graph-relationships of different types may be assigned to an untyped relationship.

A red arrow/line with no label represents a relationship with no explicit type constraints:

_**Q29:** Any dragon that froze or fired at some dragon_ (version 4)

![V1](Pictures/Q029-4.png)

The relationship is implicitly type-constrained to directional relationship-types between two dragons.

_**Q53:** Any person with a path of length up to three to Rogar Bolton_ (version 1)

![V1](Pictures/Q053-1.png)

Relationships are implicitly type-constrained also when a negator or a relationship-negator wraps an untyped relationship:

_**Q360:** Any dragon that (froze or fired at) all other dragons. **Alternative phrasing:** Any dragon for which there is no dragon (besides itself) it did not (freeze or fire at)_ (version 1)

![V1](Pictures/Q360-1.png)

In addition to the implicit relationship-type constraints, _explicit relationship-type constraints_ can be enforced using:

* a type equality constraint ('= ⟪ _rtt_ ⟫') (see Q367),
* a type inequality constraint ('≠ _relationship-type_' (see Q364) or '≠ ⟪ _rtt_ ⟫' (see Q365))
* a set of at least two allowed relationship-types and/or type-tags ('∈ {...}') (see Q121v2), or
* a set of at least two disallowed relationship-types and/or type-tags ('∉ {...}') (see Q368)

_**Q364:** Any person and an entity that have a relationship of a type other than 'friend of'_

![V1](Pictures/Q364.png)

_Implicit relationship-type constraints_ are inferred from the pattern (including from relationships directionality and from entities identicality/nonidenticality/order constraints).

For each untyped relationship, implicit and explicit type constraints must not nullify the list of valid relationship-types. Explicit type constraints must not contradict implicit type constraints. This can be asserted during query analysis.

## Relationship Type-Tags

![V1](Pictures/BB13.png)

Instead of a relationship-type, a relationship label may denote a _relationship type-tag_, depicted by a **numeric index wrapped in double angle brackets**. A relationship type-tag represents the relationship-type of the relationship in a given assignment.

Relationship type-tags may be referenced:

* in a type constraint of an untyped relationship (see Q365)
* in an extended aggregator _per_ clause (see Q369)
* in an A3 aggregator "aggop ..." clause (see Q366v2)
* in an extended M1/M2/M3 aggregator "[all but] 𝑘 ..." clause (see Q369)

_**Q365:** Any pair of dragons that have relationships of at least two types_

![V1](Pictures/Q365.png)

_**Q366:** Any pair of dragons that have relationships of at least three types_ (version 1)

![V1](Pictures/Q366-1.png)

Since there are only two relationship-types between a pair of dragons (_freezes_, _fires at_), this pattern would never have assignments. This can be detected during query analysis.

_**Q367:** Any pair of dragons that have a directional relationship of the same type in both directions_

![V1](Pictures/Q367.png)

_**Q368:** Any pair of dragons that have relationships of at least two types, neither is 'freezes'_

![V1](Pictures/Q368.png)

Again, the fact that this pattern would never have assignments can be detected during query analysis.

For any quantifier except _All_ - a relationship type-tag defined in a branch can only be referenced in that branch.

A relationship type-tag defined right of a negator can only be referenced right of that negator.

A relationship type-tag defined right of an 'O' can only be referenced right of that 'O', except when used in an aggregator's __𝑇__ (any aggregator) or __𝑀__ (A3, M1, M2, M3 aggregators) definition.

## Null Entities

As described above, graph-entities of type _Null_ realize action-types and relationships to unknown/unimportant entities.

Since each graph-entity of type _Null_ is connected by exactly one relationship, there are additional implicit constraints on each typed entity of type _Null_ and on each untyped entity that explicitly allows type _Null_:
* There is no relationship/path on its right (it is a terminal node), or there is no relationship/path on its left (it starts the pattern)
* It is not directly right of a combiner nor directly left of a quantifier
* It is not being aggregated
* Its single connection is either a relationship of a type that supports _Null_ entities on this side or a path that may end with such a relationship
* Its entity-tag is not reused (no identicality, nonidenticality, or order constraints)

As part of a pattern:
* Concrete entities may not be of type _Null_
* Typed entities may be of type _Null_ if there are no implicit type constraints that disallow it
* For untyped entities, type _Null_ may be explicitly allowed or disallowed only when there are no implicit type constraints that disallow it

As part of an assignment:
* For an untyped entity - graph entities of type _Null_ are valid assignments only if
  * There are no implicit type constraints that disallow it, and
  * There are no explicit type constraints that disallow it, or there are explicit type constraints that allow it

See G1-G20.

## Paths

A _graph-path_ is a sequence of graph-entities and graph-relationships that starts with a graph-entity and ends with a graph entity. A _simple graph-path_ is a graph-path where all entities are pairwise distinct, except, possibly, the first and last.

Each graph-path has a length. The _path length_ is equal to the number of graph-relationships along the path; hence, a relationship is a path of length 1.

A _pattern-path_ connects two pattern-entities - like a pattern-relationship. However, while a pattern-relationships are assigned with graph-relationships, pattern-paths are assigned with simple graph-paths minus the first and last entities, which are not considered a part of the assignment. Thus, pattern-paths are assigned with at least one graph-relationship.

A **blue line** between two pattern-entities represents a pattern-path. Above the line is a _constraint on the path length_. An upper bound on the path length must be defined, hence the constraint must be defined using one of the following constraint types: = _expr_, < _expr_, ≤ _expr_, in _set-expr_, in _bag-expr_, in _list-expr_, or in _interval-expr_, where all expressions or interval bounds evaluate to positive integers.

_**Q53:** Any person with a path of length up to three to Rogar Bolton_ (version 2)

![V1](Pictures/Q053-2.png)

_**Q84:** Any dragon with no paths of length up to three to other dragons_

![V1](Pictures/Q084.png)

_**Q55:** Any entity with paths of length up to three to Rogar Bolton, Robin Arryn, and Arrec Durrandon_

![V1](Pictures/Q055.png)

Constraints may be defined for both the entities and the relationships along the path:

_Constraints on relationships along the path_ are listed in blue curly brackets above the path's line. The brackets may list either:

- Allowed relationship-types - e.g., {fires at, freezes}. Any unlisted relationship-type is disallowed.
- Constraints on the number of relationships of given types, with optionally - a given direction - e.g., {freezes < 2, fires at = 2}, {freezes = 0}, {→ freezes = 2, ← freezes = 1}. Any unlisted relationship-type / direction is allowed.

_Constraints on entities along the path_ are listed in blue curly brackets below the path's line. The brackets may list either:

- Allowed entity-types - e.g., {Dragon}, {Dragon, Horse}. Any unlisted entity-type is disallowed.
- Constraint on the number of entities of given types - e.g., {Dragon = 0}, {Dragon ≥ 1, Horse ≥ 1}. Any unlisted entity-type is allowed.

A path cannot be composed of _Null_ entities since they are terminal nodes. Therefore, the list of allowed entity-types may not contain _Null_. Similarly, the list of disallowed entity-types may not contain _Null_ as it is implicitly disallowed.

_**Q44:** Any path of length up to four between Vhagar and Balerion, which is composed only of 'freezes' relationships_

![V1](Pictures/Q044.png)

_**Q45:** Any path of length up to four between Vhagar and Balerion composed only of 'freezes' and 'fired at' relationships_

![V1](Pictures/Q045.png)

_**Q46:** Any path of length up to four between a dragon owned by Rogar Bolton to a dragon owned by Robin Arryn, which is composed of up to two 'freezes' relationships and only of 'Dragon' entities_

![V1](Pictures/Q046.png)

_**Q54:** Any person with paths of length up to three to Rogar Bolton, Robin Arryn, and Arrec Durrandon. The paths composed only of 'friend of' relationships_

![V1](Pictures/Q054.png)

## Shortest Paths

Instead, or in addition to specifying a constraint on the path length - a _shortest path constraint_ can limit paths-assignments to the shortest ones that subject to the constraints on the entities/relationships along the path. If, for example, the length of the shortest path that subjects to the constraints is three - only paths of length three are valid assignments.

_**Q47:** All shortest paths between Vhagar and Balerion_

![V1](Pictures/Q047.png)

_**Q48:** All shortest paths between Vhagar and Balerion, which are neither composed of 'freezes' relationships nor 'Dragon' entities_ (version 1)

![V1](Pictures/Q048-1.png)

_shortest_ may not appear directly right of a negator or a path-negator.

## Path Patterns

An alternative to constraints on the entity-types and the relationship-types along a path are constraints on the subgraphs which assignments to a path are composed of. 

A _path pattern_ is a pattern that has one entity marked as _left-terminal_ and one entity, possibly the same one, marked as _right-terminal_.

A _path-assignment_ is composed of chained subgraphs; each subgraph is assigned to a path pattern. There is an overlap between assignments to successive path patterns:

- The graph-entity assigned to the left-terminal of the first path pattern of a path is also assigned to the entity preceding the path
- The graph-entity assigned to a right-terminal of a path pattern is also assigned to the left-terminal of the successive path pattern
- The graph-entity assigned to the right-terminal of the last path pattern of a path is also assigned to the entity following the path

A **blue table** below the path defines constraints on the path's path pattern types. The table has two columns:

- A constraint on the number of allowed path pattern instances of this type along the path. The following constraint types can be used: n, <n, ≤n, >n, ≥n, n₁..n₂, where the values are positive integer constants.
- A path pattern

The last row defines whether a path-assignment may be composed of other graph-elements ('✔' if yes, '✘' if no).

_**Q48:** All shortest paths between Vhagar and Balerion, which are neither composed of 'freezes' relationships nor 'Dragon' entities_ (version 2)

![V1](Pictures/Q048-2.png)

_**Q58:** Any Sarnorian who has a path of length up to six to an Omberian. The path passes through Rogar Bolton_

![V1](Pictures/Q058.png)

_**Q56:** constraints on path patterns_

In this example, path assignments must be composed of assignments to four path patterns

![V1](Pictures/Q056.png)

_**Q57:** constraints on path patterns_

* Path assignments must not contain people whose first name starts with 'M' (note that assignments to A and C are not considered part of the path assignment)
* There must be between two and three assignments to any of two path patterns
* The path may be composed of additional graph-elements

![V1](Pictures/Q057.png)

_**Q322:** Any dragon owned by at least five consecutive generations of the same dynasty_

![V1](Pictures/Q322.png)

_**Q323:** Any dragon owned by at least five (not necessarily consecutive) generations of the same dynasty_

![V1](Pictures/Q323.png)

See also Q290, Q329.

## Referencing Expression-Tags

![V1](Pictures/BB02.png)

Each green rectangle has an _expression-tag_ on its top-left corner, depicted by an index wrapped in curly brackets ('{xt}'). An expression-tag represents the result of the evaluation of an entity's expression, relationship's expression, or Cartesian product's expression - for a given assignment.

Expression-tags and aggregation-tags are jointly called _EA-tags_. Each EA-tag has a unique positive integer index. If an EA-tag is referenced at least once - it is depicted in bold purple. Otherwise - it is depicted in black.

Expression-tags may be referenced:

* in an entity, relationship, or Cartesian product's expression (see Q267v2, Q308, Q349)
* in an entity, relationship, or Cartesian product's expression constraint (see Q108, Q109)
* in a path length constraint
* in an extended aggregator _per_ clause (see Q114v3, Q270)
* in an A3 aggregator "aggop ..." clause (see Q116)
*	in an extended M1/M2/M3 aggregator "[all but] 𝑘 ..." clause (see Q220, Q262)
* in an M3 aggregator "with min/max ..." clause (see Q130)
* in an A1/A2/A3 aggregator constraint (see Q120, Q255)

_**Q108:** Any person who has the same birth date as Brandon Stark_

![V1](Pictures/Q108.png)

{1} is a property of the only assignment to A. {2} is a property of each unique assignment to B.

For any quantifier except _All_:
* Any EA-tag defined in a branch, except expression-tag of an entity's expression where the entity is left of the quantifier and the entity's expression is right of it, can only be referenced in that branch.

![V1](Pictures/Illegal-Tag06.png)

Similarly, for any horizontal quantifier except _All_:
* Any EA-tag defined in a branch, except expression-tag of a relationship's expression where the relationship is above the quantifier, and the relationship's expression is below it, can only be referenced in that branch.

For each entity/relationship/Cartesian product - if the same expression is used more than once - the same expression-tag will be assigned (see also Q267v3, Q311):

![V1](Pictures/Illegal-Tag01-1.png)

![V1](Pictures/Illegal-Tag01-2.png)

Self or circular references are invalid:

![V1](Pictures/Illegal-Tag02-1.png)

![V1](Pictures/Illegal-Tag02-2.png)

_**Q111:** Any person A who has no friends with whom A shares a birthdate_

![V1](Pictures/Q111.png)

An EA-tag defined right of a negator can only be referenced right of that negator.

![V1](Pictures/Illegal-Tag05.png)

_**Q353:** Any person A that in the day A turned two years old - at least one person was born, but there is no such person A became a friend of since A reached 20_

![V1](Pictures/Q353-1.png)

A relationship's expression defined below a relationship-negator can only be referenced in that chain.

![V1](Pictures/Q353-2.png)

Composite properties and subproperties are tagged and can be referenced like ordinary properties:

_**Q109:** Any person A whose parent owned a horse or a dragon before A's birth_ (two versions)

![V1](Pictures/Q109-1.png)

![V1](Pictures/Q109-2.png)

_**Q110:** Any three dragons with cyclic freezes of more than 100 minutes, in chronological order, within six months, and their owners (if any)_

![V1](Pictures/Q110.png)

_**Q112:** Any person who owned a horse and a dragon at the same timeframes_ (three versions)

![V1](Pictures/Q112-1.png)

The subproperties can be compared one-by-one:

![V1](Pictures/Q112-2.png)

The order of the expressions along a chain does not matter. The following representation is valid, as well:

![V1](Pictures/Q112-3.png)

_**Q266:** Any person A who has the same name (first and last) as A's parent_ (two versions)

![V1](Pictures/Q266-1.png)

![V1](Pictures/Q266-2.png)

_**Q267:** Any person who was a member of two guilds at overlapping timeframes_ (three versions)

![V1](Pictures/Q267-1.png)

![V1](Pictures/Q267-2.png)

The following version also covers some cases where one membership has either a _null_ (_unknown_) start date or a _null_ (_unknown_) end date:

![V1](Pictures/Q267-3.png)

## Aggregators

One can use an order constraint to represent patterns such as _Any person who owns at least two horses_, but it is quite cumbersome for patterns such as _Any person who owns at least 20 horses_. Patterns such as _Any dragon that froze dragons more than ten times_ or _Any dragon that froze dragons for a cumulative duration longer than 100 minutes_ cannot be expressed without aggregators.

**An orange rectangle** represents an aggregator. It is composed of two parts:
- The upper part (dark orange) defines how assignments are split into disjoint sets (_per_ clause)
- The lower part (light orange) defines an aggregate expression that is evaluated per each set and may contain a constraint on the result of the evaluation of the aggregate expression

![V1](Pictures/BB06-1.png)

Here are three examples:

First example: _Any person who owns at least 20 horses_ (Q59)

![V1](Pictures/Q059.png)

All assignments to the pattern, ignoring the aggregator, are found, and split into sets. In each set, all the assignments to A are identical. Only sets with more than 20 assignments to B are valid pattern assignments.

Second example: _Any dragon that froze dragons more than ten times_ (Q71)

![V1](Pictures/Q071.png)

All assignments to the pattern, ignoring the aggregator, are found, and split into sets. In each set, all the assignments to A are identical. Only sets with more than ten assignments to the relationship are valid pattern assignments.

Third example: _Any pair of dragons (A, B) where A froze B for a cumulative duration longer than 100 minutes_ (Q86)

![V1](Pictures/Q086.png)

All assignments to the pattern, ignoring the aggregator, are found, and split into sets. In each set, all the assignments to both A and B are identical. Only sets where the sum of the duration of all _freezes_ relationships is greater than 100 minutes are valid pattern assignments.

**Evaluating an aggregator:**

- Step 1: All assignments to the pattern excluding this aggregator and without constraints based on this aggregation-tag ('at') are found
  - In the first example: all (person, horse) pairs (A, B) where A owns B
  - In the second and third examples: all dragon pairs (A, B) where A froze B
  
- Step 2: The assignments are split into disjoint sets. In each set - the assignment to the entities listed in the _per_ clause is identical. Each set is to be aggregated separately.
  - In the first and second examples: In each set - all assignments to A are identical
  - In the third example: In each set - all assignments to both A and B are identical
  
- Step 3: Per each set of assignments - _at_ is evaluated
  - In the first example: per person A: _at_ = number of horses A owns
  - In the second example: per dragon A: _at_ = cumulative number of its _freezes_ relationships to all dragons
  - In the third example: per dragon pair (A, B): _at_ = sum of the duration of all _freezes_ relationships from A to B
  
- Step 4: Per each set of assignments: the aggregation constraint (if given) and any other constraint that is based on this aggregation-tag are evaluated
  - In the first example: the aggregation constraint is _at_ ≥ 20
  - In the second example: the aggregation constraint is _at_ > 10
  - In the third example: the aggregation constraint is _at_ > 100 [min]
  
- Step 5: Each set of assignments for which the constraint is satisfied - is a valid assignment to the pattern

Next, we will define three aggregator types: A1, A2, and A3. Steps 1, 2, 4, and 5 are identical for all types. Step 3 is different, as sets are aggregated in different ways.

Let __𝑆__ denote the set of all assignments to the pattern excluding this aggregator (subject to aggregators evaluation order) (that is step 1 above).

![V1](Pictures/BB06-2.png)

**Upper part**

Let __𝑇__ denote a Cartesian product of one or more entity-tags.

For all aggregator types (A1, A2, A3, M1, M2, M3, and R1), the _per_ clause has one of the following formats:

- '_et1 × et2 × ..._':

  𝑇 = _et1 × et2 × ..._
    
- '←':

  𝑇 = entity-tag directly left of the aggregator
    
- '→':

  𝑇 = entity-tag directly right of the aggregator not wrapped with a negator nor preceded by a _None_ quantifier (valid only when there is exactly one such entity-tag (A1: see Q246)
    
- '⟷':

  𝑇 = _et1 × et2_ where _et1_ is directly left of the aggregator, and there is a single entity directly right of the aggregator not wrapped with a negator nor preceded by a _None_ quantifier - _et2_ (A2: see Q75)
  
Wherever possible, the notations '←', '→' and '⟷' are used instead of entity-tags.

Next, let ___TA___ denote a of all unique assignments to 𝑇 in 𝑆. _TA[n]_ is the n'th unique assignment to 𝑇.

Let ___S(n)___ denote a subset of 𝑆: the set of all assignments composed of _TA[n]_ (that is step 2 above).

The upper part is optional. When there is no upper part - 𝑆 is not split.

**Lower part**

The lower part contains an aggregation-tag, an aggregate expression, and optionally a constraint on the result of the evaluation of the aggregate expression.

The _aggregation-tag_ is on the top-left corner and is depicted by an index wrapped in curly brackets ('{at}'). An aggregation-tag represents the result of the evaluation of the aggregate expression for a given set of assignments.

For each entity/relationship/Cartesian product - if the same expression/aggregation is used more than once - only one EA-tag will be assigned (see Q64, Q73v2, Q82v2).

_at_ is evaluated per each _TA[n]_. In the first example above, {1} is evaluated per each unique assignment to A in 𝑆. _at_ is a _calculated property_ of _TA[n]_. When there is no upper part - _at_ is a global property (see {1} in Q82v3, {1} in Q372, {2} in Q356).

Aggregation-tags may be referenced:

* in an entity, relationship, or Cartesian product's expression (see Q315, Q317)
* in an entity, relationship, or Cartesian product's expression constraint (see Q337, Q338v2)
* in a path length constraint
* in an extended aggregator _per_ clause (see Q211)
* in an A3 aggregator "[all but] 𝑘 ..." clause (see Q129)
* in an M3 aggregator "with min/max ..." clause (see Q91)
* in an extended M1/M2/M3 aggregator "[all but] 𝑘 ..." clause (see Q212)
* in an A1/A2/A3 aggregator constraint (see Q125, Q127, Q332v2)

## A1 Aggregator

![V1](Pictures/Agg-A1.png)
  
Let __𝐵__ denote a list of Cartesian products of entity-tags. 𝐵 must be nonempty and must not be composed of entity-tags composing 𝑇.

The lower part of an A1 aggregator contains the following elements:

- One of the following:

  - '_et11 × et12 × ..._ ∪ _et21 × et22 × ..._ ∪ _..._' (all products have the same arity):
  
    𝐵 = [_et11 × et12 × ..._ , _et21 × et22 × ..._ , _..._] ('∪' - see Q295)
    
  - '←':
  
    card(𝐵) = 1; _B[1]_ = [_et_] where _et_ is directly left of the aggregator (see Q249, Q250)
    
  - '→':
  
    𝐵 = [_et1, et2, ..._] where _et1, et2, ..._ are directly right of the aggregator and not wrapped with a negator nor preceded by a _None_ quantifier (equivalent to '_et1_ ∪ _et2_ ∪ _..._') (see Q294, Q175, Q176 where card(B)>1)
    
  - '⟷':
  
    𝐵 = [_et × et1, et × et2, ..._] where _et_ is directly left of the aggregator and _et1, et2, ..._ are directly right of the aggregator and not wrapped with a negator nor preceded by a _None_ quantifier (equivalent to '_et × et1_ ∪ _et × et2_ ∪ ...')
    
  Let ___BA(n, o)___ denote the list of all assignments to _B[o]_ in _S(n)_

- **aggregation-tag**: _at(n)_ = &#124;_BA(n, 1)_ ∪ _BA(n, 2)_ ∪ ..._&#124;
  
  We are using _card(union(all assignments to all elements in B))_ instead of _sum(card(assignment to one element in B))_ since two elements in 𝐵 may have the same assignment (see Q175), and we are counting _distinct_ assignments to all elements in 𝐵 per 𝑛.
  
  When an optional part has no assignments, A1 aggregation-tags defined right of the 'O' are evaluated to zero (see Q125).

- an optional **constraint** on _at(n)_

  **For each 𝑛: _S(n)_ is reported only if _at(n)_ satisfies the constraint**
  
  Note that a '= 0' constraint cannot be satisfied unless the entities composing B are right of an 'O', as such constraint means that there are no assignments to the pattern excluding this aggregator. Similarly:

  - no constraint is equivalent to '_> 0_' constraint
  - '_≠ expr_', '_< expr_', and '_≤ expr_' constraints are not satisfied when _at(n)_ is zero
  - '_= expr_', '_≥ expr_', and '_∈ [expr1, expr2]_' constraints are not satisfied when _at(n)_ is zero even if _expr_ is evaluated to zero

A1 location:

- Below a relationship/path

  A relationship/path with an A1 below it may have a relationship/path-negator (see Q63-Q66) and may be wrapped with an 'O' (see Q82v2, Q126).
- Below a quantifier-input (excluding quantifier at the start of the pattern)

  See Q305v1, Q121v1, Q122. A quantifier-input with an A1 below it may be wrapped with an 'O'.
  
- Below the pattern's start (even when followed by a quantifier)

  See Q27, Q261, Q332.
 
  
- If all entities in _T ∪ B_ are in a sequence: if all the entities in 𝑇 appear right of all the entities in 𝐵: left of the rightmost entity in 𝑇 (see Q247). Otherwise: right of the leftmost entity in 𝑇 (see Q243)
- If entities in _T ∪ B_ are in different branches of a quantifier: below the quantifier-input (see Q27)

_**Q59:** Any person who owns at least 20 horses_

![V1](Pictures/Q059.png)

{1} is a property of each unique assignment to A in 𝑆.

_**Q60:** Any dragon that was frozen by exactly five dragons_

![V1](Pictures/Q060.png)

If some dragon B froze some dragon A more than once - B would still be counted only once per A. A1 counts _distinct_ entity assignments.

_**Q61:** Any entity that owns more than two entities_

![V1](Pictures/Q061.png)

_**Q62:** Any person who has paths of length up to four to more than five people_

![V1](Pictures/Q062.png)

_**Q136:** Any dragon A that froze (dragons that froze dragons B). The cumulative number of distinct Bs (per A) is greater than 100_ (two versions)

![V1](Pictures/Q136-1.png)

![V1](Pictures/Q136-2.png)

_**Q177:** Any dragon A frozen by at least ten dragons and froze each of those_ (two versions)

![V1](Pictures/Q177-1.png)

First, any pair (A, B) matching the pattern excluding the aggregator is found. Then, the aggregation constraint is checked: For each assignment to A, there are at least ten assignments to B.

![V1](Pictures/Q177-2.png)

First, any pair matching the pattern excluding the aggregators is found. Then, the aggregation constraints are checked one by one:

For each assignment to A:

* There are at least ten assignments to B such that B froze A
* There are at least ten assignments to B such that A froze B

_**Q178:** Any dragon A either (i) frozen by exactly ten dragon and froze at least one dragon which is not one of those, (ii) frozen by exactly ten dragon and froze at least two of those, or (iii) frozen by more than ten dragons and froze at least one dragon_

![V1](Pictures/Q178.png)

First, any dragon triplet (A, B, C) matching the pattern excluding the aggregator is found. Then, the aggregation constraint is checked:

For each assignment to A, there are at least ten assignments to B such that (B froze A, and A froze a dragon that is not B)

_**Q85:** Any dragon that froze at least ten dragons and was frozen by at least ten dragons_ (two versions)

![V1](Pictures/Q085-1.png)

![V1](Pictures/Q085-2.png)

First, any dragon triplet (A, B, C) matching the pattern excluding the aggregators is found. Then, the aggregation constraints are checked one by one:

For each assignment to A:

* There are at least ten assignments to B such that A froze B, and a dragon other than B froze A
* There are at least ten assignments to C such that C froze A, and A froze a dragon other than C

Hence, for each assignment to A:

* A froze at least ten dragons, and either (i) only one dragon froze A - which is not one of those, or (ii) at least two dragons froze A
* At least ten dragons froze A, and either (i) A froze only one dragon - which is not one of those, or (ii) A froze at least two dragons

Hence, A froze at least ten dragons, and at least ten dragons froze A

_**Q101:** Any person who owns at least five white horses_

![V1](Pictures/Q101.png)

_**Q102:** Any dragon A that was frozen by exactly two dragons, each of which was frozen at least once. A might have been frozen by additional dragons that were never frozen_

![V1](Pictures/Q102.png)

_**Q166:** Any **dragon** that more than five Sarnorians own a dragon that froze **it**_

![V1](Pictures/Q166.png)

_**Q246:** Any dragon B that froze at least one dragon and was frozen by more than ten dragons_

![V1](Pictures/Q246.png)

{1} is a property of each unique assignment to B in 𝑆.

_**Q247:** Any dragon C that more than ten dragons froze dragons that froze it_

![V1](Pictures/Q247.png)

_**Q113:** Any person A who has at least five friends with whom A shares a birthdate_

![V1](Pictures/Q113.png)

_**Q114:** Any person who owns at least five horses of the same color_ (versions 1, 2)

![V1](Pictures/Q114-1.png)

B is explicit latent to avoid redundant results. Were B not latent, each pattern assignment would have one of A's dragons assigned to B and all A's dragons of the same color assigned to C. When B is latent, all such reported results are identical, hence, reported only once.

The following pattern avoids redundant results as well:

![V1](Pictures/Q114-2.png)

_**Q218:** Any person who owns at least five entities of the same type_ (version 1)

![V1](Pictures/Q218-1.png)

_**Q292:** Any person A that at least 80% of A's horses are black_

![V1](Pictures/Q292.png)

_**Q63:** Any Masons Guild member who more than fifty Masons Guild members are not friends of_

![V1](Pictures/Q063.png)

_**Q65:** Any person A who does not own more than two things that (at least) one of A's parents owns/owned_

![V1](Pictures/Q065.png)

_**Q66:** Any person from whom more than five people do not have a path of length up to six_

![V1](Pictures/Q066.png)

_**Q151:** Any horse owned by more than ten people, at least one is Sarnorian. Only the Sarnorian owners should be reported_

![V1](Pictures/Q151.png)

_**Q152:** Any horse owned by more than ten people. Only the Sarnorian owners should be reported_

![V1](Pictures/Q152.png)

_**Q125:** Any dragon that the number of dragons it froze is greater than the number of dragons that froze it_

![V1](Pictures/Q125.png)

{1} is evaluated to zero when the optional part has no valid assignments.

_**Q126:** Any dragon that the number of dragons it froze is greater than the number of dragons it did not freeze_

![V1](Pictures/Q126.png)

_**Q249:** Any triplet of sets (people A, dragons B, horses C) where (i) any person in A owns at least one dragon in B and at least one horse in C, (ii) any dragon in B is owned by at least ten people in A, and (iii) any horse in C is owned by at least ten people in A_

![V1](Pictures/Q249.png)

_**Q250:** Any triplet of sets (people A, dragons B, horses C) where (i) any person in A owns at least one dragon in B or at least one horse in C, (ii) any dragon in B is owned by at least ten people in A, and (iii) any horse in C is owned by at least ten people in A_

![V1](Pictures/Q250.png)

_**Q344:** Any triplet of sets (people A, dragons B, horses C) where (i) any person in A owns at least one dragon in B and at least one horse in C, (ii) any dragon in B is owned by at least ten people in A, and (iii) any horse in C is owned by less than five people in A_

![V1](Pictures/Q344.png)

_**Q345:** Any triplet of sets (people A, dragons B, horses C) where (i) any person in A owns at least one dragon in B and does not own at least one horse in C (ii) any dragon in B is owned by at least ten people in A, and (iii) any horse in C is not owned by at least ten people in A_

![V1](Pictures/Q345.png)

_**Q82:** Any dragon that froze all other dragons. **Alternative phrasing:** Any dragon for which there is no dragon (besides itself) it did not freeze_ (version 2)

![V1](Pictures/Q082-2.png)

The dragons that were frozen will not be a part of the assignment.

_**Q360:** Any dragon that (froze or fired at) all other dragons. **Alternative phrasing:** Any dragon for which there is no dragon (besides itself) it did not (freeze or fire at)_ (version 2)

![V1](Pictures/Q360-2.png)

_**Q64:** Any dragon that no more than four dragons owned by Sarnorians did not freeze it_

![V1](Pictures/Q064.png)

_**Q165:** Any dragon that no more than four Sarnorians own a dragon that did not freeze it_

![V1](Pictures/Q165.png)

_**Q206:** Any dragon A that for each of no more than four Sarnorians C there is a dragon C does not own that did not freeze A_

![V1](Pictures/Q206.png)

_**Q305:** Any person A that the number of horses A owns plus the number of dragons A owns - is at least ten_ (two versions)

![V1](Pictures/Q305-1.png)
![V1](Pictures/Q305-2.png)

_**Q121:** Any dragon that froze or fired at at least ten dragons_  (two versions)

![V1](Pictures/Q121-1.png)

'→' is the entity directly right of the combiner.

![V1](Pictures/Q121-2.png)

Note that any dragon that was both frozen and fired at - would be counted only once.

_**Q122:** Any dragon that fired at dragon B and fired at a dragon that fired at B - for at least ten different B's_

![V1](Pictures/Q122.png)

_**Q294:** Any dragon that fired at Balerion and at least nine other dragons_

![V1](Pictures/Q294.png)

_**Q175:** Any dragon that froze some dragon at least once and fired at some dragon at least once. The number of dragons it froze/fired at – is at least ten_

![V1](Pictures/Q175.png)

Note that if a dragon were both froze and fired at - it would be counted only once. A1 counts _distinct_ entity assignments.

_**Q298:** Any dragon A where (i) there is at least one black dragon A froze (ii) there is at least one white dragon A did not freeze, (iii) there is no gold dragon A froze, (iv) there is no silver dragon A did not freeze, and (v) the number of black or red dragons A froze plus the number of white and blue dragons A did not freeze - is at least ten. Report all black and red dragons that A froze and all white and blue dragons that A did not freeze_

![V1](Pictures/Q298.png)

Note that the entities that are right of a negator are not aggregated, but the entities that a right of a relationship/path-negator and no negator are aggregated. Compare with Q358.

_**Q176:** Any dragon that either (i) froze at least one dragon and fired at at least one dragon it did not freeze. The number of dragons it froze/fired at is at least ten, or (ii) froze at least ten dragons_

![V1](Pictures/Q176.png)

_**Q293:** Any person A that at least 80% of the horses owned by A and/or by (at least) one of A's parents - are jointly owned by A and by (at least) one of A's parents_

![V1](Pictures/Q293.png)

_**Q288:** Any person A who owns a dragon that froze more dragons than any dragon owned by any of A's ancestors_

![V1](Pictures/Q288.png)

_**Q290:** Any path of length up to ten between Rogar Bolton and Robin Arryn that does not contain hubs (in this pattern - hubs are entities with degree ≥ 1000)_

![V1](Pictures/Q290.png)

_**Q329:** Any Sarnorian who has paths of length up to six to more than five Omberians, Each of these paths passes through Rogar Bolton_

![V1](Pictures/Q329.png)

_**Q295:** Any person A that the number of dragons A owns plus the number of dragons A does not own that A's dragons fired at - is at least ten_

![V1](Pictures/Q295.png)

Note that if any of A's dragons were fired at one of A's other dragons - it would not be counted twice.

_**Q248:** Any pair of dragons (A, C) where A froze more than ten dragons that froze C_

![V1](Pictures/Q248.png)

{1} is a property of each unique assignment to A × C in 𝑆.

_**Q243:** Any pair of people (A, D) where at least five of A's dragons froze one or more D's dragons_

![V1](Pictures/Q243.png)

_**Q244:** Any pair of people (A, D) where at least five of A's dragons froze D's dragons, and at least five D's dragons were frozen by one or more A's dragons_

![V1](Pictures/Q244.png)

_**Q27:** Any person A where there are less than 500 horses (combined) of the same colors as A's owned horses_

![V1](Pictures/Q027.png)

_**Q28:** Any person who owns a horse of a rare color (there are less than 500 horses of that color)_

![V1](Pictures/Q028.png)

Patterns Q346, Q352, and Q355, are similar in structure, but the aggregation is different:

_**Q346:** Any dragon that froze at least ten dragons of the colors of horses of its owners_

![V1](Pictures/Q346.png)

_**Q352:** Any dragon that froze at least ten dragons of the colors of horses of one of its owners_

![V1](Pictures/Q352.png)

_**Q355:** Any dragon that froze at least ten dragons of the color of one horse of one of its owners_

![V1](Pictures/Q355.png)

_**Q335:** Any person and that person's parent for whom there is no horse that both are its owners_

![V1](Pictures/Q335.png)

_**Q334:** Any dragon A for which there is no dragon B that A both froze and fired at_

![V1](Pictures/Q334.png)

_**Q207:** Any pair of dragons where the difference between the number of dragons that one of them froze and the number of dragons that the other froze is less than 10_

![V1](Pictures/Q207.png)

{3} is a property of each unique assignment to A × C in 𝑆.

_**Q279:** Any pair of people (A, E), each own dragons, where no A's dragon froze any of E's dragons_

![V1](Pictures/Q279.png)

_**Q82:** Any dragon that froze all other dragons. **Alternative phrasing:** Any dragon for which there is no dragon (besides itself) it did not freeze_ (version 3)

![V1](Pictures/Q082-3.png)

{1} is a global property.

In contrast to Q82v1 and Q82v2, the frozen dragons will be a part of the assignment.

_**Q360:** Any dragon that (froze or fired at) all other dragons. **Alternative phrasing:** Any dragon for which there is no dragon (besides itself) it did not (freeze or fire at)_ (version 3)

![V1](Pictures/Q360-3.png)

_**Q375:** The difference between the number of white dragons and the number of black dragons_

![V1](Pictures/Q375.png)

{2}, {4} and {5} are global properties. {5} is a global expression.

## A2 Aggregator

![V1](Pictures/Agg-A2.png)

Let __𝑅__ denote a list where each element is a pattern-relationship/pattern-path. 𝑅 must be nonempty.

The lower part of an A2 aggregator contains the following elements:

- '↑' (an arrow pointing to the relationship/path on top of the aggregator):

  When A2 appears below a relationship/path - card(𝑅) = 1; _R[1]_ = the relationship/path.

  When A2 appears below a quantifier-input - each element in 𝑅 is the relationship/path that follows one branch of the quantifier, excluding relationships/paths wrapped with a negator or a relationship/path-negator (see Q123, Q358).

  Let ___RA(n, o)___ denote the list of all assignments to _R[o]_ in _S(n)_. 

- **aggregation-tag**: _at(n) = &#124;RA(n, 1)_ ∪ _RA(n, 2)_ ∪ ... _&#124;_

  We are using _card(union(all assignments to all elements in R))_ instead of _sum(card(assignment to one element in R))_ since two elements in 𝑅 may have the same assignment (see Q100), and we are counting _distinct_ assignments to all elements in 𝑅 per 𝑛.

  When an optional part has no assignments, A2 aggregation-tags defined right of the 'O' are evaluated to zero (see Q347).

- an optional **constraint** on _at(n)_

  **For each 𝑛: _S(n)_ is reported only if _at(n)_ satisfies the constraint**
  
  Note that a '= 0' constraint cannot be satisfied, as such constraint means that there are no assignments to the pattern excluding this aggregator. Similarly:

  - no constraint is equivalent to '_> 0_' constraint
  - '_≠ expr_', '_< expr_', and '_≤ expr_' constraints are not satisfied when _at(n)_ is zero
  - '_= expr_', '_≥ expr_', and '_∈ [expr1, expr2]_' constraints are not satisfied when _at(n)_ is zero even if _expr_ is evaluated to zero

A2 location:

- Below the relationship/path whose assignments are counted.

  A relationship/path with an A2 below it may be wrapped with an 'O'.
  
- Below the quantifier-input (except a _None_ quantifier) that assignments to relationships/paths on its right are counted (see Q297, Q174).

  A quantifier-input with an A2 below it:
  - may be wrapped with an 'O' (except at the pattern's start)
  - at least one branch of the quantifier (or subbranch of a sequence of quantifiers) must start with a relationship/path with no relationship/path-negator that is not wrapped with a negator, nor preceded by a _None_ quantifier

_**Q71:** Any dragon A that froze dragons more than ten times_

![V1](Pictures/Q071.png)

_**Q72:** Any dragon A that was frozen exactly ten times_ (two versions)

![V1](Pictures/Q072-1.png)

![V1](Pictures/Q072-2.png)

_**Q73:** Any dragon that did not freeze dragons or froze dragons no more than ten times_ (two versions)

![V1](Pictures/Q073-1.png)

![V1](Pictures/Q073-2.png)

_**Q123:** Any dragon that either froze or fired at dragons - at least ten times_

![V1](Pictures/Q123.png)

(Per assignment to A - counting the number of assignments to the relationship directly right of the quantifier)

_**Q104:** Any person who owned white horses at least ten times (same or different horses)_

![V1](Pictures/Q104.png)

_**Q296:** Any person who owned horses at least ten times - each horse is either white or weighs more than 100 Kg_ (two versions)

![V1](Pictures/Q296-1.png)

![V1](Pictures/Q296-2.png)

If B and C are assigned with the same graph-entity (a white horse that weighs more than 100 Kg), identical assignments to the two _owns_ relationships would be counted only once. A2 counts _distinct_ relationship/path assignments.

_**Q79:** Any person with more than five paths of length up to four to other people_

![V1](Pictures/Q079.png)

_**Q105:** Any dragon A that was frozen exactly two times by dragons, each of which was frozen at least once. A might have been frozen by additional dragons that were never frozen_

![V1](Pictures/Q105.png)

_**Q245:** Any dragon B that was frozen at least once and froze dragons exactly twice_

![V1](Pictures/Q245.png)

_**Q185:** Any dragon Balerion froze at least once on or after January 1, 1010, at least once for less than ten minutes, and at least twice (on or after January 1, 1010, or for less than ten minutes)_

![V1](Pictures/Q185.png)

(counting the number of *distinct* _freezes_ relationships)

_**Q127:** Any dragon that froze dragons owned by Sarnorians more times than dragons owned by Omberians_

![V1](Pictures/Q127.png)

_**Q339:** Any dragon Balerion froze more than ten times; report only those freezes that are in or after 1010_

![V1](Pictures/Q339.png)

_**Q297:** Any dragon that the number of times it fired at dragons plus the number of paths of length up to three between it and some horse - is at least ten_

![V1](Pictures/Q297.png)

_**Q124:** Any dragon that either (froze a dragon) or (fired at a dragon that fired at a dragon) - at least ten times_

![V1](Pictures/Q124.png)

_**Q173:** Any dragon that fired at at least two dragons and fired at least ten times_

![V1](Pictures/Q173.png)

(counting the number of *distinct* _fired at_ relationships). C is explicit latent to avoid redundant results (any dragon fired at would be assigned at least once to B and at least once to C).

_**Q174:** Any dragon that either (i) froze at least one dragon and fired at at least one dragon it did not freeze, or (ii) froze at least two dragons. If (i): the number of times it froze/fired at dragons is at least ten; otherwise: the number of times it froze dragons is at least ten_

![V1](Pictures/Q174.png)

_**Q75:** Any pair of dragons (A, B) where B froze A between eight and ten times_

![V1](Pictures/Q075.png)

_**Q76:** Any dragon that froze Balerion between eight and ten times_

![V1](Pictures/Q076.png)

_**Q242:** Any pair of people (A, D) where at least five times one of A's dragons froze one of D's dragons_

![V1](Pictures/Q242.png)

_**Q358:** Any dragon A where (i) there is at least one black dragon A froze (ii) there is at least one white dragon A did not freeze, (iii) there is no gold dragon A froze, (iv) there is no silver dragon A did not freeze, and (v) the number of times A froze black and red dragons is at least ten. Report all black and red dragons that A froze_

![V1](Pictures/Q358.png)

Pattern-relationships that are wrapped with a negator and pattern relationships with a relationship/path-negator are not aggregated. Compare with Q298.

_**Q372:** The number of times dragons were frozen_

![V1](Pictures/Q372.png)

{1} is a global property.

## A3 Aggregator

![V1](Pictures/Agg-A3.png)

The lower part of an A3 aggregator contains the following elements:

- One of the following:

  - _aggop expr_

    _aggop_ is _min/max/avg/sum_ - for aggregating values of a supported type, _union/intersection_ - for aggregating sets/bags of any type, or _distinct/set/bag_ - for aggregating values of any type. _distinct_ returns the number of distinct _non-null_ evaluation results; _set_ returns a set of all _non-null_ evaluation results (see Q332v2); _bag_ returns a bag of all _non-null_ evaluation results (see Q315).

    _expr_ is an expression composed of constants, properties, and subproperties of 𝑅 (when A3 appears below a relationship 𝑅 with no relationship-negator) (see Q86, Q354), EA-tags defined on top of the aggregator (see Q277), and EA-tags defined right of the aggregator (see Q116, Q137, Q169)
  
  - _distinct_ ⟨ _ett_ ⟩

    ⟨ _ett_ ⟩ is an entity type-tag defined right of the aggregator (see Q167v2)
    
  - _distinct_ ⟪ _rtt_ ⟫

    ⟪ _rtt_ ⟫ is a relationship type-tag defined on top of the aggregator or right of the aggregator (see Q366v2)
    

  Let ___BA(n)___ denote the list of the evaluation results of _expr_/⟨ _ett_ ⟩/⟪ _rtt_ ⟫ for all assignments in _S(n)_.
  
- **aggregation-tag**:
  - if _aggop_ is _min/max/avg/sum/distinct/union/intersection_: _at(n) = aggop(BA(n)[1], BA(n)[2], ...)_
  - if _aggop_ is _set_: _at(n) = {BA(n)[1], BA(n)[2], ...}_ (with no duplicate values and no _null_ values)
  - if _aggop_ is _bag_: _at(n) = [BA(n)[1], BA(n)[2], ...]_ (with duplicate values and no _null_ values)

  When an optional part has no assignments, A3 aggregation-tags defined right of the 'O' are evaluated to _null_ when _aggop_ is _min_/_max_/_avg_, evaluated to zero when _aggop_ is _sum_/_distinct_, and evaluated to {} / [] when _aggop_ is _set/bag/union/intersection_.

- an optional **constraint** on _at(n)_

  **For each 𝑛: _S(n)_ is reported only if _at(n)_ satisfies the constraint**

  When aggop is distinct: a '= 0' constraint cannot be satisfied unless _ett_ / _rtt_ is right of an 'O', as such constraint means that there are no assignments to the pattern excluding this aggregator. Similarly:
  
  - no constraint is equivalent to '_> 0_' constraint
  - '_≠ expr_', '_< expr_', and '_≤ expr_' constraints are not satisfied when _at(n)_ is zero
  - '_= expr_', '_≥ expr_', and '_∈ [expr1, expr2]_' constraints are not satisfied when _at(n)_ is zero even if _expr_ is evaluated to zero

A3 location:

- Below a relationship/path

  When A3 appears below a relationship 𝑅 and _expr_ is composed of at least one property or subproperty of 𝑅 - 𝑅 may be wrapped with an 'O'. Otherwise - a relationship/path with an A3 below it may have a relationship/path-negator and may be wrapped with an 'O'.
  
- Below a quantifier-input (excluding quantifier at the start of the pattern)

  A quantifier-input with an A3 below it may be wrapped with an 'O'.
  
- Below the pattern's start (even when followed by a quantifier)

  See Q263.
  

- If all entities in 𝑇 are in a sequence: A3 appears directly right of the leftmost member of 𝑇
- If entities in 𝑇 are in different branches of a quantifier: A3 appears directly left of the quantifier

When units of measure are defined for the expression (based on the aggregation operator, on the units of measures of the properties, and on the operators that compose the expression) - they are depicted as well (see Q86, Q117).

_**Q86:** Any pair of dragons (A, B) where A froze B for a cumulative duration longer than 100 minutes_

![V1](Pictures/Q086.png)

_**Q87:** Any dragon that was frozen at least once, and the cumulative duration it was frozen is less than 100 minutes_

![V1](Pictures/Q087.png)

_**Q89:** Any dragon that froze dragons for more than three different durations_

![V1](Pictures/Q089.png)

_**Q167:** Any person whose owned entities are all of the same type_ (version 2)

![V1](Pictures/Q167-2.png)

_**Q366:** Any pair of dragons that have relationships of at least three types_ (version 2)

![V1](Pictures/Q366-2.png)

_**Q117:** Any person whose owned horses' average weight is greater than 450 Kg_

![V1](Pictures/Q117.png)

If a horse were owned twice - it would be counted only once.

_**Q116:** Any person A that the number of distinct colors of A's owned horses - is between one and three_

![V1](Pictures/Q116.png)

_distinct_ does not aggregate _null_ evaluation results. If all dragons have a _null_ color, {2} would be equal to zero.

_**Q134:** Any person A that the number of distinct colors of all horses owned by A's friends - is between one and three_

![V1](Pictures/Q134.png)

_**Q229:** Any person A that the number of distinct colors of all horses owned by A's friends and parents - is between one and three_

![V1](Pictures/Q229.png)

_**Q135:** Any person A that the average weight of all horses owned by A's friends - is greater than 450 Kg_

![V1](Pictures/Q135.png)

Note that if some person A1 is a friend of two people who jointly own a horse, this horse's weight would be counted once.

_**Q137:** Any dragon A that froze dragons B. Each B froze at least one dragon, which is not A. All these B's together froze dragons for more than 100 minutes cumulatively_

![V1](Pictures/Q137.png)

_**Q336:** For each dragon: each time it froze some dragon for a duration longer than the average duration of all its freezes_

![V1](Pictures/Q336.png)

First, any triplet (A, B, C) matching the pattern excluding the aggregator and the constraint in {2} is found. Then, {1} is evaluated per assignment to A, and then the constraint in {2} is evaluated per relationship assignment. See also Q337.

Note that a dragon that always froze dragons for the same duration is not a valid assignment to A.

_**Q354:** Any dragon that froze one dragon and fired at another dragon, or froze a dragon and fired at more than one dragon, or fired at a dragon and froze more than one dragon, and there is some dragon that the earliest time it fired at it is earlier than the earliest time if froze any other dragon_

![V1](Pictures/Q354.png)

_**Q139:** Any person A who owns horses of the same number of colors as the number of colors of the horses owned by A's parents cumulatively_

![V1](Pictures/Q139.png)

_**Q98:** Any pair of dragons (A, X) where A froze more than three dragons and (A froze X more than ten times or for a cumulative duration of more than 100 minutes)_

![V1](Pictures/Q098.png)

_**Q88:** Any pair of dragons (A, B) where the cumulative duration A froze B is equal to the cumulative duration B froze A_

![V1](Pictures/Q088.png)

A property of entity _et_ cannot be aggregated per _et_, nor per a superset of _et_.

![V1](Pictures/Illegal-Agg01-1.png)

{1} is a property of B and hence cannot be aggregated per A × B.

If entity _et_ has a property that references
* an aggregation of a property of a Cartesian product composed of _et1_, or
* an aggregation of a property of a relationship between _et_ and _et1_

then

* a property of _et_ cannot be constrained based on a property of _et1_
* a property of _et1_ cannot be constrained based on a property of _et_

![V1](Pictures/Illegal-Agg02-1.png)

![V1](Pictures/Illegal-Agg02-2.png)

In the two patterns above, {2} - a property of A, is referencing an aggregation of a property of a relationship between A and B. Therefore, a property of A cannot be constrained by a property of B. Similarly, a property of B cannot be constrained by a property of A.

![V1](Pictures/Illegal-Agg02-3.png)

![V1](Pictures/Illegal-Agg02-4.png)
  
In the two patterns above, {2} - a property of B, is referencing an aggregation of a property of a relationship between A and B. Therefore, a property of B cannot be constrained by a property of A. Similarly, a property of A cannot be constrained by a property of B.

## Min/Max Aggregators

Sometimes we need to limit assignments to a Cartesian product of entity-tags, based on some value, to [all but] the 𝑘 assignments for which the value is the lowest/highest. Here are some examples:

- _Any person A and A's five oldest offspring_
- _Any dragon and the three dragons it froze the largest number of times_
- _Any dragon and the four dragons it froze for the maximal cumulative duration_

Three types are M aggregators are presented:

- M1: the value is the number of assignments to a union of Cartesian products of entity-tags
- M2: the value is the number of assignments to relationships/paths
- M3: the value is the evaluation result of an expression

M1 and M2 are syntactic sugar, which, alternatively, can be implemented by chaining A1 or A2 to an M3.

## M1 Aggregator

![V1](Pictures/Agg-M1.png)

Let __𝐵__ denote a Cartesian product of one or more entity-tags. 𝐵 must be nonempty and must not be composed of entity-tags composing 𝑇.

Let __𝑀__ denote a list of Cartesian products of entity-tags. 𝑀 must be nonempty and must not be composed of entity-tags composing 𝐵 or 𝑇.

The lower part of an M1 aggregator contains the following elements:

- optional: "all but"

- 𝑘: positive integer

- One of the following:
  - '_et1 × et2 × ..._': 𝐵 = _et1_ × _et2_ × ...
  - '←': 𝐵 = entity-tag directly left of the aggregator
  - '→': 𝐵 = entity-tag directly right of the aggregator not wrapped with a negator nor preceded by a _None_ quantifier (valid only when there is exactly one such entity-tag)
  - '⟷': _B = et1 × et2_, where _et1_ is the entity-tag directly left of the aggregator, and there is a single entity-tag directly right of the aggregator not wrapped with a negator nor preceded by a _None_ quantifier - _et2_

  Let ___BA(n)___ denote the list of all unique assignments to 𝐵 in _S(n)_. _BA(n)[o]_ is the _o'th_ assignment.

- One of the following:
  - with min
  - with max

- One of the following (_with min/max ..._):
  - '_et11 × et12 × ..._ ∪ _et21 × et22 × ..._ ∪ _..._' (all products have the same arity):

    𝑀 = [_et11 × et12 × ..._ , _et21 × et22 × ..._ , _..._]
    
  - '←':

    card(𝑀) = 1; 𝑀[1] = [_et_] where _et_ is directly left of the aggregator
    
  - '→':

    𝑀 = [_et1, et2, ..._] where _et1, et2, ..._ are directly right of the aggregator and not wrapped with a negator nor preceded by a _None_ quantifier (equivalent to '_et1_ ∪ _et2_ ∪ ...')
    
  - '⟷':

    𝑀 = [_et × et1, et × et2, ..._] where _et_ is directly left of the aggregator and _et1, et2, ..._ are directly right of the aggregator and not wrapped with a negator nor preceded by a _None_ quantifier (equivalent to '_et × et1_ ∪ _et × et2_ ∪ ...')

  Let ___MA(n, o, p)___ denote the list of all assignments to _M[p]_ in the subset of _S(n)_ that contains _BA(n)[o]_.

  ___MC(n, o)___ = _MA(n, o, 1) ∪ MA(n, o, 2) ∪ ..._ - the set of unique assignments to elements in 𝑀 in the subset of _S(n)_ that contains _BA(n)[o]_.

**For each 𝑛: from the set of assignments in _S(n)_ - [all but] the 𝑘 assignments _BA(n)[k]_ with the minimal/maximal positive card(_MC(n, k)_) are reported.**

If there are only 𝑗<𝑘 assignments - all those 𝑗 are valid assignments. If there are 𝑗>𝑘 assignments with equal extremum - all those 𝑗 are valid assignments.

M1 location:
- Below a relationship/path

  A relationship/path with an M1 below may have a relationship/path-negator, and may be wrapped with 'O'.
  
- Below a quantifier-input (excluding quantifier at the start of the pattern)

  A quantifier-input with an M1 below may be wrapped with an 'O' (except at the pattern's start)
  
- Below the pattern's start (even when followed by a quantifier)

  See Q299v1, Q299v2, Q262.
  
- If 𝑇 is empty - M1 appears directly right of the leftmost entity in 𝐵
- If 𝑇 is not empty and all the entities in 𝑇 appear right of all the entities in 𝐵: left of the rightmost entity in 𝑇. Otherwise: right of the leftmost entity in 𝑇
- If entities in 𝑇 ∪ 𝐵 are in different branches of a quantifier: directly left of the quantifier

_**Q67:** The three people with the largest number of parents_

![V1](Pictures/Q067.png)

_**Q68:** The two dragons that were frozen by the largest number of dragons_

![V1](Pictures/Q068.png)

_**Q69:** The two entities that own the largest number of entities_

![V1](Pictures/Q069.png)

_**Q70:** The five people who the number of people with a path of length up to four from each of them - is the smallest_

![V1](Pictures/Q070.png)

_**Q196:** Any **dragon** owned by Brandon Stark and the three dragons **it** froze that froze the largest number of dragons_

![V1](Pictures/Q196.png)

_**Q197:** Any person A and A's three dragons that the dragons they froze - froze the largest number of distinct dragons cumulatively_

![V1](Pictures/Q197.png)

_**Q234:** Any person and the three dragons whose dragons froze - that froze the largest number of dragons_

![V1](Pictures/Q234.png)

_**Q236:** Any person A and the three dragons whose dragons froze - that were frozen by the largest number of A's dragons_

![V1](Pictures/Q236.png)

_**Q227:** Any **dragon** owned by Brandon Stark, and the three dragons **it** froze or fired at - that froze the largest number of dragons_

![V1](Pictures/Q227.png)

_**Q238:** For any pair of people (A, D) where A's dragons froze D's dragons - A's three dragons that froze the largest number of D's dragons_

![V1](Pictures/Q238.png)

_**Q299:** For any pair of people (A, E) where at least one of A's dragons froze at least one of E's dragons - A's (up to) three dragons that cumulatively froze the largest number of E's dragons_ (two versions)

![V1](Pictures/Q299-2.png)

Note that the same graph-entity may be assigned to B, C, and D.

![V1](Pictures/Q299-1.png)

## M2 Aggregator

![V1](Pictures/Agg-M2.png)

Let __𝐵__ denote a Cartesian product of one or more entity-tags. 𝐵 must be nonempty and must not be composed of entity-tags composing 𝑇.

The lower part of an M2 aggregator contains the following elements:

- optional: "all but"

- 𝑘: positive integer

- One of the following:
  - '_et1 × et2 × ..._': 𝐵 = _et1_ × _et2_ × ...
  - '←': 𝐵 = entity-tag directly left of the aggregator (see Q78, Q171, Q172)
  - '→': 𝐵 = entity-tag directly right of the aggregator not wrapped with a negator nor preceded by a _None_ quantifier (valid only when there is exactly one such entity-tag) (see Q195, Q228)
  - '⟷': 𝐵 _= et1 × et2_, where _et1_ is the entity-tag directly left of the aggregator, and there is a single entity-tag directly right of the aggregator not wrapped with a negator nor preceded by a _None_ quantifier - _et2_ (see Q77, Q80)

  Let ___BA(n)___ denote the list of all unique assignments to 𝐵 in _S(n)_. _BA(n)[o]_ is the _o'th_ assignment.

- One of the following:
  - with min ↑
  - with max ↑

  Let __𝑅__ denote a list where each element is a pattern-relationship/pattern-path. 𝑅 must be nonempty.

  When M2 appears below a relationship/path - card(𝑅) = 1; _R[1]_ = the relationship/path.

  When M2 appears below a quantifier-input - each element in 𝑅 is the relationship/path that follows one branch of the quantifier, excluding relationships/paths wrapped with a negator or a relationship/path-negator.

  Let ___RA(n, o, p)___ denote the list of all assignments to _R[p]_ in the subset of _S(n)_ that contains _BA(n)[o]_.

  ___RC(n, o)___ = _RA(n,o,1)_ ∪ _RA(n, o, 2)_ ∪ ... - the set of unique assignments to elements in 𝑅 in the subset of _S(n)_ that contains _BA(n)[o]_.

**For each 𝑛: from the set of assignments in _S(n)_ - [all but] the 𝑘 assignments _BA(n)[k]_ with the minimal/maximal positive card(_RC(n, o)_) are reported.**

If there are only _j<k_ assignments - all those 𝑗 are valid assignments. If there are _j>k_ assignments with equal extremum - all those 𝑗 are valid assignments.

M2 location:
- Below the relationship/path whose assignments are counted

  A relationship/path with an M2 below it may be wrapped with an 'O'.
  
- Below a quantifier-input (except a _None_ quantifier) that assignments to relationships/paths on its right are counted

  A quantifier-input with an M2 below it:
  - may be wrapped with an 'O'
  - at least one branch of the quantifier (or subbranch of a sequence of quantifiers) must start with a relationship/path with no relationship/path-negator that is not wrapped with a negator nor preceded by a _None_ quantifier
  
_**Q78:** The four dragons that froze Balerion the largest number of times_

![V1](Pictures/Q078.png)

_**Q171:** The two dragons that were frozen the largest number of times_

![V1](Pictures/Q171.png)

_**Q172:** The five people with the smallest positive number of paths of length up to four to some person_

![V1](Pictures/Q172.png)

(Compare with Q324)

_**Q77:** The five pairs of dragons (A, B) with the largest number of times B froze A_

![V1](Pictures/Q077.png)

_**Q80:** The three pairs of people with the largest number of paths of length up to four between them_

![V1](Pictures/Q080.png)

_**Q195:** Any dragon owned by Brandon Stark and the three dragons it froze the largest number of times_

![V1](Pictures/Q195.png)

_**Q231:** Any person A and the three dragons that were frozen by dragons that were frozen the largest number of times by A's dragons_

![V1](Pictures/Q231.png)

_**Q228:** Any dragon owned by Brandon Stark that fired at at least two dragons, and the three dragons it fired the largest number of times_

![V1](Pictures/Q228.png)

(counting the number of relationships assignments directly right of the quantifier)

_**Q237:** For any pair of (A - a dragons owner, and C - a dragon that was frozen by A's dragons) - the three dragons owned by A that froze C the largest number of times_

![V1](Pictures/Q237.png)

_**Q239:** For any pair of people (A, D) where A's dragons froze D's dragons - the three pairs of (A's dragon B, D's dragon C) where B froze C the largest number of times_

![V1](Pictures/Q239.png)

## M3 Aggregator

![V1](Pictures/Agg-M3.png)

Let __𝐵__ denote a Cartesian product of one or more entity-tags.

The lower part of an M3 aggregator contains the following elements:

- optional: "all but"

- 𝑘: positive integer

- One of the following:
  - '_et1 × et2 × ..._': 𝐵 = _et1_ × _et2_ × ...
  - '←': 𝐵 = entity-tag directly left of the aggregator
  - '→': 𝐵 = entity-tag directly right of the aggregator not wrapped with a negator nor preceded by a _None_ quantifier (valid only when there is exactly one such entity-tag)
  - '⟷': 𝐵 _= et1 × et2_, where _et1_ is the entity-tag directly left of the aggregator, and there is a single entity-tag directly right of the aggregator not wrapped with a negator nor preceded by a _None_ quantifier - _et2_

  Let ___BA(n)___ denote the list of all unique assignments to 𝐵 in _S(n)_. _BA(n)[o]_ is the _o'th_ assignment.

- One of the following:
  - with min _expr_
  - with max _expr_

  _expr_ is an expression composed of constants, properties, and subproperties of 𝑅 (when M3 appears below a relationship 𝑅 with no relationship-negator) (see Q74), EA-tags defined on top of the aggregator (see Q91, Q274, Q306), and EA-tags defined right of the aggregator (see Q130, Q128)

  Let ___RA(n, o)___ denote the minimal/maximal assignment to _expr_ in the subset of _S(n)_ that contains _BA(n)[o]_.

**For each 𝑛: from the set of assignments in _S(n)_ - [all but] the 𝑘 assignments _BA(n)[k]_ with the minimal/maximal _RA(n, k)_ are reported.**

If there are only _j<k_ assignments - all those 𝑗 are valid assignments. If there are _j>k_ assignments with equal extremum - all those 𝑗 are valid assignments.

M3 location:
- Below a relationship/path

  When M3 appears below a relationship 𝑅, and _expr_ is composed of at least one property or subproperty of 𝑅, 𝑅 may be wrapped with an 'O'. Otherwise - a relationship or path with an M3 below it may have a relationship/path-negator and may be wrapped with an 'O'.

- Below a quantifier-input (excluding quantifier at the start of the pattern)

  A quantifier-input with an M3 below it may be wrapped with an 'O'.
  
- Below the pattern's start (even when followed by a quantifier)

  See Q130, Q216, Q264.
  
- If 𝑇 is empty - M3 appears directly right of the leftmost entity in 𝐵
- If 𝑇 is not empty and all the entities in 𝑇 appear right of all the entities in 𝐵: left of the rightmost entity in 𝑇. Otherwise: right of the leftmost entity in 𝑇
- If entities in 𝑇 ∪ 𝐵 are in different branches of a quantifier: directly left of the quantifier

_**Q130:** The four oldest people_

![V1](Pictures/Q130.png)

_**Q131:** The four oldest males_

![V1](Pictures/Q131.png)

_**Q118:** Any person A and A's three oldest offspring_

![V1](Pictures/Q118.png)

_**Q119:** Any person A and A's three youngest sons_

![V1](Pictures/Q119.png)

_**Q74:** Any person A and A's three horses with the minimal first ownership start date_

![V1](Pictures/Q074.png)

_**Q230:** Any person A and the three people A is a friend of or a friend of an offspring of - that own the heaviest horses_

![V1](Pictures/Q230.png)

_**Q232:** Any person A and the three heaviest horses owned by people A is a friend of or a friend of an offspring of_

![V1](Pictures/Q232.png)

## R1 Aggregator

![V1](Pictures/Agg-R1.png)

Let __𝑅__ denote the relationship R1 appears below it.

The lower part of an R1 aggregator contains the following elements:

- optional: "all but"
- 𝑘: positive integer
- '↑' (an arrow pointing to the relationship on top of the aggregator):
- One of the following:
  - with min _relExpr_
  - with max _relExpr_

  _relExpr_ is an expression containing at least one property or subproperty of 𝑅

  Let ___RA(n)___ denote the list of all unique assignments to 𝑅 in _S(n)_. _RA(n)[o]_ is the _o'th_ assignment.

**For each 𝑛: from the set of assignments in _S(n)_ - [all but] the 𝑘 assignments _RA(n)[k]_ with the minimal/maximal _relExpr_ are reported.**

If there are only _j<k_ assignments - all those 𝑗 are valid assignments. If there are _j>k_ assignments with equal extremum - all those 𝑗 are valid assignments.

R1 location:
- Below the relationship whose property is referenced

  The relationship may be wrapped with an 'O'.

_**Q241:** The four longest freezes_

![V1](Pictures/Q241.png)

_**Q161:** For each dragon that froze at least one dragon at least once: the four longest freezes_

![V1](Pictures/Q161.png)

_**Q160:** For each pair of dragons (A, B) where A froze B at least once: The four longest freezes_

![V1](Pictures/Q160.png)

_**Q240:** For any pair of people (A, D): The four longest freezes where any of A's dragons froze any of D's dragons_

![V1](Pictures/Q240.png)

## Aggregator Chains

Relationships' expressions and aggregators can be _chained_. When chained, each relationship's expression and each aggregator with a constraint is a filtering stage. An assignment is valid only if all the chained constraints are satisfied, or in other words - if it passed all filtering stages.

Within a chain, relationships' expressions may not appear below aggregators. Also, an aggregator may not appear in a horizontal quantifier's branch. It may appear below a horizontal combiner that combines all the branches (see Q302, Q303).

_**Q96:** Any dragon Balerion froze more than ten times - each on or after January 1, 1010, and for a duration shorter than ten minutes_

![V1](Pictures/Q096.png)

Note that the order of the constraints along the chain matters: the top two constraints filter relationships based on their properties' value. The third constraint is on the number of relationships that passed these filters.

_**Q302:** Any dragon A that froze at least three dragons - each at least one time where at least two of the following conditions are satisfied: (i) the freeze duration was longer than ten minutes (ii) the freeze started after January 1, 980 (iii) the freeze ended before February 1, 980_

![V1](Pictures/Q302.png)

See Q185 for a different pattern structure that could be used.

_**Q303:** Any pair of dragons (A, B) where A froze B at least three times, each for more than ten minutes, and either the freeze started after January 1, 980 or ended before February 1, 980_

![V1](Pictures/Q303.png)

_**Q186:** Any dragon Balerion froze more than ten times for less than ten minutes, and at least once for ten minutes or more_

![V1](Pictures/Q186.png)

_**Q99:** Any dragon Balerion froze more than ten times for less than ten minutes, and not once for ten minutes or more_ (two versions)

![V1](Pictures/Q099-1.png)

![V1](Pictures/Q099-2.png)

_**Q94:** Any dragon that froze more than three dragons: each more than five times for more than ten minutes_

![V1](Pictures/Q094.png)

Filtering stages:

- Pass freezes longer than ten minutes
- Pass pairs of dragons (A, B) where A froze B more than five times for more than ten minutes
- Pass dragons A that froze more than three B's each more than five times for more than ten minutes 

_**Q93:** Any dragon A that froze more than three dragons - each at least five times for more than ten minutes_

![V1](Pictures/Q093.png)

(Similar to Q94, but the order of the aggregators is switched.) Filtering stages:

- Pass freezes longer than ten minutes
- Pass dragons A that froze more than three B's - each at least once for more than ten minutes
- Pass pairs of dragons (A, B) where A froze B more than five times for more than ten minutes

A dragon A that froze more than three dragons, each at least once for more than ten minutes, but froze only a single dragon more than five times for more than ten minutes - is a valid assignment in Q93 but not in Q94.

_**Q272:** Any pair of dragons (A, B) where the longest freeze duration is at least ten times longer than the shortest freeze duration_

![V1](Pictures/Q272.png)

_**Q363:** Any dragon that the latest time it froze some dragon for the first time was in or after 1010_

![V1](Pictures/Q363.png)

_**Q91:** The four dragons with the maximal (shortest duration they were frozen for)_

![V1](Pictures/Q091.png)

_**Q90:** The four pairs of dragons (A, B) where A froze B for the maximal cumulative duration_

![V1](Pictures/Q090.png)

_**Q92:** The four dragons with the maximal (average duration they froze dragons for)_

![V1](Pictures/Q092.png)

_**Q201:** For each dragon that froze at least ten dragons: the three dragons it froze for the maximal cumulative duration_

![V1](Pictures/Q201.png)

_**Q274:** The four dragons with the largest difference between (the longest duration they froze dragon for) and (the shortest duration they froze dragon for)_

![V1](Pictures/Q274.png)

_**Q182:** Any dragon owned by Brandon Stark and the three dragons it froze for the maximal cumulative duration_

![V1](Pictures/Q182.png)

_**Q133:** The four people that the average weight of their horses is maximal_

![V1](Pictures/Q133.png)

_**Q132:** The four people who own horses of the largest number of colors_

![V1](Pictures/Q132.png)

_**Q233:** Any dragon A than froze dragons Bs that froze dragons Cs, and the three Cs for which A froze Bs for the maximal cumulative duration_

![V1](Pictures/Q233.png)

_**Q138:** The four people who the people each of them is a friend of - cumulatively own horses of the largest number of colors_

![V1](Pictures/Q138.png)

_**Q183:** The three dragons that dragons owned by Brandon Stark froze for the maximal cumulative duration_

![V1](Pictures/Q183.png)

_**Q168:** The three people who the number of types of entities each of them owns is the largest_

![V1](Pictures/Q168.png)

_**Q163:** Any dragon that the average duration of the ten shortest times it froze dragons is longer than 60 minutes_

![V1](Pictures/Q163.png)

_**Q162:** Any pair of dragons (A, B) where the second shortest duration A froze B is longer than 60 minutes_

![V1](Pictures/Q162.png)

_**Q341:** For each pair of dragons (A, B): the two shortest freezes A froze B. If there are more than two freezes with equal minimal duration - the two earliest amongst them_

![V1](Pictures/Q341.png)

_**Q268:** Any pair of dragons (A, B) where the average duration of the 11th-20th most prolonged freezes A froze B is longer than 60 minutes_ (two versions)

![V1](Pictures/Q268.png)

_**Q120:** Any person A whose three oldest offspring's cumulative height is lower than A's height_

![V1](Pictures/Q120.png)

_**Q202:** Any person A whose three oldest offspring's average height is lower than the average height of all A's offspring_

![V1](Pictures/Q202.png)

_**Q208:** Any person whose three oldest horse-owning sons cumulatively own horses of three colors_

![V1](Pictures/Q208.png)

_**Q140:** Any person whose three oldest sons cumulatively own horses of three colors_

![V1](Pictures/Q140.png)

Note that {3} is defined right of an 'O' and is evaluated to _null_ when the optional part has no valid assignment. distinct does not aggregate _null_ evaluation results.  

_**Q141:** Any person A whose three oldest sons cumulatively own horses of the same number of colors as of those cumulatively owned by A's three youngest daughters_

![V1](Pictures/Q141.png)

_**Q95:** Any dragon Balerion froze more than five times, each for more than ten minutes, and the total duration of those freezes was longer than 100 minutes_ (three versions)

![V1](Pictures/Q095-1.png)

The two _per pair_ constraints could be chained instead. The meaning would be similar:

![V1](Pictures/Q095-2.png)

![V1](Pictures/Q095-3.png)

_**Q199:** Any dragon that (froze more than ten times each of more than ten dragons) and (froze more than 20 times each of less than ten dragons)_

![V1](Pictures/Q199.png)

_**Q200:** Any dragon that (froze more than ten times each of more than ten dragons. For each of these ten dragons - the freezes had exactly two distinct durations) and (froze more than 20 times each of less than ten dragons. The average freeze duration of all these dragons - is longer than three minutes)_

![V1](Pictures/Q200.png)

_**Q277:** Any dragon that the longest (cumulative duration it froze some dragon) is more than ten times longer than the shortest (cumulative duration it froze some dragon)_

![V1](Pictures/Q277.png)

_**Q351:** Any person who owns two horses of different colors, a dragon that froze at least ten dragons of the same color as the first horse, and a dragon that froze at least ten dragons of the same color as the second horse_

![V1](Pictures/Q351.png)

_**Q320:** Any person A where there are more horses of the same colors as A's owned horses than dragons of the same colors as A's owned dragons_ (version 1)

![V1](Pictures/Q320-1.png)

The number of assignments to E is zero when the optional part has no assignment. We cannot simply test if E < {5} since if there are zero assignments to E per A - the constraint is not satisfied.

_**Q321:** Any person A where more horses than dragons have the same name-length as a horse/dragon A owns_ (version 1)

![V1](Pictures/Q321-1.png)

The number of assignments to D is zero when the optional part has no assignment. We cannot simply test if D < {4} since if there are zero assignments to D per A - the constraint is not satisfied.

_**Q259:** Any person who since 1011 become an owner of no horses or up to four horses_ (two versions)

![V1](Pictures/Q259-1.png)

![V1](Pictures/Q259-2.png)

_**Q337:** For each pair of dragons: each time one of them froze the other for a duration longer than the average duration of all the freezes that are longer than the average duration one of them froze the other_

![V1](Pictures/Q337.png)

_**Q317:** Any dragon that the time difference between the earliest time it froze/fired at some dragon and the latest time it froze/fired at some dragon - is at least one year_

![V1](Pictures/Q317.png)

{1}, {2}, {3} and {4} are defined right of an 'O', hence evaluated to _null_ when the optional part has no assignments. Since _min_({...}) and _max_({...}) ignore _nulls_, the pattern would yield the expected results even when dragon A froze no dragon or when dragon A fired at no dragon.

_**Q377:** Any dragon and the three earliest times it froze/fired at some dragon_

![V1](Pictures/Q377.png)

{1} and {2} are defined right of an 'O', hence evaluated to [] when the optional part has no assignments.

## Aggregator Sequences

Multiple aggregators may appear along a _sequence_. Along a sequence, aggregator's constraints are evaluated from right to left, except where a constraint depends on the evaluation result of a constraint on its left (see Q376).

_**Q128:** Any person A and A's three offspring who own horses of the largest number of colors_

![V1](Pictures/Q128.png)

_**Q198:** Any person A and A's three dragons that (for each of them: the four dragons it froze that froze the largest number of dragons) - froze the largest number of distinct dragons cumulatively_

![V1](Pictures/Q198.png)

_**Q328:** The three people with the maximal cumulative horse ownership days_

![V1](Pictures/Q328.png)

_**Q179:** Any pair of dragons (A, B) where A froze B for a cumulative duration longer than the cumulative duration B froze dragons_ (two versions)

![V1](Pictures/Q179-1.png)

![V1](Pictures/Q179-2.png)

_**Q103:** Any dragon A that froze at least three dragons, each of which was frozen by at least four dragons other than A_

![V1](Pictures/Q103.png)

_**Q106:** Any dragon A that froze dragons at least three times - each was frozen at least four times by dragons other than A_

![V1](Pictures/Q106.png)

_**Q164:** Any dragon that the number of times dragons it froze have frozen dragons (cumulatively) - is equal to the number of times dragons it fired at have fired at dragons (cumulatively)_

![V1](Pictures/Q164.png)

_**Q356:** The average number of horse ownerships per person_

![V1](Pictures/Q356.png)

_**Q324:** The five people with the smallest number (including zero) of paths of length up to four to some person_

![V1](Pictures/Q324.png)

(Compare with Q172)

_**Q107:** Any **dragon** that (the number of dragons, each owned by five people, that froze **it**) is five, and that the number of times **it** was frozen by those dragons (cumulatively) is not five_

![V1](Pictures/Q107.png)

_**Q169:** Any person A who (has at least one offspring who owns at least three horses) and (each of A's offspring who owns at least three horses - owns horses of at least three colors)_

![V1](Pictures/Q169.png)

_**Q129:** Any person A that (each of A's offspring who owns at least one horse - owns a different number of horses)_

![V1](Pictures/Q129.png)

_**Q181:** Any dragon with no intersection between the groups of dragons frozen by any two dragons it froze_

![V1](Pictures/Q181.png)

_**Q213:** Out of the pairs of dragons (A, B) where A froze B for a cumulative duration longer than the cumulative duration B froze dragons - the five pairs with the largest number of times A froze B_

![V1](Pictures/Q213.png)

When the order of the aggregators along the chain is switched, the meaning changes: 

_**Q376:** Out of the five pairs of dragons (A, B) with the largest number of times A froze B – the pairs where A froze B for a cumulative duration longer than the cumulative duration B froze dragons_

![V1](Pictures/Q376.png)

Since {2} has a constraint that depends on the evaluation of {1}, {1} must be evaluated first. However, the M2 aggregator filters (A, B) pairs before {1} is evaluated.

_**Q191:** Any dragon that froze dragons S and fired at dragons T. &#124;S&#124;≥3, &#124;T&#124;≥3 and &#124;S∪T&#124;≥10_

![V1](Pictures/Q191.png)

_**Q192:** Any dragon that froze dragon m≥3 times and fired at dragons n≥3 times. m+n≥10_

![V1](Pictures/Q192.png)

_**Q193:** Any dragon that froze dragons S, fired at dragons T, and fired ≥3 times. &#124;S&#124;≥3 and &#124;S∪T&#124;≥10_

![V1](Pictures/Q193.png)

_**Q194:** Any dragon that froze dragons m times, fired at dragons n≥3 times, and froze ≥3 dragons. m+n≥10_

![V1](Pictures/Q194.png)

_**Q97:** Any dragon that froze more than three dragons - each more than ten times or for a cumulative duration of more than 100 minutes_

![V1](Pictures/Q097.png)

_**Q180:** Any pair of dragons (A, B) where the cumulative duration A and B froze each other - is longer than both the cumulative duration A froze other dragons and the cumulative duration B froze other dragons_

![V1](Pictures/Q180.png)

_**Q347:** The five dragon triplets with the largest number of times triplet-members froze one another_

![V1](Pictures/Q347.png)

Note that we need to count each triplet only once (ignoring permutations).

_**Q100:** Any dragon Balerion froze more than ten times for less than ten minutes, more than ten times on or after January 1, 1010, more than 15 times for less than ten minutes or on or after January 1, 1010, and more than 100 times altogether_

![V1](Pictures/Q100.png)

## Extended Aggregators

![V1](Pictures/Agg-Extended.png)

For extended aggregators, 𝑇 denotes an _extended Cartesian product_ - a Cartesian product not just of entity-tags but of

* Zero or more entity-tags
* Zero or more expressions, each can be either
  * an inherent property of the attached relationship
  * an EA-tag
* Zero or more entity type-tags
* Zero or more relationship type-tags

Hence, for all extended aggregator types (A1, A2, A3, M1, M2, M3, and R1), the _per_ clause has the following format:

- '_et1 × et2 × ... × expr1 × expr2 × ... ×_ ⟨ _ett1_ ⟩ _×_ ⟨ _ett2_ ⟩ _× ... ×_ ⟪ _rtt1_ ⟫ _×_ ⟪ _rtt2_ ⟫ _× ..._:

  𝑇 = _et1 × et2 × ... × expr1 × expr2 × ... ×_ ⟨ _ett1_ ⟩ _×_ ⟨ _ett2_ ⟩ _× ... ×_ ⟪ _rtt1_ ⟫ _×_ ⟪ _rtt2_ ⟫ _× ..._
    
Wherever possible, the notations '←', '→' and '⟷' are used instead of entity-tags.

For all extended M aggregators (M1, M2, and M3), the same is true also for 𝐵.

Here are two examples:

First example: _Any person who at a certain date became an owner of more than five horses_ (Q115v2)
 
![V1](Pictures/Q115-2.png)

{1} is evaluated per each unique assignment to A × _df_._since_ in 𝑆.  In other words, {1} is a property of each unique assignment to A × _df_._since_ in 𝑆.

Second example: _For each color of dragons that Balerion froze - the three dragons it froze the largest number of times_ (Q215)
 
![V1](Pictures/Q215.png)

## Extended A1 Aggregator

![V1](Pictures/Agg-A1-Extended.png)

_**Q261:** Any color of which there are at least ten horses_

![V1](Pictures/Q261.png)

{2} is a property of each unique assignment to {1} in 𝑆.

_**Q115:** Any person who at a certain date became an owner of more than five horses_ (two versions)

![V1](Pictures/Q115-1.png)

![V1](Pictures/Q115-2.png)

{1} is a property of each unique assignment to A × _df_._since_ in 𝑆.

_**Q289:** Any person who at a certain three-day interval became an owner of more than five horses_

![V1](Pictures/Q289.png)

When 𝑛 intervals overlap, the intersection contains the start-time of at least one of the intervals (it also contains the end-time of at least one of the intervals).

_**Q283:** Any person who at a certain day owned at least five horses_

![V1](Pictures/Q283.png)

_**Q284:** Any person who owned the same five horses - for at least ten consecutive days_ (version 1)

![V1](Pictures/Q284-1.png)

_**Q378:** Any relationship-type that holds between more than 1000 pairs of dragons_

![V1](Pictures/Q378.png)

_**Q217:** Any dragon and the dragons it froze - on days it froze between one and five dragons_

![V1](Pictures/Q217.png)

_**Q114:** Any person who owns at least five horses of the same color_ (version 3)

![V1](Pictures/Q114-3.png)

_**Q218:** Any person who owns at least five entities of the same type_ (version 2)

![V1](Pictures/Q218-2.png)

_**Q157:** Any person who owns between one and three horses of the same color - for at least six colors_

![V1](Pictures/Q157.png)

_**Q214:** Any person who owns entities of at least four types. For each type - at least five entities_

![V1](Pictures/Q214.png)

_**Q235:** Any dragon that the number of dragons it froze on average - on months it froze dragons in - is at least ten_

![V1](Pictures/Q235.png)

_**Q153:** Any dragon A that in each of at least 11 days - froze between one and five dragons_

![V1](Pictures/Q153.png)

_**Q252:** Any dragon B that in each of at least 11 days - was frozen by a dragon that on that day froze between one and five dragons_

![V1](Pictures/Q252.png)

_**Q255:** Any dragon whose name-length is equal to the number of days in each of which it froze ten dragons_

![V1](Pictures/Q255.png)

_**Q278:** Any pair of dragons (A, B) that in each of at least 11 days - A froze between one and five dragons, one of them is B_

![V1](Pictures/Q278.png)

_**Q254:** Any dragon that in each year it froze dragons - it froze more than three dragons_

![V1](Pictures/Q254.png)

_**Q306:** The three dragons that the number of days in each of which each froze at least five dragons - is maximal_

![V1](Pictures/Q306.png)

_**Q285:** Any person who at a certain day owned at least five horses and at least five dragons_

![V1](Pictures/Q285.png)

_**Q286:** Any person who at each day of at least ten (not necessarily consecutive) days - owned at least five horses (not necessarily the same horses each day)_

![V1](Pictures/Q286.png)

_**Q158:** Any dragon that in each of at least ten days - the number of dragons it froze is greater than the number of dragons that froze it_

![V1](Pictures/Q158.png)

Note the location of {6}. If it was below {1}, the evaluation order was different. As it is, {6} counts distinct assignments to {1} for which the _All_ quantifier was satisfied.

_**Q260:** Any dragon that in each of at least ten days: (i) the number of dragons it froze is greater than the number of dragons that froze it, and (ii) it froze/was frozen at least five times_

![V1](Pictures/Q260.png)

_**Q350:** Any pair of people who own the same number of entities of each type_

![V1](Pictures/Q350.png)

_**Q332:** Any person whose horses are all of rare colors. A rare color is a color of less than 1% of the horses_ (two versions)

![V1](Pictures/Q332-1.png)

![V1](Pictures/Q332-2.png)

_**Q338:** Any person who owns at least ten horses, at least half of which are of rare colors. A rare color is a color of less than 1% of the horses_ (two versions)

![V1](Pictures/Q338-1.png)

![V1](Pictures/Q338-2.png)

## Extended A2 Aggregator

![V1](Pictures/Agg-A2-Extended.png)

_**Q330:** Any day during which more than five horse-ownerships started_

![V1](Pictures/Q330.png)

_**Q270:** Any person A and A's horses of the colors for which there are more than five horse-ownerships by a person_

![V1](Pictures/Q270.png)

## Extended A3 Aggregator

![V1](Pictures/Agg-A3-Extended.png)

_**Q331:** Any day at which the horse ownerships that started (and ended since) lasted on average for at least ten years_

![V1](Pictures/Q331.png)

_**Q271:** Any person A and A's horses of the colors for which the average ownership start date is at least January 1, 1010_

![V1](Pictures/Q271.png)

_**Q263:** Any horse of each color of which the average horses' weight is greater than 450 Kg_

![V1](Pictures/Q263.png)

_**Q265:** Any horse  of each color of which the average horses' owners' height is at least 180 cm_

![V1](Pictures/Q265.png)

_**Q155:** Any dragon that in each of at least 11 days - froze dragons for more than 100 minutes cumulatively_

![V1](Pictures/Q155.png)

_**Q156:** Any dragon that froze dragon for more than 1000 minutes cumulatively - in the days it froze dragons for more than 100 minutes_

![V1](Pictures/Q156.png)

_**Q154:** Any dragon that in each of at least four years - in each of at least 11 days - froze between one and five dragons_

![V1](Pictures/Q154.png)

_**Q159:** Any **dragon** for which there are more days where (it froze/was frozen at least five times, and the number of dragons **it** froze is greater than the number of dragons that froze **it**) than days where (it froze/was frozen at least five times, and the number of dragons that froze **it** is greater than the number of dragons **it** froze)_

![V1](Pictures/Q159.png)

(Compare with Q260)

_**Q361:** Any person A who owns horses of at least five colors, and the sets of weights of A's horses of each of these colors are identical_

![V1](Pictures/Q361.png)

_**Q320:** Any person A where there are more horses of the same colors as A's owned horses than dragons of the same colors as A's owned dragons_ (version 2)

![V1](Pictures/Q320-2.png)

_**Q321:** Any person A where more horses than dragons have the same name-length as a horse/dragon A owns_ (version 2)

![V1](Pictures/Q321-2.png)

## Extended M1 Aggregator

![V1](Pictures/Agg-M1-Extended.png)

_**Q220:** Any person A and A's horses of the three colors of which A owns the largest number of horses_

![V1](Pictures/Q220.png)

_**Q262:** Any horse of the three most common horse colors_

![V1](Pictures/Q262.png)

_**Q224:** Any person A and A's horses of the three colors with the smallest positive number of horse owners_ (two versions)

![V1](Pictures/Q224-1.png)

![V1](Pictures/Q224-2.png)

_**Q342:** The three colors for which the number of pairs of dragons of that color where at least one of them froze the other - is maximal_ (version 1)

Note that if a pair of dragons mutually froze each other - we need to count this pair only once.

![V1](Pictures/Q342-1.png)

_**Q343:** The three color-pairs (1, 2) for which the number of dragon pairs where a dragon of color 1 froze a dragon of color 2 is maximal_

![V1](Pictures/Q343.png)

_**Q251**: The three pairs (person, color) with most horses of this color owned by that person_

![V1](Pictures/Q251.png)

_**Q211:** For each total number of freezes by a dragon: the three dragons that froze the largest number of dragons_

![V1](Pictures/Q211.png)

_**Q212:** The three most common total number of freezes by a dragon_

![V1](Pictures/Q212.png)

_**Q325:** For each of the three most common horse colors: the three people who own the largest number of horses of this color_

![V1](Pictures/Q325.png)

_**Q326:** For the three most common horse colors (combined): the three people who own the largest number of horses of these colors_

![V1](Pictures/Q326.png)

## Extended M2 Aggregator

![V1](Pictures/Agg-M2-Extended.png)

_**Q215:** For each color of dragons that Balerion froze - the three dragons it froze the largest number of times_

![V1](Pictures/Q215.png)

_**Q221:** Balerion and the dragons it froze - of the three colors it froze dragons the largest number of times_

![V1](Pictures/Q221.png)

_**Q280:** Any dragon and the dragons it froze - of the three colors it froze dragons the largest number of times_

![V1](Pictures/Q280.png)

_**Q253:** For each number of dragon's owners - the three dragons Balerion froze the largest number of times_

![V1](Pictures/Q253.png)

_**Q225:** Any person A and A's horses of the three colors with the smallest positive number of horse ownerships by a person_

![V1](Pictures/Q225.png)

_**Q281:** Any dragon and the dragons it froze - of the three colors of which dragons were frozen the largest number of times_

![V1](Pictures/Q281.png)

_**Q319:** Any dragon and the dragons it froze - of the 4th-6th colors of which dragons were frozen the largest number of times_ (two versions)

![V1](Pictures/Q319-1.png)

![V1](Pictures/Q319-2.png)

_**Q282:** Balerion and the dragons it froze - of the three colors of which dragons were frozen the largest number of times_

![V1](Pictures/Q282.png)

_**Q209:** The three most owned entity-types_

![V1](Pictures/Q209.png)

_**Q210:** The three pairs (owner entity-type, owned entity-type) with most _owns_ relationships_

![V1](Pictures/Q210.png)

_**Q369:** The three most common relationship-types and the number of relationships of each of these types_

![V1](Pictures/Q369.png)

_**Q370:** The most common relationship-type between entities of the same type_

![V1](Pictures/Q370.png)

## Extended M3 Aggregator

![V1](Pictures/Agg-M3-Extended.png)

_**Q275:** Any person A and A's horses of the three colors of which A owns the heaviest horse_

![V1](Pictures/Q275.png)

Even if A's three heaviest horses are of the same color, we would still get all A's horses for two more colors (those with the next heaviest horses)

_**Q223:** Any person A and A's horses of the three colors for which the cumulative weight of A's horses is maximal_

![V1](Pictures/Q223.png)

_**Q222:** Any person A and A's horses of the three colors for which A's average ownership start date is the latest_

![V1](Pictures/Q222.png)

_**Q269:** Any person A and A's horses of the three colors for which the average ownership start date is the latest_

![V1](Pictures/Q269.png)

_**Q276:** Any person A and A's horses of the three colors for which the earliest horse ownership start date is the earliest_

![V1](Pictures/Q276.png)

Even if the three horses with the earliest ownership start date are of the same color, we would still get all people and their horses of two more colors (of the next-earliest ownership start dates)

_**Q216:** Any dragon Balerion froze during the three 30-day timeframes during which it froze dragons the largest number of times_

![V1](Pictures/Q216.png)

_**Q373:** The three 30-day timeframes during which dragons were frozen the largest number of times_

![V1](Pictures/Q373.png)

_**Q374:** The three most prolonged timeframes during which no dragon was frozen_

![V1](Pictures/Q374.png)

_**Q264:** Any horse of the three colors of which the average horses' weight is maximal_

![V1](Pictures/Q264.png)

_**Q226:** Any person A and A's horses - of the three horse-colors for which the average height of the horse owners is minimal_

![V1](Pictures/Q226.png)

_**Q342:** The three colors for which the number of pairs of dragons of that color where at least one of them froze the other - is maximal_ (versions 2, 3)

Note that if a pair of dragons mutually froze each other - we need to count this pair only once.

![V1](Pictures/Q342-2.png)

![V1](Pictures/Q342-3.png)

## Extended R1 Aggregator

![V1](Pictures/Agg-R1-Extended.png)

_**Q273:** For each horse color - the three horse-ownerships with the latest ownership start date_

![V1](Pictures/Q273.png)

## Multivalued Functions and Expressions

A _multivalued function_ is a function that may associate zero or more values to each input. In V1, expressions composed of multivalued functions are evaluated separately for each output associated with the input. 

V1 supports the following multivalued functions:

* _el_: _St_ ⇉ 𝑡 - associates a set with each of its elements
* _el_: _Bt_ ⇉ 𝑡 - associates a bag with each of its elements
* _subset_: _St_ ⇉ _St_ - associates a set with each of its nonempty subsets
* _subbag_: _Bt_ ⇉ _Bt_ - associates a bag with each of its nonempty subbags

All these multivalued functions have no associations when the input is an empty set or bag.

In the following examples, _Person_ has the following property: _nicknames_: _set(string)_.

_**Q379:** Any person who has at least one nickname_

![V1](Pictures/Q379.png)

Even though {1} has no explicit constraints, it must have at least one assignment.

_**Q307:** Any person who has a nickname containing an 'a'_

![V1](Pictures/Q307.png)

{1} is a _multivalued expression_. For each person, {1} is evaluated for each nickname. If at least one nickname satisfies the constraint - this person is a valid assignment.

_**Q316:** Any pair of people with a common nickname_

![V1](Pictures/Q316.png)

{1} is evaluated for each non-null nickname of A. {2} is evaluated for each non-null nickname of B. All combinations are evaluated.

_**Q308:** Any person who has a nickname containing both (an 'a' or an 'A') and (a 'b' or a 'B')_ 

![V1](Pictures/Q308.png)

For each person, {1} is evaluated for each non-null nickname. {2} and {3} are evaluated for each value of {1}. If {2} and {3} satisfies the constraints for at least one evaluation of {1} - the person is a valid assignment. A person with no non-null nicknames will not be evaluated.

_**Q309:** Any person who has at least two nicknames_ (two versions)

![V1](Pictures/Q309-1.png)

The _count_ function returns the number of values a multivalued expression has. {1} is a single-valued expression.

An A3 aggregator can be used for aggregating the evaluations of a multivalued expression:
- The aggregator appears below an _All_ quantifier-input
- The per clause is '←'
- There are only entity's expressions right of the quantifier
- There is exactly one multivalued expression right of the quantifier

![V1](Pictures/Q309-2.png)

{2} is assigned with the number of distinct non-null evaluations of {1}.

_**Q310:** Any person who has at least two nicknames - each contains an 'a'_ (two versions)

![V1](Pictures/Q310-1.png)

![V1](Pictures/Q310-2.png)

_**Q311:** Any person who has at least five nicknames - each contains either an 'a' or a 'b'_

![V1](Pictures/Q311.png)

_**Q312:** Any person with more nicknames containing a 'b' or a 'B' than nicknames containing an 'a' or an 'A'_

![V1](Pictures/Q312.png)

Note that if a nickname contains both an 'a' and a 'b' - it would be assigned to both {1} and {4}.

_**Q313:** Any person A that has at least one nickname and all A's nicknames contain an 'a'_

![V1](Pictures/Q313.png)

_**Q314:** Any person who has at least one nickname, but none of its nicknames contain an 'a'_

![V1](Pictures/Q314.png)

_**Q315:** Any person A that the average length of A's three shortest nicknames is at least three_

![V1](Pictures/Q315.png)

_**Q348:** Any person A that the combined weight of three, four or five of A's horses - is exactly 1000 Kg_

![V1](Pictures/Q348.png)

_**Q349:** Any person A that A's horses can be split into three groups with an identical combined weight_

![V1](Pictures/Q349.png)

Relationships may have multivalued expressions as well.

_**Q284:** Any person who owned the same five horses - for at least ten consecutive days_ (version 2)

![V1](Pictures/Q284-2.png)

The _set_ function returns a set of all individual dates within the _tf_ dateframe. _el_ is a single element of this set (a single date). Each value is evaluated separately.

_**Q340:** Any person who owned the same five horses - in at least ten (not necessarily consecutive) days_

![V1](Pictures/Q340.png)

{2} and {5} are sets of all horse ownership dates, aggregated over all 'owns' relationships.

Expression-tag {3} represents one subset of set expression {2}. The constraint in {4} limits assignments to {3} to subsets of {2} with exactly ten elements. {2}, {3} and {4} are properties of A × B.

_**Q287:** Any person who at each day of at least ten (not necessarily consecutive) days - owned the same two horses_

![V1](Pictures/Q287.png)

_**Q327:** Any person who at each day of at least ten consecutive days - owned at least five horses (not necessarily the same horses each day)_

![V1](Pictures/Q327.png)

_**Q318:** Any dragon Balerion froze/fired at during the three 30-day timeframes during which it froze/fired at dragons the largest number of times_

![V1](Pictures/Q318.png)

Note that {1} and {2} are defined right of an 'O', hence evaluated to empty sets when the optional parts have no assignments. Since 𝑆 ∪ ∅ = 𝑆, the pattern is valid even if Balerion froze no dragon or if it fired at no dragon.

{3} is a property of A, but, being an aggregated multivalued expression, must be defined right of the aggregator.

_**Q371:** Any dragon Balerion froze/fired at during the three **non-overlapping** 30-day timeframes during which it froze/fired at dragons the largest number of times_

![V1](Pictures/Q371.png)

{1}, {2} and {3} are sets of intervals of _datetimeframe_. In {5}, {3} is coerced to set of _datetimeframes_. {4} and {5} limit assignment to {3} to sets with three non-overlapping members.

{6} is a multivalued expression evaluated per each value of the multivalued expression {3}. The M2 aggregator is evaluated for all assignments to {6} of each assignment to {3}.

## Application: Spatiotemporality

Dragon-spotting is a major hobby around the world. Dragon-spotters document the time, location, and of course - the identity of the dragon they spotted. The location may be a point (longitude and latitude) or a small area - when they are not sure about the exact coordinates. Most observations are attributed to the spotter, but in many ancient documents - the dragon-spotter is unknown.

We will extend our sample schema with the following relationship-type:

* _spotted_: {(_Person_, _Dragon_), (_Null_, _Dragon_)} - _time_: _datetime_, _loc_: _location_

_spotted_ is a relationship between a dragon-spotter (which may be unknown) and the dragon spotted. The properties of the relationship are the time and the location where the dragon was spotted.

___location___ is an _opaque data type_ (see above) that contains a geographic point (e.g., latitude and longitude) or a geographic shape (a circle, an ellipse, a polygon, etc.)

Functions over _location_ expressions:

* _dist_(_location_, _location_) → _float_   - the distance [Km] between these two locations
  
Constraint operators over _location_ expressions:

* ⊃ 𝑙, ⊇ 𝑙, ⊅ 𝑙, ⊉ 𝑙   - [not] [proper] superspace of 𝑙
* ⊂ 𝑙, ⊆ 𝑙, ⊄ 𝑙, ⊈ 𝑙   - [not] [proper] subspace of 𝑙

Two entity-types are defined as well:

* **Landmark** - loc: _location_
* **City** - loc: _location_

Note that we chose to hold the location of stationary entities (landmark, city) as entity properties, and hold the location of mobile entities (dragon) as relationship properties.

_**G1:** Any dragon observed at Dragonmont Peak_

![V1](Pictures/G001.png)

_**G2:** Any dragon observed within 5 Km from Dragonmont Peak_

![V1](Pictures/G002.png)

_**G3:** Any dragon observed within 5 Km from Dragonmont Peak at least five times during a 7-day period_

![V1](Pictures/G003.png)

_**G4:** Any dragon observed within 5 Km from Dragonmont Peak between 4 AM and 7 AM in at least five days during a 7-day period_

![V1](Pictures/G004.png)

_**G5:** Any dragon observed within 5 Km from Dragonmont Peak in at least five separate weeks_

![V1](Pictures/G005.png)

_**G6:** Any week during which at least five dragons were observed within 5 Km from Dragonmont Peak_

![V1](Pictures/G006.png)

_**G7:** Any dragon with conflicting observations (given that dragons cannot fly faster than 300 Km/h)_

![V1](Pictures/G007.png)

_**G8:** Any Sarnorian B for whom at least three of B's dragons were spotted within 5 Km from Dragonmont Peak - in a timeframe of 24 hours_

![V1](Pictures/G008.png)

_**G9:** Any dragon that stayed within 5 Km from Dragonmont Peak for at least one week (To ensure this - the dragon should have been spotted there at least once per 6 hours and not spotted at any other location throughout that period)_

![V1](Pictures/G009.png)

_**G10:** Any pair of dragons that were spotted within 5 Km from each other at least five observation-pairs (To ensure this - paired observation should differ in up to 5 minutes; observation pairs should differ in at least 24 hours)_

![V1](Pictures/G010.png)

_**G11:** Any pair of dragons A and B, where at least ten times in the last 300 days A was spotted at a distance of less than 1 Km from places where B was spotted between one day and five days before_

![V1](Pictures/G011.png)

_**G12:** Any pair of dragons that were spotted within 5 Km from each other at least 5 observation-pairs during a 30-day period (To ensure this - paired observation should differ in up to 5 minutes; observation pairs should differ in at least 24 hours)_

![V1](Pictures/G012.png)

_**G13:** Any Sarnorian E whose dragons seem to "spy" on Balerion (at least one of E's dragons was spotted at most 1 Km from Balerion throughout a ten-day period. Paired observation should differ in up to 5 minutes; observation pairs should differ in no more than 1 hour)_

![V1](Pictures/G013.png)

_**G14:** Any pair of dragons that were spotted not less than 1000 Km from each other throughout a period of at least one year (To ensure this - paired observation should differ in up to 30 minutes; observation pairs should differ in no more than 24 hours)_

![V1](Pictures/G014.png)

_**G15:** Any landmark where at least five dragons were spotted within 5 Km from it_

![V1](Pictures/G015.png)

_**G16:** Any city in which at least five dragons were spotted throughout a 2-minutes period_

![V1](Pictures/G016.png)

_**G17:** Any landmark where at least five dragons were spotted within 5 Km from it throughout a 7-day period_

![V1](Pictures/G017.png)

_**G18:** Any dragon spotted in at least ten cities_

![V1](Pictures/G018.png)

_**G19:** Any dragon spotted in at least ten cities throughout a ten-day period_

![V1](Pictures/G019.png)

_**G20:** Any dragon that traveled at least 2000 Km in a 24-hours period_

![V1](Pictures/G020.png)

_**G21:** Any dragon-spotter that spotted the same dragon at two locations - at least 1000 Km apart_

![V1](Pictures/G021.png)

<script async src="https://www.googletagmanager.com/gtag/js?id=UA-89133312-1"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'UA-89133312-1');
</script>
