# FTP Interface

If you are unable to use our [REST API](rest.md), we provide a FTP service that will read your data files and make calls to the [REST interface](rest.md). Below is listed our standard file format, but customized data file readers can be developed for a fee.



## File format and content

Our service accepts files with semicolon-separated fields.
The first line must contain configuration data, while the following n lines contain detailed data changeable pr customer.

Do not use a line with header data. 

Do not use semicolon inside the text fields (as that will be interpreted as a field separator).

Files must have the .csv extension and it is encouraged to use unique file names (ie have timestamp as part of the filename).

It is recommended to encode the text file with UTF-8 encoding.



### Configuration data fields (1st line of the file):

<table><thead>
<tr>
<th>Id</th>
<th>Name</th>
<th>Description</th>
<th>Example</th>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>Version</td>
<td>Version of the FTP file processor. Set to 1.</td>
<td>1</td>
</tr>
<tr>
<td>2</td>
<td>batchReference</td>
<td>Unique identifier for the batch, note that this reference is appended with "-part{i}" if number of recipients are greater than 250.</td>
<td>test-20170101-120000</td>
</tr>
<tr>
<td>3</td>
<td>Expiry days</td>
<td>Number of days until the payment becomes invalid</td>
<td>30</td>
</tr>
<tr>
<td>4</td>
<td>SMS message text</td>
<td>Text that is sent to user in the initial SMS <sup>1</sup></td>
<td>Hello, click on the link below to pay</td>
</tr>
<tr>
<td>5</td>
<td>SMS receipt text</td>
<td>Text message sent to user after a successful payment (leave this field empty if you don't want to send a text message receipt) <sup>1</sup></td>
<td>Your payment is now confirmed.</td>
</tr>
</tbody>
</table>



### Customer data fields (line 2 and onwards of the file):

<table>
<thead>
<tr>
<th>Id</th>
<th>Name</th>
<th>Description</th>
<th>Example</th>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>msisdn</td>
<td>msisdn of the recipient</td>
<td>+4799999999</td>
</tr>
<tr>
<td>2</td>
<td>Amount</td>
<td>Amount to pay, in lowest monetary unit</td>
<td>1000</td>
</tr>
<tr>
<td>3</td>
<td>Currency</td>
<td>Currency code (ISO 4217)</td>
<td>NOK</td>
</tr>
<tr>
<td>x+n</td>
<td>Variable name x</td>
<td>Name of input variable <sup>2</sup></td>
<td>name</td>
</tr>
<tr>
<td>y+n</td>
<td>Variable value y</td>
<td>Value of input variable <sup>2</sup></td>
<td>Kjetil</td>
</tr>
</tbody>
</table>

<sup>1</sup>Note that variables can be used in these texts. Variables are notated like this: {{variable_name}} - example of text with a variable: "Hello {{name}}, please click on the link to pay your bill of {{amount}} NOK". 
Some variables are predefined and can be used without adding them at the end of the line in the file, they are listed [here](rest.md#predefined-variables)  

<sup>2</sup>These fields can be repeated n times, but be sure to have the same number of variable names as you have variable values.



### Example of a file
```
1;test-20170101-120000;30;Hello {{name}}, click here to pay {{amount}} kroner.;Your payment of {{amount}} is now confirmed
+4799999999;10000;NOK;name;Kjetil;amount;100
+4799999998;25000;NOK;name;Steven;amount;250
+4799999997;7500;NOK;name;Robbie;amount;75
```
