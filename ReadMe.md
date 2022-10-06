## Assignment Notes

Learnings:
- With a personal basic/pay-as-you-go subscription, I did not have the Microsoft.Network resource provider registered with my subscription.  I was able to register the resource provider using the Subscription blade.
  - Was not registered for Microsoft.DataLakeStore, and was still able to set up ADL Gen2 storage account.  But I registered the provider just in case.
- ADF sink mapping doesn't like strings of date and currency values in different formats
  - With and without $, 2 or 4 decimal places
  - YYYY/MM/DD and MM/DD/YYYY

Decisions:
- Every execution of top level Pipeline will result in tables being truncated and reloaded.  Not accounting for incremental delta loads.
  - For the AnomalyLog table, the first data flow will truncate it with INSERT, and the following data flows will INSERT and not truncate.
- LegacyContactId is intended to preserve the value from the original donor data.  I would prefer to create a lookup table that associates the original donor ID to an INT and use the INT surrogate key in other tables, but that is not provided as an available field/column in the assignment.  For this assignment, the ID values will be strings.
- There is no provision in the available Virtuous tables and columns for where to store data values that do not match the specification.  However, I do not want to loose that data, at least not yet.  So I am going to create an AnomalyLog tracking table to store values for cell data that can't be inserted into the destination tables or that otherwise meets some exceptional criteria.  I could choose to omit the whole row and store it in the error table.  My guess, though, is that it is more important in this context to retain all well-formed data in the destination, so unless every column in a row is 'bad', something of it will exist in the destination table(s).
- I'm assuming at this point that I am not expected to use a service for finding the valid zipcode for an address.
- I'm allowing duplicate LegacyContactId values because I don't want to confuse efforts to trace lineage of migrated data back to original client data.  However, after loading the final Contacts table, I'm identifying duplicates and inserting data into AnomalyLog so that the individual cases can be evaluated with the client.
- I'm not inserting MockGifts data into the Gifts table if there is no LegacyGiftId in the client data.  Data for these rows will be inserted into AnomalyLog.

ToDos:
- Combine data flows for CSV to SQLDB into one Data Flow object containing all as parallel data flows.
- Add data to AnomalyLog for rows in which there is no value for CompanyName, FirstName, and LastName.
- Evaluate conditional (if) code and use case where it simplifies reading/maintenance.
- Replace loading query for ContactMethods to allow duplicate methods for the same LegacyContactId.
