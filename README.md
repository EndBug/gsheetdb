# gsheetdb
Use Google Sheets as a database.

---

## Preparing the spreadsheet
_getting credentials, etc._

---

Let's assume this spreadsheet:

. | A | B | C
--- | --- | --- | ---
1 | **date** | **amount** | **description**
2 | today | 123 | abc

The first row will always be considered as headers.


## Connecting to the sheet
```javascript
import gsheetdb from 'gsheetdb'

let db = new gsheetdb({
  spreadsheetId: 'sheetId', // replace with spreadsheet id
  sheetName: 'sheetName',   // replace with sheet name
  credentialsJSON: {}       // replace with JSON formatted credentials
})
```


## Getting the data
So let's begin to read some data:
```javascript
let data = await db.getData()
```

Data returned will look like
```javascript
[
  {
    values: [ 'today', 123, 'abc' ],
    rowNb: 2
  }
]
```

May be useful to get it in `key: value` format?
```javascript
[
  {
    date: 'today',
    amount: 123,
    description: 'abc',
    rowNb: 2
  }
]
```


## Insert rows
Now if we add rows
```javascript
await db.insertRows([
  ['tomorrow', 456, 'def'],
  ['monday', 23, 'ghi']
])
```

And table will look like this

. | A | B | C
--- | --- | --- | ---
1 | **date** | **amount** | **description**
2 | today | 123 | abc
3 | tomorrow | 456 | def
4 | monday | 23 | ghi


## Update a row
To update a row:
```javascript
await db.updateRow(
  3, ['yesterday', 456, 'def']
)
```

. | A | B | C
--- | --- | --- | ---
1 | **date** | **amount** | **description**
2 | today | 123 | abc
3 | yesterday | 456 | def
4 | monday | 23 | ghi


Would it be useful to be able to update several rows at a time?