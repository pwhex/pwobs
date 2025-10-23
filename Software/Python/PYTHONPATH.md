PYTHONPATH is an environmental variable (on the system level) that has the information about which directories to look at when python initiates.

We can check this inside an enviroment as follows:
```python
import sys
print(sys.path)
```

This becomes problematic in VS code if you're working with projects sometimes.
Sometimes you need to manully add your current working folder to the path as follows:

## Method 1:
```python
import os 
import sys
sys.path.append

```

## Method 2:
In VS code under .vscode/settings.json,
we can customize our local environment as follows:
```json
{
    "terminal.integrated.env.linux": {
        "PYTHONPATH": "${workspaceFolder}"
    },
    "terminal.integrated.env.windows": {
        "PYTHONPATH": "${workspaceFolder}"
    },
    "terminal.integrated.env.osx": {
        "PYTHONPATH": "${workspaceFolder}"
    },
    "workbench.colorCustomizations": {
        "titleBar.activeBackground": "#38033a",  // Your desired background color
        "titleBar.activeForeground": "#FFFFFF"   // Your desired text color
    }
}
```

## Method 3:

[[Packaging]]