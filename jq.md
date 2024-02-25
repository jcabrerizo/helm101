# JQ

<https://jqlang.github.io/jq/>

[Playground](https://jqplay.org/)

[Another playground](https://www.devtoolsdaily.com/jq_playground/)

```shell
Usage: jq [options] <jq filter> [file...]
 jq [options] --args <jq filter> [strings...]
 jq [options] --jsonargs <jq filter> [JSON_TEXTS...]
```

**filter** must be escaped in quotes

The simplest filter is `.` that represents the full input

## Flags

* `-c`: compact output
* `-r`: raw output

```shell
EXAMPLE='{"field":{"childField":"value"}}'
jq '.field' <<< ${EXAMPLE}
# {
#  "childField": "value"
# }

jq -c '.field' <<< ${EXAMPLE}   # compact output
# {"childField":"value"}

jq '.field.childField' <<< ${EXAMPLE} 
# "value"

jq -r '.field.childField' <<< "${EXAMPLE}"    # raw output 
#value
```

## Arrays

```shell
API_ENDPOINT="https://api.github.com/repos/jqlang/jq/issues?per_page=5"
EXAMPLE=$(curl -s $API_ENDPOINT)
```

```shell
jq '.[]' <<< "${EXAMPLE}"           # all elements
jq '.[].url' <<< "${EXAMPLE}"       # list with the attribute `url` from the elements 
jq '.[1]' <<< "${EXAMPLE}"          # one element
jq '.[2:]' <<< "${EXAMPLE}"         # from element 2 to the end
jq '.[2:4]' <<< "${EXAMPLE}"        # from element 2 to the 4
```

### Construct JSON arrays

Wrapping the filter in `[` and `]` generates a valid json output

```shell
jq '[.[].url]' <<< "${EXAMPLE}"    # Creates an array with all the `url` values
```

## Object constructors

```shell
jq '[.[] | {number, url}]' <<< "${EXAMPLE}"    # extract the `number` and `url` fields from the original objects and wrap it in an array
```

## Functions

<https://jqlang.github.io/jq/manual/#builtin-operators-and-functions>

### Sort

```shell
jq 'sort' <<< "[1,3,4,2]"
jq 'sort_by(.title)' <<< "${EXAMPLE}"
```

### Count

```shell
jq 'length' <<< "${EXAMPLE}"
```

### Map

Notice 1-2-1 mapped attributes don't need the `.`, but it's required for applying functions

```shell
jq 'map({url, number, titleLength: .title | length})' <<< "${EXAMPLE}" 
```

### Select

Expects a predicate

```shell
jq 'map({url, number, titleLength: .title | length}) | map(select(.titleLength>50))' <<< "${EXAMPLE}" 
```

### Keys

### Contains

### Test

### Group by

## Tricks

Instant schema

```shell
 jq -r 'path(..) | map(tostring) | join("/")' <<< "${EXAMPLE}"  
```
