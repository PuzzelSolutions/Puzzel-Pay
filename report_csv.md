# CSV report

A report file is generated daily with all successful transactions from the day before. The file has no header row and the fields are separated with semicolon. 

Please note that the number of fields in the file depends on how many variables you provide in your request. Be aware that if you create batches with different number of variables, this report will contain lines with various number of variables.

<table><thead>
<tr>
<th>Field number</th>
<th>Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>batchReference</td>
<td>Input parameter</td>
</tr>
<tr>
<td>2</td>
<td>clientReference</td>
<td>Input parameter</td>
</tr>
<tr>
<td>3</td>
<td>msisdn</td>
<td>Input parameter</td>
</tr>
<tr>
<td>4</td>
<td>amount</td>
<td>Input parameter</td>
</tr>
<tr>
<td>5</td>
<td>currency</td>
<td>Input parameter</td>
</tr>
<tr>
<td>6</td>
<td>paymentProvider</td>
<td>Which payment provider the end user selected (Current values are Direct Pay, PCGW, Mobile Banking, SMS, Apple Pay, PayPal)</td>
</tr>
<tr>
<td>7</td>
<td>paymentSucceeded</td>
<td>Will always be 1 as only successful transactions are included in this report.</td>
</tr>
<tr>
<td>8</td>
<td>timestamp</td>
<td>Timestamp when the payment was made</td>
</tr>
<tr>
<td>9</td>
<td>externalReference</td>
<td>A reference from the payment provider.</td>
</tr>
<tr>
<td>10, 12, etc (even numbers)</td>
<td>Name of a variable</td>
<td>Input parameter</td>
</tr>
<tr>
<td>11, 13, etc (odd numbers)</td>
<td>Value of corresponding variable in previous field</td>
<td>Input parameter</td>
</tr>
</tbody>
</table>

### Example of a file
```
1234-1;100001;+4799999990;162800;NOK;PCGW;1;29.05.2017 10:20:20;755;firstname;Steven;amount;1.628,00
1234-1;100002;+4799999991;51300;NOK;PCGW;1;29.05.2017 09:08:43;754;firstname;Jamie;amount;513,00
1234-1;100003;+4799999992;103700;NOK;PCGW;1;29.05.2017 07:50:51;753;firstname;Kenny;amount;1.037,00
1234-1;100004;+4799999993;123200;NOK;PCGW;1;29.05.2017 17:33:36;756;firstname;Robbie;amount;1.232,00
```
