# Code Style

Follow the [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html).

## Imports

Import packages or modules, not individual names from builtins or third-party packages.
Project-internal classes and functions may be imported directly.

```python
# Good
import collections
import asyncio
import contextlib
import concurrent.futures
import functools
import logging
import re

import httpx
import pydantic
import bidict

from sanitizer.codec import EntityTypeCodec          # own code — ok
from sanitizer.pipeline.base import DetectedEntity   # own code — ok

# Bad
from collections import defaultdict
from asyncio import Semaphore
from concurrent.futures import ThreadPoolExecutor
from pydantic import BaseModel, Field
from pydantic_settings import BaseSettings
from httpx import AsyncClient, Timeout
```

All imports must be at the top of the module. The only exception is optional/heavy dependencies that should not be imported at module load time (e.g. `transformers`, `lingua`) — those may be imported inside the function that uses them, with a comment explaining why.

Exceptions: `from __future__ import annotations` and `from typing import ...` are fine (type-annotation helpers with no meaningful module to import).

## Blank lines

Add a blank line after the end of an indented block (`for`, `while`, `if`, `with`, `try`) when the next statement is at the outer indentation level.

```python
# Good
for item in items:
    process(item)

return result

# Bad
for item in items:
    process(item)
return result
```

## Naming

Avoid single-letter or two-letter variable names except in the simplest comprehension patterns (e.g. `[x * 2 for x in values]`). Use descriptive names as soon as the comprehension body is non-trivial or references multiple attributes.

```python
# Good
[EntitySummary(text=entity.text, ...) for entity in entities]

# Bad
[EntitySummary(text=e.text, ...) for e in entities]
```

## String formatting

Prefer f-strings over `%`-style or `.format()` formatting, including in logging calls.

```python
# Good
logger.warning(f"Sample failed: {result}")

# Bad
logger.warning("Sample failed: %s", result)
```

## Docstrings

Use 4-space indentation for continuation lines inside docstrings and multiline strings.

```python
# Good
def foo(self) -> str:
    """Short summary.

    Longer description with details:
        item one
        item two
    """

# Bad
def foo(self) -> str:
    """Short summary.
    Pattern: something  ← flush with summary, not 4-indented continuation
    """
```

## FastAPI

Do not specify `response_model` in route decorators when it duplicates the return type annotation — FastAPI infers it automatically. Only use `response_model` when you need to override the serialized type (e.g. returning a subclass but serializing as the base).

## Pydantic fields

Do not use the `...` (Ellipsis) sentinel in `pydantic.Field()`. Fields without a default are required by default; just use keyword arguments.

```python
# Good
class Foo(pydantic.BaseModel):
    name: str = pydantic.Field(description="The name")

# Bad
class Foo(pydantic.BaseModel):
    name: str = pydantic.Field(..., description="The name")
```

## Multiline strings

For multiline strings (e.g. `description=` in `pydantic.Field`), use `inspect.cleandoc("""...""")` rather than parenthesized string concatenation.
