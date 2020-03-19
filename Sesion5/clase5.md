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
ReleaseDHCPLease             Method        System.Management.ManagementBaseObject ReleaseDHCPLease()```

Que podría ser utilizado.
