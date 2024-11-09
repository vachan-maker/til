# Using tar.gz
I decided to try out the jetbrains applications. This is how I use the tarball.

Once the file is downloaded, you have to extract the file. The application can be launched by executing the `./bin/ WebStorm.sh` script.

But it is much easier to launch the application by adding the directory to the PATH

1. Extract the file
2. Add directory to PATH

    `nano ~/.bashrc`

    `export PATH=$PATH:/path/to/directory/`
3. Reload the shell environment

    `source ~/.bashrc`
