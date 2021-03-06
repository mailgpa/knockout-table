### **TABLE** binding for [Knockout](http://knockoutjs.com/)

The `table` binding provides a fast method for displaying tables of data using Knockout. `table` is about ten times faster than nested `foreach` bindings.

#### Examples

This example uses a `header` array (which determines the number of columns in the table) and a `rows` two-dimentional array.

```html
<table data-bind="table: { header: header, rows: rows }"></table>
```

```javascript
var vm = {
    header: [ 'x', 'y', 'z' ],
    rows: [
        [ 1, 2, 3 ],
        [ 4, 5, 6 ],
        [ 7, 8, 9 ]
    ]
};
```

This example uses a `columns` definition array and `rows` array of data objects.

```html
<table data-bind="table: { header: columns, columns: columns, rows: rows }"></table>
```

```javascript
var vm = {
    columns: [ 'x', 'y', 'z' ],
    rows: [
        { x: 1, y: 2, y: 3 },
        { x: 4, y: 5, y: 6 },
        { x: 7, y: 8, y: 9 }
    ]
};
```

This example uses header and data options to define the values for those items.

```html
<table data-bind="table: { columns: columns, rows: rows, header: 'heading', data: 'datavalue' }"></table>
```

```javascript
var vm = {
    columns: [
        { heading: 'x', datavalue: 'col1' },
        { heading: 'y', datavalue: 'col2' },
        { heading: 'x', datavalue: 'col3' } ]
    rows: [
        { col1: 1, col2: 2, col3: 3 },
        { col1: 4, col2: 5, col3: 6 },
        { col1: 7, col2: 8, col3: 9 }
    ]
};
```

This example uses header and data functions to define the values (uses same view model as above).

```html
<table data-bind="table: { columns: columns,
                           rows: rows,
                           header: function(col) { return col.heading },
                           data: function(row, col) { return row[col.datavalue] } }">
</table>
```

#### Parameters

The `table` binding expects an object with the following properties:

* `rows` - an array of either objects or arrays. (required)
* `columns` - either the number of columns or an array, with each item representing a column. In the former case, `rows` is assumed to be a two-dimensional array (array of arrays). In the latter case, `rows` is assumed to be an array of objects, with the values in `columns` being the keys in each object. `columns` is usually required, but is optional if a `header` array is included. `columns` can also be an array of objects, in which case the `data` option must also be specified.
* `header` - either an array of header values, a function that returns the header for each column value, or a string used to read the header from the column object. (optional)
* `data` - either a function that returns the data value for a given row and column, or a string used to read the data key from the column object (which is then used to read from the row object). (optional)
* `evenClass` - the name of a class that will be applied to even rows in the table, starting with the second (2) row. (optional)

Any of the above parameters can be an observable and will cause the table to be regenerated if updated. For the purpose of making the binding faster, the entries in `rows`, `columns`, or `headers` cannot be observables. The actual data items, though, can be observable, and if updated, will only update the value of the corresponding table cell.

#### How this binding works

The `table` binding uses a very fast method of table generation. 1) It first generates the table HTML as a string. 2) It then parses the string into DOM elements using `innerHTML` on a detached node. 3) Finally, it inserts the table contents into the table element in the document.

#### Additional interfaces

* `ko.utils.escape(string)` - returns a string with HTML special characters (&, <, >, ", etc.) converted to entities. This is used internally by the `table` binding to ensure that data values are treated as text when assembled into the table HTML.

#### License and Contact

**License:** MIT (http://www.opensource.org/licenses/mit-license.php)

Michael Best<br>
https://github.com/mbest<br>
mbest@dasya.com
