# Ejercicios de la clase 5 de sistemas operativos

### 1. ¿Cuál clase puede emplearse para consultar la dirección IP de un adaptador de red? ¿Posee dicha clase algún método para liberar un préstamo de dirección (lease) DHCP?

La clase que puede emplearse para consultar la dirección IP de un adaptador de red es Win32_NetworkAdapterConfiguration. Por ejemplo, al usar el comando ```powershell Get-WmiObject Win32_NetworkAdapterConfiguration``` su resultado es:
```console
DHCPEnabled      : False
IPAddress        : {192.168.56.1, fe80::f11a:8db3:e27:e07b}
DefaultIPGateway : 
DNSDomain        : 
ServiceName      : VBoxNetAdp
Description      : VirtualBox Host-Only Ethernet Adapter
Index            : 21
```
En cuanto al método, al usar ```powershell Get-WmiObject Win32_NetworkAdapterConfiguration | Get-Member``` se muestra un método con la siguiente información: 
```console
Name                         MemberType    Definition                                                                                                                                                                                                                     
ReleaseDHCPLease             Method        System.Management.ManagementBaseObject ReleaseDHCPLease()
```
Que podría ser utilizado.


### 2. Despliegue una lista de parches empleando WMI (Microsoft se refiere a los parches con el nombre quick-fix engineering). Es diferente el listado al que produce el cmdlet Get-Hotfix?

Se puede visualizar con el comando ```powershell Get-WmiObject Win32_QuickFixEngineering ``` que produce como resultado:
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
DESKTOP-U2... Security Update  KB4541338     NT AUTHORITY\SYSTEM  3/13/2020 12:00:00 AM    
DESKTOP-U2... Update           KB4551762     NT AUTHORITY\SYSTEM  3/14/2020 12:00:00 AM  
```
Su salida es exactamente la misma a la del cmdlet ```powershell Get-Hotfix```


### 3. Empleando WMI, muestre una lista de servicios, que incluya su status actual, su modalidad de inicio, y las cuentas que emplean para hacer login.

Se logra gracias al comando ```powershell Get-WmiObject Win32_Service | Select-Object status, startmode, systemname ```
su salida es de la forma:
```console
status  startmode systemname     
------  --------- ----------     
OK      Manual    DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Auto      DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Disabled  DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Auto      DESKTOP-U258E43
OK      Auto      DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Auto      DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Auto      DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Auto      DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Auto      DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Auto      DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Auto      DESKTOP-U258E43
OK      Auto      DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Auto      DESKTOP-U258E43
OK      Auto      DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Auto      DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Auto      DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Auto      DESKTOP-U258E43
OK      Auto      DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Auto      DESKTOP-U258E43
OK      Auto      DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Auto      DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Auto      DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
OK      Manual    DESKTOP-U258E43
```

### 4. Empleando cmdlets de CIM, liste todas las clases del namespace SecurityCenter2, que tengan product como parte del nombre.
Esto se logra con el comando ```powershell Get-CimClass -Namespace root\SecurityCenter2 | where cimclassname -Like '*product*'```
Su salida es:

```console
   NameSpace: ROOT/SecurityCenter2

CimClassName                        CimClassMethods      CimClassProperties                                                                                                        
------------                        ---------------      ------------------                                                                                                        
AntiSpywareProduct                  {}                   {displayName, instanceGuid, pathToSignedProductExe, pathToSignedReportingExe...}                                          
AntiVirusProduct                    {}                   {displayName, instanceGuid, pathToSignedProductExe, pathToSignedReportingExe...}                                          
FirewallProduct                     {}                   {displayName, instanceGuid, pathToSignedProductExe, pathToSignedReportingExe...} 
```
### 5. Empleando cmdlets de CIM, y los resultados del ejercicio anterior, muestre los nombres de las aplicaciones antispyware instaladas en el sistema. También puede consultar si hay productos antivirus instalados en el sistema.

Para ver los nombres de las aplicaciones antispyware, se usa ```powershell Get-CimInstance -Namespace root\SecurityCenter2 -ClassName AntiSpywareProduct | Select-Object displayName```
Que da como resultado:
```console
displayName     
-----------     
Windows Defender
```
En cuanto a los antivirus, se usa el comando
```powershell  Get-CimInstance -Namespace root\SecurityCenter2 -ClassName AntiVirusProduct | Select-Object displayName ```
Que también da como output
```console
displayName     
-----------     
Windows Defender
```
