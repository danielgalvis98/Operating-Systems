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


PS C:\Users\Usuario> type test1.txt
Este es un archivo de texto.
Su nombre es test1.
Estoy probando.

PS C:\Users\Usuario> type test2.txt
Este es un archivo de texto.
Su nombre es test2.
Ya probé.
