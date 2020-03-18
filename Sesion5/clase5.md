# Ejercicios de la clase 3 de sistemas operativos

### 1 Cree dos archivos de texto similares (con una o dos líneas diferentes). Compárelos empleando diff.

El comando para comparar fue:
```powershell
diff (Get-Content test1.txt) (Get-Content test2.txt)
```
Que da como salida:

```console
InputObject         SideIndicator
-----------         -------------
Su nombre es test2. =>           
Ya probé.           =>           
Su nombre es test1. <=           
Estoy probando.     <=           
```
El contenido de los archivos de texto era:
```powershel
type test1.txt
```

```console
Este es un archivo de texto.
Su nombre es test1.
Estoy probando.
```
```powershell
type test2.txt
```
```console
Este es un archivo de texto.
Su nombre es test2.
Ya probé.
```

### 2 Qué ocurre si se ejecuta: ```powershell get-service | export-csv servicios.csv | out-file``` Por qué?.

Ocurre un error que dice:
```console
out-file : Cannot process argument because the value of argument "path" is null. Change the value of argument "path" 
to a non-null value.
At line:1 char:42
+ get-service | export-csv servicios.csv | out-file
+                                          ~~~~~~~~
    + CategoryInfo          : InvalidArgument: (:) [Out-File], PSArgumentNullException
    + FullyQualifiedErrorId : ArgumentNull,Microsoft.PowerShell.Commands.OutFileCommand
```
El comando falla ya que cuando se emplea el "out-file" se debe de especificar el nombre del archivo donde se guardará la salida del comando anterior (Es a lo que hace referencia el mensaje de error cuando dice que el argumento "path" es nulo).

### 3 Cómo haría para crear un archivo delimitado por puntos y comas (;)? PISTA: Se emplea export-csv, pero con un parámetro adicional.

Se debe de agregar el parámetro -Delimiter, con lo que queda de la siguiente forma:
```powershell
Get-Process | export opera.csv -Delimiter ";"
```

### 4 ```powershell Export-cliXML``` y ```powershell Export-CSV``` modifican el sistema, porque pueden crear y sobreescribir archivos. Existe algún parámetro que evite la sobreescritura de un archivo existente? Existe algún parámetro que permita que el comando pregunte antes de sobresscribir un archivo?
Para evitar la sobreescritura de un archivo existente se utiliza el parámetro -NoClobber de la siguiente forma:
```powershell
Get-Process |Export-Csv opera.csv -NoClobber
```
Que da como salida:
```console
Export-Csv : The file 'C:\Users\danig\opera.csv' already exists.
At line:1 char:14
+ Get-Process |Export-Csv opera.csv -NoClobber
+              ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ResourceExists: (C:\Users\danig\opera.csv:String) [Export-Csv], IOException
    + FullyQualifiedErrorId : NoClobber,Microsoft.PowerShell.Commands.ExportCsvCommand
   
```
Para que el comando pregunte antes de sobreescribir un archivo se utiliza -Confirm:
```powershell
Get-Process |Export-Csv opera.csv -Confirm
```

### 5 Windows emplea configuraciones regionales, lo que incluye el separador de listas. En Windows en inglés, el separador de listas es la coma (,). Cómo se le dice a Export-CSV que emplee el separador del sistema en lugar de la coma?
Se utiliza el parámetro -UseCulture de la siguiente forma:

```powershell
Get-Process |Export-Csv opera.csv -UseCulture
```

### 6 Identifique un cmdlet que permita generar un número aleatorio.

El comando es ```powershell Get-Random```
Que da como salida, por ejemplo ```console 582138374```

### 7 Identifique un cmdlet que despliegue la fecha y hora actuales.
El comando es ```powershell Get-Date```
Que da como salida, por ejemplo ```console Wednesday, February 19, 2020 8:38:27 AM```

### 8 Qué tipo de objeto produce el cmdlet de la pregunta 7?

El tipo de objeto es "DateTime"

### 9 Usando el cmdlet de la pregunta 7 y select-object, despliegue solamente el día de la semana, así:
```console
   DayOfWeek
   ---------
    Thursday
 ```
 
 Se ejecuta el comando
 ```powershell
 Get-Date | Select-Object DayOfWeek
 ```
 Da como salida
 ```console
 DayOfWeek
---------
Wednesday
```

### 10 Identifique un cmdlet que muestre información acerca de parches (hotfixes) instalados en el sistema.
El comando es ```powershell Get-Hotfix```
En mi caso, la salida es
```console
Source        Description      HotFixID      InstalledBy          InstalledOn              
------        -----------      --------      -----------          -----------              
DESKTOP-U2... Update           KB4534132     NT AUTHORITY\SYSTEM  2/8/2020 12:00:00 AM     
DESKTOP-U2... Security Update  KB4515383     NT AUTHORITY\SYSTEM  10/19/2019 12:00:00 AM   
DESKTOP-U2... Security Update  KB4516115     NT AUTHORITY\SYSTEM  10/26/2019 12:00:00 AM   
DESKTOP-U2... Security Update  KB4521863     NT AUTHORITY\SYSTEM  10/24/2019 12:00:00 AM   
DESKTOP-U2... Security Update  KB4524569     NT AUTHORITY\SYSTEM  11/14/2019 12:00:00 AM   
DESKTOP-U2... Security Update  KB4528759     NT AUTHORITY\SYSTEM  1/17/2020 12:00:00 AM    
DESKTOP-U2... Security Update  KB4537759     NT AUTHORITY\SYSTEM  2/15/2020 12:00:00 AM    
DESKTOP-U2... Security Update  KB4538674     NT AUTHORITY\SYSTEM  2/15/2020 12:00:00 AM    
DESKTOP-U2... Update           KB4532693     NT AUTHORITY\SYSTEM  2/15/2020 12:00:00 AM    
```

### 11 Empleando el cmdlet de la pregunta 10, muestre una lista de parches instalados. Luego extienda la expresión para ordenar la lista por fecha de instalación, y muestre en pantalla únicamente la fecha de instalación, el usuario que instaló el parche, y el ID del parche. Recuerde examinar los nombres de las propiedades.
El comando extendido sería
```powershell
Get-HotFix | Sort-Object InstalledOn | Select-Object InstalledOn, InstalledBy, HotFixId
```
En mi caso, da como salida:
```console
InstalledOn            InstalledBy         HotFixId 
-----------            -----------         -------- 
10/19/2019 12:00:00 AM NT AUTHORITY\SYSTEM KB4515383
10/24/2019 12:00:00 AM NT AUTHORITY\SYSTEM KB4521863
10/26/2019 12:00:00 AM NT AUTHORITY\SYSTEM KB4516115
11/14/2019 12:00:00 AM NT AUTHORITY\SYSTEM KB4524569
1/17/2020 12:00:00 AM  NT AUTHORITY\SYSTEM KB4528759
2/8/2020 12:00:00 AM   NT AUTHORITY\SYSTEM KB4534132
2/15/2020 12:00:00 AM  NT AUTHORITY\SYSTEM KB4537759
2/15/2020 12:00:00 AM  NT AUTHORITY\SYSTEM KB4538674
2/15/2020 12:00:00 AM  NT AUTHORITY\SYSTEM KB4532693
```
### 12 Complemente la solución a la pregunta 11, para que el sistema ordene los resultados por la descripción del parche, e incluya en el listado la descripción, el ID del parche, y la fecha de instalación. Escriba los resultados a un archivo HTML.
El comando es:
```powershell
Get-HotFix | Sort-Object Description | Select-Object Description, HotFixId, InstalledOn | ConvertTo-Html | Out-File operati.html
```
Al ejecutar ```powershell type operati.html```, se obtiene:
```console
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"  "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<title>HTML TABLE</title>
</head><body>
<table>
<colgroup><col/><col/><col/></colgroup>
<tr><th>Description</th><th>HotFixId</th><th>InstalledOn</th></tr>
<tr><td>Security Update</td><td>KB4515383</td><td>10/19/2019 12:00:00 AM</td></tr>
<tr><td>Security Update</td><td>KB4516115</td><td>10/26/2019 12:00:00 AM</td></tr>
<tr><td>Security Update</td><td>KB4521863</td><td>10/24/2019 12:00:00 AM</td></tr>
<tr><td>Security Update</td><td>KB4524569</td><td>11/14/2019 12:00:00 AM</td></tr>
<tr><td>Security Update</td><td>KB4528759</td><td>1/17/2020 12:00:00 AM</td></tr>
<tr><td>Security Update</td><td>KB4537759</td><td>2/15/2020 12:00:00 AM</td></tr>
<tr><td>Security Update</td><td>KB4538674</td><td>2/15/2020 12:00:00 AM</td></tr>
<tr><td>Update</td><td>KB4534132</td><td>2/8/2020 12:00:00 AM</td></tr>
<tr><td>Update</td><td>KB4532693</td><td>2/15/2020 12:00:00 AM</td></tr>
</table>
</body></html>
```

### 13 Muestre una lista de las 50 entradas más nuevas del log de eventos System. Ordene la lista de modo que las entradas más antiguas aparezcan primero; las entradas producidas al mismo tiempo deben ordenarse por número índice. Muestre el número índice, la hora y la fuente para cada entrada. Escriba esta información en un archivo de texto plano.

El comando es:
```powershell
Get-EventLog system -Newest 50 | Sort-Object TimeGenerated, Index | Select-Object Index, TimeGenerated, Source | Out-File logs.txt
```

Al ejecutar el comando ```powershell type logs.txt``` se obtiene:
```console
Index TimeGenerated        Source                                
----- -------------        ------                                
12628 2/18/2020 8:05:24 PM Microsoft-Windows-Hyper-V-VmSwitch    
12629 2/18/2020 8:05:27 PM Microsoft-Windows-Kernel-Power        
12630 2/19/2020 7:48:48 AM Microsoft-Windows-Kernel-General      
12631 2/19/2020 7:48:49 AM Microsoft-Windows-Hyper-V-VmSwitch    
12632 2/19/2020 7:48:49 AM Microsoft-Windows-Hyper-V-VmSwitch    
12633 2/19/2020 7:48:49 AM Microsoft-Windows-Hyper-V-VmSwitch    
12634 2/19/2020 7:48:49 AM Microsoft-Windows-Hyper-V-VmSwitch    
12635 2/19/2020 7:48:49 AM Microsoft-Windows-Kernel-Power        
12636 2/19/2020 7:48:49 AM Microsoft-Windows-Kernel-Boot         
12637 2/19/2020 7:48:49 AM Microsoft-Windows-Kernel-Boot         
12638 2/19/2020 7:48:49 AM Microsoft-Windows-Kernel-Boot         
12639 2/19/2020 7:48:49 AM Microsoft-Windows-Kernel-Boot         
12640 2/19/2020 7:48:49 AM Microsoft-Windows-Kernel-Boot         
12641 2/19/2020 7:48:50 AM BTHUSB                                
12642 2/19/2020 7:48:50 AM BTHUSB                                
12643 2/19/2020 7:48:52 AM Microsoft-Windows-Kernel-Power        
12644 2/19/2020 7:48:53 AM Microsoft-Windows-Winlogon            
12645 2/19/2020 7:48:54 AM Microsoft-Windows-Hyper-V-VmSwitch    
12646 2/19/2020 7:48:54 AM Microsoft-Windows-Hyper-V-VmSwitch    
12647 2/19/2020 7:48:54 AM Microsoft-Windows-Power-Troubleshooter
12648 2/19/2020 7:48:57 AM DCOM                                  
12649 2/19/2020 7:48:57 AM DCOM                                  
12650 2/19/2020 7:49:46 AM DCOM                                  
12651 2/19/2020 7:51:03 AM Service Control Manager               
12652 2/19/2020 7:51:08 AM DCOM                                  
12653 2/19/2020 7:55:28 AM Service Control Manager               
12654 2/19/2020 7:56:46 AM Service Control Manager               
12655 2/19/2020 7:59:16 AM Service Control Manager               
12656 2/19/2020 8:07:31 AM Microsoft-Windows-DNS-Client          
12657 2/19/2020 8:42:19 AM Microsoft-Windows-DNS-Client          
12658 2/19/2020 8:55:17 AM Microsoft-Windows-Kernel-Power        
12659 2/19/2020 8:55:26 AM Microsoft-Windows-Kernel-Power        
12660 2/19/2020 8:55:26 AM Microsoft-Windows-Hyper-V-VmSwitch    
12661 2/19/2020 8:55:26 AM Microsoft-Windows-Kernel-Power        
12662 2/19/2020 8:55:27 AM Microsoft-Windows-Hyper-V-VmSwitch    
12663 2/19/2020 8:55:27 AM Microsoft-Windows-Hyper-V-VmSwitch    
12664 2/19/2020 8:55:27 AM Microsoft-Windows-Hyper-V-VmSwitch    
12665 2/19/2020 8:55:29 AM Microsoft-Windows-Kernel-Power        
12666 2/19/2020 9:00:50 AM Microsoft-Windows-Kernel-General      
12667 2/19/2020 9:00:50 AM Microsoft-Windows-Kernel-Power        
12668 2/19/2020 9:00:51 AM Microsoft-Windows-Hyper-V-VmSwitch    
12669 2/19/2020 9:00:51 AM Microsoft-Windows-Hyper-V-VmSwitch    
12670 2/19/2020 9:00:51 AM Microsoft-Windows-Hyper-V-VmSwitch    
12671 2/19/2020 9:00:51 AM Microsoft-Windows-Hyper-V-VmSwitch    
12672 2/19/2020 9:00:51 AM BTHUSB                                
12673 2/19/2020 9:00:51 AM BTHUSB                                
12674 2/19/2020 9:00:55 AM Microsoft-Windows-Hyper-V-VmSwitch    
12675 2/19/2020 9:00:55 AM Microsoft-Windows-Hyper-V-VmSwitch    
12676 2/19/2020 9:00:58 AM Microsoft-Windows-Power-Troubleshooter
12677 2/19/2020 9:00:59 AM Microsoft-Windows-DNS-Client     
```
