# a posix-compliant implementation of arrays in shell
Uses \n as delimiter, do not mess with it.

# usage

    $ . array
    $ A=$(array ' bob' 'ross' '' 'khan')
    $ echo "$A" | array_len
    4
    $ echo "$A" | array_nth 0
     bob%
    $ echo "$A" | array_nth 2
    %
    $ echo "$A" | array_nth -1
    $ echo $?
    1
