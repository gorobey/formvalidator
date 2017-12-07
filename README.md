Using the PHP form validation script

    Include formvalidator.php in your form processing script
    1
    	require_once "formvalidator.php"
    Create a FormValidator object and add the form validation descriptors.
    1
    	$validator = new FormValidator();
    2
    	$validator->addValidation("Name","req","Please fill in Name");
    3
    	$validator->addValidation("Email","email",
    4
    	"The input for Email should be a valid email value");
    5
    	$validator->addValidation("Email","req","Please fill in Email");

    The first argument is the name of the input field in the form. The second argument is the validation descriptor that tells the type of the validation required. The third argument is the error message to be displayed if the validation fails.
    Validate the form by calling ValidateForm() function
    1
    	if(!$validator->ValidateForm())
    2
    	{
    3
    	    echo "<B>Validation Errors:</B>";
    4
    	    $error_hash = $validator->GetErrors();
    5
    	    foreach($error_hash as $inpname => $inp_err)
    6
    	    {
    7
    	        echo "<p>$inpname : $inp_err</p>\n";
    8
    	    }
    9
    	}

Example

The example below will make the idea clearer
1
	<?PHP
2
	require_once "formvalidator.php";
3
	$show_form=true;
4
	if(isset($_POST['Submit']))
5
	{
6
	    $validator = new FormValidator();
7
	    $validator->addValidation("Name","req","Please fill in Name");
8
	    $validator->addValidation("Email","email",
9
	"The input for Email should be a valid email value");
10
	    $validator->addValidation("Email","req","Please fill in Email");
11
	    if($validator->ValidateForm())
12
	    {
13
	        echo "<h2>Validation Success!</h2>";
14
	        $show_form=false;
15
	    }
16
	    else
17
	    {
18
	        echo "<B>Validation Errors:</B>";
19
	 
20
	        $error_hash = $validator->GetErrors();
21
	        foreach($error_hash as $inpname => $inp_err)
22
	        {
23
	          echo "<p>$inpname : $inp_err</p>\n";
24
	        }
25
	    }
26
	}
27
	 
28
	if(true == $show_form)
29
	{
30
	?>
1
	<form name='test' method='POST' action='' accept-charset='UTF-8'>
2
	Name: <input type='text' name='Name' size='20'>
3
	Email: <input type='text' name='Email' size='20'>
4
	<input type='submit' name='Submit' value='Submit'>
5
	</form>
1
	<?PHP
2
	}//true == $show_form
3
	?>
Adding Custom Validation

If you want to add a custom validation, which is not provided by the validation descriptors, you can do so. Here are the steps:

    Create a class for the custom validation and override the DoValidate() function
    1
    	class MyValidator extends CustomValidator
    2
    	{
    3
    	    function DoValidate(&$formars,&$error_hash)
    4
    	    {
    5
    	        if(stristr($formars['Comments'],'http://'))
    6
    	        {
    7
    	            $error_hash['Comments']="No URLs allowed in comments";
    8
    	            return false;
    9
    	        }
    10
    	    return true;
    11
    	    }
    12
    	}
    Add the custom validation object
    1
    	$validator = new FormValidator();
    2
    	$validator->addValidation("Name","req","Please fill in Name");
    3
    	$validator->addValidation("Email","email",
    4
    	 "The input for Email should be a valid email value");
    5
    	$validator->addValidation("Email","req","Please fill in Email");
    6
    	$custom_validator = new MyValidator();
    7
    	$validator->AddCustomValidator($custom_validator);

The custom validation function will be called automatically after other validations.
Table of Validation Descriptors

Here is the list of all validation descriptors:
Validation Descriptor	Usage
req	The field should not be empty
maxlen=???	checks the length entered data to the maximum. For example, if the maximum size permitted is 25, give the validation descriptor as “maxlen=25”
minlen=???	checks the length of the entered string to the required minimum. example “minlen=5”
alnum	Check the data if it contains any other characters other than alphabetic or numeric characters
alnum_s	Allows only alphabetic, numeric and space characters
num	Check numeric data
alpha	Check alphabetic data.
alpha_s	Check alphabetic data and allow spaces.
email	The field is an email field and verify the validity of the data.
lt=???
lessthan=???	Verify the data to be less than the value passed. Valid only for numeric fields.
example: if the value should be less than 1000 give validation description as “lt=1000”
gt=???
greaterthan=???	Verify the data to be greater than the value passed. Valid only for numeric fields.
example: if the value should be greater than 10 give validation description as “gt=10”
regexp=???	Check with a regular expression the value should match the regular expression.
example: “regexp=^[A-Za-z]{1,20}$” allow up to 20 alphabetic characters.
dontselect=??	This validation descriptor is for select input items (lists) Normally, the select list boxes will have one item saying ‘Select One’. The user should select an option other than this option. If the value of this option is ‘Select One’, the validation description should be “dontselect=Select One”
dontselectchk	This validation descriptor is for check boxes. The user should not select the given check box. Provide the value of the check box instead of ??
For example, dontselectchk=on
shouldselchk	This validation descriptor is for check boxes. The user should select the given check box. Provide the value of the check box instead of ??
For example, shouldselchk=on
dontselectradio	This validation descriptor is for radio buttons. The user should not select the given radio button. Provide the value of the radio button instead of ??
For example, dontselectradio=NO
selectradio	This validation descriptor is for radio buttons. The user should select the given radio button. Provide the value of the radio button instead of ??
For example, selectradio=yes
selmin=??	Select atleast n number of check boxes from a check box group.
For example: selmin=3
selone	Makes a radio group mandatory. The user should select atleast one item from the radio group.
eqelmnt=???	compare two elements in the form and make sure the values are the same For example, ‘password’ and ‘confirm password’. Replace the ??? with the name of the other input element.
For example: eqelmnt=confirm_pwd
