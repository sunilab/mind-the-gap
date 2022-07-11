mind-the-gap is a tool for on-boarding custom excel workbooks onto the platform.

## TODO
- Establish a known list of worksheet names, do we depend on the worksheet name or do we understand the data in the worksheet? Initial thought is we need to understand the data in the worksheet.
- Establish a known list of entities we need for performing a calc  

## Learn
- Commission rate hierarchy
    - Sales rep overrides
        - Title overrides
            - standard commission rate (if provided)
            
- commission rate table
    - understand excel built in functions like `LOOKUP`
    - build commission rate

- Makeup of a worksheet - is it a data worksheet? is it metadata worksheet?
    - Logic for determining a data worksheet
        - Look for date, revenue/sales data, name, title, commission rate, amount and payout. 
        - If this data is present it is a data worksheet

## Template input
- Sales rep
- Title
- Commission rate
    - This column may contain references to other cells like the commission rate table

## Regular input
- Date
- Revenue
- Sales rep/name
- Title
- Deductions

## What we have to calc
- Rolling sales -> what should we call this field?
- Commission amount
- Payout

## Questions
- Will the commission rate table always be provided when users upload their data?
- How will customers load their data regularly into our calc engine? Upload, APIs?
- What data will they load for regular processing? Which columns?
- Do we need a unique ID for sales rep as a required entity? Could have duplicate sales rep names
- Can deductions be specified in the revenue column as a negative value?
- Will commission rate always be a percentage value even if not specified?
- What kind of lookups can be specified in the sheet other than commission rate?
- Should we try to build a sales hierarchy from the presented data to be used during regular uploads?

## Logic
- For each worksheet in the workbook
- Sample values from each column to determine:
    - Figure out data type of the column values - date, string, number, currency
    - Classify each column as either of:
        - required for calc
        - output of calc
        - carry over i.e. carry the column values over to results as is
    - Bias for first cell in column to be name of the entity
    - Check if it matches any of known entity names from our dictionary
    - Present data type, classificatio, entity names to user for validation.
- Static validation
    - see if all required entities are available for calc
- Required for calc
    - Date - specifies date of transaction
    - Revenue or amount - sale amount of transaction
    - Sales rep - sales agent
    - Commission rate
        - This rate could be a constant or derived from a formula, lookup etc.
- Optional for calc
    - Deductions - any amount that needs to be subtracted from payout
- Calc output
    - rolling sales amount
    - Commission amount
    - Payout
- If lookup formula is used
    - Bias towards the lookup values being metadata used for calc
    - Metadata like:
        - commission rate
        - title
        - product name (carry over)
        - customer name (carry over)
    - Store required metadata
- Run sample calc
- Compare calc output against known output in worksheet