## Getting started

* Variables

	* scalars, arrays, associative arrays, magic

* Operators	

* Falsey/truthy values

* Flow Control

	* if, switch, loops

---

### Variables - scalars

```powershell
$name = "Sam"

$greeting = "Hello, $name."

$length = "$name's name has $($name.Length) characters"

$length = 42

$literally = '$name is literally the variable name'
```

Notes: variables spring into existance; no need to declare. dyanamically typed

---

### Variables - arrays

```powershell
$names = @("Lee", "Kim", "Jamie")
$names += "Ainsley"

$numbers = @(0 .. 9)  # array size 10, filled numbers 0-9
$alphas = "abcdef".ToCharArray() # a..f doesn't work =(

# concatenate two arrays
$hex = $numbers + $alphas
```

---

### Variables - associative arrays

```powershell
$states = @{
  Vic = "Victoria";
  SA  = "South Australia";
  WA  = "Washington";
}
$states["Vic"]
$states.Vic
# Victoria

# keys not case sensitive
$states.Wa = "Western Australia"

$states.Keys
# Vic, SA, WA
$states.Values
# Victoria, South Australia, Western Australia
```

Notes: whenever I see a profile online listed as "location: WA", I _always_ think Western Australia first...

---

### Variables - magic

```powershell
$startDate = [DateTime]::Parse("2014-06-02 09:00")

# auto-parsing
$startDate = [DateTime] "2014-06-02 09:00"
```
```powershell
$xml = [xml] "<order qty='100' name='Soylent Beans' />"

Write-Host "Buy $($xml.order.qty) $($xml.order.name)"
# Buy 100 Soylent Beans
```
```powershell
$random = New-Object Random
$random.Next()

$guid = [Guid]::NewGuid()
```

---

### Operators

`<`, `>`, `&`, and `|` have special meaning, so can't be used for operators.

As such, there are a few<small class="fragment" data-fragment-index="1">*</small> operators to learn:

<div class="fragment" data-fragment-index="1"><pre class="lang-powershell"><code>= + - * / % += -= *= /= %= ++ --
-shl -shr -bAnd -bOr -bXor -bNot -join
-and -or -xor -not ! -is -isNot -as
-eq -ne -gt -ge -lt -le
-Like -NotLike -Match -NotMatch
-Contains -NotContains -In -NotIn
-ceq -cne -cgt -cge -clt -cle
-cLike -cNotLike -cMatch -cNotMatch
-cContains -cNotContains -cIn -cNotIn
-ieq -ine -igt -ige -ilt -ile
-iLike -iNotLike -iMatch -iNotMatch
-iContains -iNotContains -iIn -iNotIn
-Split -cSplit -iSplit
-Replace -cReplace -iReplace</code></pre></div>

<small class="fragment" data-fragment-index="1">* Dozen. A few dozen.</small>

---

### Operators

Though only a handful regularly used:

```powershell
= + - * / % += -= *= /= %= ++ --
```
```powershell
-and -or -not
```
```powershell
-eq -ne -gt -ge -lt -le
-Split -Replace
```

* case insensitive by default
* prefix `c`, e.g. `-ceq`

```powershell
-Like -Match -Contains -In
```

* prefix `not`, e.g. `-NotLike`

---

### Falsey Values

Following all resolve as `$false` in Boolean context
```powershell
$null
0
""    # empty string
@()   # empty array
@(0)  # empty array with single falsey value
```

---

### Truthy Values

Following all resolve as `$true` in Boolean context
```powershell
1
42
0.1
"1"
"0"
" "
"any word"
@{}    # empty (or populated) associative array
@(123) # non-empty array
@(0,0) # array with multiple falsey values ಠ_ಠ
```

Notes: also whitespace like \t, \r, \n, or zero-width spaces (U+200B)

---

### Flow control - if

```powershell
if ($name -eq "Sam") {
  # ...
}
elseif ($foo -or ($bar -and -not $baz)) {
  # note: no space between "else" and "if"
} 
elseif (Test-Path "foo/bar/baz.txt" -Type Leaf) {
  # ...
} 
else {
  # ...
}
```

---

### Flow control - switch

```powershell
switch($code) {
  0 { "OK" }
  1 { "Error" }
  default { "Invalid" }
}
```
```powershell
switch -regex ($code) {
  "^1\d\d$" { "Informational" }
  "^2\d\d$" { "Success" }
  "^3\d\d$" { "Redirection" }
  "^4\d\d$" { "Client Error" }
  "^5\d\d$" { "Server Error" }
}
```

---

### Flow control - loops

```powershell
foreach ($thing in $things) {
  # ...
}

for ($i = 0; $i -lt $things.Count; $i++) {
  # ...
}
```
```powershell
while ($someCondition) {
  # ...
}

do {
  # ...
} while($someCondition);
```
