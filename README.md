# inputtest
 A simple nodejs express middleware to check every input.

 Github: [Repository](https://github.com/jokerjoker10/Inputtests)

# How to use
To integrate the input tester, create a object containing the rules and include it as parameter. The middleware will automatically read the keys in the req.body and check them against the rules.
```
const rules = {
    id: {
        regex: /[0-9]/
    },
    uid: {
        regex: /[a-z0-9]/
    }
}

app.use(inputtests(rules));
```

Every rule object consists of many individual keys witch all require a regex key this key is the default test.
The key of every object is the same name as the key-name it will be tested against.

For example a request with the following body will be passed as ok:
```
{
    id: 3
}
```
While a request with this body will result in an error:
```
{
    id: "USER_3"
}
```

# Parameter for rules

| key             | optional | default | description                                                                                               |
|------------------|----------|---------|-----------------------------------------------------------------------------------------------------------|
| regex            | false    | none    | Contains the Regex object used as test                                                                    |
| allow_whitespace | true     | true    | Scans the data for whitespaces. If is False and the data includes a whitespace an error will be send back. |
| aliases          | true     | empty   | A list of optional key to perform this test on.                                                           |
| allow_null       | true     | true    | If this is false it return an error if the data is null.                                                  |

Examples: 

A test with just a simple regex.
```
id: {
    regex: /[0-9]/
}
```
The test will test every data with the key 'id'


A test of an id (with letters in it) it will ignore whitespaces.
```
id: {
    regex: /[0-9a-zA-Z]/,
    allow_whitespace: true
}
```


A test of the key 'id' and it will use the same rules on the key 'user_id' and on'product_id'.
```
id: {
    regex: /[0-9]/,
    aliases: ['user_id', 'product_id']
}
```

A test of the key 'id' it will allow null.
``` 
id: {
    regex: /[[0-9]]/,
    allow_null: true
}
```