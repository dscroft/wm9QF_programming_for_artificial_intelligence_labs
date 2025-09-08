<!--
module_id: python_unit_testing
author:   David Croft
email:    david.croft@warwick.ac.uk
version: 0.0.1
current_version_description: Initial version
module_type: standard
docs_version: 2.0.0
language: en
narrator: UK English Female
mode: Textbook
title: Python unit testing
comment:  This module introduces the the concepts of unit testing in Python, and how to use the unittest framework to create and run tests.
long_description: This module introduces the the concepts of unit testing in Python, and how to use the unittest framework to create and run tests.
estimated_time_in_minutes: 20

@pre_reqs
Learners should be familiar with basic programming concepts and the Python programming language, including importing modules and using functions. Learners do not need to have access to Python or Jupyter notebooks on their own computers.
@end

@learning_objectives  
- Describe what Python is and why they might want to use it for research
- Identify several ways to write Python code
- Understand the purpose and utility of a Jupyter notebook
- Download Python and Jupyter, and access a Python notebook in Google Colab

@end

good_first_module: false
collection: demystifying
coding_required: false
coding_language: python

@sets_you_up_for
- python_basics_variables_functions
@end

@depends_on_knowledge_available_in
- demystifying_command_line
@end

@style
.flex-container {
    display: flex;
    flex-wrap: wrap; /* Allows the items to wrap as needed */
    align-items: stretch;
    gap: 20px; /* Adds both horizontal and vertical spacing between items */
}

.flex-child { 
    flex: 1;
    margin-right: 20px; /* Adds space between the columns */
}

@media (max-width: 600px) {
    .flex-child {
        flex: 100%; /* Makes the child divs take up the full width on slim devices */
        margin-right: 0; /* Removes the right margin */
    }
}
@end

@version_history 

Previous versions: 

@end

@unittest_fix
```python @Pyodide.exec
import unittest

if 'original_main' not in globals():
  original_main = unittest.main

  def custom_main(*args, **kwargs):
    kwargs['exit'] = False
    return original_main(*args, **kwargs)

  unittest.main = custom_main

"Applied unittest fix"
```
@end

link:  ../assets/styles.css
import: ../module_templates/macros.md
import: ../module_templates/macros_python.md
import: https://raw.githubusercontent.com/LiaTemplates/Pyodide/master/README.md
import: https://github.com/LiaScript/CodeRunner/blob/master/README.md

-->


# Python Unit Testing

- second
- third


@overview


## Unit Testing

## Unit Testing in python

@unittest_fix

```python
import unittest

class Tests(unittest.TestCase):
	def test_bigger(self):
		self.assertTrue( 1 < 0 )

	def test_equals(self):
		self.assertEqual( 1+1, 2 )

	def test_div(self):
		with self.assertRaises(ZeroDivisionError):
			1 / 0

if __name__ == '__main__':
    unittest.main()
```
@Pyodide.eval



## Additional Resources

* [python.org](https://www.python.org/) is a great resource for documentation, FAQs, and tutorials for beginners, as well as information about what is happening in the wider Python community. Check it out and explore!

* If you're interested in practicing more with Google Colab, check out [this notebook looking at statistics](https://colab.research.google.com/drive/1zkW5Y0SoV3gMU6sQtlgnZsfR2GIXi6F_?usp=sharing).

* If you are ready to actually write some Python code, check out the [Python Basics: Functions, Methods, and Variables](https://liascript.github.io/course/?https://raw.githubusercontent.com/arcus/education_modules/main/python_basics_variables_functions_methods/python_basics_variables_functions_methods.md#1) module.
