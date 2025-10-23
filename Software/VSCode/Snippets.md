Project specific snippets

- Create a file named {name}.code-snippets under .vscode folder
- add the snippets

Global snippets
- File -> Preferences -> Configure Snippets

e.g.
```json
{
	"Lockin Sweep Config": {
		"prefix": "sweep",
		"body": [
			"{",
			"  \"sweep\": {",
			"    \"start\": ${1:0},",
			"    \"stop\": ${2:1},",
			"    \"points\": ${3:100},",
			"    \"delay\": ${4:0.1},",
			"    \"channel\": ${5:1}",
			"  }",
			"}"
		],
		"description": "Print to console"
	}
}

```

Info: https://code.visualstudio.com/docs/editor/userdefinedsnippets