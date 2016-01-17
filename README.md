# datatables-java
This library contains classes for marshalling AJAX requests from and responses to [DataTables](https://www.datatables.net/), using an object mapper like [Jackson](https://github.com/FasterXML/jackson).

## Usage with Spring MVC
In this example, a *POST* request is used to fetch the table data asynchronously, because then we can send our request as JSON in the request body. As you can see, Spring MVC will unmarshall the request automatically.

##### Controller
```java
@ResponseBody
@RequestMapping(value = "/data", method = RequestMethod.POST)
public DataTablesResponse<SomeClass> find(@RequestBody DataTablesRequest dtRequest) {
    // The argument 'dtRequest' contains search and paging parameters.
        
    // Call the service responsible for retrieving your data here.

    DataTablesResponse<SomeClass> dtResponse = new DataTablesResponse<>();
    dtResponse.setDraw(dtRequest.getDraw());
    
    // Call dtResponse.setData(..), dtResponse.setRecordsTotal(..) and
    // dtResponse.setRecordsFiltered(..) to populate the response with you data.

    return dtResponse;
}
```

##### View
```html
<table id="table">
    <thead>
        <tr>
            <th>Property 1</th>
            <th>Property 2</th>
        </tr>
    </thead>
</table>
<script>
    $('#table').DataTable({
        serverSide: true,
        ajax: {
            url: '/data',
            type: 'POST',
            contentType: 'application/json; charset=utf-8',
            data: function (d) {
                return JSON.stringify(d);
            }
        },
        columns: [
            {
                name: 'property1',
                data: 'property1'
            },
            {
                name: 'property2',
                data: 'property2'
            }
        ],
        order: [[ 0, 'asc' ]],
        lengthMenu: [ 20, 50, 100 ]
    });
</script>
```
