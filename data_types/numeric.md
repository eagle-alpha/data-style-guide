[Back to Table of Contents](../README.md)
# Numerics

### Floats vs. Integers
- __[SHOULD]__ Use integers to represent numerical data unless the value truly has the potential to have decimal values. Metrics such as counts and sums of counts should be represented by integers. 
- __[SHOULD]__ Use floating point representations if the value has the potential to be negative. This is our opinion, but we usually think of integers as representing counts or other whole positive numbers. If a negative integer is desired, consider changing the metric field itself to be the "negative" of that metric and keep the integer positive. 

### Currencies, Thousands,  and Percentages
- __[SHOULD]__ Not use currency symbols in numeric fields. Separate out the currency denomination into either a separate field or if it is always the same currency in every row, consider specifying this in the column name or the description of that column in the data dictionary. This usually results in the data type of the field being cast to a string when it should be a numeric type - obligating unnecessary extra computation by the customer to convert it.
- __[SHOULD]__ Not use commas to represent thousands delineations in numeric fields. This usually results in the data type of the field being cast to a string when it should be a numeric type - obligating unnecessary extra computation by the customer to convert it.
- __[SHOULD]__ Not use the percent sign ( % ) in numeric fields. Consider specifying this attribute (not the symbol itself) in the column name or the description of that column in the data dictionary. If the word percent or percentage is used to describe or name the column, the value of 10.3% should be represented numerically as "10.3" not "0.103". If percent is not used in the name or description, it is essentially a ratio, therefore it should use the "0.103" representation, which is generally more useful computationally.