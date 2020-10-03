    Sub stock()

    Dim ws As Worksheet
    Dim ticker As String
    Dim vol As Double
    Dim openprice As Double
    Dim closeprice As Double
    Dim yearlychange As Double
    Dim percentchange As Double
    Dim lastrow As Long
    Dim summarytablerow As Integer
    Dim rowcount As Integer
    Dim greatestincrease As Double
    Dim greatestdecrease As Double
    Dim totalvolume As Double

    For Each ws In Worksheets

    ws.Cells(1, 9).Value = "Ticker"
    ws.Cells(1, 10).Value = "Yearly Change"
    ws.Cells(1, 11).Value = "Percent Change"
    ws.Cells(1, 12).Value = "Total Stock Volume"
    ws.Cells(2, 14).Value = "Greatest % Increase"
    ws.Cells(3, 14).Value = "Greatest % Decrease"
    ws.Cells(4, 14).Value = "Greatest Total Volume"
    ws.Cells(1, 15).Value = "Ticker"
    ws.Cells(1, 16).Value = "Value"
   
    summarytablerow = 2
    vol = 0
    rowcount = 0
    lastrow = ws.Cells(Rows.Count, 1).End(xlUp).Row
   
    For i = 2 To lastrow
   
            If ws.Cells(i + 1, 1).Value = ws.Cells(i, 1).Value Then
                vol = vol + ws.Cells(i, 7).Value
           
            ElseIf ws.Cells(i + 1, 1).Value <> ws.Cells(i, 1).Value Then
                ticker = ws.Cells(i, 1).Value
                vol = ws.Cells(i, 7).Value + vol
                closeprice = ws.Cells(i, 6).Value
                openprice = ws.Cells((i - rowcount), 3).Value
                yearlychange = closeprice - openprice
           
                    If openprice <> 0 Then
                    percentchange = yearlychange / openprice
                        Else
                        percentchange = 0
   
                    End If
                   
                ws.Cells(summarytablerow, 9).Value = ticker
                ws.Cells(summarytablerow, 10).Value = yearlychange
                ws.Cells(summarytablerow, 11).Value = percentchange
                ws.Cells(summarytablerow, 12).Value = vol
               
                summarytablerow = summarytablerow + 1
                rowcount = 0
                vol = 0
               
            End If
           
        rowcount = rowcount + 1

           
    Next i
   
    summarylastrow = ws.Cells(Rows.Count, 11).End(xlUp).Row
   
    For i = 2 To summarylastrow

            If ws.Cells(i, 11) >= 0 Then
                ws.Cells(i, 11).Interior.ColorIndex = 4
                    ElseIf ws.Cells(i, 11) < 0 Then
                    ws.Cells(i, 11).Interior.ColorIndex = 3
            End If
           
    Next i
   
    ws.Cells(2, 16).Value = ws.Cells(2, 11).Value
    ws.Cells(3, 16).Value = ws.Cells(2, 11).Value
    ws.Cells(4, 16).Value = ws.Cells(2, 12).Value
   
    greatestincrease = ws.Cells(2, 16).Value
   
    For i = 2 To (summarylastrow - 1)

            If ws.Cells(i + 1, 11).Value > greatestincrease Then
                ws.Cells(2, 15).Value = ws.Cells(i + 1, 9).Value
                greatestincrease = ws.Cells(i + 1, 11).Value
                ws.Cells(2, 16).Value = greatestincrease
            End If
           
    Next i
   
    greatestdecrease = ws.Cells(3, 16).Value
   
    For i = 2 To (summarylastrow - 1)
           
            If ws.Cells(i + 1, 11).Value < greatestdecrease Then
                ws.Cells(3, 15).Value = ws.Cells(i + 1, 9).Value
                greatestdecrease = ws.Cells(i + 1, 11).Value
                ws.Cells(3, 16).Value = greatestdecrease
            End If
       
    Next i
           
    For i = 2 To (summarylastrow - 1)
           
            If ws.Cells(i + 1, 12).Value > totalvolume Then
                ws.Cells(4, 15).Value = ws.Cells(i, 9).Value
                totalvolume = ws.Cells(i + 1, 12).Value
                ws.Cells(4, 16).Value = totalvolume
            End If
           
    Next i
   
        ws.Cells(i, 11).Style = "Percent"
        ws.Cells(2, 16).Style = "Percent"
        ws.Cells(3, 16).Style = "Percent"
   
    Next ws
   
    End Sub

