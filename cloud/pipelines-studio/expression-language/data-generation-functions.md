# Data Generation Functions

Data generation functions are included in the Protector stage library. You can use data generation functions only in the Protector: Expression processor.

Data generation functions generate random fake data that you can use as a replacement for sensitive data. There are two kinds of data generation functions:

* Faker functions - Generate specific types of fake data, such as addresses, names, and credit card numbers.
* Xeger functions - Generate fake data based on user-defined regular expressions.

Both sets of functions provide two types of output:

**Random**

Values are generated randomly, regardless of input value. Each value that is replaced is replaced with a new, random value.

For example, say you use `randomFaker:email()` to generate fake email addresses to replace user email addresses. In this case, a random email address is generated for each user email address that is replaced.

**Deterministic**

Generated values are reused when the same input values appear. This allows you to determine in downstream processing that values recur, while ensuring that the data is protected.When you configure the function, you specify the input value that enables the reuse of generated values.You can use any logical expression to define the input value, but here are some common cases:

*   To use the value that is being replaced, use the field function, `f:value()`.

    For example, say you use the following expression to deterministically generate a replacement URL `${deterministicFaker:url(f:value())}`. And say the expression replaces www.RealCompanyName.com with www.fakename.com. Each time www.RealCompanyName.com appears in a URL field, it is replaced with the same URL, www.fakename.com.
*   To use values in other fields in the record, use the `record:value()` function.For example, say you want to generate the same fake name for matching UserIDs. Then, you might use the following expressions to generate the first and last names:

    ```
    ${deterministicFaker:firstName(record:value('/UserID'))} ${deterministicFaker:lastName(record:value('/UserID'))}
    ```

    So if the function generates `Mia Lakier` for the `R2204` user ID, each time `R2204` reappears as an ID, the function replaces the real name with `Mia Lakier`.

Use deterministic functions to provide insight into the frequency of repeated values while protecting sensitive data.

Note: Deterministic functions reuse generated values for the exact same input value. Data with differences in formatting, such as a dash instead of parentheses for area codes, are not considered the same. For best results, ensure that the data uses uniform formatting. When necessary, you can use category functions to standardize certain categories of data.

## Faker Functions <a href="#ariaid-title15" id="ariaid-title15"></a>

The Protector stage library includes the following faker functions to generate fake data. You can use these functions only in stages included in the Protector stage library.

Faker functions have two types of output, random and deterministic.

The following faker functions are listed by category.

### Address-Related Data

For address-related data, you can use the following faker functions:

#### deterministicFaker:country(\<input>)

Generates a fake country name. Reuses the same generated country name for the same input value. Uses the following argument:

* input - Value to determine when to reuse generated country names.

Return type: String.

For example, the following expression generates fake country names and reuses the same fake name for the same input value:

```
${deterministicFaker:country(f:value)}
```

So, if the expression generates `Indonesia` to replace `Mexico`, each time that `Mexico` appears in the data, it is replaced by `Indonesia`.

#### randomFaker:country()

Generates a fake country name. Uses a random country name for every input value.

Return type: String.

#### deterministicFaker:UsCity(\<input>)

Generates a fake city in the United States. Reuses the same city name for the same input value. Uses the following argument:

* input - Value to determine when to reuse generated city names.

Return type: String.

#### randomFaker:UsCity()

Generates a fake city in the United States. Uses a random city name for every input value.

Return type: String.

#### deterministicFaker:UsFullAddress(\<input>)

Generates a fake full address in the United States. Reuses the same address for the same input value. Uses the following argument:

* input - Value to determine when to reuse generated addresses.

Return type: String.

#### randomFaker:UsFullAddress()

Generates a fake full address in the United States. Uses a random address for every input value.

Return type: String.

#### deterministicFaker:UsState(\<input>)

Generates a fake United States state name. Reuses the same name for the same input value. Uses the following argument:

* input - Value to determine when to reuse generated state names.

Return type: String.

#### randomFaker:UsState()

Generates a fake United States state name. Uses a random state name for every input value.

Return type: String.

#### deterministicFaker:UsStreetAddress(\<input>)

Generates a fake street address in the United States. Reuses the same address for the same input value. Uses the following argument:

* input - Value to determine when to reuse generated addresses.

Return type: String

#### randomFaker:UsStreetAddress()

Generates a fake street address in the United States. Uses a random street address for every input value.

Return type: String.

#### deterministicFaker:UsZipCode(\<input>)

Generates a fake United States 9-digit zip code. Reuses the same zip code for the same input value. Uses the following argument:

* input - Value to determine when to reuse generated zip codes.

Return type: String.

#### randomFaker:UsZipCode()

Generates a fake United States 9-digit zip code. Uses a random zip code for every input value.Return type: String.

### Finance-Related Data

For finance-related data, you can use the following faker functions:

#### deterministicFaker:creditCard(\<input>)

Generates a fake credit card number. Reuses the same generated number for the same input value. Uses the following argument:

* input - Value to determine when to reuse generated numbers.

Return type: String.

For example, the following expression generates a fake credit card number and reuses the fake number when the `id` field in the record has the same ID:

```
${deterministicFaker:creditCard(record:value('/id'))}
```

So say a record has an ID of `0034` and a fake credit card number replaces the real card number in the record. Then, each time a subsequent record has the `0034` ID, the function uses the same fake credit card number to replace the real credit card number.

#### randomFaker:creditCard()

Generates a fake credit card number. Uses a random number for every input value.Return type: String.

#### deterministicFaker:creditCardOfType(\<type>, \<input>)

Generates a fake credit card number of the specified type. Reuses the same generated number for the same input value. Uses the following arguments:

* type - Credit card type to generate. Use one of the following values: `AMERICAN_EXPRESS`, `MASTERCARD`, or `VISA`.
* input - Value to determine when to reuse generated numbers. Uses a random number for every input value.

Return type: String.

For example, the following expression generates a fake American Express credit card number and reuses the same number each time the same credit card number appears:

```
${deterministicFaker:creditCard('AMERICAN_EXPRESS', 'f:value()')}
```

#### randomFaker:creditCardOfType(\<type>)

Generates a fake credit card number of the specified type. Uses a random number for every input value. Uses the following argument:

* type - Credit card type to generate. Use of the following values: `AMERICAN_EXPRESS`, `MASTERCARD`, or `VISA`.

Return type: String.

For example, the following expression generates a random Visa credit card number:

```
${randomFaker:creditCardOfType('VISA')}
```

#### deterministicFaker:iban(\<input>)

Generates a fake international bank account number (IBAN). Reuses the same generated number for the same input value. Uses the following argument:

* input - Value to determine when to reuse generated numbers.

Return type: String.

#### randomFaker:iban()

Generates a fake IBAN. Uses a random number for every input value.

Return type: String.

#### deterministicFaker:ibanForCountry(\<country code>, \<input>)

Generates a fake IBAN for the specified country. Reuses the same generated number for the same input value. Uses the following arguments:

*   country code - An [ISO-3166-1 alpha-2 country code](https://en.wikipedia.org/wiki/ISO\_3166-1\_alpha-2#Officially\_assigned\_code\_elements) to indicate the IBAN country format to generate. Note that only a subset of all countries have their own IBAN format. For a list, see [this Wikipedia page](https://en.wikipedia.org/wiki/International\_Bank\_Account\_Number#IBAN\_formats\_by\_country).\
    \
    Tip: For the country code, you can also use the first two characters listed in the IBAN Fields column in the second link. For example, the country code for Albania is `AL`, as shown below:

    ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Wiki-CountryCode.png)
* input - Value to determine when to reuse generated codes.

Return type: String.

For example, the following expression creates a fake IBAN for Costa Rica and reuses the same IBAN each time the same country code appears:

```
${deterministicFaker:ibanForCountry('CR','f:value()')}
```

#### randomFaker:ibanForCountry(\<country code>)

Generates a fake IBAN for the specified country. Uses a random IBAN for every input value. Uses the following argument:

*   country code - An [ISO-3166-1 alpha-2 country code](https://en.wikipedia.org/wiki/ISO\_3166-1\_alpha-2#Officially\_assigned\_code\_elements) to indicate the IBAN country format to generate. Note that only a subset of all countries have their own IBAN format. For a list, see [this Wikipedia page](https://en.wikipedia.org/wiki/International\_Bank\_Account\_Number#IBAN\_formats\_by\_country).\
    \
    Tip: For the country code, you can also use the first two characters listed in the IBAN Fields column in the second link. For example, the country code for Albania is `AL`, as shown below:

    ![](https://docs.streamsets.com/datacollector/3.22.x/help/datacollector/UserGuide/Graphics/Wiki-CountryCode.png)

Return type: String.

For example, the following expression generates a random Albanian IBAN for every input value:

```
${randomFaker:ibanForCountry('AL')}
```

### **Internet-Related Data**

For internet-related data, you can use the following faker functions:

#### deterministicFaker:email(\<input>)

Generates a fake email address. Reuses the same generated address for the same input value. Uses the following argument:

* input - Value to determine when to reuse generated addresses.

Return type: String.

For example, the following expression generates fake email addresses and reuses the fake address when it sees the same input email address:

```
${deterministicFaker:email(f:value)}
```

So, if the expression generates `jo@nys.com` to replace an email address, each time that same email address appears in the data, it is replaced by `jo@nys.com`.

#### randomFaker:email()

Generates a fake email address. Uses a random email address for every input value.

Return type: String.

#### deterministicFaker:url(\<input>)

Generates a fake URL. Reuses the same generated URL for the same input value. Uses the following argument:

* input - Value to determine when to reuse generated URLs.

Return type: String.

#### randomFaker:url()

Generates a fake URL. Uses a random URL for every input value.

Return type: String.

### **Person-Related Data**

For person-related data, you can use the following faker functions:

#### deterministicFaker:race(\<input>)

Generates a fake race. Reuses the same generated race for the same input value. Uses the following argument:

* input - Value to determine when to reuse generated values.

Return type: String.

For example, the following expression generates fake races and uses the same fake value when it sees the same input value:

```
${deterministicFaker:race(f:value)}
```

So, if the expression generates `Japanese` to replace `Thai`, each time that `Thai` appears in the data, it is replaced by `Japanese`.

#### randomFaker:race()

Generates a fake race. Uses a random race for every input value.

Return type: String.

#### deterministicFaker:UsFirstName(\<input>)

Generates a fake United States first name. Reuses the same generated name for the same input value. Uses the following argument:

* input - Value to determine when to reuse generated names.

Return type: String.

For example, the following expression generates a fake first name and reuses the fake name when the ID field in the record has the same ID:

```
${deterministicFaker:UsFirstName(record:value('/id'))}
```

So if the expression generates `Joan` in a record with an ID of `1234`, then each time a subsequent record has the `1234` ID, the function returns `Joan`.

#### randomFaker:UsFirstName()

Generates a fake United States first name. Uses a random name for every input value.

Return type: String.

#### deterministicFaker:UsLastName(\<input>)

Generates a fake United States last name. Reuses the same generated name for the same input value. Uses the following argument:

* input - Value to determine when to reuse generated names.

Return type: String.

#### randomFaker:UsLastName()

Generates a fake United States last name. Uses a random name for every input value.

Return type: String.

#### deterministicFaker:UsPhone(\<input>)

Generates a fake United States phone number. Reuses the same number for the same input value. Uses the following argument:

* input - Value to determine when to reuse generated numbers.

Return type: String.

#### randomFaker:UsPhone()

Generates a fake United States phone number. Uses a random number for every input value.

Return type: String.

#### deterministicFaker:UsSsn(\<input>)

Generates a fake United States social security number. Reuses the same number for the same input value. Uses the following argument:

* input - Value to determine when to reuse generated numbers.

Return type: String.

#### randomFaker:UsSsn()

Generates a fake United States social security number. Uses a random number for every input value.Return type: String.

## Xeger Functions <a href="#ariaid-title16" id="ariaid-title16"></a>

Xeger functions generate fake data based on user-defined regular expressions. You can use xeger functions only in stages included in the Protector stage library.

Before using a xeger function, check if a faker function generates the type of data that you are looking for.

You can use xeger functions with category functions to generate random data while retaining some parts of the original data. For example, the following expression generates a fake user name based on \[a-z]{5,10} and reuses it when the same email address appears in the `email` field. Then, it appends the fake user name to the real domain associated with the input email address:

```
${xeger:deterministic(‘[a-z]{5,10}’, record:value('/email')}@${EMAIL:domain()}
```

This generates user names as follows: \<fake username>@\<real domain>.

You can use the following xeger functions:

#### xeger:deterministic(\<regular expression>, \<input>)

Generates fake data based on the structure of the data defined by the regular expression. Reuses the same fake data when the input value is the same. Uses the following arguments:

* regular expression - A regular expression that defines the structure of the fake data to generate.
* input - Value to determine when to reuse generated values.

Return type: String.

For example, the following expression generates a fake social security number and reuses the number in records that have the same user ID:

```
${xeger:deterministic('[0-9]{3}-[0-9]{2}-[0-9]{4}', record:value('/userId')}
```

So when the expression generates a fake social security number for a record with a user ID of `55343`, each time the `55343` user ID appears, the expression returns the same fake social security number. Though the social security numbers may not be strictly valid, the data is protected.

#### xeger:random(\<regular expression>)

Generates fake data based on the structure of the data defined by the regular expression. Generates new fake data for every input value. Uses the following argument:

* regular expression - A regular expression that defines the structure of the fake data to generate.

Return type: String.
