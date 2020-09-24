<div align="center">

## Credit Card Validator


</div>

### Description

These functions thoroughly attempt to invalidate American Express, Discover, MasterCard and Visa with length, character validation, prefix matching, check digit, and expiration date tests. The html test document by no means will attempt to forward your credit card numbers. The action of the form is set to an invalid URL
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[vsim](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/vsim.md)
**Level**          |Intermediate
**User Rating**    |4.1 (37 globes from 9 users)
**Compatibility**  |
**Category**       |[Complete Applications](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/complete-applications__2-64.md)
**World**          |[Java](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/java.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/vsim-credit-card-validator__2-2493/archive/master.zip)





### Source Code

```
<html>
<head>
<title>Credit Card Validation</title>
<script language="JavaScript">
<!--
/*
There are three functions in this set for credit card validation.
The main function is:
validateCard(cardNumber,cardType,cardMonth,cardYear)
	parameters:
		all paramaters are string values.
		Month & Year come from the select input fields in the form, so they are defined.
		cardType can be:
			'a' for American Express
			'd' for Discover
			'm' for MasterCard
			'v' for Visa
	description:
		This function will check string length, valid characters, specific credit card prefixes and test
		the Mod 10 (LUHN Formula) for validating possible credit card numbers. This function can only
		authorize that the given card data is potentially valid. You would still need to run actual
		card validation routines to verify the actual account.
	returns:
		this function returns true if the card number could be valid for the card type and expiration date.
		false otherwise.
supporting functions:
mod10( cardNumber )
	parameters:
		this function takes the text string card number and runs the Mod 10 formula on its respective digits.
	description:
		Mod 10 is the check digit formula for the supported cards these functions attempt to validate.
	returns:
		this function returns true if the number passes the check digit test.
		false otherwise.
expired( cardMonth, cardYear )
	parameters:
		this function takes the text string values given by the html form.
	description:
		this function basically will check to make sure todays date is less than the expiration date the user inputs.
		this function is not locked into using 2 digit dates.
	returns:
		this fucntion returns true if the card is expired.
		false otherwise.
*/
function mod10( cardNumber ) { // LUHN Formula for validation of credit card numbers.
	var ar = new Array( cardNumber.length );
	var i = 0,sum = 0;
	for( i = 0; i < cardNumber.length; ++i ) {
		ar[i] = parseInt(cardNumber.charAt(i));
	}
	for( i = ar.length -2; i >= 0; i-=2 ) { // you have to start from the right, and work back.
		ar[i] *= 2;							 // every second digit starting with the right most (check digit)
		if( ar[i] > 9 ) ar[i]-=9;			 // will be doubled, and summed with the skipped digits.
	}										 // if the double digit is > 9, add those individual digits together
	for( i = 0; i < ar.length; ++i ) {
		sum += ar[i];						 // if the sum is divisible by 10 mod10 succeeds
	}
	return (((sum%10)==0)?true:false);
}
function expired( month, year ) {
	var now = new Date();							// this function is designed to be Y2K compliant.
	var expiresIn = new Date(year,month,0,0,0);		// create an expired on date object with valid thru expiration date
	expiresIn.setMonth(expiresIn.getMonth()+1);		// adjust the month, to first day, hour, minute & second of expired month
	if( now.getTime() < expiresIn.getTime() ) return false;
	return true;									// then we get the miliseconds, and do a long integer comparison
}
function validateCard(cardNumber,cardType,cardMonth,cardYear) {
	if( cardNumber.length == 0 ) {						//most of these checks are self explanitory
		alert("Please enter a valid card number.");
		return false;
	}
	for( var i = 0; i < cardNumber.length; ++i ) {		// make sure the number is all digits.. (by design)
		var c = cardNumber.charAt(i);
		if( c < '0' || c > '9' ) {
			alert("Please enter a valid card number. Use only digits. Do not use spaces or hyphens.");
			return false;
		}
	}
	var length = cardNumber.length;			//perform card specific length and prefix tests
	switch( cardType ) {
		case 'a':
			if( length != 15 ) {
				alert("Please enter a valid American Express Card number.");
				return;
			}
			var prefix = parseInt( cardNumber.substring(0,2));
			if( prefix != 34 && prefix != 37 ) {
				alert("Please enter a valid American Express Card number.");
				return;
			}
			break;
		case 'd':
			if( length != 16 ) {
				alert("Please enter a valid Discover Card number.");
				return;
			}
			var prefix = parseInt( cardNumber.substring(0,4));
			if( prefix != 6011 ) {
				alert("Please enter a valid Discover Card number.");
				return;
			}
			break;
		case 'm':
			if( length != 16 ) {
				alert("Please enter a valid MasterCard number.");
				return;
			}
			var prefix = parseInt( cardNumber.substring(0,2));
			if( prefix < 51 || prefix > 55) {
				alert("Please enter a valid MasterCard Card number.");
				return;
			}
			break;
		case 'v':
			if( length != 16 && length != 13 ) {
				alert("Please enter a valid Visa Card number.");
				return;
			}
			var prefix = parseInt( cardNumber.substring(0,1));
			if( prefix != 4 ) {
				alert("Please enter a valid Visa Card number.");
				return;
			}
			break;
	}
	if( !mod10( cardNumber ) ) {               		// run the check digit algorithm
		alert("Sorry! This is not a valid credit card number.");
		return false;
	}
	if( expired( cardMonth, cardYear ) ) {							// check if entered date is already expired.
		alert("Sorry! The expiration date you have entered would make this card invalid.");
		return false;
	}
	return true; // at this point card has not been proven to be invalid
}
//-->
</script>
<style type="text/css">
P { font-family:arial,verdana;font-size:10pt;font-weight:bold; color: #043829 }
</style>
</head>
<body bgcolor="ccfff">
<form action="URLTOPROCESSDATA" method="POST" enctype="application/x-www-form-urlencoded" name="ccform" onsubmit="return validateCard(this.cardNumber.value,this.cardType.value,this.cardMonth.value,this.cardYear.value);">
<table cellspacing="5" cellpadding="2" bgcolor="#DA9D67">
<tr>
  <td align="right" valign="middle" nowrap><p>Select Card Type:</p></td>
  <td valign="bottom" nowrap>
		<select name="cardType">
			<option value="a"> American Express
  			<option value="d"> Discover
  			<option value="m"> MasterCard
  			<option value="v"> Visa
  		</select>
	</td>
</tr>
<tr>
  <td align="right" valign="middle" nowrap><p>Enter Card Number:</p></td>
  <td valign="bottom" nowrap>
		<p><input type="Text" name="cardNumber" size="17" maxlength="16">&nbsp;&nbsp;example:&nbsp;<font size=-1><i>( 1234567890123456 )</i></i></font></p>
	</td>
</tr>
<tr>
  <td align="right" valign="middle" nowrap><p>Select Expiration Date:</p></td>
  <td valign="bottom" nowrap>
		<p>
		<select name="cardMonth">
			<option value="01"> 01
  			<option value="02"> 02
  			<option value="03"> 03
 			<option value="04"> 04
  			<option value="05"> 05
  			<option value="06"> 06
  			<option value="07"> 07
  			<option value="08"> 08
 			<option value="09"> 09
  			<option value="10"> 10
  			<option value="11"> 11
  			<option value="12"> 12
		</select>
		<select name="cardYear">
			<option value="1999"> 99
			<option value="2000"> 00
			<option value="2001"> 01
  			<option value="2002"> 02
  			<option value="2003"> 03
 			<option value="2004"> 04
  			<option value="2005"> 05
  			<option value="2006"> 06
  			<option value="2007"> 07
  			<option value="2008"> 08
 			<option value="2009"> 09
  			<option value="2010"> 10
		</select>
		&nbsp;&nbsp;example:&nbsp;<font size=-1><i>( MM YY )</i></i></font>
		</p>
	</td>
</tr>
<tr>
	<td colspan="2" align="CENTER">
		<input type="Submit" name="submit" value="Submit">&nbsp;<input type="reset" value="Reset">
	</td>
</tr>
</table>
</form>
</body>
</html>
```

