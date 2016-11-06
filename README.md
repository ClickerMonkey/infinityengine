# Infinity Engine

### Concepts

- Design: A collection of entitiy types. This allows you to "redesign" your database (have versions) and define migration strategies.
- Entity Type: A collection of component types and relationship types which define an object in a design.
- Component Type: An attribute on an entity (name, type).
- Relationship Type: Defines how entities are related (1-1, 1-n, n-1, n-m) and can have relationship data (via Entity Type).
- Entity: An instance of an entity type.
- Component: A value on an entity.
- Relationship: A relationship between two entity and a reference to an entity type with relationship data.
- View: A subset of components on an entity and joins with related entities (you can define default values if the join doesn't work, and for ?-n relationships you define aggregate functions)
- Search: Defines a set of expressions that restrict results returned - either through comparing components to constants or variables which can be user supplied.


### Design

```
design { id, name, parent_id, version }
entity_type { id, display_name, variable_name }
component_type { id, entity_type_id, display_name, variable_name, type, type_meta, required, min, max, meta )
relationship_type { id, parent_entity_type_id, child_entity_type_id, relation_entity_type_id, type, required }
entity { id, entity_type_id }
component { id, entity_id, component_type_id, value }
relationship { parent_entity_id, child_entity_id, relation_entity_id, relationship_type_id }
list { id, name, value_type }
list_value { id, list_id, name, value }
view { id, name }
view_component { id, view_id, source_id, source_type_id, component_type_id, aggregate }
search { id, name, view_id }
search_where ( id, search_id, view_component_id, comparison_type, comparision_values }

component_type.type
list.value_type
  string
  integer
  decimal
  enum (type_meta -> list_id)
  array (type_meta -> component_type.type)

relationship_type.type
  1-1 (hasOne)
  1-n (hasMany)
  n-1 (belongsTo)
  n-m (hasManyThrough)

```
