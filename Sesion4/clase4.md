# Ejercicios de la clase 4 de sistemas operativos

### 1.Mostrar una tabla de procesos que incluya únicamente los nombres de los procesos, sus IDs, y si están respondiendo a Windows (la propiedad Responding muestra eso). Haga que la tabla tome el mínimo de espacio horizontal, pero no permita que la información se trunque.

Para solucionarlo, se emplea el comando:
```powershell
Get-Process | ft -Property name,id,Responding -Wrap
```

Con el ```powershell -Wrap ``` se está evitando que la información se trunque, gracias a que extiende las líneas cuando el resultado sea muy largo.


### 2.Muestre una tabla de procesos que incluya los nombres de los procesos y sus IDs. También incluya columnas para uso de memoria virtual y física; exprese dichos valores en megabytes (MB).

Se emplea el comando 
```powershell
Get-Process | ft name,id,@{n='VM (MB)';e={$_.VM / 1MB -as [int]}},@{n='PM (MB)';e={$_.PM / 1MB -as [int]}}
```

Gracias al ```powershell @{n='VM (MB)';e={$_.VM / 1MB -as [int]}}``` se puede mostrar la memorioa virtual en MB. Lo mismo aplica con la memoria física.

### 3.Emplee Get-EventLog para mostrar una lista de los logs de eventos disponibles (revise la ayuda para encontrar el parámetro que le permitirá obtener dicha información). Formatee la salida como una tabla que incluya el nombre de despliegue del log y el período de retención. Los encabezados de columna deben ser NombreLog y Per-Retencion.

El comando a usar es:
```powershell
Get-EventLog -List | ft @{n='NombreLog';e={$_.LogDisplayName}},@{n='Per-Retencion';e={$_.MinimumRetentionDays}}
```
Acá, el ```powershell -List ``` permite listar los logs. Con ```powershel  @{n='NombreLog';e={$_.LogDisplayName}}``` se está cambiando el nombre del encabezado de esta columna. Aplica de igual forma para el periodo de retención.

### 4. Muestre una lista de servicios, de tal manera que aparezcan agrupados los servicios que están iniciados y los que están detenidos. Los que están iniciados deben aparecer primero.

El comando es:
```powershell
Get-Service | sort status -Descending |ft -GroupBy status
```

Como se ve, primero se ordena con ```sort status -Descending``` para poder después agrupar

### 5. Mostrar una lista a cuatro columnas de todos los directorios que están en el raíz de la unidad C:

Se usa el comando
```powershell
ls C:/ -Attributes Directory | fw -col 4
```
En donde primero se está filtrando para que muestre únicamente los directorios, y después se la aplica el formato de 4 columnas.

### 6. Cree una lista formateada de todos los archivos .exe del directorio C:\Windows. Debe mostrarse el nombre, la información de versión, y el tamaño del archivo. La propiedad de tamaño se llama length en Powershell, pero para mayor claridad, la columna se debe llamar Tamaño en su listado.

```powershell
ls C:\Windows | where -filter { $_.Name -like '*.exe'} | fl Name,VersionInfo,@{n='Tamaño';e={$_.Length}}
```
Primero se hace el filtrado, para que solo muestre los archivos .exe, y después se aplica el formato.

### 7. Importe el módulo NetAdapter (empleando el comando Import-Module NetAdapter). Empleando el cmdlet Get-NetAdapter, muestre una lista de adaptadores no virtuales (adaptadores cuya propiedad Virtual sea falsa. El valor lógico falso es representado por Powershell como $False).

Se usa el comando
```powershell
Get-NetAdapter | where -Filter { $_.Virtual -eq $false} | fl
```
Donde se hace el filtrado con el ```powershell where -Filter```y después se aplica el formato de lista.

### 8. Importe el módulo DnsClient. Empleando el cmdlet Get-DnsClientCache, muestre una lista de los registros A y AAAA que estén en el caché. Sugerencia: Si el caché está vacío, visite algunos sitios web para poblarlo.

El comando es: 
```powershell
Get-DnsClientCache | Where -Filter {$_.Type -eq 1 -or $_.Type -eq 28} | fl
```
Acá, Powershell trata el 1 como el tipo 'A', y el 28 como el tipo 'AAAA'.

### 9. Genere una lista de todos los archivos .exe del directorio C:\Windows\System32 que tengan más de 5 MB.

El comando a utilizar es 
```powershell
ls C:\Windows\System32 | where -filter { $_.Name -like '*.exe' -and $_.Length -gt 5*1MB} | fl Name,VersionInfo,@{n='Tamaño';e={$_.Length}}
```

Es muy similar al de los .exe de C:/Windows, solo que se le agrega la condición que el Length sea mayor a 5MB (Que se saca usando 5*1MB)

### 10. Muestre una lista de parches que sean actualizaciones de seguridad.

Se usa
```powershell
Get-HotFix | Where -Filter {$_.Description -eq 'Security Update'} | fl
```

### 11. Muestre una lista de parches que hayan sido instalados por el usuario Administrador, que sean actualizaciones. Si no tiene ninguno, busque parches instalados por el usuario System. Note que algunos parches no tienen valor en el campo Installed By.

Se usa
```powershell
Get-HotFix | Where -Filter {$_.Description -eq 'Update' -and $_.InstalledBy -eq 'NT AUTHORITY\SYSTEM'} | fl
```
Es como el anterior, solo que en vez de "Security Update" se pone solo "Update", y se le agrega la condición del InstalledBy


### 12.Genere una lista de todos los procesos que estén corriendo con el nombre Conhost o Svchost.

El comando es
```powershell
Get-Process | Where -Filter {$_.name -eq 'Conhost' -or $_.Name -eq 'Svchost'} | fl
```
