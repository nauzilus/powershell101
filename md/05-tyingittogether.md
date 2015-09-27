## Tying it all together

* Scripts

* Functions

---

### Scripts

* .ps1 extension

* Restricted policy prevents execution by default

* `Get-ExecutionPolicy -List`

* `Set-ExecutionPolicy RemoteSigned CurrentUser`

---

### Scripts

![Script Execution Error](img/script-execution-restricted.png)

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
# BangBangBang

Repeat-String Bang "3.8" ", "
# Bang, Bang, Bang, Bang
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

* Go home PowerShell, you're drunk! <!-- .element: class="fragment" -->

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