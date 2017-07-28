## Definitions
- Entity Type: a type in the system. ex: Person, Page, Comment
- Entity: an instance of a type. ex: Thomas
- Relationship: a link between one or more entities to one or more entities
- Entity Summary: a textual description of an entity. ex: `Jefferson, Thomas (jtom@gmail.com)`
- Data Type: a type of data an entity may have. ex: String, Decimal, Boolean, Address, Date, Phone
- Input Type: a type to use to input a field value. ex: Textbox, Date Picker, Slider, Checkbox, Dropdown
- Field Type: a field on an entity which has data and input types. ex: String Dropdown
- Filter Type: a type of element to use to search through one or more fields. ex: Autocomplete, Multiselect, Checkbox
- Search: a collection of filters which can look through one or more entity types.
- Query: an instance of a search that can be used to filter through all entities or a subset (relationship)
- View: a set of properties, subproperties, and relationships from an entity to use to display. ex: Search Result
- Widget: takes a view and one or more entities and renders them
- Page: links a url pattern with a set of widgets

## Design
```
entity { id, entity_type_id }
entity_type { id, name, machine_name }
entity_type_summary { id, entity_type, name, template }   
entity_summary { entity_id, entity_type_summary_id, summary }
field_type { id, name, data_type, data_type_properties, input_type, input_type_properties }
entity_type_field { entity_type_id, field_type_id }
```

## Explanations
- A data type can have any number of sub properties which take a value and generate a new one.
- A summary takes entity fields and generates a string that can be searched and listed in controls like dropdowns. They can look like `{last_name}, {first_name} ({email})`.
- The system will enable custom data types to be added.
- The system will enable custom input types to be added - they define which data types they can be used for.
- The system will enable custom filter types to be added - they define a set of data types they can be used for. Filters can provide an index option which can instruct the underyling data storage how to index a field.

## Data Types
### String 
**Formats**
- uppercase
- lowercase
- trimmed
- first
- last
- extension
### Decimal
**Formats**
- whole
- fraction (significant digit)
- percent
- currency
### Date
**Formats**
- time_ago 
- months_ago
- years_ago
- *input format*
- seconds_epoch
- milliseconds_epoch
### Phone
**Formats**
- areacode
- (###) ###-####
- ##########
- #-###-###-####
### Address
**Formats**
- zipcode
- door_number
- street
- apt_number
- city
- state
- country
- latlng

```
dataType = {
  name: 'Date',
  machine_name: 'date',
  formats: {
    time_ago: {
      subformats: { ... }
      parse: function(value, inputs) {}
    },
    years_ago: {
      parse: function(value, inputs) {}
    },
    custom: {
      inputs: {
        pattern: { default, required, help }
      },
      parse: function(value, inputs) {}
    }
  }
}
```

```
inputType = {
  name: 'Date Picker',
  machine_name: 'date_picker',
  types: ['date', 'text'],
  properties: {
    min: {
      default: null,
      help: 'The minimum date, absolute or relative, that can be selected',
      required: true
    },
    max: { default, help, required }
  },
  generate: function(type, properties, value, format) {}
}
```

```
filterType = {
  name: 'Date Range',
  machine_name: 'date_range',
  fields: {
    
  }
}
```
