# Change Log

## Version 2.2.1

### ADDITIONS

- RelaxedConstraint: a nested constraint between entities that joins entities if one of the subconstraints is met 

### CHANGES

- Temod storage exceptions have been moved to the same file

### FIXES

- Fixing a bug where an empty IN condition would be transformed to a malformed mysql query
- Fixing bug where unchanged EnumAttributes where considered updated in snapshots

### Authors

- PyAxolotl

## Version 2.2.0

### Additions

- EnumAttribute added
- TimeAttribute added
- Starting this changelog

### FIXES

- Fixing a bug where BytesAttribute wouldn't load from database
- Fixing a bug where StringAttibute wasn't able to generate a random value with the proper length

### Authors

- PyAxolotl