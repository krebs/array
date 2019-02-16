# POSIX-compliant implementation of arrays in shell
![Travis Badge]( https://travis-ci.org/makefu/array.svg )

# Usage

```bash
# load the library (better than `source`)
$ . array

# Create a new array
$ A=$(array ' bob' 'ross' '' 'khan')

# Get the length
$ printf '%s\n' "$A" | array_len
4

# Get the element at index 0
$ printf '%s\n' "$A" | array_nth 0
bob%

# Get the element at index 2 (= empty string)
$ printf '%s\n' "$A" | array_nth 2
%

# Get the element at (the invalid) index -1
$ printf '%s\n' "$A" | array_nth -1

# The error code is set to 1
$ echo $?
1

# Append item to array
A=$(array_append "$A" 'lolwut')

# Iterate over the array
$ printf '%s\n' "$A" | while IFS= read element; do printf '%s, \n' "$(printf '%s\n' "$element" | array_element_decode)"; done
 bob,
ross,
,
khan,

# Get the index of the element khan
$ printf '%s\n' "$A" | array_indexof khan
3%
```

# Notes

This is a PoC that arrays 'can' be implemented in posix shell, nothing more, nothing less.
If you want full-fledged data structures in your shell i suggest you either rewrite your script in a more powerful scripting language or you use [jq](http://stedolan.github.io/jq/) which lets you work with json in your shell.

# License

WTFPL, see COPYING
