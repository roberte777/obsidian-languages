# Python Language Tools and Structure
## Package Management
### Supported solution
For a package manager in Python, Poetry is the supported solution. 
Poetry is a fully featured python package manager, similar to NPM in Javascript, Cargo in Rust, etc. 
It has:
- great package resolution
- makes it trivial to:
	- start new projects with the modern `pyproject.toml`
	- build projects
	- deploy to package repository
- super fast install times
- clear dependency management in `pyproject.toml`
### Why not just use Pip?
Pip and `requirements.txt` have *poor* package resolution.
This means that packages will often install conflicting versions of dependencies
It is also hard to tell what the packages you have installed are, since you see all of your installed packages plus their dependencies in the same file with no indication of which is which.

Additionally, without Poetry, there is some added complexity for building and publishing packages. Poetry does this all with one command

## Formatting
The Black formatter is used for formatting. This formatter is very opinionated and doesn't allow for much configuration. This is because it very closely follows the PEP8 style guides.

Using the Black formatter means that your code will look as it should as stated in PEP8. It should look similar to other Python programmers code, assuming they are also following the style guides. 

Code formatting is important. Here are some reasons you should use one for all development:
1.  **Readability and Consistency**: Code formatters ensure that the code adheres to a consistent style, which makes it easier to read and understand. This is particularly beneficial when multiple developers are working on the same project. With a formatter, you can be sure that everyone is following the same style guidelines.
    
2.  **Efficiency**: Formatters can automatically correct minor stylistic issues, allowing developers to focus on writing functional code rather than spending time on formatting.
    
3.  **Reduced Code Review Burden**: By automating style issues, code formatters can reduce the burden on code reviewers, allowing them to focus on the logic and architecture of the code, rather than nitpicking about style.
    
4.  **Eliminates "Bikeshedding"**: Style can become a contentious issue within teams, often leading to unproductive debates (sometimes referred to as "bikeshedding"). By implementing a formatter with a defined style guide, teams can avoid these disputes altogether.
    
5.  **Integrations and Support**: Most modern code editors and IDEs integrate with popular code formatters, providing immediate feedback and automatic corrections. This saves developers time and effort.
    
6.  **Prevent Bugs**: Certain code formatters can even help to prevent bugs. For example, formatters like "Black" in Python enforce style rules that can help to avoid common pitfalls or mistakes.

## Language Server Protocol
An LSP, or Language Server Protocol, is essential the tool editors use to show you the handy dandy errors that you love so much. They are actually fairly complicated, but explaining them fully is outside the scope of this document.

For development of Python, there are several LSP's. Most of the time, the LSP your editor chooses should be good enough. For example, VSCode maintains and LSP known as Pylance (installed when you install the Python extension), which is a private version of the open source Pyright. For Neovim, I also recommended using this open source version. They are very similar. For anything else, if possible, use Pylance or Pyright. If not possible, whatever your editor provides is probably suffice. 

## Linter
Pylint should be used for linting. This is the most popular and most supported linter for Python at the moment. Pylint will tell you when you break some additional rules that it creates. For example, if you break some style guides, it will throw a warning in your editor. If you break common convention, it will also warn you and tell you how to fix it. 

## Package Repository
Torch does not currently have a package repository. This is not great for a lot of reasons. First off, it exposes developers to installing packages that have malware in them. There are many developers who will misspell something when pip installing and accidentally install a package that is set up to do something harmful. This is an innocent and common mistake. There is not much you can do to protect yourself.

A package repository is a local repository of Python packages that developers can install from. This is only approved and internal packages, meaning everything in your package repo should be safe. Additionally, there are code scans that are ran that verify the code in the package repo should be safe.

A popular enterprise Python package repo is JFrog. 

## Types
Types are ***great***. Many languages that did not have types are now implementing typing solutions. For example, most Javascript developers now use Typescript, a superset of Javascript that implements types. Python has implemented the typing library, which provides type hints to your code.

Your editor can be set up to check these type hints. They will not throw any runtime errors if they are not obeyed, but your editor can tell you and static type checkers can be setup in the CI/CD pipeline to check that they are being obeyed. Type checking should be set to the "strict" setting in editors like VSCode that provide the ability to do so. This makes sure Python code will not fail unexpectedly and all of your code is fully thought out. 

Here is an example:
```python
def foo(input: str) -> int {
	count = 0
	while True:
		print(input)
		count += 1
		if count % 10 == 9:
			break
	return count
}
```
In this example, we have a function that takes a string as input, prints it, and returns how many times it printed. Anyone who uses are function with a modern editor will get intellisense that tells them our function takes a string and returns an int. 

Additionally, when writing this function, if I were to try and assign count to a string, my editor would throw an error because count as initialized as an int. You should not be able to assign a string to an int unless you specify it! 

It would also give an error if I tried to use a function on count that is not supported. For example, if I tried to use the `split` function on count, my editor would tell my `split` does not exist for type int.