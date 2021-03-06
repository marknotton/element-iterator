
# Iterator

Iterate through a group of elements over a set period of time. With callbacks for each item both in and out.

### Installation

```
npm i element-iterator --save
```

### Options
| Setting | Default | Type | Description |
| -- | -- | -- | -- |
| duration  | 3000  | number | Time (in milliseconds) between each iteration callback |
| instant   | true  | bool | Trigger initial callback instantly with no delay |
| delay     | 0     | number | End function delay. |
| loop      | 3     | number or bool | Iterations loop infinitely or by a set amount |
| autostop  | true  | bool | Adds a listener to pause iterations when user is not actively looking at page |
| startfrom | 1     | number | Choose to start the iteration from a particular index |
| endon     | 1     | number | Loop rules are ignored until a particular index is found after the final loop. |
| autoplay  | true  | bool | Start playing immediately |
| log       | true   | bool | Dev option to console log out status changes. |

### Example

The following examples assume you are using jQuery. However, the core class can be used independently from jQuery.  Lets assume you have some elements you want to iterate through:

```js
let items = $('nav ul li');
```
Whilst not compulsory, it's recommended that you assign your iterator to a variable to allow access to more controls.
```js
let myItems = items.iterate(...)
```
If you don't want to use jQuery, you can declare a new iterator with something like this:
```js
let myItems = new Iterator(items, {...});
```

You can define global settings like this:
```js
Iterator.defaults = {
  duration  : 2000,
  delay     : 500,
  loop      : true,
  startfrom : 2,
  endon     : 2,
  autoplay  : false
}
```
Or pass options in on a per-use basis. Either anonymously or via an object

```js
let myItems = items.iterate({
  start : (element, index) => {
    $(element).addClass('show');
  },
  end : (element, index) => {
    $(element).removeClass('show');
  },
  complete : ( element ) => {
	console.log('All done', element);
  },
  delay : 1000,
  duration : 2000,
  loop : 2
})
```
Or...
```js
let myItems = items.iterate(2000, 1000, 2, (element, index) => {
  $(element).addClass('show');
}, (element, index) => {
  $(element).removeClass('show');
})
```

The ```start``` and ```end``` callbacks both pass in the currently iterated element, and the item index number.  You **must** declare at least one function to be triggered on each iteration. The ```end``` function is optional.

You can also define a third callback called ```complete``` which will trigger when the final loop has completed. This will pass in the last iterated item.

### Controls

When you store your iterator into a variable, you can refer to this for more control over what the iterator is does.

**pause**: Pauses the current iteration where it stands
```
myItems.pause();
```

**stop**: Stops the current iteration and resets the 'start from' position
```
myItems.stop();
```

**play**: Plays the iterator from where it's left of. This is triggered automatically unless otherwise specified
```
myItems.play();
```

**next**: Calls the next iteration action
```
myItems.next();
```

**prev**: Calls the previous iteration action
```
myItems.prev();
```
