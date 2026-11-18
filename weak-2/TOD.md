For these challenges we are using python and We are doing TOD(Test Oriented Development).

In python there is `unittest` module for TOD.
- The `unittest` module provides a framework for writing and running unit tests in Python.
- Use it to create automated tests, verify code correctness, and practice test-driven development.

For using `unittest` 
- we have create class and class name must starts with `Test` 
- and also function name must starts with `test_`
Eg:
```python
import unittest  
  
class TestMath(unittest.TestCase):  
  def test_addition(self):  
    self.assertEqual(2 + 2, 4)  
    print('Addition test passed')
```
