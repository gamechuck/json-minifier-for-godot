# JSON Minifier for the Godot Engine

This simple asset makes it possible to load json with comments ([jsonc](https://github.com/Microsoft/node-jsonc-parser)) into Godot.
The JSON minifier removes all comments from a jsonc so it can be correctly parsed afterwards by Godot's parse_json() without error.

## Example *.jsonc*-file

```JSONC
{
    // This is my array of animals for my zoo!
    "animals": [
        {
            "tiger": {
                "amount": 2
            },
            "pelican": {
                "amount": 69 // nice
            },
            // Unicorns don't exist!
            /*
            "unicorn": {
                "amount": 1
            }
            */
        }
    ]
}
```

Which, after going though .minify_json, will be stripped of comments as such:

```JSON
{
    "animals": [
        {
            "tiger": {
                "amount": 2
            },
            "pelican": {
                "amount": 69
            },
        }
    ]
}
```

## Example usage:

```Swift
static func load_JSON(path : String) -> Dictionary:
    var file : File = File.new()
	var dictionary := {}
	var error : int = file.open(path, File.READ)
	if error == OK:
		var text : String = file.get_as_text()
        text = JSONMinifier.minify_json(text)
		file.close()
		if typeof(parse_json(text)) != TYPE_NIL:
			dictionary = parse_json(text)
			if dictionary == null:
				push_error("Detected null-value in JSON at {0}.".format([path]))
			else:
				return dictionary

		push_error("Failed to correctly process '{0}', check file format!".format([path]))
		return {}
	else:
		push_error("Failed to open '{0}', check file availability!".format([path]))
		return {}
```

# Credits

Code is heavily inspired by the (stale) python-branch of the JSON.minify repository as found [here](https://github.com/getify/JSON.minify/tree/python)
and has been adapted to Godot from there.

