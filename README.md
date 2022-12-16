# WindowsKey

Este archivo lo que hace es basicamente saber nuestro clave de Windows.

Puedes copiar este código y lo pegas en la bloc de notas. luego lo guardas con la extensión .vbs

```sh
Option Explicit  
 
Dim objshell,path,DigitalID, Result  
Set objshell = CreateObject("WScript.Shell") 
'Set registry key path 
Path = "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\" 
'Registry key value 
DigitalID = objshell.RegRead(Path & "DigitalProductId") 
Dim ProductName,ProductID,ProductKey,ProductData 
'Get ProductName, ProductID, ProductKey 
ProductName = "Product Name: " & objshell.RegRead(Path & "ProductName") 
ProductID = "Product ID: " & objshell.RegRead(Path & "ProductID") 
ProductKey = "Installed Key: " & ConvertToKey(DigitalID)  
ProductData = ProductName  & vbNewLine & ProductID  & vbNewLine & ProductKey 
'Show messbox if save to a file  
If vbYes = MsgBox(ProductData  & vblf & vblf & "Save to a file?", vbYesNo + vbQuestion, "BackUp Windows Key Information") then 
   Save ProductData  
End If 
 
'Convert binary to chars 
Function ConvertToKey(Key) 
    Const KeyOffset = 52 
    Dim isWin8, Maps, i, j, Current, KeyOutput, Last, keypart1, insert 
    'Check if OS is Windows 8 
    isWin8 = (Key(66) \ 6) And 1 
    Key(66) = (Key(66) And &HF7) Or ((isWin8 And 2) * 4) 
    i = 24 
    Maps = "BCDFGHJKMPQRTVWXY2346789" 
    Do 
           Current= 0 
        j = 14 
        Do 
           Current = Current* 256 
           Current = Key(j + KeyOffset) + Current 
           Key(j + KeyOffset) = (Current \ 24) 
           Current=Current Mod 24 
            j = j -1 
        Loop While j >= 0 
        i = i -1 
        KeyOutput = Mid(Maps,Current+ 1, 1) & KeyOutput 
        Last = Current 
    Loop While i >= 0  
     
    If (isWin8 = 1) Then 
        keypart1 = Mid(KeyOutput, 2, Last) 
        insert = "N" 
```
Si no, puedes descargar el archivo directamente y lo abres sin quemarte la cabeza.
## Resultado cuando abres el archivo

![Captura](https://user-images.githubusercontent.com/93322506/208157993-7e77df9e-b0c7-4508-8114-888f30d7f3bc.PNG)
