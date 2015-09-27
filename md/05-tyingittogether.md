## Tying it all together

* Scripts

* Functions

* Handy Commands

---

### Scripts

* .ps1 extension

* Restricted policy prevents execution by default

* `Get-ExecutionPolicy -List`

* `Set-ExecutionPolicy Unrestricted CurrentUser`

---

### Scripts

![Script Execution Error](/img/script-execution-restricted.png)

---

### Functions

```powershell
function Say-Hello {
	Write-Host "Hello!"
}

Say-Hello
# Hello!
```

* cmdlet arguments aren't comma delimited

```powershell
function Greet-Person($name, $greeting = "Hello") {
	Write-Host "$greeting, $name"
}

Greet-Person Dylan
# Hello, Dylan
Greet-Person Rene Bonjour
# Bonjour, Rene
Greet-Person Blake, Yo # wrong!
# Hello, Blake Yo
```

---

### Functions

* Types can be enforced

```powershell
function Repeat-String([string]$str, [int]$rpt, [string]$delim) {
	(@(1..$rpt) | %{ $str }) -join $delim
}

Repeat-String Bang "3"
# Bang, Bang, Bang

Repeat-String Bang "3.8" ", "
# BangBangBangBang
```

---

#### Beware the `return` statement

```powershell
function Is-Even([int] $number) {
	"Testing if $number is even"
	return -not($number % 2)
}
Is-Even 4
# Testing if 4 is even
# True
Is-Even 5
# Testing if 5 is even
# False
```
```powershell
if (Is-Even 5) {
	"Even Steven"
} else {
	"How... odd"
}
# Even Steven
```

* Go home PowerShell, you're drunk!

---

#### Beware the `return` statement

```powershell
function Is-Even([int] $number) {
	"Testing if $number is even"
	return -not($number % 2)
}
```

* This outputs _two_ objects to the pipeline:

	* "Testing if 5 is even", and
	* $false

* Non-empty arrays will evaluate as true! (Mostly)

* Not really `return`'s fault, just a misunderstood pipeline

---

#### Pipeline. The cause of, and solution to, all of life's problems.

```powershell
function Is-Even([int] $number) {
	Write-Host "Testing if $number is even"
	return -not($number % 2)
}

if (Is-Even 5) {
	"Even Steven"
} else {
	"How... odd"
}
# How... odd
```

---

### Handy Commands

* Formatters:

	* `Format-Table [-Autosize]`

	* `Format-List`

	* `Out-GridView`

* `Show-Command`

* `Set-StrictMode latest`