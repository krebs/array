# POSIX-compliant implementation of arrays in shell

It is zero-based and uses "\n" as delimiter, do not mess with it (unless you know what you're doing, which you probably don't ;) ).
If you have suggestions for improvements feel free to send a pull request.

# Usage

```bash
# load the library (better than `source`)
$ . array

# Create a new array
$ A=$(array ' bob' 'ross' '' 'khan')

# Get the length
$ echo "$A" | array_len
4

# Get the element at index 0
$ echo "$A" | array_nth 0
bob%

# Get the element at index 2 (= empty string)
$ echo "$A" | array_nth 2
%

# Get the element at (the invalid) index -1
$ echo "$A" | array_nth -1

# The error code is set to 1
$ echo $?
1

# Append item to array
A=$(array "$A" 'lolwut')

# Iterate over the array
$ echo "$A" | while IFS= read element; do echo "$element, "; done
 bob,
ross,
,
khan,

# Get the index of the element khan
$ echo "$A" | array_indexof khan
3%
```
