
## 1. Compression and decompression with gzip

#### Compress

> ```shell
> gzip namefile.extension
> ```
> 
> Product: `namefile.extension.gz`

#### Decompress

> ```shell
> gunzip namefile.extension.gz
> ```
   Product: `namefile.extension`

#### Sample

```shell
┌──(kali㉿kali)-[~/Desktop]
└─$ gzip trash.txt     
                                                                                                               
┌──(kali㉿kali)-[~/Desktop]
└─$ ls
trash.txt.gz
                                                                                                               
┌──(kali㉿kali)-[~/Desktop]
└─$ gunzip trash.txt   
                                                                                                               
┌──(kali㉿kali)-[~/Desktop]
└─$ ls
```