---
Title: JS ES6 Destructuring notation
---

# [[javascript]] ES6 Destructuring notation

### Basic usage
`const { origin, destination } = req.query;`

### RL example
```
app.get('/', function(req, res){
   const {id, since, fields, anotherField} = req.query;
});
```

### Destructuring works with defaults too!
```
const req = {    //destructuring with provided default values!
  query: {
    id: '123',
    fields: ['a', 'b', 'c']
  }
}

const {
  id,
  since = new Date().toString(),
  fields = ['x'],
  anotherField = 'default'
} = req.query;
```
