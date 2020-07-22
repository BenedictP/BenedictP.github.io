---
layout: post
title: "Android: Validate user inputs from Widgets with Saripaar"
date: 2015-07-26
comments: true
---
Validating User Input in Android can be really ugly. More than ever when every EditText has its own validation rule. Also, if we want to save validated input after every text change.

For this problem I can highly recommend the library from [ragunathjawahar](https://github.com/ragunathjawahar): [**"android-saripaar"** (Github Repo)](https://github.com/ragunathjawahar/android-saripaar).

With this library it is possible to add **rules as annotations** to Stock Android Widgets (e.g. EditText or Checkbox). These rules can validate synchronous or asynchronous!

Right now the v2 of this library is under heavy development so the developer suggests to use the **v1 of Saripaar**.

Add the Libary to the Project
---
So we just add the v1 library in our **gradle script**:

```
dependencies {
	compile 'com.mobsandgeeks:android-saripaar:1.0.3'
}
```
After syncing the Project with the Gradle Files we can start the annotation of Android Widgets we want to validate.

Order and error messages
---
With **@Required** we can define that the annotated widget cannot be empty or set as unchecked (for Switches or Checkboxes for example). Otherwise the validation process will fail. As parameters of this annotation we must set the **order** variable.

With **@Required(order = 1)** we declare in which order the validation should proceed (starting from 1). If all Widgets have the same order number, all errors will be shown at the same time.
	The next (optional) parameter is **messageResId**. Now we can declare a Resource string as error message. So every widget can show its own error message from the proper **res/values/strings.xml** File. Otherwise the default error message: *"This field is required."* will be shown (not translated to other languages). Example:

```
@Required(order = 1, messageResId = R.string.custom_error_message)
private EditText username;
```
Rules for number inputs
---
With **@NumberRule** we can define a Rule for number inputs. Again, we must define the **order** position in which order, the error should be proceed and (optional) the custom string Resource with the custom error message (**messageResId**).

But additionally we can set a **type** so we can define which type is allowed for this Widget. Example:

```
@NumberRule(order = 4, messageResId = R.string.custom_error_message, type = NumberRule.NumberType.INTEGER)
private EditText username;
```
We only allow integers.

Min or Max Length of an input
---
The minimum or maximum input chars can be checked with **@TextRule**. Again, we must use **order** and (optional) **messageResId** here as we already know. But this time we can add **minLength** and/or **maxLength**. So we can control the minimum or maximum length of strings or numbers inputs. Example:

```
@TextRule(order = 5, messageResId = R.string.custom_error_message, minLength = 5, maxLength = 5)
private EditText username;
```
If we want exact five numbers or digits (if we have annotated this widget also with **@NumberRule**).

E-Mail input
---
And the last annotation I want to present is **@Email**. As you can guess with, this annotation it is possible to validate that in this field is a correct Email Address. This is a really nice feature! Example:

```
@Email(order = 7, messageResId = R.string.custom_error_message)
private EditText username;
```
Simple but powerful.

Prepare to handle the validation result
---
After we annotated all widgets we want to validate, we must add a validation listener to listen to the result of the validation process.
Within the **onCreate()** Method we add the listener:

```
validator = new Validator(this);
validator.setValidationListener(this);
```
So the class must implement the interface:

```
implements Validator.ValidationListener {
```
This will create two Methods:

```
public void onValidationSucceeded() {
public void onValidationFailed(View failedView, Rule<?> failedRule) {
```
In this Method we can handle the validation result.

Handle the validation result
---
So if we think it is time to validate our annotated widgets we call:

```
validator.validate();
```
This Method will run the validations an returns the result with the given callback Methods **onValidatenSucceeded()** and **onValidationFailed(...)**.

If the validation was successful the **onValidationSucceeded()** Method will be called.
In this Method we could proceed with our Programm, because all Annotated fields were correct.
But if the validation process failed the **onValidationFailed(View failedView, Rule<?> failedRule)** Method will be called.
In this Method we could set all error messages we defined in the annotation fields. Example:

```
@Override
public void onValidationFailed(View failedView, Rule<?> failedRule) {
	String message = failedRule.getFailureMessage();
	if (failedView instanceof EditText) {
		((EditText) failedView).setError(message);
	}
}
```
Now is the **order** value important in which order the errors will be proceed and shown. If it is the first validation try and every annotated widget fails, only the errors with the order value 1 (because we start at 1) will be validated and if it fails the custom error message will be shown at the Widgets with the order value 1.

In my opinion a very powerful library to validate user inputs. Especially if you have different rules for the widgets and you want all user input updates validated with a TextWatcher:

```
firstnameEditTextInput.addTextChangedListener(this);
```
So with this library, you must only call **validator.validate();** to check for a correct user input in the **public void onTextChanged(CharSequence charSequence, int i, int i1, int i2) {** Method.

For further information, visit the [***Github Repo from Saripaar***](https://github.com/ragunathjawahar/android-saripaar).
