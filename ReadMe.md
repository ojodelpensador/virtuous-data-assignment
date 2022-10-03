## Assignment Notes

Decisions:
- LegacyContactId is intended to preserve the value from the original donor data.  I would prefer to create a lookup table that associates the original donor ID to an INT and use the INT surrogate key in other tables, but that is not provided as an available field/column in the assignment.  For this assignment, the ID values will be strings.
- There is no provision in the available Virtuous tables and columns for where to store data values that do not match the specification.  However, I do not want to loose that data, at least not yet.  So I am going to create an Errors tracking table to store values for cell data that can't be inserted into the destination tables.  I could choose to omit the whole row and store it in the error table.  My guess, though, is that it is more important in this context to retain all well-formed data in the destination, so unless every column in a row is 'bad', something of it will exist in the destination table(s).
- I'm assuming at this point that I am not expected to use a service for finding the valid zipcode for an address.
