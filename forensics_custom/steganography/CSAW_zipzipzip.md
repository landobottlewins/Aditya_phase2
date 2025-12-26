# CSAW 2024 Qualifiers: ZIPZIPZIP
> "description": "Brighten up at last with the flag",
>
> "author": "knight_",
>
> "files": ["challenge.zip"]


## Solution

- On opening the `challenge.zip` we see another zip inside it. Upon unzipping it we get a text file and a zip file and this pattern goes on and on.
- The desc suggests that the final output should be an image
- Every text file only has 5 characters and it seems like we can string together all the 5 characters of each file and obtain a pattern
- `iVBOR` `w0KGg` `oAAAA` `NSUhE`...
- This seems to be base64.
- Since I cannot manually unzip and open so many files, I created a python script to do the same and give the decoded string
- then I used https://base64.guru/converter/decode/image/png to convert base64 to png

```
import zipfile
import os

current_zip = "chunk_0.zip"
full_data = ""

while True:
    try:
        with zipfile.ZipFile(current_zip, 'r') as z:
            files = z.namelist()
            
            txt_file = [f for f in files if f.endswith('.txt')][0]
            full_data += z.read(txt_file).decode('utf-8')
            
            zip_files = [f for f in files if f.endswith('.zip')]
            
            if not zip_files:
                os.remove(current_zip)
                break
            
            next_zip = zip_files[0]
            z.extract(next_zip)
            
        os.remove(current_zip)
        current_zip = next_zip
        
    except Exception as e:
        print(e)
        break

print(full_data)
```

<img width="1024" height="1024" alt="image" src="https://github.com/user-attachments/assets/474b07b7-6b3d-44ed-9f35-73ccf5034cc7" />


## Flag:

```
csawctf{ez_r3cur5iv3ne55_right7?}
```

