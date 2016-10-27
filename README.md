# angular2-hd-table
This module help to build quickly a table come with many features like: filtering, sorting, pagination,... in client or server side. Full of extendable, you can custom everything you want in a beautiful way.
### This library currently under active development.
---
## Dependencies:
- Angular2 (v2.0.0)
- [ng-bootstrap/ng-bootstrap](https://github.com/ng-bootstrap/ng-bootstrap) (Tested on v1.1.5)

## Installation:
```shell 
npm install hd-table --save
```
## Configuration
import ``HdTableModule`` to the top-level module in your application:
```js
import {HdTableModule} from 'hd-table';

@NgModule({
    declarations: [AppComponent, ...],
    imports: [HdTableModule, ...],  
    bootstrap: [AppComponent]
})
export class AppModule {
}
```
Or you can import to others module:
```js
import {HdTableModule} from 'hd-table';

@NgModule({
    declarations: [...],
    imports: [HdTableModule, ...],  
})
export class YourModule {
}
```
## How to uses:
Just push the selector ``hd-table`` to the template that your want to display the table
```js
...
import { Table, HdTableService } from 'hd-table'
import { NgbPaginationConfig } from '@ng-bootstrap/ng-bootstrap';

@Component({
    selector: 'your-selector',
    template: `
        <hd-table [table]="table"></hd-table>
    `
    providers: [NgbPaginationConfig, HdTableService,...]
})
export class YourComponent()
{
    public table: Table = {
		class_name: "table-striped table-hover hd-table",
		identify: 'id',
		columns: [
		    { title: "Word", name: "word", name_display: "word_display", placeholder: "Filter by word" },
			{ title: "Status", name: "learn_status", name_display: "learn_status_display", placeholder: "Filter by status" },
			{ title: 'Practice', name: 'consolidate', name_display: "consolidate_display", class_name: "office-header text-success" },
			{ title: 'Date learn', name: 'created_at', name_display: "created_at_display" },
			{ title: 'Date update', name: 'updated_at', name_display: "updated_at_display" },
		]
		pipe: false,
		data: DataTable, // The data you have to resolve.
		not_have_record_msg: "Not have any record",
	};
	
	constructor(ngbPaginationConfig: NgbPaginationConfig){
	    ngbPaginationConfig.size = 'sm';
		ngbPaginationConfig.boundaryLinks = true;
		ngbPaginationConfig.maxSize = 8;
		ngbPaginationConfig.pageSize = 25;
	}
	...
}
```
If you want more features like checkboxes, select, datetime filter,... you can uses ``hd-table2`` instead.

If you really want adding more features on table, you could consider create a new component and extend ``HdTable`` component, copy all content in ``HdTable`` component template to your component template and adding everything your want, the selector now is YOURS!!!.
### Sever side filter, sort, paging:
The ``table`` variable is a type of ``Table``, it has a property named ``pipe``, about we set it ``false`` because we want the table handle events in browser with data already loaded. But if you want handle all events on server, you have to set ``pipe`` is ``true``. And ``HdTableModule`` also has a service for you to play with server, it's name: ``HdTableService``. Fistoff, you have to subscribe to the service, then whenever you trigger an event like sorting, filter or paging, the ``HdTable`` announce an event to service, You load data from your server and give it back to service, then ``HdTable`` will handle the data. This is call [bi-directional service](https://angular.io/docs/ts/latest/cookbook/component-communication.html#!#bidirectional-service): 

```js
    // YourComponent.ts
    constructor(hdTableService: HdTableService,...){
        ...
        hdTableService.requestDataAnnounced$.subscribe(query => {
			YourDataResolver.yourDataGetter(query).subscribe(data => {
				hdTableService.announceReturnData(data);
			});
		});
    }
```
## TODO:
- sorting: 
    + Client side
	+ Server side
- pagination: 
	+ Client side
	+ Server side
- Flitering:
	+ Client side
	+ Server side
- Check box:
	+ Check all
	+ Check by range (Holding shift)
- Dynamic column:
	+ Each column include: name, label, 
