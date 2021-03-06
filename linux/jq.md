# jq

- [help 1](https://zerokspot.com/weblog/2013/07/18/processing-json-with-jq/)
* [help 2](https://www.howtogeek.com/529219/how-to-parse-json-files-on-the-linux-command-line-with-jq/)

Run `jq` against a file: `jq '.' <filename>`
Run `jq` against a variable: `jq '.' <<< <my-variable>`

---

### Changing property values

**Change the property value for all items in a json collection and pipe the results to a file**

*Sample input-1.json*
```json
[
    {
        "property" : "something"
    },
    {
        "property" : "something else"
    }
]
```

```
jq '.[].property = "false"' input-1.json > output.json
```

**Change the property value for all items in a collection property and pipe the results to a file**

**Sample input-2.json**
```json
{
    "MyArray": 
    [
        {
        "property" : "something"
},
    {
        "property" : "something else"
    }
    ]
}
```

### Select items from an array where the `id` is not null from `file.json`

```azurecli
jq '.[] | select(.id != null)' file.json
```
with quotes:
```
jq '.resources[] | select(.name=="my_api_appreg")'
```
using against a terraform state file:
```
terraform state pull | jq '.resources[] | select(.name=="my_api_appreg") | .instances[].attributes.app_role[] | select(.display_name=="Service Daemon") | .id'
```

### Select items with unique `id`, `description`, `displayName` and `value` as a composite key from `file.json` and return the results as an array

```azurecli
jq '[.[]] | unique_by(.id) | unique_by(.description) | unique_by(.displayName) | unique_by(.value)' file.json
```

### Merge two json array files together

```azurecli
jq -s '.[0] + .[1]' in.json file.json
```

### How to select specific properties

```azurecli
jq '.[] | {description: .description, displayName: .displayName, allowedMemberTypes: .allowedMemberTypes, id: .id}' file.json
```

*collect all results as one array*
```azurecli
jq '[.[] | {description: .description, displayName: .displayName, allowedMemberTypes: .allowedMemberTypes, id: .id}]' file.json
```

*with a filter*
```azurecli
jq '.[] | {description: .description, displayName: .displayName, allowedMemberTypes: .allowedMemberTypes, id: .id} | select(.id == null)' file.json
```

*group into separate arrays where based on display names*
```azurecli
jq '[.[] | {description: .description, displayName: .displayName, allowedMemberTypes: .allowedMemberTypes, id: .id}] | group_by(.displayName)' file.json
```

*get unique values.  precedence of what will be included is based on the results of a group_by.  the first element will be kept*
```azurecli
jq '[.[] | {description: .description, displayName: .displayName, allowedMemberTypes: .allowedMemberTypes, id: .id, value: .value}] | unique_by(.value)' in3.json
```

### Length of an array

```
jq length <file>

jq length <<< $variable
```

## Pipe in a command to `jq`

```
# get health logs from a docker container
docker inspect mycontainer | jq '.[].State.Health' 
```

[[linux.md]]


