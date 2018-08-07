# Keychain Access

I got annoyed that:

* I had to write my passwords in cleartext in my scripts, or
* I had to write my password all the time when using my own scrips, when i have this amazing osx keychain.
* The security(1) - Command line interface to the keychain is very hard to use in scripts.

So, I wrote this bash script to allow for easy access to the macOS keychain.

---

# Example usage

## Google Mail Atom Feed

First add the username and password to the osx keychain:

    security add-generic-password -s google -a mogensen -w PaSSW0rd

This adds an entry to the login keychain.

Then use the keychain script to get the username and password:

```bash
gmail () {
    USER=`keychain -u -s google`
    PASS=`keychain -p -s google`

     curl -u $USER:$PASS --silent "https://mail.google.com/mail/feed/atom" |  \
  	     perl -ne 'print "\t" if /<name>/; print "$2\n" if /<(title|name)>(.*)<\/\1>/;'
}
```

## Specifying a keychain
Create a new keychain:

    security create-keychain -P my-new-keychain

Add a new entry to the keychain:

    security add-generic-password -s secret_server -a root -w PaSSW0rd my-new-keychain

Then use the keychain script to get the username and password:

```bash
secret_server_api_credentials () {
    USER=`keychain -u -s secret_server -k my-new-keychain`
    PASS=`keychain -p -s secret_server -k my-new-keychain`
    echo $USER:$PASS
}
```
---

See the man page [security(1) Mac OS X Manual Page](http://developer.apple.com/library/mac/#documentation/Darwin/Reference/ManPages/man1/security.1.html) for more options.
