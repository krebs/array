#! /bin/sh

# must use nawk for solaris
# http://cfajohnson.com/shell/cus-faq-2.html#Q24
case `uname` in
  SunOS) arrayawkcmd=nawk ;;
  *) arrayawkcmd=awk ;;
esac

##
# Definitions:
# - An array is a newline separated list of array elements.
# - An array element is an `array_element_encode`d string.

##
# Array constructor
#
# Turn arguments into an array.
array () {
  for i in "$@" ; do
    printf '%s\n' "$i" | array_element_encode
  done
}

##
# Append items to an array
#
# Pass your array as the first argument:
#   array_append "$A" 'next' 'another'
array_append () {
  printf '%s\n' "$1" && shift &&
  for i in "$@" ; do
    printf '%s\n' "$i" | array_element_encode
  done
}


##
# Get length of array
#
# Pipe your array in:
#   printf '%s\n' "$ary" | array_len
array_len () {
  wc -l
}

##
# Element accessor
#
# Uses first argument as (zero-based) index.
#
# Exit status:
#    0  Success.
#   >0  An error occurred.
#
# Pipe your array in:
#   printf '%s\n' "$ary" | array_nth $index
array_nth () {
  printf '%d' "$1" >/dev/null 2>&1 \
    && "$arrayawkcmd" -v i="$1" '
        BEGIN { code=1 }
        NR == i + 1 { print $0; code=0 }
        END { exit code }
      ' \
    | array_element_decode
}

##
# Get the first (zero-based) index of an element.
#
# Exit status:
#    0  Success.
#   >0  An error occurred (element not in array).
#
# Pipe your array in:
#   printf '%s\n' "$ary" | array_indexof "$element"
array_indexof () {
  e=`printf '%s\n' "$1" | array_element_encode` "$arrayawkcmd" '
    BEGIN { e=ENVIRON["e"] ; i = -1 ; code = 1 }
    i < 0 && e == $0 { i = NR - 1 ; print i ; code = 0 }
    END { exit code }
  '
}

##
# Remove the first occurence of an element.
#
# Exit status:
#    0  Success.
#   >0  An error occurred (element not in array).
#
# Pipe your array in:
#   printf '%s\n' "$ary" | array_remove "$element"
array_remove () {
  e=`printf '%s\n' "$1" | array_element_encode` "$arrayawkcmd" '
    BEGIN { e=ENVIRON["e"] ; i = -1 ; code = 1 }
    { if (i < 0 && e == $0) { i = NR - 1 ; code = 0 } else { print $0 } }
    END { exit code }
  '
}

##
# Turn stdin into an array element.
#
# This is similar to urlencode but only for % and \n (and not \0).
#
array_element_encode() {
  sed 's/%/%25/g' | sed -e :a -e '$!N; s/\n/%0A/; ta'
}

##
# Inverse function of array_element_encode.
#
array_element_decode() {
  sed -e 's/%0[aA]/\
/g' -e 's/%25/%/g'
}

