## Warming up

* Cmdlets

* Pipeline

---

### Cmdlets

* "Heart-and-soul of Windows PowerShell"

* Simple, modular tasks

* Actually just .net classes that transform objects

* Follow _Verb_-_Noun_ format:

	* e.g. `Get-Content`, `Write-Error`
	
* `Get-Command`

	* List all loaded cmdlets

* `Get-Verb`

	* List all verbs (because why not?)

---

#### Cmdlets Examples


```powershell
Get-ChildItem
# or alias:
dir
```
```powershell
Test-Path config/default.json
```
```powershell
New-Item -Force some/path/to/file.txt
```
```powershell
Get-Content *.xml
```

---

### Pipeline

* Output from one process feeds directly as input to next

* Unix has text stream pipelines:
```bash
tr 'A-Z' 'a-z' < file.txt | tr -cs 'a-z' '\n' | sort | uniq
```

* PowerShell has object pipeline:
```powershell
(Get-Content file.txt) -Split "[^a-z]+" | Sort -Unique
```

* Compose simple processes to solve a larger task

* Objects stream as they're available (where possible)

---

#### Pipeline Examples

```powershell
Get-ChildItem | Sort-Object LastWriteTime | Select-Object FullName
```

* Get files/folders in current directory
  (`FileSystemInfo` objects)

* Sort using the `LastWriteTime` property

* Project the `FullName` property

* End of pipeline (output to screen)

---

### Loops/If-Then-Else Revisited

Loops and if-then-else aren't pipeline friendly

```powershell
$attachments = @();

foreach ($email in $emails) {
  if ($email.HasAttachments) {
    $attachments += $email.GetAttachments()
  }
}
```

Nice and familiar, but doesn't take advantage of the pipeline

Notes: no streaming, quick one-liners are clunky

---

### Loops/If-Then-Else Revisited

Enter `ForEach-Object` and `Where-Object`:

```powershell
list | ForEach-Object { do_something } | transformed_list

list | Where-Object { test_something } | filtered_list
```

* Script blocks are executed for every input object
	* Current object referenced as `$_`, e.g.

* `ForEach-Object` _doesn't_ mutate the original list

```powershell
@(1..10) | Where-Object { $_ % 2 }
# 1, 3, 5, 7, 9
@(1..10) | ForEach-Object { $_ * 2 }
# 2, 4, 6, 8, 10, 12, 14, 16, 18, 20
```

---

### Loops/If-Then-Else Revisited

Was:

```powershell
$attachments = @();

foreach ($email in $emails) {
	if ($email.HasAttachments) {
		$attachments += $email.GetAttachments()
	}
}
```

Our pipeline converted email example:

```powershell
$attachments = $emails | Where-Object {
	$_.HasAttachments
} | ForEach-Object {
	$_.GetAttachments()
}
```

Much better!

---

#### Pipeline Examples

```powershell
Get-Content -Raw *.xml | ForEach-Object {
	[xml] $_
} | Where-Object {
	$_.order.lineitem.action -eq "sell"
} | ForEach-Object {
	$_.order.customer
}
```

* Get raw content of all XML files

* Transform to `XmlDocument` objects (magic)

* Filter those with "sell" actions

* Map to custom object containing `customer`

* End of pipeline (output to screen)

---

### Streaming Pipeline


```powershell
$numbers = @(5..1) | %{ Sleep -Milliseconds 500; $_ };
$numbers | %{ "Got $_" }
```

Pipeline forced to run to completion; slow

```powershell
@(5..1) | %{ Sleep -Milliseconds 500; $_ } | %{ "Got $_" }
```

Pipeline, objects processed as they arrive

```powershell
@(5..1) | %{ Sleep -Milliseconds 500; $_ } | Sort | %{ "Got $_" }
```

Not all cmdlets can take advantage of streaming