In order for the handler to catch only the necessary messages / other events,
filters are needed, there are filters in the framework right out of the box.

To access rules out of the box, you can do different things:

1. Import them from waio.rules and use them,
initializing them directly in the decorator
or in any other part of the code:
```python
--8<-- "docs/assets/code/rules/unnamed_rule.py"
```

2. Use the pre-built automatic rule unpackers that are 
specified as an argument to the decorator.
but in this case, some secondary arguments for the rule cannot be passed, if the rule contains more than one argument, for example:
[TextRule](https://github.com/dotX12/waio/blob/07d69354a5658c0d7cd9a32f21093b758c5d0bd8/waio/rules/default.py#L97),
[MessageCommandsRule](https://github.com/dotX12/waio/blob/07d69354a5658c0d7cd9a32f21093b758c5d0bd8/waio/rules/default.py#L9)
```python
--8<-- "docs/assets/code/rules/named_rule.py"
```

**You can set any number of named rules as well as unnamed ones.**
```python
--8<-- "docs/assets/code/rules/nammed_unnamed.py"
```
###  Default named rules:

```python
--8<-- "docs/assets/code/rules/default_rules.py"
```

## Creating your own filter rules

> A rule is a class corresponding to the ABCRule interface, 
which must implement only one asynchronous check method that accepts an event 
and returns False if the check was not passed and True or a dictionary with 
arguments that will be unpacked into the handler as non-positional arguments.

### Unnamed filter rule
To create rules, we import the abstract ABCRule interface 
and implement the asynchronous `check` method, it is also worth 
importing `Union` from `typing` to type your code.

```python
--8<-- "docs/assets/code/rules/example_rule_001.py"
```

Our rule does not accept any arguments, 
it only checks if the message length is more than two hundred characters, 
it will return True, the filter will be triggered and the handler will be called, 
otherwise False will be returned and the filter will not be processed.

Now we can use our rule.

```python
--8<-- "docs/assets/code/rules/use_example_rule_001.py"
```

We have got a static rule that does not accept any arguments, 
the logic has a hard binding to a length of 200 characters, 
let's modernize our rule by adding the ability to 
specify the length ourselves in `__init__`.

```python
--8<-- "docs/assets/code/rules/example_rule_002.py"
```

Then we use it in the handler

```python
--8<-- "docs/assets/code/rules/use_example_rule_002.py"
```

Now we can set an identifier for our rule (the name of the rule) 
and then pass to the decorator not an object of the rule, but a 
named argument of the name of the rule.

### Named filter rule
**Please note that you need to declare the rules Now we can set an identifier for our rule (the name of the rule) and then pass to the decorator not an object of the rule, but a named argument of the name of the rule.before the handlers!**

```python
--8<-- "docs/assets/code/rules/register_rule.py"
```

Usage:

```python
--8<-- "docs/assets/code/rules/use_example_rule_003.py"
```

Full code:

```python
--8<-- "docs/assets/code/rules/full_code_rule_003.py"
```