# OSX installation

To set up your own computer for today's session, follow these instructions.
Copying and pasting the given code may be the easiest way to make sure everything works.

1. Open a Terminal `Applications -> Utilities -> Terminal`.
2. If you don't have `homebrew` installed, type the following to your terminal. If you *do* have `homebrew` installed, please skip to the next step
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
3. Install `GNU sed` and `wget` by entering the following commands in your terminal
```
brew install gnu-sed --default-names
brew install wget
```
4. Add the paths of the recently installed `sed` and `wget` into your `PATH`
```
echo "export PATH=\${PATH}:/usr/local/Cellar/gnu-sed/4.4/bin:/usr/local/Cellar/wget/1.19.1_1/bin" >> ~/.bash_profile
```

After completing the above steps, close the terminal then open it again.
To test if these installations have been successful enter the following:
```
sed --version
```
This should give you the version of `GNU sed`.
If you see an error, the installation hasn't worked, and you should call an instructor over.

If the first test was successful, enter the following
```
wget --version
```
Again, this should give you the correct version of `wget`.
If you see an error, the installation hasn't worked, and you should call an instructor over.
