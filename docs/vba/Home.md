# VBA

worksheets("Sheet1").usedrange.rows.count
worksheets("name").active

For Each row In Sheet1.UsedRange.Rows
row.Delete
Next

for each row in sheet1.UsedRange.rows
Cell(row, 1).value = 2
next
