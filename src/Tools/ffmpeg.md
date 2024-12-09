# ffmpeg
1. Convert all the files in a particular directory to another format

    - For Linux and macOS this can be done in one line, using parameter expansion to change the filename extension of the output file
        ```
        for i in *.avi; do ffmpeg -i "$i" "${i%.*}.mp4"; done
        ```
    [Source](https://stackoverflow.com/a/33766147)