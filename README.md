# Distinct Until Changed Immutable

Returns an Observable that emits all items emitted by the source Observable that are deeply distinct by comparison from the previous item.
 
 If a comparator function is provided, then it will be called for each item to test for whether or not that value should be emitted.
 
 If a comparator function is not provided, a deep equality check is used by default.
 
 ## Example
 A simple example with numbers
 ```javascript
 import { of } from 'rxjs';
 import { distinctUntilChangedImmutable } from 'distinct-until-changed-immutable';
 
 of({a: 'a'}, {b: 'b'}, {b: 'b'}).pipe(
     distinctUntilChangedImmutable(),
   )
   .subscribe(x => console.log(x)); // {a: 'a'}, {b: 'b'}
 ```
 
 An example using a compare function
 ```typescript
 import { of } from 'rxjs';
 import { distinctUntilChangedImmutable } from 'rxjs/operators';
 
 interface Person {
    age: number,
    name: string
 }
 
 of<Person>(
     { age: 4, name: 'Foo'},
     { age: 7, name: 'Bar'},
     { age: 4, name: 'Foo'},
     { age: 6, name: 'Foo'},
   ).pipe(
     distinctUntilChangedImmutable((p: Person /*I have been cloned*/, q: Person) => p === q),
   )
   .subscribe(x => console.log(x));
 
 // displays:
 // { age: 4, name: 'Foo' }
 // { age: 7, name: 'Bar' }
 // { age: 4, name: 'Foo' } // Emitted because the parameters are immutable and we are doing a simple === check in the comparator function above
 // { age: 5, name: 'Foo' }
