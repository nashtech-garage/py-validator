
## About Py Validator Package

This package brings the simplicity and flexibility of Laravel's validation system to Python. Inspired by Laravel's elegant approach to handling data validation, this library aims to provide developers with a clean, expressive, and robust way to validate data structures in their Python projects. Whether you're working on web applications, APIs, or data processing tasks, this package helps you streamline validation logic, making it easy to define, customize, and manage validation rules. With a familiar, user-friendly API, it allows for efficient error handling and data integrity, ensuring your data is always in the expected format.

## Installation

The validation library is available on PyPI. It can be installed manually using pip.
```
pip install <validation_package_name>
```

## Validation Quickstart
### Creating Validators
You may create a validator instance using the Validator library.

```Python
from validation_package_name import Validator
	# Data need to validate
	data = {
		'title': 'Foo'
		'body': 'Bar'
	}
    # Define the validation rules
    rules = {
        'title': 'required|unique:posts|max:255',
        'body': 'required',
    }

    # Validate the incoming request data
    validator = Validator(data, rules)

    if not validator.passes():
        # Handle validation failure

    # Retrieve the validated input
    validated = validator.validated()

    # Retrieve a portion of the validated input
    validated_only = validator.safe().only(["name", "email"])
    validated_except = validator.safe().except(["name", "email"])
```

The first argument passed to the Validator is the data under validation. The second argument is a dictionary of the validation rules that should be applied to the data.

### Stopping on First Validation Failure
The `stop_on_first_failure` method will inform the validator that it should stop validating all attributes once a single validation failure has occurred:
```Python
	if validator.stop_on_first_failure().fails()
		# Handle validation failure when a single validation failure has occurred
```

### Customizing the Error Messages
If needed, you may provide custom error messages that a validator instance should use instead of the default error messages provided by <validation_package_name>. There are several ways to specify custom messages. First, you may pass the custom messages as the third argument to the Validator constructor:
```Python
messages = {
    'required': 'The :attribute field is required.'
}
validator = Validator(input, rules, messages)
```

In this example, the `:attribute` placeholder will be replaced by the actual name of the field under validation. You may also utilize other placeholders in validation messages. For example:
```Python
messages = {
    'same': 'The :attribute and :other must match.',
    'size': 'The :attribute must be exactly :size.',
    'between': 'The :attribute value :input is not between :min - :max.',
    'in': 'The :attribute must be one of the following types: :values',
}
```

#### Specifying a Custom Message for a Given Attribute
Sometimes you may wish to specify a custom error message only for a specific attribute. You may do so using "dot" notation. Specify the attribute's name first, followed by the rule:
```Python
messages = {
    'email.required' : 'We need to know your email address!'
}
```

#### Specifying Custom Attribute Values
Many of <validation_package_name> built-in error messages include an `:attribute` placeholder that is replaced with the name of the field or attribute under validation. To customize the values used to replace these placeholders for specific fields, you may pass a dictionary of custom attributes as the fourth argument to the Validator constructor:
```Python
from validation_package_name import Validator
customAttributes = {
    'email' : 'email address',
}
validator = Validator(input, rules, messages, customAttributes);
```

### Performing Additional Validation
Sometimes you need to perform additional validation after your initial validation is complete. You can accomplish this using the validator's after method. The after method accepts a closure or an array of callables which will be invoked after validation is complete.
```Python
validator = Validator(/* ... */);

def after_func(validator):
    if self.somethingElseIsInvalid()
        validator.errors().add('field', 'Something is wrong with this field!')

validator.after(after_func(validator));
 
if (validator.fails()) {
    // ...
}
```

As noted, the after method also accepts an array of callables, which is particularly convenient if your "after validation" logic is encapsulated in callable classes, which will receive an Validator instance via their __call__
```Python
from App\Validation import ValidateShippingTime
from App\Validation import ValidateUserStatus

validator.after([ValidateUserStatus(), ValidateShippingTime(), after_func(validator)])
```

## Working With Validated Input
After calling the errors method on a Validator instance, you will receive an <MessageBag> instance, which has a variety of convenient methods for working with error messages. The errors variable that is automatically made available to all views is also an instance of the MessageBag class.

### Retrieving the First Error Message for a Field
To retrieve the first error message for a given field, use the `first` method:
```Python
errors = validator.errors()

print(errors.first('email'))
```

### Retrieving All Error Messages for a Field
If you need to retrieve an array of all the messages for a given field, use the `get` method:
```Python
for message in errors.get('email)':
    // ...
```

If you are validating an array form field, you may retrieve all of the messages for each of the array elements using the `*` character:
```Python
for message in errors.get('attachments.*')
    // ...
```

### Retrieving All Error Messages for All Fields
To retrieve an array of all messages for all fields, use the `all` method:
```Python
for message in errors.all()
    // ...
```

### Determining if Messages Exist for a Field
The `has` method may be used to determine if any error messages exist for a given field:
```Python
if errors.has('email')
    // ...
```

## Working With Error Messages


## Available Validation Rules
- [Accepted](#rule-accepted)
- Accepted If
- Active URL
- After (Date)
- After Or Equal (Date)
- Alpha
- Alpha Dash
- Alpha Numeric
- Array
- Ascii
- Bail
- Before (Date)
- Before Or Equal (Date)
- Between
- Boolean
- Confirmed
- Contains
- Current Password
- Date
- Date Equals
- Date Format
- Decimal
- Declined
- Declined If
- Different
- Digits
- Digits Between
- Dimensions (Image Files)
- Distinct
- Doesnt Start With
- Doesnt End With
- Email
- Ends With
- Enum
- Exclude
- Exclude If
- Exclude Unless
- Exclude With
- Exclude Without
- Exists (Database)
- Extensions
- File
- Filled
- Greater Than
- Greater Than Or Equal
- Hex Color
- Image (File)
- In
- In Array
- Integer
- IP Address
- JSON
- Less Than
- Less Than Or Equal
- List
- Lowercase
- MAC Address
- Max
- Max Digits
- MIME Types
- MIME Type By File Extension
- Min
- Min Digits
- Missing
- Missing If
- Missing Unless
- Missing With
- Missing With All
- Multiple Of
- Not In
- Not Regex
- Nullable
- Numeric
- Present
- Present If
- Present Unless
- Present With
- Present With All
- Prohibited
- Prohibited If
- Prohibited Unless
- Prohibits
- Regular Expression
- Required
- Required If
- Required If Accepted
- Required If Declined
- Required Unless
- Required With
- Required With All
- Required Without
- Required Without All
- Required Array Keys
- Same
- Size
- Sometimes
- Starts With
- String
- Timezone
- Unique (Database)
- Uppercase
- URL
- ULID
- UUID


### accepted
<a name="rule-accepted"></a>
The field under validation must be "yes", "on", 1, "1", true, or "true". This is useful for validating "Terms of Service" acceptance or similar fields.

### accepted_if:anotherfield,value,...
The field under validation must be "yes", "on", 1, "1", true, or "true" if another field under validation is equal to a specified value. This is useful for validating "Terms of Service" acceptance or similar fields.

## Validating Arrays
### Validating Nested Array Input
#### Accessing Nested Array Data
### Error Message Indexes and Positions

## Validating Files

## Validating Passwords

## Conditionally Adding Rules
### Skipping Validation When Fields Have Certain Values
### Validating When Present
### Complex Conditional Validation
### Complex Conditional Array Validation

## Custom Validation Rules
### Using Rule Objects
Provides a variety of helpful validation rules; however, you may wish to specify some of your own. One method of registering custom validation rules is using rule objects.

### Using Closures
If you only need the functionality of a custom rule once throughout your application, you may use a closure instead of a rule object. The closure receives the attribute's name, the attribute's value, and a ``fail`` callback that should be called if validation fails




