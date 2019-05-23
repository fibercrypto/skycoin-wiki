The data directory can be controlled with `-data-dir` but defaults to various locations depending on the system.  The default data directory is hidden on Unix-like systems due to the dot prefix.

Wallets are saved in a `wallets/` folder inside the data directory.  

### Windows
  * `C:\.skycoin` when running in development mode (i.e. running from source)
  * `%HOMEPATH%\.skycoin` aka `C:\Users\{user}\.skycoin` for production builds

### OS X / macOS
  * `~/.skycoin` aka `/Users/{user}/.skycoin`

To view hidden folders on OS X / macOS:
https://ianlunn.co.uk/articles/quickly-showhide-hidden-files-mac-os-x-mavericks/

### Linux
  * `~/.skycoin`