# BL: Bash Lisp, lisp flavored bash
A utility that transpiles a lisp-like syntax to bash script at the command line
"liberate yourself from the tyrany of bash script with lisp"

## Use
```
$> bl
(((((SINCE 1958)))))
$> (echo hello world)
hello world
$> (ls -a)
total 612
drwxr-xr-x  7 david david   4096 Jul 27 15:27 Desktop
drwxr-xr-x  4 david david   4096 Oct 25 01:12 Downloads
...
```

## Bash as a Lisp:

- comments
```
; ignore this text
```
translates to
```
\# ignore this text
```

- programs make the head of an unquoted list
e.g., 
```
(head foo.txt)
```
translates to
```
head foo.txt
```
- string commands together with do
e.g., 
```
(do (echo "first line") (echo "second line"))
```
does the same as:
```
echo "first line"; echo "second line"
```

- declare variable as such:
```
$> (set Name "david")
```
same as:
```
Name="David"
```
Note: Not sure if this should return the variable by default.

- refer to variable
```
(echo (deref Name))
```
means the same as
```
echo $Name
````
- control structures
- if-else-fi
```
(if (!= (deref Name) "David") (echo "name is david") (echo "name isn't david"))
```
translates to: 
```
if [ $Name != $USER ]
then
    echo "name is david"
else
    echo "name isn't david"
fi
```
- conditional execution
```
(|| (echo "Always executed") (echo "Only executed if first command fails"))
(&& (echo "Always executed") (echo "Only executed if first command does NOT fail"))
```
translates to:
```
echo "Always executed" || echo "Only executed if first command fails"
echo "Always executed" && echo "Only executed if first command does NOT fail"
```
- case statements
```
(case
  0 (echo "zero")
  1 (echo "one:)
  true (echo "not null"))
```
```
case $sum in
  0) echo "zero"
  1) echo "one"
  *) echo "not null"
esac
```

- for loops
```
(for (X (range 1 3))
  (echo X))
```

```
for X in {1..3}
do
  echo $X
done
```
- while loops
```
(while (< (deref X) 10)
  (do
    (echo (deref X))
    (set X (exp X 1)))) ; X++
```
translates as:
```
while [$X < 10]
do
    echo $X
    X=(( $X+1 ))
done
```
- function definitions
```
(dfn foo (Name)
  (echo "hello " (deref $Name)))
```
translates to
```
function foo ()
{
  echo "hello " $Name
  return 0
}
```

- function call
call foo as a native function
(foo (david)) => hello david

- expressions
``` (echo (exp (+ 10 5))) ```
translates as:
``` echo $(( 10 + 5 )) ```

- ordinary stuff works as expected:
``` (| (ls -l) (grep "\.txt")) ```
means
``` ls -l | grep "\.txt" ```

``` (cat "test.txt") ```

``` (cp . ./Desktop) ```

TODO:
redirects >>, >, <<, etc.

# built in functions
help
