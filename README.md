# Keychain Access
================================

I got annoyed that:

* I had to write my passwords in cleartext in my scripts, or
* I had to write my password all the time when using my own scrips, when i have this amazing osx keychain.
* The security(1) - Command line interface to the keychain is very hard to use in scripts.

So, I wrote this bash script to allow for easy access to the osx keychain.

## Example usage:
First add the username and password to the osx keychain

    security add-generic-password -s google -a mogensen -w PaSSW0rd

Then use the keychain script to get the username and password

```bash
gmail () {
    USER=`keychain -u -s google`
    PASS=`keychain -p -s google`

     curl -u $USER:$PASS --silent "https://mail.google.com/mail/feed/atom" |  \
  	     perl -ne 'print "\t" if /<name>/; print "$2\n" if /<(title|name)>(.*)<\/\1>/;'
	}
```

Se the man page [security(1) Mac OS X Manual Page](http://developer.apple.com/library/mac/#documentation/Darwin/Reference/ManPages/man1/security.1.html)
