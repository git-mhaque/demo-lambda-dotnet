# Lambda 

## Lambda code

``` JS
exports.handler = async (event) => {
    let {operand1, operand2} = event.input;
    let result;
    
    console.log(event)
    
    switch (event.operator) {
        case 'add':
            result = operand1 + operand2;
            break;

        case 'subtract':
            result = operand1 - operand2;
            break;

        case 'multiply':
            result = operand1 * operand2;
            break;

        case 'divide':
            result = operand1 / operand2;
            break;

        default:
            result = null;
    }
    
    const response = {
        statusCode: 200,
        body: JSON.stringify(result),
    };
    return response;
};

```

## Lambda input

``` json 
{
  "operator": "add",
  "input": {
    "operand1": 5,
    "operand2": 3
  }
}
```

## Lambda output

``` json 
{
  "statusCode": 200,
  "body": "8"
}
```

# API Gateway 

Notes: 
- REST API 


## API request

Request URI
```
 POST /math/{operation}
```

Request body

``` Json
{
    "num1": 5,
    "num2": 3
}
```


## Request mapping translation 

Under `Integration Request` 
- Make sure `Use Lambda Proxy integration` is unchecked
- Under `Mapping Templates`
    - Select `When there are no templates defined (recommended)`
    - Content type `application/json`
    - Then enter the following translation

``` Json
{
  "operator": "$input.params('operation')",
  "input": {
    "operand1": $input.json('$.num1'),
    "operand2": $input.json('$.num2')
  }
}
```

## Response mapping translation 

Under `Integration Response` 
- Under `Mapping Templates`
    - Content type `application/json`
    - Then enter the following translation

``` Json
{
  "result": $input.json('$.body')
}
```

## API response


``` Json
{
    "result": "15"
}
```

# Testing the API

``` bash
curl -v \
'https://wrys86ody0.execute-api.us-east-1.amazonaws.com/test/math/add' \
-H 'Content-Type: application/json;' \
-d '{"num1": 5, "num2": 3}'
```

```Json
{
  "result": "8"
}

```