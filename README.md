# shell_toolz
A small collection of shell scripts useful for pen testing

bin2sh:  This shell script will take a given file and compress it, base64 encode it and then convert it into a list of shell commands that, when copied and pasted into a remote shell sessions, will reassemble the file in its original form.  Also performs an md5 checksum of the file before and after for integrity checking.  This tool was inspired by exe2bat, but is designed with a use case of having a basic shell on a system (i.e., netcat shell) and then having an easy method of transferring files into the remote system when you don't have an easy way of doing so.  For example, if you are several pivot layers deep into a network and the remote system doesn't have a direct route back to you for wget, etc.  

Note that this tool, as well as others here were tested with Kali Linux and the Gnome environment.  Because they are meant for copy/paste operations, there is a small dependency.  The xsel tool is used to interface with the clipboard and can be installed with:

apt-get install xsel

shx:  This is a tool that, when run on a remote system, it will compress and base64 a file as with the bin2sh tool.  But this tool will output the lines of base64 in the remote shell session and wrap them between delimiter lines, plus add a metadata line that contains the original filename and md5.

Use case example:

Let's say you run linuxprivchecker.py or unix-privesc-check on the target.  You could scroll through the many lines of output in the shell session or you could redirect it to a file and download it for processing locally which is much more convenient.  So, in a Gnome terminal window running under Kali Linux, you have a remote shell.  This is how it would work:

./linuxprivchecker.py > lpc.txt
./shx lpc.txt

Now select Edit > Select All.  Then right click and copy.  You don't need to highlight the specific lines containing the data, just grab everything.  Now run the companion tool on the Kali system in another window:

unshx

The unshx tool will read the data off of the clipboard, identify the last set of delimiter lines found and then extract that data.  It will find the original filename and md5 checksum in the metadata line and name the file accordingly and perform an integrity check to verify the file is good.  That's it!  It's designed to be super quick and easy.

Keep in mind that the unshx tool will look for the last file you ran shx on within the clipboard output and that file only.  So you can repeat this process continuously without having to worry about manually carving out the lines pertaining to a specific file.  

Although very simple, I have found this tool to be extremely useful and convenient, hopefully you will too.  Enjoy! 
