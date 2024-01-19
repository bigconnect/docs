# Category Functions

Category functions are included with the Protector stage library. You can use category functions only in the Protector: Expression processor and the Protector: Masks processor.

Category functions return parts of certain categories of data, such as the area code of a phone number, or the domain of an email address. Category functions enable you to protect sensitive data while retaining parts of the original data for processing.

For example, to protect phone numbers while retaining the area code, you might use one of the following expressions with the Protector: Expression processor:

* `Area code ${US_PHONE:areaCode()}` - This replaces phone numbers with `Area code <area code>`.
*   `${US_PHONE:areaCode()}-xxx-xxxx` - This returns the area code of the phone number with the rest of the number removed, as follows: `<area code>-xxx-xxxx`.

    To achieve this same result, you could alternatively use the following mask with the Protector: Masks processor: `123-xxx-xxxx`.

When needed, you can use category functions to standardize the data for available categories.

## Credit Card Numbers <a href="#ariaid-title8" id="ariaid-title8"></a>

The Protector stage library includes the following category functions for credit card numbers. You can use these functions only in stages included in the Protector stage library.

### CREDIT\_CARD:type()

Returns the type of credit card that the number represents: `AMERICAN_EXPRESS`, `MASTERCARD`, or `VISA.`

Return type: String.

For example, when evaluating `5200-1200-3400-5600`, this function returns `MASTERCARD`.

### CREDIT\_CARD:firstGroup()

Returns the first numeric group of the credit card number.

Return type: String.

For example, when evaluating `5200-1200-3400-5600`, this function returns `5200`.

### CREDIT\_CARD:secondGroup()

Returns the second numeric group of the credit card number.

Return type: String.

For example, when evaluating `5200-1200-3400-5600`, this function returns `1200`.

### CREDIT\_CARD:thirdGroup()

Returns the third numeric group of the credit card number.

Return type: String.

For example, when evaluating `5200-1200-3400-5600`, this function returns `3400`. When evaluating `3700-000000-00000`, this function returns `00000`.

### CREDIT\_CARD:fourthGroup()

Returns the fourth numeric group of the credit card number. When a credit card number does not include a fourth numeric group - such as American Express numbers, which only have three groups - this function returns an empty string.To return the last group of all credit card numbers, use `CREDIT_CARD:lastPart()`.

Return type: String.

For example, when evaluating `5200-1200-3400-5600`, this function returns `5600`. When evaluating `3700-000000-00000`, this function returns an empty string.

### CREDIT\_CARD:lastPart()

Returns the last group of the credit card number. For Mastercard and Visa numbers, returns the last four digits. For American Express numbers, returns the last five digits.

Return type: String.

For example, when evaluating `5200-1200-3400-5600`, this function returns `5600`. When evaluating `370000000000000`, this function returns `00000`.

### CREDIT\_CARD:std()

Returns credit card numbers in their standard digit groups, separated by hyphens.

Return type: String.

For example, when evaluating an American Express number such as `370000000000000`, this function returns `3400-000000-00000`. When evaluating a Visa number such as `5200000000000000`, this function returns `5200-0000-0000-0000`.

## Email Addresses <a href="#ariaid-title9" id="ariaid-title9"></a>

The Protector stage library includes the following category functions for email addresses. You can use these functions only in stages included in the Protector stage library.

### EMAIL:domain()

Returns the domain of email addresses. The domain is everything after the @ symbol.

Return type: String

For example, when evaluating `myname@mydomain.com`, this function returns `mydomain.com`.

### EMAIL:domainTLD()

Returns the top level domain (TLD) of the email address. The TLD is the last part of the domain, such as com, gov, or org.

Return type: String.

For example, when evaluating `myname@mydomain.com`, this function returns `com`.

### EMAIL:domainWithoutTLD()

Returns the domain without the TLD part of the email address.

Return type: String.For example, when evaluating `myname@mydomain.com`, this function returns `mydomain`.

### EMAIL:localPart()

Returns the local part of email addresses. The local part is everything before the @ symbol.

Return type: String.

For example, when evaluating `myname@mydomain.com`, this function returns `myname`.

### EMAIL:std()

Returns the email address in the following standard format:

```
<local part>@<domain>
```

such as: `name@company.com`.

Return type: String.

For example, when evaluating `myname@mydomain.com`, this function returns `myname@mydomain.com`.



\
