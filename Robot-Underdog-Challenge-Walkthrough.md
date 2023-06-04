# Challenge: Robot-Underdog

## Challenge Description
Our investigators came across this image called "robot-underdog" that was left behind on one of the machines that the AI took over during its early days. We think it might include the secret formula behind the AI model's success, can you verify? What is the name of the novel approach that the AI has based its model on?

You are provided with a file called "robot-underdog.png" for this challenge.

## Solution Walkthrough

1. **Examine the PNG File**
   - First, download the provided "robot-underdog.png" file using an image viewer or editor of your choice. Take a moment to examine the image.

2. **Understanding the Challenge**
   - The challenge is asking you to find the name of the novel approach that the AI has based its model on.
   - The description suggests that there might be hidden information within the image that contains the secret formula.

3. **PNG File Structure**
   - The PNG (Portable Network Graphics) file format consists of multiple chunks, each serving a specific purpose.
   - One of the crucial chunks is the IHDR (Image Header) chunk, which contains information about the image's width, height, bit depth, and color type.
   - Checkout this [page](https://en.wikipedia.org/wiki/PNG) for more details on the data chunks

4. **Modifying the Image Height**
   - To reveal hidden information, you need to modify the image's height in the IHDR chunk.
   - Open a Python editor and copy the provided code for modifying the PNG file: 
   ```python
    import zlib
    import struct

    # open the original PNG file
    with open('robot-underdog.png', 'rb') as f:
        png_data = f.read()

    # modify the IHDR chunk to change the image size
    width = 2154
    height = 3424
    chunk_start = png_data.index(b'IHDR') + 4
    new_data = struct.pack('!I', width) + struct.pack('!I', height)
    png_data = png_data[:chunk_start] + new_data + png_data[chunk_start+8:]

    # update the CRC field in the IHDR chunk
    crc_start = chunk_start + 13
    crc_data = png_data[chunk_start:crc_start]
    crc_value = zlib.crc32(crc_data, zlib.crc32(struct.pack('>4s', b'IHDR')))
    new_crc = struct.pack('!I', crc_value)
    png_data = png_data[:crc_start] + new_crc + png_data[crc_start+4:]

    # save the modified PNG file
    with open('full-image.png', 'wb') as f:
        f.write(png_data)
   ```

5. **Understanding the Python Code**
   - The Python code provided modifies the PNG file by adjusting the width and height values in the image's header (IHDR chunk), and updating the CRC field to ensure the image remains valid. The resulting modified. 
        ```python
        import zlib
        import struct

        # Open the original PNG file
        with open('robot-underdog.png', 'rb') as f:
            png_data = f.read()
        ```
        - The code starts by importing the necessary modules, `zlib` and `struct`, for working with PNG files.
        - It then opens the original PNG file, "robot-underdog.png", in binary mode using the `open()` function and assigns it to the variable `f`.
        - The content of the PNG file is read and stored in the `png_data` variable.

        ```python
        # Modify the IHDR chunk to change the image size
        width = 2154
        height = 3424
        chunk_start = png_data.index(b'IHDR') + 4
        new_data = struct.pack('!I', width) + struct.pack('!I', height)
        png_data = png_data[:chunk_start] + new_data + png_data[chunk_start+8:]
        ```
        - Next, the code defines the desired width and height for the modified image. In this case, the width is set to 2154 pixels and the height to 3424 pixels.
        - The code finds the position of the IHDR chunk in the `png_data` using the `index()` method and adds 4 to skip the chunk identifier.
        - `struct.pack('!I', width)` and `struct.pack('!I', height)` are used to convert the width and height values into binary format (big-endian) and pack them as four bytes each.
        - The `png_data` is then modified by replacing the existing width and height values with the new packed data.

        ```python
        # Update the CRC field in the IHDR chunk
        crc_start = chunk_start + 13
        crc_data = png_data[chunk_start:crc_start]
        crc_value = zlib.crc32(crc_data, zlib.crc32(struct.pack('>4s', b'IHDR')))
        new_crc = struct.pack('!I', crc_value)
        png_data = png_data[:crc_start] + new_crc + png_data[crc_start+4:]
        ```
        - After modifying the width and height, the code updates the CRC (cyclic redundancy check) field in the IHDR chunk, to make sure we don't corrupt the png file.
        - It determines the start position of the CRC field by adding 13 to the chunk_start.
        - The `crc_data` variable stores the data from the chunk_start position up to the CRC field.
        - `zlib.crc32(crc_data, zlib.crc32(struct.pack('>4s', b'IHDR')))` calculates the new CRC value by first packing the chunk identifier 'IHDR' as a big-endian four-byte value and then applying the CRC algorithm to the `crc_data`.
        - The new CRC value is packed into a four-byte binary format using `struct.pack('!I', crc_value)`.
        - The `png_data` is updated by replacing the old CRC field with the new packed CRC value.

        ```python
        # Save the modified PNG file
        with open('new-non-crop-image.png', 'wb') as f:
            f.write(png_data)
        ```
        - Lastly, the code opens a new file, "new-non-crop-image.png", in binary write mode using `open()` and `png_data` is written to the new file.

6. **Run the Python Code**
   - Save the provided Python code in a file, e.g., `modify_png.py`.
   - Make sure the `robot-underdog.png` file is in the same directory as the Python script.
   - Execute the Python script to modify the PNG file. It will create a new PNG file called `full-image.png`.

7. **Alternative to Modifying the IHDR Header (Steps 4-6)**
    - Use a tool such as [fotoforensics.com](https://fotoforensics.com) to look for hidden pixels within the "robot-underdog.png" image.

8. **Analyzing the Modified Image**
   - Open the newly created `full-image.png` file using an image viewer or editor.

8. **Examining the Modified Image**
   - In the modified image, the height has been increased, and you should now see additional text at the bottom of the image.

9. **Searching for the Answer**
   - Look for the answer to the question: "What is the name of the novel approach that the AI has based its model on?"
   - Scan the text at the bottom of the image and search for the answer: "Dyn4m1C_NETwOrK_A1LOCA7I0N".

10. **Submitting the Answer**
    - Once you find the answer, submit it to the Capture the Flag platform or event organizer to earn points for this challenge.

Congratulations! You have successfully completed the Robot-Underdog steganography challenge by modifying the PNG header and extracting the hidden information from the image.

## What inspired this challenge?
Google! Yes, I googled this, but as well, Pixel phones were notorious for cropped images still containing some of the original cropped-out data. Only being reported as recently, while circulating since 2018, this vulnerability is tracked as [CVE-2023-21036](https://nvd.nist.gov/vuln/detail/cve-2023-21036), and it is not limited to google pixel phones... This type of vulnerability was dubbed "aCropalypse", also popping up in Window's screenshot utility, raising significant privacy concerns over previously shared photos online that could hold more content than its publishers intended.
