# OCR report

A report file is generated daily with all successful transactions from the day before. The format is based on NETS specification as found here: https://www.nets.eu/no-nb/PublishingImages/Lists/Accordion%20%20OCR%20giro/AllItems/OCR%20giro%20-%20System%20manual.pdf

The report file has lines with a fixed length of 80 characters. 

The first two records are always a “Start record for Transmission” and a “Start record for Assignment”. 
The last two records are always a “End record for Assignment” and a “End record for Transmission”.
For each transaction, 2 or 3 OCR transaction records are added, named “Amount item 1, 2, 3” in this specification.

### "Start record for transmission" 
<table>
<thead>
<tr>
<td>#</strong></td>
<td>Field</strong></td>
<td>Value</strong></td>
<td>Comment</strong></td>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>Format code</td>
<td>NY</td>
<td>Always</td>
</tr>
<tr>
<td>2</td>
<td>Service code</td>
<td>00</td>
<td>Always</td>
</tr>
<tr>
<td>3</td>
<td>Transmission type</td>
<td>00</td>
<td>Always</td>
</tr>
<tr>
<td>4</td>
<td>Record type</td>
<td>10</td>
<td>"Start transmission" type</td>
</tr>
<tr>
<td>5</td>
<td>Data transmitter</td>
<td>00008080</td>
<td>Nets' ID</td>
</tr>
<tr>
<td>6</td>
<td>Transmission number</td>
<td>1812171</td>
<td>7 characters date('dmy') + seq_id (seq_id = number from 1 to 9, usually 1 since we only generate one report per day)</td>
</tr>
<tr>
<td>7</td>
<td>Data recipient</td>
<td>00123456</td>
<td>Customer's id</td>
</tr>
<tr>
<td>8</td>
<td>Filler</td>
<td>49x '0'</td>
<td>Filled with zeroes</td>
</tr>
</tbody>
</table>


### "Start record for assignment"
<table>
<thead>
<tr>
<td>#</td>
<td>Field</td>
<td>Value</td>
<td>Comment</td>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>Format code</td>
<td>NY</td>
<td>Always</td>
</tr>
<tr>
<td>2</td>
<td>Service code</td>
<td>09</td>
<td>Always</td>
</tr>
<tr>
<td>3</td>
<td>Assignment type</td>
<td>00</td>
<td>Always</td>
</tr>
<tr>
<td>4</td>
<td>Record type</td>
<td>20</td>
<td>"Start assignment" type</td>
</tr>
<tr>
<td>5</td>
<td>Agreement id</td>
<td>001234567</td>
<td>Customer's agreement id 9 characters</td>
</tr>
<tr>
<td>6</td>
<td>Assignment number</td>
<td>0000001</td>
<td>Serial number accumulated per assignment in the transmission 7 characters</td>
</tr>
<tr>
<td>7</td>
<td>Assignment account</td>
<td>00012345678</td>
<td>&nbsp;Payee's (agreement's) account number.<br /> 11 characters</td>
</tr>
<tr>
<td>8</td>
<td>Filler</td>
<td>45x '0'</td>
<td>Filled with zeroes</td>
</tr>
</tbody>
</table>


### "Amount item 1"

<table>
<thead>
<tr>
<td>#</td>
<td>Field</td>
<td>Value</td>
<td>Comment</td>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>Format code</td>
<td>NY</td>
<td>Always</td>
</tr>
<tr>
<td>2</td>
<td>Service code</td>
<td>09</td>
<td>OCR giro service code</td>
</tr>
<tr>
<td>3</td>
<td>Transaction type</td>
<td>21</td>
<td>Configured pr service according to customer needs. The usual ones:<br /> 10 = Transaction from giro debited acct<br /> 19 = Purchase with KID<br /> 20 = Refund with free text (note this comes automatically on refunds, and can not be configured as default)<br /> 21 = Purchase with free text</td>
</tr>
<tr>
<td>4</td>
<td>Record type</td>
<td>30</td>
<td>Amount item 1 record type</td>
</tr>
<tr>
<td>5</td>
<td>Transaction number</td>
<td>0000001</td>
<td>Sequence number per transaction in the assignment</td>
</tr>
<tr>
<td>6</td>
<td>Date</td>
<td>181217</td>
<td>Date in DDMMYY format <em>PSP processing date</em></td>
</tr>
<tr>
<td>7</td>
<td>Centre id</td>
<td>00</td>
<td>Not in use, fill with zeros</td>
</tr>
<tr>
<td>8</td>
<td>Day code</td>
<td>00</td>
<td>Not in use, fill with zeros</td>
</tr>
<tr>
<td>9</td>
<td>Partial settlement number</td>
<td>0</td>
<td>Not in use, fill with zeros</td>
</tr>
<tr>
<td>10</td>
<td>Serial number</td>
<td>00000</td>
<td>Not in use, fill with zeros</td>
</tr>
<tr>
<td>11</td>
<td>Sign</td>
<td>0</td>
<td>Not in use, fill with zeros</td>
</tr>
<tr>
<td>12</td>
<td>Amount</td>
<td>00000000000082300</td>
<td>Amount in LMU (i.e., &oslash;re for NOK) 17 characters</td>
</tr>
<tr>
<td>13</td>
<td>Customer Reference<br /> (often KID)</td>
<td>&nbsp;1216245017</td>
<td>25 characters, right-aligned and filled with blanks (spaces)</td>
</tr>
<tr>
<td>14</td>
<td>Filler</td>
<td>6x '0'</td>
<td>Filled with zeroes</td>
</tr>
</tbody>
</table>



### "Amount item 2"
<table>
<thead>
<tr>
<td>#</td>
<td>Field</td>
<td>Value</td>
<td>Comment</td>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>Format code</td>
<td>NY</td>
<td>Always</td>
</tr>
<tr>
<td>2</td>
<td>Service code</td>
<td>09</td>
<td>Always</td>
</tr>
<tr>
<td>3</td>
<td>Transaction type</td>
<td>21</td>
<td>Configured pr service according to customer needs. The usual ones:<br /> 10 = Transaction from giro debited acct<br /> 19 = Purchase with KID<br /> 20 = Refund with free text (note this comes automatically on refunds, and can not be configured as default)<br /> 21 = Purchase with free text</td>
</tr>
<tr>
<td>4</td>
<td>Record type</td>
<td>31</td>
<td>Amount item 2 record type</td>
</tr>
<tr>
<td>5</td>
<td>Transaction number</td>
<td>0000001</td>
<td>Same as for amount item 1 record</td>
</tr>
<tr>
<td>6</td>
<td>Form number</td>
<td>0000000000</td>
<td>Not in use, fill with zeros; 10 characters</td>
</tr>
<tr>
<td>7</td>
<td>Agreement-id / Archive reference</td>
<td>000000000</td>
<td>Not in use, fill with zeros; 9 characters</td>
</tr>
<tr>
<td>8</td>
<td>Filler</td>
<td>7x '0'</td>
<td>Fill with zero; 7 chars</td>
</tr>
<tr>
<td>9</td>
<td>Bank date</td>
<td>181217</td>
<td>Date in DDMMYY format.<br /> <u>Note this will be the same as the date field in amount item 1 as Puzzel does not know the actual bank data</u></td>
</tr>
<tr>
<td>10</td>
<td>Debit account</td>
<td>11x '0'</td>
<td>We do not know the payer's account. Fill with zero; 11 chars</td>
</tr>
<tr>
<td>11</td>
<td>Filler</td>
<td>22x '0'</td>
<td>Fill with zero; 22 chars</td>
</tr>
</tbody>
</table>



### "Amount item 3"
This record is only present if transaction type is either 20 (Refund with free text) or 21 (Purchase with free text).

<table>
<thead>
<tr>
<td>#</td>
<td>Field</td>
<td>Value</td>
<td>Comment</td>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>Format code</td>
<td>NY</td>
<td>Always</td>
</tr>
<tr>
<td>2</td>
<td>Service code</td>
<td>09</td>
<td>OCR giro always service code</td>
</tr>
<tr>
<td>3</td>
<td>Transaction type</td>
<td>21</td>
<td>20 = reversing with free text 21 = payment with free text</td>
</tr>
<tr>
<td>4</td>
<td>Record type</td>
<td>32</td>
<td>Amount item 3 record type</td>
</tr>
<tr>
<td>5</td>
<td>Transaction number</td>
<td>0000001</td>
<td>Same as for amount item 1 record</td>
</tr>
<tr>
<td>6</td>
<td>Free text message</td>
<td>Fill with blanks; 40 chars</td>
</tr>
<tr>
<td>7</td>
<td>Filler</td>
<td>25x '0'</td>
<td>Fill with zero; 25 chars</td>
</tr>
</tbody>
</table>


### "End record for assignment"
<table>
<thead>
<tr>
<td>#</td>
<td>Field</td>
<td>Value</td>
<td>Comment</td>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>Format code</td>
<td>NY</td>
<td>Always</td>
</tr>
<tr>
<td>2</td>
<td>Service code</td>
<td>09</td>
<td>Always</td>
</tr>
<tr>
<td>3</td>
<td>Assignment type</td>
<td>00</td>
<td>Always</td>
</tr>
<tr>
<td>4</td>
<td>Record type</td>
<td>88</td>
<td>End assignment record type</td>
</tr>
<tr>
<td>5</td>
<td>Number of transactions</td>
<td>00000003</td>
<td>Total number of transactions in assignment; 8 chars</td>
</tr>
<tr>
<td>6</td>
<td>Number of records</td>
<td>00000011</td>
<td>Total number of records in the assignment, including start + end assignment records</td>
</tr>
<tr>
<td>7</td>
<td>Total amount</td>
<td>00000000000117300</td>
<td>Absolute sum of all the transactions in the assignment. Revesoins are not deducted. 17 chars</td>
</tr>
<tr>
<td>8</td>
<td>Date</td>
<td>181217</td>
<td>Date in DDMMYY format when the assignment was generated.</td>
</tr>
<tr>
<td>9</td>
<td>Earliest date</td>
<td>171217</td>
<td>Date in DDMMYY format. The earliest processing date for transactions in the assignment. Use transaction timestamp.</td>
</tr>
<tr>
<td>10</td>
<td>Latest date</td>
<td>191217</td>
<td>Date in DDMMYY format. The latest processing date for transactions in the assignment. We use transaction timestamp + 1 week.</td>
</tr>
<tr>
<td>11</td>
<td>Filler</td>
<td>21x '0'</td>
<td>Fill with zero; 21 chars</td>
</tr>
</tbody>
</table>


### "End record for transmission" 
<table>
<thead>
<tr>
<td>#</td>
<td>Field</td>
<td>Value</td>
<td>Comment</td>
</tr>
</thead>
<tbody>
<tr>
<td>1</td>
<td>Format code</td>
<td>NY</td>
<td>Always</td>
</tr>
<tr>
<td>2</td>
<td>Service code</td>
<td>00</td>
<td>Always</td>
</tr>
<tr>
<td>3</td>
<td>Transmission type</td>
<td>00</td>
<td>Always</td>
</tr>
<tr>
<td>4</td>
<td>Record type</td>
<td>89</td>
<td>End transmission record type</td>
</tr>
<tr>
<td>5</td>
<td>Number of transactions</td>
<td>00000003</td>
<td>Total number of transactions in transmission; 8 chars</td>
</tr>
<tr>
<td>6</td>
<td>Number of records</td>
<td>00000011</td>
<td>Total number of records in transmission, including start + end assignment records and start + end transmission records.</td>
</tr>
<tr>
<td>7</td>
<td>Total amount</td>
<td>00000000000117300</td>
<td>Absolute sum of all the transactions in the transmission. Reversals are not deducted. 17 chars</td>
</tr>
<tr>
<td>8</td>
<td>Date</td>
<td>181217</td>
<td>Date in DDMMYY format when the assignment was generated.</td>
</tr>
<tr>
<td>9</td>
<td>Filler</td>
<td>33x '0'</td>
<td>Fill with zero; 33 chars</td>
</tr>
</tbody>
</table>


### Example of a file
```
NY000010000080801406171001234560000000000000000000000000000000000000000000000000
NY090020001234567000000101234567890000000000000000000000000000000000000000000000
NY09103000000011306170000000000000000000000331200                 12345678000000
NY091031000000100000000000000000000000000130617000000000000000000000000000000000
NY090088000000010000000400000000000331200140617130617200617000000000000000000000
NY000089000000010000000600000000000331200140617000000000000000000000000000000000
```
