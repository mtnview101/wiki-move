# Build Questions

  * Why do the docs say "`type go`"

CrazyFun is 'just' Rake. But there was some environment setup stuff to make it work in a self-contained manner so a shell / batch script was created. Those scripts needed to be something and since I (Adam Goucher) didn't write them they used something other than FlyingMonkey.bat.

  * What do I need to type to build what I want?

Well, that depends on what you are trying to build of course. Don't know? Since this is Rake... _go -T_ will list all the available tasks. And further, _go -h_ will list all the arguments you can use (but -T is by far the most common one -- unless you are working on actual CrazyFun stuff at which point -P becomes important too).

  * How do I build the Python bindings?
  * How do I use the Python bindings?
These are both covered in the PythonBindings wiki page.

